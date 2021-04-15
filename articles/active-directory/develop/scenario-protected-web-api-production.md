---
title: Verschieben einer geschützten Web-API in die Produktion | Azure
titleSuffix: Microsoft identity platform
description: Erfahren Sie, wie Sie eine geschützte Web-API erstellen (Übergang in die Produktion).
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 07/15/2020
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 98be47200c12ab3097f9416b5647abf6465a89e7
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "104675973"
---
# <a name="protected-web-api---move-to-production"></a>Geschützte Web-API: Übergang in die Produktion

Da Sie nun wissen, wie Sie Ihre Web-API schützen können, sollten Sie die folgenden Punkte beachten, wenn Sie Ihre Anwendung in die Produktion verschieben.

[!INCLUDE [Common steps to move to production](../../../includes/active-directory-develop-scenarios-production.md)]

## <a name="next-steps"></a>Nächste Schritte

Hier erfahren Sie, wie Sie eine Downstream-API aufrufen: [Szenario: Eine Web-API, die Web-APIs aufruft](scenario-web-api-call-api-overview.md).


Weitere Informationen finden Sie in Tutorials und Beispielen auf GitHub:

- [Aufrufen einer geschützten API mit einem Daemon](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2/tree/master/2-Call-OwnApi)
- [Tutorial zur ASP.NET Core-Web-API](https://github.com/Azure-Samples/active-directory-dotnet-native-aspnetcore-v2)
- [Beispiel zur ASP.NET-Web-API](https://github.com/azureadquickstarts/appmodelv2-nativeclient-dotnet)
