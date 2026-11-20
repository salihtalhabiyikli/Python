-> Dependency Injektion bir fonksiyonun ihtiyaç duyduğu şeyleri kendinin oluşturması yerine dışarıdan almasıdır. 
Kısaca tekrar edecek kodları bir yere yazarsınız ve ihtiyacınız oldukça dilediğiniz fonksiyonda kullanabilirsiniz. 

```python 
from fastapi import FastAPI, Depends 

def sayfalama(sayfa:int=1,limit:int=10):
	return {"sayfa":sayfa , "limit": limit}
	
@app.get("/urunler")
def urunleri_getir(params: dict= Depends(sayfalama)):
	return{"urunler":[], "params":params}
	
@app.get("/siparisler")
def siparisleri_getir(params: dict = Depends(sayfalama)):
	return {"siparisler": [], "params":params}
```

-  Burada yazdığımız sayfalama fonksiyonunu 2 endpointte kullanıyor.

### Sınıf Tabanlı Dependency 
```python
class Sayfalama :
	def __init__(self, sayfa: int=1 , limit: int=10):
		self.sayfa= sayfa
		self.limit=limit
@app.get("/urunler")
def urunleri_getir(params: Sayfalama = Depends( Sayfalama)):
	return{
	"sayfa": params.sayfa,
	"limit": params.limit
	}
```

### Auth İle Kullanım 
```python
from fastapi.security import OAuth2PasswordBearer
from fastapi import Depends, HTTPException, status

oauth2_scheme = OAuth2PasswordBearer(tokenUrl="giris")

# Dependency — her korumalı endpoint bunu kullanır
def aktif_kullanici(token: str = Depends(oauth2_scheme)):
    if token != "gecerli-token":  # normalde JWT doğrulama
        raise HTTPException(
            status_code=status.HTTP_401_UNAUTHORIZED,
            detail="Geçersiz token"
        )
    return {"email": "ahmet@mail.com"}

# Korumalı endpointler
@app.get("/profil")
def profil(kullanici = Depends(aktif_kullanici)):
    return kullanici

@app.get("/urunlerim")
def urunlerim(kullanici = Depends(aktif_kullanici)):
    return {"kullanici": kullanici, "urunler": []}
```

### İç İçe Dependency
```python 
def veritabanş():
	db="veritabani baglantisi"
	return db 
	
def aktif_kullanici(
token : str = Depends(oauth2_scheme),
db=Depends(veritabani)
):
	kullanici=db 
	return kullanici 
@app.get("/profil")
def profil(kullanici=Depends(aktif_kullanici)):
	return kullanici
```

### 5. Yield ile Dependency (Veritabanı için)
```python
def veritabani():
    db = "baglan"       # bağlantıyı aç
    try:
        yield db        # endpoint'e ver
    finally:
        db = "kapat"    # istek bitince kapat

@app.get("/urunler")
def urunleri_getir(db = Depends(veritabani)):
    # db burada kullanılır
    return {"db": db}
```

> `yield` — SQLAlchemy ile veritabanı bağlantısı yönetirken çok kullanılır, ilerleyen konuda göreceğiz.


[[FAST API]]