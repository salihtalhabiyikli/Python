

### Problem : HTTP Hafızasızdır
- Sunucu ve tarayıcı arasındaki her iletişim adımında sunucu sizi unutmaktadır. 
- Token size bir kimlilk sağlar. 

![[Pasted image 20261008140053.png]]

### Authentication ve Authorization 
 - Authentication : Kim olduğunuzu kontrol eder. 401 döner
 - Authorization : Yetkiyi kontrol eder . 403 döner
### OAuth2 ve Password Flow 

Sunucu ve kullanıcı arasındaki akışa Flow denmektedir. FLow kurallarının tamamını Oauth2 protokolü oluşturmaktadır. 

##### Flowlar 
- Şifre hashleme : Şifreyi DB'de nasıl saklarız
- Kullanıcı Doğrulama : Gelen şifre doğrumu 
- JWT Üretme : Doğruysa bileti nasıl basarız
-  JWT Doğrulama : Gelen bilet gerçekmi süresi dolmuşmu 
- FastAPI'ye bağlama : Depends ile her endpoint nasıl korunur
- Rol Kontrolü : Doğrulanmış kullanıcıya izin verme

### Şifre Hashleme 

#### Problem : Kullanıcının Şifresini Nasıl Saklarız

Şifreleri düz bir metinde hashlemek ileride veri tabanı sızıntılarında güvenlik sorunu oluşturmaktadır. 

>Temel İlke : Sunucu bir kullanıcının şifresini bilmemeli,sadece kontrol etmeli

##### Şifreleyip Saklamak 
- Şifreleme geri döndürülebilir bir işlemdir. key- Value data stringinde çalıştığı için anahtarı  bulan kişi otomatik olarak şifreyide bulmaktadır. 
- Çünkü anahtarda aynı sunucuda tutulmaktadır. 
##### HASH 

Hash algoritmaları bir veriyi geri döndürülemez bir biçimde random bir formata çevirir. 

1. Aynı girdiler her zaman aynı çıktıyı verir
2. Çıktıdan girdiye geri dönülemez
3. Girdi 1 harf değişse bile çıktı tamamen değişir. 

*Hash taranarak bulunabilir*
*Hash hızlıca denenerek kırılabilir*

##### Çözüm 1 : Salt 
- Bu kısımda tuzlama dediğimiz bir değer mevcutt
- Salt = Rastgele sayı harf dizisi 
- Şifre + Salt -> Hash = Şifre 
- Ama saltın amacı gizlilik değil benzersizliği yakalamaktır. 

##### Çözüm 2:  Kasıtlı olarak Yavaş Hash 
- Saldırıları yavaşlatmak = Hashi yavaşlatmak 
- Her deneme 0.2 sn sürse ve milyarlarca deneme bulunsa bu süreç hem günümüz teknolojisi için hemde maliyet açısından imkansızlaşır. 
Hashi Yavaşlatmak için çeşitli algoritmalar 

- bcrypt -> Eski ve sağlamdır, CPUyu yorar 72 byte şifre sınırı vardır 
- scrypt -> CPUya ek olarak belleğide zorlar 
- Argon2 -> Modern ve Standarttır Hem CPU hem bellek harcatır 


#### Argon2

- Modern ekran kartları paralel işlem açısından mükemmeldir 
- Argon2 her bir hesap için 64 mb bellek ister 
- Bir GPUda bunu binlerce kez çalıştırmak çok pahalı olmaktadır 
- 2015te Password hashing Competitionu kazanmıştır 

Argon2 Çıktısı Şuna Benzer 

```
$argon2id$v=19$m=65536,t=3,p=4$c2FsdHNhbHQ$aGFzaGhhc2hoYXNo...
```

argon2is : Algoritma 
m=*** : Bellek maliyeti (KB)
t=3 Tur sayısı 
p=4 : Paralellik 
c2FJAah34 : Salt 
Son kısım : Asıl Hash 


##### PwdLib 

```python 
from pwdlib import PasswordHash 

password_hash=PasswordHash.hash("***")

print(password_hash.verify("***", hashed)) #True
	print(password_hash.verify ("Yanlış",hashed))
```


##### Hash Tekrar Nasıl Çözülür
1. Şifreler veritabanına kaydedilir 
2. Kullanıcı bir giriş dener 
3. Şifre + salt(bu değişmez) = Aynı argon2 çıktısıysa şifre doğrudur 
4. Fakat aynı saltla tekrar şifrelendiğinde aynı çıktıyı vermezse hata dönderir. 
