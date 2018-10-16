---
title: Azure Application Gateway web uygulaması güvenlik duvarı CRS kural gruplarının ve kuralların
description: Bu sayfa web uygulaması güvenlik duvarı CRS kural gruplarının ve kuralları hakkında bilgi sağlar.
documentationcenter: na
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: ''
ms.workload: infrastructure-services
ms.date: 4/16/2018
ms.author: victorh
ms.openlocfilehash: 15a86410e8ca853c2ca2431cb9a62de628972703
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2018
ms.locfileid: "49320110"
---
# <a name="list-of-web-application-firewall-crs-rule-groups-and-rules-offered"></a>Sunulan web uygulaması güvenlik duvarı CRS kural gruplarının ve kuralların listesi

Application Gateway web uygulaması Güvenlik Duvarı (WAF), web uygulamalarını yaygın güvenlik açıklarına ve açıklardan yararlanmaya karşı korur. Bu, OWASP çekirdek kural kümeleri üzerinde 2.2.9 veya 3.0 tanımlanan kurallara dayalı aracılığıyla gerçekleştirilir. Bu kurallar kural kural olarak devre dışı bırakılabilir. Bu makale, sunulan rulesets ve geçerli kurallarını içerir.

Aşağıdaki tablolarda kural gruplarının ve Application Gateway web uygulaması güvenlik duvarı ile kullanırken mevcut olan kuralları var.  Her tabloda belirli bir CRS sürümü için bir kural grubu bulunan kuralları gösterir.

## <a name="owasp30"></a> OWASP_3.0


### <a name="crs911"></a> <p x-ms-format-detection="none">İSTEK 911 YÖNTEMİ ZORLAMA</p>

|RuleId|Açıklama|
|---|---|
|911011|Kural 911011|
|911012|Kural 911012|
|911100|Yöntemine ilke tarafından izin verilmiyor|
|911013|Kural 911013|
|911014|Kural 911014|
|911015|Kural 911015|
|911016|Kural 911016|
|911017|Kural 911017|
|911018|Kural 911018|


### <a name="crs913"></a> <p x-ms-format-detection="none">İSTEK 913 TARAYICI ALGILAMA</p>

|RuleId|Açıklama|
|---|---|
|913011|Kural 913011|
|913012|Kural 913012|
|913100|Güvenlik tarayıcısı ile ilişkili kullanıcı aracısı bulundu|
|913110|Güvenlik tarayıcısı ile ilişkili istek üst bilgisi bulunamadı|
|913120|İstek dosya adı/güvenlik tarayıcısı ile ilişkili bağımsız değişken bulunamadı|
|913013|Kural 913013|
|913014|Kural 913014|
|913101|Betik/genel HTTP istemcisi ile ilişkili kullanıcı aracısı bulundu|
|913102|Web Gezgin/bot ile ilişkili kullanıcı aracısı bulundu|
|913015|Kural 913015|
|913016|Kural 913016|
|913017|Kural 913017|
|913018|Kural 913018|

### <a name="crs920"></a> <p x-ms-format-detection="none">İSTEK 920 PROTOKOLÜ ZORLAMA</p>

