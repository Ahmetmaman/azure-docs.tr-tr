---
title: Azure Güvenlik Merkezi'ndeki uyarlamalı uygulama denetimleri | Microsoft Docs
description: Bu belge, Azure Güvenlik Merkezi'ndeki uyarlamalı uygulama denetimlerini kullanarak Azure VM'lerinde çalışan uygulamaları beyaz listeye eklemenize yardımcı olur.
services: security-center
documentationcenter: na
author: rkarlin
manager: mbaldwin
editor: ''
ms.assetid: 9268b8dd-a327-4e36-918e-0c0b711e99d2
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/2/2018
ms.author: rkarlin
ms.openlocfilehash: b4023d45c3628df5006d076e01f32bb8f3aa80a6
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52846258"
---
# <a name="adaptive-application-controls-in-azure-security-center"></a>Azure Güvenlik Merkezi'ndeki uyarlamalı uygulama denetimleri
Bu kılavuzu kullanarak Azure Güvenlik Merkezi'ndeki uygulama denetimi özelliklerini yapılandırmayı öğrenebilirsiniz.

## <a name="what-are-adaptive-application-controls-in-security-center"></a>Güvenlik Merkezi'ndeki uyarlamalı uygulama denetimleri nelerdir?
Uyarlamalı uygulama denetimleri Azure Güvenlik Merkezi'nden akıllı, otomatik, uçtan uca uygulama beyaz listeye ekleme çözümü olan. Hangi uygulamaların Vm'leriniz Vm'lerinizi kötü amaçlı yazılımlara karşı sağlamlaştırma yardımcı olan diğer avantajlarının yanı sıra, azure'da bulunan çalıştırabilirsiniz denetlemenize yardımcı olur. Güvenlik Merkezi, sanal makinelerinizde çalışan uygulamaları analiz için makine öğrenimini kullanıyor ve bu bilgileri kullanarak belirli bir beyaz listeye ekleme kuralları uygulamanıza yardımcı olur. Bu özellik, yapılandırma ve uygulama beyaz listeye ekleme ilkeleri olanak tanıyarak, bakımını yapma işlemini büyük ölçüde kolaylaştırır:

- Blok veya kötü amaçlı yazılımdan koruma çözümleri tarafından aksi halde eksik, da dahil olmak üzere kötü amaçlı uygulamaların çalıştırma girişimlerini uyarı.
- Kuruluşunuzun yalnızca lisanslı yazılım kullanımını gerektiren kuruluş güvenlik ilkelerine uygun hareket etme.
- Ortamınızda istenmeyen yazılımların kullanılmasını önleme.
- Eski ve desteklenmeyen uygulamaların çalışmasını önleme.
- Kuruluşunuzda kullanılmasına izin verilmeyen belirli yazılım araçlarını engelleme.
- BT ekibinin uygulama üzerinden gizli verilere erişimi denetlemesini mümkün kılma.

## <a name="how-to-enable-adaptive-application-controls"></a>Uyarlamalı uygulama denetimleri nasıl etkinleştirilir?
Uyarlamalı uygulama denetimleri, bir dizi yapılandırılan VM grupları üzerinde çalışmasına izin verilen uygulamalar tanımlamanıza yardımcı olur. Bu özellik yalnızca Windows makinelerde kullanılabilir (tüm sürümler, klasik veya Azure Resource Manager). Güvenlik Merkezi'nde uygulama beyaz listesini yapılandırmak için aşağıdaki adımları kullanabilirsiniz:

1. **Güvenlik Merkezi** panosunu açın.
2. Sol bölmeden **Gelişmiş bulut savunması** altında bulunan **Uyarlamalı uygulama denetimlerini** seçin.

    ![Savunma](./media/security-center-adaptive-application/security-center-adaptive-application-fig1-new.png)

**Uyarlamalı uygulama denetimleri** sayfası açılır.

![denetimler](./media/security-center-adaptive-application/security-center-adaptive-application-fig2.png)

**VM grupları** bölümünde üç sekme bulunur:

* **Yapılandırılan**: Uygulama denetimiyle yapılandırılan VM’leri içeren grupların listesidir.
* **Önerilen**: Uygulama denetiminin önerildiği grupların listesidir. Güvenlik Merkezi makine öğrenimi özelliklerini kullanarak VM'lerin tutarlı bir şekilde aynı uygulamaları çalıştırıp çalıştırmadığına bakar ve uygulama denetimi için uygun olan VM'leri tanımlar.
* **Öneri olmayan**: Uygulama denetimi önerisi olmayan VM’leri içeren grupların listesidir. Örneğin, uygulamaların sürekli değiştiği ve kararlı bir duruma geçmediği VM'ler.

> [!NOTE]
> Güvenlik Merkezi benzer VM’lerin önerilen en iyi uygulama denetimi ilkesini alması için VM grupları oluşturan özel bir kümeleme algoritması kullanır.
>
>

