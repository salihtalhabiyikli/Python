### 1. Tuple — Değiştirilemez, Sıralı

python

```python
t = (1, 2, 3)
```

- **Immutable, sıralı, tekrar eleman içerebilir**
- Bellekte: header + **tam** eleman sayısı kadar pointer, tek bitişik blok, fazladan yer yok
- Hashable → dict key / set elemanı olabilir
- Tek elemanlı tuple: `(5,)` — virgül zorunlu

**✅ Avantajlar:**

- List'e göre daha az bellek (over-allocation yok)
- Oluşturma daha hızlı (fazladan kapasite hesabı yok)
- Immutable olduğu için **thread-safe** — birden fazla iş parçacığı güvenle okuyabilir
- Dict key / set elemanı olabilir

**❌ Dezavantajlar:**

- Değiştirilemez, her "değişiklik" = yeni nesne oluşturma
- `t = t + (x,)` ile tekrar tekrar büyütmek **O(n²) tuzağı** yaratır (her seferinde tüm veri kopyalanır)
- Metot sayısı çok kısıtlı (`count`, `index`)

**🔧 Optimizasyon:** Tekrar tekrar büyütme gerekiyorsa önce `list` kullan, sonunda `tuple()`'a çevir.

**Ne zaman:** Sabit kalacak veri (koordinat, RGB), fonksiyon çoklu dönüş değeri, dict key.

---

### 2. List — Değiştirilebilir, Sıralı

python

```python
l = [1, 2, 3]
```

- **Mutable, sıralı, tekrar eleman içerebilir**
- Bellekte: header ayrı + item dizisine pointer, **over-allocation** var
- Sona ekleme amortized O(1), **başa/ortaya ekleme O(n)**

**✅ Avantajlar:**

- Index erişim O(1) — anında ulaşım
- Esnek: farklı tip elemanlar tutabilir, dinamik büyür/küçülür
- Sona ekleme/çıkarma amortized O(1) — çoğu zaman ucuz
- Zengin metot seti (`sort`, `insert`, `remove`, slicing vb.)

**❌ Dezavantajlar:**

- Fazladan bellek kullanır (over-allocation + her eleman pointer + nesne overhead)
- Başa/ortaya ekleme-çıkarma O(n) — büyük listede pahalı
- Değere göre arama O(n) — set/dict'e göre çok yavaş
- Hashable değil, dict key olamaz

**🔧 Optimizasyon:**

python

```python
# KÖTÜ: sık başa ekleme
l = []
for i in range(100000):
    l.insert(0, i)     # her seferinde O(n) kaydırma -> toplamda O(n²)

# İYİ: sık başa/sona ekleme gerekiyorsa deque kullan
from collections import deque
d = deque()
for i in range(100000):
    d.appendleft(i)      # O(1)

# Sık "eleman var mı" kontrolü yapılacaksa, list yerine set'e çevir
buyuk_liste = list(range(1000000))
buyuk_set = set(buyuk_liste)   # bir kere dönüştür, sonra hızlı ara
```

**Ne zaman:** Sık eleman eklenip çıkarılacak, sıralı, homojen olması gerekmeyen veri.

---

### 3. Dict — Anahtar-Değer

python

```python
d = {"isim": "Ahmet", "yas": 25}
```

- **Mutable**, key'ler benzersiz ve hashable, value herhangi bir tip
- Python 3.7+ ekleme sırasını korur
- İçte hash tablosu → O(1) arama/ekleme/silme

**✅ Avantajlar:**

- Arama/ekleme/silme **O(1)** — veri büyüklüğünden bağımsız hız
- `get()` ile güvenli erişim (KeyError riski yok)
- Canlı view'lar (`keys()`, `values()`, `items()`) — kopyalama maliyeti yok
- Ekleme sırası korunur (3.7+)

**❌ Dezavantajlar:**

- List/tuple'a göre daha fazla bellek (hash tablosu, doluluk oranı düşük tutulur)
- Key'ler hashable olmak zorunda (list, dict key olamaz)
- Sıralamaya göre erişim/işlem doğal değildir (index yok)
- Çok büyük veri setlerinde resize/rehash anları pahalı olabilir (nadir ama O(n))

**🔧 Optimizasyon:**

python

