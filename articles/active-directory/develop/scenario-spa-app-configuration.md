---
title: Konfigurieren einer Single-Page-App – Microsoft Identity Platform | Azure
description: Informationen zum Erstellen einer Single-Page-Webanwendung (Codekonfiguration der App)
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: nacanuma
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 111f8d45fced1ed0e9293c874d7a56c64e9fb0e4
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2019
ms.locfileid: "74965414"
---
# <a name="single-page-application-code-configuration"></a>Single-Page-Webanwendung: Codekonfiguration

Erfahren Sie, wie Sie den Code für Ihre Single-Page-Webanwendung (SPA) konfigurieren.

## <a name="msal-libraries-that-support-implicit-flow"></a>MSAL-Bibliotheken, die den impliziten Flow unterstützen

Die Microsoft Identity Platform stellt die folgenden Microsoft-Authentifizierungsbibliotheken (Microsoft Authentication Library, MSAL) zur Unterstützung des impliziten Flows mithilfe von empfohlenen branchenüblichen Sicherheitsmethoden bereit:  

| MSAL-Bibliothek | BESCHREIBUNG |
|--------------|--------------|
| ![MSAL.js](media/sample-v2-code/logo_js.png) <br/> [MSAL.js](https://github.com/AzureAD/microsoft-authentication-library-for-js)  | Einfache JavaScript-Bibliothek zur Verwendung in einer beliebigen clientseitigen Web-App, die über JavaScript- oder SPA-Frameworks wie Angular, Vue.js und React.js erstellt wurde. |
| ![MSAL Angular](media/sample-v2-code/logo_angular.png) <br/> [MSAL Angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) | Wrapper der MSAL.js-Kernbibliothek für eine einfachere Verwendung in Single-Page-Webanwendungen, die über das Angular-Framework erstellt wurden. Diese Bibliothek ist als Vorschauversion verfügbar und weist [bekannte Probleme](https://github.com/AzureAD/microsoft-authentication-library-for-js/issues?q=is%3Aopen+is%3Aissue+label%3Aangular) mit bestimmten Angular-Versionen und Browsern auf. |

## <a name="application-code-configuration"></a>Codekonfiguration der Anwendung

In einer MSAL-Bibliothek werden die Anwendungsregistrierungsinformationen während der Initialisierung der Bibliothek als Konfiguration übergeben.

### <a name="javascript"></a>JavaScript

```javascript
// Configuration object constructed.
const config = {
    auth: {
        clientId: 'your_app_id',
        redirectUri: "your_app_redirect_uri" //defaults to application start page
    }
}

// create UserAgentApplication instance
const userAgentApplication = new UserAgentApplication(config);
```
Weitere Informationen zu den konfigurierbaren Optionen finden Sie unter [Initialisieren von Clientanwendungen mithilfe von MSAL.js](msal-js-initializing-client-applications.md).

### <a name="angular"></a>Angular

```javascript
//In app.module.ts
import { MsalModule } from '@azure/msal-angular';

@NgModule({
  imports: [ MsalModule.forRoot({
                clientID: 'your_app_id'
            })]
         })

  export class AppModule { }
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [An- und Abmeldung](scenario-spa-sign-in.md)
