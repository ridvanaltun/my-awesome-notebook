# cURL

- -X, --request - Custom request method
- -d, --data - Sends the specified data
- -H, --header - Sends headers
- -i, --include - Display response headers
- -o // output, çıktı olarak alıyoruz cURL ile indirdiğimiz linki

Tarayıcıdan basitçe GET isteği atabiliriz ancak `POST`, `DELETE`, `PUT`, `PATCH` gibi diğer HTTP isteklerini yapabilmek için cURL aracını kullanabiliriz.

# Table of Contents

- [cURL](#curl)
- [Table of Contents](#table-of-contents)
- [Usage](#usage)
- [Header](#header)
- [Session](#session)
  - [Save Cookie](#save-cookie)
  - [Use Cookie](#use-cookie)

# Usage

```bash
# GET
curl https://jsonplaceholder.typicode.com/posts

# POST
curl -X POST -H "Content-Type: application/json" -d '{"userId": 5, "title": "Stuff and Things", "body": "An amazing blog post about both stuff and things."}' https://jsonplaceholder.typicode.com/posts

# PUT
curl -X PUT -H "Content-Type: application/json" -d '{"userId": 1, "title": "Something else", "body": "A new body"}' https://jsonplaceholder.typicode.com/posts/1

# PATCH
curl -X PATCH -H "Content-Type: application/json" -d '{"title": "Only change the title"}' https://jsonplaceholder.typicode.com/posts/1

# DELETE
curl -X DELETE https://jsonplaceholder.typicode.com/posts/1
```

# Header

Aşağıdaki örnekte olduğu gibi header kısmına `-H` parametresi ile istediğimiz kadar parametre yüklüyoruz.

```bash
curl \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer <JWT_TOKEN>" \
  -H "x-jwt-assertion: <JWT_TOKEN>" \
  -X POST \
  -d  '{"key1" : "value1", "key2" : "value2"}' \
  https://example.com/
```

# Session

`cURL` aracı ile session'ı kaydetmek, cookie saklamak mümkün.

## Save Cookie

`--cookie-jar` argümanı ile request attığımız adres ile iletişimimiz bitince oluşturulmuş olan cookie gösterdiğimiz dosyaya yazılır. Kısaca `-c` argümanı kullanabiliriz.

`curl --cookie-jar cookies.txt -s -L --data "secret=secret_password"`

## Use Cookie

Kaydettiğimiz cookie'yi kullanmak için ise `--cookie` adlı argümanı kullanırız. `--cookie` yerine kısaca `-b` kullanabiliriz.

`curl --cookie cookies.txt --cookie-jar cookies.txt -s -L url/films`