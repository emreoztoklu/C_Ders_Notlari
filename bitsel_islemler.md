# Bitsel İşlemler

Bitsel işlemleri anlatırken konunun daha iyi anlaşılmasını sağlamak için tamsayıların bitlerini sık sık standart çıkış akımına yazacağız. Önce bu işi gerçekleştirecek bir işlevi tanımlayalım:

```
#include <stdlib.h>
#include <stdio.h>

void bprint(int val)
{
	char str[sizeof(int) * 8 + 1];
	itoa(val, str, 2);
	printf("%0*s\n", sizeof(int) * 8, str);
}
```

Tanımladığımız `bprint` işlevi kendisine gönderilen tam sayının bitlerini yazdırmak için standart olmayan `itoa` işlevini ve formatlı çıkış işlevi `printf`'i kullandı. Şüphesiz bir tam sayının bitlerini yazdırmanın daha iyi yolu doğrudan bitsel operatörleri kullanmak. Bitsel operatörleri ele aldıktan sonra bu işlevin kodunu bu kez doğrudan bitsel operatörleri kullanarak yazacağız.

Önce `C`'nin tüm bitsel operatörlerini `(bitwise operators)` tanıyalım:

Bitsel operatörler, bir tam sayının bitleri üzerinde işlemler yaparlar. Daha çok sistem programlarında ya da düşük seviyeli kodlarda kullanılırlar. Bitsel operatörlerin ortak özellikleri, işleme soktukları tam sayıları bir bütün olarak değil, bit bit `(bitwise)` ele almalarıdır. Bu operatörlerin terimleri (operands)  yalnızca tam sayı türlerinden olabilir. Operandlarının gerçek sayı türlerinden olması geçersizdir.

Aşağıdaki tabloda C dilinde yer alan `11` bitsel operatör daha önce oluşturmuş olduğumuz operatör öncelik tablosundaki öncelik seviyelerine göre listeleniyor:

```
operatör önceliği        atom                	operatör
2                      ~                  bitsel değil
5                      >> <<              bitsel sağa kaydırma / bitsel sola kaydırma
8                       &                 bitsel ve
9	          	^                 bitsel özel veya
10	             	|	 	  bitsel veya
14	         	>>= <<= &= ^=|=	  bitsel işlemli atama operatörleri
```

Yukarıdaki operatörler içinde, yalnızca "bitsel değil" `(bitwise not)` operatörü, tek terimli önek konumunda `(unary prefix)` bir operatördür. Diğerleri iki iterimli `(binary)` ara ek konumunda `(infix)` bulunan operatörlerdir.

## bitsel değil operatörü
Bitsel değil operatörü `(bitwise not)`, diğer tüm tek terimli `(unary)` operatörler gibi operatör öncelik tablomuzun ikinci seviyesinde yer alıyor. Bu operatör, terimi olan tam sayının bitleri üzerinde `1`'e tümleme `(one's complement)` işlemi yaparak bir değer elde eder. Yani terimi olan tam sayının `1` olan bitlerini `0`, `0` olan bitlerini `1` yapacak şekilde bir değer üretir. Bu operatörün terimi bir nesne ise bu nesnenin değeri değişmez. Yani operatörün yan etkisi `(side effect)` yoktur. Aşağıda programı inceleyin:

```
int main()
{
        int x;

	printf("bir tam sayi giriniz: ");
	scanf("%d", &x);
	bprint(x);
	bprint(~x);
	printf("x = %d\n", x);

	return 0;
}
```

`int` türünün `32` bit olduğu benim çalıştığım sistemde örnek bir program çıktısı şu şekildeydi:

```
bir tam sayi giriniz: 6542
00000000000000000001100110001110
11111111111111111110011001110001
x = 6542
```

Şimdi de şu programı derleyip çalıştıralım:

```
#include <stdio.h>

