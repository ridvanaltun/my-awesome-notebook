# Git

Git hakkında bildiklerim.

# Table of Contents

- [Git](#git)
- [Table of Contents](#table-of-contents)
  - [Help](#help)
  - [Branch Naming](#branch-naming)
  - [Tricks](#tricks)
  - [Git Object Model](#git-object-model)
  - [Git Patch Mode](#git-patch-mode)
  - [Git Bundle](#git-bundle)
  - [Git Submodules](#git-submodules)
  - [Git Hooks](#git-hooks)
  - [Ignore Files](#ignore-files)
  - [Clone](#clone)
  - [Branch](#branch)
  - [Tags](#tags)
  - [Some Example of Git Commit Messages](#some-example-of-git-commit-messages)
  - [Git Config](#git-config)
  - [Security with SSH](#security-with-ssh)
  - [Merge](#merge)
  - [Rebase](#rebase)
  - [Merge Conflict](#merge-conflict)
  - [Stash](#stash)
  - [Diff](#diff)
  - [Patch](#patch)
  - [Bare Repository](#bare-repository)
  - [Push and Pull](#push-and-pull)
  - [Cherry Pick](#cherry-pick)
  - [Add Remote to Git Repo](#add-remote-to-git-repo)
  - [Undoing Bad Commits](#undoing-bad-commits)
  - [Log](#log)
  - [Fetch](#fetch)
  - [Syncing Fork](#syncing-fork)
  - [Pull Request](#pull-request)
  - [Prune](#prune)
  - [Dismiss Permission Changes](#dismiss-permission-changes)
- [Github](#github)
  - [Find Repo](#find-repo)
- [Merging Tools](#merging-tools)
- [Technical Terms](#technical-terms)

## Help

```bash
$ git help <command-name>
```

## Branch Naming

```
wip       Works in progress; stuff I know won't be finished soon
feat      Feature I'm adding or expanding
bug       Bug fix or experiment
junk      Throwaway branch created to experiment
```

Örnek: `feat/on-basvuru`

## Tricks

- `Branch`, `log`, `tag`, `show` gibi komutları çalıştırırken tüm kayıtlar çıkmasını istemiyorsak `-n` parametresi kullanmamız gerekiyor. n argümanı matematikte sayı belirtmek için kullanılır. Misal `git log -n 5` komutu girersek son 5 komut gelir, -n için hiç argüman girmezsek sadece son commit gelir mesela.
- `Commit`, `branch` ve `tag` detaylarına bakmak istiyorsak `git show` kullanıyoruz. Adını yazsak yeterli.
- Sadece son commit'e bakmak istiyorsak `git show HEAD`
- Sondan bir önceki commit mesela `HEAD~1` şeklinde ifade edilebiliyor.

## Git Object Model

Git arka planda nasıl çalışıyor öğrenmek için git obje modelini öğrenebiliriz. Git, `.git` klasörü içinde `Git Object Database` tutar. `.git/objects` klasörü içinde `SHA-1` ile şifrelenmiş, uniq bir ada sahip şekilde objeleri tutar. Örneğin herhangi bir onje `f7de3a39b026386f8f826bc230a112ae792ec035` ada sahiptir. `Object` klasörü içinde tüm objeler tutulur.

Git objeleri nedir peki?

- blob (`binary large object`)
- tree
- commit

Dosyalar `blob` ile gösterilir. Dosyanın adı veya izinleri bu obje içinde içermez. Sadece dosyanın içeriği bu obje içinde tutulur.

`Tree` objeleri klasör gibidirler. İçlerinde sadece başka `tree` objeleri ve `blob` objeleri barındırırlar.

`Commit` objesi içinde son commit hakkında bilgileri tutar.

## Git Patch Mode

Git'in yapılma sebeplerinden biride bu. Linus Tovards'a e-posta yolu ile geliştiricilerden bir çok kod geliştirmesi alıyormuş, bunları `review` etmek zor bir işlem. Bu nedenle yapılmış olması olası.

```bash
$ git add -p
```

`hunk` denen bir olay var, türkçe karşılığı iri parça. `patch` işlemini yaptığımızda bize tek tek soruyor, istediğimiz değişiklikleri kolayca parça parça entegre edebiliyoruz.

`patch` işlemini başlatınca aşağıdaki seçenekler ile çalışıyoruz. `hunk` ile ne yapacağımızı seçiyoruz.

- y : YES, bu hunk’ı al.
- n : NO, bu hunk’ı alma.
- q : QUIT, hiçbir şey yapmadan çık ve devam etme.
- a : ALL, bu hunk dahil, dosyadaki diğer tüm hunk’ları al.
- d : DON’T, bu hunk dahil, diğer tüm hunk’ları alma.
- / : SEARCH, girilecek REGEX paternine göre hunk ara.
- e : EDIT, elle hunk’ı düzenle.
- ? : HELP, yardım için.

## Git Bundle

Tüm `repo`'yu tek bir dosya şekline getirmemizi sağlıyor.

`git bundle create myproject.bundle --all` şeklinde bir kullanımı var.

Projemizi zip'lemiş gibi oluyoruz. İnsanlarla repo'muzu paylaşmak için bu yöntemi tercih edebiliriz.

// TODO

## Git Submodules

Geliştirdiğimiz projede kütüphaneleri yada servisleri `submodule` olarak ekleyebiliyoruz. Sonrasında tek komutla `submodule`'ler güncellenebiliyor ve güncelleme sonrası commit atıp güncellenmiş kütüphane ve servislerle projemizi geliştirmeye devam ediyoruz.

```bash
# Projeyi submodule'ler ile birlikte klonla
$ git clone --recursive <url>

# Git projesine submodule ekledik
$ git submodule add [URL to Git repo]

# Gşt projesine ilgili deponun master dalını submodule olarak ekledik
$ git submodule add -b master [URL to Git repo]

# Eklediğimiz sunmodule'ü kurduk
$ git submodule init

# Bundan sonra yapmamız gereken şey projeyi uzak sunucuya göndermek
$ git add [submodule directory]
$ git commit -m "move submodule to latest commit in master"
$ git push

# submodule'e yapılmış tüm değişiklikleri origin'den pull ile güncelle
$ git submodule update --remote

# submodule'e yapılmış değişiklikleri originden rebase güncelle
$ git submodule update --rebase --remote

# tüm projeyi ve submodule'leri pull et
$ git pull --recurse-submodules
```

## Git Hooks

`.git/hooks` klasörü altında onlarca `.sample` uzantılı örnek script dosyası var. `.sample` uzantısını silerek bu dosyaları kullanabiliriz veya kendimiz bir script yazabiliriz.

Nelere `hook` yazılabilir:

- pre-commit (commit öncesi)
- pre-rebase (rebase öncesi)
- post-checkout (checkout sonrası)
- post-merge (merge sonrası)

gibi. dahası da var.

## Ignore Files

`.gitignore` dosyasına tanımladığımız dosya ve klasörler `git` tarafıdan yok sayılır.

Bunun dışında lokal olarak ta dosyaları gizleyebiliriz. `.git/info/exclude` klasörü altına koyduğumuz bütün dosya ve klasörler `git` tarafından yok sayılır.

## Clone

```bash
# Projeyi gösterdiğimiz yere klonladık
$ git clone <url> <where-to-clone>

# Tüm projeyi kopyalamak yerine sadece branchi klonladık
$ git clone <url> -b <branch-name> <where-to-clone>

# Shallow Clone
# Aşağıdaki komut ile sadece projenin HEAD versiyonunu indirdik
# Commit History'si fazla olan projeleri indirirken kullanılıyor genelde
# Projenin yapısına göre binlerce megabyte indirmekten kurtulabiliriz
# Git v1.9 sonrası için shallow clone'lar için pull ve push özelliği getirildi
# Git Shallow'u bazı git programları desteklemiyor, açarken problem yaşıyor
$ git clone --depth 1 <url>

# Sadece master dalını klonlar
$ git clone --single-branch --branch master <url>
```

## Branch

```bash
# Repomuzdaki branchler listelenir
$ git branch

# Tüm branchler uzun şekliyle listelenir
$ git branch -a

# Yeni branch açmak
$ git branch <branch-name>

# Belirtilen commit id ile yeni branch açmak
$ git branch <branch-name> <commit-id>

# Daldaki merge olmuş diğer dalları görmek
# Bu şekilde işe yaramayan dalları görüp silebiliriz
$ git branch --merged
```

Branch'i hem local hem remote kaynak üstünden silebiliyoruz.

```bash
# local olarak siler
$ git branch -d branch_name

# local olarak siler
$ git branch --delete branch_name

# merge durumuna bakmadan local olarak siler
$ git branch -D branch_name

# merge durumuna bakmadan local olarak siler
$ git branch --delete-force branch_name

# remote olarak siler
# remote olarak branch'i sildikten sonra `git branch -a` komutunu girdiğimizde silinmiş gözükür
$ git push origin --delete branch_name
```

## Tags

Tag sistemi aslında içeriği değiştirilemez `branch`. Bu kadar basit.

Tag yapısı 2'ye ayrılıyor. `Annotated` ve `Lightweight`.

`Annotated` yani açıklamalı tag. İki tip arasında sadece içerdikleri meta data sayısı değişiyor.

```bash
# tagleri listeler
$ git tag

# lightweight tag açmak için tek yapmak gereken tag'e bir ad vermek
$ git tag v1.0.0

# açıklamalı bir tag açtık, -a anlamı annotated
$ git tag -a v1.0.0

# içinde mesaj olan bir tag açtık
$ git tag -a v1.0.0 -m "Releasing version v1.0.0"

# belirtilen tag hakkında bilgi dönderir
$ git show v1.0.0
```

`Tag` adı, `tag identifier` olarak geçiyor. Aynı `identifier` tekrar açamıyoruz. Örneğin `pre-relaese` adında bir tag'imiz var. Başka bir `pre-release` çıkarmadan önce öncekini silip yenisini açıyoruz.

## Some Example of Git Commit Messages

`chore: version bump` versiyonu uniq bir şekilde değiştirdim demek.

## Git Config

```bash
# Global olarak kullanıcı adımı ayarladım
$ git config --global user.name "Rıdvan Altun"

# Global olarak e-posta adresimi ayarladım
$ git config --global user.email "ridvanaltun@outlook.com"

# Global olarak ayarladığım ada baktım
$ git config --global --get user.name

# Tüm global .gitconfig ayarları listelenir
$ git config --global --list

# Projedeki master branch'in remote'unu origin olarak değiştiridm
# Artık master dalında pull ve push yaparken origin üstünde çalışıcaz
$ git config branch.master.remote origin
```

## Security with SSH

Aşağıdaki komut ile bilgisayarımıza özel bir komut üretiyoruz, bu komut rsa algoritması ile şifrelenmiş. `-C` argümanı ile aslında sadece not yazıyoruz, genelde mail adresi tercih ediliyor ancak oraya bilgisayarımızın adınıda yazabilirdik.

Dosyalar otomatik olarak `.ssh/` altında oluşuyor, Bu klasör altında id_rsa.pub, id_rsa adlarında iki dosya oluşmuş oldu. `id_rsa` private key'imizin olduğu dosya. `id_rsa.pub` ise public keyimizi taşıyor ki dosya uzantısı söylüyor zaten public olduğunu. Uzak sunucu repomuza `.pub` uzantılı dosyamızı veriyoruz bu sebeple.

Eğer şifre koyarsak her işlemde bize şifre sorar, şifre kısmını boş bırakırsak şifresiz yapabiliriz işlemlerimizi.

```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

`.ssh/` kasörü içinde birde `known_hosts` adında bir dosya var. Bir siteye ilk defa SSH ile bağlanma isteği attığımızda SSH programı soruyor bize bu siteyi güvenilir kaynaklara ekleyeyim mi diye. Bu sayede `MITM` saldırılarına karşı korunuyoruz, araya birisi girip ben git serveriyim diye bizi kandıramıyor.

Bir siteteye manuel olarak SSH bağlantısı şu şekilde atmak mümkün: `ssh ldn01.jamieweb.net`

Bir sitenin SSH key'ini şu şekilde manuel olarak görmek mümkün: `ssh-keyscan ldn01.jamieweb.net`, known_hosts dosyasına buradan okuduğumuz verileri girerek te hazırlayabiliriz.

SSH programının kendi config dosyası var: `/etc/ssh/sshd_config`, bu dosya içinde hosts ne olursa olsun güven diye bir parametre var, istersek hiç bir host'a sordurtmadan tüm bağlantılarımızı güvenli kabül et diyebiliriz.

`ssh-keygen -R domain.com` komutu ile `known_hosts` dosyamızı otomatik olarak güncelleyebiliyoruz. Git sunucu taşındığında mesela bu yöntem ile güncellememiz gerekecektir.

## Merge

Master dalına merge yapmak istiyoruz diyeim, master dalına geçiğ `master merge feaute` gibi bir komut giriyoruz ve merge işlemi gerçekleşiyor.

`git merge --squash feature` : Kısaca merge yaparken branchdeki tüm commitleri birleştirip tek bir commit olarak merge işlemi yapıyoruz.

`squash` kelime anlamı sıkıştırmak, ezmek.

Komutu girdikten sonra merge işlemi başarılı ancak yeni commit atmanız gerekli diye bir yazı çıkacak. Yani yeni bir commit mesajı girmemiz gerekiyor. `git commit -m "yeni commit"` şeklinde girebiliriz.

`git merge --abort` komutu ile yaptığımız merge'i tamamen geri alabiliriz.

## Rebase

`git rebase master` Örneğin bu komutu feature dalından çalıştırırsak master branch'ına son eklenmiş commitleri bize ekler, araya ekler.

1. master dalında 3 tane commit atılmış olsun: m1, m2, m3
2. master dalından feature dalı açalım ve 2 commit atalım m1, m2, m3, f1, f2
3. master dalına 2 commit girelim m1, m2, m3, m4 ,m5
4. feaure dalına geçip `git rebase master` çalıştıralım m1, m2, m3, m4, m5, f1, f2
5. master dalına geçip `git rebase feature` çalıştıralım m1, m2, m3, m4, m5, f1, f2

Örneğin master dalımıza kritik bir güncelleme geldi. Merge sırasında bu işi yapmak yerine anında bu bu gelen değişikliği başka bir branch'e aktarabiliriz.

Farklı dal üstündeyken `git rebase master` diyerek master üstündeki değişikliği dalımıza alıyoruz.

Bir ekip ile çalışıyorsak eğer `rebase` komutu hayat kurtarıcı olabilir. Şöyle ki; Projeyi geliştirmek için 2 farklı yol izleyebiliriz;

Birincisi projenin `maintainer`'ı oluruz, yazdığımız tüm kodları ayrı bir branch olarak açar ve sonunda pull request göndeririz. Direkt olarak production branchlerine commit atmamız yasaklanır. Sadece `pull request` atabiliriz ve birisi kodumuzu review edip, hatta merge conflictleri çözüp production'a uygular. Hem bu sayede birden fazla kişi aynı `feature` üstünde kolayca çalışabilir çünkü herkes tek bir remote üstünden çalışmış olur.

Diğer yöntem ise ana projeyi forklayıp bir upstream remote'u eklemek. Bundan sonrasında pull request öncesi ilk olarak upstream kanalı güncellenir, upstream'den local'de ilgili dalına `rebase` işlemi yapılır. Sonrasında origin'i güncelleme için git push komutu çalıştırılır ve en sonunda pull request işlemi oluşturulur.

Rebase komutunu sadece `publish` edilmemiş dallarda kullanmalıyız. Örneğin `feature` yazma aşamasında ilgili branch için rebase komutu çalıştırmamız sorun teşkil etmez ancak ilgili dal publish edildikten sonra rebase işlemi yapmak projeyş geliştiren diğer insanlara zarar verecektir.

## Merge Conflict

Bu durumu yaşadığımızda conflict'i çözmek için commit'i atan diğer kişi ile birlikte oturup sorunu çözebiliriz.

Merge conflict olduğu zaman conflict dosyaları branch'imiz üstünde durur ve içine konflict olan yerler işaretlenir, burası senin kısmın burası gelen kısım diye.

Conflict çözerken nano vs kullanabiliriz ancak kolaylık için bir text editöründe düzenleme yapmalıyız.

Düzenleme bitince kaydedip yeni bir commit olarak atıyoruz.

Karşıdan gelen tüm değişiklkleri kullan yada beni mkendi değişikliklerim kalsın dersen bunlar için ayrı komutlar var.

```bash
# tüm dosyalarda kendi değişikliğimiz kalsın
$ git checkout --ours .

# tüm dosyalarda karşıdan gelen değişikliği kabul et
$ git checkout --theirs .
```

Devamında senaryo yine aynı. Değişiklikleri stage'e ekle ve commit'le.

## Stash

Geçici olarak durumu kaydetmek için kullanıyoruz bunu. `git stash` komutu girmek yeterli.

Örneğin bir özellik geliştiriyoruz ama kritik bir hata çıktı, tüm işlemizi `stash`'layip kritik hata ile ilgilenebiliriz.

```bash
# Stashlerimizi görebiliriz
$ git stash list

# Son stash'imiz geri yüklenir ve stash'ler içinden silinir
$ git stash pop

# Stash'imizi uyguladık, ancak silinmedi
$ git stash apply stash@{1}

# Stash'i sildik
$ git stash drop stash@{1}

# Stash'a diff attık, bakıyoruz neler değiştirmişim diye
$ git stash show -p stash@{0}

# Stash'ı özel bir isimle kaydettik. 'git stash list' yaparsak 'stash@{0}: message' şeklinde bir çıktı çıkacak
$ git stash push -m "message"
```

## Diff

```bash
# Dalımızdaki değişiklikleri gösterir
$ git diff

# İki commit arasındaki farkı gösterir
$ git diff commit_id_one commit_id_two

# İki branch arasındaki dosyaların karşılaştırılması
$ git diff brach1:path/to/file branch2:path/to/file
```

## Patch

Stashyerine patch methodu tercih edilebilir. İnsanlar bu methodu çok bilmiyor. `.gitignore` dosyasına `.patch` uzantılı dosyaları eklersek eğer projemiz üstünde rahatça çalışabiliriz.

```bash
# Tüm değişiklikleri bir dosyaya kaydediyoruz
$ git diff > some.patch

# Dosyada değişiklikleri uyguluyoruz
$ git apply some.patch
```

## Bare Repository

Normalde git'i projemize entegre ederken `git init` komutunu kullanırız. `git init --bare` kullandığımızda ise `Remote Repostory` için  git initilaize ediyoruz.

Bare repo'da `remote origin` bulunmaz.

Bunun mantığı remote amaçlı repo açmak. Başka insanlar projeyi buradan clone'layıp değişikliklerini pushlayacaklar.

Bare ile repo açtığımızda `.git` klasörü oluşmaz, onun yerine .git klasörü içindeki bir kaç dosya projeyi açtığımız klasörün içine gelir.

Kısaca bare kurulum ile sadece git'in database kısmını alıyoruz, remote amaçlı kuruyoruz ve insanlar ortak çalışmak için bunun üstünden çalışıyorlar.

## Push and Pull

```bash
# Origin adlı remote'a, develop adındaki dalı push'la.
$ git push origin develop

# Origin'de yapılan değişiklikleri local'imizdeki repo'ya çek.
$ git pull origin master

# Yaparsak eğer budan sonra `pull` yaparken uzun uzun yazmamıza gerek kalmıyor.
# Direkt olarak `git pull` komutu ile remote'dan çekebiliyoruz değişiklikleri.
# Aynı şekilde `git push` içinde bir daha remote göstermemize gerek kalmıyor
# Genelde ilk kez uzak repo'ya bağlanırken bu komut kullanılır
# İnsanlar bunu ezbere kullanıyor ama ortada magical bir durum yok
$ git push --set-upstream origin <branch> # yada
$ git push -u origin <branch>
```

## Cherry Pick

Bir commit'i alıp başka bir branch'e uygulama işine `cherry pick` deniyor.
Commit hash'i farklı geliyor.

```bash
# master branchindeki commitleri bulunduğumuz branch'e attık
$ git cherry-pick master
```

## Add Remote to Git Repo

```bash
# Repo'ya remote ekle. Origin adı altında bir remote ekledik.
$ git remote add origin <url>

# Repo'ya bağlanmış remote'ları listele
$ git remote

# Repo'daki remote'ları detaylı listele, -v anlamı verbose.
$ git remote -v

# Belirtilen remote'u sil.
$ git remote remove <remote-adı>

# Remote için verilen adı değiştir, örneğin origin inin
$ git remote rename <remote-adı> <yeni-remote-adı>

# Remote'un URL'ini değiştir
$ git remote set-url <remote-adı> <url>
```

## Undoing Bad Commits

```bash
# Dosya üstünde yaptığımız değişikliği discard ediyoruz
$ git checkout file_name

# Commit mesajını değiştirmek
# Son attığımız commit'in mesajı değişti
# Ancak bunun yanında commit'in hash'i de değişti çünkü mesaj commit'in bir parçası
# Repo'yu kullanan insanlara sorun çıkarmamak için Git History'imizi değiştirmemeliyiz
# Kısaca pushladığımız commitlerin history'sini değiştirmemeliyiz
$ git commit --amend -m "Yeni Commit Mesajı"

# Son commit'e ekstra dosya eklemek
# Örneğin commit içinde bir adet dosyayı unuttuk
# Bu komutu girince son commit'e stage'deki değişiklikler ekleniyor
# Commit mesajını düzenlememiz için bir commit'in sayfası açılıyor
# Düzenlemek zorunda değiliz, istersek bu sayfada commit mesajını değiştirebiliriz
# Git History değişeceği için sadece push edilmemiş commitler üzerinde uygulanmalıdır
$ git commit --amend

# Son commit' ekstra oalrak stage'e eklediğimiz dosyaları ekledik
# Commit mesajını değiştirmel zorunda değiliz çünkü --no-edit kullandık
# Commit id değişiyor tabi
$ git commit --amend --no-edit

# Commit'i yanlış branch'e attık
# Commit'i silip diğer branch'e taşımalıyız
# İlk olarak commit'i almak istediğimiz dala geçiyoruz
# Dal üstünde komutu çalıştırıyoruz ve otomatik olarak commit'i kendine kopyalıyor
# İkinci olarak yanlış commit attığımız dala geçip ikinci komutu giriyoruz
# Verdiğimiz commit'e geri döndük, ancak eski commitlerdiğimiz değişiklikler geri döndü
# git status komutu girdiğimizde tüm eski değişiklikler stage'e geri döndü
# --soft parametresi ile hiç bir verimizi kaybetmedik
$ git cherry-pick commit_id
$ git reset --soft donmek_istedigimiz_commit_id

# mixed reset
# --soft ile aynı çalışıyor ancak commitler stage değil working directory'ye geliyor
$ git reset donmek_istedigimiz_commit_id

# hard reset
# mantık yine aynı ancak bu sefer commit ile yazdığımız tüm değişiklikleri direkt olarak sildik
# git status yaptığımızda geri aldığımız commit hakkında hiç bir şey olmayacak
$ git reset --hard

# Takip edilmeyen dosyaları silmek, Delete Unracked Files
# burada d -> directory, f -> force anlamına geliyor
# stage aşamasında olmayan, hiç commit görmemiş dosya ve klasörler silinir
# d -> untracked varsa silmek için
$ git clean -df

# Değiştirilen dosyaları eski haline getirmek
$ git checkout -f

# Clean komutu çalıştırıldığında hangi dosyalar silinecek gösterir
# Dry run deniyor buna, -df parametresi ile clean yapmadan önce çalıştırıp çıktıya bakabiliriz
$ git clean -nd

# Yanlışlıkla dosya sildik, reset yaptık vs.
# Silinen commit'in comit_id'sini öğrenmek için `git reflog` çalıştırırız
# Yani --hard reset sonrası bile dosyalarımızı geri getirebiliyoruz
$ git checkout geri_almak_istedigimiz_commit_in_id_si

# Bir dosyayı sildik veya değiştirip commit attık
# Ancak dosyayı geri getirmek istiyoruz ama yanında bir çok dosya daha silmiştik
# Sadece ilgili dosyayı geri getirebiliyoruz
$ git checkout geri_almak_istedigimiz_commit_in_id_si alacağımız_dosyanın_yolu

# Git History'sini değiştirmeden commitlerimiz üstünde değişiklik yapmak
# İnsanlar projemizi Pull etmiş olacağı için History'yi değiştirecek bir şey yapmamalıyız
# Bu sebeple silmek istediğimiz commit'i ektra bir commit ile siliyoruz
# Revert işlemi yapılırken bizden commit girmemiz isteniyor, girdiğimizde yeni bir commit eklemiş oluyoruz
# Eski commit yerinde kalıyor, yeni commit ile eski committe yaptığımız değişiklikler kaldırılıyor
# git diff ile özellikle neleri geri aldığımızı commit_id'leri belirterek görebiliriz
# Dikkat edilmesi gerek: donmek istediğimiz değil, revert etmek istediğimiz commit'e kadar gösteriyoruz
$ git revert commit_id

# Son iki commit'i default texxt editörümüzde hangi işlemden geçirmek istediğimzii sorar
# İstersek tüm commitlerimizi elden tek bir komutla geçirebiliriz
# Tüm commit mesajlarını elden geçirebiliriz
# İstediğimiz commitleri kolaylıkla silebiliriz
# Commitlerin sırasını değiştirebiliriz, komut sonrası açılan pencerede seçili commitlerin sırasını değiştirmek yeterli
# Commitleri squash yada fixup ile birleştirebiliriz
# Commit'i bölebiliriz, iki farklı commit şekline getirebilriiz mesela, edit seçeneğini kullanarak
# Bunu yapmak biraz karışık, edit yaptığımızda yeni-özel bir branch açılır, git reset ^HEAD komutu ile unstage yaparız
# Normal git add ve git commit komutlarını kullanarak commit atarız, en sonunda `git rebase --continue` ile işlemi bitiririz
# Commit id bu işlemler sonrası değişir
# Burada -i parametresi interaktif anlamına geliyor
# NOT: 'git rebase --abort' komutu ile yaptığımız değişiklikleri geri alıp işemi iptal edebiliriz
$ git rebase -i HEAD~2

# ilk commit dahil tüm commitleri interaktif modda açar
$ git rebase -i --root
```

## Log

```bash
# Tek satırda yapılan tüm değişiklikleri gösterir
$ git log --pretty=oneline

# İstediğimiz remote'un dalına bakabiliriz
$ git log upstream/cashflow

# Hangi commit'te hangi dosyalar değişmiş gösterir
$ git log --stat

# Yaptığımız tüm işlemler, reset, checkout, chery-pick, commit, amend vs. listelenir
$ git reflog

# Branch 2 de olan ama 1 de olmaya commitleri sıralar
$ git log branch_name_one..branch_name_two

# Bir dosya üstünden geçen commitleri göster
$ git log --follow -- filename

# Pager olmadan tüm çıktıyı gösteriyor
$ git --no-pager log
```

## Fetch

`git pull` ile tek farkı `merge` işlemi yapmadan değişiklikleri çekmesi.

```bash
# upstream adındaki remote'u güncelle
$ git fetch upstream

# Tüm remote'ları güncelle
$ git fetch --all

# Tüm remote'ları güncelle, silinmiş bir dal varsa onuda sil
$ git fetch --all --prune
```

## Syncing Fork

```bash
# Öncelikle forkladığımız projenin içine orjinal remote'u ekliyoruz
# Remote'u upstream adında bir dala ekleyebiliriz
$ git remote add upstream git://github.com/ORIGINAL-DEV-USERNAME/REPO-YOU-FORKED-FROM.git

# Remote'daki değişiklikleri upstream dalına çekiyoruz
$ git fetch upstream

# Yapılmış değişiklikleri master dalımıza uyguluyoruz
$ git rebase upstream/master

# Fork'umuza yeni bir branch yükledik
$ git push origin branch_name

# Fork'umuzdan branch sildik
$ git push origin --delete branch_name

# branch_name adlı remote dalını branch_name adında lokal olarak çektik
# yeni dalları bu yöntemle çekebiliriz
$ git fetch upstream branch_name:branch_name
```

## Pull Request

Git içinde `git request-pull` diye bir komut var ancak bu komut `Github Pull Request` yada türevleri ile çok farklı. Git komutu yapılacak değişiklikleri bir email listesine atmamızı sağlıyor sadece. Yani kısaca `pull request` için Github gibi servislere ihtiyacımız var.

Git'in komutunu kullanmak istersek aşağıdaki şekilde ilerleyebiliriz.

```bash
$ git request-pull origin/master feature/awesomeFeature
```

Komut satırından Github'da yaptığımız gibi pull request yapmak istersek bunun için geliştirilmiş araçları kullanmamız gerekiyor. Örneğin [hub](https://hub.github.com) adında bir araç var, bu araç ile Github'ı konsol üstünden yönetebiliyoruz. Pull request için aşağıdaki gibi bir komut kullanabiliriz.

```bash
$ hub pull-request
```

Aynı zamanda gelen pull requestleri aşağıdaki gibi kontrol edebiliriz.

```bash
# "develop" dalında yapılmış en az 20 pr'ın URL'lerini listele
$ hub pr list -L 20 -b develop --format='%t [%H] | %U%n'
```

## Prune

Bu komut ile git projemizde bulunan ve hiç bir referansa bağlı olmayan yani kısaca kullanılamayan git objeleri silinir. Örneğin git reset komutu ile bir commit sildik diyelim, commit git history de silindi gözükse de silinen commit'e checkout ile gidebiliriz mesela. Git halen kendi içinde sildiğimiz commit hakkında bilgileri saklar. Git prune komutunu çalıştırdığımızda sildiğimiz commit'in hiç bir bağlantısı olmadığı için silinecektir.

`git fetch --prune` komutu ile örneğin bir commitimiz merge edildi ve bir branch silindi, localimizde aynı branch'in silinmesini istiyorsak `git fetch --prune` komutu kullanmamız yeterli oluyor. Bu komutu `git fetch --all` hemen sonrası kullanabiliriz.

```bash
# git pprune komutunu çalıştırırsak neler silinecek listeler, eğer boş dönerse prune komutu bir şeyi silmeyecek demektir
$ git prune --dry-run --verbose

# fetch edilen depodaki silinen objeleri localdede siler
$ git fetch --prune

# origin'deki ref'i olmayan objeleri prune ile sildirdik
$ git remote prune origin
```

## Dismiss Permission Changes

`git config core.fileMode false` komutu ile dosya izininde yapılan değişiklikler git tarafından görmezden gelinir. Bormalde bir dosyanın iznini değiştirdiğimiz zaman dosya stage eklenir ve dosyayı commitlememiz gerekir, saçma bir kullanım olduğu için izin değişikliklerini görmezden gelebiliriz.

## Merge Pull Request on Local

Bazı zamanlar bazı projelerde pull requestler kabul edilmiyor ve kalıyor durdukları yerde. Yada pull requestin merge edilmeden önce test edilmesi gerekiyor, bu gibi durumlarda localimize merge isteğini kabul edebiliriz.

```bash
# 37 numaralı pr'yi pr37 adında bir branch haline getirerek indir
$ git fetch upstream pull/37/head:pr37

# pr37 adlı branche git
$ git checkout pr37

# pr update oldu diyelim, bu komut ile yapılan değişiklikleri localimize alabiliriz
# bu komutu pr'nin oldupu branch içindeyken çalıştırmamız gerekiyor
$ git pull upstream pull/37/head
```

# Github

Github'ın kendine has bazı özellikleri.

## Find Repo

Bir kütüphane kullanacağız ancak kütüphaneyi nasıl kullanacağımızdan emin değiliz. Başka projelerin içine bakmak istiyoruz diyelim.

> Github Proje Sayfası >> Insights >> Dependency graph >> Dependents

Bu şekilde kütüphaneyi kullanan örnek projeleri görebiliriz.

# Merging Tools

Komut satırından `merge conflict`'leri yönetmek büyük projeler için zor, bu sebeple bize yardımcı olacak yazılımlar kullanmalıyız.

// TODO

# Technical Terms

**Origin:** Kısaca remote repo olarak adlandırabiliriz. Origin ismi sonradan değiştirilebilir ancak herkes origin kullanmayı tercih ediyor.
**Upstream:** Bir repo'yu forkladığımız zaman origin bizim forkumuz olur, orjinal repo üstünde işlemler yapabilmek için upstream adında ikinci bir remote ekleyebiliriz. Upstream ismi vermek zorunda değiliz ancak genelde bu isim tercih ediliyor.
