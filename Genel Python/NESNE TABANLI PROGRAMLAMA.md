### 1. Temel Kavramlar: Sınıf (Class) ve Nesne (Object)

Bu iki kavramı kafanda şöyle ayırabilirsin:

- **Class (Sınıf):** Karakter oluşturma ekranındaki "Şablon"dur. Savaşçı şablonu, Büyücü şablonu vb. Henüz ortada yaşayan, koşan bir karakter yoktur; sadece tarif vardır.
    
- **Object (Nesne):** O şablondan üretip oyuna soktuğun "Gerçek Karakter"dir. Örneğin: _Geralt_, _Arthas_ veya _Mario_.
    

#### Somut Örnek: "Oyun Karakteri" Tasarlayalım

Bir oyun yazıyoruz ve `Karakter` adında bir sınıfımız (kalıbımız) olsun istiyoruz. Her karakterin bir adı, canı ve bir de saldırı gücü olsun.

### 2. Kod Üzerinden Adım Adım Gidelim

Python'da bu kalıbı şöyle yazarız:
```python
class Karakter:
    # __init__ metodu: Karakterin doğduğu andır (Kurucu Metot)
    # self: "Ben" demektir. O an hangi karakterden bahsediyorsak "o".
    def __init__(self, isim, can, guc): # Burda init kullanmamızın sebebi bunun constructor olmasıdır 
        self.isim = isim   # Karakterin adı
        self.can = can     # Karakterin canı
        self.guc = guc     # Karakterin vuruş gücü
        print(f"{self.isim} oyuna katıldı!")

    # Metotlar (Method): Karakterin yapabildiği hareketler
    def saldir(self):
        print(f"{self.isim}, {self.guc} gücünde saldırdı!")

    def yemek_ye(self):
        self.can += 10
        print(f"{self.isim} yemek yedi. Yeni canı: {self.can}")

```
Burada neler oldu?

1. **`class Karakter`:** Kalıbı tanımladık.
    
2. **`def __init__`:** Fabrika ayarları. Karakter yaratılırken bu fonksiyon _otomatik_ çalışır.
    
3. **`self` (Çok Önemli):** `self` kelimesini, karakterin aynaya bakıp **"Benim ismim..."** demesi gibi düşün. Eğer `self` kullanmazsak, Python o ismin kime ait olduğunu bilemez.

### 3. Fabrikayı Çalıştıralım: Nesne Üretmek (Instantiation)

Şimdi elimizde bir kalıp (Class) var. Hadi bundan iki farklı karakter (Object) üretelim.
```python
# 1. Nesneyi üretiyoruz (Savaşçı)
oyuncu1 = Karakter("Geralt", 100, 25)

# 2. Nesneyi üretiyoruz (Büyücü)
oyuncu2 = Karakter("Yennefer", 80, 40)
```
**Şu an ne oldu?**

- Hafızada iki ayrı kutu oluştu.
    
- `oyuncu1` kutusunda: İsim="Geralt", Can=100
    
- `oyuncu2` kutusunda: İsim="Yennefer", Can=80
    

Bu nesneler tamamen bağımsızdır. `oyuncu1` yemek yerse, `oyuncu2`'nin canı artmaz.

### 4. Etkileşim: Metotları Kullanmak

Karakterleri konuşturmaya veya savaştırmaya başlayalım.

Python
```python 
# Geralt saldırıyor
oyuncu1.saldir() 
# Çıktı: Geralt, 25 gücünde saldırdı!

# Yennefer yemek yiyor
oyuncu2.yemek_ye()
# Çıktı: Yennefer yemek yedi. Yeni canı: 90 (80 + 10 oldu)

# Geralt'ın canına bakalım, değişmiş mi?
print(oyuncu1.can) 
# Çıktı: 100 (Hala aynı, çünkü yemeği Yennefer yedi)
```

### Neden OOP Kullanıyoruz? (Soyuttan Somuta Fayda)

Eğer OOP kullanmasaydın, 100 tane karakter için 100 tane ayrı `isim1`, `isim2`, `can1`, `can2` değişkeni oluşturman ve her biri için if-else blokları yazman gerekirdi. Kod çorba olurdu.

OOP sayesinde:

1. **Düzen:** Her şey kendi kutusunda.
    
2. **Tekrar Kullanım:** `Karakter` sınıfını bir kere yazarsın, binlerce karakter üretirsin.
    
3. **Miras (Inheritance):** Bunu aşağıda görelim.****
### 5. Bir Adım İleri: Miras Alma (Inheritance)

Diyelim ki oyuna "Okçu" eklemek istiyorsun. Okçu da bir karakterdir, onun da canı ve ismi vardır. Ama ek olarak "Ok Sayısı" vardır. Sıfırdan kod yazmak yerine `Karakter` sınıfının özelliklerini kopyalarız (Miras alırız).

```python
# Parantez içine 'Karakter' yazarak ondan miras aldık
class Okcu(Karakter):
    def ok_at(self):
        print(f"{self.isim} ok fırlattı! Tam 12'den vurdu.")

# Hemen bir Legolas üretelim
okcu1 = Okcu("Legolas", 90, 30)

# Okçu, Karakter sınıfındaki özellikleri de kullanabilir!
okcu1.yemek_ye()  # Miras aldığı özellik
okcu1.ok_at()     # Kendi özel yeteneği
```

