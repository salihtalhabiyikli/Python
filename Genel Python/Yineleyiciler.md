- burda iki kavram var ilki ıterator bu üzerinde gezinebildiğiniz foreach veya for döngüleriyle tarayabildiğiniz stack verilerdir.
- Diğeri ise Iterator buda elemanları teker teker çağırabilen bir nesnedir.
- Next metodu kullanılarak yazılmış fonksiyon sistemlerinin tek bir parça halinde çalışması hakinde olur 
```python 
nums = [1, 2, 3]
it = iter(nums)   # __iter__ çağrılır, iterator döner

print(next(it))   # 1
print(next(it))   # 2
print(next(it))   # 3
print(next(it))   # StopIteration!
```

Iterator nesneleri tıpkı YAmL fonksiyonu gibi çalışır durduğu yeri hafızaya kaydeder tekrar çağırıldığında kaldığı yerden devam eder.

`for` döngüsü aslında bunları arka planda yapıyor. `for x in nums` yazınca Python:

1. `iter(nums)` ile iterator alır
2. `next()` çağırarak ilerler
3. `StopIteration` gelince durur

---

lemek yerine, generator ile her seferinde bir eleman üretebilirsin. Büyük dosyaları satır satır okumak, veri pipeline'ları, streaming işlemleri için çok kullanışlı.

---

### Generator Expression

Liste comprehension gibi ama `[]` yerine `()`:

```python
kare_listesi = [x**2 for x in range(10)]    # tüm liste belleğe alınır
kare_gen     = (x**2 for x in range(10))    # generator, lazy

# ikisi de aynı sonucu verir ama bellek kullanımı farklı
```

---

Kısacası: `iter()` + `next()` mekanizmasını anlayınca, `for` döngüsünün, `zip`, `map`, `enumerate` gibi built-in'lerin nasıl çalıştığı da netleşiyor. Hepsi iterator protokolü üzerine kurulu.

Çoğu durumda yield kullanmak gerekecek ama sınıflar üzeridne genel bir sistem kurmak istiyorsanızda ufak bir ıterator metodu halledecektir.

**Sınıf yazmak zorunda kaldığın durumlar:**

1. **State tutman gerekiyorsa** — birden fazla metot, reset özelliği vs.

```python
class CountUp:
    def reset(self):
        self.current = 0  # bunu generator'da yapamazsın
```

2. **`__len__` veya başka dunder metotlar gerekiyorsa**


```python
class MyRange:
    def __len__(self): return self.limit  # generator'da yok
```

3. **İki yönlü iletişim gerekiyorsa** — `generator.send(value)` var aslında ama okunması zorlaşır, sınıf daha temiz olur.

---

Pratikte şunu bilmen yeterli:

- Basit veri üretimi → `yield`
- Karmaşık durum yönetimi → sınıf

> Bu kısımda kullancağımız genel metod sistemleri vardır aşağıda kullanımlarına bakınız.


## 1. Built-in Iterator Fonksiyonları

### `enumerate()`

```python
isimler = ["ali", "veli", "ayşe"]

for i, isim in enumerate(isimler):
    print(i, isim)
# 0 ali
# 1 veli
# 2 ayşe

# başlangıç indeksi değiştirebilirsin
for i, isim in enumerate(isimler, start=1):
    print(i, isim)  # 1'den başlar
```

### `zip()`

```python
isimler = ["ali", "veli", "ayşe"]
yaslar = [20, 25, 22]

for isim, yas in zip(isimler, yaslar):
    print(isim, yas)
# ali 20
# veli 25
# ayşe 22

# kısa olan bitince durur
# zip_longest ile eşit uzunlukta olmalarına gerek kalmaz (itertools'da)
```

### `map()`

```python
sayilar = [1, 2, 3, 4]

kareler = map(lambda x: x**2, sayilar)
print(list(kareler))  # [1, 4, 9, 16]

# lambda yerine fonksiyon da verebilirsin
def karesi(x):
    return x**2

kareler = map(karesi, sayilar)
```

### `filter()`

```python
sayilar = [1, 2, 3, 4, 5, 6]

ciftler = filter(lambda x: x % 2 == 0, sayilar)
print(list(ciftler))  # [2, 4, 6]
```

`map` ve `filter` lazy'dir — `list()` ile sarmadan hesaplamaz. BUrdaki metodlar sonucu hemen döndürmeyip bellekte sakladığı için list ile bellekten alırız.

---

## 2. Iterator Metodları

### `next()` — default değer


```python
it = iter([1, 2, 3])

print(next(it))           # 1
print(next(it, "bitti"))  # 2
print(next(it, "bitti"))  # 3
print(next(it, "bitti"))  # "bitti" — exception fırlatmaz
```

### `send()` — generator'a değer göndermek


```python
def toplayici():
    toplam = 0
    while True:
        sayi = yield toplam  # hem değer verir hem alır
        toplam += sayi

gen = toplayici()
next(gen)        # başlatmak için bir kere next çağırman lazım

print(gen.send(10))  # 10
print(gen.send(5))   # 15
print(gen.send(3))   # 18
```

`yield` iki yönlü çalışıyor: sağ tarafı dışarı verir, `send()` ile gelen değeri sol tarafa atar.

### `throw()` — generator'a exception göndermek

