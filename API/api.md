# API

API hakkında notlarım.

# Open API Specs

@TODO

# Rest Apı

[Rest API Tasarım Rehberi](https://medium.com/kodgemisi/rest-api-tasarim-rehberi-2f004a87426d)

# Fundamentals

## API Versioning

Geriye uyumluluk amaçlı API'ımızda versiyon sistemi kullanmamız gerekiyor. Eğer bunu yapmazsak API değiştiğinde programı veya servisi kullanan herkes sistemini güncellemek zorunda kalır.

URL içinde versiyon kullanmamalıyız. Client tarafında her request için API'ın version numarasını vermeliyiz. Sunucu tarafında header'a bakıp duruma göre `redirect` ile özel response'lar üretilmeli. Bu şekilde kolayca geriye uyumluluk sağlayabiliriz.

## HTTP Request

`CRUD` yapısı, `Create`, `Read`, `Update` ve `Delete`. Sırasıyla HTTP protokolünde bu yapını karşılığı; POST, GET, PUT, DELETE

## HTTP Response

https://http.cat/

- 1xx: Information
- 2xx: Success
- 3xx: Redirection
- 4xx: Client Error
- 5xx: Server Error

```
GET    200 (OK)
POST   201 (Created)
PUT    200 (OK)
DELETE 200 (OK), 202 (Accepted), or 204 (No Content)
```

## Endpoint

API'a istek atarken kullandığımız URL'e deniyor.

Endpoint 3 aşamadan oluşur, bunlar; `root`, `path`, `query params`

`Query params` kısmı geliştricinin tercihine bağlı.

Örnek: `https://api.example.com/users?limit=10`

# RESTful Best Practices

- `endpoint` dosya uzantısı gösterilmez
- Çoğul isimler kullanılmalı, kullanıcı bilgisi çekeceksek `/user/5` değil `/users/5` olmalı
- `endpoint` versiyon bilgisi içermemeli, `redirect` ile header'da bulunan versiyon kullanılmalı
- Alt çizgi kullanılmamalı. Yerine düz çizgi kullanılmalı
- Tüm harfler küçük olmalı
- `endpoint` içinde fiil kullanılmamalı

# RESTful Example

```bash
# Tüm kullanıcıları döndürür
GET /users

# ID'si 5 olan kullanıcıyı döndürür
GET /users/5

# Kullanıcı kaydı yaptırır
POST /users

# Method not allowed (405)
POST /users/5

# Kullanıcıları toplu günceller
PUT /users

# Belirli bir kullanıcıyı güncelleriz
PUT /users/5

# Tüm kullanıcıları siler
DELETE /users

# Belirli bir kullanıcıyı siler
DELETE /users/5
```

# API Security

Kullanıcı 10 dakika içinde sadece 100 kez istek atabilsin gibi sınırlamalar koymak.

// TODO
