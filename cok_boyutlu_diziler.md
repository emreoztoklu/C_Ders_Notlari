`C` dilinde iki ya da daha çok boyuta sahip diziler tanımlanabilir:

```
double a[5][10];
```
`a` iki boyutlu bir dizi. Aşağıda ismi `ar` olan üç boyutlu bir dizi tanımlanıyor:

```
int ar[4][5][10];
```
Uygulamalarda daha çok kullanılan iki boyutlu dizilerdir. İki boyutlu diziler "matris” olarak da isimlendirilir.

`C` dilinde çok boyutlu bir dizi aslında elemanları dizi olan bir dizi olarak ele alınır. Tanımda birinci köşeli parantez içine yazılan sabit ifadesi dizinin boyutu, diğer köşeli parantezler içine yazılan sabit ifadeleri ise dizinin tür bilgisinin bileşenleridir. Aşağıdaki tanımlamalara bakalım:

```
int a[5][10][20];
int b[10][20];
int c[20];
int d;
```

+ `a` dizisi her elemanı iki boyutlu dizi olan `5` elemanlı bir dizidir. `a` dizisinin boyutu birinci köşeli parantezin içine yazılan `5` değeridir. `a` dizisi, `b` dizisi gibi `5` elemana sahip bir dizidir. `a` dizisinin elemanlarının türü 
`int [10][20]` türüdür.
+ `b` dizisi her elemanı tek boyutlu dizi olan `10` elemanlı bir dizidir. `b` dizisinin boyutu birinci köşeli parantezin içine yazılan `10` değeridir. `b` dizisi, `c` dizisi gibi `10` elemana sahip bir dizidir. `b` dizisinin elemanlarının türü 
`int [20]` türüdür.
+ `c` dizisi her elemanı `int` türden olan `20` elemanlı bir dizidir. `c` dizisinin boyutu köşeli parantezin içine yazılan `10` değeridir. `c` dizisi `d` gibi `20` elemana sahip bir dizidir. `c`dizisinin elemanlarını `int` türündendir.

```
int a[10][20];
```
Yukarıdaki tanımlamaya dayanarak derleyici `a` dizisinin yeri `(storage)` olarak bellekte

```
10 * 20 * sizeof(int)
```
byte büyüklüğünde bir blok ayırır. Bu dizi de tüm diğer diziler gibi bellekte tek bir blok halinde `(contiguous)` bulunur:

```
#include <stdio.h>

int main()
{
	int a[10][20];

	printf("sizeof(a)       = %u\n", sizeof(a));
	printf("sizeof(a[0])    = %u\n", sizeof(a[0]));
	printf("sizeof(a[0][0]) = %u\n", sizeof(a[0][0]));

	return 0;
}
```
Yukarıdaki kodu `int` türünün `4` byte olduğu benim çalıştığım sistemde derleyip çalıştırdığımda çıktısı şu şekilde oldu:

```
sizeof(a)       = 800
sizeof(a[0])    = 80
sizeof(a[0][0]) = 4
```

+ `a` ifadesi elemanları `20` elemanlı`int` dizi olan `10` elemana sahip diziye
+ `a[0]` ifadesi `a` dizisinin ilk elemanı olan `20` elemanlı `int` diziye
+ `a[0][0]` ifadesi ise `a` dizisinin ilk elemanı olan `20` elemanlı dizinin ilk elemanı olan `int türden nesneye karşılık gelmektedir.

Çok boyutlu dizilerin tanımlanmasında tür eş isim `(typedef)` bildirimlerinden de faydalanılabilir:

```
typedef int Intar5[5];
Intar5 a[10];
```
Yukarıdaki kodda bir `typedef` bildirimiyle `5` elemanlı `int` dizi türüne `Intar5` eş ismi veriliyor. Daha sonra elemanları `Intar5` türünden boyutu `10` olan bir dizi tanımlanıyor. `a` dizisi `typedef` bildirimi olmadan aşağıdaki gibi tanımlanabilirdi:

```
int a[10][5];
```
`ar` bir dizi ismi olmak üzere

```
sizeof(ar) / sizeof(ar[0])
```
ifadesinin değeri `ar` dizisinin boyutudur, değil mi?

```
#include <stdio.h>

