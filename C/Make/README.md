# Make and Makefile

Make programı bir GNU build alma aracıdır. Programın özündeki amacı geliştiricinin daha kolay derleme yapmasını sağlamak. Make programı ile projemizde sadece değişen kısımlar derlenir ve içeriği değiştirilmemiş, tekrar derlenmesine gerek olmayan dosyalar aynı bırakılır, bu şekilde geliştiriciye zaman kazandırılır.

Derlemek için illa make komutuna ihtiyaç duymuyoruz. Büyük projelerde birden fazla dosya ile çalışıyorsak ve bunların hepsini derlemek istiyorsak make tercih ediliyor.

Make komutu işimizi kolaylaştırır, bir chip için program yazmak yerine sayesinde 30 chip için aynı kaynak dosyalarını sadece ekstra bir makefile dosyası ile kullanabiliriz mesela.

- Make programı çalıştırıldığında dizinde `GNUmakefile`, `makefile` ve `Makefile` adlı dosyaları arar.
- Bunlara ek olarak `-f` bayrağı ile özel olarak bir Makefile dosyası işaret edebiliriz.
- Genelde hoş gözüktüğünden `Makefile` ismi kullanılır.
- Makefile dosyası oluştururken yapılan en büyük hata TAB yeni space karakterinin kullanılmasıdır.
- Make programına hiçbir parametre vermeden sadece make diyerek çağırdığımızda makefile içindeki tanımladığımız ilk kuralı çalıştırır.
- Makefile dosyası içinde değişken kullanabiliriz.

# Table of Contents

- [Make and Makefile](#make-and-makefile)
- [Table of Contents](#table-of-contents)
- [Usage](#usage)
- [Bash Script vs Makefile](#bash-script-vs-makefile)
- [Makefile Examples](#makefile-examples)
  - [Example 1:](#example-1)
  - [Example 2:](#example-2)
- [Using a File with Rule Name](#using-a-file-with-rule-name)

# Usage

En yaygın kullanım amacı kaynak dosyalarını derlemek, birleştirmek ve çalışabiler hale getirmek. Derleme işlemi için otomatik olarak sırayla makefile içindeki komutları uygulayarak daha hızlı ve hatasız bir şekilde derlenmiş dosyalar elde ediyoruz. Amacı her teknolojide olduğu gibi işimizi kolaylaştırmak.

Biz proje içinde değişiklik yapar ve `make` komutunu kullanırız, bunun sonucunda proje derlenmiş olur.

Make komutunu kullanırken bulunduğumuz dizinde bir tane `makefile` adında bir dosya olması gerekiyor. Bu dosya yapılacak işler, sırasıyla uygulanacak komutlar bulunuyor.

Make komutunu verirken verdiğimiz parametrelere göre derleme işlemi gerçekleşiyor. Misal avr-gcc kullanıyoruz derleyici olarak ve yazdığımız kodu hex dosyasına çevirmek istiyoruz, make derken hangi işlemci için hangi kristalde vs makefile dosyası içinde belirtiyoruz.

# Bash Script vs Makefile

Bu güzel bir soru. Aslında aynı işlemler bir `bash script` ile de yapılabilir ancak C kodu yazan insanlar genelde make programını kullanıyorlar. Bu durum `de facto standart`'ı olmuş.

# Makefile Examples

## Example 1:

```
CC     = gcc
CFLAGS = -O2 -Wall -pedantic
LIBS   = -lm -lnsl

all: install

ornek: ornek.o
    $(CC) $(CFLAGS) $(LIBS) -o ornek ornek.o

ornek.o: ornek.c
    $(CC) $(CFLAGS) -c ornek.c

clean:
    rm -f ornek *.o

install: ornek
    cp ornek /usr/local/bin
```

## Example 2:

```
build:
    gcc main.c expAvg.c -o main

clean:
    rm -rf main expAvg_log.csv

iterate:
    make clean && make build && ./main

plot:
    python3 ./python/plot_data.py expAvg_log.csv
```

# Using a File with Rule Name

Örneğin klasörde clean adlı bir dosya var; Bu durumda `make clean` çalıştırdığımızda clean adlı bir kural yazmış olsak bile yazdığımız kural çalışmaz. Çözümü basit; Aşağıdaki gibi yeni bir kural oluşturup dosya ile kural arasındaki farkı belirtmemiz gerekiyor.

`.PHONY: clean`