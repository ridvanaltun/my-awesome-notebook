# fzf

`Fuzzy Finder`. Arama işlemini kolaylaştıran bir program. Bir çok ekstra özelliği var.

# Table of Contents

- [fzf](#fzf)
- [Table of Contents](#table-of-contents)
- [Installation](#installation)
- [Key Bindings](#key-bindings)
- [Usage](#usage)
  - [Delete Multiple Files](#delete-multiple-files)
  - [Find CSS Files](#find-css-files)
  - [Navigate to a Folder](#navigate-to-a-folder)
  - [Respecting `.gitignore`](#respecting-gitignore)

# Installation

Git üstünden yüklemek mümkün.

```bash
git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf
~/.fzf/install
```

Bir çok yükleme yöntemi var ancak `WSL` üstünde yapabileceğim şey bu.

# Key Bindings

```
CTRL + R // history search, seçtiğimiz history'i konsola yapıştırır
ALT + C  // gösterdiğimiz klasöre cd komutu çalıştırır
CTRL + T // gösterdiğimiz klasörün yolunu konsole yapıştırır
```

# Usage

Bu programı kullanmanın bir çok yolu var.

## Delete Multiple Files

- Terminale tm yazılır ve boşluk bırakılır
- `CTRL + T` tuşlanır
- Tab tuşu ile silmek istediğimiz dosyalar seçilir
- Enter tuşuna basılır
- Terminalde enter tuşuna basılır ve dosyalar silinmiş olur

## Find CSS Files

- Fuzzy Finder istediğimiz bir key binding ile açılır
- .css$ yazarız, karşımızda sadece .css ile biten dosyalar listelenir

## Navigate to a Folder

Normalde bu işi `CTRL + T` ile yapabiliyoruz, ancak bunun başka bir yolu daha var.

- cd ** yazılır ve bir kez tab tuşuna basılır

NOT: ** ve tab ile istediğimiz komut için `fzf`'yi çalıştırabiliyoruz.

## Respecting `.gitignore`

Git programını kullandığımız bir projede `fzf` kullanmak istiyorsak `.gitignore` olan dosyaları gizlememiz gerekiyor. Örneğin projemizde `node_modules` klasörü varsa `fzf` kullanmak işimizi çok zorlaştırabilir. Bu sebeple fzf'nin `.gitignore` dosyasını tanıyıp ona göre çalışması gerekiyor.

Konu [orjinal dokümanda](https://github.com/junegunn/fzf#respecting-gitignore) anlatılmış.