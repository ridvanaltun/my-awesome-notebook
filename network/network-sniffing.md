# Network Sniffing

Bu başlık altında ağ dinlemeyeyi irdeliyor olacağım.

# Wireshark

En alt düzeye kadar tüm ağ iletişimi ve diğer tüm iletişimleri vs bu program ile izleyebiliyoruz. `HTTPS` bağlantılar kolaylıkla izlenemiyor. Belirli ayarları yaptıkran sonra wireshark'ı develepment için uyumlu hale getirmek mümkün.

## Sniffing SSL/TLS

Windows üstünde `SSLKEYLOGFILE` adında bir ortam değişkeni açıp istemiz adda ve uzantıda bir dosyayı işaret ediyoruz. Örneğin `sslkey.log` olabilir.

`Chrome` ve `Firefox` otomatik olarak değişken ile gösterdiğimiz dosyaya gerekli olan data'yu yazar.

Sonrasında `Wireshark` üstünden `Edit > Preferences > Protocols > TLS > [Pre]-Master Secret Log Filename` ile dosyamızı seçiyoruz.

Bu kadar basit. Artık `SSL` bir isteği şifresiz şekilde `Wireshark` üstünden inceleyebiliriz.

### Does it work for all SSL/TLS communications

Hayır. Bu method aşağıdaki yöntemi kullanan bağlantıların `decryte` edemez:

- Diffie Hellmann (DHE) ciphers
- New TLS 1.3 protocol

Bunun için bir `workaround` yok. Protokolün yapısı gereği yapabileceklerimiz kısıtlı.

# Charles Proxy, Fiddler and Burp Suite

Bu programların hepsi aynı amacı yerine getiriyor. En popüleri `Fiddler`. Bu yazılımlar `wireshark`'tan farklı çalışıyor. Genel amaçları `HTTP` ve `HTTPS` bağlantılarını dinlemek için buna özel geliştirilmişler. `Proxy` mantığı ile çalışıyorlar, biz web isteği yaptığımızda bu programa yapıyoruz aslında, program bizim adımıza gerçekten isteği yapıyor ve cevabı alıp bize sunuyor. Bu sebeple ekstra bir konfigürasyon olmadan şifreli trafiğimizi izleyebiliyoruz. Bu nedenle `development` için bu programlar tercih ediliyor.

Bu programların kullandığı yönteme `Web Debugging Proxy` veya `HTTP Proxy` deniyor.

Eskiden programlar'ın içinde proxy ayarları olurdu, artık proxy ayarları işletim sistemi seviyesinde yapılabiliyor. İşletim sistemi seviyesinde ayar yapabildiğimiz için artık tüm programalr kolaylıkla istediğimiz adres üstünden iletişim kurup konuşabiliyor.

## Fiddler

Açar açmaz bilgisayardaki proxy ayarını otomatik yapıyor ve direkt kullanmaya başlıyabiliyoruz. `FiddleScript` adında betik dili var. Gelen response'ları manipüle edip kullanıcıya sunabiliyoruz bu sayede.

`Enterprise` olarak `FiddlerCore` adında bir ürünleri var, bu ürün ile üçüncü parti uygulamalar kullanabiliyoruz ve gönderilen ve alınan paketler üstünde istediğimiz işlemleri gerçekleştirebiliyoruz.

### Listening Phone Network

[Telefonun trafiğini fiddler üstünden dinlemek](https://medium.com/@gokhansengun/a%C4%9F-trafi%C4%9Fi-dinleme-ara%C3%A7lar%C4%B1-3-27f2a1fc634c)