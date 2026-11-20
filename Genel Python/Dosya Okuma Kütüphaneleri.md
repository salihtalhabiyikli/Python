## pypdf 
- Saf pythonla yazılmıştır. Büyük metinlerde yavaş çalışabilir. Türkçe harflerde sorun yaşamaz 
- Metindeki bailıkları ve fontu ayırt edemez biraz yüzeyseldir 
```python 
from pypdf import PdfReader # Kütüphane ekleme 

def load_pdf(file_path:str) -> str: # Burada bir fonksiyon oluşturuyoruz 
reader= PdfReader(file_path) # Burda fonksiyonumuza bir değişken yönlenidiyoruz 
text=" " # Text okunurken kaydedileceği ve ramde tutulacak değişkeni atıyoruz
for page in reader.pages: #tüm sayfaların numarasını alıyoruz 
text+=page.extract_text()+ "\n"  # Asıl sayfaları okuyan kısım burasıdır 
return text 

if __name__=="__main__" 
# Bu kısımda dosyanın terminaldenmi yoksa script olarak çalıştırıldığını kontrol eder. Eğer bu terminalden çalıştırıldıysa adı main olarak atanır.
 
text=load_pdf("test.pdf") 
print(f" Toplam karakter sayısı {len(text)}) 
print(text[:500]) # İlk 500 karakteri yazdırır
``` 

## pymupdf
- C ve C++ kullanılarak yazılmıştır. pypdften kat ve kat hızlıdır. 
- Resim renderı ve metin renderı alabilmektedir. 
- metnin fontunu yazı boyutunu ayırt edebilmektedir. 
- Özel bir c derleyicisi gerektirmez

```python
pip install pymupdf
```

```python 
import fitz
def extract_text_from_pdf (file_path):
    doc= fitz.open(file_path)
    tam_metin=""
    for page_num in range(len(doc)):
        page=doc.load_page (page_num) # Sayfalar numaralarına göre belleğe yüklenir
    sayfa_metni=page.get_text() # sayfalar okunur
    tam_metin+=sayfa_metni+ "\n"
   doc.close()
    return tam_metin
alinan_metin = extract_text_from_pdf("Test.pdf")
print(alinan_metin)


```