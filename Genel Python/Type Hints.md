- Değişkenlere verdiğimiz genel kurallardır. Büyük projelerde değişken tiplerinin karışmamasını sağlar. 

```python 
def calculate_price(quantity:int , unit_price:float) -> float:
	return quantity * unit_price 
```

Kısaca fonksiyon parametrelerinin tipini belirleriz. 

## Generics 
- Üstte bahsettiğimiz fonksşyonu daha esnek hale getirmeyi sağlar. 
```python 
from typing import TypeVar, Generic

T= TypeVar("T")

class kutu(Generic[T]):
	def __init__(self,icerik:T):
		self.icerik=icerik
		
	def getir(self) -> T:
		return self.icerik
```

Bu sayede fonksiyonun içerisine hangi tip girerse o tip bir değer döndürür. 

