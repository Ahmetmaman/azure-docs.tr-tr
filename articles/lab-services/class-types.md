---
title: Azure Lab Services üzerinde örnek sınıf türleri | Microsoft Docs
description: ', Azure Lab Services kullanarak laboratuvarları ayarlayabileceğiniz bazı sınıf türlerini sağlar.'
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: 7ebc89c040be732c230f0c3f54f68fb91466eb68
ms.sourcegitcommit: 5e5a0abe60803704cf8afd407784a1c9469e545f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2020
ms.locfileid: "96436411"
---
# <a name="class-types-overview---azure-lab-services"></a>Sınıf türlerine genel bakış-Azure Lab Services

Azure Lab Services, bulutta ders Laboratuvarı ortamlarını hızlıca ayarlamanıza olanak sağlar. Bu bölümdeki makalelerde, Azure Lab Services kullanarak birkaç tür laboratuvarın nasıl ayarlanacağı hakkında rehberlik sağlanır.

## <a name="big-data-analytics"></a>Büyük veri analizi
Büyük bir veri analizi sınıfı öğretmek için bir GPU Laboratuvarı ayarlayabilirsiniz. Bu tür bir sınıfla, öğrenciler büyük hacimlere yönelik verileri nasıl işleyeceğinizi ve veri öngörüleri türetmede makine ve istatistiksel öğrenme algoritmaları uygulamayı öğreniyor. Öğrenciler için önemli bir amaç, büyük verileri depolamak, yönetmek ve işlemek için araçlar sağlayan Apache Hadoop açık kaynaklı yazılım paketi gibi veri analizi araçlarını kullanmayı öğrenmektir. 

Bu tür laboratuvar kurulumu hakkında ayrıntılı bilgi için bkz. [Hortonçalışmalar veri platformunun Docker dağıtımını kullanarak büyük veri analizi için laboratuvar ayarlama](class-type-big-data-analytics.md).

