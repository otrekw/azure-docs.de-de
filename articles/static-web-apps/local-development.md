---
title: Einrichten der lokalen Entwicklung für Azure Static Web Apps
description: Es wird beschrieben, wie Sie Ihre lokale Entwicklungsumgebung für Azure Static Web Apps einrichten.
services: static-web-apps
author: burkeholland
ms.service: static-web-apps
ms.topic: how-to
ms.date: 05/08/2020
ms.author: buhollan
ms.custom: devx-track-js
ms.openlocfilehash: 4d6dae8a4f4ed83af3103e95e711bacdb62cf522
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "91326166"
---
# <a name="set-up-local-development-for-azure-static-web-apps-preview"></a>Einrichten der lokalen Entwicklung für Azure Static Web Apps (Vorschau)

Eine Instanz von Azure Static Web Apps besteht aus zwei verschiedenen Arten von Anwendungen. Die erste Anwendungsart ist eine Web-App für Ihren statischen Inhalt. Web-Apps werden häufig mit Front-End-Frameworks und -Bibliotheken oder mit Generatoren für statische Websites erstellt. Die zweite Art ist die API. Hierbei handelt es sich um eine Azure Functions-App mit einer umfassenden Back-End-Entwicklungsumgebung.

Bei der Ausführung in der Cloud ordnet Azure Static Web Apps Anforderungen nahtlos der `api`-Route von der Web-App zur Azure Functions-App zu, ohne dass eine CORS-Konfiguration erforderlich ist. Sie müssen Ihre Anwendung lokal so konfigurieren, dass dieses Verhalten widergespiegelt wird.

In diesem Artikel werden die empfohlenen bewährten Methoden für die lokale Entwicklung veranschaulicht, z. B. die folgenden Konzepte:

- Einrichten der Web-App für statischen Inhalt
- Konfigurieren der Azure Functions-App für die API Ihrer Anwendung
- Debuggen und Ausführen der Anwendung
- Bewährte Methoden für die Datei- und Ordnerstruktur Ihrer App

## <a name="prerequisites"></a>Voraussetzungen

