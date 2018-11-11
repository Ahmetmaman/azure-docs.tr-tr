---
title: Azure HDInsight erişim için tünel SSH kullanma
description: Linux tabanlı HDInsight düğümlerinizi üzerinde barındırılan web kaynaklarına güvenli bir şekilde göz atmak için bir SSH tüneli kullanmayı öğrenin.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 04/30/2018
ms.author: hrasheed
ms.openlocfilehash: 0be2895bf08cc7d6aa0b2e55b62b2d6705b27725
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51007305"
---
# <a name="use-ssh-tunneling-to-access-ambari-web-ui-jobhistory-namenode-oozie-and-other-web-uis"></a>Ambari web kullanıcı Arabirimi, JobHistory, NameNode, Oozie ve diğer web kullanıcı arabirimlerine erişim için SSH tünel oluşturmayı kullanma

HDInsight kümeleri Ambari web kullanıcı Arabirimi Internet üzerinden erişmeyi, ancak bazı özellikler bir SSH tüneli gerektirir. Örneğin, web kullanıcı Arabirimi Oozie hizmeti için bir SSh tüneli olmaksızın internet üzerinden erişilemez.

## <a name="why-use-an-ssh-tunnel"></a>Neden bir SSH tüneli kullanma

Ambari menülerde bazıları yalnızca SSH tüneli çalışır. Web siteleri ve alt düğümleri gibi diğer düğüm türleri üzerinde çalışan hizmetleri bu menüler dayanır.

Aşağıdaki Web kullanıcı arabirimleri SSH tüneli gerektirir:

* JobHistory
* NameNode
* İş parçacığı yığınları
* Oozie web kullanıcı Arabirimi
* HBase Master ve günlükleri kullanıcı Arabirimi

Betik eylemleri, kümenizin özelleştirmek için kullandığınız herhangi bir hizmet veya bir web hizmeti kullanıma sunmak, yüklediğiniz yardımcı programları SSH tüneli gerektirir. Örneğin, bir betik eylemi kullanarak Hue yüklemek, Hue web kullanıcı arabirimini erişmek için bir SSH tüneli kullanmanız gerekir.