int main()
{
	bprint(~0);
	printf("%d %u\n", ~0, ~0);
}
```

Bütün bitleri `0` olan `0` tam sayısının bitsel değili bütün bitleri `1` olan sayıdır. Bu sayı işaretli olarak ele alındığında `-1` ve işaretsiz olarak ele alındığında işaretsiz `int` türünün en büyük değeridir, değil mi?

## bitsel kaydırma operatörleri

İki tane bitsel kaydırma operatörü `(bitwise shift operator)` vardır:
Bitsel sağa kaydırma operatörü >> `(bitwise right shift)`
Bitsel sola kaydırma operatörü << `bitwise left shift)`

Her iki operatör de, (oluşturduğumuz) öncelik tablosunun `5.` seviyesindedir. Dolayısıyla bu operatörlerin önceliği tüm aritmetik operatörlerden daha düşük, fakat karşılaştırma operatörlerinden daha yüksektir. Ara ek konumunda bulunan bitsel kaydırma operatörlerinin iki operandları vardır `(binary infix)`.

Kaydırma operatörlerinin sağ operandı, negatif değerde olmamalı ve sistemin `int` türünün toplam bit sayısından daha küçük olmalıdır. Bu koşullar sağlanmamış ise oluşan durum tanımsızdır `(undefined behaviour)`. Örneğin `Windows` sistemlerinde `int` türden bir değerin `32` ya da daha fazla sola ya da sağa kaydırılması tanımsızdır. Bu durumdan kaçınılmalıdır.

Bitsel sola kaydırma operatörü, sol terimi olan tam sayının, sağ terimi olan olan tam sayı kadar pozisyon sola kaydırılmasından elde edilen değeri üretir. Sınır dışına çıkan bitler için, sayının sağından `0` biti ile besleme yapılır. Aşağıdaki kodu inceleyin:

```
void bprint(int val);

int main()
{
	int x = 1;

	while (x) {
		bprint(x);
		x <<= 1;
	}
}
```
Yukarıdaki kodda yer alan

```
x <<= 1
```

ifadesi

```
x = x << 1
```

ifadesi ile aynı anlamdadır.

Bir tam sayıyı, sola bitsel olarak `1` pozisyon kaydırmakla o tam sayının ikiyle çarpılmış değerini elde etmiş oluruz:

```
#include <stdio.h>

int main()
{
	unsigned int x;

	printf("bir tamsayi girin : ");
	scanf("%u", &x);

	printf("x        = %u\n", x);
	printf("x << 1   = %u\n", x << 1);
}
```

Bitsel sağa kaydırma operatörü, sol operandı olan tam sayının, sağ operandı olan tam sayı kadar pozisyon sağa kaydırılmış değerini üretir. Sol operand işaretsiz `(unsigned)` bir tam sayı türünden ise, ya da işaretli `(signed)` bir tamsayı türünden ancak pozitif değere sahip ise, sınır dışına çıkan bitler yerine, sayının solundan besleme `0` biti ile yapılır. Sağa kaydırılacak ifadenin işaretli bir tam sayı türünden ve negatif değerde olması durumunda sınır dışına çıkan bitler için soldan yapılacak beslemenin `0` ya da `1` bitleriyle yapılması derleyiciye bağlıdır `(implementation defined)`. Yani derleyiciler bu durumda sayının işaretini korumak için soldan yapılacak beslemeyi `1` biti ile yapabilecek bir kod üretebilecekleri gibi, sayının işaretini korumayı düşünmeksizin `0` biti ile besleyecek bir kod da üretebilirler. İşaretli negatif bir tam sayının bitsel sağa kaydırılması taşınabilir bir özellik değildir. Aşağıdaki koda bakalım:

```
void bprint(int val);

int main()
{
	unsigned int x = ~(~0u >> 1);

	while(x) {
		bprint(x);
		x >>= 1;
	}
}
```

Yukarıdaki kodda

`~(~0u >> 1)`

ifadesiyle en yüksek anlamlı biti `1` diğer tüm bitleri `0` olan işaretsiz tam sayı elde ediliyor.

Bir tam sayıyı sağa bitsel olarak `1` kaydırmakla, o sayının ikiye bölünmüş değeri elde edilir:

```
#include <stdio.h>

