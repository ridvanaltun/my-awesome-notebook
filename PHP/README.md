# PHP

PHP öğrenirken aldığım notlar.

## Should Know

```php
// Her php betik yani php dosyası aşağıdaki şekilde başlayıp sonlanmalı
<?php
    // betik..
?>
```

## Variable

```php
$var = "foo";

// ??
static $var = 0;

// ??
global $var = 0;
```

## Exprassions

```php
// Değişkeni +1 arttır
$var++;
```

## Echo & Print

```php
echo "Hello World";
```

## Array

```php
$arr = array(
    'id' => 5,
    'name' => "foo",
    'job' => null,
);

echo "id is {$arr['id']}!"; // id is 5!
```

## Class

```php
class Vehicle {
    function __construct() {
        // Bu kısım her obje oluşturduğumuzda kullanılır.
        print "Araç oluşturuldu.";
    }
}

class Car extends Vehicle {
    // Aracın özellikleri arabaya verildi.
    function __construct() {
        parent::__construct();
        print "Araba oluşturuldu.";
    }
}

class Almera extends Car {
    function __construct() {
        print "Almera oluşturuldu.";
    }
}

// Yeni nesneler üretiyoruz

$obj = new Vehicle();
// Araç oluşturuldu.

$obj = new Car();
// Araç oluşturuldu.
// Araba oluşturuldu.

$obj = new Almera();
// Almera oluşturuldu.
```

## Encapsulation

Tanımladığımız değişken ve fonksiyonların kapsama alanlarını bazı ön eklerle sağlayabiliyoruz.

Public ise her yerden erişebiliriz.
Private ise sadece bulunduğu class içinden erişebiliriz.
Protected ise class extends edildiğinde diğer classalrda erişebilir ancak class dışında kullanılamaz.

Scope: Public > Protected > Private

```php
// Public
public $variable;
public function doSomething() {
  // ...
}

// Private
private $variable;
private function doSomething() {
  // ...
}

// Protected
protected $variable;
protected function doSomething() {
  // ...
}
```

!!! Eğer scope belirten bir kelime ile tanımlama yapılmamışsa default olarak `public` scope tanımı kullanılıyor.

# Functions

PHP ile kullandığımız bazı fonksiyonlar.

## exit() : void

Betiği sonlandırır.

exit(5) // hata koduyla betik sonlanır.

## die()

Betiği sonlandırır. Exit ile arasında ufak bir fark var. ??

die('there') // die öncesi istediğimiz yazıyı print edebiliriz

## include('foo.php')

Betiğe başka betikler ekleyebiliriz.

## include_once('foo.php')

Betiğe bir kez belirtilen dosyayı ekleme işi yapılır. örneğin birden fazla aynı dosya farklı betikler aracılığıyla eklenmek istendi, sadece bir kez alır.

## defined("foo") : bool

Belirtilen değişken sabit ise -const- true döndürür.

## isset($foo, ...) : bool

Belirtilen değişken tanımlı ise TRUE döndürür. Değer NULL ise FALSE döndürür.

## unset($foo) : void

Belirtilen değişkeni kaldırır.

## function_exists("foo") : bool

Belirtilen fonksiyon tanımlı ise TRUE döndürür.

## array_key_exists("foo", $arr) : bool

Belirtilen key dizi içinde mevcut ise TRUE döndürür.

## var_dump($foo) : void

JavaScript te console.log() neyse PHP için var_dump() o.
Hata tespiti amaçlı kullanabiliriz.

## explode($ayrac, $text) : array

Verdiğimiz ayraç ile dizi üretir.

```php
$foo = "Ali Veli Mehmet";
$arr = explode(" ", $foo);
echo $arr[0]; // Ali
```

## basename($path, ? $suffix) : string

Dosya yada klasör yolu gösteriyoruz. Suffix verirsek dosya uzantısını kaldırabiliriz.

```php
echo basename("/etc/sudoers.d", ".d"); // sudoers
echo basename("/etc/sudoers.d"); // sudoers.d
echo basename("/etc/passwd"); // passwd
echo basename("/etc/"); // etc
echo basename("/"); //
echo basename("."); // .
```

## implode("-", $arr) : string

Verdiğimiz array'in içindeki string'i arasında verdiğimiz parametre olacak şekilde birleştirir ve döndürür.

## list(...) : var

JavaScript ES6 içindeki `const { var } = this.porps;` gibi bir şey. 

```php
$data = array('ali', 'veli', 'ahmet');
list( , $foo, $bar) = $data;

echo $foo; // veli
```
# MySQL Related Functions

DB işlemleri için kullandığımız fonksiyonlar.

## mysqli_real_escape_string($con, $str) : string

Gelen input'ları mysql server üstünde mysql ile kullanılabilir hale çeviriyoruz. Bunun sebebi muhtemelen regex ile bazı rare case'ler açık kalıyor. Amaç güvenlik. Artık verimizi sql cümle içinde kullanabiliriz.

Bu işin regex ile değilde mysql server içinde yapılması biraz garip ancak düşününce neden yapıldığı anlaşılıyor.

İnsanlar input olarak escape karakterler girebilirler, sql cümlemizi bozuk istedikleri şekle getirebilirler ki bunu istemediğimiz için böyle bir şey yaptık.
