### bu ders notu henüz tamamlanmadı

#### Gösterici Operatörleri (Pointer Operators)

C dilinin bazı operatörleri adres verileri ile ilgili olarak kullanılır. Gösterici operatörleri şunlardır:

* içerik operatörü __(\*)__  _(indirection operator - dereferencing operator)_
* adres operatörü __(&)__ _(address of operator)_
* indeks operatörü __(\[ ])__ _(index operator - subscript operator)_
* ok operatörü __(->)__ _(member selection operator - arrow operator)_

ok operatörü yapı türünden adreslerle kullanıldığı için bu operatör "yapılar" konusunda ayrıntılı olarak incelenecek.

#### Adres operatörü
Adres operatörü _(adress of operator)_, ön ek konumunda tek terimli _(unary prefix)_ bir operatördür.
Oluşturmuş olduğunuz operatör öncelik tablosunun ikinci seviyesinde yer alır. 
Bu operatörün  ürettiği değer, terimi olan nesnenin adresidir. 
Adres operatörünün terimi mutlaka bir nesne _(L value expression)_ olmalıdır. 
Çünkü yalnızca nesnelerin -sol taraf değerlerinin- adreslerinden söz edilebilir. 
Adres operatörünün teriminin nesne olmayan bir ifade olması geçersizdir.

```
int ival = 0;
```
gibi bir tanımlamadan sonra yazılan

```
&k
```

ifadesini ele alalım. Bu ifadenin ürettiği değer _int_ türden bir adres bilgisidir.
Bu ifadenin türü _(int \*)_ türüdür.

_&_ operatörü diğer tek terimli _(unary)_ operatörler gibi oluşturduğumuz operatör öncelik tablosunun 2. seviyesinde bulunur. 
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

Adres operatörü ile elde edilen adres, aynı türden bir gösterici değişkene atanmalıdır. 
Örneğin aşağıdaki programda bir gösterici değişkene farklı türden bir adres atanıyor:

```
char ch = 'x';
int *p;
p = &ch; /* Yanlış */
```

C dilinde farklı türden adreslerin birbirine atanması sentaks açısından geçerli olsa da doğru değildir.

Adres operatörü ile oluşturulan ifade bir _"sol taraf değeri"_ değildir. Örneğin:

```
int x;
++&x /* Geçersiz */
```

gibi bir kod geçersizdir. 
Arttırma _(increment)_ operatörünün terimi nesne gösteren bir ifade olmalıdır. 
Yukarıdaki ifadede _++_ operatörünün terimi olan _&x_ ifadesi bir nesne değildir. Yalnızca bir adres değeridir.

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
C dilinde dizi isimleri bir işleme sokulduğunda derleyici tarafından otomatik olarak bir
adres bilgisine dönüştürülür.
char s[5];
gibi bir dizi tanımlamasında sonra, dizinin ismi olan s bir işleme sokulduğunda bu dizinin
ilk elemanının adresine dönüştürülür.
Dizi isimleri derleyici tarafından, diziler için bellekte ayrılan blokların başlangıç yerini gösteren bir adres bilgisine dönüştürülür. 
Yukarıdaki örnekte dizinin bellekte aşağıdaki şekilde yerleştirildiğini düşünün:

``` 
1COOO s[0]
s[4]
s[3]
s[2]
s[1]
1C00
1C04
1C03
1C02
1C01
```

Bu durumda dizi ismi olan _s_, char türden 1C00 adresine eşdeğerdir. 
Yani bu adres bir adres sabiti biçiminde  yazılmış olsaydı:

```
(char *)0x1COO
```

biçiminde yazılırdı.

Bu durumda s ifadesi ile &s[0] ifadesi aynı adres bilgisidir, değil mi?
Gösterici değişkenlere kendi türlerinden bir adres bilgisi atamak gerektiğine göre
aşağıdaki atamaların hepsi geçerli ve doğrudur:
int a[100];
long l[20];
char s[100];
double d[10];
int *p;
long *lp;
char *cp;
double *dp;
p = a;
lp = l;
cp = s;
dp = d;
Bir göstericiye yalnızca aynı türden bir dizinin ismi atanabilir. Örneğin:
int *p;
char s[] = "Necati";
p = s; /YANLIŞ */
Dizi İsimleri Nesne Göstermez
int a[100];
int *ptr;
gibi bir tanımlamadan sonra
a
gibi bir ifade kullanılırsa, bu iafade derleyici tarafından otomatik olarak int * türüne
dönüştürülür. Yani bu ifadenin türü de (int *) türüdür.
ptr
ifadesi nesne gösteren bir ifadeyken, yani bir sol taraf değeriyken,
a
ifadesi nesne göstermeyen bir ifade değeridir. Değiştirilebilir sol taraf değeri (modifiable L
value) olarak kullanılamaz. Örneğin
a++
ifadesi geçersizdir.
C dilinde hiçbir değişkenin ya da dizinin programın çalışma zamanında bulunacağı yer
programcı tarafından belirlenemez. Programcı değişkeni tanımlar, derleyici onu herhangi
bir yere yerleştirebilir.
Dizi isimleri göstericiler gibi sol taraf değeri olarak kullanılamaz. Örneğin, s bir dizi ismi
olmak üzere
C ve Sistem Programcıları Derneği - C Ders Notları - Necati Ergin
238
++s;
deyimi geçersizdir.

