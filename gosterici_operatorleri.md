### bu ders notu henüz tamamlanmadı

#### Gösterici Operatörleri (Pointer Operators)

C dilinin bazı operatörleri adres verileri ile ilgili olarak kullanılır. Gösterici operatörleri şunlardır:

* içerik işleci __(\*)__  _(indirection operator - dereferencing operator)_
* adres işleci __(&)__ _(address of operator)_
* indeks operatörü __(\[ ])__ _(index operator - subscript operator)_
* ok operatörü __(->)__ _(member selection operator - arrow operator)_

ok operatörü yapı türünden adreslerle kullanıldığı için bu operatör "yapılar" konusunda ayrıntılı olarak incelenecek.

#### Adres operatörü
Adres operatörü _(adress of operator)_, ön ek konumunda tek terimli _(unary prefix)_ bir operatördür.
Oluşturmuş olduğunuz operatör öncelik tablosunun ikinci seviyesinde yer alır. Bu operatörün  ürettiği değer, terimi olan nesnenin adresidir. 
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

& operatörü diğer tek terimli _(unary)_ operatörler gibi, işleç öncelik tablomuzun 2. seviyesinde bulunur. 
Bu öncelik seviyesinin öncelik yönünün "sağdan sola" _(right associative)_ olduğunu biliyorsunuz.
Bir gösterici değişken bir adres bilgisi tutan bir nesne olduğuna göre, bir gösterici değişkene adres operatörünün ürettiği bir adres verisi atanabilir.
```
int ival = 20;
int *ptr;
ptr = &ival;
```

Böyle bir atamadan sonra şunlar söylenebilir:
_ptr_ nesnesinin değeri _x_ değişkeninin adresidir. 
_ptr_ nesnesi _x_ değişkeninin adresini tutmaktadır.
Adres operatörü ile elde edilen adres, aynı türden bir gösterici değişkene atanmalıdır. 
Örneğin aşağıdaki programda bir gösterici değişkene farklı türden bir adres atanıyor:

```
char ch = 'x';
int *p;
p = &ch; /* Yanlış */
```
Tabi bu operatör ile oluşturulan ifade bir _"sol taraf değeri"_ değildir. Örneğin:

```
int x;
++&x /* Geçersiz */
```

gibi bir kod geçersizdir. 
Arttırma _(increment)_ operatörünün terimi nesne gösteren bir ifade olmalıdır. 
Yukarıdaki ifadede _++_ operatörünün terimi olan &x ifadesi bir nesne değildir. Yalnızca bir adres değeridir.