int main()
{
	unsigned int x;

	printf("bir tamsayi girin : ");
	scanf("%u", &x);

	printf("x        = %u\n", x);
	printf("x >> 1   = %u\n", x >> 1);
}
```

Bitsel kaydırma operatörlerinin yan etkileri yoktur. Yani sol terimleri bir nesne ise, bu nesnenin bellekteki değeri değişmez. Kaydırma işlemi ile sol terim olan nesnenin değeri değiştirilmek isteniyorsa, bu operatörlerin işlemli atama biçimleri kullanılmalıdır.

Bitsel kaydırma operatörlerinin öncelik yönü soldan sağadır:

`x << 4 >> 8`

`x`, `16` bitlik işaretsiz bir tam sayı değişken olsun. Yukarıdaki ifade derleyici tarafından

```
x << 4 >> 8
```


biçiminde ele alınır. Bu ifade ile `x` değişkeninin ortadaki `8` bitinin tam sayı değeri elde edilir.

## bitsel ve işleci 

"Bitsel ve" operatörü `(bitwise and)`, operatör öncelik tablomuzun 8. seviyesinde yer alıyor. Bu seviyenin öncelik yönü soldan sağadır `(left associative)`. Operatörün terimleri nesne gösteren ifadeler ise, bu nesnelerin değerleri değişmez, yani operatörün yan etkisi yoktur. Operatör değer üretmek için terimi olan tam sayıların karşılıklı bitlerini "ve" işlemine sokar. "ve" operatörüne ilişkin işlem tablosunu hatırlayalım:

```
x	  y	  x & y
0	  0	    0
0	  1	    0
1	  0	    0
1	  1	    1
```

"Bitsel ve" operatörünün ürettiği değer, terimlerinin karşılıklı bitlerinin "ve" işlemine sokulmasıyla elde edilen değerdir:

+ 1 biti "bitsel ve" işleminde etkisiz elemandır.
+ 0 biti "bitsel ve" işleminde yutan elemandır.

```
#include <stdio.h>

void bprint(int val);

int main()
{
	int x, y;

	printf("iki tamsayi giriniz : ");
	scanf("%d%d", &x, &y);
	bprint(x);
	bprint(y);
	bprint(x & y);

	printf("x = %d\n", x);
	printf("y = %d\n", y);
}
```

"Mantıksal ve" operatörü yerine yanlışlıkla "bitsel ve" operatörünü kullanmak sık yapılan bir hatadır. Aşağıdaki kodu inceleyin:

```
#include <stdio.h>

void bprint(int val);

int main()
{
	int x, y;

	printf("iki tamsayi giriniz : ");
	scanf("%d%d", &x, &y);
	printf("lojik ve islemi\n");
	if (x && y)
		printf("dogru\n");
	else
		printf("yanlis\n");
	
	printf("bitsel ve islemi\n");
	if (x & y)
		printf("dogru\n");
	else
		printf("yanlis\n");
}
```

Yukarıdaki programda standart giriş akımından alınan `x` ve `y` değerlerinin çoğu için hem lojik ve hem bitsel ve operatörü için ekrana aynı yazının yazdırıldığını göreceksiniz. Ancak şimdi `x` değişkeni  `7517` `y` değişkeni ise `8866` değerinde olsun:

```
x       7517   0001110101011101
y       8866   0010001010100010
x & y    0     0000000000000000
```

Bu durumda `x && y` ifadesinin değeri `1 (lojik doğru)` iken `x & y` ifadesini değeri `0 (lojik yanlış)` olacak.

## bitsel özel veya İşleci

Bitsel "özel veya" operatörü` (bitwise exor)` operatör öncelik tablomuzun `9.` seviyesinde yer alıyor. Bu seviyenin öncelik yönü yine soldan sağadır `(left associative)`. Bu operatörün bir yan etkisi yoktur, yani operatörün terimleri olan nesnelerin değeri değişmez. Bitsel özel veya işlevi terimleri olan tamsayıların karşılıklı bitlerini özel veya `(exclusive or)` işlemine sokarak bir değer elde eder. Bitsel "özel veya" operatörüne ilişkin işlem tablosu aşağıdaki gibidir:

```
x	y	x ^ y
0	0	0
0	1	1
1	0	1
1	1	0
```

Yukarıdaki tablo şöyle özetlenebilir: Operatörün terimleri aynı değere sahip ise, üretilen değer `0`, terimler farklı değerlerde ise üretilen değer `1` olur. Aşağıdaki kodu derleyip çalıştırın:

```
#include <stdio.h>

void bprint(int val);

