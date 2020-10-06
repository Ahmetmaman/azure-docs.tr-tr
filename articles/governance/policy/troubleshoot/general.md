---
title: Sık karşılaşılan hataları giderme
description: İlke tanımları, çeşitli SDK ve Kubernetes için eklenti oluşturma sorunlarını giderme hakkında bilgi edinin.
ms.date: 10/05/2020
ms.topic: troubleshooting
ms.openlocfilehash: 6026dc75187c8a70203a2484380eed70d519599d
ms.sourcegitcommit: a07a01afc9bffa0582519b57aa4967d27adcf91a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91743446"
---
# <a name="troubleshoot-errors-using-azure-policy"></a>Azure Ilkesini kullanarak hatalarda sorun giderme

İlke tanımları oluştururken, SDK ile çalışırken veya [Kubernetes eklentisi Için Azure ilkesini](../concepts/policy-for-kubernetes.md) ayarlarken hatalarla karşılaşabilirsiniz. Bu makalede, oluşabilecek çeşitli hatalar ve bunların nasıl çözümleneceği açıklanmaktadır.

## <a name="finding-error-details"></a>Hata ayrıntılarını bulma

Hata ayrıntılarının konumu hataya neden olan eyleme bağlıdır.

- Özel bir ilkeyle çalışırken, şema hakkında geri bildirim almak veya kaynakların nasıl değerlendirildiğini görmek üzere sonuç [uyumluluk verilerini](../how-to/get-compliance-data.md) gözden geçirmek için Azure Portal deneyin.
- SDK, çeşitli SDK ile çalışırken işlevin neden başarısız olduğuna ilişkin ayrıntıları sağlar.
- Kubernetes eklentisi ile çalışırken, kümede [oturum açma](../concepts/policy-for-kubernetes.md#logging) ile başlayın.

## <a name="general-errors"></a>Genel hatalar

### <a name="scenario-alias-not-found"></a>Senaryo: diğer ad bulunamadı

#### <a name="issue"></a>Sorun

Azure Ilkesi Azure Resource Manager özellikleriyle eşlemek için [takma adlar](../concepts/definition-structure.md#aliases) kullanır.

#### <a name="cause"></a>Nedeni

İlke tanımında yanlış veya varolmayan diğer ad kullanılıyor.

#### <a name="resolution"></a>Çözüm

İlk olarak, Kaynak Yöneticisi özelliğinin bir diğer adı olduğunu doğrulayın. Kullanılabilir diğer adları aramak için Visual Studio Code, [Azure Kaynak Grafiği](../../resource-graph/samples/starter.md#distinct-alias-values)veya SDK [için Azure ilke uzantısı](../how-to/extension-for-vscode.md)'nı kullanın. Bir Kaynak Yöneticisi özelliği için diğer ad yoksa, bir destek bileti oluşturun.

### <a name="scenario-evaluation-details-not-up-to-date"></a>Senaryo: değerlendirme ayrıntıları güncel değil

#### <a name="issue"></a>Sorun

Kaynak "başlatılmadı" durumunda veya uyumluluk ayrıntıları güncel değil.

#### <a name="cause"></a>Nedeni

Yeni bir ilke veya girişim atamasının uygulanması yaklaşık 30 dakika sürer. Mevcut bir atamanın kapsamındaki yeni veya güncelleştirilmiş kaynaklar 15 dakika sonra kullanılabilir hale gelir. Her 24 saatte bir standart uyumluluk taraması yapılır. Daha fazla bilgi için bkz. [değerlendirme Tetikleyicileri](../how-to/get-compliance-data.md#evaluation-triggers).

#### <a name="resolution"></a>Çözüm

İlk olarak, bir değerlendirmenin tamamlanmasını ve uyumluluk sonuçlarının Azure portal veya SDK 'da kullanılabilir hale gelmesi için uygun süreyi bekleyin. Azure PowerShell veya REST API ile yeni bir değerlendirme taraması başlatmak için bkz. [isteğe bağlı değerlendirme taraması](../how-to/get-compliance-data.md#on-demand-evaluation-scan).

### <a name="scenario-compliance-not-as-expected"></a>Senaryo: uyumluluk beklendiği gibi değil

#### <a name="issue"></a>Sorun

Kaynak, bu kaynak için beklenen, _uyumlu_ veya _uyumsuz_olan değerlendirme durumunda değil.

#### <a name="cause"></a>Nedeni

Kaynak, ilke atamasının doğru kapsamında değil veya ilke tanımı istenen şekilde çalışmıyor.

#### <a name="resolution"></a>Çözüm

İlke tanımınızda sorun gidermek için aşağıdaki adımları izleyin:

1. İlk olarak, bir değerlendirmenin tamamlanmasını ve uyumluluk sonuçlarının Azure portal veya SDK 'da kullanılabilir hale gelmesi için uygun süreyi bekleyin. Azure PowerShell veya REST API ile yeni bir değerlendirme taraması başlatmak için bkz. [isteğe bağlı değerlendirme taraması](../how-to/get-compliance-data.md#on-demand-evaluation-scan).
1. Atama parametrelerinin ve atama kapsamının doğru şekilde ayarlandığından emin olun.
1. [İlke tanımı modunu](../concepts/definition-structure.md#mode)denetleyin:
   - Tüm kaynak türleri için ' All ' modu.
   - İlke tanımı etiketleri veya konumu denetlediğinde ' dizinli ' modu.
1. Kaynak kapsamının [dışlandığından](../concepts/assignment-structure.md#excluded-scopes) veya [muaf](../concepts/exemption-structure.md)olmadığından emin olun.
1. Bir ilke atamasının uyumluluğu `0/0` kaynakları gösteriyorsa, atama kapsamında geçerli olacak hiçbir kaynak belirlenmemiştir. Hem ilke tanımını hem de atama kapsamını denetleyin.
1. Uyumlu olması beklenen uyumlu olmayan bir kaynak için, [uyumsuzluk nedenlerini belirlemek](../how-to/determine-non-compliance.md)için denetleyin. Tanımın değerlendirilen Özellik değeri ile karşılaştırılması, kaynağın neden uyumlu olmadığını gösterir.
   - **Hedef değer** yanlış ise, ilke tanımını gözden geçirin.
   - **Geçerli değer** yanlışsa kaynak yükünü ile doğrulayın `resources.azure.com` .
1. Sorun gidermeyi denetle: diğer yaygın sorunlar ve çözümler için [zorlama beklendiği gibi değil](#scenario-enforcement-not-as-expected) .

Yinelenen ve özelleştirilmiş yerleşik ilke tanımı veya özel tanımınızda sorun yaşıyorsanız, sorunu doğru bir şekilde yönlendirmek için **Ilke yazma** altında bir destek bileti oluşturun.

### <a name="scenario-enforcement-not-as-expected"></a>Senaryo: zorlama beklendiği gibi değil

#### <a name="issue"></a>Sorun

Azure Ilkesi tarafından işlem yapılması beklenen bir kaynak değildir ve [Azure etkinlik günlüğünde](../../../azure-monitor/platform/platform-logs-overview.md)giriş yoktur.

#### <a name="cause"></a>Nedeni

İlke ataması, _Disabled_'ın [Enforcementmode](../concepts/assignment-structure.md#enforcement-mode) için yapılandırılmış. Zorlama modu devre dışı olsa da, ilke efekti zorlanmaz ve etkinlik günlüğünde giriş yoktur.

#### <a name="resolution"></a>Çözüm

İlke atamalarınızın zorlanmasıyla ilgili sorunları gidermek için aşağıdaki adımları izleyin:

1. İlk olarak, bir değerlendirmenin tamamlanmasını ve uyumluluk sonuçlarının Azure portal veya SDK 'da kullanılabilir hale gelmesi için uygun süreyi bekleyin. Azure PowerShell veya REST API ile yeni bir değerlendirme taraması başlatmak için bkz. [isteğe bağlı değerlendirme taraması](../how-to/get-compliance-data.md#on-demand-evaluation-scan).
1. Atama parametrelerinin ve atama kapsamının doğru ayarlandığından ve **Enforcementmode** 'un _etkin_olduğundan emin olun. 
1. [İlke tanımı modunu](../concepts/definition-structure.md#mode)denetleyin:
   - Tüm kaynak türleri için ' All ' modu.
   - İlke tanımı etiketleri veya konumu denetlediğinde ' dizinli ' modu.
1. Kaynak kapsamının [dışlandığından](../concepts/assignment-structure.md#excluded-scopes) veya [muaf](../concepts/exemption-structure.md)olmadığından emin olun.
1. Kaynak yükünün ilke mantığını eşleştirdiğini doğrulayın. Bu işlem, [BIR har izlemesi yakalanarak](../../../azure-portal/capture-browser-trace.md) veya ARM şablon özelliklerini inceleyerek yapılabilir.
1. Sorun gidermeyi denetle: diğer yaygın sorunlar ve çözümler için [Uyumluluk beklendiği gibi değil](#scenario-compliance-not-as-expected) .

Yinelenen ve özelleştirilmiş yerleşik ilke tanımı veya özel tanımınızda sorun yaşıyorsanız, sorunu doğru bir şekilde yönlendirmek için **Ilke yazma** altında bir destek bileti oluşturun.

### <a name="scenario-denied-by-azure-policy"></a>Senaryo: Azure Ilkesi tarafından reddedildi

#### <a name="issue"></a>Sorun

Bir kaynağın oluşturulması veya güncelleştirilmesi reddedildi.

#### <a name="cause"></a>Nedeni

Yeni veya güncelleştirilmiş kaynağınızın kapsamına yönelik bir ilke ataması, bir ilke tanımının bir [reddetme](../concepts/effects.md#deny) etkisi ile ölçütlerine sahip olduğunu karşılar. Bu tanımların toplantılarının oluşturulması veya güncelleştirilmesini engellenir.

#### <a name="resolution"></a>Çözüm

Bir reddetme ilke atamasından alınan hata iletisi, ilke tanımı ve ilke atama kimliklerini içerir. İletideki hata bilgileri kaçırıldığında, [etkinlik günlüğünde](../../../azure-monitor/platform/activity-log.md#view-the-activity-log)da kullanılabilir. Bu bilgileri, kaynak kısıtlamalarını anlamak ve izin verilen değerleri eşlemek için istekteki kaynak özelliklerini ayarlamak üzere daha fazla bilgi almak için kullanın.

## <a name="template-errors"></a>Şablon hataları

### <a name="scenario-policy-supported-functions-processed-by-template"></a>Senaryo: şablon tarafından işlenen Ilke tarafından desteklenen işlevler

#### <a name="issue"></a>Sorun

Azure Ilkesi, bir dizi Azure Resource Manager şablonu (ARM şablonu) işlevini ve yalnızca bir ilke tanımında bulunan işlevleri destekler. Kaynak Yöneticisi, bu işlevleri bir ilke tanımının bir parçası yerine bir dağıtımın parçası olarak işler.

#### <a name="cause"></a>Nedeni

Veya gibi desteklenen işlevleri kullanarak `parameter()` `resourceGroup()` , ilke tanımı ve Azure ilke altyapısının işlevini işlemek yerine, dağıtım sırasında işlevin işlenen sonucuna neden olur.

#### <a name="resolution"></a>Çözüm

Bir işlevi bir ilke tanımının parçası olacak şekilde geçirmek için, tüm dizeyi, özelliği gibi görünecek şekilde kaçış `[` `[[resourceGroup().tags.myTag]` . Kaçış karakteri, şablonu işlerken Kaynak Yöneticisi değeri bir dize olarak işlemesine neden olur. Daha sonra Azure Ilkesi, işlevin beklenen şekilde dinamik olmasını sağlayan ilkeyi ilke tanımına koyar. Daha fazla bilgi için bkz. [Azure Resource Manager şablonlarındaki sözdizimi ve ifadeler](../../../azure-resource-manager/templates/template-expressions.md).

## <a name="add-on-installation-errors"></a>Eklenti yükleme hataları

### <a name="scenario-install-using-helm-chart-fails-on-password"></a>Senaryo: parola üzerinde Held grafik kullanarak Install başarısız oluyor

#### <a name="issue"></a>Sorun

`helm install azure-policy-addon`Komut aşağıdaki iletilerden biriyle başarısız olur:

- `!: event not found`
- `Error: failed parsing --set data: key "<key>" has no value (cannot end with ,)`

#### <a name="cause"></a>Nedeni

Oluşturulan parola, `,` helb grafiğinin bölünmesi için bir virgül () içerir.

#### <a name="resolution"></a>Çözüm

`,` `helm install azure-policy-addon` Bir ters eğik çizgiyle () çalışırken parola değerindeki virgül () karakterini kaçış `\` .

### <a name="scenario-install-using-helm-chart-fails-as-name-already-exists"></a>Senaryo: ad zaten mevcut olduğu için Held grafiğini kullanarak Install başarısız olur

#### <a name="issue"></a>Sorun

`helm install azure-policy-addon`Komut aşağıdaki iletiyle başarısız olur:

- `Error: cannot re-use a name that is still in use`

#### <a name="cause"></a>Nedeni

Ada sahip Held grafiği `azure-policy-addon` zaten yüklenmiş veya kısmen yüklenmiş.

#### <a name="resolution"></a>Çözüm

[Kubernetes eklentisinin Azure ilkesini kaldırmak](../concepts/policy-for-kubernetes.md#remove-the-add-on)için yönergeleri izleyin, sonra komutu yeniden çalıştırın `helm install azure-policy-addon` .

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

- [Microsoft Q&A](/answers/topics/azure-policy.html)aracılığıyla uzmanlardan yanıt alın.
- [@AzureSupport](https://twitter.com/azuresupport)Azure Community 'yi doğru kaynaklara bağlayarak müşteri deneyimini iyileştirmeye yönelik resmi Microsoft Azure hesabı ile bağlanın: yanıtlar, destek ve uzmanlar.
- Daha fazla yardıma ihtiyacınız varsa, bir Azure destek olayı dosyası gönderebilirsiniz. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve **Destek Al**' ı seçin.
