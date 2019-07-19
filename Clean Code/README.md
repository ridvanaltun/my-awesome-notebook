# Clean Code Guideline

Yazdığımız program hangi dilde olursa olsun diğer insanların kolayca anlayabileceği bir şekilde yazmamız gerekiyor. Eğer kodun güzel görünmesine dikkat etmezsek ileride kodumuzu güncellemek istediğimizde yazdığımız kodu anlamakta güçlük çekeriz. Projeye dahil olan diğer insanlar yazdığımız kodu estetik hatalar yüzünden zor anlayabilir veya hiç anlamayabilirler.

Bu dokümanda geliştiricilerin proje standartında temiz kod yazabilmesi için yapması gerekenler yer alıyor. Neden böyle bir dokümana ihtiyaç duyuyoruz? Yazılan kodun tek elden çıkmış gibi gözükmesi ve ileride kodu değiştirmek istediğimizde maliyeti düşürmek için projede `Clean Code` standartları kullanıyoruz.

Kısaca amacımız okunabilirliği yüksek kod yazmak. 

Bunun dışında dokümanda, projede kullanılan `Clean Code` yardımcısı programların nasıl konfigüre edileceği ve genel olarak `Clean Code` konsepti anlatılıyor. 

# Table of Contents

<!-- toc -->

