# Bash

Bash, aslında en temelinde kendi için yazılmış komutları yorumlayan ve çıktı veren bir programdan fazlası değil. Linux ta birlikte bu program da geliyor, istediğimiz takdirde kolay bir şekilde Windows'a bash kurmak mümkün.

Bash, kısaca terminal işlemlerimizi kolaylaştıran bir script dili. Bu dil ile yazılmış script'leri çalıştırmak için `sh` yada diğer adıyla `bash` programına script'i ve varsa parametreleri veriyoruz. `sh` yazımı daha kolay olduğu için genelde tercih ediliyor.

Bash kullanarak bir çok işi otomatize edebilir ve komplike bazı işleri daha kolay tamamlanabilir hale getirebiliriz.

# Table of Contents

<!-- toc -->

- [Bash](#bash)
- [Table of Contents](#table-of-contents)
- [Bash Alternatives](#bash-alternatives)
- [Shebang](#shebang)
- [Login and Logout Scripts](#login-and-logout-scripts)
- [Edit Profile File](#edit-profile-file)
- [Execute Bash Scripts](#execute-bash-scripts)
- [Define Variable](#define-variable)
- [Environment Variables](#environment-variables)
- [Assign Command's Output to Variable](#assign-commands-output-to-variable)
- [Exit When Error](#exit-when-error)
- [Getting Arguments from the Command Line](#getting-arguments-from-the-command-line)
- [Shift](#shift)
- [Getting Value from the User](#getting-value-from-the-user)
- [Arithmetic Operations](#arithmetic-operations)
- [Comparison](#comparison)
- [User Input](#user-input)
- [String](#string)
- [Array](#array)
- [Conditions](#conditions)
- [Logical Operators](#logical-operators)
- [File Operations](#file-operations)
- [Case](#case)
- [Loops](#loops)
  - [For Loop](#for-loop)
  - [While Loop](#while-loop)
  - [Until Loop](#until-loop)
  - [Special Commands](#special-commands)
- [Functions](#functions)
- [Select](#select)
- [Extra Commands](#extra-commands)
- [Resources](#resources)

<!-- tocstop -->

# Bash Alternatives

Son dönemlerde popüler olan `Zsh` var. Daha çok var ancak ayrıca incelemeye gerek yok.

# Shebang

UYARI! : Windows üstünde bash script yazıyorsak shebag kullanımı bize hata verebilir, Windows ile shebag kullanmaya gerek yokturr.

Bash script'ler `#!/bin/bash` satırı ile başlar. Eğer scriptimizi `./filename` şeklinde çalıştırırsak ilk satır Linux işletim sisteminde okunur ve ilk satırda işaret edilen program ile birlikte dosya açılır.

Misal bash programı bin klasörünün altında değil diyelim, nerede olduğunu öğrenmek için `which bash` yazarsak konsolda bize programın hangi dizinde olduğunu döndürür, örneğin `perl` programı için `which perl` şeklinde yazarak hangi dizinde olduğunu döndürtebiliriz.

Windows işletim sisteminde bir dosya açarken dosyanın uzantısına bakılır, Linux sistemlerde dosya açılırken ilk satırına bakılır, işaret edilen program ile dosya açılır. Bu sebeple shebag sadece linux içindir. Linux için dosya uzantısının bir önemi yoktur, dosyanın içeriğine bakmadan hangi program ile çalıştırılacağını anlamak için uzantı kullanabiliriz, örneğin `.sh` koyarak bir `shell` programı ile çalıştırılması gerektiğini anlarız.

# History

Bash üstünde yazdığımız tüm komutlar history dosyasına yazılır. Her kullanıcının history dosyası kendisine özeldir.

**Trick:** Komutun başına bir boşluk bırakırsak eğer history içine kaydedilmez.

# Login and Logout Scripts

Bash'i login olup yada guest olarak kullanabiliriz. Bash programı login olduğumuzda özel bir yapı ile çalışır. Login olarak girmek için `--login` veya `-l` argümanıyla bash programını çalıştırmamız gerekiyor.

Bash programı açılırken eğer login olarak açmışsak sırasıyla aşağıdaki dosyalar execute edilir.

```bash
/etc/profile
~/.bash_profile
~/.bash_login
~/.profile
```
Bu dosyaların çalışması için var olması ve okuma izinlerinin olması gerekiyor.

Bash programını açarken `-noprofile ` argümanı kullandıysak profile dosyaları execute edilmez.

Çıkış yaparken de aşağıdaki dosyalar sırasıyla execute edilir.

```bash
~/.bash_logout
/etc/bash.bash_logout
```

Çalıştırılan bu dosyalar kullanıcıya göre değişiklik gösteriyor.

```bash
~/.bash_profile     # kullanıcıya özel
/etc/profile        # root hariç tüm kullanıcılar
/root/.bash_profile # sadece root kullanıcısı
```

# Edit Profile File

```bash
source ~/.bash_profile

# veya

. ~/.bashrc
```

Bu komut ile değişikliği anında uygulamış oluruz. Diğer türlü bash programını kapatıp açmamız gerekir.

# Execute Bash Scripts

Eğer yazdığımız scripti `sh foo.sh` şeklinde çalıştırmak istemiyorsak, sadece dosya adı ile çağırmak istiyorsak `chmod +x foo.sh` şeklinde dosyamıza çalıştırma izni vermeliyiz.

`ls -l foo.sh` diyerek dosyamızın izinlerini ve diğer ayrıntılarını görüntüleyebiliriz.

Çalıştırma izni verdikten sonra dosyamızı `./foo.sh` şeklinde çalıştırabiliriz.

Normalde dizinimizi tarihe göre listelemek için `ls -lat` komutunu çalıştırmak gerekiyor, bunun yerine sh dosyamızı;

```bash
ls -lat
```

Şeklinde ayarlayıp chmod+x ile çalıştırma izni verip aynı zamanda bu dosyayı `cp -rp foo.sh /usr/bin/dizinlistele` diyerek yazdığımız scripti gösterdiğimiz hedefe kopyalarsak eğer bundan sonra hangi dizinde olursak olalım konsola sadece `dizinlistele` yazdığımızda tarihe göre dizin listeler.

# Define Variable

```bash
a=ali
b=99
c="Deneme Yazısı"
echo $a $b $c

# OUTPUT: ali 99 Deneme
```

Değişken tanımlamanın bazı kuralları:

- Bash scriptte değişken tipi belirlemeye gerek yok.
- String tanımlarken birden fazla kelime yazacaksak çift tırnak yada tek tırnak ile yazmak gerekiyor.
- Eğer tek kelime yazacaksak tırnak yada çift tırnak kullanmamıza gerek kalmıyor.
- Değişken, eşittir işareti ve tanımladığımız değer arasında boşluk karakteri olmamalı.

Aşağıdaki örnekte bir değişken oluşturduk sonra bu değişkeni sildik.

```bash
var=value
unset var
```

# Environment Variables

Ortam değişkenlerine tüm alt-betikler erişebilir, normal değişken tanımladığımızda bu değişken diğer betiklere aktarılamaz. Bir script ile başka bir script çağırırken ana script içine bir ortam değişkeni koyarsak çağırdığımız script'in içinde bu ortam değişkenini kullanabiliriz.

- $HOME -> Çalıştığımız kullancının home dizini nerede onu döndürüyor.
- $USER -> Çalıştığımız kullanıcıyı döndürür.
- $PWD -> Bulunduğu dizini döndürür.
- $0 -> Scriptin ismini uzantısıyla birlikte döndürür.
- $# -> Girilen argüman sayısını döndürür.
- $HOSTNAME -> host adını döndürür.

Çok fazla çevresel değişken var, internette daha fazlasını öğrenmek mümkün.

Bunlar dışında özel olarak ortam değişkeni tanımlayabiliyoruz.

```bash
export var=value
```

# Assign Command's Output to Variable

```bash
load=$(cat /proc/loadavg)
echo $load

# OUTPUT: 0.00.0.00.0.00 1/228 7007
```

Parantez içinde ki komut ne döndürüyorsa bunu load değişkeni içine array olarak attık çünkü tek parantez kullanımı array anlamına geliyor.

```bash
mem=$(free -m | grep Mem: | awk{'print $3'})
echo Sunucunun Kullanimindaki Bellek Miktari: $mem MB
```

# Exit When Error

Yazdığımız script hata kodu üretince bash çıksın.

Script içine `set -e` şeklinde bir komut girmemiz yeterli. Script çalışırken hata verirse script sonlandırılır. `set +e` ile hata alınca sonlandırma işlemini pasif hale getirebiliriz.

Bir diğer yöntem ise `shebag`'e `-e` şeklinde bir argüman vermek.

`#!/bin/bash -e` şeklinde `shebag` kullanırsak eğer hata aldığımızda program sonlandırılır.

`-e` argümanı gibi `-u` ve `-x` parametreside kullanabiliriz.

`-u` ile önceden tanımlanmamış bir değişken kullanılmışsa hata verir ve bize durumu bildirir.

`-x` ise `debug` modu ile çalıştırılır, execute edilen komutlar hata çıktısında `+` şeklinde gösterilir, bizde başında `+` olmayan komutun hata verdiğini bu şekilde anlarız.

# Getting Arguments from the Command Line

```bash
echo "Merhaba $1 $2"

: '
INPUT : sh deneme.sh Osman Deneme Naber
OUTPUT: Merhaba Osman Deneme

INPUT : sh deneme.sh "Osman Deneme" Naber
OUTPUT: Merhaba Osman Deneme Naber
'
```

Görüldüğü üzere çift tırnak ile birden fazla kelime içeren bir ifadeyi tek bir parametre gibi gönderebiliyoruz.

# Shift

```bash
# Shift Komutu İle Argüman Çevirmek
while[$1];
do
    echo $1
    shift
done

: '
INPUT: sh deneme.sh Osman Deneme Naber
OUTPUT:
    Osman
    Deneme
    Naber
'
```

```bash
1="first"
2="second"
3="third"
while[$1];
do
    echo $1
    shift
done

: '
OUTPUT:
    first
    second
    third
'
```

Bu özellik genelde komut satırı üstünden argüman aldığımız zaman kullanılıyor. Shift komutu ile kolaylıkla bütün argümanları okuyabiliyoruz.

# Getting Value from the User

```bash
echo -n "Kullanici Adi : "
read user
echo -n "Parola : "
read -s pass
echo "Kullanici $user ve sifresi $pass"
echo "İki tane sayı giriniz : "
read x y
echo "Girdiğiniz Sayılar, $x ve $y"
read -p "İki Adet Sayı Giriniz : "a b
echo "Girdiğiniz Değerler, $a ve $b"
echo "Hadi Bir Sayi Daha Gir : "
read
echo "Girdiğiniz Sayı, $REPL"
echo "Beş Tane Sayı Giriniz : "
read -a array
echo "Girdiğiniz İlk Sayı : ${array[0]}"
echo "Girdiğiniz İkinci Sayı : ${array[1]}"
```

Burada read için -s parametresi kullanınca yazdığımız yazı komut satırında ekrana basılmaz. "man read" diyerek read komutunun diğer parametrelerini öğrenmek mümkün.

# Arithmetic Operations

```bash
num=$(($a - $b))

((num=a - b))

let "num=a - b"

num="expr $a - $b"

echo $a + $b | bc
```

Buradaki 5 yöntemde aynı işi yapıyor. Bash'te değişken tipi değiştirmek için bir yöntem yoktur, bunun yerine değişken nasıl bir değer döndürmesini istiyorsak aritmatik yada string karsilastirma islemi yapiyoruz.

```bash
# Basit Bir Toplama İşlemi
sonuc=$((3 + 3))
echo $sonuc

# OUTPUT: 6
```

```bash
# Basit Bir Arttırma İşlemi
topla=$1
sonuc=$(($1 + 3))
((sonuc++))
echo $sonuc
```

# Comparison

```bash
a=1
b=2
[[ $a == $b]]
echo " a b ye esit mi $?"
[[ $a != $b]]
echo " a b ye esit degil $?"
```

Sonuc olarak doğruluğa göre 1 yada 0 dönecektir. 1 ise doğru değil, 0 ise doğru.

# User Input

```bash
[[ -z $1 ]]
echo "sonuc $?"

# veri girişi yapıldıysa 1, tersi 0
```

```bash
[[ -n $1 ]]
echo "sonuc $?"

# veri girişi yapıldıysa 0, tersi 1
```

```bash
[[ -n $1 && -z $2]]
echo "sonuc $?"

# burada ilk parametre ve ikinci parametrelere bakıp hangisi girilmiş girilmemiş ve operatörüyle bakıp sonuç üretiyoruz.
```

```bash
[[ -n $1 && $2=$1]]
echo "sonuc $?"

# $2 $1 e eşitse ve $1 e veri girişi yapılduysa 0 ve 0 elde var 0.
```

# String

```bash
# Substring İşlemi
foo="Osman"
echo ${foo:1:2}

# OUTPUT: sm
```

```bash
# Harf Sayma
foo="Osman"
echo ${#foo}

# OUTPUT: 5
```

```bash
# String Birleştirme
name="Rıdvan"
surname="Altun"
fullName=$ad$soyad
echo "Full Name: $fullName"

# OUTPUT: RıdvanAltun
```

# Array

```bash
# Array Tanımlamak
days=(Pazartesi Sali Carsamba Persembe Cuma)
echo ${days[0]}

# OUTPUT: Pazartesi
```

```bash
# Dizi Eleman Sayısı Döndürmek
days=(Pazartesi Sali Carsamba Persembe Cuma)
echo ${#days[*]}

# OUTPUT: 5
```

```bash
# Dizi Tüm Elemanları Yazdırmak
days=(Pazartesi Sali Carsamba Persembe Cuma)
echo ${days[@]}

# OUTPUT: Pazartesi Sali Carsamba Persembe Cuma
```

```bash
# Dizi'ye Eleman Eklemek
days=(Pazartesi Sali Carsamba Persembe Cuma)
days=(${days[*]} yeniEleman)
echo ${days[@]}

# OUTPUT: Pazartesi Sali Carsamba Persembe Cuma yeniEleman
```

```bash
# Array Substring
days=(Pazartesi Sali Carsamba Persembe Cuma)
echo ${days[@]:1:3}

# OUTPUT: Sali Carsamba Persembe
```

# Conditions

Sayısal Karşılaştırma Operatörleri:

- -eq -> equal -> esittir
- -ne -> not equal -> esit degildir
- -lt -> less then -> kucuktur
- -gt -> greater then -> buyuktur
- -ge -> greater equal then -> buyuk esittir
- -le -> less equal then -> kucuk esittir

```bash
# Sayısal Karşılaştırma Kullanılıyor
if [ $1 -eq 1 ];then
    echo "Sayi bire esit"
elif [ $1 -eq 2 ];then
    echo "Sayi ikiye esit"
else
    echo "Sayi bire veya ikiye esit degil"
fi
```

String Karşılaştırma Operatörleri:

- =, == -> esittir
- != -> esit degildir
- birde > ve < işareti var. Ascii karakter olarak karşılaştırıyor

```bash
# String Karşılaştırma Kullanılıyor
if["$x" == "$y"];then
    echo "İki string deger de esit"
fi
```

# Logical Operators

```bash
# OR (Veya) Kullanımı
if[$x -eq 50] || [$y -ge 80];then
    //kodlar..
fi

# AND (Ve) Kullanımı
if[$x -eq 50] && [$y -ge 80];then
    //kodlar..
fi
```

# File Operations

- -f (file) parametresi dosya olup olmadığını döndürür.
- -d (directory) parametresi klasör olup olmadığını döndürür.
- -r (readable) dosyanın okunabilir bir dosya olup olmadığını gösterir.
- -w (writeable) dosyanın yazılabilir olup olmadığını gösterir.
- -x (executable) dosyanın çalıştırılabilir olup olmadığını gösterir.
- -z dosyanın boş olup olmadığını döndürür.
- -h (hyperlink) dosyanın link olup olmadığını gösterir.

```bash
if [ -d /root/osman ];then
    echo "Osman diye klasör var"
else
    echo "Osman diye klasör yok"
    mkdir -p /root/osman
    echo "Ama ben olusturdum.."
fi
```

`if [ -d /root/osman -a ];then` dersek burada `-a` `ve` anlamı taşıyor `-o` kullansaydık `veya` anlamı taşıyacaktı. Bir if yapısı ile bir çok koşul kontrolü sağlayabiliyoruz bu parametreler ile.

```bash
if[! -r /home/tmp/abc.csv];then
    echo "Dosya okunabilir değil. Siliniyor!"
    rm -f /home/tmp/abc.csv
else
    echo "Dosya okunabilir bir durumda."
fi
```

Burada Görüleceği üzere ! işareti ile negatif durumları temsil edebiliyoruz.

```bash
# Klasördeki Tum .txt Uzantılı Dosyalara Bakılıyor
for file in "*.txt"
do
    // Kodlar..
done

# Klasördeki Tüm Dosya ve Klasörlere Bakılıyor
for file in *
do
    // Kodlar..
done
```

# Case

```bash
echo -n "1 ile 3 arasında sayi: "
read sayi
case $sayi in
    1) echo "bir girildi";;
    2) echo "iki girildi";;
    3) echo "uc girildi";;
    *) echo "1-3 arası sayi girilmedi";;
esac
```

Burada `;;` ifadesi `break` görevi görüyor.

# Loops

## For Loop

```bash
for i in 1 2 3
do
    echo $i
done

: '
OUTPUT:
    1
    2
    3
'
```

```bash
for i in ${1..99}
do
    echo $i
done

: '
OUTPUT:
    1
    ..
    99
'
```

```bash
x=3
for((i=0;i<$x;i++))
do
    echo $i
done

: '
OUTPUT:
    0
    1
    2
'
```

## While Loop

```bash
count=0
while[ $count -le 100];
do
    echo $count
    ((count++))
done

: '
OUTPUT:
    0
    ..
    100
'
```

## Until Loop

```bash
count=0
until[ $count -ge 100];
do
    echo $count
    ((count++))
done

: '
OUTPUT:
    0
    ..
    100
'
```

## Special Commands

```bash
# Döngüyü Başa Sarar
continue

# Döngüyü Bitirir
break
```

# Functions

```bash
# Void Fonksiyon
function writeSiteName(){
    echo "www.abc.com"
}
echo "Sitenin adresi nedir?"
writeSiteName

: '
OUTPUT:
    Sitenizin adresi Nedir?
    www.abc.com
'
```

```bash
# Değer Döndüren Fonksiyon
function sum(){
    return $(($1 + $2))
}
echo sum 3 5

# OUTPUT: 8
```

Görüldüğü üzere fonksiyon içindeki parametrelere $1 gibi ifadelerle erişiyoruz.

# Select

```bash
select gunsec in "Pazartesi" "Sali" "Carsamba"
do
    echo "Secilen gun $gunsec"
    break
done

: '
OUTPUT:
    1) Pazartesi
    2) Sali
    3) Carsamba
'
```

Daha Komplex Bir Çözüm:

```bash
select gunsec in "Pazartesi" "Sali" "Carsamba" "Cikis"
do
    case $gunsec in
        Pazartesi) echo "pazartesi secildi";;
        Sali) echo "sali secildi";;
        Carsamba) echo "carsamba secildi";;
        Cikis) break;;
        *)  echo "Yanlis bir secim yaptiniz..";;
    asec
done

: '
OUTPUT:
    1) Pazartesi
    2) Sali
    3) Carsamba
    4) Cikis
'
```

Burada pazartesi seçtik diyelim, kod break olmaz, pazartesi seçtikten sonra bu sefer 4 demek gerekiyor kodun devamı gelemsi için.

# Extra Commands

```bash
# Programdan Çıkmak
exit 1

# Bir Saniye Beklet
sleep 1

# Rasgele Sayı Üret
echo $(( ( RANDOM % 10 )  + 1 ))

# OUTPUT: 1 ile 10 arası ragele bir sayı
```

# Sh with -c flag

-c bayrağı ile verdiğimiz bir string içindeki komutları çağır diyebiliyoruz.

```bash
$ sh -c "ls --all && cd .."
```

Docker ile bunu çok kullanıyorlar çünkü direkt olarak komut execute edebiliyoruz.

# Resources

[Denizli HackerSpace "Bash Programlama Diline Giriş"](https://www.youtube.com/watch?v=l-ALk0j09XQ)