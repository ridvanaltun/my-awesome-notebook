# C Language

Bu doküman C dili hakkında hatırlatıcı ve pratik bilgiler vermektir.

# Why C Language

Bu dilde bellek yönetimini biz yapmak zorundayız, çoğu dilde bellek yönetimi otomatik olarak yapılır. Bunun yanında C dilinin nesne tabanlı programlama için uygun bir yapısı yok.

# Where Is C Language Used?

Gömülü sistemler, driver yazılımları ve işletim sistemlerinin çekirdekleri bu dil ile geliştiriliyor. Kişisel tahminim donanım programlarken objeye yönetik programlamaya ihtiyaç duyulmaması, bu sebeple dili değiştirecek bir eksiklik bulunmaması. Keza driver yazımı içinde aynı durum geçerli.

# C Language Standarts

C dili yaygınlaşmaya başlayınca ortaya bir problem çıktı, başkasının yazdığı kod diğer sistemler ile uyumsuz olabiliyordu, bu problemden ötürü taşınabilir programların yazılması zorlaşıyordu. Bu sebeple C diline bir standart getirmek gerekiyordu.

Derleyici programları C Standartlarını sağlayacak şekilde yazılırlar. İnternet'te ufak bir aramayla kullandığımız derleyici programın sürümü hangi C standardını kullanıyor öğrenmek mümkün.

## Evolution 

