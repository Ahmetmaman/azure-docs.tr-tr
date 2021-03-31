---
title: 'Öğretici: sunucu tasarlama-Azure portal-MySQL için Azure veritabanı'
description: Bu öğreticide, Azure portal kullanarak MySQL için Azure veritabanı sunucusu ve veritabanı oluşturma ve yönetme açıklanmaktadır.
author: savjani
ms.author: pariks
ms.service: mysql
ms.topic: tutorial
ms.date: 3/20/2020
ms.custom: mvc
ms.openlocfilehash: 7559bc2246ca26cf2b14071396e075b28d2af3a7
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "94532690"
---
# <a name="tutorial-design-an-azure-database-for-mysql-database-using-the-azure-portal"></a>Öğretici: Azure portalını kullanarak MySQL için Azure Veritabanı tasarlama

MySQL için Azure Veritabanı, bulutta yüksek oranda kullanılabilir olan MySQL veritabanları çalıştırmanızı, yönetmenizi ve ölçeklendirmenizi sağlayan ve yönetilen bir hizmettir. Azure portalını kullanarak, sunucunuzu kolayca yönetebilir ve bir veritabanı tasarlayabilirsiniz.

Bu öğreticide, Azure portalını kullanarak şu işlemleri gerçekleştirmeyi öğreneceksiniz:

> [!div class="checklist"]
> * MySQL için Azure Veritabanı oluşturma
> * Sunucu güvenlik duvarını yapılandırma
> * Veritabanı oluşturmak için mysql komut satırı aracı kullanma
> * Örnek verileri yükleme
> * Verileri sorgulama
> * Verileri güncelleştirme
> * Verileri geri yükleme

## <a name="prerequisites"></a>Önkoşullar

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

