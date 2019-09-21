# HTTPie

`cURL`'ün alternatifi. Çıktıları renkli veriyor vs. JSON formatını tanıyor ve ekrana bu şekilde getiriyor.

```bash
# Basit bir GET isteği
http http://google.com

# Put isteği aıyoruz
# foo=bar kısmı data kısmımız
# Biz aksini belirtmedikçe datamız json formatına çevrilip atılır
# -v parametresi --verbose anlamına geliyor, request ve response'ların başlık bilgilerinide gösteriyor
http -v PUT http://google.com foo=bar

# Özel bir header ile get isteği attık
# Header'a X-foo:bar şeklinde bir veri eklendi
http http://google.com X-foo:bar

# Dosya indiriyoruz
http --download http://foo.com/bar.zip

# Dosyayı belirlediğimiz bir yere indiriyoruz
http --download --output ~/downloads http://foo.com/bar.zip

# Redirect'leri takip et
http --follow http://google.com

# Gelen response a göre bir çıkış kodu üretir
# Normalde işlem başarılı olduğu için  üretir
# Google bize 404 döndürseydi mesela burada 4 çıktısını üretecekti
# 5 ile başlayan bir hata döndürseydi 5 çıkış kodunu üretecekti
# Bu şekilde kolayca hata tespiti yapabiliriz
http --check-status http://google.com

# Sertifika kontrolünü kapar
# Geliştirme yaparken gerekli bir özellik
http --verify=no http://google.com
```

# Session

```bash
# Create Session
http --session=/tmp/session.json example.org API-Token:123

# Use Session
http --session=/tmp/session.json example.org

# Create Named Session
# Bu şekilde verdiğimiz isim ile session'ı çağırabilicez
http --session=user1 -a user1:password example.org X-Foo:Bar

# Use Named Session
http --session=user1 example.org
```