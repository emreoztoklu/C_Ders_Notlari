# sizeof operatörü – 1


C dilinin anahtar sözcüklerinden biri olan `sizeof` bir operatör görevindedir. 
Tek terimli `(operand)` olan ve önek konumunda `(unary prefix)` bulunan `sizeof` operatörü, bir nesnenin bellekte kaç `byte` yer kapladığı değerini üretir. 
Bu operatörün ürettiği değer, derleme zamanında derleyici için bir sabit ifadesidir. 
`sizeof` operatörünün ürettiği değer standart bir `typedef` ismi `(tür eş ismi)` olan `size_t` türündendir. 
`size_t` türü derleyiciye bağlı olarak seçilen bir işaretsiz tamsayı türüdür. 
`size_t` türü derleyiciler tarafından `unsigned int, unsigned long` ya da `unsigned long long` türlerinden birinin eş ismi olarak seçilebilir. 
`size_t` türünün `typedef` bildirimi standart `stddef.h` isimli başlık dosyasında yapılmıştır.

`sizeof` operatörü iki ayrı şekilde kullanılabilir:

__Operatörün terimi  olarak bir tür bilgisi kullanılabilir.__
Bu durumda terimin parantez içine alınması zorunludur:

```
sizeof(int)
sizeof(double)
sizeof(long)
```

Operatör bu durumda terimi olan türden bir nesnenin kullanılan sistemde, bellekte kaç `byte` yer kapladığı değerini üretir. 
Örneğin `Windows` ya da `UNIX` sistemlerinde

```
sizeof(int)
```

ifadesinin değeri tipik olarak `4` ya da `8`'dir.

__Operatörün terimi olarak herhangi bir ifade kullanılabilir.__
Bu durumda terimin parantez içine alınması zorunlu değildir. Ancak programcıların çoğu okunabilirlik açısından operatörün terimi olan ifadeyi parantez içine almayı tercih eder:

```
#include <stdio.h>

int main()
{
    int x = 10;
    double d = 2.3;

    printf("sizeof x   = %zu\n", sizeof x);
    printf("sizeof (d) = %zu\n", sizeof(d));
    printf("sizeof (2 + 5.3) = %zu\n", sizeof(2 + 5.3));
}

```

Yukarıdaki kodda

```
sizeof(2 + 5.3)
```

`sizeof` operatörünün terimi olan ifade `double` türdendir. Bu yüzden bu ifadenin değeri

```
sizeof(double)
```

ifadesinin değerine eşittir.

__Bir dizinin ismi `sizeof` operatörünün terimi olduğunda, derleyici dizi ismini dizinin ilk elemanının adresine dönüştürmez. `sizeof` operatörü bu durumda dizinin bellekte kaç `byte` yer kapladığı değerini üretir.__
Aşağıdaki koda bakalım:

```
#include <stdio.h>

int main()
{
    char s[10] = "ali";
    int a[20] = { 0 };
    double b[40] = { 0 };

    printf("sizeof(s) = %u\n", sizeof(s));
    printf("sizeof(a) = %u\n", sizeof(a));
    printf("sizeof(b) = %u\n", sizeof(b));
}
```

Benim çalıştığım sistemde bu programı derleyip çalıştırdığımda programın çıktısı şu şekilde oldu:

```
sizeof(s) = 10
sizeof(a) = 80
sizeof(b) = 320
```
Aşağıdaki kod `sizeof` operatörünün terimi olması durumunda dizi isminden dizi adresine dönüşüm `(array decay/array to pointer conversion)` yapılmadığını gösteriyor.
```
#include <stdio.h>
int main()
{
    int a[10] = { 0 };
    printf("sizeof (int*) = %zu\n", sizeof (int *));
    printf("sizeof a      = %zu\n", sizeof a);
    printf("sizeof &a[0]  = %zu\n", sizeof &a[0]);
}
```
Dizi için elde edilen `sizeof` değerini dizinin bir elemanı için elde edilen `sizeof` değerine bölersek bir derleme zamanı sabiti olarak dizinin boyutunu buluruz:

`a` herhangi türden bir dizi olsun. Bu durumda

```
sizeof(a) / sizeof(a[0])
```

bir sabit ifadesidir ve bu ifadenin değeri `a` dizisinin boyutudur. Dizinin bir elemanını bir ifade olarak kullanmak için içerik `(dereferencing)` operatörü de kullanılabilir:

```
sizeof(a) / sizeof(*a)
```

ifadesinin değeri yine `a` dizisinin boyutudur:

Bir dizi tanımında tanımlanan diziye eğer ilk değer verilirse dizinin boyutunu belirtmek zorunlu değildir. Derleyici bu durumda dizi boyutunu listedeki ilk değerlerin sayısından çıkartır. Aşağıdaki kodu inceleyin:

```
#include <stdio.h>

int main()
{
    int a[] = {2, 5, 7, 8, 9, 23, 67};
    
    for (size_t i = 0; i < sizeof(a) / sizeof(a[0]); ++i)
		printf("%d ", a[i]);
}
```

