# switch Deyimi

`switch` deyimi bir tamsayı ifadesinin farklı değerleri için, programın akışının farklı noktalara yönlendirilmesini, böylece farklı işlerin yapılmasını sağlar. 
`switch` deyimi, `else if` merdivenine `(else if ladder)` okunabilirlik yönünden bir seçenek oluşturur. 
Deyimin genel biçimi aşağıdaki gibidir:

```
switch (tamsayı ifadesi) {
    case sabit ifadesi1 :
    case sabit_ifadesi2 :	
    case sabit_ifadesi3 :
    .......
    case sabit ifadesi n:
    default:
}
```
`switch, case, default` C dilinin anahtar sözcükleridir.

## `switch` deyiminin yürütülmesi
`switch` parantezi içindeki ifadenin sayısal değeri hesaplanır. 
Bu değerde bir `case` ifadesi olup olmadığı sınanır. 
Eğer böyle bir `case` ifadesi bulunursa programın akışı o `case` etiketini izleyen deyime yönlendirilir. 
Artık program buradan akarak ilerler. 
Daha aşağıdaki diğer deyimler de yapılır `(fall through)`. 
`switch` parantezi içindeki ifadenin sayısal değerine eşit bir `case` ifadesi yok ise, programın akışı (varsa) `default` anahtar sözcüğünü `(default case)` izleyen deyime yönlendirilir.

```
#include <stdio.h>

int main()
{
    int n;

    printf("haftanin kacinci gunu : ");
    scanf("%d", &n);
    switch (n) {
        case 1: printf("pazartesi");
	case 2: printf("sali");
	case 3: printf("carsamba");
	case 4: printf("persembe");
	case 5: printf("cuma");
	case 6: printf("cumartesi");
	case 7: printf("pazar");
    }
}
```
Yukarıdaki örnekte `scanf` işlevi ile standart giriş akımından `n` değişkenine `3` değeri alınmış olsun. 
Bu durumda programın ekran çıktısı şu şekilde olur:

```
carsamba
persembe
cuma
cumartesi
pazar
```

Eğer eşdeğer `case` ifadesi bulunduğunda yalnızca bu ifadeye ilişkin deyim(ler)in yürütülmesi istenirse `break` kontrol deyiminden faydalanılır. 
`break` deyiminin kullanılmasıyla, döngülerden olduğu gibi `switch` deyiminden de çıkılabilir. 
Daha önce verilen örnekteki koda `break` deyimlerini ekliyoruz:

```
#include <stdio.h>

int main()
{
    int n;

    printf("haftanın kaçıncı gunu : ");
    scanf("%d", &n);
    switch (n) {
        case 1: printf("pazartesi"); break;
	case 2: printf("sali"); break;
	case 3: printf("carsamba"); break;
	case 4: printf("persembe"); break;
	case 5: printf("cuma"); break;
	case 6: printf("cumartesi"); break;
	case 7: printf("pazar"); break;
    }
}
```

Yukarıdaki kodda `scanf` işlevi ile, standart giriş akımından `n` değişkenine yine `3` değeri alınmış olsun. 
Bu durumda programın ekran çıktısı şu şekilde olur:

`carsamba`

Uygulamalarda, `switch` deyiminde çoğunlukla her `case` ifadesi için bir `break` deyimi kullanılır. 
Tabi böyle bir zorunluluk yoktur.
`switch` deyiminin ana bloğu tek bir bilinirlik alanı `(scope)` kabul edilir. 
Bir `case` etiketini birden fazla deyimin izlemesi durumunda bu deyimlerin bloklanmasına gerek yoktur. 
`case` ifadelerinin belirli bir sırayı izlemesi gibi bir zorunluluk yoktur. 
`switch` parantezi içindeki tamsayı ifadesinin değerine eşit bir `case` ifadesi bulunamaz ise `switch` deyiminden bir şey yapılmadan çıkılır.

## default case
`default` bir anahtar sözcüktür. `switch` deyimi gövdesine yerleştirilen `default` anahtar sözcüğünü ':' atomu izler. 
Oluşturulan bu `case`'e `default case` denir.
`switch` parantezi içindeki ifadenin değerine eşit bir `case` ifadesi bulunamazsa programın akışı `default case`'den sonra gelen deyime yönlendirilir. 
Daha önceki örnek kodda yer alan `switch` deyimine `default case` ekliyoruz:

