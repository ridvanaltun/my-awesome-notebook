# Phinx

PHP için yazılmış bir migration aracı. Migration dosyalarını `PHP betiği` olarak yazıyoruz ve program üstünden migration işlemlerini yürütüyoruz.

# Table of Contents

- [Phinx](#phinx)
- [Table of Contents](#table-of-contents)
- [Installation](#installation)
- [Migration](#migration)
- [Seed](#seed)
- [Phinx Migrations Generator](#phinx-migrations-generator)
  - [Install](#install)
  - [Usage](#usage)
- [Faker](#faker)
  - [Install](#install-1)
  - [Usage Example of Phinx Seed with Faker](#usage-example-of-phinx-seed-with-faker)
- [More](#more)

# Installation

```
$ php composer require robmorgan/phinx
```

# Migration

```
$ php vendor/bin/phinx create MyNewMigration
```

Komut sonrası aşağıdaki dosya `20180313142719_my_new_migration.php` gibi benzer bir isimle oluşur.

```php
<?php

use Phinx\Migration\AbstractMigration;

class MyNewMigration extends AbstractMigration
{
    /**
     * Change Method.
     *
     * Write your reversible migrations using this method.
     *
     * More information on writing migrations is available here:
     * http://docs.phinx.org/en/latest/migrations.html#the-abstractmigration-class
     *
     * The following commands can be used in this method and Phinx will
     * automatically reverse them when rolling back:
     *
     *    createTable
     *    renameTable
     *    addColumn
     *    renameColumn
     *    addIndex
     *    addForeignKey
     *
     */
    public function change()
    {

    }
}
```

- `php vendor/bin/phinx migrate` komutu verdiğimizde change içindeki komut çalışır.
- `php vendor/bin/phinx rollback` komutu verdiğimizde change içinde verdiğimiz işlemin tersi yapılır ve yaptığımız değişiklikler geri alınır.
- Migrate yapınca database içine Phinx için özel bir tabloya bunun logu tutuluyor, aynı şekilde rollback yapınca bu log siliniyor. Phinx programı neyi migrate edip etmeyeceğini logları takip ederek karar veriyor.
- Phinx programına config dosyası tanımlayabiliyoruz.
- Phinx `conposer` ile kurulabiliyor.

# Seed

Dummy data üretmek için `phinx`'in sunduğu bir özellik. `Migration` açar gibi seed açıyoruz ve dummy data oluşturuyoruz.

Seed dosyası oluşturmak:

```
$ php vendor/bin/phinx seed:create UserSeeder
```

Özel seed dosyası çalıştırma:

```
$ php vendor/bin/phinx seed:run -s UserSeeder
```

Tüm seed dosyalarını çalıştırmak:

```
$ php vendor/bin/phinx seed:run
```

# Phinx Migrations Generator

[Github Linki](https://github.com/odan/phinx-migrations-generator)

Bu araç ile tablo üstünde manüel olarak yaptığımız değişiklikleri tek komut ile otomatik olarak migration dosyasına çevirmek mümkün.

## Install

```
$ composer require odan/phinx-migrations-generator --dev
```

## Usage

Otomatik olarak Phinx'in konfigürasyon dosyasını kullanıyor. Phinx'in konfig dosyasını kullanarak bu araç için özel olarak 4 farklı ayar yapılabiliyor.

```
$ vendor/bin/phinx-migrations generate
```

# Faker

[Github Linki](https://github.com/fzaninotto/Faker)

Bu kütüphane ile kolayca fake data üretebiliyoruz.

## Install

```
$ composer require fzaninotto/faker
```

## Usage Example of Phinx Seed with Faker

```php
<?php


use Phinx\Seed\AbstractSeed;

class UserSeeder extends AbstractSeed
{
    /**
     * Run Method.
     *
     * Write your database seeder using this method.
     *
     * More information on writing seeders is available here:
     * http://docs.phinx.org/en/latest/seeding.html
     */
    public function run()
    {
        $faker = Faker\Factory::create();
        $data = [];
        for ($i = 0; $i < 100; $i++) {
            $data[] = [
                'username'      => $faker->userName,
                'password'      => sha1($faker->password),
                'password_salt' => sha1('foo'),
                'email'         => $faker->email,
                'first_name'    => $faker->firstName,
                'last_name'     => $faker->lastName,
                'created'    => date('Y-m-d H:i:s'),
            ];
        }

        $this->table('users')->insert($data)->save();
    }
}

```

# More

Bu aracın en temel özellikleri bunlar, daha bir çok özelliği var. Örneğin migration dosyası oluştururken template kullanmak gibi. Resmi doküman takip edilebilir.