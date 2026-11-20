```python 
class name:
	#Özellikler 
	#Metodlar
pass


nesne1=name()
nesne2=name()
```

## __init__ nedir?
- Aslında bir constructor methoddur.Sınıf oluşturulduğu an çalıştırılır. 
```python 
class Ogrenci:
	def__init__(self,ad,yas):
	self.ad=ad
	self.yas=yas
	
	
ogrenci1=Ogrenci("Ali",20)
ogrenci2=Ogrenci("Ayşe",22)
```