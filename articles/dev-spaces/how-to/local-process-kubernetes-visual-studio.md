---
title: Verwenden des lokalen Prozesses mit Kubernetes und Visual Studio (Vorschau)
services: azure-dev-spaces
ms.date: 06/02/2020
ms.topic: conceptual
description: Erfahren Sie, wie Sie den lokalen Prozess mit Kubernetes und Visual Studio verwenden, um Ihren Entwicklungscomputer über Azure Dev Spaces mit einem Kubernetes-Cluster zu verbinden
keywords: Lokaler Prozess mit Kubernetes, Azure Dev Spaces, Dev Spaces, Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, Container
ms.openlocfilehash: 8e89baeac8d9042a87685ed1268ed694db8d9455
ms.sourcegitcommit: 69156ae3c1e22cc570dda7f7234145c8226cc162
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/03/2020
ms.locfileid: "84316551"
---
# <a name="use-local-process-with-kubernetes-with-visual-studio-preview"></a>Verwenden des lokalen Prozesses mit Kubernetes und Visual Studio (Vorschau)

Der lokale Prozess mit Kubernetes ermöglicht Ihnen das Ausführen und Debuggen von Code auf Ihrem Entwicklungscomputer, während der Kubernetes-Cluster weiterhin mit den übrigen Anwendungen oder Diensten verbunden ist. Wenn Sie z. B. über eine umfangreiche Microservicearchitektur mit einer Vielzahl von Diensten und Datenbanken verfügen, die voneinander abhängig sind, kann es schwierig sein, diese Abhängigkeiten auf Ihrem Entwicklungscomputer zu replizieren. Außerdem kann der Vorgang zum Erstellen und Bereitstellen von Code in Ihrem Kubernetes-Cluster für jede Codeänderung während der Inner Loop-Entwicklung langsam, zeitaufwendig und bei Verwendung mit einem Debugger wenig benutzerfreundlich sein.

Bei Verwendung des lokalen Prozesses mit Kubernetes muss Ihr Code nicht erstellt und im Cluster bereitgestellt werden. Stattdessen wird eine direkte Verbindung zwischen Ihrem Entwicklungscomputer und Ihrem Cluster hergestellt. Indem Sie Ihren Entwicklungscomputer während des Debuggens mit Ihrem Cluster verbinden, können Sie Ihren Dienst im Handumdrehen im Kontext der gesamten Anwendung testen und entwickeln, ohne dazu eine Docker- oder Kubernetes-Konfiguration erstellen zu müssen.

Wenn Sie den lokalen Prozess mit Kubernetes verwenden, wird Datenverkehr zwischen Ihrem verbundenen Kubernetes-Cluster und Ihrem Entwicklungscomputer umgeleitet. Diese Umleitung des Datenverkehrs ermöglicht es, dass Code auf dem Entwicklungscomputer und die im Kubernetes-Cluster ausgeführten Dienste so kommunizieren, als befänden sie sich im gleichen Kubernetes-Cluster. Der lokale Prozess bietet außerdem die Möglichkeit, die für Pods im Kubernetes-Cluster verfügbaren Umgebungsvariablen und bereitgestellten Volumes auf dem Entwicklungscomputer zu replizieren. Indem Sie auf Ihrem Entwicklungscomputer auf Umgebungsvariablen und bereitgestellte Volumes zugreifen können, können Sie Ihren Code rasch bearbeiten, ohne diese Abhängigkeiten manuell replizieren zu müssen.

In dieser Anleitung erfahren Sie, wie Sie den lokalen Prozess mit Kubernetes nutzen, um Datenverkehr zwischen Ihrem Kubernetes-Cluster und ausgeführtem Code auf Ihrem Entwicklungscomputer umzuleiten. Darüber hinaus finden Sie in dieser Anleitung ein Skript zur Bereitstellung einer umfangreichen Beispielanwendung mit mehreren Microservices auf einem Kubernetes-Cluster.