int main()
{
	int a[5][10][20];
	double b[8][20];

	printf("a dizisinin boyutu = %u\n", sizeof(a) / sizeof(a[0]));
	printf("b dizisinin boyutu = %u\n", sizeof(b) / sizeof(b[0]));

	return 0;
}
```

İki boyutlu bir dizinin elemanlarıyla bu dizinin temsil ettiği matrisin elemanlarını birbirlerine karıştırmak, sık yapılan hatalardandır:

```
int ar[5][10];
```
`5 x 10` boyutunda bir tamsayı matrisinin `50` elemanı vardır. Ancak yukarıdaki `a` dizisi yalnızca `5` elemana sahiptir. Matristeki tamsayılar `a` dizisinin elemanı olan dizilerin elemanlarıdır. `a` dizisinin elemanları matrisin satırlarıdır.
Yukarıdaki kodda `ar` isimli dizide yer alan toplam `50 int` nesnenin herhangi birine nasıl erişilebilir? Bunun için bir gösterici `(pointer)` operatörü olan köşeli parantez operatörü dizi ismiyle birlikte iki kez kullanılabilir. En yüksek öncelik seviyesinde bulunan köşeli parantez operatörü öncelik yönü soldan sağadır `(left associative)`:

```
ar[3]
```
ifadesiyle `ar` dizisinin `3` indisli elemanı olan `10` elemanlı `int` diziye erişilir.

```
ar[3][4]
```
ifadesiyle de `ar` dizisinin `3` indisli elemanı olan `10` elemanlı `int` dizinin `4` indisli elemanına erişilir.

## Çok boyutlu dizilere ilk değer verilmesi
Çok boyutlu dizilere de ilk değer verilebilir. Tüm dizilerde olduğu gibi çok boyutlu bir dizinin elemanlarına da küme parantezi içinde virgüllerle ayrılan bir liste `(comma separated list)` ile ilk değer verilir:

```
#include <stdio.h>

int main()
{
	int a[3][4] = { { 1, 2, 3, 4}, 
	                { 6, 7, 8, 9}, 
			{ 11, 12, 13, 14} 
	};
	int i, k;

	for (i = 0; i < 3; ++i) {
		for (k = 0; k < 4; ++k)
			printf("%2d ", a[i][k]);
		printf("\n");
	}

	return 0;
}
```
`a` dizisinin elemanları da dizi olduğu için küme parantezi içinde yeniden küme parantezlerinin kullanıldığını görüyorsunuz. İstenirse ilk değerleri `(initializer)` içeren bloğun içinde içsel bloklar kullanılmadan da ilk değer verilebilir. Bu durumda derleyici verilen ilk değerleri sırasıyla, eleman olan dizilerin elemanları ile eşler:

```
#include <stdio.h>

int main()
{
 	int a[3][4] = { 1, 2, 3, 4, 5, 6, 7, 8, 9};    
   	int i, k;
    	
	for (i = 0; i < 3; ++i) {
		for (k = 0; k < 4; ++k)
			printf("%2d ", a[i][k]);
		printf("\n");
	}    
   	
}
```
Kodu derleyip çalıştırdığınızda şöyle bir ekran çıktısı elde edeceksiniz:

 ```
 1  2  3  4
 5  6  7  8
 9  0  0  0
 ```
Aşağıdaki kodda ise `3` boyutlu bir tamsayı dizisine ilk değer veriliyor:

```
#include <stdio.h>

int main()
{
	int a[4][2][5] = {
		{ { 1, 2}, { 4, 5, 6} },
		{ {7, 8, 9}, {10, 11, 12, 13, 14} },
		{ {15, 16, 17, 18, 19}, {20, 21, 22} }
	};
	int i, j, k;

	for (i = 0; i < 4; ++i) {
		for (j = 0; j < 2; ++j) {
			for (k = 0; k < 5; ++k) {
				printf("%2d ", a[i][j][k]);
			}
			printf("\n");
		}
		printf("\n\n");
	}
}
```
Bir diziye ilk değer verilmesi durumunda ilk değer verilmeyen dizi elemanlarının `0` değeriyle hayata başlatıldığını hatırlayalım. Yukarıdaki program derlenip çalıştırıldığında ekran çıktısı şöyle oldu:

 ```
 1  2  0  0  0
 4  5  6  0  0

 7  8  9  0  0
10 11 12 13 14

15 16 17 18 19
20 21 22  0  0

 0  0  0  0  0
 0  0  0  0  0