```python
# KÖTÜ: key var mı diye önce kontrol, sonra işlem
if "sayac" not in d:
    d["sayac"] = 0
d["sayac"] += 1

# İYİ: defaultdict ile kontrolü ortadan kaldır
from collections import defaultdict
d = defaultdict(int)
d["sayac"] += 1    # otomatik 0'dan başlar

# Sabit key setiyle sık erişim varsa dict.fromkeys() ile hızlı başlat
d = dict.fromkeys(["a", "b", "c"], 0)
```

**Ne zaman:** Anahtar-değer ilişkisi olan her veri, hızlı lookup gereken durumlar.

---

### 4. Set — Benzersiz, Sırasız

python

```python
s = {1, 2, 3}
```

- **Mutable**, elemanlar benzersiz ve hashable, sırasız
- Dict ile aynı hash tablosu mantığı, value yok
- Arama, ekleme, silme O(1)

**✅ Avantajlar:**

- Arama (`in`) **O(1)** — list'e göre büyük veri setlerinde muazzam fark
- Küme operasyonları (`|`, `&`, `-`, `^`) native ve hızlı
- Otomatik tekrar eleme (deduplication) tek satırda
- İki veri grubunu karşılaştırmak (ortak/fark bulma) çok pratik

**❌ Dezavantajlar:**

- List/tuple'dan **en fazla bellek** kullanan yapı (doluluk oranı düşük tutulur, ~%40+ boş slot)
- Sırasız — index ile erişim yok, sıralama garantisi yok
- Elemanlar hashable olmak zorunda (list eleman olamaz)
- `pop()` hangi elemanı sileceği garanti değil

**🔧 Optimizasyon:**

python

```python
# KÖTÜ: iki listede ortak elemanları bulmak için iç içe döngü
ortak = []
for x in liste1:
    for y in liste2:
        if x == y:
            ortak.append(x)   # O(n*m)

# İYİ: set kesişimi kullan
ortak = set(liste1) & set(liste2)   # O(n+m)

# Sadece "var mı" kontrolü sık yapılacaksa, veriyi baştan set olarak tut
yasakli = {"kötü", "yasak", "spam"}   # list değil, doğrudan set
```

**Ne zaman:** Tekrar eleme, hızlı üyelik kontrolü, iki veri grubunu karşılaştırma.

---

### 5. `collections` Modülü — Özelleştirilmiş Versiyonlar

|Yapı|Temel|Avantaj|Dezavantaj|
|---|---|---|---|
|`namedtuple`|tuple|Okunabilirlik + tuple'ın hafifliği|Yine immutable, alan sayısı sabit|
|`Counter`|dict (miras)|Sayma işlemleri tek satırda (`most_common`)|Genel dict'ten biraz daha fazla overhead|
|`defaultdict`|dict (miras)|`KeyError` kontrolünü ortadan kaldırır|Yanlışlıkla var olmayan key'i "oluşturabilir" (dikkat gerektirir)|
|`OrderedDict`|dict (miras)|Sıraya duyarlı eşitlik, `move_to_end()`|3.7+ dict zaten sıralı, çoğu zaman gereksiz|
|`ChainMap`|çoklu dict görünümü|Config/ayar önceliklendirme kolay|Performans olarak normal dict kadar hızlı değil|
|`deque`|bağımsız yapı|Her iki uçta O(1)|Ortadan index erişim O(n)|

python

```python
from collections import namedtuple, Counter, defaultdict, deque
```

---

### 6. Stack (Yığın) — LIFO

python

```python
stack = []
stack.append(1)   # push
stack.pop()         # pop
```

**✅ Avantajlar:**

- List ile ekstra kütüphaneye gerek kalmadan uygulanır
- Sonda push/pop O(1) — çok verimli

**❌ Dezavantajlar:**

- Yanlışlıkla `insert(0, x)` veya `pop(0)` kullanılırsa O(n)'e düşer (stack mantığı bozulur)
- Çok derin rekürsiyonda (stack overflow riski) dikkat gerekir

**🔧 Optimizasyon:** Her zaman `append`/`pop` (sondan) kullan, asla `insert(0,...)` ile karıştırma.

**Kullanım:** Call stack, undo/redo, parantez kontrolü, DFS, tarayıcı geçmişi.

---

### 7. Queue (Kuyruk) — FIFO

python

```python
from collections import deque
kuyruk = deque()
kuyruk.append(1)      # enqueue
kuyruk.popleft()        # dequeue
```

**✅ Avantajlar:**

