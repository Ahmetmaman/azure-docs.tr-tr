---
title: Azure Data Box’ı ayarlama| Microsoft Docs
description: Azure Data Box’ınız için kabloları bağlamayı ve bağlanmayı öğrenin
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: tutorial
ms.date: 01/17/2019
ms.author: alkohli
ms.openlocfilehash: 460a05ac25ca4af56b81fb2025e0886c7bda3070
ms.sourcegitcommit: 9f07ad84b0ff397746c63a085b757394928f6fc0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2019
ms.locfileid: "54391154"
---
# <a name="tutorial-cable-and-connect-to-your-azure-data-box"></a>Öğretici: Kablo ve Azure Data Box için bağlanın

Bu öğreticide, Azure Data Box’ınız için kabloları bağlama, bağlanma ve çalıştırma işlemleri açıklanmaktadır.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Data Box’ınızın kablolarını bağlama
> * Data Box’ınıza bağlanma

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce aşağıdakilerden emin olun:

1. Tamamladığınızda [Öğreticisi: Azure Data Box sipariş](data-box-deploy-ordered.md).
2. Data Box’ınızı teslim aldınız ve portaldaki sipariş durumu **Teslim Edildi** oldu. 
    - Cihazın üzerindeki etiketin altındaki şeffaf bölüme yerleştirilmiş bir sevkiyat etiketi vardır. Bu etiketi iade için kullanmak üzere saklayın.
    - Bazı Avrupa bölgelerinde bir kutuda paketlenmiş cihaz alabilirsiniz. Cihazın ambalajını açmak ve Kaydet iade sevk irsaliyesi için kutusu emin olun.
3. [Data Box güvenlik yönergelerini](data-box-safety.md) gözden geçirdiniz.
4. 100 TB depolama cihazınızla birlikte kullanılacak bir topraklanmış güç kablosu aldınız.
5. Data Box üzerinden kopyalamak istediğiniz verileri içeren bir ana bilgisayarınız var. Ana bilgisayarınız:
    - [Desteklenen bir işletim sistemi](data-box-system-requirements.md) çalıştırılmalıdır.
    - Yüksek hızlı ağa bağlısınız. En az bir adet 10 GbE bağlantınızın olması önemle tavsiye edilir. 10 GbE bağlantı yoksa, 1 GbE veri bağlantısı kullanılabilir ancak kopyalama hızı etkilenir. 
6. Data Box’ı yerleştirebileceğiniz düz bir yüzeye erişiminiz olmalıdır. Cihazı standart bir rafa yerleştirmek istiyorsanız, veri merkezi rafınızda bir 7U yuvası olmalıdır. Cihazı düz veya dik şekilde rafa yerleştirebilirsiniz.
7. Data Box'ınızı ana bilgisayara bağlamak için aşağıdaki kabloları temin ettiniz.
    - İki 10 GbE SFP + Twinax kabloları (veri 1, veri 2 ağ arabirimi ile kullanın) Bakır. Bu arabirimiyle uyumlu olan kabloları çalışması gerekir böylece veri kutusu Mellanox ConnectX®-3 Pro tr çift bağlantı noktası 10GBASE T bağdaştırıcıları PCI Express 3.0 ağ arabirimi sahiptir. Örneğin, bir CISCO SFP H10GB CU3M 10GBASE CU TWINMAX SFP + 3 M kablo inhouse sınama amacıyla kullanılmıştır.
    - Bir RJ-45 CAT 6 ağ kablosu (MGMT ağ arabirimi ile kullanın)
    - Bir RJ-45 CAT 6A VEYA bir RJ-45 CAT 6 ağ kablosu (sırasıyla 10 Gb/sn veya 1 Gb/sn olarak yapılandırılmış DATA 3 ağ arabirimi ile birlikte kullanın)

## <a name="cable-your-device"></a>Cihazınızın kablolarını bağlama

Cihazınızın kablolarını bağlamak için aşağıdaki adımları uygulayın.

