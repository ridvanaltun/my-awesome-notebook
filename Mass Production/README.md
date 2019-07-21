# Mass Production

`Mass production` seri üretim anlamına geliyor. Bu doküman seri üretim amaçlı elektronik projelerin detaylandırmasını içeriyor.

# Table of Contents

<!-- toc -->

- [Fiducial](#fiducial)
- [Panel](#panel)
- [Cheap Mass Production](#cheap-mass-production)
- [Reduce Component Diversity](#reduce-component-diversity)
- [Reel and Cut Tape](#reel-and-cut-tape)
- [Digi-Reel](#digi-reel)
- [Quality Control, Test and Programming](#quality-control-test-and-programming)
- [Sources](#sources)

<!-- tocstop -->

# Fiducial

Pcb üstünde dizgi makinesi kordinatları optimize edebilsin diye 3 tane parlak `via` koyuyoruz. Koyduğumuz bu via'lar `fiducial` adına sahip. Bunu pcb üstüne değilde panel üstündede konabiliyor. Hatta koymadığımız durumlarda pcp üstündeki normal via'ları referans olarak gösterebiliyoruz.

# Panel

Pcb yi tek tek dizgi makinesine vermek zor bir işlem olabiliyor. üretim yapacak dizgi makinesinin ölçülerine bağlı kalarak bir kaç pcb yi bir panel üstünde dizgi makinesine vermek daha hızlı, ucuz ve pratik bir çözüm.

2 tip panel sistemi mevcut, birincisinde pcb ler sıfıra yakın birbirine delikli çok küçük bir çizgi ile ayrılmış durumda. İkincisi ise arada baya mesefe var ve küçük delikli pcb parçaları pcb ler arasında panel üstünde ayrım sağlıyor.

Panellerde dizgi makinasının tutabilmesi için kulaklar oluyor. Eğer kulak olmasaydı belki pcb üstünde bir kaç parça el ile dizilmek zornda kalınabilirdi.

# Cheap Mass Production

Ucuza seri üretim nasıl yapılabilir?

- Pcb siparişini çinli firmalardan.
- Stencil'i evde üreterek.
- Dizgi işini evde manuel pick and place makinasıyla yaparak.
- Lehim işlemi ev yapımı reflow ile.

# Reduce Component Diversity

Misal 1k direnç ve 1.2k direnç var pcb üstünde, eğer iki direncide 1k yada 1.2k yaparsak bu dizgi esnasında işimizi kolaylaştırır. Keza kapasitör içinde bu durum gereçerli. 100nF ile 47nF arasında genelde fark olmuyor. Kritik işler dışında akıllıca davranıp komponenet çeşitliliğini azaltmak mümkün.

# Reel and Cut Tape

`Digikey`'in sitesine girip bir ürün arattığında karşımıza `cut tape`, `reel` veya `digi-reel` olmak üzere 3 farklı şekilde çıkabiliyor. 

Reel alırsak eğer bildiğimiz silindir içinde parçalarımız geliyor, dizgiciye vermeye uygun kompanenet siparişi vermiş oluyoruz.

Cut tape ise adı üstünde kesilip veriliyor. Parçanın minimum adedi 1 seçimi vs bunun sayesinde mümkün aslında. Reel'de ise misal min 2500 adet vs aşmak gerekebiliyor. Dizgiciler cut tape şekilinde kompanenet kabul etmezler.

# Digi-Reel

Misal reel şeklinde en az 2500 adet alabiliyoruz ama bize 1000 adet lazım, digikey ile bu alışverişi yapıyorsak 1000 adeti `digi-reel` ile alabiliyoruz çözüm için. Eğer digi-reel şeklinde alırsak 1000 adet bize yine rulo yapılarak gönderiliyor ancak $7usd ekstra ücret isteniyor.

Aynı sistem `farnell`, `mouser` gibi tedarikçilerdede mevcut.

# Quality Control, Test and Programming

Ürettiğimiz pcb'lerde %1000 de %1 lik bir hata payı varsa her 1000 üründen 1 tanesinde hata vardır demektir. Yani çok az hata olsa bile tüm kartları test etmemiz gerekiyor.

Son kullanıcıya verilen üründe programlama için header bulunduramayacağımıza göre test aapratı tasarlayıp kullanmak gerekiyor. Bunu pogo pinleriyle yapıyoruz.

Voltaj testleri önemli, pcb üstünde hangi hattan ne kadar voltaj geçtiğini bilmemiz gerekiyor, voltaj hattı üstünde test point adında via'lar koyarak pogo pini ile ölçüm yapma şansımız doğuyor.

İdeal bir test devresinde kartı yerleştirdikten sonra elimizi bir daha sürmeden test işleminin bitmesi gerekiyor.

# Sources

[Elektronik Kartları Seri Üretime Hazırlamak](https://www.youtube.com/watch?v=iw5qXzi28cw)
