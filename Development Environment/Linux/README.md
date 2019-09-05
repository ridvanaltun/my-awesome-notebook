# Linux

Bu dokümanda kişisel linux deneyimimi paylaşıyorum.

# Table of Contents

<!-- toc -->

- [Linux](#linux)
- [Table of Contents](#table-of-contents)
- [LTS](#lts)
- [Grub](#grub)
  - [Config](#config)
- [Linux Folder Structure](#linux-folder-structure)
- [User Management](#user-management)
- [Permission Management](#permission-management)
- [Package Management](#package-management)
  - [Installation](#installation)
  - [Upgrade](#upgrade)
  - [Remove](#remove)
- [File Commands](#file-commands)
- [System Commands](#system-commands)
- [Super User](#super-user)
- [Command Operators](#command-operators)
- [Port Forwarding](#port-forwarding)
- [Linking](#linking)
  - [Symbolic Link](#symbolic-link)
  - [Hard Link](#hard-link)
- [Hostname](#hostname)
- [Bin](#bin)
- [Run Script on Startup](#run-script-on-startup)
  - [Method 1: rc.local](#method-1-rclocal)
  - [Method 2: .bashrc](#method-2-bashrc)
  - [Method 3: init.d](#method-3-initd)
  - [Method 4: crontab](#method-4-crontab)

<!-- tocstop -->

# LTS

`Long term support` anlamına geliyor. Bir linux distro dağıtımını yüklerken bu versiyonu yüklemek gerek stabilite için.

# Grub

Grub nedir? Bilgisayarımızı `dual-boot` kullanacaksak eğer bu yazılım ile bilgisayarı hangi işletim sistemi ile başlatabileceğimizi seçebiliyoruz. Grub, Ubuntu ile beraber gelen bir yazılım.

## Config

Grub ekranındayken, `ALT+ F2`, `gksudo gedit /etc/default/grub`

```
GRUB_DEFAULT=0
GRUB_HIDDEN_TIMEOUT=0
GRUB_HIDDEN_TIMEOUT_QUIET=true
GRUB_TIMEOUT=10
GRUB_DISTRIBUTOR=`lsb_release -i -s 2> /dev/null || echo Debian`
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"
GRUB_CMDLINE_LINUX=""
```

Burada `GRUB_DEFAULT` ayarı ile boot edilecek işletim sistemini seçmek mümkün.
`GRUB_TIMEOUT` ile kaç saniye içinde boot işlemi olacak seçmek mümkün.

Değişikliği kaydetmek için `CTRL + S` ardından çıkmak için `CTRL + Q`

`CTRL + ALT + T ` ile terminal açıp değişikliği kalıcı hale getirmek için `sudo update-grub` komutunu giriyoruz.

# Linux Folder Structure

```
/bin    : Kritik komut dosyalarını içerir
/boot   : Başlangıç için gerekli dosyaları bulundurur
/dev    : Donanım dosyaları vardır
/etc    : Sistem ayarlarını barındırır, kullanıcı ve şifreler vs.
/lib    : Kütüphane dosyaları ve kernel modülleri bulunur 
/media  : CD-Rom, Flash bellek vs. sisteme eklendiği klasördür.
/mnt    : Bir dosya sistemini geçici olarak eklemek için kullanılır. 
/opt    : Program Files gibi bir yapı, 3. parti uygulamalar buraya yüklenir
/sbin   : Sistemi yöneticisiyle ilgili çalıştırabilir dosyaları tutar.
/srv    : Sistemin sunduğu hizmetlerle alakalıdır
/tmp    : Geçici dosyaları tutmak içindir, OS açılırken burası silinir
/usr    : İkincil bir hiyerarşi 
/var    : Değişken verileri saklar, log dosyaları burada bulunur
/home   : kullanıcımızın ana dizinidir
/root   : root kullanıcısının ana dizinidir
```

# User Management

```bash
usermode -L santa               # Lock User
usermode -I cruz santa          # User Rename
usermode -p 1234 santa          # Change Password
usermode -s /bin/csh/ santa     # Change Shell
usermode -d /home/santa santa   # Change Home Directory
usermode -a -G sudo santa       # Make user sudo(superuser do)
usermode -u santa               # Unlock User

useradd -m santa                # Create User
adduser santa                   # Create User
useradd -m santa -G office      # Create User the Add It A Group

userdel santa                   # Delete User
deluser santa                   # Delete User
deluser santa office            # Exit Group
deluser -f santa                # Force Delete User (If Is Group Member)

Groupadd office                 # Create Group
gpasswd office 1234             # Change Group Password
Groups office                   # Show Members Of Group's
Groupdel office                 # Delete Group

su santa                        # Login As User
sudo -i                         # Login As Sudo

ssh santa@192.168.1.80          # SSH Connection
```

# Permission Management

```
User - Group - Others

rwx  -  rwx  - rwx
111  -  111  - 111
7    -  7    - 7

r--  -  rw-  - rw-
100  -  110  - 110
4    -  6    - 6
```

- r = read
- w = write
- x = execute

Binary to decimal hesabı yaparak izinleri ayarlıyoruz

- chmod 777 dosya.txt <-- bu sekilde bir kullanım mevcut
- chmod u+w dosya.txt <-- dosyanın user write hakkı true yapıldı
- chmod g-x dosya.txt <-- dosyanın group execute hakkı false yapıldı
- chmod +x  dosya.txt <-- tüm kullanıcılara execute hakkı verdik

# Package Management

Paket yükleme, yükseltme ve silme işlemleri için kullandığımız komutlar.

## Installation

```bash
# Paket deposundan paketi yükler
sudo apt-get install packet-name

# Kullanımı dogru
sudo apt-get install paket1 paket2 paket3

# Cikan Yes-No lara hep Yes de
sudo apt-get install paket1 paket2 paket3 -y
```

## Upgrade

```bash
# Paket kaynaklarını güncellemek
sudo apt-get update

# Kurulu paketleri güncellemek
sudo apt-get upgrade

# Sistemi komple guncellemek
sudo apt-get dist-upgrade
```

## Remove

```bash
# Uygulamayı kaldırmak
sudo apt-get remove packet-name

# Uygulamayı konfigure dosyalarıyla kaldırmak, ancak bütün konfigure dosyaları kaldırılmaz
sudo apt-get purge packet-name

# Kullanılmayan bütün bağımlılıkları siler
sudo apt-get autoremove
```

# File Commands

```bash
# home klasörüne gider
cd ~

# root klasörüne gider
cd /

# önceki klasöre gider
cd..

# siliyoruz
rm dosyaadi.txt

# klasorun altindaki tum dosyalari silmek için -r, silmeye zorlamak için -f (force) parametresi kullanıyoruz
rm -rf /klasor/*

# bu sekilde dosya1 in adini dosya2 yaptik
mv dosya1.txt dosya2.txt

# dosyayi home > calisma dizinine tasidik
mv dosya.txt ~/calisma

# dosyayi home > calisma dizinine kopyaladik
cp dosya.txt ~/calisma

# tmp klasörünü home > calisma dizinine kopyaladik
cp -R /tmp ~/calisma
```

# System Commands

```bash
# Sistemi baştan başlat
reboot

# Sistemi kapatmak için bir task ayarlar, 30-40 saniye sonra sistem kapanır
shutdow

# Planlanan kapatma işlemini iptal ediyor
shutdow -c

# Anında kapatıyor, sanırım plansız kapatıyor (denemem lazım)
shutdown now

# Sistemi çok daha hızlı kapatır, yapılması gereken çoğu şeyi atlıyor
poweroff
```

# Super User

```bash
# super user ile giriş yap
sudo -s

# son çalıştırılan komutu sudo ön ekiyle çalıştırır
sudo !!
```

# Command Operators

Terminalde bir komutun döndürdüğü değeri başka bir komuta aktarmak mümkün.

- Operator: < 
- Bir dosyayı bir komuta yönlendirmek için kullanılır.

```bash
# dosyanın kelime sayısını ksayi.txt'ye yazar
wc dosya.txt < ksayi.txt
```

- Operator: >
- Bir dosyayı bir komuta yönlendirmek için kullanılır.

```bash
# dosyanın kelime sayısını ksayi.txt'ye yazar
ksayi.txt > wc dosya.txt
```

- Operator: >> veya <<
- String bir çıktıyı bir dosyanın sonuna eklemek için kullanılır.

```bash
# dosyanın kelime sayısını ksayi.txt'ye ekler
wc dosya.txt << ksayi.txt
```

- Operator: | 
- Bir komutun çıktısını başka bir komuta aktarır.
- Linux dünyasında `Pipline` olarak geçiyor.

```bash
# dosyanın satır sayısını print eder
cat dosya.txt | wc - l
```

# Port Forwarding

```bash
sudo iptables -t nat -A PREROUTING -p tcp --dport 443 -j REDIRECT --to-port 8096

# 8096 portunu 443'e yönlendirdik.
# Ancak kalıcı değil, iptables kullanarak kalıcı yapmak gerekiyor bu kuralı.
```

# Linking

TODO

## Symbolic Link

- Windows üstünde dosya veya klasörler için oluşturduğumuz shortcut'ın aynısı. 
- Dosyayı farklı yerlerde kullanmak isterken kopya oluşturmak yerine sembolik link oluşturuyoruz.

```bash
# Bu şekilde hem dosya hem klasör linki oluşturabiliriz. 
ln -s /dizin/dosya.txt /linkin/olusturulacagi/dizin/dosya.txt

# Bu şekilde linki silebiliriz.
unlink dosya.txt
```

## Hard Link

- Hardlink genellikle güvenlik, yedekleme için kullanılıyor. 
- Bir hardlinki sildiğimizde diğeri sapasağlam yerinde kalır.

```bash
# Bu şekilde linkleme yaptığımız zaman dosya birebir kopyalanıyor, yaptığımız tüm değişiklikler kopyalanıyor.
ln /dizin/dosya.txt /linkin/olusturulacagi/dizin/dosya.txt
```

# Hostname

Local bir ağda biz local ip adresimiz yerine tanımlayabilen isim.

> hostname

Komutu ile adımızı görmemiz mümkün.

```bash
# hostname değiştir

hostname -s yeni_ad

# veya

# /etc/hostname içindeki değeri değiştirebiliriz.
```

Yaptığımız ayarın geçerli olabilmesi için bilgisayarı baştan başlatmamız gerekiyor, yada servis olan network'u baştan başlatmamız lazım.

> sudo service network restart

# Bin

Neden bin dizinini anlatıyorum? Sebebi çok basit, distronun reposunda bulunmayan, ev dizinine indirdiğimiz programları terminalden tek kelime ile çağırabilmek için bin dizini kullanılıyor.

Bin şeklinde birden fazla dizin var;

```bash
# Sistem başlangıcında ilk olarak burası çalıştırılır, 
# Çok temel, hayati linux programaları bu dizin içinde tutuklur. 
# rm, cp, mkdir, bash, ls gibi.
/bin 

# Hayati olmayan programlar için bu dizin kullanılıyor. 
# ping, tar, unzip, sudo gibi. 
# Ayrıca distroya bir program yükleyince bir sembolik link ekleniyor 
# ki programa tek kelime ile terminalden erişelim.
/usr/bin

# Sadece root kullanıcısının çalıştırabildiği üst düzey programlar burada yer alıyor.
/sbin
```

# Run Script on Startup

Linux açılırken bir program nasıl otomatik olarak başlatılabilir bunu irdeliyor olacağım.

## Method 1: rc.local

Bu yöntem ile hangi kullanıcı sisteme girerse girsin çalıştırılacak scriptleri koyuyoruz.

```bash
# Dosyayı değiştirmek için vim ile açıyoruz
sudo vim /etc/rc.local
```

Burada `exit 0`'dan bir önceki satıra satırımızı ekleyebiliriz. 

- Eğer komutun sonuna ampersan `&` işareti koyarsak bunun anlamı script sürekli çalışacak, bir kez çalışıp kapanmayacak.
- Eğer sona ampersan koymazsak ve program sürekli dönüyorsa; program hiç çalıştırılmaz.

```bash
sudo python /home/pi/sample.py &
```

Eğer boot ederken bir hata alırsak diye bir log dosyası oluşturmasını söyleyebiliriz;

```bash
sudo python /home/pi/sample.py & > /home/pi/Desktop/log.txt 2>&1
```

İşlemin gerçekleşmesi için sistemin yeniden başlatılması lazım.

> sudo reboot

## Method 2: .bashrc

Bu yöntemde kodun çalışacağı durumlar:

- Sistem boot edildiğinde
- Her yeni ssh bağlantısı kurulduğunda
- Her yeni terminal açıldığında

```bash
# Dosyayı değiştirmek için vim ile açıyoruz
sudo nano /home/pi/.bashrc
```

Son satıra aşağıdaki kodu ekleyebiliriz.

```bash
echo Running at boot 
sudo python /home/pi/sample.py
```

İşlemin gerçekleşmesi için sistemin yeniden başlatılması lazım.

> sudo reboot

## Method 3: init.d

`init.d` bir klasör. Bu yöntem ile script, sistem boot olduğunda çalışır.

Aynı zamanda bu klasör içinde sistem restart veya shutdown olurken çalışacak programlar çalıştırılır. Biz seçiyoruz hangi durumda çalışacağını.

[Detaylı Döküman](https://wiki.debian.org/LSBInitScripts)

```bash
# betiğimizi gerekli klasöre taşıdık
sudo cp /home/pi/sample.py /etc/init.d/

# betiğimizi değiştirmek için açıyoruz
sudo vim /etc/init.d/sample.py
```

Bir python script'i init.d methodu için nasıl başlamalı?
    
```
#!/usr/bin/env python
# /etc/init.d/sample.py
### BEGIN INIT INFO
# Provides:          sample.py
# Required-Start:    $remote_fs $syslog
# Required-Stop:     $remote_fs $syslog
# Default-Start:     2 3 4 5
# Default-Stop:      0 1 6
# Short-Description: Start daemon at boot time
# Description:       Enable service provided by daemon.
### END INIT INFO
```

```bash
# İzin veriyoruz
sudo chmod +x sample.py

# Gerekli bir komut
sudo update-rc.d sample.py defaults

# İşlemin gerçekleşmesi için sistemin yeniden başlatılması lazım.
sudo reboot
```

## Method 4: crontab

Bu bir program, service olarak arka planda çalışıyor. Bu programa her çarşamba saat 12'de şu işi yap diye direktif verebiliyoruz.

```bash
# crontab ayarı yapmak için bu komutu girmek yeterli, text editörü soracak
sudo crontab -e

# eklenebilecek örnek bir satır, hem start-up hem reboot sonrası calisir
@reboot python /home/pi/pytest.py > /home/pi/log.txt
```

Dikkat!: Crontab yazarken sudo kullanılması tavsiye edilmiyor, bunun yerine root kullanıcısına bir crontab ekleyip sudo kullanmadan yazılabilir, uygulaması çok basit, "sudo crontab -e" komutu ile crontab scripti yazarken sudo kullanmamıza gerek kalmıyor.