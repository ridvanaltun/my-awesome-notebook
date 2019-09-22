# Rmate

Uzak sunuculardaki dosyaları `VSCode`, `Sublime` veya `Atom` editörü ile editleyebilmek için kullanabileceğimiz bir program.

Ben `VSCode` için [Remote VSCode](https://marketplace.visualstudio.com/items?itemName=rafaelmaiolla.remote-vscode) adında bir eklenti kullanıyorum.

Bu programı kullanmak için uzakta ki makineye [rmate](https://github.com/textmate/rmate) programını kurmamız gerekiyor.

`rmate` ile sadece dosya açılabildiği söyleniyor, direkt olarak bir projeyi açamıyoruz.

# Usage

- Editörümüz üstünden remote server başlatıyoruz, kurduğumuz eklentiye ait bir komut ile başlatılabilir.
- Terminal üstünden `ssh -R 52698:localhost:52698 VIRTUAL_MACHINE_IP_ADDRESS` komutu ile bağlandık uzak bilgisayara.
- `52698` portu VSCode Remote eklentisi için default port, bu sebeple bu portu kullandık. Bu ayar değiştirilebilir.
- Örnek olarak `rmate demo.py` şeklinde uzak bilgisayarda komut çalıştırdığımızda editörümüz otomatik açılıyor.