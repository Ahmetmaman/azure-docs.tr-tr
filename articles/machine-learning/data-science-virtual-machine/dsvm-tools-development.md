---
title: Veri bilimi sanal makinesi geliştirme araçları - Azure | Microsoft Docs
description: Araçlar ve veri bilimi sanal makinesi üzerinde önceden yüklenen tümleşik geliştirme ortamları hakkında bilgi edinin.
keywords: veri bilimi araçları, veri bilimi sanal makinesi, veri bilimi için araçlar, linux veri bilimi
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.custom: seodec18
ms.assetid: ''
ms.service: machine-learning
ms.subservice: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/11/2017
ms.author: gokuma
ms.openlocfilehash: 7983169c2b1123c57a48471e3f4d9ad6f19c84dc
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58314731"
---
# <a name="development-tools-on-the-data-science-virtual-machine"></a>Veri bilimi sanal makinesi üzerinde geliştirme araçları

Veri bilimi sanal makinesi (DSVM), çeşitli popüler Araçlar ve IDE paketleme tarafından üretken bir geliştirme ortamı sağlar. DSVM'nin sağlanan bazı araçları şunlardır. 

## <a name="visual-studio-2017"></a>Visual Studio 2017  

|    |           |
| ------------- | ------------- |
| Nedir?   | Genel amaçlı IDE      |
| Desteklenen DSVM sürümleri      | Windows      |
| Tipik kullanımları      | Yazılım geliştirme    |
| Nasıl, yapılandırılmış / DSVM üzerinde yüklü?      | Veri bilimi iş yükü (Python ve R araçları), Azure iş yükü (Hadoop, Data Lake), Node.js, SQL Server araçlarını, [Visual Studio Code için Azure Machine Learning](https://github.com/Microsoft/vs-tools-for-ai)    |
| Kullanma / çalıştırın nasıl?      | Masaüstü kısayolu (`C:\Program Files (x86)\Microsoft Visual Studio\2017\Community\Common7\IDE\devenv.exe`)    |
| DSVM ilgili araçları      |     Visual Studio kodu, RStudio, Juno  |

## <a name="visual-studio-code"></a>Visual Studio Code 

|    |           |
| ------------- | ------------- |
| Nedir?   | Genel amaçlı IDE      |
| Desteklenen DSVM sürümleri      | Windows, Linux     |
| Tipik kullanımları      | Kod Düzenleyicisi ve Git tümleştirmesi   |
| Kullanma / çalıştırın nasıl?      | Masaüstü kısayolu (`C:\Program Files (x86)\Microsoft VS Code\Code.exe`) Windows, masaüstü kısayolu veya terminal (`code`) Linux'ta    |
| DSVM ilgili araçları      |     RStudio, Visual Studio 2017 Juno  |

## <a name="rstudio--desktop"></a>RStudio Desktop 

|    |           |
| ------------- | ------------- |
| Nedir?   | İstemci için R IDE    |
| Desteklenen DSVM sürümleri      | Windows, Linux      |
| Tipik kullanımları      |  R geliştirme     |
| Kullanma / çalıştırın nasıl?      | Masaüstü kısayolu (`C:\Program Files\RStudio\bin\rstudio.exe`) Windows, masaüstü kısayolu üzerinde (`/usr/bin/rstudio`) Linux'ta      |
| DSVM ilgili araçları      |   Visual Studio 2017, Visual Studio kodu, Juno      |

## <a name="rstudio--server"></a>RStudio  Server 

|    |           |
| ------------- | ------------- |
| Nedir?   | R için Web tabanlı IDE    |
| Desteklenen DSVM sürümleri      | Linux      |
| Tipik kullanımları      |  R geliştirme     |
| Kullanma / çalıştırın nasıl?      | Etkinleştirme hizmete _systemctl etkinleştirme rstudio server_, hizmeti ile başlatın _systemctl Başlat rstudio server_. Ardından RStudio Server için http oturum:\//,-vm-ip:8787.       |
| DSVM ilgili araçları      |   Visual Studio 2017, Visual Studio kodu, RStudio Desktop      |

## <a name="juno"></a>Juno 

|    |           |
| ------------- | ------------- |
| Nedir?   | Julia diline için IDE istemcisi   |
| Desteklenen DSVM sürümleri      | Windows, Linux      |
| Tipik kullanımları      |  Julia geliştirme     |
| Kullanma / çalıştırın nasıl?      | Masaüstü kısayolu (`C:\JuliaPro-0.5.1.1\Juno.bat`) Windows, masaüstü kısayolu üzerinde (`/opt/JuliaPro-VERSION/Juno`) Linux'ta      |
| DSVM ilgili araçları      |   Visual Studio 2017, Visual Studio kodu, RStudio      |

## <a name="pycharm"></a>Pycharm

|    |           |
| ------------- | ------------- |
| Nedir?   | Python dil için IDE istemcisi    |
| Desteklenen DSVM sürümleri      | Linux      |
| Tipik kullanımları      |  Python geliştirme     |
| Kullanma / çalıştırın nasıl?      | Masaüstü kısayolu (`/usr/bin/pycharm`) Linux'ta      |
| DSVM ilgili araçları      |   Visual Studio 2017, Visual Studio kodu, RStudio      |



## <a name="powerbi-desktop"></a>PowerBI Masaüstü 

|    |           |
| ------------- | ------------- |
| Nedir?   | Etkileşimli veri görselleştirmesi ve BI aracı    |
| Desteklenen DSVM sürümleri      | Windows  |
| Tipik kullanımları      |  Veri Görselleştirme ve panolar oluşturma   |
| Kullanma / çalıştırın nasıl?      | Masaüstü kısayolu (`C:\Program Files\Microsoft Power BI Desktop\bin\PBIDesktop.exe`)      |
| DSVM ilgili araçları      |   Visual Studio 2017, Visual Studio kodu, Juno      |

