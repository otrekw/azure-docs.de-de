---
title: Entfernen von Konten aus dem Tokencache bei der Abmeldung – Microsoft Identity Platform | Azure
description: Erfahren Sie, wie Sie ein Konto bei der Abmeldung aus dem Tokencache entfernen
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 09/30/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: e138b3513b42dda47b0a114d866d657e18e3e393
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "82181646"
---
# <a name="a-web-app-that-calls-web-apis-remove-accounts-from-the-token-cache-on-global-sign-out"></a>Web-App, die Web-APIs aufruft: Entfernen von Konten aus dem Tokencache bei der globalen Abmeldung

Sie haben bereits erfahren, wie Sie Ihrer Web-App eine Anmeldefunktion hinzufügen (siehe [Web-App, die Benutzer anmeldet: An- und Abmeldung](scenario-web-app-sign-user-sign-in.md)).

Bei einer Web-App, die Web-APIs aufruft, funktioniert die Abmeldung anders. Wenn sich der Benutzer von Ihrer Anwendung (oder von einer beliebigen Anwendung) abmeldet, müssen Sie die diesem Benutzer zugeordneten Token aus dem Tokencache entfernen.

## <a name="intercept-the-callback-after-single-sign-out"></a>Abfangen des Rückrufs nach einmaliger Abmeldung

Um den Eintrag, der mit dem abgemeldeten Konto verbunden ist, aus dem Tokencache zu löschen, kann Ihre Anwendung das „after `logout`“-Ereignis abfangen. Web-Apps speichern die Zugriffstoken für die einzelnen Benutzer in einem Tokencache. Durch das Abfangen des „after `logout`“-Rückrufs kann Ihre Web-App den Benutzer aus dem Cache entfernen.

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

Microsoft.Identity.Web übernimmt die Implementierung der Abmeldung für Sie.

# <a name="aspnet"></a>[ASP.NET](#tab/aspnet)

Im ASP.NET-Beispiel werden Konten bei der globalen Abmeldung nicht aus dem Cache entfernt.

# <a name="java"></a>[Java](#tab/java)

Im Java-Beispiel werden Konten bei der globalen Abmeldung nicht aus dem Cache entfernt.

# <a name="python"></a>[Python](#tab/python)

Im Python-Beispiel werden Konten bei der globalen Abmeldung nicht aus dem Cache entfernt.

---

## <a name="next-steps"></a>Nächste Schritte

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

> [!div class="nextstepaction"]
> [Abrufen eines Tokens für die Web-App](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-call-api-acquire-token?tabs=aspnetcore)

# <a name="aspnet"></a>[ASP.NET](#tab/aspnet)

> [!div class="nextstepaction"]
> [Abrufen eines Tokens für die Web-App](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-call-api-acquire-token?tabs=aspnet)

# <a name="java"></a>[Java](#tab/java)

> [!div class="nextstepaction"]
> [Abrufen eines Tokens für die Web-App](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-call-api-acquire-token?tabs=java)

# <a name="python"></a>[Python](#tab/python)

> [!div class="nextstepaction"]
> [Abrufen eines Tokens für die Web-App](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-call-api-acquire-token?tabs=python)

---
