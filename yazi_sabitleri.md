# Yazı Sabitleri

_C_ dilinde çift tırnak içinde yazılan karakterlerin oluşturduğu atomlara _(token)_ yazı sabiti _(string literal)_ denir. Örneğin:

```
"Necati Ergin"
"x = %d\n"
"lütfen bir tamsayı giriniz : "
```
ifadelerinin hepsi yazı sabitleridir.
_C_'de bir yazı sabiti, derleyiciler tarafından aslında _char_ türden bir dizi _(array)_ olarak ele alınır ve bir ifade içinde kullanıldığında söz konusu dizinin adresine dönüştürülür. _C_ derleyicileri, derleme aşamasında bir yazı sabiti ile karşılaştığında, bu yazıyı oluşturan karakterleri belleğin güvenli bir bölgesine, sonunda sonlandırıcı karakter _(null character)_ olacak şekilde yerleştirecek bir kod üretirler. Bu durumda yazı sabitleri kod içinde kullanıldıklarında aslında, derleyici tarafından oluşturulan dizilerin başlangıç adresleri olarak ele alınırlar. Örneğin:

```
char *p = "CAN";
```
gibi bir kod parçasının derlenmesi sırasında, derleyici önce _"CAN"_ yazısını taşıyacak _4_ elemanlı bir _char_ dizi için belleğin güvenli bir bölgesinde yer ayırır ve bu diziye `CAN` yazısı ile ilk değer verir. Daha sonra bu dizinin adresiyle `p` gösterici değişkenine ilk değer verecek şekilde kod üretir.
Bir yazı sabiti kod içinde `char` türden bir dizinin başlangıç adresi olarak ele alındığına göre, bir yazı sabitinin `char` türden bir adres gereken yerde kullanılması geçerlidir. Aşağıdaki `main` işlevini derleyerek oluşturacağınız programı çalıştırın:

```
#include <stdio.h>

int main()
{
	printf("adres = %p\n", "Gurbuz");
	printf("isim  = %s\n", "Gurbuz");

	return 0;
}
```
Yukarıdaki kodda _main_ işlevi içinde yapılan ilk _printf_ çağrısında _"Gurbuz"_ yazı sabiti ile _%p_ format dönüştürme karakterleri _(conversion specifier)_ eşleniyor. _printf_ işlevi ile, adres bilgilerinin sayısal bileşenlerinin _%p_ format dönüştürme karakteriyle ekrana onaltılık sayı sisteminde yazdırılabileceğini hatırlayın. Bu durumda çalışan kod, derleyicinin _"Gurbuz"? yazısının yerleştirdiği dizinin başlangıç adresini ekrana yazar. İkinci _printf_ çağrısında ise _"Gurbuz"_ yazısı _%s_ format dönüştürme karakteri ile eşleniyor. Bu durumda ekrana ilgili adresteki yazı, yani

```
Gurbuz
```
yazısı yazdırılır. Şimdi de aşağıdaki çağrıya bakın:

```
putchar(*"Kagan");
```
_"Kagan"_ yazı sabitinin bu kez içerik _(dereferencing)_ operatörünün terimi olduğunu görüyorsunuz. İçerik operatörü, terimi olan adresteki nesneye erişimi sağladığına göre, bu nesnenin değeri _'K'_ karakterinin kod numarası olan tam sayıdır. Çağrılan _putchar_ işlevinin çalışmasıyla standart çıkış akımına _'K'_ karakteri verilir.
Aşağıda tanımlanan _get_hex_char_ işlevi, onaltılık sayı sisteminde bir basamak değerine karşılık gelen karakterin kullanılan karakter setindeki kod numarasını döndürüyor:

```
#include <stdio.h>

int get_hex_char(int val)
{
	return "0123456789ABCDEF"[val];
}

int main()
{
	for (int i = 0; i < 16; ++i)
		putchar(get_hex_char(i));
}
```
İşlevin geri dönüş değeri

