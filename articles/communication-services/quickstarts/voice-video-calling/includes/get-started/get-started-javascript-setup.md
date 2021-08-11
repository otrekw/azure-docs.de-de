---
author: mikben
ms.service: azure-communication-services
ms.topic: include
ms.date: 03/10/2021
ms.author: mikben
ms.openlocfilehash: 5e09a3cbeac53393c686ceb298a217e1626442da
ms.sourcegitcommit: 096e7972e2a1144348f8d648f7ae66154f0d4b39
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2021
ms.locfileid: "112535981"
---
## <a name="setting-up"></a>Einrichten

### <a name="create-a-new-nodejs-application"></a>Erstellen einer neuen Node.js-Anwendung

Öffnen Sie Ihr Terminal- oder Befehlsfenster, erstellen Sie ein neues Verzeichnis für Ihre App, und navigieren Sie zu diesem Verzeichnis.

```console
mkdir calling-quickstart && cd calling-quickstart
```

Führen Sie `npm init -y` aus, um die Datei **package.json** mit den Standardeinstellungen zu erstellen.

```console
npm init -y
```

### <a name="install-the-package"></a>Installieren des Pakets

Verwenden Sie den Befehl `npm install`, um das Azure Communication Services Calling SDK für JavaScript zu installieren.

```console
npm install @azure/communication-common --save
npm install @azure/communication-calling@1.1.0 --save
```

Für diese Schnellstartanleitung werden die folgenden Webpack-Versionen empfohlen:

```console
"webpack": "^4.42.0",
"webpack-cli": "^3.3.11",
"webpack-dev-server": "^3.10.3"
```

Mit der Option `--save` wird die Bibliothek als Abhängigkeit in Ihrer Datei **package.json** aufgelistet.

### <a name="set-up-the-app-framework"></a>Einrichten des App-Frameworks

In dieser Schnellstartanleitung wird Webpack verwendet, um die Anwendungsressourcen zu bündeln. Führen Sie den folgenden Befehl aus, um die npm-Pakete „webpack“, „webpack-cli“ und „webpack-dev-server“ zu installieren und als Entwicklungsabhängigkeiten in Ihrer Datei **package.json** aufzulisten:

```console
npm install webpack@4.42.0 webpack-cli@3.3.11 webpack-dev-server@3.10.3 --save-dev
```

Erstellen Sie im Stammverzeichnis Ihres Projekts die Datei **index.html**. Diese Datei wird zum Konfigurieren eines grundlegenden Layouts verwendet, das es dem Benutzer ermöglicht, einen Anruf zu tätigen.
