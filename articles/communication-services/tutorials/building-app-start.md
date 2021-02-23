---
title: 'Tutorial: Vorbereiten einer Web-App für Azure Communication Services (Node.js)'
titleSuffix: An Azure Communication Services tutorial
description: Es wird beschrieben, wie Sie eine Baseline-Webanwendung erstellen, die Azure Communication Services unterstützt.
author: nmurav
services: azure-communication-services
ms.author: nmurav
ms.date: 01/03/2012
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: 4655a20ddd419993f5a73ec54420abec96d32a62
ms.sourcegitcommit: 5a999764e98bd71653ad12918c09def7ecd92cf6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/16/2021
ms.locfileid: "100546175"
---
# <a name="tutorial-prepare-a-web-app-for-azure-communication-services-nodejs"></a>Tutorial: Vorbereiten einer Web-App für Azure Communication Services (Node.js)

[!INCLUDE [Public Preview Notice](../includes/public-preview-include.md)]

Azure Communication Services ermöglicht Ihnen das Hinzufügen von Echtzeitkommunikation zu Ihren Anwendungen. In diesem Tutorial wird beschrieben, wie Sie eine Webanwendung einrichten, die Azure Communication Services unterstützt. Dies ist ein Einführungstutorial für neue Entwickler, die in die Echtzeitkommunikation einsteigen möchten.

Am Ende dieses Tutorials verfügen Sie über eine Baseline-Webanwendung, für die Azure Communication Services-Clientbibliotheken konfiguriert sind, die Sie für den Einstieg in die Entwicklung Ihrer Lösungen für die Echtzeitkommunikation verwenden können.

