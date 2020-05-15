# OAuth (Open Authorization)

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

## OIDC (OpenID Connect)


`OAuth2`'ye dayalı bir `authentication protocol`. Tek bir kullanıcı hesabıyla bu sistemi kullanan bütün sitelere girebiliyoruz. Bir nevi `Microsoft Pasaport`, `Google Account`, `Monzilla Persona`, `Facebook Connect` gibi bir yapı.

// TODO