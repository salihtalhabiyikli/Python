-> Bir web sitesinde adresin sonuna ? koyup gönderdiğiniz parametrelere sorgu parametresi denir 
API Anahtar = Değer dengesini yakalamaya çalışmaktadır.

[[FAST API]]
```python 
import fastapi from FastAPI
app=FastAPI()

@app.get("/urunler")
def urunleriGetir(kategori:str,sayfa:int,limit:int):
	return {"kategori":kategori,"sayfa":sayfa,"limit":limit}
```
- Siz tipi belirttiğinizde fast api bunu otomatik dönüştürür doğrular. 
### 2. Varsayılan Değerler
```python
@app.get("/urunler")
def urunleri_getir(
    sayfa: int = 1,
    limit: int = 10,
    siralama: str = "yeni"
):
    return {"sayfa": sayfa, "limit": limit, "siralama": siralama}
```

- Varsayılan değer varsa → **opsiyonel**, gönderilmezse varsayılan kullanılır
- Varsayılan yoksa → **zorunlu**, gönderilmezse 422 hatası döner
### Bool Değerler 
FastAPI bool değerleri akıllıca çevirir:

```python
@app.get("/urunler")
def urunleri_getir(stokta_var: bool = False):
    return {"stokta_var": stokta_var}
```

Aşağıdakilerin hepsi `True` sayılır:

```
?stokta_var=true
?stokta_var=1
?stokta_var=on
?stokta_var=yes
```
### Query() ile Gelişmiş Doğrulama

```python
from fastapi import Query

@app.get("/urunler")
def urunleri_getir(
    q: str = Query(
        default=...,          # zorunlu alan (... = required)
        min_length=3,
        max_length=50,
        description="Arama terimi"
    ),
    limit: int = Query(
        default=10,
        ge=1,                 # 1 veya daha büyük
        le=100,               # 100 veya daha küçük
        description="Sayfa başına sonuç"
    )
):
    return {"q": q, "limit": limit}
```

#### Query() Doğrulama Seçenekleri

| Parametre    | Açıklama          | Örnek                |
| ------------ | ----------------- | -------------------- |
| `min_length` | Minimum karakter  | `min_length=3`       |
| `max_length` | Maksimum karakter | `max_length=50`      |
| `ge`         | Büyük eşit        | `ge=1`               |
| `le`         | Küçük eşit        | `le=100`             |
| `gt`         | Kesin büyük       | `gt=0`               |
| `lt`         | Kesin küçük       | `lt=1000`            |
- Web geliştirmede url mantığı genel olarak 2 alana hizmet etmektedir
1. Neleri göstereyim
2. Nasıl göstereyim 
URL'ler tamamen bu mantıkla oluşturulmuştur. Bu yüzden bu kısımda biraz fazla detaylı işledik. 