|RuleId|Açıklama|
|---|---|
|920011|Kural 920011|
|920012|Kural 920012|
|920100|HTTP isteği geçersiz satır|
|920130|İstek gövdesi ayrıştırılamadı.|
|920140|Çok parçalı istek gövdesi, katı bir doğrulama başarısız oldu PE %@{REQBODY_PROCESSOR_ERROR =} BQ %@{MULTIPART_BOUNDARY_QUOTED} BW %@{MULTIPART_BOUNDARY_WHITESPACE} DB %@{MULTIPART_DATA_BEFORE} DA %@{MULTIPART_DATA_AFTER} HF %@{MULTIPART_HEADER_FOLDING} LF % @ {MULTIPART_LF_LINE}     FLE %@{MULTIPART_FILE_LIMIT_EXCEEDED MÜŞTERİYE %@{MULTIPART_INVALID_HEADER_FOLDING IQ %@{MULTIPART_INVALID_QUOTING SM %@{MULTIPART_SEMICOLON_MISSING}}}}|
|920160|Content-Length HTTP üstbilgisi sayısal değil.|
|920170|GET veya HEAD ile gövde içeriği isteyin.|
|920180|POST isteği Content-Length üst bilgisi eksik.|
|920190|Aralık = son bayt değeri geçersiz.|
|920210|Birden çok ve çakışan bağlantı üstbilgi verileri bulunamadı.|
|920220|URL kodlaması kötüye saldırı denemesi|
|920240|URL kodlaması kötüye saldırı denemesi|
|920250|UTF8 Uygunsuz kullanım saldırı girişimini kodlaması|
|920260|Unicode tam/yarım genişlik kötüye saldırı denemesi|
|920270|Geçersiz karakter isteğinde (boş karakter için)|
|920280|İstek ana bilgisayar üst bilgisi eksik|
|920290|Boş bir ana bilgisayar üstbilgisi|
|920310|İstek boş olan üst bilgisi kabul edin|
|920311|İstek boş olan üst bilgisi kabul edin|
|920330|Boş Kullanıcı Aracısı üstbilgisi|
|920340|İçeren içerik ancak eksik Content-Type üst bilgi isteği|
|920350|Ana bilgisayar üst bilgisi bir sayısal IP adresidir|
|920380|İstek çok fazla bağımsız değişken|
|920360|Bağımsız değişken adı çok uzun|
|920370|Bağımsız değişken değeri çok uzun|
|920390|Toplam bağımsız değişkenleri boyutu aşıldı|
|920400|Karşıya yüklenen dosya boyutu çok büyük|
|920410|Karşıya yüklenen toplam dosya boyutu çok büyük|
|920420|İstek içerik türü ilke tarafından izin verilmiyor|
|920430|HTTP protokolü sürümünü ilke tarafından izin verilmiyor|
|920440|URL uzantısına İlkesi tarafından kısıtlanıyor|
|920450|HTTP üst bilgisi (%@{MATCHED_VAR}) ilkesi tarafından kısıtlanıyor|
|920013|Kural 920013|
|920014|Kural 920014|
|920200|Aralık = çok fazla alan (6 ya da daha fazla)|
|920201|Aralık = çok fazla alan için pdf isteği (35 veya daha fazla)|
|920230|Birden çok URL kodlaması algılandı|
|920300|İstek eksik bir üst bilgisi kabul edin|
|920271|Geçersiz karakter isteğinde (olmayan yazdırılabilir karakter)|
|920320|Kullanıcı Aracısı üst bilgisi eksik|
|920015|Kural 920015|
|920016|Kural 920016|
|920272|(Dışında yazdırılabilir karakter ASCII 127 aşağıda) isteğinde geçersiz karakter|
|920017|Kural 920017|
|920018|Kural 920018|
|920202|Aralık = çok fazla alan için pdf isteği (6 ya da daha fazla)|
|920273|İstek (dışında çok sıkı kümesi) içinde geçersiz karakter|
|920274|İstek üstbilgilerini (dışında çok sıkı kümesi) içinde geçersiz karakter|
|920460|Kural 920460|

### <a name="crs921"></a> <p x-ms-format-detection="none">İSTEK 921 PROTOKOLÜ SALDIRI</p>

