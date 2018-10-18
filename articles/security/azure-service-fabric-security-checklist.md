---
title: Azure service fabric güvenlik denetim listesi | Microsoft Docs
description: Bu makalede Azure fabric güvenlik güvenliği için Denetim listesi sunmaktadır.
services: security
documentationcenter: na
author: unifycloud
manager: mbaldwin
editor: tomsh
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/04/2017
ms.author: tomsh
ms.openlocfilehash: d0826d60d766874ddd186bcf406ea987134dab16
ms.sourcegitcommit: f20e43e436bfeafd333da75754cd32d405903b07
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2018
ms.locfileid: "49385878"
---
# <a name="azure-service-fabric-security-checklist"></a>Azure Service Fabric güvenlik denetim listesi
Bu makalede, Azure Service Fabric ortamınızın güvenliğini sağlamanıza yardımcı olacak bir kullanımı kolay denetim sağlar.

## <a name="introduction"></a>Giriş
Azure Service Fabric; ölçeklenebilir ve güvenilir mikro hizmetleri paketlemeyi, dağıtmayı ve yönetmeyi kolaylaştırmayı sağlayan bir dağıtılmış sistemler platformudur. Service Fabric ayrıca bulut uygulamalarını geliştirme ve yönetme sürecinde karşılaşılan başlıca sorunların giderilmesini de sağlar. Geliştiriciler ve yöneticiler, karmaşık altyapı sorunlarını çözmeye çalışmak yerine görev açısından kritik, zorlu iş yüklerini uygulamaya odaklanabilir. Service Fabric, bu iş yüklerinin ölçeklenebilir, güvenilir ve yönetilebilir olmasını sağlar.

## <a name="checklist"></a>Denetim listesi
Yönetim ve yapılandırma güvenli Azure Service Fabric çözümünün önemli sorunları atlamış henüz emin olmanıza yardımcı olması için aşağıdaki denetim listesini kullanın.


|Denetim kategorisi| Açıklama |
| ------------ | -------- |
|[Rol tabanlı erişim denetimi (RBAC)](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security-roles) | <ul><li>Küme Yöneticisi, belirli küme işlemleri farklı kümeye daha güvenli hale getirme, kullanıcı grupları için erişimi sınırlandırmak erişim denetimi sağlar.</li><li>Yöneticiler yönetim özellikleri (okuma/yazma özellikleri dahil olmak üzere) tam erişime sahiptir. </li><li> Kullanıcılar, varsayılan olarak, yalnızca yönetim özelliklerine (örneğin, sorgu işlevleri) okuma erişimi ve uygulamaları ve Hizmetleri çözümlenebilmesi sahiptir.</li></ul>|
|[X.509 sertifikaları ve Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security) | <ul><li>[Sertifikaları](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/working-with-certificates) çalışan üretim iş yükleri doğru şekilde yapılandırılmış bir Windows Server sertifika hizmetini kullanarak oluşturulmamalı veya onaylanan alınan kümelerinde kullanılan [sertifika yetkilisi (CA)](https://en.wikipedia.org/wiki/Certificate_authority).</li><li>Hiçbir zaman kullanmak [geçici veya test sertifikaları](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-create-temporary-certificates-for-use-during-development) gibi araçları ile oluşturulmuş üretimde [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968.aspx). </li><li>Kullanabileceğiniz bir [otomatik olarak imzalanan sertifika](https://docs.microsoft.com/azure/service-fabric/service-fabric-windows-cluster-x509-security) ancak yalnızca test kümeleri için alan ve üretim yapmalısınız.</li></ul>|
|[Küme güvenliği](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security) | <ul><li>Düğümden düğüme güvenlik için istemci düğümü güvenlik kümesi güvenlik senaryoları dahil [rol tabanlı erişim denetimi (RBAC)](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security-roles).</li></ul>|
|[Küme kimlik doğrulaması](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) | <ul><li>Kimlik doğrulaması [düğümden düğüme iletişim](https://github.com/MicrosoftDocs/azure-docs/blob/master/articles/service-fabric/service-fabric-cluster-security.md) küme Federasyon için. </li></ul>|
|[Sunucu kimlik doğrulaması](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) | <ul><li>Kimlik doğrulaması [küme yönetim uç noktalarını](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-portal) bir yönetim istemcisi için.</li></ul>|
|[Uygulama güvenliği](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm)| <ul><li>Şifreleme ve şifre çözme uygulama yapılandırma değerlerini.</li><li>   Çoğaltma sırasında düğümler üzerinden verilerin şifrelenmesi.</li></ul>|
|[Küme sertifikası](https://docs.microsoft.com/azure/service-fabric/service-fabric-windows-cluster-x509-security) | <ul><li>Bu sertifika, bir kümedeki düğümlerden arasındaki iletişimin güvenliğini sağlamak için gereklidir.</li><li>    Parmak izi bölümü ve ikincil ThumbprintSecondary değişkenlerine birincil sertifikanın parmak izini ayarlayın.</li></ul>|
|[ServerCertificate](https://docs.microsoft.com/azure/service-fabric/service-fabric-windows-cluster-x509-security)| <ul><li>Bu sertifika, bu kümeye bağlanmayı denediğinde istemciye sunulur. İki farklı sunucu sertifikaları, birincil ve ikincil bir yükseltme için kullanabilirsiniz.</li></ul>|
|ClientCertificateThumbprints| <ul><li>Bu, kimliği doğrulanmış istemcilere yüklemek istediğiniz sertifikaları kümesidir. </li></ul>|
|ClientCertificateCommonNames| <ul><li>İlk istemci sertifikası ortak adı için CertificateCommonName ayarlayın. Bu sertifika verenin parmak izi CertificateIssuerThumbprint olur. </li></ul>|
|ReverseProxyCertificate| <ul><li>Bu, olabilir, isteğe bağlı bir sertifikadır güvenli istiyorsanız belirtilen, [Ters Proxy](https://docs.microsoft.com/azure/service-fabric/service-fabric-reverseproxy). </li></ul>|
|Key Vault| <ul><li>Azure'da Service Fabric kümelerine ait sertifikaları yönetmek için kullanılır.  </li></ul>|


## <a name="next-steps"></a>Sonraki adımlar
- [Service Fabric kümesini yükseltme işlemi ve sizden beklentileri](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-upgrade)
- [Visual Studio'da Service Fabric uygulamalarınızı yönetmek](https://docs.microsoft.com/azure/service-fabric/service-fabric-manage-application-in-visual-studio).
- [Service Fabric sistem durumu modeli giriş](https://docs.microsoft.com/azure/service-fabric/service-fabric-health-introduction).
