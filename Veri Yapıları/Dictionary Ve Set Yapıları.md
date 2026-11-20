## Dictionary

Dict key value ilişkisi bulunan değerler saklamamızı sağlar. Sıralı bir veri yapısıdır. Her key benzersizdir. 

- Eleman eklenebilir silinebilir 
- keyler benzersizdir. 
- Keyler hashable olmalıdır. int str tuple olabilirler 
- Valuelar herhangi bir tip olabilir 



```python 

kisi={
"isim":"Talha",
"Yaş":20,
"Boy":1.90
}
```
### 3. Erişim, Ekleme, Silme
```python
d = {"a": 1, "b": 2, "c": 3}

# Erişim
print(d["a"])          # 1
print(d.get("x"))       # None (hata vermez, key yoksa)
print(d.get("x", 0))    # 0 (varsayılan değer)

# Var mı kontrolü
print("a" in d)         # True

# Silme
del d["a"]
d.pop("b")               # değeri döndürür ve siler
d.popitem()               # son eklenen çifti siler (LIFO)

# Güncelleme
d.update({"d": 4, "e": 5})   # birden fazla ekleme/güncelleme
```



## Set 
- Set benzersiz sırasız elemanlardan oluşan mutable bir koleksiyondur. Matematiksle bir kümeye benzer 
- Tekrarlar otomatik elenir 
- Genellikle iki küme arasındaki matematiksel ifadelerde kullanılır
```python 
a = {1, 2, 3, 4}
b = {3, 4, 5, 6}

print(a | b)    # birleşim (union)         -> {1,2,3,4,5,6}
print(a & b)    # kesişim (intersection)   -> {3,4}
print(a - b)    # fark (difference)        -> {1,2}
print(a ^ b)    # simetrik fark (XOR)      -> {1,2,5,6}

# metot karşılıkları
a.union(b)
a.intersection(b)
a.difference(b)
a.symmetric_difference(b)
```

Ekleme Silme
```python 
s = {1, 2, 3}

s.add(4)          # tek eleman ekle
s.update([5, 6])  # birden fazla eleman ekle

s.remove(2)        # eleman yoksa HATA verir
s.discard(10)       # eleman yoksa HATA vermez, sessizce geçer
s.pop()              # rastgele bir elemanı çıkarır (sırasız olduğu için hangi eleman belirsiz)
s.clear()            # tümünü temizler
```

###  `frozenset` — Set'in Immutable Versiyonu

```python
fs = frozenset([1, 2, 3])
fs.add(4)          # HATA! frozenset immutable

# Bu yüzden dict key'i veya set içinde eleman olabilir
s = {frozenset([1,2]), frozenset([3,4])}
```