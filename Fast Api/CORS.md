Cors Tarayıcının güvenlik kurallarından birisidir. 
Bir webs ayfası kendi geldiği adresten farklı bir adrese istek attığında tarayıcı bunu varsayılan olarak engeller cors sunucunun şu adreslerden gelen isteklere izin veriyorum demesini sağlayan mekanizmadır. 



#### Origin Nedir 
- Protokol + Alan Adı + Port üçlüsüdür bunlardan birisi farklıysa origin farklıdır 
- `https://site.com` ve `http://site.com` → farklı (protokol)
- `https://site.com` ve `https://api.site.com` → farklı (alan adı)
- `http://localhost:3000` ve `http://localhost:8000` → farklı (port)


Frontend’iniz `localhost:3000` adresinde, API’niz `localhost:8000` adresinde çalışıyor. Bunlar tarayıcı için “farklı yerler”. Frontend API’ye istek atınca tarayıcı şöyle düşünür: “Bu sayfa başka bir adrese veri soruyor, API izin veriyor mu bakayım.”

```python 
from fastapi import FastAPI
from fastapi.middleware.cors import CORSMiddleware

app= FastAPI()

origins=[
"http://localhost.tiangolo.com", 
"https://localhost.tiangolo.com", 
"http://localhost", 
"http://localhost:8080"
]
# Bu kısım izin verdiğimiz adreslerin tamamı
app.add_middleware(
CORSMiddleware,
allow_origins=origins, # Listedeki adreslere izin ver
allow_credentials=True, # Kimlik bilgileri taşıyan isteklerede izin ver
allow_methods=["*"], # Tüm http metotlarına izin ver 
allow_headers=["*"], # TÜm headerlara izin ver
)
# Bu kısımdada verdiğmiz izinler var 

@app.get("/")
async def main():
	return {"message":"hello World"}

```

> API portu değiştirmek için *fastapi dev main.py --host 0.0.0.0 --port 8080* # Ağdaki diğer cihazlardan erişim için
> *fastapi run main.py --port 8080*  # Geliştirici modu için