|RuleId|Açıklama|
|---|---|
|921011|Kural 921011|
|921012|Kural 921012|
|921100|HTTP isteği kaçakçılığı saldırı.|
|921110|HTTP isteği kaçakçılığı saldırı|
|921120|HTTP yanıtı bölme saldırı|
|921130|HTTP yanıtı bölme saldırı|
|921140|HTTP üstbilgisi ekleme saldırısına aracılığıyla üst bilgileri|
|921150|HTTP üstbilgisi ekleme saldırısına Yükü (CR/LF algılandı) aracılığıyla|
|921160|HTTP üstbilgisi ekleme saldırısına Yükü (CR/LF ve üst bilgi adı algılandı) aracılığıyla|
|921013|Kural 921013|
|921014|Kural 921014|
|921151|HTTP üstbilgisi ekleme saldırısına Yükü (CR/LF algılandı) aracılığıyla|
|921015|Kural 921015|
|921016|Kural 921016|
|921170|Kural 921170|
|921180|HTTP parametresi kirlilik (% @{TX.1})|
|921017|Kural 921017|
|921018|Kural 921018|

### <a name="crs930"></a> <p x-ms-format-detection="none">İSTEK-930-UYGULAMASI-SALDIRI-LFI</p>

|RuleId|Açıklama|
|---|---|
|930011|Kural 930011|
|930012|Kural 930012|
|930100|Yol çapraz geçiş saldırısı (/.. /)|
|930110|Yol çapraz geçiş saldırısı (/.. /)|
|930120|İşletim sistemi dosya erişim girişimi|
|930130|Kısıtlanmış dosya erişim denemesi|
|930013|Kural 930013|
|930014|Kural 930014|
|930015|Kural 930015|
|930016|Kural 930016|
|930017|Kural 930017|
|930018|Kural 930018|

### <a name="crs931"></a> <p x-ms-format-detection="none">İSTEK-931-UYGULAMASI-SALDIRI-RFI</p>

|RuleId|Açıklama|
|---|---|
|931011|Kural 931011|
|931012|Kural 931012|
|931100|Uzaktan dosya eklemeye (RFI) olası bir saldırı IP adresini kullanarak URL parametresi =|
|931110|Uzaktan dosya eklemeye (RFI) olası bir saldırı ortak RFI savunmasız kullanılan parametre w/URL yükü =|
|931120|Olası bir uzak dosya ekleme (RFI) Saldırıcı URL kullanılan yükü w/sondaki soru işareti karakteri (?) =|
|931013|Kural 931013|
|931014|Kural 931014|
|931130|Olası bir uzak dosya ekleme (RFI) Saldırıcı etki başvuru/Link|
|931015|Kural 931015|
|931016|Kural 931016|
|931017|Kural 931017|
|931018|Kural 931018|

### <a name="crs932"></a> <p x-ms-format-detection="none">İSTEK-932-UYGULAMASI-SALDIRI-NAK</p>

|RuleId|Açıklama|
|---|---|
|932011|Kural 932011|
|932012|Kural 932012|
|932120|Uzaktan komut yürütme Windows PowerShell komutunu bulundu =|
|932130|Uzaktan komut yürütme UNIX Kabuk ifadesi bulundu =|
|932140|Uzaktan komut yürütme için / komutu BULUNAMAZSA Windows =|
|932160|Uzaktan komut yürütme = UNIX Kabuk kodu bulundu|
|932170|Uzaktan komut yürütme Shellshock (CVE-2014-6271) =|
|932171|Uzaktan komut yürütme Shellshock (CVE-2014-6271) =|
|932013|Kural 932013|
|932014|Kural 932014|
|932015|Kural 932015|
|932016|Kural 932016|
|932017|Kural 932017|
|932018|Kural 932018|

### <a name="crs933"></a> <p x-ms-format-detection="none">İSTEK-933-UYGULAMASI-SALDIRI-PHP</p>

