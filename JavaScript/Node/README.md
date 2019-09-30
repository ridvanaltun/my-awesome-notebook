# Node

Node bir runtime environment. JavaScript dilini kullanıyor.

# Table of Contents

- [Node](#node)
- [Table of Contents](#table-of-contents)
- [Installation](#installation)
- [Basics](#basics)
  - [Interpreter](#interpreter)
  - [Automatically Refresh/Reload](#automatically-refreshreload)
  - [Environment Variables](#environment-variables)
- [Advanced](#advanced)
  - [Schedule Cron Jobs](#schedule-cron-jobs)
    - [Setting Time](#setting-time)
  - [Type Checking](#type-checking)
- [NPM](#npm)

# Installation

Node sürümlerini yönetmek için `nvm (Node Version Manager)` adlı programı kullanabiliriz. Bu program Ruby'deki `rvm` ve Python'daki `pyenv` gibi bir işlevi var. `nvm` ile birden fazla node sürümü ile çalışabiliriz.

```bash
# Son çıkan LTS (Long Term Support) sürümü kurar
$ nvm install --lts

# Son çıkan sürümü yükler (LTS olmayabilir)
$ nvm install node

# Belirtilen sürümü yükler
$ nvm install 7.0.0

# Belirtilen sürümü yükler ve global paketleri eğer uygunsa bu sürümün desteğine göre günceller
$ nvm install 6.9.0 --reinstall-packages-from=node

# Yüklü başka bir sürüme geçer
$ nvm use 7.4.0
```

# Basics

```bash
# Etkileşimli kabuğu açar
$ node

# script.js dosyasını node ile çalıştır
$ node script.js

# script.js dosyasını node ile debug modunda çalıştır
$ node debug script.js

# Lokalde yüklü node paketlerini gösterir
$ nvm ls

# Yüklü tüm node sürümlerini gösterir
$ nvm ls-remote
```

## Interpreter

```bash
$ node # start a Node REPL
> console.log('Hey node!');
Hey node!
undefined
> .exit //Quit
```

## Automatically Refresh/Reload

```bash
# Bu işi yapan programı global olarak yüklüyoruz
$ npm install nodemon -g

# server.js dosyası değiştiiğinde otomatik olarak yeniden yükle
$ nodemon server.js
```

## Environment Variables

Aşağıda ötnek bir `.env` dosyası bulunuyor:

```env
PORT=65534

DB_CONN="mongodb://react-cosmos-db:swQOhAsVjfHx3Q9VXh29T9U8xQNVGQ78lEQaL6yMNq3rOSA1WhUXHTOcmDf38Q8rg14NHtQLcUuMA==@react-cosmos-db.documents.azure.com:10255/?ssl=true&replicaSet=globaldb"

SECRET_KEY="b6264fca-8adf-457f-a94f-5a4b0d1ca2b9"
```

JS içinde bu dosyayı aşağıdaki gösterildiği şekildeki gibi kullanabiliriz:

```js
// Use dotenv to read .env vars into Node
require('dotenv').config();
var MongoClient = require('mongodb').MongoClient;

// Reference .env vars off of the process.env object
MongoClient.connect(process.env.DB_CONN, function(err, db) {
  if(!err) {
    console.log("We are connected");
  }
});
```

Tabi bunun için `dotenv` adındaki pakete ihtiyacımız var, şu keilde kurabiliriz:

```bash
$ npm install dotenv --save
```

# Advanced

## Schedule Cron Jobs

```bash
# Bu işi yapan paketi yüklüyoruz
$ npm install node-cron --save
```

```js
// Paketi tanımladık
const cron = require(“node-cron”);

// Her dakika "Running Cron Job" şeklinde konsolda yazı yazacak
cron.schedule(“* * * * *”, function () {
    console.log(“Running Cron Job”);
});
```

### Setting Time

```
* (Second) (Optional) - * (Minute) - * (Hour) - * (Day of Month) - * (Month) - * (Day of Week)
```

Aşağıda verilmiş örnekler incelenebilir:

```
* * 5 * *  -> Her ayın 5. günü
30 7 * * * -> Her günün 7. saatinin 30. dakikası
```

## Type Checking

Bu iş için `flow` adına `type checking` yapan bir aracı kullanıyoruz.

```bash
# Global olarak gerekli programı yüklüyoruz
$ npm install flow-bin -g

# .flowconfig dosyası oluşturur
$ flow init
```

Bunun çalışması için dosyaların başına `//@flow` şeklinde yazmamız gerekiyor.

Normalde `flow`'u şu şekilde kullanıyoruz:

```js
//@flow
const c /*: number*/ = 'Not a number';
```

Babel programına ayar yaparsak eğer JS içinde yorum şeklinde tip belirtmemize gerek kalmaz. Bunun için projemize bazı paketler yüklememiz gerekiyor.

```bash
$ npm install babel-plugin-transform-flow-strip-types --save-dev
```

Ardından `.babelrc` dosyamızı aşağıdaki gibi düzenlememiz gerekiyor:

```json
{
  "presets": ["env"],
  "plugins": ["transform-flow-strip-types"]
}
```

Artık aşağıdaki gibi kod yazabiliriz:

```js
// @flow
function f(name: string): string {
    return `Hello ${name}`;
}

console.log(f('Bob'));    // Types match - all Good
console.log(f(5));        // Will generate Flow error
```

3. parti kütüphaneler için tip kontrolü yaptırmamız mümkün, bu iş için aşağıda gösterildiği şekilde bir programı geliştirme ortamımıza kurmamız gerekiyor:

```bash
$ npm install flow-typed -g
```

Bu program her kütüphane için çalışmaz, kütüphanenin tipleri program içinde tanımlanmışsa çalışır. Artık otomatik olarak 3. parti kütühanelerin tiplerine göre program geliştirebiliriz.

Editor/IDE'miz için `flow` eklentisi kurabilir ve geliştirme ortamımızı daha iyi bir hale getirebiliriz.

# NPM

```bash
# Belirtilen paktin son sürümünü yükler
# Bunu package.json'a kaydeder
$ npm install request --save

# Belirtilen paketi yükler
# Bunu package.json'a kaydeder
$ npm install request@2.60.0 --save

# Belirtilen paketi yükler
# Bunu package.json da dev bölümüne kaydeder
# Production için gerekli olmayan paketler buraya yüklenir
$ npm install babel-cli --save-dev

# Global olarak belirtilen paketi yükler
$ npm install nodemon -g

# package.json içindeki tüm paketleri yükler
$ npm install
```
