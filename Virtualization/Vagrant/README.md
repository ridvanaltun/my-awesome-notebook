# Vagrant

Komut satırından sanal makienler açıp yönetebiliyoruz, sanal makinelerimizi yazdığımız scriptler ile yönetebiliriz mesela. İnsanların hazırladıkları sanal makineleri bilgisayarımızda bir komutla kurabilir ve çalıştırabiliriz.

Docker'dan farkı komple bir sanal makineyi kaldırıyor olmamız, Docker sadece linux desteklerken vagrant içine istediğimiz işletim sistemini koyabiliyoruz.

Docker'daki `image` mantığı burada `box`.

Box'lara bu [resmi adres](https://app.vagrantup.com/boxes/search)ten erişebiliriz.

# Table of Contents

- [Vagrant](#vagrant)
- [Table of Contents](#table-of-contents)
- [Usage](#usage)
- [Configuration](#configuration)
- [Multiple Box Definition on Vagrantfile](#multiple-box-definition-on-vagrantfile)
- [Default Provider](#default-provider)
- [Plugins](#plugins)
- [Create Box](#create-box)
- [Vagrant with HyperV](#vagrant-with-hyperv)
  - [Limitations](#limitations)

# Usage

```bash
# Bulunduğun klasöre Vagrantfile dosyası oluştur
# Vagrantfile aslında bir ruby dosyası
# Ancak ruby syntax'ini bilmemize gerek yok, sadece değişken tanımlıyoruz çünkü
vagrant init centos/7

# Box'u direkt olarak sistemimize ekledik
vagrant box add ubuntu/trusty64

# Vagrant klasörü içindeki ayarlarla box'ı ayağa kaldır
# Eğer gösterdiğimiz image bilgisayar içinde yoksa kurulur
vagrant up

# Default provider yerine kendi seçtiğim provider ile ayağa kaldır
vagrant up --provider=hyperv

# Box'larımıza bakıyoruz
vagrant box list

# Makinemizde çalışan box'ların bilgileri burada yazıyor
vagrant global-status

# SSH ile makineye bağlanıyoruz
# Girdiğimiz ID değerini `vagrant global-status` komutu ile almıştık
vagrant ssh f9cf40a

# Makinemizi yekrardan başlatıyoruz
vagrant reload f9cf40a

# Makineyi kapat
vagrant halt f9cf40a

# Suspend, Kapan ama tekrar açtığımda eski kaldığın yerden devam et
vagrant suspend f9cf40a

# Anladığım kadrıyla makineyi ilk defa açıyormuşuz gibi hale getiriyor
# Ama makine silinmedi, sistemimizde hala aynı ID ile barınıyor
vagrant destroy f9cf40a

# Kaldırdığımız box'ların volumelarını siler
# Güvenlik amaçlı datalar hep kalıyor normalde
# Bu sbeple bilgisayarımızda kullanmadığımız eski datalar birikiyor böylece.
vagrant box prune
```

# Configuration

```bash
# docker-compose'da olduğu gibi dosyanın sürümü var
Vagrant.configure("2") do |config|

# Buna base box deniyor, kısaca kullandığımız imaj
config.vm.box = "bento/ubuntu-18.04"

# Box'u ayağa kaldırdığımızda otomatik sürüm kontrolü yapar ve yükseltir
config.vm_box_check_update = false

# Box'un 80. portu makinamızın 8080. portuna yönlendirildi
config.vm.network "forwarded_port", guest: 80, host: 8080

# Sadece ana makinemiz box'a erişebilir hale gelir, uzaktan insanlar erişemez
config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

# İnsanlar dışarıdan makineye erişebilirler
config.vm.network "public_network"

# Private ağ içinde makine ip adresi alır, yani privite ağ içinde erişilebilir olur
config.vm.network "private_network", ip: "192.168.33.10"
config.vm.network "private_network", type: dhcp

# Local makine ile box'arasında paylaşımlı dosya
config.vm.synced_folder "../data", "/vagrant_data"

# Box ilk defa çalıştırıldığında çalıştırılacak komutlar
config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install -y apache2
SHELL

# Ayağa hangi sanal makine ile kaldıracağız? WM, VirtualBox, HyperV
# Sonrasında makinenin fiziksel özellikleri ne olacak? Ram vs.
# Birde provider related ayarlar, sanal makine açılırken ekran açılsın mı mesela.
config.vm.provider "virtualbox" do |vb|
   # Display the VirtualBox GUI when booting the machine
   vb.gui = true

   # Customize the amount of memory on the VM:
   vb.memory = "1024"
 end
```

# Multiple Box Definition on Vagrantfile

Birden fazla box'u tek Vagrantfile'a tanımlanabilir.

// TODO

# Default Provider

`vagrant up` komutunu çalıştırırken ger zaman `--provider` parametresi vermemize gerek yok. Sistemimizde `VAGRANT_DEFAULT_PROVIDER` ortam değişkeni tanımlayarak default provider'ı seçmek mümkün.

# Plugins

// TODO

# Create Box

// TODO

# Vagrant with HyperV

Dokümanında yazana göre HyperV için ayrı bir yöntemle box oluşturulabiliyor.

## Limitations

Limitasyon için [resmi doküman](https://www.vagrantup.com/docs/hyperv/limitations.html)a bakılabilir.

Vagrant HyperV ile çalışabiliyor ancak bazı özellikleri çalışmıyor bu durumda. Vagranfile içinde yazptığımız network ayarları (static ip adresi atamak vs) hyperV ile yapamayız, otomatik olarak ignore edilir.

`vagrant snapshot restore` ve `vagrant snapshot pop` komutları bazı zamanlar hata veriyormuş.
