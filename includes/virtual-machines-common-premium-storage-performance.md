# <a name="azure-premium-storage-design-for-high-performance"></a>Azure Premium Depolama: Yüksek performans tasarımı

Bu makalede, Azure Premium depolama kullanan yüksek performanslı uygulamalar oluşturmak için yönergeler sağlar. Performansı en iyi uygulamaları, uygulamanız tarafından kullanılan teknolojiler için geçerli bir araya geldiğinde bu belgede sağlanan yönergeleri kullanabilirsiniz. Yönergeler göstermek için bu belge boyunca örnek olarak Premium depolama üzerinde çalışan SQL Server kullandınız.

Biz bu makalede depolama katmanı için performans senaryosu, ancak uygulama katmanına en iyi duruma getirmek gerekir. Örneğin, Azure Premium depolama bir SharePoint Çiftliğini barındırıyorsanız, veritabanı sunucusu en iyi duruma getirmek için bu makalede SQL Server örneklerinden kullanabilirsiniz. Ayrıca, SharePoint grubunun Web sunucusu ve uygulama sunucusu çoğu performansı elde etmek için iyileştirin.

Bu makalede, Azure Premium depolama üzerinde uygulama performansını iyileştirme hakkında sık sorulan sorular aşağıdaki yanıt yardımcı olur,

* Uygulama performansını ölçmek nasıl?  
* Neden beklenen yüksek performanslı görmediğinizden?  
* Hangi faktörlerin Premium depolama, uygulama performansınızı etkileyen?  
* Nasıl bu faktörlerin Premium depolama, uygulamanızın performansını etkileyen?  
* Nasıl, IOPS, bant genişliği ve gecikme süresi için en iyi duruma getirebilirsiniz?  

Premium depolama alanında çalışan iş yükleri yüksek performans duyarlı olduğu için özel olarak Premium depolama için bu yönergeleri sağladık. Uygun yerlerde örnek sağladık. Ayrıca standart depolama diskleri ile Iaas Vm'lerinde çalışan uygulamaları bu yönergeleri bazıları uygulayabilirsiniz.

Premium depolamaya bilginiz yoksa, başlamadan önce okumanız [Premium Depolama: Azure sanal makine iş yükleri için yüksek performanslı depolama](../articles/virtual-machines/windows/premium-storage.md) ve [Azure depolama ölçeklenebilirlik ve performans hedefleri](../articles/storage/common/storage-scalability-targets.md)makaleler.

## <a name="application-performance-indicators"></a>Uygulama performans göstergeleri
Bir uygulama performans göstergeleri gibi iyi ya da olmasın kullanarak gerçekleştiriyor olup olmadığını biz değerlendirme, ne kadar hızlı bir uygulama, kullanıcı isteği, istek başına bir uygulama işleme ne kadar veri işleme, kaç uygulamanın belirli bir işleme istekleri bir kullanıcının kendi İsteği gönderdikten sonra bir yanıt almak için beklenecek zaman, ne kadar süre. Bu performans göstergelerini teknik koşulları olan, IOPS, üretilen işi veya bant genişliği ve gecikme süresi.

Premium depolama bağlamında yaygın performans göstergeleri Bu bölümde ele alınacaktır. Aşağıdaki bölümde, uygulama gereksinimlerini toplama, uygulamanız için bu performans göstergelerini ölçmek öğreneceksiniz. Daha sonra en iyi duruma getirme uygulama performansını, bu performans göstergeleri ve bunları iyileştirmeye yönelik öneriler etkileyen faktörleri hakkında bilgi edineceksiniz.

## <a name="iops"></a>IOPS
IOPS sayısı, uygulamanızın depolama diskleri bir saniyede gönderdiği istekleri. Bir giriş/çıkış işlemi okunamadı veya sıralı veya rastgele yazma. Hemen birçok eş zamanlı kullanıcı isteği işlemek bir çevrimiçi satış Web sitesi gibi OLTP uygulamaları gerekir. Kullanıcı istekler INSERT ve update yoğun veritabanı işlemi, uygulamanın hızlı bir şekilde işlemesi gerekir. Bu nedenle, çok yüksek IOPS OLTP uygulamalar gerektirir. Bu tür uygulamalar milyonlarca küçük ve rastgele g/ç isteği işler. Bu tür bir uygulama varsa, uygulama altyapısı IOPS için en iyi duruma getirme tasarlamanız gerekir. Sonraki bölümde *uygulama performansını en iyi duruma getirme*, yüksek IOPS almak için dikkate almanız gereken tüm faktörlerin ayrıntılı olarak ele.

Ne zaman bir premium depolama disk, disk belirtimi uyarınca garantili bir IOPS sayısını Azure hükümlerinin, büyük ölçekli VM ekleyin. Örneğin, P50 disk 7500 IOPS sağlar. Her büyük ölçekli bir VM boyutu da sürdürmek belirli bir IOPS sınırı vardır. Örneğin, standart GS5 VM 80.000 IOPS sınırlama vardır.

## <a name="throughput"></a>Aktarım hızı
Aktarım hızını veya bant genişliği için depolama diskleri belirli bir aralıktaki, uygulamanızın gönderdiği veri miktarıdır. Uygulamanızın büyük GÇ birim boyutları ile giriş/çıkış işlemi gerçekleştiriliyorsa, yüksek aktarım hızı gerektirir. Veri ambarı uygulamalarını teker teker büyük bölümlerinde veri erişim ve sık toplu işlemleri tarama yoğunluklu işlemlerdir sorun eğilimindedir. Diğer bir deyişle, bu tür uygulamalar daha yüksek işlem hacmi gerektiren. Bu tür bir uygulama varsa, aktarım hızını iyileştirmek için kendi altyapısını tasarlamanız gerekir. Sonraki bölümde ayrıntılı olarak bunu başarmanın ayarlamak gerekir faktörleri ele alır.

Ne zaman bir premium depolama disk, disk belirtimi uyarınca Azure hükümlerinin aktarım hızı yüksek ölçeklendirme için VM ekleyin. Örneğin, P50 disk 250 MB, ikinci disk başına aktarım hızı sağlar. Her büyük ölçekli bir VM boyutu, karşılayabilir belirli aktarım hızı sınırı de vardır. Örneğin, standart GS5 VM, bir saniye başına 2.000 MB maksimum aktarım sahiptir. 

Aktarım hızı ve IOPS formülde gösterilen şekilde arasında bir ilişki yoktur.

![](media/premium-storage-performance/image1.png)

Bu nedenle, uygulamanızın ihtiyaç duyduğu en iyi aktarım hızı ve IOPS değerlerini belirlemek önemlidir. Bir en iyi duruma getirmek çalışırken, diğeri de etkilenir. Bir sonraki bölümde *uygulama performansını en iyi duruma getirme*, IOPS ve aktarım hızını iyileştirme hakkında daha fazla ayrıntı biz ele alınacaktır.

## <a name="latency"></a>Gecikme süresi
Gecikme süresi uygulamanın tek bir istek alma, depolama diskleri gönderin ve yanıtı istemciye göndermek için gereken süre anlamına gelmektedir. Bu, IOPS ve aktarım hızı yanı sıra uygulama performansının kritik bir ölçüdür. Bir premium depolama disk gecikme süresini, bir istek için bilgi almak ve uygulamanıza geri iletişim için gereken süredir. Premium depolama, tutarlı düşük gecikme süreleri sağlar. Salt okunur konak üzerinde premium depolama diskleri önbelleğe almayı etkinleştirirseniz, çok daha düşük okuma gecikme süresi elde edebilirsiniz. Diski önbelleğe alma işlemi sonraki bölümünde daha ayrıntılı üzerinde ele alınacaktır *uygulama performansını en iyi duruma getirme*.

Uygulamanıza daha yüksek IOPS ve aktarım hızı alma iyileştirirken, uygulamanızın gecikme süresini etkiler. Uygulama performansı ayarlama sonra gecikme süresi yüksek oranda gecikme süreleri beklenmeyen davranışları önlemek için uygulamanın her zaman değerlendirin.

## <a name="gather-application-performance-requirements"></a>Uygulama Performans gereksinimlerini toplama
Azure Premium depolama alanında çalışan yüksek performanslı uygulamalar tasarlamanın ilk adımı, uygulamanızın performans gereksinimlerini öğrenmektir. Performans gereksinimleri topladıktan sonra uygulamanızı en iyi performans elde etmek için en iyi duruma getirebilirsiniz.

Önceki bölümde, biz ortak performans göstergeleri, IOPS, üretilen işi ve gecikme süresi açıklanmıştır. Bu performans göstergelerinin uygulamanızı istenen kullanıcı deneyimini sunmak için kritik olan belirlemeniz gerekir. Örneğin, yüksek IOPS, saniyede milyonlarca işlem işleme OLTP uygulamaları için en önemlidir. Oysa yüksek aktarım hızı yüksek miktarda verilerden oluşan bir saniye içinde işleme veri ambarı uygulamaları için önemlidir. Son derece düşük gecikme süresi, Web siteleri akış canlı video gibi gerçek zamanlı uygulamalar için önemlidir.

