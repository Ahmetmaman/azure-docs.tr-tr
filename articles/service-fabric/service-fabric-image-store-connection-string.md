---
title: Azure Service Fabric görüntü deposu bağlantı dizesi
description: Kullanımları ve uygulamaları Service Fabric kümesine dahil olmak üzere görüntü deposu bağlantı dizesi hakkında bilgi edinin.
author: alexwun
ms.topic: conceptual
ms.date: 02/27/2018
ms.author: alexwun
ms.openlocfilehash: 8fc0239dd18fc7071823a129a7dbc4f102023d66
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "86246206"
---
# <a name="understand-the-imagestoreconnectionstring-setting"></a>ImageStoreConnectionString ayarını anlama

Bazı belgelerimizde, gerçekten ne anlama geldiğini açıklamadan "ımatoreconnectionstring" parametresinin varlığını kısaca açıkladık. [PowerShell kullanarak uygulama dağıtma ve kaldırma][10]gibi bir makalede ilerledikten sonra, tüm yaptığınız gibi görünen değeri hedef kümenin küme bildiriminde gösterildiği gibi kopyalayın/yapıştırın. Bu nedenle ayar küme başına yapılandırılabilir olmalıdır, ancak [Azure Portal][11]aracılığıyla bir küme oluşturduğunuzda, bu ayarı yapılandırma seçeneği yoktur ve her zaman "Fabric: ımaıt" olur. Bu ayarın amacı ne olacak?

![Küme bildirimi][img_cm]

Çok çeşitli ekipler tarafından dahili Microsoft tüketimine yönelik bir platform olarak Service Fabric, bu nedenle, bunun bazı yönleri oldukça özelleştirilebilirdir. "Görüntü Deposu" böyle bir yöntür. Temelde Görüntü Deposu, uygulama paketlerini depolamak için takılabilir bir depodur. Uygulamanız kümedeki bir düğüme dağıtıldığında, bu düğüm Görüntü Deposu uygulama paketinizin içeriğini indirir. Imatoreconnectionstring, belirli bir küme için doğru Görüntü Deposu bulmak üzere hem istemciler hem de düğümler için gerekli tüm bilgileri içeren bir ayardır.

Şu anda üç olası Görüntü Deposu sağlayıcısı vardır ve bunlara karşılık gelen bağlantı dizeleri aşağıdaki gibidir:

1. Görüntü Deposu Service: "Fabric: ımabir"

2. Dosya sistemi: "dosya: [dosya sistemi yolu]"

3. Azure depolama: "XSTORE: DefaultEndpointsProtocol = https; AccountName = [...]; AccountKey = [...]; Kapsayıcı = [...] "

Üretimde kullanılan sağlayıcı türü, Service Fabric Explorer görebileceğiniz bir durum bilgisi olmayan kalıcı sistem hizmeti olan Görüntü Deposu hizmetidir. 

![Görüntü Deposu hizmeti][img_is]

Görüntü Deposu küme içindeki bir sistem hizmetinde barındırmak, paket deposunun dış bağımlılıklarını ortadan kaldırır ve depolama alanı üzerinde daha fazla denetim elde etmenizi sağlar. Görüntü Deposu bir sonraki iyileştirmeler, özel olarak değil, önce Görüntü Deposu sağlayıcıyı hedefleyebilir. İstemci hedef kümeye zaten bağlı olduğundan, Görüntü Deposu hizmet sağlayıcısı için bağlantı dizesi benzersiz bilgiler içermez. İstemcinin yalnızca sistem hizmetini hedefleyen protokollerin kullanılması gerektiğini bilmeleri gerekir.

Kümeyi biraz daha hızlı önyüklemek için geliştirme sırasında yerel tek Box kümelerine yönelik Görüntü Deposu hizmeti yerine dosya sistemi sağlayıcısı kullanılır. Fark genellikle küçüktür, ancak geliştirme sırasında çoğu katlara yönelik faydalı bir iyileştirmedir. Yerel bir kutu kümesini diğer depolama sağlayıcısı türleriyle birlikte dağıtmak da mümkündür, ancak geliştirme/test iş akışı sağlayıcıdan bağımsız olarak aynı kaldığından genellikle bunu yapmanız gerekmez. Azure depolama sağlayıcısı yalnızca Görüntü Deposu hizmeti sağlayıcısı sunulmadan önce dağıtılan eski kümelerin eski desteği için mevcuttur.

Ayrıca, dosya sistemi sağlayıcısı veya Azure depolama sağlayıcısı, birden çok küme arasında Görüntü Deposu paylaşma yöntemi olarak kullanılmalıdır. Bu, her küme, Görüntü Deposu çakışan veriler yazabileceğinden, küme yapılandırma verilerinin bozulmasına neden olur. Sağlanan uygulama paketlerini birden çok küme arasında paylaştırmak için, indirme URI 'SI olan herhangi bir harici depoya yüklenebilen yerine [sfpkg][12] dosyalarını kullanın.

Bu nedenle, ımabtoreconnectionstring yapılandırılabilir, ancak varsayılan ayarı kullanmanız yeterlidir. Visual Studio aracılığıyla Azure 'a yayımlarken, parametre sizin için otomatik olarak ayarlanır. Azure 'da barındırılan kümelere programlı dağıtım için bağlantı dizesi her zaman "doku: ımabir" dir. Şüpheli halde, bunun değeri [PowerShell](/powershell/module/servicefabric/get-servicefabricclustermanifest), [.net](/previous-versions/azure/reference/mt161375(v=azure.100))veya [rest](/rest/api/servicefabric/get-a-cluster-manifest)tarafından küme bildirimi alınırken her zaman doğrulanabilir. Hem şirket içi test hem de üretim kümeleri her zaman Görüntü Deposu hizmet sağlayıcısını kullanacak şekilde yapılandırılmalıdır.

### <a name="next-steps"></a>Sonraki adımlar
[PowerShell kullanarak uygulama dağıtma ve kaldırma][10]

<!--Image references-->
[img_is]: ./media/service-fabric-image-store-connection-string/image_store_service.png
[img_cm]: ./media/service-fabric-image-store-connection-string/cluster_manifest.png

[10]: service-fabric-deploy-remove-applications.md
[11]: service-fabric-cluster-creation-via-portal.md
[12]: service-fabric-package-apps.md#create-an-sfpkg