```
"0123456789ABCDEF"[val]
```
ifadesinin değeridir. Bu da yazı sabitine ilişkin yazının yerleştirildiği dizinin _val_ indisli karakterinin değeridir. `char` türden bu nesnenin değeri de yazıda yer alan karakterlerden herhangi birinin kod numarasıdır. _main_ işlevinde _0 – 15_ aralığındaki tam sayı değerleri döngü içinde _get_hex_char_ işlevine gönderilerek işlevden geri dönüş değeri ile alınan karakterler _putchar_ işlevi ile standart çıkış akımına gönderiliyor. Programın ekran çıktısı aşağıdaki gibi olur:

```
0123456789ABCDEF
```
Yazı sabitlerine karşılık gelen dizilerin bellekte kaplayacakları yer _(storage)_ derleme zamanında derleyici tarafından belirlenir. Aşağıdaki program her çalıştırıldığında ekrana hep aynı adres değeri yazılır:

```
#include <stdio.h>

int main()
{
	for (int i = 0; i < 10; ++i)
		printf("%p\n", "Tayyip");
}
```

## yazı sabitleri salt okunur yazılardır
Yazı sabitleri salt okunur bellek alanlarında tutulabilir. _C_ dilinin standartlarına göre bir yazı sabitinde yer alan karakterlerin kaynak kod içinde değiştirilme girişimi tanımsız davranıştır _(undefined behavior)_. Aşağıdaki örneği inceleyin:

```
#include <stdio.h>

int main()
{
	char *ptr = "Durak";
	*ptr = 'B';	 /* Tanımsız Davranış */

	puts(ptr);
}
```
_main_ işlevi içinde tanımlanan _ptr_ isimli gösterici değişken _"Durak"_ yazısını gösteriyor. Yazılan kod _C_ dilinin sentaksına uygun olmasına karşın, _ptr_ gösterici değişkeninin gösterdiği yazının değiştirilmesi tanımsız davranıştır. Bazı derleyiciler seçeneğe bağlı olarak _(compiler switches)_ yazı sabitlerinin değiştirilmesine olanak veren kod üretebilirler. Programcıya derleyiciler tarafından sunulan bu seçenek geçmişe doğru uyumluluğun korunması gibi çok özel durumlarda kullanılmalıdır.

## yazı sabitlerinin işlevlere argüman olarak gönderilmesi
Parametre değişkeni _char_ türden bir gösterici olan `(char *)` bir işlevi, `char` türden bir adres ile çağırmak gerektiğini biliyorsunuz. Çünkü `char` türden bir gösterici değişkene, doğal olarak `char` türden bir adres atanmalıdır. Derleyiciler açısından yazı sabitleri de `char` türden bir dizi olduklarına göre, parametre değişkeni `char` türden gösterici olan bir işlevi, bir yazı sabiti ile çağırmak son derece doğal bir durumdur:

```
puts("Kagan Aslan");
```

Burada derleyici `"Kagan Aslan"` yazısını belleğe yerleştirip sonuna sonlandırıcı karakteri koyduktan sonra artık bu yazı sabitini, karakterlerini yerleştirdiği bellek bloğunun başlangıç adresi olarak görür. `puts` işlevinin parametre değişkenine de artık `char` türden bir adres kopyalanır. `puts` işlevi parametre değişkeninde tutulan adresten başlayarak sonlandırıcı karakteri görene kadar tüm karakterleri ekrana yazar. Bu durumda ekranda

`Kagan Aslan`

yazısı çıkar.

```
#include <stdio.h>
#include <string.h>

int main()
{
	char str[20];
	
	strcpy(str, "Oguz Karan");
	puts(str);
}
```
Yukarıdaki kodda `"Oguz Karan"` yazısı `str` adresine kopyalanır. Yazı sabiti ifadelerinin derleyici tarafından `char` türden bir dizinin adresi olarak ele alındığı düşünülmelidir.

