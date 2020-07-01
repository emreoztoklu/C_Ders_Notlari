# Virgül Operatörü

Virgül iki terimli `(operand)` ara ek `(infix)` konumlu bir operatördür.

`exp1, exp2`

gibi bir ifadede `exp1` virgül operatörünün  sol terimi `exp2` ise virgül işlecinin sağ terimidir.

Virgül operatörünün birinci teriminden sonra bir yan etki noktası `(sequence point)` vardır.
Operatörün ikinci terimi olan ifade, birinci terimin değerlendirilmesinden sonra ele alınır.
Operatörün sağ terimi ele alındığında sol terimi olan ifadenin doğurduğu tüm yan etkiler `(side effects)` gerçekleşmiş durumdadır.
Aşağıdaki koda bakalım:

```
#include <stdio.h>

int func(int);

int main()
{
	int x = 10, y = 5;

	x += y, y = func(x);
}
```
Yukarıdaki kodda

```
x += y, y = func(x)
```
ifadesinde kullanılan virgül operatörünün sol terimi

```
x += y
```
ifadesi, sağ terimi ise

```
y = func(x)
```
ifadesidir. `func` işlevinin `x` değişkeninin artmış değeri olan `15` ile çağrılması garanti altındadır.

## virgül operatörünün önceliği
Virgül `C` dilinin en düşük önceliğe sahip operatörüdür. Aşağıdaki ifadeye bakalım:

```
x = f1(), f2();
```
Bu ifadede virgül operatörünün sol terimi

```
x = f1()
```
sağ terimi ise

```
f2()
```
ifadesidir. `x` değişkenine virgül operatörünün ürettiği değeri atamak için öncelik parantezi kullanılmalıydı:

```
x = (f1(), f2());
```
## virgül operatörünün ürettiği değer
Virgül operatörünün ürettiği değer sağ teriminin değeridir. Aşağıdaki koda bakalım:

```
#include <stdio.h>

int main()
{
	int x = 10, y = 20, z;

	z = (x, y);
	printf("z = %d\n", z); //20
	z = (y, x);
	printf("z = %d\n", z); //10

	return 0;
}
```
Bir gerçek sayı sabitini yazarken nokta karakteri yerine yanlışlıkla virgül operatörünün kullanılması sık yapılan kodlama hatalarındandır:

```
#include <stdio.h>

int main()
{
	double d;

	for (d = 3.5; d < 5,0; d += 0.2) {
		printf("%f\n", d);
	}

	return 0;
}
```
Yukarıdaki kodda

`d < 5,0`

ifadesinde nokta karakteri yerine yanlışlıkla virgül karakteri yazılmış. 
Bu durumda virgül operatörünün ürettiği değer `0` olacağından koşul ifadesi lojik `"yanlış"` olarak yorumlanacak ve döngü gövdesindeki deyim hiç yürütülmeyecek.

## virgül operatörünün kullanılmasına ilişkin temalar
Virgül operatörünün kullanılmasıyla birden fazla ifade deyimi `(expression statement)` tek bir ifade deyimine dönüştürülebilir. 
Mantıksal ilişki içinde kullanılan aşağıdaki gibi üç deyim olsun:

```
++x;
y += x;
z = foo(y);
```
Bu üç ifade deyimi yerine aşağıdaki ifade deyimini yazabiliriz:

```
++x, y += x, z = foo(y);
```
`Virgül` operatörü bir yan etki noktası oluşturduğundan, yukarıda yazılan deyimin daha önce yazılan 3 ayrı deyimden sonuç olarak bir farkı yoktur.
Peki neden birden fazla ifade deyimi yazmak yerine tek bir ifade deyimi yazmak isteyelim? Kodlayıcıyı buna teşvik eden birden fazla neden olabilir:
+ Birden fazla ifade deyimi arasındaki mantıksal ilişki daha iyi vurgulanabilir.
+ Tek bir deyim yazmanın sentaks açısından zorunlu olduğu yerlerde yani birden fazla deyim yazmanın geçersiz olduğu durumlarda yaptırılacak işlemler `virgül` operatörünün kullanıldığı bir ifade deyimi `(expression statement)` olarak yazılabilir.
+ Blok kullanımından kaçınılabilir.

Aşağıdaki kodda bir yazıyı ters çeviren `strrev` isimli bir işlevin tanımı yer alıyor. İşlev ters çevirdiği yazının adresiyle geri dönüyor:

```
#include <string.h>

char *strrev(char *p)
{
	char *ps, *pe, temp;

	for (ps = p, pe = p + strlen(p) - 1; ps < pe; ++ps, --pe) 
		temp = *ps, *ps = *pe, *pe = temp;

	return p;
}
```
İşlevimiz `virgül` operatörünün kullanımına ilişkin temaları vurguluyor. 
`for` parantezinin birinci ve üçüncü kısmında `virgül` operatörünün kullanıldığını görüyorsunuz. 
`for` deyiminin bloklanmamış gövdesinde, `virgül` operatörlerinin kullanıldığı ifade deyimi ile döngünün her turunda yazının iki karakteri takas ediliyor.

