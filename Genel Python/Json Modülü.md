-  Json kelime anlamı olarak Javascript Object Notation anlamına glemektedir. 
- İnternette veri aktarımı olsun veyahut bazı programların ayar dosyaları olsun bunlar json olarak saklanır. 
- Önceden XML kullanılırken şuan daha hafif ve okunabilir olan Json kullanılmaktadır. 
- Json value-key templateine göre şekil almaktadır. 
```json 
{
  "ad": "Ahmet",
  "yas": 28,
  "ogrenciMi": false,
  "yetenekler": ["JavaScript", "Python", "SQL"],
  "adres": {
    "sehir": "İstanbul",
    "ulke": "Türkiye"
  }
}
```

Jsonun string ve integer değer duyarlılığı bulunmaktadır. 

JSON 2001 yılında bilgisayar bilimci Douglas Crockford tarafından tanımlanmıştır.  Döneminde web tarayıcılar ve sunucular arasında veri alışverisi yapmak için karmaşık ve ağır yapılar kullanılıyordu. Crockford flash veya java eklentilerine bağlı kalmadan web sayfalarının arka planda veri alıp vermesini sağlayacak daha hafif bir çözüm ararken Json formatını ortaya çıkarmıştır. 2000li yılların ortalarında Yahoo gibi büyük şirketlerin desteğiyle hızla yayılmış ve 2013 yılında resmi olarak ECMA standartı haline gelmiştir.

Json javascriptten doğsada hiçbir dilde sınırı yoktur. Her dil desteklemektedir 


#### Pythonda JSON Kullanımı 
```python 
import json
```


Bir python sözlüğünü json verisine çevirebiliriz 
```python 
import json 

kullanici={
"isim":"ahmet",
"yas":25
"aktif":True
}

veri=json.dumps(kullanici)
print(veri)
```
Burada önemli nokta:

- Python `True` → JSON `true`
- Python `False` → JSON `false`
- Python `None` → JSON `null`

`dumps` kelimesini **"dump string"** gibi düşünebilirsin: Python verisini JSON **stringine** dönüştürür.

 Json Verisini Python objesine çevirebiliriz 
```python 

import json 

jsonVerisi='{"isim":"ahmet" , "yas":"25 , "aktif" :true }'
kullanici=json.loads(jsonVerisi)

print(kullanici)
print(kullanici["isim"])
```
JSON string
    ↓
json.loads()
    ↓
Python dictionary


Jsonu bir  json dosyasına kaydetmek için dump kullanılır 
```python 
import json 

kullanici={
"isim":"Ahmet",
"yas":25,
"hobiler":["futbol","kitap"]
}

with open("kullanici.json","w",encoding="utf-8") as dosya:
	json.dump(kullanici,dosya,ensure_ascii=False,indent=4)
```

bu işlem sonucunda kullanici.json oluşur 
Burada:
- `ensure_ascii=False` → Türkçe karakterlerin düzgün yazılmasını sağlar.
- `indent=4` → JSON'u okunabilir şekilde biçimlendirir.


Json dosyasından okumak 
```python 
import json 

with open("kullanici.json","r",encoding="utf-8") as dosya:
	kullanici=json.load(dosya)
print(kullanici["isim"])
print(kullanici["kullanici"])

```