# Authentication

Kimlik doğrulama ve yetkilendirme üzerine notlarım.

# Table of Contents

- [Authentication](#authentication)
- [Table of Contents](#table-of-contents)
- [Session](#session)
  - [Cookie](#cookie)
- [JWT](#jwt)
- [OAuth](#oauth)
  - [OAuth1 vs OAuth2](#oauth1-vs-oauth2)
  - [Auth0](#auth0)
  - [OIDC](#oidc)
- [Techinical Terms](#techinical-terms)

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

# JWT

Açılımı: JSON Web Tokens

Popüler bir yöntem. Session kullanmadan Authentication ve Authorization işimizi hallediyor. RESTful servislere entegre edilebiliyor. Session normalde sisteme depolanır ancak JWT ile sessipn'a depolamaya gerek kalmıyor. Session kullanırken `expired date` yönetimi daha kolay.

[Express ile JWT'nin birlikte kullanımına örnek](https://juffalow.com/javascript/express-server-with-jwt-authentication)

[A guide for adding JWT token-based authentication to your single page Node.js applications](https://medium.com/dev-bits/a-guide-for-adding-jwt-token-based-authentication-to-your-single-page-nodejs-applications-c403f7cf04f4)

Elimizdeki token içinde `expired date` oluyor, her get isteği atıldığında karşıdaki kişi `JWT token`'ı göndermek zorunda. Client tarafı token almak için `POST` isteği ile kullanıcı adı / şifre gönderiyor ve gelen token'ı kaydediyor. Sonrasında her request'te bu token kontrol ediliyor sunucu tarafında. Client her seferinde lindeki token'ı request'in HEADER'ına koyuyor. Sunucu tarafında algoritma token'ı gözden geçirip 200, 401, 403, 404 gibi yanıtlar dönüyor bu sayede.

`JWT`, `stateless` çalışır, yani her kimlik doğrulama aşamasında sunucu tarafı bizim hakkımızda hiç bir bilgiye sahip değil.

Aşağıda örnek bir `JWT` token mevcut.

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyX2lkIjoiMTMyMzQ1Njc4OTAiLCJzY29wZXMiOlsiYWRkUG9zdCIsImRlbGV0ZVBvc3QiXX0.1uoEIlEU7v3VQdXUjCrJ-BOXw4PpAvcj3rl1HmQ7LWI
```

Ürettiği `token` 3 parçadan oluşuyor ve bu parçalar `.` ile birbirinden yarılıyor. Yani bir JWT içinde 3 ayrı bilgi var.

1. İlk parça header, içinde imzanın hanagi algoritma ile üretileceği hakkında bilgi taşıyor
2. İkinci parça payload, bu parça içinde verilerimiz taşınıyor
3. Son parça signature, payload ve header parçaları kullanılarak oluşturuluyor

İhtiyacımız olan tüm veriler token içinde olduğu için sunucu tarafında veritabanına bağlanıp gelen token'ın doğru olup olmadığını kontrol etmemize gerek kalmıyor.

HEADER örneği;

```JSON
{
  "alg": "HS256",
  "typ": "JWT"
}
```

Paylaod örneği:

```JSON
{
  "user_id": "13234567890",
  "scopes": ["addPost", "deletePost"]
}
```

HEADER ve `payload` kısımları `base64` ile `encrypte` edilebilir. `Payload` kısmına bu sebeple hassas bilgiler konmaması gerekiyor.

HEADER ile belirlediğimiz şifreleme algoritması ile `OAuth` vs olaylara girebiliyoruz, bu kısmın böyle bir önemi var kısaca.

# OAuth

Açılımı: Open Authorization

`JWT` için token format, `OAuth` için bir protokol deniyor.

Bazı sitelerde bulunan `login with google`, `login with facebook` gibi şeyler bu protokol ile yapılıyor. Webservislerini daha kolay kullanmak, belki tek tıkla servisi kullanabilmek için kullanılabilir.

`OAuth Mekanizması` temelde 3 parçadan oluşuyor, bunlar:

- OAuth Provider (Facebook, Google vs.)
- OAuth Client (login with google gibi butonlarla giriş imkanı sağlayan site vs.)
- Owner (Siteye facebook, google vs. ile giriş yapan kişi)

## OAuth1 vs OAuth2

- OAuth2 daha basit
- OAuth2 browser tabanlı olmayan servisler ile daha uyumlu
- OAuth2 ile token oluştururken `SSL` bağlantısı kurmak zorunlu
- OAuth1 de her API isteğinde ayrı ayrı 2 adet token göndermek zorundaydık, OAuth2 ile sadece bir kez 1 token gönderiyoruz

OAuth2'de iletişim `SSL` üstünden sağlandığı için OAuth1'deki komplekslik hayli azalıyor.

## Auth0

Bir web servisi. Projemize bunu ekleyerek web servisimize auth kısmı yazmaktan kurtuluyoruz. Popüler bir sistem. Literetürde `Identity Management Platform` olarak geçiyor.

## OIDC

Açılımı: OpenID Connect

`OAuth2`'ye dayalı bir `authentication protocol`. Tek bir kullanıcı hesabıyla bu sistemi kullanan bütün sitelere girebiliyoruz. Bir nevi `Microsoft Pasaport`, `Google Account`, `Monzilla Persona`, `Facebook Connect` gibi bir yapı.

// TODO

# Techinical Terms

Authentication: Sisteme girerken yapılan kimlik doğrulama işi. Bir kez yapılır.

Authorization: Sisteme giriş yapıldıktan sonra her işlemde yapılır. Bu durum geliştirdiğimiz uygulamanın yapısına göre değişebilir.