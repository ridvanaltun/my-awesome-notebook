# Clean Code

PHP yazarken kodumuzun temiz kalması için kullanabileceğimiz geliştirici araçları.

# Table of Contents

- [Clean Code](#clean-code)
- [Table of Contents](#table-of-contents)
- [PHP CodeSniffer](#php-codesniffer)
  - [Installation](#installation)
  - [Usage](#usage)
    - [PHPCS](#phpcs)
    - [PHPCBF](#phpcbf)

# PHP CodeSniffer

Bu iş için bir çok araç var ancak en yaygın kullanılan araç `PHP CodeSniffer`.

## Installation

```
$ composer global require "squizlabs/php_codesniffer"
```

## Usage

### PHPCS

Bu programa php dosyamızı gösterdiğimizde bize estetik hataları diziyor.

Aşağıdaki örnekte `psr2` standartını kullanarak EmailController.php adındaki dosyada bulunan hataları göster diyoruz.

```
$ phpcs --standard=psr2 EmailController.php
```

### PHPCBF

Aşağıdaki örnekte hatalar otomatik düzeltiliyor.

```
$ phpcbf --standard=psr2 EmailController.php
```