- [Some Rules for Writing Beautiful Code](#some-rules-for-writing-beautiful-code)
- [Codding Standarts](#codding-standarts)
- [Concepts](#concepts)
  * [Dotfiles](#dotfiles)
  * [Linter](#linter)
  * [Prettier](#prettier)
  * [Git Hooks](#git-hooks)
  * [Node Scripts](#node-scripts)
- [Tools](#tools)
  * [EditorConfig](#editorconfig)
  * [EsLint](#eslint)
    + [Overwrite Rules](#overwrite-rules)
  * [Prettier](#prettier-1)
  * [Husky](#husky)
  * [Lint-Staged](#lint-staged)
- [Setting Up EsLint and Prettier for React and React-Native Projects](#setting-up-eslint-and-prettier-for-react-and-react-native-projects)
  * [Prettier-Editor Integration](#prettier-editor-integration)
    + [VSCode](#vscode)
    + [Sublime Text 3](#sublime-text-3)
- [Git Related](#git-related)
  * [GitIgnore](#gitignore)
  * [GitAttributes](#gitattributes)

<!-- tocstop -->

# Some Rules for Writing Beautiful Code

Bu başlığın amacı sadece temiz kod yazmak için programcıların `best practice` olarak uyguladığı bazı standartlar hakkında bilgi vermek.

- Satır sayısı maksimum 80 karakter olmalı.
- Fonksiyonlar sadece bir iş yapmalı ve en fazla 24x2 satır sürmeli.
- Oluşturulmuş her fonksiyon yorum ile süslenmeli.
- Programımızın en başında dosya hakkında bilgi veren bir `Header Comment` olmalı.
- TAB yerine boşluk tuşunu kullanmalıyız. Bunun sebebi projemizle ilgilenen geliştirici kişilerin vs. editör ayarlarında TAB tuşuna farklı bir boşluk miktarı tanımlanmış olabilir ve yazdığımız kod başka editörlerde okunamaz boyutta kötü gösteriyor olabilir.
- Veri tipi belirtilen dillerde `Macar Notasyonu` kullanılmamalı.
- İç içe girinti en fazla 3 seviye olmalı.

ANSI konsol ekranı 80x24 satır oluyor. Listedeki ilk iki kural buradan geliyor.

# Codding Standarts

Temiz kod yazmak için dilden dile değişen bir kod yazma standartı bulunuyor. Örneğin python ile kod yazarken `PEP8` adında bir standart ile kod yazabiliriz.

# Concepts

Bu başlık altında bazı konsept terimlerden bahsediyoruz.

## Dotfiles

Programcı alışkanlığı ve geçmişten gelen bir şey olarak ayar dosyaları başında nokta olan dosyalarda tutulur. 

## Linter

Yazdığımız kodun belirlediğimiz kurallara uygun olup olmadığını kontrol eden ve bize bildiren programlara `linter` diyoruz. Hemen hemen her şeye koral belirlememiz mümkün.

## Prettier

Prettier amacı adından da anlaşılacağı üzere bizim belirlediğimiz kurallara uyarak otomatik kodu değiştiren sistemlare deniyor. Geliştirici kodu kaydettiğinde yada programa `input` olarak dosya veya dosyaları verdiğimizde çalışır bu program. Editörümüz üstünde yaptığımız ayara göre nasıl çalıştığı değişebilir.

Bu programlar genelde `linter`'a destek amaçlı kullanılıyor çünkü `linter` ile koyduğumuz kuralları koda `prettier` ile otomatik uygulatabiliyoruz.

## Git Hooks

Kodumuzu git sunucusna yüklemeden önce veya git ile bir işlem yapamdan önce işlemin gerçekleşebilmesi için şart koşabiliyoruz. Örneğin commit öncesi eğer EsLint yazdığımız kodda kural dışı bir durum olduğunu belirtirse `commit` işlemi yapılmaz ve EsLint'in verdiği hatayı çöz diye bize bildirebilir.

`Git hooks` ile bir çok şey yapılabilir, biz temiz kod yazmak için bu yapıyı kullanıyoruz.

## Node Scripts

Projemizin `package.json` dosyasına scriptler tanımlayabilir ve projemize yüklediğimiz test yazılımlarını bu scriptler ile kombine bir şekilde çağırabiliriz. Tek yapmamız gereken `npm run script_name` şeklinde komutu çağırmak.

# Tools

Projede `Clean Code` için kullanabileceğimiz araçlar. 

## EditorConfig

Text editörüne eklenmesi gereken bir eklenti bu. Genel amacı text editörleri arasındaki farkları kapamak ve farklı editörlerde ve editör ayarında geliştirme yapan insanalr arasında oluşan bazı hataları gidermek. Projenin kök klasöründe bulunan `.editorconfig` dosyasını bu program kullanıyor. Dosya içinde editörü hangi ayarlar ile kullanacağımızı yazıyoruz. 

Örneğin tab karekterini 2 space karekterine çevirmek.

## EsLint

JavaScript, React ve React-Native için tercih edilen en popüler linter bu. Bu linter ile bazı popüler şirketlerde (Google, Airbnb vs.) yazılan kodlar ile benzer şekilde kodlar yazabiliyoruz çünkü bu şirketler internette EsLint config dosyasını açık bir şekilde paylaşıyorlar.

### Overwrite Rules

Eğer ayar import ettiysek ama özel bir ayar yapmak istiyorsak `.eslintrc` dosyası içinde yeni ayarlar tanılayıp eski ayarların üstüne yazabiliriz. Örneğin Airbnb kuralları yükleyip üstüne değiştirmek istediğimiz kuralları belirtebiliriz.

## Prettier

Prettier bir konsept olması yanı sıra kendi başına çalışabilen popüler bir araç. En büyük artısı EsLint ile ortak çalışabilmesi. Sublime Text editöründe `JsPrettier` adıyla geçiyor.

`.prettierignore` dosyasında prettier işleminde geçmeyecek dosya ve klasörleri belirtebiliyoruz.

`.prettierrc` dosyasında programa kurallar ve genel bazı ayarları belirtebiliyoruz.

## Husky

`Git hooks` özelliği `.git` klasörü içinde tutulduğu için insanlar arasında paylaşılması zor. Bunu kolaylaştırmak için `husky` adında bir araç kullanıyoruz.

`package.json` dosyamıza 

```javascript
"husky": {
    "hooks": {
      "pre-commit": "lint-staged"
    }
}
```

Şeklinde commit öncesi çalışması istediğimiz komutu belirtebiliriz. `lint-staged` aslında bir program, projemizin içinde yüklü olduğu için erişebiliyoruz.

## Lint-Staged

`Husky` aracı ile beraber kullanıyoruz. Stage olmuş dosyalarımızı bu araç ile belirlediğimiz bir komut veya komut zinciri ile işleyebiliyoruz.

`.lintstagedrc` dosyasına hangi uzantıya sahip dosya hangi komuttan geçsin belirtebiliyoruz. İşin güzel tarafı sadece staged içindeki dosyalar bu işlemlerden geçiyor.

```json
// .lintstagedrc örneği
{
  "*.{js, jsx}": ["eslint --fix", "git add"]
}
```

# Setting Up EsLint and Prettier for React and React-Native Projects

```bash
# eslint ve eslint-babel uyumu için ekstra bir paket yüklüyoruz
npm install --save-dev eslint babel-eslint

# EsLint JSX uyumu ve Airbnb standartı için yüklediğimiz paketler
npm install --save-dev eslint-config-airbnb eslint-plugin-jsx-a11y eslint-plugin-react eslint-plugin-import

# Prettier ve prettier-eslint birlikte çalışması için gerekli paketleri yükle
npm install --save-dev prettier eslint-config-prettier eslint-plugin-prettier
```

`.eslintrc` dosyamız aşağıdaki gibi olmalı.

```json
{
  "parser": "babel-eslint",
  "extends": ["airbnb", "prettier", "prettier/react"],
  "plugins": [
    "react",
    "jsx-a11y",
    "import",
    "eslint-plugin-prettier",
    "eslint-plugin-react"
  ],
  "rules": {
    "import/no-extraneous-dependencies": ["error", { "devDependencies": true }],
    "import/prefer-default-export": "off",
    "react/jsx-filename-extension": [1, { "extensions": [".js", ".jsx"] }],
    "react/prefer-stateless-function": [0],
    "react/jsx-indent": [0],
    "react/sort-comp": [0],
    "react/destructuring-assignment": [0],
    "react/forbid-prop-types": [0],
    "react/no-unescaped-entities": ["error", { "forbid": [">", "}"] }],
    "quotes": [
      "error",
      "single",
      { "avoidEscape": true, "allowTemplateLiterals": false }
    ],
    "jsx-quotes": ["error", "prefer-double"],
    "camelcase": "off",
    "no-use-before-define": "off",
    "semi": ["error", "always"],
    "prettier/prettier": [
      "error",
      {
        "trailingComma": "none",
        "singleQuote": true,
        "jsxSingleQuote": false,
        "printWidth": 100,
        "semi": true,
        "jsxBracketSameLine": true
      }
    ]
  },
  "env": {
    "jest": true,
    "es6": true
  }
}
```

Görüldüğü üzere Airbnb ayarları üstüne bir ton ayar yazılmış ve bunun üstüne prettier ayarları `.eslintrc` dosyasına yazılmış.

Dokümanın orijinal kaynağına [Set up ESLint and Prettier
](https://medium.com/fullstack-with-react-native-aws-serverless-and/set-up-eslint-and-prettier-5e4131f9296f) adresinden erişebilirsiniz.

## Prettier-Editor Integration

Otomatik olarak kod düzeltmesi için prettier-editör entegrasyonu yapmamız gerekiyor aksi halde her dosyaya komut yada plugin'in sunduğu arayüz ile düzeltme yapmamız gerekecekti.

Orjinal dokümana [buradan](https://prettier.io/docs/en/editors.html?source=post_page) erişebilirsiniz.

### VSCode

```json
{
  // add below to VSCode User settings to auto fix eslint formatting issues on file save
  "editor.formatOnSave": true,
  "eslint.autoFixOnSave": true,
  "eslint.alwaysShowStatus": true
}
```

### Sublime Text 3

`Sublime Text 3` için nedense `JsPrettier` eklentisi dosya kaydedildiği anda otomatik çalışmıyor. Bu sebeple `Sublime Text 3` için bir ayar öneremiyorum.

Prettier ile kodu düzelttirmek yerine `EsLint Formatter` eklentisinin auto fix özelliği ile bu işi halletmek mümkün. Eklentinin `Key Bindings - User` sayfasını açıp aşağıdaki tanımı eklediğimiz zaman `CTRL+S` sonrası otomatik kod düzeltilir.

```
[
    {
        "keys": ["ctrl+s"],
        "command": "format_eslint"
    }
]
```

# Git Related

Git deposunu temiz tutmak için yapılabilecek bazı ayarlar.

## GitIgnore

Proje için yayınlanmaması gereken kritik dosyaları ve build sonrası oluşan dosyalar burada tanımlanıyor. Git aracı ile çalışırken bu dosya sayesinde belirttiğimiz dosya ve klasörleri git yapımızdan soyutlamış oluyoruz.

## GitAttributes

Sık kullandığım bir özellik değil. Detaylı bilgi için: [Be a Git ninja: the .gitattributes file](https://medium.com/@pablorsk/be-a-git-ninja-the-gitattributes-file-e58c07c9e915)
