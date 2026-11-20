- Proje karmaşasını önlemek için kullanılır. 

```
FastAPI/
├── .venv/
├── main.py
└── routers/
    ├── __init__.py      (boş dosya)
    └── notlar.py
```

### `routers/notlar.py`

python

```python
from fastapi import APIRouter, HTTPException

router = APIRouter(
    prefix="/notlar",
    tags=["Notlar"],
)

notlar_db = {
    1: {"id": 1, "baslik": "FastAPI çalış", "icerik": "Bölüm 4'tesin"},
    2: {"id": 2, "baslik": "Router öğren", "icerik": "include_router unutma"},
}


@router.get("/")
def notlari_listele():
    return list(notlar_db.values())


@router.get("/{not_id}")
def not_getir(not_id: int):
    if not_id not in notlar_db:
        raise HTTPException(status_code=404, detail="Not bulunamadı")
    return notlar_db[not_id]
```

### `main.py`

python

```python
from fastapi import FastAPI
from routers import notlar

app = FastAPI()
app.include_router(notlar.router)


@app.get("/")
def ana_sayfa():
    return {"mesaj": "Ana uygulama çalışıyor"}
```

### Çalıştırma

Terminalde **`main.py`'nin olduğu klasörde** (`routers` klasörünün bir üstünde):

powershell

```powershell
fastapi dev main.py
```