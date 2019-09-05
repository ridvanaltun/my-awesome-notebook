# Algorithm Analysis

Bu dokümanda C dilinde yazılmış algoritmalar ve bu algoritmaların analizi yer alıyor. C dili neden tercih edildi? Çünkü C dili derlenen bir dil ve Bellek Yönetimi gibi problemleri ileri düzey algoritmalar ile çözmemiz gerekebiliyor.

Algoritmalar arası genelde ya bellekte ki alandan kazanç sağlıyoruz yada zamandan kazanç sağlıyoruz. Bazı zamanalr her ikisinide birlikte yapabiliyoruz. Senaryomuza göre algoritma seçimi yapmak önemli. Buna `complexity` deniyor.

Algoritmaları karşılaştırma bazı zamanlar yanıltıcı olabiliyor.

Örneğin: A ve B iki algoritmamız olsun. A `5.000n` ve B `1.1**n` adımda işlem yapsın. B algoritması çok hızlı gibi duruyor. `n = 10` için A algoritması 50.000 adımda işlem yaparken B algoritması 3 3 adımda tamamlar. `n = 1000` için A algoritması `5.000.000` adımda tamamlarken B algoritması `2.5 * 10**41` adımda bitirir. Görldüğü üzere elimizdeki veri sayısına göre algoritmanın faydası değişebiliyor.

Algoritmaların bu değişimine `growth rate` deniyor.

Bu doküman sonrası algoritmaları analiz yeteneğine sahip olabimeli ve hızlarını matematiksel olarak çıkarabilmeliyiz.

# Table of Contents

<!-- toc -->