int main()
{
	int x, y;

	printf("iki tamsayi giriniz : ");
	scanf("%d%d", &x, &y);
	bprint(x);
	bprint(y);
	bprint(x ^ y);

	printf("x = %d\n", x);
	printf("y = %d\n", y);
}
```

Bir tamsayı, arka arkaya aynı değerle bitsel özel veya işlemine sokulursa, tamsayının kendi değeri elde edilir:

```
#include <stdio.h>

int main()
{
	int x, y;

	printf("iki tamsayi giriniz: ");
	scanf("%d%d", &x, &y);

	x ^= y;

	printf("x = %d\n", x);

	x ^= y;

	printf("x = %d\n", x);

	return 0;
}
```

Bazı şifreleme algoritmalarında "özel veya" işleminin bu özelliğinden faydalanılır. 

## bitsel veya operatörü

"Bitsel veya" `(bitwise or)` operatörü, operatör öncelik tablomuzun `10.` seviyesindedir ve bu operatörün öncelik yönü soldan sağadır. Bu operatörün de yan etkisi yoktur, yani terimi olan nesnelerin değeri değişmez. Bitsel özel veya operatörü terimleri olan tamsayıların karşılıklı bitlerini "veya" işlemine sokar. Bitsel veya operatörüne ilişkin işlem tablosu şöyledir:

```
x	y	x | y
1	1	1
1	0	1
0	1	1
0	0	0
```

`0` biti “bitsel veya” işleminde etkisiz elemandır. `1` biti “bitsel veya” işleminde yutan elemandır.
Aşağıdaki programı derleyerek çalıştırın:

```
#include <stdio.h>

void bprint(int val);

int main()
{
	int x, y;

	printf("iki tamsayi giriniz : ");
	scanf("%d%d", &x, &y);
	bprint(x);
	bprint(y);
	bprint(x | y);

	printf("x = %d\n", x);
	printf("y = %d\n", y);

	return 0;
}
```

Mantıksal operatörlerden farklı olarak bitsel operatörler kısa devre davranışına sahip değildir. Yani bu operatörlerin her iki terimi de mutlaka işlenir. 

```
#include <stdio.h>

int main()
{
	int x = 1;
	int y = 16;
	int z;

	z = y | x--;
	y = x & ++z;

	printf("x = %d\n", x); //0
	printf("y = %d\n", y); //0
	printf("z = %d\n", z); //18
}
```

## bitsel işlemli atama operatörleri

Bitsel değil operatörünün dışında, tüm bitsel işleçlere ilişkin işlemli atama biçimleri vardır. Daha önce de belirtildiği gibi bitsel operatörlerin yan etkileri `(side effect)` yoktur. Bitsel operatörler terimleri olan nesnelerin bellekteki değerlerini değiştirmez. Bir bitsel operatör ile bir nesnenin değerinin değiştirilmesi isteniyorsa yazma ve okuma kolaylığı için için işlemli atma operatörleri tercih edilmelidir:

```
x = x << y yerine x <<= y
x = x >> y yerine x >>= y
x = x & y yerine x &= y
x = x ^ y yerine x ^= y
x = x | y yerine x |= y
```

ifadeleri kullanılabilir.

Bitsel özel veya işlemli atama operatörleriyle, tamsayı türlerinden iki değişkenin değerlerinin, üçüncü bir değişken kullanılmadan takas `(swap)` edilmesi `"exorswap"` olarak isimlendirilen bir `C` idiyomudur.

```
#include <stdio.h>

int main()
{
	int x, y;

	printf("iki tamsayi giriniz: ");
	scanf("%d%d", &x, &y);

	x ^= y, y ^= x, x ^= y;

	printf("x = %d\n", x);
	printf("y = %d\n", y);
}
```

Yukarıdaki programda, `x` ve `y` değişkenlerinin değerleri takas ediliyor. Bir tam sayı kendisiyle bitsel özel veya işlemine sokulursa `0`
değeri elde edilir. Yani nesne kendisiyle bu yolla takas edilmemelidir.

Özellikle alt seviyeli kodlarda bir tam sayının bitleri üzerinde bazı işlemlerin yapılması `(bitwise manipulation)` sıklıkla gerekli olur. En sık yapılan bitsel işlemler şunlardır:

## bir tam sayının belirli bir bitinin birlenmesi
Buna tam sayının belirli bir bitinin `"set edilmesi"` de denir. Bir tam sayının belirli bir bitini birlemek için, tam sayı ilgili biti `1` olan ve diğer bitleri `0` olan bir sayıyla "bitsel veya" işlemine sokulmalıdır. Çünkü bitsel veya işleminde `1` yutan eleman `0` ise etkisiz elemandır. Aşağıdaki koda bakalım:

```
#include <stdio.h>

