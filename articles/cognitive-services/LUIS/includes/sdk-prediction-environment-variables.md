---
title: include file
description: include file
ms.topic: include
ms.date: 01/23/2020
ms.openlocfilehash: a7f4e0b47b3aaf9924f8c82eb6a9fa37decc0b2a
ms.sourcegitcommit: 984c5b53851be35c7c3148dcd4dfd2a93cebe49f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/28/2020
ms.locfileid: "76772422"
---
### <a name="create-an-environment-variable"></a>Erstellen einer Umgebungsvariablen

Erstellen Sie unter Verwendung Ihres Runtimeschlüssels und des Runtimeendpunkts Umgebungsvariablen für die Authentifizierung und den Zugriff:

* `LUIS_RUNTIME_KEY`: Der Runtimeressourcenschlüssel zum Authentifizieren Ihrer Anforderungen.
* `LUIS_RUNTIME_ENDPOINT`: Der Ihrem Schlüssel zugeordnete Runtimeendpunkt.
* `LUIS_APP_ID`: Die öffentliche LUIS-IoT-App-ID lautet `df67dcdb-c37d-46af-88e1-8b97951ca1c2`.
* `LUIS_APP_SLOT_NAME` - `production` oder `staging`

Wenn Sie diese Schnellstartanleitung für den Zugriff auf Ihre eigene App verwenden möchten, sind zusätzliche Schritte erforderlich:
* Erstellen der App und Abrufen der App-ID
* Zuweisen des Runtimeschlüssels zur App im LUIS-Portal
* Testen der URL mit dem Browser, um sich zu vergewissern, dass Sie auf die App zugreifen können

Führen Sie die Schritte für Ihr Betriebssystem aus:

#### <a name="windowstabwindows"></a>[Windows](#tab/windows)

```console
setx LUIS_RUNTIME_KEY <replace-with-your-resource-key>
setx LUIS_RUNTIME_ENDPOINT <replace-with-your-resource-endpoint>
setx LUIS_APP_ID <replace-with-your-app-id>
setx LUIS_APP_SLOT_NAME <replace-with-production-or-staging>
```

Starten Sie das Konsolenfenster neu, nachdem Sie die Umgebungsvariablen hinzugefügt haben.

#### <a name="linuxtablinux"></a>[Linux](#tab/linux)

```bash
export LUIS_RUNTIME_KEY= <replace-with-your-resource-key>
export LUIS_RUNTIME_ENDPOINT= <replace-with-your-resource-endpoint>
export LUIS_APP_ID= <replace-with-your-app-id>
export LUIS_APP_SLOT_NAME= <replace-with-production-or-staging>
```

Führen Sie nach dem Hinzufügen der Umgebungsvariablen im Konsolenfenster `source ~/.bashrc` aus, damit die Änderungen wirksam werden.

#### <a name="macostabunix"></a>[macOS](#tab/unix)

Bearbeiten Sie `.bash_profile`, und fügen Sie die Umgebungsvariable hinzu:

```bash
export LUIS_RUNTIME_KEY= <replace-with-your-resource-key>
export LUIS_RUNTIME_ENDPOINT= <replace-with-your-resource-endpoint>
export LUIS_APP_ID= <replace-with-your-app-id>
export LUIS_APP_SLOT_NAME= <replace-with-production-or-staging>
```

Führen Sie nach dem Hinzufügen der Umgebungsvariablen im Konsolenfenster `source .bash_profile` aus, damit die Änderungen wirksam werden.

***