- [Visual Studio Code](https://code.visualstudio.com/)
- [Azure Functions-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) für Visual Studio Code
- [Live Server-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer) für Visual Studio Code
  - Nur erforderlich, wenn Sie keine CLI eines Front-End-JavaScript-Frameworks oder eines Generators für statische Websites verwenden.

## <a name="run-projects-locally"></a>Lokales Ausführen von Projekten

Die lokale Ausführung einer statischen Azure-Web-App umfasst drei Prozesse – je nachdem, ob Ihr Projekt eine API enthält.

- Ausführen eines lokalen Webservers
- Ausführen der API
- Verbinden des Webprojekts mit der API

Je nachdem, wie eine Website erstellt wird, ist ggf. ein lokaler Webserver erforderlich, um die Anwendung im Browser auszuführen. Bei Verwendung von Front-End-JavaScript-Frameworks und Generatoren für statische Websites ist diese Funktionalität in die entsprechenden CLIs (Command Line Interfaces, Befehlszeilenschnittstellen) integriert. Die folgenden Links verweisen jeweils auf die CLI-Referenz für einige Frameworks, Bibliotheken und Generatoren.

### <a name="javascript-frameworks-and-libraries"></a>JavaScript-Frameworks und -Bibliotheken

- [Angular-CLI](https://angular.io/cli)
- [Vue-CLI](https://cli.vuejs.org/guide/creating-a-project.html)
- [React-CLI](https://create-react-app.dev/)

### <a name="static-site-generators"></a>Generatoren für statische Websites

- [Gatsby-CLI](https://www.gatsbyjs.org/docs/gatsby-cli/)
- [Hugo](https://gohugo.io/getting-started/quick-start/)
- [Jekyll](https://jekyllrb.com/docs/usage/)

Wenn Sie für die Bereitstellung Ihrer Website ein CLI-Tool nutzen, können Sie zum Abschnitt [Ausführen der API](#run-api-locally) springen.

### <a name="running-a-local-web-server-with-live-server"></a>Ausführen eines lokalen Webservers mit Live Server

Die Live Server-Erweiterung für Visual Studio Code verfügt über einen Webserver für die lokale Entwicklung, mit dem statischer Inhalt bereitgestellt wird.

#### <a name="create-a-repository"></a>Erstellen eines Repositorys

1. Stellen Sie sicher, dass Sie bei GitHub angemeldet sind. Navigieren Sie zu [https://github.com/staticwebdev/vanilla-api/generate](https://github.com/staticwebdev/vanilla-api/generate), und erstellen Sie mit dieser Vorlage ein neues GitHub-Projekt namens **vanilla-api**.

    :::image type="content" source="media/local-development/vanilla-api.png" alt-text="GitHub-Fenster für neues Repository":::

1. Öffnen Sie Visual Studio Code.

1. Drücken Sie **F1**, um die Befehlspalette zu öffnen.

1. Geben Sie im Suchfeld den Suchbegriff **clone** ein, und wählen Sie **Git: Clone** aus.

    :::image type="content" source="media/local-development/command-palette-git-clone.png" alt-text="Option „git clone“ in Visual Studio Code":::

1. Geben Sie den folgenden Wert als **Repository-URL** ein.

   ```http
   git@github.com:<YOUR_GITHUB_ACCOUNT>/vanilla-api.git
   ```

1. Wählen Sie für das neue Projekt einen Ordnerspeicherort aus.

1. Wählen Sie **Öffnen** aus, wenn Sie zum Öffnen des geklonten Repositorys aufgefordert werden.

    :::image type="content" source="media/local-development/open-new-window.png" alt-text="In neuem Fenster öffnen":::

Visual Studio Code öffnet das geklonte Projekt im Editor.

### <a name="run-the-website-locally-with-live-server"></a>Lokales Ausführen der Website mit Live Server

1. Drücken Sie **F1**, um die Befehlspalette zu öffnen.

1. Geben Sie im Suchfeld den Suchbegriff **Live Server** ein, und wählen Sie **Live Server: Open with Live Server** (Live Server: Mit Live Server öffnen) aus.

    Im Browser wird ein Tab geöffnet, um die Anwendung anzuzeigen.

    :::image type="content" source="media/local-development/vanilla-api-site.png" alt-text="Einfache statische Website, die im Browser ausgeführt wird":::

    Diese Anwendung sendet eine HTTP-Anforderung an den Endpunkt `api/message`. Derzeit tritt für diese Anforderung ein Fehler auf, weil der API-Teil der Anwendung gestartet werden muss.

### <a name="run-api-locally"></a>Lokales Ausführen der API

APIs für Azure Static Web Apps basieren auf Azure Functions. Ausführliche Informationen zum Hinzufügen einer API zu einem Azure Static Web Apps-Projekt finden Sie unter [Hinzufügen einer API zu Azure Static Web Apps mit Azure Functions](add-api.md).

Beim API-Erstellungsprozess wird für Visual Studio Code eine Startkonfiguration erstellt. Diese Konfiguration befindet sich im Ordner _.vscode_. Dieser Ordner enthält alle erforderlichen Einstellungen für das lokale Entwickeln und Ausführen der API.

1. Drücken Sie in Visual Studio Code die Taste **F5**, um die API zu starten.

1. Es wird eine neue Terminalinstanz geöffnet, in der die Ausgabe des API-Buildprozesses angezeigt wird.

    :::image type="content" source="media/local-development/terminal-api-debug.png" alt-text="Ausführung der API im Visual Studio Code-Terminal":::

   Die Statusleiste in Visual Studio Code ist jetzt orange gekennzeichnet. Diese Farbe gibt an, dass die API nun ausgeführt wird und der Debugger angefügt wurde.

1. Drücken Sie als Nächstes **STRG/CMD**, und klicken Sie im Terminal auf die URL, um ein Browserfenster zum Aufrufen der API zu öffnen.

    :::image type="content" source="media/local-development/hello-from-api-endpoint.png" alt-text="Browseranzeige mit dem Ergebnis des API-Aufrufs":::

### <a name="debugging-the-api"></a>Debuggen der API

1. Öffnen Sie die Datei _api/GetMessage/index.js_ in Visual Studio Code.

1. Klicken Sie in Zeile 2 auf den linken Randbereich, um einen Breakpoint festzulegen. Ein roter Punkt wird angezeigt, um anzugeben, dass der Breakpoint festgelegt wurde.

    :::image type="content" source="media/local-development/breakpoint-set.png" alt-text="Breakpoint in Visual Studio Code":::

1. Aktualisieren Sie im Browser die Seite, die unter <http://127.0.0.1:7071/api/message> ausgeführt wird.

1. Der Breakpoint wird in Visual Studio Code erreicht, und die Ausführung des Programms wird angehalten.

   :::image type="content" source="media/local-development/breakpoint-hit.png" alt-text="Erreichter Breakpoint in Visual Studio Code":::

   Eine vollständige [Benutzeroberfläche zum Debuggen Ihrer API ist in Visual Studio Code verfügbar](https://code.visualstudio.com/Docs/editor/debugging).

1. Wählen Sie in der Debugleiste die Schaltfläche **Weiter** aus, um die Ausführung fortzusetzen.

    :::image type="content" source="media/local-development/continue-button.png" alt-text="Schaltfläche „Weiter“ in Visual Studio Code":::

### <a name="calling-the-api-from-the-application"></a>Aufrufen der API aus der Anwendung

Nach der Bereitstellung werden diese Anforderungen von Azure Static Web Apps automatisch den Endpunkten im Ordner _api_ zugeordnet. Mit dieser Zuordnung wird sichergestellt, dass Anforderungen von der Anwendung an die API wie im folgenden Beispiel aussehen.

```javascript
let response = await fetch("/api/message");
```

Je nachdem, ob Ihre Anwendung mit einer JavaScript-Framework-CLI oder auf andere Weise erstellt wurde, gibt es zwei Konfigurationsmöglichkeiten für den Pfad zur `api`-Route, wenn Ihre Anwendung lokal ausgeführt wird.

- Konfigurationsdateien für die Umgebung (empfohlen für JavaScript-Frameworks und -Bibliotheken)
- Lokaler Proxy

### <a name="environment-configuration-files"></a>Konfigurationsdateien für die Umgebung

Wenn Sie Ihre App mit Front-End-Frameworks erstellen, die über eine CLI verfügen, sollten Sie Konfigurationsdateien für die Umgebung nutzen. Diese Konfigurationsdateien für die Umgebung werden in jedem Framework bzw. jeder Bibliothek anders verarbeitet. Normalerweise wird eine Konfigurationsdatei für die Entwicklung eingesetzt, wenn Ihre Anwendung lokal ausgeführt wird. Eine andere Konfigurationsdatei wird für die Produktion genutzt, wenn Ihre Anwendung in der Produktion ausgeführt wird. Die von Ihnen verwendete CLI für das JavaScript-Framework oder den Generator für statische Websites weiß automatisch, wann die Datei für die lokale Entwicklung und wann die Produktionsdatei verwendet werden muss, wenn Ihre App mit Azure Static Web Apps erstellt wird.

In der Konfigurationsdatei für die Entwicklung können Sie den Pfad zur API angeben, der auf den lokalen Speicherort `http:127.0.0.1:7071` verweist, an dem die API für Ihre Website lokal ausgeführt wird.

```
API=http:127.0.0.1:7071/api
```

Geben Sie in der Konfigurationsdatei für die Produktion den Pfad zur API als `api` an. Ihre Anwendung ruft die API bei Ausführung in der Produktion dann über „yoursite.com/api“ auf.

```
API=api
```

Auf diese Konfigurationswerte kann im JavaScript-Code der Web-App in Form von Node-Umgebungsvariablen verwiesen werden.

```js
let response = await fetch(`${process.env.API}/message`);
```

Wenn die CLI für die Ausführung Ihrer Website im Entwicklungsmodus oder zum Erstellen der Website für die Produktion genutzt wird, wird der Wert `process.env.API` durch den Wert aus der entsprechenden Konfigurationsdatei ersetzt.

Weitere Informationen zum Konfigurieren von Umgebungsdateien für Front-End-JavaScript-Frameworks und -Bibliotheken finden Sie in den folgenden Artikeln:

- [Angular-Umgebungsvariablen](https://angular.io/guide/build#configuring-application-environments)
- [React: Hinzufügen von benutzerdefinierten Umgebungsvariablen](https://create-react-app.dev/docs/adding-custom-environment-variables/)
- [Vue: Modi und Umgebungsvariablen](https://cli.vuejs.org/guide/mode-and-env.html)

[!INCLUDE [static-web-apps-local-proxy](../../includes/static-web-apps-local-proxy.md)]

##### <a name="restart-live-server"></a>Neustarten von Live Server

1. Drücken Sie **F1**, um die Befehlspalette in Visual Studio Code zu öffnen.

1. Geben Sie **Live Server** ein, und wählen Sie **Live Server: Stop Live Server** (Live Server: Live Server beenden) aus.

    :::image type="content" source="media/local-development/stop-live-server.png" alt-text="Befehl zum Beenden von Live Server in der Visual Studio-Befehlspalette":::

1. Drücken Sie **F1**, um die Befehlspalette zu öffnen.

1. Geben Sie **Live Server** ein, und wählen Sie **Live Server: Mit Live-Server öffnen** aus.

1. Aktualisieren Sie die Anwendung, die unter `http://locahost:3000` ausgeführt wird. Im Browser wird jetzt die Meldung angezeigt, die von der API zurückgegeben wurde.

    :::image type="content" source="media/local-development/hello-from-api.png" alt-text="Anzeige von „Hello from API“ im Browser":::

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Konfigurieren von App-Einstellungen](application-settings.md)