`if` deyiminin koşul ifadelerinde ya da `while` döngü deyiminin kontrol ifadelerinde virgül operatörünün kullanılması bazı kodlayıcıların tercih ettiği idiyomatik bir yapıdır:

```
if (y = func(x), y > x)
	foo(y);
```
Yukarıdaki `if` deyiminin parantezi içindeki

```
y = func(x)
```
ataması `if` deyiminden önce ayrı bir deyim olarak yazılabilirdi. 
Kodlayıcımız bu atamanın `if` deyimi ile mantıksal ilişkisini vurgulamak için virgül operatörünü kullanarak atamayı `if` parantezi içine almış.

`assert` makrosunun kullanımında `virgül` operatörü ile oluşturulan bir ifade ile kullanıcı ekranına hatayı açıklayan bir yazı yazdırılabilir:

```
#include <stdio.h>
#include <assert.h>

int main()
{
	int x, y;

	printf("iki sayi giriniz: ");
	scanf("%d%d", &x, &y);
	assert(("sifira bolme hatasi", y != 0));
	x /= y;
	//
}
```

`Virgül` operatörü `return` deyiminin ifadelerinde de karşımıza çıkabilir. Bir işlev tanımında aşağıdaki gibi bir `if` deyimi olsun:

```
if (error_flag) {
	errno = EPIPE;
	return -1;
}
```

`error_flag` değişkeni `0`'dan farklı bir değerde ise global `errno` değişkenine hatanın kimliğini ifade den `EPIPE` değeri atanıyor ve 
işlev bir hata değeri olarak `-1` değerini döndürüyor. 
Şimdi de yukarıdakine eşdeğer olan aşağıdaki koda bakalım:

```
if (error_flag) 
	return errno = EPIPE, -1;
```

`if` deyiminin doğru kısmında tek bir deyim olduğu için artık `if` deyiminin doğru kısmı bloklanmak zorunda değil. 
`return` ifadesinde virgül operatörünün kullanımına dikkat edin. 
Önce `errno` değişkenine atama yapılacak. 
İşlevin geri dönüş değeri `virgül` operatörünün ürettiği `-1` değeri olacak.

## operatör olan `virgül` ile ayıraç olan `virgül`ün birbirinden ayrılması
`Virgül` atomu bazı yerlerde bir ayıraç olarak kullanılır.
Bu durumda sentaksa ilişkin öğeler `virgül` atomuyla birbirinden ayrılarak bir liste oluşturulur. 
Bu yapıya ingilizcede `"comma separated list"` (virgüllerle ayrılan liste) denir. 
`Virgül` atomunun bu biçimde kullanılması operatör olarak kullanımından tamamen ayrıdır.

```
int x = 10, y = 20, z = 30;
```
Yukarıdaki kodda `x, y` ve `z` isimli değişkenler tanımlanıyor. Burada kullanılan `virgül` atomu operatör değildir.

```
func(a, b)
```
Yukarıdaki kodda ise `func` işlevine `a` ve `b` ifadeleri argüman olarak gönderiliyor.
Buradaki `virgül` de bir operatör değildir.

```
int a[5] = { 1, 2, 3, 4, 5 };
```

Yukarıdaki kodda ismi `a` olan `5` öğeli bir diziye ilk değer veriliyor. 
Burada kullanılan virgül atomları da operatör değildir. 
Sentaksın gerektirdiği `virgül` atomu yerine operatör olan virgül kullanmak için ifade öncelik parantezi içine alınmalıdır:

```
int a = 10, b = 5, c = 18;
foo((a += b, c - a));
```

Yukarıdaki kodda ise `foo` işlevi virgül operatörünün ürettiği değerle çağrılıyor. 
`Virgül` operatörünün yan etki noktası oluşturduğunu hatırlayalım. 
İşlev `3` değeri ile çağrılıyor.
Şimdi de aşağıdaki kodu derleyip çalıştırın:

```
#include <stdio.h>

int main()
{
	int a[5] = { 1, 2, 3, 4, 5};
	int b[5] = { (1, 2, 3, 4, 5) };
	int k;

	for (k = 0; k < 5; ++k)
		printf("%d ", a[k]);
	
	printf("\n");

	for (k = 0; k < 5; ++k)
		printf("%d ", b[k]);

	printf("\n");

	return 0;
}
```
`a` dizisine ilk değer vermekte kullanılan virgül bir operatör değilken b dizisine ilk değer verilirken kullanılan virgül atomları operatör görevindeler. Programın çıktısı şu şekilde olacak:

```
1 2 3 4 5
5 0 0 0 0
```
