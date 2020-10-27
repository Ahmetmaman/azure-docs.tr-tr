---
author: rayne-wiselman
ms.service: site-recovery
ms.topic: include
ms.date: 10/26/2018
ms.author: raynew
ms.openlocfilehash: 31e61069c95be9bd1c7a684bb83ebcd93bcb14be
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2020
ms.locfileid: "92548260"
---
1. Azure Site Recovery UnifiedSetup.exe dosyasını başlatın
2. **Başlamadan önce** kısmında **Dağıtımı genişletmek için ek işlem sunucuları ekleyin** ’i seçin.

   ![İşlem sunucusu ekleme](./media/site-recovery-add-process-server/ps-page-1.png)

3. **Yapılandırma Sunucusu Ayrıntıları** ’nda Yapılandırma Sunucusunun IP adresini ve parolayı belirtin.

   ![İşlem sunucusu ekleme 2](./media/site-recovery-add-process-server/ps-page-2.png)
4. **Internet ayarları** ' nda, yapılandırma sunucusunda çalışan sağlayıcının ınternet üzerinden Azure Site Recovery nasıl bağlandığını belirtin.

   ![İşlem sunucusu ekleme 3](./media/site-recovery-add-process-server/ps-page-3.png)

   * O anda makine üzerinde ayarlanmış olan bir ara sunucu ile bağlanmak isterseniz **Var olan ara sunucu ayarlarıyla bağlan** seçeneğini belirleyin.
   * Sağlayıcı'nın doğrudan bağlanmasını istiyorsanız **Ara sunucu olmadan doğrudan bağlan** seçeneğini belirleyin.
   * Var olan ara sunucu kimlik doğrulaması gerektiriyorsa veya Sağlayıcı bağlantısı için özel bir ara sunucu kullanmak istiyorsanız **Özel ara sunucu ayarlarıyla bağlan** seçeneğini belirleyin.

     * Özel bir ara sunucu kullanırsanız adresi, bağlantı noktasını ve kimlik bilgilerini belirtmeniz gerekir.
     * Ara sunucu kullanıyorsanız hizmet URL'lerine izin vermiş olmanız gerekir.

5. **Önkoşul Denetimi** menüsünde Kurulum, yüklemenin çalışabildiğinden emin olmak üzere bir denetim gerçekleştirir. **Genel saat eşitleme denetimi** hakkında bir uyarı görünürse, sistem saatindeki zamanın ( **Tarih ve Saat** ayarları) saat dilimiyle aynı olduğunu doğrulayın.

     ![İşlem sunucusu ekleme 4](./media/site-recovery-add-process-server/ps-page-4.png)

6. **Ortam Ayrıntıları** ’nda VMware sanal makinelerini çoğaltıp çoğaltmayacağınızı seçin. Çoğaltacaksanız, kurulum PowerCLI 6.0’ın yüklü olup olmadığını denetler.

     ![İşlem sunucusu ekleme 5](./media/site-recovery-add-process-server/ps-page-5.png)

7. **Yükleme Konumu** alanında ikili dosyaları yüklemek ve önbelleği depolamak istediğiniz konumu seçin. Seçtiğiniz sürücü en az 5 GB kullanılabilir disk alanına sahip olmalıdır, ancak en az 600 GB boş alanı olan bir önbellek sürücüsü seçmeniz önerilir.
     ![İkili dosyaların ve önbellek depolamanın yükleme konumunu gösteren ekran görüntüsü.](./media/site-recovery-add-process-server/ps-page-6.png)

8. **Ağ seçimi** ' nde, yapılandırma sunucusunun çoğaltma verilerini gönderdiği ve aldığı dinleyiciyi (ağ BAĞDAŞTıRıCıSı ve SSL bağlantı noktası) belirtin. Bağlantı noktası 9443, çoğaltma trafiğini gönderip almak için kullanılan varsayılan bağlantı noktasıdır, ancak bu bağlantı noktası numarasını ortamınızın gereksinimlerine uyacak şekilde değiştirebilirsiniz. Bağlantı noktası 9443’e ek olarak, çoğaltma işlemlerini düzenlemek için web sunucusu tarafından kullanılan bağlantı noktası 443 de açılır. Bağlantı noktası 443’ü çoğaltma trafiği göndermek veya almak için kullanmayın.

     ![İşlem sunucusu ekleme 6](./media/site-recovery-add-process-server/ps-page-7.png)
9. **Özet** alanındaki bilgileri gözden geçirin ve **Yükle** ’ye tıklayın. Yükleme tamamlandığında bir parola oluşturulur. Çoğaltmayı etkinleştirdiğinizde bu parola gerekli olacaktır; bu yüzden kopyalayıp güvenli bir yerde saklayın.

     ![İşlem sunucusu ekleme 7](./media/site-recovery-add-process-server/ps-page-8.png)