```

## çok boyutlu dizilerin adresleri
Aşağıdaki gibi bir dizimiz olsun:

```
int a[5][64];
```
Bu dizinin adresi nedir? Bu dizinin ilk elemanı `a[0]`'dır. `a[0]`, `64` elemanlı `int` türden bir dizidir. Şimdi dizinin ilk elemanının adresinin alındığını düşünelim:

```
&a[0]
```
Bu ifadenin türü nedir? `C` dilini yeni öğrenmekte olanlar böyle bir ifadenin türünün

```
int **
```
olması gerektiğini düşünebilirler. Oysa bu adresin türü

```
int (*)[64]
```
olarak ifade edilir.
Bir dizinin ilk elemanının adresine `1` topladığımızda dizinin ikinci elemanının adresini elde ederiz, değil mi? Bu durumda, `a` dizisinin ilk elemanının adresini tutacak bir gösterici de `1` arttırıldığında `a` dizisinin `2.` elemanının adresi elde edilmelidir. Yani adres

```
sizeof(int) * 64
```
kadar artmalıdır. Böyle bir gösterici değişken aşağıdaki gibi tanımlanır:

```
int (*pa)[64]
```
`a` dizisinin ilk elemanının adresi bu türden bir gösterici değişkene ilk değer verebilir:

```
int (*pa)[64] = &a[0];
```
Bir dizinin ismi bir ifade içinde kullanıldığında otomatik olarak dizinin ilk elemanının adresine dönüştürülür `(array to pointer conversion / array decay)`, değil mi? O zaman

```
&a[0]
```
ifadesi yerine doğrudan `a` ifadesi de yazılabilir:

```
int (*pa)[64] = a;
```
Bu durum şöyle de ifade edilebilir: `pa`, `int` türden `10` elemanlı bir diziyi gösteren gösterici değişkendir. `pa` herhangi bir boyuttaki `int` türden bir diziyi değil, yalnızca `10` elemanlı `int` türden bir diziyi gösterebilir. `pa`'nın değeri örneğin `++` işleci ile `1` artırılırsa, bellekte yer alan bir sonraki `64` elemanlı `int` türden diziyi gösterecek hale gelir. Yani

```
pa + 1
```

adresinin sayısal bileşeni `pa` adresinin sayısal bileşeninden `sizeof(int) * 64` kadar daha büyüktür.
`pa + 1` ifadesi iki boyutlu a dizisinin ikinci elemanı olan `64` elemanlı `int` türden dizinin adresine karşılık gelir. Aşağıdaki koda bakalım:

```
#include <stdio.h>

#define		ROW		4	
#define		COL		5

int main()
{
	int a[5][64] = { 0 };
	int(*p)[64] = a;

	int k;

	for (k = 0; k < 5; ++k) {
		printf("%p %p\n", a + k, p);
		++p;
	}
}
```
int türünün `4` byte olduğu bir sistemde örnek bir çıktı şöyle olabilir:

```
008FF8DC 008FF8DC
008FF9DC 008FF9DC
008FFADC 008FFADC
008FFBDC 008FFBDC
008FFCDC 008FFCDC
```
Şimdi de aşağıdaki koda bakalım:

```
#include <stdio.h>

#define		ROW		3	
#define		COL		4

