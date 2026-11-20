-> Yol parametreleri bir web adersinin içerisine dinamik olarak yerleştirilen ve sunucuya hangi spesifik veriyle çalışmak istediğimizi söyleyen değişkenlerdir. 

-> Geliştirdiğiniz APIlerde her bir veri için ayrı ayrı rotalar yazmak yerine adresi esnek hale getirerek tek bir rotada işi çözmenizi sağlar. 
[[FAST API]]
#### Kod Yapısı 
```python
from fastapi import FastAPI
app=FastAPI()

@app.get("/urunler/{urun_id}")
async def urun_detay(urun_id):
	return{"secilen_urun":urun_id}
```

Siz tarayıcıdan http://xxx.x.x.x:8000/urunler/45 adresine istek atarsanız FastApi otomatik olarak adresteki 45 değerini yakalar ve urun_İd değişkenine eşitler. Çıktı olarka bu eşitlemenin json formatını gösterir. 

-> Burada kritik bir veri güvenliği anlatmakta fayda var. Yuklarıda yazdığımız 45 sisteme düz bir string ifade olarak gider ancak FastApinin en güçlü yanlarından birisi Pythonun tip belirleme özelliğini kullanarak bu parametreyi kısıtlayabilmesidir 

```python
@app.get("/urunler/{urun_id}")
async def urun_detay(urun_id: int):  # Sadece sayısal (integer) değerleri kabul et dedik
    return {"secilen_urun": urun_id}
```

Eğer birisi ürünün id si yerine sözel bir ifade ile erişmeye çalışırsa FastApi bunu otomatik olarka engeller ve kullanıcıya bir hata mesajı dönderir. 
```json
{
    "detail": [
        {
            "loc": ["path", "urun_id"],
            "msg": "value is not a valid integer",
            "type": "type_error.integer"
        }
    ]
}
```

### Path Hiyerarşisi 
FastApi yukarıdan aşağıya doğru rotaları kontrol ederek çalışır. Bu neden sabit olani her zaman kodda daha üstte tanımlamalısınız 

```python
# 1. Önce sabit rota (Çünkü /urunler/aktif adresi alttaki dinamik rotaya takılabilir)
@app.get("/urunler/aktif")
async def aktif_urunler():
    return {"mesaj": "Sadece aktif ürünler listeleniyor"}

# 2. Sonra dinamik rota
@app.get("/urunler/{urun_id}")
async def urun_detay(urun_id: int):
    return {"secilen_urun": urun_id}
```

### Swagger Dökümanında Nasıl Görünür?

Sen koda `{urun_id: int}` yazdığın an, FastAPI gidip **OpenAPI** şemasına bunu işler. Tarayıcıda `/docs` sayfasına girdiğinde, bu rotanın yanında mavi renkli bir **"path"** (yol) parametresi kutucuğu görürsün. Sistem senden o parametreyi girmeden testi çalıştırmana izin vermez, böylece API'ni kullanan herkes orada bir değişken olması gerektiğini anlar 