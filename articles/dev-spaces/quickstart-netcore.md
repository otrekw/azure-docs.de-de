---
title: 'Debuggen und iteratives Entwickeln unter Kubernetes: Visual Studio Code und .NET Core'
services: azure-dev-spaces
ms.date: 07/08/2019
ms.topic: quickstart
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie mit Azure Dev Spaces und Visual Studio Code eine .NET Core-Anwendung im Azure Kubernetes Service debuggen und schnell durchlaufen.
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, Container, Helm, Service Mesh, Service Mesh-Routing, kubectl, k8s
manager: gwallace
ms.openlocfilehash: f3e91ec11e054e0a069eb97a3700b051476be0a1
ms.sourcegitcommit: e4c33439642cf05682af7f28db1dbdb5cf273cc6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2020
ms.locfileid: "78251983"
---
# <a name="quickstart-debug-and-iterate-on-kubernetes-visual-studio-code-and-net-core---azure-dev-spaces"></a>Schnellstart: Debuggen und iteratives Entwickeln unter Kubernetes: Visual Studio Code und .NET Core: Azure Dev Spaces

In diesem Leitfaden lernen Sie Folgendes:

- Einrichten von Azure Dev Spaces mit einem verwalteten Kubernetes-Cluster in Azure
- Iteratives Entwickeln von Code in Containern mit Visual Studio Code
- Debuggen des Codes in Ihrem Entwicklerbereich über Visual Studio Code

