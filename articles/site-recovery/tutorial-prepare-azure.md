---
title: Azure Site Recovery ile şirket içi makineler için Azure'da olağanüstü durum kurtarma hazırlığı yapma | Microsoft Docs
description: Azure Site Recovery ile şirket içi makinelerin olağanüstü durum kurtarma işlemleri için Azure’ın nasıl hazırlanacağını öğrenin.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 11/27/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 5c5699d6e25b1f12f87e389011d044315baccd4d
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52838880"
---
# <a name="prepare-azure-resources-for-disaster-recovery-of-on-premises-machines"></a>Şirket içi makinelerin olağanüstü durum kurtarma işlemleri için Azure kaynaklarını hazırlama

 [Azure Site Recovery](site-recovery-overview.md), planlı ve plansız kesintiler sırasında iş uygulamalarınızı çalışır durumda tutarak, iş sürekliliğinize ve olağanüstü durum kurtarma (BCDR) stratejinize katkıda bulunur. Site Recovery, şirket içi makinelerin ve Azure sanal makinelerinin çoğaltma, yük devretme ve kurtarma gibi olağanüstü durum kurtarma işlemlerini yönetir ve düzenler.

Bu makale, şirket içi sanal makineler için olağanüstü durum kurtarmanın nasıl ayarlanacağını gösteren serideki ilk öğreticidir. Hem şirket içi VMware VM'lerini, Hyper-V VM'lerini hem de fiziksel sunucuları koruyanlar için uygundur.

> [!NOTE]
> Öğreticiler, bir senaryo için en basit dağıtım yolunu size göstermek için tasarlanmıştır. Mümkün olduğunca varsayılan seçenekleri kullanır ve tüm olası ayarları ve yolları göstermez. Ayrıntılı yönergeler için ilgili senaryonun **Nasıl Yapılır** bölümüne başvurun.

