Bu performans katili olan bir problemdir. Siz veritabanına 1 sorgu attım zannederken aslında 50 100 veya daha fazla sorgu gitmekte ve proje performansı düşmektedir. 
Peki bu nasıl olur ona bir bakalım 
** Senaryo **

1. Kütüphaneciye gidip soruyorsunuz: _"Bana en popüler 10 yazarın listesini ver."_ Kütüphaneci size listeyi veriyor. **(Bu 1. Sorgu)**
2. Listeye bakıyorsunuz. Birinci sıradaki yazar Ahmet Hamdi Tanpınar. Kütüphaneciye tekrar gidiyorsunuz: _"Bana Ahmet Hamdi'nin kitaplarını ver."_ **(Bu 2. Sorgu)**
3. İkinci sıradaki yazar Orhan Pamuk. Kütüphaneciye bir daha gidiyorsunuz: _"Bana Orhan Pamuk'un kitaplarını ver."_ **(Bu 3. Sorgu)**


Bu şekilde 10 yazar için kütüphanecinin yanına 10 defa daha gidersiniz. Toplamda **1 (Yazarlar) + 10 (Kitaplar) = 11 kere** kütüphaneciyi meşgul ettiniz. Eğer yazar listeniz 10 değil de 1.000 kişi olsaydı, veritabanına tam 1.001 kez sorgu atacaktınız!

Bu kod üzerindede gayet normal gibi durur 
```python
# Veritabanından 50 yazar çekiliyor (1 ADET SORGULAMA YAPILDI)
yazarlar = Yazar.objects.all()

for yazar in yazarlar:
    # ORM tembeldir. Kitapları baştan çekmediği için, siz ".kitaplar" 
    # dediğiniz an veritabanına GİZLİCE yeni bir sorgu atar!
    # Döngü 50 kere döneceği için 50 YENİ SORGU daha atılır.
    print(f"Yazar: {yazar.isim}, Kitapları: {yazar.kitaplar.all()}")
```

Bu problemin çözümü en başta doğru sorguyu atmaktır. Buna yazılmda Eager Loading denir.  Veritabanındaki JOIN komutları kullanılarak tüm veriler tek seferde çekilir. 
Optimize kod aşağıdaki gibidir. 
```python
# Django'ya diyoruz ki: Yazarları alırken, onlara bağlı kitapları da aynı anda getir.
# (SADECE 1 ADET BÜYÜK SORGULAMA YAPILDI)
yazarlar = Yazar.objects.prefetch_related('kitaplar').all()

for yazar in yazarlar:
    # Veriler zaten ilk satırda hafızaya alındığı için, 
    # burada veritabanına HİÇBİR SORGULAMA YAPILMAZ.
    print(f"Yazar: {yazar.isim}, Kitapları: {yazar.kitaplar.all()}")
```

Şimdi kodumuzun kaç tane sorgu yaptığını bulalım.

- Django kullanıyorsanız toolbar zaten bunu gösterecektir. PHP içinse Laravel bu işi yapmaktadır. 

##### Terminale Sql Loglarını Basmak 
- SQL Alchemy kullanıyorsanız motoru yazarken echo=True ifadesini eklersiniz. 
- Django kullanıyorsanız settings.py dosyanızdaki LOGGING ayarına bu kuralı ekleyebilirsiniz. 

#### Test Yazma 
- Her defasında log kontrol etmek istemeyebilrsiniz. Bunun için sadece belirli bir sorgu sayısında çalışan test yazalım 
```python 
def test_yazar_listesi_sayfası(self)
 # biz burda sadece 2 sorgu bekliyoruz 3. bir sorguda test başarısız olacak
 with self.assertNumQueries(2)
	 response= self.client.get('/yazarlar/')
	 self.assertEqual(response.status_code,200)
```


- Veya hepsini bırakıp Sentry , New Relic ve Datadog gibi araçları kullanarak canlı server akışını izleyebilirsiniz. 
- 