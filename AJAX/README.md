# AJAX

Açılımı: Asynchronous Javascript and XML

AJAX bir teknoloji değil, bir geliştirme yöntemidir. `SPA` siteler tasarlamak için kullanıyoruz bu yöntemi. Kısaca sayfa hiç yenilenmeden, asenkron bir şekilde sunucu ile iletişim kurmamız için kullanabileceğimiz bir yöntem.

Eskiden `XML` çokça kullanılıyormuş ancak artık `JSON` tercih ediliyor. Bunun sebebi daha kolay, daha hızlı ve daha popüler olması. `AJAX` içinde `XML` geçse dahi `JSON` kullanırken `AJAX` yapısını kullanabiliriz. Hatta düz yazı ile bile `AJAX` yapısını kullanabiliriz.

`AJAX` yapısını düz JS ile yapabiliriz. İşlerimizi kolaylkaştırması için framework'de tercih edebiliriz.

# XHR

`XMLHttpRequest` yani `XHR` objesi.

Bu obje tüm modern tarayıcıların JS ortamında mevcut.

## Alternatives

AJAX kullanmak için bu objeyi kullanmak zorunda değiliz. Bunun yerine aşağıdaki kütüphane veya methodları kullanabiliriz:

- Axios
- Superagent
- Fetch API
- jQuery
- Node HTTP

Fetch API tarayıcılara implemente edilmiş, tüm tarayıcılar-eski tarayıcı sürümleri ile desteği yok, bu sebeple kullanırken dikkatli olmak gerkiyor.

jQuery ağır bir kütüphane, sadece AJAX için kullanmak ağır kaçabilir.

## Usage

```javascript
function go_for_it() {
    var xhr = new XMLHttpRequest();
    // Son değer ile işlemi senkron mu çalıştırılacak bu belirleniyor
    xhr.open('GET', 'foo.com/bar', true);

    xhr.onload = function() {
        // this.status === xhr.status,
        // fonksiyon içinde this, xhr objesini işaret ediyor
        if(this.status == 200) {
            console.log(this.responseText);
        }
    }

    // Sends request
    xhr.send();
}
```

`onreadystatechange` fonksiyonu sunucudan yanıt döndükten sonra çalışacak olan fonksiyondur. Bu method genellikle `readyState` ifadesi ile birlikte kullanılır. `readyState` ifadesinin 4 olmasına bakılır, eğer 4'e eşitse response sonrası yapılmak istenen işlemler gerçekleştirilir.

ReadyState 5 farklı değerde olabilir:

- 0: request not initialized
- 1: server connection established
- 2: request received
- 3: processing request
- 4: request finished and response ready

```javascript
function go_for_it() {
    var xhr = new XMLHttpRequest();
    xhr.open('GET', 'foo.com/bar', true);
    // State ready olduğunda request at
    xhr.onreadystatechange = function() {
        if(this.readyState == 4 && this.status == 200) {
            console.log(this.responseText);
        }
    }

    xhr.send();
}
```

Aşağıdaki örnekte JSON bir veri çektik ve parçalayıp içinden ihtiyacımız olan bir veriyi aldık.

```javascript
function go_for_it() {
    var xhr = new XMLHttpRequest();
    xhr.open('GET', 'foo.com/bar', true);
    xhr.onload = function() {
        if(this.status == 200) {
            var user = JSON.parse(this.responseText);
            console.log(user.name);
        }
    }

    xhr.send();
}
```