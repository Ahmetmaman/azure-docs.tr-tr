---
title: .NET’te Azure Relay Karma Bağlantılar Web Yuvaları ile çalışmaya başlama | Microsoft Docs
description: Azure Relay Karma Bağlantılar Web Yuvaları için bir C# konsol uygulaması yazın.
services: service-bus-relay
documentationcenter: .net
author: spelluru
manager: timlt
editor: ''
ms.assetid: d1386900-b942-4abf-acfc-38d2ef826253
ms.service: service-bus-relay
ms.devlang: tbd
ms.topic: conceptual
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 11/01/2018
ms.author: spelluru
ms.openlocfilehash: 16b1ad580ce9d2a6c34da7c243ab6dd9ab810078
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2019
ms.locfileid: "57763985"
---
# <a name="get-started-with-relay-hybrid-connections-websockets-in-net"></a>Geçiş karma bağlantıları WebSockets .NET içinde kullanmaya başlayın
[!INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

Bu hızlı başlangıçta, Azure geçiş karma bağlantıları WebSockets kullanarak ileti alma ve gönderme .NET gönderen ve alıcı uygulamalar oluşturun. Azure geçişi hakkında genel bilgi edinmek için [Azure geçişi](relay-what-is-it.md). 

Bu hızlı başlangıçta, aşağıdaki adımları uygulayın:

1. Azure portalını kullanarak Geçiş ad alanı oluşturma.
2. Azure portalını kullanarak o ad alanında karma bağlantı oluşturma.
3. İleti almak için bir sunucu (dinleyici) konsol uygulaması yazma.
4. İleti göndermek için bir istemci (gönderen) konsol uygulaması yazma.
5. Uygulamalar çalıştırın. 

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdaki önkoşulları karşılamanız gerekir:

* [Visual Studio 2015 veya üzeri](http://www.visualstudio.com). Bu öğreticideki örneklerde Visual Studio 2017 kullanılmaktadır.
* Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="create-a-namespace"></a>Ad alanı oluşturma
[!INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="create-a-hybrid-connection"></a>Karma bağlantı oluşturma
[!INCLUDE [relay-create-hybrid-connection-portal](../../includes/relay-create-hybrid-connection-portal.md)]

## <a name="create-a-server-application-listener"></a>Sunucu uygulaması (dinleyici) oluşturma
Geçiş hizmetinden ileti dinleyip almak için Visual Studio kullanarak bir C# konsol uygulaması yazın.

[!INCLUDE [relay-hybrid-connections-dotnet-get-started-server](../../includes/relay-hybrid-connections-dotnet-get-started-server.md)]

## <a name="create-a-client-application-sender"></a>İstemci uygulaması (gönderici) oluşturma
Geçiş hizmetine ileti göndermek Visual Studio kullanarak bir C# konsol uygulaması yazın.

[!INCLUDE [relay-hybrid-connections-dotnet-get-started-client](../../includes/relay-hybrid-connections-dotnet-get-started-client.md)]

## <a name="run-the-applications"></a>Uygulamaları çalıştırma
1. Sunucu uygulamasını çalıştırın.
2. İstemci uygulamasını çalıştırın ve metin girin.
3. Sunucu uygulama konsolunun istemci uygulamasına girilen metni görüntülediğinden emin olun.

    ![running-applications](./media/relay-hybrid-connections-dotnet-get-started/running-applications.png)

Tebrikler, uçtan uca bir Karma Bağlantılar uygulaması oluşturdunuz!

## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta, WebSockets ileti göndermek ve almak için kullanılan .NET istemci ve sunucu uygulamaları oluşturuldu. Azure geçişi karma bağlantılar özelliği, ileti göndermek ve almak için HTTP kullanarak da destekler. Azure geçiş karma bağlantıları ile HTTP kullanmayı öğrenmek için bkz [HTTP hızlı](relay-hybrid-connections-http-requests-dotnet-get-started.md).

Bu hızlı başlangıçta, .NET Framework istemci ve sunucu uygulamaları oluşturmak için kullanılır. Node.js kullanarak istemci ve sunucu uygulamaları yazmak öğrenmek için bkz: [Node.js WebSockets hızlı](relay-hybrid-connections-node-get-started.md) veya [Node.js HTTP hızlı](relay-hybrid-connections-http-requests-dotnet-get-started.md).

