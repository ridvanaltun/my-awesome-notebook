# Create PIP Package

[Bu kaynak](https://python-packaging.readthedocs.io/en/latest/index.html) bu olayı iyi anlatıyor.

Yazacağımız pip paketi 3 farklı şekilde olabilir:

1. CLI
2. Modul
3. CLI ve Modul

Python'da yazdığımız kodun bir modül olabilmesi için kodun bulunduğu dosyanın içinde `__init__.py` adında boş bir dosya olması gerekiyor.
Projemizin kök klasörü en basit haliyle aşağıdaki gibi olabilir:

```
- setup.py
- .gitignore
- README.md
- LICENSE
- package_name
    - __init__.py
    - module_name.py
    - another_module_name.py
```

Böyle bir projede modülümüzü `from package_name import module_name` yazarak çağırırız. Modul içindeki bir fonksiyon yada class'ı `modul_name.ClassOrFunctionName()` şeklinde kullanabiliriz.

`setup.py` dosyası kesinlikle olması gerekiyor. Aşağıda örnek bir setup.py dosyası var.

```python
from setuptools import setup

setup(
    name='hellostackoverflow',
    version='0.0.1',
    description='a pip-installable package example',
    license='MIT',
    packages=['hellostackoverflow'],
    author='Benjamin Gerfelder',
    author_email='benjamin.gerfelder@gmail.com',
    keywords=['example'],
    url='https://github.com/bgse/hellostackoverflow'
)
```

`.gitignore` dosyamız aşağıdaki gibi olabilir:

```
# Compiled python modules.
*.pyc

# Setuptools distribution folder.
/dist/

# Python egg metadata, regenerated from source files by setuptools.
/*.egg-info
```

Paketimiz için her yeni sürüm çıkaracağımız zaman setup.py dosyası içindeki `version` key'ini düzenlemek gerekiyor.


```bash
# yazdığımız kodu test.pypi ve pypi'e yükleyebilmek için `twine` adında bir program kuruyoruz
$ pip install twine

# yazdığımız projeyi paketlemek için `setuptools` adında bir python program kuruyoruz
$ pip install setuptools

# pip paketimizi derliyoruz
$ python setup.py sdist bdist_wheel

# derleme sonunda projemizin kök klasöründe aşağıdaki klasörler oluşuyor
# - dist/
# - project-name.egg-info/

# pip paketimizi test etmek için lokal olarak kurabiliriz
$ pip install ./dist/project-name-0.0.1.tar.gz

# daha kısası setup.py dosyasının olduğu klasörde yani kök klasörde aşağıdaki komut ile paketimizi sistemimize kurabiliriz
$ pip install .

# veyahut test pypi adresine yükleyip oradan bilgisayarımıza kurabiliriz

# pip paketimizi test pypi'ye yüklüyoruz
# dist/* anlamı dist klasörü altındaki tüm paketleri yükle demek
# sadece .tar.gz uzantılı dosyamızı göstermemiz mümkün
$ twine upload --repository testpypi dist/*

# test pypi adresindeki project-name adındaki pip paketimizi bilgisayarımıza kuruyoruz
# buradaki -i install anlamına geliyor
# buradaki --upgrade ise eğer eski sürüm yüklüyse yükselt anlamına geliyor
$ pip install --upgrade -i https://test.pypi.org/simple/ project-name

# pip paketimizi pypi'ye yüklüyoruz
# dist/* anlamı dist klasörü altındaki tüm paketleri yükle demek
# sadece .tar.gz uzantılı dosyamızı göstermemiz mümkün
$ twine upload --repository pypi dist/*

# pip paketimizi yüklüyoruz
# önceden yüklemişsek upgrade etsin diye --upgrade parametresi ekledik
$ $ pip install --upgrade project-name
```

# CLI Yazmak

Yazdığımız python programı modül yanında CLI olabilir yada script çalıştırabilir. Örneğin abc adında bir script yazıp `python abc` diye çağırınca istediğimiz bir işi yapan küçük bir PIP paketi yazabiliriz.

Modülümüzü hem komut satırı üstünden hemde modül olarak kullanabiliriz.

setup.py içinde yazacağımız bir kaç parametre ile bu mümkün.

// TODO

# PyPI ve Test PyPi Konfigurasyon Dosyası

`twine` aracı her seferinde bizden kullanıcı adı şifre istiyor, soru sormasını engellemek için home klasörümüz içine `.pypirc` adında bir dosya oluşturup aşağıdaki gibi bilgilerimizi girmemiz gerekiyor:

```
[distutils]
index-servers=
    pypi
    testpypi

[pypi]
username: your-user-name
password: xxxxxxxxxx

[testpypi]
repository: https://test.pypi.org/legacy/
username: your-user-name
password: xxxxxxxxxx
```

Görüldüğü üzere hem test pypi hem pypi için ayrı ayrı şifre girdik çünkü ikisi içinde ayrı bir şekilde kayıt açmak gerekiyor. Bir hesapla ikisinide kullanamıyoruz.

# Tricks

Emin değilim ancak `__init__.py` dosyası içine fonksiyon yada class'ımızı yazınca `form packagename import modulename` şeklinde yerine direkt olarak `import packagename` şeklinde kullanabiliyoruz. Yani kısaca modul için ayrı bir dosya açmaya gerek kalmıyor.
