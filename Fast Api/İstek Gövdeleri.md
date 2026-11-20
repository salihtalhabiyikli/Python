-> Bu kısma başlamadan biraz Pydanticten bahsetmemizde fayda var. pyDantic veri analizi aşamalarını hantallıktan kurtarmak için yazılmış bir teknolojidir 

-> PyDanticte model demek aslında bir veri şablonu demektir. Biz genel olarak Base Model üzerine yoğunlaşacağız. 
[[FAST API]]
-> PyDantic daha yazılmadığı zamanlar veri tiplerini boşlukları ve hataları dev bir if else süzgecinden geçiriyorduk pydantic şuan pythona otomatik veri tanıma sistemi otomatik hata fırlatma sistemi ve daha bir çok araç sunmaktadır. 

-> Pandastan farkı tamamen veri aktarımı kısmıdır. Pandas veriyi olduğu gibi aktarır ve gruplar sınıflandırır veya filtreler. PyDantic ise veriyi analiz eder eksikleri söyler veya kapatır. 


### PyDantic Model 
- Fast apide istek gövdeleri pydantic ile tanımlanır 
```python 
from fastapi import FastAPI 
from pydantic import BaseModel 

app=FastAPI()

class Urun(BaseModel):
	ad:str
	fiyat:int
	stok:int
	aktif:bool 
@app.post("/urunler")
def urun_olustur(urun:Urun):
	return urun
```

```json
// POST /urunler
{
  "ad": "Laptop",
  "fiyat": 15000.99,
  "stok": 50,
  "aktif": true
}
```

Bu kısımda fast api otomatik olarak 
1. Jsonu parse eder
2. Tipleri doğrular 
3. hatalı veri gelirse 422 döner 
4. swagger üzerine ekler 

### Field İle Gelişmiş Doğrulama 
```python 
from pydantic import BaseModel, Field

class Urun(BaseModel):
    ad: str = Field(
        ...,                         # zorunlu
        min_length=2,
        max_length=100,
        description="Ürün adı"
    )
    fiyat: float = Field(
        ...,
        gt=0,                        # 0'dan büyük olmalı
        description="TL cinsinden fiyat"
    )
    stok: int = Field(
        default=0,
        ge=0,                        # negatif olamaz
        le=10000
    )
    aciklama: Optional[str] = Field(
        default=None,
        max_length=500
    )
```

-  Kısaca field diye model içerisinde bi mekanizmamız var bu metod bize sınıf içerisindeki değişkenleri bir sanal sınıf haline getirerek daha fazla özellik aktarmamızı ve bu geniş özellikleri kullanarak daha detaylı bir arama yapmamızı sağlıyor 

### Validator İle Sistemsel Doğrulama 
```python 
from pydantic import BaseModel,validator

class KullaniciKayit(BaseModel):
	kullaniciAdi:str
	sifre:str
	sifreTekrar:str
	yas:int 
	
	@validator("kullaniciAdi")
	def kullanıcıAdi_kontrol(cls,v):
		if " " in v:
			raise ValueError("Kullanıcı adında boşluk olamaz")
		return v.lower()
		
	@validator("sifreTekrar")
	def sifreEslessin(cls,v,values):
		if "sifre" in values and v!=values["sifre"]:
			raise ValueError("Şifreler eşleşmiyor")
		return v
	@validator("yas") 
	def yas_kontrol(cls, v):
		 if v < 18: 
			 raise ValueError("18 yaşından küçükler kayıt olamaz") 
		 return v
		
```

- Burada cls v ve values değerlerini bilmiyor olabilirsiniz 
cls : Sınıfın kendisi 
v: Şu an doğrulanan alanın değeri 
values: daha önce doğrulanmış alanlar 

-> PyDantic verileri yukarıdan aşağıya doğru sırayla doğrular. Bunlar pydanticin otomatik olarak gönderdiği parametrelerdir 



