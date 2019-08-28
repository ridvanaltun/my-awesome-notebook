# Phinx

PHP için yazılmış bir migration aracı. Migration dosyalarını PHP betiği olarak yazıyoruz ve program üstünden migration işlemlerini yürütüyoruz.

`php vendor/bin/phinx create CreateStokDurumuTable`

Bu komutu girince mesela `migrations` klasörü altında `20180313142719_create_stok_durumu_table.php` şeklinde bir dosya açıyor ve içine `CreateStokDurumuTable` adında bir class açıyor.

Aşağıdaki örnek incelenebilir.

```php
<?php
use Phinx\Migration\AbstractMigration;

class CreateStokDurumuTable extends AbstractMigration
{

    /**
     * Migrate Up.
     */
    public function up()
    {

    }

    /**
     * Migrate Down.
     */
    public function down()
    {

    }
}
?>
```

`php vendor/bin/phinx migrate` komutu verdiğimizde `up` fonksiyonu çalışıyor.

`php vendor/bin/phinx rollback` komutu verdiğimizde `down` fonksiyonu çalışıyor.

Migrate yapınca database içine Phinx için özel bir tabloya bunun logu tutuluyor, aynı şekilde rollback yapınca bu log siliniyor. Phinx programı neyi migrate edip etmeyeceğini logları takip ederek karar veriyor.

Phinx programına config dosyası tanımlayabiliyoruz. Ayrıca Phinx `conposer` ile kurulabiliyor.

# Warning

Phinx'in dokümanını okuduğumda `0.2.0` versiyonu ile change adında bir fonksiyon eklendiğini öğrendim. Change kulandığımızda up ve down methodları geçersiz sayılıyor ve otomatik up down işlemleri tek fonksiyon altında handle ediliyor, yani ayrı ayrı yazmak yerine sadece change'e yazarak hem up hem down yazmış oluyoruz. Change kullanma zorunluluğumuz yok, change kullanmayıp up ve down methodlarını kullanabiliriz yine de.

# More

Bu aracın en temel özellikleri bunlar, daha bir çok özelliği var. Örneğin migration dosyası oluştururken template kullanmak gibi. Resmi doküman takip edilebilir.