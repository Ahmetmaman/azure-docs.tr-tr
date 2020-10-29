---
title: Kurumsal güvenlik ve idare
titleSuffix: Azure Machine Learning
description: 'Azure Machine Learning güvenli bir şekilde kullanın: kimlik doğrulama, yetkilendirme, ağ güvenliği, veri şifreleme ve izleme.'
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: aashishb
author: aashishb
ms.reviewer: larryfr
ms.date: 09/09/2020
ms.openlocfilehash: b45c5cd1a750ee4b3f182920c4ee2f2e47756867
ms.sourcegitcommit: 693df7d78dfd5393a28bf1508e3e7487e2132293
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/28/2020
ms.locfileid: "92899322"
---
# <a name="enterprise-security-and-governance-for-azure-machine-learning"></a>Azure Machine Learning için kurumsal güvenlik ve idare

Bu makalede Azure Machine Learning için kullanılabilen güvenlik özellikleri hakkında bilgi edineceksiniz.

Bir bulut hizmeti kullandığınızda, erişimi yalnızca ihtiyacı olan kullanıcılarla kısıtlayabilmeniz en iyi uygulamadır. Hizmetini, hizmet tarafından kullanılan kimlik doğrulama ve yetkilendirme modelini inceleyerek başlatın. Ayrıca, ağ erişimini kısıtlamak veya şirket içi ağınızdaki kaynakları güvenli bir şekilde buluta katmak isteyebilirsiniz. Veri şifreleme Ayrıca, hem bekleyen hem de veriler hizmetler arasında taşınıyor. Ayrıca, uyumlu olmayan yapılandırmaların oluşturulması durumunda belirli yapılandırmaların veya günlüğün uygulanması için ilkeler oluşturmak isteyebilirsiniz. Son olarak, hizmeti izleyebilmeniz ve tüm etkinliklerin denetim günlüğünü üretmeniz gerekir.

> [!NOTE]
> Bu makaledeki bilgiler, Azure Machine Learning Python SDK sürümü 1.0.83.1 veya üzeri ile birlikte çalışmaktadır.

## <a name="authentication"></a>Kimlik Doğrulaması

Azure Active Directory (Azure AD) kullanmak üzere yapılandırılmışsa Multi-Factor Authentication desteklenir. Kimlik doğrulama işlemi şu şekildedir:

1. İstemci Azure AD 'de oturum açar ve bir Azure Resource Manager belirteci alır.  Kullanıcılar ve hizmet sorumluları tam olarak desteklenmektedir.
1. İstemci belirteci Azure Resource Manager ve tüm Azure Machine Learning gösterir.
1. Machine Learning hizmeti, Kullanıcı işlem hedefine bir Machine Learning hizmet belirteci sağlar (örneğin, Machine Learning İşlem). Bu belirteç, Kullanıcı işlem hedefi tarafından, çalıştırma tamamlandıktan sonra Machine Learning hizmetine geri çağrı yapmak için kullanılır. Kapsam, çalışma alanıyla sınırlıdır.

