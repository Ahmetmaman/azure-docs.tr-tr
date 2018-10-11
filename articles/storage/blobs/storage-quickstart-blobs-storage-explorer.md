---
title: Hızlı Başlangıç - Azure Depolama Gezgini kullanarak nesne depolamada blob oluşturma | Microsoft Docs
description: Bu hızlı başlangıçta, kapsayıcı ve blob oluşturmak için Azure Depolama Gezgini’ni nasıl kullanacağınızı öğreneceksiniz. Ardından, blob’u yerel bilgisayarınıza indirmeyi ve bir kapsayıcıdaki tüm blobları görüntülemeyi öğreneceksiniz. Ayrıca bir blobun anlık görüntüsünü oluşturma, kapsayıcı erişim ilkelerini yönetme ve paylaşılan erişim imzası oluşturma hakkında bilgi edineceksiniz.
services: storage
author: tamram
ms.custom: mvc
ms.service: storage
ms.topic: quickstart
ms.date: 08/27/2018
ms.author: tamram
ms.openlocfilehash: 3f0418e9ccdc5fe3babd2f1ac4482795994fe3b4
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46996137"
---
# <a name="quickstart-use-azure-storage-explorer-to-create-a-blob-in-object-storage"></a>Hızlı Başlangıç - Azure Depolama Gezgini kullanarak nesne depolamada blob oluşturma | Microsoft Docs

Bu hızlı başlangıçta, kapsayıcı ve blob oluşturmak için [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/)’ni nasıl kullanacağınızı öğreneceksiniz. Ardından, blob’u yerel bilgisayarınıza indirmeyi ve bir kapsayıcıdaki tüm blobları görüntülemeyi öğreneceksiniz. Ayrıca bir blobun anlık görüntüsünü oluşturma, kapsayıcı erişim ilkelerini yönetme ve paylaşılan erişim imzası oluşturma hakkında bilgi edineceksiniz.

## <a name="prerequisites"></a>Ön koşullar

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

Bu hızlı başlangıç Azure Depolama Gezgini'ni yüklemenizi gerektirir. Windows, Macintosh veya Linux işletim sisteminde Azure Depolama Gezgini’ni yüklemek için bkz. [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/).

[!INCLUDE [storage-create-account-portal-include](../../../includes/storage-create-account-portal-include.md)]

## <a name="log-in-to-storage-explorer"></a>Depolama Gezgini oturumu açma

Uygulamayı ilk kez başlattığınızda **Microsoft Azure Depolama Gezgini - Bağlan** penceresi görüntülenir. Depolama Gezgini depolama hesaplarına bağlamak için birçok yol sağlar. Aşağıdaki tabloda bağlantı için kullanabileceğiniz yöntemlere yer verilmiştir:

|Görev|Amaç|
|---|---|
|Azure Hesabı ekleme | Azure'da kimlik doğrulaması gerçekleştirmek için kuruluşunuzun oturum açma sayfasını açar. |
|Bağlantı dizesi veya paylaşılan erişim imzası URI'si kullanma | SAS belirteci veya paylaşılan bağlantı dizesiyle doğrudan bir kapsayıcıya veya depolama hesabına erişmek için kullanılabilir. |
|Depolama hesabı adını ve anahtarını kullanma| Azure depolama alanına bağlanmak için depolama hesabı adını ve anahtarını kullanın.|

**Azure Hesabı Ekle**'yi seçip **Oturum açın**'a tıklayın. Azure hesabınızda oturum açmak için ekrandaki talimatları izleyin.

![Microsoft Azure Depolama Gezgini - Bağlan penceresi](media/storage-quickstart-blobs-storage-explorer/connect.png)

Bağlantı kurulduğunda Azure Depolama Gezgini yüklenir ve **Gezgin** sekmesi gösterilir. Bu görünümde tüm Azure depolama hesaplarınıza ek olarak [Azure Depolama Öykünücüsü](../common/storage-use-emulator.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json), [Cosmos DB](../../cosmos-db/storage-explorer.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) hesapları veya [Azure Stack](../../azure-stack/user/azure-stack-storage-connect-se.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) ortamları üzerinden yapılandırılan yerel depolama alanlarını görebilirsiniz.

![Microsoft Azure Depolama Gezgini - Bağlan penceresi](media/storage-quickstart-blobs-storage-explorer/mainpage.png)

## <a name="create-a-container"></a>Bir kapsayıcı oluşturma

Bloblar her zaman bir kapsayıcıya yüklenir. Bu, blob gruplarını bilgisayarınızdaki dosyaları klasörler halinde düzenlediğiniz gibi düzenleyebilmenizi sağlar.

