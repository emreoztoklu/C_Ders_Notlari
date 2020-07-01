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
