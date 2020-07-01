# Fonksiyon Göstericileri (Funcion Pointers)
Nesnelerin nasıl adresleri varsa fonksiyonların da adresleri vardır. 
Bir fonksiyonun adresi, o fonksiyonun makine kodlarının yerleştiği bellek bloğunun adresidir. 
`C` dilinde bir fonksiyonun adresi, fonksiyon göstericisi `(function pointer)` denen özel bir gösterici değişkende saklanabilir. 
Bir fonksiyon adresi başka bir fonksiyona argüman olarak gönderilebilir. 
Bir fonksiyonun geri dönüş değeri bir fonksiyon adresi olabilir. 
Öğeleri fonksiyon adresleri olan diziler oluşturulabilir. 
Fonksiyon adresleri `C` ve `C++` dillerinde en sık kullanılan öğeler arasındadır.

## Fonksiyon adreslerinin elde edilmesi
Bir fonksiyonun adresi, fonksiyon isminin adres operatörünün operandı yapılmasıyla elde edilir. Yani örneğin

```
int func(int x, int y);
```
biçiminde bildirilen `func` fonksiyonunun adresi

```
&func
```
ifadesiyle kullanılabilir.

## Fonksiyon adreslerinin türleri
`C` dilinde her ifadenin `(expression)` bir türü vardır. 
Bir fonksiyonun adresinin türü, söz konusu fonksiyonun geri dönüş değerinin ve parametre değişkenlerinin türleriyle ilişkilendirilmiş bir adres türüdür. 
Örneğin yukarıda bildirilen `func` fonksiyonunun adresi aşağıdaki türdendir:

```
int (*)(int, int)
```
Soldaki parantezin içindeki asterisk `(*)` atomu türün bir adrese ilişkin olduğunu anlatıyor. 
Bu parantezin soluna yazılan `int` söz konusu adrese ilişkin fonksiyonun geri dönüş değerinin `int` türden olduğuna işaret ediyor. 
Sağdaki parantez içinde virgüllerle ayrılmış tür listesi ise söz konusu fonksiyonun parametre değişkenlerinin türlerini gösteriyor.
Birkaç örnek daha verelim. 
Örneğin standart `strcmp `fonksiyonunun adresi

```
int (*)(const char *, const char *)
```

türündendir. Standart `strcpy` fonksiyonunun adresi
```
char *(*)(char *, const char *)
```
türündendir.

## Fonksiyon isimlerinin fonksiyon adreslerine dönüştürülmesi
Bir dizi isminin bir ifade içinde kullanıldığında, derleyici tarafından dizinin ilk elemanının adresine dönüştürüldüğünü `(array to pointer conversion)` biliyorsunuz. 
Benzer şekilde bir fonksiyon ismi de bir ifade içinde kullanıldığında, derleyici tarafından ilgili fonksiyonun adresine dönüştürülür `(function to pointer conversion)`. 
Bir ifade içinde yer alan fonksiyon isimlerine, yazılımsal olarak fonksiyonların adresleri gözüyle bakılabilir.

```
&func
```


ile

```
func
```
eşdeğer ifadeler olarak düşünülebilir. 
Her iki ifade de `func` fonksiyonunun adresi olarak kullanılabilir.

## Fonksiyon göstericisi değişkenler
Bir fonksiyon göstericisi değişken değeri bir fonksiyonun adresi olan değişkendir. 
Bu tür değişkenlerin amacı fonksiyonların adreslerini tutmaktır. 
Yukarıda bildirilen `func` fonksiyonunun adresini tutabilecek `fptr` isimli bir gösterici değişkeni aşağıdaki gibi tanımlayabiliriz:

```
int func(int, int);
int (*fptr)(int, int);
```
`fptr` değişkenine `func` fonksiyonunun adresi ilk değer olarak verilebilir ya da atanabilir

```
fptr = &func;
```
Fonksiyon isimlerinin derleyici tarafından fonksiyonların adreslerine dönüştürülmesinden faydalanarak bu atama deyimi aşağıdaki gibi de yazılabilirdi:

```
fptr = func;
```
Şimdi de aşağıdaki koda bakalım:

```
#include <string.h>

int main()
{
	int(*fp)(const char *, const char *) = &strcmp;
	//
	fp = &strcoll;
	//
}
```
Yukarıdaki `main` fonksiyonunda tanımlanan `fp` isimli fonksiyon göstericisi değişkene standart `strcmp` fonksiyonunun adresiyle ilk değer veriliyor. 
Daha sonra yer alan deyimle `fp` değişkenine bu kez aynı parametrik yapıda olan standart `strcoll` fonksiyonunun adresi atanıyor.

