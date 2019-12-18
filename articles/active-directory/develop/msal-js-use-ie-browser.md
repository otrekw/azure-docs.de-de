---
title: Verwenden von Internet Explorer mit MSAL.js | Azure
titleSuffix: Microsoft identity platform
description: Verwenden Sie die Microsoft-Authentifizierungsbibliothek für JavaScript (MSAL.js) mit Internet Explorer.
services: active-directory
author: navyasric
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/16/2019
ms.author: nacanuma
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 715e92a10ca0b4cbe38119931bc66d36c3b9259a
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/08/2019
ms.locfileid: "74916280"
---
# <a name="use-internet-explorer-and-microsoft-edge-browsers-with-msaljs"></a>Verwenden der Browser Internet Explorer und Microsoft Edge mit „MSAL.js“

Die Microsoft-Authentifizierungsbibliothek für JavaScript (MSAL.js) wird für [JavaScript ES5](https://fr.wikipedia.org/wiki/ECMAScript#ECMAScript_Edition_5_.28ES5.29) generiert, um die Ausführung in Internet Explorer zu ermöglichen. Hierbei sollten aber einige Dinge beachtet werden.

## <a name="run-an-app-in-internet-explorer"></a>Ausführen einer App in Internet Explorer
Wenn Sie „MSAL.js“ in Anwendungen nutzen möchten, die in Internet Explorer ausgeführt werden können, müssen Sie einen Verweis auf ein Zusagen-Polyfill-Element hinzufügen, bevor Sie auf das Skript „MSAL.js“ verweisen.

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/bluebird/3.3.4/bluebird.min.js" class="pre"></script>
```

Der Grund ist, dass JavaScript-Zusagen von Internet Explorer nicht nativ unterstützt werden.

## <a name="debugging-an-application-running-in-internet-explorer"></a>Debuggen einer in Internet Explorer ausgeführten Anwendung

### <a name="running-in-production"></a>Ausführen in der Produktion
Die Bereitstellung Ihrer Anwendung in der Produktion (z. B. in Azure-Web-Apps) funktioniert, sofern der Endbenutzer Popups akzeptiert hat. Wir haben dies mit Internet Explorer 11 getestet.

### <a name="running-locally"></a>Lokales Ausführen
Falls Sie Ihre in Internet Explorer ausgeführte Anwendung lokal ausführen und debuggen möchten, sollten Sie über folgende Aspekte Bescheid wissen (Annahme: Sie möchten Ihre Anwendung als *http://localhost:1234* ausführen):

- Internet Explorer verfügt über den Sicherheitsmechanismus „Geschützter Modus“, der verhindert, dass „MSAL.js“ richtig funktioniert. Eines der Symptome nach dem Anmelden kann darin bestehen, dass die Seite an http://localhost:1234/null umgeleitet wird.

- Zum lokalen Ausführen und Debuggen Ihrer Anwendung müssen Sie diesen „Geschützten Modus“ deaktivieren. Hier:

    1. Klicken Sie in Internet Explorer auf **Extras** (Zahnradsymbol).
    1. Wählen Sie **Internetoptionen** und dann die Registerkarte **Sicherheit**.
    1. Klicken Sie auf die Zone **Internet**, und deaktivieren Sie die Option **Geschützten Modus aktivieren (erfordert IE-Neustart)** . In Internet Explorer wird eine Warnung mit dem Hinweis angezeigt, dass Ihr Computer nicht mehr geschützt ist. Klicken Sie auf **OK**.
    1. Starten Sie Internet Explorer neu.
    1. Führen Sie die Anwendung aus, und debuggen Sie sie.

Wenn Sie fertig sind, können Sie die Sicherheitseinstellungen von Internet Explorer wieder auf den vorherigen Stand zurücksetzen.  Wählen Sie **Einstellungen** -> **Internetoptionen** -> **Sicherheit** -> **Alle Zonen auf Standardstufe zurücksetzen**.

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie mehr zu den [bekannten Problemen bei der Verwendung von „MSAL.js“ in Internet Explorer](msal-js-use-ie-browser.md).