`main` işlevi içinde tanımlanan `int` türden `a` isimli dizi boyutu belirtilmeden ilk değer verilerek tanımlanmış. 
Bu durumda derleyici listedeki değerlerin sayısını sayarak dizinin boyutunu `8` kabul eder. 
`main` işlevi içinde yer alan `for` döngü deyimi, dizinin eleman sayısı kadar, yani `8` kez döner.
Şimdi kaynak kodda değişiklik yapıldığını, `a` dizisine birkaç eleman daha eklendiğini düşünelim:

```
int a[] = {2, 5, 7, 8, 9, 23, 67, 34, 58, 45, 92};
```

Bu durumda `for` döngü deyiminde bir değişiklik yapılmasına gerek kalmaz. 
Çünkü derleyici bu kez derleme zamanında dizinin boyutunu `11` olarak hesaplar ve `for` döngü deyimi içinde kullanılan

```
sizeof(a) / sizeof(a[0])
```

ifadesi de bu kez `11` değerini üretir.

`sizeof` operatörü ile dizi boyutunu elde eden ifade okuma ve yazma kolaylığı sağlaması için çoğunlukla bir işlevsel makro `(functional macro)` olarak kullanılır:

```
#define asize(a)  (sizeof((a)) / sizeof((*a)))
```

#### sizeof operatörünün önceliği

Tek terimli tüm operatörlerin, daha önce oluşturduğumuz operatör öncelik tablosunun ikinci seviyesinde yer aldığını biliyorsunuz.
`sizeof` da ikinci seviyede bulunan bir operatördür:

```
#include <stdio.h>

int main()
{
    int x = 10;

    size_t y1 = sizeof x + 5;
    size_t y2 = sizeof (x + 5);
}
```

Yukarıdaki kod `int` türünün `4 byte` olduğu bir sistemde derleniyor olsun:

```
sizeof x + 5
```

ifadesinin değeri `9` iken

```
sizeof (x + 5)
```

ifadesinin değeri `4`’tür.

#### sizeof operatörünün terimi olan ifadenin yan etkisi

`sizeof` operatörünün terimi olan ifade değerlendirilmez. Derleyici operatörünün terimi olan ifadeyi derleme zamanında bir tür bilgisi olarak ele alır ve bu ifade için bir işlem kodu üretmez.

```
#include <stdio.h>

int func()
{
    printf("func()");
    return 1;
}


int main()
{
    int x = 10;

    size_t y = sizeof(x++);
    size_t z = sizeof(func());
	
    printf("x = %d\n", x);
}
```` 

Yukarıdaki kodda `main` işlevi içinde `x` değişkeni arttırılmaz. 

```
x++
```

ifadesi `int` türdendir. 
Derleyici bu ifadeyi yalnızca bir tür bilgisi olarak ele alır. 
İfadenin türü `int` olduğundan `y` değişkenine `sizeof(int)` değeri atanır.
Yine örnek kodda yer alan `func` işlevi çağrılmayacaktır.

```
func()
```

ifadesi `int` türdendir. 
Derleyici yine bu ifadeyi yalnızca bir tür bilgisi olarak ele alır. 
İfadenin türü `int` olduğundan `z` değişkenine `sizeof(int)`  değeri atanır.

__`sizeof` operatörünün ürettiği değer derleme zamanında elde edildiğinden bir sabit ifadesidir `(constant expression)`.__
Bu yüzden bu değer sabit ifadesi gereken her yerde kullanılabilir:

```
#include <stdio.h>

int main()
{
    int a[] = { 1, 5, 6, 7 };
    double b[sizeof(a) / sizeof(a[0])] = { 0. };
    //...
}
``` 
`C99` standartları ile dile eklenen `C11` standartları ile seçimlik hale getirilen değişken boyutta diziler `(variable length array)` için bu doğru değildir. Eğer `sizeof` operatörünün terimi değişken boyutta bir dizi ise bir sabit ifadesi elde edilmez.

#### sizeof operatörü ne amaçla kullanılır?

Belirli bir türden nesne için bellekte ne büyüklükte bir bellek alanına gereksinim duyulacağı sistemden sisteme farklılık gösterebilir. 
Türler için belek ihtiyacının sistemden sisteme farklı olabilmesi bazı uygulamalarda taşınabilirlik sorunlarına yol açabilir. 
`sizeof` operatörünün, genel olarak bu tür taşınabilirlik sorunlarını ortadan kaldırmaya yönelik olarak kullanıldığı söylenebilir.

Yazımızı bir soruyla bitirelim. Aşağıdaki C programı derlenip çalıştırıldığında standart çıkış akımına ne yazar?
```
#include<stdio.h>

#define asize(a) (sizeof(a) / sizeof(*a))
int a[] = {1, 2, 3, 4, 5};

int main()
{
    for (int i = -1; i <= asize(a) - 2; ++i)
        printf("%d\n", a[i + 1]);
}
```