|RuleId|Açıklama|
|---|---|
|933011|Kural 933011|
|933012|Kural 933012|
|933100|PHP ekleme saldırısına açma/kapatma etiketi bulundu =|
|933110|PHP ekleme saldırısına PHP betik dosyasını karşıya yükleme bulundu =|
|933120|PHP ekleme saldırısına yapılandırma yönergesi bulundu =|
|933130|PHP ekleme saldırısına değişkenleri bulundu =|
|933150|PHP ekleme saldırısına = yüksek riskli bir PHP işlevi adı bulunamadı|
|933160|PHP ekleme saldırısına yüksek riskli PHP işlev çağrısı bulundu =|
|933180|PHP ekleme saldırısına değişken işlev çağrısı bulundu =|
|933013|Kural 933013|
|933014|Kural 933014|
|933151|PHP ekleme saldırısına = orta riskli PHP işlev adı bulunamadı|
|933015|Kural 933015|
|933016|Kural 933016|
|933131|PHP ekleme saldırısına değişkenleri bulundu =|
|933161|PHP ekleme saldırısına değeri düşük PHP işlev çağrısı bulundu =|
|933111|PHP ekleme saldırısına PHP betik dosyasını karşıya yükleme bulundu =|
|933017|Kural 933017|
|933018|Kural 933018|

### <a name="crs941"></a> <p x-ms-format-detection="none">İSTEK-941-UYGULAMASI-SALDIRI-XSS</p>

|RuleId|Açıklama|
|---|---|
|941011|Kural 941011|
|941012|Kural 941012|
|941100|XSS saldırı algılandığında libinjection aracılığıyla|
|941110|XSS filtre - kategori 1 = betiği etiketi vektör|
|941130|XSS filtre - 3 Kategori özniteliği vektör =|
|941140|XSS filtre - kategori 4 Javascript URI vektör =|
|941150|XSS filtre - kategori 5 izin verilmeyen HTML özniteliklerini =|
|941180|Düğüm Doğrulayıcı kara liste anahtar sözcükleri|
|941190|IE XSS filtreleri - saldırısı algılandı.|
|941200|IE XSS filtreleri - saldırısı algılandı.|
|941210|IE XSS filtreleri - saldırısı algılandı.|
|941220|IE XSS filtreleri - saldırısı algılandı.|
|941230|IE XSS filtreleri - saldırısı algılandı.|
|941240|IE XSS filtreleri - saldırısı algılandı.|
|941260|IE XSS filtreleri - saldırısı algılandı.|
|941270|IE XSS filtreleri - saldırısı algılandı.|
|941280|IE XSS filtreleri - saldırısı algılandı.|
|941290|IE XSS filtreleri - saldırısı algılandı.|
|941300|IE XSS filtreleri - saldırısı algılandı.|
|941310|US-ASCII hatalı kodlama XSS filtre - saldırısı algılandı.|
|941350|UTF-7 kodlama IE XSS - saldırısı algılandı.|
|941013|Kural 941013|
|941014|Kural 941014|
|941320|Olası XSS saldırısı algılandı - HTML etiketi işleyicisi|
|941015|Kural 941015|
|941016|Kural 941016|
|941017|Kural 941017|
|941018|Kural 941018|

### <a name="crs942"></a> <p x-ms-format-detection="none">İSTEK-942-UYGULAMASI-SALDIRI-SQLI</p>

|RuleId|Açıklama|
|---|---|
|942011|Kural 942011|
|942012|Kural 942012|
|942100|SQL ekleme saldırısı algılandı libinjection aracılığıyla|
|942140|SQL ekleme saldırısına ortak DB adları algılandı =|
|942160|Sleep() veya benchmark() kullanarak görme sqli testlerini algılar.|
|942170|Koşullu sorguları da dahil olmak üzere SQL Kıyaslama ve uyku ekleme denemesi algılar|
|942230|Koşullu SQL ekleme girişimlerini algılar|
|942270|Temel sql ekleme aranıyor. Mysql oracle ve diğerleri için ortak saldırı dizesi.|
|942290|Temel MongoDB SQL ekleme girişimlerini bulur|
|942320|MySQL ve PostgreSQL algılar depolanan yordam/işlev ekleme|
|942350|MySQL UDF ekleme ve diğer veri/yapısı düzenleme algılar çalışır|
|942013|Kural 942013|
|942014|Kural 942014|
|942150|SQL ekleme saldırısına|
|942410|SQL ekleme saldırısına|
|942440|SQL açıklama dizisi algılandı.|
|942450|Belirtilen SQL onaltılık kodlama|
|942015|Kural 942015|
|942016|Kural 942016|
|942251|HAVING eklemelerini algılar|
|942460|Meta karakter Anomali algılama uyarısı - yinelenen sözcük olmayan karakter|
|942017|Kural 942017|
|942018|Kural 942018|