[![Azure Machine Learning kimlik doğrulaması](media/concept-enterprise-security/authentication.png)](media/concept-enterprise-security/authentication.png#lightbox)

Daha fazla bilgi için bkz. [Azure Machine Learning kaynakları ve iş akışları için kimlik doğrulamasını ayarlama](how-to-setup-authentication.md). Bu makalede, kimlik doğrulama hakkında hizmet sorumluları ve otomatik iş akışları gibi bilgiler ve örnekler sağlanmaktadır.

### <a name="authentication-for-web-service-deployment"></a>Web hizmeti dağıtımı için kimlik doğrulaması

Azure Machine Learning, Web Hizmetleri için iki kimlik doğrulama biçimini destekler: anahtar ve belirteç. Her Web hizmeti tek seferde yalnızca bir kimlik doğrulama biçimi etkinleştirebilir.

|Kimlik doğrulama yöntemi|Açıklama|Azure Container Instances|AKS|
|---|---|---|---|
|Anahtar|Anahtarlar statiktir ve yenilenmek zorunda değildir. Anahtarlar el ile yeniden oluşturulabilir.|Varsayılan olarak devre dışı| Varsayılan olarak etkindir|
|Belirteç|Belirteçlerin süresi belirtilen süre geçtikten sonra ve yenilenmesi gerekiyor.| Kullanılamaz| Varsayılan olarak devre dışı |

Kod örnekleri için, [Web hizmeti kimlik doğrulaması bölümüne](how-to-setup-authentication.md#web-service-authentication)bakın.

## <a name="authorization"></a>Yetkilendirme

Birden çok çalışma alanı oluşturabilirsiniz ve her çalışma alanı birden çok kişi tarafından paylaşılabilir. Bir çalışma alanını paylaştığınızda, kullanıcılara bu rolleri atayarak erişimi denetleyebilirsiniz:

* Sahip
* Katılımcı
* Okuyucu

Aşağıdaki tabloda, bazı önemli Azure Machine Learning işlemleri ve bunları gerçekleştirebilen roller listelenmektedir:

| Azure Machine Learning işlemi | Sahip | Katılımcı | Okuyucu |
| ---- |:----:|:----:|:----:|
| Çalışma alanı oluşturma | ✓ | ✓ | |
| Çalışma alanını paylaşma | ✓ | |  |
| İşlem hedefi oluştur | ✓ | ✓ | |
| İşlem hedefi Ekle | ✓ | ✓ | |
| Veri depoları Ekle | ✓ | ✓ | |
| Deneme Çalıştır | ✓ | ✓ | |
| Çalıştırmaları/ölçümleri görüntüle | ✓ | ✓ | ✓ |
| Modeli kaydetme | ✓ | ✓ | |
| Görüntü oluştur | ✓ | ✓ | |
| Web hizmetini dağıtma | ✓ | ✓ | |
| Modelleri/görüntüleri görüntüleme | ✓ | ✓ | ✓ |
| Web hizmetini çağır | ✓ | ✓ | ✓ |

Yerleşik roller ihtiyaçlarınızı karşılamıyorsa, özel roller oluşturabilirsiniz. Özel roller, bir işlem oluşturma, çalıştırma gönderme, bir veri deposunu kaydetme veya bir modeli dağıtma gibi çalışma alanındaki tüm işlemleri denetlemek için desteklenir. Özel rollerin kümeler, veri depoları, modeller ve uç noktalar gibi bir çalışma alanının çeşitli kaynakları üzerinde okuma, yazma veya silme izinleri olabilir. Rolü belirli bir çalışma alanı düzeyinde, belirli bir kaynak grubu düzeyinde veya belirli bir abonelik düzeyinde kullanılabilir hale getirebilirsiniz. Daha fazla bilgi için bkz. [Azure Machine Learning çalışma alanında kullanıcıları ve rolleri yönetme](how-to-assign-roles.md).

> [!WARNING]
> Azure Machine Learning Azure Active Directory işletmeden işletmeye işbirliğiyle desteklenir, ancak şu anda Azure Active Directory işletmeden müşteriye işbirliğiyle desteklenmez.

### <a name="securing-compute-targets-and-data"></a>İşlem hedeflerinin ve verilerin güvenliğini sağlama

Sahipler ve katkıda bulunanlar, çalışma alanına bağlı tüm işlem hedeflerini ve veri depolarını kullanabilir.  

Her çalışma alanı Ayrıca, çalışma alanıyla aynı ada sahip ilişkili bir sistem tarafından atanan yönetilen kimliğe sahiptir. Yönetilen kimliğin, çalışma alanında kullanılan bağlı kaynaklar üzerinde aşağıdaki izinleri vardır.

Yönetilen kimlikler hakkında daha fazla bilgi için bkz. [Azure kaynakları Için Yönetilen kimlikler](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview).

| Kaynak | İzinler |
| ----- | ----- |
| Çalışma alanı | Katılımcı |
| Depolama hesabı | Depolama Blob Verileri Katkıda Bulunanı |
| Key Vault | Tüm anahtarlar, gizlilikler, sertifikalara erişim |
| Azure Container Registry | Katılımcı |
| Çalışma alanını içeren kaynak grubu | Katılımcı |
| Anahtar kasasını içeren kaynak grubu (çalışma alanını içeren bunlardan farklıysa) | Katılımcı |

Yöneticilerin yönetilen kimliğin önceki tabloda bahsedilen kaynaklara erişimini iptal etmemenizi önermiyoruz. Yeniden eşitleme anahtarları işlemini kullanarak erişimi geri yükleyebilirsiniz.

Azure Machine Learning, `aml-` `Microsoft-AzureML-Support-App-` her çalışma alanı bölgesi için aboneliğinizde katkıda bulunan düzeyinde erişim ile ek bir uygulama (adı ile başlar) oluşturur. Örneğin, Doğu ABD ' de bir çalışma alanınız varsa ve bir diğeri aynı abonelikte Kuzey Avrupa, bu uygulamalardan ikisini de görürsünüz. Bu uygulamalar, işlem kaynaklarını yönetmenize yardımcı olmak için Azure Machine Learning sağlar.

## <a name="network-security"></a>Ağ güvenliği

Azure Machine Learning, işlem kaynakları için diğer Azure hizmetlerini kullanır. İşlem kaynakları (işlem hedefleri) modelleri eğitmek ve dağıtmak için kullanılır. Bu işlem hedeflerini bir sanal ağda oluşturabilirsiniz. Örneğin, bir modeli eğitebilmek ve sonra modeli AKS 'e dağıtmak için Azure Veri Bilimi Sanal Makinesi kullanabilirsiniz.  

Daha fazla bilgi için bkz. [sanal ağ yalıtımı ve gizliliğe genel bakış](how-to-network-security-overview.md).

Ayrıca, çalışma alanınız için Azure özel bağlantısını etkinleştirebilirsiniz. Özel bağlantı, bir Azure sanal ağından çalışma alanınıza yönelik iletişimleri kısıtlamanıza olanak sağlar. Daha fazla bilgi için bkz. [özel bağlantı yapılandırma](how-to-configure-private-link.md).

## <a name="data-encryption"></a>Veri şifrelemesi

> [!IMPORTANT]
> __Eğitim__ sırasında üretim sınıfı şifrelemesi için, Microsoft Azure Machine Learning işlem kümesi kullanmayı önerir. Microsoft __, Microsoft__ Azure Kubernetes hizmetini kullanarak üretim sınıfı şifrelemesi için önerilir.
>
> Azure Machine Learning işlem örneği bir geliştirme/test ortamıdır. Bunu kullanırken, dosya paylaşımında Not defterleri ve betikler gibi dosyalarınızı depolamanızı öneririz. Verileriniz bir veri deposunda depolanmalıdır.

### <a name="encryption-at-rest"></a>Bekleme sırasında şifreleme

> [!IMPORTANT]
> Çalışma alanınız hassas veriler içeriyorsa, çalışma alanınızı oluştururken [hbi_workspace bayrağını](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace%28class%29?view=azure-ml-py&preserve-view=true#&preserve-view=truecreate-name--auth-none--subscription-id-none--resource-group-none--location-none--create-resource-group-true--sku--basic---friendly-name-none--storage-account-none--key-vault-none--app-insights-none--container-registry-none--cmk-keyvault-none--resource-cmk-uri-none--hbi-workspace-false--default-cpu-compute-target-none--default-gpu-compute-target-none--exist-ok-false--show-output-true-) ayarlamayı öneririz. `hbi_workspace`Bayrak yalnızca bir çalışma alanı oluşturulduğunda ayarlanabilir. Mevcut bir çalışma alanı için değiştirilemez.

`hbi_workspace`Bayrak, [Microsoft 'un tanılama amacıyla topladığı veri](#microsoft-collected-data) miktarını denetler ve [Microsoft tarafından yönetilen ortamlarda ek şifrelemeye](../security/fundamentals/encryption-atrest.md)izin vermez. Ayrıca, aşağıdaki eylemleri sunar:

* Azure Machine Learning işlem kümenizde, bu abonelikte daha önceki kümelerin oluşturulmadığından, yerel çalışma diskini şifrelemeye başlar. Aksi takdirde, işlem kümelerinizin karalama diskinin şifrelenmesini etkinleştirmek için bir destek bileti yükseltmeniz gerekir 
* Çalıştırmalar arasında yerel karalama diskinizi temizler
* Anahtar kasanızı kullanarak depolama hesabınız, kapsayıcı kayıt defteriniz ve SSH hesabınız için kimlik bilgilerini yürütme katmanından işlem kümelerinize güvenli bir şekilde geçirir
* Temel alınan toplu iş havuzlarının AzureMachineLearningService dışında herhangi bir dış hizmet tarafından çağrılıp çağrılmaması için IP filtrelemeyi sağlar

#### <a name="azure-blob-storage"></a>Azure Blob depolama

Azure Machine Learning, Azure Machine Learning çalışma alanına ve aboneliğinize bağlı Azure Blob depolama hesabında anlık görüntüler, çıktılar ve Günlükler depolar. Azure Blob depolama alanında depolanan tüm veriler, Microsoft tarafından yönetilen anahtarlarla Rest 'te şifrelenir.

Azure Blob depolama alanında depolanan veriler için kendi anahtarlarınızı kullanma hakkında daha fazla bilgi için, bkz. [Azure Key Vault içindeki müşteri tarafından yönetilen anahtarlarla Azure depolama şifrelemesi](../storage/common/storage-encryption-keys-portal.md).

Eğitim verileri genellikle Azure Blob Storage 'da depolanır, böylece işlem hedeflerini eğitmek için erişilebilir. Bu depolama Azure Machine Learning tarafından yönetilmez ancak uzak bir dosya sistemi olarak işlem hedeflerine bağlanır.

Anahtarınızı __döndürmenize veya iptal__ etmeniz gerekiyorsa, bunu istediğiniz zaman yapabilirsiniz. Bir anahtar döndürürken depolama hesabı, bekleyen verileri şifrelemek için yeni anahtarı (en son sürüm) kullanmaya başlar. Bir anahtarı iptal ettiğinizde (devre dışı bırakırken), depolama hesabı başarısız istekleri üstlenir. Genellikle döndürme veya İptalin etkili olması için bir saat sürer.

Erişim anahtarlarını yeniden oluşturma hakkında daha fazla bilgi için bkz. [depolama erişim anahtarlarını yeniden üretme](how-to-change-storage-access-key.md).

#### <a name="azure-cosmos-db"></a>Azure Cosmos DB

Azure Machine Learning ölçümleri ve meta verileri bir Azure Cosmos DB örneğinde depolar. Bu örnek, Azure Machine Learning tarafından yönetilen bir Microsoft aboneliği ile ilişkilendirilir. Azure Cosmos DB depolanan tüm veriler, Microsoft tarafından yönetilen anahtarlarla birlikte geri kalanında şifrelenir.

Azure Cosmos DB örneğini şifrelemek için kendi (müşteri tarafından yönetilen) anahtarlarınızı kullanmak için, çalışma alanınız ile kullanmak üzere adanmış bir Cosmos DB örneği oluşturabilirsiniz. Çalışma geçmişi bilgilerini, Microsoft aboneliğimizde barındırılan çok kiracılı Cosmos DB örneğinin dışında depolamak istiyorsanız bu yaklaşımı öneririz. 

Abonelikinizde müşteri tarafından yönetilen anahtarlarla Cosmos DB bir örnek sağlamayı etkinleştirmek için aşağıdaki eylemleri gerçekleştirin:

* Henüz yapılmadıysanız Microsoft. Machinöğrenim ve Microsoft.DocumentDB kaynak sağlayıcılarını aboneliğinize kaydedin.

* Azure Machine Learning çalışma alanını oluştururken aşağıdaki parametreleri kullanın. Her iki parametre de zorunludur ve SDK, CLı, REST API 'Ler ve Kaynak Yöneticisi şablonlarda desteklenir.

    * `resource_cmk_uri`: Bu parametre, anahtarın [sürüm bilgileri](../key-vault/about-keys-secrets-and-certificates.md#objects-identifiers-and-versioning)de dahil olmak üzere, anahtar kasasındaki müşterinin yönettiği anahtarın tam kaynak URI 'sidir. 

    * `cmk_keyvault`: Bu parametre, aboneliğinizdeki anahtar kasasının kaynak KIMLIĞIDIR. Bu anahtar kasasının, Azure Machine Learning çalışma alanı için kullanacağınız bölge ve abonelikte olması gerekir. 
    
        > [!NOTE]
        > Bu Anahtar Kasası örneği, çalışma alanını sağladığınızda Azure Machine Learning tarafından oluşturulan anahtar kasasından farklı olabilir. Çalışma alanı için aynı Anahtar Kasası örneğini kullanmak istiyorsanız, [key_vault parametresini](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace%28class%29?view=azure-ml-py&preserve-view=true#&preserve-view=truecreate-name--auth-none--subscription-id-none--resource-group-none--location-none--create-resource-group-true--sku--basic---friendly-name-none--storage-account-none--key-vault-none--app-insights-none--container-registry-none--cmk-keyvault-none--resource-cmk-uri-none--hbi-workspace-false--default-cpu-compute-target-none--default-gpu-compute-target-none--exist-ok-false--show-output-true-)kullanarak çalışma alanını sağlarken aynı anahtar kasasını geçirin. 

[!INCLUDE [machine-learning-customer-managed-keys.md](../../includes/machine-learning-customer-managed-keys.md)]

Anahtarınızı __döndürmenize veya iptal__ etmeniz gerekiyorsa, bunu istediğiniz zaman yapabilirsiniz. Bir anahtar döndürürken Cosmos DB, bekleyen verileri şifrelemek için yeni anahtarı (en son sürüm) kullanmaya başlar. Bir anahtarı iptal ettiğinizde (devre dışı bırakırken), Cosmos DB başarısız isteklerden yararlanır. Genellikle döndürme veya İptalin etkili olması için bir saat sürer.

Cosmos DB ile müşteri tarafından yönetilen anahtarlar hakkında daha fazla bilgi için bkz. [Azure Cosmos DB hesabınız için müşteri tarafından yönetilen anahtarları yapılandırma](../cosmos-db/how-to-setup-cmk.md).

#### <a name="azure-container-registry"></a>Azure Container Registry

Kayıt defterinizde (Azure Container Registry) bulunan tüm kapsayıcı görüntüleri, bekleyen olarak şifrelenir. Azure, bir görüntüyü depolamadan önce otomatik olarak şifreler ve Azure Machine Learning görüntüyü aldığında şifresini çözer.

Azure Container Registry şifrelemek için kendi (müşteri tarafından yönetilen) anahtarlarınızı kullanmak için, kendi ACR 'nizi oluşturmanız ve çalışma alanını sağlarken veya çalışma alanı sağlama sırasında oluşturulan varsayılan örneği şifrelemeniz gerekir.

> [!IMPORTANT]
> Azure Machine Learning, yönetici hesabının Azure Container Registry etkinleştirilmesini gerektirir. Varsayılan olarak, bir kapsayıcı kayıt defteri oluşturduğunuzda bu ayar devre dışıdır. Yönetici hesabını etkinleştirme hakkında daha fazla bilgi için bkz. [yönetici hesabı](/azure/container-registry/container-registry-authentication#admin-account).
>
> Bir çalışma alanı için Azure Container Registry oluşturulduktan sonra silmeyin. Bunu yapmak Azure Machine Learning çalışma alanınızı bozacaktır.

Mevcut bir Azure Container Registry kullanarak çalışma alanı oluşturma örneği için aşağıdaki makalelere bakın:

* [Azure CLI ile Azure Machine Learning için bir çalışma alanı oluşturun](how-to-manage-workspace-cli.md).
* [Python SDK ile bir çalışma alanı oluşturun](how-to-manage-workspace.md?tabs=python#create-a-workspace).
* [Azure Machine Learning için bir çalışma alanı oluşturmak üzere Azure Resource Manager şablonu kullanma](how-to-create-workspace-template.md)

#### <a name="azure-container-instance"></a>Azure Container Örneği

Dağıtılan bir Azure Container Instance (acı) kaynağını, müşteri tarafından yönetilen anahtarları kullanarak şifreleyebilirsiniz. ACI için kullanılan müşteri tarafından yönetilen anahtar, çalışma alanınızın Azure Key Vault depolanabilir. Anahtar oluşturma hakkında bilgi için bkz. [müşteri tarafından yönetilen bir anahtarla verileri şifreleme](../container-instances/container-instances-encrypt-data.md#generate-a-new-key).

Azure Container Instance 'a model dağıttığınızda anahtarı kullanmak için kullanarak yeni bir dağıtım yapılandırması oluşturun `AciWebservice.deploy_configuration()` . Aşağıdaki parametreleri kullanarak anahtar bilgilerini sağlayın:

* `cmk_vault_base_url`: Anahtarı içeren anahtar kasasının URL 'SI.
* `cmk_key_name`: Anahtarın adı.
* `cmk_key_version`: Anahtarın sürümü.

Dağıtım yapılandırması oluşturma ve kullanma hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [AciWebservice.deploy_configuration ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aci.aciwebservice?view=azure-ml-py&preserve-view=true#&preserve-view=truedeploy-configuration-cpu-cores-none--memory-gb-none--tags-none--properties-none--description-none--location-none--auth-enabled-none--ssl-enabled-none--enable-app-insights-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--ssl-cname-none--dns-name-label-none--primary-key-none--secondary-key-none--collect-model-data-none--cmk-vault-base-url-none--cmk-key-name-none--cmk-key-version-none-) başvurusu
* [Dağıtım nereye ve nasıl yapılır?](how-to-deploy-and-where.md)
* [Modeli Azure Container Instances’a dağıtma](how-to-deploy-azure-container-instance.md)

ACI ile müşteri tarafından yönetilen anahtar kullanma hakkında daha fazla bilgi için bkz. [müşteri tarafından yönetilen bir anahtarla verileri şifreleme](../container-instances/container-instances-encrypt-data.md#encrypt-data-with-a-customer-managed-key).

#### <a name="azure-kubernetes-service"></a>Azure Kubernetes Service

Dağıtılan bir Azure Kubernetes hizmeti kaynağını, müşteri tarafından yönetilen anahtarları dilediğiniz zaman kullanarak şifreleyebilirsiniz. Daha fazla bilgi için bkz. [Azure Kubernetes hizmeti ile kendi anahtarlarınızı getirme](../aks/azure-disk-customer-managed-keys.md). 

Bu işlem, Kubernetes kümesindeki dağıtılan sanal makinelerin hem verilerini hem de işletim sistemi diskini şifrelemenizi sağlar.

> [!IMPORTANT]
> Bu işlem yalnızca AKS K8s sürüm 1,17 veya üzeri sürümlerle kullanılabilir. 13 Ocak 2020 ' de AKS 1,17 için destek eklendi Azure Machine Learning.

#### <a name="machine-learning-compute"></a>Machine Learning İşlem

Azure depolama 'da depolanan her işlem düğümü için işletim sistemi diski, Azure Machine Learning depolama hesaplarında Microsoft tarafından yönetilen anahtarlarla şifrelenir. Bu işlem hedefi kısa ömürlü ve hiçbir çalışma sıraya alınmaz kümeler genellikle ölçeği aşağı ölçeklendirilir. Temel alınan sanal makine de sağlanmamıştır ve işletim sistemi diski silinir. Azure disk şifrelemesi, işletim sistemi diski için desteklenmez.

Her bir sanal makinenin işletim sistemi işlemleri için yerel bir geçici diski de vardır. İsterseniz eğitim verilerini hazırlamak için diski kullanabilirsiniz. Parametresi, parametresi olarak ayarlanan çalışma alanları için varsayılan olarak şifrelenir `hbi_workspace` `TRUE` . Bu ortam yalnızca çalıştırma süresince kısa ömürlü ve şifreleme desteği yalnızca sistem tarafından yönetilen anahtarlarla sınırlıdır.

#### <a name="azure-databricks"></a>Azure Databricks

Azure Databricks, Azure Machine Learning işlem hatları içinde kullanılabilir. Varsayılan olarak, Azure Databricks tarafından kullanılan Databricks dosya sistemi (DBFS), Microsoft tarafından yönetilen bir anahtar kullanılarak şifrelenir. Azure Databricks, müşteri tarafından yönetilen anahtarları kullanacak şekilde yapılandırmak için, bkz. [varsayılan (root) için müşteri tarafından yönetilen anahtarları yapılandırma](/azure/databricks/security/customer-managed-keys-dbfs).

### <a name="encryption-in-transit"></a>Aktarım sırasında şifreleme

Azure Machine Learning, çeşitli Azure Machine Learning mikro hizmetler arasında iç iletişimin güvenliğini sağlamak için TLS kullanır. Tüm Azure depolama erişimi, güvenli bir kanal üzerinden de gerçekleşir.

Puanlama uç noktasına yapılan dış çağrıların güvenliğini sağlamak için Azure Machine Learning TLS kullanır. Daha fazla bilgi için bkz. [Azure Machine Learning aracılığıyla bir Web hizmetini güvenli hale getirmek IÇIN TLS kullanma](https://docs.microsoft.com/azure/machine-learning/how-to-secure-web-service).

### <a name="using-azure-key-vault"></a>Azure Key Vault kullanma

Azure Machine Learning, çeşitli türlerdeki kimlik bilgilerini depolamak için çalışma alanıyla ilişkili Azure Key Vault örneğini kullanır:

* İlişkili depolama hesabı bağlantı dizesi
* Azure Container Repository deposu örneklerine parolalar
* Veri depolarına yönelik bağlantı dizeleri

Azure HDInsight ve VM 'Ler gibi hedefleri hesaplamak için SSH parolaları ve anahtarları, Microsoft aboneliğiyle ilişkili ayrı bir anahtar kasasında depolanır. Azure Machine Learning, kullanıcılar tarafından sunulan herhangi bir parolayı veya anahtarı depolamaz. Bunun yerine, denemeleri çalıştırmak için VM 'Leri ve HDInsight 'a bağlanmak üzere kendi SSH anahtarlarını oluşturur, yetkilendirir ve depolar.

Her çalışma alanı, çalışma alanıyla aynı ada sahip ilişkili, sistem tarafından atanan bir yönetilen kimliğe sahiptir. Bu yönetilen kimliğin, anahtar kasasındaki tüm anahtar, gizli dizi ve sertifikalara erişimi vardır.

## <a name="data-collection-and-handling"></a>Veri toplama ve işleme

### <a name="microsoft-collected-data"></a>Microsoft tarafından toplanan veriler

Microsoft, kaynak adları (örneğin, veri kümesi adı veya makine öğrenimi deneme adı) gibi kullanıcı olmayan tanımlama bilgilerini veya tanılama amacıyla iş ortamı değişkenlerini toplayabilir. Bu tür veriler, Microsoft 'un sahip olduğu aboneliklerde barındırılan depolamada Microsoft tarafından yönetilen anahtarlar kullanılarak depolanır ve [Microsoft 'un standart Gizlilik ilkesi ve veri işleme standartlarını](https://privacy.microsoft.com/privacystatement)izler.

Microsoft ayrıca, önemli bilgileri (örneğin, hesap anahtarı gizli dizileri) ortam değişkenlerine depolamamanızı da önerir. Ortam değişkenleri günlüğe kaydedilir, şifrelenir ve bizimle saklanır. Benzer şekilde [run_id](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29?view=azure-ml-py&preserve-view=true)adlandırırken, Kullanıcı adları veya gizli proje adları gibi hassas bilgileri de eklemekten kaçının. Bu bilgiler, Microsoft Desteği mühendislerine erişilebilen telemetri günlüklerinde görünebilir.

`hbi_workspace`Çalışma alanını sağlarken parametresini olarak ayarlayarak, toplanan tanılama verilerinden bu verileri devre dışı kalabilirsiniz `TRUE` . Bu işlev, AzureML Python SDK 'Sı, CLı, REST API 'Leri veya Azure Resource Manager şablonları kullanılırken desteklenir.

### <a name="microsoft-generated-data"></a>Microsoft tarafından oluşturulan veriler

Microsoft, otomatik Machine Learning gibi hizmetleri kullanırken, birden çok modeli eğitmek için geçici, önceden işlenmiş bir veri oluşturabilir. Bu veriler, çalışma alanınızdaki bir veri deposunda depolanır ve bu da erişim denetimlerini ve şifrelemeyi uygun şekilde zorlamanıza olanak sağlar.

[Dağıtılmış uç noktanıza kaydedilen tanılama bilgilerini](how-to-enable-app-insights.md) Azure Application Insights örneğinize şifrelemek da isteyebilirsiniz.

## <a name="monitoring"></a>İzleme

### <a name="metrics"></a>Ölçümler

Azure Machine Learning çalışma alanınızın ölçümlerini görüntülemek ve izlemek için Azure Izleyici ölçümlerini kullanabilirsiniz. [Azure Portal](https://portal.azure.com), çalışma alanınızı seçin ve ardından **ölçümler** ' i seçin:

[![Bir çalışma alanı için örnek ölçümleri gösteren ekran görüntüsü](media/concept-enterprise-security/workspace-metrics.png)](media/concept-enterprise-security/workspace-metrics-expanded.png#lightbox)

Ölçümler, çalıştırmalar, dağıtımlar ve kayıtlar hakkındaki bilgileri içerir.

Daha fazla bilgi için bkz. [Azure izleyici 'de ölçümler](/azure/azure-monitor/platform/data-platform-metrics).

### <a name="activity-log"></a>Etkinlik günlüğü

Çalışma alanında gerçekleştirilen çeşitli işlemleri görmek için bir çalışma alanının etkinlik günlüğünü görüntüleyebilirsiniz. Günlük, işlem adı, olay başlatıcısı ve zaman damgası gibi temel bilgileri içerir.

Bu ekran görüntüsünde bir çalışma alanının etkinlik günlüğü gösterilmektedir:

[![Bir çalışma alanının etkinlik günlüğünü gösteren ekran görüntüsü](media/concept-enterprise-security/workspace-activity-log.png)](media/concept-enterprise-security/workspace-activity-log-expanded.png#lightbox)

Puanlama isteği ayrıntıları Application Insights depolanır. Application Insights, bir çalışma alanı oluşturduğunuzda aboneliğinizde oluşturulur. Günlüğe kaydedilen bilgiler aşağıdakiler gibi alanları içerir:

* HTTPMethod
* Kullanıcı
* ComputeType
* RequestUrl
* Durum
* No
* Süre

> [!IMPORTANT]
> Azure Machine Learning çalışma alanındaki bazı eylemler etkinlik günlüğüne bilgileri günlüğe vermez. Örneğin, bir eğitimin başlangıcı ve bir modelin kaydı günlüğe kaydedilmez.
>
> Bu eylemlerden bazıları, çalışma alanınızın **Etkinlikler** alanında görünür ancak bu bildirimler etkinliği kimin başlattığını göstermez.

### <a name="vulnerability-scanning"></a>Güvenlik açığı taraması

Azure Güvenlik Merkezi, hibrit bulut iş yükleri arasında birleşik güvenlik yönetimi ve gelişmiş tehdit koruması sağlar. Azure Machine Learning için Azure Container Registry kaynağınızın ve Azure Kubernetes hizmet kaynaklarınızın taranmasını etkinleştirmeniz gerekir. Bkz. Güvenlik Merkezi [Ile Azure Kubernetes hizmet tümleştirmesi ve Güvenlik Merkezi ile](https://docs.microsoft.com/azure/security-center/azure-kubernetes-service-integration) [Azure Container Registry görüntü tarama](https://docs.microsoft.com/azure/security-center/azure-container-registry-integration) .

## <a name="data-flow-diagrams"></a>Veri akışı diyagramları

### <a name="create-workspace"></a>Çalışma alanı oluşturma

Aşağıdaki diyagramda, çalışma alanı oluşturma iş akışı gösterilmektedir.

* Desteklenen Azure Machine Learning istemcilerinden birinden (Azure CLı, Python SDK, Azure portal) Azure AD 'de oturum açıp uygun Azure Resource Manager belirtecini istemeniz gerekir.
* Çalışma alanını oluşturmak için Azure Resource Manager çağırın. 
* Azure Resource Manager, çalışma alanını sağlamak için Azure Machine Learning kaynak sağlayıcısıyla iletişim kurar.

Çalışma alanı oluşturma sırasında kullanıcının aboneliğinde ek kaynaklar oluşturulur:

* Key Vault (gizli dizileri depolamak için)
* Bir Azure depolama hesabı (blob ve dosya paylaşma dahil)
* Azure Container Registry (çıkarım/Puanlama ve deneme için Docker görüntülerini depolamak için)
* Application Insights (Telemetriyi depolamak için)

Kullanıcı, gerektiğinde bir çalışma alanına (Azure Kubernetes hizmeti veya VM 'Ler gibi) bağlı diğer işlem hedeflerini de temin edebilir.

[![Çalışma alanı iş akışı oluştur](media/concept-enterprise-security/create-workspace.png)](media/concept-enterprise-security/create-workspace.png#lightbox)

### <a name="save-source-code-training-scripts"></a>Kaynak kodunu kaydet (eğitim betikleri)

Aşağıdaki diyagramda, kod anlık görüntüsü iş akışı gösterilmektedir.

Bir Azure Machine Learning çalışma alanıyla ilişkili, kaynak kodu (eğitim betikleri) içeren dizinlerdir (denemeleri). Bu betikler yerel makinenizde ve bulutta depolanır (aboneliğiniz için Azure Blob depolama alanında). Kod anlık görüntüleri, geçmiş denetimi için yürütme veya denetleme için kullanılır.

[![Kod anlık görüntüsü iş akışı](media/concept-enterprise-security/code-snapshot.png)](media/concept-enterprise-security/code-snapshot.png#lightbox)

### <a name="training"></a>Eğitim

Aşağıdaki diyagramda eğitim iş akışı gösterilmektedir.

* Azure Machine Learning, önceki bölümde kaydedilen kod anlık görüntüsünün anlık görüntü KIMLIĞIYLE çağırılır.
* Azure Machine Learning, bir çalıştırma KIMLIĞI (isteğe bağlı) ve Machine Learning bir hizmet belirteci oluşturur. Bu, daha sonra Machine Learning hizmetiyle iletişim kurmak için Machine Learning İşlem/VM gibi işlem hedefleri tarafından daha sonra kullanılır.
* Eğitim işlerini çalıştırmak için yönetilen bir işlem hedefi (Machine Learning İşlem gibi) veya yönetilmeyen bir işlem hedefi (VM 'Ler gibi) seçebilirsiniz. Her iki senaryo için de veri akışları aşağıda verilmiştir:
   * Microsoft aboneliğindeki bir anahtar kasasında SSH kimlik bilgileri tarafından erişilen VM 'Ler/HDInsight. Azure Machine Learning, işlem hedefinde yönetim kodu çalıştırır:

   1. Ortamı hazırlar. (Docker, VM 'Ler ve yerel bilgisayarlar için bir seçenektir. Docker kapsayıcılarındaki denemeleri 'ın nasıl çalıştığını anlamak için Machine Learning İşlem için aşağıdaki adımlara bakın.)
   1. Kodu indirir.
   1. Ortam değişkenlerini ve konfigürasyonları ayarlar.
   1. Kullanıcı komut dosyalarını çalıştırır (önceki bölümde bahsedilen kod anlık görüntüsü).

   * Machine Learning İşlem, çalışma alanı tarafından yönetilen bir kimlikle erişilir.
Machine Learning İşlem yönetilen bir işlem hedefi olduğundan (Microsoft tarafından yönetilen), Microsoft Aboneliğiniz kapsamında çalışır.

   1. Gerekirse uzak Docker oluşturma kapalıdır.
   1. Yönetim kodu kullanıcının Azure dosya paylaşımında yazılır.
   1. Kapsayıcı bir başlangıç komutuyla başlatılır. Diğer bir deyişle, önceki adımda açıklandığı gibi yönetim kodu.

#### <a name="querying-runs-and-metrics"></a>Çalıştırmaları ve ölçümleri sorgulama

Aşağıdaki akış diyagramında, bu adım, eğitim işlem hedefi Cosmos DB veritabanındaki depolamadan Azure Machine Learning için yeniden çalıştırma ölçümleri yazdığında meydana gelir. İstemciler, Azure Machine Learning çağırabilir. Machine Learning, Cosmos DB veritabanından çekme ölçümlerini açıp istemciye geri döndürmeyecektir.

[![Eğitim iş akışı](media/concept-enterprise-security/training-and-metrics.png)](media/concept-enterprise-security/training-and-metrics.png#lightbox)

### <a name="creating-web-services"></a>Web Hizmetleri oluşturma

Aşağıdaki diyagramda çıkarım iş akışı gösterilmektedir. Çıkarım veya model Puanlama, dağıtılan modelin, en yaygın olarak üretim verileri üzerinde tahmin için kullanıldığı aşamadır.

Ayrıntılar aşağıda verilmiştir:

* Kullanıcı, Azure Machine Learning SDK gibi bir istemciyi kullanarak bir modeli kaydeder.
* Kullanıcı bir model, bir puan dosyası ve diğer model bağımlılıklarını kullanarak bir görüntü oluşturur.
* Docker görüntüsü oluşturulup Azure Container Registry depolanır.
* Web hizmeti, önceki adımda oluşturulan görüntüyü kullanarak işlem hedefine (Container Instances/AKS) dağıtılır.
* Puanlama isteği ayrıntıları, Kullanıcı aboneliğindeki Application Insights depolanır.
* Telemetri ayrıca Microsoft/Azure aboneliğine de gönderilir.

[![Çıkarım iş akışı](media/concept-enterprise-security/inferencing.png)](media/concept-enterprise-security/inferencing.png#lightbox)

## <a name="audit-and-manage-compliance"></a>Uyumluluğu denetleme ve yönetme

[Azure ilkesi](/azure/governance/policy) , Azure kaynaklarının ilkelerinizle uyumlu olduğundan emin olmanızı sağlayan bir idare aracıdır. Azure Machine Learning, aşağıdaki ilkeleri atayabilirsiniz:

* **Müşteri tarafından yönetilen anahtar** : çalışma alanlarının müşteri tarafından yönetilen anahtar kullanması gerekip gerekmediğini denetleyin veya zorlayın.
* **Özel bağlantı** : çalışma alanlarının bir sanal ağla iletişim kurmak için özel uç nokta kullanıp kullanmadığını denetleyin.

Azure Ilkesi hakkında daha fazla bilgi için bkz. [Azure ilkesi belgeleri](/azure/governance/policy/overview).

Azure Machine Learning özgü ilkeler hakkında daha fazla bilgi için bkz. [Azure ilkesiyle uyumluluğu denetleme ve yönetme](how-to-integrate-azure-policy.md).

## <a name="resource-locks"></a>Kaynak kilitleri

[!INCLUDE [resource locks](../../includes/machine-learning-resource-lock.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [TLS ile güvenli Azure Machine Learning Web Hizmetleri](how-to-secure-web-service.md)
* [Web hizmeti olarak dağıtılan bir Machine Learning modeli kullanma](how-to-consume-web-service.md)
* [Azure Güvenlik Duvarı ile Azure Machine Learning kullanma](how-to-access-azureml-behind-firewall.md)
* [Azure sanal ağ ile Azure Machine Learning kullanma](how-to-network-security-overview.md)
* [Öneri sistemleri oluşturmak için en iyi uygulamalar](https://github.com/Microsoft/Recommenders)
* [Azure 'da gerçek zamanlı bir öneri API 'SI oluşturun](https://docs.microsoft.com/azure/architecture/reference-architectures/ai/real-time-recommendation)
