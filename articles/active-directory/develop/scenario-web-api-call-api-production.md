---
title: Verschieben einer Web-API, die Web-APIs aufruft, in die Produktion – Microsoft Identity Platform | Azure
description: Erfahren Sie, wie Sie eine Web-API in die Produktion verschieben, die Web-APIs aufruft.
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1426ad250e18d0132e116162a374120dda2e1200
ms.sourcegitcommit: 05cdbb71b621c4dcc2ae2d92ca8c20f216ec9bc4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/16/2020
ms.locfileid: "76044137"
---
# <a name="a-web-api-that-calls-web-apis-move-to-production"></a>Web-API, die Web-APIs aufruft: Überführen in die Produktion

Nachdem Sie ein Token zum Aufrufen von Web-APIs abgerufen haben, können Sie Ihre App in die Produktionsumgebung verschieben.

[!INCLUDE [Move to production common steps](../../../includes/active-directory-develop-scenarios-production.md)]

## <a name="learn-more"></a>Weitere Informationen

Sie haben sich mit den Grundlagen des Aufrufens von Web-APIs über Ihre eigene Web-API vertraut gemacht. Nun könnte Sie das folgende Tutorial über den Code interessieren, der zum Erstellen einer geschützten Web-API verwendet wird, die Web-APIs aufruft.

| Beispiel | Plattform | Beschreibung |
|--------|----------|-------------|
| [active-directory-aspnetcore-webapi-tutorial-v2](https://github.com/Azure-Samples/active-directory-dotnet-native-aspnetcore-v2/tree/master/2.%20Web%20API%20now%20calls%20Microsoft%20Graph) | ASP.NET Core 2.2-Web-API, Desktop (WPF) | Die ASP.NET Core 2.2-Web-API ruft Microsoft Graph auf. Microsoft Graph wiederum wird von einer WPF-Anwendung über Microsoft Identity Platform (v2.0) aufgerufen. |
