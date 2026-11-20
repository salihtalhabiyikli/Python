- Hayatınızda asla hatasız bir apı yazamazsınız. Kullanıcı olmayan bir ürün isteyebilir veya yetkisiz bir işlem yapabilir. Bu gibi durumlarda sistemin çöktüğünü söylemek yerine belirli durumlar için tasarlanmış hata mesajları kullanırız. 

```python
# Kötü — hiç hata yönetimi yok
@app.get("/urun/{id}")
def urun_getir(id: int):
    return urunler[id]  # id yoksa Python hatası fırlatır, API çöker
```
[[FAST API]]
```python
# İyi — hata yönetimi var
@app.get("/urun/{id}")
def urun_getir(id: int):
    if id not in urunler:
        raise HTTPException(status_code=404, detail="Ürün bulunamadı")
    return urunler[id]
```

### Header ile Hata Döndürme

Bazı durumlarda hataya ek bilgi eklemek gerekir. Mesela `401` hatalarında:
```python
@app.get("/giris-gerekli")
def giris_gerekli(token: str = None):
    if not token:
        raise HTTPException(
            status_code=401,
            detail="Giriş yapmanız gerekiyor",
            headers={"WWW-Authenticate": "Bearer"}
        )
    return {"mesaj": "Hoşgeldiniz"}
```
### Global Hata Yakalama

Beklenmedik tüm hataları yakalamak için:
```python
@app.exception_handler(Exception)
async def genel_hata_handler(request: Request, exc: Exception):
    return JSONResponse(
        status_code=500,
        content={"mesaj": "Beklenmedik bir hata oluştu"}
    )
```
[[FAST API]]
-  Buradda faklı hata kodları göndermemizin sebebi birinde apinin çökmesi birinde ise serverın komple çökmesidir **`HTTPException`** kullanırken API ayakta, her şey yolunda ve kod senin kontrolünde. Sen "Bak bu durum hatalı" deyip **`raise`** ile süreci sonlandırıyorsun.

**`JSONResponse`** (Global Hata) kısmındaki mantığın da mantıksal olarak tamamen doğru: Ortada bilinçli bir `raise` yapacak kod kalmamış çünkü sunucu veya o anki fonksiyon patlamış. Ancak buradaki teknik nüans şu:

Hata anında tarayıcıdaki değil, yine **sunucunun içinde çalışan son bir savunma hattı (guard)** devreye giriyor.

Şöyle düşün:

1. `urunleri_getir` fonksiyonunun içinde beklenmedik bir Python hatası oldu (örneğin veri tabanı koptu) ve bu fonksiyon çöktü.
    
2. Fonksiyon çöktüğü için artık `raise HTTPException` çalışamaz.
    
3. Tam o esnada, fonksiyonun bir tık üstünde bekleyen ve tüm FastAPI uygulamasını koruyan **"Global Hata Yakalayıcı" (Exception Handler)** devreye girer. Çöken fonksiyondan düşen hatayı havada yakalar.
    
4. Bu yakalayıcı mekanizma, tarayıcıya çirkin bir Python çökme raporu gitmesin diye aceleyle temiz bir **`JSONResponse`** paketi hazırlar ve bunu tarayıcıya **`return`** eder.
    

Yani o `JSONResponse` u üreten yer yine tarayıcı değil, sunucunun içindeki "B planı" mekanizmasıdır. Ama tespiti şahane yapmışsın: Fonksiyon zaten çöktüğü için artık yeni bir hata fırlatılamaz (`raise` edilemez), o yüzden en tepedeki kurtarıcı mekanizma doğrudan bitmiş bir cevabı **`return`** etmek zorundadır.