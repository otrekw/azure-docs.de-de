---
title: Protokollieren von Fehlern und Ausnahmen in MSAL.js
titleSuffix: Microsoft identity platform
description: Erfahren Sie, wie Sie Fehler und Ausnahmen in MSAL.js protokollieren
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 01/25/2021
ms.author: marsma
ms.reviewer: saeeda, jmprieur
ms.custom: aaddev
ms.openlocfilehash: d7463e3376847a219750254548c25ca79f4cdfb6
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "98954816"
---
# <a name="logging-in-msaljs"></a>Protokollierung in MSAL.js

[!INCLUDE [MSAL logging introduction](../../../includes/active-directory-develop-error-logging-introduction.md)]

## <a name="configure-logging-in-msaljs"></a>Konfigurieren der Protokollierung in MSAL.js

Aktivieren Sie die Protokollierung in MSAL.js (JavaScript), indem Sie beim Konfigurieren ein Protokollierungsobjekt zum Erstellen einer Instanz vom Typ `UserAgentApplication` übergeben. Dieses Protokollierungsobjekt weist die folgenden Eigenschaften auf:

- `localCallback`: Eine Rückrufinstanz, die vom Entwickler bereitgestellt werden kann, um Protokolle benutzerdefiniert zu verarbeiten und zu veröffentlichen. Implementieren Sie die „localCallback“-Methode abhängig davon, wie Sie Protokolle umleiten möchten.
- `level` (optional): Der konfigurierbare Protokolliergrad. Die unterstützten Protokollierungsebenen sind: `Error`, `Warning`, `Info` und `Verbose`. Der Standardwert lautet `Info`.
- `piiLoggingEnabled` (optional): protokolliert personenbezogene und organisationsbezogene Daten, wenn der Wert auf „true“ festgelegt wird. Dieser Parameter ist standardmäßig auf „false“ festgelegt, damit Ihre Anwendung keine personenbezogenen Daten protokolliert. Protokolle mit personenbezogenen Daten werden nie in Standardausgaben wie die Konsole, Logcat oder NSLog geschrieben.
- `correlationId` (optional): Ein eindeutiger Bezeichner, der die Anforderung der Antwort zu Debugzwecken zuordnet. Standardmäßig wird ein in RFC 4122 definierter GUID der Version 4 (128 Bit) verwendet.

```javascript
function loggerCallback(logLevel, message, containsPii) {
   console.log(message);
}

var msalConfig = {
    auth: {
        clientId: "<Enter your client id>",
    },
    system: {
        logger: new Msal.Logger(
            loggerCallback , {
                level: Msal.LogLevel.Verbose,
                piiLoggingEnabled: false,
                correlationId: '1234'
            }
        )
    }
}

var UserAgentApplication = new Msal.UserAgentApplication(msalConfig);
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Codebeispiele finden Sie unter [Microsoft Identity Platform-Codebeispiele](sample-v2-code.md).