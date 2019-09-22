# Console Tricks

Terminal deneyimini arttırmak için tuttuğum notlar.

# Table of Contents

<!-- toc -->

- [Console Tricks](#console-tricks)
- [Table of Contents](#table-of-contents)
- [Tricks](#tricks)
- [Tab Managment](#tab-managment)
- [Shorcuts](#shorcuts)
- [Tmux](#tmux)
  - [Shorcuts](#shorcuts-1)
  - [Command Mode](#command-mode)
  - [Sessions](#sessions)
  - [Tmuxinator](#tmuxinator)
  - [Plugins](#plugins)
    - [Tmux Plugin Manager](#tmux-plugin-manager)
    - [Tmux Themepack](#tmux-themepack)
    - [Tmux Sensible](#tmux-sensible)
- [Windows](#windows)
  - [Cmder](#cmder)
    - [Shorcuts](#shorcuts-2)
    - [Config](#config)
      - [Alias](#alias)
      - [Clink](#clink)
    - [Startup Tasks](#startup-tasks)
    - [Theme](#theme)
      - [Blade Theme](#blade-theme)
    - [Context Menu Integration](#context-menu-integration)
    - [Terminal Encoding](#terminal-encoding)
    - [Start as Admin](#start-as-admin)
  - [Cygwin and Mingw](#cygwin-and-mingw)
  - [WSL](#wsl)
- [Console Programs](#console-programs)
  - [awk](#awk)
  - [cut](#cut)
  - [find](#find)
  - [grep](#grep)
  - [rev](#rev)
  - [sort](#sort)
  - [tar](#tar)
  - [tr](#tr)
  - [wc](#wc)
  - [tail and head](#tail-and-head)
- [Extra Console Programs](#extra-console-programs)
- [JSON, YAML, XML and HTML Processing](#json-yaml-xml-and-html-processing)

<!-- tocstop -->

# Tricks

Konsola `\` yazdığımız zaman aşağı satıra geçebiliyoruz.
Uzun komutları yazmak için tercih edilebilir.

```bash
echo "Hello" \
&& echo "World" \
&& ls
```

Bir programın çıktısını `clip` programıyla `clip board`'a kopyalayabiliyoruz. Aşağıda gösterildiği gibi.

```bash
echo Hello | clip
```

# Tab Managment

Linux ta birden fazla terminali aynı anda açıp kullanmak mümkün. Uzaktan bağlantı ile bağlandığımız bilgisayarlarda bu özellik olmazsa olmaz çünkü uzun süren işlerin yanında yeni bir tab açıp başka işlemler yapabilmemizi sağlıyor.

```
CTRL + Shift + T        -> Yeni tab aç.
CTRL + Shift + W        -> Tab'ı kapat.
CTRL + Pg Up            -> Tab değiştir.
CTRL + Pg Dn            -> Tab değiştir.
CTRL + Shift + Pg Up    -> Tab taşı.
CTRL + Shift + Pg Dn    -> Tab taşı.
```

# Shorcuts

```
CTRL + A                -> İmleci satır başına taşı.
CTRL + E                -> İmleci satır sonuna git.
CTRL + D                -> Terminali kapatır.
CTRL + L                -> Ekranı temizle.
CTRL + Right Arrow      -> Bir kelime sağa atla.
CTRL + Left Arrow       -> Bir kelime sola atla.

CTRL + R                -> Arama yapmamızı sağlar
CTRL + G                -> Aramadan çık

CTRL + U                -> İmleçten satır başına kadar olan kısmı kes.
CTRL + K                -> İmleçten satır sonuna kadar olan kısmı kes.
CTRL + Y                -> Yapıştır

ALT + F                 -> Sonraki kelimenin başına git.
ALT + B                 -> Önceki kelimenin başına git.

CTRL + X + E            -> Shellde uzun bir komut yazıyorsak,
                          mesela alt alta yazmak için buffer ekranı açabiliriz.
                          :wq yazarak kaydedip çıkılıyor.

FN + PageDown           -> 1 sayfa aşağı in
FN + PageUp             -> 1 sayfa yukarı çık
FN + Home               -> CTRL + A ile aynı işlevi görüyor
FN + End                -> CTRL + E ile aynı işevi görüyor
```

# Tmux

Anlamı `Terminal Multiplexer`. Linux üstünde kullanabileceğim bir araç. İster remote (SSH vs.) ister local makine terminalimizi özelleştirmek-geliştirmek için kullanabiliriz.

Terminale `tmux` yazarak programımızı başlatabilriiz.

## Shorcuts

Tmux programına komut girebilmek için öncelikle `prefix` tuşlaması yapmamız gerekiyor.

`CTRL + B` -> Prefix Key

Sonrasında tuşlayabileceğimiz şeyler;

```
C -> (Create) Terminal oluştur.
N -> (Next) Sonraki terminal.
P -> (Previous) Önceki terminal.
W -> Listele ve terminal seç.
% -> Güncel tarminali dikey olarak böl.
" -> Güncel tarminali yatay olarak böl.
: -> Komut modu.
```

`CTRL + D` -> (Detach) Terminali kapat.

## Command Mode

Shortcut ile girdimiz komutlar ve daha fazlasını komut modu ile yapmamız mümkün.

Horizantal Modda bölmek için command moda geçip 'split-window' komutu verebiliyoruz mesela.

## Sessions

Session oluşturmak mümkün, yani uzun süreli çalışacak bir komut yazdık mesela, bunu arka plana atıp bir theread gibi çalışabilir:

```bash
# Sension oluşturmak
tmux new -s sessin_name_demo

# Session'ları Listelemek
tmux list sessions
```

## Tmuxinator

[Bu proje](https://github.com/tmuxinator/tmuxinator) ile tmux profili oluşturup çağırabiliriz.

## Plugins

Tmux'a eklenti ekleyerek özelliklerini çoğaltabiliriz.

### Tmux Plugin Manager

Eklenti yükleme işini otomatize etmek için [bu tmux eklentisini](https://github.com/tmux-plugins/tpm) kullanabiliriz.

Home dizini içinde `.tmux.conf` adlı bir dosyanın içine kurulacak eklentileri yazıyoruz, `CTRL + B` sonra `I` tuşladığımız zaman eklentiler yükleniyor. Eklentiyi silmek için `.tmux.conf` dosyası içinden eklentiyi çıkartıp `CTRL + B` sonra `ALT + u` tuşlayarak silebiliyoruz.

Kısaca `CTRL + B` sonrası -->

```
I         -> .tmux.conf dosyasında tanımlanmış eklentiler yükleniyor.
U         -> eklentiler update ediliyor.
Alt + u   -> .tmux.conf içinden çıkardığımız eklentiler kaldırılıyor.
```

### Tmux Themepack

[Bu eklentide](https://github.com/jimeh/tmux-themepack) bir sürü tema hazırlanmış, 2 satırda istediğimizi seçebiliyoruz.

### Tmux Sensible

[Bu eklentide](https://github.com/tmux-plugins/tmux-sensible) basit ve yapılması gereken ayarları tek bir eklentide toplayıp sunmuşlar. Amaç, basit şeyleri tek eklentide toplayıp eklenti karmaşasını ve kalabalığı engellemek.

# Windows

Linux kullanıcıları için terminal vazgeçilmez ancak Windows kullanıcıları genelde terminal kullanmayı tercih etmiyor, Windows içinde terminal kullanmak çok daha zor ve zevksiz. Bu olayı zevkli hale getirmek ve işlerimizi kolaylaştırmak için kullanabileceğimiz yazılımlar var.

## Cmder

Bu proje aslında ConEmu denen başka bir projenin forku. 
Bu program ile bazı Unix programları gömülü olarak geliyor, yani Cmder terminali üstünden bazı linux programlarını çalıştırabiliyoruz.

Cmder'i etkili kullanmak için Cygwin, Mingw yada WSL yüklemek mümkün.

Neden bu terminali kullanayım?

- Konsol ekranını bölebiliyoruz.
- Sekmeler şeklinde çalışabiliyoruz.
- Renklendirmesi güzel.
- Alias atayıp komplike ve rutin bazı şeyleri kolaylaştırabiliyoruz.
- Bazı Unix programları yanında geliyor.
- Komut tamamlama özelliği ve diğer Clink özellikleri.

### Shorcuts

```bash
CTRL + "         -> Alta indirmek ve yukarı çıkarmak.
CTRL + T         -> Yeni Konsol Açma Kısayolu.
CTRL + W         -> Aktif tab'ı kapatmak.
CTRL + 1         -> Birinci Tab'a geçer.
CTRL + Tab       -> Sıradaki tab'a geçer.
ALT + ENTER      -> Tam Ekran.
CTRL + 1         -> 1. Ekranı açar, 0-> son açılan ekranı açar.
```

### Config

Bütün ayar dosyaları `Cmder\config` klasöründe tutuluyor.

#### Alias

Terminalin ayar dosyaları içinden kendimize alias ayarlayıp ileride yazdığımız uzun kodları kısa hale getirmemzi mümkün. Bunun için `Cmder\config\user_aliases.cmd` adlı dosyayı düzenlememiz gerekiyor, bu dosya Cmder açılırken `Cmder\vendor\init.bat` tarafından çalıştırılıyor.

Gelen `alias`'lar:

- e.=explorer .
- gl=git log --oneline --all --graph --decorate  $*
- ls=ls --show-control-chars -F --color $*
- pwd=cd
- clear=cls
- history=cat "%CMDER_ROOT%\config\.history"
- unalias=alias /d $1
- vi=vim $*
- cmderr=cd /d "%CMDER_ROOT%"

#### Clink

Bu programa Clink adında bir program entegre edilmiş. Bu program termianldeki bir çok özelliği sağlayan şey aslında. 

Sağladığı şeyler:

- Komut satırı düzenleme özellikleri.
- Komut geçmişi.
- Komut tamamlama.

Bu programa `LUA` dilinde ektra scriptler yazarak genişletmek ve özelleştirmek mümkün.

### Startup Tasks

TODO

### Theme

Programın temasını özelleştirip bize hitap bir şekle getirmek mümkün.

#### Blade Theme

[Github](https://github.com/namanUIUC/Blade-Cmder-Theme)

Bu tema hoşuma gidiyor, bunu kullanıyorum.

### Context Menu Integration

istediğim klasör içinde sağ tıklayıp Cmder konsolu açmak için yapmam gerekenler:

1. Windows standart command prompt'u admin olarak çalıştır.
2. Cmder'i yüklediğiniz klasöre gir.
3. `.\cmder.exe /register all` komutunu çalıştır.

### Terminal Encoding

Yazdığımız bir program türkçe karakter döndürdüğünde konsol karakter setini tanımadığı için hata veriyor. Örneğin Python ile türkçe karakter return eden bir program yazdığımızda bu sorunla karşılaşıyoruz.

`Settings > Envoriment` diyerek kutuya `chcp utf8` satırını eklersek artık tükçe karakterler sorun çıkarmayacaktır.

### Start as Admin

Programın admin olarak başlatılmasını sağlamak için `Settings > General` sekmesinde ilk gördüğümüz pencereden ayarlayabiliriz.

## Cygwin and Mingw

Bazı linux programlarını windows sisteminde kullanabilmek için bu programları kurup kullanabiliyoruz. Alternatifi WSL.

## WSL

`Windows Subsystem for Linux` olarak geçiyor. Windows içinde bir terminal arayüzüyle Linux kullanma deneyimi elde ediyoruz. Kişisel olarak Linux yerine bunu kullanıyorum çünkü bir bilgisayarda hem Windows hem Linux avantajlarını birlikte kullanma fırsatı sunuyor.

# Console Programs

Bu programların çoğu Linux sistemlerde mevcut. Gerekli binary'leri edindikten sonra veya `Cygwin`, `Mingw` gibi çözümlerle bu programları Windows içinde kullanabilmek mümkün.

## awk

Bu programın adı kendisini yazan 3 yazılımcıların adının ilk harflerinden geliyor. İngilizce bir çevirisi yok bu yüzden.
Bu program kelime işlemek için bize bir dil sunuyor.

```bash
cat dosya.txt

: '
Ad Soyad Yaş Şehir Cinsiyet
Burcu Yılmaz 33 Kars

Murat Durmuş 27 Antalya
   30 K
'

awk '{print $1}' ~/dosya.txt

: '
Ad
Burcu

Murat
30s
'

awk '{print $1 $2}' ~/dosya.txt # Bu komut ile ad soyad birlikte yazdırabiliriz

# NR ile satır sayısı
# NF ile kelime sayısı

awk '{print NF}' ~/dosya.txt # Satırlardaki kelime sayıları

: '
5
0
4
2
'

awk '{if (NR!=1) {print}}' ~/ornek.txt # satır sayısı 1 olmayanların çıktısı

```

## cut

```bash
cat dosya.txt

: '
Ali, 50, 49
Veli, 78, 10
Ayşe, 40, 40
'

cut -d ',' -f 1 dosya.txt

: '
Ali
Veli
Ayşe
'

cut -f 3 dosya.txt

: '
49
10
40
'

cut -f 1,3 dosya.txt

: '
Ali, 49
Veli, 10
Ayşe, 40
'

cut -f 1-3 dosya.txt

: '
Ali, 50, 49
Veli, 78, 10
Ayşe, 40, 40
'

# Burada -f parametresi field-alan belirtir, -d parametresi ise strip edecegimiz karakteri gosterir.
```

## find

```bash
# Bulundugumuz konumda ki abc ile baslayan dosyaları listele
find . -type f -name 'abc*'

# Bulundugumuz konumda ki, İzinleri 777 olan dosyaları listeler
find . perm 777

# Home klasöründe adı abc ile baslayan klasörleri listeler
find ~ type d -name 'abc*'

# Bulundugumuz konumda ki bos dosya, klasörleri siler
find . -size 0 -delete

# Bulundugumuz konumda ki 20M'den buyuk olan dosyaları listeler
find . -type f -size +20M 

# Uzantısı php olmayan dosyaları listeler
find . -not -name '*.php'
find . !-name '*.php'
```

## grep

```bash
# nor ile başlayan satırları listele
grep '^nor' dosya.txt

# nor ile biten satırları listele
grep 'nor$' dosya.txt

# bos satırları listele
grep '^$' dosya.txt

# bos satırları say
grep -c '^$' dosya.txt

# harf ile baslayan satırları listele
grep '^[a-zA-Z]' dosya.txt

# buyuk harf ile baslayan satirlari listele
grep '^[A-Z]' dosya.txt

# nor içermeyen satırları listele
grep -v 'nor' dosya.txt
```

## rev

```bash
# Dosyayı tersten yazdırır
cat dosya.txt | rev
```

## sort

```bash
# Satirlari kucukten buyuge siralar
sort dosya.txt

# Satirlari buyukten kucuge siralar
sort -r dosya.txt

# Birbirinden farklı satırları kucukten buyuge siralar
sort dosya.txt | uniq

# u parametresi uniq programı ile aynı işi yapıyor
sort -u dosya.txt
```

## tar

```bash
# Birden fazla dosyayı paketleme
# Buradaki c'yi compress gibi düşünürsek akılda kalıcı olur
tar -zcvf paket.tar dosya1.txt dosya2.txt

# Sıkıştırılmış dosya açmak, -xf ile farkı nedir bilmiyorum
# Buradaki x'i execute gibi düşünürsek akılda kalıcı olur
tar -zxvf paket.tar dosya1.txt dosya2.txt

# Dosyalar klasörünü sıkıştırdık
tar -cf yedek.tar /dosyalar 

# Sıkıştırılmış dosya açmak
tar -xf yedek.tar
```

## tr

tr -> translate

```bash
# Ali kelimesini veli yaptık
tr 'ali' 'veli' dosya.txt

# Büyük harfleri küçük harfe çevir
tr [:upper:] [:lower:]
```

## wc

wc -> word count

```bash
# Dosyanın kaç satırı var. -l -> line
wc -l dosya.txt

# Dosyanın kaç harfi ve kelimesi var
wc dosya.txt
```

## tail and head

```bash
# Dosyanın ilk 5 satırını alır
head -5 dosya.txt

# Dosyanın son satırını alır
tail -1 dosya.txt
```

# Extra Console Programs

Çok fazla konsol programı var, çoğu basit ve kısa işler yapıyor, bunlar için tek tek başlık açmak yerine gerekli gördüklerimi başlık halinde ayrıntılı bir şekilde anlattım, geri kalan ve kullanımı görece basit olan programlar bu başlık altında toplandı.

```bash
diff        <-- iki dosya arasındaki farkları gosterir
uniq        <-- birbirinden farklı seyleri listelemek icin kullanılır
less        <--
more        <--
printenv    <-- sistemdeki kayıtlı degiskenleri yazdırır
cat a.txt   <-- dosyadaki veriyi print eder
pwd         <-- bulunduğum dizini gösterir
whoami      <-- şuanda ki aktif kullanıcının id sini döndürür
man echo    <-- echo programı hakkında bilgi veren detaylı bir doküman döndürür
free        <-- boştaki ram miktarını dondurur
cal         <-- takvim dondurur
shutdown    <-- bilgisayarı kapatır
time        <-- sistem saatini dondurur
du          <-- disk usage, bütün klasör-dosyaların kullandığı alan list
df          <-- ??
dig google.com <-- DNS lookup, DNS sunucusu biz google.com a girince ne döndürüyor
man tool_name  <-- manual, ismini verdiğimiz aracın kullanım detaylarını verir
```

# JSON, YAML, XML and HTML Processing

`jq` programıyla elimizdeki `.json` çıktısını renklendirme ve çıktıdan istediğimiz veriye kolayca erişebilmek.

```bash
# Çıktıyı renklendiriyoruz
cat test.json | jq .

# İlk elementi al
cat test.json | jq .[0]

# İkinci elementi al
cat test.json | jq .[1]

# Tüm alt elementlerin email değerini yazdır
cat test.json | jq .[].email

# "abc@gmail.com"
# "xyz@example.com"

# Tüm alt elementlerin email değerini raw şekilde yazdır
cat test.json | jq -r .[].email

# abc@gmail.com
# xyz@example.com

# Yeni bir JSON çıktısı oluşturduk
cat test.json | jq '.[] | { Name: .name, Surname: .surname }' | jq -s
```

Yaml

```bash
# Çıktıyı renklendir
cat test.yml | yq . -y

# Yaml dosyasını özel bir json'a çevirip yazdırdık
cat test.yml | yq '.[] | { Name: .name, Email: .email }'

# Yaml dosyasını özel bir yaml dosyasına çevirip yazıdrdık
cat test.yml | yq '.[] | { Name: .name, Email: .email }' -y
```

Mantığı çok basit. `XML` için de `xq` adlı bir program var. Mantığı tamamen `jq` ve `yq` ile aynı. XML çıktılar almak için `xq` ile `-x` parametresi kullanıyoruz sadece.

HTML içinde `pup` adında bir program var. Bu araç ile HTML sayfasını parçalayıp istediğimiz verileri komut satırında alabiliriz.

```bash
# Eksisozlukteki gundem basliklarinin linklerini getirir
curl -sL eksisozluk.com | pup 'ul[class="topic-list partial"]' | pup 'li a attr{href}'

# Çıktıyı JSON'a çevirdik
curl -sL eksisozluk.com | pup 'ul[class="topic-list partial"]' | pup 'li a json{}' | jq .[] | { count: .children[0].text, href: .href } | jq -s 'sort_by(.count | tonumber) | .[]'

# {
#  "count": "11",
#  "href": "/abc?a=popular"
# }
# {
#  "count": "12",
#  "href": "/xyz?a=populer"
# }
```