Bu makale, şirket içi sanal makineleri (Hyper-V veya VMware) veya Windows/Linux fiziksel sunucularını Azure'a çoğaltmak istediğinizde Azure bileşenlerini nasıl hazırlayacağınızı gösterir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure hesabınızın çoğaltma izinlerine sahip olduğunu doğrulayın.
> * Bir Azure Storage hesabı oluşturun. Çoğaltılan makinelerin görüntüleri burada depolanır.
> * Kurtarma Hizmetleri kasası oluşturun. Kasada VM'lerin meta veri ve yapılandırma bilgileri ile diğer çoğaltma bileşenleri tutulur.
> * Bir Azure ağı ayarlayın. Yük devretmeden sonra Azure sanal makineleri oluşturulduğunda sanal makineler bu Azure ağına katılır.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[Azure Portal](http://portal.azure.com) oturum açın.

## <a name="verify-account-permissions"></a>Hesap izinlerini doğrulama

Ücretsiz Azure hesabınızı oluşturduysanız aboneliğinizin yöneticisi siz olursunuz. Abonelik yöneticisi değilseniz, ihtiyaç duyduğunuz izinleri atamak için yöneticiyle birlikte çalışın. Yeni bir sanal makineye yönelik çoğaltmayı etkinleştirmek için şunları yapma iznine sahip olmalısınız:

- Seçilen kaynak grubunda sanal makine oluşturma.
- Seçilen sanal ağda sanal makine oluşturma.
- Seçilen depolama hesabına yazma.

Bu görevleri tamamlamak için hesabınıza Sanal Makine Katkıda Bulunan yerleşik rolünün atanması gerekir. Ayrıca Site Recovery işlemlerini bir kasada yönetmek için hesabınıza Site Recovery Katkıda Bulunan yerleşik rolünün atanması gerekir.

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Çoğaltılan makinelerin görüntüleri Azure depolama alanında tutulur. Şirket içinden Azure’a yük devretme gerçekleştirdiğinizde depolama alanından Azure sanal makineleri oluşturulur. Depolama hesabının, Kurtarma Hizmetleri kasasıyla aynı bölgede olması gerekir. Bu öğreticide Batı Avrupa'yı kullanıyoruz.

1. [Azure portal](https://portal.azure.com) menüsünde **Kaynak oluştur** > **Depolama** > **Depolama hesabı - blob, dosya, tablo, sorgu**'yu seçin.
2. **Depolama hesabı oluştur** bölümüne hesap için bir ad girin. Bu öğreticiler için **contosovmsacct1910171607** adını kullanıyoruz. Seçtiğiniz ad, Azure içinde benzersiz olmalı, 3 ila 24 karakter uzunluğunda olmalı ve yalnızca rakamlar ve küçük harfler içermelidir.
3. **Dağıtım modeli** bölümünde **Kaynak Yöneticisi**’ni seçin.
4. **Hesap türü** bölümünde **Depolama (genel amaçlı v1)** öğesini seçin. Blob depolamayı seçmeyin.
5. **Çoğaltma** bölümünde depolama yedeklemesi için **Okuma Erişimli Coğrafi Olarak Yedekli depolama**’yı seçin. **Güvenli aktarım gereklidir** seçeneğini **Devre Dışı** olarak bırakıyoruz.
6. **Performans**’ta **Standart**’ı seçin ve **Erişim katmanı**’nda varsayılan **Sık erişim** seçeneğini belirleyin.
7. **Abonelik** bölümünde yeni depolama hesabını oluşturmak istediğiniz aboneliği seçin.
8. **Kaynak grubu** bölümünde yeni bir kaynak grubu adı girin. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Bu öğreticiler için **ContosoRG** kullanıyoruz.
9. **Konum** bölümünde depolama hesabınız için coğrafi konumu seçin. 

   ![Depolama hesabı oluşturma](media/tutorial-prepare-azure/create-storageacct.png)

9. Depolama hesabını oluşturmak için **Oluştur**’u seçin.

## <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturma

1. Azure portalında, **Kaynak oluştur** > **Depolama** > **Backup and Site Recovery (OMS)** öğesini seçin.
2. **Ad** alanına kasayı tanımlamak için kolay bir ad girin. Bu öğretici dizisi için **ContosoVMVault**’u kullanacağız.
3. **Kaynak grubu**'nda **contosoRG** kullanacağız.
4. **Konum**'da **Batı Avrupa** kullanacağız.
5. Panodan kasaya hızlı şekilde erişmek için **Panoya sabitle** > **Oluştur**’u seçin.

   ![Yeni kasa oluştur](./media/tutorial-prepare-azure/new-vault-settings.png)

   Yeni kasa, **Pano** > **Tüm kaynaklar** bölümünde ve ana **Kurtarma Hizmetleri kasaları** sayfasında görüntülenir.

## <a name="set-up-an-azure-network"></a>Azure ağı ayarlama

Yük devretmeden sonra depolamadan Azure sanal makineleri oluşturulduğunda sanal makineler bu ağa katılır.

1. [Azure portalında](https://portal.azure.com) **Kaynak oluştur** > **Ağ** > **Sanal ağ** seçeneklerini belirleyin.
2. Dağıtım modeli olarak **Resource Manager**’ı seçili bırakıyoruz.
3. **Ad** bölümünde bir ağ adı girin. Ad, Azure kaynak grubu içinde benzersiz olmalıdır. Bu öğreticide **ContosoASRnet** kullanıyoruz.
4. İçinde ağın oluşturulacağı kaynak grubunu belirtin. Biz mevcut **contosoRG** kaynak grubunu kullanıyoruz.
5. **Adres aralığı** bölümünde ağ için **10.0.0.0/24** aralığını girin. Biz bu ağda alt ağ kullanmıyoruz.
6. **Abonelik** bölümünde ağın oluşturulacağı aboneliği seçin.
7. **Konum** bölümünde **Batı Avrupa**’yı seçin. Ağın, Kurtarma Hizmetleri kasasıyla aynı bölgede olması gerekir.
8. Ağda hizmet uç noktası olmadan temel DDoS korumasının varsayılan seçeneklerini bırakıyoruz.
9. **Oluştur**’a tıklayın.

   ![Sanal ağ oluşturma](media/tutorial-prepare-azure/create-network.png)

   Sanal ağın oluşturulması birkaç saniye sürer. Oluşturulduktan sonra, Azure portalı panosunda görünür.

## <a name="useful-links"></a>Yararlı bağlantılar

- Azure ağları [hakkında bilgi edinin](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview).
- Azure depolama türleri [hakkında bilgi edinin](https://docs.microsoft.com/azure/storage/common/storage-introduction#types-of-storage-accounts).
- Depolama yedekliği [hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/storage/common/storage-redundancy-grs#read-access-geo-redundant-storage) ve depolama için [güvenli aktarımı](https://docs.microsoft.com/azure/storage/common/storage-require-secure-transfer) öğrenin.



## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure’da olağanüstü durum kurtarma için şirket içi VMware altyapısını hazırlama](tutorial-prepare-on-premises-vmware.md)
