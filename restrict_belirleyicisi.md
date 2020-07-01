## restrict Belirleyicisi (restrict specifier)

Bir nesneyi (bir bellek alanını) programın çalışma zamanında birden fazla gösterici değişken gösterebilir. Bu duruma İngilizcede `"pointer aliasing"` terimi karşılığı "gösterici örtüşmesi" diyeceğim.  Aşağıdaki koda bakalım:

```
#include <stdio.h>

void copy(int *pdest, const int *psource, size_t n)
{
	while (n--) {
		*pdest++ = *psource++;
	}
}
```

`copy` fonksiyonuna yapılan çağrıda fonksiyonun her iki parametresine de aynı diziye ilişkin adresler gelmiş olabilir. Eğer farklı göstericilerle yapılan işlemlerden en az birisi aynı nesneyi değiştirmeye yönelik ise derleyici bu nesne üzerinde yapılacak erişim işlemlerinin sıralamasını genellikle değiştiremez. Bir gösterici tarafından erişilen bir nesneye bir başka gösterici değişken ile de erişilebilmesi ihtimali derleyicilerin optimizasyon olanaklarını kısıtlar. Başta GCC derleyicisi olmak üzere birçok derleyici derleme zamanında yaptığı analzilerle gösterici örtüşmesi olup olmadığını saptamaya çalışırlar.

`C99` Standartları ile C diline eklenen `restrict` belirteci gösterici `(pointer`) değişkenlerin bildirimlerinde ya da tanımında kullanılabilir:

```   
void func(int *restrict p1, int *restrict p2);

```

`restrict` anahtar sözcüğü derleyiciye söz konusu gösterici değişkenin gösterdiği nesneye erişimi sağlayabilecek tek gösterici değişken olduğu bilgisini vermektedir. Derleyici bu bilgiye dayanarak kodu daha iyi optimize edebilir.

`restrict` anahtar sözcüğü doğrudan bildirilen ya da tanımlanan gösterici değişkeni nitelediği için bildirimde `*` (asterisk) atomundan sonra gelmeli:
```
void f1 (int * restrict p); //geçerli
void f2 (int restrict * p); //geçersiz
```

### restrict anahtar sözcüğünün kullanıldığı temalar

+ Boyutu programın çalışma zamanında belli olan dizileri gösterecek global değişkenler `restrict` anahtar sözcüğü ile tanımlanabilir. Derleyici böyle değişkenlerin bu dizilere erişmenin tek yolu olduğunu varsayarak dizi elemanlarına erişen kodları çok daha iyi optimize edebilir. Aşağıdaki kodda tanımlanan `init` fonksiyonu ile oluşturulan dinamik dizi tanımlanan global `gp1` ve `gp2` gösterici değişkenleri vasıtasıyla iki ayrı diziymiş gibi kullanılıyor:

```
double * restrict gp1, * restrict gp2;

void init(size_t n)
{
	double *pd = (double *)malloc(2 * n * sizeof(double));
	if (! pd) {
		printf("bellek yetersiz");
		exit(EXIT_FAILURE);
	}
	gp1 = pd;  // gp1 dinamik dizinin ilk yarisini gösteriyor
	gp2 = pd + n;  // gp2 dinamik dizinin ikinci yarisini gosteriyor
}
```

+ Bazı fonksiyonların parametre değişkenlerinin `restrict` anahtar sözcüğüyle tanımlanması derleyicinin daha etkin bir kod üretmesini sağlayabilir. Aşağıdaki kodu inceleyelim:

```
int ga[200];
int *gp;

void foo(int n, int* restrict pdest, const int *psource)
{
	int i;
	for (i = 0; i < n; i++)
		pdest[i] = psource[i] + gp[i];
}

int main()
{
	int ar1[200];
	int ar2[200];
	gp = ga; 
	
	foo(200, ar1, ar2);	 // [1] tanımlı davranış
	foo(50, ar1, ar1 + 50);	 // [2] tanımlı davranış
	foo(99, ar1 + 1, ar1);   // [3] tanımsız davranış
	gp = ar1; 
	foo(99, ar1 + 1, ar2);	 // [4] tanımsız davranış
	foo(99, ar2, ar1 + 1);	 // [5] tanımlı davranış
}
```

`main` fonksiyonu içinde `foo` fonksiyonuna yapılan 3. ve 4. çağrıda gösterici örtüşmesi `restrict` anahtar sözcüğü ile yapılan bildirime ters düştüğü için tanımsız davranış söz konusudur.
