---
title: "SSH’yi Hadoop - Azure HDInsight ile Kullanma | Microsoft Docs"
description: "Secure Shell (SSH) kullanarak HDInsight&quot;a erişebilirsiniz. Bu belgede; Windows, Linux, Unix veya macOS istemcilerinden ssh ve scp komutlarını kullanarak HDInsight’a bağlanmaya ilişkin bilgi sağlanmıştır."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
keywords: "linux’taki hadoop komutları,hadoop linux komutları,hadoop macos,ssh hadoop,ssh hadoop kümesi"
ms.assetid: a6a16405-a4a7-4151-9bbf-ab26972216c5
ms.service: hdinsight
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/12/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive,hdiseo17may2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 17c4dc6a72328b613f31407aff8b6c9eacd70d9a
ms.openlocfilehash: 3eb1d4df7ab87ec692716339eb0ecb9df4c58732
ms.contentlocale: tr-tr
ms.lasthandoff: 05/16/2017


---
# <a name="connect-to-hdinsight-hadoop-using-ssh"></a>SSH kullanarak HDInsight’a (Hadoop) bağlanma

[Secure Shell (SSH)](https://en.wikipedia.org/wiki/Secure_Shell) kullanarak Azure HDInsight’ta Hadoop’a güvenli bir şekilde bağlanma hakkında bilgi edinin. 

HDInsight, Hadoop kümesi içindeki düğümler için işletim sistemi olarak Linux’ı (Ubuntu) kullanabilir. Aşağıdaki tablo, SSH istemcisi kullanılarak Linux tabanlı HDInsight’a bağlanılırken gereken adres ve bağlantı noktası bilgilerini içerir:

| Adres | Bağlantı noktası | Bağlandığı yer... |
| ----- | ----- | ----- |
| `<clustername>-ed-ssh.azurehdinsight.net` | 22 | Kenar düğümü (HDInsight üzerinde R Sunucusu) |
| `<edgenodename>.<clustername>-ssh.azurehdinsight.net` | 22 | Kenar düğümü (bir kenar düğümü varsa diğer küme türleri) |
| `<clustername>-ssh.azurehdinsight.net` | 22 | Birincil baş düğüm |
| `<clustername>-ssh.azurehdinsight.net` | 23 | İkincil baş düğüm |

> [!NOTE]
> `<edgenodename>` ifadesini kenar düğümünün adıyla değiştirin.
>
> `<clustername>` değerini kümenizin adıyla değiştirin.
>
> Kümeniz bir kenar düğümü içeriyorsa __kenar düğümüne her zaman SSH’yi kullanarak bağlanmanızı__ öneririz. Baş düğümler, Hadoop’un sistem durumu için kritik öneme sahip olan hizmetleri barındırır. Kenar düğümü yalnızca üzerine yerleştirdiğiniz öğeleri çalıştırır.
>
> Kenar düğümlerini kullanma hakkında daha fazla bilgi için bkz. [HDInsight’ta kenar düğümlerini kullanma](hdinsight-apps-use-edge-node.md#access-an-edge-node).

## <a name="ssh-clients"></a>SSH istemcileri

Linux, Unix ve macOS sistemleri `ssh` ve `scp` komutlarını sağlar. `ssh` istemcisi, yaygın olarak Linux veya Unix tabanlı bir sistemle uzak komut satırı oturumu oluşturmak için kullanılır. `scp` istemcisi, dosyaları istemciniz ve uzak sistem arasında güvenli bir şekilde kopyalamak için kullanılır.

Microsoft Windows, varsayılan olarak SSH istemcisi sağlamaz. `ssh` ve `scp` istemcileri, aşağıdaki paketler aracılığıyla Windows için kullanılabilir:

* [Azure Cloud Shell](../cloud-shell/quickstart.md): Cloud Shell, tarayıcınızda bir Bash ortamı sunar. Ayrıca `ssh` ve `scp` komutu ile diğer sık kullanılan Linux komutlarını sağlar.

* [Windows 10 üzerinde Ubuntu’da Bash](https://msdn.microsoft.com/commandline/wsl/about): `ssh` ve `scp` komutları, Windows üzerinde Bash komut satırı ile kullanılabilir.

* [Git (https://git-scm.com/)](https://git-scm.com/): `ssh` ve `scp` komutları, GitBash komut satırıyla kullanılabilir.

* [GitHub Masaüstü (https://desktop.github.com/)](https://desktop.github.com/) `ssh` ve `scp` komutları, GitHub Kabuğu komut satırıyla kullanılabilir. GitHub Masaüstü, Git Kabuğunun komut satırı olarak Bash, Windows Komut İstemi veya PowerShell kullanacak şekilde yapılandırılabilir.

* [OpenSSH (https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH)](https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH): PowerShell ekibi, OpenSSH’i Windows’a taşımakta ve test yayınları yapmaktadır.

    > [!WARNING]
    > OpenSSH paketi, `sshd` adlı SSH sunucu bileşenini içerir. Bu bileşen, sisteminizde başkalarının bağlanmasına izin verilen bir SSH sunucusu başlatır. Sisteminizde bir SSH sunucusu barındırmak istemiyorsanız, bu bileşeni yapılandırmayın veya 22 numaralı bağlantı noktasını açmayın. HDInsight ile iletişim kurulması gerekmez.

Ayrıca [PuTTY (http://www.chiark.greenend.org.uk/~sgtatham/putty/)](http://www.chiark.greenend.org.uk/~sgtatham/putty/) ve [MobaXterm (http://mobaxterm.mobatek.net/)](http://mobaxterm.mobatek.net/) gibi çeşitli grafiksel SSH istemcisi mevcuttur. Bu istemciler HDInsight’a bağlanmak için kullanılabilse de bağlanma işlemi `ssh` yardımcı programını kullanmaktan farklıdır. Daha fazla bilgi için, kullanmakta olduğunuz grafiksel istemcinin belgelerine bakın.

## <a id="sshkey"></a>Kimlik doğrulaması: SSH Anahtarları

SSH anahtarları, SSH oturumlarının kimliğini doğrulamak için [Ortak anahtar şifrelemesi](https://en.wikipedia.org/wiki/Public-key_cryptography) kullanır. SSH anahtarları parolalara göre daha güvenlidir ve Hadoop kümenize erişimin güvenliğini sağlamak için kolay bir yol sağlar.

SSH hesabınızın güvenliği bir anahtar yardımıyla sağlanıyorsa, bağlantı kurduğunuzda istemci eşleşen özel anahtarı sağlamalıdır:

* Çoğu istemci, bir __varsayılan anahtar__ kullanmak üzere yapılandırılmıştır. Örneğin, `ssh` istemcisi Linux ve Unix ortamlarında `~/.ssh/id_rsa` üzerinde bir özel anahtar arar.

* __Özel anahtarın yolunu__ belirtebilirsiniz. `ssh` istemcisi ile özel anahtarın yolunu belirtmek için `-i` parametresi kullanılır. Örneğin, `ssh -i ~/.ssh/id_rsa sshuser@myedge.mycluster-ssh.azurehdinsight.net`.

* Farklı sunucularla kullanılacak __birden fazla özel anahtarınız__ varsa [ssh-agent (https://en.wikipedia.org/wiki/Ssh-agent)](https://en.wikipedia.org/wiki/Ssh-agent) gibi bir yardımcı program kullanmayı deneyin. `ssh-agent` yardımcı programı, SSH oturumu oluşturulurken kullanılacak anahtarı otomatik olarak seçmek için kullanılabilir.

> [!IMPORTANT]
>
> Özel anahtarınızın güvenliğini şifre ile sağlıyorsanız, anahtarı kullanmak için şifreyi girmeniz gerekir. `ssh-agent` gibi yardımcı programlar, size kolaylık sağlamak için parolayı önbelleğe alabilir.

### <a name="create-an-ssh-key-pair"></a>SSH anahtar çifti oluşturma

Ortak ve özel anahtar dosyaları oluşturmak için `ssh-keygen` komutunu kullanın. Aşağıdaki komut, HDInsight ile kullanılabilecek bir 2048-bit RSA anahtar çifti oluşturur:

    ssh-keygen -t rsa -b 2048

Anahtar oluşturma işlemi sırasında sizden bilgiler istenir. Örneğin, anahtarların nerede depolanacağı veya şifre kullanılıp kullanılmayacağı. İşlem tamamlandıktan sonra biri ortak anahtar, diğeri özel anahtar olmak üzere iki dosya oluşturulur.

* __Ortak anahtar__ bir HDInsight kümesi oluşturmak için kullanılır. Ortak anahtar `.pub` uzantısına sahiptir.

* __Özel anahtar__, HDInsight kümesinde istemcinizin kimliğini doğrulamak için kullanılır.

> [!IMPORTANT]
> Anahtarlarınızın güvenliğini şifre ile sağlayabilirsiniz. Parola, aslında özel anahtarınız üzerindeki bir şifredir. Özel anahtarınız başkası tarafından ele geçirilirse, anahtarın kullanılması için şifrenin girilmesi gerekir.

### <a name="create-hdinsight-using-the-public-key"></a>Ortak anahtar kullanarak HDInsight oluşturma

| Oluşturma yöntemi | Ortak anahtarı kullanma |
| ------- | ------- |
| **Azure portal** | __Küme oturumu açmak için kullanılan parolayı kullan__ seçeneğinin işaretini kaldırın ve ardından SSH kimlik doğrulama türü olarak __Ortak Anahtar__’ı seçin. Son olarak, ortak anahtar dosyasını seçin veya dosyanın metin içeriğini __SSH ortak anahtarı__ alanına yapıştırın.</br>![HDInsight küme oluşturma işleminde SSH ortak anahtarı iletişim kutusu](./media/hdinsight-hadoop-linux-use-ssh-unix/create-hdinsight-ssh-public-key.png) |
| **Azure PowerShell** | `New-AzureRmHdinsightCluster` cmdlet'inin `-SshPublicKey` parametresini kullanarak, ortak anahtarın içeriğini dize olarak geçirin.|
| **Azure CLI 1.0** | `azure hdinsight cluster create` komutunun `--sshPublicKey` parametresini kullanarak, ortak anahtarın içeriğini dize olarak geçirin. |
| **Resource Manager Şablonu** | SSH anahtarlarını şablonla kullanma örneği için bkz. [HDInsight’ı SSH anahtarı ile Linux’a dağıtma](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-ssh-publickey/). [azuredeploy.json](https://github.com/Azure/azure-quickstart-templates/blob/master/101-hdinsight-linux-ssh-publickey/azuredeploy.json) dosyasında `publicKeys` öğesi, kümeyi oluştururken Azure’a anahtarları geçirmek için kullanılır. |

## <a id="sshpassword"></a>Kimlik doğrulaması: Parola

SSH hesaplarının güvenliği bir parola kullanılarak sağlanabilir. SSH kullanarak HDInsight’a bağlandığınızda, parola girmeniz istenir.

> [!WARNING]
> SSH için parola kimlik doğrulamasının kullanılması önerilmez. Parolalar tahmin edilebilir ve deneme yanılma saldırılarına karşı savunmasızdır. Bunun yerine, [kimlik doğrulaması için SSH anahtarları](#sshkey) kullanmanız önerilir.

### <a name="create-hdinsight-using-a-password"></a>Parola kullanarak HDInsight oluşturma

| Oluşturma yöntemi | Parola belirtme |
| --------------- | ---------------- |
| **Azure portal** | Varsayılan olarak, SSH kullanıcı hesabı ile küme oturum açma hesabı aynı parolaya sahiptir. Farklı bir parola kullanmak için __Küme oturumu açmak için kullanılan parolayı kullan__ seçeneğinin işaretini kaldırın ve __SSH parolası__ alanına parolayı girin.</br>![HDInsight küme oluşturma işleminde SSH parolası iletişim kutusu](./media/hdinsight-hadoop-linux-use-ssh-unix/create-hdinsight-ssh-password.png)|
| **Azure PowerShell** | `New-AzureRmHdinsightCluster` cmdlet’inin `--SshCredential` parametresini kullanın ve SSH kullanıcı hesabı adı ile parolasını içeren bir `PSCredential` nesnesi geçirin. |
| **Azure CLI 1.0** | `azure hdinsight cluster create` komutunun `--sshPassword` parametresini kullanarak parola değerini belirtin. |
| **Resource Manager Şablonu** | Parolayı şablonla kullanma örneği için bkz. [HDInsight’ı SSH parolası ile Linux’a dağıtma](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-ssh-password/). [azuredeploy.json](https://github.com/Azure/azure-quickstart-templates/blob/master/101-hdinsight-linux-ssh-password/azuredeploy.json) dosyasındaki `linuxOperatingSystemProfile` öğesi, kümeyi oluştururken SSH hesabı adı ile parolasını Azure’a geçirmek için kullanılır.|

### <a name="change-the-ssh-password"></a>SSH parolasını değiştirme

SSH kullanıcı hesabı parolasını değiştirme hakkında bilgi için, [HDInsight’ı Yönetme](hdinsight-administer-use-portal-linux.md#change-passwords) belgesinin __Parolaları değiştirme__ bölümüne bakın.

## <a id="domainjoined"></a>Kimlik doğrulama: Etki alanına katılmış HDInsight

__Etki alanına katılmış HDInsight kümesi__ kullanıyorsanız, SSH ile bağlantı kurduktan sonra `kinit` komutunu kullanmanız gerekir. Bu komut sizden bir etki alanı kullanıcı adı ile parolası ister ve kümenizle ilişkili Azure Active Directory etki alanını kullanarak oturumunuzun kimliğini doğrular.

Daha fazla bilgi için bkz. [Etki alanına katılmış HDInsight yapılandırma](hdinsight-domain-joined-configure.md).

## <a name="connect-to-worker-and-zookeeper-nodes"></a>Çalışan ve Zookeeper düğümlerine bağlanma

Çalışan düğümlerine ve Zookeeper düğümlerine doğrudan internetten erişilemez. Bunlara, küme baş düğümleri veya kenar düğümlerinden erişilebilir. Diğer düğümlere bağlanmak için uygulamanız gereken genel adımlar şunlardır:

1. SSH kullanarak bir baş veya kenar düğümüne bağlanın:

        ssh sshuser@myedge.mycluster-ssh.azurehdinsight.net

2. Baş veya kenar düğümüne yaptığınız SSH bağlantısında `ssh` komutunu kullanarak kümedeki bir çalışan düğümüne bağlanın:

        ssh sshuser@wn0-myhdi

    Kümedeki düğümlerin etki alanı adlarının listesini almak için [Ambari REST API’yi kullanarak HDInsight’ı yönetme](hdinsight-hadoop-manage-ambari-rest-api.md#example-get-the-fqdn-of-cluster-nodes) belgesine bakın.

SSH hesabının güvenliği bir __parola__ kullanılarak sağlanıyorsa bağlanırken parolayı girin.

SSH hesabının güvenliği __SSH anahtarları__ kullanılarak sağlanıyorsa istemciden SSH iletmenin etkinleştirildiğinden emin olun.

> [!NOTE]
> Kümedeki tüm düğümlere doğrudan erişmenin başka bir yolu ise HDInsight’ın bir Azure Sanal Ağına bağlanmasıdır. Bundan sonra, uzak makinenizi aynı sanal ağa bağlayabilir ve kümedeki tüm düğümlere doğrudan erişebilirsiniz.
>
> Daha fazla bilgi için bkz. [HDInsight ile sanal ağ kullanma](hdinsight-extend-hadoop-virtual-network.md).

### <a name="configure-ssh-agent-forwarding"></a>SSH aracı iletmeyi yapılandırma

> [!IMPORTANT]
> Aşağıdaki adımlar, Linux veya UNIX tabanlı bir sistem için geçerlidir ve Windows 10 üzerinde Bash ile birlikte çalışır. Bu adımlar sisteminizde çalışmazsa SSH istemcinizin belgelerine bakmanız gerekebilir.

1. Bir metin düzenleyicisiyle `~/.ssh/config` dosyasını açın. Bu dosya yoksa, komut satırında `touch ~/.ssh/config` girerek oluşturabilirsiniz.

2. Aşağıdakileri metni `config` dosyasına ekleyin.

        Host <edgenodename>.<clustername>-ssh.azurehdinsight.net
          ForwardAgent yes

    __Ana bilgisayar__ bilgisini, SSH kullanarak bağlandığınız düğümün adresiyle değiştirin. Önceki örnekte kenar düğümü kullanılmıştır. Bu giriş, belirtilen düğüm için SSH aracı iletmeyi yapılandırır.

3. Terminalde aşağıdaki komutu kullanarak, SSH aracı iletmeyi test edin:

        echo "$SSH_AUTH_SOCK"

    Bu komutun aşağıdaki metne benzer bilgiler döndürmesi gerekir:

        /tmp/ssh-rfSUL1ldCldQ/agent.1792

    Hiçbir şeyin döndürülmemesi, `ssh-agent` özelliğinin çalışmadığını gösterir. Daha fazla bilgi için [ssh ile ssh-agent kullanma (http://mah.everybody.org/docs/ssh)](http://mah.everybody.org/docs/ssh) sayfasındaki aracı başlatma komut dosyası bilgilerine bakın veya SSH istemcinizin belgelerine başvurun.

4. **ssh aracı**nın çalıştığını doğruladıktan sonra, SSH özel anahtarınızı aracıya eklemek için aşağıdakini kullanın:

        ssh-add ~/.ssh/id_rsa

    Özel anahtarınızı farklı bir dosyada saklanıyorsa, `~/.ssh/id_rsa` ile dosyanın yolunu değiştirin.

5. SSH kullanarak küme kenar düğümüne veya baş düğümlerine bağlanın. Ardından, SSH komutunu kullanarak bir çalışan veya zookeeper düğümüne bağlanın. İletilen anahtar kullanılarak bağlantı kurulur.

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight ile SSH tüneli kullanma](hdinsight-linux-ambari-ssh-tunnel.md)
* [HDInsight ile sanal ağ kullanma](hdinsight-extend-hadoop-virtual-network.md)
* [HDInsight’ta kenar düğümlerini kullanma](hdinsight-apps-use-edge-node.md#access-an-edge-node)