### <a name="crs943"></a> <p x-ms-format-detection="none">REQUEST-943-APPLICATION-ATTACK-SESSION-FIXATION</p>

|RuleId|Açıklama|
|---|---|
|943011|Kural 943011|
|943012|Kural 943012|
|943100|Olası oturum sabitleme saldırı = tanımlama bilgisi değerleri HTML olarak ayarlama|
|943110|Olası oturum sabitleme saldırı SessionID parametre adıyla etki alanı kapalı başvuran =|
|943120|Olası oturum sabitleme saldırı SessionID parametre adıyla hiçbir başvuran =|
|943013|Kural 943013|
|943014|Kural 943014|
|943015|Kural 943015|
|943016|Kural 943016|
|943017|Kural 943017|
|943018|Kural 943018|

## <a name="owasp229"></a> OWASP_2.2.9

### <a name="crs20"></a> crs_20_protocol_violations

|RuleId|Açıklama|
|---|---|
|960911|HTTP isteği geçersiz satır|
|981227|Apache hata = isteğinde geçersiz bir URI.|
|960912|İstek gövdesi ayrıştırılamadı.|
|960914|Çok parçalı istek gövdesi, katı bir doğrulama başarısız oldu PE %@{REQBODY_PROCESSOR_ERROR =} BQ %@{MULTIPART_BOUNDARY_QUOTED} BW %@{MULTIPART_BOUNDARY_WHITESPACE} DB %@{MULTIPART_DATA_BEFORE} DA %@{MULTIPART_DATA_AFTER} HF %@{MULTIPART_HEADER_FOLDING} LF % @ {MULTIPART_LF_LINE}     FLE %@{MULTIPART_FILE_LIMIT_EXCEEDED MÜŞTERİYE %@{MULTIPART_INVALID_HEADER_FOLDING IQ %@{MULTIPART_INVALID_QUOTING SM %@{MULTIPART_SEMICOLON_MISSING}}}}|
|960915|Çok bölümlü ayrıştırıcı olası eşleşmeyen bir sınır algılandı.|
|960016|Content-Length HTTP üstbilgisi sayısal değil.|
|960011|GET veya HEAD ile gövde içeriği isteyin.|
|960012|POST isteği Content-Length üst bilgisi eksik.|
|960902|Kimlik kodlama geçersiz kullanımı.|
|960022|HTTP 1.0 için izin verilmiyor üstbilgi bekler.|
|960020|Pragma üst bilgisi Cache-Control üst bilgisi, HTTP/1.1 istekleri için gerektirir.|
|958291|Aralık = alan varsa ve 0 ile başlar.|
|958230|Aralık = son bayt değeri geçersiz.|
|958295|Birden çok ve çakışan bağlantı üstbilgi verileri bulunamadı.|
|950107|URL kodlaması kötüye saldırı denemesi|
|950109|Birden çok URL kodlaması algılandı|
|950108|URL kodlaması kötüye saldırı denemesi|
|950801|UTF8 Uygunsuz kullanım saldırı girişimini kodlaması|
|950116|Unicode tam/yarım genişlik kötüye saldırı denemesi|
|960901|İstek içinde geçersiz karakter|
|960018|İstek içinde geçersiz karakter|

### <a name="crs21"></a> crs_21_protocol_anomalies

