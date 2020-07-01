## _Bool Türü

`C` dilini öğrenenlerin kafasını karıştıran noktalardan biri mantıksal `(lojik)` veri türü. `C99` standartlarına kadar `C`'de bir mantıksal veri türü yoktu. Mantıksal veri türü yerine işaretli `int` türünün kullanılması son derece doğaldı.  Halen de büyük ölçüde durum böyle.

`C99` standartlarından önce yazılan kodlarda dilde `bool` türü olmadığından lojik veri türü kullanımını vurgulamak için farklı teknikler kullanılıyordu. Önişlemci makrolarını kullanmak bu tekniklerden biriydi:

```
#define bool		int
#define true		(1)
#define false		(0)
```

Bir başka teknik de `bool`, `Bool`, `BOOL` vs. sözcüklerinden birini bir türün eş ismi `(type alias)` olarak bildirmekti:

```
typedef int Bool;
```
Mantıksal veri türü olarak bir yapı `(structur)` türü de seçilebiliyordu:

```
typedef enum {False, True} Bool;
```
`C99` standartları ile `C` diline yeni bir işaretsiz tam sayı türü olarak `_Bool` eklendi.

`_Bool` bir `makro` ya da bir `typedef` ismi değil, bir anahtar sözcük. `C99` standartları `C89` standartlarında `32` olan anahtar sözcük sayısını aşağıdaki anahtar sözcükleri de dile ekleyerek `37`'ye çıkarttı:

```
restrict, inline, _Bool , _Complex, _Imaginary
```
`C99` standartları `_Bool` veri türü hakkında şunları söylüyor:

> _Bool, 0 ve 1 değerlerini tutabilecek büyüklükte bir tamsayı türü ("An object declared as type _Bool is large enough to store the values 0 and 1.").

> _Bool işaretsiz bir tamsayı türü ("The type _Bool and the unsigned integer types that correspond to the standard signed integer types are the standard unsigned integer types.").

Tür dönüşümlerinde `_Bool` türü tamsayı türleri arasında en düşük dereceye `(rank)` sahip.
> "The rank of _Bool shall be less than the rank of all other standard integer types."

`_Bool` türü işaretsiz bir tam sayı türü. Diğer aritmetik türlerden bu türe de doğal olarak tür dönüşümü `(type conversion)` var. `C99` standartları ile dile bu türün eklenmesi dilin kurallarında bir değişikliğe yol açmadı. `C` dilinde karşılaştırma operatörleri ve mantıksal operatörler yine işaretli `int` türden `1` ve `0` değerlerini üretiyorlar.

Standartların `bool` yerine anahtar sözcük tercihini `_Bool`'dan yana kullanmış olmasının nedeni geçmişte yazılmış kodların geçerliliğini korumak. Bu anahtar sözcük dile eklenmeden birçok `C` kodunda `bool` sözcüğü bir önişlemci `(preprocessor)` makrosu olarak tanımlamış ya da bir `typedef` bildirimiyle bir türün eş ismi `(type alias)` olarak bildirilmişti. Eğer `bool` standartlar ile bir anahtar sözcük yapılsaydı bu tür kodların hepsi yeni derleyiciler ile tekrar derlendiğinde sentaks hatası oluşurdu.

`C99` standartları ile dile eklenen standart başlık dosyalarından biri de `stdbool.h`. Bu başlık dosyasında tanımlanan `4` makro var:

```
#define		bool							_Bool
#define		true 							(1)
#define		false							(0)
#define		__bool_true_false_are_defined	                         1
```

Eğer bir isim çakışması söz konusu olmayacak ise bu başlık dosyasını kod dosyamıza dahil ederek `bool`, `true` ve `false` sözcüklerini kullanabiliriz. Başlık dosyasında yer alan `"__bool_true_false_are_defined"` makrosu ise koşullu derleme `(conditional compiling)` işlemlerine yönelik.

20 yıldır `C` dilinde `bool` türünün olmasına karşın bazı `C` programcılarının yazdıkları kodlarda bu türü hiç kullanmamalarının tipik nedenleri şunlar:
`
+ `_Bool` türünün dile eklenmesinden önce lojik veri `int` türü ile temsil ediliyordu. `0` değeri false olarak kullanılıyor `0`'dan farklı tüm değerler `true` olarak ele alınıyordu. Örneğin standart `ctype` kütüphanesindeki karakter test işlevlerinin hepsinin geri dönüş değeri `int` türden. Birçok programcı halen bu geleneği devam ettiriyor. Standart kütüphane ile uyum sağlamak için birçok `C` kodunda mantıksal veri türü yerine halen işaretli `int` türü kullanılıyor.

+ Programcının alışkanlıklarının dışına çıkmamak için `_Bool` türünü kullanmıyor olması.
+ Programcının`C99` standartları ile `_Bool` türünün dile eklenmiş olduğunu bilmiyor olması.
+ Programcının `C99` standartlarını desteklemeyen bir derleyici kullanıyor olması. C99 standartlarını desteklemeyen birçok C derleyicisi halen endüstride aktif olarak kullanılıyor.
+ Programcının `stdbool.h` dosyasını koda dahil etme zahmetine katlanmaması (klavye tembelliği).

Eğer bu nedenlerden biri söz konusu değil ise `_Bool` türünü ya da `<stdbool.h>` başlık dosyasındaki `bool` makrosunu kullanmakta fayda var.