Azure Dev Spaces ermöglicht außerdem das Debuggen und Durchlaufen mit Folgendem:
- [Java und Visual Studio Code](quickstart-java.md)
- [Node.js und Visual Studio Code](quickstart-nodejs.md)
- [.NET Core und Visual Studio](quickstart-netcore-visualstudio.md)

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Abonnement. Falls Sie über keins verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free) erstellen.
- [Installation von Visual Studio Code](https://code.visualstudio.com/download).
- Installation der Erweiterungen [Azure Dev Spaces](https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds) und [C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) für Visual Studio Code.
- Die [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest) muss installiert sein.

## <a name="create-an-azure-kubernetes-service-cluster"></a>Erstellen eines Azure Kubernetes Service-Clusters

Sie müssen einen AKS-Cluster in einer [unterstützten Region][supported-regions] erstellen. Mit den unten angegebenen Befehlen wird eine Ressourcengruppe mit dem Namen *MyResourceGroup* und der AKS-Cluster *MyAKS* erstellt.

```azurecli
az group create --name MyResourceGroup --location eastus
az aks create -g MyResourceGroup -n MyAKS --location eastus --generate-ssh-keys
```

## <a name="enable-azure-dev-spaces-on-your-aks-cluster"></a>Aktivieren von Azure Dev Spaces in Ihrem AKS-Cluster

Verwenden Sie den Befehl `use-dev-spaces`, um Dev Spaces in Ihrem AKS-Cluster zu aktivieren, und befolgen Sie die angezeigten Anweisungen. Mit dem unten angegebenen Befehl wird Dev Spaces im Cluster *MyAKS* in der Gruppe *MyResourceGroup* aktiviert und der Entwicklerbereich *default* erstellt.

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

Öffnen Sie Visual Studio Code, klicken Sie auf *Datei* und dann auf *Öffnen...* , navigieren Sie zum Verzeichnis *dev-spaces/samples/dotnetcore/getting-started/webfrontend*, und klicken Sie auf *Öffnen*.

Das Projekt *webfrontend* ist nun in Visual Studio Code geöffnet. Um die Anwendung in Ihrem Entwicklungsbereich auszuführen, generieren Sie in der Befehlspalette mithilfe der Azure Dev Spaces-Erweiterung die Docker- und die Helm-Chart-Ressourcen.

Klicken Sie zum Öffnen der Befehlspalette in Visual Studio Code auf *Ansicht* und dann auf *Befehlspalette*. Beginnen Sie mit dem Eingeben von `Azure Dev Spaces`, und klicken Sie auf `Azure Dev Spaces: Prepare configuration files for Azure Dev Spaces`.

![Vorbereiten von Konfigurationsdateien für Azure Dev Spaces](./media/common/command-palette.png)

Wenn Sie von Visual Studio Code auch zum Konfigurieren Ihres öffentlichen Endpunkts aufgefordert werden, wählen Sie `Yes` aus, um einen öffentlichen Endpunkt zu aktivieren.

![Auswählen des öffentlichen Endpunkts](media/common/select-public-endpoint.png)

Dieser Befehl generiert ein Dockerfile und ein Helm-Chart, um Ihr Projekt für die Ausführung in Azure Dev Spaces vorzubereiten. Außerdem wird das Verzeichnis *.vscode* mit der grundlegenden Debugkonfiguration Ihres Projekts generiert.

> [!TIP]
> Azure Dev Spaces nutzt das [Dockerfile und das Helm-Diagramm](how-dev-spaces-works.md#prepare-your-code) zum Erstellen und Ausführen Ihres Codes. Sie können sie jedoch ändern, wenn Sie anpassen möchten, wie das Projekt erstellt und ausgeführt wird.

## <a name="build-and-run-code-in-kubernetes-from-visual-studio-code"></a>Erstellen und Ausführen von Code in Kubernetes über Visual Studio Code

Klicken Sie links auf das Symbol *Debuggen* und dann oben auf *.NET Core Launch (AZDS)* (.NET Core-Start (AZDS)).

![](media/get-started-netcore/debug-configuration.png)

Mit diesem Befehl wird Ihr Dienst in Azure Dev Spaces im Debugmodus erstellt und ausgeführt. Im Fenster *Terminal* am unteren Rand werden die Buildausgabe und die URLs zur Ausführung in Azure Dev Spaces für Ihren Dienst angezeigt. In der *Debugging-Konsole* wird die Protokollausgabe angezeigt.

> [!Note]
> Falls in der *Befehlspalette* keine Azure Dev Spaces-Befehle angezeigt werden, sollten Sie überprüfen, ob die [Visual Studio Code-Erweiterung für Azure Dev Spaces](https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds) installiert wurde. Vergewissern Sie sich auch, dass Sie das Verzeichnis *dev-spaces/samples/dotnetcore/getting-started/webfrontend* in Visual Studio Code geöffnet haben.

Wenn Sie die öffentliche URL öffnen, sehen Sie, dass der Dienst ausgeführt wird.

Klicken Sie auf *Debuggen* und dann auf *Debuggen beenden*, um den Debugger zu beenden.

## <a name="update-code"></a>Aktualisieren des Codes

Zum Bereitstellen einer aktualisierten Version Ihres Diensts können Sie Dateien in Ihrem Projekt aktualisieren und *.NET Core Launch (AZDS)* (.NET Core-Start (AZDS)) erneut ausführen. Beispiel:

1. Falls Ihre Anwendung noch ausgeführt wird, klicken Sie auf *Debuggen* und anschließend auf *Debuggen beenden*, um sie zu beenden.
1. Aktualisieren Sie die [Zeile 22 in `Controllers/HomeController.cs`](https://github.com/Azure/dev-spaces/blob/master/samples/dotnetcore/getting-started/webfrontend/Controllers/HomeController.cs#L22) wie folgt:
    
    ```csharp
    ViewData["Message"] = "Your application description page in Azure.";
    ```

1. Speichern Sie die Änderungen.
1. Führen Sie *.NET Core Launch (AZDS)* (.NET Core-Start (AZDS)) erneut aus.
1. Navigieren Sie zu Ihrem ausgeführten Dienst, und klicken Sie auf *Info*.
1. Sehen Sie sich Ihre Änderungen an.
1. Klicken Sie auf *Debuggen* und anschließend auf *Debuggen beenden*, um Ihre Anwendung zu beenden.

## <a name="setting-and-using-breakpoints-for-debugging"></a>Festlegen und Verwenden von Haltepunkten für das Debuggen

Starten Sie Ihren Dienst im Debugmodus, indem Sie *.NET Core Launch (AZDS)* (.NET Core-Start (AZDS)) verwenden.

Navigieren Sie zurück zur Ansicht *Explorer*, indem Sie auf *Ansicht* und dann auf *Explorer* klicken. Öffnen Sie `Controllers/HomeController.cs`, und klicken Sie auf die Zeile 22, um den Cursor darin zu platzieren. Drücken Sie zum Festlegen eines Haltepunkts *F9*, oder klicken Sie auf *Debuggen* und dann auf *Haltepunkt umschalten*.

Öffnen Sie Ihren Dienst in einem Browser. Sie sehen, dass keine Meldung angezeigt wird. Wechseln Sie zurück zu Visual Studio Code. Sie sehen, dass Zeile 20 hervorgehoben ist. Durch den von Ihnen festgelegten Haltepunkt wurde der Dienst in Zeile 20 angehalten. Drücken Sie zum Fortsetzen des Diensts *F5*, oder klicken Sie auf *Debuggen* und dann auf *Weiter*. Wechseln Sie zurück zum Browser. Sie sehen, dass die Meldung jetzt angezeigt wird.

Beim Ausführen Ihres Diensts in Kubernetes mit einem angefügten Debugger haben Sie Vollzugriff auf Debuginformationen, z. B. Aufrufliste, lokale Variablen und Ausnahmeninformationen.

Entfernen Sie den Breakpoint, indem Sie Ihren Cursor in `Controllers/HomeController.cs` in der Zeile 22 platzieren und *F9* drücken.

## <a name="update-code-from-visual-studio-code"></a>Aktualisieren von Code aus Visual Studio Code

Aktualisieren Sie die Zeile 22 in `Controllers/HomeController.cs`, während der Dienst im Debugmodus ausgeführt wird. Beispiel:

```csharp
ViewData["Message"] = "Your application description page in Azure while debugging!";
```

Speichern Sie die Datei . Klicken Sie auf *Debuggen* und dann auf *Debugging erneut starten*, oder klicken Sie in der *Debug-Symbolleiste* auf die Schaltfläche *Debugging erneut starten*.

![](media/common/debug-action-refresh.png)

Öffnen Sie Ihren Dienst in einem Browser. Sie sehen, dass Ihre aktualisierte Meldung angezeigt wird.

Anstatt bei jeder vorgenommenen Codeänderung erneut ein neues Containerimage zu erstellen und bereitzustellen, wird Code von Azure Dev Spaces im vorhandenen Container inkrementell kompiliert, um den Bearbeitungs-/Debugkreislauf zu beschleunigen.

## <a name="clean-up-your-azure-resources"></a>Bereinigen Ihrer Azure-Ressourcen

```azurecli
az group delete --name MyResourceGroup --yes --no-wait
```

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich darüber, wie Azure Dev Spaces Sie bei der Entwicklung komplexerer containerübergreifender Anwendungen unterstützt und wie Sie die gemeinsame Entwicklung vereinfachen können, indem Sie in verschiedenen Bereichen mit verschiedenen Versionen oder Branches Ihres Codes arbeiten. 

> [!div class="nextstepaction"]
> [Arbeiten mit mehreren Containern und Teamentwicklung](multi-service-netcore.md)


[supported-regions]: https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service
