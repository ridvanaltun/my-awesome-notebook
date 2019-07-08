# What is jQuery

Bu çok eski bir `javascript freamwork`. Bu freamwork ile geliştirilmiş çok eski ve devam eden projeler olduğu için ölmeyecek deniyor. Sloganı: `write less, do more`.

React, Vue gibi framework'ler jQuery ile aynı klasmana giriyor aslında. Temelde bu kütüphanelerin amacı HTML DOM yapısını daha kolay manipüle edebilmemizi sağlamak.

Açılımı `javascript query` yani amacı bir nevi SQL ile data çeker gibi elementleri getirip kullanabilme kolaylığı sağlamak.

jQuery'nin bir çok yan dal projesi var, `jQuery Mobile` gibi. Bu dokümanda sadece yalın jQuery'den bahsedicem.

# Table of Contents

<!-- toc -->

- [Should I Learn jQuery?](#should-i-learn-jquery)
- [Before Start](#before-start)
- [An Example](#an-example)
- [Modifying the Properties of Elements](#modifying-the-properties-of-elements)
- [Cleaning Properties of Elements](#cleaning-properties-of-elements)
- [Adding Elements to Page](#adding-elements-to-page)
- [Access Events](#access-events)
- [Change Styles](#change-styles)
- [Show / Hide Element](#show--hide-element)
- [Show / Hide Style Property with Animation](#show--hide-style-property-with-animation)
- [Get Value From Element](#get-value-from-element)
- [Assign Value to Element](#assign-value-to-element)
- [Extra](#extra)

<!-- tocstop -->

# Should I Learn jQuery?

jQuery eski projelerde kullanıldığı için ileride karşıma çıkma ihtimali olan bir konu. jQuery ile Javascript DOM Manipulasyonu arasındaki farkı ve sağladığı kolaylıkları görmek için bu dokümanı oluşturuyorum. Ayrıca ileride jQuery kodu gördüğümde ne işe yaradığını anlamak ve bu dokümana başvurabilmek için `cheat sheet` bir dokümana ihtiyacım var.

# Before Start

Body etiketinin içinde en alta `.js` dosyamızı ekliyoruz. 

`.js` dosyasının min ve normal sürümleri mevcut. Min sürümünde space karakteri kaldırılmış yani dosya minified edilmiş oluyor. Production için minified sürümü kullanılmalı, geliştirme sürecinde debug amaçlı normal sürüm kullanılmalı.

# An Example

```javascript
$(document).ready(function() {
        $("h1").click(function(){
            alert("Hello Wolrd!");
        });
});
```

jQuery'i kullanabilmek için sayfamızın hazır hale gelmesini beklememiz gerekiyor, bu sebeple `$(document).ready();` şeklinde bir ifade kullanıyoruz.

```javascript
$(document).ready(function() {
    $("h1").click(function() { // etiket
        $("h1").hide();
    });
    $("#p1").click(function() { // id
        $("#p1").hide();
    });
    $("div.p3").click(function() { // etiketi div class'ı p3 sahip etiketler
        $("div.p3").hide();
    });
});
```

Görüldüğü üzere id, sınıf ve etiket üstünden elementlere erişebiliyoruz.

# Modifying the Properties of Elements

```javascript
// Özelliği öğren
$(seçici).attr(özellik);

$("#rsm").attr("src"); // rsm id'ine sahip etiketin src özelliğini döndürdük

// Özelliği değiştir
$(seçici).attr(özellik, değeri);

$("#rsm").attr("src", "a.jpg") // src özelliğini a.jpg olarak değiştirdik.
```

# Cleaning Properties of Elements

```javascript
// Elemanın özelliklerini siler
&(secici).emty();

// Elemanı siler
$(secici0).remove()
```

# Adding Elements to Page

```javascript
// Ön tarafa veri eklemek
$(secici).prepend(veri);

// Sonrasına veri eklemek
$(secici).append(veri);

// Sonrasına veri eklemek ??
$(secici).after(veri);

// Öncesine veri eklemek
$(secici).before(veri);
```

# Access Events

```javascript f
// Temel yapı
$(secici).olay()

// Örnek
$("#goster").click(function() {
    $("h1").show();
});
```

# Change Styles

```javascript
// Stil değiştirmek
$(secici).css("özellik", "deger");

$("h1").css("color", "blue");

// Still Eklemek
$(this).addClass("secici");

//Still Kaldırmak
$(this).removeClass("secici");

// Stil yoksa eklemek varsa kaldırmak
$(this).toggleClass("secici");
```

# Show / Hide Element

```javascript
// Elementi gizle
$(secici).hide();

// Elementi göster
$(secici).show();

// Elementi yavaş gizle
$(secici).hide("slow");

// Elementi hızlı göster
$(secici).show("fast");

// Elementi 2 saniyede göster
$(secici).show(2000);

// Element gizliyse göster gösterilmişse gizle
$(secici).toggle();
$(secici).toggle(2000);
$(secici).toggle("slow");
$(secici).toggle("fast");
```

```javascript
/*

    Fade Animasyonu

    Elementin transparanlık seviyesi ile göster veya gizle.

*/

// Elementi fade animasyonu ile göster
$(secici).fadeIn();
$(secici).fadeIn(2000);
$(secici).fadeIn("slow");
$(secici).fadeIn("fast");

// Elementi fade animasyıonu ile gizle
$(secici).fadeOut();
$(secici).fadeOut(2000);
$(secici).fadeOut("slow");
$(secici).fadeOut("fast");

// Elementi fade animasyonu ile gösterilmişse gizle gizlenmişse göster
$(secici).fadeToggle();
$(secici).fadeToggle(2000);
$(secici).fadeToggle("slow");
$(secici).fadeToggle("fast");
```

```javascript
/*

    Slide Animasyonu

    Elemnti aşağıdan çıkar, yukarıya doğru götürerek gizle.

*/

// Elementi Slide animasyonu ile göster
$(secici).slideDown();
$(secici).slideDown(2000);
$(secici).slideDown("slow");
$(secici).slideDown("fast");

// Elementi slide animasyonu ile gizle
$(secici).slideUp();
$(secici).slideUp(2000);
$(secici).slideUp("slow");
$(secici).slideUp("fast");

// Elementi slide animasyonu ile gizlenmişse göster gösterilmişse gizle
$(secici).slideToggle();
$(secici).slideToggle(2000);
$(secici).slideToggle("slow");
$(secici).slideToggle("fast");
```

```javascript
/*

    Göster / Gizle Fonksiyonlarında Callback Kullanımı

    Fonksiyonun içinde hız parametresinden sonra bir callback fonksiyonu çağırmamız mümkün.
    Bu sayede animasyonun hemen ardından yapıalcak işleri tanımlayabiliyoruz.
    
*/

$(document).ready(function() {
    $("#btn1").click(function(){
        $("#rsm1").slideUp("slow", function(){
            $("#rsm2").slideDown("fast");
        });
    });
});
```

# Show / Hide Style Property with Animation

```javascript
$(secici).aniamte({ stil özellikleri }, hız, callback);
```

# Get Value From Element

```javascript
// Input ile değer al
alert($("#p11").val());

// Elementin değerini al
alert($("#p11").html());
alert($("#p11").text());
```

# Assign Value to Element

```javascript
$("#p11").text($("#btn1").val());

// Radiobutton'dan değer almak
var value = $('input[name:c]:checked').val();
```

# Extra

[JQuery Eğitim Serisi](https://www.youtube.com/playlist?list=PL_f2F0Oyaj4-Jt6F-dyl6LEHNVaudH3rk)