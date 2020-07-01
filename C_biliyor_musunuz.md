C dilinin sözdizimine ne kadar hakimsiniz? 
Aşağıdaki kurallardan kaç tanesini biliyorsunuz?

1. Bir bildirimde _const_ ya da _volatile_ belirteçlerini birden fazla kez kullanmak geçerlidir:

```
const const const volatile volatile int ival = 10;
```

2. Dosya bilinirlik alanında _(file scope)_ bir değişken ilk değer verilmemek kaydıyla birden fazla kez bildirilebilir _(tentative definition)_ . 
Aşağıdaki bildirimler geçerlidir.

```
int x, x, x, x;
int x;
int x;
```

3. Parametre değişkeni gösterici _(pointer)_ olan işlev bildirimlerinin aşağıdaki biçimlerde yapılması derleyici açısından bir farklılık oluşturmaz:

```
void f1(int *p);
void f2(int p[]);
```

Ancak tamamlanmamış türler _(incomplete types)_ için birinci biçim geçerli iken ikinci biçim geçerli değildir:

```
struct Neco;
 
void f1(struct Neco *p); //gecerli
void f2(struct Neco p[]); //gecersiz
```

4. _C99_ standartları öncesinde bir işlevin gösterici  parametre değişkeninin kendisinin _const_ olması durumunda _(const pointer)_ bu bildirimi [ ] bildirgeci _(declarator)_ ile yapma olanağı yoktu. 
Bu olanak _C99_ standartları ile sağlandı. 
Aşağıdaki bildirimler birbirine eşdeğer:

```
void f(int * const ptr);
void f(int p [const]);
```

5. Aşağıdaki bildirimler birbirine eşdeğerdir:

```
void f(int p[]);
void f(int p[20]);
void f(int p[static 20]);
```

Ancak _static_ anahtar sözcüğünün kullanıldığı bildirim derleyiciye verilen bir ipucu niteliğindedir. 
Bu durumda derleyici bu fonksiyona adresi gönderilecek dizinin en az _20_ boyutunda olacağı yönünde bir bilgi edinir ve bu bilgiyi kullanarak daha iyi bir optimizasyon gerçekleştirebilir.

6. _for_ döngü deyiminin birinci kısmında bildirilen isimler ile döngünün ana bloğu içinde bildirilen isimler ayrı kapsama _(scope)_ sahiptir. 
_C++_ dilinde geçerli olmayan aşağıdaki C kodu geçerlidir:

```
#include <stdio.h>
 
int main()
{
	for (int i = 0; i < 5; ++i) {
		int i = 1;
		printf("%d ", i); //1 1 1 1 1
	}	
}
```

7. Fonksiyon bildirimlerinde ve tanımlarında fonksiyonun geri dönüş değeri türünün yazıldığı yerde bir tür bildirimi yapılabilir. 
Aşağıdaki kod geçerlidir:

```
#include <stdio.h>
 
struct A { int x; } func()
{
	struct A ax = { 34 };
 
	return ax;
}
 
int main()
{
	struct A ay = func();
	
	printf("%d\n", ay.x);
}
```

8. Fonksiyon bildirimlerinde ya da tanımlarında parametre parantezi içinde bir tür tanımlaması yapılabilir. 
Ancak tanımlanan bu türün kapsamıı dosya kapsamı değildir, işlevin ana bloğu ile sınırlıdır:

```
#include <stdio.h>
 
void func(enum { Red, Green, Yellow } x)
{
	int y = Red;
	printf("%d %d\n", x, y);
}
 
 
int main()
{
	func(1);
	//func(Red); //Gecersiz
}
```

9. _sizeof_ operatörünün içinde ve tür dönüştürme operatörünün parantezi içinde bir tür bildirilebilir:

```
#include <stdio.h>
 
int main()
{
	size_t n = sizeof(struct A { int x; }) + sizeof(enum { A, B, C });
	struct A ax = { 10 };
 
	printf("%zu %d %d\n", n, B, ax.x);
}
```

10. _sizeof_ operatörünün terimi olan ifade (bir tür ismi değil ise) parantez içine alınmak zorunda değildir. 
_sizeof_ operatörünün terimi olan ifade için derleyici işlem kodu üretmez. 
Ancak _sizeof_ operatörünün terimi değişken boyutta bir dizi _(variable length array)_ ise bu ifade için işlem kodu üretilir.

```
#include <stdio.h>
 
int main()
{
	int x = 10;
	int y = 20;
 
	size_t n = sizeof x++ + sizeof (int [y++]);
	printf("x = %d\n", x); //10
	printf("y = %d\n", y); //21
}
```
