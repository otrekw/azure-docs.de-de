---
title: Eingabeaufforderungsverhalten bei interaktiven Anforderungen (MSAL.js) | Azure
titleSuffix: Microsoft identity platform
description: Erfahren Sie, wie Sie das Eingabeaufforderungsverhalten in interaktiven Aufrufen mithilfe der Microsoft Authentication Library für JavaScript (MSAL.js) anpassen.
services: active-directory
author: navyasric
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 04/24/2019
ms.author: nacanuma
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: 778e89655019a49a30904fbe8d8e6aedf1833e9a
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2020
ms.locfileid: "76695975"
---
# <a name="prompt-behavior-in-msaljs-interactive-requests"></a>Eingabeaufforderungsverhalten in interaktiven MSAL.js-Anforderungen

Wenn ein Benutzer eine aktive Sitzung in Azure AD mit mehreren Benutzerkonten hergestellt hat, werden Benutzer von der Anmeldeseite in Azure AD standardmäßig dazu aufgefordert, ein Konto auszuwählen, bevor mit der Anmeldung fortgefahren wird. Benutzer wird nicht die Möglichkeit zum Auswählen eines Kontos angezeigt, wenn nur eine einzige authentifizierte Sitzung in Azure AD vorhanden ist.

Die MSAL.js-Bibliothek (ab Version 0.2.4) sendet während den interaktiven Anforderungen (`loginRedirect`, `loginPopup`, `acquireTokenRedirect` und `acquireTokenPopup`) keine Eingabeaufforderungsparameter und erzwingt folglich keinerlei Eingabeaufforderungsverhalten. Bei automatischen Tokenanforderungen mithilfe der `acquireTokenSilent`-Methode übergibt MSAL.js einen Eingabeaufforderungsparameter, der auf `none` festgelegt ist.

Je nach Anwendungsszenario können Sie das Eingabeaufforderungsverhalten für die interaktiven Anforderungen steuern, indem Sie den Eingabeaufforderungsparameter in den Anforderungsparametern festlegen, die an die Methoden übergeben werden. Ein Beispiel: Sie möchten, dass für Benutzer die Möglichkeit zum Auswählen eines Kontos angezeigt wird:

```javascript
var request = {
    scopes: ["user.read"],
    prompt: 'select_account',
}

userAgentApplication.loginRedirect(request);
```


Die folgenden Eingabeaufforderungswerte können während der Authentifizierung bei Azure AD übergeben werden:

**login:** Dieser Wert erzwingt, dass der Benutzer bei der Authentifizierungsanforderung Anmeldeinformationen eingibt.

**select_account:** Dieser Wert stellt dem Benutzer die Möglichkeit zur Verfügung, ein Konto auszuwählen. Dazu werden alle Konten aufgeführt, die in der Sitzung vorhanden sind.

**consent:** Dieser Wert ruft ein Dialogfeld zur OAuth-Einwilligung auf, mithilfe dessen der Benutzer der App Berechtigungen erteilen kann.

**none:** Mit diesem Wert ist sichergestellt, dass dem Benutzer keinerlei interaktive Eingabeaufforderung angezeigt wird. Es wird empfohlen, diesen Wert nicht an interaktive Methoden in MSAL.js zu übergeben, da er zu unerwartetem Verhalten führen kann. Verwenden Sie stattdessen die `acquireTokenSilent`-Methode, damit Aufrufe automatisch erfolgen.

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich weiter über den `prompt`-Parameter im [OAuth 2.0-Protokoll für die implizite Gewährung](v2-oauth2-implicit-grant-flow.md), das die MSAL.js-Bibliothek verwendet.
