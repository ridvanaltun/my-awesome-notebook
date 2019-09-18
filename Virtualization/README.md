# Virtualization

Türkçede sanallaştırma deniyor.

Sanallaştırmanın amacı sistemler arası izalasyon ve sunucunun fiziksel kaynaklarını en verimli şekilde kullanmak.

Sanallaştırma yazılımları ile birden çok işletim sistemini aynı anda tek sunucu üstünde yürütebiliyoruz. Eğer bir sunucumuz varsa ve birden fazla servisi ayağa kaldırmak için sunucumuzu parçalara bölmek yada birden fazla servis için farklı sunucular satın almak gerekiyor. En makul çözüm sunucumuzu yazılımsal olarak sanallaştırıp projelerimizi uygulayabileceğimiz alanlar oluşturmak.

Eskiden bir web servisini vs ayağa kaldırmak için fiziksel olarak ayrı makinalar kullanılıyordu ancak bu çok pahalı bir çözüm.

Sonrasında insanlar sanallaştırma yazılımları (Virtual PC vs.) ile tek bir fiziksel bilgisayarı yazılımsal oalrak bölmeye başladılar. Ancak bu çözümde sanallaştırma yazılımlarının bir ağırlığı var. Birde her seferinde yeni bir sanal makina için yeni bir işletim sistemi kuruyoruz, yani fiziksel kaynaklarımızı iyi kullanamıyoruz.

Son olarak docker denen conteiner teknolojisi çıktı. Artık sunucu üstünde işletim sistemi ortak yürütülebiliyor çünkü işletim sisteminin kernel düzeyde izole edebiliyor. Ayrıca yönetmesi çok daha kolay, tek komutla başkasının hazırlamış olduğu sistemi kolayca ayağa kaldırabiliyoruz.

# Table of Contents

- [Virtualization](#virtualization)
- [Table of Contents](#table-of-contents)
- [VPS](#vps)
- [VDS](#vds)

# VPS

İngilizce açılımı `Virtual Private Server`. Sunucular tek bir işletim sistemi üstünde yazılımsal olarak bölünmüş ise `VPS` adı veriliyor.

# VDS

İngilizce açılımı `Virtual Dedicated Server`. Sunucular fiziksel olarak bölünmüş ise buna `VDS` adı veriliyor.
