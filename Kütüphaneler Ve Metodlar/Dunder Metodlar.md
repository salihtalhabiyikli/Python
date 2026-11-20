```python 
class Kitap:
    def __init__(self, ad, sayfa):      # nesne oluşturulurken çalışır
        self.ad = ad
        self.sayfa = sayfa

    def __str__(self):                  # print(nesne) yazınca çalışır
        return f"{self.ad} ({self.sayfa} sayfa)"

    def __len__(self):                  # len(nesne) yazınca çalışır
        return self.sayfa

    def __eq__(self, diger):            # nesne1 == nesne2 yazınca çalışır
        return self.ad == diger.ad

    def __add__(self, diger):           # nesne1 + nesne2 yazınca çalışır
        return self.sayfa + diger.sayfa


k1 = Kitap("Python", 300)
k2 = Kitap("ML", 200)

print(k1)        # Python (300 sayfa)  -> __str__
print(len(k1))   # 300                 -> __len__
print(k1 == k2)  # False               -> __eq__
print(k1 + k2)   # 500                 -> __add__
```