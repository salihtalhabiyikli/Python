-  Dosya açmak veya oluşturmak için open() fonksiyonu kullanılır.
``` python 
open(dosya_adi, dosya erişme modu)
```
Dosya erişme modu dosyayı hangi amaçla açtığımızı belirtir.

### METODLAR 
>W : Write yazma modu, Dosyayı konumda oluşturur
>A : Append ekleme, Dosya varsa ekler yoksa oluşturur.
>X : Create oluşturma, Dosya zaten varsa hata verir 
>R : read okuma , dosya konumda yoksa hata verir
>Bu metodlar dosya adının yanına yazılır mod olarakta geçerler, Dosyayı niye hangi amaçla açtığımız belirli ve sistemli olsun diye varlar 



```python 
	file= open("Newfile.txt","w")
	file.close()
	file=open("C")
```

-  3. parametre olarak ``encoding='utf-8`` girerek türkçe okunabilirlik ve karater desteği eklenebilir
# 🐍 Python Dosya İşlemleri: Kapsamlı Rehber

Python'da dosyalarla çalışmak, sadece veri okumak değil, sistemleri yönetmek demektir. İşte tüm detaylar:

## 1. Dosya Açma Modları (Detaylı Liste)

Dosyayı hangi amaçla açtığınızı Python'a belirtmeniz gerekir.

|**Mod**|**İsim**|**Açıklama**|
|---|---|---|
|**`r`**|Read|Okuma modudur. Dosya yoksa `FileNotFoundError` verir.|
|**`w`**|Write|Yazma modudur. Dosya varsa **içini siler**, yoksa oluşturur.|
|**`a`**|Append|Ekleme modudur. İmleci dosya sonuna taşır, mevcut veriyi korur.|
|**`r+`**|Read+|Hem okuma hem yazma yapar. Dosyanın başında başlar.|
|**`w+`**|Write+|Yazma ve okuma. Mevcut içeriği siler.|
|**`rb` / `wb`**|Binary|Metin olmayan (resim, exe, video) dosyalar için kullanılır.|

---

## 2. Modern ve Güvenli Yaklaşım: `with` Bloğu

Dosyayı `open()` ile açıp `close()` ile kapatmak eski bir yöntemdir. Eğer `close()` satırı çalışmadan kod hata verirse, dosya açık kalır ve bozulabilir. `with` (Context Manager) bu işi otomatik yapar.



```python
with open("sunucu_loglari.txt", "r", encoding="utf-8") as dosya:
    icerik = dosya.read()
# Burada dosya güvenli bir şekilde kapandı.
```

---

## 3. Veri Okuma Yöntemleri ve Farkları

- **`read(n)`**: Belirtilen `n` karakter kadar okur. Parametre verilmezse her şeyi okur (Büyük dosyalarda RAM'i şişirebilir!).
    
- **`readline()`**: Sadece imlecin bulunduğu satırı okur.
    
- **`readlines()`**: Tüm satırları bir liste haline getirir.
    
- **Döngü ile Okuma (En Verimli)**:
    
    Python
    
    ```python
    with open("data.txt", "r") as f:
        for satir in f:  # Satır satır okur, belleği yormaz.
            print(satir.strip())
    ```
    
> Burdaki strip metodu boşlukları temizler
---

## 4. Dosya İçinde Gezinme: `seek` ve `tell`

Dosyayı okurken görünmez bir imleç hareket eder.

- **`f.tell()`**: İmlecin şu an kaçıncı indexte  olduğunu söyler.
    
- **`f.seek(offset)`**: İmleci istediğiniz indexe  ışınlar. `f.seek(0)` ile dosyanın başına dönebilirsin.
    

---

## 5. İleri Seviye: Yapılandırılmış Veriler (JSON)

DevOps dünyasında `.txt` yerine `.json` veya `.yaml` kullanılır.

|**İşlem**|**Metod**|**Ne Yapar?**|
|---|---|---|
|**Kaydetme**|`json.dump()`|Nesneyi dosyaya JSON olarak yazar.|
|**Okuma**|`json.load()`|Dosyadaki JSON'u Python nesnesine çevirir.|
|**Metne Çevirme**|`json.dumps()`|Nesneyi dosyaya değil, bir string değişkene çevirir.|
|**Metinden Okuma**|`json.loads()`|Elindeki bir string'i (metni) nesneye çevirir.|
```python
import json

kullanici = {"id": 1, "yetki": "admin", "ip": "192.168.1.1"}

# Kaydetme (Serialization)
with open("user.json", "w") as f:
    json.dump(kullanici, f, indent=4) # indent, okunabilirliği artırır

# Okuma (Deserialization)
with open("user.json", "r") as f:
    data = json.load(f)
```

---

## 6. Hata Yönetimi (Exception Handling)

Dosya işlemleri her zaman risklidir (Dosya silinmiş olabilir, disk dolu olabilir). Kodunun çökmesini engellemek için:

Python

```python
try:
    with open("config.py", "r") as f:
        data = f.read()
except FileNotFoundError:
    print("Kritik Hata: Config dosyası bulunamadı!")
except Exception as e:
    print(f"Beklenmedik bir hata oluştu: {e}")
```

---

## 7. Dosya ve Dizin Sistemi (`os` ve `pathlib`)

Sadece dosya içeriği değil, dosya yapısıyla oynamak:

- `os.remove("dosya.txt")`: Dosyayı siler.
    
- `os.mkdir("yeni_klasor")`: Klasör oluşturur.
    
- `os.listdir(".")`: Bulunduğun klasördeki dosyaları listeler.
    
- **Modern Yaklaşım (`pathlib`):**
    
    
    ```python
    from pathlib import Path
    yol = Path("veriler/test.txt")
    if yol.exists():
        print("Dosya mevcut!")
    ```