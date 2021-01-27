---
title: Azure Storage ile AzCopy 'i yapılandırma, iyileştirme ve sorunlarını giderme | Microsoft Docs
description: Azure depolama ile AzCopy 'i yapılandırın, iyileştirin ve sorun giderin. Planı ve günlük dosyalarını değiştirin veya kaldırın. Varsayılan günlük düzeyini değiştirin.
author: normesta
ms.service: storage
ms.topic: how-to
ms.date: 07/27/2020
ms.author: normesta
ms.subservice: common
ms.reviewer: dineshm
ms.openlocfilehash: 1a319c728b918dbad7dd5f240bc7a0bfeb0c4c09
ms.sourcegitcommit: aaa65bd769eb2e234e42cfb07d7d459a2cc273ab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/27/2021
ms.locfileid: "98881053"
---
# <a name="configure-optimize-and-troubleshoot-azcopy"></a>AzCopy 'i yapılandırma, iyileştirme ve sorun giderme

AzCopy, bir depolama hesabına iki yönlü blob veya dosya kopyalama işlemi gerçekleştirmenizi sağlayan bir komut satırı yardımcı programıdır. Bu makale, gelişmiş yapılandırma görevleri gerçekleştirmenize ve AzCopy kullanırken karşılaşabileceğiniz sorunları gidermenize yardımcı olur.

