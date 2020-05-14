# Create NPM Package

```bash
# klasör oluşturup içine girdik
$ mkdir deneme-projesi

# npm projesi oluşturuyoruz
# bize projemiz hakkında sorular soruyor
# en sonunda çıktı olarak package.json veriyor
$ npm init

# projemizin main dosyasını oluşturuyoruz
$ touch index.js
```

Örnek bir `index.js` dosyası aşağıda verilmiştir.

```js
module.exports.area = radius => Math.PI * radius * radius;

module.exports.perimeter = radius => 2 * Math.PI * radius;
```

# Testing on Local

```bash
# öncelikle paketimizi lokal ortamda linkliyoruz
$ npm link

# artık yolunu gösterek lokalde paketi kullanabiliriz
$ npm install /absolute/local/path/to/your/other/package

# işimiz bitince paketimizi linkini kaldırabiliriz
$ npm unlink
```

Linkleme işlemi yapılan kütüphane için değişiklik yapınca sil-yeniden yükle işlemi yapmamıza geerk yok. Kütüphane linklendiği için yapılan tüm değişiklikler yüklenen projede geçerli oluyor.

# Register to NPM Registry

```bash
# npm programı ile npm üyeliği oluşturabiliriz
# veyahut npmjs.com sitesi üstünden üyelik açabiliriz
$ npm adduser

# npm hesabımıza bulunduğumuz bilgisayar üstünden giriş yapıyoruz
$ npm login

# hangi kullanıcı ile giriş yaptığımızı görebiliriz
$ npm whoami
```

# Publish Package

```bash
# projenin kök klasöründe bu komutu girdiğimizde projemize bağlı paketlerin versiyonları dönecektir
$ npm version

# projemizizn versiyon numarası bu şekilde değiştirilebilir
$ npm version 1.0.1

# projemizi yayınlıyoruz
# dikkat etmemiz gereken tek şey proje adı uniq olmak zorunda
# yeni versiyon çıktığımızda da publish ederek release çıkabiliriz
$ npm publish
```

# Use Package

```bash
# ilk olarak paketimizi projemize yüklüyoruz
$ npm install deneme-projesi --save
```

```js
import {area, perimeter} from 'deneme-projesi';

console.log(area(5));

console.log(perimeter(5));
```

# Privite Registry Usage

Şirketin özel npm registry'si olabilir, bu registry içine paket publish edebiliriz. npm programı ile tek bir registry kullanabişliyoruz, başka bir registry'e geçmek için her seferinde komut yazmak gerekiyor.

```bash
# özel bir registry'e geçtik
$ npm config set registry https://registry.your-registry.npme.io/
```

Birden fazla registry'i aynı anda kullanmak istiyorsak `npmrc` adında başka bir prgoramı kullanabiliriz.

```bash
# npmrc'yi yükle
$ npm i npmrc -g

# work adında bir profile oluşturduk, -c -> create
$ npmrc -c work

# yukarıdaki komutun hemen ardından bu komutu girince profile bu komuttaki registry kayıt oluyor
$ npm config set registry https://registry.your-company-registry.npme.io/

# open-source adında başka bir profile açtık
$ npmrc -c open-source

# global npm registry'sini open-source adlı profile kaydettik
$ npm config set registry https://registry.npmjs.com

# work adlı profili kullan dedik
$ npmrc work
```

# Create CLI (Command Line Interface)

`package.json` dosyamız içine aşağıdaki gibi bir key ekleyebiliriz:

```json
{
    "bin": {
        "say-hello": "./cli.js"
    }
}
```

Kullandığımız komut adının uniq olmasına özen göstermeliyiz, komutumuz başka bir komut ile çakıştığında çalıştırılmayacaktır.

npm paketi global olarak yüklendikten sonra say-hello çağrıldığı zaman `cli.js` dosyası çalıştırılır.
Aşağıda örnek bir cli.js dosyası gösterilmiştir.

```js
#!/usr/bin/env node

// Grab provided args.
const [,, ...args] = process.argv;

// Print hello world provided args.
console.log(`Hello world ${args}`);
```

Aşağıda `cli.js` dosyasının kullanımı ve çıktısı gösterilmiştir.

```bash
$ ./cli.js

# OUTPUT: Hello World

$ ./cli.js 1

# OUTPUT: Hello World 1

$ ./cli.js 1 2 3

# OUTPUT: Hello World 1,2,3
```

## Test CLI Package

```bash
# projemiz içinde aşağıdaki komutu girdikten sonra CLI aracımız local'de global olarak kullanılabiliyor
$ npm link

# işimiz bitince global'den kaldırıyoruz
$ npm unlink
```

## Comman Line Helper Packages

CLI aracımız ile daha kolay bir şekilde gelen parametreleri yönetmek için bazı nodejs paketleri mevcut.

### yargs

[Offical Repo](https://github.com/yargs/yargs)

Kullanımı kolay bir paket.

# Badges

Bu [linkte](https://github.com/dwyl/repo-badges) güzel bir şekilde badge'ler tanıtılmış.