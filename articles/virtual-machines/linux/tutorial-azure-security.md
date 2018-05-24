---
title: Öğretici - Azure’da Linux VM’ler için Azure Güvenlik Merkezi’ni kullanma | Microsoft Docs
description: Bu öğreticide, Azure’da Linux sanal makinelerinizi korumaya ve güvenliğini sağlamaya yardımcı olmak için Azure Güvenlik Merkezi özelliklerini öğreneceksiniz.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/07/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: e049bed6336f87d8077726843bbc870be90c633f
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
ms.locfileid: "32188852"
---
# <a name="tutorial-use-azure-security-center-to-monitor-linux-virtual-machines"></a>Öğretici: Linux sanal makinelerini izlemek için Azure Güvenlik Merkezi kullanma

Azure Güvenlik Merkezi, Azure kaynak güvenliği uygulamalarınıza yönelik görünürlük elde etmenize yardımcı olabilir. Güvenlik Merkezi, tümleşik güvenlik izleme işlevi sunar. Gözden kaçabilecek tehditleri algılayabilir. Bu öğreticide, Azure Güvenlik Merkezi hakkında bilgi edinecek ve aşağıdakilerin nasıl yapılacağını öğreneceksiniz:
 
> [!div class="checklist"]
> * Veri toplamayı ayarlama
> * Güvenlik ilkeleri ayarlama
> * Yapılandırma durumu sorunlarını görüntüleme ve düzeltme
> * Algılanan tehditleri gözden geçirme

## <a name="security-center-overview"></a>Güvenlik Merkezine genel bakış

Güvenlik Merkezi, olası sanal makine (VM) yapılandırma sorunlarını ve hedeflenmiş güvenlik tehditlerini algılar. Bunlar arasında, ağ güvenlik grupları olmayan, şifrelenmemiş diskler ve deneme yanılma Uzak Masaüstü Protokolü (RDP) saldırıları içeren sanal makineler yer alır. Güvenlik Merkezi panosunda bilgiler kolay okunabilen graflarda gösterilir.

Güvenlik Merkezi panosuna erişmek için Azure portalındaki menüde **Güvenlik Merkezi**’ni seçin. Panoda, Azure ortamınızın güvenlik durumunu görebilir, geçerli öneri sayısını bulabilir ve tehdit uyarılarının geçerli durumunu görüntüleyebilirsiniz. Daha fazla ayrıntı görmek için her bir üst düzey grafiği genişletebilirsiniz.

![Güvenlik Merkezi panosu](./media/tutorial-azure-security/asc-dash.png)

Güvenlik Merkezi, algıladığı sorunlara yönelik öneriler sağlamak için veri keşfinin ötesinde işlev sunar. Örneğin, bir sanal makine, ağ güvenliği grubu eklenmemiş şekilde dağıtıldıysa Güvenlik Merkezi, uygulayabileceğiniz düzeltme adımlarıyla birlikte bir öneri görüntüler. Güvenlik Merkezi bağlamından çıkmadan otomatik düzeltme elde edersiniz.  

![Öneriler](./media/tutorial-azure-security/recommendations.png)

## <a name="set-up-data-collection"></a>Veri toplamayı ayarlama

Sanal makine güvenlik yapılandırmalarına yönelik görünürlük elde edebilmeniz için önce Güvenlik Merkezi veri toplama ayarlamanız gerekir. Bu işlem, veri koleksiyonunun açılmasını ve toplanan verileri barındıracak bir Azure depolama hesabı oluşturulmasını kapsar. 

1. Güvenlik Merkezi panosunda **Güvenlik ilkesi**’ne tıklayın ve sonra aboneliğinizi seçin. 
2. **Veri toplama** için **Açık** seçeneğini belirleyin.
3. Depolama hesabı oluşturmak için **Depolama hesabı seçin** seçeneğini belirleyin. Sonra **Tamam**’ı seçin.
4. **Güvenlik İlkesi** dikey penceresinde **Kaydet**’i seçin. 

Güvenlik Merkezi veri toplama aracısı tüm sanal makinelere yüklenir ve veri toplama başlar. 

## <a name="set-up-a-security-policy"></a>Güvenlik ilkesi ayarlama

