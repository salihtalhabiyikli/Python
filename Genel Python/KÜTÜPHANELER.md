
### DATETİME MODÜLÜ
Zaman damgaları kullanan bir kütüphanedir.
İçinde bulunduğumuz zamanı ekrana yazdırır.

```python 
import datetime 
result=dir(datetime)


```

Genel method kaynakları ve terminoloji için 
https://share.google/TJh6dxsDaYXo0Gcjg
https://share.google/GtHeSxidosQCBLstx
https://docs.python.org/tr/3.14/library/datetime.html#module-datetime


### .OS MODÜLÜ 
-  İşletim sistmei yönetimi üzerine çalışan bir kütüphanedir 
```python 
importy os 
result=dir(os)
print(result)
```
ifadesi bize işletim sistemi bilgilerimizi verecektir 

-  Dosya yolu ve kopyalama yapabilmektedir. Dizin üzerine dosya açma işlemlerini gerçekleştirebilir.
- Karar yapılarını okuyarak filtreleme veya hedef dosyayı değiştirme yapabilir 
- Uygulamaları komutlarla çalıştırabilir.
- Dosya isimlerini yönetebilir.

#### PATH:
- Bu modülde os modelinin içinde bulunur ve dosya uzantıları ve konumlandırma üzerinde geliştirilmiştir.
- Bilinmeyen dosyaları tarayabilir.
Kaynaklar 
https://docs.python.org/tr/3.14/library/os.html#module-os
https://pypi.org/search/?q=os
https://www.w3schools.com/python/module_os.asp
https://www.reddit.com/r/learnpython/comments/wj4y4x/what_is_os_module/
https://www.codecademy.com/resources/docs/python/os-module

### RE MODÜLÜ (REGULAR EXPRESSİON )
Genel bir modüldür tüm dillerde çalışır.
Genel metinlerde arama eşleştirme ayıklama veya değiştirme yapabilir 

Gelen verileri ayıklayabilir,Telefon numarası bulabilir , Loglama yaparak tarih çekebilir, Karar yapılarını bozabilir

Match ve None ifadelerini kullanır bunlar 1,0 parametrelidir.

Bu doküman, Python `re` modülünde kullanılan **regex (regular expression)** sembollerini **mantık + kullanım** odaklı olarak toplu halde sunar. Amaç **ezber değil**, gördüğünde tanıma ve gerektiğinde doğru deseni kurabilme becerisi kazandırmaktır.

---

## 1. Regex Nedir?

Regex, metin içinde **belirli bir desene uyan** kısımları aramak, eşleştirmek, ayıklamak veya değiştirmek için kullanılan bir ifade dilidir.

Python’da `re` modülü ile kullanılır:

```python
import re
```

---

## 2. En Temel Regex Karakterleri (Character Classes)

|Sembol|Anlam|Örnek|
|---|---|---|
|`.`|Herhangi bir karakter (satır sonu hariç)|`a.c` → abc, a1c|
|`\d`|Rakam (0–9)|`\d\d` → 42|
|`\D`|Rakam olmayan|`\D+`|
|`\w`|Harf + rakam + `_`|`\w+`|
|`\W`|`\w` dışı||
|`\s`|Boşluk (space, tab, newline)||
|`\S`|Boşluk olmayan||

> Not: Python’da regex stringleri **raw string** (`r"..."`) olarak yazmak güvenlidir.

---

## 3. Tekrar (Quantifiers)

Bir desenin **kaç kez** tekrar edileceğini belirtir.

|Sembol|Anlam|Örnek|
|---|---|---|
|`*`|0 veya daha fazla|`a*`|
|`+`|1 veya daha fazla|`a+`|
|`?`|0 veya 1|`colou?r`|
|`{n}`|Tam n kez|`\d{4}`|
|`{n,}`|En az n|`\d{2,}`|
|`{n,m}`|n–m arası|`\d{2,4}`|

---

## 4. Konum Belirleyiciler (Anchors)

Metnin **neresinde** arama yapılacağını belirtir.

|Sembol|Anlam|
|---|---|
|`^`|Metnin başı|
|`$`|Metnin sonu|
|`\b`|Kelime sınırı|
|`\B`|Kelime sınırı olmayan|