`C89`: 1989 yılında [ANSI](http://ansi.org/) tarafından yapılmış.

`C90`: ISO tarafından duyurulmuş. Bir kaç teknik hata düzeltmesi dışında C90 ile C89 neredeyse aynı. ANSI resmi olarak bu standardı benimsemiş.

`C95`: C90 üstüne bir kaç yeni özellik eklendi. Ağırlıklı olarak "digraphs" ve "wide character" özellikleri eklendi.

`C99`: (ISO) tarafından duyuruldu, 3 farklı teknik hata düzeltmesi içeriyor. Bu sürümdeki en büyük fark önceki sürümlere göre `//` şeklinde yorum satırına izin veriyor oluşu, öncesinde sadece `/* */` şeklinde yorum yapabiliyorduk.

`C11`: 2011 yılında ISO tarafından duyuruldu. Sadece bir teknik düzeltme içeriyor, "__STDC_VERSION__" ve "__STDC_LIB_EXT1__" tanımları değiştiriliyor.

## Other C Language Standarts

Aslında C dilinin kullanıldığı sektöre göre bir çok standardı mevcut. Bu extra standartların genelde amacı dilin bazı özelliklerinden feragat edip daha güvenli bir dil yaratabilmek.

### Embedded C

Gömülü sistemler için kod yazarken kullandığımız standart.

### MISRA C

Askeri, uçak ve otomotiv sanayinde kullanılan C dili standardı. Bu standart dinamik bellek yönetimini yasaklar.

## Which Standard Does the Compiler Use?

Kullandığımız derleyici varsayılan olarak hangi C sili standardını kullanıyor öğrenmek için küçük bir test programı kullanabiliriz.

```c
// test.c
#include <stdio.h>

int main(void) {
#ifdef __STDC_VERSION__
    printf("__STDC_VERSION__ = %ld \n", __STDC_VERSION__);
#endif
#ifdef __STRICT_ANSI__
    puts("__STRICT_ANSI__");
#endif
    return 0;
}
```

Aşağıdaki bash script ile test edebiliriz.

```bash
#!/usr/bin/env bash
for std in c89 c99 c11 c17 gnu89 gnu99 gnu11 gnu17; do
  echo $std
  gcc -std=$std -o test test.c
  ./test
  echo
done
echo default
gcc -o test test.c
./c.out
```

Çıktı:

```bash
# c89
__STRICT_ANSI__

# c99
__STDC_VERSION__ = 199901
__STRICT_ANSI__

# c11
__STDC_VERSION__ = 201112
__STRICT_ANSI__

# c17
__STDC_VERSION__ = 201710
__STRICT_ANSI__

# gnu89

# gnu99
__STDC_VERSION__ = 199901

# gnu11
__STDC_VERSION__ = 201112

# gnu17
__STDC_VERSION__ = 201710

# default
__STDC_VERSION__ = 201710
```

Anlaşıldığı üzere varsayılan olarak GCC derleyicimiz gnu17 sürümünü kullanıyor.

# C vs C++

TODO

# Development Environment

Malum, C derlenen bir dil, geliştirme ortamı için bir derleyici programına ihtiyaç duyuyoruz.

## Compiler

Popüler olan `gcc` yada `clang` derleyici programlarını kullanabiliriz.

### How Does the Compiler Work?

- Ön işlem (preprocessing)
- Derleme, Assembly Koduna Çevirme (compling)
- Makine Koduna Çevirme (assembling)
- Linkleme (linking)

Ön işlem sırasında program içinde # ile belirttiğimiz parçalar yani `derleyici direktifleri` çalıştırılır. #include, #define gibi...

Linklemenin yegane amacı derleme süresini kısaltmak. Derleyici program, eğer yazdığımız projede değiştirilmemiş bir dosya varsa (kütüphane dosyaları örn. stdio.h) bu dosyayı tekrar makine koduna yani 0 ve 1'lere çevirmek yerine önceden makine koduna çevirdiği dosyayı yedeklediği yerden çağırıp değiştirilmiş olan dosyanın makine kodları ile birleştiriyor. Bu sebeple linkleme işlemi en son adımda yapılır. Linkleme sayesinde derleyici hızlı çalışır.

### Adding a Library to The Compiler

TODO

### Compiler Directives

C ile alakası olmayan, daha kolay program yazmamızı sağlayan bir özellik.

```c
// MAX_LENGTH is 55
#define MAX_LENGTH 55

// 
#ifndef __ANY_NAME_H__
// ...
#endif

// Sistemdeki tanımlı kütüphaneleri çağırabiliyoruz.
#include <stdio.h>

// Dosya dizinindeki diğer kütüphaneleri ekleyebiliyoruz.
#include "src/file.h"
```

### GCC Usage

GCC içinde bir compiler ve linker özelliği bulunduruyor. Compile etmek demek C kodumuzu object dosyasına çevirmek demek yani derleme sonrası elimizde .o uzantılı bir dosya olması demek. Link demek ise object dosyamızı executable yani çalışabilir bir program haline getirmek demek. GCC programı ile kullandığımız Flag'ler ile object dosyası yada linklenmiş bir dosya çıktısı alabiliyoruz.

#### General Purpose

```bash
# yazdığımız program abc olarak çıktı verir, diğer türlü işletim sistemimize göre .exe yada .out gibi çıktılar verir derleyiciler
-o abc
```

#### Compiling and Linking

```bash
# Tek satırda programı compile ve link etmek, yani çalıştırılabilir program elde etmek
gcc file1.c file2.c -o foo

# Programımızı çalıştırabiliyoruz
./foo

# Üç farklı C programını obje'ye çeviriyoruz
gcc -c foo1.c foo2.c foo3.c

# Gerekli objeleri bar adında bir dosyaya linkliyoruz
gcc foo1.o foo2.o foo3.o -o bar

# Programı çalıştırabiliyoruz
./bar
```

#### Assembly

```bash
# C kodunu assembly olarak çıktı verir
gcc file.c -S -o file.S

# Assembly kodunu derler
gcc file.S -o foo

# Programı çalıştırabiliyoruz
./foo

# Normal derleme sırasında .s uzantılı assembly dosyalarını kaydeder
gcc -save-temps -o test test.c
```

#### Compiler C Language Standart Selection

```bash
# First Offical C Standart
-ansi -pedantic
-std=c90 -pedantic 
-std=c89 -pedantic
-std=iso9899:1990 -pedantic

# Supports the C90 standard plus the 1995 amendment, which added a few minor features (all of which are also in C99)
-std=iso9899:199409 -pedantic

# C99 standartı tam olarak bitmedi: https://gcc.gnu.org/c99status.html
-std=c99 -pedantic
-std=c9x -pedantic
-std=iso9899:1999 -pedantic

# C11 Standartı tam olarak bitmedi: https://gcc.gnu.org/wiki/C11Status
-std=c11 -pedantic
-std=c0x -pedantic
-std=iso9899:2011 -pedantic

# Standartta yapılan kısaltmaları ve söz dizimimlerini yazdırır
-pedantic

# -pedantic yerine kullanılabilir, program sadece fatal error verince print et
-pedantic-errors
```

#### Static Library Usage

C Statik kütüphaneleri Unix Sistemlerinde "/lib" ve "/usr/lib" altında tutulur.

```bash
# -l parametresi ile ile /lib ve /usr/lib altındaki pthread adındaki statik kütüphaneyi 
# projemiz ile linklenmesini sağladık
gcc sample.c -o sample -l pthread
```
-l ile belirtilen dosya /lib veya /usr/lib altında varsa eğer ilk bu konumdaki dosya kullanılır.
Yoksa eğer belirttiğimiz hedef dosya kullanılır.

```bash
# güncel konumda bulunan test isimli statik kütüphaneyi projeye linkle
gcc -o  sample sample.c -L . -l test
```

#### Dynamic Library Usage

```bash
# /lib veya /usr/lib konumundaki dinamik bir dosyayı kullan
gcc sample.c -o sample libtest.so

# konumda bulunan dinamik bir kütüphaneyi kullan
gcc -o sample sample -L . -l mylib
```

### What is Optimization?

Derleyiciler kodumuzun daha hızlı çalışması ve bellekte daha az yer kaplaması için otomatik optimize eder.

```bash
# güvenli bir şekilde optimize et
gcc -O2 -o test test.c

# optimize etmeyi kapat
gcc -O0 -o test test.c
```

Optimizasyon temelli sorunlardan kurtulmak için kodu tamamen optimizasyona kapatmak akıllıca bir çözüm değildir. Sorunu çözmek için problemin kaynağı nedir bunu anlamak gerekiyor. Anladığım kadarıyla bu sorun değişkenlerin derleyici tarafından optimizasyon işlemi ile yanlış organize edilmesi sonucuyla oluşuyor.

Sorunu aşmak için `volatile` deyimi kullanmak gerekiyor. Daha farklı çözümler de mevcut, ancak `best practice` bu deyimi kullanmak.

## Debugger

Karşımıza çıkan hataları bulmanın en kolay yolu genelde programımıza `printf` satırları eklemek ve değerleri takip etmektir. Kompleks programlarda `printf` kullanımı ve hata tespiti zorlaşıyor, bu nedenle `debugger` adlı programlara ihtiyaç duyuyoruz.

C dili için `gdb` adlı programı kullanabiliriz.

### GDB Usage

TODO

## Text Editor: Sublime Text 3

Ben kod geliştirirken `Sublime Text 3` editörünü kullanıyorum çünkü bu editör eklentiler ile kişiselleştirilebiliyor, hafif bir `IDE` haline getirilebiliyor. Başlık altında C dili için `Sublime Text 3` editörü nasıl ayarlanır, hangi eklentiler yüklenir gibi sorulara cevap veriyor olacağım.

### Settings

TODO

#### Converting Tabs Characters to Space

> Preferences > Settings > "translate_tabs_to_spaces": true

#### Limiting Column Size

> View > Word Wrap Column > 80
> View > Word Wrap

Veya bunun yerine Ruller özelliği kullanılabilir.

> View > Ruler > 80

### Plugins

Daha okunabilir ve hızlı bir şekilde C kodu yazabilmek için edinebileceğimiz eklentiler.

#### SublimeLinter and SublimeLinter-gcc

SublimeLinter eklentisi ile C ve C++ dilinde yazılmış kodları `flake8` ve `pep257` standartları kullanarak yazıyoruz. Bu işi yapabilmek için SublimeLinter için yazılmış olan `SublimeLinter-gcc` adlı eklentisiyi ekstra olarak kurmamız gerekiyor.

#### EditorConfig

Bu eklenti C dilinden bağımsız. C projemize `.editorconfig` dosyası ekleyip editörün tab yerine space kullanması, kolon limiti gibi değerleri bir dosya ile diğer editörlere diretebiliyoruz. Takım çalışması yaparken kullanılabilir.

# Returns Value from Main Function

Main fonksiyonu boşuna integer bir değer döndürmüyor, integer değer döndürmesinde ki amaç programın hata veya başarılı bir şekilde sonlanıp sonlanmadığını anlayabilmek.

Main içinde eğer bir return kullanmazsak derleyici otomatik olarak `return 0` deyimini kullanıyor biz görmesek de, eğer biz bir return değeri koyup değer döndürürsek program biter. Döndürdüğümüz main değerine `echo $?` komutu ile komut satırından görebiliriz. Yani kısacası ileride gelişmiş programlar yazarken ana programımızdan bir kod döndürüp ikincil vs. bir program ile bu kodu okutmamız mümkün hale geliyor.

# C Language Features

TODO

## Array Definitions

```c
// Burada foo değişkeni 3 + 1 elemanlı, 0, 1, 2 veri girişi için ayrılmış, 3. hücrede \0 karakterini barındırıyor.
int foo[3];

// Basit bir dizi tanımı.
int foo[] = {5, 4, 2};
```

## Array-Memory Relation

Dizinin en sonuna bir byte `00000000` veya `\0` şeklinde bir bitiş byte'ı yerleştiriliyor. Bu bilgiye dayanarak algoritmalar geliştirmek mümkün, ders içinde `strlen()` fonksiyonu bu özellik kullanılarak basit bir şekilde yazılmıştı.

Dizi oluştururken `int foo[3]` demek aslında 4 elemanlı, son elemanı `\0` olan bir değişken yarat demek.

## Null, False, 0, 0x0

Bu 4 ifade aynı anlama geliyor.

```c
// Eğer s null vs. ise if çalışır, bu şekilde daha iyi kod yazmış oluruz.
if (!S) {
    // logic..
}
```

## Data Types

Veri tiplerini iki ayrı başlıkta incelemek mümkün; 

- Temel veri tipleri
- Özel veri tipleri.

### General Data Types

TODO

### Custom Data Types

TODO

## Getting Arguments from Console

`argc` = argument count
`argv` = argument vector

```c
int main(int argc, char *argv[])
{
    // logic..
}
```

Eğer programımızı `.\test` şeklinde çalıştırılırsa; `argv[0]` değeri `.\test` değerini taşır.

## Prototype

Programımız içinde kullandığımız fonksiyonlar her zaman main fonksiyonunun üstünde tanımlanmak zorunda. Eğer aşağıda tanımlama yaparsak derleyici bu tanımlamayı görmüyor ve fonksiyonu bulamadım diyor. Main fonksiyonunu üstte tutmak için `Prototype` özelliğini kullanmamız gerekiyor.

```c
// HATALI KOD
int main()
{
    foo(5);
}

void foo(int n)
{
    // logic..
}
```

```c
// DOĞRU KOD: Prototype Özelliği Burada Kullanılmış
void foo(int n);

int main()
{
    foo(5);
}

void foo(int n)
{
    // logic..
}
```

```c
// DOĞRU AMA ÇİRKİN KOD
void foo(int n)
{
    // logic..
}

int main()
{
    foo(5);
}
```

## Custom Data Types: Typedef and Struct

C dili özünde nesne tabanlı programlamayı desteklemiyor, `struct` yapısı ile takla atarak nesne tabanlı programlamaya elverişli hale getirebiliyoruz. Nesne tabanlı programlamayı örnekleyecek olursak, yüz tane öğrencinin bilgisini alacak olalım, normalde öğrencilerin bilgilerini değişik diziler içinde toplamamız gerekir, programın tasarımını kötüleştiren bir detay bu.

Struct yapısı ile kendi veri tipimizi yaratıyoruz, örneğin öğrenci nesnesi yaratabiliyoruz.
Struct yapımızı bir header dosyasında tutup gerektiğinde projelere include ediyoruz.

```c
// struct.h
typdef struct
{
    char * name;
    char * dorm;
} 
student;
```

```c
// program.c
include <cs50.h>
include <stdio.h>
include "struct.h"

// 50 tane öğrenci oluşturduk.
student students[50];

students[0].name = "Ali";
```

C dilinde `typedef` kelimesi ile int gibi char gibi bir `data type` oluşturabiliriz. Ayrıca olan bir veri tipini değiştirebiliriz.

```c
// byte adlı bir data type oluşturduk
typedef unsigned char byte; 

// cs50 kütüphanesinde string data tipinin karşılığı
typedef char* string;

// car adında bir data tipi oluşturduk
struct car
{
    int year;
    char model[10];
    char plate[7];
    int odometer;
    double engine_size;
};

typedef struct car car_t;

// yada aynı şeyi aşağıdaki gibi tanımlayabiliriz

typedef struct car
{
    int year;
    char model[10];
    char plate[7];
    int odometer;
    double engine_size;
}
car_t;

// yeni tipimizle nesnemizi oluşturduk
car_t mycar;

// nesnemizin özelliklerini değiştirdik
mycar.year = 2015;
mycar.plate = "cs50";
mycar.odometer = 50505;

```

## Bitwise Operators

Bu özellik genelde mikro denetleyici programlarken kullanılıyor. Mikro denetleyicilerin `Register`'larını programlarken işi yarar bir özellik kendisi. Yüksek seviyeli dillerde de bu özellik mevcut olabilir, örneğin Python dilinde de bu özellik var ancak bu özelliğin en işe yarar olduğu dile C dilidir. Bunun sebebi mikro denetleyici programlarken C dilinin tercih edilmesidir.

## Pointers

Pointer değişkenler bellek adresi tutarlar.
Bu sebepler Pointer'a bir değer atarken sadece bellek adresi atabiliriz.

```c
// integer pointer
int *foo;

// char pointer
char *bar;

int alpha = 10; // ex. 0x1000000
char beta = 'x'; // ex. 0x2000000

printf("size of integer is %d\n", sizeof(int)); // returns 4
printf("size of char is %d\n", sizeof(char)); // returns 1

// alpha değerinin bellekteki adresini pointer'ımıza kaydettik
foo = &alpha;
bar = &beta;

// Print Adress of Pointers
printf("foo adress is %d\n", foo); // returns 0x1000000
printf("bar adress is %d\n", bar); // returns 0x2000000

foo++;
bar++;

// Print Adress of Pointers
printf("foo adress is %d\n", foo); // returns 0x1000004
printf("bar adress is %d\n", bar); // returns 0x2000001
```

```c
// Simple Example
#include <stdio.h>

int main(void)
{
    int i;
    int a[5] = {1, 2, 3, 4, 5};
    int *p = a;     // same as int*p = &a[0]
    for (i = 0; i < 5; i++)
    {
        printf("%d", *p);
        p++;
    }
    
    return 0;
}
```

### Pointing A Pointer To Another Pointer And Pointer Star Definition

Eğer bir değişkenin adresini göstermek istiyorsak bir yıldız. Bir Pointer'ın adresini göstermek istiyorsak iki yıldız, bir Pointer'ın Pointer'ını göstermek istiyorsak üç yıldız.. şeklinde gidiyor.

Birbirini gösteren Pointer yapısı dinamik bellek ile birlikte sıkça kullanılıyor.

```c
#include<stdio.h>

int main(void)
{
    int r = 50;
    int *p;
    int **k;
    int ***m;
    printf( "r: %d\n", r );
    p = &r;
    k = &p;
    m = &k;
    ***m = 100;
    printf( "r: %d\n", r );
    
    return 0;
}
```

Yukarıdaki kodu [bu resim](http://www.cagataycebi.com/programming/c_programming/c_operands/sm_pointer_to_pointer.png) iyi tarih ediyor. 

### Fear of pointers: What is the output of the following C program?

Pointer'ları anlamak için güzel bir soru, kendisini Linkedin üstünden buldum.

```c
#include <stdio.h>

int a[] = { -1, -2, -3, -4 };
int b[] = { 0, 1, 2, 3 };

int main()
{
    int *p[] = { a, b };
    int **pp = p;
    ++pp;
    ++*pp;
    ++**pp;

    printf("%d\n", (++**pp)[a]);
}

// a) it is a compilation error
// b) it is unspecified / implementation defined
// c) it is undefined behavior
// d) the output is: 
```

Cevap:

```c
int *p[] = {a, b}; // a ve b arraylerinin ilk elemanlarinin adreslerini tutan bir array

int **pp =p; // yukaridaki p arrayinin ilk elemaninin adresini tutan bir pointer (su anda a arrayinin ilk elemaninin adresini tutan yeri gosteriyor)

++pp; // su anda b arrayinin ilk elemaninin adresini  tutan yeri gostermeye basladi.

++*pp; // ++ daha oncelikli oldugu icin bir int kadar arttirildi. Su anda b arrayinin 1. elemanini gosteriyor.

++**pp; // oncelikten dolayi pp yine bir int kadar arttirildi ve b arrayinin 2. elemanini gostermeye basladi. **pp su anda 2

printf("%d\n", (++**pp)[a]); // bu ifade asagidakine esit.

printf("%d\n", a[++**pp]);

// ++**pp ifadesi yine ++ onceliginden dolayi bir int kadar arttirildi ve b arrayinin 3. elemanini gostermeye basladi. **pp su anda 3

printf("%d\n", a[3]); // yukaridaki printf bu hale donustu.

// Output : -4
```

## String Data Type

String veri tipi için char pointer'ı kullanılıyor.

```c
char *s = "Hello World!";

printf("first character is %c\n", s[0]); // returns H
printf("%s\n", s); // returns Hello World!
```

## Header File

Başlık dosyası yazdığımız programın teknik dokümanını içeriyor. Başlık dosyası sayesinde yazdığımız bir kütüphanenin fonksiyonlarını `Prototype` özelliği ile diğer programların kullanımına açabiliyoruz. 

### Header Guard

Bir C projesinde bir başlık dosyası birden fazla kez include edilebilir. Bunu engellemek için `header` dosyamıza aşağıdaki yapıyı kullanıyoruz. Kullandığımız kelime dosyanın ismi olarak kullanılıyor çünkü bir projede iki tane aynı ada sahip dosya ismi olamaz. Ayrıca kelimenin projemizde define edilmemiş olması gerekiyor.

```c
// foo.h
#ifndef FOO_H
#define FOO_H

//..

#endif
```

## Const, Static, Volatile

Const ile tanımlanan değişken belleğe bir kez yazılır ve bu belleğe bir daha dokunulmaz. Bu veriler `NVM` (non-volatile memory) adında bir bölgeye yazılırlar.

Static ile tanımlanan değişkenlerin yeri bellekte değişmez.

Static ile tanımlamanın bir avantajı var, C dilinde global olarak Static bir değişken tanımladığımızda bu değişken sadece o dosya içinde kullanılabilir çünkü derleyici dosyaları tek tek obje haline getirir (dosyayı makine kodu haline getirir) ve sonrasında linkler.

Aynı zamanda Static değişken bir fonksiyon içinde sadece bir kez `initilize` edilir, bu sebeple fonksiyon her çağrıldığında yeni bir değişken oluşmaz, aynı değişken Static tanımı sayesinde kullanılabilir.

Volatile ile tanımlanan değişkenlere daha hızlı ulaşabiliriz çünkü belleğe yazmak yerine işlemcinin Register'ında tutulur verdiğimiz değer. Bu deyimle tanımlanan bir değişken derleyici tarafından optimizasyona uğramaz. Derleyiciler için Volatile'ın anlamı `avoid aggressive optimization involving the object`.

```c
// doğru
static const uint_8 = 50; 

// yanlış çünkü mantığı çelişiyor
volatile const foo uint_8 = 50
```

## Portable, Secure Variable Usage

Değişkenlerin bellekte kapladığı alan çalıştığı sisteme göre değişebiliyor. Örnek olarak 64 bit sistemlerde integer bir değişken 8 byte yer kaplarken 32 bit bir sistemde integer bir değişken 4 byte yer kaplar. Aynı durum gömülü sistemler içinde geçerli, yazdığımız bir algoritmanın diğer mikro denetleyiciler ile uyumlu olmasını istiyorsak bu duruma dikkat etmemiz gerekiyor.

Yazdığımız programın tüm sistemlerde hiç bir değişikliğe uğramadan çalışabilmesi için değişkenleri daha akıllıca kullanmamız gerekiyor. 

C dilinde `stdint.h` başlık dosyasını kullanarak bellekte kapladığı alan sabit olan değişkenler ediniyoruz. Programımızda sadece bu dosya ile değişken tanımı  yaparsak eğer değişik platformlarda sorunsuz bir şekilde değişkenlerimizi kullanabiliriz.

```c
#include <stdint.h>

// Unsigned 32 bit integer, 32 bit -> 4 byte, Unsigned -> Only positive integers
uint32_t max = 2050;
```

# Dynamic Memory Usage

Dinamik bellek yönetimi nedir? Programı kullanan kişinin sürekli veri girdiğini varsayalım, bu verileri bir değişken üstünde depolamak isteyelim. C dilinde değişkenin bellekteki alanını Python gibi yüksek seviyeli dillerde olduğu gibi kolayca büyütemiyoruz, bu konuda dil soyutlanmamış. Python gibi bir dilde mesela bir dizinin içine bir veri `append` edebilirken C dilinde bu mümkün değil.

C dilinde bellek yönetimi kullanıcıya bırakılıyor. Belleği dinamik olarak yönetebilmek için `stdlib.h` başlık dosyasında `Malloc`, `Realloc`, `Free` gibi fonksiyonları kullanabiliriz veya bu iş için geliştirilmiş algoritmaları kullanabiliriz.

## Malloc

`Malloc` fonksiyonu ile dinamik olarak bellek ayırabiliyoruz.

```c
// 10 tane integer oluşturduk
int *foo = malloc(sizeof(int) * 10);

// degişkenimize array gibi davranabiliriz
foo[0] = 50;

// bu kullanım genelde çalışsada teknik olarak yanlış, bizim için erişim izni aldığımız bölge 0-9 arası, 
// 10 ve üstüne çıktığımızda program çalışsada şansa çalışmış olur, "Segmentation Fault" alma ihtimali var.
// işletim sistemi genelde verdiği bölge ile başka bölgeye bir kaç byte boşluk koyar.
// eğer sistemin bellek kullanımı fazlaysa "Segmentation Fault" alma ihtimalimiz artar.
foo[10] = 100;
```

## Realoc

`Realloc` fonksiyonu ile `malloc` fonksiyonu kullanarak açtığımız değişkenin alanını değiştirebiliyoruz. Bu fonksiyon bellekte bizim belirttiğimiz kadar boş alan bulup eski adreste bulunan veriyi yeni adrese kopyalıyor, sonrasında değişken Pointer'ını yeni adrese ayarlıyor. Kopyalama işlemi yaptığı için bu sebeple lineer bir çalışma hızına sahip.

C dilinde bir değişkenin boyutunu değiştiremiyoruz, her seferinde bellekte ne kadar yer işgal edeceğini bildirmemiz gerekiyor. Sürekli veri girişi yapılacak bir program yazdığımız gibi durumlarda bu sorunu çözmemiz gerekiyor. Algoritma ile bu durumu aşmak mümkün ancak elimizdeki veri ne kadar büyükse algoritmanın işleme süresi lineer büyüyor. 

Algoritma yerine `realloc` yani `re allocate memory` fonksiyonunu kullanabiliriz ancak aynı şekilde işleme süresi lineer şekilde büyür.

```c
// Örnek Realloc Kullanımı
#include <cs50.h>
#include <stdio.h>

int main(void)
{
    int *numbers = NULL;
    int capacity = 0;
    int size = 0;

    while (true)
    {
        int number = get_int("Number: ");

        if (number == INT_MAX)
        {
            break;
        }

        if (size == capacity)
        {
            int *tmp = realloc(numbers, sizeof(int) * (size + 1));
            capacity++;
            // realloc failed
            if (tmp == NULL)
            {
                //quit
            }
        }

        numbers[size] = number;
        size++;
    }

    for (int i = 0; i < size; i++)
    {
        printf("Your number is %i\n", numbers[i]);
    }

    free(numbers);
}
```

Peki `Realloc` fonksiyon nasıl çalışıyor? Bilgisayar açtığımız dizinin sonunda istediğimiz miktarda byte varsa veriyor bize, yoksa bile `Realloc` dizimizde bulunan tüm veriyi bellekte bulunan daha geniş bir yere `relocate` ediyor yani yeni ve daha uzun bir adres bulup oraya kopyalıyor ve verilerimizi o adresten artık okumaya devam ediyoruz. Bu yüzden `Realloc` iyi bir kullanım şekli değil. Çünkü eldeki veri büyüdükçe işlem süresi lineer büyür. Veri sayısı ne kadar büyürse veriyi genişletmek için bir o kadar zaman harcaması gerekiyor algoritmanın.

Dinamik bellek yönetimini çok daha hızlı yapan algoritmalar var. Bu algoritmalar `Dinamik Bellek Algoritmaları` başlığı altında anlatıldı.

## Free

Sistemden dinamik olarak aldığımız belleği geri vermemiz gerekiyor çünkü C dili derlenen bir dil olduğu için `Garbage Collection` özelliği bulunmuyor. Eğer aldığımız belleği geri vermezsek zamanla programımızın büyüklüğüne göre sistemin belleği dolmaya başlar ve zamanla bilgisayarda kasılmalar ve donmalar meydana gelir. Bazı zamanlar bilgisayarı günlerce açık tutabiliyoruz, bu gibi durumlarda programımız aldığı belleği hiç geri vermediği için bellekte aslında kullanılmayan ama programımız tarafından ayrılmış atıl yerler kalacaktır. Bu probleme literatürde `Memory Leak` adı veriliyor.

# Some Algorithm Errors

Programda `runtime` durumunda çıkabilecek, çıkması çok müsait, bazı yaygın algoritmik hataların nedenleri ve çözümleri.

## Swap Values

a ve b değerimiz olsun, a yı b, b yi a yapmaya çalışırken klasik yöntem tmp adında bir değişken oluşturup bu değişken üstünden değerleri değiştirmektir.

```c
int a = 1;
int b = 2;

swap(a, b);

void swap(int a, int b)
{
    int tmp = a;
    a = b;
    b = tmp;
}
```

Mantıklı görünüyor ancak burada bir hata var. Bir fonksiyona değişken gönderdiğimiz zaman değişkenin yeni bir kopyası bellekte oluşturuluyor. Bu demek oluyor ki global halde bulunan değişkenler hala aynı değere sahipler. Hatayı düzeltmek için değişkenin kopyası yerine değişken adresini göndermeliyiz.

```c
int a = 1;
int b = 2;

swap(&a, &b);

void swap(int * a, int * b)
{
    int tmp = * a;
    * a = * b;
    * b = tmp;
}
```

Burada Int göndermek yerine Int'in Pointer'larını yani adres göstericilerini gönderdik.

`*` işareti `dereferance` operatörü oluyor ve her zaman adres belirtiyor. & işareti değişkenin ilk byte'ı belleğin hangi adresinde onu gösteriyor. String'de zaten adres kaydedildiği için & işaretine gerek yok.

# Useful Libraries

Yaygın kullanılan bazı kütüphaneler ve yetenekleri.

## String

String veriler ile ilgili fonksiyonların yer aldığı bir kütüphane.

Bunun için `string.h` adında bir başlık dosyası çağırıyoruz. Bu dosya içinde String ile alakalı fonksiyonlar yer alıyor. 
```c
// değikenin uzunluğu, örn. ali -> 3
int len = strlen(s);

// string karşılaştırmanın kolay yolu, aynı zamanda negatif yada pozitif integer döndürerek
// alfabetik olarak hangisi daha önde return edilen değerden anlamamız mümkün.
// 0 döndürüyorsa isimler aynı, else ise farklı.
strcmp(a, b);
```

## Ctype

Bu kütüphaneyi kullanmak için `ctype.h` dosyasını projemize `include` ediyoruz.

```c
// karakter büyük harf olarak döndürülür.
foo[i] = toupper(c);

// karakter küçük harf olarak döndürülür.
bar[j] = tolower(c);

// karakter küçük harf ise 0, değilse pozitif bir integer değer döndürür.
int status = isupper(c);

// karakter büyük harf ise 0, değilse pozitif bir integer değer döndürür.
int status = islower(c);

// karakter rakam değilse 0 döndürür, değilse pozitif bir integer değer döndürür.
int value = isdigit(c);
```

## Math

TODO

# File Operations

TODO

# Static and Dynamic Libraries

C dilinde kütüphane yapısı.

## Static Library

Eskiden bilgisayarların işlemci gücü düşüktü bu yüzden işlemcinin gücünü tasarruflu harcamak önemliydi. Bu sebeple önceden derlenmiş ve obje koduna çevrilmiş kütüphaneler kullanılıyordu. Bu şekilde projemizi her derleme işleminden geçirdiğimizde; Kullandığımız kütüphaneler tekrar tekrar derleme işleminden geçmediği için hızlı bir derleme işlemi oluyordu.

Bilgisayarların işlemci gücü artık yüksek olduğu için kullanılmıyor statik kütüphaneler.

- Statik kütüphaneler Unix sistemler için .a, Windows için .lib uzantısıdır.
- İşlemci gücü arttığı için artık bu yöntem tercih edilmiyor olsa bile programımızın kaynak kodunu saklamak için için tercih edilebilir.
- Unix sistemlerde statik kütüphanelerin ön eki lib ile başlar, örneğin, libusb.so, libfloat.lib vs.

### Creating a Static Library

Windows için bu işi yapan programın adı `lib.exe`, Unix sistemler için `ar`

Aşağıdaki örnekte Calculator.o adlı derlenmiş dosyamızı kullanarak libcalculator.a adında Static bir kütüphane oluşturduk.

```bash
ar cr libcalculator.a Calculator.o
```

## Dynamic (Shared) Libraries

- Bu dosyaların uzantısı Unix sistemler için `.so` (shared object), Windows için `.dll` oluyor.
- Bu kütüphaneler bizim programımız derlenirken programın içine linklenmezler bu sebeple programımız boyut olarak büyümez. 
- Bu kütüphanenin kolaylığı ileride tüm projemizi derlemek zorunda kalmadan sadece kütüphane dosyasını değiştirerek programı değiştirmiş oluyoruz.

Dinamik bir kütüphaneden tek bir fonksiyon çağırsak bile tüm dinamik kütüphane, hatta dinamik kütüphaneye bağlı diğer dinamik kütüphanelerin tamamı bellekte yer kaplar.

Kısaca yazdığımızprogram dinamik bir kütüphane kullandığı zaman boyutu küçük kalır. Programlar ortak olarak bazı kütüphaneleri bu şekilde ortak kullanırlar.

### Creating a Dynamic Library

Aşağıdaki örnekte calculator.c adlı dosyamızı vererek libcalculator.so adlı dinamik bir kütüphane elde ettik.

```bash
gcc -o libcalculator.so -fPIC -shared calculator.c
```

# Some Terms

Bilinmesi gereken, temel bazı programcılık terimleri.

## Abstraction

Soyutlama anlamına geliyor. Bilgisayar biliminde sıkça gördüğümüz olay. Bu kelimeye programlama dillerini dahil edebiliriz mesela, programlama dilleri bizi Assembly dilinden, Assembly dili bizi makine dilinden soyutlaştırıyor. Hayatımızı daha kolay bir hale getiriyor.

Konuyu açmak gerekirse soyutlama ile gereksiz teknik detaylar geliştirici tarafından gizleniyor ve üretkenlik artıyor.

## Best Practice

Bir problemi veyahut bir işi yapmak için herkes tarafından kabul edilen yöntem, yapış tarzı. Bir işi yapmak için bir çok yöntem olabilir ancak `best practice` yolları izleyerek daha okunabilir, daha anlaşılır ve daha az maliyetli kodlar üretmemiz mümkün.

## Rubber Duck Debugging

Çalışma masasına bir plastik ördek koyup programımızda sorun çıktığında problemi daha hızlı bulup çözmek için masamızdaki plastik ördek ile konuşuyoruz. Genelde sorunu sesli şekilde anlatırken "ahh" anı yaşarız ve belki saatlerce sürecek hata ayıklama işlemi dakikalara iner.

## Recursion

Bir fonksiyonun kendisini çağırmaya `recursion` deniyor.

```c
int fact(int n)
{
    if (n == 1)
        return 1;
    else
        return n * fact(n - 1);
}
```

Diğer türlü yazsaydık kodumuz daha çirkin gözükecekti. 

```c
int fact(int n)
{
    int product = 1;
    while (n > 0)
    {
        product *= n;
        n--;
    }
    return product;
}
```

## Garbage Collection

C ve C++ dilinde olmayan bir özellik, çünkü bu diller derleniyor. C ve C++ dillerinde yazdığımız programda `memory leak` olmaması için kodumuzun içinde bellek yönetimini iyi yapmamız gerekiyor. Ancak derleme işleminden geçmeyen dillerde bu iş `abstraction` edilmiştir. Geliştirici üst seviye dillerde bellek yönetimi ile uğraşmaz.

C#, Python ve Java gibi C diline göre yüksek seviyeli dillerde `Garbage Collection` özelliği bulunuyor.

## Heap ve Stack

Bellek üstünde bilgiler rastgele olarak depolanmıyor, belleğin altı, üstü ve ortası belleğin daha performanslı çalışması için işletim sistemi tarafından bazı grup program ve dosyalar için ayrılıyor. Örneğin belleğin en üstüne yani `heap` bölgesine işletim sistemi açılır, aynı zamanda değişkenler işletim sisteminin hemen altında bir bölgeye kaydedilir. Belleğin en alt bölgesine yani `stack` içinde programlardaki fonksiyonlar kaydedilir. 

## Program Errors

C ile program geliştirirken karşımıza çıkabilecek bazı hatalar.

### Overflow ve Underflow

`Overflow`, bir yazılımda ki değişken değerinin kapasitesi üstüne çıkmasına deniyor. Örneğin bir byte yer tutabilen bir değişkenin maksimum halini `11111111`'dir eğer bu sayıya +1 eklersek matematikteki eldeler ile birlikte `100000000` olur ve program `overflow` hatası verir.

`Underflow` da ise değişken değerinin minimum değerinden daha aşağıya çekince yaşanır, `00000000` sayısından 1 çıkartınca `011111111` olur ve programımız `underflow` hatası verir.

### Buffer Overflow, Stack Overflow ve Heap Overflow

TODO

### Memory Leak

Bu bir mantık hatası. Yazdığımız program eğer bilgisayar belleğinden alan alıyorsa ve program sonunda geri vermiyorsa bu terim kullanılıyor.

### Segmentation Fault

Bellek içinde bize ait olmayan bir alana eriştiğimiz zaman gelen bir hata. 

Dinamik olarak `memory allocate` ederken eğer çok uzun bir yazı yazacak olursak bu hatayı alırız. `scanf` ile String bir değer verirken eğer çok uzun bir yazı yazarsak mesela bu hata çıkar.

Bilgisayar güvenlik amaçlı biz bellekte bir yer istediğimiz zaman bir kaç boşluk ile verir ki bunun gibi hatalarla karşılaşmayalım diye.

####  Valgrind Usage (Memory Leak and Segmentation Fault Detection)

Bunun için `valgrind` adında bir konsol programı var. 

Örnek Kullanım;

```bash
valgrind ./name_of_program
```

Kısaca `valgrind` aracına derlenmiş programımızı çalıştırabilecek şekilde `valgrind` aracına parametre olarak veriyoruz. Program kullanıcı dostu değil, programın verdiği çıktı kafa karıştırıcı olabilir. Verdiği çıktıyı basitleştiren CS50 dersinde kullandığımız programı -help50- adlı başka bir program kullanabiliriz.

```bash
help50 valgrind ./name_of_program
```

# Communities

TODO

# Useful Links

[Harvard CS50 Class](https://courses.edx.org/courses/course-v1:HarvardX+CS50+X/course/)

[Çoşkun Taşdemir](https://coskuntasdemir.net/)

[Ozen Ozkaya](http://ozenozkaya.com/blog/)

[Lojik Prob](http://www.lojikprob.com/)

# Read More

[Make](https://demirten.gitbooks.io/gomulu-linux/gnubuild/make.html)

[Different C Standards: The Story of C](https://opensourceforu.com/2017/04/different-c-standards-story-c/)

[Does Column Width of 80 Make Sense in 2019?](https://hackernoon.com/does-column-width-of-80-make-sense-in-2018-50c161fbdcf6)