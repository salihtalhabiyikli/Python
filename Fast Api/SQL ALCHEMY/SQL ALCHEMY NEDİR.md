-> Sql aLchemy bir ORM kütüphanesidir. Amacı sizin sql sorgusu yazmamanızı sağlamaktır. Siz veri tabanı kullancağınız zaman bunu python kodlarıyla yazarsınız. Bu ORM kütüphanesi ise arka planda bunlar birer sql sorgusuna dönüştürür. 
[[SQL ALCHEMY]]
[[FAST API]]
- Normal Sql ile farklarına bakalım 
```sql
# Ham SQL string'i yazmak zorundasın. Yazım hatası yapmaya çok müsaittir.
imlec.execute("INSERT INTO urunler (isim, fiyat, stokta_var) VALUES ('Klavye', 1500, 1)")
baglanti.commit()
```

``` python 
# Tamamen Python nesneleriyle oynarsın. Tarzın hiç bozulmaz.
yeni_urun = Urun(isim="Klavye", fiyat=1500, stokta_var=True)
db.add(yeni_urun)
db.commit()
```

### Avantajları Nelerdir? 

1. **Veri Tabanından Bağımsızlık :** Bugün projenin başında küçük ve kurulumsuz olduğu için **SQLite** kullandın. Yarın proje büyüdü ve **PostgreSQL** veya **MySQL**'e geçmek istedin. Ham SQL yazdıysan, iki veri tabanının ufak tefek kod farklılıkları yüzünden yüzlerce satır kodu tek tek elden geçirmen gerekir. SQLAlchemy'de ise sadece bağlantı linkini (`DATABASE_URL`) değiştirirsin, kodunun geri kalanına **tek bir dokunuş bile yapmazsın**.
  
2. **Yazım Hatalarına Son:** Ham SQL yazarken tablo ismini veya sütun ismini yanlış yazarsan bunu ancak kodu çalıştırıp hata aldığında anlarsın. SQLAlchemy'de ise editörün  sana otomatik tamamlama sunar. Hatalı sütun yazarsan kod daha çalışmadan altını kırmızıyla çizer.
 
3. **Güvenlik :** Hacker'ların en sevdiği şey, input alanlarına zararlı SQL kodları yazarak veri tabanını ele geçirmektir. SQLAlchemy, gelen tüm verileri arkada otomatik olarak temizler  ve seni SQL Injection saldırılarından tamamen korur.

4. **Temiz ve Okunabilir İlişkiler:** İki tabloyu birbirine bağlarken ham SQL'deki o upuzun ve kafa karıştırıcı cümleler yerine, Python'da sadece `urun.kategori` diyerek o ürünün hangi kategoriye ait olduğunu şak diye çekebilirsin.
  
