---
title: Varsayılan TEMP klasörünün boyutu, bir rol için çok küçük değil | Microsoft Docs
description: Bir bulut hizmeti rolü alanı TEMP klasörü için sınırlı bir süre vardır. Bu makalede nasıl alanı yetersiz çalışmasını önlemek bazı öneriler sağlar.
services: cloud-services
documentationcenter: ''
author: simonxjx
manager: felixwu
editor: ''
tags: top-support-issue
ms.assetid: 9f2af8dd-2012-4b36-9dd5-19bf6a67e47d
ms.service: cloud-services
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 06/15/2018
ms.author: v-six
ms.openlocfilehash: 5d60c5f2d759f3329997762270e9a0335ebbe1e9
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47095974"
---
# <a name="default-temp-folder-size-is-too-small-on-a-cloud-service-webworker-role"></a>Varsayılan TEMP klasörünün boyutu, bir bulut hizmeti web/çalışan rolünde çok küçük
Varsayılan geçici dizin bir bulut hizmeti çalışan veya web rolünün belirli bir noktada tam durdurabilir 100 MB boyut sınırı vardır. Bu makalede, geçici dizin için alanı çalıştırmaktan kaçınmak açıklar.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="why-do-i-run-out-of-space"></a>Yer kalmadı neden çalıştırılsın mı?
TEMP ve TMP standart Windows ortam değişkenlerini, uygulamanızda çalışan kodu tarafından kullanılabilir. TEMP ve TMP hem bir en fazla 100 MB boyutuna sahip tek bir dizin noktası. Bulut hizmeti yaşam döngüsü bu dizinde depolanan tüm veriler kalıcı değil; bir bulut hizmetinde rol örneklerini dönüştürülünceye, dizin temizlendiği.

## <a name="suggestion-to-fix-the-problem"></a>Sorunu gidermek için öneri
Aşağıdaki yollardan birini uygulayın:

* TEMP veya TMP kullanmak yerine doğrudan erişmek ve yerel depolama kaynağı yapılandırın. Uygulamanızda çalışan kodu yerel depolama kaynağına erişmek için çağrı [RoleEnvironment.GetLocalResource](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.getlocalresource.aspx) yöntemi.
* Yerel depolama kaynağı yapılandırın ve yerel depolama kaynağı yoluna işaret edecek şekilde TEMP ve TMP dizinleri gelin. Bu değişikliği içinde yapılması [RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) yöntemi.

Aşağıdaki kod örneği, hedef dizin TEMP ve TMP gelen OnStart yöntemi içinde nasıl değiştirileceğini gösterir:

```csharp
using System;
using Microsoft.WindowsAzure.ServiceRuntime;

namespace WorkerRole1
{
    public class WorkerRole : RoleEntryPoint
    {
        public override bool OnStart()
        {
            // The local resource declaration must have been added to the
            // service definition file for the role named WorkerRole1:
            //
            // <LocalResources>
            //    <LocalStorage name="CustomTempLocalStore"
            //                  cleanOnRoleRecycle="false"
            //                  sizeInMB="1024" />
            // </LocalResources>

            string customTempLocalResourcePath =
            RoleEnvironment.GetLocalResource("CustomTempLocalStore").RootPath;
            Environment.SetEnvironmentVariable("TMP", customTempLocalResourcePath);
            Environment.SetEnvironmentVariable("TEMP", customTempLocalResourcePath);

            // The rest of your startup code goes here…

            return base.OnStart();
        }
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar
Açıklayan blogunu okuyun [Azure Web rolü ASP.NET geçici klasörü boyutunu artırmak nasıl](http://blogs.msdn.com/b/kwill/archive/2011/07/18/how-to-increase-the-size-of-the-windows-azure-web-role-asp-net-temporary-folder.aspx).

Daha fazlasını görüntüle [sorun giderme makaleleri](https://github.com/MicrosoftDocs/azure-docs/blob/master/articles/vs-azure-tools-debugging-cloud-services-overview.md) bulut Hizmetleri için.

Azure PaaS bilgisayar tanılama verilerini kullanarak bulut hizmeti rolü sorunlarını giderme konusunda bilgi almak için görüntüleyin [Kevin Williamson'ın blog dizisini](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).