python

```python
def gen():
    try:
        yield 1
        yield 2
    except ValueError:
        yield "hata yakalandı"

g = gen()
print(next(g))               # 1
print(g.throw(ValueError))   # "hata yakalandı"
```

### `close()` — generator'ı kapatmak

python

```python
def sayac():
    try:
        i = 0
        while True:
            yield i
            i += 1
    except GeneratorExit:
        print("generator kapandı")

g = sayac()
print(next(g))  # 0
print(next(g))  # 1
g.close()       # "generator kapandı"
```

---

## 3. `yield from`

Başka bir iterable'ı generator'a gömmek için:

python

```python
# yield from olmadan
def generator1():
    for x in [1, 2, 3]:
        yield x
    for x in [4, 5, 6]:
        yield x

# yield from ile
def generator2():
    yield from [1, 2, 3]
    yield from [4, 5, 6]

print(list(generator2()))  # [1, 2, 3, 4, 5, 6]
```

İç içe generator'larda çok işe yarıyor:

python

```python
def ic_gen():
    yield 1
    yield 2

def dis_gen():
    yield from ic_gen()  # ic_gen'i tamamen delege eder
    yield 3

print(list(dis_gen()))  # [1, 2, 3]
```

---

## 4. `itertools` Modülü

python

```python
import itertools
```

### `chain()` — iterableleri birleştir

python

```python
a = [1, 2, 3]
b = [4, 5, 6]

for x in itertools.chain(a, b):
    print(x)  # 1 2 3 4 5 6
```

### `islice()` — lazy slice

python

```python
def sonsuz():
    n = 0
    while True:
        yield n
        n += 1

# sonsuz generatordan ilk 5 elemanı al
for x in itertools.islice(sonsuz(), 5):
    print(x)  # 0 1 2 3 4
```

### `product()` — kartezyen çarpım

python

```python
for x, y in itertools.product([1, 2], ["a", "b"]):
    print(x, y)
# 1 a
# 1 b
# 2 a
# 2 b

# iç içe for döngüsünün kısa hali
```

### `combinations()` ve `permutations()`

python

```python
harfler = ["a", "b", "c"]

list(itertools.combinations(harfler, 2))
# [('a','b'), ('a','c'), ('b','c')]  — sıra fark etmez

list(itertools.permutations(harfler, 2))
# [('a','b'), ('a','c'), ('b','a'), ('b','c'), ('c','a'), ('c','b')]  — sıra fark eder
```

### `groupby()` — gruplama

python

```python
data = [
    {"isim": "ali", "sehir": "ankara"},
    {"isim": "veli", "sehir": "ankara"},
    {"isim": "ayşe", "sehir": "istanbul"},
]

# önce sıralaman lazım
data.sort(key=lambda x: x["sehir"])

for sehir, grup in itertools.groupby(data, key=lambda x: x["sehir"]):
    print(sehir, list(grup))
```

### `cycle()` ve `repeat()`

python

```python
# sonsuza kadar döner
for x in itertools.islice(itertools.cycle([1, 2, 3]), 7):
    print(x)  # 1 2 3 1 2 3 1

# n kere tekrar eder
for x in itertools.repeat("merhaba", 3):
    print(x)  # merhaba merhaba merhaba
```

---

## Özet Tablosu

### Python İterasyon ve Generator Araçları

|**Metot / Fonksiyon**|**Açıklama**|**Kategorisi**|
|---|---|---|
|**`next(it, default)`**|İteratörün sonraki elemanını getirir; eğer bitmişse `default` değeri döner.|Temel İterasyon|
|**`send(val)`**|Generator'a (üreteç) dışarıdan bir değer gönderir ve yürütmeyi devam ettirir.|Generator Kontrol|
|**`throw(exc)`**|Generator içinde belirtilen noktada bir hata (exception) fırlatır.|Generator Kontrol|
|**`close()`**|Generator'ın çalışmasını sonlandırır ve kapatır.|Generator Kontrol|
|**`yield from`**|Başka bir iterable veya generator'ı doğrudan delege eder (aktarır).|Generator Akışı|
|**`map` / `filter`**|Fonksiyonları verilere uygular (Lazy/Tembel değerlendirme ile).|Dönüştürme / Filtreleme|
|**`zip` / `enumerate`**|Birden fazla iterable'ı paralel yürütür veya elemanlara indeks ekler.|Döngü Yardımcıları|
|**`itertools.chain`**|Birden fazla iterable'ı tek bir uzun diziymiş gibi birleştirir.|İtertools (Birleştirme)|
|**`itertools.islice`**|Belleği yormadan (lazy) belirli bir aralıkta dilimleme yapar.|İtertools (Dilimleme)|
|**`itertools.product`**|Girdilerin Kartezyen çarpımını üretir.|İtertools (Kombinasyon)|
|**`combinations` / `permutations`**|Verilen veriden kombinasyon veya permütasyon dizileri oluşturur.|İtertools (Kombinasyon)|
|**`itertools.groupby`**|Ardışık gelen benzer elemanları gruplandırır.|İtertools (Gruplama)|
|**`cycle` / `repeat`**|Bir iterable'ı sonsuza kadar tekrar eder veya bir değeri belirli sayıda basar.|İtertools (Sonsuz Döngü)|