int main()
{
	int ch = 0x0041;		/* ch = 65 (0000 0000 0100 0001) */
	int mask = 0x0020;		/* mask = 32 (0000 0000 0010 0000) */

	ch |= mask;			/* ch = 97 (0000 0000 0110 0001) */
	printf("ch = %d\n", ch);	/* ch = 97 */
}
```

Yukarıdaki kodda `ch` değişkeninin `5.` biti birleniyor:

`x` bir tam sayı, `n` de bu sayının herhangi bir bitinin indeksi olmak üzere bir tam sayının`n`. bitini birleyecek bir ifade şu biçimde yazılabilir:

```
x |= 1 << k
```
Bu tür bitsel işlemlerde kullanılan bitleri manipüle etmek için kullanılan

```
1 << k
```

gibi ifadelere "bitsel maske" `(bitmask)` denir.

## bir tam sayının belirli bir bitinin sıfırlanması
Bir tam sayının belirli bir bitini sıfırlamak `(clear / reset)` için tam sayı, ilgili biti `0` olan ve diğer bitleri `1` olan bir maskeyle "bitsel ve" işlemine sokulur. Çünkü "bitsel ve" işleminde `0` yutan eleman `1` ise etkisiz elemandır. Bu bitsel maske aynı biti birlemekte kullanılan maskenin bitsel değilidir. Aşağıdaki örnekte bir tam sayının `5.` biti sıfırlanıyor:

```
#include <stdio.h>

int main()
{
	int ch = 0x0061;		/* ch = 97 (0000 0000 0110 0001)   */
	int mask = ~0x0020;		/* mask = ~32 (1111 1111 1101 1111)*/
	ch &= mask;                     /* ch = 65 (0000 0000 0100 0001)    */
	printf("ch = %d\n", ch);	/* ch = 65 */
}
```

`x` bir tam sayı, `n` de bu sayının herhangi bir bitinin indeksi olmak üzere, `x` tam sayısının `n.` bitini sıfırlayan bir ifade aşağıdaki gibi genelleştirilebilir:

```
x  &= ~(1 << k);
```

## bir tam sayının belirli bir bitini değiştirmek
Bazı kodlarda bir tam sayının belirli bir bitinin değiştirilmesi `(toggle - flip)` gerekir. Yani söz konusu bit `1` ise `0`, `0` ise `1` yapılmalıdır. Bu amaçla "bitsel özel veya" operatörü kullanılır. Bitsel özel veya işlecinde `0` biti etkisiz elemandır.
Bir sayının `n`. bitinin değerini değiştirmek için, sayı, `n.` biti `1`, diğer bitleri `0` olan bir maske ile "bitsel özel veya" işlemine sokulur. `x`, bir tam sayı, `n` de bu sayının herhangi bir bit numarası olmak üzere, `x` tam sayısının `n.` bitini ters çeviren bir ifade şu şekilde yazılabilir:

```
x ^= 1 << n;
```

## bir tam sayının belirli bir bit değerinin elde edilmesi (0 mı 1 mi)
Bir tam sayının belirli bir bitinin `0` mı `1` mi olduğunun öğrenilmesi için, söz konusu tam sayı, ilgili biti `1` olan ve diğer bitleri `0` olan bir maskeyle "bitsel ve" işlemine sokulmasından elde edilen değer mantıksal olarak yorumlanmalıdır. Çünkü "bitsel ve" işleminde `0` yutan eleman, `1` ise etkisiz elemandır. İfadenin lojik değeri "dogru" olursa, ilgili bit `1`, yanlış ise ilgili bit `0` demektir. `x` bir tam sayı, `n` de bu tam sayının herhangi bir bitinin indeksi olmak üzere `x` tam sayısının `n`. bitinin `1` ya da `0` olduğunu sınayan bir deyim aşağıdaki biçimde yazılabilir:

```
if (x & (1 << n))
	/* n. bit 1 */