|RuleId|Açıklama|
|---|---|
|960008|İstek ana bilgisayar üst bilgisi eksik|
|960007|Boş bir ana bilgisayar üstbilgisi|
|960015|İstek eksik bir üst bilgisi kabul edin|
|960021|İstek boş olan üst bilgisi kabul edin|
|960009|İstek bir kullanıcı aracısı üst bilgisi eksik|
|960006|Boş Kullanıcı Aracısı üstbilgisi|
|960904|İçeren içerik ancak eksik Content-Type üst bilgi isteği|
|960017|Ana bilgisayar üst bilgisi bir sayısal IP adresidir|

### <a name="crs23"></a> crs_23_request_limits

|RuleId|Açıklama|
|---|---|
|960209|Bağımsız değişken adı çok uzun|
|960208|Bağımsız değişken değeri çok uzun|
|960335|İstek çok fazla bağımsız değişken|
|960341|Toplam bağımsız değişkenleri boyutu aşıldı|
|960342|Karşıya yüklenen dosya boyutu çok büyük|
|960343|Karşıya yüklenen toplam dosya boyutu çok büyük|

### <a name="crs30"></a> crs_30_http_policy

|RuleId|Açıklama|
|---|---|
|960032|Yöntemine ilke tarafından izin verilmiyor|
|960010|İstek içerik türü ilke tarafından izin verilmiyor|
|960034|HTTP protokolü sürümünü ilke tarafından izin verilmiyor|
|960035|URL uzantısına İlkesi tarafından kısıtlanıyor|
|960038|HTTP üst bilgisi ilkesi tarafından kısıtlanıyor|

### <a name="crs35"></a> crs_35_bad_robots

|RuleId|Açıklama|
|---|---|
|990002|Site bir güvenlik tarayıcısı taranan isteğini belirtir.|
|990901|Site bir güvenlik tarayıcısı taranan isteğini belirtir.|
|990902|Site bir güvenlik tarayıcısı taranan isteğini belirtir.|
|990012|Sahte web sitesi Gezgini|

### <a name="crs40"></a> crs_40_generic_attacks

|RuleId|Açıklama|
|---|---|
|960024|Meta karakter Anomali algılama uyarısı - yinelenen sözcük olmayan karakter|
|950008|Belgelenmemiş ColdFusion etiket ekleme|
|950010|LDAP ekleme saldırısına|
|950011|Saldırı SSI ekleme|
|950018|Evrensel PDF XSS URL algılandı.|
|950019|E-posta ekleme saldırısına|
|950012|HTTP isteği kaçakçılığı saldırı.|
|950910|HTTP yanıtı bölme saldırı|
|950911|HTTP yanıtı bölme saldırı|
|950117|Uzak dosya ekleme Saldırıcı|
|950118|Uzak dosya ekleme Saldırıcı|
|950119|Uzak dosya ekleme Saldırıcı|
|950120|Olası bir uzak dosya ekleme (RFI) Saldırıcı etki başvuru/Link|
|981133|Kural 981133|
|981134|Kural 981134|
|950009|Oturum sabitleme saldırı|
|950003|Oturum sabitleme|
|950000|Oturum sabitleme|
|950005|Uzak dosya erişim denemesi|
|950002|Sistem komut erişimi|
|950006|Sistem komut ekleme|
|959151|PHP ekleme saldırısına|
|958976|PHP ekleme saldırısına|
|958977|PHP ekleme saldırısına|

### <a name="crs41sql"></a> crs_41_sql_injection_attacks

