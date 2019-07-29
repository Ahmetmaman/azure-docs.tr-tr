---
title: -Azure'da bir coğrafi yapay zeka sanal makinesi sağlama Azure | Microsoft Docs
description: Coğrafi AI veri bilimi sanal makinesi yapılandırma ve oluşturma hakkında bilgi edinin. Coğrafi AI veri bilimi sanal makinesi, coğrafi veri kullanarak yapay ZEKA ve ML çözümler oluşturmak için araçlar sağlar.
keywords: derin öğrenme yapay ZEKA, veri bilimi araçları, veri bilimi sanal makinesi, Jeo-uzamsal analiz
services: machine-learning
documentationcenter: ''
author: vijetajo
manager: cgronlun
ms.custom: seodec18
ms.assetid: ''
ms.service: machine-learning
ms.subservice: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 03/05/2018
ms.author: vijetaj
ms.openlocfilehash: 4772bf8341196485a91b3df30801b9714a4a64a8
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68591880"
---
# <a name="provision-a-geo-artificial-intelligence-virtual-machine-on-azure"></a>Azure'da bir coğrafi yapay zeka sanal makinesi sağlama 

Coğrafi AI veri bilimi sanal makinesi (DSVM coğrafi) popüler uzantısıdır [Azure veri bilimi sanal makinesi](https://aka.ms/dsvm) özel yapay ZEKA ve Jeo-uzamsal analiz birleştirmek üzere yapılandırılmış. VM'de Jeo-uzamsal analiz tarafından desteklenen [Arcgıs Pro](https://www.arcgis.com/features/index.html). Veri bilimi sanal makine öğrenme modellerini ve hatta derin öğrenme modellerini, coğrafi bilgileri ile zenginleştirilmiş verileri hızlı eğitim sağlar. Yalnızca Windows 2016 DSVM üzerinde desteklenir. 

Coğrafi-DSVM AI dahil olmak üzere çeşitli araçlar içerir:

- Microsoft Bilişsel araç seti, TensorFlow, Keras, Caffe2, bağlayıcı popüler derin öğrenme çerçeveleri GPU sürümleri ister; 
- elde etmeye ve metinsel verileri önceden işleme görüntüsü 
- Microsoft R Server Geliştirici sürümü, Anaconda Python Jupyter not defterleri için Python ve R, Python ve SQL, R IDE'ler gibi geliştirme etkinlikleri için veritabanı araçları
- Esri tarafından sağlanan Arcgıs Pro masaüstü yazılımı birlikte, yapay ZEKA uygulamaları Jeo-uzamsal verilerle çalışabilirsiniz Python ve R arabirimleri. 
 

## <a name="create-your-geo-ai-data-science-vm"></a>Coğrafi AI veri bilimi VM'si oluşturma

Coğrafi AI veri bilimi sanal makinesi örneğini oluşturmak için bir yordam şöyledir: 


1. Sanal makine üzerinde listeleme gidin [Azure portalında](https://ms.portal.azure.com/#create/microsoft-ads.geodsvmwindows).
2. Seçin **Oluştur** Sihirbazı'na alınması için alt kısımdaki düğmesi.
![create-geo-ai-dsvm](./media/provision-geo-ai-dsvm/Create-Geo-AI.png)
3. Coğrafi-DSVM oluşturma için kullanılan sihirbaz gerektirir **girişleri** her biri için **dört adımı** bu şekilde sağ tarafındaki numaralandırılır. Bu adımların her biri yapılandırmak için gerekli girişleri şunlardır:



   - **Temel Bilgiler**

      1. **Ad**: Oluşturmakta olduğunuz veri bilimi sunucusunun adı.

      2. **Kullanıcı adı**: Yönetici hesabı oturum açma kimliği.

      3. **Parola**: Yönetici hesabı parolası.

      4. **Abonelik**: Birden fazla aboneliğiniz varsa, bir makine oluşturulması ve fatura olduğu seçin.

      5. **Kaynak grubu**: Yeni bir tane oluşturabilir veya aboneliğinizde **boş** bir mevcut Azure Kaynak grubu kullanabilirsiniz.

      6. **Konum**: En uygun veri merkezini seçin. Genellikle, verilerinizden en iyi olduğundan veya en hızlı ağ erişimi için fiziksel konumunuza en yakın veri merkezi bulunur. GPU üzerinde derin öğrenme yapmanız gerekiyorsa, NC serisi GPU sanal makine örneklerine sahip Azure'da konumlardan birini seçmeniz gerekir. Şu anda GPU VM 'Leri olan Konumlar şunlardır: **Doğu ABD, Orta Kuzey ABD, Orta Güney ABD, Batı ABD 2, Kuzey Avrupa, Batı Avrupa**. En son liste için [bölge sayfasına göre Azure ürünleri](https://azure.microsoft.com/regions/services/) ve Ara **NC serisi** altında **işlem**. 


   - **Ayarları**: Coğrafi bir DSVM 'niz üzerinde GPU üzerinde derin öğrenme çalıştırmayı planlıyorsanız, NC serisi GPU sanal makine boyutundan birini seçin. Aksi takdirde, örnek tabanlı CPU birini seçebilirsiniz.  Sanal Makineniz için bir depolama hesabı oluşturun. 
   
   - **Özet**: Girdiğiniz tüm bilgilerin doğru olduğunu doğrulayın.

   - **Satın alma**: Sağlamayı başlatmak için **satın al** ' a tıklayın. Hizmet koşulları için bir bağlantı sağlanır. VM, seçtiğiniz sunucu boyutu için işlem ötesinde herhangi bir ek ücreti yok **boyutu** adım. 
 
>[!NOTE]
> Sağlama yaklaşık 20-30 dakika sürer. Sağlama durumunu Azure portalında görüntülenir.

 
## <a name="how-to-access-the-geo-ai-data-science-virtual-machine"></a>Coğrafi AI veri bilimi sanal makinesi erişme

 Sanal makinenizin oluşturulduktan sonra yüklenmiş ve önceden yapılandırılmış araçları kullanmaya başlamak hazırsınız. Başlat menüsü kutucukları ve masaüstü simgelerini birçok araç vardır. Uzak Masaüstü uygulamasına önceki yapılandırdığınız yönetici hesabı kimlik bilgilerini kullanarak yapabilecekleriniz **Temelleri** bölümü. 

 
## <a name="using-arcgis-pro-installed-in-the-vm"></a>Arcgıs Pro kullanarak sanal Makineye yüklenen

Coğrafi-DSVM Arcgıs Pro Masaüstü önceden yüklenmiş ve DSVM tüm araçlar ile çalışmak için önceden yapılandırılmış ortam zaten sahip. Arcgıs başlattığınızda sizden Arcgıs hesabınıza bir oturum açma için ister. Zaten bir Arcgıs hesabınız ve yazılım lisansları varsa, var olan kimlik bilgilerinizi kullanabilirsiniz.  

![Yay GIS oturum](./media/provision-geo-ai-dsvm/ArcGISLogon.png)

Aksi takdirde, yeni Arcgıs hesabı ve lisans için kaydolun veya alma bir [ücretsiz deneme sürümü](https://www.arcgis.com/features/free-trial.html). 

![Arcgıs ücretsiz deneme](./media/provision-geo-ai-dsvm/ArcGIS-Free-Trial.png)

Ücretli veya ücretsiz bir deneme yanılma hesabına kaydolduktan sonra, argıs [Pro 'Yu kullanmaya](https://www.esri.com/library/brochures/getting-started-with-arcgis-pro.pdf)başlama yönergelerini izleyerek hesabınız için argıs Pro 'yu yetkilendirebilirsiniz. 

Arcgıs Pro masaüstüne Arcgıs hesabınızla oturum açtıktan sonra yüklü olan ve VM Jeo-uzamsal analiz ve makine öğrenimi projeleri için yapılandırılan veri bilimi araçlarını kullanmaya başlamak hazırsınız.

## <a name="next-steps"></a>Sonraki adımlar

Coğrafi AI veri bilimi sanal makinesi ile aşağıdaki konular Kılavuzu kullanmaya başlayın:

* [Coğrafi AI veri bilimi VM'si kullanma](use-geo-ai-dsvm.md)
