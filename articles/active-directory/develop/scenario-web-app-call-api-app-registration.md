---
title: Registrieren einer Web-App, die Web-APIs aufruft – Microsoft Identity Platform | Azure
description: Erfahren Sie, wie Sie eine Web-App registrieren, die Web-APIs aufruft.
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
ms.openlocfilehash: 5a57fcef3569734964bf6e8a41faa49800798f9b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "76759056"
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
