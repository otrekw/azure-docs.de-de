---
title: Verschieben einer Web-APIs aufrufenden Web-App in die Produktion | Azure
titleSuffix: Microsoft identity platform
description: Hier erfahren Sie, wie Sie eine Web-App, die Web-APIs aufruft, in die Produktion überführen.
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
ms.openlocfilehash: cf32274a49cb1b790e9d872efe36f2e1cb188d1d
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "104675939"
---
# <a name="a-web-app-that-calls-web-apis-move-to-production"></a>Verschieben einer Web-APIs aufrufenden Web-App in die Produktion

Nachdem Sie nun wissen, wie Sie ein Token zum Aufrufen von Web-APIs abrufen, sollten Sie die folgenden Punkte beachten, wenn Sie Ihre Anwendung in die Produktion verschieben.

[!INCLUDE [Common steps to move to production](../../../includes/active-directory-develop-scenarios-production.md)]

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie im vollständigen schrittweisen Tutorial zu ASP.NET Core-Web-Apps. Das Tutorial bietet Folgendes:

- Anmelden von Benutzern bei mehreren Zielgruppen oder bei nationalen Clouds bzw. mithilfe sozialer Identitäten
- Aufrufen von Microsoft Graph
- Aufrufen mehrerer Microsoft-APIs
- Behandeln der inkrementellen Zustimmung
- Aufrufen Ihrer eigenen Web-API

[Tutorial: ASP.NET Core-Web-App](https://github.com/Azure-Samples/ms-identity-aspnetcore-webapp-tutorial#scope-of-this-tutorial)

<!--- Removing this diagram as it's already shown from the next step linked tutorial

![Tutorial overview](media/scenarios/aspnetcore-webapp-tutorial.svg)

--->
