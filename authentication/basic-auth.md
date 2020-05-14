# Basic Auth

Bir web sayfasına girdiğimizde karşımıza çıkan pop-up şeklindeki kullanıcı adı-şifre girdiğimiz kimlik doğrulama sistemi. Kimlik bilgisi genelde `.htpasswd` adına sahip bir dosyadan okunur ve doğrulanır.

Basic-auth ile korunan bir siteye aşağıdaki şekilde erişirsek eğer şifre sorma formunu atlarız:

```
http://username:password@domain.com
```

Basic-auth'un header yapısı aşağıda verilmiştir:

```
Authorization: Basic base64('username:password')
```

Örnek bir basic-auth header aşağıda verilmiştir:

```
Authorization: Basic c2FsYTpzYXNkYQ==
```

Base64 algoritmasının kırılması çok kolay, bu sebeple SSL ile kullanılması öneriliyor.