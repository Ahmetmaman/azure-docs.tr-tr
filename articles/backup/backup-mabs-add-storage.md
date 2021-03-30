---
title: Azure Backup Sunucusu ile Modern Yedekleme Alanı kullanma
description: Azure Backup Sunucusu yeni özellikler hakkında bilgi edinin. Bu makalede, yedekleme sunucusu yüklemenizin nasıl yükseltileceği açıklanır.
ms.topic: conceptual
ms.date: 11/13/2018
ms.openlocfilehash: b077296e58e1193e454a686a392d802e905500a5
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "91292906"
---
# <a name="add-storage-to-azure-backup-server"></a>Azure Backup Sunucusu’na depolama alanı ekleme

Azure Backup Sunucusu v2 ve üzeri, %50 depolama tasarrufu sağlayan Modern Yedekleme Alanı destekler ve üç kez daha hızlı yedeklemeler ve daha verimli depolama alanı sağlar. Ayrıca iş yükü kullanan depolama alanı sunar.

> [!NOTE]
> Modern Yedekleme Alanı kullanmak için Windows Server 2019 üzerinde Windows Server 2016 veya v3 üzerinde Backup Server v2 veya v3 ' i çalıştırmanız gerekir.
> Yedekleme sunucusu v2 'yi Windows Server 'ın önceki bir sürümünde çalıştırırsanız, Azure Backup Sunucusu Modern Yedekleme Alanı avantajlarından yararlanabilir. Bunun yerine, yedekleme sunucusu v1 ile çalıştığı için iş yüklerini korur. Daha fazla bilgi için yedekleme sunucusu sürüm [koruması matrisine](backup-mabs-protection-matrix.md)bakın.
>
> Gelişmiş yedekleme performanslarını elde etmek için Windows Server 2019 ' de katmanlı depolama ile MABS v3 'i dağıtmanız önerilir. Katmanlı depolamayı yapılandırma adımları için "[katmanlı depolama Ile MB ayarlama" ADLı](/system-center/dpm/add-storage#set-up-mbs-with-tiered-storage)DPM makalesine başvurun.

## <a name="volumes-in-backup-server"></a>Yedekleme sunucusundaki birimler

Backup Server v2 veya üzeri, depolama birimlerini kabul eder. Bir birim eklediğinizde, Backup Server, birimi gereken Modern Yedekleme Alanı dayanıklı dosya sistemi (ReFS) olarak biçimlendirir. Bir birim eklemek ve daha sonra gerekirse genişletmek için, bu iş akışını kullanmanızı öneririz:

1. VM 'de yedekleme sunucusu ayarlayın.
2. Depolama havuzundaki bir sanal disk üzerinde birim oluşturma:
    1. Depolama havuzuna bir disk ekleyin ve basit düzene sahip bir sanal disk oluşturun.
    2. Ek diskler ekleyin ve sanal diski genişletin.
    3. Sanal diskte birimler oluşturun.
3. Birimleri yedek sunucuya ekleyin.
4. İş yükü kullanan depolamayı yapılandırma.

## <a name="create-a-volume-for-modern-backup-storage"></a>Modern Yedekleme Alanı için birim oluşturma

Disk depolaması, yedekleme sunucusu v2 veya sonraki sürümleri birimler ile kullanmak, depolama üzerinde denetimi korumanıza yardımcı olabilir. Birim tek bir disk olabilir. Ancak, daha sonra depolamayı genişletmek istiyorsanız, depolama alanları kullanılarak oluşturulan bir diskten bir birim oluşturun. Bu, yedekleme depolaması için birimi genişletmek istediğinizde yardımcı olabilir. Bu bölümde, bu kurulumla bir birim oluşturmak için en iyi yöntemler sunulmaktadır.

1. Sunucu Yöneticisi, **dosya ve depolama hizmetleri**  >  **birimleri**  >  **depolama havuzları**' nı seçin. **FIZIKSEL diskler** altında **Yeni depolama havuzu**' nu seçin.

    ![Yeni bir depolama havuzu oluştur](./media/backup-mabs-add-storage/mabs-add-storage-1.png)

2. **Görevler** açılan kutusunda **Yeni sanal disk**' i seçin.

    ![Sanal Disk Ekle](./media/backup-mabs-add-storage/mabs-add-storage-2.png)

3. Depolama havuzunu seçin ve ardından **fiziksel disk Ekle**' yi seçin.

    ![Fiziksel disk ekleme](./media/backup-mabs-add-storage/mabs-add-storage-3.png)

4. Fiziksel disk ' i seçin ve ardından **sanal diski Genişlet**' i seçin.

    ![Sanal diski genişletme](./media/backup-mabs-add-storage/mabs-add-storage-4.png)

5. Sanal diski seçin ve ardından **Yeni birim**' i seçin.

    ![Yeni birim oluştur](./media/backup-mabs-add-storage/mabs-add-storage-5.png)

6. **Sunucu ve disk seçin** iletişim kutusunda, sunucuyu ve yeni diski seçin. Ardından **İleri**' yi seçin.

    ![Sunucu ve diski seçin](./media/backup-mabs-add-storage/mabs-add-storage-6.png)

## <a name="add-volumes-to-backup-server-disk-storage"></a>Yedekleme sunucusuna birim ekleme disk depolama

> [!NOTE]
>
> - Sütun sayısını 1 olarak tutmak için havuza yalnızca bir disk ekleyin. Daha sonra gerektikçe daha sonra disk ekleyebilirsiniz.
> - Bir go 'da depolama havuzuna birden çok disk eklerseniz, disk sayısı sütun sayısı olarak depolanır. Daha fazla disk eklendiğinde, bu sütunlar yalnızca sütun sayısının birden fazla olması olabilir.

Yedekleme sunucusuna bir birim eklemek için, **Yönetim** bölmesinde, depolamayı yeniden tarayın ve **Ekle**' yi seçin. Yedekleme sunucusu depolaması için eklenebilecek tüm birimlerin listesi görüntülenir. Kullanılabilir birimler seçili birimler listesine eklendikten sonra, bunları yönetmenize yardımcı olacak kolay bir ad verebilirsiniz. Yedekleme sunucusunun Modern Yedekleme Alanı avantajlarını kullanabilmesi için bu birimleri ReFS 'e biçimlendirmek için **Tamam**' ı seçin.

![Kullanılabilir birimleri ekleme](./media/backup-mabs-add-storage/mabs-add-storage-7.png)

## <a name="set-up-workload-aware-storage"></a>İş yüküne duyarlı depolamayı ayarlama

İş yükü kullanan depolama ile, belirli iş yükü türlerini tercih edilen olarak depolayan birimleri seçebilirsiniz. Örneğin, yalnızca sık, yüksek hacimli yedeklemeler gerektiren iş yüklerini depolamak için saniyede yüksek sayıda giriş/çıkış işlemi (ıOPS) destekleyen pahalı birimler ayarlayabilirsiniz. İşlem günlükleriyle birlikte SQL Server bir örnektir. VM 'Ler gibi daha az sıklıkta yedeklenen diğer iş yükleri düşük maliyetli birimlere yedeklenebilir.

### <a name="update-dpmdiskstorage"></a>Update-DPMDiskStorage

Bir Azure Backup Sunucusu depolama havuzundaki bir birimin özelliklerini güncelleştiren-DPMDiskStorage PowerShell cmdlet 'ini kullanarak iş yüküne duyarlı depolamayı ayarlayabilirsiniz.

Söz dizimi:

`Parameter Set: Volume`

```powershell
Update-DPMDiskStorage [-Volume] <Volume> [[-FriendlyName] <String> ] [[-DatasourceType] <VolumeTag[]> ] [-Confirm] [-WhatIf] [ <CommonParameters>]
```

Aşağıdaki ekran görüntüsünde, PowerShell penceresindeki Update-DPMDiskStorage cmdlet 'i gösterilmektedir.

![PowerShell penceresindeki Update-DPMDiskStorage komutu](./media/backup-mabs-add-storage/mabs-add-storage-8.png)

PowerShell kullanarak yaptığınız değişiklikler yedekleme sunucusuna Yönetici Konsolu yansıtılır.

![Yönetici Konsolu diskler ve birimler](./media/backup-mabs-add-storage/mabs-add-storage-9.png)

## <a name="migrate-legacy-storage-to-modern-backup-storage"></a>Eski depolamayı Modern Yedekleme Alanı geçir

Yedekleme sunucusu v2 'ye yükselttikten veya yükledikten sonra işletim sistemini Windows Server 2016 ' e yükselttikten sonra, Modern Yedekleme Alanı kullanmak için koruma gruplarınızı güncelleştirin. Varsayılan olarak, koruma grupları değiştirilmez. Bunlar, başlangıçta ayarlandığı gibi çalışmaya devam eder.

Modern Yedekleme Depolama Alanı kullanmak için koruma gruplarını güncelleştirmek isteğe bağlıdır. Koruma grubunu güncelleştirmek için verileri sakla seçeneğini kullanarak tüm veri kaynaklarının korumasını durdurun. Ardından, veri kaynaklarını yeni bir koruma grubuna ekleyin.

1. Yönetici Konsolu **koruma** özelliğini seçin. **Koruma grubu üyesi** listesinde, üyeye sağ tıklayın ve sonra **üyenin korumasını durdur**' u seçin.

   ![Üyenin korumasını durdur](/system-center/dpm/media/upgrade-to-dpm-2016/dpm-2016-stop-protection1.png)

2. **Gruptan Kaldır** iletişim kutusunda, depolama havuzu için kullanılan disk alanını ve kullanılabilir boş alanı gözden geçirin. Varsayılan, diskteki kurtarma noktalarını bırakmak ve ilişkilendirildikleri bekletme ilkesine göre sürelerinin dolmasına izin vermektir. **Tamam**’ı seçin.

   Kullanılan disk alanını hemen boş depolama havuzuna döndürmek istiyorsanız, bu üyeyle ilişkili yedekleme verilerini (ve kurtarma noktalarını) silmek için **diskte çoğaltmayı Sil** onay kutusunu seçin.

   ![Gruptan Kaldır iletişim kutusu](/system-center/dpm/media/upgrade-to-dpm-2016/dpm-2016-retain-data.png)

3. Modern Yedekleme Alanı kullanan bir koruma grubu oluşturun. Korumasız veri kaynaklarını dahil edin.

## <a name="add-disks-to-increase-legacy-storage"></a>Eski depolamayı artırmak için disk ekleme

Yedekleme sunucusu ile eski depolamayı kullanmak istiyorsanız, eski depolamayı artırmak için disk eklemeniz gerekebilir.

Disk depolama eklemek için:

1. Yönetici Konsolu **Yönetim**  >  **disk depolama**  >  **Ekle**' yi seçin.

    ![Disk Depolama iletişim kutusu Ekle](/system-center/dpm/media/upgrade-to-dpm-2016/dpm-2016-add-disk-storage.png)

2. **Disk depolama Ekle** iletişim kutusunda, **Disk Ekle**' yi seçin.

3. Kullanılabilir diskler listesinde, eklemek istediğiniz diskleri seçin, **Ekle**' yi seçin ve ardından **Tamam**' ı seçin.

## <a name="next-steps"></a>Sonraki adımlar

Yedekleme sunucusunu yükledikten sonra, sunucunuzu nasıl hazırlayacağınızı veya bir iş yükünü korumaya nasıl başlayacağınızı öğrenin.

- [Yedekleme sunucusu iş yüklerini hazırlama](backup-azure-microsoft-azure-backup.md)
- [VMware sunucusunu yedeklemek için Yedekleme Sunucusu kullanma](backup-azure-backup-server-vmware.md)
- [SQL Server'ı yedeklemek için Yedekleme Sunucusu kullanma](backup-azure-sql-mabs.md)