> [!IMPORTANT]
> Diese Funktion steht derzeit als Vorschau zur Verfügung. Vorschauversionen werden Ihnen zur Verfügung gestellt, wenn Sie die [zusätzlichen Nutzungsbedingungen][preview-terms] akzeptieren. Einige Aspekte dieses Features werden bis zur allgemeinen Verfügbarkeit unter Umständen noch geändert.

## <a name="before-you-begin"></a>Voraussetzungen

In dieser Anleitung wird die [Azure Dev Spaces-Beispielanwendung „Bike Sharing“][bike-sharing-github] verwendet, um zu veranschaulichen, wie Sie Ihren Entwicklungscomputer mit einem Kubernetes-Cluster verbinden. Wenn Sie bereits über eine eigene Anwendung in einem Kubernetes-Cluster verfügen, können Sie die folgenden Schritte dennoch ausführen und dabei die Namen Ihrer eigenen Dienste verwenden.

### <a name="prerequisites"></a>Voraussetzungen

* ein Azure-Abonnement. Falls Sie über kein Azure-Abonnement verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free) erstellen.
* Die [Azure CLI][azure-cli] muss installiert sein.
* [Visual Studio 2019][visual-studio] Version 16.7 Preview 2 oder höher unter Windows 10 mit Installation der Workloads *ASP.NET und Webentwicklung* und *Azure-Entwicklung*.
* [Installierte Azure Dev Spaces-Befehlszeilenschnittstelle][azds-cli].

### <a name="enable-the-local-process-with-kubernetes-preview-feature-in-visual-studio"></a>Aktivieren des lokalen Prozesses mit Kubernetes (Previewfunktion) und Visual Studio

Um den lokalen Prozess mit Kubernetes in Visual Studio zu aktivieren, klicken Sie auf *Tools* > *Optionen* > *Umgebung* > *Vorschaufeatures*. Wählen Sie *Lokales Debuggen für Kubernetes-Dienste aktivieren* aus.

Installieren Sie für .NET-Konsolenanwendungen außerdem das NuGet-Paket *Microsoft.VisualStudio.Azure.Kubernetes.Tools.Targets*.

## <a name="create-a-kubernetes-cluster"></a>Erstellen eines Kubernetes-Clusters

Erstellen Sie einen AKS-Cluster in einer [unterstützten Region][supported-regions]. Mit den unten angegebenen Befehlen wird eine Ressourcengruppe mit dem Namen *MyResourceGroup* und der AKS-Cluster *MyAKS* erstellt.

```azurecli-interactive
az group create \
    --name MyResourceGroup \
    --location eastus

az aks create \
    --resource-group MyResourceGroup \
    --name MyAKS \
    --location eastus \
    --node-count 3 \
    --generate-ssh-keys
```

## <a name="install-the-sample-application"></a>Installieren der Beispielanwendung

Installieren Sie die Beispielanwendung mithilfe des bereitgestellten Skripts auf Ihrem Cluster. Sie können dieses Skript auf Ihrem Entwicklungscomputer oder über die [Azure Cloud Shell][azure-cloud-shell] ausführen.

```azurecli-interactive
git clone https://github.com/Azure/dev-spaces
cd dev-spaces/
chmod +x ./local-process-quickstart.sh
./local-process-quickstart.sh -g MyResourceGroup -n MyAKS
```

Navigieren Sie zur Beispielanwendung auf Ihrem Cluster, indem Sie die öffentliche URL aufrufen, die in der Ausgabe des Installationsskripts angezeigt wird.

```console
$ ./local-process-quickstart.sh -g MyResourceGroup -n MyAKS
Defaulting Dev spaces repository root to current directory : ~/dev-spaces
Setting the Kube context
...
To try out the app, open the url:
dev.bikesharingweb.EXTERNAL_IP.nip.io
```

Im obigen Beispiel lautet die öffentliche URL `dev.bikesharingweb.EXTERNAL_IP.nip.io`.