`C` dilinde bir yazı üstünde işlem yapacak bir işlevin parametre değişkeninin `char*` olarak bildirilmesinin, bu işlevin adresini aldığı diziye bir yazma işlemi yapacağını ya da adresini aldığı yazıyı değiştireceği anlamına geldiğini hatırlayalım. Eğer işlevin parametre değişkeni `const char *` olarak `(low level const)` bildirilmiş ise işlev adresini aldığı yazıyı yalnızca okuma amacıyla kullanacak demektir. Bir yazı sabiti salt okunur bir yazıyı ifade ettiğinden yalnızca parametresi `const char *` türden olan işlevlere gönderilmelidir. Eğer bir yazı sabiti yazma amaçlı bir işleve argüman olarak gönderilirse tanımsız davranış oluşur:

```
#include <string.h>

int main()
{
	char *p = "mustafa";

	strrev(p); //tanımsız davranış
	//
	return 0;
}
```
Yukarıdaki kodda `p` isimli gösterici değişkene bir yazı sabiti ile ilk değer veriliyor. Daha sonra `p` değişkeninin değeri olan adres, `strrev` işlevine argüman olarak gönderiliyor. `strrev` işlevinin bildirimi şöyle:

```
char *strrev(char *p);
```
`strrev` adresini aldığı yazıyı ters çeviriyor. Bu durumda `strrev` işlevi, değiştirilmemesi gereken yani salt okuma amacıyla kullanılması gereken `"mustafa"` yazısını değiştirme girişiminde bulunacak ve çalışma zamanı hatası oluşacak.

## özdeş yazı sabitleri
`C` derleyicileri kaynak kodun çeşitli yerlerinde tamamen özdeş yazı sabitlerine rastlasa bile bunlar için farklı yerler ayırabilir. Ya da derleyici, yazı sabitlerinin salt okunur yazılar olmasına dayanarak, özdeş yazı sabitlerinin yalnızca bir kopyasını bellekte saklayabilir. Özdeş yazı  sabitlerine ilişkin yazıların bellekte nasıl saklanacağı derleyicinin seçimine bırakılmıştır. Birçok derleyici, özdeş yazı sabitlerinin bellekte nasıl tutulacakları konusunda programcının seçim yapmasına olanak verir.

## Yazı sabitlerinin karşılaştırılması
Yazı sabitlerinin doğrudan karşılaştırılması yanlış bir işlemdir:

```
#include <stdio.h>

int main()
{
	char *p1 = "Ankara";
	char *p2 = "Ankara";

	if (p1 == p2)
		printf("dogru!\n");
	else
		printf("yanlis!\n");
}
```
Yukarıdaki kodun çalıştırılması durumunda, ekrana `"yanlış"` ya da `"doğru"` yazdırılması garanti altında değildir. Üretilecek kod derleyiciye bağlı olarak değişebilir. Eğer derleyici iki `"Ankara"` yazısını bellekte ayrı ayrı yerlere yerleştirmiş ise, `==` karşılaştırma operatörü ` 0 (yanlış)` değerini üretir. Ancak bir derleyici, `"Ankara"` yazısını tek bir yere yerleştirip her iki yazı sabitini de aynı adres olarak da ele alabilir. Böyle bir durumda `==` karşılaştırma operatörü `1 (doğru)` değerini üretir.
Benzer bir yanlışlık aşağıdaki kod parçasında da yapılıyor:

```
#include <stdio.h>

int main()
{
	char *pstr = "bukalemun";
	char str[20];

	printf("parolayı giriniz : ");
	scanf("%s", str);	
	if (pstr == str)
		printf("dogru parola\n");
	else
		printf("yanlış parola\n");
}
```
Yukarıdaki programda `str` bir dizinin ismi. `str` ismi işleme sokulduğunda derleyici tarafından otomatik olarak bu dizinin başlangıç adresine dönüştürülür. `pstr` ise `char` türden bir gösterici değişkendir. `pstr` gösterici değişkenine `"bukalemun"` yazı sabiti ile ilk değer verildiğinde, derleyici önce `"bukalemun"` yazısını bellekte güvenli bir yere yerleştirir. Daha sonra `pstr` değişkenini yazının yerleştirildiği yerin başlangıç adresi ile hayata başlatır. Kullanıcının, parola olarak `"bukalemun"` girişi yaptığını varsayın. Bu durumda `if` deyimi içinde yalnızca `s` adresiyle `pstr` değişkeninin değeri olan adresin eşit olup olmadığı sınanır. Bu adresler eşit olmadıkları için ekrana `"yanlış parola"` yazılır. İki yazının birbirine eşit olup olmadığı standart `strcmp` işlevi ile sınanmalıydı:

```
#include <stdio.h>
#include <string.h>

int main()
{
	char *pstr = "bukalemun";
	char str[20];

	printf("parolayı giriniz : ");
	scanf(%s", s);
	if (!strcmp(pstr, s))
		printf("dogru parola\n");
	else
		printf("yanlış parola\n");
}
```
Tabi eşitlik ya da eşitsizlik karşılaştırması gibi, büyüklük küçüklük karşılaştırması da doğru değildir:

```
#include <stdio.h>

int main()
{
	const char *p1 = "CAN";
	const char *p2 = "ATA";

	if (p1 > p2)
		printf("doğru!\n");
	else
		printf("yanlis!\n");
}
```
Yukarıdaki `if` deyiminde `CAN` ve `ATA` isimleri `strcmp` işlevinin yaptığı biçimde, yani birer yazı olarak karşılaştırılmıyor. Aslında karşılaştırılan yalnızca iki adresin sayısal bileşenleridir.

## yazı sabitlerinin ömürleri
Yazı sabitleri statik ömürlü varlıklardır. Yazı sabitleri ile ifade edilen yazılar, tıpkı global gibi programın yüklenmesiyle bellekte yer kaplamaya başlar, programın sonuna kadar bellekte kalır. Dolayısıyla yazı sabitleri çalıştırılabilen kodu büyütür. Birçok sistemde statik ömürlü verilerin yerleştirileceği bellek alanının büyüklüğü belli bir sınırlamaya tabidir. Yazı sabitleri derleyici tarafından amaç koda `(object code)` bağlayıcı program tarafından da çalıştırılabilir dosyaya yazılır. Programın belleğe yüklenmesiyle hayat kazanırlar.

## bir işlevin bir yazı sabiti ile geri dönmesi
Adrese geri dönen bir işlevin, yerel bir değişkenin ya da yerel bir dizinin adresi ile geri dönmesi, gösterici hatasıdır. İşlev sonlandığında yerel değişkenler bellekten boşaltılacağı için, işlevin geri döndürdüğü adres güvenli bir adres olmaz. Aşağıdaki programda böyle bir hata yapılıyor:

```
#include <stdio.h>

char *getName()
{
	char s[100];

	printf("ad ve soyad giriniz : ");
	fgets(s, 100, stdin);

	return s;	/* Yanlış! */
}

int main()
{
	char *ptr;

	ptr = getName();
	puts(ptr);
}
```
Ancak `char` türden bir adres döndüren bir işlev bir yazı sabiti ile geri dönebilir. Bu durumda bir çalışma zamanı hatası söz konusu olmaz. Statik ömürlü varlıklar olan yazı sabitlerine ilişkin yazılar programın çalışma süresi boyunca bellekteki yerlerini korurlar. Örneğin aşağıdaki işlev geçerli ve doğrudur:

```
#include <stddef.h>

const char *get_day_name(int day)
{
	switch (day) {
	case 1: return "Sunday";
	case 2: return "Monday";
	case 3: return "Tuesday";
	case 4: return "Wednesday";
	case 5: return "Thursday"; 
	case 6: return "Friday";
	case 7: return "Saturday";
    }
    return NULL;
}
```
Yukarıda tanımlanan `get_day_name` işlevi bir günün sıra numarasını alarak ilgili günün ismini içeren bir yazının başlangıç adresini döndürüyor. İşleve gönderilen değer istenen aralıkta değil ise işlev `NULL` adresini döndürüyor.

