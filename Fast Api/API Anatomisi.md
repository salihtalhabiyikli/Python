
### Bir Projeye Başlamadan 
mkdir yeni-proje
cd yeni-proje
python -m venv .venv
.venv\Scripts\activate
pip install fastapi uvicorn



#### Fast API'yi Kurun 
```python
pip install fastapi uvicorn
```

- Basit bir çalışma dosyası örneği 

```python 
from fastapi import FastAPI # Kütüphan ekleme

app=FastAPI() # Burdaki app kendi seçimimiz FastAPI() ise bizim bir web sunucusu iskeleti oluşturmamız için var.
@app.get("/")

def home():
	return {"mesaj":"Slm Dünya"}
	
@app.get("/selam/{isim}")
	def selam (isim:str):
		return{"mesaj":f"Selam {isim}"}
		
```
- Çalıştırmak için 
```bash 
uvicorn main:app --reload
```