## <a name="connect-to-your-cluster-and-debug-a-service"></a>Herstellen einer Clusterverbindung und Debuggen eines Diensts

Laden Sie auf dem Entwicklungscomputer die Kubernetes-CLI herunter, und konfigurieren Sie sie, um unter Verwendung von [az aks get-credentials][az-aks-get-credentials] eine Verbindung mit Ihrem Kubernetes-Cluster herzustellen.

```azurecli
az aks get-credentials --resource-group MyResourceGroup --name MyAKS
```

Öffnen Sie in Visual Studio in der [Azure Dev Spaces-Beispielanwendung „Bike Sharing“][bike-sharing-github] den Pfad *dev-spaces/samples/BikeSharingApp/ReservationEngine/app.csproj*.

Wählen Sie in Ihrem Projekt wie nachfolgend gezeigt im Dropdownmenü mit den Starteinstellungen die Option *Lokaler Prozess mit Kubernetes* aus.

![Auswählen des lokalen Prozesses mit Kubernetes](../media/local-process-kubernetes-visual-studio/choose-local-process.png)

Klicken Sie auf die Startschaltfläche neben *Lokaler Prozess mit Kubernetes*. Führen Sie im Dialogfeld *Lokaler Prozess mit Kubernetes* folgende Schritte aus:

* Wählen Sie Ihr Abonnement aus.
* Wählen Sie *MyAKS* als Cluster aus.
* Wählen Sie *dev* als Namespace aus.
* Wählen Sie *reservationengine* als Dienst für die Umleitung aus.
* Wählen Sie *app* als Startprofil aus.
* Wählen Sie `http://dev.bikesharingweb.EXTERNAL_IP.nip.io` als URL zum Starten Ihres Browsers aus.

![Auswählen des lokalen Prozesses mit Kubernetes-Cluster](../media/local-process-kubernetes-visual-studio/choose-local-process-cluster.png)

> [!IMPORTANT]
> Sie können nur Dienste umleiten, die über einen einzelnen Pod verfügen.

Klicken Sie auf *Speichern und Debuggen starten*.

Der gesamte Datenverkehr im Kubernetes-Cluster wird für den Dienst *reservationengine* an die Version der Anwendung umgeleitet, die auf dem Entwicklungscomputer ausgeführt wird. Der lokale Prozess mit Kubernetes leitet zudem den gesamten ausgehenden Datenverkehr der Anwendung zurück an den Kubernetes-Cluster.

> [!NOTE]
> Sie werden aufgefordert, die Ausführung von *KubernetesDNSManager* mit erhöhten Rechten zuzulassen und zu erlauben, dass Ihre Hostdatei geändert wird.

Sobald in der Statusleiste angezeigt wird, dass Sie mit dem *reservationengine*-Dienst verbunden sind, ist Ihr Entwicklungscomputer verbunden.

![Verbundener Entwicklungscomputer](../media/local-process-kubernetes-visual-studio/development-computer-connected.png)

> [!NOTE]
> Bei nachfolgenden Startvorgängen wird das Dialogfeld *Lokaler Prozess mit Kubernetes* nicht erneut angezeigt. Diese Einstellungen werden in den Projekteigenschaften im Bereich *Debuggen* aktualisiert.

Sobald Ihr Entwicklungscomputer verbunden ist, wird Datenverkehr für den Dienst, den Sie ersetzen, an den Entwicklungscomputer umgeleitet.

## <a name="set-a-break-point"></a>Festlegen eines Haltepunkts

Öffnen Sie [BikesHelper.cs][bikeshelper-cs-breakpoint], und klicken Sie auf Zeile 26, um den Cursor dort zu platzieren. Drücken Sie zum Festlegen eines Haltepunkts die Taste *F9*, oder klicken Sie auf *Debuggen* und dann auf *Haltepunkt umschalten*.