else
	/* n. bit 0 */
 ```
Bir pozitif tam sayının tek sayı olup olmadığı "bitsel ve" operatöryle sınanabilir. Bir tam sayı tek sayı ise sayının `0`. biti `1`'dir.

```
#include <stdio.h>

int main()
{
  	int x;
        
 	printf("bir sayı giriniz ");
 	scanf("%d", &x);
	if (x & 1)
		printf("%d tek sayı\n", x);
	else
		printf("%d çift sayı\n", x);
}
```

## birden fazla bit üzerinde işlem yapmak
Bir tam sayının belirli bitlerini sıfırlamak için ne yapılabilir? Örneğin `int` türden bir değişkenin `7. , 8. ve 9.` bitlerini, diğer bitlerin değerlerini değiştirmeden sıfırlamak isteyelim. Bunu gerçekleştirmek için, tam sayı 7., 8. ve `9` bitleri `0` olan diğer bitleri `1` olan bir maske ile bitsel ve işlemine sokabiliriz. Örneğin `int` türünün `16` bit olduğu bir sistemde bu maskenin ikilik sayı sistemindeki gösterimi aşağıdaki gibidir:

```
1111 1100 0111 1111
```

Bu tam sayının onaltılık sayı sisteminde gösterimi

`0XF7CF`

biçimindedir, değil mi?

`x &= 0xFC7F;`

Şimdi de bir tam sayının bitlerini standart çıkış akımına yazdıran `showbits` isimli işlevi tanımlayalım. Sayının ilk olarak en yüksek anlamlı bitini yazdırmalıyız, değil mi?

```
#include <stdio.h>

void showbits(int x)
{
	int i = (int)(sizeof(int) * 8 - 1);
	for (; i >= 0; --i)
		putchar (x >> i & 1 ? '1' : '0');
}

int main()
{
	int val;

	printf("bir sayı giriniz : ");
	scanf("%d", &val);
	showbits(val);
}
```

`showbits` işlevinde `i` değişkenine tam sayının bit sayısının bir eksiği ile ilk değer veriliyor. Örneğin `32` bitlik `int` türü için `i` değişkenine `31` değeri verilecek. `i >> 31` ifadesini `1` değeri ile bitsel ve işlemine sokarak sayının `31.` bitinin `1` mi `0` mı olduğunu öğreneceğiz.

Aşağıda aynı işi farklı bir biçimde yapan `showbits2` isimli işlevi tanımlıyoruz:

```
#include <stdio.h>

void showbits2(int x)
{
	unsigned int i = ~(~0u >> 1);
	
	while (i) {
		putchar (x & i ? '1' : '0');
		i >>= 1;
	}
	putchar('\n');
}
```

Bu kez maske olarak kullanılacak en yüksek anlamlı biti `1` diğer bitleri `0` olan bir tam sayı ile başladık. Döngünün her turunda bitsel maskemizi sağa bir pozisyon kaydırdık. Böylece maskemiz içinde bulunan `1` biti döngünün her turunda `x`'in farklı bir bitine karşılık geldi.

Alt seviyeli kodlarda sık yapılan bir işlem bir tam sayının kaç bitinin `1` olduğunu saptamaktır. Birçok uygulamada bu işlemin çok hızlı yapılması gerekir. İşlev çağrılarının oluşturacağı ek maliyetten kaçınmak için `32` bitlik bir tam sayının kaç bitinin `1` olduğunu hesaplayan kodu bir işlevsel makro `(function-like macro)` tanımlıyoruz:

```
//bitmanip.c dosyasi
const char asbc[] = {
	0, 1, 1, 2, 1, 2, 2, 3, 1, 2, 2, 3, 2, 3, 3, 4,
	1, 2, 2, 3, 2, 3, 3, 4, 2, 3, 3, 4, 3, 4, 4, 5,
	1, 2, 2, 3, 2, 3, 3, 4, 2, 3, 3, 4, 3, 4, 4, 5,
	2, 3, 3, 4, 3, 4, 4, 5, 3, 4, 4, 5, 4, 5, 5, 6,
	1, 2, 2, 3, 2, 3, 3, 4, 2, 3, 3, 4, 3, 4, 4, 5,
	2, 3, 3, 4, 3, 4, 4, 5, 3, 4, 4, 5, 4, 5, 5, 6,
	2, 3, 3, 4, 3, 4, 4, 5, 3, 4, 4, 5, 4, 5, 5, 6,
	3, 4, 4, 5, 4, 5, 5, 6, 4, 5, 5, 6, 5, 6, 6, 7,
	1, 2, 2, 3, 2, 3, 3, 4, 2, 3, 3, 4, 3, 4, 4, 5,
	2, 3, 3, 4, 3, 4, 4, 5, 3, 4, 4, 5, 4, 5, 5, 6,
	2, 3, 3, 4, 3, 4, 4, 5, 3, 4, 4, 5, 4, 5, 5, 6,
	3, 4, 4, 5, 4, 5, 5, 6, 4, 5, 5, 6, 5, 6, 6, 7,
	2, 3, 3, 4, 3, 4, 4, 5, 3, 4, 4, 5, 4, 5, 5, 6,
	3, 4, 4, 5, 4, 5, 5, 6, 4, 5, 5, 6, 5, 6, 6, 7,
	3, 4, 4, 5, 4, 5, 5, 6, 4, 5, 5, 6, 5, 6, 6, 7,
	4, 5, 5, 6, 5, 6, 6, 7, 5, 6, 6, 7, 6, 7, 7, 8,
};
```

```
//bitmanip.h dosyası

