---
title: 'Debuggen und iteratives Entwickeln unter Kubernetes: Visual Studio Code und Node.js'
services: azure-dev-spaces
ms.date: 07/08/2019
ms.topic: quickstart
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie mit Azure Dev Spaces und Visual Studio Code eine Node.js-Anwendung im Azure Kubernetes Service debuggen und schnell durchlaufen.
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, Container, Helm, Service Mesh, Service Mesh-Routing, kubectl, k8s
manager: gwallace
ms.openlocfilehash: 74063e03e8298e388efd6888fc05bcbbc9aefa4b
ms.sourcegitcommit: e4c33439642cf05682af7f28db1dbdb5cf273cc6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2020
ms.locfileid: "78245064"
---
# <a name="quickstart-debug-and-iterate-on-kubernetes-with-visual-studio-code-and-nodejs---azure-dev-spaces"></a>Schnellstart: Debuggen und iteratives Entwickeln unter Kubernetes mit Visual Studio Code und Node.js: Azure Dev Spaces

In dieser Schnellstartanleitung richten Sie Azure Dev Spaces mit einem Managed Kubernetes-Cluster ein und verwenden eine Node.js-App in Visual Studio Code, um Code in Containern iterativ zu entwickeln und zu debuggen. Azure Dev Spaces ermöglicht es Ihnen, alle Komponenten Ihrer Anwendung mit minimalem Einrichtungsaufwand für Entwicklungscomputer in Azure Kubernetes Service (AKS) zu debuggen und zu testen. 

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio). 
- [Aktuelle Version von Node.js](https://nodejs.org/download/).
- [Visual Studio Code](https://code.visualstudio.com/download)
- [Azure Dev Spaces](https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds)-Erweiterung für Visual Studio Code.
- [Azure-Befehlszeilenschnittstelle](/cli/azure/install-azure-cli?view=azure-cli-latest).
- [Git](https://www.git-scm.com/downloads).

## <a name="create-an-azure-kubernetes-service-cluster"></a>Erstellen eines Azure Kubernetes Service-Clusters

Sie müssen einen AKS-Cluster in einer [unterstützten Region][supported-regions] erstellen. Mit den folgenden Befehlen werden eine Ressourcengruppe mit dem Namen *MyResourceGroup* und der AKS-Cluster *MyAKS* erstellt:

```azurecli
az group create --name MyResourceGroup --location eastus
az aks create -g MyResourceGroup -n MyAKS --location eastus --generate-ssh-keys
```

## <a name="enable-azure-dev-spaces-on-your-aks-cluster"></a>Aktivieren von Azure Dev Spaces in Ihrem AKS-Cluster

Verwenden Sie den Befehl `use-dev-spaces`, um Dev Spaces in Ihrem AKS-Cluster zu aktivieren, und befolgen Sie die angezeigten Anweisungen. Mit dem folgenden Befehl wird Dev Spaces im Cluster *MyAKS* in der Gruppe *MyResourceGroup* aktiviert und der Entwicklerbereich *default* erstellt.

> [!NOTE]
> Mit dem Befehl `use-dev-spaces` wird außerdem die Azure Dev Spaces-Befehlszeilenschnittstelle installiert, falls diese nicht bereits installiert ist. Die Azure Dev Spaces-Befehlszeilenschnittstelle kann nicht in Azure Cloud Shell installiert werden.

```azurecli
az aks use-dev-spaces -g MyResourceGroup -n MyAKS
```

```output
'An Azure Dev Spaces Controller' will be created that targets resource 'MyAKS' in resource group 'MyResourceGroup'. Continue? (y/N): y

Creating and selecting Azure Dev Spaces Controller 'MyAKS' in resource group 'MyResourceGroup' that targets resource 'MyAKS' in resource group 'MyResourceGroup'...2m 24s

Select a dev space or Kubernetes namespace to use as a dev space.
 [1] default
Type a number or a new name: 1

Kubernetes namespace 'default' will be configured as a dev space. This will enable Azure Dev Spaces instrumentation for new workloads in the namespace. Continue? (Y/n): Y

Configuring and selecting dev space 'default'...3s

Managed Kubernetes cluster 'MyAKS' in resource group 'MyResourceGroup' is ready for development in dev space 'default'. Type `azds prep` to prepare a source directory for use with Azure Dev Spaces and `azds up` to run.
```

## <a name="get-sample-application-code"></a>Abrufen des Codes für die Beispielanwendung

In diesem Artikel verwenden Sie die [Azure Dev Spaces-Beispielanwendung](https://github.com/Azure/dev-spaces), um die Nutzung von Azure Dev Spaces zu veranschaulichen.

Klonen Sie die Anwendung von GitHub.

```cmd
git clone https://github.com/Azure/dev-spaces
```

## <a name="prepare-the-sample-application-in-visual-studio-code"></a>Vorbereiten der Beispielanwendung in Visual Studio Code

Öffnen Sie Visual Studio Code, wählen Sie **Datei** und dann **Öffnen** aus, navigieren Sie zum Verzeichnis *dev-spaces/samples/nodejs/getting-started/webfrontend*, und wählen Sie **Öffnen** aus.

Das Projekt *webfrontend* ist nun in Visual Studio Code geöffnet. Um die Anwendung in Ihrem Entwicklungsbereich auszuführen, generieren Sie in der Befehlspalette mithilfe der Azure Dev Spaces-Erweiterung die Docker- und die Helm-Chart-Ressourcen.

Wählen Sie zum Öffnen der Befehlspalette in Visual Studio Code **Ansicht** und dann **Befehlspalette** aus. Beginnen Sie mit der Eingabe von `Azure Dev Spaces`, und wählen Sie **Azure Dev Spaces: Vorbereiten von Konfigurationsdateien für Azure Dev Spaces** aus.

![Vorbereiten von Konfigurationsdateien für Azure Dev Spaces](./media/common/command-palette.png)

Wenn Sie von Visual Studio Code auch zum Konfigurieren Ihres öffentlichen Endpunkts aufgefordert werden, wählen Sie `Yes` aus, um einen öffentlichen Endpunkt zu aktivieren.

![Auswählen des öffentlichen Endpunkts](media/common/select-public-endpoint.png)

Dieser Befehl generiert ein Dockerfile und ein Helm-Chart, um Ihr Projekt für die Ausführung in Azure Dev Spaces vorzubereiten. Außerdem wird das Verzeichnis *.vscode* mit der grundlegenden Debugkonfiguration Ihres Projekts generiert.

> [!TIP]
> Azure Dev Spaces nutzt das [Dockerfile und das Helm-Chart](how-dev-spaces-works.md#prepare-your-code) Ihres Projekts zum Erstellen und Ausführen Ihres Codes. Sie können diese Dateien aber ändern, wenn Sie anpassen möchten, wie das Projekt erstellt und ausgeführt wird.

## <a name="build-and-run-code-in-kubernetes-from-visual-studio-code"></a>Erstellen und Ausführen von Code in Kubernetes über Visual Studio Code

Wählen Sie links das Symbol **Debuggen** und dann oben **Launch Server (AZDS)** (Server starten (AZDS)) aus.

![Starten des Servers](media/get-started-node/debug-configuration-nodejs.png)

Mit diesem Befehl wird Ihr Dienst in Azure Dev Spaces erstellt und ausgeführt. Im Fenster **Terminal** am unteren Rand werden die Buildausgabe und die URLs zur Ausführung von Azure Dev Spaces für Ihren Dienst angezeigt. In der **Debugging-Konsole** wird die Protokollausgabe angezeigt.

> [!Note]
> Falls in der **Befehlspalette** keine Azure Dev Spaces-Befehle angezeigt werden, sollten Sie überprüfen, ob die [Visual Studio Code-Erweiterung für Azure Dev Spaces](https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds) installiert wurde. Vergewissern Sie sich auch, dass Sie das Verzeichnis *dev-spaces/samples/nodejs/getting-started/webfrontend* in Visual Studio Code geöffnet haben.

Wenn Sie die öffentliche URL öffnen, sehen Sie, dass der Dienst ausgeführt wird.

Wählen Sie **Debuggen** und dann **Debuggen beenden** aus, um den Debugger zu beenden.

## <a name="update-code"></a>Aktualisieren des Codes

Zum Bereitstellen einer aktualisierten Version Ihres Diensts können Sie Dateien in Ihrem Projekt aktualisieren und **Launch Server** (Server starten) erneut ausführen. Beispiel:

1. Falls Ihre Anwendung noch ausgeführt wird, wählen Sie **Debuggen** und anschließend **Debuggen beenden** aus, um sie zu beenden.
1. Aktualisieren Sie die [Zeile 13 in `server.js`](https://github.com/Azure/dev-spaces/blob/master/samples/nodejs/getting-started/webfrontend/server.js#L13) wie folgt:
    
    ```javascript
        res.send('Hello from webfrontend in Azure');
    ```

1. Speichern Sie die Änderungen.
1. Führen Sie **Launch Server** (Server starten) erneut aus.
1. Navigieren Sie zu Ihrem ausgeführten Dienst, und sehen Sie sich Ihre Änderungen an.
1. Wählen Sie **Debuggen** und anschließend **Debuggen beenden** aus, um Ihre Anwendung zu beenden.

## <a name="setting-and-using-breakpoints-for-debugging"></a>Festlegen und Verwenden von Haltepunkten für das Debuggen

Starten Sie Ihren Dienst, indem Sie **Launch Server (AZDS)** (Server starten (AZDS)) verwenden.

Navigieren Sie zurück zur Explorer-Ansicht, indem Sie **Ansicht** und dann **Explorer** auswählen. Öffnen Sie *server.js*, und klicken Sie auf Zeile 13, um den Cursor darin zu platzieren. Drücken Sie zum Festlegen eines Breakpoints **F9**, oder wählen Sie **Debuggen** und dann **Haltepunkt ein/aus** aus.

Öffnen Sie Ihren Dienst in einem Browser. Sie sehen, dass keine Meldung angezeigt wird. Wechseln Sie zurück zu Visual Studio Code. Zeile 13 ist hervorgehoben. Durch den von Ihnen festgelegten Breakpoint wurde der Dienst in Zeile 13 angehalten. Drücken Sie zum Fortsetzen des Diensts **F5**, oder wählen Sie **Debuggen** und dann **Weiter** aus. Wechseln Sie zurück zum Browser. Sie sehen, dass die Meldung jetzt angezeigt wird.

Beim Ausführen Ihres Diensts in Kubernetes mit einem angefügten Debugger haben Sie Vollzugriff auf Debuginformationen, z. B. Aufrufliste, lokale Variablen und Ausnahmeninformationen.

Entfernen Sie den Breakpoint, indem Sie den Cursor in *server.js* in Zeile 13 platzieren und **F9** drücken.

Wählen Sie **Debuggen** und dann **Debuggen beenden** aus, um den Debugger zu beenden.

## <a name="update-code-from-visual-studio-code"></a>Aktualisieren von Code aus Visual Studio Code

Ändern Sie den Debugmodus in **Attach to a Server (AZDS)** (An Server anfügen (AZDS)), und starten Sie den Dienst:

![](media/get-started-node/attach-nodejs.png)

Mit diesem Befehl wird Ihr Dienst in Azure Dev Spaces erstellt und ausgeführt. Außerdem wird im Container Ihres Diensts ein Prozess vom Typ [nodemon](https://nodemon.io) gestartet und VS Code angefügt. Der Prozess *nodemon* lässt automatische Neustarts zu, wenn Änderungen am Quellcode vorgenommen werden, um auf ähnliche Weise wie auf Ihrem lokalen Computer eine schnellere Entwicklung von inneren Schleifen zu ermöglichen.

Navigieren Sie nach dem Starten des Diensts über Ihren Browser dorthin, und interagieren Sie damit.

Wechseln Sie während der Ausführung des Diensts zurück zu VS Code, und aktualisieren Sie Zeile 13 in *server.js*. Beispiel:
```javascript
    res.send('Hello from webfrontend in Azure while debugging!');
```

Speichern Sie die Datei, und kehren Sie über einen Browser zu Ihrem Dienst zurück. Interagieren Sie mit dem Dienst, und achten Sie auf die Anzeige Ihrer aktualisierten Meldung.

Während der Ausführung von *nodemon* wird der Node-Prozess automatisch neu gestartet, sobald Codeänderungen erkannt werden. Dieser Prozess des automatischen Neustarts ähnelt dem Bearbeiten und Neustarten Ihres Diensts auf Ihrem lokalen Computer, um eine Umgebung für die Entwicklung von inneren Schleifen bereitzustellen.

## <a name="clean-up-your-azure-resources"></a>Bereinigen Ihrer Azure-Ressourcen

```azurecli
az group delete --name MyResourceGroup --yes --no-wait
```

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich darüber, wie Azure Dev Spaces Sie bei der Entwicklung komplexerer containerübergreifender Anwendungen unterstützt und wie Sie die gemeinsame Entwicklung vereinfachen können, indem Sie in verschiedenen Bereichen mit verschiedenen Versionen oder Branches Ihres Codes arbeiten.

> [!div class="nextstepaction"]
> [Arbeiten mit mehreren Containern und Teamentwicklung](multi-service-nodejs.md)


[supported-regions]: https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service