```
#include <stdio.h>

int main()
{
    int n;

    printf("haftanin kacinci gunu : ");
    scanf("%d", &n);
    switch (n) {
	case 1 : printf("pazartesi"); break;
	case 2 : printf("sali"); break;
	case 3 : printf("carsamba"); break;
	case 4 : printf("persembe"); break;
	case 5 : printf("cuma"); break;
	case 6 : printf("cumartesi"); break;
	case 7 : printf("pazar"); break;
	default: printf("gecersiz gun");
    }
}

```
`default case` tipik olarak sonuncu `case` yapılsa da bu bir zorunluluk değildir. 
`default case`, `switch` deyimi gövdesi içinde herhangi bir sıradaki `case` olabilir. 
Bir `switch` deyiminde birden fazla `default case` bulunması geçerli değildir.

Derleyici farklı bir şekilde eniyileme `(optimization)` yapmaz ise, `switch` parantezi içindeki ifadenin sayısal değerine eşit bir `case` ifadesi bulunana kadar, 
tüm `case` ifadeleri yukarıdan aşağıya doğru sırayla sınanır. 
`case` ifadelerinin oluşma sıklığı ya da olasılığı hakkında elde bir bilgi varsa, olasılığı ya da sıklığı yüksek olan `case` ifadelerinin daha önce yazılması gereksiz karşılaştırma sayısını azaltabilir, 
kodun okunmasını ve anlaşılmasını kolaylaştırabilir.

`case` ifadelerinin, tamsayı türünden `(integer types)` sabit ifadesi `(constant expression)` olması gerekir. 
Sabit ifadeleri, derleme aşamasında derleyici tarafından net sayısal değerlere dönüştürülebilir:

```
case 1 + 3:
```
Yukarıdaki case ifadesi geçerlidir. Çünkü `1 + 3` bir sabit ifadesidir. Ama,

```
case x + 5:
```
Eğer `x` bir değişkenin ismi ise yukarıdaki `case` ifadesi geçersizdir. 
Derleyici, derleme aşamasında sabit ifadesi olmayan bir ifadenin değerini hesaplayamaz.

```
case 'a':
```
Yukarıdaki `case` ifadesi geçerlidir. `'a'` bir karakter sabitidir `(character literal)`. 
`case` ifadesi tamsayı türünden bir sabit ifadesidir.

```
case "a":
```

Yukarıdaki `case` ifadesi geçersizdir. `"a"` bir string sabitidir `(string literal)`. 
Bir string sabiti tamsayı tamsayı sabiti değildir ve `case` ifadesi olarak kullanılamaz.

```
case 3.5:
```
Yukarıdaki `case` ifadesi geçersizdir. 
`3.5` bir gerçek sayı sabitidir.

`C`'de kendisi `const` nesneler sabit ifadesi gereken yerde kullanılamazlar. 
`C++` dilinde sabit ifadesiyle ilk değerlerini almış `const` nesneler sabit ifadesi gereken yerlerde kullanılabilirler. 
Aşağıdaki `switch` deyimi `C`'de geçersiz `C++` dilinde geçerlidir:

```
int getSize(void);
void processSmallSize(void);
void processMediumSize(void);
void processLargeSize(void);

int main()
{
    const int smallSize = 10;
    const int mediumSize = 20;
    const int largeSize = 30;

    switch (getSize()) {
        case smallSize: processSmallSize(); break;
	case mediumSize: processMediumSize(); break;
	case largeSize: processLargeSize(); break;
    }
}
```
Bir `switch` deyiminde aynı değerde birden fazla `case` ifadesinin bulunması geçersizdir. 
Yani tüm `case` ifadeleri farklı değerlerde olmalıdır:

```
switch (x) {
    case 1:
    case 2:
    case 1: //geçersiz
}

```
`switch` kontrol deyimi yerine bir `else if` merdiveni yazılabilir. 
Yani `switch` deyimi olmasaydı, yapılmak istenen iş, bir `else if` merdiveni ile de gerçekleştirilebilirdi. 
Ancak bazı durumlarda `else if` merdiveni yerine `switch` deyimi kullanmak kodun okunmasını ve yazılmasını kolaylaştırır. 
Ayrıca derleyiciler `else if` merdiveni yerine `switch` deyiminin kullanılması durumunda bir sıçrama tablosu `(jump table)` oluşturarak daha etkin bir kod üretebilirler. Örneğin:

```
if (a == 1) 				
    deyim1;					
else if (a == 2)			
    deyim2;					
else if (a == 3)			
    deyim3;					
else if (a == 4)			
    deyim4;
else
    deyim5;
```

Yukarıdaki `else if` merdiveni ile aşağıdaki `switch` deyimi işlevsel olarak eşdeğerdir:

```
switch (a) {
    case 1 : deyim1; break;
    case 2 : deyim1; break;
    case 3 : deyim1; break;
    case 4 : deyim1; break;
    default: deyim5;
}
```

`else if` merdivenindeki son basamakta yer alan `else` kısmının `switch` deyimindeki `default case` karşılığı olduğunu görebiliyor musunuz? 
Her `switch` deyiminin yerine aynı işi görecek şekilde bir `else if` merdiveni yazılabilir. 
Ama her `else if` merdiveni karşılığı bir `switch` deyimi yazılamaz. 
`switch` parantezi içindeki ifadenin bir tam sayı türünden olması gerektiğini hatırlayalım. 
`case` ifadeleri de tam sayı türlerinden sabit ifadesi olmak zorundadır. 
`switch` deyimi, tam sayı türünden bir ifadenin değerinin değişik tam sayı değerlerine eşitliğinin sınanması ve eşitlik durumunda farklı işlerin yapılması için kullanılır. 
Oysa `else if` merdiveninde her türlü karşılaştırma söz konusu olabilir. Örnek:

```
if (x > 20)
    m = 5;
else if (x > 30 && x < 55)
    m = 3;
else if (x > 70 && x < 90)
    m = 7;
else
    m = 2;
```

Yukarıdaki `else if` merdiveninin yerine doğrudan bir `switch` deyimi yazılamaz.

`switch` deyimi bazı durumlarda `else if` merdivenine göre çok daha kolay okunabilir ve anlaşılabilir bir yapı oluşturur. 
Yani `switch` deyiminin kullanılması her şeyden önce kodun daha kolay okunabilmesini, anlaşılmasını, değiştirilebilmesini sağlar. 
`switch` deyiminin `debug` edilmesi `else if` merdivenine göre daha kolaydır.

Birden fazla `case` ifadesi için aynı işlemlerin yapılması şöyle sağlanabilir:

```
case 1:
case 2:
case 3:
    deyim1;
    deyim2;
    break;
case 4:
```
Bunu yapmanın daha kısa bir yolu yoktur. Bazı programcılar kaynak kodun yerleşimini aşağıdaki gibi düzenler:

```
case 1: case 2: case 3: case 4: case 5:
    deyim1; deyim2;
```
Aşağıdaki kodu inceleyin:

```
void print_season(int month)
{
    switch (month) {
        case 12:
	case 1 :
	case 2 : printf("winter"); break;
	case 3 :
	case 4 :
	case 5 : printf("spring"); break;
	case 6 :
	case 7 :
	case 8 : printf("summer"); break;
	case 9 :
	case 10:
	case 11: printf("autumn"); break;
    }
}
```
`print_season` işlevi, bir ayın sıra numarasını, yani yılın kaçıncı ayı olduğu bilgisini alıyor. 
Bu ay yılın hangi mevsimi içinde ise, o mevsimin ismini ekrana yazdırıyor. 
Aynı iş bir `else if` merdiveniyle nasıl yapılabilirdi? 
Her `if` deyiminin koşul ifadesi içinde “mantıksal veya” işleci kullanılabilirdi:

```
void print_season(int month)
{
    if (month == 12 || month == 1 || month == 2)
	printf("winter");
    else if (month == 3 || month == 4 || month == 5)
	printf("spring");
    else if (month == 6 || month == 7 || month == 8)
	printf("summer");
    else if (month == 9 || month == 10 || month == 11)
	printf("autumn");
}
```

`case` ifadeleri olarak sembolik sabitlerin `(symbolic constants)` kullanılması çok sık karşılaşılan bir durumdur:

```
#define		OFF       0
#define		ON        1
#define		STANDBY   2
#define		HOLD      3

int get_remote_case(void);

int main()
{
	//
    switch (get_remote_case()) {
        case OFF		: //
	case ON			: //
	case STANDBY            : //
	case HOLD		: //
	default			: //
    }
	//
}
```

`case` ifadeleri olarak karakter sabitleri de kullanılabilir:

```
#include <stdio.h>

void displayGrade(int grade)
{
    switch (grade) {
	case 'A': printf("Mukemmel"); break;
	case 'B': printf("Cok iyi"); break;
	case 'C': printf("İyi"); break;
	case 'D': printf("Kotu"); break;
	case 'F': printf("Basarisiz"); break;
	default : printf("Gecersiz not"); break;
    }
}
```

`case` etiketlerini izleyen tam sayı ifadelerinin numaralandırma sabitleri `(enumaration constants)` olması çok sık karşılaşılan bir durumdur:

```
#include <stdio.h>

typedef enum
{
    COLOR_BLACK,
    COLOR_WHITE,
    COLOR_RED,
    COLOR_GREEN,
    COLOR_BLUE
}Colors;

void printColor(Colors color)
{
    switch (color) {
	case COLOR_BLACK: printf("Black"); break;
	case COLOR_WHITE: printf("White"); break;
	case COLOR_RED: printf("Red"); break;
	case COLOR_GREEN: printf("Green"); break;
	case COLOR_BLUE: printf("Blue"); break;
        default: printf("invalid color"); break;
    }
}
```
`case` bir anahtar sözcük olsa da teknik olarak bir etikettir `(label)`. 
`C` dilinde etiketlerden sonra bir deyimin `(statement)` bulunması zorunludur:

```
#include<stdio.h>

int main()
{
    int x = 1;
    switch (x) {
        case 1: printf("bir\n"); break;
	case 2: printf("iki\n"); break;
	case 3: 
    }
}

```
Yukarıdaki `switch` deyiminde `case 3` etiketinden sonra bir deyim yer almıyor. Kod geçersizdir.

`case` ifadelerini izleyen deyimlerin sayısının fazla olması kodun okunmasını zorlaştırır. 
Bu durumda, yapılacak işlemlerin uygun şekilde isimlendirilerek tanımlanmış işlevlerin çağrılmasına dönüştürülmesi iyi bir tekniktir:

```
switch (option) {
    case ADD_RECORD   :
	addRecord();
	break;
    case DELETE_RECORD:
	deleteRecord();
	break;
    case FIND_RECORD  :
	findRecord();
	break;
}
```
Yukarıdaki örnekte `case` ifadesi olarak kullanılan `ADD_RECORD, DELETE_RECORD, FIND_RECORD` daha önce tanımlanmış sembolik sabitler olabilir. 
Her bir `case` için yaptırılacak işlemler birer işlev içinde sarmalanmış.

`switch` deyimi, başka bir `switch` deyiminin ya da bir döngü deyiminin gövdesini oluşturabilir:

```
#include <stdio.h>
#include <stdlib.h>

int main()
{
    for (int i = 0; i < 100; ++i)
	switch (rand() % 7 + 1) {
		case 1: printf("Pazartesi\n"); break;
		case 2: printf("Sali\n"); break;
		case 3: printf("Carsamba\n"); break;
		case 4: printf("Persembe\n"); break;
		case 5: printf("Cuma\n"); break;
		case 6: printf("Cumartesi\n"); break;
		case 7: printf("Pazar\n"); break;
	}
}
```

Yukarıdaki `main` işlevinde `switch` deyimi, dıştaki `for` döngüsünün gövdesini oluşturuyor. 
`case` ifadelerini izleyen `break` deyimiyle yalnızca `switch` deyiminden çıkılır. 
`switch` deyimini içine alan `for` döngüsünün de dışına çıkmak için `case` ifadesinden sonra `goto` deyimi kullanılabilir.

Şimdi de aşağıdaki programı inceleyin. Programda `display_date` isimli bir işlev tanımlanıyor. 
İşlev gün, ay ve yıl değeri olarak aldığı bir tarih bilgisini İngilizce olarak

`5th Jan 1998`

yukarıdaki formatta ekrana yazdırıyor:

```
include <stdio.h>

void display_date(int day, int month, int year)
{
    printf("%d", day);
	
   switch (day) {
	case 1  :
	case 21 :
	case 31 : printf("st "); break;
	case 2  :
	case 22 : printf("nd "); break;
	case 3  :
	case 23 : printf("rd "); break;
	default : printf("th ");
   }

   switch (month) {
	case 1 : printf("Jan "); break;
	case 2 : printf("Feb "); break;
	case 3 : printf("Mar "); break;
	case 4 : printf("Apr "); break;
	case 5 : printf("May "); break;
	case 6 : printf("Jun "); break;
	case 7 : printf("Jul "); break;
	case 8 : printf("Aug "); break;
	case 9 : printf("Sep "); break;
	case 10: printf("Oct "); break;
	case 11: printf("Nov "); break;
	case 12: printf("Dec ");
    }
    printf("%d", year);
}

int main()
{
    int day, month, year;
    int n = 20;

    while (n-- > 0) {
	printf("gün ay yıl olarak bir tarih girin : ");
	scanf("%d%d%d", &day, &month, &year);
	display_date(day, month, year);
	putchar('\n');
    }
}
```

İşlevin tanımında iki ayrı `switch` deyimi kullanılıyor. 
İlk `switch` deyimiyle, gün değerini izleyen `(th, st, nd, rd)` sonekleri, ikinci `switch` deyimiyle, aylara ilişkin kısaltmalar `(Jan, Feb, Mar...)` yazdırılıyor.

`case` ifadelerini izleyen deyimlerden biri `break` deyimi olmak zorunda değildir. 
Bazı durumlarda break deyimi özellikle kullanılmaz. 
Uygun bir `case` ifadesi bulunduğunda daha aşağıdaki tüm deyimlerin de yapılması özellikle istenir. 
Aşağıdaki programı derleyerek çalıştırın:

```
#include <stdio.h>

int isleap(int y)
{
    return y % 4 == 0 && (y % 100 != 0 || y % 400 == 0);
}

int dayOfYear(int day, int month, int year)
{
    int sum = day;

    switch (month - 1) {
	case 11	: sum += 30; //fallthrough
	case 10	: sum += 31;  //fallthrough
	case 9	: sum += 30;  //fallthrough
	case 8	: sum += 31;  //fallthrough
	case 7	: sum += 31;  //fallthrough
	case 6	: sum += 30;  //fallthrough
	case 5	: sum += 31;  //fallthrough
	case 4	: sum += 30;  //fallthrough
	case 3	: sum += 31;  //fallthrough
	case 2	: sum += 28 + isleap(year); //fallthrough
	case 1	: sum += 31; 
    }
    return sum;
}

int main()
{
    int day, month, year;
    int n = 5;

    while (n-- > 0) {
 	printf("gün ay yıl olarak bir tarih girin : ");
	scanf("%d%d%d", &day, &month, &year);
	printf("%d yılının %d. günüdür!\n", year, dayOfYear(day, month, year));
    }
}
```

`dayOfYear` işlevi aldığı gün, ay ve yıl değerinin ilgili yılın kaçıncı günü olduğunu hesaplıyor. 
İşlev içinde kullanılan `switch` deyimini dikkatli bir şekilde inceleyin. `switch` deyiminin parantezi içinde, dışarıdan gelen ay değerinin `1` eksiği kullanılmış. 
Hiçbir `case` etiketinden sonra bir `break` deyimi kullanılmamış. 
Eşdeğer bir `case` ifadesi bulunduğunda, daha aşağıda yer alan tüm deyimler de yapılır. 
Böylece, dışarıdan gelen ay değerinden daha düşük olan her bir ayın kaç çektiği bilgisi, gün toplamını tutan `sum` değişkenine katılmış.

`switch` deyiminde eğer bir `case` etiketini izleyen deyim(ler) var ise ancak bu deyim(ler)den sonra bir `break` deyimi bulunmuyor ise bu şüpheli bir durumdur. 
`break` deyiminin yazılması unutulmuş olabilir. Derleyiciler ya da statik kod analizi yapan programlar uygun uyarı ayarlarında `(warning switches)` uyarı iletisi verebilirler. 
`break` deyiminin özellikle yazılmamış olması durumunda yorum `"fallthrough"` açıklamasının yazılması `break` deyiminin özellikle yazılmamış olduğunu vurgular.