- [Algorithm Analysis](#algorithm-analysis)
- [Table of Contents](#table-of-contents)
- [Algorithms](#algorithms)
  - [Searching Algorithms](#searching-algorithms)
    - [Lineer Search](#lineer-search)
    - [Binary Search](#binary-search)
  - [Sort Algorithms](#sort-algorithms)
    - [Selection Sort](#selection-sort)
    - [Bubble Sort](#bubble-sort)
    - [Merge Sort](#merge-sort)
  - [Dynamic Memory Algorithms](#dynamic-memory-algorithms)
    - [Linked List](#linked-list)
      - [Singly-Linked List](#singly-linked-list)
      - [Doubly-Linked List](#doubly-linked-list)
    - [Hash Table](#hash-table)
    - [Trie](#trie)
  - [Buffer Algorithms](#buffer-algorithms)
    - [Ring Buffer](#ring-buffer)
- [Some Terms](#some-terms)
  - [Growth Rate](#growth-rate)
  - [Asymptotic Analysis](#asymptotic-analysis)
    - [Big-0 Notation](#big-0-notation)
    - [Omega Notation](#omega-notation)
    - [Teta Notation](#teta-notation)

<!-- tocstop -->

# Algorithms

C dili yazılmış algoritmalar ve mantıkları. Bunun yanında bahsi geçen algoritmaların kullanım alanları, maliyetleri ve hızları bu başlık altında inceleniyor.

## Searching Algorithms

Arama algoritmaları.

### Lineer Search

Her elemena tek tek baktığımız en basit arama algoritması. Elemanı bulduğumuzda diğer işlemlerimize devam ediyoruz.

Örneğin `n = 2**30` elemanlı bir sayı için lineer aramada en kötü ihtimalle bulması yıllar alabilir.

Peki bu akdar kötü, o zaman neden kullanıyoruz? elimizde küçük sayıda bir veri varsa o zaman tercih edilebilir bir algoritma oluyor. Diğer algoritmalara oranla okuması ve yazması basit olduğu için tercih edilebilir.

### Binary Search

Eğer arama yaptığımız veri kümesi sıralı bir şekildeyse logorotmik olarak yani her işlşlemde işlem sayısını 2 ye bölerek sonuca ulaşmamız mümkün. İlk olarak ortadaki sayıya bakıyoruz, ya sağ ya sol tarafa aradığımız sayıya göre yarım yarım ilerleyip aradığımız değeri bulabiliyoruz.

Örneğin `n = 2**30` elemanlı bir sayı için binary (ikili) arama algoritması en fazla 30 adımda bulur cevabı.

## Sort Algorithms

Bir çok sıralama algoritması var, bu algoritmaların hızlarını matematiksel olarak belirtebiliriz. 

Algoritmanın matematiksel olarak en yavaş hız formülü yazılım jargonunda `O` harfi ile gösteriliyor, en hızlı ise `Omega` (Ω) işareti ile gösteriliyor. Örneğin 5 adet veriyi `Bubble Sort`  algoritması ile sıralarken maksimum O(25) adımda, minimum Ω(5) adımda bitiriyoruz.

Burada listelenen algoritmalar dışında bir çok sıralama algoritması mevcut. Burada bir çok algoritma sıralanmasının nedeni farkları ve algoritma mantığını kavramak için. Örneğin buradaki en hızlı sıralama algoritması `merge`. Mantıklı düşünüldüğünde `bubble` veya `selection` algoritmalarını kullanmaya gerek kalmıyor.

### Selection Sort

O(n²)
Ω(n²)

Bu algoritmada dizideki tüm değerlere bakıp en küçük değeri başa koyuyoruz ve en küçük değeri sıraladığımız için bir daha en küçük değere dokunmuyoruz, bir ileri adım atıyoruz ve sıradaki en küçük değeri bulup aynı şekilde sıralama yapıyoruz. Mantığı basit bir algoritma ama matematiksel formülüne baktığımızda yavaş olduğu anlaşılıyor.

### Bubble Sort

O(n²)
Ω(n)

Küçükten büyüğe doğru sıralama yapmak için ilk adım olarak dizinin ilk elemanını en küçük değer olarak kabul edip listedeki diğer elemanlar ile karşılaştırıyoruz, eğer karşılaştırdığımız eleman daha küçük ise listede bu iki elemanın yerini birbiriyle değiştiriyoruz.

### Merge Sort

O(nlogn)
Ω(nlogn)

Listede en hızlı sıralama algoritması bu. Sıralamak istediğimiz veri kümesini parçalara bölüp her parçayı kendi içinde sıralıyoruz, sonrasında her iki parçayı tek parça olarak şekilde sıralayarak oluşturuyoruz ve bu işlem tüm sıralama işlemi bitene kadar devam ediyor.

## Dynamic Memory Algorithms

C dili ile gelen dinamik bellek yönetimi sorunu için türetilmiş algoritmalar ve bunların karşılaştırılması.

Dinamik bellek algoritmasına neden ihtiyacımız olduğunu anlamak için C dilinde çoklu veri depolamanın yollarının atası olan dizi yapısını incelemek gerekiyor.

Dizi;

Eleman eklemek ve silmek için lineer bir zaman harcıyoruz.

Dezavantajlar:

- Dizinin sonuna eleman eklemek basit bir iş ancak sıralanmış bir dizi kullanıyorsak ve orta kısma koymamız gereken bir veri varsa bu işi yapamıyoruz.
- Bellekte kapladığı alan büyümüyor, esnekliği az. Bu yüzden ekstra bir veri girmek istediğimizde bize gereken kadar büyüklükte yeni bir dizi açıp eski diziyi yeni diziye tek tek kopyalanır ve yeni veriyi de üstüne kaydederiz.

Avantajlar:

- Bellekte diğer yöntemlere göre daha az yer kaplıyor.
- Kullanması kolay.
- Rastgele bir veriye erişmesi hızlı ve kolay.

### Linked List

Verilere geri dönemeden erişmeye `Singly Linked List` deniliyor. Ekstra olarak geri gidebilmek istiyorsak yazacağımız algoritmaya `Doubly Linked List` deniyor.

Dezavantajlar:

- Rastgele bir veriye erişmek zaman alıyor. Veriyi lineer olarak arıyoruz.
- Verileri sıralamak zor.

Avantajlar:

- Dinamik olarak veri eklemek ve silmek kolay.
- Bellekte diziler kadar olmasa da az yer kaplıyor.

#### Singly-Linked List

Anlatması zor bir yöntem. Bellekte bir değer depoladıktan sonra bu değerin hemen yanına bir sonraki değerin bellek adresini depoluyoruz, bu işlem her yeni değer kaydı için geçerli. Sadece bir adımda bir yerine 2 veri kullanarak C dilinin bu sınırlamasını aşabiliyoruz.

Linklemeyi uygulamanın basit bir yolu var, yöntem aşağıda anlatılıyor.

Bu kodda 2 tane "node" kullanılmış, C dilinde kodlar her zaman yukarıdan aşağıya doğru okunur, biz yukarıya node tanımlaması yaptığımız zaman `struct ` içinde node'u kullanabilir hale geliyoruz.

Bana bir integer ve bir pointer ver diyoruz bu veri tipinde.

```c
// Example of Singly-Linked List Algorithm

#include <cs50.h>
#include <stdio.h>

typdef struct node
{
    int number;
    struct node *next;
}
node;

int main(void)
{
    node *numbers = NULL;

    // prompt for number (until EOF)
    while (true)
    {
        // propmt for number
        int number = get_int("Number: ");

        // check for EOF
        if (number == INT_MAX)
        {
            break;
        }

        // allocate space for numbers
        node *n = malloc(sizeof(node));
        if (!n)
        {
            return 1;
        }

        // add number to list
        n->number = number; // (*n).number = numver;, * means go to pointed adress
        n->next = NULL;
        if (numbers)
        {
            for (note *ptr = numbers; ptr != NULL; ptr = ptr->next)
            {
                if (!ptr->next)
                {
                    ptr->next = n;
                    break;
                }
            }
        }
        else
        {
            numbers = n;
        }
    }
}
```

Oluşturduğumuz algoritmayı daha hızlı yapmak için `Link Listing` ile diziyi birleştirebiliriz. Buna `Hash Table` deniyor.

#### Doubly-Linked List

TODO

### Hash Table

Bu yapıda aslında parçalanmış şekilde diziler içinde `Linked List`'ler kullanıyoruz. Bir veri aramak `Linked List`'e göre daha kolay çünkü alakasız bazı verileri eleyerek istediğimiz veriyi bulabiliyoruz. `Linked List'`e göre bellekte daha fazla yer kaplıyor ancak `Tries` algoritmasına göre daha az yer kaplıyor.

Bu yapı temel olarak dizi ile `Linked List`'in birleşimi.
Sıralama işlemi yapmak istediğimizde bu yapının bize kazandırdığı tüm avantajları kaybediyoruz, sıralama yapmak istediğimiz bir programımız varsa bu yapının kullanılması tavsiye edilmiyor.

Bu algoritma için bize pozitif `hash code` üreten bir `hash function`'a ihtiyacımız var. İkinci olarak bize bir dizi gerekiyor. Elimizdeki veriyi `hash function`'a veriyoruz, `hash funstion` bir `hash code` üretiyor ve bu kodu bir arrayde depoluyoruz.

Örnek olarak isim kaydetmek isteyebiliriz. `Hash Table` algoritmasını kullanmak için 26 elemanlı bir dizi açalım, `hash function` oluşturalım ve bu fonksiyon bize A ile başlayan bir isim girildiğinde "0", Z ile başlayan bir isim girildiğinde "26" döndürsün. A ile başlayan tüm isimleri dizinin 0. adresini kullanarak `Linked List` algoritması ile depolayalım. A ile başlayan başka bir isim verisi geldiğinde `Linked List` ile gerekli adrese kayıt etmeye devam edeceğiz.

Bu şekilde ileride bu kişiyi aradığımızda `Linked List` algoritmasıyla karşılaştırdığımız, teoride 26 kat daha hızlı bulabiliriz.

`Linked List`'lerin dezavantajı rastgele olarak bir veriye erişme zorluluğu. `Linked List`'de n zamanda bir veriye erişirken bu algoritmada (n / Linked List sayısı). Ne kadar çok `Linked List`'imiz varsa ki bu array'in boyuna eşit, o kadar hızlı arama yapmamız mümkün.

### Trie

Rastgele bir veriye erişmek hızlı. Verileri sıralanmış bir şekilde kaydediyoruz. Elimizde az bir veri olsa bile bellekte çok yer kaplıyor. Aldığımız bellek ile elimizde sıralanmış bir veri elde ediyoruz.

## Buffer Algorithms

Verilerin tampon bellekte saklanma yöntemleri, kullanım alanları, bunların avantaj ve dezavantajları.

### Ring Buffer

Örnek bir kullanım amacı; İki taraflı bir iletişimde iletişim hızı farklı ise eksiksiz bir iletişim için gelen mesajın bir yerde saklanması ve bekletilmesi gerekebiliyor. Eğer gelen mesajları saklamasak ikinci bir seçenek aynı anda sadece bir defa mesaj okumak olurdu ve bu şekilde karşı taraftan bazı mesajları kaçırmış olurduk.

Karşı taraf bizim işlem gücümüzden daha hızlı bir şekilde mesaj gönderiyorsa aradaki mesajları kaçırmamız normal bir durum. 

Kısaca gelen mesajların biriktirilmesi ve tane tane okunmasını sağlıyor bu algoritma.

Bu algoritma daha çok gömülü sistemlerde iletişim amaçlı kullanılabilir diye düşünüyorum. Her ne kadar mikro denetleyicilerde kesme özelliği olasa da bazı senaryolarda kesme donanımına erişimimiz olmayabilir yada gelen verileri depolamak ve ardından işlemek daha ucuz bir çözüm olabilir.

- Dinamik bellek kullanmadan sabit bir bellek içinde veriler depolanıyor.
- Bu algoritma ile `FIFO` (first in first out) yöntemi kullanılıyor. Biriktirilen verilerin önce en eskisi okunur.
- Buffer dolduğu zaman gelen yeni veriyi ya ilk hücreye yazıyoruz yada karşı sisteme alan kalmadığını bildiriyoruz.
- Dinamik bellek yönetimi yapmadığımız için nispeten uygulaması kolay bir algoritma.

# Some Terms

Algoritma analizi alakalı bazı terimler.

## Growth Rate

Growth rate yavaştan büyüğe doğru.
Karmaşıklık giderek artıuor sıralamada.

- 1
- log n
- n
- n log n
- n ** 2
- n ** 3
- 2 ** n
- n!

## Asymptotic Analysis

Bir algoritmanın çalışma performansını ölçmek için kullanılan bir yöntem. Algoritmanın çalışma süresinin alt ve üst sınırını bulmamızı sağlayan algoritma analizi metodu.

### Big-0 Notation

Bir algoritmanın girilen n sayıdaki input ile çalışma süresini gösterme biçimi.

Algoritmaların `Big-O Notation` performansları hakkında bir `cheat sheet`:
http://bigocheatsheet.com/

### Omega Notation

Bir algoritmanın girilen n sayıdaki input ile çalışma süresini alt limiti verir. Yani algoritmanın en kötü pozisyonda n kadar veriyi ne kadar sürede işler bunun cevabını verir.

### Teta Notation

`Big-O Notation` ile `Omega Notation`'ın ortalaması alınca `Teta Notation` çıkıyor. Bunun sonucu daha gerçekçi çünkü hem üst hem alt limiti bulup ortak bir değer belirliyoruz.