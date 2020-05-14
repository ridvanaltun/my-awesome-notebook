# Write Composer Package

Yazdığımız PHP fonksiyon ve Class'ları paket halien getirmek için composer paketi yazmayı tercih edebiliriz. Bu doküman içinde composer paketi oluştururken aldığım notlar yer alıyor.

# Title of Contents

- [Write Composer Package](#write-composer-package)
- [Title of Contents](#title-of-contents)
- [File Structure](#file-structure)
- [Gitignore](#gitignore)
- [Testing on Dev Envoriment](#testing-on-dev-envoriment)
- [Use Packge](#use-packge)
- [Upload Pacakge to Packagist](#upload-pacakge-to-packagist)
- [Packagist Badges](#packagist-badges)

# File Structure

Aşağıda örnek bir composer paketinin dosya yapısı örneği verilmiştir.

```
- .gitignore
- README.md
- LICENSE
- composer.json
- composer-lock.json
- src
    - ClassName.php
```

`composer.json` dosyamız aşağıdaki gibi olabilir:

```json
{
    "name": "vendor-name/package-name",
    "description": "Create JSON Patch (IETF RFC-6902).",
    "type": "library",
    "license": "MIT",
    "authors": [
        {
            "name": "Ridvan Altun",
            "email": "ridvan@etom.com.tr",
            "homepage": "https://github.com/ridvanaltun"
        }
    ],
    "minimum-stability": "stable",
    "autoload": {
        "psr-4": {"foo\\Bar\\": "src/"}
    },
    "require": {}
}
```
name anahtarı karşısında belirttiğimiz vendor-name organizasyon adı ve package-name ise paket adı anlamına geliyor.

# Gitignore

`.gitignore` dosyamız aşağıdaki gibi olmalı, genel kullanım bu şekilde.
`composer-lock.json` dosyası `.gitignore` koyulmaması tavsiye ediliyor.

```
composer.phar
/vendor/

# Commit your application's lock file https://getcomposer.org/doc/01-basic-usage.md#commit-your-composer-lock-file-to-version-control
# You may choose to ignore a library lock file http://getcomposer.org/doc/02-libraries.md#lock-file
# composer.lock
```

# Testing on Dev Envoriment

Aşağıdaki örnek `composer.json` dosyası içinde local olarak paketin bir proje içinde kullanılması yer alıyor, projemiz içinde `composer install --prefer-source` şeklinde bir komut çalıştırdığımız zaman verdiğimiz local adresteki paketi kuracak projeye.

`--prefer-source` kullanmamızın sebebi orjinal composer registry'sinde aynı ada sahip başka bir paket varsa bizim gösterdiğimiz paket kullanılsın diye. Örneğin ilk composer paketimizi hazırladık ve yayınladık, aynı paketin başka bir sürümü üstünde çalışırken `composer install` veya `composer update` komutu orjinal registry üstündeki eski paketi kullanacaktı.

```json
{
    "repositories": [
        {
            "type": "path",
            "url": "S:/openstack-sdk",
            "options": {
                "symlink": true
            }
        }
    ],
    "require": {
        "ridvanaltun/json-patch-generator": "dev-master"
    }
}
```

# Use Packge

Yazdığımız paketin `{"foo\\Bar\\": "src/"}` şeklinde belirtilen kısmı projemizdeki kaynak kodların başka projelere nasıl include edileceği hakkında bilgi içeriyor. Bu örnekte paketi kullanan herhangi bir proje `use foo\Bar\Baz` şeklinde bir kullanım yaptığında aslında `src\Baz.php` dosyasını çağırdığı anlamına geliyor.

Aşağıda örnek bir kullanım mevcut.

```php
<?php

require_once __DIR__ . '/vendor/autoload.php';

use foo\Bar\Baz;

$baz = new Baz();
```

Büyük projelerde paketin kullanım kolaylığını arttırmak için birden fazla tanım yapabiliriz `{"foo\\Bar\\": "src/"}` gibi.

# Upload Pacakge to Packagist

- Paketimizi github'da hostluyoruz.
- Sonrasında `packagist` hesabımıza github'ı bağlıyoruz ve projemizi ekliyoruz.

Repomuzda olan değişikliklere göre otomatik olarak paketimiz güncelleniyor. Örneğin yeni sürüm oluşturduğumuz otomatik olarak packagist üstüne yeni sürüm ekleniyor.

# Packagist Badges

[Bu siteyi](https://poser.pugx.org/) kullanarak `packagist` için badge ekleyebiliriz.