## yazı sabitlerinin derleyici tarafından birleştirilmesi
Yazı sabitleri tek bir atom `(token)` olarak ele alınır. Bir yazı sabiti aşağıdaki gibi parçalanamaz:

```
void func()
{
        char *ptr =  "Necati Ergin'in C ders
        notlarını okuyorsunuz";	/* Geçersiz */
        /***/
}
```
Ancak yazıların uzunlukları arttıkça, hem onların bir yazı sabiti olarak tek bir satırda yazılması hem de kodda bu yazıların okunması zorlaşır. Kodlama editörlerinde bir satırlık görüntüye sığmayan yazılar kaynak kodun okunmasını zorlaştırır. Uzun yazılara ilişkin yazı sabitlerinin parçalanmasına olanak vermek amacıyla, `C` dilinde aralarında boşluk karakteri dışında başka bir karakter olmayan yazı sabitleri derleyici tarafından birleştirerek tek bir yazı sabiti olarak ele alınır. Örneğin:

```
ptr = "Necati Ergin'in C ders "

"notlarını okuyorsunuz";
```
geçerli bir ifadedir. Bu durumda derleyici iki yazı sabitini birleştirerek kodu aşağıdaki biçimde ele alır:

```
ptr = "Necati Ergin'in C ders notlarını okuyorsunuz";
```
Derleyicinin iki yazı sabitini birleştirmesi için, yazı sabitlerinin arasında boşluk karakterlerinin `(whitespace)` dışında hiçbir karakterin olmaması gerekir:

```
p = "Oguz" "Karan";
```
bildirimi ile

```
p = "OguzKaran";
```
bildirimi eşdeğerdir.

Yazı sabiti kapatılmadan bir ters bölü karakteri ile sonlandırılarak da sonraki satıra geçiş sağlanabilir. Örneğin:

```
ptr = "Necati Ergin'in C ders \
notlarını okuyorsunuz";
```
bildirimi ile

```
ptr = "Necati Ergin'in C ders notlarını okuyorsunuz";
```
bildirimi eşdeğerdir. Ters bölü karakterinden sonra yazı sabitinin karakterleri aşağıdaki satırın başından devam etmelidir. Söz konusu yazı sabiti aşağıdaki gibi yazılırsa:

```ptr = "Necati Ergin'in C ders \
         notlarını okuyorsunuz";
```
satır başındaki boşluk karakterleri de yazıya katılır. Sonuç aşağıdaki deyime eşdeğer olur:

```
ptr = "Necati Ergin'in C ders        notlarını okuyorsunuz";
```
## yazı sabitlerinde ters bölü karakter sabitlerinin kullanılması
Yazı sabitlerinde ters bölü karakter sabitleri de `(escape sequence)` kullanılabilir. Derleyiciler yazı sabitleri içinde bir ters bölü karakteri gördüğünde, onu yanındaki karakter ile birlikte tek bir karakter olarak ele alır. Örneğin:

```
char *p;
p = "Ali\tCan";
```
Yukarıdaki kodda yazı sabiti içinde kullanılan `\t` tek bir karakterdir `(9 numaralı ASCII karakteri olan tab karakteri)`.
Yani

```
printf("yazidaki karakter sayısı = %d\n", strlen(p));
```
deyimi ile ekrana

```
yazidaki karakter sayısı = 7
```
yazdırılır.
Yazı sabitlerinde doğrudan çift tırnak ya da ters bölü karakterleri kullanılamaz. Bu karakterlerin özel işlevleri vardır. Bir yazı sabiti içinde `"çift tırnak"` karakter sabitinin kendisini ifade etmek için çift tırnak karakterinden önce bir ters bölü karakteri kullanılır. Örneğin:

```
#include <stdio.h>

int main()
{
	puts("\"Kagan Aslan\"");
}
```
`main` işlevi içinde yapılan çağrı ile ekrana

`"Kagan Aslan"`

yazısı yazdırılır. Aşağıdaki koddaki hatayı görebiliyor musunuz?

```
#include <stdio.h>

int main()
{
	FILE *f = fopen("C:\necati\ali.txt", "r");
	//...
}
```
Yazı sabiti içinde yer alan ters bölü karakter sabitleri, onaltılık `(hexadecimal)`sayı sisteminde de ifade edilebilir. Aşağıdaki programı derleyerek çalıştırın:

```
#include <stdio.h>

int main()
{
	puts("\x41\x43\x41\x42\x41"); //ACABA
}
```
Yazı sabiti içinde yer alan ters bölü karakter değişmezleri sekizlik sayı sisteminde de ifade edilebilir. Aşağıdaki programı derleyerek çalıştırın:

```
#include <stdio.h>

int main()
{
	puts("\116\145\143\141\164\151"); //Necati
}
```
## boş yazı sabiti `(null string literal)`
Bir yazı sabiti bir boş yazıyı yani uzunluğu sıfır olan bir yazıyı ifade edebilir `(null string)`. Bu durum iki çift tırnak karakterinin arasına hiçbir karakter yazılmaması ile oluşturulur:

```
const char *ptr = "";
```
Yukarıdaki bildirim ile `ptr` gösterici değişkenine boş yazı ile ilk değer veriliyor. Boş yazı sabitleri de bellekte bir adres belirtir. Derleyici boş bir yazı sabiti gördüğünde, bellekte güvenilir bir yere yalnızca sonlandırıcı karakteri yerleştirir.
Boş bir yazı uzunluğu sıfır olan bir yazıdır. Aşağıdaki programı derleyerek çalıştırın:

```
#include <stdio.h>
#include <string.h>

int main()
{
	const char *ptr = "";

	printf("uzunluk = %d\n", strlen(ptr)); //0
}
```
## gösterici değişkenlere yazı sabitlerine ilk değer verilmesi
`char` türden gösterici değişkenlere yazı sabitleri ile ilk değer verilebilir. Örneğin:

```
char *p = "İstanbul";
char *err = "Bellek yetersiz";
char *s = "Devam etmek için bir tuşa basınız";
```
Yazı sabitleri derleyiciler tarafından `char` türden bir dizi adresi olarak ele alındığına göre, `char` türden gösterici değişkenlere yazı sabitleri ile ilk değer verilmesi doğal bir durumdur.
Dizilere çift tırnak içinde ilk değer verilmesiyle gösterici değişkenlere çift tırnak içinde ilk değer verilmesi tamamen farklıdır.
Gösterici değişkenlere yazı sabitleri ile ilk değer verildiğinde, derleyici yazı sabitini bir adres ifadesi olarak ele alır. Yani ilgili yazı belleğe yerleştirildikten sonra başlangıç adresi gösterici değişkene atanır. Oysa dizilerde önce dizi için yer ayrılır, daha sonra karakterler tek tek dizi elemanlarına yerleştirilir. Dizilere ilk değer verirken kullanılan çift tırnak ifadeleri adres bilgisine dönüştürülmez. Dizi elemanlarına tek tek `char` türden sabitlere ilk değer verme işlemi zahmetli olduğu için, programcının işini kolaylaştırmak amacı ile böyle bir ilk değer verme kuralı getirilmiştir.

Bir `char` diziye ilk değer vermek ile, gösterici değişkenlere yazı sabitlerine ilk değer vermek arasındaki farka dikkat etmek gerekir:

```
char s[10] = "Deneme";
```

bildirimi 

```
char s[10] = {'D', 'e', 'n', 'e', 'm', 'e', '\0'};
```
ile aynı anlamdadır.