> [!IMPORTANT]
> HDInsight için bir sanal ağ üzerinden doğrudan erişimi varsa, SSH tünelleri kullanın gerekmez. HDInsight üzerinden bir sanal ağa doğrudan erişim ilişkin bir örnek için bkz [HDInsight'ı şirket içi ağınıza bağlama](connect-on-premises-network.md) belge.

## <a name="what-is-an-ssh-tunnel"></a>SSH tüneli nedir

[Güvenli Kabuk (SSH) tüneli](https://en.wikipedia.org/wiki/Tunneling_protocol#Secure_Shell_tunneling) yerel makinenizde bir bağlantı noktası bir HDInsight baş düğümüne bağlanır. Yerel bağlantı noktasına gönderilen trafik, baş düğüm için bir SSH bağlantısı üzerinden yönlendirilir. Bu baş düğümünde oluşturulmuş gibi istek çözülür. Yanıt, ardından geri istasyonunuzu tüneli üzerinden yönlendirilir.

## <a name="prerequisites"></a>Önkoşullar

* Bir SSH istemcisi. Çoğu işletim sistemi ile bir SSH istemcisi sağlar `ssh` komutu. Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).

* SOCKS5 Ara sunucusunu kullanacak şekilde yapılandırılmış bir web tarayıcısı.

    > [!WARNING]
    > Windows Internet ayarlarına yerleşik SOCKS proxy desteği SOCKS5 desteklemiyor ve bu belgedeki adımlar ile çalışmıyor. Aşağıdaki tarayıcılardan Windows proxy ayarlarını kullanan ve şu anda bu belgedeki adımlar ile çalışmaz:
    >
    > * Microsoft Edge
    > * Microsoft Internet Explorer
    >
    > Google Chrome, ayrıca Windows proxy ayarlarını kullanır. Ancak, SOCKS5 destekleyen uzantıları yükleyebilirsiniz. Öneririz [FoxyProxy standart](https://chrome.google.com/webstore/detail/foxyproxy-standard/gcknhkkoolaabfmlnjonogaaifnjlfnp).

## <a name="usessh"></a>SSH komutunu kullanarak bir tünel oluşturma

SSH oluşturmak için aşağıdaki komutu kullanarak tünel kullanımı `ssh` komutu. Değiştirin **sshuser** değiştirin ve HDInsight kümesi için bir SSH kullanıcısı ile **clustername** HDInsight kümenizin adıyla:

```bash
ssh -C2qTnNf -D 9876 sshuser@clustername-ssh.azurehdinsight.net
```

Bu komut, yerel bağlantı 9876 kümeye SSH üzerinden trafiği yönlendiren bir bağlantı oluşturur. Seçenekler şunlardır:

* **D 9876** -Tünel üzerinden trafiği yönlendiren yerel bağlantı noktası.
* **C** -web trafiğini çoğunlukla metin olduğundan tüm verileri sıkıştırın.
* **2** -force Protokolü sürüm 2 yalnızca denemek için SSH.
* **q** -Sessiz mod.
* **T** -yalnızca bir bağlantı noktası ilettiğiniz beri sözde tty ayırma devre dışı.
* **n** -yalnızca bir bağlantı noktası ilettiğiniz beri STDIN, okunması engelle.
* **N** -yalnızca bir bağlantı noktası ilettiğiniz olduğundan bir uzak komutu yürütülmez.
* **f** -arka planda çalıştırın.

Komut bittikten sonra yerel bilgisayarda 9876 numaralı bağlantı noktasına gönderilen trafik küme baş düğümüne yönlendirilir.

## <a name="useputty"></a>PuTTY kullanarak tünel oluşturma

[PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty) grafiksel bir SSH istemcisi Windows için. PuTTY kullanarak SSH tüneli oluşturmak için aşağıdaki adımları kullanın:

1. Putty'yi açın ve bağlantı bilgilerinizi girin. PuTTY ile ilgili bilgi sahibi değilseniz bkz [PuTTY belgeleri (http://www.chiark.greenend.org.uk/~sgtatham/putty/docs.html)](http://www.chiark.greenend.org.uk/~sgtatham/putty/docs.html).

2. İçinde **kategori** için iletişim kutusunun sol bölümünde, genişletmek **bağlantı**, genişletme **SSH**ve ardından **tüneller**.

3. Aşağıdaki bilgileri sağlamaları **SSH denetleme seçenekleri bağlantı noktası iletme** formu:
   
   * **Kaynak bağlantı noktası** - İletmek istediğiniz istemci üzerindeki bağlantı noktası. Örneğin, **9876**.

   * **Hedef** -Linux tabanlı HDInsight kümesi için SSH adresi. Örneğin, **mycluster-ssh.azurehdinsight.net**.

   * **Dinamik** - Dinamik SOCKS proxy yönlendirmesini etkinleştirir.
     
     ![tünel oluşturma seçenekleri resmi](./media/hdinsight-linux-ambari-ssh-tunnel/puttytunnel.png)

4. Tıklayın **Ekle** ayarları eklemek ve ardından **açın** bir SSH bağlantısı açın.

5. İstendiğinde, sunucuda oturum açın.

## <a name="use-the-tunnel-from-your-browser"></a>Tarayıcınızdan tüneli kullanma

> [!IMPORTANT]
> Aynı proxy ayarları tüm platformlarda sağladığı gibi Mozilla FireFox tarayıcısı bu bölümdeki adımları kullanın. Google Chrome gibi modern tarayıcılar gibi FoxyProxy tünel ile çalışmak için bir uzantı gerektirebilir.

1. Kullanılacak tarayıcı yapılandırma **localhost** ve ne zaman kullanılan bağlantı noktası olarak tünel oluşturma bir **SOCKS v5** proxy. İşte Firefox ayarları şöyle görünür. Farklı bir bağlantı noktasıyla 9876 kullandıysanız, kullandığınız bir bağlantı noktasını değiştirin:
   
    ![Firefox ayarlarının görüntüsü](./media/hdinsight-linux-ambari-ssh-tunnel/firefoxproxy.png)
   
   > [!NOTE]
   > Seçme **uzak DNS** HDInsight kümesi kullanarak etki alanı adı sistemi (DNS) istekleri giderir. Bu ayar kümesinin baş düğümünü kullanarak DNS çözümleniyor.

2. Tünel bir siteyi ziyaret ederek gibi çalıştığını doğrulamak [ http://www.whatismyip.com/ ](http://www.whatismyip.com/). Döndürülen IP, bir Microsoft Azure veri merkezi tarafından kullanılan olmalıdır.

## <a name="verify-with-ambari-web-ui"></a>Ambari web kullanıcı Arabirimi ile doğrulama

Küme oluşturulduktan sonra Ambari Web hizmeti web kullanıcı arabirimleri erişebildiğinizi doğrulamak için aşağıdaki adımları kullanın:

1. Tarayıcınızda, Git http://headnodehost:8080. `headnodehost` Adresi çözümleme Ambari çalıştıran baş düğüme ve küme tüneli üzerinden gönderilir. İstendiğinde, kümeniz için yönetici kullanıcı adını (Yönetici) ve parolasını girin. Ambari web kullanıcı arabirimini ikinci kez istenebilir. Öyleyse, bilgileri yeniden girin.

   > [!NOTE]
   > Kullanırken http://headnodehost:8080 Tünel üzerinden bağlanıyorsanız, kümeye bağlanmak için adres. SSH tüneli yerine HTTPS kullanarak iletişim sağlanır. İnternet üzerinden HTTPS kullanarak bağlanmak için https://clustername.azurehdinsight.netburada **clustername** kümenin adıdır.

2. Ambari Web kullanıcı arabirimini HDFS sayfasının sol taraftaki listeden seçin.

    ![Görüntü seçildi HDFS ile](./media/hdinsight-linux-ambari-ssh-tunnel/hdfsservice.png)

3. HDFS hizmet bilgilerini görüntülendiğinde seçin **hızlı bağlantılar**. Bir kümenin baş düğümlerinden listesi görüntülenir. Baş düğümler birini seçin ve ardından **NameNode UI**.

    ![Genişletilmiş QuickLinks menüsünde görüntü](./media/hdinsight-linux-ambari-ssh-tunnel/namenodedropdown.png)

   > [!NOTE]
   > Seçtiğinizde, __hızlı bağlantılar__, bekleme göstergesi alabilirsiniz. Yavaş bir internet bağlantınız varsa bu durum ortaya çıkabilir. Bir veya iki sunucudan alınacak veri için dakika bekleyin ve listeyi yeniden deneyin.
   >
   > Bazı girişleri **hızlı bağlantılar** menü ekranın sağ tarafındaki tarafından kesilmiş. Bu durumda, farenizi kullanarak menüsünü genişletin ve ekran menü kalanını görmek için sağa kaydırmak için sağ ok tuşunu kullanın.

4. Aşağıdaki görüntüye benzer bir sayfa görüntülenir:

    ![Görüntü NameNode kullanıcı arabirimi](./media/hdinsight-linux-ambari-ssh-tunnel/namenode.png)

   > [!NOTE]
   > Bu sayfa URL'si dikkat edin. aşağıdakine benzer olmalıdır **http://hn1-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:8088/cluster**. Bu URI düğümünün dahili tam etki alanı adı (FQDN) kullanıyor ve bir SSH tüneli kullanırken yalnızca erişilebilir.

## <a name="next-steps"></a>Sonraki adımlar

Oluşturma ve bir SSH tüneli kullanma öğrendiniz, aşağıdaki belge Ambari kullanılacak diğer yolları için bkz:

* [Ambari kullanarak HDInsight kümelerini yönetme](hdinsight-hadoop-manage-ambari.md)

HDInsight ile SSH kullanma hakkında daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).

