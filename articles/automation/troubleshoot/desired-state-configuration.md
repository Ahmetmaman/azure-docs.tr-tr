---
title: İle Azure Otomasyonu Desired State Configuration (DSC) ile ilgili sorunları giderme
description: Bu makale Desired State Configuration ' nı (DSC) sorun giderme hakkında bilgi sağlar.
services: automation
ms.service: automation
ms.subservice: ''
author: georgewallace
ms.author: gwallace
ms.date: 06/19/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: a34dea7e1eb53531db55dc62df8fbad8541f7a35
ms.sourcegitcommit: 7723b13601429fe8ce101395b7e47831043b970b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2019
ms.locfileid: "56586809"
---
# <a name="troubleshoot-desired-state-configuration-dsc"></a>Desired State Configuration (DSC) sorunlarını giderme

Bu makalede, sorun giderme konuları Desired State Configuration (DSC) ile hakkında bilgi sağlar.

## <a name="common-errors-when-working-with-desired-state-configuration-dsc"></a>Desired State Configuration (DSC) ile çalışırken sık karşılaşılan hatalar

### <a name="unsupported-characters"></a>Senaryo: Portaldan özel karakterler içeren bir yapılandırma silinemez.

#### <a name="issue"></a>Sorun

Portaldan bir DSC yapılandırması silme girişiminde bulunduğunuzda aşağıdaki hatayı görürsünüz:

```
An error occured while deleteing the DSC configuration '<name>'.  Error-details: The arguement configurationName with the value <name> is not valid.  Valid configuration names can contain only letters,  numbers, and underscores.  The name must start with a letter.  The length of the name must be between 1 and 64 characters.
```

#### <a name="cause"></a>Nedeni

Çözümlenecek planlı geçici bir sorundur.

#### <a name="resolution"></a>Çözüm

* Yapılandırmayı silmek için "Remove-AzAutomationDscConfiguration" Az cmdlet'ini kullanın.
* Bu cmdlet belgeleri henüz güncelleştirilmemiş.  O zamana kadar AzureRM modülünü belgelerine bakın.
  * [Remove-AzureRmAutomationDSCConfiguration](https://docs.microsoft.com/en-us/powershell/module/azurerm.automation/Remove-AzureRmAutomationDscConfiguration?view=azurermps-6.13.0)

### <a name="failed-not-found"></a>Senaryo: Bir "Bulunamadı" hatası ile başarısız durumundaki düğümüdür

#### <a name="issue"></a>Sorun

Düğüm içeren bir rapor olan **başarısız** durumu ve hata içeren:

```
The attempt to get the action from server https://<url>//accounts/<account-id>/Nodes(AgentId=<agent-id>)/GetDscAction failed because a valid configuration <guid> cannot be found.
```

#### <a name="cause"></a>Nedeni

Düğüm yapılandırması adı (örneğin, ABC) atandığında bir düğüm yapılandırması adı yerine (örneğin, ABC. Bu hata genellikle oluşur. Web sunucusu).

#### <a name="resolution"></a>Çözüm

* "Düğüm yapılandırması adı" ve "yapılandırma adı değil" düğümle atadığınız emin olun.
* Düğüm yapılandırması, Azure portalını kullanarak bir düğüme veya bir PowerShell cmdlet'i ile atayabilirsiniz.

  * Azure portalını kullanarak bir düğüme bir düğüm yapılandırması Ata için açık **DSC düğümleri** sayfası, ardından bir düğüm seçin ve tıklayın **düğüm yapılandırması Ata** düğmesi.  
  * Düğüm yapılandırması PowerShell cmdlet'ini kullanarak bir düğüme atamak için kullanılması **kümesi AzureRmAutomationDscNode** cmdlet'i

### <a name="no-mof-files"></a>Senaryo: Düğüm yapılandırması (MOF dosyaları) bir yapılandırma derlendiğinde üretilmiş

#### <a name="issue"></a>Sorun

DSC derleme işi askıya alır ve şu hata oluştu:

```
Compilation completed successfully, but no node configuration.mofs were generated.
```

#### <a name="cause"></a>Nedeni

Zaman ifade aşağıdaki **düğüm** DSC yapılandırma anahtar sözcüğü değerlendirilen `$null`, düğüm yapılandırması üretilen sonra.

#### <a name="resolution"></a>Çözüm

Aşağıdaki çözümlerden birini sorunu düzeltin:

* Emin olun yanındaki ifade **düğüm** $null için yapılandırma tanımı'ndaki anahtar sözcüğü değerlendirme yok.
* Yapılandırma derlenirken ConfigurationData geçiriyorsanız, gelen yapılandırma gerektirir. beklenen değerler geçirdiğinizden emin olun [ConfigurationData](../automation-dsc-compile.md#configurationdata).

### <a name="dsc-in-progress"></a>Senaryo: DSC düğüm raporu "devam ediyor" durumuna kalmış olur

#### <a name="issue"></a>Sorun

DSC aracı çıkışlar:

```
No instance found with given property values
```

#### <a name="cause"></a>Nedeni

WMF sürümünüzü yükseltmişseniz ve WMI bozulmuş.

#### <a name="resolution"></a>Çözüm

Sorun izleme yönergeleri düzeltmek için [bilinen sorunlar ve sınırlamalar DSC](https://msdn.microsoft.com/powershell/wmf/5.0/limitation_dsc) makalesi.

### <a name="issue-using-credential"></a>Senaryo: Bir kimlik bilgisi bir DSC yapılandırması kullanılamıyor

#### <a name="issue"></a>Sorun

Hata, DSC derleme işi askıya alındı:

```
System.InvalidOperationException error processing property 'Credential' of type <some resource name>: Converting and storing an encrypted password as plaintext is allowed only if PSDscAllowPlainTextPassword is set to true.
```

#### <a name="cause"></a>Nedeni

Bir kimlik bilgisi bir yapılandırmada kullanmış ancak uygun sağlamadı **ConfigurationData** ayarlanacak **PSDscAllowPlainTextPassword** her düğüm yapılandırması için true.

#### <a name="resolution"></a>Çözüm

* Uygun geçirdiğinizden emin olun **ConfigurationData** ayarlanacak **PSDscAllowPlainTextPassword** yapılandırmasında belirtilen her düğüm yapılandırması için true. Daha fazla bilgi için [Azure Automation DSC varlıkları](../automation-dsc-compile.md#assets).

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görülmez veya sorununuzu çözmenize yüklenemiyor, daha fazla destek için aşağıdaki kanalları birini ziyaret edin:

* [Azure Forumları](https://azure.microsoft.com/support/forums/) aracılığıyla Azure uzmanlarından yanıtlar alın
* [@AzureSupport](https://twitter.com/azuresupport) hesabı ile bağlantı kurun. Bu resmi Microsoft Azure hesabı, müşteri deneyimini geliştirmek için Azure topluluğunu doğru kaynaklara ulaştırır: yanıtlar, destek ve uzmanlar.
* Daha fazla yardıma ihtiyacınız varsa, bir Azure destek olayına dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) seçip **alma desteği**.