C'de geleneksel olarak yazı sabitlerinin `char *` türden gösterici değişkenlere atanması ya da ilk değer olarak verilmesi doğru kabul edilir. Ancak `C++` dilinde bu durum sentaks hatasıdır. Yazı sabitleri salt okunur amaçlı kullanılacak yazılar olduğuna göre bunları gösterecek göstericilerin de yalnızca okuma amaçlı olarak kullanılacak, yazma işlemine izin vermeyecek göstericiler olması daha doğrudur. Kodlama hatalarına karşı yazı sabitlerinin `char *` türünden değil `const char *` türünden göstericilere atanması daha doğrudur.

Yazı tutan `char` türden diziler ile bir yazı sabitini gösteren `char` türden göstericilerin karşılaştırılması
`C` dilinde bir yazı bilgisi en az iki ayrı biçimde saklanabilir:

1. Bir yazı `char` türden bir dizi içinde tutulabilir:

```
#include <stdio.h>
#include <string.h>

void foo()
{
	char s1[100] = "Ali Serce";
	char s2[100];
	char s3[100];

	printf("bir yazı giriniz: ");
	fgets(s2, 100, stdin);		
	strcpy(s3, s2);
	/***/
}
```
Yukarıdaki kodda tanımlanan `foo` işlevinde `char` türden `s1` dizisine `Ali Serce` yazısı ile ilk değer veriliyor. Yine `char` türden `s2` dizisine ise standart `fgets` işlevi ile standart giriş akımından bir yazı alınıyor. `s2` dizisindeki yazı, standart `strcpy` işlevi ile `s3` dizisine kopyalanıyor.

2. Yazı doğrudan bir yazı sabiti olarak kullanılır. Ya da `char` türden bir gösterici değişkenin bir yazı sabitine ilişkin yazıyı göstermesi sağlanır:

```
const char *ptr = "Necati Ergin";
```
İki yöntem birbirinin tamamen eşdeğeri değildir. Aşağıdaki noktalara dikkat edilmesi gerekir:
Yazı sabitleri olarak kullanılan yazılar statik ömürlü varlıklar oldukları için programın sonlanmasına kadar bellekte yer kaplarlar. Bir gösterici değişkenin bir yazı sabitine ilişkin yazıyı gösterirken, daha sonra başka bir yazıyı gösterir duruma getirilmesi, daha önceki yazının bellekten boşaltılacağı anlamına gelmez:

```
const char *p = "Bu yazı programın sonuna kadar bellekte kalacak.";
p = "artık yukarıdaki yazı ile bir bağlantı kalmayacak...";
```
Statik ömürlü varlıkların saklanacağı bellek alanının kısıtlı olduğu sistemlerde, yazı sabitlerinin kullanımına dikkat etmek gerekir.

Yazının `(const olmayan) char` türden bir dizi içinde tutulması durumunda bu yazıyı değiştirmek mümkündür. Dizi elemanlarına yeniden atamalar yapılarak yazı istenildiği gibi değiştirilebilir. Ama yazı sabitlerine ilişkin yazıların değiştirilmesi tanımsız davranış özelliği gösterir, bir hatadır.
`char` türden bir dizi otomatik ömürlü ya da statik ömürlü olabilir. Yani yazının bellekte kalma süresi dizinin ömrünün otomatik ya da statik ömürlü olmasına göre farklılık gösterir.

## strlen işlevi ve sizeof operatörü
Yazılar söz konusu olduğunda `sizeof` operatörü ile `strlen` işlevinin birbiriyle karıştırılması sık karşılaşılan bir durumdur. `sizeof` derleme zamanında ele alınan bir operatördür. Yani `sizeof` operatörünün ürettiği `size_t` türünden değer derleyici tarafından derleme zamanında elde edilir. `sizeof`'un bir isim `(identifier)` olmadığını bir anahtar sözcük `(keyword)` olduğunu biliyorsunuz.

