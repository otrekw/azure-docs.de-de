---
title: Web-App für Benutzeranmeldungen (Übergang in die Produktion) – Microsoft Identity Platform
description: Erfahren Sie, wie Sie eine Web-App erstellen, die Benutzer anmeldet (Übergang in die Produktion).
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
ms.date: 09/17/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: efcc7ed4f80d0e3e9750d19ff95d010052a08b87
ms.sourcegitcommit: 8cf199fbb3d7f36478a54700740eb2e9edb823e8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/25/2019
ms.locfileid: "74481892"
---
# <a name="web-app-that-signs-in-users-move-to-production"></a>Web-App für Benutzeranmeldungen: Überführen in die Produktion

Da Sie nun wissen, wie Sie ein Token zum Abrufen von Web-APIs aufrufen, erfahren Sie in diesem Artikel, wie Ihnen der Übergang in die Produktion gelingt.

[!INCLUDE [Move to production common steps](../../../includes/active-directory-develop-scenarios-production.md)]

## <a name="next-steps"></a>Nächste Schritte

### <a name="scenario-for-calling-web-apis"></a>Szenario zum Aufrufen von Web-APIs

Sobald Ihre Web-App Benutzer anmeldet, kann sie für diese angemeldeten Benutzer Web-APIs aufrufen. Genau darum geht es im folgenden Szenario:

> [!div class="nextstepaction"]
> [Web-App, die Web-APIs aufruft](scenario-web-app-call-api-overview.md)

### <a name="deep-dive-aspnet-core-web-app-tutorial"></a>Ausführliche Betrachtung: Tutorial für ASP.NET Core-Web-Apps

Lernen Sie mithilfe dieses ASP.NET Core-Tutorials andere Möglichkeiten zum Anmelden von Benutzern kennen: 

> [!div class="nextstepaction"]
> [Ermöglichen Sie Ihren Web-Apps das Anmelden von Benutzern und das Aufrufen von APIs mit der Microsoft Identity-Plattform für Entwickler](https://github.com/Azure-Samples/ms-identity-aspnetcore-webapp-tutorial) 

Dieses fortschreitende Tutorial enthält Web-App-Code, der in Produktionsumgebungen eingesetzt werden kann. Außerdem wird beschrieben, wie eine Anmeldung mit Konten hinzugefügt wird, die sich in folgenden Umgebungen befinden:

- Ihrer Organisation
- mehreren Organisationen
- Geschäfts-, Schul- oder Unikonten bzw. persönlichen Microsoft-Konten
- [Azure AD B2C](https://aka.ms/aadb2c)
- Nationale Clouds

### <a name="sample-code-java-web-app"></a>Beispielcode: Java-Web-App

Weitere Informationen zur Java-Web-App aus dem Beispiel auf GitHub finden Sie hier: 

> [!div class="nextstepaction"]
> [Eine Java-Webanwendung, die Benutzer bei Microsoft Identity Platform anmeldet und Microsoft Graph aufruft](https://github.com/Azure-Samples/ms-identity-java-webapp)