Wir freuen uns auf Ihr Feedback, das Sie uns auf der [Azure Communication Services-GitHub-Seite](https://github.com/Azure/communication) zukommen lassen können.

In diesem Tutorial lernen Sie Folgendes:
> [!div class="checklist"]
> * Konfigurieren der Entwicklungsumgebung
> * Einrichten eines lokalen Webservers
> * Hinzufügen der Azure Communication Services-Pakete zu Ihrer Website
> * Veröffentlichen Ihrer Website auf statischen Azure-Websites

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. Details finden Sie unter [kostenloses Azure-Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F). Beachten Sie hierbei Folgendes: Beim kostenlosen Konto erhalten Sie eine Azure-Gutschrift in Höhe von 200 US-Dollar, damit Sie Kombinationen verschiedener Dienste ausprobieren können.
- [Visual Studio Code](https://code.visualstudio.com/): Wird hier verwendet, um Code in Ihrer lokalen Entwicklungsumgebung zu bearbeiten.
- [Webpack](https://webpack.js.org/): Wird verwendet, um Ihren Code zu bündeln und lokal zu hosten.
- [Node.js](https://nodejs.org/en/): Wird zum Installieren und Verwalten von Abhängigkeiten verwendet, z. B. Azure Communication Services-Clientbibliotheken und Webpack.
- [nvm und npm](https://docs.microsoft.com/windows/nodejs/setup-on-windows) zum Verarbeiten der Versionskontrolle.
- [Azure Storage-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurestorage) für Visual Studio Code. Diese Erweiterung wird benötigt, um Ihre Anwendung in Azure Storage zu veröffentlichen. Informieren Sie sich über das [Hosten von statischen Websites in Azure Storage](https://docs.microsoft.com/azure/storage/blobs/storage-blob-static-website).
- Die [Azure App Service-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice). Die Erweiterung ermöglicht die Bereitstellung von Websites (ähnlich wie im vorherigen Fall), verfügt aber zusätzlich über die Option zum Konfigurieren von vollständig verwalteter Continuous Integration und Continuous Delivery (CI/CD).
- Die [Azure Functions-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) zum Entwickeln Ihrer eigenen serverlosen Anwendungen. Beispielsweise können Sie Ihre Authentifizierungsanwendung in Azure Functions hosten.
- Eine aktive Communication Services-Ressource und eine Verbindungszeichenfolge. [Erstellen Sie eine Communication Services-Ressource](../quickstarts/create-communication-resource.md).
- Ein Benutzerzugriffstoken. Eine Anleitung finden Sie unter [Schnellstart: Erstellen und Verwalten von Zugriffstoken](https://docs.microsoft.com/azure/communication-services/quickstarts/access-tokens?pivots=programming-language-javascript) oder im Tutorial zum Thema [Erstellen eines vertrauenswürdigen Authentifizierungsdiensts mithilfe von Azure Functions](https://docs.microsoft.com/azure/communication-services/tutorials/trusted-service-tutorial).


## <a name="configure-your-development-environment"></a>Konfigurieren der Entwicklungsumgebung

Ihre lokale Entwicklungsumgebung wird wie folgt konfiguriert:

:::image type="content" source="./media/step-one-pic-one.png" alt-text="Architektur der Entwicklungsumgebung":::


### <a name="install-nodejs-nvm-and-npm"></a>Installieren von Node.js, nvm und npm

Wir verwenden Node.js, um verschiedene Abhängigkeiten herunterzuladen und zu installieren, die wir für unsere clientseitige Anwendung benötigen. Hiermit werden statische Dateien generiert, die von uns dann in Azure gehostet werden, sodass Sie sich nicht um die entsprechende Konfiguration auf Ihrem Server kümmern müssen.

Windows-Entwickler können [dieses Node.js-Tutorial](https://docs.microsoft.com/windows/nodejs/setup-on-windows) durcharbeiten, um Node, nvm und npm zu konfigurieren. 

Zum Testen dieses Tutorials haben wir Version 12.20.0 von LTS verwendet. Verwenden Sie nach der Installation von nvm den folgenden PowerShell-Befehl, um die Version bereitzustellen, die Sie nutzen möchten:

```PowerShell
nvm list available
nvm install 12.20.0
nvm use 12.20.0
```

:::image type="content" source="./media/step-one-pic-two.png" alt-text="Verwenden von nvm zum Bereitstellen von Node.js":::

### <a name="configure-visual-studio-code"></a>Konfigurieren von Visual Studio Code

Sie können [Visual Studio Code](https://code.visualstudio.com/) über eine der [unterstützten Plattformen](https://code.visualstudio.com/docs/supporting/requirements#_platforms) herunterladen.

### <a name="create-a-workspace-for-your-azure-communication-services-projects"></a>Erstellen eines Arbeitsbereichs für Ihre Azure Communication Services-Projekte

Erstellen Sie einen neuen Ordner zum Speichern Ihrer Projektdateien, z. B.: `C:\Users\Documents\ACS\CallingApp`. Klicken Sie in Visual Studio Code auf „Datei“ > „Add Folder to Workspace“ (Ordner dem Arbeitsbereich hinzufügen), und fügen Sie den Ordner dann Ihrem Arbeitsbereich hinzu.

:::image type="content" source="./media/step-one-pic-three.png" alt-text="Erstellen eines neuen Arbeitsbereichs":::

Wenn Sie in Visual Studio Code im linken Bereich zu „Explorer“ navigieren, wird Ihr Ordner „CallingApp“ im Arbeitsbereich „Unbenannt“ angezeigt.

:::image type="content" source="./media/step-one-pic-four.png" alt-text="Explorer":::

Es ist für Sie jederzeit möglich, den Namen Ihres Arbeitsbereichs zu ändern. Sie können Ihre Node.js-Version überprüfen, indem Sie mit der rechten Maustaste auf Ihren Ordner „CallingApp“ klicken und die Option „Open in Integrated Terminal“ (Im integrierten Terminal öffnen) auswählen.

:::image type="content" source="./media/step-one-pic-five.png" alt-text="Öffnen eines Terminals":::

Geben Sie im Terminal den folgenden Befehl ein, um die im vorherigen Schritt installierte Node.js-Version zu überprüfen:

```JavaScript
node --version
```

:::image type="content" source="./media/step-one-pic-six.png" alt-text="Überprüfen der Node.js-Version":::

### <a name="install-azure-extensions-for-visual-studio-code"></a>Installieren von Azure-Erweiterungen für Visual Studio Code

Installieren Sie die [Azure Storage-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurestorage) entweder über den Visual Studio Marketplace oder mit Visual Studio Code („Ansicht“ > „Erweiterungen“ > „Azure Storage“).

:::image type="content" source="./media/step-one-pic-seven.png" alt-text="Installieren der Azure Storage-Erweiterung 1":::

Führen Sie die gleichen Schritte für [Azure Functions](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions)- und [Azure App Service](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice)-Erweiterungen aus.


## <a name="set-up-a-local-webserver"></a>Einrichten eines lokalen Webservers

### <a name="create-a-new-npm-package"></a>Erstellen eines neuen npm-Pakets

Geben Sie im Terminal unter dem Pfad Ihres Arbeitsbereichsordners Folgendes ein:

``` console
npm init -y
```

Mit diesem Befehl wird ein neues npm-Paket initialisiert und `package.json` dem Stammordner Ihres Projekts hinzugefügt.

:::image type="content" source="./media/step-one-pic-eight.png" alt-text="package.json":::

Weitere Dokumentation zum Befehl „npm init“ finden Sie [hier](https://docs.npmjs.com/cli/v6/commands/npm-init).

### <a name="install-webpack"></a>Installieren von Webpack

Mit [Webpack](https://webpack.js.org/) können Sie Code in statischen Paketdateien zusammenfassen, die Sie in Azure bereitstellen können. Darüber hinaus ist hierbei ein Entwicklungsserver vorhanden, den wir für das Anrufbeispiel konfigurieren.

Geben Sie im Terminal Folgendes ein, um Webpack zu installieren:

``` Console
npm install webpack@4.42.0 webpack-cli@3.3.11 webpack-dev-server@3.10.3 --save-dev
```

Dieses Tutorial wurde mit den oben angegebenen Versionen getestet. Die Angabe von `-dev` ist ein Hinweis für den Paket-Manager, dass diese Abhängigkeit für Entwicklungszwecke bestimmt ist und nicht Teil des Codes sein sollte, den wir in Azure bereitstellen.

Sie sehen, dass Ihrer Datei `package.json` zwei neue Pakete als „devDependencies“ hinzugefügt werden. Die Pakete werden im Verzeichnis `./CallingApp/node_modules/` installiert.

:::image type="content" source="./media/step-one-pic-ten.png" alt-text="Webpack-Konfiguration":::

### <a name="configure-the-development-server"></a>Konfigurieren des Entwicklungsservers

Beim Ausführen einer statischen Anwendung (z. B. Ihrer Datei `index.html`) über Ihren Browser wird das `file://`-Protokoll verwendet. Damit Ihre npm-Module richtig funktionieren, benötigen wir das HTTP-Protokoll. Hierfür wird Webpack als lokaler Entwicklungsserver genutzt.

Wir erstellen zwei Konfigurationen: eine für die Entwicklung und die andere für die Produktion. Für die Produktion vorbereitete Dateien werden verkleinert. Dies bedeutet, dass nicht verwendete Leerzeichen bzw. andere Zeichen entfernt werden. Dies eignet sich für Produktionsszenarien, in denen die Latenz minimiert oder Code verschleiert werden soll.

Wir verwenden das Tool `webpack-merge`, um [unterschiedliche Konfigurationsdateien für Webpack](https://webpack.js.org/guides/production/) zu nutzen.

Wir beginnen nun mit der Entwicklungsumgebung. Zunächst müssen wir `webpack merge` installieren. Führen Sie in Ihrem Terminal Folgendes aus:

```Console
npm install --save-dev webpack-merge
```

In Ihrer Datei `package.json` sehen Sie, dass „devDependencies“ eine oder mehrere Abhängigkeiten hinzugefügt wurden.

Im nächsten Schritt müssen wir die neue Datei `webpack.common.js` erstellen und den folgenden Code hinzufügen:

```JavaScript
const path = require('path');
module.exports ={
    entry: './app.js',
    output: {
        filename:'app.js',
        path: path.resolve(__dirname, 'dist'),
    }     
}
```

Anschließend fügen wir zwei weitere Dateien hinzu (eine für jede Konfiguration):

* webpack.dev.js
* webpack.prod.js

Im nächsten Schritt müssen wir die Datei `webpack.dev.js` ändern. Fügen Sie dieser Datei den folgenden Code hinzu:

```JavaScript
const { merge } = require('webpack-merge');
const common = require('./webpack.common.js');

module.exports = merge(common, {
    mode: 'development',
    devtool: 'inline-source-map',
});
```
Bei dieser Konfiguration importieren wir häufig verwendete Parameter aus `webpack.common.js`, führen die beiden Dateien zusammen, legen den Modus „development“ fest und konfigurieren „SourceMap“ als „inline-source-map“.

Im Entwicklungsmodus wird Webpack angewiesen, die Dateien nicht zu verkleinern und keine optimierten Produktionsdateien zu erstellen. Eine ausführliche Dokumentation zu den Webpack-Modi finden Sie [hier](https://webpack.js.org/configuration/mode/).

Eine Liste mit den Optionen für die Quellzuordnung (Source Map) finden Sie [hier](https://webpack.js.org/configuration/devtool/#root). Das Festlegen der Quellzuordnung erleichtert Ihnen das Debuggen über Ihren Browser.

:::image type="content" source="./media/step-one-pic-11.png" alt-text="Konfigurieren von Webpack":::

Navigieren Sie zum Ausführen des Entwicklungsservers zu `package.json`, und fügen Sie unter „scripts“ den folgenden Code hinzu:

```JavaScript
    "build:dev": "webpack-dev-server --config webpack.dev.js"
```

Ihre Datei sollte nun wie folgt aussehen:

```JavaScript
{
  "name": "CallingApp",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build:dev": "webpack-dev-server --config webpack.dev.js"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "webpack": "^4.42.0",
    "webpack-cli": "^3.3.11",
    "webpack-dev-server": "^3.10.3"
  }
}
```

Sie haben den Befehl hinzugefügt, der über npm verwendet werden kann. 

:::image type="content" source="./media/step-one-pic-12.png" alt-text="Ändern von „package.json“":::

### <a name="testing-the-development-server"></a>Testen des Entwicklungsservers

 Erstellen Sie in Visual Studio Code in Ihrem Projekt drei Dateien:

* `index.html`
* `app.js`
* `app.css` (optional, Formatvorlage für Ihre App)

Fügen Sie Folgendes in `index.html` ein:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My first ACS application</title>
    <link rel="stylesheet" href="./app.css"/>
    <script src="./app.js" defer></script>
</head>
<body>
    <h1>Hello from ACS!</h1>
</body>
</html>
```
:::image type="content" source="./media/step-one-pic-13.png" alt-text="HTML-Datei":::

Fügen Sie den folgenden Code zu `app.js` hinzu:

```JavaScript
alert('Hello world alert!');
console.log('Hello world console!');
```
Fügen Sie den folgenden Code zu `app.css` hinzu:

```CSS
html {
    font-family: sans-serif;
  }
```
Vergessen Sie das Speichern nicht! Die nicht gespeicherte Datei ist durch weiße Punkte neben den Dateinamen im Datei-Explorer gekennzeichnet.

 :::image type="content" source="./media/step-one-pic-14.png" alt-text="Datei „app.js“ mit JS-Code":::

Wenn Sie diese Seite öffnen, sollte Ihre Meldung mit einer Warnung und in der Konsole Ihres Browsers angezeigt werden.

:::image type="content" source="./media/step-one-pic-15.png" alt-text="Datei „app.css“":::

Verwenden Sie den folgenden Terminalbefehl, um Ihre Entwicklungskonfiguration zu testen:

```Console
npm run build:dev
```

In der Konsole wird angezeigt, wo der Server ausgeführt wird. Dieser lautet standardmäßig `http://localhost:8080`. Der Befehl „build:dev“ ist der Befehl, den wir weiter oben der Datei `package.json` hinzugefügt haben.

 :::image type="content" source="./media/step-one-pic-16.png" alt-text="Starten eines Entwicklungsservers":::
 
 Navigieren Sie in Ihrem Browser zu der Adresse. Es sollte die Seite mit der Warnung angezeigt werden, die Sie in den obigen Schritten konfiguriert haben.
 
  :::image type="content" source="./media/step-one-pic-17.png" alt-text="HTML-Seite":::
  
 
Während der Server ausgeführt wird, können Sie den Code ändern. Der Server und die HTML-Seite werden automatisch neu geladen. 

Navigieren Sie als Nächstes zur Datei `app.js` in Visual Studio Code, und löschen Sie `alert('Hello world alert!');`. Speichern Sie Ihre Datei, und vergewissern Sie sich, dass die Warnung nicht mehr in Ihrem Browser angezeigt wird.

Zum Beenden Ihres Servers können Sie in Ihrem Terminal `Ctrl+C` verwenden. Sie können jederzeit `npm run build:dev` eingeben, um Ihren Server zu starten.

## <a name="add-the-azure-communication-services-packages"></a>Hinzufügen der Azure Communication Services-Pakete

Verwenden Sie den Befehl `npm install`, um die Clientbibliothek für Telefonie von Azure Communication Services für JavaScript zu installieren.

```Console
npm install @azure/communication-common --save
npm install @azure/communication-calling --save
```

Mit dieser Aktion werden die allgemeinen und Aufrufpakete von Azure Communication Services als Abhängigkeiten Ihres Pakets hinzugefügt. Sie sehen, dass der Datei `package.json` zwei neue Pakete hinzugefügt werden. Weitere Informationen zum Befehl `npm install` finden Sie [hier](https://docs.npmjs.com/cli/v6/commands/npm-install).

:::image type="content" source="./media/step-one-pic-nine.png" alt-text="Installieren von Azure Communication Services-Paketen":::

Diese Pakete werden vom Azure Communication Services-Team bereitgestellt und enthalten die Authentifizierungs- und Anrufbibliotheken. Mit dem Befehl „--save“ wird signalisiert, dass unsere Anwendung für die Nutzung in der Produktion von diesen Paketen abhängig ist und dass sie in den `dependencies` unserer `package.json`-Datei enthalten sind. Wenn wir die Anwendung für die Produktion erstellen, werden die Pakete in den Produktionscode eingefügt.


## <a name="publish-your-website-to-azure-static-websites"></a>Veröffentlichen Ihrer Website auf statischen Azure-Websites

### <a name="create-a-configuration-for-production-deployment"></a>Erstellen einer Konfiguration für die Produktionsbereitstellung

Fügen Sie der Datei `webpack.prod.js` den folgenden Code hinzu:

```JavaScript
const { merge } = require('webpack-merge');
 const common = require('./webpack.common.js');

 module.exports = merge(common, {
   mode: 'production',
 });
 ```

Beachten Sie, dass diese Konfiguration mit der Datei „webpack.common.js“ (mit der Angabe der Eingabedatei und zum Speichern der Ergebnisse) zusammengeführt und dass der Modus „production“ festgelegt wird.
 
Fügen Sie in `package.json` den folgenden Code hinzu:

```JavaScript
"build:prod": "webpack --config webpack.prod.js" 
```

Ihre Datei sollte wie folgt aussehen:

```JavaScript
{
  "name": "CallingApp",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build:dev": "webpack-dev-server --config webpack.dev.js",
    "build:prod": "webpack --config webpack.prod.js" 
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "@azure/communication-calling": "^1.0.0-beta.3",
    "@azure/communication-common": "^1.0.0-beta.3"
  },
  "devDependencies": {
    "webpack": "^4.42.0",
    "webpack-cli": "^3.3.11",
    "webpack-dev-server": "^3.10.3",
    "webpack-merge": "^5.7.3"
  }
}
```

 :::image type="content" source="./media/step-one-pic-20.png" alt-text="Konfigurierte Dateien":::


Führen Sie im Terminal Folgendes aus:

```Console
npm run build:prod
```

Mit dem Befehl wird der Ordner `dist` und darin dann die für die Produktion geeignete statische Datei `app.js` erstellt. 

 :::image type="content" source="./media/step-one-pic-21.png" alt-text="Produktionsbuild":::
 
 
### <a name="deploy-your-app-to-azure-storage"></a>Bereitstellen Ihrer App in Azure Storage
 
Kopieren Sie `index.html` und `app.css` in den Ordner `dist`.

Erstellen Sie im Ordner `dist` eine neue Datei, und geben Sie ihr den Namen `404.html`. Kopieren Sie den folgenden Markupcode in diese Datei:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="./app.css"/>
    <title>Document</title>
</head>
<body>
    <h1>The page does not exists.</h1>
</body>
</html>
```

Speichern Sie die Datei (STRG+S).

Klicken Sie mit der rechten Maustaste, und wählen Sie die Option „Deploy to Static Website via Azure Storage“ (Über Azure Storage auf statischer Website bereitstellen) aus.

:::image type="content" source="./media/step-one-pic-22.png" alt-text="Starten der Bereitstellung in Azure":::
 
Wählen Sie im Feld `Select subscription` die Option „Bei Azure anmelden“ aus (oder „Kostenloses Azure-Konto erstellen“, falls Sie noch kein Abonnement erstellt haben).
 
:::image type="content" source="./media/step-one-pic-23.png" alt-text="Anmelden bei Azure":::
 
Wählen Sie `Create new Storage Account` > `Advanced` aus:

 :::image type="content" source="./media/step-one-pic-24.png" alt-text="Erstellen der Speicherkontogruppe":::
 
 Geben Sie den Namen der Speichergruppe an:
 
 :::image type="content" source="./media/step-one-pic-25.png" alt-text="Hinzufügen eines Namens für das Konto":::
 
Erstellen Sie bei Bedarf eine neue Ressourcengruppe:
 
  :::image type="content" source="./media/step-one-pic-26.png" alt-text="Erstellen einer neuen Gruppe":::
  
  Beantworten Sie die Frage „Would you like to enable static website hosting?“ (Möchten Sie das Hosten von statischen Websites aktivieren?) mit „Ja“.
  
  :::image type="content" source="./media/step-one-pic-27.png" alt-text="Auswählen der Option zum Aktivieren des Hostens von statischen Websites":::
  
Akzeptieren Sie den Standarddateinamen unter „Enter the index document name“ (Namen des Indexdokuments eingeben), da wir die Datei `index.html` erstellt haben.

Geben Sie `404.html` unter „Enter the 404 error document path“ (Pfad für 404-Fehlerdokument eingeben) ein.  
  
Wählen Sie den Standort der Anwendung aus. Anhand des von Ihnen ausgewählten Speicherorts wird definiert, welcher Medienprozessor in Ihrer zukünftigen Anrufanwendung bei Gruppenanrufen verwendet wird. 

Der Medienprozessor wird von Azure Communication Services basierend auf dem Standort der Anwendung ausgewählt.

:::image type="content" source="./media/step-one-pic-28.png" alt-text="Auswählen des Standorts":::
  
Warten Sie ab, bis die Ressource und Ihre Website erstellt wurden. 
 
Klicken Sie auf „Browse to website“ (Zur Website wechseln):

:::image type="content" source="./media/step-one-pic-29.png" alt-text="Bereitstellung abgeschlossen":::
 
In den Entwicklungstools Ihres Browsers können Sie die Quelle untersuchen und unsere für die Produktion vorbereitete Datei anzeigen.
 
:::image type="content" source="./media/step-one-pic-30.png" alt-text="Website":::

Wählen Sie im [Azure-Portal](https://portal.azure.com/#home) Ihre Ressourcengruppe und dann die von Ihnen erstellte Anwendung aus, und navigieren Sie zu `Settings` > `Static website` („Einstellungen“ > „Statische Website“). Sie sehen, dass die Verwendung von statischen Websites aktiviert ist und dass die Dateien für den primären Endpunkt, das Indexdokument und das Fehlerpfaddokument angezeigt werden.

:::image type="content" source="./media/step-one-pic-31.png" alt-text="Ausgewählte Option „Statische Website“":::

Wenn Sie unter „Blob-Dienst“ die Option „Container“ auswählen, werden zwei Container erstellt: einer für Protokolle ($logs) und einer für den Inhalt Ihrer Website ($web).

:::image type="content" source="./media/step-one-pic-32.png" alt-text="Containerkonfiguration":::

Wenn Sie zu `$web` navigieren, werden Ihre Dateien angezeigt, die Sie in Visual Studio erstellt und in Azure bereitgestellt haben. 

:::image type="content" source="./media/step-one-pic-33.png" alt-text="Bereitstellung":::

Sie können die Anwendung über Visual Studio Code jederzeit erneut bereitstellen.

Sie können nun Ihre erste Azure Communication Services-Webanwendung erstellen.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Hinzufügen von Sprachanrufen zu Ihrer App](../quickstarts/voice-video-calling/getting-started-with-calling.md)

Das könnte Sie auch interessieren:

- [Hinzufügen von Chatfunktionen zu Ihrer App](../quickstarts/chat/get-started.md)
- [Erstellen von Benutzerzugriffstoken](../quickstarts/access-tokens.md)
- [Erfahren Sie mehr über die Client- und Serverarchitektur](../concepts/client-and-server-architecture.md)
- [Informationen zur Authentifizierung](../concepts/authentication.md)