1. Üzerinde oynanıp oynanmadığını veya başka herhangi bir hasarı olup olmadığını anlamak için cihazı inceleyin. Cihaz kurcalanmış ya da ciddi hasar görmüşse devam etmeyin. Cihazın düzgün çalışır durumda olup olmadığını ve yerine başka bir cihazın gönderilmesinin gerekip gerekmediğini değerlendirmenize yardımcı olmaları için hemen Microsoft Desteği'ne başvurun.
2. Cihazı çalıştırılmasını istediğiniz konuma taşıyın. Cihazı düz bir yüzeye yerleştirin. Cihaz standart bir rafa da yerleştirilebilir.
3. Güç ve ağ kablolarını bağlayın. Genel bir yapılandırma için bağlı bir cihazın devre kartı aşağıda gösterilmiştir. 
    
    ![Kablo bağlantısı yapılmış Data Box cihaz devre kartı](media/data-box-deploy-set-up/data-box-cabled-dhcp.png)

    1. Güç kablosunu etiketli güç girişi konumuna bağlayın. Güç kablosunun diğer ucu bir güç dağıtım birimine bağlı olmalıdır.
    2. RJ-45 CAT 6 kablosunu kullanarak MGMT bağlantı noktasını bir uca, dizüstü bilgisayarı ise diğer uca bağlayın.            
    3. RJ-45 CAT 6A kablosunun bir ucunu DATA 3 bağlantı noktasına bağlayın. RJ-45 CAT 6A kablosu ile bağlarsanız DATA 3 10 GbE olarak, RJ-45 CAT 6 kablosuyla bağlarsanız 1 GbE olarak yapılandırılır.
    4. 10 GbE SFP+ Twinax bakır kablolarını kullanarak sırasıyla DATA 1 ve DATA 2 bağlantı noktalarını bağlayın. 
    5. Veri bağlantı noktalarından çıkan kabloların diğer ucu 10 GbE anahtar ile ana bilgisayara bağlanır.

4. Cihazın ön çalışma panelindeki güç düğmesini bulun. Cihazı açın.

    ![Data Box güç düğmesi](media/data-box-deploy-set-up/data-box-powered-door-open.png)

## <a name="connect-to-your-device"></a>Cihazınıza bağlanma

Yerel web kullanıcı arabirimi ve portal kullanıcı arabirimini kullanarak cihazınızı ayarlamak için aşağıdaki adımları uygulayın.

1. Cihaza 192.168.100.5 statik IP adresi ve 255.255.255.0 alt ağı ile bağlanmak için kullanmakta olduğunuz dizüstü bilgisayardaki Ethernet bağdaştırıcısını yapılandırın. 
2. Cihazınızı MGMT bağlantı noktasına bağlayın ve kendi yerel web kullanıcı Arabirimi https erişim\:/ / 192.168.100.10. Bu işlem, cihaz açıldıktan sonra 5 dakika sürebilir.
3. **Ayrıntılar**’a ve sonra **Web sayfasına gidin**’e tıklayın.

   ![Yerel web kullanıcı arabirimine bağlanma](media/data-box-deploy-set-up/data-box-connect-local-web-ui.png) 

4. Yerel web kullanıcı arabiriminin **Oturum açma** sayfasını görürsünüz. Portal kullanıcı arabiriminde ve yerel web kullanıcı arabiriminde cihaz seri numarasının eşleştiğinden emin olun. Bu noktada cihaz kilitlidir.
5. [Azure portal](https://portal.azure.com) oturum açın.
6. Portaldan cihaz kimlik bilgilerini indirin. **Genel > Cihaz ayrıntıları**’na gidin. **Cihaz parolası**’nı kopyalayın. Cihaz parolası, portalda belirli bir sıraya bağlıdır. 

    ![Cihaz kimlik bilgilerini alma](media/data-box-deploy-set-up/data-box-device-credentials.png)
    
    
7. Cihazın yerel web kullanıcı arabiriminde oturum açmak için önceki adımda Azure portalından aldığınız cihaz parolasını sağlayın. **Oturum aç**’a tıklayın.
8. **Pano**’da ağ arabirimlerinin yapılandırıldığından emin olun. 
    - Ortamınızda DHCP etkinse, ağ arabirimleri otomatik olarak yapılandırılır. 
    - DHCP etkin değilse, **Ağ arabirimlerini ayarla**’ya gidin ve gerekirse statik IP’ler atayın.

    ![Cihaz panosu](media/data-box-deploy-set-up/data-box-dashboard-1.png)

Veri ağı arabirimleri yapılandırıldıktan sonra DATA 1 - DATA 3 arabirimlerinden herhangi birinin IP adresini kullanarak `https://<IP address of a data network interface>` adresinden yerel web arabirimine erişebilirsiniz. 

Cihaz kurulumu tamamlandıktan sonra cihaz paylaşımlarına bağlanabilir ve verileri bilgisayarınızdan cihaza kopyalayabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide aşağıdaki Azure Data Box konularını öğrendiniz:

> [!div class="checklist"]
> * Data Box’ınızın kablolarını bağlama
> * Data Box’ınıza bağlanma

Data Box'ınıza verileri kopyalama hakkında bilgi edinmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Azure Data Box'a verilerinizi kopyalama](./data-box-deploy-copy-data.md)

