-> Bir fast api projesi oluşturduğunuzda şu temel yapıyı yazınız 
```python 
from fastapi import FastAPI
app=FastAPI() # Bu kısımda app adında bir nesne oluştururuz. Uvıcorn sunucusu bu nesne üzerinden çalıştırılır

@app.get()  # Eğer bizim main nesnemize get isteğini iletir. Aslında burası bir dekoratördür. Hemen altındaki fonksiyonu çalıştırsın diye yazılır 
async def root(){
return {"message":"Hello World}
}
```
[[FAST API]]
Bunu bir main.py dosyasında çalıştırdığınızda terminalinizde http://xxx.x.x.x:8000 çıktısını göreceksiniz. Bu adresi kopyalayıp açtığınızda kodunuzun çıktısını görebilirsiniz. 
 - Eğer ki uygulama derlenmez hata verirse terminale şu komutu yazıp çalıştırabilirsiniz
 ```
 uvicorn main:app --reload
 ```

Yine hata vermesi sonucu lütfen hata koduna göre bir araştırma yapınız. 

-> Çağırma Komutları :
- POST : Veri Oluşturmak 
- Get : Veri getirmek 
- Put : Veri güncellemek 
- Delete: Veri Silmek 
- Options:Tarayıcı güvenlik izinlerini tarar
- Head: Verinin sadece başlığını çeker 

Dolayısıyla Swıgger UI'da HTTP yönlendirmelerinin her birinin adına işlem adı verilmektedir.
Yazdığınız API'leri dağıtmanız adına Fast API Cloud gibi bir servis bulunmaktadır. Sona doğru bu cloud üzerine nasıl aktaracağınızıda ayrıca anlatacağız.

