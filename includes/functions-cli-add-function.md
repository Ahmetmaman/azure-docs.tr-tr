---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/25/2020
ms.author: glenga
ms.openlocfilehash: a33cc604d45d51a63a73ea6ed58abc67269437d7
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2020
ms.locfileid: "80673192"
---
::: zone pivot="programming-language-csharp,programming-language-javascript,programming-language-typescript,programming-language-powershell,programming-language-python" 
Aşağıdaki komutu kullanarak, `--name` bağımsız değişkenin işlevinizin (HttpExample) benzersiz adı olduğu ve `--template` bağımsız değişkeninin işlevin TETIKLEYICISINI (http) belirttiği, projenize bir işlev ekleyin. 

```
func new --name HttpExample --template "HTTP trigger"
```  
::: zone-end  
::: zone pivot="programming-language-csharp"
`func new` bir HttpExample.cs kod dosyası oluşturur.
::: zone-end
::: zone pivot="programming-language-javascript,programming-language-typescript,programming-language-python,programming-language-powershell"
`func new` Projenin seçtiği dile uygun bir kod dosyası ve * üzerindefunction.js*adlı bir yapılandırma dosyası içeren işlev adıyla eşleşen bir alt klasör oluşturur.
::: zone-end