Güvenlik ilkeleri, Güvenlik Merkezi’nin kendisi için veriler topladığı ve önerilerde bulunduğu öğeleri tanımlamak için kullanılır. Farklı Azure kaynaklarına farklı güvenlik ilkeleri uygulayabilirsiniz. Varsayılan olarak Azure kaynakları tüm ilke öğelerine karşı değerlendirilse de, tüm Azure kaynakları için veya bir kaynak grubu için tek tek ilke öğelerini kapatabilirsiniz. Güvenlik Merkezi güvenlik ilkeleri hakkında ayrıntılı bilgi için bkz. [Azure Güvenlik Merkezi’nde güvenlik ilkelerini ayarlama](../../security-center/security-center-policies.md). 

Tüm Azure kaynaklarına yönelik bir güvenlik ilkesi ayarlamak için:

1. Güvenlik Merkezi panosunda **Güvenlik ilkesi**’ni seçin ve sonra aboneliğinizi seçin.
2. **Önleme ilkesi**’ni seçin.
3. Tüm Azure kaynaklarına uygulamak istediğiniz ilke öğelerini açın veya kapatın.
4. Ayarlarınızı seçmeyi tamamladığınızda **Tamam**’ı seçin.
5. **Güvenlik ilkesi** dikey penceresinde **Kaydet**’i seçin. 

Belirli bir kaynak grubuna yönelik ilke ayarlamak için:

1. Güvenlik Merkezi panosunda **Güvenlik ilkesi**’ni seçin ve sonra bir kaynak grubu seçin.
2. **Önleme ilkesi**’ni seçin.
3. Kaynak grubuna uygulamak istediğiniz ilke öğelerini açın veya kapatın.
4. **DEVRALMA** bölümünde **Benzersiz**’i seçin.
5. Ayarlarınızı seçmeyi tamamladığınızda **Tamam**’ı seçin.
6. **Güvenlik ilkesi** dikey penceresinde **Kaydet**’i seçin.  

Bu sayfada belirli bir kaynak grubu için veri toplamayı da kapatabilirsiniz.

Aşağıdaki örnekte, *myResoureGroup* adlı bir kaynak grubu için benzersiz bir ilke oluşturulmuştur. Bu ilkede, disk şifreleme ve web uygulaması güvenlik duvarı önerileri kapatılmıştır.

![Benzersiz ilke](./media/tutorial-azure-security/unique-policy.png)

## <a name="view-vm-configuration-health"></a>Sanal makine yapılandırma durumunu görüntüleme

Veri toplamayı açıp bir güvenlik ilkesi ayarlamanızın ardından Güvenlik Merkezi, uyarılar ve öneriler sağlamaya başlar. Sanal makineler dağıtılırken veri toplama aracısı yüklenir. Güvenlik Merkezi, yeni sanal makineler için verilerle doldurulur. Sanal makine yapılandırma durumu hakkında ayrıntılı bilgi için bkz. [Güvenlik Merkezinizdeki sanal makinelerinizi koruma](../../security-center/security-center-virtual-machine-recommendations.md). 

Veriler toplanırken, her bir sanal makine ve ilgili Azure kaynağı için kaynak durumu toplanır. Bilgiler, kolay okunur bir grafikte gösterilir. 

Kaynak durumunu görüntülemek için:

1.  Güvenlik Merkezi panosunda **Kaynak güvenlik durumu** bölümünde **İşlem**’i seçin. 
2.  **İşlem** dikey penceresinde **Sanal makineler**’i seçin. Bu görünüm, tüm sanal makinelerinizin yapılandırma durumunun özetini sağlar.

![İşlem durumu](./media/tutorial-azure-security/compute-health.png)

Bir sanal makineye yönelik tüm önerileri görmek için sanal makineyi seçin. Bu öğreticinin sonraki kısmında öneriler ve düzeltme adımları ayrıntılı olarak ele alınmaktadır.

## <a name="remediate-configuration-issues"></a>Yapılandırma sorunlarını düzeltme

