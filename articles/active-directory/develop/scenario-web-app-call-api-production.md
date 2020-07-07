---
title: 'Web-App, die Web-APIs aufruft: Überführen in die Produktion – Microsoft Identity Platform | Azure'
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
ms.openlocfilehash: 6404d00b87b9ee745b9e3a92c646404e574417c1
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "80881660"
---
# <a name="a-web-app-that-calls-web-apis-move-to-production"></a>Web-App, die Web-APIs aufruft: Überführen in die Produktion

Nachdem Sie nun wissen, wie Sie ein Token abrufen, um Web-APIs aufzurufen, erfahren Sie in diesem Artikel, wie Ihnen der Übergang in die Produktion gelingt.

[!INCLUDE [Common steps to move to production](../../../includes/active-directory-develop-scenarios-production.md)]

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie im vollständigen schrittweisen Tutorial zu ASP.NET Core-Web-Apps. Das Tutorial bietet Folgendes:

- Anmelden von Benutzern bei mehreren Zielgruppen oder bei nationalen Clouds bzw. mithilfe sozialer Identitäten
- Aufrufen von Microsoft Graph
- Aufrufen mehrerer Microsoft-APIs
- Behandeln der inkrementellen Zustimmung
- Aufrufen Ihrer eigenen Web-API

> [!div class="nextstepaction"]
> [Tutorial: ASP.NET Core-Web-App](https://github.com/Azure-Samples/ms-identity-aspnetcore-webapp-tutorial#scope-of-this-tutorial)

<!--- Removing this diagram as it's already shown from the next step linked tutorial

![Tutorial overview](media/scenarios/aspnetcore-webapp-tutorial.svg)

--->
