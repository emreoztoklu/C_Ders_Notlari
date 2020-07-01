### bu ders notu henüz tamamlanmadı. Her gün biraz ekleme yapıyorum. Allah'ın izniyle yakında bitireceğim

### Göstericiler (Pointers)

C dilinde _pointer_ sözcüğü _"adres"_ anlamında kullanılmaktadır. 
Adres donanımsal bir kavram olmakla birlikte birçok programlama dilinin sentaksında yer almaz. 
C dilinde adresler ve adreslere ytönelik işlemler yoğun olarak kullanılır.

Adresler temel olarak iki kategoriye ayrılmaktadır:
- Nesne adresleri _(object pointers)
- Fonksiyon adresleri (function pointers)

Fonksiyon adreslerini daha sonra ele alacağız.

C dilinde bir ifade (expression) bir nesnenin adresi anlamına gelebilir. Bazı ifadelerin değeri adrestir.
T bir nesnenin türü olmak üzere
T türünden bir nesnenin adresi anlamına gelen ifadenin türü T * türüdür.

Yani _int_ türden bir nesnenin adresi anlamıa gelene bir ifadenin türü _int *_
 doublre  türden bir nesnenin adresi anlamıa gelene bir ifadenin türü _double *_ türüdür

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
C dilinde dizi isimleri bir ifade içinde kullanıldığında derleyici tarafından örtülü olarak _(implicitly)_ bir adres bilgisine dönüştürülür.

```
char str[20];
```
gibi bir dizi tanımlamasında sonra, dizinin ismi olan str bir işleme sokulduğunda bu dizinin ilk elemanının adresine dönüştürülür.

Bu dönüşüm neticesinde _str_ ifadesi ile _&str[0]_ ifadesi aynı adres olarak kullanılabilir.

Bu durumdsa bir dizinin adresi bir gösterici değişkene bir dizinin adresi ile ilk değer verilebilir ya da gösterici değişkenlere    