Sık kullandığınız web tarayıcınızı açıp [Microsoft Azure portalı](https://portal.azure.com/)’na gidin. Portalda oturum açmak için kimlik bilgilerinizi girin. Varsayılan görünüm hizmet panonuzu içerir.

## <a name="create-an-azure-database-for-mysql-server"></a>MySQL için Azure Veritabanı sunucusu oluşturma

MySQL için Azure Veritabanı sunucusu, tanımlı bir dizi [işlem ve depolama kaynağı](./concepts-pricing-tiers.md) ile oluşturulur. Sunucu, [Azure kaynak grubu](../azure-resource-manager/management/overview.md) içinde oluşturulur.

1. Portalın sol üst köşesinde bulunan **Kaynak oluştur** düğmesini (+) seçin.

2. **Veritabanları**  >  **MySQL için Azure veritabanı**' nı seçin. **Veritabanları** kategorisi altında MySQL sunucusu bulamıyorsanız, kullanılabilir tüm veritabanı hizmetlerini görüntülemek Için **Tümünü gör** ' e tıklayın. Ayrıca arama kutusuna **MySQL için Azure Veritabanı** yazarak hizmeti hızlıca bulabilirsiniz.
   
   :::image type="content" source="./media/tutorial-design-database-using-portal/1-Navigate-to-MySQL.png" alt-text="MySQL’e gidin":::

3. **MySQL Için Azure veritabanı** Kutucuğu ' na tıklayın. MySQL için Azure Veritabanı formunu doldurun.
   
   :::image type="content" source="./media/tutorial-design-database-using-portal/2-create-form.png" alt-text="Oluşturma formu":::

    **Ayar** | **Önerilen değer** | **Alan açıklaması**
    ---|---|---
    Sunucu adı | Benzersiz sunucu adı | Azure veritabanınızı MySQL sunucusuna tanıtan benzersiz bir ad seçin. Örneğin, demosunucum. Etki alanı adı *. MySQL.Database.Azure.com* , sağladığınız sunucu adına eklenir. Sunucu adı yalnızca küçük harf, sayı ve kısa çizgi (-) karakterini içerebilir. 3 ila 63 karakter arası içermelidir.
    Abonelik | Aboneliğiniz | Sunucunuz için kullanmak istediğiniz Azure aboneliğini seçin. Birden fazla aboneliğiniz varsa kaynağın faturalandığı aboneliği seçin.
    Kaynak grubu | *myresourcegroup* | Yeni veya mevcut bir kaynak grubu adı girin.
    Kaynak seçme | *Adet* | Sıfırdan yeni bir sunucu oluşturmak için *Boş*’u seçin. (Mevcut bir MySQL için Azure Veritabanı sunucusunun bir coğrafi yedeğinden bir sunucu oluşturuyorsanız, *Yedek* seçeneğini belirleyin).
    Sunucu yöneticisi oturumu açma | myadmin | Sunucuya bağlanırken kullanılacak oturum açma hesabı. Yönetici oturum açma adı **azure_superuser**, **admin**, **administrator**, **root**, **guest** veya **public** olamaz.
    Parola | *Tercih ettiğiniz* | Sunucu yönetici hesabı için yeni bir parola girin. 8 ila 128 karakter arası içermelidir. Parolanız şu üç kategoride yer alan karakterlerden oluşmalıdır: İngilizce büyük ve küçük harfler, sayılar (0-9) ve alfasayısal olmayan karakterler (!, $, #, %, vb.).
    Parolayı onayla | *Tercih ettiğiniz*| Yönetici hesabı parolasını onaylayın.
    Konum | *Kullanıcılarınıza en yakın bölge*| Kullanıcılarınıza veya diğer Azure uygulamalarınıza en yakın konumu seçin.
    Sürüm | *En son sürüm*| En son sürüm (başka bir sürüm gerektiren belirli gereksinimleriniz yoksa).
    Fiyatlandırma katmanı | **Genel Amaçlı**, **5. Nesil**, **2 sanal çekirdek**, **5 GB**, **7 gün**, **Coğrafi Olarak Yedekli** | Yeni sunucunuz için işlem, depolama ve yedekleme yapılandırmaları. **Fiyatlandırma katmanı**'nı seçin. Sonra **genel amaçlı** sekmesini seçin. *Gen 5*, *2 sanal çekirdek*, *5 GB* ve *7 gün* , **işlem oluşturma**, **sanal çekirdek**, **depolama** ve **yedekleme saklama süresi** için varsayılan değerlerdir. Bu kaydırıcıları olduğu gibi bırakabilirsiniz. Coğrafi olarak yedekli depolamada sunucu yedeklemelerinizi etkinleştirmek için **yedek artıklığı seçeneklerinde** **coğrafi olarak yedekli** ' ı seçin. Bu fiyatlandırma katmanı seçimini kaydetmek için **Tamam**’ı seçin. Sonraki ekran görüntüsü bu seçimleri yakalar.

   :::image type="content" source="./media/tutorial-design-database-using-portal/3-pricing-tier.png" alt-text="Fiyatlandırma katmanı":::

   > [!TIP]
   > **Otomatik büyüme** etkinken sunucunuz, ayrılan sınıra yaklaşdığınızda, iş yükünüzü etkilemeden depolama alanını arttırır.

4. **Gözden geçir ve oluştur**’a tıklayın. Dağıtım işlemini izlemek için araç çubuğunda **Bildirimler** düğmesine tıklayabilirsiniz. Dağıtım 20 dakikaya kadar sürebilir.

## <a name="configure-firewall"></a>Güvenlik duvarını yapılandırma

MySQL için Azure Veritabanları bir güvenlik duvarı tarafından korunur. Varsayılan olarak, sunucuya ve sunucu içindeki veritabanlarına yönelik tüm bağlantılar reddedilir. MySQL için Azure Veritabanı’na ilk kez bağlanmadan önce, güvenlik duvarını istemcinin genel ağ IP adresini (veya IP adres aralığını) ekleyecek şekilde yapılandırın.

1. Yeni oluşturduğunuz sunucuya ve ardından **Bağlantı güvenliği**’ne tıklayın.

   :::image type="content" source="./media/tutorial-design-database-using-portal/1-Connection-security.png" alt-text="Bağlantı güvenliği":::
2. **IP’mi Ekle** seçeneğini kullanabilir veya güvenlik duvarı kurallarını buradan yapılandırabilirsiniz. Kuralları oluşturduktan sonra **Kaydet**’e tıklamayı unutmayın.
Artık mysql komut satırı aracını veya MySQL Workbench GUI aracını kullanarak sunucuya bağlanabilirsiniz.

> [!TIP]
> MySQL için Azure Veritabanı sunucusu, 3306 bağlantı noktası üzerinden iletişim kurar. Kurumsal ağ içinden bağlanmaya çalışıyorsanız, ağınızın güvenlik duvarı tarafından 3306 numaralı bağlantı noktası üzerinden giden trafiğe izin verilmiyor olabilir. Bu durumda BT departmanınız 3306 numaralı bağlantı noktasını açmadığı sürece Azure MySQL sunucusuna bağlanamazsınız.

## <a name="get-connection-information"></a>Bağlantı bilgilerini alma

Azure portalından MySQL için Azure Veritabanı sunucunuz için tam **Sunucu adı** ve **Sunucu yöneticisi oturum açma adını** alın. Tam sunucu adını, mysql komut satırı aracı kullanarak sunucunuza bağlanmak için kullanırsınız.

1. [Azure portalı](https://portal.azure.com/)’nda soldaki menüden **Tüm kaynaklar**’a tıklayın, adı yazın ve MySQL için Azure Veritabanı sunucunuzu arayın. Ayrıntıları görüntülemek için sunucu adını seçin.

2. **Genel bakış** sayfasında, **Sunucu Adı** ve **Sunucu yöneticisi oturum açma adı** değerlerini not edin. Panoya kopyalamak için her alanın yanındaki kopyala düğmesine tıklayabilirsiniz.
   :::image type="content" source="./media/tutorial-design-database-using-portal/2-server-properties.png" alt-text="4-2 sunucu özellikleri":::

Bu örnekte sunucu adı *mydemoserver.MySQL.Database.Azure.com*' dir ve Sunucu Yöneticisi oturum açma adı *myadmin \@ demosunucum*' dır.

## <a name="connect-to-the-server-using-mysql"></a>mysql kullanarak sunucuya bağlanma

MySQL sunucusu için Azure Veritabanınız ile bağlantı kurmak üzere [mysql komut satırı aracını](https://dev.mysql.com/doc/refman/5.7/en/mysql.html) kullanın. mysql komut satırı aracını tarayıcıdaki Azure Cloud Shell’den veya yerel olarak yüklenmiş mysql araçlarını kullanarak kendi makinenizden çalıştırabilirsiniz. Azure Cloud Shell’i başlatmak için bu makaledeki bir kod bloğu üzerinde `Try It` düğmesine tıklayın veya Azure portalını ziyaret ederek sağ üstteki araç çubuğunda `>_` simgesine tıklayın.

Bağlanma komutunu yazın:

```azurecli-interactive
mysql -h mydemoserver.mysql.database.azure.com -u myadmin@mydemoserver -p
```

## <a name="create-a-blank-database"></a>Boş veritabanı oluşturma

Sunucuya bağlandıktan sonra, ile çalışmak üzere boş bir veritabanı oluşturun.

```sql
CREATE DATABASE mysampledb;
```

İstemde, bağlantıyı bu yeni oluşturulan veritabanına değiştirmek için şu komutu çalıştırın:

```sql
USE mysampledb;
```

## <a name="create-tables-in-the-database"></a>Veritabanında tablo oluşturma

Artık MySQL veritabanı için Azure Veritabanına nasıl bağlanacağınızı bildiğinize göre bazı temel görevleri tamamlayabilirsiniz:

İlk olarak, bir tablo oluşturun ve bu tabloya bazı veriler yükleyin. Envanter bilgilerini depolayan bir tablo oluşturalım.

```sql
CREATE TABLE inventory (
    id serial PRIMARY KEY, 
    name VARCHAR(50), 
    quantity INTEGER
);
```

## <a name="load-data-into-the-tables"></a>Tablolara veri yükleme

Bir tablonuz olduğuna göre içine bazı veriler ekleyin. Açık komut istemi penceresinde, birkaç veri satırı eklemek için şu sorguyu çalıştırın.

```sql
INSERT INTO inventory (id, name, quantity) VALUES (1, 'banana', 150);
INSERT INTO inventory (id, name, quantity) VALUES (2, 'orange', 154);
```

Daha önce oluşturduğunuz tabloda artık iki satırlık örnek verileriniz var.

## <a name="query-and-update-the-data-in-the-tables"></a>Tablolardaki verileri sorgulama ve güncelleştirme

Veritabanı tablosundan bilgi almak için şu sorguyu yürütün.

```sql
SELECT * FROM inventory;
```

Ayrıca tablolardaki verileri de güncelleştirebilirsiniz.

```sql
UPDATE inventory SET quantity = 200 WHERE name = 'banana';
```

Siz veri aldıkça satır güncelleştirilir.

```sql
SELECT * FROM inventory;
```

## <a name="restore-a-database-to-a-previous-point-in-time"></a>Bir veritabanını daha önceki bir noktaya geri yükleme

Önemli bir veritabanı tablosunu yanlışlıkla sildiğinizi ve verileri kolayca kurtaramayacağınızı düşünün. MySQL için Azure Veritabanı, sunucuyu bir zaman noktasına geri yükleyerek, veritabanlarının bir kopyasının yeni sunucuda oluşturulmasını sağlar. Bu yeni sunucuyu silinen verilerinizi kurtarmak için kullanabilirsiniz. Şu adımlar, örnek sunucuyu tablo eklenmeden önceki bir noktaya geri yükler.

1. Azure portalında MySQL için Azure Veritabanınızı bulun. **Genel bakış** sayfası üzerinde, araç çubuğunda **Geri yükle**’ye tıklayın. Geri Yükle sayfası açılır.

   :::image type="content" source="./media/tutorial-design-database-using-portal/1-restore-a-db.png" alt-text="10-1 veritabanını geri yükleme":::

2. **Geri Yükle** formunu gerekli bilgiler ile doldurun.

   :::image type="content" source="./media/tutorial-design-database-using-portal/2-restore-form.png" alt-text="10-2 geri yükleme formu":::

   - **Geri yükleme noktası**: Listelenen zaman dilimi içerisindeki geri yüklemek istediğiniz noktayı seçin. Yerel saat diliminizi UTC'ye dönüştürdüğünüzden emin olun.
   - **Yeni sunucuya geri yükleme**: Geri yüklemek istediğiniz yeni bir sunucu adı sağlayın.
   - **Konum**: Bölge, kaynak sunucu ile aynıdır ve değiştirilemez.
   - **Fiyatlandırma katmanı**: Fiyatlandırma katmanı, kaynak sunucu ile aynıdır ve değiştirilemez.
   
3. Sunucuyu, tablo silinmeden önceki [bir zamana geri yüklemek](./howto-restore-server-portal.md) için **Tamam**’a tıklayın. Bir sunucuyu geri yüklemek, belirttiğiniz zamandan itibaren sunucunun yeni bir kopyasını oluşturur.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Gelecekte bu kaynaklara ihtiyaç duymazsanız, kaynak grubunu silerek veya MySQL sunucusunu silmeniz durumunda bunları silebilirsiniz. Kaynak grubunu silmek için şu adımları izleyin:
1. Azure portal, **kaynak gruplarını** arayıp seçin. 
2. Kaynak grubu listesinde, kaynak grubunuzun adını seçin.
3. Kaynak grubunuzun Genel Bakış sayfasında **kaynak grubunu sil**' i seçin.
4. Onay iletişim kutusunda, kaynak grubunuzun adını yazın ve ardından **Sil**' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure portalını şunları yapmayı öğrenmek için kullanırsınız:

> [!div class="checklist"]
> * MySQL için Azure Veritabanı oluşturma
> * Sunucu güvenlik duvarını yapılandırma
> * Veritabanı oluşturmak için mysql komut satırı aracı kullanma
> * Örnek verileri yükleme
> * Verileri sorgulama
> * Verileri güncelleştirme
> * Verileri geri yükleme

> [!div class="nextstepaction"]
> [MySQL için Azure Veritabanına nasıl uygulama bağlanır](./howto-connection-string.md)