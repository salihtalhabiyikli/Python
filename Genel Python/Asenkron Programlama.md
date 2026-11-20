- Asenkron programlama programın bir işi beklerken başka işlerle ilgilenmesini sağlayan programlama yaklaşımıdır. 
- Pythonda asenkron programlama asyncio kütüphanesi üzerinden yapılır. 

```python 

import asyncio 

async def gorev(isim,sure):# Bu fonksiyonun await ile beklenmesi gerekir.Bunlara coroutine fonksiyon denir.
	print(f"{isim} basladı")
	await asyncio.sleep(sure)# await bir coroutine fonksiyonun bitmesini bekler. Bu beklemede event loop başka işler yapabilir.
	print(f"{isim} bitti")
	

async def main(): 
	await asyncio.gather( # Birden fazla coroutine fonksiyonu aynı anda çalıştırmak için kullanılır 
	gorev("A",2),
	gorev("B",2)
	)
	
asyncio.run(main()) # asenkron programın başlangıcıdır. event loopu başlatır. 
	
```

### Event Loop Nedir ?

Asenkron programlamanın temel motorudur. Bir görevin ne zaman çalışacağına karar veren bir motordur. 

> Pythonun asynciosu tek iş parçacıklıdır. yani gerçek paralel işlem yapamaz. bunun yerine bekleme sürelerini akıllıca değerlendirerek verimlilik sağlar. 


async def ---> Bu fonksiyon bekleme yapabilir. 
await ---> Burada bekleme olabilir. Bekleme olursa başkası çalışsın 


```python 
import asyncio

async def kahve_yap():
    print("Kahve makinesi başladı...")
    await asyncio.sleep(2)   # 2 saniye bekliyor ama bloklamıyor
    print("Kahve hazır!")
 
async def ekmek_kizart():
    print("Ekmek kızartıcı başladı...")
    await asyncio.sleep(1)
    print("Ekmek hazır!")

async def main():
    # İkisini AYNI ANDA başlat
    await asyncio.gather(kahve_yap(), ekmek_kizart())

asyncio.run(main())
```


### Task Group 
- Normalde asenkron döngü içerisindeki görevlerin hepsinin tamamlanacağının garantisi verilmez. Ama with ve taskgroup kullandığımız fonksiyonlar içerisinde en ufak bir terslikte exception hatası fırlatır. 

```python 

import asyncio 

def gorev(sure,isim):
	await asyncio.sleep(sure)
	print(f"{isim} tamamlandı")
	
async def main():
	async with asyncio.Taskgroup() as tg:
		tg.create_task(gorev("A",2))
		tg.create_task(gorev("B",1))
		tg.create_task(gorev("C",3))
		
	print("Tüm görevler bitti")
	
asyncio.run(main())
```
`TaskGroup`, bir **context manager** (`async with`) olarak çalışıyor. Bunun sağladığı şey:

1. **Otomatik bekleme**: `async with` bloğu bitmeden önce, içindeki **tüm** task'ların bitmesini garantiler. Manuel olarak `await` etmene gerek yok.
2. **Bir görev patlarsa, diğerleri otomatik iptal edilir**: Eğer `gorev("B", 1)` hata fırlatırsa, `TaskGroup` diğer çalışan görevleri (`A` ve `C`) **otomatik olarak iptal eder**. Böylece hiçbir görev "kayıp" kalmaz, sistem tutarlı bir durumda kalır.
3. **Hatalar toplu şekilde raporlanır**: Birden fazla görev aynı anda hata verirse, bunlar tek tek kaybolmak yerine bir `ExceptionGroup` içinde toplanır, hepsini görebilirsin.