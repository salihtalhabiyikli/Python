 >Yazdığımız programlarda  elbette hatalar olacaktır. Bu kısımda amacımız hata ihtimallerinide koda eklemek olacak. Yani kullanıcı bir konuda hata yaptığında ekrana hata tipi değilde neden hata verdiği ve nasıl olması gerektiği yazılacak .
  Hata verebilecek kod tek bir blok içerisinde bir deneme döngüsüne sokulur. 

``` python 
try:
   x= int(input("X: "))
   y= int(input("Y: "))
	print(x/y)
except ZeroDivisionError
	print("Y değeri için 0 girilemez")
except:
.
.
.
.


```
Bu bloğun içerisine döngüler veya karar yapıları gibi koşul bildiren ifadeler eklenebilir 
[`try`](https://docs.python.org/tr/3.14/reference/compound_stmts.html#try) ifadesi aşağıdaki gibi çalışır.

- İlk olarak, _try yan tümcesi_ ([`try`](https://docs.python.org/tr/3.14/reference/compound_stmts.html#try) ve [`except`](https://docs.python.org/tr/3.14/reference/compound_stmts.html#except) anahtar kelimeleri arasındaki ifadeler) yürütülür.
    
- Özel durum oluşmazsa, _except yan tümcesi_ atlanır ve [`try`](https://docs.python.org/tr/3.14/reference/compound_stmts.html#try) ifadesinin yürütülmesi tamamlanır.
    
- [`try`](https://docs.python.org/tr/3.14/reference/compound_stmts.html#try) yan tümcesinin yürütülmesi sırasında bir özel durum oluşursa, yan tümcenin geri kalanı atlanır. Daha sonra belirtilen [`except`](https://docs.python.org/tr/3.14/reference/compound_stmts.html#except) türü ile karşılaşılırsa, _except yan tümcesi_ yürütülür ve try/except bloğundan sonra yürütme devam eder.
    
- _Eğer `except` bloğunda_ belirtilen istisna ile eşleşmeyen bir istisna oluşursa , bu istisna dıştaki ifadelere iletilir [`try`](https://docs.python.org/tr/3.14/reference/compound_stmts.html#try); eğer hiçbir işleyici bulunamazsa, bu _işlenmemiş bir istisna_ olarak kabul edilir ve hata mesajıyla birlikte yürütme durdurulur.

- ### KAYNAK :  https://docs.python.org/tr/3.14/tutorial/errors.html