---
title: Azure NetApp Files için bir alt ağ devretmek | Microsoft Docs
description: Azure NetApp Files için bir alt ağ temsilci seçme hakkında bilgi edinin. Bir birim oluştururken Temsilcili alt ağı belirtin.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 07/28/2020
ms.author: b-juche
ms.openlocfilehash: da7aa0889940c560df705e3c47f5ccb1960aee2c
ms.sourcegitcommit: d95cab0514dd0956c13b9d64d98fdae2bc3569a0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91361033"
---
# <a name="delegate-a-subnet-to-azure-netapp-files"></a>Azure NetApp Files için bir alt ağı temsilci olarak belirleme 

Azure NetApp Files için bir alt ağ temsilciliğini almalısınız.   Bir birim oluşturduğunuzda, temsilci alt ağını belirtmeniz gerekir.

## <a name="considerations"></a>Dikkat edilmesi gerekenler

* Yeni bir alt ağ oluşturmak için sihirbaz, 251 kullanılabilir IP adresi sağlayan bir/24 ağ maskesini varsayılan olarak belirler. 11 kullanılabilir IP adresi sağlayan/28 ağ maskesini kullanarak hizmet için yeterlidir.
* Her bir Azure sanal ağında (VNet), Azure NetApp Files için yalnızca bir alt ağ atanabilir.   
   Azure, bir sanal ağda birden fazla Temsilcili alt ağ oluşturmanızı sağlar.  Ancak, birden fazla Temsilcili alt ağ kullanırsanız, yeni bir birim oluşturma girişimleri başarısız olur.  
   VNet 'te yalnızca tek bir Temsilcili alt ağa sahip olabilirsiniz. Bir NetApp hesabı, her biri kendi Temsilcili alt ağına sahip olan birden fazla VNET 'e birim dağıtabilir.  
* Temsilci alt ağında bir ağ güvenlik grubu veya hizmet uç noktası belirtemezsiniz. Bunun yapılması alt ağ temsilcisinin başarısız olmasına neden olur.
* Genel olarak eşlenmiş bir sanal ağdan bir birime erişim şu anda desteklenmiyor.
* Azure NetApp Files için temsilci atanmış bir alt ağa adres ön eki (hedef) ile VM alt ağları üzerinde [Kullanıcı tanımlı özel yollar](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview#custom-routes) oluşturma desteklenmiyor. Bunun yapılması, VM bağlantısını etkiler. Azure NetApp Files, temsilci alt ağına bir sistem yolu oluşturur. Yol, sorun giderme için gerekiyorsa yol tablosundaki **etkin yollar** ' da gösterilir.

## <a name="steps"></a>Adımlar

1.  Azure portal **sanal ağlar** dikey penceresine gidin ve Azure NetApp Files için kullanmak istediğiniz sanal ağı seçin.    

1. Sanal ağ dikey penceresinde **alt ağlar** ' ı seçin ve **+ alt ağ** düğmesine tıklayın. 

1. Alt ağ Ekle sayfasında aşağıdaki gerekli alanları tamamlayarak Azure NetApp Files için kullanılacak yeni bir alt ağ oluşturun:
    * **Ad**: alt ağ adını belirtin.
    * **Adres aralığı**: IP adresi aralığını belirtin.
    * **Alt ağ temsili**: **Microsoft. NetApp/birimler**' i seçin. 

      ![Alt ağ temsilcisi](../media/azure-netapp-files/azure-netapp-files-subnet-delegation.png)
    
Ayrıca, [Azure NetApp Files için bir birim oluşturduğunuzda](azure-netapp-files-create-volumes.md)bir alt ağ oluşturup atayabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

* [Azure NetApp Files için birim oluşturma](azure-netapp-files-create-volumes.md)
* [Azure hizmetleri için sanal ağ tümleştirmesi hakkında bilgi edinin](https://docs.microsoft.com/azure/virtual-network/virtual-network-for-azure-services)


