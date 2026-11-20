## Response Model

---
### Neden Gerekli?

Veritabanından çekilen bir kullanıcı verisini dönerken **şifre, token** gibi hassas alanların gitmemesi gerekir. Response model bu kontrolü sağlar.

```python
# Tehlikeli — her şey gider
@app.get("/kullanici/{id}")
def kullanici_getir(id: int):
    return {
        "ad": "Ahmet",
        "email": "ahmet@mail.com",
        "sifre": "12345",        # ❌ şifre gitti!
        "token": "abc123"        # ❌ token gitti!
    }
```

---

### Temel Kullanım
[[FAST API]]
```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

# Veritabanındaki tam model
class KullaniciDB(BaseModel):
    ad: str
    email: str
    sifre: str
    token: str

# Dışarıya gönderilecek model
class KullaniciResponse(BaseModel):
    ad: str
    email: str
    # sifre ve token yok!

@app.get("/kullanici/{id}", response_model=KullaniciResponse)
def kullanici_getir(id: int):
    # Normalde veritabanından gelir
    return KullaniciDB(
        ad="Ahmet",
        email="ahmet@mail.com",
        sifre="12345",
        token="abc123"
    )
```

```json
// Dışarıya sadece bu gider:
{
  "ad": "Ahmet",
  "email": "ahmet@mail.com"
}
```

> FastAPI `response_model`'e bakarak fazla alanları **otomatik filtreler.**

---

### Liste Response
```python
from typing import List

class UrunResponse(BaseModel):
    ad: str
    fiyat: float

@app.get("/urunler", response_model=List[UrunResponse])
def urunleri_getir():
    return [
        {"ad": "Laptop", "fiyat": 15000, "stok": 50},  # stok gitmez
        {"ad": "Mouse",  "fiyat": 250,   "stok": 10}   # stok gitmez
    ]
```

```json
[
  {"ad": "Laptop", "fiyat": 15000},
  {"ad": "Mouse",  "fiyat": 250}
]
```

---
### Belirli Alanları Hariç Tutmak

```python
@app.get(
    "/kullanici/{id}",
    response_model=KullaniciDB,
    response_model_exclude={"sifre", "token"}  # bu alanlar gitmesin
)
def kullanici_getir(id: int):
    return kullanicilar[id]
```

Ya da sadece belirli alanları dahil etmek için:
 
```python
@app.get(
    "/kullanici/{id}",
    response_model=KullaniciDB,
    response_model_include={"ad", "email"}  # sadece bunlar gitsin
)
def kullanici_getir(id: int):
    return kullanicilar[id]
```

---
### Farklı Durumlar İçin Farklı Response

```python
from fastapi.responses import JSONResponse
from typing import Union

class UrunResponse(BaseModel):
    ad: str
    fiyat: float

class HataResponse(BaseModel):
    mesaj: str

@app.get(
    "/urun/{id}",
    responses={
        200: {"model": UrunResponse},
        404: {"model": HataResponse}
    }
)
def urun_getir(id: int):
    if id not in urunler:
        return JSONResponse(
            status_code=404,
            content={"mesaj": "Ürün bulunamadı"}
        )
    return urunler[id]
```

### Özet

|Kullanım|Ne İşe Yarar|
|---|---|
|`response_model=Model`|Dışarıya gidecek alanları belirler|
|`response_model=List[Model]`|Liste dönerken kullanılır|
|`response_model_exclude_unset=True`|Boş alanları gizler|
|`response_model_exclude={"alan"}`|Belirli alanları gizler|
|`response_model_include={"alan"}`|Sadece belirli alanları gösterir|## Response Model



[[FAST API]]