### bu ders notu henüz tamamlanmadı

#### Gösterici Operatörleri (Pointer Operators)

C dilinin bazı operatörleri adres verileri ile ilgili olarak kullanılır. Gösterici operatörleri
Gösterici operatörleri 

\* içerik işleci  _(indirection operator - dereferencing operator)_
& adres işleci _(address of operator)_
[ ] indeks operatörü _(index operator - subscript operator)_
-> ok operatörü _(member selection operator - arrow operator)_

ok operatörü yapı türünden adreslerle kullanıldığı için bu operatör "yapılar" konusunda ayrıntılı olarak incelenecek.

Adres operatörü
Adres operatörü (adress of operator), ön ek konumunda tek terimli (unary prefix) bir operatördür.
Operatör öncelik tablosunun ikinci seviyesinde yer alır. Bu işlecin ürettiği değer, terimi olan nesnenin adresidir. 
Adres operatörünün operandı mutlaka bir nesne olmalıdır. 
Çünkü yalnızca nesnelerin -sol taraf değerlerinin- adreslerinden söz edilebilir. 
Adres operatörünün teriminin nesne olmayan bir ifade olması geçersizdir.