|RuleId|Açıklama|
|---|---|
|981231|SQL açıklama dizisi algılandı.|
|981260|Belirtilen SQL onaltılık kodlama|
|981320|SQL ekleme saldırısına ortak DB adları algılandı =|
|981300|Kural 981300|
|981301|Kural 981301|
|981302|Kural 981302|
|981303|Kural 981303|
|981304|Kural 981304|
|981305|Kural 981305|
|981306|Kural 981306|
|981307|Kural 981307|
|981308|Kural 981308|
|981309|Kural 981309|
|981310|Kural 981310|
|981311|Kural 981311|
|981312|Kural 981312|
|981313|Kural 981313|
|981314|Kural 981314|
|981315|Kural 981315|
|981316|Kural 981316|
|981317|SQL SELECT deyimi Anomali algılama Uyarısı|
|950007|Görme engelli SQL ekleme saldırısına|
|950001|SQL ekleme saldırısına|
|950908|SQL ekleme saldırısına.|
|959073|SQL ekleme saldırısına|
|981272|Sleep() veya benchmark() kullanarak görme sqli testlerini algılar.|
|981250|Koşullu sorguları da dahil olmak üzere SQL Kıyaslama ve uyku ekleme denemesi algılar|
|981241|Koşullu SQL ekleme girişimlerini algılar|
|981276|Temel sql ekleme aranıyor. Mysql oracle ve diğerleri için ortak saldırı dizesi.|
|981270|Temel MongoDB SQL ekleme girişimlerini bulur|
|981253|MySQL ve PostgreSQL algılar depolanan yordam/işlev ekleme|
|981251|MySQL UDF ekleme ve diğer veri/yapısı düzenleme algılar çalışır|

### <a name="crs41xss"></a> crs_41_xss_attacks

