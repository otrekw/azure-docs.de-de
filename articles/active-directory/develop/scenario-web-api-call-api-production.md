---
title: Verschieben von Web-APIs aufrufenden Web-APIs in die Produktion | Azure
titleSuffix: Microsoft identity platform
description: Erfahren Sie, wie Sie eine Web-API in die Produktion verschieben, die Web-APIs aufruft.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 0ab5baef925b7c8589dd7852b6ff8058d67ba745
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "104675871"
---
# <a name="a-web-api-that-calls-web-apis-move-to-production"></a>Web-API, die Web-APIs aufruft: Überführen in die Produktion

Nachdem Sie ein Token zum Aufrufen von Web-APIs abgerufen haben, sollten Sie die folgenden Punkte beachten, wenn Sie Ihre Anwendung in die Produktion verschieben.

[!INCLUDE [Common steps to move to production](../../../includes/active-directory-develop-scenarios-production.md)]

## <a name="next-steps"></a>Nächste Schritte

Sie haben sich mit den Grundlagen des Aufrufens von Web-APIs über Ihre eigene Web-API vertraut gemacht. Nun könnte Sie das folgende Tutorial über den Code interessieren, der zum Erstellen einer geschützten Web-API verwendet wird, die Web-APIs aufruft.

| Beispiel | Plattform | BESCHREIBUNG |
|--------|----------|-------------|
| [active-directory-aspnetcore-webapi-tutorial-v2](https://github.com/Azure-Samples/active-directory-dotnet-native-aspnetcore-v2/tree/master/2.%20Web%20API%20now%20calls%20Microsoft%20Graph), Kapitel 1 | ASP.NET Core-Web-API, Desktop (WPF) | Die ASP.NET Core-Web-API ruft Microsoft Graph auf. Microsoft Graph wiederum wird von einer WPF-Anwendung über Microsoft Identity Platform aufgerufen. |