> [!NOTE]
> AzCopy kullanmaya başlamanıza yardımcı olacak içerik arıyorsanız, aşağıdaki makalelerden birine bakın:
> - [AzCopy’yi kullanmaya başlama](storage-use-azcopy-v10.md)
> - [AzCopy ve BLOB Storage ile veri aktarma](./storage-use-azcopy-v10.md#transfer-data)
> - [AzCopy ve dosya depolama ile verileri aktarma](storage-use-azcopy-files.md)
> - [AzCopy ve Amazon S3 demetleri ile veri aktarma](storage-use-azcopy-s3.md)

## <a name="configure-proxy-settings"></a>Ara sunucu ayarlarını yapılandırma

AzCopy ara sunucu ayarlarını yapılandırmak için `HTTPS_PROXY` ortam değişkenini ayarlayın. AZCopy’yi Windows’da çalıştırıyorsanız AzCopy ara sunucu ayarlarını otomatik olarak algılar, bu nedenle bu ayarı Windows’da kullanmanız gerekmez. Bu ayarı Windows’da kullanmayı seçerseniz otomatik algılama geçersiz kılınır.

| İşletim sistemi | Komut  |
|--------|-----------|
| **Windows** | Komut isteminde şunu kullanın: `set HTTPS_PROXY=<proxy IP>:<proxy port>`<br> PowerShell kullanımı: `$env:HTTPS_PROXY="<proxy IP>:<proxy port>"`|
| **Linux** | `export HTTPS_PROXY=<proxy IP>:<proxy port>` |
| **macOS** | `export HTTPS_PROXY=<proxy IP>:<proxy port>` |

Şu anda AzCopy, NTLM veya Kerberos ile kimlik doğrulaması gerektiren proxy 'leri desteklemiyor.

### <a name="bypassing-a-proxy"></a>Proxy atlama ###

Windows üzerinde AzCopy çalıştırıyorsanız ve bu sunucuya _hiçbir_ proxy (ayarları otomatik olarak algılama yerine) kullanmayı söylemek istiyorsanız bu komutları kullanın. Bu ayarlarla AzCopy, herhangi bir proxy 'yi aramaz veya kullanmayı denemez.

| İşletim sistemi | Ortam | Komutlar  |
|--------|-----------|----------|
| **Windows** | Komut istemi (CMD) | `set HTTPS_PROXY=dummy.invalid` <br>`set NO_PROXY=*`|
| **Windows** | PowerShell | `$env:HTTPS_PROXY="dummy.invalid"` <br>`$env:NO_PROXY="*"`<br>|

Diğer işletim sistemlerinde, ara sunucu kullanmak istiyorsanız HTTPS_PROXY değişkenini yok etmeniz yeterlidir.

## <a name="optimize-performance"></a>Performansı iyileştirme

Performans ve kaynak tüketimine ilişkin en iyi zorunluluğunu getirir bulmak için, performansı ve ardından komut ve ortam değişkenlerini kullanabilirsiniz.

Bu bölüm, bu iyileştirme görevlerini gerçekleştirmenize yardımcı olur:

> [!div class="checklist"]
> * Kıyaslama testlerini Çalıştır
> * Aktarım hızını iyileştirme
> * Bellek kullanımını iyileştirme 
> * Dosya eşitlemesini iyileştirme

### <a name="run-benchmark-tests"></a>Kıyaslama testlerini Çalıştır

Genel performans istatistiklerini görüntülemek ve kimlik performans sorunlarını gidermek için, belirli blob kapsayıcıları veya dosya paylaşımlarında performans kıyaslama testini çalıştırabilirsiniz. Oluşturulan test verilerini karşıya yükleyerek veya indirerek testi çalıştırabilirsiniz. 

Bir performans kıyaslama testi çalıştırmak için aşağıdaki komutu kullanın.

|    |     |
|--------|-----------|
| **Syntax** | `azcopy benchmark 'https://<storage-account-name>.blob.core.windows.net/<container-name>'` |
| **Örnek** | `azcopy benchmark 'https://mystorageaccount.blob.core.windows.net/mycontainer/myBlobDirectory?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D'` |

> [!TIP]
> Bu örnek tek tırnak (' ') ile yol bağımsız değişkenlerini barındırır. Windows komut kabuğu (cmd.exe) hariç tüm komut kabukta tek tırnak işaretleri kullanın. Bir Windows komut kabuğu (cmd.exe) kullanıyorsanız, yol bağımsız değişkenlerini tek tırnak (' ') yerine çift tırnak işareti ("") içine alın.

Bu komut test verilerini belirtilen bir hedefe yükleyerek bir performans kıyaslaması çalıştırır. Test verileri bellekte oluşturulur, hedefe yüklenir ve ardından test tamamlandıktan sonra hedefteki silinir. İsteğe bağlı komut parametrelerini kullanarak oluşturulacak dosya sayısını ve bunların kaç boyutunu istediğinizi belirtebilirsiniz.

Bu testi verileri indirerek çalıştırmayı tercih ederseniz `mode` parametresini olarak ayarlayın `download` . Ayrıntılı başvuru belgeleri için bkz. [AzCopy kıyaslaması](storage-ref-azcopy-bench.md). 

### <a name="optimize-throughput"></a>Aktarım hızını iyileştirme

`cap-mbps`İş hızı veri hızına bir tavan koymak için komutlarınızın bayrağını kullanabilirsiniz. Örneğin, aşağıdaki komut bir işi sürdürür ve `10` saniye başına megabit (MB) cinsinden bir işleme devam eder. 

```azcopy
azcopy jobs resume <job-id> --cap-mbps 10
```

Aktarım hızı küçük dosyalar aktarılırken düşebilir. Ortam değişkenini ayarlayarak aktarım hızını artırabilirsiniz `AZCOPY_CONCURRENCY_VALUE` . Bu değişken gerçekleşebilecek eşzamanlı istek sayısını belirtir.  

Bilgisayarınızda 5 ' ten az CPU varsa, bu değişkenin değeri olarak ayarlanır `32` . CPU dana fazlaysa, varsayılan değer CPU sayısının 16 katına eşittir. Bu değişkenin en büyük varsayılan değeri `3000` , ancak bu değeri el ile veya daha düşük bir şekilde ayarlayabilirsiniz. 

| İşletim sistemi | Komut  |
|--------|-----------|
| **Windows** | `set AZCOPY_CONCURRENCY_VALUE=<value>` |
| **Linux** | `export AZCOPY_CONCURRENCY_VALUE=<value>` |
| **macOS** | `export AZCOPY_CONCURRENCY_VALUE=<value>` |

`azcopy env`Bu değişkenin geçerli değerini denetlemek için öğesini kullanın. Değer boşsa, herhangi bir AzCopy günlük dosyasının başlangıcına bakarak hangi değerin kullanıldığını okuyabilirsiniz. Seçili değer ve seçildiği neden burada raporlanır.

Bu değişkeni ayarlamadan önce, bir kıyaslama testi çalıştırmanızı öneririz. Kıyaslama test süreci, önerilen eşzamanlılık değerini rapor eder. Alternatif olarak, ağ koşullarınız ve yüklerinizin farklılık göstermesi durumunda bu değişkeni `AUTO` belirli bir sayı yerine kelimeyle ayarlayın. Bu, AzCopy 'in kıyaslama testlerinde kullandığı otomatik ayarlama işlemini her zaman çalıştırmasına neden olur.

### <a name="optimize-memory-use"></a>Bellek kullanımını iyileştirme

`AZCOPY_BUFFER_GB`Dosya indirme ve karşıya yükleme sırasında AzCopy 'in kullanmasını istediğiniz en fazla sistem belleği miktarını belirtmek için ortam değişkenini ayarlayın.
Bu değeri gigabayt (GB) cinsinden ifade edin.

| İşletim sistemi | Komut  |
|--------|-----------|
| **Windows** | `set AZCOPY_BUFFER_GB=<value>` |
| **Linux** | `export AZCOPY_BUFFER_GB=<value>` |
| **macOS** | `export AZCOPY_BUFFER_GB=<value>` |

### <a name="optimize-file-synchronization"></a>Dosya eşitlemesini iyileştirme

[Eşitleme](storage-ref-azcopy-sync.md) komutu hedefteki tüm dosyaları tanımlar ve ardından eşitleme işlemine başlamadan önce dosya adlarını ve son değiştirilme zaman damgalarını karşılaştırır. Çok sayıda dosya varsa, bu ön işlemden yararlanarak performansı artırabilirsiniz. 

Bunu gerçekleştirmek için, bunun yerine [AzCopy kopyalama](storage-ref-azcopy-copy.md) komutunu kullanın ve `--overwrite` bayrağını olarak ayarlayın `ifSourceNewer` . AzCopy, dosyaları, ön tarama ve karşılaştırmalar yapılmadan kopyalandıkları gibi karşılaştıracaktır. Bu, Karşılaştırılacak çok sayıda dosya olduğu durumlarda bir performans kenarı sağlar.

[AzCopy kopyalama](storage-ref-azcopy-copy.md) komutu, hedefteki dosyaları silmez, bu nedenle hedefteki dosyaları kaynak üzerinde olmayan bir şekilde silmek isterseniz [AzCopy Sync](storage-ref-azcopy-sync.md) komutunu `--delete-destination` bayrağıyla veya değerine ayarlanmış şekilde kullanın `true` `prompt` . 

## <a name="troubleshoot-issues"></a>Sorunları giderme

AzCopy, her bir iş için günlük ve plan dosyaları oluşturur. Günlükleri kullanarak olası sorunları araştırabilir ve giderebilirsiniz. 

Günlükler hatanın durumunu ( `UPLOADFAILED` , `COPYFAILED` , ve `DOWNLOADFAILED` ), tam yolu ve hatanın nedenini içerecektir.

Varsayılan olarak, günlük ve plan dosyaları `%USERPROFILE%\.azcopy` `$HOME$\.azcopy` Mac ve Linux üzerindeki Windows veya dizindeki dizinde bulunur, ancak isterseniz bu konumu değiştirebilirsiniz.

İlgili hata, dosyada görüntülenen ilk hata değildir. Ağ hataları, zaman aşımları ve sunucu meşgul hataları gibi hatalar için AzCopy en fazla 20 kez yeniden dener ve genellikle yeniden deneme işlemi başarılı olur.  Gördüğünüz ilk hata, başarıyla yeniden denenen bir zararsız olabilir.  Bu nedenle, dosyadaki ilk hataya bakmak yerine, veya yakınındaki hataları arayın `UPLOADFAILED` `COPYFAILED` `DOWNLOADFAILED` . 

> [!IMPORTANT]
> Microsoft Desteği bir istek gönderirken (veya herhangi bir üçüncü taraf ile ilgili sorunu gidermeye çalıştığınızda), yürütmek istediğiniz komutun Redaksiyonu yapılmış sürümünü paylaşabilirsiniz. Bu, SAS 'nin yanlışlıkla herhangi bir gövdele paylaşılmamasını sağlar. Redaksiyonu yapılmış sürümü günlük dosyasının başlangıcında bulabilirsiniz.

### <a name="review-the-logs-for-errors"></a>Günlükleri hatalara karşı gözden geçirin

Aşağıdaki komut, günlükteki durum ile tüm hataları alacak `UPLOADFAILED` `04dc9ca9-158f-7945-5933-564021086c79` :

**Windows (PowerShell)**

```
Select-String UPLOADFAILED .\04dc9ca9-158f-7945-5933-564021086c79.log
```

**Linux**

```
grep UPLOADFAILED .\04dc9ca9-158f-7945-5933-564021086c79.log
```

### <a name="view-and-resume-jobs"></a>İşleri görüntüleme ve sürdürme

Her aktarım işlemi bir AzCopy işi oluşturur. İşlerin geçmişini görüntülemek için aşağıdaki komutu kullanın:

```
azcopy jobs list
```

İş istatistiklerini görüntülemek için aşağıdaki komutu kullanın:

```
azcopy jobs show <job-id>
```

Aktarımları duruma göre filtrelemek için aşağıdaki komutu kullanın:

```
azcopy jobs show <job-id> --with-status=Failed
```

Başarısız/iptal edildi bir işi yeniden başlatmak için aşağıdaki komutu kullanın. Bu komut, güvenlik nedenleriyle kalıcı olmadığından, kimliğini SAS belirteciyle birlikte kullanır:

```
azcopy jobs resume <job-id> --source-sas="<sas-token>"
azcopy jobs resume <job-id> --destination-sas="<sas-token>"
```

> [!TIP]
> SAS belirteci gibi yol bağımsız değişkenlerini tek tırnak işareti (' ') ile çevrele. Windows komut kabuğu (cmd.exe) hariç tüm komut kabukta tek tırnak işaretleri kullanın. Bir Windows komut kabuğu (cmd.exe) kullanıyorsanız, yol bağımsız değişkenlerini tek tırnak (' ') yerine çift tırnak işareti ("") içine alın.

Bir işi sürdürürseniz AzCopy iş planı dosyasına bakar. Plan dosyası, iş ilk oluşturulduğunda işleme için tanımlanan tüm dosyaları listeler. Bir işi sürdürürseniz AzCopy, önceden aktarılmayan plan dosyasında listelenen tüm dosyaları aktarmaya çalışır.

## <a name="change-the-location-of-the-plan-and-log-files"></a>Planın ve günlük dosyalarının konumunu değiştirme

Varsayılan olarak, plan ve günlük dosyaları `%USERPROFILE%\.azcopy` Windows veya `$HOME$\.azcopy` Mac ve Linux dizinindeki dizinde bulunur. Bu konumu değiştirebilirsiniz.

### <a name="change-the-location-of-plan-files"></a>Plan dosyalarının konumunu değiştirme

Bu komutlardan herhangi birini kullanın.

| İşletim sistemi | Komut  |
|--------|-----------|
| **Windows** | PowerShell`$env:AZCOPY_JOB_PLAN_LOCATION="<value>"` <br> Komut isteminde şunu kullanın:: `set AZCOPY_JOB_PLAN_LOCATION=<value>` |
| **Linux** | `export AZCOPY_JOB_PLAN_LOCATION=<value>` |
| **macOS** | `export AZCOPY_JOB_PLAN_LOCATION=<value>` |

`azcopy env`Bu değişkenin geçerli değerini denetlemek için öğesini kullanın. Değer boşsa, plan dosyaları varsayılan konuma yazılır.

### <a name="change-the-location-of-log-files"></a>Günlük dosyalarının konumunu değiştirme

Bu komutlardan herhangi birini kullanın.

| İşletim sistemi | Komut  |
|--------|-----------|
| **Windows** | PowerShell`$env:AZCOPY_LOG_LOCATION="<value>"` <br> Komut isteminde şunu kullanın:: `set AZCOPY_LOG_LOCATION=<value>`|
| **Linux** | `export AZCOPY_LOG_LOCATION=<value>` |
| **macOS** | `export AZCOPY_LOG_LOCATION=<value>` |

`azcopy env`Bu değişkenin geçerli değerini denetlemek için öğesini kullanın. Değer boşsa, günlükler varsayılan konuma yazılır.

## <a name="change-the-default-log-level"></a>Varsayılan günlük düzeyini değiştirme

Varsayılan olarak, AzCopy günlük düzeyi olarak ayarlanır `INFO` . Disk alanından tasarruf etmek için günlük ayrıntı düzeyini azaltmak isterseniz, seçeneğini kullanarak bu ayarın üzerine yazın ``--log-level`` . 

Kullanılabilir günlük düzeyleri şunlardır: `NONE` , `DEBUG` , `INFO` , `WARNING` , `ERROR` , `PANIC` , ve `FATAL` .

## <a name="remove-plan-and-log-files"></a>Planı ve günlük dosyalarını kaldır

Disk alanını kazanmak için yerel makinenizden tüm planı ve günlük dosyalarını kaldırmak istiyorsanız `azcopy jobs clean` komutunu kullanın.

Yalnızca bir işle ilişkili planı ve günlük dosyalarını kaldırmak için kullanın `azcopy jobs rm <job-id>` . `<job-id>`Bu örnekteki yer tutucuyu işin iş kimliğiyle değiştirin.