int main()
{
	int a[ROW][COL] = {
		{ 1, 1, 1, 1 },
		{ 2, 2, 2, 2 },
		{ 3, 3, 3, 3 },
	};
	
	int *ptr = &a[0][0];
	
	for (int k = 0; k < ROW * COL; ++k) {
		if (k && k % 4 == 0)
			printf("\n");
		printf("%d ", *ptr++);
	}
}
```

`main` işlevi içinde `a` isimli iki boyutlu diziye ilk değer verildiğini görüyorsunuz.

```
int *ptr = &a[0][0];
```
deyimiyle `ptr` göstericisine `a` dizisinin ilk elemanı olan dizinin ilk elemanının adresiyle ilk değer veriliyor. Bu deyim yerine

```
int *ptr = a;
```
yazılsaydı `C`'de geçerli olmakla birlikte yanlış kabul edilecek ve `C++` dilinde de bu kod geçersiz olacaktı. Çünkü `a` dizi ismi derleyici tarafından `a` dizisinin ilk elemanının adresine dönüştürülecekti. Bu adresin türünün

```
int (*)[4]
```
olduğunu hatırlayalım. İlk değer verme aşağıdaki gibi de yapılabilirdi:

```
int *ptr = (int *)a;
```
Yazdığımız kod iki boyutlu bir dizinin elemanlarının bellekte ardışık `(contiguous)` olmasından faydalanıyor. `ptr` göstericisi `a` dizisinin ilk elemanı olan dizinin son elemanını yani

```
a[0][3]
```
nesnesini gösterdiği zaman onu `1` arttırdığımızda, `a` dizisinin ikinci elemanı olan dizinin ilk elemanını yani

```
a[1][0]
```
nesnesini gösteriyor olacak. Buradan şu anlaşılmalıdır: Çok boyutlu dizinin elemanları aslında belleğe ardışık olarak yerleştirilen tek boyutlu bir dizi olarak kullanılabilir.

## iki boyutlu dizilerin işlevlere gönderilmesi
İki boyutlu bir dizinin işleve gönderilmesi, tek boyutlu dizilerin işleve gönderilmesinden farklı değildir. Bir diziyi bir işleve göndermek için dizinin ilk elemanının adresi ile dizinin boyutu işleve argüman olarak gönderilir, değil mi? Bu durumda işlevin bir parametre değişkeni, dizinin başlangıç adresini tutmaya uygun türden bir gösterici olmalıdır. İşlevin diğer parametresi de dizinin boyutunu alabilir. Aşağıdaki programı inceleyin:

```
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

#define	ROW		5
#define	COL		10

void fillMatrix(int(*ptr)[COL], size_t size)
{
	size_t i, k;

	for (i = 0; i < size; ++i) {
		for (k = 0; k < COL; ++k)
			ptr[i][k] = rand() % 100;
	}
}

void displayMatrix(const int(*ptr)[COL], size_t size)
{
	size_t i, k;

	for (i = 0; i < size; ++i) {
		for (k = 0; k < COL; ++k)
			printf("%2d ", ptr[i][k]);
		printf("\n");
	}
}

int main()
{
	int a[ROW][COL];

	srand((unsigned int)time(NULL));

	fillMatrix(a, ROW);
	displayMatrix(a, ROW);

	return 0;
}
```
Yukarıdaki programda `fillMatrix` isimli işlev, adresini ve boyutunu aldığı, elemanları `int[COL]` türünden olan dizileri, rastgele değerlerle dolduruyor. `displayMatrix` isimli işlev ise bu tür dizileri bir matris formunda standart çıkış akımına yazdırıyor.

```
int(*ptr)[COL]
```
biçiminde tanımlanan bir gösterici, bir işlevin parametre değişkeni olarak kullanıldığında

```
int ptr[][COL]
```
biçiminde de yazılabilir. Yani aşağıdaki iki bildirim aslında derleyici açısından birbirine eşdeğerdir:

```
void fillMatrix(int (*ptr)[10], size_t size);
void fillMatrix(int ptr[][10], size_t size);
```

Aşağıdaki gibi tanımlanmış dizilerimiz olsun:

```
int a[10][20];
int b[5][30];
int c[20][30];
```
`a` ve `b` dizilerinin türleri farklıdır. Bu dizileri işleyecek işlevlerin parametrik yapıları da farklı olmalıdır. Ancak `b` ve `c` dizilerinin türleri aynı, boyutları farklıdır. Bu iki diziyi aynı parametrik yapıda bir işlev işleyebilir. Şimdi akla şöyle soru gelebilir: Türleri birbirinden farklı çok sayıda iki boyutlu dizimiz (matrisimiz) olsun. Her farklı türde bir matris için ayrı bir işlev mi tanımlayacağız? İşimizi kolaylaştırmak için iki ayrı yol izleyebiliriz:

+ Bu işlevlerin kodlarını kendimiz yazmak yerine bu işi önişlemci programa `(preprocessor)` yaptırabiliriz. (Bu seçeneği başka bir yazımda ele alacağım.)
+ İki boyutlu bir dizinin bellekte tek bir blokta bulunmasından faydalanarak bu diziyi tek boyutlu bir diziymiş gibi işleyecek tek bir işlev yazabiliriz:

```
void displayMatrix(const int *ptr, size_t row, size_t col);
```

`displayMatrix` işlevi kendisini çağıran koddan, elemanlarını yazdıracağı matrisin adresini, satır ve sütun sayısını alıyor.
Ancak bu parametrik yapıda bir işlev tanımlandığında bu işlev müşteri kodlar tarafından iki boyutlu bir dizinin ismiyle çağrılmamalıdır. Böyle bir çağrı `C`'de yanlış kabul edilirken `C++` dilinde de tür farklılığı yüzünden sentaks hatası olarak değerlendirilir. İşlev aşağıdaki biçimlerde çağrılabilir:

```
void displayMatrix(const int *ptr, size_t row, size_t col);

