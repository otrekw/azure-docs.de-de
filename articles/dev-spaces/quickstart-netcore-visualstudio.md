---
title: 'Debuggen und iteratives Entwickeln unter Kubernetes: Visual Studio und .NET Core'
services: azure-dev-spaces
ms.date: 11/13/2019
ms.topic: quickstart
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie mit Azure Dev Spaces und Visual Studio eine .NET Core-Anwendung im Azure Kubernetes Service debuggen und schnell durchlaufen.
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, Container, Helm, Service Mesh, Service Mesh-Routing, kubectl, k8s
manager: gwallace
ms.custom: vs-azure
ms.workload: azure-vs
ms.openlocfilehash: 909e4638b3b0919919320a09cbfa0e8d9ac92f2e
ms.sourcegitcommit: fc718cc1078594819e8ed640b6ee4bef39e91f7f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2020
ms.locfileid: "83995937"
---
# <a name="quickstart-debug-and-iterate-on-kubernetes-visual-studio--net-core---azure-dev-spaces"></a>Schnellstart: Debuggen und iteratives Entwickeln unter Kubernetes: Visual Studio und .NET Core – Azure Dev Spaces

In diesem Leitfaden lernen Sie Folgendes:

- Einrichten von Azure Dev Spaces mit einem verwalteten Kubernetes-Cluster in Azure
- Iteratives Entwickeln von Code in Containern mit Visual Studio
- Debuggen von Code in Ihrem Cluster mit Visual Studio

Azure Dev Spaces ermöglicht außerdem das Debuggen und Durchlaufen mit:
- [Java und Visual Studio Code](quickstart-java.md)
- [Node.js und Visual Studio Code](quickstart-nodejs.md)
- [.NET Core und Visual Studio Code](quickstart-netcore.md)

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Abonnement. Falls Sie über keins verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free) erstellen.
- Visual Studio 2019 unter Windows mit installierter Workload für die Azure-Entwicklung. Wenn Sie Visual Studio nicht installiert haben, können Sie es [hier](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs) herunterladen.
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

## <a name="create-a-new-aspnet-web-app"></a>Erstellen einer neuen ASP.NET-Web-App

1. Öffnen Sie Visual Studio.
1. Erstellen Sie ein neues Projekt.
1. Wählen Sie *ASP.NET Core-Webanwendung* aus, und klicken Sie auf *Weiter*.
1. Nennen Sie Ihr Projekt *webfrontend*, und klicken Sie auf *Erstellen*.
1. Wählen Sie bei entsprechender Aufforderung die Option *Webanwendung (Model-View-Controller)* für die Vorlage.
1. Wählen Sie oben die Optionen *.NET Core* und *ASP.NET Core 2.1* aus.
1. Klicken Sie auf *Erstellen*.

## <a name="connect-your-project-to-your-dev-space"></a>Verbinden Ihres Projekts mit Ihrem Entwicklerbereich

Wählen Sie in Ihrem Projekt wie nachfolgend gezeigt im Dropdownmenü mit den Starteinstellungen die Option **Azure Dev Spaces**.

![](media/get-started-netcore-visualstudio/LaunchSettings.png)

Wählen Sie im Azure Dev Spaces-Dialogfeld Ihr *Abonnement* und Ihren *Azure Kubernetes-Cluster* aus. Behalten Sie für *Space* (Bereich) die Einstellung *default* bei, und aktivieren Sie das Kontrollkästchen *Öffentlich zugänglich*. Klicken Sie auf *OK*.

![](media/get-started-netcore-visualstudio/Azure-Dev-Spaces-Dialog.png)

Mit diesem Prozess wird Ihr Dienst im Entwicklerbereich *default* mit einer öffentlich zugänglichen URL bereitgestellt. Wenn Sie einen Cluster auswählen, der nicht für die Verwendung mit Azure Dev Spaces konfiguriert wurde, wird eine Meldung mit der Frage angezeigt, ob Sie ihn konfigurieren möchten. Klicken Sie auf *OK*.

![](media/get-started-netcore-visualstudio/Add-Azure-Dev-Spaces-Resource.png)

Die öffentliche URL für den Dienst, die im Entwicklerbereich *default* ausgeführt wird, wird im Fenster *Ausgabe* angezeigt:

```cmd
Starting warmup for project 'webfrontend'.
Waiting for namespace to be provisioned.
Using dev space 'default' with target 'MyAKS'
...
Successfully built 1234567890ab
Successfully tagged webfrontend:devspaces-11122233344455566
Built container image in 39s
Waiting for container...
36s

Service 'webfrontend' port 'http' is available at `http://default.webfrontend.1234567890abcdef1234.eus.azds.io/`
Service 'webfrontend' port 80 (http) is available at http://localhost:62266
Completed warmup for project 'webfrontend' in 125 seconds.
```

Im obigen Beispiel lautet die öffentliche URL `http://default.webfrontend.1234567890abcdef1234.eus.azds.io/`. 