## <a name="database-management"></a>Veritabanı yönetimi
Veritabanları kavramlarından biri, üniversitenin çoğu bilgisayar bilimi departmanlarından biridir. Azure Lab Services bir temel veritabanı yönetim sınıfı için laboratuvar ayarlayabilirsiniz. Örneğin, bir sanal makine şablonunu [MySQL](https://www.mysql.com/) veritabanı sunucusu veya [SQL Server 2019](https://www.microsoft.com/sql-server/sql-server-2019) sunucusuyla bir laboratuvarda ayarlayabilirsiniz.

Bu tür Laboratuvarı ayarlama hakkında ayrıntılı bilgi için bkz. [ilişkisel veritabanları için veritabanı yönetimini öğretmek için laboratuvar ayarlama](class-type-database-management.md).

## <a name="deep-learning-in-natural-language-processing"></a>Doğal dilde işleme içinde derin öğrenme
Azure Lab Services kullanarak doğal dil işleme (NLP) için derin öğrenmeye odaklanan bir laboratuvar ayarlayabilirsiniz. Doğal dil işleme (NLP), çeviri, konuşma tanıma ve diğer dil anlama özelliklerine sahip bilgisayarları sağlayan yapay zeka (AI) biçimidir. Bir NLP sınıfı alan öğrenciler bir Linux sanal makinesi (VM) alır ve yazılı insan dilini çözümlemek için kullanılan derin öğrenme modelleri geliştirmek üzere sinir ağ algoritmalarının nasıl uygulanacağını öğrenin.

Bu tür Laboratuvarı ayarlama hakkında ayrıntılı bilgi için, bkz. [Azure Lab Services kullanarak doğal dil işlemede derin öğrenmeye odaklanan bir laboratuvar ayarlama](class-type-deep-learning-natural-language-processing.md).

## <a name="ethical-hacking"></a>Etik korsanlık
Ahlak hackinin çatalına odaklanan bir sınıf için laboratuvar ayarlayabilirsiniz. Ahlak hacham topluluk tarafından kullanılan bir uygulama olan sızma testi, birisi kötü niyetli bir saldırganın yararlanmasına yönelik güvenlik açıklarını göstermek üzere sisteme veya ağa erişim kazanmayı denediğinde oluşur.

Ahlak hacklik bir sınıfta öğrenciler, güvenlik açıklarına karşı savunma için modern teknikler öğrenmesini sağlayabilir. Her öğrenci, iki iç içe sanal makineye sahip bir sanal makine olan [Metasploitable3](https://github.com/rapid7/metasploitable3) görüntüsüne sahip bir sanal makine ve [kalı Linux](https://www.kali.org/) görüntüsüne sahip başka bir makine içeren bir Windows Server konak sanal makinesi alır Metasplosever sanal makinesi, yararlanmak amacıyla kullanılır.  Kalı Linux sanal makinesi, adli görevlerini yürütmek için gereken araçlara erişim sağlar.

Bu tür laboratuvarın nasıl ayarlanacağı hakkında ayrıntılı bilgi için bkz. [ahlak hacme sınıfı öğretmek için laboratuvar ayarlama](class-type-ethical-hacking.md).

## <a name="matlab"></a>MATLAB
Matris Laboratuvarı temsil eden [MATLAB](https://www.mathworks.com/products/matlab.html), [MathWorks](https://www.mathworks.com/)'den programlama platformudur.  İşlem gücünü ve görselleştirmeyi, Math, mühendislik, fizik ve Chemistry alanlarında popüler bir araç haline getirir.

Bu tür laboratuvarın nasıl ayarlanacağı hakkında ayrıntılı bilgi için bkz. [bir laboratuvarı, MATLAB öğretmek için](class-type-matlab.md)ayarlama.

## <a name="project-lead-the-way-pltw"></a>Proje için yol (PLTW)
[Proje lideri (PLTW)](https://www.pltw.org/) , bilgisayar bilimi, mühendislik ve biyotıp bilimi içindeki Birleşik Devletler Için prek-12 ' yi sağlayan kar amacı gütmeyen bir kuruluştur.  Öğrenciler her bir PLTW sınıfında, uygulamalı öğrenme deneyiminin bir parçası olarak çeşitli yazılım uygulamaları kullanır.

Bu laboratuvar türlerini nasıl ayarlayacağından ilgili ayrıntılı bilgi için bkz. [proje için laboratuvarları ayarlama sınıfları](class-type-pltw.md).

## <a name="python-and-jupyter-notebooks"></a>Python ve Jupyıter Not defterleri
Öğrencilerle, [jupi not defterlerini](http://jupyter-notebook.readthedocs.io)nasıl kullanacağınızı öğretmek için gereken araçlarla Azure Lab Services bir şablon makinesi ayarlayabilirsiniz. Jupyter Not defterleri, zengin metin ve yürütülebilir [Python](https://www.python.org/) kaynak kodunu not defteri adlı tek bir tuval üzerinde kolayca birleştirebilmenizi sağlayan açık kaynaklı bir projem. Bir not defteri çalıştırmak, giriş ve çıkışları doğrusal bir kaydıyla sonuçlanır.  Bu çıktılar metin, bilgi tabloları, dağılım çizimleri ve daha fazlasını içerebilir.

Bu tür laboratuvar kurulumu hakkında ayrıntılı bilgi için bkz. [Python ve jupi Not defterleri ile veri bilimi öğretmek için laboratuvar ayarlama](class-type-jupyter-notebook.md).

## <a name="shell-scripting-on-linux"></a>Linux üzerinde kabuk betiği oluşturma
Linux 'ta kabuk betiği oluşturmak için bir laboratuvar ayarlayabilirsiniz. Komut dosyası, yöneticilerin yinelenen görevleri önlemeyi sağlayan, sistem yönetiminin yararlı bir parçasıdır. Bu örnek senaryoda, sınıfı geleneksel Bash betiklerini ve gelişmiş betikleri içerir. Gelişmiş betikler, Bash komutlarını ve Ruby 'yi birleştiren betiklerdir. Bu yaklaşım, Ruby 'nin kabuğa ve Bash komutlarına karşı etkileşimde bulunmak için veri geçmesini sağlar.

Bu komut dosyası sınıflarını alan öğrenciler Linux 'un temel bilgilerini öğrenmenizi ve Bash Shell komut dosyası hakkında bilgi sahibi olmak için bir Linux sanal makinesi alır. Linux sanal makinesi, uzak masaüstü erişimi etkinleştirilmiş ve [gedit](https://help.gnome.org/users/gedit/stable/) ve [Visual Studio Code](https://code.visualstudio.com/) metin düzenleyicileri yüklüyken birlikte gelir.

Bu tür laboratuvar kurulumu hakkında ayrıntılı bilgi için bkz. [Linux 'Ta Shell Scripting](class-type-shell-scripting-linux.md).

## <a name="solidworks-computer-aided-design-cad"></a>SolidWorks bilgisayar destekli tasarım (CAD)
Mühendisler [için mühendislik](https://www.solidworks.com/)öğrencilerine erişim sağlayan bir GPU Laboratuvarı ayarlayabilirsiniz.  Solidçalışmalar, kesintisiz nesneler modellemeye yönelik bir 3B CAD ortamı sağlar.  Solidçalışmalarla mühendisler, tasarımlarını kolayca oluşturabilir, görselleştirin, benzetimini yapabilir ve belgeleyin.

Bu tür laboratuvar kurulumu hakkında ayrıntılı bilgi için bkz. [Solidçalışmalar kullanarak mühendislik sınıfları için laboratuvar ayarlama](class-type-solidworks.md)

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

- [Azure Lab Services kullanarak doğal dil işlemede derin öğrenmeye odaklanan bir laboratuvar kurun](class-type-deep-learning-natural-language-processing.md)
- [Linux üzerinde kabuk betiği oluşturma](class-type-shell-scripting-linux.md)
- [Etik korsanlık](class-type-ethical-hacking.md)