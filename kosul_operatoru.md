Koşul operatörü `(conditional operator / ternary operator)`, C dilinin üç terimli `(operandlı)` tek operatörüdür. Koşul operatörünün terimlerinden her biri türü `void` olmayan herhangi bir ifade olabilir. Koşul operatörünün genel sözdizimi aşağıdaki gibidir:

```ifade1 ? ifade2 : ifade3```

Koşul operatörü birbirinden ayrılmış iki atomdan `(token)` oluşur. `"?"` ve `":"` atomları, operatörün üç terimini birbirinden ayırır. Operatörün birinci terimi `"?"` atomundan önce, ikinci terimi `"?"` ve `":"` atomları arasına, üçüncü operatörü ise `":"` atomundan sonra yazılır:

```
x > 0 ? x : -x
```
ifadesinde koşul operatörünün birinci terimi `x > 0`, ikinci terimi `x` ve üçüncü terimi `-x` ifadeleridir. Koşul operatörünün birinci terimi olan ifade lojik olarak yorumlanır. Yani bu ifadenin değeri `0`'dan farklı ise ifade lojik `"doğru"`, ifadenin değeri `0` ise lojik yanlıştır. Diğer tüm operatörler gibi koşul operatörü de bir değer üretir. Koşul operatörünün ürettiği değer, birinci terim doğru ise ikinci terimin değeri, yanlış ise üçüncü terimin değeridir. Yani koşul operatörü, bir ifadenin doğru ya da yanlış olmasına göre iki değerden birini üretir:

```
max = x > y ? x : y;
```
Yukarıdaki deyimde koşul operatörü `x > y` ifadesinin doğru olması durumunda `x` değerini aksi halde `y` değerini üretiyor. Bu durumda, yazılan deyim ile `x` ve `y`'den büyük olanının değeri `max` değişkenine aktarılmış oluyor.

Okuma kolaylığı sağlaması amacıyla, operatörün birinci teriminin öncelik parantezi içine alınması sık tercih edilen bir durumdur:

```
c = (c >= 'A' && c <= 'Z') ? c - 'A' + 'a' : c;
```
## koşul operatörünün önceliği
Koşul operatörünün öncelik seviyesi virgül operatöründen ve atama operatörlerinden yüksek diğer tüm operatörlerden düşüktür:

```
a = x > y ? x : y * 5;
```
Yukarıdaki deyim ile `max(x, y)` değerinin `5` katının `a` değişkenine atanmak istendiğini düşünelim. Çarpma operatörünün önceliği koşul operatöründen daha yüksek olduğundan `y + 5` ifadesi koşul operatörünün `3.` terimi olarak değerlendirilir. Yani `x`'in `y`'den küçük olması durumunda a değişkenine `y * 5` değeri atanır. Deyim şu şekilde yazılmalıydı:

```
a = (x < y ? x : y) * 5;
```
Şimdi de aşağıdaki `if` deyimine bakalım:

```
if (mday > (isleap(year) ? 29 : 28))
	//
```
`if` deyimi ile `isleap(year)` ifadesinin doğru olup olmadığına göre `mday` değişkeninin `29` ya da `28`'den büyük olup olmadığı sınanıyor. Koşul ifadesi aşağıdaki gibi yazılsaydı

```
mday > isleap(year) ? 29 : 28
```
üretilen değer her zaman doğru `(always true)` olurdu.

## koşul operatörünün öncelik yönü
Koşul operatörünün öncelik yönü `(associativity)` sağdan soladır `(right associative)`. Bir ifade içinde birden fazla koşul operatörü varsa, daha sağdaki koşul operatörünün ürettiği değer daha solda bulunan koşul operatörünün terimi olur. Aşağıdaki kod parçasını inceleyin:

```
#include <stdio.h>

int main()
{
	int x = 1, y = 1, m;

	m = x  <  5 ? y == 0 ? 4 : 6 : 8;
	printf("m = %d\n", m);

	return 0;
}
```
Yukarıdaki `main` işlevinde `m` değişkenine `6` değeri atanır. İfade aşağıdaki gibi ele alınır:

```
m = (x  <  5) ? ((y == 0) ? 4 : 6) : 8;
```
Şimdi de aşağıdaki koda bakalım:

```
m = a < b ? a : b < c ? b : c;
```

Yukarıdaki deyimin aşağıdaki gibi bir `if` deyimine eşdeğer olduğunu görebiliyor musunuz?

```
if (a < b)
	m = a;
else if (b < c)
	m = b;
else
	m = c;
```
Son örnek olarak aşağıdaki kodu inceleyelim:

```
int func(int val)
{
	//
	return val == 10 ? 1 :
		   val == 20 ? 3 :
		   val == 40 ? 7 :
		   0;
}
```
İşlevin `return` deyimi ifadesinde birden fazla koşul operatörü kullanılmış. `return` deyimi şöyle bir `if` yapısı karşılığı kullanılmış:

```
int func(int val)
{
	//
	if (val == 10)
		return 1;

	if (val == 20)
		return 3;
		
	if (val == 40)
		return 7;
		
	return 0;
}
```
## koşul operatörünün kullanıldığı tipik durumlar
Koşul operatörünün kullanılmasının önerildiği tipik durumlarda genel fikir koşul operatörünün ürettiği değerden aynı ifade içinde faydalanarak bu değeri bir yere aktarmaktır:

1. Koşul operatörünün ürettiği değer bir nesneye atanabilir:

```
min = a < b ? a : b;
```
Yukarıdaki deyimde `min` değişkenine `a` ve `b` değerlerinden küçüğü atanıyor.

```
x = x > 0 ? x : -x;
```
Yukarıdaki deyimde `x`'e, `x`'in mutlak değeri atanıyor.

```
no_of_days = isleap(y) ? 366 : 365;
```
Yukarıdaki deyimde `no_of_days` değişkenine `y` artık yıl ise `366` değilse `365` değeri atanıyor.

2. Koşul operatörünün ürettiği değer bir işlevin geri dönüş değeri `(return value)` olabilir :

```
int to_lower(int c)
{
	return c >= 'A' && c <= 'Z' ? c - 'A' + 'a' : c;
}
```

Yukarıda tanımlanan `to_lower` işlevi `ASCII` kodlamasının kullanıldığı bir sistemde büyük harf karakterini küçük harfe dönüştürüyor. `to_lower` işlevi kendisine gelen karakter büyük harf değilse karakterin kendi kod değerini döndürüyor. İşlevin bu geri dönüş değerini üretmesi için bir `if` deyimi de kullanılabilirdi:

```
int to_lower(int c)
{
	if (c >= 'A' && c <= 'Z')
		return c - 'A' + 'a';
	
	return c;
}
```
3. Koşul operatörünün ürettiği değer ile bir işlev çağrılabilir:

```
func(a == b ? x : y);
```
Yukarıdaki deyimde, `a`, `b`'ye eşit ise `func` işlevi `x` değeri ile, `a`, `b`'ye eşit değil ise `y` değeri ile çağrılır. Aynı işi gören bir `if` deyimi de yazılabilirdi:

```
if (a == b)
	func(x);
else
	func(y);
```
4. Koşul operatörünün ürettiği değer aynı ifade içinde başka bir operatörünün terimi olarak kullanılabilir:

```
if (y == (x > 5 ? 10 : 25))
	foo();
```
Koşul operatörü `if` deyiminin bir başka biçimi olarak görülmemelidir. Koşul operatörü bir ifade `(expression)` oluşturur. Koşul operatörü bir kontrol deyimi `(control statement)` değildir. Özellikle bir ifadeye açılması gereken işlevsel makrolarda `(function-like macro)` `if` deyimi kullanma şansımız olmaz. Böyle durumlarda koşul operatörünün kullanımı çok yaygındır:

```
#define min(a, b)   ((a) < (b) ? (a) : (b))
```
Eğer koşul operatörünün ürettiği değerden doğrudan faydalanma fikri yok ise koşul operatörü yerine `if` kontrol deyimini tercih etmek okunması daha kolay bir kod oluşturur:

```
x > y ? printf("dogru") : printf("yanlis");
```
Yukarıdaki deyimde koşul operatörünün ürettiği değerden faydalanılmamış. Böylesi durumlarda koşul operatörü yerine `if` deyimi kullanmak okunabilirlik açısından daha iyi bir seçenek olabilir:

```
if (x > y)
	printf("dogru");
else
	printf("yanlis");
```
## C dilinde koşul operatörü bir sol taraf değeri ifadesi `(L value expression)` oluşturmaz.
Koşul operatörü bir sol taraf değeri üretmez, yani koşul operatörü ile  oluşturulan ifade bir nesne göstermez. Aşağıdaki `if `deyimini inceleyin:

```
if (x > 0)
	y = 3;
else
	z = 3;
```
Yukarıdaki `if` deyiminde `x > 0` ifadesinin doğru olması durumunda `y` değişkenine `3`, yanlış olması durumunda ise `7` değeri atanıyor. Bu iş için koşul operatörünün kullanıldığını düşünelim:

```
(x > 0 ? y : z) = 3; //Geçersiz
```
Deyim geçersizdir. Atama operatörünün sol terimi nesne gösteren bir ifade değildir. Aynı nedenden dolayı aşağıdaki ifade de geçersizdir:

```
(x > 5 ? y : z)++; //Geçersiz
```
Öncelik parantezi içine alınan ifade sol taraf değeri değildir. 
C++ dilinde koşul operatörünün 2. ya da 3. teriminin nesne olması durumunda operatör sol taraf değeri üretir. Yani yukarıdaki deyimler C de geçersiz iken C++'da geçerlidir. C'de koşul operatöründen bir sol taraf değeri alınması için şöyle bir hile yapılır:

```
*(x > 0 ? &y : &z) = 3;
```
Yukarıdaki deyimde koşul operatörü `x > 0` ifadesinin doğru olup olmadığına bağlı olarak ya da `z` değişkeninin adresini üretiyor. Öncelik parantezi içinde alınmış ifadenin içerik `(dereferencing)` operatörünün terimi yapıldığını görüyorsunuz. Bu durumda ya `x` ya `y` nesnesine erişmiş oluyoruz.

Bazı durumlarda, `if` deyiminin de koşul operatörünün de kullanılması gerekmez:

```
if (x > 5)
	y = 1;
else
	y = 0;
```
Yukarıdaki `if` deyimi yerine aşağıdaki deyim yazılabilirdi:

```
m = (x > 5) ? 1 : 0;
```
Koşul operatörünün üreteceği değerlerin yalnızca `1` veya `0` olabileceği durumlarda, doğrudan karşılaştırma ya da mantıksal operatörleri kullanmak daha iyi teknik olarak değerlendirilmelidir:

```
m = x > 5;
```
Başka bir örnek:

```
return x == y ? 1 : 0;
```
yerine

```
return x == y;
```
yazılabilirdi.

## koşul operatörü ve tür dönüşümü

Koşul operatörünün ikinci ve üçüncü terimlerinin türleri farklı ise, diğer operatörlerde olduğu gibi tür dönüştürme kuralları devreye girer. `ival`, `int` türden `dval` ise `double` türden değişkenler olsun:

```
x > 0 ? ival : dval
```
ifadesi, `x > 0` koşulu doğru da olsa yanlış olsa, `double` türdendir. Aşağıdaki kodu çalıştırın: 

```
#include <stdio.h>

int main()
{
	double dval = (10 > 5 ? 10 : 1.) / 3;
	printf("dval = %f\n", dval);
}
```