extern const char asbc[];

#define sbc(x) asbc[((x) & 0xff)] + asbc[((x >> 8) & 0xff)] + \
               asbc[((x >> 16) & 0xff)] + asbc[((x >> 24) & 0xff)]
```

Kod dosyamızda tanımlanmış olan `256` öğeli `asbc` dizisi bir arama tablosu `(lookup table)` olarak kullanılıyor. Bu dizinin `n` indisli öğesinin değeri `n` tam sayısının kaç bitinin bir olduğu bilgisi. Örneğin dizinin `15` indisli öğesinin değeri `4.` Çünkü `15` tam sayı değerinin `4` biti `1`.
Böylece bir byte değerini bu diziye indis yaptığımızda o byte'ın kaç bitinin `1` olduğunu bu tablodan elde edebiliyoruz. `32` bitlik bir tam sayı toplam `4` byte'a sahip olduğu için her bir byte için `1` olan bit sayısını ayrı ayrı bu tablodan bakarak buluyor ve bu değerleri topluyoruz.
Bir tam sayının düşük anlamlı `byte`'ını tam sayı değeri olarak elde etmek için tam sayıyı `255` değeriyle bitsel ve `(&)` işlemine sokmamız gerekiyor. `32` bitlik tam sayımız `x`

```
0XA2B4FC86
```

değerinde olsun.

```
x & 0xFF           ---> 0x86 ---> asbc[0x86]  ---->3
(x >> 8) & 0xFF    ---> 0xFC ---> asbc[0xFC]  ---->6
(x >> 16) & 0xFF   ---> 0xB4 ---> asbc[0xB4]  ---->4
(x >> 24) & 0xFF   ---> 0xA2 ---> asbc[0xA2]  ---->3
```

Şimdi de bir tam sayının `2`'nin kuvveti olup olmadığını sınayan bir işlevsel makro yazacağız. `2`'nin tüm kuvvetlerinin yalnızca tek bir biti `1`'dir. Bu durumda `n` bir tam sayı olmak üzere `2`'nin `n`. kuvvetinin `1` eksiği `n`. bitinden daha küçük indisli bitleri `1` diğer bitleri sıfır olan sayıdır.
Örneğin `2`'nin `5`. kuvveti için

```
32     00000000000000000000000000100000
31     00000000000000000000000000011111
```

Bu durumda eğer bir sayı `2`'nin kuvveti ise, bu sayının kendisi ile bir eksiğinin bitsel ve işlemine sokulması durumunda `0` değeri elde edilmelidir. `0` tam sayısı `2`'nin kuvveti olmamasına karşın bu kurala uyar. Bu yüzden makromuzda tam sayının `0` olması olasılığını da göz önüne alıyoruz:

```
#define isPowerOfTwo(x)    ((x) && !((x) & (x - 1)))
```