Rufen Sie die öffentliche URL auf, um zur Beispielanwendung zu navigieren. Wählen Sie *Aurelia Briggs (customer)* als Benutzer und dann ein zu mietendes Fahrrad aus. Klicken Sie auf *Rent Bike* (Fahrrad mieten). Wechseln Sie zurück zu Visual Studio. Sie sehen, dass Zeile 26 hervorgehoben ist. Durch den festgelegten Haltepunkt wurde der Dienst in Zeile 26 angehalten. Drücken Sie zum Fortsetzen des Diensts *F5*, oder klicken Sie auf *Debuggen* und dann auf *Weiter*. Wechseln Sie erneut in Ihren Browser, und überprüfen Sie, ob auf der Seite angezeigt wird, dass Sie das Fahrrad gemietet haben.

Entfernen Sie den Haltepunkt, indem Sie den Cursor in `BikesHelper.cs` in Zeile 26 platzieren und *F9* drücken.

> [!NOTE]
> Wenn Sie den Debugtask anhalten, wird standardmäßig auch Ihr Entwicklungscomputer vom Kubernetes-Cluster getrennt. Sie können dieses Verhalten ändern, indem Sie im Abschnitt *Kubernetes-Debugtools* in den Debugoptionen die Einstellung *Verbindung nach dem Debuggen trennen* auf *false* festlegen. Wenn Sie diese Einstellung geändert haben, bleibt Ihr Entwicklungscomputer verbunden, wenn Sie das Debuggen beenden und starten. Zum Trennen Ihres Entwicklungscomputers vom Cluster klicken Sie in der Symbolleiste auf die Schaltfläche *Trennen*.
>
> Wenn Visual Studio die Verbindung mit dem Cluster abrupt beendet, wird der Dienst, für den Sie die Umleitung vornehmen, möglicherweise nicht im ursprünglichen Zustand wiederhergestellt, bevor die Verbindung mit dem lokalen Prozess mit Kubernetes hergestellt wurde. Informationen zum Beheben dieses Problems finden Sie in den Anleitungen zur [Problembehandlung][troubleshooting].

## <a name="using-logging-and-diagnostics"></a>Verwenden der Protokollierung und Diagnose

Sie finden die Diagnoseprotokolle im Verzeichnis `Azure Dev Spaces` im Verzeichnis [*TEMP* des Entwicklungscomputers][azds-tmp-dir].

## <a name="remove-the-sample-application-from-your-cluster"></a>Entfernen der Beispielanwendung aus dem Cluster

Entfernen Sie die Beispielanwendung mithilfe des bereitgestellten Skripts von Ihrem Cluster.

```azurecli-interactive
./local-process-quickstart.sh -c -g MyResourceGroup -n MyAKS
```

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie mithilfe von Azure Dev Spaces und GitHub-Aktionen Änderungen eines Pull Request direkt in AKS testen können, bevor der Pull Request im Hauptzweig des Repositorys zusammengeführt wird.

> [!div class="nextstepaction"]
> [GitHub-Aktionen und Azure Kubernetes Service][gh-actions]

[azds-cli]: install-dev-spaces.md#install-the-client-side-tools
[azds-tmp-dir]: ../troubleshooting.md#before-you-begin
[azds-vs-code]: https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds
[azure-cli]: /cli/azure/install-azure-cli?view=azure-cli-latest
[azure-cloud-shell]: ../../cloud-shell/overview.md
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[az-aks-vs-code]: https://marketplace.visualstudio.com/items?itemName=ms-kubernetes-tools.vscode-aks-tools
[bike-sharing-github]: https://github.com/Azure/dev-spaces/tree/master/samples/BikeSharingApp
[gh-actions]: github-actions.md
[preview-terms]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/
[bikeshelper-cs-breakpoint]: https://github.com/Azure/dev-spaces/blob/master/samples/BikeSharingApp/ReservationEngine/BikesHelper.cs#L26
[supported-regions]: https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service
[troubleshooting]: ../troubleshooting.md#fail-to-restore-original-configuration-of-deployment-on-cluster
[visual-studio]: https://www.visualstudio.com/vs/