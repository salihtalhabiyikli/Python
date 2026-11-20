> Normalde bir iş için yazdığımız fonksiyonları başka fonksiyonlara ekleyerek, yani fonksiyonları özelleştirerek kullanmamızı sağlayan decorator fonksiyonlar vardır.

1-) Bir fonksiyon yazılır 
2-) Fonksiyonun görevleri atanır 
3-) Fonksiyona kalıtım metodu girilir 
4-) Metod ile aktarılan kalıtım diğer fonksiyonlarda ID yolu ile kullanılır 

```python 
def my_decorator(func)
	def wrapper():
		print("Fonksiyon işlemleri")
		func()
		print("Genel işlemler")
	return wrapper 
	
@my_decorator 
def sayHello():
	print("Hello World")	
sayHello()
```

### YIELD FONKSİYONU 
Python'daki `yield` anahtar sözcüğünü anlamak için önce "normal" fonksiyonlar ile "jeneratör" (generator) fonksiyonlar arasındaki farkı kavramak çok işe yarar.

Basitçe söylemek gerekirse: **`return`** bir şeyi verir ve kapıyı çekip çıkar; **`yield`** ise bir şeyi verir, olduğu yerde durur ve bir sonraki sefer kaldığı yerden devam etmek üzere bekler.

---

## 1. Temel Mantık: Dur ve Devam Et

Normal bir fonksiyonda `return` satırına gelindiğinde fonksiyon biter, içindeki tüm yerel değişkenler yok olur. `yield` kullandığında ise Python fonksiyonun o anki durumunu (değişkenlerin değerini, nerede kaldığını) **dondurur.**

### Örnek Üzerinden Bakalım

Aşağıdaki iki kodu kıyaslayalım:

**Normal Fonksiyon (`return`):**

Python

``` python 
def sayi_getir():
    return 1
    return 2  # Bu satıra asla ulaşılamaz

print(sayi_getir()) # Çıktı: 1
```

**Jeneratör Fonksiyon (`yield`):**

Python

``` python
def sayi_ureticisi():
    yield 1
    yield 2
    yield 3

gen = sayi_ureticisi()

print(next(gen)) # Çıktı: 1 (Fonksiyon burada durdu)
print(next(gen)) # Çıktı: 2 (Kaldığı yerden devam etti ve 2'yi verdi)
```

---

## 2. Neden `yield` Kullanmalısın?

"Neden hepsini bir listeye atıp return etmiyoruz?" diye düşünebilirsin. Cevap: **Hafıza (RAM) yönetimi.**

- **Listeler:** 1 milyon elemanlık bir liste oluşturursan, bu listenin tamamı RAM'de yer kaplar.
    
- **Yield (Jeneratörler):** Elemanları tek tek, ihtiyaç duyulduğu anda üretir. RAM'de sadece o anki eleman tutulur.
    

> **Özetle:** `yield` bir **üretim bandı** gibidir. Sipariş geldikçe (yani `next()` veya bir `for` döngüsü tetikledikçe) bir sonraki ürünü üretir ve gönderir.

---

## 3. Gerçek Hayat Senaryosu

Büyük bir veri dosyasını okuduğunu hayal et. Tüm dosyayı hafızaya yüklemek bilgisayarı kilitleyebilir. `yield` ile satır satır okuyup işlem yapabilirsin:


```python
def buyuk_dosya_oku(dosya_yolu):
    with open(dosya_yolu) as f:
        for satir in f:
            yield satir.strip()

# Dosya 10 GB olsa bile, bu döngü RAM'i şişirmeden çalışır.
for satir in buyuk_dosya_oku("dev_veri.txt"):
    print(satir)
```

---

### SEND METODU
`send()` metodu, jeneratörün durduğu yere (yani `yield` satırına) dışarıdan bir değer fırlatmanı sağlar. Bu durumda `yield` bir **kapı** gibi çalışır: hem dışarı ürün çıkarır hem de içeri malzeme alır.
## 1. `send()` Nasıl Çalışır?

Normalde `yield` kullandığımızda satır şöyle görünür: `yield x`. Ancak dışarıdan veri alacaksak satırı şu hale getiririz: `gelen_veri = yield x`.

### Adım Adım Akış:

1. Jeneratör `yield` satırına gelir ve değeri dışarı gönderip durur.
    
2. Sen dışarıdan `.send(deger)` komutunu verirsin.
    
3. Jeneratör uyanır, gönderdiğin `deger`i `yield`ın olduğu yere yerleştirir ve çalışmaya devam eder.
``` python
def liste_isleyici():
    liste = [1, 2, 3]
    
    # 1. elemanı gönderiyoruz
    yield liste[0]
    
    # 2. elemanı gönderiyoruz VE dışarıdan gelecek yeni değeri bekliyoruz
    # Buradaki 'yield' hem 2'yi dışarı atar, hem de 'send' ile geleni içeri alır.
    gelen = yield liste[1] 
    
    if gelen is not None:
        liste[1] = gelen # İşte 2 yerine 4 yazdığımız an!
    
    # 3. elemanı gönderiyoruz (eğer güncellendiyse yeni haliyle)
    yield liste[2]
    
    print(f"Listenin son hali: {liste}")

gen = liste_isleyici()

print(next(gen))       # 1 gelir
print(next(gen))       # 2 gelir (ve işaretçi burada durur)
gen.send(4)            # İçeri 4 göndeririz, 2'nin üzerine yazar. ```