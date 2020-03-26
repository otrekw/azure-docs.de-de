---
title: 'Schnellstart: Erstellen einer Node.js-Web-App'
description: Stellen Sie in wenigen Minuten Ihre erste Node.js-App vom Typ „Hallo Welt“ in Azure App Service bereit. Die Bereitstellung erfolgt mithilfe von Visual Studio Code. Dies ist eine von zahlreichen verschiedenen Möglichkeiten für die Bereitstellung in App Service.
ms.assetid: 582bb3c2-164b-42f5-b081-95bfcb7a502a
ms.topic: quickstart
ms.date: 03/04/2020
ms.custom: mvc, devcenter, seodec18
experimental: false
experiment_id: a231f2b4-2625-4d
ms.openlocfilehash: bcb0ae2457175e7511c3539187d72d48e80baa5b
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/26/2020
ms.locfileid: "80047132"
---
# <a name="create-a-nodejs-web-app-in-azure"></a>Erstellen einer Node.js-Web-App in Azure 

Beginnen Sie mit der Verwendung von Azure App Service, indem Sie lokal eine Node.js-/Express-App mit Visual Studio Code erstellen und die App anschließend in der Cloud bereitstellen. Da Sie einen kostenlosen App Service-Tarif verwenden, fallen bei dieser Schnellstartanleitung keine Kosten an.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-app-service-extension&mktingSource=vscode-tutorial-app-service-extension).
- [Node.js und npm](https://nodejs.org). Führen Sie den Befehl `node --version` aus, um sich zu vergewissern, dass Node.js installiert ist.
- [Visual Studio Code](https://code.visualstudio.com/)
- Die [Azure App Service-Erweiterung](vscode:extension/ms-azuretools.vscode-azureappservice) für Visual Studio Code.

## <a name="clone-and-run-a-local-nodejs-application"></a>Klonen und Ausführen einer lokalen Node.js-Anwendung

1. Öffnen Sie auf Ihrem lokalen Computer ein Terminal, und klonen Sie das Beispielrepository:

    ```bash
    git clone https://github.com/Azure-Samples/nodejs-docs-hello-world
    ```

1. Navigieren Sie zu dem neuen App-Ordner:

    ```bash
    cd nodejs-docs-hello-world
    ```

1. Starten Sie die App, um sie lokal zu testen:

    ```bash
    npm start
    ```
    
1. Navigieren Sie in Ihrem Browser zu [http://localhost:1337](http://localhost:1337). Im Browser sollte „Hello World!“ angezeigt werden.

1. Drücken Sie im Terminal **STRG**+**C**, um den Server zu beenden.

> [!div class="nextstepaction"]
> [Ich bin auf ein Problem gestoßen](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azure-app-service&step=create-app)

## <a name="deploy-the-app-to-azure"></a>Bereitstellen der Anwendung in Azure

In diesem Abschnitt wird Ihre Node.js-App mithilfe von VS Code und der Azure App Service-Erweiterung in Azure bereitgestellt.

1. Vergewissern Sie sich im Terminal, dass Sie sich im Ordner *nodejs-docs-hello-world* befinden, und starten Sie Visual Studio Code mit dem folgenden Befehl:

    ```bash
    code .
    ```

1. Wählen Sie auf der Aktivitätsleiste von VS Code das Azure-Logo aus, um den **AZURE APP SERVICE-Explorer** anzuzeigen. Wählen Sie **Bei Azure anmelden...** aus, und befolgen Sie die Anweisungen. (Sollte bei Ihnen ein Fehler auftreten, lesen Sie [Behandeln von Problemen bei der Azure-Anmeldung](#troubleshooting-azure-sign-in) weiter unten.) Nach der Anmeldung sollte im Explorer der Name Ihres Azure-Abonnements angezeigt werden.

    ![Anmelden bei Azure](containers/media/quickstart-nodejs/sign-in.png)

1. Wählen Sie im **AZURE APP SERVICE-Explorer** von VS Code den blauen Aufwärtspfeil aus, um Ihre App in Azure bereitzustellen. (Der gleiche Befehl kann auch über die **Befehlspalette** (**STRG**+**UMSCHALT**+**P**) aufgerufen werden. Geben Sie hierzu „In Web-App bereitstellen“ ein, und wählen Sie den Befehl **Azure App Service: In Web-App bereitstellen** aus.)

    ![Bereitstellen in Web-App](containers/media/quickstart-nodejs/deploy.png)
        
1. Wählen Sie den Ordner *nodejs-docs-hello-world* aus.

1. Wählen Sie eine Erstellungsoption basierend auf dem Betriebssystem aus, auf dem die Bereitstellung erfolgen soll:

    - Linux: Wählen Sie **Neue Web-App erstellen** aus.
    - Windows: Wählen Sie **Neue Web-App erstellen... Erweitert** aus.

1. Geben Sie einen global eindeutigen Namen für Ihre Web-App ein, und drücken Sie die **EINGABETASTE**. Der Name muss innerhalb von Azure eindeutig sein und darf nur alphanumerische Zeichen („A–Z“, „a–z“ und „0–9“) und Bindestriche („-“) enthalten.

1. Wenn Linux Ihre Zielplattform ist, wählen Sie eine Node.js-Version aus, wenn Sie dazu aufgefordert werden. Eine **LTS**-Version wird empfohlen.

1. Wenn Sie Windows als Zielplattform verwenden, führen Sie die folgenden Zusatzschritte aus:
    1. Wählen Sie **Neue Ressourcengruppe erstellen** aus, und geben Sie einen Namen für die Ressourcengruppe ein (beispielsweise `AppServiceQS-rg`).
    1. Wählen Sie als Betriebssystem **Windows** aus.
    1. Wählen Sie **Neuen App Services-Plan erstellen** aus, geben Sie einen Namen für den Plan ein (beispielsweise `AppServiceQS-plan`), und wählen Sie anschließend den Tarif **F1 Free** aus.
    1. Wählen Sie **Vorerst überspringen** aus, wenn eine Eingabeaufforderung zu Application Insights sehen.
    1. Wählen Sie eine Region in Ihrer Nähe oder in der Nähe von Ressourcen aus, auf die Sie zugreifen möchten.

1. Wenn Sie auf alle Eingabeaufforderungen reagiert haben, werden in VS Code in einem Benachrichtigungspopup die Azure-Ressourcen angezeigt, die für Ihre App erstellt werden.

    Wählen Sie bei der Bereitstellung unter Linux **Ja** aus, wenn Sie aufgefordert werden, Ihre Konfiguration für die Ausführung von `npm install` auf dem Linux-Zielserver zu aktualisieren.

    ![Aufforderung zum Aktualisieren der Konfiguration auf dem Linux-Zielserver](containers/media/quickstart-nodejs/server-build.png)

1. Wählen Sie **Ja** aus, wenn **Always deploy the workspace „nodejs-docs-hello-world“ to (app name)** (Arbeitsbereich „nodejs-docs-hello-world“ immer in (App-Name) bereitstellen) angezeigt wird. Durch Auswählen von **Ja** wird VS Code angewiesen, für nachfolgende Bereitstellungen automatisch die gleiche App Service-Web-App zu verwenden.

1. Wählen Sie bei einer Bereitstellung für Linux in der Aufforderung die Option **Website durchsuchen** aus, um nach Abschluss der Bereitstellung Ihre neu bereitgestellte Web-App anzuzeigen. Im Browser sollte „Hello World!“ angezeigt werden.

1. Legen Sie bei einer Bereitstellung für Windows zunächst die Node.js-Versionsnummer für die Web-App fest:

    1. Erweitern Sie in VS Code den Knoten für den neuen App-Dienst, klicken Sie mit der rechten Maustaste auf **Anwendungseinstellungen**, und wählen Sie anschließend **Neue Einstellung hinzufügen...** aus:

        ![Hinzufügen eines Befehls für eine App-Einstellung](containers/media/quickstart-nodejs/add-setting.png)

    1. Geben Sie `WEBSITE_NODE_DEFAULT_VERSION` für den Einstellungsschlüssel ein.
    1. Geben Sie `10.15.2` für den Einstellungswert ein.
    1. Klicken Sie mit der rechten Maustaste auf den Knoten für den App-Dienst, und wählen Sie **Neu starten** aus.

        ![Befehl zum Neustarten der App](containers/media/quickstart-nodejs/restart.png)

    1. Klicken Sie noch einmal mit der rechten Maustaste auf den Knoten für den App-Dienst, und wählen Sie **Website durchsuchen** aus.

> [!div class="nextstepaction"]
> [Ich bin auf ein Problem gestoßen](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azure-app-service&step=deploy-app)

### <a name="troubleshooting-azure-sign-in"></a>Behandeln von Problemen bei der Azure-Anmeldung

Sollte bei der Azure-Anmeldung der Fehler **Abonnement mit dem Namen [Abonnement-ID] nicht gefunden** angezeigt werden, befinden Sie sich möglicherweise hinter einem Proxy und können die Azure-API nicht erreichen. Konfigurieren Sie die Umgebungsvariablen `HTTP_PROXY` und `HTTPS_PROXY` mithilfe von `export` in Ihrem Terminal mit Ihren Proxyinformationen.

```bash
export HTTPS_PROXY=https://username:password@proxy:8080
export HTTP_PROXY=http://username:password@proxy:8080
```

Wenn sich das Problem durch Festlegen der Umgebungsvariablen nicht beheben lässt, wenden Sie sich an uns, indem Sie oben die Schaltfläche **Ich bin auf ein Problem gestoßen** auswählen.

### <a name="update-the-app"></a>Aktualisieren der App

Sie können Änderungen für diese App bereitstellen, indem Sie in VS Code Änderungen vornehmen, Ihre Dateien speichern und anschließend den gleichen Prozess verwenden wie vorhin, dabei aber keine neue App erstellen, sondern die vorhandene App auswählen.

## <a name="viewing-logs"></a>Anzeigen von Protokollen

Sie können sich die Protokollausgabe (Aufrufe von `console.log`) der App direkt im VS Code-Ausgabefenster ansehen.

1. Klicken Sie im **AZURE APP SERVICE-Explorer** mit der rechten Maustaste auf den App-Knoten, und wählen Sie **Start Streaming Logs** (Streamen der Protokolle starten) aus.

    ![„Start Streaming Logs“ (Streamen der Protokolle starten)](containers/media/quickstart-nodejs/view-logs.png)

1. Wenn Sie dazu aufgefordert werden, aktivieren Sie die Protokollierung, und starten Sie die Anwendung neu. Sobald die App neu gestartet wurde, wird das VS Code-Ausgabefenster mit einer Verbindung mit dem Protokollstream geöffnet. 

    ![Aktivieren von Protokollierung und Neustart](containers/media/quickstart-nodejs/enable-restart.png)

1. Nach einigen Sekunden wird im Ausgabefenster eine Meldung mit dem Hinweis angezeigt, dass eine Verbindung mit dem Protokollstreamingdienst hergestellt wurde. Sie können weitere Ausgabeaktivitäten generieren, indem Sie die Seite im Browser aktualisieren.

    <pre>
    Connecting to log stream...
    2020-03-04T19:29:44  Welcome, you are now connected to log-streaming service. The default timeout is 2 hours.
    Change the timeout with the App Setting SCM_LOGSTREAM_TIMEOUT (in seconds).    
    </pre>

> [!div class="nextstepaction"]
> [Ich bin auf ein Problem gestoßen](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azure-app-service&step=tailing-logs)

## <a name="next-steps"></a>Nächste Schritte

Herzlichen Glückwunsch! Sie haben diese Schnellstartanleitung erfolgreich abgeschlossen!

Sehen Sie sich als Nächstes die anderen Azure-Erweiterungen an.

* [Cosmos DB](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)
* [Azure-Funktionen](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions)
* [Docker-Tools](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker)
* [Azure CLI-Tools](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli)
* [Azure Resource Manager Tools](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)

Sie können auch alle Erweiterungen auf einmal erhalten, indem Sie das [Node Pack for Azure](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack) installieren.