|RuleId|Açıklama|
|---|---|
|973336|XSS filtre - kategori 1 = betiği etiketi vektör|
|973338|XSS filtre - kategori 3 Javascript URI vektör =|
|981136|Kural 981136|
|981018|Kural 981018|
|958016|Siteler arası betik (XSS) saldırı|
|958414|Siteler arası betik (XSS) saldırı|
|958032|Siteler arası betik (XSS) saldırı|
|958026|Siteler arası betik (XSS) saldırı|
|958027|Siteler arası betik (XSS) saldırı|
|958054|Siteler arası betik (XSS) saldırı|
|958418|Siteler arası betik (XSS) saldırı|
|958034|Siteler arası betik (XSS) saldırı|
|958019|Siteler arası betik (XSS) saldırı|
|958013|Siteler arası betik (XSS) saldırı|
|958408|Siteler arası betik (XSS) saldırı|
|958012|Siteler arası betik (XSS) saldırı|
|958423|Siteler arası betik (XSS) saldırı|
|958002|Siteler arası betik (XSS) saldırı|
|958017|Siteler arası betik (XSS) saldırı|
|958007|Siteler arası betik (XSS) saldırı|
|958047|Siteler arası betik (XSS) saldırı|
|958410|Siteler arası betik (XSS) saldırı|
|958415|Siteler arası betik (XSS) saldırı|
|958022|Siteler arası betik (XSS) saldırı|
|958405|Siteler arası betik (XSS) saldırı|
|958419|Siteler arası betik (XSS) saldırı|
|958028|Siteler arası betik (XSS) saldırı|
|958057|Siteler arası betik (XSS) saldırı|
|958031|Siteler arası betik (XSS) saldırı|
|958006|Siteler arası betik (XSS) saldırı|
|958033|Siteler arası betik (XSS) saldırı|
|958038|Siteler arası betik (XSS) saldırı|
|958409|Siteler arası betik (XSS) saldırı|
|958001|Siteler arası betik (XSS) saldırı|
|958005|Siteler arası betik (XSS) saldırı|
|958404|Siteler arası betik (XSS) saldırı|
|958023|Siteler arası betik (XSS) saldırı|
|958010|Siteler arası betik (XSS) saldırı|
|958411|Siteler arası betik (XSS) saldırı|
|958422|Siteler arası betik (XSS) saldırı|
|958036|Siteler arası betik (XSS) saldırı|
|958000|Siteler arası betik (XSS) saldırı|
|958018|Siteler arası betik (XSS) saldırı|
|958406|Siteler arası betik (XSS) saldırı|
|958040|Siteler arası betik (XSS) saldırı|
|958052|Siteler arası betik (XSS) saldırı|
|958037|Siteler arası betik (XSS) saldırı|
|958049|Siteler arası betik (XSS) saldırı|
|958030|Siteler arası betik (XSS) saldırı|
|958041|Siteler arası betik (XSS) saldırı|
|958416|Siteler arası betik (XSS) saldırı|
|958024|Siteler arası betik (XSS) saldırı|
|958059|Siteler arası betik (XSS) saldırı|
|958417|Siteler arası betik (XSS) saldırı|
|958020|Siteler arası betik (XSS) saldırı|
|958045|Siteler arası betik (XSS) saldırı|
|958004|Siteler arası betik (XSS) saldırı|
|958421|Siteler arası betik (XSS) saldırı|
|958009|Siteler arası betik (XSS) saldırı|
|958025|Siteler arası betik (XSS) saldırı|
|958413|Siteler arası betik (XSS) saldırı|
|958051|Siteler arası betik (XSS) saldırı|
|958420|Siteler arası betik (XSS) saldırı|
|958407|Siteler arası betik (XSS) saldırı|
|958056|Siteler arası betik (XSS) saldırı|
|958011|Siteler arası betik (XSS) saldırı|
|958412|Siteler arası betik (XSS) saldırı|
|958008|Siteler arası betik (XSS) saldırı|
|958046|Siteler arası betik (XSS) saldırı|
|958039|Siteler arası betik (XSS) saldırı|
|958003|Siteler arası betik (XSS) saldırı|
|973300|Olası XSS saldırısı algılandı - HTML etiketi işleyicisi|
|973301|XSS saldırısı algılandı|
|973302|XSS saldırısı algılandı|
|973303|XSS saldırısı algılandı|
|973304|XSS saldırısı algılandı|
|973305|XSS saldırısı algılandı|
|973306|XSS saldırısı algılandı|
|973307|XSS saldırısı algılandı|
|973308|XSS saldırısı algılandı|
|973309|XSS saldırısı algılandı|
|973311|XSS saldırısı algılandı|
|973313|XSS saldırısı algılandı|
|973314|XSS saldırısı algılandı|
|973331|IE XSS filtreleri - saldırısı algılandı.|
|973315|IE XSS filtreleri - saldırısı algılandı.|
|973330|IE XSS filtreleri - saldırısı algılandı.|
|973327|IE XSS filtreleri - saldırısı algılandı.|
|973326|IE XSS filtreleri - saldırısı algılandı.|
|973346|IE XSS filtreleri - saldırısı algılandı.|
|973345|IE XSS filtreleri - saldırısı algılandı.|
|973324|IE XSS filtreleri - saldırısı algılandı.|
|973323|IE XSS filtreleri - saldırısı algılandı.|
|973348|IE XSS filtreleri - saldırısı algılandı.|
|973321|IE XSS filtreleri - saldırısı algılandı.|
|973320|IE XSS filtreleri - saldırısı algılandı.|
|973318|IE XSS filtreleri - saldırısı algılandı.|
|973317|IE XSS filtreleri - saldırısı algılandı.|
|973329|IE XSS filtreleri - saldırısı algılandı.|
|973328|IE XSS filtreleri - saldırısı algılandı.|

### <a name="crs42"></a> crs_42_tight_security

|RuleId|Açıklama|
|---|---|
|950103|Yol çapraz geçiş saldırısı|

### <a name="crs45"></a> crs_45_trojans

|RuleId|Açıklama|
|---|---|
|950110|Arka kapı erişimi|
|950921|Arka kapı erişimi|
|950922|Arka kapı erişimi|

## <a name="next-steps"></a>Sonraki adımlar

WAF kurallarını ederek devre dışı bırakma hakkında bilgi edinin: [özelleştirme WAF kuralları](application-gateway-customize-waf-rules-portal.md)

[1]: ./media/application-gateway-integration-security-center/figure1.png
