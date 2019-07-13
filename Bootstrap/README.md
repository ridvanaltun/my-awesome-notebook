# Bootstrap

Boostrap , kısaca bir `CSS framework`. Bu aracın amacı CSS gerektiren bazı işlerimizi kolaylaştırmak, bu aracın genel amacı ise `Responsive Web Design` işini kolay hale getirmek, daha az ve daha anlaşılır kodlar yazıp aynı çıktıyı almak. Bootstrap içinde yüzlerce CSS sınıfı var ve biz bu sınıfları HTML etiketlerine tanımlayarak kolayca kullanabiliyoruz.

Bu framework'ü neden kullanmalıyım? Eğer geliştirdiğim web uygulamasında front-end kısmını önemsemiyorsam ve front-end yükünü azaltmak istiyorsam güzel bir web tasarıma sahip olmak için kullanabilirim.


# Table Of Contents

<!-- toc -->

- [Before Start](#before-start)
- [Usage](#usage)
- [Color Class](#color-class)
- [Table Class](#table-class)
- [Image Class](#image-class)
- [Card Class](#card-class)
- [Warning Messages](#warning-messages)
- [Button Class](#button-class)
- [Icon](#icon)
- [Tag](#tag)
- [Badge](#badge)
- [Breadcrump](#breadcrump)
- [Pagination](#pagination)
- [Lists](#lists)
- [Extra Components](#extra-components)

<!-- tocstop -->

# Before Start

Sayfamız kesinlikle `HTML5` olmalı ve Bootstarp için CSS dosyası ve jQuery dosyasını sayfamıza eklememiz gerekiyor. jQuey dosyasını body etiketinin içine en sona, Bootstrap CSS dosyasını head etiketinin içine en başa koyabiliriz.

İstersek normal Bootstrap CSS dosyası yerine `min` versiyonunu kullanabiliriz.

```html
<meta name="viewport" content="width-width, initial-scale=1">
```

Bu şekilde sayfamız telefondan bakıldığında otomatik küçülecek yada daha büyük bir cihazla bakıldığında ona göre boyutu değişecek.

# Usage

HTML etiketlerine boostrap'ın sahip olduğu sınıfları tanıtıyoruz, bu kadar. Artık sayfayı küçültürken vs telefon-tablet uyumuna kavuşmuş olduk. Ekranı farazi olarak aslında `12` sütuna bölüyoruz ve yatay bir biçimde sıralıyoruz, bunu `4-4-4` yada `6-6` yada `3-3-3-3` gibi şeklinde istediğimiz gibi bölebiliriz. sayfa küçüldükçe yine bizim seçtiğimiz sınıfa göre sağ yada soldaki etiket aşağıya vs. kayıyor olacak.

Grid (Izgara) şeklinde bölüyoruz sayfayı.
Her farklı cihaz için bir alt sınıf bulunur.

- xs: telefonlar
- sm: tabletler
- md: diz üstü
- lg: masaüstü

Boostrap sistemini kullanmak için bunu kullanacağımız bölgeye bir div açıp sınıfı `row` olarak ayarlarız. Sonra her satır için bir div açarız.

```html
<div class="col-sm-4">4 lü sütun</div>
```

Bunun anlamı telefonlar için 4 adet kolon birleştir.
Sitemizde `row` sınıfını bir çok kez kullanıp 12 sütunluk bir sürü hücre ediniriz ve sayfamızı döşeriz.

Biz elementlerimize sınıf atıyoruz ve site boyununa göre otomatik şekilleniyor.

Eğer tasarımda zorlanırsak Bootstrap'ın şablon dosyalarına bakıp yapmak istediğimiz site için fikir alabiliriz yada kendi şablonumuz için kullanabiliriz.

# Color Class

Bootstrap içinde renk sınıfları var, p elementine vs. tanımlayarak default olarak renk değerleri kullanabiliriz sitemizde.

- text-muted
- text-primary
- text-success
- text-info
- text-warning
- text-danger

Arkaplan renkleri

- active
- success
- bg-ingo
- bg-warning
- bg-danger

# Table Class

Table etiketinin sınıfını `table` yaparsak eğer Bootstrap'in tablo tasarımını elde ederiz. Aşağıdaki sınıflar ektra olarak `table` sınıfının yanına eklenebilirler.

```
// kenarlık ekle
table-bordered

//satır rengi degisikligi
table-hover

// çözünürlüklere duyarlı tablo
table-responsive
```

# Image Class

```
//kenarları yuvarlatılmış göster
rounded

//elips filtreli göster
rounded-circle

//çerçeveli göster
img-thumbnail

// çözünürlüğe duyarlı hale getir
img-responsive
```

# Card Class

`card` sınıfı ile kartlar oluşturabiliyoruz.

```html
<div class="container">
    <div class="row">
        <div class="col-sm-3">
            <div class="card card-body bg-light">Bilgiler</div>
        </div>
    </div>
</div>
```

# Warning Messages

- alert alert-info
- alert alert-danger
- alert alert-success
- alert alert-warning

Çıkan mesajın çarpı işareti ile kapatılabilir olması için `fade in` sınıfı eklenebilir. Bu özellik için `Bootstrap jQuery` dosyasını projemize eklemiş olmamız gerekiyor.

```html
<a href="#" class="close" data-dismiss="alert" aria-label="close"></a>
```

Bunun yanında alert `alert-danger fade in` şeklinde sınıf tanımlamamız oluyor.

# Button Class

- btn btn-default
- btn btn-primary
- btn btn-success
- btn btn-info
- btn btn-warning
- btn btn-danger
- btn btn-link

Butonun aktif veya pasif olması için ekstra olarak `active` veya `disabled` sınıfları eklenebilir.

Butonlar farklı çözünürlükler için tanımlanabilir, örneğin `btn btn-default btn-xs`.

Grup şeklinde buton oluşturmak:

```html
<div class="btn-group">
    <button type="button" class="btn btn-default">Kaydet</button>
    <button type="button" class="btn btn-warning">Yoksay</button>
    <button type="button" class="btn btn-danger">Çık</button>
</div>
```

Butonu dikey çizdirmek için: `btn-group-vertical`

# Icon

```html
<span class="glyphicon glyphicon-bell"></span>
```

# Tag

Tag şeklinde kutular. Bir yazıya etiket vermek istediğimizde kullanabiliriz mesela.

```html
<span class="label label-default">default</span>
<span class="label label-primary">primary</span>
<span class="label label-success">success</span>
<span class="label label-danger">danger</span>
<span class="label label-warning">warning</span>
<span class="label label-info">info</span>
```

# Badge

Bu şekilde mesaj kutusundaki mesaj sayısı yada bildirimlerin sayısı gösterilebilir.

```html
<span class="badge badge-secondary">15</span>
```

# Breadcrump

Bu sayede sayfamızda `/` şareti ile `sayfalama` yapabiliriz.

```html
<ul class="breadcrump">
    <li class="breadcrump-item">1</li>
    <li class="breadcrump-item">2</li>
    <li class="breadcrump-item">3</li>
</ul>
```

# Pagination

Kutu şeklinde `sayfalandırma` yapmak için kullanıyoruz.

```html
<ul class="pagination">
    <li class="page-item">1</li>
    <li class="page-item">2</li>
    <li class="page-item">3</li>
</ul>
```

# Lists

```html
<ul class="list-group">
    <li class="list-group-item list-group-item-success"></li>
    <li class="list-group-item list-group-item-info"></li>
    <li class="list-group-item list-group-item-warning active"></li>
    <li class="list-group-item list-group-item-danger"></li>
</ul>
```

# Extra Components

Projemizde Bootstrap ile bütünleşik çalışan tasarımı güzel parçalar katmak mümkün, internette `Extending the Bootstrap` gibi terimler aratmak yeterli. Ayrıca Bootstrap kendisi bazı hazır kompenentleri bize sunuyor.

`Snippet` diye ingilizce bir terim var, kendi başına bir anlamı olmayan, yeniden kullanılabilir kod parçacığı olarak geçiyor literatürde.

[Bu siteden](https://bootsnipp.com/) Bootstrap kullanarak üretilmiş web elementleri bulabiliriz. Örnek olarak site yüklenirken loading ekranını vs önceden hazırlanmış bir `preloader` elementiyle kolaylıkla kullanıcı deneyimi güzel sistemler geliştirebiliriz.

Bootstrap'in kendi geliştirdiği komponentleri [buradan](https://getbootstrap.com/docs/3.3/components/) kullanmak mümkün.