---
title: 'Schnellstart: Erstellen einer Node.js-Web-App'
description: Stellen Sie in wenigen Minuten Ihre erste Node.js-App vom Typ „Hallo Welt“ in Azure App Service bereit.
ms.assetid: 582bb3c2-164b-42f5-b081-95bfcb7a502a
ms.topic: quickstart
ms.date: 08/01/2020
ms.custom: mvc, devcenter, seodec18
zone_pivot_groups: app-service-platform-windows-linux
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 7cd9add699d9bd4a3eff9cdcf1e65af0d754b70a
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "101747558"
---
# <a name="create-a-nodejs-web-app-in-azure"></a>Erstellen einer Node.js-Web-App in Azure

Erste Schritte mit <abbr title="Ein HTTP-basierter Dienst zum Hosten von Webanwendungen, REST-APIs und mobilen Back-End-Anwendungen">Azure App Service</abbr> , indem Sie lokal eine Node.js-/Express-App mit Visual Studio Code erstellen und die App anschließend in der Azure-Cloud bereitstellen. Da Sie einen <abbr title="Ein Basistarif in Azure App Service, in dem Ihre App auf den gleichen virtuellen Computern wie andere Apps ausgeführt wird (einschließlich Apps anderer Kunden). Dieser Tarif eignet sich für die Entwicklung und zu Testzwecken.">kostenlosen Tarif verwenden</abbr>, fallen im Rahmen dieser Schnellstartanleitung keine Kosten an.

## <a name="1-prepare-your-environment"></a>1. Vorbereiten der Umgebung

- Ein Azure-Konto mit einem aktiven <abbr title="Ein Azure-Abonnement ist ein logischer Container zur Bereitstellung von Ressourcen in Azure. Es enthält Details zu all Ihren Ressourcen, z. B. zu Ihren virtuellen Computern, Datenbanken usw.">Abonnement</abbr>. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-app-service-extension&mktingSource=vscode-tutorial-app-service-extension).
- <a href="https://git-scm.com/" target="_blank">Installation von Git</a>
- [Node.js und npm](https://nodejs.org). Führen Sie den Befehl `node --version` aus, um sich zu vergewissern, dass Node.js installiert ist.
- [Visual Studio Code](https://code.visualstudio.com/)
- Die [Azure App Service-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice) für Visual Studio Code.

[Problem melden](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azure-app-service&prepare-your-environment)




<br>
<hr/>

## <a name="2-clone-and-run-a-local-nodejs-application"></a>2. Klonen und Ausführen einer lokalen Node.js-Anwendung

1. Öffnen Sie auf Ihrem lokalen Computer ein Terminal, und klonen Sie das Beispielrepository:

    ```bash
    git clone https://github.com/Azure-Samples/nodejs-docs-hello-world
    ```

1. Navigieren Sie zu dem neuen App-Ordner:

    ```bash
    cd nodejs-docs-hello-world
    ```

1. Installieren Sie die Abhängigkeiten:

    ```bash
    npm install
    ```

1. Starten Sie die App, um sie lokal zu testen:

    ```bash
    npm start
    ```
    
1. Navigieren Sie in Ihrem Browser zu `http://localhost:1337`. Im Browser sollte „Hello World!“ angezeigt werden.

1. Drücken Sie im Terminal <kbd>STRG</kbd> + <kbd>C</kbd>, um den Server zu beenden.

[Problem melden](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azure-app-service&prepare-your-environment)


<br>
<hr/>




<!-- VS Code extension works differently for Windows/Linus - Step 3 -->

::: zone pivot="platform-windows"  

[!INCLUDE [Windows](./includes/quickstart-nodejs-uiex-windows.md)]


::: zone-end

::: zone pivot="platform-linux"  

[!INCLUDE [Windows](./includes/quickstart-nodejs-uiex-linux.md)]

::: zone-end


## <a name="4-viewing-logs-from-visual-studio-code"></a>4. Anzeigen von Protokollen aus Visual Studio Code

Zeigen Sie das <abbr title="Alle Aufrufe von `console.log` in der App werden in Visual Studio Code im Ausgabefenster angezeigt.">log</abbr> der ausgeführten App Service-App an.

1. Suchen Sie die App im **AZURE APP SERVICE**-Explorer, klicken Sie mit der rechten Maustaste auf den App-Namen, und wählen Sie **Start streaming logs** (Streamen der Protokolle starten) aus.

1. Das Visual Studio Code-Ausgabefenster wird geöffnet.

    ![Anzeigen von Streamingprotokollen](./media/quickstart-nodejs/view-logs.png)

    :::image type="content" source="./media/quickstart-nodejs/enable-restart.png" alt-text="Screenshot: Visual Studio Code-Aufforderung zum Aktivieren der Dateiprotokollierung und zum Neustarten der Web-App mit ausgewählter Schaltfläche „Ja“":::

1. Nach einigen Sekunden sehen Sie eine Meldung, die Sie darüber informiert, dass eine Verbindung mit dem Protokollstreamingdienst hergestellt wurde. 
1. Aktualisieren Sie die Seite mehrmals, um weitere Aktivitäten anzuzeigen.

    <pre class="is-monospace is-size-small has-padding-medium has-background-tertiary has-text-tertiary-invert">
    2020-09-20 20:37:39.574 INFO  - Initiating warmup request to container msdocs-vscode-node_2_00ac292a for site msdocs-vscode-node
    2020-09-20 20:37:55.011 INFO  - Waiting for response to warmup request for container msdocs-vscode-node_2_00ac292a. Elapsed time = 15.4373071 sec
    2020-09-20 20:38:08.233 INFO  - Container msdocs-vscode-node_2_00ac292a for site msdocs-vscode-node initialized successfully and is ready to serve requests.
    2020-09-20T20:38:21  Startup Request, url: /Default.cshtml, method: GET, type: request, pid: 61,1,7, SCM_SKIP_SSL_VALIDATION: 0, SCM_BIN_PATH: /opt/Kudu/bin, ScmType: None
    </pre>

<br>

[Problem melden](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azure-app-service&prepare-your-environment)

<br>
<hr/>

## <a name="5-clean-up-resources"></a>5. Bereinigen von Ressourcen

Suchen Sie die App im **AZURE APP SERVICE**-Explorer, klicken Sie mit der rechten Maustaste auf den App-Namen, und wählen Sie **Löschen** aus. 

:::image type="content" source="./media/quickstart-nodejs/delete-resource-ieux.png" alt-text="Suchen der App im AZURE APP SERVICE-Explorer, Klicken auf den App-Namen mit der rechten Maustaste und Auswählen von „Löschen“":::

## <a name="next-steps"></a>Nächste Schritte

Herzlichen Glückwunsch! Sie haben diese Schnellstartanleitung erfolgreich abgeschlossen! Sie können Änderungen an dieser App bereitstellen, indem Sie den gleichen Prozess ausführen und die vorhandene App auswählen anstatt eine neue zu erstellen.

Sehen Sie sich als Nächstes die anderen Azure-Erweiterungen an.

* [Cosmos DB](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)
* [Azure-Funktionen](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions)
* [Docker-Tools](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker)
* [Azure CLI-Tools](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli)
* [Azure Resource Manager Tools](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)

Sie können auch alle Erweiterungen auf einmal erhalten, indem Sie das [Node Pack for Azure](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack) installieren.