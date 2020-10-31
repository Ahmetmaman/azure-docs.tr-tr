---
title: 'Hızlı başlangıç: Windows Veri Bilimi Sanal Makinesi oluşturma'
titleSuffix: Azure Data Science Virtual Machine
description: Analiz ve makine öğrenimi için Azure 'da bir Veri Bilimi Sanal Makinesi yapılandırın ve oluşturun.
ms.service: machine-learning
ms.subservice: data-science-vm
author: gvashishtha
ms.author: gopalv
ms.topic: quickstart
ms.date: 12/31/2019
ms.openlocfilehash: e807cde2d99cf23d459b395444d642e46ada32ab
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93071462"
---
# <a name="quickstart-set-up-the-data-science-virtual-machine-for-windows"></a>Hızlı başlangıç: Windows için Veri Bilimi Sanal Makinesi ayarlama

Windows Server 2019 Veri Bilimi Sanal Makinesi ile çalışmaya başlayın.

## <a name="prerequisite"></a>Önkoşul

Bir Windows Veri Bilimi Sanal Makinesi oluşturmak için bir Azure aboneliğinizin olması gerekir. [Azure 'ı ücretsiz deneyin](https://azure.com/free).
GPU destekli sanal makine SKU'larının ücretsiz Azure hesaplarında desteklenmediğini lütfen unutmayın.

## <a name="create-your-dsvm"></a>DSVM 'nizi oluşturma

DSVM örneği oluşturmak için:

1. Daha önce oturum açmadıysanız Azure hesabınızda oturum açmanız istenebilir [Azure Portal](https://portal.azure.com) gidin.
1. "Veri bilimi sanal makinesi" ni yazarak ve "Veri Bilimi Sanal Makinesi-Windows 2019" öğesini seçerek sanal makine listesini bulun.

1. Alt kısımdaki **Oluştur** düğmesini seçin.

1. "Sanal makine oluşturma" dikey penceresine yönlendirilmelisiniz.

1. **Temel bilgiler** sekmesini doldur:
      * **Abonelik** : birden fazla aboneliğiniz varsa makinenin oluşturulacağı ve faturalandırılabileceği bir tane seçin. Bu abonelikte kaynak oluşturma ayrıcalıklarına sahip olmanız gerekir.
      * **Kaynak grubu** : yeni bir grup oluşturun veya var olan bir grubu kullanın.
      * **Sanal makine adı** : sanal makinenin adını girin. Azure portal bu şekilde görünür.
      * **Konum** : en uygun veri merkezini seçin. En hızlı ağ erişimi için bu, verilerinizin çoğunu veya fiziksel konumunuza en yakın olan veri merkezdir. [Azure bölgeleri](https://azure.microsoft.com/global-infrastructure/regions/)hakkında daha fazla bilgi edinin.
      * **Görüntü** : varsayılan değeri bırakın.
      * **Boyut** : Bu, genel iş yükleri için uygun bir boyutla otomatik olarak doldurulur. [Azure 'Da WINDOWS VM boyutları](../../virtual-machines/windows/sizes.md)hakkında daha fazla bilgi edinin.
      * **Kullanıcı adı** : yönetici kullanıcı adını girin. Bu, sanal makinenizde oturum açmak için kullanacağınız Kullanıcı adı ve Azure Kullanıcı adınızla aynı olmaması gerekir.
      * **Parola** : sanal makinenizde oturum açmak için kullanacağınız parolayı girin.    
1. **Gözden geçir ve oluştur** ’u seçin.
1. **Gözden geçir + oluştur**
   * Girdiğiniz tüm bilgilerin doğru olduğundan emin olun. 
   * **Oluştur** ’u seçin.


> [!NOTE]
> * Sanal makinede önceden yüklenmiş olarak gelen yazılımlar için lisans ücretleri ödemezsiniz. **Boyut** adımında seçtiğiniz sunucu boyutu için işlem maliyetini ödeyin.
> * Sağlama 10 ila 20 dakika sürer. Azure portal sanal makinenizin durumunu görüntüleyebilirsiniz.

## <a name="access-the-dsvm"></a>DSVM 'ye erişme

VM oluşturulup sağlandıktan sonra, [Azure tabanlı sanal makinenize bağlanmak](https://docs.microsoft.com/azure/marketplace/azure-vm-create-using-approved-base)için listelenen adımları izleyin. Bir sanal makine oluşturmanın **temelleri** adımında yapılandırdığınız Yönetici hesabı kimlik bilgilerini kullanın. 

VM 'de yüklü ve yapılandırılmış araçları kullanmaya başlamaya hazırsınız. Araçların çoğuna **Başlangıç** menüsü kutucukları ve Masaüstü simgeleri aracılığıyla erişilebilir.

Ayrıca, sanal makine üzerinde jupi not defterlerini çalıştırmak ve ücretsiz hizmet katmanının sınırlamalarını atlamak için Azure Notebooks bir DSVM ekleyebilirsiniz. Daha fazla bilgi için bkz. [Not defteri projelerini yönetme ve yapılandırma](../../notebooks/configure-manage-azure-notebooks-projects.md#manage-and-configure-projects).

<a name="tools"></a>


## <a name="next-steps"></a>Sonraki adımlar

* **Başlat** menüsünü açarak dsvm 'deki araçları araştırın.
* [Azure Machine Learning ne olduğunu](../overview-what-is-azure-ml.md) okuyarak Azure Machine Learning hakkında bilgi edinin ve [öğreticiler](../index.yml)deneyin.
* [Azure 'da bir Windows veri bilimi sanal makinesi veri bilimi](./vm-do-ten-things.md) makalesini okuyun