Ardından, uygulamanızı ömrü boyunca en yüksek performans gereksinimlerini ölçer. Aşağıdaki örnek denetim listesini bir başlangıç kullanın. Normal sırasında en yüksek performans gereksinimleri, yoğun olan ve yoğun olmayan saatlerde iş yükü nokta kaydedin. Tüm iş yükleri düzeyleri için gereksinimleri belirleme, uygulamanızın genel performansını gereksinim belirlemek mümkün olacaktır. Örneğin, bir e-ticaret sitesi normal iş yükünü yıllık çoğu günlerde hizmet işlemleri olacaktır. Web sitesinin en yoğun iş yükü, mağazalarımızdaki ürünler veya özel satış olayları sırasında hizmet işlemleri olacaktır. En yoğun iş yükü için sınırlı bir süre genellikle deneyimli olduğu halde normal işleyişi iki veya daha fazla kez ölçeklendirmek için uygulamanızı gerektirebilir. 50. yüzdebirlik, 90. yüzdebirlik ve 99. yüzdebirlik gereksinimleri öğrenin. Bu performans gereksinimleri herhangi bir aykırı değer filtrelemek yardımcı olur ve çalışmalarınızı doğru değerleri için en iyi duruma getirme üzerinde odaklanabilirsiniz.

**Uygulama performans gereksinimleri denetim listesi**

| **Performans gereksinimleri** | **50. yüzdebirlik** | **90. yüzdebirlik** | **99. yüzdebirlik** |
| --- | --- | --- | --- |
| En çok, Saniye başına işlem | | | |
| % Okuma işlemleri | | | |
| % Yazma işlemleri | | | |
| % Rastgele işlemleri | | | |
| % Sıralı işlemler | | | |
| G/ç istek boyutu | | | |
| Ortalama aktarım hızı | | | |
| En çok, Aktarım hızı | | | |
| Min. Gecikme süresi | | | |
| Ortalama gecikme süresi | | | |
| En çok, CPU | | | |
| Ortalama CPU | | | |
| En çok, Bellek | | | |
| Ortalama bellek | | | |
| Sıra Derinliği | | | |

> [!NOTE]
> Bu sayı, uygulamanızın beklenen gelecekteki büyümenin üzerinde temel ölçeklendirme dikkate almanız gerekir. Daha sonra performansı artırmak için altyapıyı değiştirmek zorunda zor olabileceğinden büyüme önceden planlamak için iyi bir fikirdir.
>
>

Var olan bir uygulamanız varsa ve Premium depolama birimine taşımak istiyorsanız öncelikle var olan uygulama için yukarıda bir denetim listesi oluşturun. Ardından, Premium depolama, uygulamanızın bir prototip oluşturun ve açıklanan yönergeleri göre tasarlayın *uygulama performansını en iyi duruma getirme* bu belgenin sonraki bir bölümde bulunan. Sonraki bölümde, performans ölçümleri toplamak için kullanabileceğiniz araçları açıklar.