```
sizeof("necati")
```
ifadesi `size_t` türündendir ve bu ifadenin değeri derleyicinin `"necati"` yazısını yerleştirdiği dizinin boyutu yani `7`'dir.
Oysa `strlen` `C`’nin standart bir işlevidir. Şüphesiz bu işlevin `size_t` türünden geri dönüş değeri programın çalışması sırasında elde edilir. `strlen` işlevinin geri dönüş değeri, adresi alınan yazının uzunluğudur:

```
strlen("necati")
```
ifadesinin `size_t` türünden olan değeri `"necati"`, yazısının uzunluğu, yani `6`'dır.

## yazı sabitleri nerelerde kullanılmalı
Programın çalışma zamanında dinamik olarak içeriği değişmeyen, kaynak kod içinde salt okuma amacıyla kullanılacak yazıların yazı sabitleri ile ifade edilmesi kaynak kodun daha kolay okunmasını ve anlaşılmasını sağlar. Aşağıdaki örneği inceleyin:

```
#include <stdio.h>
#include <string.h>

#define MAX_NAME_LEN	80	

int main()
{
	char fileName[MAX_NAME_LEN];
	char newFileName[MAX_NAME_LEN];
	char *ptr;

	printf("dosya ismini giriniz: ");
	fgets(fileName, MAX_NAME_LEN, stdin);
	if ((ptr = strchr(fileName, '\n')) != NULL)
		*ptr = '\0';

	strcpy(newFileName, fileName);

	if ((ptr = strrchr(newFileName, '.')) == NULL)
		strcat(newFileName, ".dat");
	else if (!strcmp(ptr, ".doc"))
		strcpy(ptr, ".txt");
	else if (!strcmp(ptr, ".gif"))
		strcpy(ptr, ".jpg");
	else if (!strcmp(ptr, ".exe"))
		*ptr = '\0';
	else
		strcpy(ptr, ".nec");
	printf("(%s) -----> (%s)\n", fileName, newFileName);
}
```
Yukarıdaki `main` işlevinde `fileName` isimli diziye standart giriş akımından bir dosya ismi alınıyor. Eger standart giriş akımından alınıp diziye kopyalanmış bir `'\n'` karakteri var ise bu karakterin yeri `strchr` işlevi ile ile bulunuyor ve bu karakterin yerine sonlandırıcı karakter yazılıyor. Böylece yazının sonundaki `'\n'` karakteri silinmiş oluyor. Diziye alınan isim daha sonra standart `strcpy` işleviyle newFileName isimli diziye kopyalanıyor.

Standart `strrchr` işleviyle, kopyalanan yazı içinde `'.'` karakterinin olup olmadığı, yani dosya isminin bir uzantısı olup olmadığı sınanıyor. Eğer ismin içinde nokta karakteri varsa bu karakterin adresi `ptr` isimli gösterici değişkende tutuluyor. Dosya isminin uzantısı yoksa, standart `strcat` işleviyle yazının sonuna `".dat"` yazısı ekleniyor. Bu durumda artık dosya isminin uzantısı `"dat"` olur, değil mi? Daha sonra standart `strcmp` işleviyle, dosya ismi uzantısının `"doc", "gif"` ya da `"exe"` olup olmadığı sınanıyor. Dosyanın uzantısı `"doc"` ise bu uzantı `"txt"`, `"gif` "ise uzantı `"jpg"` olarak değiştiriliyor. Eğer dosya uzantısı `"exe"` ise, dosyanın uzantısı siliniyor. Bunun dışındaki tüm durumlarda dosya uzantıları, `"nec"` yapılıyor.

Tüm bu işlemlerin yapılmasında okuma amaçlı yazılar için yazı sabitlerinin kullanıldığını görüyorsunuz. Bu amaçla yazı sabitlerinin kullanılması yerine `char` türden diziler kullanılsaydı programın algısal karmaşıklığı artar, okuyanlar hangi yazıların kullanıldığını kolayca göremezlerdi.