## Fonksiyon göstericileri ve gösterici aritmetiği
Bir fonksiyon göstericisi gösterici aritmetiği ile kullanılamaz yani fonksiyon adresleri diğer adresler gibi tam sayılar ile toplanamaz. 
Fonksiyon göstericisi değişkenler, `++` ya da `--` operatörlerinin operandı olamazlar. 
Zaten gösterici aritmetiğinin fonksiyon adresleri için bir anlamı da olamazdı. 
Bir fonksiyon adresine bir tam sayı toplayıp bellekte bir sonraki fonksiyonun adresine erişmek nasıl mümkün olurdu? 
Fonksiyon adresleri içerik operatörünün `(dereferencing)` terimi olabilir. 
Böyle ifadeler ile yalnızca fonksiyon çağrısı yapılabilir.

## Fonksiyon göstericileri ve karşılaştırma işlemleri
Fonksiyon göstericileri karşılaştırma operatörlerinin de operandı olabilirler. 
Örneğin, iki fonksiyon göstericisinin değerlerinin aynı adres olup olmadığı yani aynı fonksiyonu gösterip göstermedikleri  `==` ya da `!=` operatörleri ile karşılaştırılabilir:

```
#include <stdio.h>

double f1(double) { return 1.; }
double f2(double) { return 2.; }

int main()
{
	double(*fptr1)(double) = &f1;
	double(*fptr2)(double) = &f2;

	if (fptr1 == fptr2)
		printf("esit\n");
	else
		printf("esit degil\n");

	fptr1 = &f2;

	if (fptr1 == fptr2)
		printf("esit\n");
	else
		printf("esit degil\n");
}
```
Hiçbir fonksiyonu göstermeyen bir fonksiyon göstericisi değişken, değeri `NULL` gösterici adresi olan değişkendir. 
Fonksiyon göstericisi değişkenler için de `NULL` gösterici semantiğinden faydalanılabilir:

```
#include <stddef.h>

int main()
{
	void(*fp)(void) = NULL;

	//
	if (fp) 
		fp();
	//
}
```
Yukarıdaki `main` fonksiyonu içinde `fp` isimli fonksiyon göstericisine `NULL` adresi ile ilk değer veriliyor. 
Daha aşağıdaki `if` deyiminde ise `fp` fonksiyon göstericisinin değerinin `NULL` gösterici olup olmadığına yani `fp`'nin bir fonksiyonu gösterip göstermediğine bakılıyor.

## Fonksiyon çağrı operatörü (function call operator)
Fonksiyon çağrı operatörü tek terimli son ek konumunda bir operatördür. 
Operatör öncelik tablomuzun en yüksek seviyesindedir `(primary expression)`. 
Bu operatörün operandı bir fonksiyon adresidir. 
Operatör programın akışını o adrese yöneltir, yani o adresteki kodun çalıştırılmasını sağlar. Örneğin:

```
func(10, 20)
```
Burada fonksiyon çağrı operatörünün operandı `func` adresidir. 
Operatör, `func` adresinde bulunan kodu, yani `func` fonksiyonunun kodunu çalıştırır. 
Fonksiyon çağrı operatörünün ürettiği değer, çağrılan fonksiyonun geri dönüş değeridir. Örneğin:

```
int result = strcmp(s1, s2);
```
Burada `strcmp` ifadesinin türü, geri dönüş değeri `int` parametreleri `(const char *, const char *)` olan bir fonksiyon adresidir. 
Yani `strcmp` ifadesi

```
int (*)(const char *, const char *)
```
türüne dönüştürülür.

```
strcmp(s1, s2)
```
gibi bir fonksiyon çağrısının oluşturduğu ifade ise `int` türdendir.

## Fonksiyonların fonksiyon göstericileri ile çağrılması
Bir fonksiyon göstericisinin gösterdiği fonksiyon iki ayrı biçimde çağrılabilir. 
`pf` bir fonksiyon gösterici değişkeni olmak üzere, bu değişkenin gösterdiği fonksiyon

```
pf()
```
biçiminde, ya da

```
(*pf)()
```
biçiminde çağrılabilir. 
Birinci biçim daha doğal görünmekle birlikte, `pf` isminin bir gösterici değişkenin ismi mi yoksa bir fonksiyonun ismi mi olduğu çok açık değildir. 
İkinci biçimde, `*pf` ifadesinin öncelik parantezi içine alınması zorunludur. 
Çünkü fonksiyon çağrı operatörünün  öncelik seviyesi, içerik `(dereferencing)` operatörünün öncelik seviyesinden daha yüksektir. 
Bu çağrı biçimi kullanılan ismin bir fonksiyon göstericisine ilişkin olduğu vurgusunu yaptığından tercih edilebilmektedir. 
Aşağıdaki programı derleyerek çalıştırın:

```
#include <stdio.h>

void func()
{
	printf("func()\n");
}

int main()
{
	void (*pf)(void) = func;
	
	pf();
	(*pf)();
 	
	return 0;
}
```
`main` fonksiyonu içinde tanımlanan `pf` isimli gösterici değişkene `func` fonksiyonunun adresi ile ilk değer veriliyor. 
Daha sonra `pf` fonksiyon göstericisinin gösterdiği fonksiyonun iki ayrı biçimde çağrıldığını görüyorsunuz.

## Fonksiyon göstericileri ve `typedef` bildirimleri
Fonksiyon göstericilerine ilişkin kodların kolay yazılması ve okunması amacıyla çoğunlukla fonksiyon göstericisi türlerine `typedef` bildirimleriyle eş isimler verilir. 
Aşağıdaki bildirime bakalım:

```
int(*func(int(*fp)(int, int)))(int, int);
```
Bu bildirimi okumak bir hayli zor değil mi? Bir de bildirimi yapana sorun:

`func`, geri dönüş değeri, geri dönüş değeri `int` türden ve iki `int` parametreli bir fonksiyonun adresi, türünden olan ve parametresi yine geri dönüş değeri `int` türden ve iki `int` parametreli bir fonksiyonun adresini isteyen gösterici olan bir fonksiyon. 
Yani `func` fonksiyonunun hem geri dönüş değeri hem de parametresi

```
int (*)(int, int)
```
türünden. Oysa bu türe bir `typedef` bildirimiyle eş isim verilseydi bu türe bağlı karmaşık bildirimleri okumak ya da yazmak çok daha kolay olacaktı:

```
typedef int(*Fptr)(int, int);

Fptr func(Fptr fp);

int main()
{
	Fptr fp1, fp2;
	Fptr ar_fp[10];
	Fptr *fpptr = &fp1;
	//
}
```
Yukarıdaki kodda global isim alanında, geri dönüş değeri `int` türden olan ve `int` türden iki parametre değişkeni olan fonksiyonların adreslerinin türüne `Fptr` eş ismi veriliyor. 
Artık bu ismin kapsamı `(scope)` içinde bu isim bu türün karşılığı eş isim olarak kullanılabilir. 
Diğer bildirimlere sırasıyla bakalım:

```
Fptr func(Fptr fp);
```
Burada bildirilen `func` fonksiyonunun hem parametre değişkeni hem de geri dönüş değeri fonksiyon adresleri. 
Eğer `typedef` bildirimi olmasaydı bu bildirim

```
int(*func(int(*fp)(int, int)))(int, int);
```
biçiminde yapılacaktı.

```
Fptr fp1, fp2;
```
Burada hem `fp1` hem de `fp2` fonksiyon göstericisi değişkenler.

```
Fptr ar_fp[10];
```
Burada `ar_fp` öğeleri `fp1`, `fp2` gibi olan, yani öğeleri fonksiyon göstericileri olan, `10` elemanlı bir dizidir.

```
Fptr *fpptr = &fp1;
```
Burada `fpptr`, `fp1` ve `fp2` gibi fonksiyon göstericisi değişkenlerin adresini tutacak bir gösterici değişkendir `(pointer to function pointer)`.

## Fonksiyon adresi döndüren işlevler
İşlevlerin geri dönüş değerleri fonksiyon adresleri olabilir. 
Bu durumda böyle bir fonksiyona çağrı yapacak bir kod işlevden geri dönüş değeri yoluyla bir fonksiyonun adresini alabilir:

## İşlevlere fonksiyon göndermek
Bir fonksiyon, işinin bir kısmını gerçekleştirmesi için kendisini çağıran koddan adresini aldığı bir fonksiyonu çağırabilir. 
Fonksiyonu çağıran kod, çağırdığı fonksiyonun işinin belirli bir kısmının kendi belirlediği gibi yapılmasını sağlayabilir. 
Böylece bir fonksiyonun davranışının bir kısmı onu çağıran kodun belirleyeceği şekilde değiştirilebilir ya da özelleştirilebilir. 
Fonksiyon göstericileri ile çağrılan işlevler genelleştirilmiş işlevlerdir. 
Müşteri kodlar böyle işlevlere diledikleri fonksiyonun adresini geçerek onları daha özel hale getirmiş olurlar. 
Bu mekanizmaya popüler olarak geri çağrı" `(call back)` denilmektedir. 
`C` dilinde geri çağrı mekanizması bir fonksiyonun çağıracağı fonksiyona bir fonksiyonun adresini göndermesi şeklinde gerçekleşir. 
Bu durumda çağrılan fonksiyonun bir parametre değişkeni bir fonksiyon göstericisi olur. 
Fonksiyon göstericilerinin en sık kullanıldığı tema budur:

```
#include <stdio.h>

void func(void(*fp)(void))
{
	printf("func cagrildi\n");
	fp();
	//
}

void f1()
{
	printf("f1 cagrildi\n");
	//
}

int main()
{
	func(f1);
}
```
Yukarıdaki kodda `func` fonksiyonunun parametre değişkeni bir fonksiyon göstericisi. `main` fonksiyon içinde `func` fonksiyonu `f1` fonksiyonunun adresi ile çağrılıyor. 
`func` fonksiyonu de adresini aldığı fonksiyonu çağırıyor.

## Türden bağımsız işlem yapan işlevler
Bazı işlevler belirli bir tür için yazılır ve dolayısıyla yalnızca belirli bir türe hizmet verirler. 
Örneğin öğeleri `int` türden olan bir diziyi sıralayan bir fonksiyon şöyle bildirilebilir:

```
void sort_int_array(const int *ptr, size_t size);
```
Böyle bir fonksiyon öğeleri double türden olan bir diziyi sıralayamaz. 
Bu tür durumlarda aynı kodu, farklı türlere göre yeniden yazmak gerekir. 
Ancak aynı işi her tür için yapabilecek, türden bağımsız olarak tek bir fonksiyonun yazılabilmesi mümkün olabilir. 
Türden bağımsız işlem yapan fonksiyonların gösterici parametreleri `void *` türünden olmalıdır. 
Ancak parametrelerin void * türünden olması yalnızca çağrı açısından kolaylık sağlar. 
Fonksiyonu yazacak olan programcı, yine de fonksiyona geçirilen adresin türünü saptamak zorundadır. 
Bunun için fonksiyona tür bilgisine karşılık gelen bir numaralandırma değeri gönderilebilir. 
Örneğin herhangi bir türden diziyi sıralayacak fonksiyonun bildirimi aşağıdaki gibi olsun:

```
void *gSort(void *parray, size_t size, int type);
```
Şimdi fonksiyonu yazacak programcı type isimli parametre değişkenini kullanarak bir switch deyimiyle dışarıdan adresi alınan dizinin türünü saptayabilir. 
Ancak bu yöntem C'nin doğal türleri için çalışsa da programcı tarafından oluşturulan türlerden `(user defined types)` diziler için doğru çalışmaz.
Böyle genel işlevler ancak fonksiyon göstericileri kullanılarak yazılabilir. 
Şimdi bir dizinin en büyük elemanın adresiyle geri dönen bir fonksiyonu türden bağımsız olarak yazmaya çalışalım. 
Dizi türünden bağımsız olarak işlem yapan fonksiyonların parametre değişkenleri tipik olarak aşağıdaki gibi olur:

```
void *g_max(const void *pa, size_t size, size_t width, int (*cmp)(const void *, const void *));
```
+ Dizinin başlangıç adresini alacak `void *` türden bir gösterici
+ Dizinin öğe sayısını alacak `size_t` türünden bir parametre değişkeni.
+ Dizinin bir öğesinin bellekte kaç byte yer kapladığı değerini yani `sizeof` değerini alan `size_t` türünden bir parametre değişkeni.
+ Dizinin elemanlarını karşılaştırma amacıyla kullanılacak bir fonksiyonun başlangıç adresini alan, fonksiyon göstericisi parametre değişkeni.

Bu tür fonksiyonların tasarımındaki genel yaklaşım şudur: Fonksiyon her bir dizi öğesinin adresini gösterici aritmetiğinden faydalanarak bulabilir. 
Ancak dizi elemanlarının türü bilinmediğinden fonksiyon dizinin elemanlarının değerlerini karşılaştırma işlemini yapamaz. 
Bu karşılaştırmayı fonksiyon göstericisi kullanarak fonksiyonu çağıran kodun gönderdiği fonksiyona yaptırır. 
Fonksiyonu çağıracak programcı, karşılaştırma fonksiyonunun dizinin herhangi iki öğesinin adresiyle çağrılacağını göz önüne alarak karşılaştırma işlevini şöyle yazar:
Karşılaştırma fonksiyonu karşılaştırılacak iki nesnenin adresini alır. 
Fonksiyonun birinci parametresine adresi alınan nesne, ikinci parametreye adresi alınan nesneden daha büyükse fonksiyon pozitif herhangi bir değere, küçükse negatif herhangi bir değere, bu iki değer eşitse sıfır değerine geri döner. 
Bu, standart `strcmp` fonksiyonunun sunduğu uzlaşımdır `(convention)`. 
Aşağıdaki programı inceleyin:

