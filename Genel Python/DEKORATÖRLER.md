https://www.datacamp.com/tutorial/decorators-python

Dekoratörler bir fonksiyonun yapısını değiştirmeden onu başka bir fonksiyonla paketleyerek özelliklerini değiştirmemizi sağlayan bir yapıdır.
### 3 Temel Taşı Vardır 
***Dış Fonksiyon (Decorator): Paketi hazırlar içine fonksiyonu alır.
İç Fonksiyon: Asıl paketleme işlemini yapan ve orijinal fonksiyonun ne zaman çalışacağına karar veren kısımdır 
Return : Dekoratörün hazırladığı paketi geri döndürmesdir.



```python
def deneme1(func):
    def wrapper():
        print("Birinci fonksiyon")
        func()
        print("İkinci fonksiyon")
    return wrapper
@deneme1
def greet():
    print("Son Fonksyion")
greet()
```

### Çalışma Mantığı 
- Öncelikle greet ifadesi olmadıkça kod çıktı vermez 
- Biz deneme1 adında bir fonksiyon oluştururuz bunun içerisinede 2 tane alt fonksiyon ekleriz. 
- Diyelim bu çok büyük bir fonksiyon ve yapısının bozulmaması gerekiyor. 
- O zaman @fonksiyon adı kulllanaran bu ifadenin altına ana fonksiyonumuza ekleyeceğimiz fonksiyonu yazarız. 
- Python en altta greetin çağırıldığını gördüğünde @ kısmını okuarak hedef fonksiyona gider. Fakat kodu yukarıdan aşağıya doğru okuduğu için ilk birinci fonksiyonu sonra sonuncu fonksiyonu ardından 2. fonksiyonu çalıştırır. 

### ARGS VE KWARGS PARAMETRELERİ 
- Standartlaşmış bir yazımdır bunların yerine tek yıldız veya çift yıldızda koysanız olur. 
***Args : Sınırsız isimsiz argümanlar 
Kwargs : Sınırsız isimli argümanlar***


`wrapper` orijinal fonksiyonun 
ördüğümüz bir **"zaman tüneli"** gibidir. Fonksiyon o tünele girer, tünelin girişindeki işlemleri görür (`Before`), tünelin ortasında asıl işini yapar (`func()`), çıkışta da son işlemleri görür (`After`).

### 1. `*args` (Sınırsız İsimsiz Argümanlar)
- bazen bir fonksiyona kaç tane değer gönderileceğini bilemeyiz burda bu kısım devreye girer 
- Gönderilen her şeyi alır fonksiyona sokar 
```python 
def toplama(*args):
    print(args)  # İçeride bir Tuple gibi görünür
    return sum(args)

print(toplama(1, 2, 3))        # Çıktı: (1, 2, 3) -> 6
print(toplama(10, 20, 30, 40)) # Çıktı: (10, 20, 30, 40) -> 100
```

### 2. `**kwargs` (Sınırsız İsimli Argümanlar)
Buradaki "kw" harfleri **Keyword** (Anahtar Kelime) anlamına gelir. Bu yapı, fonksiyona `anahtar=değer` şeklinde veri göndermeni sağlar. Python bunları bir **Dictionary (Sözlük)** içinde toplar.
**Mantık:** "Bana etiketli paketler gönder, ben onları bir ajandaya kaydedip sana vereceğim."
```python
def kimlik_bilgileri(**kwargs):
    print(kwargs) # İçeride bir Sözlük gibi görünür
    for anahtar, deger in kwargs.items():
        print(f"{anahtar}: {deger}")

kimlik_bilgileri(ad="Ali", soyad="Veli", yas=25)
# Çıktı: {'ad': 'Ali', 'soyad': 'Veli', 'yas': 25}
```

| **İfade**  | **Anlamı**   | **Topladığı Veri Tipi**   | **Örnek Kullanım** |
| ---------- | ------------ | ------------------------- | ------------------ |
| `*args`    | Arguments    | **Tuple** (Sıralı liste)  | `func(1, 2, 3)`    |
| `**kwargs` | Keyword Args | **Dictionary** (Etiketli) | `func(a=1, b=2)`   |
## GENEL PROJE 
``` python # 1. DEKORATÖR: İşlemi süsleyelim
def islem_ozeti(func):
    def wrapper(*args, **kwargs):
        print("\n--- Hesaplama Başlatıldı ---")
        sonuc = func(*args, **kwargs)
        print(f"Kullanılan Ayarlar: {kwargs}")
        print("--- İşlem Tamamlandı ---\n")
        return sonuc
    return wrapper

# 2. FONKSİYON: *args ve **kwargs kullanan asıl işçi
@islem_ozeti
def hesapla(*sayilar, **ayarlar):
    islem = ayarlar.get("islem", "topla")
    if islem == "topla":
        return sum(sayilar)
    elif islem == "carp":
        sonuc = 1
        for s in sayilar: sonuc *= s
        return sonuc

# 3. KULLANICI GİRİŞİ: Senin favorin olan 'while'
sayi_listesi = []
print("Sayıları girin (Durmak için 'ok' yazın):")

while True:
    girdi = input("> ")
    if girdi.lower() == 'ok':
        break
    if girdi.isdigit(): # Sadece sayıysa ekle
        sayi_listesi.append(int(girdi))

# 4. ÇALIŞTIRMA: Paketi açıp (unpack) gönderme
islem_tipi = input("Hangi işlem yapılsın? (topla/carp): ")

# İşte burası sihirli an: *sayi_listesi paketi açar!
nihai_sonuc = hesapla(*sayi_listesi, islem=islem_tipi)

print(f"KAPATILIYOR... FİNAL SONUCU: {nihai_sonuc}")
```
- Bu kod en kolay işi yapmasına rağmen en zor yolla yazılmıştır...
- - **Döngü:** "Dur" diyene kadar dön (`while True`).
    
- **Kontrol:** Gelen veri "ok" mi? Değilse sayı mı? (`isdigit`).
    
- **Depolama:** Listeye at (`append`).
    
- **İnfaz:** Listeyi fonksiyona fırlat ve sonucu al.
## Python Dekoratörleri Özeti

Dekoratörler, alt sınıfları doğrudan kullanmaya veya dekore edilen fonksiyonun kaynak kodunu değiştirmeye gerek kalmadan bir fonksiyonun, metodun veya sınıfın işlevselliğini dinamik olarak değiştirir. Python'da dekoratör kullanmak ayrıca kodunuzun DRY (Kendini Tekrarlama) ilkesine uygun olmasını sağlar. Dekoratörlerin çeşitli kullanım alanları vardır, örneğin:

- [Flask ve Django](https://www.datacamp.com/tutorial/web-development-django) gibi Python framework'lerinde yetkilendirme[](https://www.datacamp.com/tutorial/web-development-django)
- Günlük kaydı
- Yürütme süresinin ölçülmesi
- Senkronizasyon

Python dekoratörleri hakkında daha fazla bilgi edinmek için Python'ın [Dekoratör Kütüphanesine](https://wiki.python.org/moin/PythonDecoratorLibrary) göz atabilirsiniz .