### <a name="configure-a-new-application-control-policy"></a>Yeni bir uygulama denetim ilkesi yapılandırma
1. Uygulama denetimi önerileri bulunan grupların listesi için **Önerilen** sekmesine tıklayın:

  ![Önerilen](./media/security-center-adaptive-application/security-center-adaptive-application-fig3.png)

  Liste aşağıdakileri içerir:

  - **AD**: Aboneliğin ve grubun adı
  - **VM'ler**: Grup içindeki sanal makine sayısı
  - **Durum**: önerilerin durumu
  - **ÖNEM DERECESİ**: Önerilerin önem derecesi

2. Açmak için bir grubu tıklatın **uygulama denetimi kuralları oluştur** seçeneği.

  ![Uygulama denetimi kuralları](./media/security-center-adaptive-application/security-center-adaptive-application-fig4.png)

3. İçinde **Vm'leri Seç**, önerilen VM'lerin listesini gözden geçirin ve herhangi bir uygulama whitelising ilkesi uygulamak istemediğiniz işaretini kaldırın. Daha sonra iki liste görürsünüz:

  - **Önerilen uygulamalar**: Bu gruptaki vm'lerde yaygın olan ve çalışmasına izin verilecek önerilen uygulamaların bir listesi.
  - **Daha fazla uygulama**: Bu gruptaki vm'lerde daha az sıklıkta ya da yararlanılabilir olarak bilinen uygulamaların bir listesini (daha fazla bilgi aşağıdadır) ve gözden geçirmeniz için önerilir.

4. Her bir listedeki uygulamaları gözden geçirin ve uygulamak istemediklerinizin işaretini kaldırın. Her liste aşağıdakileri içerir:

  - **AD**: sertifika bilgileri veya bir uygulamanın tam yolu
  - **DOSYA TÜRLERİ**: Uygulama dosya türü. Bu EXE, betik, MSI veya bu tür bir sıralamaya olabilir.
  - **AÇIKLARDAN**: belirli bir uygulama bir saldırgan tarafından uygulama beyaz listeye ekleme çözümü atlamak için kullanılabilir değilse bir uyarı simgesi gösterir. Bu uygulamaları onaylamadan önce gözden geçirmeniz önerilir.
  - **KULLANICILAR**: Bir uygulama çalıştırmasına izin verilmesi önerilen kullanıcılar

5. Seçimlerinizi tamamladıktan sonra **Oluştur**’u seçin. <br>
Oluşturma seçtikten sonra Azure Güvenlik Merkezi (AppLocker) Windows sunucularında kullanılabilir yerleşik uygulama beyaz listeye ekleme çözümü üzerinde uygun kuralları otomatik olarak oluşturur.


> [!NOTE]
> - Güvenlik Merkezi, temel yapılandırma oluşturmak ve VM gruplarına benzersiz öneri sunmak için en az iki haftalık veri kullanmaktadır. Güvenlik Merkezi standart katmanının yeni müşterileri başlangıçta VM gruplarının *öneri yok* sekmesi altında olduğunu görebilir.
> - Güvenlik Merkezi'ndeki Uyarlamalı Uygulama Denetimleri, GPO veya yerel güvenlik ilkesi ile AppLocker ilkesinin önceden etkinleştirilmiş olduğu VM'leri desteklemez.
> -  En iyi uygulama, Güvenlik Merkezi her zaman çalışır güvenlik izin verilmesi için seçili uygulamalar için bir yayımcı kuralı oluşturmaya ve yalnızca bir uygulama (imzalanmış olmayan) bir yayımcı bilgisi olmayan, tam yolu için bir yol kuralı oluşturulur belirli bir uygulama.
>   

### <a name="editing-and-monitoring-a-group-configured-with-application-control"></a>Uygulama denetimiyle yapılandırılmış bir grubu düzenleme ve izleme

1. Bir uygulama beyaz listeye ekleme İlkesi ile yapılandırılmış bir grubu düzenleyip izlemek için dönüş **Uyarlamalı uygulama denetimleri** sayfasından seçim yapıp **YAPILANDIRILDI** altında **VM grupları**:

  ![Gruplar](./media/security-center-adaptive-application/security-center-adaptive-application-fig5.png)

  Liste aşağıdakileri içerir:

  - **Ad**: aboneliğin ve grubun adı
  - **VM'ler**: Grup içindeki sanal makine sayısı
  - **Modu**: Denetim modu; alınmamış uygulamaların çalıştırma girişimlerini günlüğe Zorunlu alınmamış uygulamaların çalışmasına izin verme
  - **Uyarılar**: herhangi bir geçerli ihlal

2. Bir grup değişiklik yapmak için tıklatın **uygulama denetim ilkesini Düzenle** sayfası.

  ![Koruma](./media/security-center-adaptive-application/security-center-adaptive-application-fig6.png)

