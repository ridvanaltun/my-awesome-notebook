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