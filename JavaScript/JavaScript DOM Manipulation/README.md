# Javascript DOM Manipulation

HTML'in sonu ML, yani `Markup Language` olarak bitiyor, her markup language yapısında (XML, HTML vs.) etiket sistemi vardır, bu etiketler kendi aralarında hiyerarşi oluşturur. Javascript'in doğma amacı DOM manipulasyonu yaparak interaktif sayfalar hazırlayabilmek. Yani Javascript, bir dokümanın elementlerine ulaşıp değiştirebilmemiz için geliştirildi. Dokümanın elementlerine erişmek için de bahsettiğim hiyerarşiyi kullanıyoruz.

Biz Javascript kullanarak HTML'de bulunan hiyerarşi yapısına;

- Element ekleyebilir yada silebiliriz.
- Elementlerin yani etiketlerin içine atanmış özelliklere müdahele edebiliriz.
- Elementlerin arasında bulunan içeriklere müdahele edebiliriz.
- Elementlerin stiline (CSS) ulaşabiliriz.

Kısaca HTML içindeki etiketleri birer obje olarak düşünüp bu şekilde objelere erişip manipüle edebiliyoruz.

# Table of Contents

<!-- toc -->

- [Change Inner HTML](#change-inner-html)
- [Accessing HTML Objects](#accessing-html-objects)
- [Change Attribute](#change-attribute)
- [Change Style](#change-style)
- [Object Operations](#object-operations)
- [Events](#events)
- [Examples](#examples)
  * [Example 1](#example-1)
  * [Example 2](#example-2)
  * [Example 3](#example-3)
  * [Example 4](#example-4)

<!-- tocstop -->

## Change Inner HTML

`innerHTML` parametresi ile bir HTML etiketi kapsadığı alanı düzenlememiz mümkün.

```javascript
document.getElementById("foo").innerHTML = "Hello World!";
```

## Accessing HTML Objects

3 farklı şeklide HTML elementine erişebiliyoruz; Id, etiket ve class adına göre.

```javascript
var foo = document.getElementById("foo");
var bar = document.getElementByTagName("bar"); // a, h1 gibi..
var baz = document.getElementByClassName("jam");
```

## Change Attribute

`Attribute` yani etiket içinde yazdığımız parametreler.

```javascript
var element = document.getElementById("foo");

element.attribute = foo;

// veya

element.setAtribute(attribute, foo);
```

## Change Style

Bu şekilde elementin stil (CSS) özelliklerini değiştirebiliriz.

```javascript
var element = document.getElementById("foo");

element.style.property = foo;
```

## Object Operations

```javascript
document.createElement(eleman) // HTML elemanı oluştur
document.removeChild(eleman) // elemanı sil
document.appendChild(eleman) // varolan elemana ekle
document.replaceChild(yeni, degisecek) // html elemanını değiştir
document.write(text) // ekrana yazdır
```

## Events

Örneğin; OnMouseOver, OnMouseOut, OnLoad, OnUnload, OnChange..

```javascript
// Id adresi verilen elemana event ekle.
document.getElementById("foo").onclick = function(){}
```

HTML objelerinin `onload` ve `onunload` adında 2 adet özelliği var. Sayfa yüklenince yada kullanıcı sayfadan ayrılınca çalıştırılacak eventler.

```javascript
var element = document.getElementById("foo");

// Sayfa yüklenince çalıştırılacak fonksiyon
element.onload = function() {}
```

Başka bir örnek ise `onchange` özelliği, onchange; Elementin içindeki değer değişince çalıştırılır.

## Examples

Javascript DOM manipulasyonunun pratikte nasıl çalıştığını görmek ve daha hızlı anlamak için örnekler yardımcı olabilir.

### Example 1

```html
<!DOCTYPE html>
<html>
    <body>
        <button id='myButton1' onclick="alert('merhaba')"> Button 1</button>
        <button id='myButton2'> Button 2</button>
        <button id='myButton3' onclick="button3Click();"> Button 3</button>
        <script type="text/javascript">
            document.getElementById('myButton2').onclick=function(){ alert('merhaba'); }
            function button3Click(){ alert("merhaba"); }
        </script>
    </body>
</html>
```

Burada üç butonda aynı işi yapıyor. Birinci butonda direkt olarak javascipt kodunu `onclick` içine yazdık. 

İkinci butonda DOM yapısı içindeki id'si myButton2 olan elemente ulaştık ve elementin onclick özelliğini sonradan oluşturduk.

Üçüncü butonda direkt olarak javascript içinden fonksiyon çağırıyoruz. Fonksiyon ile javascript kodu çalıştırmanın faydası, tekrar eden işleri kolaştırmak, tekrarları kaldırıp basit yapılar oluşturmak.

### Example 2

Şimdi ise bir elementin içindeki veriyi değiştireceğiz.

```html
<!DOCTYPE html>
<html>
    <body>
        <p id="paragraf">Merhaba</p>
        <script type="text/javascript">
            document.getElementById('paragraf').innerHTML="<b>Tadaa</b>";
        </script>
    </body>
</html>
```

Ekranda göreceğimiz yazı kalın şekilde Tadaa olacaktır.

### Example 3

```html
<!DOCTYPE html>
<html>
    <body>
        <img id="resim" src="img/1.png">
        <br>
        <button id="imgDegistir">Resmi Değiştir</button>
        <script type="text/javascript">
            document.getElementById('imgDegistir').onclick=function(){
            document.getElementById('resim').src = "img/2.png";}   
        </script>
    </body>
</html>
```

Bu örnekte buton tıklamasıyla bir resmi değiştiriyoruz.

### Example 4

Bu örnekte elementin `style` özelliğine erişip değiştireceğiz.

```html
<!DOCTYPE html>
<html>
    <body>
        <p id="paragraf">Merhaba</p>
        <script type="text/javascript">
            document.getElementById('paragraf').style.color = "blue";
        </script>
    </body>
</html>
```

Merhaba yazısı mavi olacaktır.