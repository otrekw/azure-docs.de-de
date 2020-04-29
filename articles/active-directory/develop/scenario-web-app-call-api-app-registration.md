---
title: Registrieren einer Web-App, die Web-APIs aufruft – Microsoft Identity Platform | Azure
description: Erfahren Sie, wie Sie eine Web-App registrieren, die Web-APIs aufruft.
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
ms.openlocfilehash: 8cb7d86bd419563363779c499962c81f0c59e3b6
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "80881875"
---
# <a name="a-web-app-that-calls-web-apis-app-registration"></a>Web-App, die Web-APIs aufruft: App-Registrierung

Eine Web-App, die Web-APIs aufruft, verfügt über die gleiche Registrierung wie eine Web-App, die Benutzer anmeldet. Befolgen Sie daher die Anweisungen unter [Web-App, die Benutzer anmeldet: App-Registrierung](scenario-web-app-sign-user-app-registration.md).

Da die Web-App jetzt auch Web-APIs aufruft, wird sie zu einer vertraulichen Clientanwendung. Aus diesem Grund ist eine zusätzliche Registrierung erforderlich. Die App muss Clientanmeldeinformationen, sogenannte *Geheimnisse*, für Microsoft Identity Platform freigeben.

[!INCLUDE [Registration of client secrets](../../../includes/active-directory-develop-scenarios-registration-client-secrets.md)]

## <a name="api-permissions"></a>API-Berechtigungen

Web-Apps rufen APIs im Namen des angemeldeten Benutzers auf. Zu diesem Zweck müssen sie *delegierte Berechtigungen* anfordern. Weitere Informationen finden Sie unter [Konfigurieren einer Clientanwendung für den Zugriff auf Web-APIs](quickstart-configure-app-access-web-apis.md#add-permissions-to-access-web-apis).

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Web-App, die Web-APIs aufruft: Codekonfiguration](scenario-web-app-call-api-app-configuration.md)