int main()
{
	int a[10][20];
	//
	displayMatrix((int *)a, 10, 20);
	displayMatrix(&a[0][0], 10, 20);
	displayMatrix(a[0], 10, 20);
}
```
Yazılacak işlevin `void *` türden bir bir parametreye sahip olması işlevin doğrudan dizi ismi ile çağrılmasını mümkün kılar. Ancak bu durumda derleyicinin yapabileceği tür uyumu kontrollerini de devre dışı bırakmış oluruz:

```
void displayMatrix(const void *vptr, size_t row, size_t col)
{
	const int *ptr = (const int *)vptr;
	size_t i, k;

	for (i = 0; i < row; ++i) {
		for (k = 0; k < col; ++k)
			printf("%d ", ptr[i + row * k]);
		printf("\n");
	}
}
```

## elemanları char dizi olan diziler
Nasıl bir yazı `char` türden bir dizide tutulabiliyor ise, mantıksal bir ilişki içindeki birden fazla yazı, elemanları `char` dizi olan iki boyutlu bir dizi içinde tutulabilir:

```
char names[10][50];
```
Yukarıda tanımlanan `names` isimli dizinin `10` elemanı vardır. `names` isimli dizinin her bir elemanı `char` türden `50` elemanlı bir dizidir. `names` dizisinde uzunluğu `49` karakteri geçmeyen, `10` yazı tutulabilir.

```
names[3]
```
Bu yazılardan dördüncüsünü tutan dizidir ve bir ifade içinde kullanıldığında derleyici `names[3]` dizisini bu dizinin adresine dönüştürür. Aşağıdaki kodu inceleyelim:

```
#include <stdio.h>
#include <string.h>

#define ARRAY_SIZE	10

const char *pnames[ARRAY_SIZE] = { 
	"Necati", "Velican", "Mehmet", "Kagan", "Nefes",
    "Poyraz", "Reyhan", "Bahadir", "Turhan", "Figen" 
};

int main()
{
	char names[ARRAY_SIZE][20];
	int k;

	for (k = 0; k < ARRAY_SIZE; ++k)
		strcpy(names[k], pnames[k]);

	for (k = 0; k < ARRAY_SIZE; ++k)
		printf("%s ", names[k]);
	printf("\n");

	for (k = 0; k < ARRAY_SIZE; ++k)
		strrev(names[k]);

	for (k = 0; k < ARRAY_SIZE; ++k)
		printf("%s ", names[k]);
	
	return 0;
}
```
`main` işlevi içinde iki boyutlu names isimli bir dizi tanımlanıyor:

```
char names[ARRAY_SIZE][20];
```
Bu dizi içinde uzunluğu en fazla `19` karakter olabilecek `ARRAY_SIZE` sayıda isim tutulabilir, değil mi?

```
for (k = 0; k < ARRAY_SIZE; ++k)
        strcpy(names[k], pnames[k]);
```
döngü deyimiyle `pnames` isimli bir gösterici dizisinin `(pointer array)` elemanlarının gösterdiği isimler, iki boyutlu `names` dizisinin elemanları olan `char` türden dizilere standart `strcpy` işleviyle kopyalanıyor. Aşağıdaki döngü deyimiyle ise her bir isim standart çıkış akımına yazdırılıyor:

```
for (k = 0; k < ARRAY_SIZE; ++k)
        printf("%s ", names[k]);
```
Aşağıdaki döngü deyimi ile `strrev` işlevine yapılan çağrılarla, iki boyutlu dizi içinde tutulan isimlerin hepsi ters çevriliyor:

```
for (k = 0; k < ARRAY_SIZE; ++k)
        strrev(names[k]);
