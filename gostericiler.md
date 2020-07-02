### bu ders notu henüz tamamlanmadı. Her gün biraz ekleme yapıyorum. Allah'ın izniyle yakında bitireceğim

### Göstericiler (Pointers)

_C_ dilinde _pointer_ sözcüğü _"adres"_ anlamında kullanılmaktadır. 
Adres donanımsal bir kavram olmakla birlikte birçok programlama dilinin sentaksında yer almaz. 
_C_ dilinde ise adresler ve adreslere yönelik işlemler yoğun olarak kullanılır. 

<br>
Adresler temel olarak iki kategoriye ayrılmaktadır:

- Nesne adresleri _(object pointers)_
- Fonksiyon adresleri _(function pointers)_

Fonksiyon adreslerini daha sonra ele alacağız. </br>

C dilinde  nesnelerin _(değişkenlerin)_ adresleri değişkenlerde tutulabilir, fonksiyonlara argüman olarak gönderilebilir. 
Fonksiyonların geri dönüş değeri adres olabilir.
Bir dizinin elemanları adres tutan değişkenler olabilir. 

Peki bir değişkenin adresi dendiğinde ne anlaşılmalıdır. 
Bir nesnenin adresi nasıl bir soyutlamadır?
Nasıl _Necati Ergin_'in adresi onun nerede oturduğu bilgisi ise bir değişkenin adresi de o değişkenin programın çalışma zamanında bellekte nerede olduğunu belirten bir bilgidir.
C dilinde nesnelere adresleri yoluyla erişebilir onları adresleri yoluyla kullanabiliriz.

Bir ifade _(expression)_ bir nesnenin adresi anlamına gelebilir. 
Yani bazı ifadelerin değeri adrestir. </br>
_T_ bir nesnenin türü olmak üzere
_T_ türünden bir nesnenin adresi anlamına gelen ifadenin türü C dilinde _T *_ türü olarak kabul edilir.

Örneğin _int_ türden bir nesnenin adresi anlamına gelene bir ifadenin türü _int *_,  
_double_ türden bir nesnenin adresi anlamına gelene bir ifadenin türü _double *_ türüdür.
Her nesne türü için ona karşılık gelen bir adres türü vardır.

#### gösterici değişkenler
Bir gösterici değişken belirli türden bir nesnenin adresini tutabilen bir değişkendir. 
Bir gösterici değişken aşağıdaki gibi tanımlanabilir:

```
int *ptr;
```

Böyle bir değişken için aşağıdaki nitelemeleri kullanabiliriz:

* _ptr_ değişkeninin türü _int *_ türüdür. Bu tür İngilizce'de şöyle ifade edilir: _"pointer to int"_
* _ptr_ değişkeninin değeri _int_ türden bir nesnenin adresi olacaktır.

Bildirimde kullanılan \* atomu yalnızca önüne geldiği ismi nitelemektedir. 
Aşağıdaki bildirime bakalım:

```
int * p1, p2;
```

Geçerli bu bildirimde yalnızca _p1_ bir gösterici değişkendir. 
_p1_ değişkeninin türü _int *_ _p2_ değişkenin türü ise _int_'tir.
Yani yukarıdaki bildirim aşağıdaki bildirimlere eşdeğerdir:

```
int * p1;
int p2;
```

Böyle bildirimde kullanılan \* ve \[] atomları listedeki isimlerin tamamını değil yalnızca önlerine ya da arkalarına geldiği isimleri nitelemektedir. 
Aşağıdaki bildirimlere bakalım:

```
int x, *p, a[10], *b[20];
```

Yukarıdaki bildirimde 

- _x_, _int_ türden bir değişken
- _p_, int * türden değişken
- _a_, elemanları _int_ türden olan _10_ elemanlı bir dizi
- _b_, elemanları _int *_ türden olan _20_ elemanlı bir dizi

Bir gösterici değişken global ya da yerel olabilir. Yerel bir gösterici değişken otomatik ömürlü ya da statik ömürlü olabilir. İlk değer verilmeyen otomatik ömürlü bir gösterici değişken hayata çöp değerle başlar.

#### gösterici değişkenlere ilk değer verilmesi

Gösterici değişkenlere de diğer tüm değişkenlerde olduğu gibi ilk değer verilebilir, atama yapılabilir. Göstericilere ilk değer veren ya da atanan ifadelerin uygun türden adres olması gerekir.

```
int main()
{
    int x = 10;
    int y = 20;
    int *ptr = &x;
    //...
    ptr = &y;
    //...
}
```

