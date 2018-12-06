---
title: 'Azure Cosmos DB: SQL söz dizimi sorgusu başvurusu'
description: Azure Cosmos DB SQL sorgu dili için başvuru belgeleri.
services: cosmos-db
author: LalithaMV
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.topic: reference
ms.date: 08/19/2018
ms.author: laviswa
ms.openlocfilehash: 26dc21a7d6d24df70a0d7884c67180624074636a
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52972482"
---
# <a name="azure-cosmos-db-sql-language-reference"></a>Azure Cosmos DB SQL dil başvurusu 

Tanıdık bir SQL (yapılandırılmış sorgu dili) kullanarak belgelerin sorgulanmasını azure Cosmos DB destekler, bir açık şema veya ikincil dizinlerin oluşturulmasını gerek kalmadan, hiyerarşik JSON belgelerini dilbilgisi ister. Bu makalede SQL API hesabı ile uyumlu SQL sorgu dili sözdizimi için belgeler sağlar. Örnek SQL sorguları için bkz [Cosmos DB'de SQL sorguları](how-to-sql-query.md).  
  
Ziyaret [sorgu oyun alanı](http://www.documentdb.com/sql/demo) burada Cosmos DB'yi deneyin ve bizim veri kümesinde SQL sorguları çalıştırın.  
  
## <a name="select-query"></a>SELECT sorgusu  
Her sorgu bir SELECT yan tümcesi ve isteğe bağlı FROM oluşur ve WHERE yan tümcelerini başına ANSI SQL standartları. Genellikle, her sorgu için kaynak FROM yan tümcesindeki numaralandırılmış alan şeklinde. Ardından filtre WHERE yan tümcesinde bir alt kümesi JSON belgelerini almak için kaynak uygulanır. Son olarak, SELECT yan tümcesi, select listesindeki istenen JSON değerleri proje için kullanılır. SELECT deyimleri tanımlamak için kullanılan kuralları söz dizimi kuralları bölümünde tabloda verilmiştir. Örnekler için bkz [SELECT sorgu örnekleri](how-to-sql-query.md#SelectClause)
  
**Söz dizimi**  
  
```sql
<select_query> ::=  
SELECT <select_specification>   
    [ FROM <from_specification>]   
    [ WHERE <filter_condition> ]  
    [ ORDER BY <sort_specification> ]  
```  
  
 **Açıklamalar**  
  
 Her yan tümcesi için bölümlere aşağıdaki bakın:  
  
-   [SELECT yan tümcesi](#bk_select_query)    
-   [FROM yan tümcesi](#bk_from_clause)    
-   [WHERE yan tümcesi](#bk_where_clause)    
-   [ORDER BY yan tümcesi](#bk_orderby_clause)  
  
Yan tümceleri SELECT deyiminde, yukarıda gösterildiği gibi sıralanmış olmaları gerekmektedir. İsteğe bağlı yan tümceleri herhangi biri atlanabilir. Ancak, isteğe bağlı yan tümceleri kullanıldığında, doğru sırada yer almalıdır.  
  
### <a name="logical-processing-order-of-the-select-statement"></a>Mantıksal sipariş işleme SELECT deyiminin  
  
Yan tümceleri işlenme sırası şöyledir:  

1.  [FROM yan tümcesi](#bk_from_clause)  
2.  [WHERE yan tümcesi](#bk_where_clause)  
3.  [ORDER BY yan tümcesi](#bk_orderby_clause)  
4.  [SELECT yan tümcesi](#bk_select_query)  

Bu söz diziminde görünme sırasını farklı olduğunu unutmayın. İşlenen bir yan tümce tarafından kullanıma sunulan tüm yeni simgeler görünür ve daha sonra işlenen yan tümcelerinde kullanılabilir olacağı şekilde sıralamadır. Örneğin, bir FROM yan tümcesinde bildirilen diğer adlar nerede erişilebilir ve SELECT yan.  

### <a name="whitespace-characters-and-comments"></a>Boşluk karakterleri ve açıklamalar  

Tırnak işaretli dize parçası olmayan ya da tanımlayıcı tırnak içinde tüm boşluk karakterleri dil dilbilgisi parçası değildir ve ayrıştırma sırasında yok sayılır.  

T-SQL stili açıklamaları gibi sorgu dili destekler  

-   SQL deyimi `-- comment text [newline]`  

Boşluk karakterleri ve açıklamalar herhangi bir anlam dilbilgisi değil olsa da, belirteçlerin ayırmak için kullanılmalıdır. Örneğin: `-1e5` tek bir sayı belirteç süre olan`: – 1 e5` eksi bir belirteç sayısı 1 ve tanımlayıcı e5 tarafından izlenir.  

##  <a name="bk_select_query"></a> SELECT yan tümcesi  
Yan tümceleri SELECT deyiminde, yukarıda gösterildiği gibi sıralanmış olmaları gerekmektedir. İsteğe bağlı yan tümceleri herhangi biri atlanabilir. Ancak, isteğe bağlı yan tümceleri kullanıldığında, doğru sırada yer almalıdır. Örnekler için bkz [SELECT sorgu örnekleri](how-to-sql-query.md#SelectClause).

**Söz dizimi**  

```sql
SELECT <select_specification>  

<select_specification> ::=   
      '*'   
      | <object_property_list>   
      | VALUE <scalar_expression> [[ AS ] value_alias]  
  
<object_property_list> ::=   
{ <scalar_expression> [ [ AS ] property_alias ] } [ ,...n ]  
  
```  
  
 **Bağımsız Değişkenler**  
  
- `<select_specification>`  

  Özellikleri veya sonuç kümesi için seçilecek değeri.  
  
- `'*'`  

  Değer herhangi bir değişiklik yapmadan alınması gerektiğini belirtir. İşlenen değer bir nesne ise, özellikle, tüm özellikleri alınır.  
  
- `<object_property_list>`  
  
  Alınacak özelliklerinin listesini belirtir. Her döndürülen değer, belirtilen özellikleri içeren bir nesne olacaktır.  
  
- `VALUE`  

  JSON değerinin yerine tam JSON nesne alınacağını belirtir. Bunun aksine `<property_list>` Öngörülen Değer nesneyi kaydırma değil.  
  
- `<scalar_expression>`  

  Hesaplanmasını değeri gösteren ifade. Bkz: [skaler ifadelerin](#bk_scalar_expressions) ayrıntıları bölümü.  
  
**Açıklamalar**  
  
`SELECT *` Söz dizimi FROM yan tümcesi tam olarak bir diğer ad bildirilmişlerse geçerli yalnızca. `SELECT *` projeksiyon gerekirse yararlı olabilecek bir kimlik yansıtma sağlar. SEÇİN * yalnızca FROM yan tümcesi belirtilmiş ise geçerlidir ve yalnızca tek bir giriş kaynağı kullanıma sunulmuştur.  
  
Her ikisi de `SELECT <select_list>` ve `SELECT *` "söz dizimi sugar" olan ve aşağıda gösterildiği gibi basit bir SELECT deyimi kullanarak alternatif olarak belirtilebilir.  
  
1. `SELECT * FROM ... AS from_alias ...`  
  
   eşdeğerdir:  
  
   `SELECT from_alias FROM ... AS from_alias ...`  
  
2. `SELECT <expr1> AS p1, <expr2> AS p2,..., <exprN> AS pN [other clauses...]`  
  
   eşdeğerdir:  
  
   `SELECT VALUE { p1: <expr1>, p2: <expr2>, ..., pN: <exprN> }[other clauses...]`  
  
**Ayrıca bkz:**  
  
[Skaler ifade](#bk_scalar_expressions)  
[SELECT yan tümcesi](#bk_select_query)  
  
##  <a name="bk_from_clause"></a> FROM yan tümcesi  
Kaynak veya birleştirilmiş kaynakları belirtir. Kaynak filtre veya sorguyu daha sonra öngörülen sürece FROM yan tümcesi isteğe bağlıdır. Bu yan tümce amacı, veri kaynağına bağlı sorgu çalışmalıdır belirtmektir. Yaygın olarak tüm kaynak kapsayıcısıdır, ancak bir kapsayıcının alt bunun yerine belirtebilirsiniz. Bu yan tümce belirtilmezse, diğer yan tümceleri hala FROM yan tümcesi tek bir belge sağladıysanız olarak yürütülür. Örnekler için bkz [yan tümcesi ÖRNEKLERDEN](how-to-sql-query.md#FromClause)
  
**Söz dizimi**  
  
```sql  
FROM <from_specification>  
  
<from_specification> ::=   
        <from_source> {[ JOIN <from_source>][,...n]}  
  
<from_source> ::=   
          <container_expression> [[AS] input_alias]  
        | input_alias IN <container_expression>  
  
<container_expression> ::=   
        ROOT   
     | container_name  
     | input_alias  
     | <container_expression> '.' property_name  
     | <container_expression> '[' "property_name" | array_index ']'  
```  
  
**Bağımsız Değişkenler**  
  
- `<from_source>`  
  
  Bir veri kaynağı olan veya olmayan bir diğer ad belirtir. Diğer ad belirtilmezse, içinden gösterilen `<container_expression>` kuralları kullanarak:  
  
  -  İfade bir container_name ise container_name diğer ad olarak kullanılır.  
  
  -  İfade ise `<container_expression>`, property_name sonra property_name diğer ad olarak kullanılır. İfade bir container_name ise container_name diğer ad olarak kullanılır.  
  
- FARKLI `input_alias`  
  
  Belirten `input_alias` temel alınan kapsayıcı ifadesi tarafından döndürülen değerler kümesidir.  
 
- `input_alias` GİRİŞ  
  
  Belirten `input_alias` temel alınan kapsayıcı ifadesi tarafından döndürülen her dizinin tüm dizi öğeleri üzerinde yineleme tarafından alınan değerler kümesini temsil etmelidir. Bir dizi değil temel alınan kapsayıcı ifadesi tarafından döndürülen herhangi bir değer yoksayılır.  
  
- `<container_expression>`  
  
  Belgeleri almak için kullanılacak kapsayıcı ifadesini belirtir.  
  
- `ROOT`  
  
  Bu belge, varsayılan, bağlı durumda kapsayıcı alınması gerektiğini belirtir.  
  
- `container_name`  
  
  Bu belgede sağlanan kapsayıcıdan alınacağını belirtir. Kapsayıcının adı şu anda bağlı kapsayıcısının adı eşleşmelidir.  
  
- `input_alias`  
  
  Bu belgede sağlanan diğer ad tarafından tanımlanan diğer kaynaktan alınması gerektiğini belirtir.  
  
- `<container_expression> '.' property_`  
  
  Bu belge erişerek alınabilir belirtir `property_name` özelliği veya dizi_dizini dizi öğesi tarafından alınan tüm belgeler için belirtilen kapsayıcı ifade.  
  
- `<container_expression> '[' "property_name" | array_index ']'`  
  
  Bu belge erişerek alınabilir belirtir `property_name` özelliği veya dizi_dizini dizi öğesi tarafından alınan tüm belgeler için belirtilen kapsayıcı ifade.  
  
**Açıklamalar**  
  
Tüm diğer adları içinde çıkarımı yapılan veya sağlanan `<from_source>(`s) benzersiz olmalıdır. Söz dizimi `<container_expression>.`property_name aynıdır `<container_expression>' ['"property_name"']'`. Ancak, bir özellik adı bir tanımlayıcı olmayan karakter içeriyorsa, ikinci sözdizimi kullanılabilir.  
  
### <a name="handling-missing-properties-missing-array-elements-and-undefined-values"></a>Dizi öğeleri yanı sıra, tanımsız değerler eksik özellikler eksik işleme
  
Bir kapsayıcı ifadesi özellikleri veya dizi öğeleri ve değeri yok erişirse, bu değer yoksayılır ve daha fazla işlenmedi.  
  
### <a name="container-expression-context-scoping"></a>Kapsayıcı bağlamı ifadesi kapsamı  
  
Bir kapsayıcı ifade kapsayıcı kapsamlı veya belge kapsamlı olabilir:  
  
-   Kapsayıcı-kapsayıcı ifadenin temel alınan kaynak ya da kök ise kapsamı, bir ifade veya `container_name`. Böyle bir ifade, bir kapsayıcıdan doğrudan alınan belge kümesini temsil eder ve diğer kapsayıcı ifadeler işleme bağımlı değildir.  
  
-   Bir ifade belge temel alınan kaynak kapsayıcı ifadenin ise kapsamlı, `input_alias` sorgu daha önce sunulan. Böyle bir ifade, diğer adlı kapsayıcı ile ilişkili bir kümeye ait her bir belgenin kapsamında kapsayıcı ifadesinin değerlendirilmesi elde belgeleri kümesini temsil eder.  Sonuç kümesi, her bir temel alınan belgeler için kapsayıcı ifadesinin değerlendirilmesi elde edilen kümeleri birleşimi olacaktır.  
  
### <a name="joins"></a>Birleştirme 
  
Geçerli sürümde, Cosmos DB iç birleştirmeler destekler. Ek birleştirme özellikleri yeni çıkacak. 

İç birleştirmeler birleştirme işleminde katılan kümeleri, eksiksiz bir çapraz ürün sonuçlanır. Çok yönlü birleştirme sonucunu, burada her bir tanımlama grubu değeri katılan birleştirme işleminde ayarlamak diğer adlı ile ilişkilendirilir ve bu diğer ad diğer yan tümceleri içinde başvurarak erişilebilir N-öğe dizilerini kümesidir. Örnekler için bkz [birleştirme anahtar sözcüğü örnekleri](how-to-sql-query.md#Joins)
  
Birleşimin değerlendirme katılımcı kümelerinin bağlam kapsamına bağlıdır:  
  
-  Bir birleştirme arasında kapsayıcı-set A ve kapsayıcı kapsamlı B, A ve b kümelerindeki tüm öğelerin çapraz ürün sonuçları ayarlayın
  
-   A. B her belge için olarak belge kapsamı ayarlayın değerlendirerek alınan tüm ayarlar birleşimini kümesi belge kapsamı küme B arasında birleştirme sonuçlanıyor  
  
 Geçerli sürümde, kapsayıcı kapsamlı bir ifadenin en fazla sorgu işlemcisi tarafından desteklenir.  
  
### <a name="examples-of-joins"></a>Birleştirme örnekleri  
  
Aşağıdaki FROM yan tümcesi göz atalım: `<from_source1> JOIN <from_source2> JOIN ... JOIN <from_sourceN>`  
  
 Her kaynak tanımlamak olanak `input_alias1, input_alias2, …, input_aliasN`. Bu FROM yan tümcesi, N-tanımlama grubu (N değeri ile tanımlama grubu) kümesini döndürür. Her bir tanımlama grubunu tüm kapsayıcı diğer adları kendi ilgili ayarlar yineleme tarafından üretilen değerler içeriyor.  
  
**Örnek 1** -2 kaynakları  
  
- İzin `<from_source1>` kapsayıcı kapsamına- ve set {A, B, C} temsil eder.  
  
- İzin `<from_source2>` input_alias1 başvuran belge kapsamlı ve kümeleri temsil eder:  
  
    {1, 2} için `input_alias1 = A,`  
  
    {3} için `input_alias1 = B,`  
  
    {4, 5} için `input_alias1 = C,`  
  
- FROM yan tümcesi `<from_source1> JOIN <from_source2>` de aşağıdaki tanımlama gruplarına neden olur:  
  
    (`input_alias1, input_alias2`):  
  
    `(A, 1), (A, 2), (B, 3), (C, 4), (C, 5)`  
  
**Örnek 2** -3 kaynakları  
  
- İzin `<from_source1>` kapsayıcı kapsamına- ve set {A, B, C} temsil eder.  
  
- İzin `<from_source2>` belge kapsamlı başvuran olması `input_alias1` ve kümeleri temsil eder:  
  
    {1, 2} için `input_alias1 = A,`  
  
    {3} için `input_alias1 = B,`  
  
    {4, 5} için `input_alias1 = C,`  
  
- İzin `<from_source3>` belge kapsamlı başvuran olması `input_alias2` ve kümeleri temsil eder:  
  
    {100, 200} için `input_alias2 = 1,`  
  
    {300} için `input_alias2 = 3,`  
  
- FROM yan tümcesi `<from_source1> JOIN <from_source2> JOIN <from_source3>` de aşağıdaki tanımlama gruplarına neden olur:  
  
    (input_alias1, input_alias2, input_alias3):  
  
    (BİR, 1, 100), (A, 1, 200), (B, 3, 300)  
  
  > [!NOTE]
  > Diğer değerler için demetleri eksikliği `input_alias1`, `input_alias2`, kendisi için `<from_source3>` herhangi bir değer döndürmedi.  
  
**Örnek 3** -3 kaynakları  
  
- < {A, B, C} kümesini temsil eder ve kapsayıcı kapsamlı from_source1 > sağlar.  
  
- İzin `<from_source1>` kapsayıcı kapsamına- ve set {A, B, C} temsil eder.  
  
- < Belge kapsamlı başvuru input_alias1 olması ve kümelerini göstermek from_source2 > sağlar:  
  
    {1, 2} için `input_alias1 = A,`  
  
    {3} için `input_alias1 = B,`  
  
    {4, 5} için `input_alias1 = C,`  
  
- İzin `<from_source3>` kapsamı `input_alias1` ve kümeleri temsil eder:  
  
    {100, 200} için `input_alias2 = A,`  
  
    {300} için `input_alias2 = C,`  
  
- FROM yan tümcesi `<from_source1> JOIN <from_source2> JOIN <from_source3>` de aşağıdaki tanımlama gruplarına neden olur:  
  
    (`input_alias1, input_alias2, input_alias3`):  
  
    (BİR, 1, 100), (A, 1, 200), (A, 2, 100), (A, 2, 200), (C, 4, 300), (C, 5, 300)  
  
  > [!NOTE]
  > Bu arasında çapraz ürün içinde sonuçlanan `<from_source2>` ve `<from_source3>` her ikisi de aynı belirlenir çünkü `<from_source1>`.  (2 x 2) 4'te bu durum diziler değerini 0 tanımlama grubu B (1 x 0) değerine sahip olan ve 2 (2 x 1) değeri c diziler  
  
**Ayrıca bkz:**  
  
 [SELECT yan tümcesi](#bk_select_query)  
  
##  <a name="bk_where_clause"></a> WHERE yan tümcesi  
 Sorgu tarafından döndürülen belgeler için arama koşulunu belirtir. Örnekler için bkz [WHERE yan tümcesi örnekleri](how-to-sql-query.md#WhereClause)
  
 **Söz dizimi**  
  
```sql  
WHERE <filter_condition>  
<filter_condition> ::= <scalar_expression>  
  
```  
  
 **Bağımsız Değişkenler**  
  
-   `<filter_condition>`  
  
     Döndürülecek belgeler için karşılanması gereken bir koşulu belirtir.  
  
-   `<scalar_expression>`  
  
     Hesaplanmasını değeri gösteren ifade. Bkz: [skaler ifadelerin](#bk_scalar_expressions) ayrıntıları bölümü.  
  
 **Açıklamalar**  
  
 Filtre olarak belirtilen bir ifade döndürülecek belge sırada koşul true olarak değerlendirilmelidir. Başka bir değer koşulu, Boole değeri true yerine getirecek yalnızca: tanımsız, null, false, sayı, dizi veya nesne karşılamaz koşul.  
  
##  <a name="bk_orderby_clause"></a> ORDER BY yan tümcesi  
 Sorgu tarafından döndürülen sonuçları sıralama düzenini belirtir. Örnekler için bkz [ORDER BY yan tümcesi örnekleri](how-to-sql-query.md#OrderByClause)
  
 **Söz dizimi**  
  
```sql  
ORDER BY <sort_specification>  
<sort_specification> ::= <sort_expression> [, <sort_expression>]  
<sort_expression> ::= <scalar_expression> [ASC | DESC]  
  
```  
  
 **Bağımsız Değişkenler**  
  
-   `<sort_specification>`  
  
     Bir özellik ya da üzerinde sorgu sonucu kümesini sıralama yapılacak ifade belirtir. Sıralama sütunu adı ya da sütun diğer adı belirtilebilir.  
  
     Sütunları Sırala birden çok belirtilebilir. Sütun adları benzersiz olmalıdır. Sıralanmış bir sonuç kümesi organizasyonu ORDER BY yan tümcesindeki sıralama sütun sırasını tanımlar. Diğer bir deyişle, sonuç kümesi ilk özelliği tarafından sıralanır ve ardından bu sıralı liste ikinci özelliği vb. göre sıralanır.  
  
     ORDER BY yan tümcesinde başvurulan sütun adları ya da bir sütuna seçim listesinde veya tanımlanan tüm belirsizlikleri olmadan FROM yan tümcesinde belirtilen bir tablodaki bir sütuna karşılık gelmelidir.  
  
-   `<sort_expression>`  
  
     Tek bir özellik veya üzerinde sorgu sonucu kümesini sıralama yapılacak ifade belirtir.  
  
-   `<scalar_expression>`  
  
     Bkz: [skaler ifadelerin](#bk_scalar_expressions) ayrıntıları bölümü.  
  
-   `ASC | DESC`  
  
     Belirtilen sütundaki değerleri artan veya azalan olarak sıralanması gerektiğini belirtir. ASC en düşük değerden yüksek değer sıralar. DESC en yüksek değerden en düşük değere doğru sıralar. ASC varsayılan sıralama sırasıdır. Null değerler, en düşük olası değerler kabul edilir.  
  
 **Açıklamalar**  
  
 Sorgu dil bilgisi, özellik tarafından birden çok sipariş desteklese de, Cosmos DB sorgu çalışma zamanı yalnızca tek bir özelliğe karşı ve yalnızca özellik adlarının (değil karşı hesaplanan Özellikler) göre sıralamayı destekler. Sıralama, aynı zamanda dizin oluşturma ilkesini özelliği ve verilen duyarlık ile belirtilen tür için bir aralık dizini içerir gerektirir. Daha fazla ayrıntı için dizin oluşturma ilkesi belgeleri bakın.  
  
##  <a name="bk_scalar_expressions"></a> Skaler ifade  
 Skaler bir ifade, semboller ve işleçleri tek bir değer almak için değerlendirilen bir birleşimidir. Basit ifadeler sabitler, özellik başvuru, dizi öğesi başvuruları, diğer başvurular veya işlev çağrıları olabilir. Basit ifadeler işleçleri kullanarak karmaşık ifadelere birleştirilebilir. Örnekler için bkz [skaler ifade örnekleri](how-to-sql-query.md#scalar-expressions)
  
 Skaler ifade olabilecek değerler hakkında daha fazla bilgi için bkz: [sabitleri](#bk_constants) bölümü.  
  
 **Söz dizimi**  
  
```sql  
<scalar_expression> ::=  
       <constant>   
     | input_alias   
     | parameter_name  
     | <scalar_expression>.property_name  
     | <scalar_expression>'['"property_name"|array_index']'  
     | unary_operator <scalar_expression>  
     | <scalar_expression> binary_operator <scalar_expression>    
     | <scalar_expression> ? <scalar_expression> : <scalar_expression>  
     | <scalar_function_expression>  
     | <create_object_expression>   
     | <create_array_expression>  
     | (<scalar_expression>)   
  
<scalar_function_expression> ::=  
        'udf.' Udf_scalar_function([<scalar_expression>][,…n])  
        | builtin_scalar_function([<scalar_expression>][,…n])  
  
<create_object_expression> ::=  
   '{' [{property_name | "property_name"} : <scalar_expression>][,…n] '}'  
  
<create_array_expression> ::=  
   '[' [<scalar_expression>][,…n] ']'  
  
```  
  
 **Bağımsız Değişkenler**  
  
-   `<constant>`  
  
     Sabit bir değeri temsil eder. Bkz: [sabitleri](#bk_constants) ayrıntıları bölümü.  
  
-   `input_alias`  
  
     Tarafından tanımlanan bir değeri temsil `input_alias` sunulan `FROM` yan tümcesi.  
    Bu değer olmaması garanti **tanımlanmamış** –**tanımlanmamış** giriş değerleri atlanır.  
  
-   `<scalar_expression>.property_name`  
  
     Bir nesnenin özellik değerini temsil eder. Özellik mevcut değil veya özelliği bir nesne olmayan bir değer başvurulan durumunda ifadenin değerlendirdiği **tanımlanmamış** değeri.  
  
-   `<scalar_expression>'['"property_name"|array_index']'`  
  
     Bir ada sahip özelliğin değerini temsil eder `property_name` veya dizi öğesi `array_index` nesne/dizi. Özelliği/dizi dizini mevcut değil veya özellik/dizi dizininden başvurulan bir değeri bir nesne/dizisi olmayan ve ardından ifadeyi tanımlanmamış değerini hesaplar.  
  
-   `unary_operator <scalar_expression>`  
  
     Tek bir değer için uygulanan bir işleç temsil eder. Bkz: [işleçleri](#bk_operators) ayrıntıları bölümü.  
  
-   `<scalar_expression> binary_operator <scalar_expression>`  
  
     İki değer için uygulanan bir işleç temsil eder. Bkz: [işleçleri](#bk_operators) ayrıntıları bölümü.  
  
-   `<scalar_function_expression>`  
  
     Bir işlev çağrısı sonucunu tarafından tanımlanan bir değeri temsil eder.  
  
-   `udf_scalar_function`  
  
     Kullanıcı tanımlı skaler işlevin adı.  
  
-   `builtin_scalar_function`  
  
     Yerleşik skaler işlevin adı.  
  
-   `<create_object_expression>`  
  
     Belirtilen özelliklere sahip yeni bir nesne oluşturarak elde ettiği değerle ve bunların değerlerini temsil eder.  
  
-   `<create_array_expression>`  
  
     Öğeleri olarak belirtilen değerlerle yeni bir dizi oluşturarak elde ettiği değerle temsil eder  
  
-   `parameter_name`  
  
     Belirtilen parametre adı değerini temsil eder. Tek bir parametre adları olmalıdır \@ ilk karakteri olarak.  
  
 **Açıklamalar**  
  
 Tüm bağımsız değişkenleri, yerleşik veya kullanıcı tanımlı skaler bir işlev çağrılırken tanımlanmalıdır. Bağımsız değişken tanımlanmamış, işlev çağrılmaz ve sonuç tanımsız olur.  
  
 Bir nesne oluştururken, tanımlanmamış değeri atanır. herhangi bir özelliği atlandı ve oluşturulan nesneyi dahil değildir.  
  
 Zaman dizisi, herhangi bir öğe değer oluşturma atandığı **tanımlanmamış** değeri atlandı ve oluşturulan nesneyi dahil değildir. Bu şekilde oluşturulan dizi dizinleri atlandı değil, onun yerine almak sonraki tanımlanan öğe neden olur.  
  
##  <a name="bk_operators"></a> İşleçleri  
 Bu bölümde, desteklenen işleçleri açıklanmaktadır. Her işleç tam olarak bir kategoriye atayabilirsiniz.  
  
 Bkz: **işleci kategorileri** Ayrıntılar için aşağıdaki tabloya işlenmesi ilgili **tanımlanmamış** değerleri, giriş değerleri ve işleme ile eşleşmeyen türler değer türü gereksinimleri.  
  
 **İşleç kategorisi:**  
  
|**Kategori**|**Ayrıntılar**|  
|-|-|  
|**Aritmetik**|İşleç girişlere numaralarını olmasını bekliyor. Çıkış ayrıca bir sayıdır. Herhangi biri ise **tanımlanmamış** veya sayı sonuç dışında türü **tanımlanmamış**.|  
|**bit düzeyinde**|İşleci, girişlere 32 bitlik işaretli tamsayı numaraları olmasını bekliyor. Çıkışı, 32 bitlik işaretli tamsayı numarası da yapılır.<br /><br /> Herhangi bir tamsayı olmayan değer yuvarlanır. Pozitif değer aşağı yuvarlanır, negatif değerleri yuvarlanır.<br /><br /> Son 32 biti kendi ikiye tamamlayıcı gösterimini yararlanarak 32-bit tamsayı aralığın dışında herhangi bir değer dönüştürülür.<br /><br /> Herhangi biri ise **tanımlanmamış** veya sonuç ise sayı,'den başka **tanımlanmamış**.<br /><br /> **Not:** yukarıdaki davranışı JavaScript bit düzeyinde işleci davranışı ile uyumludur.|  
|**Mantıksal**|İşleci, girişlere Boolean(s) olmasını bekliyor. Çıkış, ayrıca bir Boole değeri.<br />Herhangi biri ise **tanımlanmamış** veya sonucu sonra Boolean, dışındaki **tanımlanmamış**.|  
|**Karşılaştırma**|İşleci, aynı türe sahip ve tanımlanmamış olmaması için girişlere bekliyor. Çıkış bir Boole değeri.<br /><br /> Herhangi biri ise **tanımlanmamış** veya girişleri farklı türlere sahip ve ardından sonuç **tanımlanmamış**.<br /><br /> Bkz: **değerleri karşılaştırma için sıralama** ayrıntıları sıralama değeri için tablo.|  
|**dize**|İşleci, girişlere dizelerini olmasını bekliyor. Çıkış de bir dizedir.<br />Herhangi biri ise **tanımlanmamış** veya sonuç sonra dize dışındaki **tanımlanmamış**.|  
  
 **Birli işleçler:**  
  
|**Ad**|**İşleci**|**Ayrıntılar**|  
|-|-|-|  
|**Aritmetik**|+<br /><br /> -|Sayı değerini döndürür.<br /><br /> Bitwise olumsuzlama. Sayı değeri negatif döndürür.|  
|**bit düzeyinde**|~|Olanları tamamlama. Bir tamamlayıcı bir sayı değeri döndürür.|  
|**Mantıksal**|**DEĞİL**|Değilleme. Boole değeri döndürür tasarruflarını.|  
  
 **İkili işleçler:**  
  
|**Ad**|**İşleci**|**Ayrıntılar**|  
|-|-|-|  
|**Aritmetik**|+<br /><br /> -<br /><br /> *<br /><br /> /<br /><br /> %|Ayrıca.<br /><br /> Çıkarma.<br /><br /> Çarpma.<br /><br /> Bölme.<br /><br /> Modülasyon.|  
|**bit düzeyinde**|&#124;<br /><br /> &<br /><br /> ^<br /><br /> <<<br /><br /> >><br /><br /> >>>|Bit düzeyinde OR.<br /><br /> Bit düzeyinde and<br /><br /> Bit düzeyinde XOR.<br /><br /> Sola kaydırma.<br /><br /> Sağa kaydırma.<br /><br /> Sıfır dolgu sağa kaydırma.|  
|**Mantıksal**|**VE**<br /><br /> **VEYA**|Mantıksal ve işlecini. Döndürür **true** her iki bağımsız değişkenler ise **true**, döndürür **false** Aksi takdirde.<br /><br /> Mantıksal ve işlecini. Döndürür **true** her iki bağımsız değişkenler ise **true**, döndürür **false** Aksi takdirde.|  
|**Karşılaştırma**|**=**<br /><br /> **!=, <>**<br /><br /> **>**<br /><br /> **>=**<br /><br /> **<**<br /><br /> **<=**<br /><br /> **??**|Eşittir. Döndürür **true** bağımsız değişkenlerin eşit olup olmadığını döndürür **false** Aksi takdirde.<br /><br /> Eşit değildir. Döndürür **true** bağımsız değişkenleri eşit değilse döndürür **false** Aksi takdirde.<br /><br /> Büyüktür. Döndürür **true** ilk bağımsız değişken ikinci sürümden daha büyük ise, dönüş **false** Aksi takdirde.<br /><br /> Büyüktür veya eşittir. Döndürür **true** ilk bağımsız değişken büyüktür veya eşittir ikincisi için ise, dönüş **false** Aksi takdirde.<br /><br /> Küçüktür. Döndürür **true** ilk bağımsız değişken küçükse değerinden ikinci bir dönüş **false** Aksi takdirde.<br /><br /> Küçüktür veya eşittir. Döndürür **true** ilk bağımsız değişken ikinci bir veya daha az ise, dönüş **false** Aksi takdirde.<br /><br /> Birleşim. İlk bağımsız değişken ikinci bağımsız değişkeni döndürür bir **tanımlanmamış** değeri.|  
|**dize**|**&#124;&#124;**|Birleştirme. Her iki bağımsız değişkenler birleşimi döndürür.|  
  
 **Üçlü işleçler:**  
  
|Üçlü işleci|?|İlk bağımsız değişken için değerlendiriliyorsa, ikinci bağımsız değişkeni döndürür **true**; üçüncü bağımsız değişken Aksi halde döndürür.|  
|-|-|-|  
  
 **Değerleri karşılaştırma için sıralama**  
  
|**Tür**|**Değerleri sırasını**|  
|-|-|  
|**Tanımsız**|Karşılaştırılabilir değil.|  
|**Null**|Tek değer: **null**|  
|**Sayı**|Doğal bir gerçek sayı.<br /><br /> Negatif sonsuz değerle başka sayı değeri küçüktür.<br /><br /> Pozitif sonsuz değer diğer sayı değeri büyüktür. **NaN** değeri karşılaştırılabilir değil. İle karşılaştırma **NaN** sonuçlanır **tanımlanmamış** değeri.|  
|**dize**|Lexicographical sırası.|  
|**Dizi**|Ancak equitable sıralama yok.|  
|**Nesne**|Ancak equitable sıralama yok.|  
  
 **Açıklamalar**  
  
 Veritabanından alınana kadar Cosmos DB'de değer türleri bilinen değildir. Verimli sorgular yürütülmesini desteklemek için işleçlerin en katı tür gereksinimleri vardır. Ayrıca işleçleri kendileri tarafından örtülü dönüştürmeler yapmayın.  
  
 Bu sorgu gibi sağladığı anlamına gelir: seçin * ÖĞESİNDEN kök r nerede r.Age = 21 yalnızca döndürecektir özelliği yaş belgelerle sayı 21 eşittir. Belge özelliği yaş dize "21" veya "0021" dizesi ile eşleşmez, "21" ifadesiyle = 21 için tanımlanmamış değerlendirir. Belirli bir değer (örneğin, sayı 21) arama sonsuz sayıda arama kıyasla daha hızlı olduğundan için daha iyi kullanımını dizinler, böylece olası (21 sayı veya dize "21", "021", "21.0"...) eşleşir. Bu, farklı türlerde değerler işleçlerin nasıl JavaScript değerlendirir öğesinden farklıdır.  
  
 **Diziler ve nesneleri eşitlik ve karşılaştırma**  
  
 Aralık işleçleri kullanarak dizi veya nesne değerleri karşılaştırma (>, > =, <, < =) nesne veya dizi değerleri üzerinde tanımlanan sırası olarak tanımlanmamış sonuçlanır. Ancak eşitlik/eşitsizlik işleçlerini kullanma (=,! =, <>) desteklenir ve değerler karşılaştırılabilir yapısal olarak.  
  
 Diziler, iki dizi aynı sayıda öğe varsa ve konumlarını eşleşen en öğeleri de eşit eşit olur. Tüm öğeleri sonuçlarında çiftinin karşılaştırma tanımlı değil, dizi karşılaştırma sonucu tanımsızdır.  
  
 Her iki nesne tanımlanan aynı özelliklere sahipse ve özellikleri eşleşen değer de eşitse, nesneler eşit olur. Herhangi bir özellik değerleri sonuçlarında çifti ile karşılaştırma tanımlı değil, nesne karşılaştırma sonucu tanımsızdır.  
  
##  <a name="bk_constants"></a> Sabitleri  
 Bir sabit olarak da bilinen bir sabit değer ya da bir skalar değer belirli bir veri değerini temsil eden bir semboldür. Biçimi bir sabiti temsil ettiği değerin veri türüne göre değişir.  
  
 **Skaler türleri desteklenir:**  
  
|**Tür**|**Değerleri sırasını**|  
|-|-|  
|**Tanımsız**|Tek değer: **tanımlanmamış**|  
|**Null**|Tek değer: **null**|  
|**Boole değeri**|Değerler: **false**, **true**.|  
|**Sayı**|Bir çift duyarlıklı kayan noktalı sayı, standart IEEE 754.|  
|**dize**|Sıfır veya daha fazla Unicode karakter dizisi. Dizeleri tek veya çift tırnak içine alınmalıdır.|  
|**Dizi**|Sıfır veya daha fazla öğe dizisi. Her öğe tanımlanmamış dışındaki tüm skaler veri türünde bir değer olabilir.|  
|**Nesne**|Sırasız bir sıfır veya daha fazla ad/değer çiftleri kümesi. Adı bir Unicode dize, değer dışında herhangi bir skaler veri türde olabilir **tanımlanmamış**.|  
  
 **Söz dizimi**  
  
```sql  
<constant> ::=  
   <undefined_constant>  
     | <null_constant>   
     | <boolean_constant>   
     | <number_constant>   
     | <string_constant>   
     | <array_constant>   
     | <object_constant>   
  
<undefined_constant> ::= undefined  
  
<null_constant> ::= null  
  
<boolean_constant> ::= false | true  
  
<number_constant> ::= decimal_literal | hexadecimal_literal  
  
<string_constant> ::= string_literal  
  
<array_constant> ::=  
    '[' [<constant>][,...n] ']'  
  
<object_constant> ::=   
   '{' [{property_name | "property_name"} : <constant>][,...n] '}'  
  
```  
  
 **Bağımsız Değişkenler**  
  
* `<undefined_constant>; undefined`  
  
  Türü tanımlanmamış değerini tanımsız temsil eder.  
  
* `<null_constant>; null`  
  
  Temsil eden **null** türünün değerini **Null**.  
  
* `<boolean_constant>`  
  
  Boole türünde bir sabiti temsil eder.  
  
* `false`  
  
  Temsil eden **false** türü Boolean değeri.  
  
* `true`  
  
  Temsil eden **true** türü Boolean değeri.  
  
* `<number_constant>`  
  
  Bir sabiti temsil eder.  
  
* `decimal_literal`  
  
  Ondalık sabit değerleri ondalık gösterim veya bilimsel gösterim kullanılarak temsil sayılardır.  
  
* `hexadecimal_literal`  
  
  Onaltılık değişmez değerler, önek '0 x'ı bir veya daha fazla onaltılık basamağın geldiği' kullanılarak temsil sayılardır.  
  
* `<string_constant>`  
  
  String türünde bir sabiti temsil eder.  
  
* `string _literal`  
  
  Dize değişmez değerleri, sıfır veya daha fazla Unicode karakter dizisi veya kaçış dizileri tarafından temsil edilen Unicode dizelerdir. Dize sabit değerlerinin tek tırnak içine alınmış (kesme işareti: ') veya çift tırnak (tırnak işareti: ").  
  
 Aşağıdaki kaçış dizileri izin verilir:  
  
|**Kaçış sırası**|**Açıklama**|**Unicode karakter**|  
|-|-|-|  
|\\'|kesme işareti (')|U + 0027|  
|\\"|tırnak işareti (")|U + 0022|  
|\\\|Ters solidus (\\)|U + 005C|  
|\\/|solidus (/)|U + 002F|  
|\b|Geri Al|U + 0008|  
|\f|form besleme|U + 000C|  
|\n|satır besleme|U + 000A|  
|\r|satır başı|U + 000D|  
|\t|sekme|U + 0009|  
|\uXXXX|4 onaltılık basamak tarafından tanımlanan Unicode karakter.|U + XXXX|  
  
##  <a name="bk_query_perf_guidelines"></a> Sorgu performans yönergeleri  
 Büyük bir kapsayıcı için verimli bir şekilde yürütülecek bir sorgu için sırayla bir veya daha fazla dizin sunulan filtreleri kullanmanız gerekir.  
  
 Aşağıdaki filtreler için dizin arama kabul edilir:  
  
-   Eşitlik işleci (=), bir belge yol ifadesi ve bir sabiti kullanın.  
  
-   Aralık işleçleri kullanın (<, \<=, >, > =) bir belge yol ifadesi ve sayı sabitleri.  
  
-   Başvurulan veritabanı kapsayıcı belgelerinden sabit bir yolda tanımlayan bir ifade belge yol ifadesi gösterir.  
  
 **Belge yol ifadesi**  
  
 Belge yolu ifadelerdir ifadeleri, özelliği veya dizi dizin oluşturucu denetçiler üzerinden bir belge veritabanı kapsayıcı belgelerden yakında yolu. Bu yol, doğrudan veritabanı kapsayıcısında belgelerde bir filtrede başvurulan değerleri konumunu tanımlamak için kullanılabilir.  
  
 Bir ifade belge yol ifadesi kabul edilmelidir:  
  
1.  Kapsayıcı kök doğrudan başvuru.  
  
2.  Bazı belge yol ifadesi başvuru özelliği veya sabiti dizisi Oluşturucusu  
  
3.  Bazı belge yol ifadesi temsil eden bir diğer ada başvuru.  
  
     **Söz dizimi kuralları**  
  
     Aşağıdaki tabloda, söz dizimi aşağıdaki SQL Başvurusu açıklamak için kullanılan kuralları açıklar.  
  
    |**Kuralı**|**İçin kullanılan**|  
    |-|-|    
    |BÜYÜK HARF|Büyük küçük harf duyarlı anahtar sözcükler.|  
    |Küçük|Büyük küçük harfe duyarlı anahtar sözcükler.|  
    |\<bildirimlere >|Bildirimlere, ayrı olarak tanımlanır.|  
    |\<bildirimlere >:: =|Bildirimlere tanımı söz dizimi.|  
    |other_terminal|Ayrıntılı sözcükler olarak açıklanan Terminal (belirteç).|  
    |tanımlayıcı|Tanımlayıcısı. Şu karakterlere yalnızca sağlar: a-z A-Z 0-9 _First karakter, bir sayı olamaz.|  
    |"dize"|Tırnak işaretli dize. Herhangi bir geçerli dize verir. Bir string_literal açıklamasına bakın.|  
    |'symbol'|Değişmez sözdiziminin bir parçası olan simge.|  
    |&#124;(dikey çubuk)|Alternatifleri için söz dizimi öğeleri. Belirtilen öğelerin yalnızca birini kullanabilirsiniz.|  
    |[] /(brackets)|Köşeli ayraç bir veya daha fazla isteğe bağlı öğeleri alın.|  
    |[,.. .n]|Önündeki öğeyi yinelenen n sayıda olabilir gösterir. Örnekleri virgülle ayrılır.|  
    |[...n]|Önündeki öğeyi yinelenen n sayıda olabilir gösterir. Örnekleri boşlukla ayrılır.|  
  
##  <a name="bk_built_in_functions"></a> Yerleşik işlevler  
 Cosmos DB, çok sayıda yerleşik SQL işlevleri sağlar. Yerleşik işlevler kategorileri aşağıda listelenmiştir.  
  
|İşlev|Açıklama|  
|--------------|-----------------|  
|[Matematik işlevleri](#bk_mathematical_functions)|Matematik işlevleri her, genellikle bağımsız değişken olarak sağlanan ve sayısal bir değer döndürmesi giriş değerlerine göre bir hesaplama gerçekleştirir.|  
|[Tür denetimini işlevleri](#bk_type_checking_functions)|Tür denetimi işlevleri SQL sorgusu içindeki bir ifadenin türü denetlemenizi sağlar.|  
|[Dize işlevleri](#bk_string_functions)|Dize işlevleri, bir dize giriş değeri bir işlem gerçekleştirmek ve bir dize, sayısal veya Boolean değeri döndürür.|  
|[Dizi işlevleri](#bk_array_functions)|Dizi işlevler bir dizi giriş değeri ve dönüş sayısal, Boole değeri veya dizi değeri üzerinde bir işlem gerçekleştirir.|  
|[Uzamsal İşlevler](#bk_spatial_functions)|Uzamsal İşlevler, uzamsal nesne giriş değeri bir işlem gerçekleştirmek ve bir sayısal veya Boolean değeri döndürür.|  
  
###  <a name="bk_mathematical_functions"></a> Matematik işlevleri  
 Aşağıdaki işlevler her, genellikle bağımsız değişken olarak sağlanan ve sayısal bir değer döndürmesi giriş değerlerine göre bir hesaplama gerçekleştirir.  
  
||||  
|-|-|-|  
|[ABS](#bk_abs)|[ACOS](#bk_acos)|[ASIN](#bk_asin)|  
|[ATAN](#bk_atan)|[ATN2](#bk_atn2)|[TAVAN](#bk_ceiling)|  
|[COS](#bk_cos)|[COT](#bk_cot)|[DERECE](#bk_degrees)|  
|[EXP](#bk_exp)|[KAT](#bk_floor)|[GÜNLÜK](#bk_log)|  
|[LOG10](#bk_log10)|[PI](#bk_pi)|[GÜÇ](#bk_power)|  
|[RADYAN CİNSİNDEN](#bk_radians)|[YUVARLAK](#bk_round)|[SIN](#bk_sin)|  
|[SQRT](#bk_sqrt)|[KARE](#bk_square)|[OTURUM](#bk_sign)|  
|[TAN](#bk_tan)|[TRUNC](#bk_trunc)||  
  
####  <a name="bk_abs"></a> ABS  
 Belirtilen sayısal ifade (pozitif) mutlak değerini döndürür.  
  
 **Söz dizimi**  
  
```  
ABS (<numeric_expression>)  
```  
  
 **Bağımsız Değişkenler**  
  
-   `numeric_expression`  
  
     Sayısal bir ifadedir.  
  
 **Dönüş türleri**  
  
 Sayısal bir ifade döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, üç farklı numaralarında ABS işlevi kullanarak sonuçları gösterir.  
  
```  
SELECT ABS(-1), ABS(0), ABS(1)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{$1: 1, $2: 0, $3: 1}]  
```  
  
####  <a name="bk_acos"></a> ACOS  
 Kosinüsü belirtilen sayısal ifadesidir radyan cinsinden açı döndürür; arkkosinüsünü olarak da adlandırılır.  
  
 **Söz dizimi**  
  
```  
ACOS(<numeric_expression>)  
```  
  
 **Bağımsız Değişkenler**  
  
-   `numeric_expression`  
  
     Sayısal bir ifadedir.  
  
 **Dönüş türleri**  
  
 Sayısal bir ifade döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek ACOS-1 döndürür.  
  
```  
SELECT ACOS(-1)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1": 3.1415926535897931}]  
```  
  
####  <a name="bk_asin"></a> ASIN  
 Açının sinüsü belirtilen sayısal ifadesidir radyan cinsinden döndürür. Bu arksinüsünü olarak da adlandırılır.  
  
 **Söz dizimi**  
  
```  
ASIN(<numeric_expression>)  
```  
  
 **Bağımsız Değişkenler**  
  
-   `numeric_expression`  
  
     Sayısal bir ifadedir.  
  
 **Dönüş türleri**  
  
 Sayısal bir ifade döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, ASIN-1 döndürür.  
  
```  
SELECT ASIN(-1)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1": -1.5707963267948966}]  
```  
  
####  <a name="bk_atan"></a> ATAN  
 Tanjantı belirtilen sayısal ifadesidir radyan cinsinden açı döndürür. Bu arktanjantını olarak da adlandırılır.  
  
 **Söz dizimi**  
  
```  
ATAN(<numeric_expression>)  
```  
  
 **Bağımsız Değişkenler**  
  
-   `numeric_expression`  
  
     Sayısal bir ifadedir.  
  
 **Dönüş türleri**  
  
 Sayısal bir ifade döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, ATAN belirtilen değeri döndürür.  
  
```  
SELECT ATAN(-45.01)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1": -1.5485826962062663}]  
```  
  
####  <a name="bk_atn2"></a> ATN2  
 Radyan cinsinden ifade edilen x, y / Ark tanjant değerini döndürür.  
  
 **Söz dizimi**  
  
```  
ATN2(<numeric_expression>, <numeric_expression>)  
```  
  
 **Bağımsız Değişkenler**  
  
-   `numeric_expression`  
  
     Sayısal bir ifadedir.  
  
 **Dönüş türleri**  
  
 Sayısal bir ifade döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek için belirtilen ATN2 hesaplar x ve y bileşenleri.  
  
```  
SELECT ATN2(35.175643, 129.44)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1": 1.3054517947300646}]  
```  
  
####  <a name="bk_ceiling"></a> TAVAN  
 Büyüktür veya eşittir, belirtilen sayısal ifadenin en küçük tamsayı değerini döndürür.  
  
 **Söz dizimi**  
  
```  
CEILING (<numeric_expression>)  
```  
  
 **Bağımsız Değişkenler**  
  
-   `numeric_expression`  
  
     Sayısal bir ifadedir.  
  
 **Dönüş türleri**  
  
 Sayısal bir ifade döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, pozitif bir sayısal, negatif ve sıfır değerlerini CEILING işlevi ile gösterilir.  
  
```  
SELECT CEILING(123.45), CEILING(-123.45), CEILING(0.0)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{$1: 124, $2: -123, $3: 0}]  
```  
  
####  <a name="bk_cos"></a> COS  
 Radyan cinsinden belirtilen ifade trigonometrik belirtilen bir açının kosinüsünü döndürür.  
  
 **Söz dizimi**  
  
```  
COS(<numeric_expression>)  
```  
  
 **Bağımsız Değişkenler**  
  
-   `numeric_expression`  
  
     Sayısal bir ifadedir.  
  
 **Dönüş türleri**  
  
 Sayısal bir ifade döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek belirtilen bir açının COS hesaplar.  
  
```  
SELECT COS(14.78)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1": -0.59946542619465426}]  
```  
  
####  <a name="bk_cot"></a> COT  
 Trigonometrik belirtilen bir açının kotanjantını radyan cinsinden belirtilen bir sayısal ifade döndürür.  
  
 **Söz dizimi**  
  
```  
COT(<numeric_expression>)  
```  
  
 **Bağımsız Değişkenler**  
  
-   `numeric_expression`  
  
     Sayısal bir ifadedir.  
  
 **Dönüş türleri**  
  
 Sayısal bir ifade döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek belirtilen bir açının COT hesaplar.  
  
```  
SELECT COT(124.1332)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1": -0.040311998371148884}]  
```  
  
####  <a name="bk_degrees"></a> DERECE  
 Karşılık gelen açıyı derece için radyan cinsinden belirtilen bir açı cinsinden döndürür.  
  
 **Söz dizimi**  
  
```  
DEGREES (<numeric_expression>)  
```  
  
 **Bağımsız Değişkenler**  
  
-   `numeric_expression`  
  
     Sayısal bir ifadedir.  
  
 **Dönüş türleri**  
  
 Sayısal bir ifade döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, PI/2 radyan cinsinden açı dereceye döndürür.  
  
```  
SELECT DEGREES(PI()/2)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1": 90}]  
```  
  
####  <a name="bk_floor"></a> KAT  
 Belirtilen sayısal ifade küçük veya eşit en büyük tamsayı döndürür.  
  
 **Söz dizimi**  
  
```  
FLOOR (<numeric_expression>)  
```  
  
 **Bağımsız Değişkenler**  
  
-   `numeric_expression`  
  
     Sayısal bir ifadedir.  
  
 **Dönüş türleri**  
  
 Sayısal bir ifade döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, pozitif bir sayısal, negatif ve sıfır değerlerini FLOOR işlevi ile gösterilir.  
  
```  
SELECT FLOOR(123.45), FLOOR(-123.45), FLOOR(0.0)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{$1: 123, $2: -124, $3: 0}]  
```  
  
####  <a name="bk_exp"></a> EXP  
 Üstel belirtilen sayısal ifadenin değerini döndürür.  
  
 **Söz dizimi**  
  
```  
EXP (<numeric_expression>)  
```  
  
 **Bağımsız Değişkenler**  
  
-   `numeric_expression`  
  
     Sayısal bir ifadedir.  
  
 **Dönüş türleri**  
  
 Sayısal bir ifade döndürür.  
  
 **Açıklamalar**  
  
 Sabit **e** (2.718281...) Doğal logaritmanın tabanıdır.  
  
 Bir sayının üssünü sabittir **e** sayının üssü. Örneğin, EXP(1.0) = e ^ 1.0 = 2.71828182845905 ve EXP(10) = e ^ 10 = 22026.4657948067.  
  
 Bir sayının doğal logaritmasını üssü sayıdır kendisini: EXP (günlüğü (n)) = n. Ve üstel bir sayının doğal logaritmasını sayı kendisini: günlük (EXP (n)) = n.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, bir değişken bildirir ve üstel belirtilen değişkeni (10) değerini döndürür.  
  
```  
SELECT EXP(10)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{$1: 22026.465794806718}]  
```  
  
 Aşağıdaki örnek, üstel değeri 20 natural logarithm ve üstel 20 doğal logaritmasını döndürür. Bu işlevler, başka bir ters işlevler olduğundan, dönüş değeri her iki durumda kayan nokta Matematiği için yuvarlama ile 20'dir.  
  
```  
SELECT EXP(LOG(20)), LOG(EXP(20))  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{$1: 19.999999999999996, $2: 20}]  
```  
  
####  <a name="bk_log"></a> GÜNLÜK  
 Belirtilen sayısal ifadenin doğal logaritmasını döndürür.  
  
 **Söz dizimi**  
  
```  
LOG (<numeric_expression> [, <base>])  
```  
  
 **Bağımsız Değişkenler**  
  
-   `numeric_expression`  
  
     Sayısal bir ifadedir.  
  
-   `base`  
  
     Logaritmanın tabanı ayarlayan isteğe bağlı sayısal bağımsız değişken.  
  
 **Dönüş türleri**  
  
 Sayısal bir ifade döndürür.  
  
 **Açıklamalar**  
  
 Varsayılan olarak, LOG() doğal logaritmasını döndürür. Logaritmanın tabanı, isteğe bağlı temel parametresini kullanarak başka bir değere değiştirebilirsiniz.  
  
 Logaritmanın tabanı için doğal logaritmasını olan **e**burada **e** bir Irrational 2.718281828 için yaklaşık olarak eşit sabittir.  
  
 Üstel bir sayının doğal logaritmasını sayıdır kendisini: günlük (EXP (n)) = n. Ve üstel bir sayının doğal logaritma sayı kendisini: EXP (günlüğü (n)) = n.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, bir değişken bildirir ve belirtilen değişkeni (10) logaritmasını döndürür.  
  
```  
SELECT LOG(10)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{$1: 2.3025850929940459}]  
```  
  
 Aşağıdaki örnek günlük için sayının üssünü hesaplar.  
  
```  
SELECT EXP(LOG(10))  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{$1: 10.000000000000002}]  
```  
  
####  <a name="bk_log10"></a> LOG10  
 Belirtilen sayısal ifade 10 tabanında logaritmasını döndürür.  
  
 **Söz dizimi**  
  
```  
LOG10 (<numeric_expression>)  
```  
  
 **Bağımsız Değişkenler**  
  
-   `numeric_expression`  
  
     Sayısal bir ifadedir.  
  
 **Dönüş türleri**  
  
 Sayısal bir ifade döndürür.  
  
 **Açıklamalar**  
  
 LOG10 ve güç işlevleri inversely birbirleriyle ilişkilidir. Örneğin, 10 ^ LOG10(n) = n.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, bir değişken bildirir ve belirtilen değişkeni (100) LOG10 değerini döndürür.  
  
```  
SELECT LOG10(100)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{$1: 2}]  
```  
  
####  <a name="bk_pi"></a> PI  
 PI sayısının sabit değerini döndürür.  
  
 **Söz dizimi**  
  
```  
PI ()  
```  
  
 **Bağımsız Değişkenler**  
  
-   `numeric_expression`  
  
     Sayısal bir ifadedir.  
  
 **Dönüş türleri**  
  
 Sayısal bir ifade döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, PI değerini döndürür.  
  
```  
SELECT PI()  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1": 3.1415926535897931}]  
```  
  
####  <a name="bk_power"></a> GÜÇ  
 Belirtilen güç için belirtilen ifadenin değerini döndürür.  
  
 **Söz dizimi**  
  
```  
POWER (<numeric_expression>, <y>)  
```  
  
 **Bağımsız Değişkenler**  
  
-   `numeric_expression`  
  
     Sayısal bir ifadedir.  
  
-   `y`  
  
     Hangi yükseltmek güç `numeric_expression`.  
  
 **Dönüş türleri**  
  
 Sayısal bir ifade döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, bir sayı değerini 3 (numarası küp) için yükseltme gösterir.  
  
```  
SELECT POWER(2, 3), POWER(2.5, 3)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{$1: 8, $2: 15.625}]  
```  
  
####  <a name="bk_radians"></a> RADYAN CİNSİNDEN  
 Derece sayısal bir ifadenin girildiğinde radyan cinsinden döndürür.  
  
 **Söz dizimi**  
  
```  
RADIANS (<numeric_expression>)  
```  
  
 **Bağımsız Değişkenler**  
  
-   `numeric_expression`  
  
     Sayısal bir ifadedir.  
  
 **Dönüş türleri**  
  
 Sayısal bir ifade döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, birkaç açıları girdi olarak alır ve bunlara karşılık gelen radian değerler döndürür.  
  
```  
SELECT RADIANS(-45.01), RADIANS(-181.01), RADIANS(0), RADIANS(0.1472738), RADIANS(197.1099392)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{  
       "$1": -0.7855726963226477,  
       "$2": -3.1592204790349356,  
       "$3": 0,  
       "$4": 0.0025704127119236249,  
       "$5": 3.4402174274458375  
   }]  
```  
  
####  <a name="bk_round"></a> YUVARLAK  
 En yakın tamsayı değerine yuvarlanır sayısal bir değer döndürür.  
  
 **Söz dizimi**  
  
```  
ROUND(<numeric_expression>)  
```  
  
 **Bağımsız Değişkenler**  
  
-   `numeric_expression`  
  
     Sayısal bir ifadedir.  
  
 **Dönüş türleri**  
  
 Sayısal bir ifade döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, aşağıdaki pozitif ve negatif sayıları en yakın tamsayıya yuvarlar.  
  
```  
SELECT ROUND(2.4), ROUND(2.6), ROUND(2.5), ROUND(-2.4), ROUND(-2.6)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{$1: 2, $2: 3, $3: 3, $4: -2, $5: -3}]  
```  
  
####  <a name="bk_sign"></a> OTURUM  
 (+ 1) pozitif, sıfır (0) veya eksi (-1) belirtilen sayısal ifade döndürür.  
  
 **Söz dizimi**  
  
```  
SIGN(<numeric_expression>)  
```  
  
 **Bağımsız Değişkenler**  
  
-   `numeric_expression`  
  
     Sayısal bir ifadedir.  
  
 **Dönüş türleri**  
  
 Sayısal bir ifade döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnekte, -2 ' 2'ye sayıların işaretini değerleri döndürür.  
  
```  
SELECT SIGN(-2), SIGN(-1), SIGN(0), SIGN(1), SIGN(2)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{$1: -1, $2: -1, $3: 0, $4: 1, $5: 1}]  
```  
  
####  <a name="bk_sin"></a> SIN  
 Radyan cinsinden belirtilen ifade trigonometrik belirtilen bir açının sinüsünü döndürür.  
  
 **Söz dizimi**  
  
```  
SIN(<numeric_expression>)  
```  
  
 **Bağımsız Değişkenler**  
  
-   `numeric_expression`  
  
     Sayısal bir ifadedir.  
  
 **Dönüş türleri**  
  
 Sayısal bir ifade döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek belirtilen bir açının SIN hesaplar.  
  
```  
SELECT SIN(45.175643)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1": 0.929607286611012}]  
```  
  
####  <a name="bk_sqrt"></a> SQRT  
 Belirtilen sayısal değerinin kare kökünü döndürür.  
  
 **Söz dizimi**  
  
```  
SQRT(<numeric_expression>)  
```  
  
 **Bağımsız Değişkenler**  
  
-   `numeric_expression`  
  
     Sayısal bir ifadedir.  
  
 **Dönüş türleri**  
  
 Sayısal bir ifade döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek sayılar 1-3 kareköklerini döndürür.  
  
```  
SELECT SQRT(1), SQRT(2.0), SQRT(3)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{$1: 1, $2: 1.4142135623730952, $3: 1.7320508075688772}]  
```  
  
####  <a name="bk_square"></a> KARE  
 Belirtilen bir sayısal değer karesini döndürür.  
  
 **Söz dizimi**  
  
```  
SQUARE(<numeric_expression>)  
```  
  
 **Bağımsız Değişkenler**  
  
-   `numeric_expression`  
  
     Sayısal bir ifadedir.  
  
 **Dönüş türleri**  
  
 Sayısal bir ifade döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, 1-3 sayıların kareler döndürür.  
  
```  
SELECT SQUARE(1), SQUARE(2.0), SQUARE(3)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{$1: 1, $2: 4, $3: 9}]  
```  
  
####  <a name="bk_tan"></a> TAN  
 Radyan cinsinden belirtilen ifadesi belirtilen bir açının tanjantını döndürür.  
  
 **Söz dizimi**  
  
```  
TAN (<numeric_expression>)  
```  
  
 **Bağımsız Değişkenler**  
  
-   `numeric_expression`  
  
     Sayısal bir ifadedir.  
  
 **Dönüş türleri**  
  
 Sayısal bir ifade döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, PI () tanjantını hesaplar / 2.  
  
```  
SELECT TAN(PI()/2);  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1": 16331239353195370 }]  
```  
  
####  <a name="bk_trunc"></a> TRUNC  
 En yakın tamsayı değerine kesilmiş sayısal bir değer döndürür.  
  
 **Söz dizimi**  
  
```  
TRUNC(<numeric_expression>)  
```  
  
 **Bağımsız Değişkenler**  
  
-   `numeric_expression`  
  
     Sayısal bir ifadedir.  
  
 **Dönüş türleri**  
  
 Sayısal bir ifade döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, aşağıdaki pozitif ve negatif sayıları en yakın tamsayı değerine keser.  
  
```  
SELECT TRUNC(2.4), TRUNC(2.6), TRUNC(2.5), TRUNC(-2.4), TRUNC(-2.6)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{$1: 2, $2: 2, $3: 2, $4: -2, $5: -2}]  
```  
  
###  <a name="bk_type_checking_functions"></a> Tür denetimini işlevleri  
 Tür denetimini karşı giriş değerleri aşağıdaki işlevleri destekler ve her bir Boole değeri döndürür.  
  
||||  
|-|-|-|  
|[IS_ARRAY](#bk_is_array)|[IS_BOOL](#bk_is_bool)|[IS_DEFINED](#bk_is_defined)|  
|[IS_NULL](#bk_is_null)|[IS_NUMBER](#bk_is_number)|[IS_OBJECT](#bk_is_object)|  
|[IS_PRIMITIVE](#bk_is_primitive)|[IS_STRING](#bk_is_string)||  
  
####  <a name="bk_is_array"></a> IS_ARRAY  
 Belirtilen ifade türü bir dizi olup olmadığını gösteren bir Boole değeri döndürür.  
  
 **Söz dizimi**  
  
```  
IS_ARRAY(<expression>)  
```  
  
 **Bağımsız Değişkenler**  
  
-   `expression`  
  
     Herhangi bir geçerli ifade var.  
  
 **Dönüş türleri**  
  
 Bir Boolean ifadesi döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek JSON Boolean, sayı, dize, null Nesne, nesne, dizi ve IS_ARRAY işlevi kullanarak tanımlanmamış türler denetler.  
  
```  
SELECT   
 IS_ARRAY(true),   
 IS_ARRAY(1),  
 IS_ARRAY("value"),  
 IS_ARRAY(null),  
 IS_ARRAY({prop: "value"}),   
 IS_ARRAY([1, 2, 3]),  
 IS_ARRAY({prop: "value"}.prop2)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{$1: false, $2: false, $3: false, $4: false, $5: false, $6: true}]  
```  
  
####  <a name="bk_is_bool"></a> IS_BOOL  
 Belirtilen ifade türünü bir Boole değeri olup olmadığını gösteren bir Boole değeri döndürür.  
  
 **Söz dizimi**  
  
```  
IS_BOOL(<expression>)  
```  
  
 **Bağımsız Değişkenler**  
  
-   `expression`  
  
     Herhangi bir geçerli ifade var.  
  
 **Dönüş türleri**  
  
 Bir Boolean ifadesi döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek JSON Boolean, sayı, dize, null Nesne, nesne, dizi ve IS_BOOL işlevi kullanarak tanımlanmamış türler denetler.  
  
```  
SELECT   
    IS_BOOL(true),   
    IS_BOOL(1),  
    IS_BOOL("value"),   
    IS_BOOL(null),  
    IS_BOOL({prop: "value"}),   
    IS_BOOL([1, 2, 3]),  
    IS_BOOL({prop: "value"}.prop2)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{$1: true, $2: false, $3: false, $4: false, $5: false, $6: false}]  
```  
  
####  <a name="bk_is_defined"></a> IS_DEFINED  
 Özellik değeri atanıp atanmadığını gösteren bir Boole değeri döndürür.  
  
 **Söz dizimi**  
  
```  
IS_DEFINED(<expression>)  
```  
  
 **Bağımsız Değişkenler**  
  
-   `expression`  
  
     Herhangi bir geçerli ifade var.  
  
 **Dönüş türleri**  
  
 Bir Boolean ifadesi döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, bir özelliği belirtilen JSON belgesi içinde varlığını denetler. İlk "a" var, ancak "b" eksik olduğundan ikinci false döndürürse bu yana true döndürür.  
  
```  
SELECT IS_DEFINED({ "a" : 5 }.a), IS_DEFINED({ "a" : 5 }.b)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{  
       "$1": true,    
       "$2": false   
   }]  
```  
  
####  <a name="bk_is_null"></a> IS_NULL  
 Belirtilen ifadenin türü null olup olmadığını gösteren bir Boole değeri döndürür.  
  
 **Söz dizimi**  
  
```  
IS_NULL(<expression>)  
```  
  
 **Bağımsız Değişkenler**  
  
-   `expression`  
  
     Herhangi bir geçerli ifade var.  
  
 **Dönüş türleri**  
  
 Bir Boolean ifadesi döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek JSON Boolean, sayı, dize, null Nesne, nesne, dizi ve IS_NULL işlevi kullanarak tanımlanmamış türler denetler.  
  
```  
SELECT   
    IS_NULL(true),   
    IS_NULL(1),  
    IS_NULL("value"),   
    IS_NULL(null),  
    IS_NULL({prop: "value"}),   
    IS_NULL([1, 2, 3]),  
    IS_NULL({prop: "value"}.prop2)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{$1: false, $2: false, $3: false, $4: true, $5: false, $6: false}]  
```  
  
####  <a name="bk_is_number"></a> IS_NUMBER  
 Belirtilen ifade türünü bir sayı olup olmadığını gösteren bir Boole değeri döndürür.  
  
 **Söz dizimi**  
  
```  
IS_NUMBER(<expression>)  
```  
  
 **Bağımsız Değişkenler**  
  
-   `expression`  
  
     Herhangi bir geçerli ifade var.  
  
 **Dönüş türleri**  
  
 Bir Boolean ifadesi döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek JSON Boolean, sayı, dize, null Nesne, nesne, dizi ve IS_NULL işlevi kullanarak tanımlanmamış türler denetler.  
  
```  
SELECT   
    IS_NUMBER(true),   
    IS_NUMBER(1),  
    IS_NUMBER("value"),   
    IS_NUMBER(null),  
    IS_NUMBER({prop: "value"}),   
    IS_NUMBER([1, 2, 3]),  
    IS_NUMBER({prop: "value"}.prop2)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{$1: false, $2: true, $3: false, $4: false, $5: false, $6: false}]  
```  
  
####  <a name="bk_is_object"></a> IS_OBJECT  
 Belirtilen ifade türünü bir JSON nesnesi olup olmadığını gösteren bir Boole değeri döndürür.  
  
 **Söz dizimi**  
  
```  
IS_OBJECT(<expression>)  
```  
  
 **Bağımsız Değişkenler**  
  
-   `expression`  
  
     Herhangi bir geçerli ifade var.  
  
 **Dönüş türleri**  
  
 Bir Boolean ifadesi döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek JSON Boolean, sayı, dize, null Nesne, nesne, dizi ve IS_OBJECT işlevi kullanarak tanımlanmamış türler denetler.  
  
```  
SELECT   
    IS_OBJECT(true),   
    IS_OBJECT(1),  
    IS_OBJECT("value"),   
    IS_OBJECT(null),  
    IS_OBJECT({prop: "value"}),   
    IS_OBJECT([1, 2, 3]),  
    IS_OBJECT({prop: "value"}.prop2)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{$1: false, $2: false, $3: false, $4: false, $5: true, $6: false}]  
```  
  
####  <a name="bk_is_primitive"></a> IS_PRIMITIVE  
 Belirtilen ifadenin türü basit bir tür olup olmadığını gösteren bir Boole değeri döndürür (, Boole, sayısal ya da boş dize).  
  
 **Söz dizimi**  
  
```  
IS_PRIMITIVE(<expression>)  
```  
  
 **Bağımsız Değişkenler**  
  
-   `expression`  
  
     Herhangi bir geçerli ifade var.  
  
 **Dönüş türleri**  
  
 Bir Boolean ifadesi döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek JSON Boolean, sayı, dize, null Nesne, nesne, dizi ve IS_PRIMITIVE işlevi kullanarak tanımlanmamış türler denetler.  
  
```  
SELECT   
           IS_PRIMITIVE(true),   
           IS_PRIMITIVE(1),  
           IS_PRIMITIVE("value"),   
           IS_PRIMITIVE(null),  
           IS_PRIMITIVE({prop: "value"}),   
           IS_PRIMITIVE([1, 2, 3]),  
           IS_PRIMITIVE({prop: "value"}.prop2)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1": true, "$2": true, "$3": true, "$4": true, "$5": false, "$6": false, "$7": false}]  
```  
  
####  <a name="bk_is_string"></a> IS_STRING  
 Belirtilen ifadenin türü dize olup olmadığını gösteren bir Boole değeri döndürür.  
  
 **Söz dizimi**  
  
```  
IS_STRING(<expression>)  
```  
  
 **Bağımsız Değişkenler**  
  
-   `expression`  
  
     Herhangi bir geçerli ifade var.  
  
 **Dönüş türleri**  
  
 Bir Boolean ifadesi döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek JSON Boolean, sayı, dize, null Nesne, nesne, dizi ve IS_STRING işlevi kullanarak tanımlanmamış türler denetler.  
  
```  
SELECT   
       IS_STRING(true),   
       IS_STRING(1),  
       IS_STRING("value"),   
       IS_STRING(null),  
       IS_STRING({prop: "value"}),   
       IS_STRING([1, 2, 3]),  
       IS_STRING({prop: "value"}.prop2)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{$1: false, $2: false, $3: true, $4: false, $5: false, $6: false}]  
```  
  
###  <a name="bk_string_functions"></a> Dize işlevleri  
 Aşağıdaki skaler İşlevler, bir dize giriş değeri bir işlem gerçekleştirmek ve bir dize, sayısal veya Boolean değeri döndürür.  
  
||||  
|-|-|-|  
|[CONCAT](#bk_concat)|[İÇERİR](#bk_contains)|[ENDSWITH](#bk_endswith)|  
|[INDEX_OF](#bk_index_of)|[SOL](#bk_left)|[UZUNLUĞU](#bk_length)|  
|[DAHA DÜŞÜK](#bk_lower)|[LTRIM](#bk_ltrim)|[DEĞİŞTİR](#bk_replace)|  
|[ÇOĞALTILAN](#bk_replicate)|[GERİYE DOĞRU](#bk_reverse)|[SAĞ](#bk_right)|  
|[RTRIM](#bk_rtrim)|[STARTSWITH](#bk_startswith)|[ALT DİZE](#bk_substring)|  
|[ToString](#bk_tostring)|[KIRPMA](#bk_trim)|[ÜST](#bk_upper)||| 
  
####  <a name="bk_concat"></a> CONCAT  
 İki veya daha fazla dize değerlerini birleştirirken sonucu olan bir dize döndürür.  
  
 **Söz dizimi**  
  
```  
CONCAT(<str_expr>, <str_expr> [, <str_expr>])  
```  
  
 **Bağımsız Değişkenler**  
  
-   `str_expr`  
  
     Herhangi bir geçerli dize ifade var.  
  
 **Dönüş türleri**  
  
 Bir dize ifadesi döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, belirtilen değerler birleştirilmiş dizeyi döndürür.  
  
```  
SELECT CONCAT("abc", "def")  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1": "abcdef"}  
```  
  
####  <a name="bk_contains"></a> İÇERİR  
 Döndürür bir Boolean gösteren ikinci ilk dize ifade olup olmadığını içerir.  
  
 **Söz dizimi**  
  
```  
CONTAINS(<str_expr>, <str_expr>)  
```  
  
 **Bağımsız Değişkenler**  
  
-   `str_expr`  
  
     Herhangi bir geçerli dize ifade var.  
  
 **Dönüş türleri**  
  
 Bir Boolean ifadesi döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, "abc" "ab" ve "d" içerir, denetler.  
  
```  
SELECT CONTAINS("abc", "ab"), CONTAINS("abc", "d")  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1": true, "$2": false}]  
```  
  
####  <a name="bk_endswith"></a> ENDSWITH  
 Boole döndürüp döndüremeyeceğini belirten döndürür ilk dize ifade olup olmadığını ve ikinci sona erer.  
  
 **Söz dizimi**  
  
```  
ENDSWITH(<str_expr>, <str_expr>)  
```  
  
 **Bağımsız Değişkenler**  
  
-   `str_expr`  
  
     Herhangi bir geçerli dize ifade var.  
  
 **Dönüş türleri**  
  
 Bir Boolean ifadesi döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, "abc", "b" ve "bc" ile biten döndürür.  
  
```  
SELECT ENDSWITH("abc", "b"), ENDSWITH("abc", "bc")  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1": false, "$2": true}]  
```  
  
####  <a name="bk_index_of"></a> INDEX_OF  
 İkinci dizenin başlangıç konumunu döndürür dize bulunamazsa, ilk belirtilen dize ifadesi veya -1 içindeki ifadenin dize.  
  
 **Söz dizimi**  
  
```  
INDEX_OF(<str_expr>, <str_expr>)  
```  
  
 **Bağımsız Değişkenler**  
  
-   `str_expr`  
  
     Herhangi bir geçerli dize ifade var.  
  
 **Dönüş türleri**  
  
 Sayısal bir ifade döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, "abc" içindeki çeşitli alt dizeler dizinini döndürür.  
  
```  
SELECT INDEX_OF("abc", "ab"), INDEX_OF("abc", "b"), INDEX_OF("abc", "c")  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1": 0, "$2": 1, "$3": -1}]  
```  
  
####  <a name="bk_left"></a> SOL  
 Belirtilen sayıda karakteri içeren bir dize sol bölümünü döndürür.  
  
 **Söz dizimi**  
  
```  
LEFT(<str_expr>, <num_expr>)  
```  
  
 **Bağımsız Değişkenler**  
  
-   `str_expr`  
  
     Herhangi bir geçerli dize ifade var.  
  
-   `num_expr`  
  
     Geçerli bir sayısal ifade var.  
  
 **Dönüş türleri**  
  
 Bir dize ifadesi döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, çeşitli uzunluğu değerler için sol "abc" bölümünü döndürür.  
  
```  
SELECT LEFT("abc", 1), LEFT("abc", 2)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1": "a", "$2": "ab"}]  
```  
  
####  <a name="bk_length"></a> UZUNLUĞU  
 Belirtilen dize ifadesinin karakter sayısını döndürür.  
  
 **Söz dizimi**  
  
```  
LENGTH(<str_expr>)  
```  
  
 **Bağımsız Değişkenler**  
  
-   `str_expr`  
  
     Herhangi bir geçerli dize ifade var.  
  
 **Dönüş türleri**  
  
 Bir dize ifadesi döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek bir dizenin uzunluğunu döndürür.  
  
```  
SELECT LENGTH("abc")  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1": 3}]  
```  
  
####  <a name="bk_lower"></a> DAHA DÜŞÜK  
 Büyük harf karakter verileri küçük harfe dönüştürmenin sonra bir dize ifadesi döndürür.  
  
 **Söz dizimi**  
  
```  
LOWER(<str_expr>)  
```  
  
 **Bağımsız Değişkenler**  
  
-   `str_expr`  
  
     Herhangi bir geçerli dize ifade var.  
  
 **Dönüş türleri**  
  
 Bir dize ifadesi döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek sorguda DÜŞÜREBİLİRSİNİZ kullanmayı gösterir.  
  
```  
SELECT LOWER("Abc")  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1": "abc"}]  
  
```  
  
####  <a name="bk_ltrim"></a> LTRIM  
 Baştaki boşluklar kaldırdıktan sonra bir dize ifadesi döndürür.  
  
 **Söz dizimi**  
  
```  
LTRIM(<str_expr>)  
```  
  
 **Bağımsız Değişkenler**  
  
-   `str_expr`  
  
     Herhangi bir geçerli dize ifade var.  
  
 **Dönüş türleri**  
  
 Bir dize ifadesi döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, bir sorgu içinde LTRIM kullanmayı gösterir.  
  
```  
SELECT LTRIM("  abc"), LTRIM("abc"), LTRIM("abc   ")  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1": "abc", "$2": "abc", "$3": "abc   "}]  
```  
  
####  <a name="bk_replace"></a> DEĞİŞTİR  
 Belirtilen dize değeri tüm oluşumlarını başka bir dize değeri ile değiştirir.  
  
 **Söz dizimi**  
  
```  
REPLACE(<str_expr>, <str_expr>, <str_expr>)  
```  
  
 **Bağımsız Değişkenler**  
  
-   `str_expr`  
  
     Herhangi bir geçerli dize ifade var.  
  
 **Dönüş türleri**  
  
 Bir dize ifadesi döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, bir sorgu Değiştir kullanmayı gösterir.  
  
```  
SELECT REPLACE("This is a Test", "Test", "desk")  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1": "This is a desk"}]  
```  
  
####  <a name="bk_replicate"></a> ÇOĞALTMA  
 Bir dize değeri, belirtilen sayıda yineler.  
  
 **Söz dizimi**  
  
```  
REPLICATE(<str_expr>, <num_expr>)  
```  
  
 **Bağımsız Değişkenler**  
  
-   `str_expr`  
  
     Herhangi bir geçerli dize ifade var.  
  
-   `num_expr`  
  
     Geçerli bir sayısal ifade var.  
  
 **Dönüş türleri**  
  
 Bir dize ifadesi döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, bir sorgu çoğaltma kullanmayı gösterir.  
  
```  
SELECT REPLICATE("a", 3)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1": "aaa"}]  
```  
  
####  <a name="bk_reverse"></a> GERİYE DOĞRU  
 Bir dize değerinin ters sırada döndürür.  
  
 **Söz dizimi**  
  
```  
REVERSE(<str_expr>)  
```  
  
 **Bağımsız Değişkenler**  
  
-   `str_expr`  
  
     Herhangi bir geçerli dize ifade var.  
  
 **Dönüş türleri**  
  
 Bir dize ifadesi döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, bir sorgu ters kullanmayı gösterir.  
  
```  
SELECT REVERSE("Abc")  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1": "cbA"}]  
```  
  
####  <a name="bk_right"></a> SAĞ  
 Belirtilen sayıda karakteri içeren bir dize sağ bölümünü döndürür.  
  
 **Söz dizimi**  
  
```  
RIGHT(<str_expr>, <num_expr>)  
```  
  
 **Bağımsız Değişkenler**  
  
-   `str_expr`  
  
     Herhangi bir geçerli dize ifade var.  
  
-   `num_expr`  
  
     Geçerli bir sayısal ifade var.  
  
 **Dönüş türleri**  
  
 Bir dize ifadesi döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, çeşitli uzunluğu değerleri için "abc" sağ bölümünü döndürür.  
  
```  
SELECT RIGHT("abc", 1), RIGHT("abc", 2)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1": "c", "$2": "bc"}]  
```  
  
####  <a name="bk_rtrim"></a> RTRIM  
 Sonundaki boşlukları kaldırdıktan sonra bir dize ifadesi döndürür.  
  
 **Söz dizimi**  
  
```  
RTRIM(<str_expr>)  
```  
  
 **Bağımsız Değişkenler**  
  
-   `str_expr`  
  
     Herhangi bir geçerli dize ifade var.  
  
 **Dönüş türleri**  
  
 Bir dize ifadesi döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, bir sorgu içinde RTRIM kullanmayı gösterir.  
  
```  
SELECT RTRIM("  abc"), RTRIM("abc"), RTRIM("abc   ")  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1": "   abc", "$2": "abc", "$3": "abc"}]  
```  
  
####  <a name="bk_startswith"></a> STARTSWITH  
 Boole döndürüp döndüremeyeceğini belirten döndürür ilk dize ifade olup olmadığını ve ikinci başlatır.  
  
 **Söz dizimi**  
  
```  
STARTSWITH(<str_expr>, <str_expr>)  
```  
  
 **Bağımsız Değişkenler**  
  
-   `str_expr`  
  
     Herhangi bir geçerli dize ifade var.  
  
 **Dönüş türleri**  
  
 Bir Boolean ifadesi döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, "abc" dize "b" ile başlayıp başlamadığını denetler ve "a".  
  
```  
SELECT STARTSWITH("abc", "b"), STARTSWITH("abc", "a")  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1": false, "$2": true}]  
```  
  
####  <a name="bk_substring"></a> ALT DİZE  
 Belirtilen karakterin sıfır tabanlı konumunda başlayan bir dize ifadesi bölümünü döndürür ve belirtilen uzunlukta veya dizenin sonuna kadar devam eder.  
  
 **Söz dizimi**  
  
```  
SUBSTRING(<str_expr>, <num_expr> [, <num_expr>])  
```  
  
 **Bağımsız Değişkenler**  
  
-   `str_expr`  
  
     Herhangi bir geçerli dize ifade var.  
  
-   `num_expr`  
  
     Geçerli bir sayısal ifade var.  
  
 **Dönüş türleri**  
  
 Bir dize ifadesi döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, "abc" alt 1 ve 1 karakter uzunluğunu başlangıç döndürür.  
  
```  
SELECT SUBSTRING("abc", 1, 1)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1": "b"}]  
```  
####  <a name="bk_tostring"></a> ToString  
 Skaler ifade bir dize gösterimini döndürür. 
  
 **Söz dizimi**  
  
```  
ToString(<expr>)
```  
  
 **Bağımsız Değişkenler**  
  
-   `expr`  
  
     Geçerli bir skaler ifade var.  
  
 **Dönüş türleri**  
  
 Bir dize ifadesi döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, ToString farklı türleri arasında nasıl davranacağını gösterir.   
  
```  
SELECT ToString(1.0000), ToString("Hello World"), ToString(NaN), ToString(Infinity),
ToString(IS_STRING(ToString(undefined))), IS_STRING(ToString(0.1234), ToString(false), ToString(undefined))
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1": "1", "$2": "Hello World", "$3": "NaN", "$4": "Infinity", "$5": "false", "$6": true, "$7": "false"}]  
```  
 Şu giriş verilen:
```  
{"Products":[{"ProductID":1,"Weight":4,"WeightUnits":"lb"},{"ProductID":2,"Weight":32,"WeightUnits":"kg"},{"ProductID":3,"Weight":400,"WeightUnits":"g"},{"ProductID":4,"Weight":8999,"WeightUnits":"mg"}]}
```    
 Aşağıdaki örnek, diğer CONCAT gibi dize işlevlerle ToString'ın nasıl kullanılabileceğini gösterir.   
 
```  
SELECT 
CONCAT(ToString(p.Weight), p.WeightUnits) 
FROM p in c.Products 
```  

 Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1":"4lb" },
 {"$1":"32kg"},
 {"$1":"400g" },
 {"$1":"8999mg" }]

```  
Şu giriş verilir.
```
{"id":"08259","description":"Cereals ready-to-eat, KELLOGG, KELLOGG'S CRISPIX","nutrients":[{"id":"305","description":"Caffeine","units":"mg"},{"id":"306","description":"Cholesterol, HDL","nutritionValue":30,"units":"mg"},{"id":"307","description":"Sodium, NA","nutritionValue":612,"units":"mg"},{"id":"308","description":"Protein, ABP","nutritionValue":60,"units":"mg"},{"id":"309","description":"Zinc, ZN","nutritionValue":null,"units":"mg"}]}
```
 Aşağıdaki örnek, diğer değiştirme gibi dize işlevlerle ToString'ın nasıl kullanılabileceğini gösterir.   
```
SELECT 
    n.id AS nutrientID,
    REPLACE(ToString(n.nutritionValue), "6", "9") AS nutritionVal
FROM food 
JOIN n IN food.nutrients
```
 Sonuç kümesini burada verilmiştir.  
 ```
[{"nutrientID":"305"},
{"nutrientID":"306","nutritionVal":"30"},
{"nutrientID":"307","nutritionVal":"912"},
{"nutrientID":"308","nutritionVal":"90"},
{"nutrientID":"309","nutritionVal":"null"}]
 ``` 
 
####  <a name="bk_trim"></a> KIRPMA  
 Baştaki ve sondaki boşlukları kaldırır sonra bir dize ifadesi döndürür.  
  
 **Söz dizimi**  
  
```  
TRIM(<str_expr>)  
```  
  
 **Bağımsız Değişkenler**  
  
-   `str_expr`  
  
     Herhangi bir geçerli dize ifade var.  
  
 **Dönüş türleri**  
  
 Bir dize ifadesi döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, bir sorgu içinde TRIM kullanmayı gösterir.  
  
```  
SELECT TRIM("   abc"), TRIM("   abc   "), TRIM("abc   "), TRIM("abc")   
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1": "abc", "$2": "abc", "$3": "abc", "$4": "abc"}]  
``` 
####  <a name="bk_upper"></a> ÜST  
 Küçük harf karakter verileri büyük harfe dönüştürmenin sonra bir dize ifadesi döndürür.  
  
 **Söz dizimi**  
  
```  
UPPER(<str_expr>)  
```  
  
 **Bağımsız Değişkenler**  
  
-   `str_expr`  
  
     Herhangi bir geçerli dize ifade var.  
  
 **Dönüş türleri**  
  
 Bir dize ifadesi döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek sorguda üst kullanma işlemi gösterilmektedir  
  
```  
SELECT UPPER("Abc")  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1": "ABC"}]  
```  
  
###  <a name="bk_array_functions"></a> Dizi işlevleri  
 Aşağıdaki skaler işlevler bir dizi giriş değeri ve dönüş sayısal, Boole veya dizi değeri üzerinde bir işlem gerçekleştirme  
  
||||  
|-|-|-|  
|[ARRAY_CONCAT](#bk_array_concat)|[ARRAY_CONTAINS](#bk_array_contains)|[ARRAY_LENGTH](#bk_array_length)|  
|[ARRAY_SLICE](#bk_array_slice)|||  
  
####  <a name="bk_array_concat"></a> ARRAY_CONCAT  
 İki veya daha fazla dizi değerlerini birleştirirken sonucu olan bir dizi döndürür.  
  
 **Söz dizimi**  
  
```  
ARRAY_CONCAT (<arr_expr>, <arr_expr> [, <arr_expr>])  
```  
  
 **Bağımsız Değişkenler**  
  
-   `arr_expr`  
  
     Tüm geçerli dizi ifadesidir.  
  
 **Dönüş türleri**  
  
 Bir dizi ifadesi döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek iki diziyi birleştirme.  
  
```  
SELECT ARRAY_CONCAT(["apples", "strawberries"], ["bananas"])  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1": ["apples", "strawberries", "bananas"]}]  
```  
  
####  <a name="bk_array_contains"></a> ARRAY_CONTAINS  
Dizi belirtilen değeri içerip içermediğini gösteren bir Boole değeri döndürür. Tam veya kısmi eşleşme olup olmadığını belirtebilirsiniz. 

 **Söz dizimi**  
  
```  
ARRAY_CONTAINS (<arr_expr>, <expr> [, bool_expr])  
```  
  
 **Bağımsız Değişkenler**  
  
-   `arr_expr`  
  
     Tüm geçerli dizi ifadesidir.  
  
-   `expr`  
  
     Herhangi bir geçerli ifade var.  

-   `bool_expr`  
  
     Herhangi bir boolean ifadesi var.       
  
 **Dönüş türleri**  
  
 Bir Boolean değer döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek nasıl ARRAY_CONTAINS kullanarak bir diziye üyeliğini denetleyin.  
  
```  
SELECT   
           ARRAY_CONTAINS(["apples", "strawberries", "bananas"], "apples"),  
           ARRAY_CONTAINS(["apples", "strawberries", "bananas"], "mangoes")  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1": true, "$2": false}]  
```  

 Aşağıdaki örnek nasıl bir JSON dizi ARRAY_CONTAINS kısmi bir eşleşme olup olmadığını denetleyin.  
  
```  
SELECT  
    ARRAY_CONTAINS([{"name": "apples", "fresh": true}, {"name": "strawberries", "fresh": true}], {"name": "apples"}, true), 
    ARRAY_CONTAINS([{"name": "apples", "fresh": true}, {"name": "strawberries", "fresh": true}], {"name": "apples"}),
    ARRAY_CONTAINS([{"name": "apples", "fresh": true}, {"name": "strawberries", "fresh": true}], {"name": "mangoes"}, true) 
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{
  "$1": true,
  "$2": false,
  "$3": false
}] 
```  
  
####  <a name="bk_array_length"></a> ARRAY_LENGTH  
 Belirtilen bir dizi ifadesinin öğelerin sayısını döndürür.  
  
 **Söz dizimi**  
  
```  
ARRAY_LENGTH(<arr_expr>)  
```  
  
 **Bağımsız Değişkenler**  
  
-   `arr_expr`  
  
     Tüm geçerli dizi ifadesidir.  
  
 **Dönüş türleri**  
  
 Sayısal bir ifade döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek nasıl ARRAY_LENGTH kullanarak bir dizinin uzunluğu.  
  
```  
SELECT ARRAY_LENGTH(["apples", "strawberries", "bananas"])  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1": 3}]  
```  
  
####  <a name="bk_array_slice"></a> ARRAY_SLICE  
 Bir dizi ifadesi bölümünü döndürür.
  
 **Söz dizimi**  
  
```  
ARRAY_SLICE (<arr_expr>, <num_expr> [, <num_expr>])  
```  
  
 **Bağımsız Değişkenler**  
  
-   `arr_expr`  
  
     Tüm geçerli dizi ifadesidir.  
  
-   `num_expr`  
  
     Dizi başlanan sıfır tabanlı sayısal dizini. Negatif değerler dizisi yani -1 başvuruları son öğesi göreli başlangıç dizini dizideki son öğeyi belirtmek için kullanılabilir.  

-   `num_expr`  

     Sonuçta elde edilen dizideki öğelerin sayısı.    

 **Dönüş türleri**  
  
 Bir dizi ifadesi döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, ARRAY_SLICE kullanarak bir dizi farklı dilim alma gösterilmektedir.  
  
```  
SELECT   
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1),  
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1, 1),
           ARRAY_SLICE(["apples", "strawberries", "bananas"], -2, 1),
           ARRAY_SLICE(["apples", "strawberries", "bananas"], -2, 2),
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1, 0),
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1, 1000),
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1, -100)      
  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{  
           "$1": ["strawberries", "bananas"],   
           "$2": ["strawberries"],
           "$3": ["strawberries"],  
           "$4": ["strawberries", "bananas"], 
           "$5": [],
           "$6": ["strawberries", "bananas"],
           "$7": [] 
}]  
```  
 
###  <a name="bk_spatial_functions"></a> Uzamsal İşlevler  
 Aşağıdaki skaler İşlevler, uzamsal nesne giriş değeri bir işlem gerçekleştirmek ve bir sayısal veya Boolean değeri döndürür.  
  
||||  
|-|-|-|  
|[ST_DISTANCE](#bk_st_distance)|[ST_WITHIN](#bk_st_within)|[ST_INTERSECTS](#bk_st_intersects)|[ST_ISVALID](#bk_st_isvalid)|  
|[ST_ISVALIDDETAILED](#bk_st_isvaliddetailed)|||  
  
####  <a name="bk_st_distance"></a> ST_DISTANCE  
 İki GeoJSON noktası, çokgen veya LineString ifadeler uzaklığı döndürür.  
  
 **Söz dizimi**  
  
```  
ST_DISTANCE (<spatial_expr>, <spatial_expr>)  
```  
  
 **Bağımsız Değişkenler**  
  
-   `spatial_expr`  
  
     Tüm geçerli GeoJSON noktası, çokgen veya LineString nesne ifadesidir.  
  
 **Dönüş türleri**  
  
 Uzaklık içeren sayısal bir ifade döndürür. Bu varsayılan başvuru sistemi için ölçümleri ifade edilir.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, ST_DISTANCE yerleşik işlevi kullanarak belirtilen konumun içinde 30 KM tüm ailesi belgeler döndürülecek gösterilmektedir. .  
  
```  
SELECT f.id   
FROM Families f   
WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{  
  "id": "WakefieldFamily"  
}]  
```  
  
####  <a name="bk_st_within"></a> ST_WITHIN  
 İlk bağımsız değişkende belirtilen GeoJSON nesne (noktası, çokgen veya LineString) ikinci bağımsız değişkende GeoJSON (noktası, çokgen veya LineString) içinde olup olmadığını gösteren bir Boole ifadesi döndürür.  
  
 **Söz dizimi**  
  
```  
ST_WITHIN (<spatial_expr>, <spatial_expr>)  
```  
  
 **Bağımsız Değişkenler**  
  
-   `spatial_expr`  
  
     Tüm geçerli GeoJSON noktası, çokgen veya LineString nesne ifadesidir.  
 
-   `spatial_expr`  
  
     Tüm geçerli GeoJSON noktası, çokgen veya LineString nesne ifadesidir.  
  
 **Dönüş türleri**  
  
 Bir Boolean değer döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, tüm ailesi bulmanın ST_WITHIN kullanarak bir Çokgen içinde gösterilmektedir.  
  
```  
SELECT f.id   
FROM Families f   
WHERE ST_WITHIN(f.location, {  
    'type':'Polygon',   
    'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]  
})  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{ "id": "WakefieldFamily" }]  
```  

####  <a name="bk_st_intersects"></a> ST_INTERSECTS  
 İlk bağımsız değişkende belirtilen GeoJSON nesne (noktası, çokgen veya LineString) ikinci bağımsız değişkende GeoJSON (noktası, çokgen veya LineString) kesişip kesişmediğini belirten bir Boole ifadesi döndürür.  
  
 **Söz dizimi**  
  
```  
ST_INTERSECTS (<spatial_expr>, <spatial_expr>)  
```  
  
 **Bağımsız Değişkenler**  
  
-   `spatial_expr`  
  
     Tüm geçerli GeoJSON noktası, çokgen veya LineString nesne ifadesidir.  
 
-   `spatial_expr`  
  
     Tüm geçerli GeoJSON noktası, çokgen veya LineString nesne ifadesidir.  
  
 **Dönüş türleri**  
  
 Bir Boolean değer döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, ile belirtilen Çokgen kesişen tüm alanları gösterilmektedir.  
  
```  
SELECT a.id   
FROM Areas a   
WHERE ST_INTERSECTS(a.location, {  
    'type':'Polygon',   
    'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]  
})  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{ "id": "IntersectingPolygon" }]  
```  
  
####  <a name="bk_st_isvalid"></a> ST_ISVALID  
 Belirtilen GeoJSON noktası, çokgen veya LineString ifade geçerli olup olmadığını gösteren bir Boole değeri döndürür.  
  
 **Söz dizimi**  
  
```  
ST_ISVALID(<spatial_expr>)  
```  
  
 **Bağımsız Değişkenler**  
  
-   `spatial_expr`  
  
     Herhangi bir geçerli GeoJSON noktası, çokgen veya LineString ifade var.  
  
 **Dönüş türleri**  
  
 Bir Boolean ifadesi döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, bir nokta ST_VALID kullanarak geçerli olup olmadığını denetlemek gösterilmektedir.  
  
 Örneğin, bu nokta geçerli değerler [-90, 90] aralığında böylece sorgu döndürür false değil bir enlem değeri vardır.  
  
 Çokgen için GeoJSON belirtimi sağlanan son koordinat çifti kapalı şekli oluşturmak için birinci ile aynı olması gerekir. İçinde bir Çokgen noktalarının saat yönünün tersi düzende belirtilmesi gerekir. Bir çokgenin belirtilen saat yönünde sırayla bölgesinde tersini temsil eder.  
  
```  
SELECT ST_ISVALID({ "type": "Point", "coordinates": [31.9, -132.8] })  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{ "$1": false }]  
```  
  
####  <a name="bk_st_isvaliddetailed"></a> ST_ISVALIDDETAILED  
 Bir Boole değeri içeren bir JSON değeri, belirtilen GeoJSON noktası, çokgen veya LineString ifade geçerliyse ve geçersiz değeri döndürür, ayrıca bir dize değeri olarak nedeni.  
  
 **Söz dizimi**  
  
```  
ST_ISVALID(<spatial_expr>)  
```  
  
 **Bağımsız Değişkenler**  
  
-   `spatial_expr`  
  
     Tüm geçerli GeoJSON noktası veya Çokgen ifadesidir.  
  
 **Dönüş türleri**  
  
 Bir Boole değeri içeren bir JSON değeri, belirtilen GeoJSON noktası veya Çokgen ifade geçerliyse ve geçersiz değeri döndürür, ayrıca bir dize değeri olarak nedeni.  
  
 **Örnekler**  
  
 Aşağıdaki örnek nasıl (Ayrıntılar ile) ST_ISVALIDDETAILED kullanarak geçerliliğini denetlemek.  
  
```  
SELECT ST_ISVALIDDETAILED({   
  "type": "Polygon",   
  "coordinates": [[ [ 31.8, -5 ], [ 31.8, -4.7 ], [ 32, -4.7 ], [ 32, -5 ] ]]  
})  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{  
  "$1": {   
    "valid": false,   
    "reason": "The Polygon input is not valid because the start and end points of the ring number 1 are not the same. Each ring of a polygon must have the same start and end points."   
  }  
}]  
```  
  
## <a name="next-steps"></a>Sonraki adımlar  

- [SQL söz dizimi ve Cosmos DB SQL sorgusu](how-to-sql-query.md)

- [Cosmos DB belgeleri](https://docs.microsoft.com/azure/cosmos-db/)  
