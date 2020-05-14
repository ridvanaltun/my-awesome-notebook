# Basic Auth

Bir web sayfasına girdiğimizde karşımıza çıkan pop-up şeklindeki kullanıcı adı-şifre girdiğğimiz kimlik doğrulama sistemi. Kimlik bilgisi bir text dosyasından okunur ve doğrulanır.

Formu doldurmadan şu şekilde giriş sağlanabilir: `http://username:password@domain.com`.
Veyahut header ile giriş sağlanabilir, `Authorization: Basic base64('username:password')`.

Örnek bir basic-auth heade'ı aşağıda verilmiştir:

```
Authorization: Basic c2FsYTpzYXNkYQ==
```

Base64 algoritmasının kırılması çok kolay bu sebeple SSL ile kullanılması öneriliyor.