|**Kavram**|**Oyun Analojisi**|**Kod Karşılığı**|
|---|---|---|
|**Class**|Karakter Şablonu / Kalıp|`class Karakter:`|
|**Object**|Oyundaki Geralt veya Legolas|`oyuncu1 = Karakter(...)`|
|**Attribute**|Can barı, Güç seviyesi, İsim|`self.can`, `self.isim`|
|**Method**|Saldırı tuşu, İksir içme tuşu|`def saldir(self):`|
|**Inheritance**|Savaşçının "Karakter" özelliklerini taşıması|`class Okcu(Karakter)`|

### GERÇEK E TİCARET DÖKÜMANTASYONLARI 

```PYTHON
class Urun:
    """
    E-ticaret sistemindeki tüm ürünler için temel sınıf (Base Class).
    Bu sınıf tek başına kullanılmaz, diğer ürün tipleri buradan türetilir.
    """
    
    def __init__(self, isim: str, fiyat: float):
        self.isim = isim
        # Kapsülleme (Encapsulation): 
        # Fiyatın dışarıdan doğrudan değiştirilip -50 yapılmasını engellemek için
        # başına iki alt çizgi (__) koyarak onu "private" yaptık.
        self.__fiyat = fiyat 

    def satis_fiyati_getir(self) -> float:
        """Ürünün güncel satış fiyatını döndürür."""
        return self.__fiyat

    def indirim_yap(self, oran: float):
        """
        Ürüne belirtilen oranda (0.0 - 1.0 arası) indirim uygular.
        Örnek: %20 indirim için oran = 0.20
        """
        if 0 < oran < 1:
            indirim_miktari = self.__fiyat * oran
            self.__fiyat -= indirim_miktari
            print(f"{self.isim} ürününe %{oran*100} indirim yapıldı.")
        else:
            print("Hata: Geçersiz indirim oranı!")

    def bilgi_ver(self):
        """Ürün detaylarını yazdırır. Alt sınıflarda ezilecektir (Override)."""
        pass
```
#### 2. Alt Sınıflar (Subclasses) ve Çok Biçimlilik (Polymorphism)

Şimdi bu temel kalıptan iki farklı gerçek ürün tipi türetelim. Dikkat et, ikisi de `bilgi_ver` komutuna farklı tepkiler verecek. İşte buna **Çok Biçimlilik** diyoruz.
```PYTHON 
class FizikselUrun(Urun):
    """Kargo gerektiren fiziksel ürünler (Örn: Laptop, Gömlek)."""
    
    def __init__(self, isim: str, fiyat: float, agirlik: int):
        # super() fonksiyonu, miras aldığımız üst sınıfın (Urun) __init__'ini çalıştırır.
        super().__init__(isim, fiyat) 
        self.agirlik = agirlik # Gram cinsinden

    def kargo_hesapla(self) -> float:
        """Ağırlığa göre kargo ücreti hesaplar."""
        return self.agirlik * 0.5 

    # Polymorphism: Üst sınıftaki metodu kendimize göre yeniden yazıyoruz (Overriding)
    def bilgi_ver(self):
        fiyat = self.satis_fiyati_getir()
        print(f"[KUTU] {self.isim} | Fiyat: {fiyat} TL | Ağırlık: {self.agirlik}gr")


class DijitalUrun(Urun):
    """İndirilebilir ürünler (Örn: PDF, Lisans Anahtarı). Kargo yoktur."""
    
    def __init__(self, isim: str, fiyat: float, indirme_linki: str):
        super().__init__(isim, fiyat)
        self.link = indirme_linki

    # Polymorphism: Dijital ürünün bilgi verme şekli farklıdır.
    def bilgi_ver(self):
        fiyat = self.satis_fiyati_getir()
        print(f"[İNDİR] {self.isim} | Fiyat: {fiyat} TL | Link: {self.link}")
```

### 3. Kullanım Örneği (Test Senaryosu)

Şimdi yazdığımız bu "kütüphaneyi" kullanan bir geliştirici olduğumuzu düşünelim.
```PYTHON 
# Nesneleri (Objects) örnekleyelim (Instantiation)
laptop = FizikselUrun("Gaming Laptop", 25000, 2500)
ebook = DijitalUrun("Python Kitabı", 150, "www.site.com/indir/py101")

# Listeye atıp toplu işlem yapalım
sepet = [laptop, ebook]

print("--- SEPET DETAYLARI ---")

for urun in sepet:
    # DİKKAT: Burada urun'ün fiziksel mi dijital mi olduğunu kontrol etmemize gerek yok!
    # Her ikisi de "Urun" olduğu için "bilgi_ver()" metodunu anlar.
    # Ama her biri kendi doğasına uygun cevabı verir. (Polymorphism)
    urun.bilgi_ver()

# Kapsülleme Testi: Fiyatı değiştirmeyi deneyelim
print("\n--- GÜVENLİK TESTİ ---")
# laptop.fiyat = -500  # BU ÇALIŞMAZ! (Hata verir veya değişkene ulaşamaz)
# laptop.__fiyat = -500 # BU DA ÇALIŞMAZ! Python bunu gizler.

# Doğru yöntem: Metot kullanarak güvenli indirim yapma
laptop.indirim_yap(0.10) # %10 indirim
laptop.bilgi_ver()
```
- **`super().__init__`**: "Önce babamın (üst sınıfın) özelliklerini yükle, sonra benimkileri ekle" demektir.
    
- **`__fiyat` (Private)**: Kasiyerin fiyatı eliyle silip değiştirmesini engellemek gibidir. Fiyatı değiştirmek için "Müdür anahtarı" (yani `indirim_yap` metodu) gerekir.
    
- **Overriding (Metot Ezme)**: `bilgi_ver` metodu `Urun` sınıfında boştu. Ama `FizikselUrun` ona "Kargo bilgisi" ekledi, `DijitalUrun` ise "Link bilgisi" ekledi.