Kapsayıcı oluşturmak için önceki adımda oluşturduğunuz depolama hesabını genişletin. **Blob Kapsayıcıları**'nı ve ardından **Blob Kapsayıcısı Oluştur**'u seçin. Blob kapsayıcınızın adını girin. Blob kapsayıcılarını adlandırmayla ilgili kural ve kısıtlamaların listesi için [Kapsayıcı oluşturma ve ayarlama izinleri](storage-quickstart-blobs-dotnet.md#create-the-container-and-set-permissions) bölümüne bakın. Girişleri tamamladığınızda blob kapsayıcısını oluşturmak için **Enter**'a basın. Blob kapsayıcısı başarıyla oluşturulduktan sonra, seçili depolama hesabının **Blob Kapsayıcıları** klasörü altında gösterilir.

## <a name="upload-blobs-to-the-container"></a>Blobları kapsayıcıya yükleme

Blob depolama blok blobları, ekleme bloblarını ve sayfa bloblarını destekler. IaaS VM’lerini yedeklemek için kullanılan VHD dosyaları sayfa bloblarıdır. Ekleme blobları, bir dosyaya yazıp daha sonradan daha fazla bilgi eklemek istediğiniz durumlarda günlüğe kaydetme için kullanılır. Blob depolamada depolanan çoğu dosya blok blobudur.

Kapsayıcı şeridinde **Yükle**'yi seçin. Bu işlemi kullanarak klasör veya dosya yükleyebilirsiniz.

Yüklenecek dosyaları veya klasörü seçin. **Blob türü**'nü seçin. **Ekleme**, **Sayfa** veya **Blok** bloblarını seçebilirsiniz.

.vhd veya .vhdx dosyası yüklüyorsanız **.vhd/.vhdx dosyalarını sayfa blobları olarak yükle (önerilen)** seçeneğini belirleyin.

**Klasöre yükle (isteğe bağlı)** alanına dosyaların veya klasörlerin depolanması için kapsayıcıdaki bir klasörün adını girebilirsiniz. Klasör seçilmezse dosyalar doğrudan kapsayıcı altına yüklenir.

![Microsoft Azure Depolama Gezgini - blob yükleme](media/storage-quickstart-blobs-storage-explorer/uploadblob.png)

**Tamam**'ı seçtiğinizde dosyalar yüklenmek üzere kuyruğa alınır ve tüm dosyalar yüklenir. Yükleme işlemi tamamlandığında sonuçlar **Etkinlikler** penceresinde gösterilir.

## <a name="view-blobs-in-a-container"></a>Blobları bir kapsayıcıda görüntüleme

**Azure Depolama Gezgini** uygulamasında depolama hesabının altındaki bir kapsayıcıyı seçin. Ana bölmede seçilen kapsayıcı içindeki blobların listesi gösterilir.

![Microsoft Azure Depolama Gezgini - kapsayıcı içindeki blobları listeleme](media/storage-quickstart-blobs-storage-explorer/listblobs.png)

## <a name="download-blobs"></a>Blob’ları indirme

**Azure Depolama Gezgini**'ni kullanarak blobları indirmek için istediğiniz blobu seçtikten sonra şeritteki **İndir**'e tıklayın. Açılan dosya iletişim kutusuna dosya adı girebilirsiniz. Blobu yerel konuma indirmeye başlamak için **Kaydet**'i seçin.

## <a name="manage-snapshots"></a>Anlık görüntüleri yönetme

Azure Depolama Gezgini, bloblarınızın [anlık görüntülerini](storage-blob-snapshots.md) almanızı ve görüntülemenizi sağlar. Bir blobun anlık görüntüsünü almak için bloba sağ tıklayıp **Ekran Görüntüsü Yap**'ı seçin. Bir blobun anlık görüntülerini görüntülemek için bloba sağ tıklayıp **Ekran Görüntülerini Yönet**'i seçin. Geçerli sekmede blobun ekran görüntülerinin listesi gösterilir.

![Microsoft Azure Depolama Gezgini - kapsayıcı içindeki blobları listeleme](media/storage-quickstart-blobs-storage-explorer/snapshots.png)

## <a name="manage-access-policies"></a>Erişim ilkelerini yönetme

Depolama Gezgini, kullanıcı arabiriminde kapsayıcılar için erişim ilkelerini yönetme imkanı sunar. Hizmet düzeyi ve hesap düzeyi olmak üzere iki güvenlik erişim ilkesi (SAS) türü vardır. Hesap düzeyi SAS, depolama hesabını hedefler ve birden fazla hizmete ve kaynağa uygulanabilir. Hizmet düzeyi SAS, belirli bir hizmet altındaki kaynak için tanımlanır. Hizmet düzeyi SAS oluşturmak için kapsayıcıya sağ tıklayıp **Erişim İlkelerini Yönet...** öğesini seçin. Hesap düzeyi SAS oluşturmak için depolama hesabına sağ tıklayın.

Yeni bir erişim ilkesi eklemek ve ilkenin izinlerini tanımlamak için **Ekle**'yi seçin. İşlemi tamamladığınızda erişim ilkesini kaydetmek için **Kaydet**'i seçin. Bu ilkeyi Paylaşılan Erişim İmzası yapılandırma sırasında kullanabilirsiniz.

## <a name="work-with-shared-access-signatures"></a>Paylaşılan Erişim İmzaları ile çalışma

Paylaşılan Erişim İmzaları (SAS) Depolama Gezgini'nden alınabilir. Depolama hesabına, kapsayıcıya veya bloba sağ tıklayıp **Paylaşılan Erişim İmzası Al...** öğesini seçin. Başlangıç ve bitiş zamanını ve SAS URL izinlerini seçip **Oluştur**'a tıklayın. Sonraki ekranda sorgu dizesini içeren tam URL'nin yanı sıra sorgu dizesinin kendisi sağlanır ve bu değerler kopyalanabilir.

![Microsoft Azure Depolama Gezgini - kapsayıcı içindeki blobları listeleme](media/storage-quickstart-blobs-storage-explorer/sharedaccesssignature.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, dosyaları **Azure Depolama Gezgini** kullanarak yerel bir disk ve Azure Blob depolama arasında aktarmayı öğrendiniz. Blob depolamayla çalışma hakkında daha fazla bilgi edinmek için, Blob depolama nasıl yapılır öğreticisiyle devam edin.

> [!div class="nextstepaction"]
> [Blob Depolama İşlemleri Nasıl Yapılır](storage-how-to-use-blobs-powershell.md)