Yukarıdaki kodda _ptr_ isimli değişken tanımlanıyor ve bu değişkene _&x_ ifadesi ile ilk değer veriliyor. 
Daha sonra _ptr_ değişkenine _&y_ ifadesi atanıyor.

#### gösterici değişkenlerin bellekte kapladığı alan (storage) 
Nesne adresleri için bellekte ayrılacak yer türden bağımsız olarak aynı büyüklüktedir. 
Bir başka deyişle tüm nesne adresi türlerine ilişkin _sizeof_ değeri aynıdır. _(2, 4 ya da 8 byte gibi)_. 
Aşağıdaki programı derleyip çalıştırın:

```
int main()
{
	printf("sizeof (char)       = %zu\n", sizeof(char ));
	printf("sizeof (char *)     = %zu\n", sizeof(char *));
	printf("sizeof (short)      = %zu\n", sizeof(short));
	printf("sizeof (short *)    = %zu\n", sizeof(short *));
	printf("sizeof (int)        = %zu\n", sizeof(int));
	printf("sizeof (int *)      = %zu\n", sizeof(int *));
	printf("sizeof (double)     = %zu\n", sizeof(double));
	printf("sizeof (double*)    = %zu\n", sizeof(double *));
}
```

#### Gösterici Operatörleri (Pointer Operators)

_C_ dilinin bazı operatörleri adres verileri ile ilgili olarak kullanılır. 
Gösterici operatörleri şunlardır:

* içerik operatörü __(\*)__  _(indirection operator - dereferencing operator)_
* adres operatörü __(&)__ _(address of operator)_
* indeks operatörü __(\[ ])__ _(index operator - subscript operator)_
* ok operatörü __(->)__ _(member selection operator - arrow operator)_

ok operatörü yapı türünden adreslerle kullanıldığı için bu operatör* "yapılar" konusunda ayrıntılı olarak inceleyeceğiz.

#### Adres operatörü
Adres operatörü _(adress of operator)_, ön ek konumunda tek terimli _(unary prefix)_ bir operatördür.
Oluşturmuş olduğumuz operatör öncelik tablosunun ikinci seviyesinde yer alır. 
Bu operatörün  ürettiği değer terimi olan nesnenin adresidir. 
Adres operatörünün terimi mutlaka bir nesne _(L value expression)_ olmalıdır. 
Çünkü yalnızca nesnelerin -sol taraf değerlerinin- adreslerinden söz edilebilir. 
Bu operatörün teriminin nesne olmayan bir ifade yani bir "sağ taraf değeri" olması geçersizdir.

```
int ival = 0;
```
gibi bir tanımlamadan sonra yazılan

```
&k
```

ifadesini ele alalım. 
Bu ifadenin ürettiği değer _int_ türden bir adres bilgisidir.
Bu ifadenin türü _(int \*)_ türüdür.

_&_ operatörü diğer tek terimli _(unary)_ operatörler gibi oluşturduğumuz operatör öncelik tablosunun _2._ seviyesinde bulunur. 
Bu öncelik seviyesinin öncelik yönünün "sağdan sola" _(right associative)_ olduğunu hatırlayın.
Bir gösterici değişken bir adres bilgisi tutan bir nesne olduğuna göre, bir gösterici değişkene adres operatörünün ürettiği bir adres verisi atanabilir.

```
int ival = 20;
int *ptr;
ptr = &ival;
```

Böyle bir atamadan sonra şunlar söylenebilir:
_ptr_ nesnesinin değeri _x_ değişkeninin adresidir. 
_ptr_ nesnesi _x_ değişkeninin adresini tutmaktadır.
_ptr_ değişkeni x'i göstermektedir. _(ptr points to x)_

Adres operatörü ile elde edilen adres aynı türden bir gösterici değişkene atanmalıdır. 
Örneğin aşağıdaki programda bir gösterici değişkene farklı türden bir adres atanıyor:

```
char ch = 'x';
int *p;
p = &ch; /* Yanlış */
```

C dilinde farklı türden adreslerin birbirine atanması sentaks açısından geçerli olsa da doğru değildir.

Adres operatörü ile oluşturulan ifade bir _"sol taraf değeri"_ değildir. 
Sol taraf değeri beklenen bir yerde böyle bir ifade kullanılamaz. 
Örneğin:

```
int x;
++&x     /* Geçersiz */
```

gibi bir kod geçersizdir. 
Arttırma _(increment)_ operatörünün terimi nesne gösteren bir ifade olmalıdır. 
Yukarıdaki ifadede _++_ operatörünün terimi olan _&x_ ifadesi bir nesne değildir. 
Yalnızca bir adres değeridir.

