---
title: Eingabeaufforderungsverhalten bei interaktiven Anforderungen (MSAL.js) | Azure
titleSuffix: Microsoft identity platform
description: Erfahren Sie, wie Sie das Eingabeaufforderungsverhalten in interaktiven Aufrufen mithilfe der Microsoft Authentication Library für JavaScript (MSAL.js) anpassen.
services: active-directory
author: mtillman
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: how-to
ms.workload: identity
ms.date: 04/24/2019
ms.author: mtillman
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: 3cd1b6a0015f51902dd3228648f16fb79ab6957d
ms.sourcegitcommit: 3bb9f8cee51e3b9c711679b460ab7b7363a62e6b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/14/2021
ms.locfileid: "112077174"
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

**login:** Dieser Wert erzwingt, dass der Benutzer bei der Authentifizierungsanforderung Anmeldeinformationen eingeben muss.

**select_account:** Dieser Wert bietet dem Benutzer die Möglichkeit, ein Konto auszuwählen. Dazu werden alle Konten aufgeführt, die in der Sitzung vorhanden sind.

**consent:** Dieser Wert ruft ein Dialogfeld zur OAuth-Einwilligung auf, in dem der Benutzer der App Berechtigungen erteilen kann.

**none:** Mit diesem Wert ist sichergestellt, dass dem Benutzer keine interaktiven Eingabeaufforderungen angezeigt werden. Es wird empfohlen, diesen Wert nicht an interaktive Methoden in MSAL.js zu übergeben, da er zu unerwartetem Verhalten führen kann. Verwenden Sie stattdessen die `acquireTokenSilent`-Methode, damit Aufrufe automatisch erfolgen.

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich weiter über den `prompt`-Parameter im [OAuth 2.0-Protokoll für die implizite Gewährung](v2-oauth2-implicit-grant-flow.md), das die MSAL.js-Bibliothek verwendet.
