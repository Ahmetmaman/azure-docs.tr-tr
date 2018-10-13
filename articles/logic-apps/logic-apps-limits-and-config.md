---
title: Limitler ve yapılandırma - Azure Logic Apps | Microsoft Docs
description: Hizmet sınırlamaları ve Azure Logic Apps için yapılandırma değerleri
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 10/11/2018
ms.openlocfilehash: 8aa2627f46be1e375fb3c3e565848a930ba6726b
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/12/2018
ms.locfileid: "49167452"
---
# <a name="limits-and-configuration-information-for-azure-logic-apps"></a>Limitler ve yapılandırma bilgilerini Azure Logic Apps

Bu makalede, sınırlar ve oluşturma ve Azure Logic Apps ile otomatik iş akışları çalıştırma için yapılandırma ayrıntılarını açıklar. Microsoft Flow için bkz: [limitler ve yapılandırma, Microsoft Flow](https://docs.microsoft.com/flow/limits-and-config).

<a name="definition-limits"></a>

## <a name="definition-limits"></a>Tanım limitleri

Bir tek bir mantıksal uygulama tanımını sınırları şunlardır:

| Ad | Sınır | Notlar | 
| ---- | ----- | ----- | 
| İş akışı başına Eylemler | 500 | Bu sınırı genişletmek için gerektiği şekilde iç içe geçmiş iş akışları ekleyebilirsiniz. |
| İzin verilen eylemleri için iç içe geçme derinliği | 8 | Bu sınırı genişletmek için gerektiği şekilde iç içe geçmiş iş akışları ekleyebilirsiniz. | 
| Her Abonelikteki bölge başına iş akışları | 1000 | | 
| İş akışı başına Tetikleyicileri | 10 | Tasarımcı, kod görünümü çalışırken | 
| Geçiş kapsamı çalışmaları sınırı | 25 | | 
| İş akışı başına değişkenleri | 250 | | 
| Karakter başına ifadesi | 8,192 | | 
| En büyük boyutu `trackedProperties` | 16.000 karakter | 
| Adında `action` veya `trigger` | 80 karakter | | 
| Uzunluğu `description` | 256 karakter | | 
| En fazla `parameters` | 50 | | 
| En fazla `outputs` | 10 | | 
||||  

<a name="run-duration-retention-limits"></a>

## <a name="run-duration-and-retention-limits"></a>Çalışma süresi ve bekletme sınırları

Bir tek bir mantıksal uygulama çalıştırması sınırları şunlardır:

| Ad | Sınır | Notlar | 
|------|-------|-------| 
| Çalıştırma süresi | 90 gün | Bu sınırı değiştirmek için bkz: [değişiklik çalıştırma süresini](#change-duration). | 
| Depolama bekletme | 90 gün içinde çalıştırmanın başlangıç saati | 7 gün-90 gün arasında bir değer bu sınırı değiştirmek için bkz: [depolama bekletmeyi değiştirme](#change-retention). | 
| En az yinelenme aralığı | 1 saniye | | 
| En fazla yinelenme aralığı | 500 gün | | 
|||| 

<a name="change-duration"></a>
<a name="change-retention"></a>

### <a name="change-run-duration-and-storage-retention"></a>Çalışma süresi ve depolama bekletmeyi değiştirme

Varsayılan sınır 7 gün ve 90 gün arasında değiştirmek için aşağıdaki adımları izleyin. Maksimum sınırı Git gerekiyorsa [Logic Apps ekibiyle](mailto://logicappsemail@microsoft.com) gereksinimlerinizi ile ilgili Yardım için.

1. Azure portalında mantıksal uygulama menüsünde seçin **iş akışı ayarları**. 

2. Altında **çalışma zamanı seçenekleri**, gelen **geçmişinin saklanacağı gün içinde çalıştırma** listesinde **özel**. 

3. Girin veya istediğiniz gün sayısı için kaydırıcıyı sürükleyin.

<a name="disable-delete"></a>

## <a name="disabling-or-deleting-logic-apps"></a>Logic apps silmeden veya devre dışı bırakma

Mantıksal uygulama devre dışı bıraktığınızda, hiçbir yeni çalıştırmaları örneği oluşturulur. Tüm süren ve bunlar, tamamlanması uzun sürebilir bitene kadar çalıştırmalar devam eder.

Mantıksal uygulamayı sildiğinizde yeni çalıştırma başlatılmaz. Devam eden ve bekleme durumunda olan tüm çalıştırmalar iptal edilir. Binlerce çalıştırma varsa iptal işleminin tamamlanması zaman alabilir.

<a name="looping-debatching-limits"></a>

## <a name="concurrency-looping-and-debatching-limits"></a>Eşzamanlılık, döngü ve limitleri

Bir tek bir mantıksal uygulama çalıştırması sınırları şunlardır:

| Ad | Sınır | Notlar | 
| ---- | ----- | ----- | 
| Tetikleyici eşzamanlılık | 50 | Varsayılan sınırı 20'dir. Aynı anda ya da paralel mantıksal uygulama örneği sayısı bu sınırı açıklar. <p><p>Varsayılan sınır 1 ile 50 arasında bir değer aralığında değiştirmek için bkz: [değişiklik tetikleyici eşzamanlılık](../logic-apps/logic-apps-workflow-actions-triggers.md#change-trigger-concurrency) veya [tetikleme örnekleri sırayla](../logic-apps/logic-apps-workflow-actions-triggers.md#sequential-trigger). | 
| En fazla bekleme çalıştırmalar | 100 | Varsayılan sınır 10'dur. Mantıksal uygulamanızı zaten en fazla eşzamanlı örneği çalışırken çalıştırmak için bekleyebileceği mantıksal uygulama örneği sayısı bu sınırı açıklar. <p><p>Varsayılan sınırı 0 ile 100 arasında bir değer aralığında değiştirmek için bkz: [değişiklik bekleme çalıştırmaları sınırlamak](../logic-apps/logic-apps-workflow-actions-triggers.md#change-waiting-runs). | 
| Foreach öğeleri | 100.000 | Bu sınır en fazla bir "for each" döngüsü işleyebilen dizi öğe sayısını açıklar. <p><p>Daha büyük dizileri filtrelemek için kullanabileceğiniz [sorgu eylemi](../connectors/connectors-native-query.md). | 
| Foreach yineleme | 50 | Varsayılan sınırı 20'dir. "For each" en fazla sayısı bu sınırı açıklar aynı anda ya da paralel yineleme döngüsü. <p><p>Varsayılan sınır 1 ile 50 arasında bir değer aralığında değiştirmek için bkz. ["for each" eşzamanlılık değiştirmek](../logic-apps/logic-apps-workflow-actions-triggers.md#change-for-each-concurrency) veya ["for each" çalıştırma sırayla döngü](../logic-apps/logic-apps-workflow-actions-triggers.md#sequential-for-each). | 
| SplitOn öğeleri | 100.000 | | 
| Yinelemelere kadar | 5.000 | | 
|||| 

<a name="throughput-limits"></a>

## <a name="throughput-limits"></a>İşleme sınırları

Bir tek bir mantıksal uygulama çalıştırması sınırları şunlardır:

| Ad | Sınır | Notlar | 
| ---- | ----- | ----- | 
| Eylem: Yürütme 5 dakika başına | 300,000 | Varsayılan sınır 100. 000 ' dir. Varsayılan sınırı değiştirmek için bkz ["yüksek aktarım hızı" modunda mantıksal uygulamanızı çalıştırın](../logic-apps/logic-apps-workflow-actions-triggers.md#run-high-throughput-mode), Önizleme aşamasında olduğu. Veya iş yükü, gerektiğinde birden fazla mantıksal uygulama arasında dağıtabilirsiniz. | 
| Eylem: Eş zamanlı giden çağrılar | ~2,500 | Eş zamanlı istek sayısını azaltın veya gerektiğinde süresini azaltın. | 
| Çalışma zamanı uç noktası: eş zamanlı gelen çağrılar | ~1,000 | Eş zamanlı istek sayısını azaltın veya gerektiğinde süresini azaltın. | 
| Çalışma zamanı uç noktası: çağrılar 5 dakika başına okuma  | 60,000 | İş yükü, gerektiğinde birden fazla uygulama arasında dağıtabilirsiniz. | 
| Çalışma zamanı uç noktası: 5 dakika başına çağrıları | 45,000 | İş yükü, gerektiğinde birden fazla uygulama arasında dağıtabilirsiniz. | 
| İçerik aktarım hızı 5 dakika başına | 600 MB | İş yükü, gerektiğinde birden fazla uygulama arasında dağıtabilirsiniz. | 
|||| 

Normal işleme bu sınırların üzerinde gidin veya bu sınırların üzerinde geçebilir, yük testi çalıştırmak için [Logic Apps ekibiyle](mailto://logicappsemail@microsoft.com) gereksinimlerinizi ile ilgili Yardım için.

<a name="request-limits"></a>

## <a name="http-request-limits"></a>HTTP istek sınırları

Tek HTTP isteği veya zaman uyumlu bağlayıcı çağrı sınırları şunlardır:

#### <a name="timeout"></a>Zaman Aşımı

Bazı bağlayıcı işlemler zaman uyumsuz çağrıları yapmak veya bu işlemler için zaman aşımı limitler uzun olabilir. Bu nedenle, Web kancası isteğini, dinleme. Daha fazla bilgi için özel bağlayıcı için teknik ayrıntıları bakın ve ayrıca [iş akışı tetikleyici ve Eylemler](../logic-apps/logic-apps-workflow-actions-triggers.md#http-action).

| Ad | Sınır | Notlar | 
| ---- | ----- | ----- | 
| Giden istek | 120 saniye | Uzun çalışan işlemleri için bir [yoklama zaman uyumsuz desen](../logic-apps/logic-apps-create-api-app.md#async-pattern) veya [until döngüsü](../logic-apps/logic-apps-workflow-actions-triggers.md#until-action). | 
| Zaman uyumlu yanıt | 120 saniye | İç içe geçmiş iş akışı olarak başka bir mantıksal uygulama çağırmadığınız sürece özgün istek yanıt almak tüm adımları yanıt sınırı içinde tamamlanmalıdır. Daha fazla bilgi için [çağrı, tetikleyici veya iç içe mantıksal uygulamalar](../logic-apps/logic-apps-http-endpoint.md). | 
|||| 

#### <a name="message-size"></a>İleti boyutu

| Ad | Sınır | Notlar | 
| ---- | ----- | ----- | 
| İleti boyutu | 100 MB | Bu sınırını çözmek için bkz: [Öbekleme ile büyük iletileri işleyen](../logic-apps/logic-apps-handle-large-messages.md). Ancak, bazı bağlayıcılar ve API Öbekleme desteklemez veya varsayılan sınır bile. | 
| Öbekleme ile ileti boyutu | 1 GB | Bu limit, yerel olarak destekleyen parçalama veya çalışma zamanı yapılandırmalarını Öbekleme etkinleştirmenize izin eylemler için geçerlidir. Daha fazla bilgi için [Öbekleme ile büyük iletileri işleyen](../logic-apps/logic-apps-handle-large-messages.md). | 
| İfade değerlendirme limiti | 131.072 karakter | `@concat()`, `@base64()`, `@string()` İfadeleri bu sınırdan daha uzun olamaz. | 
|||| 

#### <a name="retry-policy"></a>Yeniden deneme ilkesi

| Ad | Sınır | Notlar | 
| ---- | ----- | ----- | 
| Yeniden deneme sayısı | 90 | Varsayılan 4'tür. Varsayılan değiştirmek için kullanın [ilke parametresi yeniden](../logic-apps/logic-apps-workflow-actions-triggers.md). | 
| En uzun gecikme yeniden deneyin | 1 gün | Varsayılan değiştirmek için kullanın [ilke parametresi yeniden](../logic-apps/logic-apps-workflow-actions-triggers.md). | 
| Yeniden deneme gecikmesi Min | 5 saniye | Varsayılan değiştirmek için kullanın [ilke parametresi yeniden](../logic-apps/logic-apps-workflow-actions-triggers.md). |
|||| 

<a name="sftp"></a>

## <a name="sftp-and-sftp-ssh-limits"></a>SFTP ve SFTP-SSH sınırları

### <a name="file-size"></a>Dosya boyutu

| Ad | Sınır | Notlar |
|------|-------|-------|
| SFTP | 50 MB | Bu sınırı geçici olarak çözmek için kullanın [SFTP-SSH bağlayıcı](../connectors/connectors-sftp-ssh.md) veya [Öbekleme ile büyük iletileri işleyen](../logic-apps/logic-apps-handle-large-messages.md). Ancak, bazı bağlayıcılar ve API Öbekleme desteklemez veya varsayılan sınır bile. | 
| SFTP-SSH | 1 GB | Bu sınırını çözmek için bkz: [Öbekleme ile büyük iletileri işleyen](../logic-apps/logic-apps-handle-large-messages.md). Ancak, bazı bağlayıcılar ve API Öbekleme desteklemez veya varsayılan sınır bile. | 
|||| 

<a name="custom-connector-limits"></a>

## <a name="custom-connector-limits"></a>Özel bağlayıcı sınırlamaları

Web API'leri oluşturabileceğiniz özel bağlayıcılar için sınırlar aşağıda verilmiştir.

| Ad | Sınır | 
| ---- | ----- | 
| Özel bağlayıcılar sayısı | Azure aboneliği başına 1000 | 
| Özel bir bağlayıcı tarafından oluşturulan her bağlantı için dakika başına istek sayısı | Bağlantı başına 500 istek |
|||| 

<a name="managed-identity"></a>

## <a name="managed-identities"></a>Yönetilen kimlik

| Ad | Sınır | 
| ---- | ----- | 
| Azure abonelik kimlikleri sayısı sistem tarafından atanan mantıksal uygulamalarla yönetilen | 10 | 
|||

<a name="integration-account-limits"></a>

## <a name="integration-account-limits"></a>Tümleştirme hesabı sınırları

<a name="artifact-number-limits"></a>

### <a name="artifact-limits-per-integration-account"></a>Tümleştirme hesabı başına yapıt sınırlar

Her bir tümleştirme hesabı yapıtları sayısına yönelik sınırlar aşağıda verilmiştir. Daha fazla bilgi için [Logic Apps fiyatlandırma](https://azure.microsoft.com/pricing/details/logic-apps/). 

*Ücretsiz katmanı*

Ücretsiz katmanı, yalnızca keşif senaryolarda, üretim senaryolarında kullanın. Bu katman, aktarım hızı ve kullanımını kısıtlayan ve hiçbir hizmet düzeyi sözleşmesi (SLA) sahiptir.

| Yapıt | Sınır | Notlar | 
|----------|-------|-------| 
| EDI ticari iş ortakları | 25 | | 
| EDI ticari sözleşmeleri | 10 | | 
| Haritalar | 25 | | 
| Şemalar | 25 | 
| Derlemeler | 10 | | 
| Toplu iş yapılandırmaları | 5 | 
| Sertifikalar | 25 | | 
|||| 

*Temel katman*

| Yapıt | Sınır | Notlar | 
|----------|-------|-------| 
| EDI ticari iş ortakları | 2 | | 
| EDI ticari sözleşmeleri | 1 | | 
| Haritalar | 500 | | 
| Şemalar | 500 | 
| Derlemeler | 25 | | 
| Toplu iş yapılandırmaları | 1 | | 
| Sertifikalar | 2 | | 
|||| 

*Standart katman*

| Yapıt | Sınır | Notlar | 
|----------|-------|-------| 
| EDI ticari iş ortakları | 500 | | 
| EDI ticari sözleşmeleri | 500 | | 
| Haritalar | 500 | | 
| Şemalar | 500 | 
| Derlemeler | 50 | | 
| Toplu iş yapılandırmaları | 5 |  
| Sertifikalar | 50 | | 
|||| 

<a name="artifact-capacity-limits"></a>

### <a name="artifact-capacity-limits"></a>Yapıt kapasite sınırları

| Ad | Sınır | Notlar | 
| ---- | ----- | ----- | 
| Şema | 8 MB | 2 MB'tan büyük dosyaları yüklemek için kullandığınız [URI blob](../logic-apps/logic-apps-enterprise-integration-schemas.md). | 
| Harita (XSLT dosyası) | 2 MB | | 
| Çalışma zamanı uç noktası: çağrılar 5 dakika başına okuma | 60,000 | İş yükü, gerektiğinde birden fazla hesap genelinde dağıtabilirsiniz. | 
| Çalışma zamanı uç noktası: 5 dakika başına çağrıları | 45,000 | İş yükü, gerektiğinde birden fazla hesap genelinde dağıtabilirsiniz. | 
| Çalışma zamanı uç noktası: 5 dakika başına çağrıları izleme | 45,000 | İş yükü, gerektiğinde birden fazla hesap genelinde dağıtabilirsiniz. | 
| Çalışma zamanı uç noktası: eş zamanlı çağrılar engelleme | ~1,000 | Eş zamanlı istek sayısını azaltın veya gerektiğinde süresini azaltın. | 
||||  

<a name="b2b-protocol-limits"></a>

### <a name="b2b-protocol-as2-x12-edifact-message-size"></a>B2B Protokolü (AS2, EDIFACT olan X12) ileti boyutu

B2B protokolleri için uygulanan limitler şunlardır:

| Ad | Sınır | Notlar | 
| ---- | ----- | ----- | 
| AS2 | 50 MB | Kod çözme ve kodlama için geçerlidir | 
| X12 | 50 MB | Kod çözme ve kodlama için geçerlidir | 
| EDIFACT | 50 MB | Kod çözme ve kodlama için geçerlidir | 
|||| 

<a name="configuration"></a>

## <a name="configuration-ip-addresses"></a>Yapılandırması: IP adresleri

### <a name="azure-logic-apps-service"></a>Azure Logic Apps hizmetinin

Bir bölgede tüm logic apps, aynı IP adresi aralıklarını kullanın. Logic apps ile doğrudan olun çağrıları desteklemek için [HTTP](../connectors/connectors-native-http.md), [da HTTP + Swagger](../connectors/connectors-native-http-swagger.md)ve içerirler göre bu giden ve gelen IP adresleri, bu nedenle, güvenlik duvarı yapılandırmalarını ayarlamak diğer HTTP istekleri logic apps mevcut şirket burada:

| Logic Apps bölgesi | Giden IP |
|-------------------|-------------|
| Avustralya Doğu | 13.75.149.4, 52.187.226.96, 52.187.226.139, 52.187.227.245, 52.187.229.130, 52.187.231.184, 104.210.90.241, 104.210.91.55 |
| Avustralya Güneydoğu | 13.70.159.205, 13.73.114.207, 13.77.3.139, 13.77.56.167, 13.77.58.136, 52.189.214.42, 52.189.220.75, 52.189.222.77 |
| Güney Brezilya | 191.234.161.28, 191.234.161.168, 191.234.162.131, 191.234.162.178, 191.234.182.26, 191.235.82.221, 191.235.91.7, 191.237.255.116 |
| Orta Kanada | 13.71.184.150, 13.71.186.1, 40.85.250.135, 40.85.250.212, 40.85.252.47, 52.233.29.92, 52.228.39.241, 52.228.39.244 |
| Doğu Kanada | 40.86.203.228, 40.86.216.241, 40.86.217.241, 40.86.226.149, 40.86.228.93, 52.229.120.45, 52.229.126.25, 52.232.128.155 |
| Orta Hindistan | 52.172.154.168, 52.172.185.79, 52.172.186.159, 104.211.74.145, 104.211.90.162, 104.211.90.169, 104.211.101.108, 104.211.102.62 |
| Orta ABD | 13.67.236.125, 23.100.82.16, 23.100.86.139, 23.100.87.24, 23.100.87.56, 40.113.218.230, 40.122.170.198, 104.208.25.27 |
| Doğu Asya | 13.75.94.173, 40.83.73.39, 40.83.75.165, 40.83.77.208, 40.83.100.69, 40.83.127.19, 52.175.33.254, 65.52.175.34 |
| Doğu ABD | 13.92.98.111, 23.100.29.190, 23.101.132.208, 23.101.136.201, 23.101.139.153, 40.114.82.191, 40.121.91.41, 104.45.153.81 |
| Doğu ABD 2 | 40.70.26.154, 40.70.27.236, 40.70.29.214, 40.70.131.151, 40.84.30.147, 104.208.140.40, 104.208.155.200, 104.208.158.174 |
| Japonya Doğu | 13.71.158.3, 13.71.158.120, 13.73.4.207, 13.78.18.168, 13.78.20.232, 13.78.21.155, 13.78.35.229, 13.78.42.223 |
| Japonya Batı | 40.74.64.207, 40.74.68.85, 40.74.74.21, 40.74.76.213, 40.74.77.205, 40.74.140.4, 104.214.137.243, 138.91.26.45 |
| Orta Kuzey ABD | 52.162.208.216, 52.162.213.231, 65.52.8.225, 65.52.9.96, 65.52.10.183, 157.55.210.61, 157.55.212.238, 168.62.248.37 |
| Kuzey Avrupa | 40.112.92.104, 40.112.95.216, 40.113.1.181, 40.113.3.202, 40.113.4.18, 40.113.12.95, 52.178.165.215, 52.178.166.21 |
| Orta Güney ABD | 13.65.82.17, 13.66.52.232, 23.100.124.84, 23.100.127.172, 23.101.183.225, 70.37.54.122, 70.37.50.6, 104.210.144.48 |
| Güney Hindistan | 52.172.50.24, 52.172.52.0, 52.172.55.231, 104.211.227.229, 104.211.229.115, 104.211.230.126, 104.211.230.129, 104.211.231.39 |
| Güneydoğu Asya | 13.67.91.135, 13.67.107.128, 13.67.110.109, 13.76.4.194, 13.76.5.96, 13.76.133.155, 52.163.228.93, 52.163.230.166 |
| Batı Orta ABD | 13.78.129.20, 13.78.137.179, 13.78.141.75, 13.78.148.140, 13.78.151.161, 52.161.18.218, 52.161.9.108, 52.161.27.190 |
| Batı Avrupa | 13.95.147.65, 23.97.210.126, 23.97.211.179, 23.97.218.130, 40.68.209.23, 40.68.222.65, 51.144.182.201, 104.45.9.52 |
| Batı Hindistan | 104.211.154.7, 104.211.154.59, 104.211.156.153, 104.211.158.123, 104.211.158.127, 104.211.162.205, 104.211.164.80, 104.211.164.136 |
| Batı ABD | 40.83.164.80, 40.118.244.241, 40.118.241.243, 52.160.92.112, 104.42.38.32, 104.42.49.145, 157.56.162.53, 157.56.167.147 |
| Batı ABD 2 | 13.66.201.169, 13.66.210.167, 13.66.246.219, 13.77.149.159, 52.175.198.132, 52.183.29.132, 52.183.30.169 |
| Birleşik Krallık Güney | 51.140.28.225, 51.140.73.85, 51.140.74.14, 51.140.78.44, 51.140.137.190, 51.140.142.28, 51.140.153.135, 51.140.158.24 |
| Birleşik Krallık Batı | 51.141.45.238, 51.141.47.136, 51.141.54.185, 51.141.112.112, 51.141.113.36, 51.141.114.77, 51.141.118.119, 51.141.119.63 |
| | |

| Logic Apps bölgesi | Gelen IP |
|-------------------|------------|
| Avustralya Doğu | 13.75.153.66, 52.187.231.161, 104.210.89.222, 104.210.89.244 |
| Avustralya Güneydoğu | 13.73.115.153, 40.115.78.70, 40.115.78.237, 52.189.216.28 |
| Güney Brezilya | 191.234.166.198, 191.235.86.199, 191.235.94.220, 191.235.95.229 |
| Orta Kanada | 13.88.249.209, 40.85.241.105, 52.233.29.79, 52.233.30.218 |
| Doğu Kanada | 40.86.202.42, 52.229.125.57, 52.232.129.143, 52.232.133.109 |
| Orta Hindistan | 52.172.157.194, 52.172.184.192, 52.172.191.194, 104.211.73.195 |
| Orta ABD | 13.67.236.76, 40.77.31.87, 40.77.111.254, 104.43.243.39 |
| Doğu Asya | 13.75.89.159, 23.97.68.172, 40.83.98.194, 168.63.200.173 |
| Doğu ABD | 40.117.99.79, 40.117.100.228, 137.116.126.165, 137.135.106.54 |
| Doğu ABD 2 | 40.70.27.253, 40.79.44.7, 40.84.25.234, 40.84.59.136 |
| Japonya Doğu | 13.71.146.140, 13.78.43.164, 13.78.62.130, 13.78.84.187 |
| Japonya Batı | 40.74.68.85, 40.74.81.13, 40.74.85.215, 40.74.140.173 |
| Orta Kuzey ABD | 65.52.9.64, 65.52.211.164, 168.62.249.81, 157.56.12.202 |
| Kuzey Avrupa | 13.79.173.49, 40.112.90.39, 52.169.218.253, 52.169.220.174 |
| Orta Güney ABD | 13.65.98.39, 13.84.41.46, 13.84.43.45, 40.84.138.132 |
| Güney Hindistan | 52.172.9.47, 52.172.49.43, 52.172.51.140, 104.211.225.152 |
| Güneydoğu Asya | 52.163.93.214, 52.187.65.81, 52.187.65.155, 104.215.181.6 |
| Batı Orta ABD | 13.78.137.247, 52.161.8.128, 52.161.19.82, 52.161.26.172 |
| Batı Avrupa | 13.95.155.53, 52.174.49.6, 52.174.49.6, 52.174.54.218 |
| Batı Hindistan | 104.211.157.237, 104.211.164.25, 104.211.164.112, 104.211.165.81 |
| Batı ABD | 13.91.252.184, 52.160.90.237, 138.91.188.137, 157.56.160.212 |
| Batı ABD 2 | 13.66.128.68, 13.66.224.169, 52.183.30.10, 52.183.39.67 |
| Birleşik Krallık Güney | 51.140.78.71, 51.140.79.109, 51.140.84.39, 51.140.155.81 |
| Birleşik Krallık Batı | 51.141.48.98, 51.141.51.145, 51.141.53.164, 51.141.119.150 |
| | |

### <a name="connectors"></a>Bağlayıcılar

Çağrıları desteklemek için [Bağlayıcılar](../connectors/apis-list.md) içerirler bu giden IP adresleri için güvenlik duvarı yapılandırmalarınızı ayarlama yapma, logic apps bulunduğu bölgelerine bağlı.

> [!IMPORTANT]
> Var olan yapılandırmaları varsa, lütfen güncelleştirmeniz **olabildiğince çabuk 1 Eylül 2018'den önce** içerir ve logic apps bulunduğu bölge için bu listedeki IP adresleriyle eşleşen. 
> 
> Logic Apps, doğrudan güvenlik duvarları üzerinden Azure depolama hesaplarınızı bağlama desteklemiyor. Bu depolama hesaplarından erişmeye, burada iki seçenekten birini kullanın: 
>
> * Oluşturma bir [tümleştirme hizmeti ortamı](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md), hangi Azure sanal ağdaki kaynaklara bağlanabilir. 
> 
> * API Management'ı zaten kullanıyorsanız, bu senaryo için bu hizmeti kullanabilirsiniz. Daha fazla bilgi için bkz. [basit Kurumsal tümleştirme mimarisi](http://aka.ms/aisarch).

| Logic Apps bölgesi | Giden IP | 
|-------------------|-------------|  
| Avustralya Doğu | 13.70.72.192 - 13.70.72.207, 13.72.243.10 40.126.251.213 | 
| Avustralya Güneydoğu | 13.70.136.174, 13.77.50.240 - 13.77.50.255, 40.127.80.34 | 
| Güney Brezilya | 104.41.59.51, 191.232.38.129 191.233.203.192 - 191.233.203.207 | 
| Orta Kanada | -13.71.170.223, 13.71.170.224 - 13.71.170.208 13.71.170.239, 52.228.33.76, 52.228.34.13, 52.228.42.205, 52.233.26.83, 52.233.31.197, 52.237.24.126 | 
| Doğu Kanada | 40.69.106.240 - 40.69.106.255, 52.229.120.52, 52.229.120.131, 52.229.120.178, 52.229.123.98, 52.229.126.202, 52.242.35.152 | 
| Orta Hindistan | 52.172.211.12, 104.211.81.192 - 104.211.81.207, 104.211.98.164 | 
| Orta ABD | 13.89.171.80 - 13.89.171.95, 40.122.49.51 52.173.245.164 | 
| Doğu Asya | 13.75.36.64 - 13.75.36.79, 23.99.116.181 52.175.23.169 | 
| Doğu ABD | 40.71.11.80 - 40.71.11.95, 40.71.249.205 191.237.41.52 | 
| Doğu ABD 2 | 40.70.146.208 - 40.70.146.223, 52.232.188.154 104.208.233.100 | 
| Japonya Doğu | 13.71.153.19, 13.78.108.0 - 13.78.108.15, 40.115.186.96 | 
| Japonya Batı | 40.74.100.224 - 40.74.100.239, 40.74.130.77 104.215.61.248 | 
| Orta Kuzey ABD | 52.162.107.160 - 52.162.107.175, 52.162.242.161 65.52.218.230 | 
| Kuzey Avrupa | 13.69.227.208 - 13.69.227.223, 52.178.150.68 104.45.93.9 | 
| Orta Güney ABD | 13.65.86.57, 104.214.19.48 - 104.214.19.63, 104.214.70.191 | 
| Güney Hindistan | 13.71.125.22, 40.78.194.240 - 40.78.194.255, 104.211.227.225 | 
| Güneydoğu Asya | 13.67.8.240 - 13.67.8.255, 13.76.231.68 52.187.68.19 | 
| Batı Orta ABD | 13.71.195.32 - 13.71.195.47, 52.161.24.128, 52.161.26.212, 52.161.27.108, 52.161.29.35, 52.161.30.5, 52.161.102.22 | 
| Batı Avrupa | 13.69.64.208 - 13.69.64.223, 40.115.50.13 52.174.88.118 | 
| Batı Hindistan | 104.211.146.224 - 104.211.146.239, 104.211.161.203 104.211.189.218 | 
| Batı ABD | 40.112.243.160 - 40.112.243.175, 104.40.51.248 104.42.122.49 | 
| Batı ABD 2 | 13.66.140.128 - 13.66.140.143, 13.66.218.78, 13.66.219.14, 13.66.220.135, 13.66.221.19, 13.66.225.219, 52.183.78.157 | 
| Birleşik Krallık Güney | 51.140.80.51, 51.140.148.0 - 51.140.148.15 | 
| Birleşik Krallık Batı | 51.140.211.0 - 51.140.211.15, 51.141.47.105 | 
| | | 

## <a name="next-steps"></a>Sonraki adımlar  

* Bilgi edinmek için nasıl [ilk mantıksal uygulamanızı oluşturun](../logic-apps/quickstart-create-first-logic-app-workflow.md)  
* Hakkında bilgi edinin [yayın örnekleri ve senaryoları](../logic-apps/logic-apps-examples-and-scenarios.md)