3. **Koruma modu** altında aşağıdakilerden birini seçebilirsiniz:

  - **Denetim**: Bu modda uygulama denetimi çözümü kuralları zorunlu kılmaz ve yalnızca korumalı VM'lerdeki etkinliği denetler. Bu, bir uygulamanın hedef VM'de çalışmasını engellemeden önce genel davranışını gözlemlemek istediğiniz senaryolar için önerilir.
  - **Zorunlu kıl**: Bu modda uygulama denetimi çözümü kuralları zorunlu kılar ve çalışmasına izin verilmeyen uygulamaların engellenmesini sağlar.

   > [!NOTE]
   > -  **Zorunlu** yapılana kadar koruma modu devre dışı.
   > - Yukarıda belirtildiği gibi yeni uygulama denetimi ilkeleri her zaman *Denetim* modunda yapılandırılır. 
   >

4. Altında **İlkesi uzantısı**, izin vermek istediğiniz herhangi bir uygulama yolu ekleyebilirsiniz. Bu yolları eklediğinizde Güvenlik Merkezi VM'lerin seçilen grup içindeki VM'ler üzerinde uygulama whielisting İlkesi güncelleştirmeleri ve yerinde zaten kuralların yanı sıra, uygulamalar için uygun kuralları oluşturur.

5. Listelenen geçerli ihlallerini gözden geçirmek **son uyarılar** bölümü. Her satırda yönlendirilmesini tıklayın **uyarılar** sayfa içinde Azure Güvenlik Merkezi ve bağlantılı vm'lerdeki Azure Güvenlik Merkezi tarafından algılanan tüm uyarıları görüntüleyin.
  - **Uyarılar**: günlüğe kaydedilen herhangi bir ihlali.
  - **Hayır VM'lerin**: Bu uyarı türü sanal makinelerin sayısı.

6. Altında **beyaz listeye yayımcı ekleme kuralları**, **beyaz listeye yol ekleme kuralları**, ve **karma beyaz listeye ekleme kuralları** kurallardır, şu anda hangi uygulama beyaz listesini görebilirsiniz. bir grup içindeki VM'ler üzerinde bir kural koleksiyonu türünün göre yapılandırılmış. Her kural için görebilirsiniz:

  - **Kural**: belirli parametreleri olan göre bir uygulama bir uygulama çalıştırmasına izin verilip verilmeyeceğini belirlemek için AppLocker tarafından incelenir.
  - **Dosya türü**: belirli bir kuralı tarafından kapsanan dosya türleri. Bu, aşağıdakilerden biri olabilir: EXE, betik, MSI veya bunlardan herhangi bir sıralamaya dosya türleri.
  - **Kullanıcılar**: adı veya bir uygulama beyaz listeye ekleme kuralı tarafından kapsanan bir uygulamayı çalıştırmak için izin verilen kullanıcıların sayısı.

   ![Beyaz listeye ekleme kuralları](./media/security-center-adaptive-application/security-center-adaptive-application-fig9.png)

7. Kuralı silebilir veya izin verilen kullanıcıları düzenleyebilirsiniz istiyorsanız her satırın sonundaki üç noktaya tıklayın.

8. Değişiklik yapmadan sonra bir **Uyarlamalı uygulama denetimleri** İlkesi tıklayın **Kaydet**.

### <a name="not-recommended-list"></a>Önerilmeyenler listesi

Güvenlik Merkezi, yalnızca uygulama beyaz listeye ekleme ilkeleri kararlı bir uygulama kümesi çalıştıran sanal makineler için önerir. Bağlantılı VM'lerdeki uygulamalar sürekli değişiyorsa öneriler oluşturulmayacaktır.

![Öneri](./media/security-center-adaptive-application/security-center-adaptive-application-fig11.png)

Liste aşağıdakileri içerir:
- **AD**: Aboneliğin ve grubun adı
- **VM'ler**: Grup içindeki sanal makine sayısı

Azure Güvenlik Merkezi uygulama beyaz listeye ekleme ilkesi olmayan önerilen VM grupları üzerinde de tanımlamanızı sağlar. Daha önce bu gruplara göre bir uygulama beyaz listeye ekleme İlkesi yapılandırmak için açıklanan aynı ilkeler izleyin.


## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Azure Güvenlik Merkezi'ndeki uyarlamalı uygulama denetimlerini kullanarak Azure VM'lerinde çalışan uygulamaları beyaz listeye eklemeyi öğrendiniz. Azure Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve ele alma](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts). Güvenlik Merkezi’nde uyarıları yönetme ve güvenlik olaylarına yanıt vermeyi öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md). Azure kaynaklarınızı durumunu izleme hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'ndeki güvenlik uyarılarını anlama](https://docs.microsoft.com/azure/security-center/security-center-alerts-type). Farklı güvenlik uyarısı türleri hakkında bilgi edinin.
* [Azure Güvenlik Merkezi Sorun Giderme Kılavuzu](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide). Güvenlik Merkezi’nde sık karşılaşılan sorunları gidermeyi öğrenin.
* [Azure Güvenlik Merkezi SSS](security-center-faq.md). Hizmet kullanımı ile ilgili sık sorulan soruları bulun.
* [Azure Güvenlik Blogu](https://blogs.msdn.com/b/azuresecurity/). Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulun.