#### adres değerlerinin standart çıkış akımına yazdırılması

Standart _printf_ işlevi ile aritmetik türden ifadelerin değerlerinin ekrana yazdırılabileceğini biliyorsunuz. 
Bir ifadenin değerini ekrana yazdırmak için, 
_printf_ işleviyle birinci argüman olarak geçilen string içinde önceden belirlenmiş format karakterlerinin _(conversion specifiers)_ kullanıldığını hatırlayın. 
Acaba bir adres verisi de uygun format karakteri kullanılarak ekrana yazdırılabilir mi? Evet! 
Standart _printf_ işlevinde bu amaç için _%p_ format karakterleri kullanılır. 
%p dönüştürücüsü ile eşlenen argüman bir adres bilgisi ise, _printf_ işlevi ilgili adres bilgisinin yalnızca sayısal bileşenini 
onaltılık sayı sisteminde standart çıkış akımına yazdırır.
Aşağıdaki programı derleyerek çalıştırın:

```
#include <stdio.h>

int main()
{
    int *ptr;
    int x = 20;
    ptr = &x;
    printf("x nesnesinin adresi = %p\n", &x);
    printf("ptr değişkeninin değeri = %p\n", ptr);
    printf("ptr nesnesinin adresi = %p\n", &ptr);
}
```

_ptr_ bir nesne olduğu için adres operatörünün terimi olabilir, değil mi? 
_ptr_ nesnesinin değeri olan adres, _x_ nesnesinin adresidir. 
Ama _ptr_ nesnesinin kendi adresinden de söz edilebilir. 
Bir gösterici değişkenin değeri olan adres ile gösterici değişkenin kendi adresi farklı değerlerdir.

```
printf("ptr nesnesinin adresi = %p\n", &ptr);
```

çağrısıyla _ptr_ değişkeninin kendi adresi ekrana yazdırılıyor.

#### dizi isimlerinin adrese dönüştürülmesi (array decay)
C dilinde dizi isimleri bir ifade içinde kullanıldığında derleyici tarafından örtülü olarak _(implicitly)_ bir adres bilgisine dönüştürülür.

```
char str[20];
```
gibi bir dizi tanımlamasında sonra, dizinin ismi olan _str_ bir işleme sokulduğunda bu dizinin ilk elemanının adresine dönüştürülür.

Bu dönüşüm neticesinde _str_ ifadesi ile _&str[0]_ ifadesi aynı adres olarak kullanılabilir.

Bu durumda bir dizinin adresi bir gösterici değişkene bir dizinin adresi ile ilk değer verilebilir ya da gösterici değişkenlere    

#### içerik operatörü

İçerik operatörü _(indirection operator / dereferencing operator)_ ön ek konumunda kullanılan tek terimli _(unary prefix)_ bir operatördür. 
İçerik operatörünün terimi bir adres olmalıdır. Operatörün teriminin bir adres olmaması sentaks hatasıdır.İçerik operatörü terimi olan adresteki nesneye erişimi sağlar:
```
*adres
```
gibi bir ifade o adresteki nesne anlamına gelir ve böyle bir ifade bir sol taraf değeridir.
Aşağıdaki C kodunda yapılan atamalara bakalım:

```
#include <stdio.h>

int main()
{
	int ival = 10;
	int *ptr = &ival;
	int a[5] = { 1, 2, 3, 4, 5 };

	*ptr = 24;
	printf("ival = %d\n", ival);
	*a = 45;
	printf("a[0] = %d\n", a[0]);
	*&ival = 75;
	printf("ival = %d\n", ival);
}
```

_*ptr_ ifadesi ele alınırken _ptr_'nin değeri _ival_'in adresidir. 
Yani _ptr_ _ival_'i göstermektedir. Bu durumda _*ptr_ ifadesi _ival_ değişkenin kendisi anlamına gelir.
```
*ptr = 25;
```

deyimi ile atama _ival_ değişkenine yapılmıştır.

_*a_ ifadesinde dizi ismi olan _a_ dizinin ilk elemanının adresine dönüştürülür. _(array decay)_ Bu durumda içerik operatörü ile a dizisinin ilk elemanına erişilir. 
```
*a = 45;
```

deyimi ile atama _a_ dizisinin ilk elemanına yapılmaktdır

_a? bir dizinin ismi olmak üzere _C_ dilinde _a[0]_ ifadesini yazmak ile _*a_ ifadesini yazmak aynı anlama gelir.