Örnek:

```python
re.search(r"^Merhaba", text)
```

---

## 5. Karakter Kümeleri (Character Sets)

|Desen|Anlam|
|---|---|
|`[abc]`|a, b veya c|
|`[a-z]`|Küçük harfler|
|`[A-Z]`|Büyük harfler|
|`[0-9]`|Rakam|
|`[^abc]`|a, b, c dışı|

---

## 6. Gruplama ve Yakalama (Groups)

Parantezler `()` ile gruplar oluşturulur.

```python
match = re.search(r"(\d{4})-(\d{2})-(\d{2})", text)
match.group(1)  # yıl
```

|Sembol|Anlam|
|---|---|
|`( )`|Yakalama grubu|
|`(?: )`|Yakalamayan grup|

---

## 7. Alternatifler (OR işlemi)

|Sembol|Anlam|
|---|---|
|`|`|

```python
re.search(r"kedi|köpek", text)
```

---

## 8. Lookaround (İleri Seviye – Tanıman Yeterli)

|Desen|Anlam|
|---|---|
|`(?=...)`|Pozitif lookahead|
|`(?!...)`|Negatif lookahead|
|`(?<=...)`|Pozitif lookbehind|
|`(?<!...)`|Negatif lookbehind|

> Bunlar ezberlenmez, gerektiğinde bakılır.

---

## 9. Regex Bayrakları (Flags)

|Flag|Anlam|
|---|---|
|`re.I`|Büyük/küçük harf duyarsız|
|`re.M`|Çok satırlı mod|
|`re.S`|`.` newline dahil|

```python
re.search(pattern, text, re.I)
```

---

## 10. Python `re` Fonksiyonları – Kısa Özet

|Fonksiyon|Ne yapar|
|---|---|
|`re.search()`|Metnin her yerinde arar|
|`re.match()`|Baştan arar|
|`re.findall()`|Tüm eşleşmeleri liste döner|
|`re.finditer()`|Iterator döner|
|`re.sub()`|Değiştirir|
|`re.split()`|Böler|

---

## 11. Gerçekçi Örnekler

### 📧 Email yakalama (basit)

```python
r"\w+@\w+\.\w+"
```

### 📅 Tarih yakalama

```python
r"\d{4}-\d{2}-\d{2}"
```

### 📞 Telefon

```python
r"0\d{10}"
```

---
Genel kaynak olarak çoğu yazılımcı Google ve regex101 kullanarak bu modülü kullanır 

Kaynaklar 
https://docs.python.org/tr/3.14/library/re.html#module-re
https://www.w3schools.com/python/python_regex.asp
https://realpython.com/regex-python-part-2/
https://regex101.com/


### JSON MODÜLÜ
 - JSON ( Javascript Object Notation ) Javascritp nesneerine verilen genel addır.
 - Sadece Javascripte değil neredeyse her dilde veri akışı sunabilen bir yapıdır 
 - Web tarayıcılarında yaptığı mükemmel ayrıştırmalar sayesinde bu kadar popüler olmuştur.
``` json 
{
  "greeting": "Hello, world!"
}
```
Basit syntaxa sahiptir hem insanlar hemde bilgisayarlar tarafından rahatlıkla okunurlar.

> JSON tamamen sözel metine dayalı bir veri aracıdır.

Pythonda JSON formatını daha çok bir veri seti gibi kullanırız. NumPy,Matplotlib gibi kütüphanelerin veri setlerini okuma mantığıda aslında c dilinde okunabilen bir jsona dayanmaktadır.

Daha karmaşık bir json dosyasına bakalım 2 
```json
{
  "name": "Frieda",
  "isDog": true,
  "hobbies": ["eating", "sleeping", "barking"],
  "age": 8,
  "address": {
    "work": null,
    "home": ["Berlin", "Germany"]
  },
  "friends": [
    {
      "name": "Philipp",
      "hobbies": ["eating", "sleeping", "reading"]
    },
    {
      "name": "Mitch",
      "hobbies": ["running", "snacking"]
    }
  ]
}
```


