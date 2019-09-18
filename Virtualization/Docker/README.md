# Docker

Docker, imajın tamamını indirmez, diff sonrası değişiklik yapılan yerleri indirir ve yeni bir ad ile merge eder. Bunun anlamı ubuntu imajı 300mb olsa bile yeni sürümü çıktığında bazen 20mb gibi düşük miktarda indirme yapar.

Docker kendi içinde veri tutmaz, tüm ekstra konfigure dosyaları vs. biz konfigure dosyasına yazar ve işaret ederiz. Hedef gösterdiğimiz dosyalar mount edilir. Output almak istiyorsak yine kendi bilgisayarımızda bir klasöre gel şuraya yaz diyebiliriz.

# Table of Contents

- [Docker](#docker)
- [Table of Contents](#table-of-contents)
- [Image vs Container](#image-vs-container)
- [Docker Compose](#docker-compose)
- [Dockerfile](#dockerfile)
  - [Instructions](#instructions)
    - [RUN](#run)
    - [CMD](#cmd)
    - [EXPOSE](#expose)
    - [ADD](#add)
    - [WORKDIR](#workdir)
    - [HEALTHCHECK](#healthcheck)
- [Share Image To DockerHub](#share-image-to-dockerhub)
- [Some Commands](#some-commands)
- [Create Docker Image From A Container](#create-docker-image-from-a-container)
- [Docker Network Structure](#docker-network-structure)
- [Docker Compose](#docker-compose-1)

# Image vs Container

Image'i ayağa kaldırdığımız konteynır oluyor. Bir image ile 10 tane konteynır ayağa kaldırabiliyoruz mesela.

`docker ps -a` tüm konteynırlarımızı gösterir.

`docker images` tüm imagelerimizi gösterir.

`Dockerfile` ile image yaratıyoruz.
`docker-compose` ile konteynırlarımızı kolayca yöntebiliyoruz.

# Docker Compose

`docker-compose.yml` dosyası oluşturarak içine yazdığımız ayarlar ile `docker-compose up` komutu ile kontainer ayağa kaldırmak mümkün. `docker-compose down` komutu ile aktif sunucuyu kapatmak mümkün.

`docker-compose exec <active-container-name> sh` bu şekilde arkada çalışan kontainera sh programı ile erişmek mümkün, istersek `bash` ta kullanabiliriz. Bu sadece bazı kontainerlarda geçerli. Bu işi yalın `docker` programıyla da yapmak mümkün ancak `docker-compose` kullanmak daha basit. Hatta `docker-compose` tüm işlerini docker programı üstünden hallediyor.

Aşağıdaki örnek `docker-compose.yml` dosyası ile nginx serverini 8080 portu üstünden kaldırabiliyoruz.

```yml
web:
 image: nginx:latest
 ports:
 - "8080:80"
```

Compose yaparken verdiğimiz `volume` bölümü yazdığımız dosyaları kontainer'ın dosya sistemine yazıyor. Sol tarafta kendi yazdığımız ayarlar sağ tarafta kontainer'ın dosyası. Bu şekilde dosya üzerine yazabiliyoruz ve istediğimiz ayarları geçerli kılabiliyoruz.

Environment başlığı altında konteynırın kullanacağı değişkenleri tanımlayabiliyoruz. Örneğin default mysql root şifresi yerine bunu değişken tanımlayarak ayarlayabiliriz. Kullandığımız imajın dokümanda kullanabileceğimiz değikenler yazar.

# Dockerfile

Bu dosya `Makefile`'a çok benziyor. `docker` programını kullanarak bu dosyayı kullanarak build yapabiliyoruz. Build yaparken `.inc` uzantılı dosyalar ile parametreler belirtebiliyoruz.

Bu dosyayı yazarken içinde kullanabileceğimiz özel komutlar var. 

```
# Örnek bir Dockerfile dosyası
FROM httpd:2.4-alpine
MAINTAINER Rıdvan Altun <ridvanaltun@outlook.com>
COPY ./ /usr/local/apache2/htdocs/
```

Örnek olarak bu Dockerfile'da kısacak `httpd:2.4-alpine` konternırını al ve içine kök klasörde ne varsa `/usr/local/apache2/htdocs/` içine kopyala diyoruz.

`2.4-alpine` aslında konternırın tag'i, istediğimiz versiyonu tag üstünden kullanabiliyoruz . Imajın tamamını `DockerHub` denen platformdan aldık. 

İleride DockerHub'a imajımızı push ettiğimizde `maintainer` kısmı kullanılacak.

`docker build .` komutu ile konteynırımızı oluşturabiliriz.

## Instructions

`Dockerfile` içinde instruction'lar giriyoruz.

### RUN
```
# Imajın konsolunda komut çalıştırdık
RUN apt-get install -y iputils-ping
```

### CMD

Imaj ayağa kalkarken otomatik olarak çalıştırılacak komut, Dockerfile için sadece bir tane CMD talimatı bulunabilir
CMD talimatını yazmanın 3 farklı yöntemi var

```
# 1. yöntem
CMD [ "executable", "param1", "param2" ]

# 1. yönteme örnek
CMD [ "/bin/ping", "8.8.8.8" ]

# 2. yöntem
ENTRYPOINT [ "executable" ]
CMD [ "param1", "param2" ]

# 2. yönteme örnek
ENTRYPOINT [ "ping" ]
CMD [ "-help" ]

#3. yöntem
CMD command param1 param2

# 3. yönteme örnek
CMD ping 8.8.8.8
```

`ENTRYPOINT` kullanımının bir detayı var, Bununla birlikte docker ayağa kalkarken çalıştırılacak programın parametresini kullanıcı imajı ayağa kaldırırken verebiliyor, vermezse default olarak CMD içine yazılmış parametreler kullanılıyor.

`docker run ridvanaltun/myubuntu:0.1 8.8.8.8`

Şeklinde çalıştırırsak mesela ENTRYPOINT argüman olarak verdiğimiz argümanı alır ve ona göre çalışır.

### EXPOSE

// TODO

### ADD

3 farklı yazım şekli var

```
ADD [ "./BuildDir/" "/app/bin"]
ADD [ "./BuildDir/", "/app/bin"]
ADD [ "https://curl.haxx.se/download/curl-7.50.1.tar.gz", "/tmp/curl.tar.gz" ]
```

İnternetten veya local ortamımızdan bir dosyayı imajın içine koyabiliyoruz.
`.dockerignore` şeklidne bir dosya oluşturup içine konmamasını istediğimiz dosya ve klasörleri özel oalrak belirtebiliyoruz. Bu komutun fonksiyonneliği COPY ile aynı ancak ekstra olarak internetten dosya indirme özelliği var.

### WORKDIR

Bu talimat Dockerfile içinde bulunan diğer taliamtları etkiler.

```
WORKDIR /app/src
```

Örneğin `ADD [ "./BuildDir/", "binaries/"]` talimatı çalıştırılacak, imajın yolu aslında şu olur `binaries/app/src`

### HEALTHCHECK

// TODO

# Share Image To DockerHub

` docker tag a748835505b2 ridvanaltun/myubuntu:0.1`

`a748835505b2` numarası build ettiğimiz docker imajının ID'si.

Imaj isimleri karışmasın diye `ridvanaltun/myubuntu` şeklinde yazdık, başında DockerHub kullanıcı ID'imiz var, en sona da `0.1` ekledik buda bizim versiyon adımız.

Yaptığımız konternırı paylaşmak için önce login olmamız gerekiyor. `docker login` komutunu giriyoruz.s

Son olarak `docker push ridvanaltun/myubuntu:0.1` komutu ile yaptığımız konteynırı paylaşıyoruz.

# Some Commands

Docker programı ile `image` ve `container`'ları yönetebiliyoruz. Genelde komutlar `docker image` yada `docker container` ile başlıyor, ancak kısa kullanım amaçlı komutlarda mevcut, örneğin `docker rm` konteynır siler, `docker rmi` image siler. `docker container rm` veya `docker image rm` yerine bu yapıyı kullanabiliriz.

```bash
# Yüklü imajları göster
docker images

# Yüklü tüm imajları göster
docker image ls --all

# Çalışan tüm kontainerları listele
docker container ls

# Tüm kontenırları görüntüle
docker container ls --all

# DockerHub üstünden bilgisayarımıza bir image edindik
docker image pull alpine

# Image'i konteynır olarak ayağa kaldırdık
# Run komutu ile her seferinde yeni bir konteynır ayağa kaldırırız
docker container run alpine

# Konternır ayağa kaldırdık ve merhaba komutu çalıştırdık konteynır içinde
docker container run alpine echo "merhaba"

# Konteynırı başalttık ve shell programına eriştik
# Biz shell'den exit yapana kadar konteynır çalışır
# Sonrasıdna konteynır kapanır
docker container run -it alpine /bin/sh

# Çalışan bir konteynır içinde komut çalıştırdık
docker container exec 17638aba6c37 ls

# Çalışan bir konteynırın shell'ine eriştik
docker container exec 176 -it alpine /bin/sh

# Çalışan tüm 'process status' ları göster
docker ps

# Tüm process'leri göster
docker ps -a

# nginx adlı kontainırı webserver adıyla 80 portu üstünden çalıştır
# ilk 80 bizim bilgisayarımız, 2. 80 docker network içindeki 80 portunu temsil ediyor
docker run --detach --publish 80:80 --name webserver nginx

# --detach veya -d : uygulama arkaplanda çalışsın
# --name : kontainer'a isim ver

# webserver adlı çalışan kontainer'ı durdur
docker container stop webserver # veya ksıaca docker stop

# Konteynırı sonlandır
docker container kill  # veya kısaca docker kill

# Adı verilen 3 container'ı sil
docker container rm webserver laughing_kowalevski relaxed_sammet # veya kısaca docker rm

# Image yapısı detaylı olarak incelenebilir
# inspect komutu bize JSON bir çıktı verir
docker image inspect deneme:v0.1

# Docker volume'leri listele
# Biz her ne kadar indir-kaldır işlemi yapsakta konteynıra bağlı volume'ler hep kalıyor
# Yani konteynırdaki data'mız bilgisayarın bir yerinde aslında hep kalıyor
docker volume ls

# Spesific bir volume'u temizle
docker volume rm <eski-container-id>

# Kullanılmayan tüm volume'leri temizle
docker volume prune
```

# Create Docker Image From A Container

Bir konteynırı ayağa kaldırıp shell'ine eriştikten sonra komutlar çalıştırabiliriz. Konteynırı kapatıp `docker container diff 965ade02a6cb` şeklinde konteynır üstünde yapılan değişiklikler listelenebilir.

`docker container commit 965ade02a6cb` komutu ile direkt olarak image oluşturulur. `docker image ls` yaptığımızda yeni image'i görebiliriz. Image'i çağırabilmek için tag ve isim vermemiz gerekiyor.

`docker image tag 3c2d4954f61b cowsay_deneme_image` şeklinde tag verdik.

Artık bu image'i ismiyle çağırabiliriz.
Artık bu image üstünden konteynır yaratabiliriz.

İstersek hiç bunla uğraşmayıp `Dockerfile` üstünden image oluşturabiliriz.

# Docker Network Structure

Konteynırlar dışarıya kapalıdır, kendi içinde bir ağ yapısı vardır. Dışarıya açılmak için `docker bridge` kullanılır.

`docker network inspect bridge` komutu ile docker'ın bridge yapısı gözlemlenebilir.

Conteynırları ayağa kaldırırken dahil olmasını istediğimiz ağ'ı belirtebiliyoruz, eğer belirtmezsek default olarak tüm konteynırlar `docker0` adında bir `bridge`'i kullanıyor. Bu `bridge` ise bizim makinemizde `eth0` üstünden ip adresi alıyor.

Konteynır kendi `local`'inde `eth0` olarak görüyor, gerçek bir makinenin network kartına direkt bağlıymış gibi.

Konteynırlar aksi söylenmedikçe default olarak `Docker bridge network` ile ayağa kaldırılır.

`Docker host network` ile çalıştırırsak eğer bridge'e gelmeden docker'ı host eden cihaz üstünden ip alır konteynır. Host'un bulunduğu ağa bağlı olan her şeye erişebilir hale geliriz.

Konteynır için herhangi bir network konfigürasyonu yapılmaması için `Docker none network` şeklinde ayarlarız.

Bununda kendimiz özel olarak bir network ayarlayıp konteynırı bağlayabiliriz. Buna `Docker user-defined network` deniyor. `docker network create — driver bridge docker_network` şeklinde `docker_network` adında, `bridge` tipinde bir network oluşturduk. Konteynır oluştururken `docker container run .. --network docker_network` şeklinde oluşturduğumuz network'ü kullanabiliyoruz. Sonrasında `docker network inspect docker_network` dediğimizde bu networku kullanan konteynırların network içinde yapılandırılma yaarları, network'ün kendi ayarları ile vs gösterilir.

Yani toplamda 4 farklı şekilde network konfigüre edilebiliyor;

- Docker bridge network (default)
- Docker host network
- Docker none network
- Docker user-defined network

Önemli: Konteynırlar aynı network içindeyse birbirleri ile IP adresleri üstünden haberleşebilirler.
Önemli: Konteynırlar aynı network içindeyse birbirleri ile konteynır adları ile haberleşebilirler. Örnek olarak nginx_sunucu adında bir konteynır açtım diyelim, ikinci olarak php_container adında bir konteynır açtım diyelim. İkiside aynı network içinde olsun. nginx_sunucu konteynırın içindeki terminalden `ping php_container` diye komut girebilirim, ip adresini bilmeme gerek yok.

# Docker Compose

Docker-Compose kullanmak için bir `.yaml` dosyası oluşturuyoruz. Dosyanın farklı versiyonları var, docker engine sürümüne göre `.yaml` dosyamızın sürümünü ayarlamamız gerekiyor.

Resmi dokümana [buradan](https://docs.docker.com/compose/compose-file/compose-versioning/) ulaşılabilir.

`.yaml` dosyasında son versiyonu referans alana bir kullanım yok. Yani lastest sürüm diye bir kullanım yok, major ve minor olarak yazmamız gerekiyor versiyonu.

Versiyona `2` yazdık diyelim, aslında bunun karşılığı `2.0`'dır.

`Services` bölümünde yönetmek istediğimiz tüm servisleri tek tek yazarız. Servis adlarını istediğimiz şekilde verebiliriz. Servisleri hazır bir image üstünden türetiriz. Yada Dockerfile üstünden yapabiliriz bu işi.

```yml
# Örnek bir docker-compose.yml dosyası
version: '2.0'

services:
   company_a_web_server:
      image: nginx:latest
      ports:
         - "8001:80"

   company_b_web_server:
      image: nginx:latest
      ports:
         - "8002:80"

   company_c_web_server:
      image: nginx:latest
      ports:
         - "8003:80"
```

Dosyanın olduğu klasöre girip `docker-compose up` komutu çalıştırırsak eğer sistemimizde belirtilen image'ler yoksa pull işlemi gerçekleştirilir. Sonra çalıştırılır.

Docker containerlarını durdurup yeniden ilgili klasöre girip `docker-compose up` komutu girdiğimizde konteynırlar yeniden oluşturulmaz, onun yerine sistemdeki aynı ada sahip konteynırlar ayağa kaldırılır.

`docker-compose ps` komutu sadece `docker-compose` ile oluşturulmuş konteynırları listeler.

`docker-compose logs` komutu ile `docker-compose` ile çalıştırılmış tüm konteynırların konsol çıkışlarını görebiliriz.

Tüm konteynırları kaldırmak için `docker-compose down` komutu verebiliriz. Tanımlı tüm konteynırlar silinecektir.
Eğer `docker-compose down -v` komutu çalıştırırsak kontenırlara bağlı olan volume'lar temizlenir.

`docker-compose up -d` komut ile `detach mode` ile açarız ve terminal ile zorunlu bir etkileşime girmek zorunda kalmayız.

`docker compose run --rm` komutu ile konteynır ayağa kaldırıldıktan sonra siliniyor. Eğer detach modunda açtıysak ignore edilir.

`docker-compose build` veya `docker-dile build <service-name>` komutu ile Dockerfile şeklinde tanımladığımız servisleri ve yapısı değiştirilmiş servisleri tekrardan build eder.

```yal
version: '2'

services:
   nginx-service:
      build: WebSite
      depends_on:
      - ruby-service

   ruby-service:
      build: WebApp
      depends_on:
      - redis-service

   redis-service:
      image: redis
```

depends_on parametresi konteynırların başlama sırasını belirlemez. Tek sebebi `dıcker-compose up service_name` şeklinmde kullandığımızda yanında otomatik olarak depends_on olan servisleride başaltmasıdır.

`expose` dışarıya değilde içeriden diğer konteynırlar ile iletişim için port açmak amaçlı kullanılıyor. `Dockerfile` içindeki `EXPOSE` ile aynı.

`environment` ile `environment variable` tanımları yapılabilir.
`dns` ile konteynırın DNS ayarı yapılabilir.

Konteynır içinde `/etc/hosts` dosyasını otomatik değiştiren `extra_hosts` parametresi kullanıalbilir. Konteynır içinde ip yerine domaşn yapısını bu şekilde kullanabiliriz.
