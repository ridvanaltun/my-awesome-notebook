# TypeScript

Bu bir Microsoft teknolojisi. `Superset` olarak tanımlanıyor. Amacı JavaScript'i `C#`, `Java` gib i dillere benzetmek ve JS'de yazması kolay olmayan bazı şeyleri kolaylaştırmak. Birinci önceliği ise `OOP` mantığını entegre etmek.

Bu teknoloji bir `superset` olduğu için güncel JavaScript ile yazdığımız kodu eski tarayıcıların destekleyeceği şekilde `compile` edebiliyor. Ticari uygulamalarda va çok kullanıcılı uygulamalarda bu çözüme ihtiyacımız var. Bu yapılan eski tarayıcı desteği ekleme işine `polyfill` deniyor. `TS` bize `polyfill` desteği sağlıyor.

Kullanımı çok kolay bir program.  `NPM` ile global olarak sistemimize yükleyip `tsc` yani `typescript compiler` programı ile kullanmaya başlayabiliriz.

Bu teknolojinin bize getirdiği en büyük avantaj adında da bulunduğu üzere tip atamaları. Normalde JavaScript `Type Safety` bir dil değil. Yani istediğimiz değişkene istediğimiz tipte bir değer atayabiliyoruz, bu sebeple programımızı çalıştırıp ilgili kodun çalıştırıldığı kısma gelene kadar değişken tipine bağlı olan hataları almıyoruz.

JS `dinamik tip`'li bir dildir. Değişkene istediğimiz değeri atarız. TS ile `static tip` desteği eklemiş oluyoruz.

`TS`'nin eklediği bir çok `OOP` özelliği `ES6` ile geldi zaten. `TS`, `ES6` ile uyumlu gidiyor, yani ileride `ES6` ya geçmek istediğimiz zaman kolay bir şekilde geçebiliriz.

Kısaca compile time hatalar alarak debug süreci hızlanıyor.

`TypeScript` ile değişkenimizin sadece number, string vs. aldığını tanımlıyoruz. Böylelikle hatalarımızı debug etmemiz kolaylaşıyor ve `any type` içeren değişkenleri kullanmamız azalıyor ki bu sayede de daha az hatalar yapıyoruz. Kısaca bize düzgün programlar yazmamızı sağlayacak güzel ve basit bir araç kendisi.

## Usage

tsc yi kurduktan sonra `.ts` uzantılı dosyamıza `tsc deneme.ts` şeklinde komut giriyoruz ve çıktı olarak `deneme.js` dosyası oluşuyor.

İstersek `tsconfig.js` adlı bir dosyayı kök klasörümüze koyarak `tsc` programını proje üstünde kullandığımızda otomatik bazı ayarları geçerli kıldırtabiliriz.

Normalde kodumuzu compile ttiğimizde en düşük ve default `ecmascript 3`'e kadar compile eder. Özel config dosyasında hangi dile kadar uyumlu şekilde compile edebielceğini yazabiliriz.

## Interface

Bu özellik sayesinde kendi veri tipimizi oluşturuyoruz. Örneğin insan adında bir veri tipi oluşturup alabileceği değerleri belirleyebiliriz.

```js
interface Person = {
    name: String,
    height: Number,
    color: String,
}

let ali : Person;

ali.name = 'Ali';
```

## Encapsulation

```js
// ts
class encapsulate {
    private secret : string;
    constructor(){
        this.secret = '';
    }
    set (value: string){
        this.secret = value;
    }
    get (){
        return this.secret;
    }
}
```

```js
// js
var encapsulate = /** @class */ (function () {
function encapsulate() {
    this.secret = '';
}
encapsulate.prototype.set = function (value) {
    this.secret = value;
};
encapsulate.prototype.get = function () {
    return this.secret;
};
return encapsulate;
}());
```