```
#include <stdio.h>
#include <string.h>

#define   MAX_NAME_LEN  20
#define   asize(a)      (sizeof(a) / sizeof(*a))

typedef struct {
	char name[MAX_NAME_LEN];
	int no;
}Person;

typedef unsigned char Byte;
typedef int(*Cmpfn)(const void *, const void *);

void *g_max(const void *pa, size_t size, size_t width, Cmpfn fp)
{
	Byte *pb = (Byte *)pa;
	void *pmax = (void *)pa;
	size_t k;

	for (k = 1; k < size; k++)
		if (fp(pb + k * width, pmax) > 0)
			pmax = pb + k * width;

	return pmax;
}

int cmp_int(const void *vp1, const void *vp2)
{
	if (*(const int *)vp1 > *(const int *)vp2)
              return 1;
        return *(const int *)vp1 < *(const int *)vp2 ? -1 : 0;

}

int cmp_person_by_name(const void *vp1, const void *vp2)
{
	return strcmp(((const Person *)vp1)->name, ((const Person *)vp2)->name);
}

int cmp_person_by_no(const void *vp1, const void *vp2)
{
	return ((const Person *)vp1)->no - ((const Person *)vp2)->no;
}

int main()
{
	int a[] = { 3, 8, 4, 7, 6, 9, 12, 1, 9, 10 };

	Person pa[] = {
		{ "Oguz Karan", 123 },{ "Kaan Aslan", 563 },{ "Ali Serce", 312 },
		{ "Necati Ergin", 197 },{ "Guray Sonmez", 297 },{ "Gurbuz Aslan", 144 } };

	Person *pper;
	int *iptr;

	iptr = (int *)g_max(a, asize(a), sizeof(int), cmp_int);
	printf("%d degerinde olan %d indisli oge max\n", *iptr, iptr - a);

	pper = (Person *)g_max(pa, asize(pa), sizeof(Person), cmp_person_by_name);
	printf("pa dizisinin en buyuk ogesi (isme gore) %s %d\n",
		pper->name, pper->no);
	pper = (Person *)g_max(pa, asize(pa), sizeof(Person), cmp_person_by_no);
	printf("per dizisinin en buyuk ogesi (numaraya gore) %s %d\n",
		pper->name, pper->no);

	return 0;
}
```
Yukarıdaki kodda,

```
typedef int(*Cmpfn)(const void *, const void *);
```
bildirimi ile geri dönüş değeri `int` türden olan const `void *` türden iki parametresi olan fonksiyonların adresi olan türe `Cmpf` eş ismi veriliyor.
Daha sonra bir dizinin en büyük öğesinin adresini döndüren türden bağımsız `g_max` isimli bir fonksiyon tanımlanıyor. 
Fonksiyonun son parametresinin bir fonksiyon göstericisi olduğunu görüyorsunuz. 
`g_max` fonksiyonu, türünü bilmediği bir dizinin başlangıç adresini, boyutunu ve elemanlarının `sizeof` değerlerini kendisini çağıran koddan alıyor. 
Böylece adresini aldığı dizinin türünü bilmese de bu dizinin herhangi bir öğesinin adresini gösterici aritmetiği ile hesaplayabiliyor. 
Dizinin iki öğesinin büyüklük karşılaştırması için son parametresine adresi geçilen fonksiyonun çağrıldığını görüyorsunuz. 
İşleve çağrı yapacak tüm müşteri kodlar fonksiyonun bu parametresine, bir karşılaştırma fonksiyonunun adresini göndermek zorundalar. Kodda tanımlanan

```
int cmp_int(const void *, const void *);
cmp_person_by_no(const void *, const void *);
cmp_person_by_name(const void *, const void *)
```
fonksiyonları dizi elemanlarının karşılaştırılması amacını taşıyorlar.

Yazımızın ikinci bölümünde standart C kütüphanesinin fonksiyon gösterici parametreli bazı işlevlerini inceleyeceğiz.
