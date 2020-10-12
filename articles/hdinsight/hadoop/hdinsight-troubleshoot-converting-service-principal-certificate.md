---
title: Sertifika içeriğini temele dönüştürme-64-Azure HDInsight
description: Azure HDInsight 'ta hizmet sorumlusu sertifika içeriğini Base-64 kodlu dize biçimine dönüştürme
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/31/2019
ms.custom: devx-track-csharp
ms.openlocfilehash: 7f4974d9e9a2ff3b63b36e45d406a016078bb290
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "89008174"
---
# <a name="converting-service-principal-certificate-contents-to-base-64-encoded-string-format-in-hdinsight"></a>HDInsight 'ta hizmet sorumlusu sertifika içeriğini Base-64 kodlu dize biçimine dönüştürme

Bu makalede, Azure HDInsight kümeleriyle etkileşim kurarken sorun giderme adımları ve olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

Temel olmayan bir 64 karakteri, ikiden fazla doldurma karakteri veya doldurma karakterleri arasında boşluk olmayan bir karakter içerdiğinden girişin geçerli bir Base-64 dizesi olduğunu belirten bir hata iletisi alırsınız.

## <a name="cause"></a>Nedeni

Birincil veya ek depolama alanı olarak Data Lake sahip kümeler oluşturmak için PowerShell veya Azure şablon dağıtımı kullanılırken, Data Lake depolama hesabına erişmek için belirtilen hizmet sorumlusu sertifika içerikleri Base-64 biçimindedir. PFX Sertifika içeriklerinin Base-64 kodlu dizeye hatalı dönüştürülmesi bu hataya neden olabilir.

## <a name="resolution"></a>Çözüm

Pfx biçiminde hizmet sorumlusu sertifikası aldıktan sonra (örnek hizmet sorumlusu oluşturma adımları için [buraya](https://github.com/Azure/azure-quickstart-templates/tree/master/201-hdinsight-datalake-store-azure-storage) bakın), sertifika içeriğini base-64 biçimine dönüştürmek Için aşağıdaki PowerShell komutunu veya C# kod parçacığını kullanın.

```powershell
$servicePrincipalCertificateBase64 = [System.Convert]::ToBase64String([System.IO.File]::ReadAllBytes(path-to-servicePrincipalCertificatePfxFile))
```

```csharp
using System;
using System.IO;

namespace ConsoleApplication
{
    class Program
    {
        static void Main(string[] args)
        {
            var certContents = File.ReadAllBytes(@"<path to pfx file>");
            string certificateData = Convert.ToBase64String(certContents);
            System.Diagnostics.Debug.WriteLine(certificateData);
        }
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* Azure [topluluk desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıt alın.

* [@AzureSupport](https://twitter.com/azuresupport)Azure Community 'yi doğru kaynaklara bağlayarak müşteri deneyimini iyileştirmeye yönelik resmi Microsoft Azure hesabı ile bağlanın: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **destek** ' i seçin veya **Yardım + Destek** hub 'ını açın. Daha ayrıntılı bilgi için lütfen [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)konusunu inceleyin. Abonelik yönetimi ve faturalandırma desteği 'ne erişim Microsoft Azure aboneliğinize dahildir ve [Azure destek planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla teknik destek sağlanır.