| JSON Veri Türü | Tanım                                                                                        |
| -------------- | -------------------------------------------------------------------------------------------- |
| `object`       | `{}`Süslü parantezler ( ) içinde anahtar-değer çiftlerinden oluşan bir koleksiyon.           |
| `array`        | `[]`Köşeli parantezler ( ) içine alınmış değerler listesi                                    |
| `string`       | Çift tırnak içine alınmış metin ( `""`)                                                      |
| `number`       | Tamsayılar veya ondalık sayılar                                                              |
| `boolean`      | Tırnak işaretleri olsun `true`veya `false`olmasın.                                           |
| `null`         | [Boş değeri](https://realpython.com/null-in-python/) temsil eder , şu şekilde yazılır:`null` |
> JSON dosyaları pythonda tanımladığımız dictonary yapısına benzemektedir.


## Veri Tipi vs Format

**Dictionary**: Python'a özgü bir veri yapısı. Sadece Python kodunun çalıştığı bellekte var olur.

**JSON**: Metin tabanlı bir veri formatı. Farklı programlama dilleri ve sistemler arasında veri paylaşımı için kullanılır.

```python
# Dictionary - Python'da direkt kullanılır
user = {
    "isim": "Ali",
    "yas": 25
}
print(user["isim"])  # Direkt erişim
```

```python
import json

# JSON - string formatında, parse edilmesi gerek
json_string = '{"isim": "Ali", "yas": 25}'
user = json.loads(json_string)  # String'i dict'e çevir
print(user["isim"])
```
**Dictionary** daha esnektir, şunları içerebilir:

- Tuple, set gibi Python objeleri
- Fonksiyonlar
- Custom class'lar
- `None`, `True`, `False` (Python syntaxı)

**JSON** daha kısıtlıdır, sadece:

- String, number, boolean, null
- Array (list), object (dict)
- `null`, `true`, `false` (küçük harfle)
## Ne Zaman Hangisi?
- **Dictionary kullan**: Python içinde veri saklarken/işlerken
- **JSON kullan**: Dosyaya kaydederken, API'ye gönderirken, başka programlarla paylaşırken

> JSON dosyaları python sözlüklerinde veya başka dillerde objelerde olduğu gibi birgül,nokta yorum satırı  desteklemez ve hata verir.

Aslında ayrı bir JSON dosyası yazmamıza gerek yoktur. Çünkü Pythondaki JSON modülü python kullanarak json yazmamıza olanak sağlamaktadır .
Yazılan verilerin json formatına dönüştürülmesine serileşirme deniliyor. 

Bu formlar geri alınabilmektedir. Yani JSON formatındaki dosyalarıda python formatına çevirebilirsiniz.

### PYTHON-JSON DÖNŞÜMÜ 

```python 
import json # Kütüphane ekleme
food_ratings=("Organşc dog food":2, "human food": 10) # dictionary
json.dumps(food_ratings) # üstteki dictionary yapısını jsona çeviren kısım
```
Bu modül, yaygın Python veri türlerini  JSON'a dönüştürmenizi sağlar . İşte JSON değerlerine dönüştürebileceğiniz tüm Python veri türleri ve değerlerine genel bir bakış:[
|Python    |JSON|
	|---|---|
	|`dict`| `object`|
	|`list`|`array`|
	|`tuple`|`array`|
	|`str`|`string`|
	|`int`|`number`|
	|`float`|`number`|
	|`True`|`true`|
	|`False`|`false`|
	|`None`|`null`|
	
Python'da liste ve demet gibi farklı veri türlerinin aynı JSON veri türüne dönüştürüldüğünü unutmayın . Bu durum, JSON verilerini Python'a geri dönüştürürken sorunlara yol açabilir, çünkü veri türü eskisiyle aynı olmayabilir. Bu kısımda alacağımız hatanın adı skipkeys.tir 

Javasciript veri tiplerine erişmek için bkz. https://javascript.info/types

#### ÖRNEK DÖNÜŞÜMLER 
  json.loads : Json formatını dictionary formatına çevirir. 

- Dosyayla Açarken:
```python 
import json 
with open("DosyaAdi.json") as f
data= json.load(f)
print(data["Name"])
```

Daha detaylı anlatımlar için bkz. https://www.w3schools.com/python/python_json.asp



