# Git Flow

Sematik versiyonlama sistemini daha kolay uygulamamız için geliştirilmiş bir program. Aynı zamanda kendisi bir git stratejisi, programın amacı bu stratejiyi daha kolay uygulamamızı sağlamak.

Windows için `git-flow` yüklemek: https://github.com/nvie/gitflow/wiki/Windows
Kurulum esnasında bir kaç soru soruyor.

# Table of Contents

<!-- toc -->

- [Semantic Versioning](#semantic-versioning)
- [Branches](#branches)
  * [Master](#master)
  * [Hotfix](#hotfix)
  * [Release](#release)
  * [Devolop](#devolop)
  * [Feature](#feature)
- [Usage](#usage)
- [Sources](#sources)

<!-- tocstop -->

# Semantic Versioning

Geliştirdiğimiz programda versiyon çıkartırken `x.y.z` şeklinde çıkartabiliriz.

- x -> major güncelleme
- y -> minor güncelleme
- z -> hotfix, patch, yama

Git sistemi ile bu şekilde projemizi geliştiriyoruz.

- Master branch üstünde projenin yayınlanan hali bulunuyor.
- Feature branch ile özellik geliştiriyoruz.
- Devolop branch ile geliştirme yapıyoruz.
- Hotfix branch ile ufak şeyleri yamıyoruz.
- Release branch ile yayından öncesi geliştirmeler. README.md gibi.

# Branches

Geliştirme esnasında kullanacağımız dallar ve detayları.

## Master

Projenin son hali burada bulunuyor. Bütün dalları birleştirdikten sonra master ile yayına alıyoruz.

## Hotfix

Projemizde çıkan kritik bir hatayı çözmek için yaptığımız ufak kod değişikliklerini bu branch üstünden yapıyoruz ve sorunu çözer çözmez merge işlemi yapıyoruz. Burada dikkat edilmesi gereken şey merge işlemini hem devolopment hemde master için yapıyoruz. Geliştirdiğimiz koddaki kritik hatanın çözümünü hemen yayına alıyoruz ve master ile paralel giden devolop kısmına da bu kritik hatayı düzeltmek için hotfix ile merge işlemi yapıyoruz.

## Release

İsminin aksine burada sadece projeyi yayına almadan, release öncesi README.md gibi dosyaların düzenlenmesi ve gerekli bilgilerin buralara yazılmasını üstleniyor. Bu branch devolop branch'ından türüyor.

## Devolop

Bu dal master dalı üstünden türüyor. Projemizi bu dal üstünden geliştiriyoruz. Bir feature eklemek istediğimizde bu dal üstünden feature branch'i açıyoruz.

## Feature

Projemize bir özellik eklemek istediğimiz zaman devolop üstünden bir feature açıp yine devolop branch'ı üstüne kapatıyoruz.

# Usage

> git flow init

Komutunu çalıştırınca feature, devolop, hotfix gibi dalların ismi ne olsun diye soruyor. Değiştirmemek en mantıklısı. Git flow projesi oluşturmuş olduk.

Şimdi git flow ile projemizi geliştirelim.

> git flow feature start ozelligimizin_ismi

Burada "ozelligimizin_ismi" adında, devolop branch'ı üstünde bir feature branch'ı oluşturduk.

> git flow feature finish ozelligimizin_ismi

Yazdığımız özelliği burada devolop ile birleştiriyoruz.

Projemizi eklediğimiz yeni özellik ile yayına almak istiyoruz diyelim.

> git flow release start 1.0.0

Devolop üstünde bir relase oluşturduk, şimdi yapmamız gereken şey readme.md gibi yayından önce editlenmesi gereken son bilgi veren dosyaları editlemek.
Burada 1.0.0 bizim versiyon numaramızı belirtiyor.

> git flow release finish 1.0.0

Artık yaptığımız değişiklikler master ile birleşti, proje yayında.

> git flow hotfix start 1.1.1

Baktık projede kritik bir hata var ve hemen düzeltilip yayınlanması gerekiyor.
Komut sonrası kodumuzu güncelliyoruz.

> git flow hotfix finish 1.1.1 

Hotfix'i kapadık. Çözdük olayı.

Git flow eklentisi `SourceTree` adlı GUI tabanlı bir program ile uyumlu çalışıyor. Komut satırına bağlı değiliz.

# Sources

- [git-flow cheatsheet](https://danielkummer.github.io/git-flow-cheatsheet/index.tr_TR.html)

- [AnkaraPHP Mart Etkinliği — Git Flow](https://medium.com/ankara-php/ankaraphp-mart-etkinli%C4%9Fi-git-flow-13bb74b4981b)