Mevcut uygulamanızı prototip için benzer bir denetim listesi oluşturun. Benchmarking araçlarını kullanarak iş yüklerini benzetimini yapmak ve prototip uygulama performansına ölçen. Bölümüne [Benchmarking](#benchmarking) daha fazla bilgi için. Premium depolama veya eşleşen uygulama performans gereksinimlerinizi aşan belirlemek için yaparak. Ardından, bir üretim uygulamanız için aynı yönergeler uygulayabilirsiniz.

### <a name="counters-to-measure-application-performance-requirements"></a>Uygulama performans gereksinimleri ölçmek için sayaç
Uygulamanızın, performans gereksinimlerini ölçmek için en iyi yoludur işletim sistemi tarafından sağlanan performans izleme araçları kullanmak için. Linux için PerfMon için Windows ve iostat kullanabilirsiniz. Bu araçlar yukarıdaki bölümde açıklanan her ölçü karşılık gelen sayaçları yakalayın. Uygulamanız çalışırken, normal, yoğun olan ve yoğun olmayan saatlerde iş yükleri Bu sayaç değerlerini yakalamalısınız.

PerfMon sayaçları işlemci, bellek ve her mantıksal disk ve sunucunuzun fiziksel disk için kullanılabilir. Bir VM ile premium depolama disklerini kullanmak için her bir premium depolama diski fiziksel disk sayaçlarıdır ve premium depolama diskleri üzerinde oluşturulan her birim için mantıksal disk sayaçlarıdır. Uygulama yükünüz konak diskleri değerlerini yakalamalısınız. Mantıksal ve fiziksel diskler arasında bire bir eşleme varsa, fiziksel disk sayaçları başvurabilir; Aksi takdirde, mantıksal disk sayaçlara bakın. Linux üzerinde bir CPU ve disk kullanımı raporu iostat komut oluşturur. Disk kullanımı raporu, fiziksel cihazın veya bölüm başına istatistikler sağlar. Bir veritabanı sunucusu, veri ve günlük ile ayrı disklerde varsa, her iki diskin için bu verileri toplayın. Aşağıdaki tabloda, diskler, işlemci ve bellek sayaçları açıklanmaktadır:

| Sayaç | Açıklama | PerfMon | iostat |
| --- | --- | --- | --- |
| **IOPS veya saniye başına işlem** |Saniye başına depolama diskini verilen g/ç isteklerinin sayısı. |Disk Okuma/sn <br> Disk Yazma/sn |tps <br> r/sn <br> w/sn |
| **Disk okuma ve yazma işlemleri** |% Okuma ve yazma disk üzerinde gerçekleştirilen işlemler. |% Disk okuma süresi <br> Disk yazma süresi % |r/sn <br> w/sn |
| **Aktarım hızı** |Saniye başına yazılan veya okunan veri miktarı. |Disk Okuma Bayt/sn <br> Disk Yazma Bayt/sn |kB_read/sn <br> kB_wrtn/sn |
| **Gecikme süresi** |Bir disk g/ç isteği toplam süre. |Ortalama Disk sn/Okuma <br> Ortalama disk sn/yazma |await <br> svctm |
| **G/ç boyutu** |G/ç boyutu, depolama disklerini sorunları ister. |Ortalama Disk bayt/okuma <br> Ortalama Disk Bayt/yazma |avgrq sz |
| **Sıra Derinliği** |Bekleyen g/ç sayıda okuyamaz ya da depolama diske yazılan üzere bekleyen istekleri. |Geçerli Disk Sırası Uzunluğu |avgqu sz |
| **Maks. Bellek** |Uygulamayı sorunsuz bir şekilde çalıştırmak için gerekli bellek miktarını |% Kullanılan kaydedilmiş bayt |Vmstat'ı kullanın |
| **Maks. CPU** |Uygulama düzgün çalışması için gerekli CPU tutar |% İşlemci zamanı |% kullanımı |

Daha fazla bilgi edinin [iostat](https://linux.die.net/man/1/iostat) ve [PerfMon](https://msdn.microsoft.com/library/aa645516.aspx).

## <a name="optimizing-application-performance"></a>Uygulama performansını iyileştirme
Premium depolama alanında çalışan bir uygulamanın performansı etkileyen ana faktörler yapısı, GÇ, VM boyutunu, Disk boyutu, disk sayısı, Disk önbelleğe alma, çoklu iş parçacığı kullanımı ve sıra derinliğini isteklerdir. Bu etkenler bazıları, sistem tarafından sağlanan düğmelerini ile denetleyebilirsiniz. Çoğu uygulama GÇ boyutu ve sıra derinliğini doğrudan değiştirmek için bir seçenek vermeyebilir. Örneğin, SQL Server kullanıyorsanız, g/ç boyutu ve sıra derinliği seçemezsiniz. SQL Server çoğu performansı elde etmek için en iyi g/ç boyutu ve sıra derinliği değerleri seçer. Performans gereksinimlerini karşılamak için uygun kaynakların sağlayabilmeniz Etkenler her iki tür, uygulama performansı üzerindeki etkisini anlamak önemlidir.

Bu bölümde, uygulama performansınızı en iyi duruma getirmek ne kadar gerektiğini belirlemek için oluşturduğunuz uygulama gereksinimleri denetim listesine bakın. Üzerinde bağlı olarak ayarlamak gerekir, bu bölümdeki hangi faktörlerin belirlemek mümkün olacaktır. Her faktör, uygulama performansı üzerindeki etkisini Tanık için uygulama kurulumunuzu Kıyaslama Araçları'nı çalıştırın. Başvurmak [Benchmarking](#Benchmarking) ortak Kıyaslama araçları, Windows ve Linux Vm'leri çalıştırma adımları için bu makalenin sonunda bölüm.

### <a name="optimizing-iops-throughput-and-latency-at-a-glance"></a>IOPS, üretilen işi ve gecikme süresi bir bakışta en iyi duruma getirme

Aşağıdaki tabloda, performans etmenleri ve IOPS, aktarım hızı ve gecikme süresini iyileştirmek için gereken adımları özetler. Bu Özet aşağıdaki bölümler her anlatmaktadır çok daha ayrıntılı bir faktördür.

Daha fazla VM boyutları ve IOPS, aktarım hızı ve gecikme süresi kullanılabilir VM her türü için bilgi için [Linux VM boyutları](../articles/virtual-machines/linux/sizes.md) veya [Windows VM boyutları](../articles/virtual-machines/windows/sizes.md).

| &nbsp; | **IOPS** | **Aktarım hızı** | **Gecikme süresi** |
| --- | --- | --- | --- |
| **Örnek senaryo** |İkinci oranı başına çok yüksek işlem gerektiren Kurumsal OLTP uygulama. |Uygulama işleme, çok miktarda veri depolamaya yönelik kurumsal veri. |Çevrimiçi oyun gibi kullanıcı isteklerine hızlı yanıtlar gerektiren gerçek zamanlı uygulamalar. |
| Performans etmenleri | &nbsp; | &nbsp; | &nbsp; |
| **G/ç boyutu** |Daha küçük g/ç boyutu daha yüksek IOPS verir. |Daha büyük g/ç boyutu için daha yüksek iş hacmi üretir. | &nbsp;|
| **VM boyutu** |Uygulama ihtiyacınıza göre daha yüksek IOPS sağlayan bir VM boyutu kullanın. |Aktarım hızı sınırı, uygulama dağıtımı gereksinimi büyük bir VM boyutu kullanın. |Bir VM boyutu teklifler sınırları, uygulama dağıtımı gereksinimi büyük ölçeklendirme kullanın. |
| **Disk boyutu** |Uygulama ihtiyacınıza göre daha yüksek IOPS sağlayan bir disk boyutunu kullanın. |Aktarım hızı sınırı, uygulama dağıtımı gereksinimi büyük bir disk boyutunu kullanın. |Bir disk boyutu teklifler sınırları, uygulama dağıtımı gereksinimi büyük ölçeklendirme kullanın. |
| **VM ve Disk ölçek sınırları** |Seçilen VM boyutu, IOPS sınırı bağlı premium depolama disklerini tarafından toplam IOPS değerinden büyük olmalıdır. |Aktarım hızı sınırı seçilen VM boyutu premium depolama disklerini bağlı tarafından toplam aktarım hızı büyük olmalıdır. |Ölçek sınırları seçtiğiniz VM boyutuna bağlı premium depolama disklerini toplam ölçek sınırları büyük olmalıdır. |
| **Diski önbelleğe alma** |Salt okunur Önbellek Okuma yoğun işlemleri daha yüksek okuma IOPS almak için premium depolama diskleri etkinleştirin. | &nbsp; |Premium depolama disklerini gecikme süreleri çok düşük okuma almaya hazır yoğun işlemleri salt okunur önbellek etkinleştirin. |
| **Disk şeridi oluşturma** |Birden çok disk kullanın ve bunları birbirine birleştirilmiş daha yüksek IOPS ve aktarım hızı sınırı almak için stripe. VM başına birleşik sınırı bağlı premium disklerin toplam sınırlardan daha yüksek olması gerektiğini unutmayın. | &nbsp; | &nbsp; |
| **Stripe boyutu** |OLTP uygulamalarda görülen rastgele küçük g/ç deseni için daha küçük stripe boyutu. Örneğin, Şerit boyutu 64 KB'ın SQL Server OLTP uygulama için kullanın. |Veri ambarı uygulamalarda görülen sıralı büyük g/ç deseni için stripe boyutu daha büyük. Örneğin, SQL Server veri ambarı uygulaması için stripe boyutu 256 KB kullanın. | &nbsp; |
| **Çoklu iş parçacığı kullanımı** |Premium depolama, daha yüksek IOPS ve aktarım hızı önünü açacak daha yüksek sayıda istek göndermek için çoklu iş parçacığı kullanımı. Örneğin, SQL Server üzerinde SQL Server için daha fazla CPU tahsis etmek için yüksek MAXDOP değeri ayarlayın. | &nbsp; | &nbsp; |
| **Sıra Derinliği** |Daha büyük bir sıra derinliğini daha yüksek IOPS verir. |Daha büyük bir sıra derinliğini daha yüksek iş hacmi üretir. |Daha küçük sıra derinliğini daha düşük gecikme süreleri sağlar. |

## <a name="nature-of-io-requests"></a>G/ç istekleri yapısı
Bir g/ç isteği, uygulamanızın çalıştırdığı giriş/çıkış işlemi birimidir. G/ç istekleri, rastgele veya sıralı, yapısını tanımlayan okuma veya yazma, küçük veya büyük, uygulamanızın performans gereksinimlerini belirlemenize yardımcı olur. Uygulama altyapınızı tasarlama doğru kararları sağlamak için g/ç istekleri yapısını anlamak çok önemlidir.

GÇ boyutu daha önemli faktörler biridir. Uygulamanız tarafından oluşturulan giriş/çıkış işlem istek boyutu g/ç boyutudur. GÇ boyutu, performans özellikle uygulama sunmayı başarabilseydiniz nasıl bant genişliği ve IOPS üzerinde önemli bir etkisi yoktur. Aşağıdaki formülü IOPS, arasındaki ilişkiyi gösterir GÇ boyutu ve bant genişliği/aktarım hızı.  
    ![](media/premium-storage-performance/image1.png)

Bazı uygulamalar, desteklerken bazı uygulamalar, g/ç boyutu alter olanak tanır. Örneğin, SQL Server en iyi GÇ kendi boyutunu belirler ve kullanıcıların değiştirmek için tüm düğmelerini sağlamaz. Öte yandan, Oracle adlı bir parametreyi sağlar [DB\_BLOCK\_BOYUTU](https://docs.oracle.com/cd/B19306_01/server.102/b14211/iodesign.htm#i28815) yapılandırabileceğiniz kullandığı veritabanı g/ç isteği boyutu.

GÇ boyutunu değiştirmek izin vermez, bir uygulama kullanıyorsanız, uygulamanız için en uygun KPI performansı iyileştirmek için bu makaledeki yönergeleri kullanın. Örneğin,

* OLTP uygulamanın milyonlarca küçük ve rastgele g/ç isteği oluşturur. Bu tür bir GÇ isteklerini işlemek için daha yüksek IOPS almak için uygulama altyapınızı tasarlamanız gerekir.  
* Büyük ve sıralı g/ç istekleri bir veri ambarı uygulaması oluşturur. Bu tür bir GÇ isteklerini işlemek için daha yüksek bant genişliği veya işleme almak için uygulama altyapınızı tasarlamanız gerekir.

GÇ boyutu değiştirmenize olanak tanıyan bir uygulama kullanıyorsanız, diğer performans yönergeleri ek olarak g/ç boyutu için bu kural karşısında kullanın,

* Daha yüksek IOPS almak için daha küçük g/ç boyutu. Örneğin, 8 KB'lık bir OLTP uygulama.  
* Daha yüksek bant genişliği/aktarım hızı alma için daha büyük g/ç boyutu. Örneğin, 1024 KB veri ambarı uygulaması.

Nasıl, IOPS ve aktarım hızını veya bant genişliği, uygulamanız için hesaplayabilirsiniz hakkında bir örnek aşağıdadır. P30 disk kullanarak bir uygulamayı düşünün. Maksimum IOPS ve aktarım hızı/P30 disk bant genişliği elde edebilirsiniz 5000 IOP 200 MB saniye başına sırasıyla olduğundan. Uygulamanızın gerektirdiği P30 diskten maksimum IOPS ve daha küçük bir g/ç boyutu 8 KB'lık gibi kullanırsanız, sonuçta elde edilen bant genişliği almanız mümkün olacaktır 40 MB saniye başına sunulmuştur. Ancak, en yüksek verimi/bant genişliği P30 diskten uygulamanızın gerektirdiği ve daha büyük bir g/ç boyutu 1024 KB gibi kullanıyorsanız, elde edilen IOPS daha az olacaktır 200 IOPS. Bu nedenle, her iki uygulamanın IOPS ve aktarım hızını veya bant genişliği gereksinimi karşıladığından emin GÇ boyutu ayarlayın. Aşağıdaki tabloda, farklı GÇ boyutları ve bunların karşılık gelen IOPS ve aktarım hızı için P30 disk özetler.

| Uygulama dağıtımı gereksinimi | G/ç boyutu | IOPS | Aktarım hızını veya bant genişliği |
| --- | --- | --- | --- |
| Maks. IOPS |8 KB |5.000 |Saniye başına 40 MB |
| En fazla aktarım hızı |1.024 KB |200 |Saniye başına 200 MB |
| En fazla aktarım hızı + yüksek IOPS |64 KB |3,200 |Saniye başına 200 MB |
| En fazla IOPS + yüksek aktarım hızı |32 KB |5.000 |Saniye başına 160 MB |

IOPS ve tek bir premium depolama diski en yüksek değerden daha yüksek bant genişliği almak için birden fazla premium disk şeritli birlikte kullanın. Örneğin, bir birleşik IOPS, 10.000 IOPS veya 400 MB'ın birleşik bir aktarım hızı, saniye başına almak için iki P30 disk stripe. Sonraki bölümde açıklandığı gibi birleşik destekleyen bir VM boyutu kullanmalısınız disk IOPS ve aktarım hızı.

> [!NOTE]
> IOPS ya da artırır diğer aktarım hızı arttıkça, aktarım hızı veya IOPS limitlerine disk veya sanal makine herhangi birini artırma isabet değil emin olun.
>
>

GÇ boyutu uygulama performansı üzerindeki etkisini Tanık için diskleri ve VM üzerinde Kıyaslama araçları çalıştırabilirsiniz. Birden çok test çalıştırması oluşturun ve her çalıştırma için farklı g/ç boyutuna yönelik etkisini öğrenmek için kullanın. Başvurmak [Benchmarking](#Benchmarking) daha fazla bilgi için bu makalenin sonunda bölüm.

## <a name="high-scale-vm-sizes"></a>Büyük ölçekli VM boyutları
Bir uygulama tasarlama başlattığınızda, yapmanız gereken ilk unsur, uygulamanızı barındırmak için bir VM seçin biridir. Premium depolama, daha yüksek bilgi işlem gücü ve yüksek yerel disk g/ç performansı gerektiren uygulamalar çalıştıran yüksek ölçek VM boyutları ile birlikte gelir. Bu sanal makineler yerel disk için daha hızlı işlemciler, daha yüksek bellek / çekirdek oranına ve bir Solid-State sürücüsü (SSD) sağlayın. DS, DSv2 ve GS serisi VM'ler yüksek ölçek destekleyen Premium Storage Vm'lerini örnekleridir.

Yüksek ölçek VM'ler, farklı boyutlarda farklı sayıda CPU çekirdekleri, bellek, işletim sistemi ve geçici disk boyutu ile kullanılabilir. Her VM boyutu VM'ye ekleyebilirsiniz veri diskleri en fazla sayısını da vardır. Bu nedenle, seçtiğiniz VM boyutu ne kadar bellek, işleme etkiler ve depolama kapasitesi, uygulamanız için kullanılabilir. İşlem de etkiler ve depolama maliyeti. Örneğin, aşağıdaki DS serisi, DSv2 serisi ve GS serisi en büyük sanal makine boyutu özellikleri şunlardır:

| VM boyutu | CPU çekirdekleri | Bellek | VM disk boyutları | En çok, Veri diskleri | Önbellek boyutu | IOPS | Bant genişliği önbelleği GÇ sınırları |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS14 |16 |112 GB |İŞLETİM SİSTEMİ 1023 GB = <br> Yerel SSD 224 GB = |32 |576 GB |50.000 IOPS <br> Saniye başına 512 MB |4000 IOPS ve saniyede 33 MB |
| Standard_GS5 |32 |448 GB |İŞLETİM SİSTEMİ 1023 GB = <br> Yerel SSD 896 GB = |64 |4224 GB |ÜST SINIR: 80.000 IOPS <br> Saniyede 2.000 MB |5.000 IOPS ve saniyede 50 MB |

Tüm kullanılabilir Azure VM boyutlarını tam bir listesini görüntülemek için başvurmak [Windows VM boyutları](../articles/virtual-machines/windows/sizes.md) veya [Linux VM boyutları](../articles/virtual-machines/linux/sizes.md). Karşılayan ve istenen uygulama performans gereksinimlerinizi ölçeklendirme bir VM boyutu seçin. Buna ek olarak, VM boyutları seçerken önemli noktalar aşağıdaki dikkate alın.

*Ölçek sınırları*  
Disk ve VM başına maksimum IOPS limitlerine farklı ve birbirinden bağımsızdır. Uygulama için bağlı premium disklerin yanı sıra VM sınırları dahilinde IOPS yönlendirdiğini emin olun. Aksi takdirde, uygulama performansı azaltma ile karşılaşırsınız.

Örneğin, bir uygulama dağıtımı gereksinimi en fazla 4000 IOPS olduğunu varsayın. Bunu başarmak için DS1 VM P30 diskte sağlayın. P30 disk, en fazla 5000 IOPS teslim edebilirsiniz. Ancak, DS1 VM 3,200 IOPS için sınırlıdır. Sonuç olarak, uygulama performansı 3,200 IOPS VM sınırında tarafından kısıtlanmış ve performansın düşmesine neden olur. Bu durumu önlemek için hem yapacak bir VM ve disk boyutu uygulama gereksinimlerini karşılayan'ı seçin.

*İşlem maliyeti*  
Çoğu durumda, Premium depolama kullanan işlemi, genel maliyeti standart depolama kullanmaktan daha düşük olduğunu mümkündür.

Örneğin, 16. 000 IOPS gerektiren bir uygulamayı düşünün. Bu performans elde etmek için standart bir gerekir\_16.000 32 standart depolama 1 TB'tan büyük disklere kullanarak bir maksimum IOPS değeri verebilirsiniz D14 Azure Iaas VM. Her 1TB standart depolama diskini, en fazla 500 IOPS elde edebilirsiniz. Bu VM başına aylık tahmini maliyeti $1,570 olacaktır. Aylık maliyet 32 standart depolama disklerinin $1,638 olacaktır. Tahmini aylık toplam maliyeti $3,208 olacaktır.

Ancak, aynı uygulama Premium depolama üzerinde barındırılıyorsa, daha küçük bir VM boyutu ve daha az sayıda premium depolama diskleri, bu nedenle ilişkin genel maliyeti azaltır gerekir. Standart\_DS13 VM dört P30 diskleri kullanarak 16.000 IOPS gereksinimi karşılamak. DS13 VM 25,600 bir maksimum IOPS değeri ve bir maksimum IOPS'ye 5.000 her P30 disk sahiptir. Genel olarak, bu yapılandırma 5.000 x 4 = 20.000 ulaşabileceği IOPS. Bu VM başına aylık tahmini maliyeti $1,003 olacaktır. Aylık maliyeti dört P30 premium depolama diskler $544.34 olacaktır. Tahmini aylık toplam maliyeti $1,544 olacaktır.

Standart ve Premium depolama için aşağıdaki tabloda bu senaryonun maliyeti dökümü özetler.

| &nbsp; | **Standart** | **Premium** |
| --- | --- | --- |
| **Sanal makinenin aylık maliyet** |$1,570.58 (standart\_D14) |$1,003.66 (standart\_DS13) |
| **Disk aylık maliyet** |$1,638.40 (32 x 1 TB disk) |$544.34 (4 x P30 diskler) |
| **Aylık toplam maliyet** |$3,208.98 |$1,544.34 |

*Linux dağıtımları*  

Azure Premium depolama sayesinde, Windows ve Linux çalıştıran sanal makineler için performans aynı düzeyde alın. Linux dağıtım paketlerini birçok türdeki destekliyoruz ve listenin tamamını görmek [burada](../articles/virtual-machines/linux/endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Farklı dağıtım paketlerini farklı türde iş yükleri için daha uygun olduğunu unutmayın. Çeşitli iş yükünüzü çalıştıran distro bağlı olarak performans düzeylerini görürsünüz. Linux dağıtımları ile uygulamanızı test edin ve en uygun olanı seçin.

Linux çalıştıran Premium depolama sayesinde, yüksek performans sağlamak için gerekli sürücüler hakkında en son güncelleştirmeleri denetleyin.

## <a name="premium-storage-disk-sizes"></a>Premium depolama diski boyutları
Azure Premium depolama şu anda sekiz disk boyutları sunar. Her disk boyutu, IOPS, bant genişliği ve depolama için farklı ölçek sınırına sahiptir. Sağ uygulama gereksinimleri ve büyük ölçekli VM boyutuna bağlı olarak Premium depolama Disk boyutu seçin. Aşağıdaki tabloda, sekiz diskleri boyutlara ve bunların özelliklerini gösterir. P4, P6 ve P15 boyutları şu anda yalnızca yönetilen diskler için desteklenir.

| Premium disk türü  | P4    | P6    | P10   | P15 | P20   | P30   | P40   | P50   | 
|---------------------|-------|-------|-------|-------|-------|-------|-------|-------|
| Disk boyutu           | 32 GB | 64 GB | 128 GB| 256 GB| 512 GB            | 1024 GB (1 TB)    | 2048 GB (2 TB)    | 4095 GB (4 TB)    | 
| Disk başına IOPS       | 120   | 240   | 500   | 1100 | 2300              | 5000              | 7500              | 7500              | 
| Disk başına aktarım hızı | Saniye başına 25 MB  | Saniye başına 50 MB  | Saniye başına 100 MB |Saniye başına 125 MB | 150 MB / saniye | Saniye başına 200 MB | Saniye başına 250 MB | Saniye başına 250 MB | 


Seçilen diskte bağlıdır seçtiğiniz kaç diskinin boyutu. Uygulama dağıtımı gereksinimi karşılamak için tek bir P50 disk veya birden çok P10 disk kullanabilirsiniz. Seçim yaparken, aşağıda listelenen hesabında dikkate alınacak noktalar alın.

*Ölçek sınırlarını (IOPS ve aktarım hızı)*  
Her Premium disk boyutu, IOPS ve aktarım hızı sınırlarını, VM ölçek sınırları farklı ve bağımsız. Toplam IOPS ve aktarım hızı disklerden seçtiğiniz VM boyutuna ölçek sınırları içinde olduğundan emin olun.

Örneğin, bir uygulama dağıtımı gereksinimi olan en fazla 250 MB/sn aktarım hızı ve tek bir P30 disk DS4 VM kullanıyorsanız. DS4 VM'yi en fazla 256 MB/sn aktarım hızı verebilirsiniz. Ancak, tek bir P30 disk 200 MB/sn aktarım hızı sınırı vardır. Sonuç olarak, uygulama 200 MB/sn disk sınırı nedeniyle, kısıtlı. Bu sınırı aşmak için birden fazla veri diskleri VM sağlama veya P40 veya P50 disklerinizi yeniden boyutlandırabilirsiniz.

> [!NOTE]
> Okuma önbelleği tarafından sunulan disk IOPS ve aktarım hızı, dolayısıyla disk sınırları için değil konu içinde yer almaz. Önbellek, VM başına ayrı, IOPS ve aktarım hızı sınırına sahiptir.
>
> Örneğin, başlangıçta okuma ve yazma işlemleri 60 MB/sn ve 40 MB/sn sırasıyla. Zaman içerisinde, önbellek warms ve daha fazlasını okur, önbellekten sunar. Ardından, diskten daha yüksek yazma aktarım hızı elde edebilirsiniz.
>
>

*Disk sayısı*  
Uygulama gereksinimleri değerlendirerek ihtiyacınız olacak disk sayısını belirler. Her VM boyutu ayrıca VM ekleyebileceğiniz disk sayısı üzerinde bir sınırı vardır. Genellikle, bu iki kez çekirdek sayısıdır. Seçtiğiniz VM boyutuna gerekli disk sayısı desteklediğinden emin olun.

Premium depolama diskleri için standart depolama disklerinin kıyasla daha yüksek performans özelliklerine sahip unutmayın. Bu nedenle, uygulamanız için Premium depolama standart depolama kullanarak Azure Iaas VM geçiş yapıyorsanız büyük olasılıkla uygulamanız için aynı veya daha yüksek performans elde etmek için daha az premium diskler gerekir.

## <a name="disk-caching"></a>Diski önbelleğe alma
Azure Premium depolama kullanan yüksek ölçek VM'ler BlobCache adlı çok katmanlı bir önbelleğe alma teknolojisini sahiptir. BlobCache önbelleğe almak için bir sanal makinenin RAM ve yerel SSD bileşimini kullanır. Bu önbellek, Premium depolama kalıcı disk ve VM yerel diskler için kullanılabilir. Varsayılan olarak, önbellek okuma/yazma için işletim sistemi diskleri ve salt okunur için Premium depolama alanında barındırılan veri diskleri için ayar. Premium depolama disklerini etkin disk önbelleğe alma ile yüksek ölçek Vm'leri temel disk performansı aşan son derece yüksek performans düzeyi elde edebilirsiniz.

> [!WARNING]
> Bir Azure diskinin önbellek ayarları değiştirmesini ayırır ve hedef disk yeniden ekler. İşletim sistemi diski ise, sanal makine yeniden başlatılır. Tüm uygulamalar/disk önbellek ayarı değiştirmeden önce bu kesintisi tarafından etkilenebilecek hizmetlerini durdurun.
>
>

BlobCache nasıl çalıştığı hakkında daha fazla bilgi için iç başvuran [Azure Premium depolama](https://azure.microsoft.com/blog/azure-premium-storage-now-generally-available-2/) blog gönderisi.

Doğru ortaklık diskler kümesi önbelleğini etkinleştirmek önemlidir. Disk bir premium disk üzerinde önbelleğe alma etkinleştirmelisiniz mı üzerinde iş yükü deseninin o diski bağımlı olmadan işleme. Aşağıdaki tabloda, varsayılan işletim sistemi ve veri diskleri için önbellek ayarlarını gösterir.

| **Disk türü** | **Varsayılan önbellek ayarı** |
| --- | --- |
| İşletim sistemi diski |ReadWrite |
| Veri diski |SaltOkunur |

Veri diskleri için önerilen disk önbellek ayarları verilmiştir,

| **Disk önbellek ayarı** | **Bu ayarı kullanmak ne zaman öneriye** |
| --- | --- |
| None |Konak önbelleği salt yazılır ve yazma yoğunluklu disklerin hiçbiri olarak yapılandırın. |
| SaltOkunur |Konak önbelleği salt okunur ve okuma-yazma diskleri için salt okunur olarak yapılandırın. |
| ReadWrite |Yalnızca uygulamanızın düzgün bir şekilde önbelleğe alınmış verileri gerektiğinde kalıcı disk yazma işliyorsa, konak önbelleği ReadWrite yapılandırın. |

*Salt okunur*  
Salt okunur verileri Premium depolama diskleri önbelleğe alma yapılandırarak, düşük okuma gecikme süresi elde etmek ve uygulamanız için çok yüksek okuma IOPS ve aktarım hızı alma. İki neden şudur

1. Okuma VM bellek ve yerel SSD önbellek, gerçekleştirilen, Azure blob depolama alanında veri diskten okuma hızlıdır.  
1. Premium depolama, önbellek, doğru disk IOPS ve aktarım hızı hizmet okuma sayılmaz. Bu nedenle, uygulamanızın daha yüksek toplam IOPS ve aktarım hızı elde edebilirsiniz.

*ReadWrite*  
Varsayılan olarak, işletim sistemi diskleri etkin okuma/yazma önbelleği sahiptir. Yakın zamanda ReadWrite verilerini diskler de önbelleğe alma desteği ekledik. Önbelleğe alma ReadWrite kullanıyorsanız, verileri önbellekten kalıcı diske yazmak için uygun bir yol olmalıdır. Örneğin, SQL Server önbelleğe alınmış veri kalıcı depolama diskleri kendi başına yazma işler. ReadWrite önbellek kullanarak gerekli verileri kalıcı hale getirmeniz işlemez bir uygulama ile VM kilitlenmesi durumunda veri kaybına neden olabilir.

Örneğin, aşağıdakileri yaparak Premium depolama üzerinde çalışan SQL Server için bu yönergeleri uygulayabilirsiniz

1. "Salt okunur" önbellek veri dosyalarını barındıran premium depolama disklerini yapılandırın.  
   a.  Hızlı, veri sayfaları, çok daha hızlı olarak önbellekten alınır olduğundan SQL Server sorgusu süresi için verileri doğrudan diskleri kıyasla daha düşük önbellekten okur.  
   b.  Okuma Önbelleği'ndeki hizmet verebilmesi için ek aktarım hızı premium veri diskleri kullanılabilir anlamına gelir. SQL Server kullanmak daha fazla veri sayfaları ve yedekleme/geri yükleme gibi diğer işlemler alınırken doğrultusunda bu ek aktarım hızı, toplu iş yükleri ve dizin oluşturur.  
1. Yapılandırma "None" premium depolama disklerini günlük dosyalarını barındıran önbellek.  
   a.  Günlük dosyaları, öncelikli olarak yazma yoğunluklu operations sahiptir. Bu nedenle, bunlar salt okunur önbellekten yarar görmez.

## <a name="disk-striping"></a>Disk şeridi oluşturma
Premium depolama kalıcı diski ile VM'ye yüksek ölçek, diskleri birlikte, IOPS, bant genişliği ve depolama kapasitesi toplamak için Şerit.

Windows üzerinde depolama alanları için stripe diskleri birlikte kullanabilirsiniz. Bir havuzda her disk için bir sütun yapılandırmanız gerekir. Aksi takdirde, genel performansını şeritli birim beklenen trafiği disklerde dağılımına nedeniyle daha düşük olabilir.

Önemli: Sunucu Yöneticisi kullanıcı Arabirimi kullanarak, toplam 8 şeritli birim için en fazla sütun sayısını ayarlayabilirsiniz. 8'den fazla disk eklerken, birim oluşturmak için PowerShell kullanın. PowerShell kullanarak, sütun sayısı eşit disk sayısını ayarlayabilirsiniz. Örneğin, tek kümesi 16 disk varsa; 16 sütunlarda belirttiğiniz *NumberOfColumns* parametresinin *New-VirtualDisk* PowerShell cmdlet'i.

Linux üzerinde stripe disklere MDADM yardımcı birlikte kullanın. Linux üzerinde şeritleme disklerde ayrıntılı adımlar için bkz [Linux'ta yazılım RAID yapılandırma](../articles/virtual-machines/linux/configure-raid.md).

*Stripe boyutu*  
Disk bölümleme türüyle önemli bir yapılandırmada stripe boyutudur. Şerit veya blok boyutu en küçük şeritli birim üzerinde uygulama gideren veri öbektir. Yapılandırdığınız stripe boyutu, uygulama ve talep desenine türüne bağlıdır. Yanlış stripe boyutu seçerseniz, müşteri adayları, uygulamanızın performans için GÇ yanlış hizalanmış neden olabilir.

Uygulamanız tarafından oluşturulan bir g/ç isteği disk stripe boyutundan daha büyük ise, örneğin, depolama sistemi bunu stripe birim sınırları arasında birden fazla diskte yazar. Bu verilere erişmek için zaman olduğunda, isteği tamamlamak için birden fazla stripe birimlerindeki arama gerekecektir. Toplu etkisi, benzer bir davranış önemli performans düşüşüne neden olabilir. Öte yandan, g/ç istek boyutunun stripe boyuttan daha küçükse ve doğası gereği rastgele ise, g/ç istekleri bir performans sorununa neden ve sonuçta g/ç performansının düşmesinde aynı disk üzerinde ekleme.

Uygulamanızı çalıştıran iş yükü türüne bağlı olarak, uygun stripe boyutu seçin. Rastgele küçük g/ç istekleri için stripe daha küçük bir boyut kullanın. Oysa için büyük sıralı g/ç istekleri daha büyük bir Şerit boyutu kullanır. Premium depolama alanında çalışan stripe boyut önerileri için uygulamayı öğrenin. SQL Server için stripe boyutu 64 KB OLTP iş yükleri için veri ambarı iş yükleri için 256 KB ve yapılandırın. Bkz: [Azure vm'lerde SQL Server için en iyi performans](../articles/virtual-machines/windows/sql/virtual-machines-windows-sql-performance.md#disks-guidance) daha fazla bilgi için.

> [!NOTE]
> Ayrıca, en fazla 32 premium depolama disklerini DS serisi VM üzerinde ve bir GS serisi VM 64 premium depolama disklerini birlikte stripe.
>
>

## <a name="multi-threading"></a>Çoklu iş parçacığı
Azure Premium depolama platformu yüksek düzeyde paralel olacak şekilde tasarlanmıştır. Bu nedenle, çok iş parçacıklı bir uygulama, tek iş parçacıklı bir uygulamadan daha çok daha yüksek performans elde eder. Çok iş parçacıklı bir uygulamanın kendi görevlerini birden çok iş parçacığı arasında böler ve en fazla VM ve disk kaynakları yararlanarak yürütme işleminin verimliliğini artırır.

Örneğin, uygulamanız tek bir çekirdek iki iş parçacığı kullanarak VM üzerinde çalışıyorsa, CPU verimliliği elde etmek için iki iş parçacığı arasında geçiş yapabilirsiniz. CPU, tek bir iş parçacığı bir disk g/ç tamamlamak için beklerken diğer iş parçacığına geçiş yapabilirsiniz. Bu şekilde, daha fazla tek bir iş parçacığı olduğu kadar temkinli iki iş parçacığı gerçekleştirebilirsiniz. VM birden fazla çekirdek varsa, her temel görevleri paralel olarak yürütmek olduğundan daha fazla çalışma süresi azaltır.

Kullanıma hazır bir uygulama uygulayan tek şeklini değiştirmek mümkün olmayabilir iş parçacığı veya çoklu iş parçacığı. Örneğin, SQL Server birden çok CPU ve çok çekirdekli işleyebilir. Ancak, SQL Server, hangi koşullar altında bir sorgu işlemek için bir veya daha fazla iş parçacığı özelliğinden yararlanır karar verir. Bu sorguları çalıştırmak ve çoklu iş parçacığı kullanan dizinleri oluşturun. Büyük tabloları birleştirme ve kullanıcıya döndürmeden önce verileri sıralama içeren bir sorgu için SQL Server, büyük olasılıkla birden çok iş parçacığı kullanır. Ancak, bir kullanıcı SQL Server tek bir iş parçacığı veya birden çok iş parçacığı kullanarak bir sorgu yürütmediğini denetleyemezsiniz.

Çoklu iş parçacığı veya paralel bu etkilemek için alter yapılandırma ayarlarını bir uygulamayı işleme. Örneğin, durumunda SQL Server maksimum paralellik derecesi yapılandırma olur. Bu ayar, en fazla paralel sırasında SQL Server kullanabilirsiniz işlemci sayısını yapılandırmanıza olanak sağlar, MAXDOP adlı işleniyor. Tek tek sorguları veya dizin işlemleri için MAXDOP yapılandırabilirsiniz. Performans kritik bir uygulama için sistem kaynakları Bakiye istediğinizde yararlıdır.

Örneğin, SQL Server'ı kullanarak uygulamanızı geniş bir sorgu ve aynı zamanda bir dizin işlemi yürütülürken varsayalım. Bize dizin işlemi büyük sorguya kıyasla daha fazla yüksek performanslı olmasını istediğinizi varsayalım. Böyle bir durumda, sorgu için MAXDOP değeri daha yüksek olması için dizin işlemi, MAXDOP değeri ayarlayabilirsiniz. Bu şekilde, SQL Server daha fazla büyük sorguya ayırabilirsiniz işlemci sayısını karşılaştırıldığında dizin işlemi için yararlanabileceğiniz işlemci sayısını sahiptir. Unutmayın, her işlem için SQL Server kullanacak bir iş parçacığı sayısını kontrol etmez. İşlemci için ayrılmış en fazla sayısını denetleyebilirsiniz çoklu iş parçacığı.

Daha fazla bilgi edinin [biri derece paralellik](https://technet.microsoft.com/library/ms188611.aspx) SQL Server'da. Performansı iyileştirmek için uygulamanızı ve yapılandırmalarına çoklu iş parçacığı etkileyen gibi ayarları bulmak.

## <a name="queue-depth"></a>Sıra Derinliği
Sıra Derinliği veya sıra uzunluğu veya kuyruk boyutu bekleyen g/ç istekleri sistemde sayısıdır. Sıra Derinliği değeri, uygulamanızın hangi depolama disklerini işlenmeye, satır kaç GÇ işlemleri belirler. Bu, bu makalede viz. içinde IOPS, üretilen işi ve gecikme süresi ele aldığımız tüm üç uygulama performans göstergelerini etkiler.

Derinlik ve çoklu iş parçacığı olan kuyruk yakından ilgili. Sıra Derinliği değeri çoklu iş parçacığı ne kadar can gösterir uygulama tarafından elde edilebilir. Sıra Derinliği büyükse, uygulama daha fazla işlem eşzamanlı olarak, diğer bir deyişle, yürütebilir daha çok iş parçacığı. Sıra Derinliği küçük ise, uygulamanın çok iş parçacıklı olsa bile yeterli istekleri için eş zamanlı yürütme sıralanmalıdır olmaz.

Genellikle, raf devre dışı uygulamaları sıra derinliğini değiştirmenizi olduğundan izin verme, hatalı bunu daha iyi daha fazla zarar ayarlayın. Uygulamaları sıra derinliğini, en iyi performansı elde etmek için doğru değerini ayarlar. Ancak, böylece uygulamanız ile performans sorunlarını giderebilirsiniz bu kavramı anlamak önemlidir. Ayrıca, sisteminizde Kıyaslama Araçlar çalıştırarak sıra derinliğini etkilerini gözlemleyebilirsiniz.

Bazı uygulamalar, sıra derinliğini etkilemek için ayarları belirtin. Örneğin, SQL Server MAXDOP (maksimum paralellik derecesi) ayarı, önceki bölümde açıklanmıştır. MAXDOP sıra derinliğini etkilemek için bir yoldur ve çoklu iş parçacığı, ancak sıra derinliği değeri SQL Server'ın doğrudan değiştirmez.

*Yüksek sıra derinliği*  
Disk üzerinde daha fazla işlemleri'kurmak yüksek sıra derinliğini satırlar. Disk önceden kuyruğunu sonraki istekte bilir. Sonuç olarak, disk işlemleri önceden zamanlayabilir ve bunları en iyi dizisi işleyebilirsiniz. Uygulama disk için daha fazla istek gönderiyor olduğundan, diskin daha fazla paralel IOs işleyebilir. Sonuç olarak, uygulama daha yüksek IOPS elde etmek mümkün olacaktır. Uygulama, daha fazla istekleri işliyor olduğundan, uygulamanın toplam aktarım hızı da artar.

Genellikle, uygulamaya 8 ile en fazla aktarım hızı elde edebilirsiniz-16 + bekleyen IOs ekli disk başına. Sıra Derinliği ise, uygulama yeterli IOs sisteme gönderme değil ve belirli bir dönemde daha az miktarda işleyecektir. Diğer bir deyişle, daha az aktarım hızı.

Örneğin, SQL Server'da "4" için bir sorgu için MAXDOP değeri ayarı SQL Server, en fazla dört çekirdek sorguyu kullanabilirsiniz bildirir. SQL Server en iyi sıra derinliği değeri ve sorgu yürütme için çekirdek sayısı ne olduğunu belirler.

*En iyi sıra derinliğini*  
Çok yüksek sıra derinliği değeri, ayrıca kendi engelleri vardır. Sıra Derinliği değeri çok yüksekse, uygulamanın çok yüksek IOPS sürücü dener. Uygulama yeterli sağlanan IOPS ile kalıcı disk olmadığı sürece, bu uygulama gecikme sürelerini olumsuz yönde etkileyebilir. Aşağıdaki formül, IOPS, gecikme süresi ve sıra derinliğini arasındaki ilişkiyi gösterir.  
    ![](media/premium-storage-performance/image6.png)

Yüksek bir değer, ancak uygulama için yeterli IOPS gecikmeleri etkilemeden sunabilirsiniz en uygun bir değer sıra derinliğini yapılandırmamalısınız. Örneğin, uygulama gecikmesi 1 milisaniyelik olması gerekiyorsa, sıra derinliğini 5.000 IOPS elde etmek için gerekli olan QD = 5000 x 0,001 = 5.

*Şeritli birim için sıra derinliği*  
Şeritli birim için her diski ayrı ayrı en yüksek sıra derinliğini sahip olacak şekilde, bir yeterince yüksek sıra derinliğini korumak. Örneğin, bir sıra derinliği / 2 gönderen bir uygulama düşünün ve bir Şerit içine 4 diskler vardır. İki g/ç isteği için iki disk geçer ve iki disk kalan boş olacaktır. Bu nedenle, tüm disklerin meşgul olacak şekilde sıra derinliğini yapılandırın. Aşağıdaki formülü şeritli birimler sıra derinliğini belirlemek nasıl gösterir.  
    ![](media/premium-storage-performance/image7.png)

## <a name="throttling"></a>Azaltma
Azure Premium depolama hükümlerinin IOPS ve aktarım hızı sayısı VM boyutları ve seçtiğiniz disk boyutları bağlı olarak belirtilen. IOPS veya aktarım hızı ne VM veya disk başa çıkabilir, bu sınırların üzerinde sürücü uygulamanız çalıştığında zaman, Premium depolama, kısıtlama. Bu, uygulamanızdaki performans biçiminde bildirimleri. Bu daha yüksek gecikme süresi anlamına gelir, daha düşük aktarım hızı veya IOPS azaltın. Premium depolama azaltma değil, uygulamanızı tamamen kaynaklarını elde edin özellikli nelerdir aşılan başarısız olabilir. Bu nedenle, kısıtlama nedeniyle performans sorunlarından kaçınmak için her zaman uygulamanız için yeterli kaynakları sağlayın. VM boyutları ve Disk boyutları bölümlerde yukarıdaki ne Bahsettiğimiz dikkate alın. Değerlendirmesi, uygulamanızı barındırmak için ihtiyacınız olacak hangi kaynaklara anlamak için en iyi bir yoludur.

## <a name="benchmarking"></a>Değerlendirmesi
Değerlendirmesi, uygulamanızdaki farklı iş yükleri benzetimi ve her iş yükü için uygulama performansını ölçme işlemidir. Bir önceki bölümde açıklanan adımları kullanarak, uygulama performans gereksinimleri aldınız. Uygulama barındırma Vm'lerde Kıyaslama Araçlar çalıştırarak uygulamanızı Premium depolama ile ulaşabilir performans düzeylerini belirleyebilirsiniz. Bu bölümde, Azure Premium depolama diskleri ile sağlanan standart DS14 VM değerlendirmesi örnekleri sunuyoruz.

Ortak Kıyaslama araçları Iometer ve FIO, Windows ve Linux için sırasıyla kullandık. Bu araçlar, bir üretim iş yükü gibi benzetimi birden çok iş parçacığı oluşturma ve sistem performansına ölçen. Araçları kullanarak, bir uygulama için normalde değiştiremezsiniz blok boyutu ve sıra derinliğini gibi parametreleri de yapılandırabilirsiniz. Bu, farklı uygulama iş yükleri için premium disklere sahip VM'de yüksek ölçekte en yüksek performansı sürücü daha fazla esneklik sağlar. Ziyaret Kıyaslama her araç hakkında daha fazla bilgi edinmek için [Iometer](http://www.iometer.org/) ve [FIO](http://freecode.com/projects/fio).

Aşağıdaki örneklerde takip etmek için standart DS14 VM oluşturma ve 11 Premium depolama diskleri VM'e ekleyin. 11 disklerin "None" önbelleğe alma konakla 10 diskleri yapılandırın ve bunları NoCacheWrites adlı bir birime stripe. "Salt okunur olarak" kalan disk üzerinde önbelleğe alma konak yapılandırın ve bu diskle CacheReads adlı bir birim oluşturun. Bu ayarı kullanarak, bir standart DS14 VM'den en yüksek okuma ve yazma performansını görmek mümkün olacaktır. Premium diskler sayesinde DS14 VM oluşturma ile ilgili ayrıntılı adımlar için Git [oluşturma ve bir Premium depolama hesabı için sanal makine veri diski kullanma](../articles/virtual-machines/windows/premium-storage.md).

*Önbelleği hazırlanıyor*  
Salt okunur konak önbelleği disk disk sınırdan daha yüksek IOPS vermek mümkün olacaktır. Öncelikle bu en yüksek okuma performansı ana bilgisayar önbelleğe almak için bu disk önbelleği sıcak gerekir. Bu, hangi Kıyaslama aracı CacheReads birimde artıracak okuma IOs gerçekten önbellek ve diskin değil doğrudan ulaştığını sağlar. Önbellek isabet sayısı sonucu tek önbellekten ek IOPS disk etkin.

> **Önemli:**  
> VM her yeniden değerlendirmesi, çalıştırmadan önce önbelleği sıcak gerekir.
>
>

#### <a name="iometer"></a>Iometer
[Iometer Aracı'nı indirme](http://sourceforge.net/projects/iometer/files/iometer-stable/2006-07-27/iometer-2006.07.27.win32.i386-setup.exe/download) VM üzerinde.

*Test dosyası*  
Iometer Kıyaslama test çalıştırılacağı biriminde depolanan bir test dosyası kullanır. Okuma sürücü ve disk ölçmek için bu test dosyası üzerinde IOPS ve aktarım hızı yazar. Bir sağlamadınız Iometer bu test dosyası oluşturur. İobw.tst CacheReads ve NoCacheWrites birimlere adlı bir 200 GB test dosyası oluşturun.

*Erişim belirtimleri*  
Belirtimleri isteği % okuma/yazma g/ç boyutu, % rastgele/sıralı yapılandırılmış Iometer içinde "Access belirtimleri" sekmesini kullanarak. Aşağıda açıklanan senaryoların her biri için bir erişim belirtimini oluşturun. Erişim belirtimleri oluşturun ve "Kaydet" uygun bir ad gibi – RandomWrites\_8K, RandomReads\_8 K. Test senaryosu çalıştırırken karşılık gelen belirtimi seçin.

Maksimum IOPS, yazma senaryosu için erişim belirtimleri örneği aşağıda gösterilmiştir,  
    ![](media/premium-storage-performance/image8.png)

*Maksimum IOPS Test özellikleri*  
Maksimum IOPS göstermek için daha küçük istek boyutu kullanın. 8 K istek boyutu ve rastgele yazma ve okuma özellikleri oluşturun.

| Erişim belirtimi | İstek boyutu | Rastgele % | Okuma % |
| --- | --- | --- | --- |
| RandomWrites\_8 K |8K |100 |0 |
| RandomReads\_8 K |8K |100 |100 |

*En fazla aktarım hızı testi belirtimleri*  
En yüksek aktarım göstermek için istek boyutu daha büyük kullanın. 64 K istek boyutu ve rastgele yazma ve okuma özellikleri oluşturun.

| Erişim belirtimi | İstek boyutu | Rastgele % | Okuma % |
| --- | --- | --- | --- |
| RandomWrites\_64 K |64K |100 |0 |
| RandomReads\_64 K |64K |100 |100 |

*Iometer testi çalıştırma*  
Önbelleği sıcak için aşağıdaki adımları gerçekleştirin

1. Aşağıda gösterilen değerlerle iki erişim belirtimleri oluşturun

   | Ad | İstek boyutu | Rastgele % | Okuma % |
   | --- | --- | --- | --- |
   | RandomWrites\_1 MB |1MB |100 |0 |
   | RandomReads\_1 MB |1MB |100 |100 |
1. Aşağıdaki parametrelerle önbellek diski başlatma için Iometer testi çalıştırın. Hedef birim ve 128 sıra derinliği için üç çalışan iş parçacıkları kullanın. Test "çalışma zamanı" süresini 2hrs için "Test" Kurulum"sekmesinde belirleyin.

   | Senaryo | Hedef birim | Ad | Süre |
   | --- | --- | --- | --- |
   | Önbellek diski başlatın |CacheReads |RandomWrites\_1 MB |2hrs |
1. Önbellek diski aşağıdaki parametrelerle hazırlanıyor için Iometer testi çalıştırın. Hedef birim ve 128 sıra derinliği için üç çalışan iş parçacıkları kullanın. Test "çalışma zamanı" süresini 2hrs için "Test" Kurulum"sekmesinde belirleyin.

   | Senaryo | Hedef birim | Ad | Süre |
   | --- | --- | --- | --- |
   | Önbellek diski ısınıyor |CacheReads |RandomReads\_1 MB |2hrs |

Önbellek diski warmed sonra aşağıda listelenen test senaryoları ile devam edin. Iometer testi çalıştırmak için en az üç çalışan iş parçacıkları için kullanın. **her** hedef birim. Her iş parçacığı için hedef birimi seçin, sıra derinliğini ayarlayın ve karşılık gelen test senaryosu çalıştırmak için aşağıdaki tabloda gösterildiği gibi kaydedilmiş test belirtimleri birini seçin. Tabloda ayrıca beklenen sonuçları IOPS ve aktarım hızı için bu testleri çalıştırırken gösterir. Tüm senaryolar için 8 KB'lık bir küçük g/ç boyutu ve 128 yüksek sıra derinliğini kullanılır.

| Test senaryosu | Hedef birim | Ad | Sonuç |
| --- | --- | --- | --- |
| En çok, Okuma IOPS |CacheReads |RandomWrites\_8 K |50.000 IOPS |
| En çok, Yazma IOPS |NoCacheWrites |RandomReads\_8 K |64.000 IOPS |
| En çok, Toplam IOPS |CacheReads |RandomWrites\_8 K |100.000 IOPS |
| NoCacheWrites |RandomReads\_8 K | &nbsp; | &nbsp; |
| En çok, Okuma MB/sn |CacheReads |RandomWrites\_64 K |524 MB/sn |
| En çok, Yazma MB/sn |NoCacheWrites |RandomReads\_64 K |524 MB/sn |
| Birleşik MB/sn |CacheReads |RandomWrites\_64 K |1000 MB/sn |
| NoCacheWrites |RandomReads\_64 K | &nbsp; | &nbsp; |

Iometer ekran görüntüleri için toplam IOPS ve aktarım hızı senaryoları test sonuçları aşağıda verilmiştir.

*Birleşik okuma ve yazma işlemleri maksimum IOPS*  
![](media/premium-storage-performance/image9.png)

*Birleşik okuma ve yazma işlemleri, en fazla aktarım hızı*  
![](media/premium-storage-performance/image10.png)

### <a name="fio"></a>FIO
FIO Linux vm'lerinde Kıyaslama depolama için popüler bir araçtır. Bu farklı GÇ boyutları, sıralı seçmek için esneklik veya rastgele okuma ve yazma. Çalışan iş parçacıkları veya belirtilen g/ç işlemleri gerçekleştirmek için işlemleri olarak çoğaltılır. Her iş parçacığı proje dosyalarını kullanarak gerçekleştirmelidir g/ç işlemleri türünü belirtebilirsiniz. Aşağıdaki örneklerde gösterildiği senaryo başına bir proje dosyası oluşturduk. Premium depolama alanında çalışan farklı iş yüklerini karşılaştırılmasıdır bu proje dosyalarındaki belirtimleri değiştirebilirsiniz. Örneklerde standart DS 14 sanal makine çalıştırma kullanıyoruz **Ubuntu**. Başında açıklanan aynı kurulumunda [bölümü değerlendirmesi](#Benchmarking) ve Kıyaslama testleri çalıştırmadan önce önbellek ısınıyor.

Başlamadan önce [FIO indirme](https://github.com/axboe/fio) ve sanal makinenize yükleyin.

Ubuntu için aşağıdaki komutu çalıştırın

```
apt-get install fio
```

Yazma işlemleri kullanımını dört çalışan iş parçacıkları ve dört çalışan iş parçacıkları disklerde sürüş okuma işlemleri için kullanacağız. Yazma çalışanlar, önbellek "Yok" olarak ayarlanmış olan 10 diskler olduğu "nocache" birim trafiği yönlendiren. Okuma çalışanlar, sahip 1 diski önbellek kümesi "Salt okunur" "readcache" birim trafiği yönlendiren.

*Maksimum yazma IOPS*  
Maksimum yazma IOPS almak için aşağıdaki özelliklerle iş dosyasını oluşturun. "Fiowrite.ini" olarak adlandırın.

```
[global]
size=30g
direct=1
iodepth=256
ioengine=libaio
bs=8k

[writer1]
rw=randwrite
directory=/mnt/nocache
[writer2]
rw=randwrite
directory=/mnt/nocache
[writer3]
rw=randwrite
directory=/mnt/nocache
[writer4]
rw=randwrite
directory=/mnt/nocache
```

Önceki bölümlerde ele alınan tasarım ilkelerine uygun olarak izleme önemli şey unutmayın. Bu belirtimler maksimum IOPS desteklemek üzere gerekli olan,  

* 256 yüksek sıra derinliği.  
* 8 KB'lık bir küçük blok boyutu.  
* Birden çok iş parçacığı rastgele yazma işlemlerini gerçekleştirme.

30 saniye için FIO test hız kazandırın için aşağıdaki komutu çalıştırın,  

```
sudo fio --runtime 30 fiowrite.ini
```

Test çalışırken, sayısını, VM IOPS yazma ve Premium diskleri teslim görmek mümkün olacaktır. Aşağıdaki örnekte gösterildiği gibi maksimum, yazma IOPS sınırı 50.000 IOPS DS14 VM sağlıyor.  
    ![](media/premium-storage-performance/image11.png)

*En yüksek okuma IOPS*  
Proje dosyası, maksimum okuma IOPS almak için aşağıdaki özelliklerle oluşturun. "Fioread.ini" olarak adlandırın.

```
[global]
size=30g
direct=1
iodepth=256
ioengine=libaio
bs=8k

[reader1]
rw=randread
directory=/mnt/readcache
[reader2]
rw=randread
directory=/mnt/readcache
[reader3]
rw=randread
directory=/mnt/readcache
[reader4]
rw=randread
directory=/mnt/readcache
```

Önceki bölümlerde ele alınan tasarım ilkelerine uygun olarak izleme önemli şey unutmayın. Bu belirtimler maksimum IOPS desteklemek üzere gerekli olan,

* 256 yüksek sıra derinliği.  
* 8 KB'lık bir küçük blok boyutu.  
* Birden çok iş parçacığı rastgele yazma işlemlerini gerçekleştirme.

30 saniye için FIO test hız kazandırın için aşağıdaki komutu çalıştırın,

```
sudo fio --runtime 30 fioread.ini
```

Test çalışırken, VM okuma IOPS sayısı ve Premium diskleri teslim görmek mümkün olacaktır. Aşağıdaki örnekte gösterildiği gibi birden fazla 64.000 okuma IOPS DS14 VM sağlıyor. Disk ve önbellek performansı budur.  
    ![](media/premium-storage-performance/image12.png)

*En yüksek okuma ve yazma IOPS*  
Şunlarla birlikte proje dosyasını oluşturmak en fazla almak için özellikleri birleştirilmiş okuma ve yazma IOPS. "Fioreadwrite.ini" olarak adlandırın.

```
[global]
size=30g
direct=1
iodepth=128
ioengine=libaio
bs=4k

[reader1]
rw=randread
directory=/mnt/readcache
[reader2]
rw=randread
directory=/mnt/readcache
[reader3]
rw=randread
directory=/mnt/readcache
[reader4]
rw=randread
directory=/mnt/readcache

[writer1]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
[writer2]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
[writer3]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
[writer4]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
```

Önceki bölümlerde ele alınan tasarım ilkelerine uygun olarak izleme önemli şey unutmayın. Bu belirtimler maksimum IOPS desteklemek üzere gerekli olan,

* Yüksek sıra derinliğini 128.  
* 4KB'lık bir küçük blok boyutu.  
* Birden çok iş parçacığı rastgele gerçekleştirme okur ve yazar.

30 saniye için FIO test hız kazandırın için aşağıdaki komutu çalıştırın,

```
sudo fio --runtime 30 fioreadwrite.ini
```

Test çalışırken, birleşik okuma sayısı görebilir ve IOPS yazmak VM olacak ve Premium diskler sunma. Aşağıdaki örnekte gösterildiği gibi DS14 VM 100. 000'den fazla birleştirilmiş okuma ve yazma IOPS sağlıyor. Disk ve önbellek performansı budur.  
    ![](media/premium-storage-performance/image13.png)

*En fazla aktarım hızı birleştirilmiş*  
Okuma ve yazma aktarım hızı üst sınırı almak için birleştirilmiş, daha büyük blok boyutu ve büyük sıra derinliğini okuma ve yazma işlemleri gerçekleştiren birden çok iş parçacığı ile kullanın. Bir blok boyutu 64 KB'ın ve 128 sıra derinliğini kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki Adımlar
Azure Premium depolama hakkında daha fazla bilgi edinin:

* [Premium Depolama: Azure Sanal Makine İş Yükleri için Yüksek Performanslı Depolama](../articles/virtual-machines/windows/premium-storage.md)  

SQL Server kullanıcıları için SQL Server için en iyi performans uygulamaları makalelerini okuyun:

* [Azure sanal makineler'de SQL Server için en iyi performans](../articles/virtual-machines/windows/sql/virtual-machines-windows-sql-performance.md)
* [Azure Premium depolama, Azure VM'deki SQL Server için yüksek performans sağlar.](http://blogs.technet.com/b/dataplatforminsider/archive/2015/04/23/azure-premium-storage-provides-highest-performance-for-sql-server-in-azure-vm.aspx)