```
## elemanları char dizi olan dizilere ilkdeğer verilmesi
`char` türden bir diziye dizgelerle ilk değer verilebileceğine göre, iki boyutlu bir dizinin elemanları olan `char` türden tek boyutlu dizilere de benzer biçimde ilk değer verilebilir:

```
char names[5][10] = {"Ali", "Veli", "Hasan", "Tuncay", "Deniz"};
```
Yukarıda, `names` isimli iki boyutlu dizinin elemanları olan, `10` elemanlı `char` türden dizilere, string sabitleri `(string literals)` ilk değer veriliyor.

Şimdi de iki boyutlu `char` türden bir dizi üzerinde işlem yapacak bazı işlevler tanımlayalım:

```
#include <stdio.h>
#include <string.h>

#define		ARRAY_SIZE	20
#define		MAX_NAME_LEN	16

void swap_str(char *p1, char *p2)
{
	char temp[ARRAY_SIZE];
	strcpy(temp, p1);
	strcpy(p1, p2);
	strcpy(p2, temp);
}

void sort_names(char ptr[][MAX_NAME_LEN], size_t size)
{
	size_t i, k;

	for (i = 0; i < size - 1; ++i) {
		for (k = 0; k < size - 1 - i; ++k) {
			if (strcmp(ptr[k], ptr[k + 1]) > 0)
				swap_str(ptr[k], ptr[k + 1]);
		}
	}
}

void display_names(const char ptr[][MAX_NAME_LEN], size_t size)
{
	size_t i;

	for (i = 0; i < size; ++i)
		printf("%s ", ptr[i]);
	printf("\n");
}

int main()
{
	char names[ARRAY_SIZE][MAX_NAME_LEN] = { 
	"Ali", "Veli", "Hasan", "Necati", "Deniz",
	"Kaan", "Selami", "Salah", "Nejla", "Macit",
	"Derya", "Funda", "Kemal", "Burak", "Bayram",
	"Ozlem", "Nuri", "Metin", "Ferhan", "Korhan" };

	display_names(names, ARRAY_SIZE);
	sort_names(names, ARRAY_SIZE);
	printf("***********************************\n");
	display_names(names, ARRAY_SIZE);
}
```

Yukarıdaki kodda tanımlanan `swap_str` işlevi adreslerini aldığı iki yazıyı takas ediyor.
`display_names` isimli işlev ise başlangıç adresini ve boyutunu aldığı iki boyutlu dizide yer alan isimleri ekrana yazdırıyor.
`sort_names` isimli işlev ise başlangıç adresini ve boyutunu aldığı iki boyutlu dizi içinde tutulan isimleri küçükten büyüğe doğru sıralıyor.

## char * türden diziyle elemanları char dizi olan dizi arasındaki farklar
Mantıksal ilişki içinde `n` tane yazı, bir gösterici dizisi yardımıyla tutulabileceği gibi iki boyutlu bir dizi içinde de tutulabilir:

```
const char *pnames[5] = {"Ali", "Veli", "Hasan", "Deniz", "Ferda"};
char names[5][20] = {"Ali", "Veli", "Hasan", "Deniz", "Ferda"};
```
Yukarıdaki diziler birbirinden tamamen farklıdır: `pnames` isimli dizinin elemanları

```
const char *
```
türdendir. Bu dizinin elemanları yazıları değil yazıların başlangıç adreslerini tutar. Yukarıdaki bildirimle tanımlanan `pnames` dizisi string sabitlerinin `(string literals)` adreslerini tutmaktadır. String sabitlerinin yalnızca okuma amacıyla kullanılabilecek yazılar olduğunu anımsamalısınız. Derleyicinin yaptığı tür kontrolünden geçse de `pnames` dizisinin elemanlarının gösterdiği yazıları değiştirme girişimi çalışma zamanı hatasına neden olur. Diğer taraftan `pnames` dizisine ilk değer olarak verilen string sabitleri statik ömürlüdür. Yani programın sonuna kadar bellekte tutulurlar.
Ancak `names` dizisinin elemanları göstericiler değil `char` dizilerdir. `names` dizisi `const` anahtar sözcüğüyle tanımlanmadığı için bu dizinin elemanlarında tutulan yazılar değiştirilebilir. `names` dizisine ilk değer vermede kullanılan `string` sabitlerinden  derleyici yalnızca derleme zamanında faydalanır. Bu yazılar programın çalışma zamanında `names` dizisi dışında başka bir bellek alanında tutulmazlar.
