# Session

`Cookie` ile `authentication` işlemini aslında `session` yöntemi ile yapıyoruz. Session tarayıcının kapanmasıyla silinir ancak `cookie` bizim belirlediğimiz `son kullanma tarihi`ne kadar sistemde kalacaktır.

Konu hakkında detaylı bir kaynak: [HTTP İşleyişi ve Güvenliği Açısından Cookie ve Session Yönetimi](https://www.netsparker.com.tr/blog/web-guvenligi/http-isleyisi-ve-guvenligi-acisindan-cookie-ve-session-yonetimi/). Çok uzun ve detaylı bir konu olduğu için her şeyi öğrenme ve yazma gereği duymadım.

## Cookie

`Cookie`'ler 2'ye ayrılır. Birisi `sension cookie` diğer `persistent cookie`. Kullanıcı bir web servisine giriş yaparken `beni hatırla` kısmını seçerse `cookie`'ye bir `expired date` atanır. Kalıcı cookie'ler ile kullanıcının dil seçeneği kayıt edilebilir mesela. `persistent cookie`'lere ayrıca `Tracking Cookie` de denir.

Siteler artık yeni yasa gereği `persistent cookie` kaydederken iznimizi istiyor. 

`Cookie` oluşturmak kolay bir iş. `Client`'a `response` olarak bir HEADER belirteci ile yarayıcının cookie kaydı yapması sağlanabilir.

Aşağıda bir HTTP request sonrası sunucu tarafından oluşturulmuş bir HTTP response'un header'ı yer alıyor.

```
HTTP/1.1 200 OK
Content-Type: text/html
Set-Cookie: CookieName=CookieValue; path=/;
```

`Cookie` ile gelen tüm bilgiler `;` ve space ile birbirinden ayrılır. Yukarıdaki örnekte bu durum görülüyor zaten.

HTTP response içindeki `Set-Cookie` yapısı aşağıdaki gibi olabilir.

`Set-Cookie: value[; expires=tarih][; domain=alan adı][; path=path][; secure]`

`expires`, `domain`, `path` adında özel key'lerimiz var. Bunun dışında kendimiz özel olarak veri ayarlayabiliyoruz.

Domain kısmı default olarak request yapan adresin domain'idir. Domain kısmına kendi domainimiz dışında bir adres yazamayız, yazarsak eğer client tarafındaki program bunu engeller.

| Cookie Set Eden Domain | Cookie'nin Gönderileceği Domainler                                  | Cookie'nin Gönderilmeyeceği Domainler          |
|------------------------|---------------------------------------------------------------------|------------------------------------------------|
| www.example.com        | www.example.com other.www.example.com                               | example.com art.example.com other.example.com  |
| art.example.com        | art.example.com other.art.example.com                               | example.com www.example.com  other.example.com |
| example.com            | example.com art.example.com  www.example.com  any.other.example.com |                                                |

`expires`, `cookie`'nin ne zaman biteceği belirtilir. Yazılmazsa eğer session kapandığında cookie direkt silinir. Yazılırsa eğer `persistent cookie` değerini elde etmiş oluruz.

`path` ile `cookie` hangi linkte kullanılacağı belirtilir. `/` kullanırsak bütün linklerde kullanılacak demek, `/login` dersek mesela `site.com/login`, `site.com/login/foo` gibi adreslerde cookie kullanılır. Eğer `path` ayarlanmazsa varsayılan path cookie'yi set eden sayfanın cookie değeri kabul edilir.

En son kısımda `httponly` kullanabiliriz. Bu şekilde `cookie` JavaScript ile erişimi kapatılır, bu sayede `cookie`'nin `XSS` ile çalınmasının önüne geçilebilir.