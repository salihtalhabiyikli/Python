### Seviye 1: Temel (1-5)

1. **Selamlama API’si:** `GET /hello/{isim}` ve `GET /hello?isim=Ali`. _Öğrenilen: path ve query parametreleri._
2. **Hesap makinesi:** `/topla`, `/cikar`, `/carp`, `/bol`. Sıfıra bölmede düzgün hata dönsün. _Öğrenilen: `HTTPException`, durum kodları._
3. **Birim çevirici:** Celsius/Fahrenheit, km/mil, kg/lb. _Öğrenilen: fonksiyonları düzenli yapılandırma._
4. **Not defteri (bellekte):** Not ekle, listele, sil. Veri bir listede dursun. _Öğrenilen: Pydantic modelleri, POST/DELETE._
5. **Yapılacaklar listesi (CRUD):** Ekle, oku, güncelle, sil, tamamlandı işaretle. _Öğrenilen: tam CRUD, PUT/PATCH farkı._

### Seviye 2: Kalıcı veri (6-10)

6. **JSON dosyasına kaydeden todo:** Sunucu kapanınca veri kaybolmasın. _Öğrenilen: dosya okuma/yazma._
7. **SQLite ile kitap listesi:** SQLAlchemy ile veritabanı. _Öğrenilen: ORM, veritabanı oturumu, `Depends`._
8. **Arama, filtre ve sayfalama:** 7. projeye `?yazar=...&sayfa=2&limit=10` ekleyin. _Öğrenilen: query parametreleri ile gerçek dünya API’si._
9. **URL kısaltıcı:** `POST /kisalt` kod üretir, `GET /{kod}` yönlendirir. _Öğrenilen: yönlendirme, benzersiz kod üretme._
10. **Harcama takipçisi:** Kategori bazlı toplamlar ve aylık özet. _Öğrenilen: ilişkili tablolar, toplama sorguları._

### Seviye 3: Güvenlik ve yapı (11-15)

11. **Kayıt ve giriş sistemi:** Şifreler bcrypt/argon2 ile hash’lensin. _Öğrenilen: şifre güvenliği._
12. **JWT ile korumalı rotalar:** Token olmadan erişilemesin. _Öğrenilen: OAuth2, `Depends` ile yetkilendirme._
13. **Kullanıcıya özel veri:** Herkes sadece kendi notlarını/harcamalarını görsün. _Öğrenilen: yetkilendirme mantığı._
14. **Rate limit ve loglama:** İstek başına süre loglayan middleware, `slowapi` ile limit. _Öğrenilen: middleware, konuştuğumuz konular._
15. **Testler:** Önceki projelerden birine `pytest` ve `TestClient` ile test yazın. _Öğrenilen: test yazma alışkanlığı._

### Seviye 4: ML ve deploy (16-20)

16. **Duygu analizi API’si:** Hazır bir model (Hugging Face) ile `POST /analiz` metnin olumlu/olumsuz olduğunu söylesin. _Öğrenilen: modeli API ile sunma, modeli uygulama açılışında bir kez yükleme._
17. **Kendi eğittiğiniz scikit-learn modeli:** Küçük bir veri setiyle model eğitin, `joblib` ile kaydedin, API’den tahmin döndürün. _Öğrenilen: eğitim ile sunum ayrımı._
18. **Metin özetleme veya Türkçe sınıflandırma:** Türkçe bir model kullanın. _Öğrenilen: Türkçe NLP, ağır modellerin kaynak ihtiyacı._
19. **Dockerize edin:** 16 veya 17'yi `Dockerfile` ile paketleyin, `docker compose` ile veritabanıyla birlikte çalıştırın. _Öğrenilen: Docker._
20. **VPS’e deploy:** 19'u kiralık bir sunucuya alın, Caddy ile HTTPS kurun, alan adı bağlayın. _Öğrenilen: SSH, güvenlik duvarı, reverse proxy. Tüm konuştuklarımızın birleşimi._

### Günlük çalışma önerisi

- **Bir proje 1-3 gün sürebilir.** Seviye 1 hızlı biter, bir günde 2-3 tane yapabilirsiniz. Seviye 3-4 birkaç gün alır, ve bu normal.
- **Her projeyi GitHub’a atın** ve kısa bir `README` yazın. 4 yıl sonra portföyünüz olur.
- **Önce kendiniz deneyin, takılınca sorun.** Çözümü baştan istemek öğrenmeyi yarıya düşürür. Takıldığınız yerde hata mesajıyla gelin, yönlendireyim.
- **Hep `/docs` sayfasından test edin.** FastAPI’nin otomatik dokümantasyonu en iyi test aracınız.