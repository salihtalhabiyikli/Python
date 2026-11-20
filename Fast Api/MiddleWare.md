Client ve sunucu arasındaki her adımda araya giren bir katmandır 

İstek geldi
     ↓
[ Middleware ]   ← burada işlem yapılır
     ↓
Endpoint çalışır
     ↓
[ Middleware ]   ← burada da işlem yapılır
     ↓
Cevap gitti


---
### Ne İşe Yarar?

```
✅ Her isteği loglama       → "kim, ne zaman, neye istek attı"
✅ CORS                     → frontend bağlantısı
✅ İstek süresi ölçme       → "bu endpoint kaç ms sürdü"
✅ Token kontrolü           → her istekte auth kontrolü
✅ IP engelleme             → belirli IP'leri reddetme
```
Bu kısımda elbette bu istekleri ve özellikleri fonksiyonlara gömebiiriz ama 50 tane fonksiyona ayrı ayrı loglama kodu yazmak yerine tek bir middlewar tüm işi çözmektedir 
[[FAST API]]
### Temel MiddleWar
```python 
from fastapi import FastAPI,Request 
import time 

app=FastAPI()

## @app.middlewar("http")
async def istekSuresiMD(request: Request,call_next):
	baslangic=time.time()
	response=await call_next(request) # endpointi çalıştırır 
	
	sure=time.time()-baslangic
	response.headers["X-Istek-Suresi"]=str(sure)
	
	return response 

```Her istekte response header'ına süre eklenir:
X-Istek-Suresi: 0.0023
```

### `request` ve `call_next` Nedir?
```python
async def middleware(request: Request, call_next):
#                    ↑                  ↑
#              gelen istek         "devam et" fonksiyonu
```
- **`request`** → gelen isteğin tüm bilgileri (URL, header, method...)
- **`call_next`** → "tamam geç, endpoint'i çalıştır" demek

### Loglama MiddleWare
```python 
@app.mdware("http")
async def loglama_mdwr(request:Request , call_next):
	print(f"{request.method} {request.url}")
	
	response=await call_next(request)
	print(f"← Durum kodu: {response.status_code}") 
	return response
```
→ GET http://localhost:8000/urunler
← Durum kodu: 200

→ POST http://localhost:8000/urun
← Durum kodu: 201

→ GET http://localhost:8000/olmayan
← Durum kodu: 404

### 3. CORS Middleware

Frontend (React, Vue) farklı bir adreste çalışır. Tarayıcı güvenlik nedeniyle bunu engeller. CORS bu engeli kaldırır:

```python
from fastapi.middleware.cors import CORSMiddleware

app.add_middleware(
    CORSMiddleware,
    allow_origins=["http://localhost:3000"],  # React adresi
    allow_methods=["*"],                      # GET, POST, PUT, DELETE...
    allow_headers=["*"],                      # tüm headerlar
    allow_credentials=True                    # cookie, token gönderebilsin
)
```

> To-Do App'te React kullanacaksan **bu middleware şart.**

-> request nesnesinin metodlarını bilmiyor olabilirsiniz 
request.method          # "GET", "POST", "PUT"...
request.url             # http://localhost:8000/urunler
request.url.path        # /urunler
request.headers         # {"authorization": "Bearer eyJ...", ...}
request.client.host     # "192.168.1.1"  → kullanıcının IP'si
request.query_params    # {"sayfa": "1", "limit": "10"}

- Bunlar sizin sıfırdan değişken yazmanızı ve ölçüm yapmanızı kolaylaştıran araçlardır 
[[FAST API]]