- Her iki uçta da O(1) — list'in zayıf noktasını (başa ekleme O(n)) tamamen çözer
- Blok bazlı yapı sayesinde cache-dostu (saf linked list'ten daha hızlı)
- `maxlen` parametresiyle otomatik sabit boyutlu kuyruk yapılabilir (`deque(maxlen=100)`)

**❌ Dezavantajlar:**

- Ortadan index erişim O(n) — `d[500]` gibi erişim list kadar hızlı değil
- List'e göre biraz daha fazla bellek overhead'i (blok yapısı)

**🔧 Optimizasyon:** Queue için ASLA `list.pop(0)` kullanma — mutlaka `deque.popleft()`.

**Kullanım:** Mesaj kuyrukları, görev zamanlama, BFS, yazıcı kuyruğu.

---

### 8. Linked List — Bağlı Liste

python

```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None
```

**✅ Avantajlar:**

- Ortadan/baştan ekleme-çıkarma O(1) (node referansı elde varsa)
- Bellek, tam ihtiyaç kadar (over-allocation yok)

**❌ Dezavantajlar:**

- Index ile erişim O(n) — list'in tam tersi
- Cache-unfriendly — dağınık bellek, CPU önbelleğinden faydalanamaz
- Python'da ekstra nesne overhead'i (her node bir Python objesi) yüzünden pratikte list'ten bile yavaş kalabilir
- Kendi yazman gerekir, yerleşik/optimize değil

**🔧 Optimizasyon:** Python'da saf linked list yazmak yerine, aynı ihtiyacı karşılayan optimize edilmiş `deque` kullan.

**Kullanım:** Algoritma eğitimi, LRU cache, bazı ağaç/graf yapıları.

---

### 9. Array (Dizi) — C'ye En Yakın Karşılık

python

```python
from array import array
a = array('i', [1, 2, 3])
```

**✅ Avantajlar:**

- List'e göre çok daha az bellek (pointer katmanı yok, ham değer)
- Aynı tip veri için daha hızlı işlem

**❌ Dezavantajlar:**

- Tip karışımı yapılamaz (homojen olmak zorunda)
- Metot desteği list'e göre kısıtlı
- Günlük kullanımda nadiren tercih edilir

**🔧 Optimizasyon:** Büyük ölçekli sayısal işlemler için `array` yerine doğrudan **NumPy** kullan — vectorization ile çok daha hızlı.

**Kullanım:** Bellek kritik, tek tip sayısal veri; ama pratikte yerini NumPy alır.

---

### 10. Genel Performans Tablosu

|İşlem|list|tuple|dict|set|deque|
|---|---|---|---|---|---|
|Index erişim|O(1)|O(1)|—|—|O(n)*|
|Arama (`in`)|O(n)|O(n)|O(1)|O(1)|O(n)|
|Sona ekleme|O(1)|—|O(1)|O(1)|O(1)|
|Başa ekleme|O(n)|—|—|—|**O(1)**|
|Silme|O(n)|—|O(1)|O(1)|O(1)**|
|Bellek kullanımı|Orta|Az|Fazla|En fazla|Orta-fazla|

*deque'de ortadan erişim O(n), uçlardan O(1)  
**deque'de uçlardan silme O(1)

---

### 11. Genel Optimizasyon Kontrol Listesi

Kod yazarken şu kontrolleri otomatik yap:

- [ ]  Sık `in` kontrolü yapıyorsan → `list` yerine `set`/`dict` kullan
- [ ]  Başa/sona sık ekleme-çıkarma yapıyorsan → `list` yerine `deque` kullan
- [ ]  Veri hiç değişmeyecekse → `list` yerine `tuple` kullan
- [ ]  Key yoksa hata almak istemiyorsan → normal `dict` yerine `defaultdict`
- [ ]  Aynı hesaplamayı tekrar tekrar yapıyorsan → `@lru_cache` ekle
- [ ]  Büyük sayısal veri işliyorsan → `list`/`array` yerine NumPy
- [ ]  İç içe döngü ile karşılaştırma yapıyorsan → `set` kesişimi kullanmayı düşün

---

### 12. Ana Prensip

> **Her veri yapısı bir şeyi kazanırken başka bir şeyden feragat eder — ya bellekten, ya da işlem zamanından.**

```
Bitişik bellek (list, tuple, array) -> hızlı okuma, pahalı değiştirme
Dağınık + pointer (linked list)      -> ucuz değiştirme, yavaş okuma  
Fazladan bellek (set, dict)           -> hızlı arama, bellek israfı
```