
## List
Listeler pythonda birden fazla veriyi tek bir değişkende tutmamıza yarar. 
Değiştirilebilir bir veriyapısıdır farklı veri tiplerine sahip karakterleri aynı listede tutabilirsiniz. 

```python 
sayilar= [1,2,3,4,5]
```

> Temel Özellikler 
- Elemanlar eklenme sırasını korur 
- Değiştirilebilirlerdir. 
- İndekselenebilirlerdir 

### İndeksleme ve Dilimleme (Slicing)
```python
liste = [10, 20, 30, 40, 50]

liste[0]      # 10 (ilk eleman)
liste[-1]     # 50 (son eleman)
liste[1:4]    # [20, 30, 40] -> 1'den 4'e kadar (4 dahil değil)
liste[:3]     # [10, 20, 30] -> baştan 3. indekse kadar
liste[2:]     # [30, 40, 50] -> 2. indeksten sona kadar
liste[::-1]   # [50, 40, 30, 20, 10] -> tersten sıralar
liste[::2]    # [10, 30, 50] -> ikişer atlayarak
```

### İndeksleme ve Dilimleme (Slicing)
```python
liste = [10, 20, 30, 40, 50]

liste[0]      # 10 (ilk eleman)
liste[-1]     # 50 (son eleman)
liste[1:4]    # [20, 30, 40] -> 1'den 4'e kadar (4 dahil değil)
liste[:3]     # [10, 20, 30] -> baştan 3. indekse kadar
liste[2:]     # [30, 40, 50] -> 2. indeksten sona kadar
liste[::-1]   # [50, 40, 30, 20, 10] -> tersten sıralar
liste[::2]    # [10, 30, 50] -> ikişer atlayarak
```
### Sık Kullanılan Metotlar

| Metot              | Açıklama                                                                |
| ------------------ | ----------------------------------------------------------------------- |
| `append(x)`        | Sona tek eleman ekler                                                   |
| `insert(i, x)`     | Belirli indekse eleman ekler                                            |
| `extend(iterable)` | Başka bir listeyi/iterable'ı sonuna ekler                               |
| `remove(x)`        | İlk bulduğu `x` değerini siler                                          |
| `pop(i)`           | Belirtilen indeksteki elemanı siler ve döndürür (varsayılan son eleman) |
| `clear()`          | Tüm elemanları siler                                                    |
| `index(x)`         | `x` değerinin ilk indeksini döndürür                                    |
| `count(x)`         | `x` değerinin kaç kez geçtiğini sayar                                   |
| `sort()`           | Listeyi yerinde (in-place) sıralar                                      |
| `reverse()`        | Listeyi yerinde ters çevirir                                            |
| `copy()`           | Listenin sığ kopyasını (shallow copy) döndürür                          |
### Liste İşlemleri 
```python
a = [1, 2, 3]
b = [4, 5, 6]

a + b        # [1, 2, 3, 4, 5, 6] -> birleştirme
a * 2        # [1, 2, 3, 1, 2, 3] -> tekrarlama
3 in a       # True -> eleman var mı kontrolü
len(a)       # 3 -> uzunluk
```

#### List Comprehension 

Pythona özgü bir liste üretme yoludur 

```python 
kareler=[x**2 for x in rage(10) ]
ciftler= [x for x in range(20) if x%2 ==0] 
```

### Listeleri Ölçeklenmesi 
Pythonda listelerin boyutu hazır belirlenmez bunun sebebi arkada çalışan bir ölçekleme sisteminin olmasındandır 
### Formül

```
yeni_kapasite = eski_kapasite + (eski_kapasite / 8) + (eski_kapasite < 9 ise 3, değilse 6)
```

Yani kabaca: **mevcut kapasitenin ~%12.5 fazlası + küçük bir sabit ek**.

### Somut Sayılarla Takip

|Dolu eleman|Eski kapasite|Yeni kapasite|Fazladan kaç yer açıldı|
|---|---|---|---|
|1. append|0|**3**|3 (boştan başladı)|
|4. append|3|**6**|3|
|7. append|6|**9**|3|
|10. append|9|**16**|7|
|17. append|16|**25**|9|
|26. append|25|**35**|10|
|36. append|35|**46**|11|

Gördüğün gibi kapasite **9'a kadar** hep +3 artıyor (küçük listelerde sabit ek baskın). 9'dan sonra sabit ek +6'ya çıkıyor ve büyüme oranı da (kapasitenin 1/8'i) devreye girmeye başlıyor, bu yüzden artışlar giderek büyüyor.



### Tuples 
Türkçesini demet olarak görürüz. listeyle amacı aynıdır fakat ayrılan bazı yanları vardır.  
Tuplellar sıralı ve değiştirilemez veriyapılarıdır. 
```python 
thistuple = ("apple", "banana", "cherry")  
print(len(thistuple))
```

Kısaca değiştireceğiniz ve sıralama yapacağınız yerlerde Listeler sabit dizilerde ise tuple kullanmanız gerekmeektedir.