Wählen Sie **Debuggen** und dann **Debuggen starten** aus. Nach einigen Sekunden wird Ihr Dienst gestartet, und von Visual Studio wird ein Browser mit der öffentlichen URL des Diensts geöffnet. Gehen Sie wie folgt vor, falls der Browser nicht automatisch geöffnet wird: Navigieren Sie im Browser zur öffentlichen URL Ihres Diensts, und interagieren Sie mit dem Dienst, der in Ihrem Entwicklungsbereich ausgeführt wird.

Dieser Prozess kann den öffentlichen Zugriff auf Ihren Dienst deaktiviert haben. Um den öffentlichen Zugriff zu aktivieren, können Sie den [eingehenden Wert in der Datei *values.yaml*][ingress-update] aktualisieren.

## <a name="update-code"></a>Aktualisieren des Codes

Klicken Sie auf die Schaltfläche „Beenden“, wenn Visual Studio noch mit Ihrem Entwicklerbereich verbunden ist. Ändern Sie Zeile 20 in `Controllers/HomeController.cs` in:
    
```csharp
ViewData["Message"] = "Your application description page in Azure.";
```

Speichern Sie Ihre Änderungen, und wählen Sie **Debuggen** und dann **Debuggen starten** aus. Nach einigen Sekunden wird Ihr Dienst gestartet, und von Visual Studio wird ein Browser mit der öffentlichen URL des Diensts geöffnet. Gehen Sie wie folgt vor, falls der Browser nicht automatisch geöffnet wird: Navigieren Sie in einem Browser zur öffentlichen URL Ihres Diensts, und klicken Sie auf *Info*. Sie sehen, dass Ihre aktualisierte Meldung angezeigt wird.

Anstatt bei jeder vorgenommenen Codeänderung erneut ein neues Containerimage zu erstellen und bereitzustellen, wird Code von Azure Dev Spaces im vorhandenen Container inkrementell kompiliert, um den Bearbeitungs-/Debugkreislauf zu beschleunigen.

## <a name="setting-and-using-breakpoints-for-debugging"></a>Festlegen und Verwenden von Haltepunkten für das Debuggen

Klicken Sie auf die Schaltfläche „Beenden“, wenn Visual Studio noch mit Ihrem Entwicklerbereich verbunden ist. Öffnen Sie `Controllers/HomeController.cs`, und klicken Sie in Zeile 20, um den Cursor darin zu platzieren. Drücken Sie zum Festlegen eines Haltepunkts *F9*, oder klicken Sie auf *Debuggen* und dann auf *Haltepunkt umschalten*. Drücken Sie zum Starten des Diensts im Debugmodus in Ihrem Entwicklerbereich *F5*, oder klicken Sie auf *Debuggen* und dann auf *Debuggen starten*.

Öffnen Sie Ihren Dienst in einem Browser. Sie sehen, dass keine Meldung angezeigt wird. Wechseln Sie zurück zu Visual Studio. Sie sehen, dass Zeile 20 hervorgehoben ist. Durch den von Ihnen festgelegten Haltepunkt wurde der Dienst in Zeile 20 angehalten. Drücken Sie zum Fortsetzen des Diensts *F5*, oder klicken Sie auf *Debuggen* und dann auf *Weiter*. Wechseln Sie zurück zum Browser. Sie sehen, dass die Meldung jetzt angezeigt wird.

Beim Ausführen Ihres Diensts in Kubernetes mit einem angefügten Debugger haben Sie Vollzugriff auf Debuginformationen, z. B. Aufrufliste, lokale Variablen und Ausnahmeninformationen.

Entfernen Sie den Haltepunkt, indem Sie Ihren Cursor in `Controllers/HomeController.cs` in Zeile 20 platzieren und *F9* drücken.

## <a name="clean-up-your-azure-resources"></a>Bereinigen Ihrer Azure-Ressourcen

Navigieren Sie im Azure-Portal zu Ihrer Ressourcengruppe, und klicken Sie auf *Ressourcengruppe löschen*. Alternativ hierzu können Sie auch den Befehl [az aks delete](/cli/azure/aks#az-aks-delete) verwenden:

```azurecli
az group delete --name MyResourceGroup --yes --no-wait
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Arbeiten mit mehreren Containern und Teamentwicklung](multi-service-netcore-visualstudio.md)

[ingress-update]: how-dev-spaces-works-up.md#how-running-your-code-is-configured
[supported-regions]: https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service