Güvenlik Merkezi, yapılandırma verileriyle doldurma işlemine başladıktan sonra, ayarladığınız güvenlik ilkesine göre önerilerde bulunulur. Örneğin, ilişkili ağ güvenlik grubu olmadan bir sanal makine ayarlandıysa, bir tane oluşturulması için öneride bulunulur. 

Tüm önerilerin listesini göstermek için: 

1. Güvenlik Merkezi panosunda **Öneriler**’i seçin.
2. Belirli bir öneri seçin. Önerinin geçerli olduğu tüm kaynakların listesi görüntülenir.
3. Bir öneri uygulamak için belirli bir kaynak seçin. 
4. Düzeltme adımları için yönergeleri izleyin. 

Çoğu durumda Güvenlik Merkezi, Güvenlik Merkezi’nden çıkmadan bir öneriyi ele almak için uygulayabileceğiniz adımları sağlar. Aşağıdaki örnekte Güvenlik Merkezi, sınırsız gelen kuralı olan bir ağ güvenlik grubunu algılar. Öneri sayfasında **Gelen kurallarını düzenle** düğmesini seçebilirsiniz. Kuralı değiştirmek için gerekli kullanıcı arabirimi görüntülenir. 

![Öneriler](./media/tutorial-azure-security/remediation.png)

Öneriler düzeltildikçe çözümlendi olarak işaretlenir. 

## <a name="view-detected-threats"></a>Algılanan tehditleri görüntüleme

Güvenlik Merkezi, kaynak yapılandırma önerilerine ek olarak tehdit algılama uyarıları görüntüler. Güvenlik uyarıları özelliği, Azure kaynaklarına karşı güvenlik tehditlerini algılamak için her bir sanal makineden, Azure ağ bağlantısı günlükleri ve bağlantılı iş ortağı çözümlerinden toplanan verileri bir araya getirir. Güvenlik Merkezi tehdit algılama özellikleri hakkında ayrıntılı bilgi için [Azure Güvenlik Merkezi algılama özellikleri](../../security-center/security-center-detection-capabilities.md) konusuna bakın.

Güvenlik uyarıları özelliği, Güvenlik Merkezi fiyatlandırma katmanının *Ücretsiz* katmanından *Standart* katmanına yükseltilmesini gerektirir. Bu yüksek fiyatlandırma katmanına geçtiğinizde 30 günlük **ücretsiz deneme sürümü** kullanılabilir. 

Fiyatlandırma katmanını değiştirmek için:  

1. Güvenlik Merkezi panosunda **Güvenlik ilkesi**’ne tıklayın ve sonra aboneliğinizi seçin.
2. **Fiyatlandırma katmanı**'nı seçin.
3. Yeni katmanı seçin ve sonra **Seç**’e tıklayın.
4. **Güvenlik ilkesi** dikey penceresinde **Kaydet**’i seçin. 

Fiyatlandırma katmanını değiştirmenizin ardından, güvenlik tehditleri algılandıkça güvenlik uyarıları grafı doldurulmaya başlar.

![Güvenlik uyarıları](./media/tutorial-azure-security/security-alerts.png)

Bilgileri görüntülemek için bir uyarı seçin. Örneğin, tehdidin bir açıklamasını, algılama süresini, tüm tehdit girişimlerini ve önerilen düzeltmeyi görebilirsiniz. Aşağıdaki örnekte, 294 başarısız RDP girişimiyle birlikte bir RDP deneme yanılma saldırısı algılandı. Önerilen bir çözüm sağlanır.

![RDP saldırısı](./media/tutorial-azure-security/rdp-attack.png)

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, Azure Güvenlik Merkezi’ni ayarladınız ve sonra Güvenlik Merkezi'ndeki sanal makineleri gözden geçirdiniz. Şunları öğrendiniz:

> [!div class="checklist"]
> * Veri toplamayı ayarlama
> * Güvenlik ilkeleri ayarlama
> * Yapılandırma durumu sorunlarını görüntüleme ve düzeltme
> * Algılanan tehditleri gözden geçirme

Jenkins, GitHub ve Docker ile bir CI/CD işlem hattı oluşturma hakkında daha fazla bilgi edinmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Jenkins, GitHub ve Docker ile CI/CD altyapısı oluşturma](tutorial-jenkins-github-docker-cicd.md)

