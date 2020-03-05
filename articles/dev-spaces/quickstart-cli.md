---
title: Entwickeln einer Anwendung unter Kubernetes
services: azure-dev-spaces
ms.date: 02/20/2020
ms.topic: quickstart
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie mit Azure Dev Spaces und der Befehlszeile eine Anwendung in Azure Kubernetes Service entwickeln.
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, Container, Helm, Service Mesh, Service Mesh-Routing, kubectl, k8s
manager: gwallace
ms.openlocfilehash: 974f0039bb16dc685bb056e279df63933e358edd
ms.sourcegitcommit: e4c33439642cf05682af7f28db1dbdb5cf273cc6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2020
ms.locfileid: "78245253"
---
# <a name="quickstart-develop-an-application-on-kubernetes---azure-dev-spaces"></a>Schnellstart: Entwickeln einer Anwendung unter Kubernetes – Azure Dev Spaces
In diesem Leitfaden lernen Sie Folgendes:

- Einrichten von Azure Dev Spaces mit einem verwalteten Kubernetes-Cluster in Azure
- Entwickeln und Ausführen von Code in Containern über die Befehlszeile

## <a name="prerequisites"></a>Voraussetzungen

- ein Azure-Abonnement. Falls Sie über kein Azure-Abonnement verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free) erstellen.
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

Klonen Sie die Anwendung über GitHub, und navigieren Sie zum Verzeichnis *dev-spaces/samples/nodejs/getting-started/webfrontend*:

```cmd
git clone https://github.com/Azure/dev-spaces
cd dev-spaces/samples/nodejs/getting-started/webfrontend
```

## <a name="prepare-the-application"></a>Vorbereiten der Anwendung

Um die Anwendung in Azure Dev Spaces ausführen zu können, benötigen Sie ein Dockerfile und ein Helm-Diagramm. Bei einigen Sprachen wie z.B. [Java][java-quickstart], [.NET Core][netcore-quickstart] und [Node.js][nodejs-quickstart] können die Azure Dev Spaces-Clienttools alle erforderlichen Ressourcen generieren. Bei vielen anderen Sprachen (wie etwa Go, PHP und Python) können die Clienttools das Helm-Chart generieren, wenn Sie ein gültiges Dockerfile bereitstellen.

Generieren Sie die Docker- und Helm-Diagrammobjekte zum Ausführen der Anwendung in Kubernetes mit dem Befehl `azds prep`:

```cmd
azds prep --enable-ingress
```

Sie müssen den Befehl `prep` im Verzeichnis *dev-spaces/samples/nodejs/getting-started/webfrontend* ausführen, um die Docker- und Helm-Diagrammobjekte richtig zu generieren.

> [!TIP]
> Der Befehl `prep` versucht, [ein Dockerfile und Helm-Diagramm](how-dev-spaces-works.md#prepare-your-code) für Ihr Projekt zu generieren. Azure Dev Spaces nutzt diese Dateien zum Erstellen und Ausführen Ihres Codes. Sie können sie jedoch ändern, wenn Sie anpassen möchten, wie das Projekt erstellt und ausgeführt wird.

## <a name="build-and-run-code-in-kubernetes"></a>Erstellen und Ausführen von Code in Kubernetes

Verwenden Sie den Befehl `azds up`, um Ihren Code in AKS zu erstellen und auszuführen:

```cmd
$ azds up
Using dev space 'default' with target 'MyAKS'
Synchronizing files...2s
Installing Helm chart...2s
Waiting for container image build...2m 25s
Building container image...
Step 1/8 : FROM node
Step 2/8 : ENV PORT 80
Step 3/8 : EXPOSE 80
Step 4/8 : WORKDIR /app
Step 5/8 : COPY package.json .
Step 6/8 : RUN npm install
Step 7/8 : COPY . .
Step 8/8 : CMD ["npm", "start"]
Built container image in 6m 17s
Waiting for container...13s
Service 'webfrontend' port 'http' is available at http://webfrontend.1234567890abcdef1234.eus.azds.io/
Service 'webfrontend' port 80 (http) is available at http://localhost:54256
...
```

Sie können die Ausführung des Diensts verfolgen, indem Sie die öffentliche URL öffnen, die in der Ausgabe des Befehls `azds up` angezeigt wird. In diesem Beispiel lautet die öffentliche URL *http://webfrontend.1234567890abcdef1234.eus.azds.io/* .

> [!NOTE]
> Wenn Sie während der Ausführung von `azds up` zu Ihrem Dienst navigieren, werden die Ablaufverfolgungen der HTTP-Anforderung ebenfalls in der Ausgabe des Befehls `azds up` angezeigt. Diese Ablaufverfolgungen können Sie bei der Problembehandlung und beim Debuggen Ihres Diensts unterstützen. Sie können diese Ablaufverfolgungen bei der Ausführung von `azds up` mit `--disable-http-traces` deaktivieren.

Wenn Sie die Ausführung des Befehls `azds up` mit *STRG+C* anhalten, wird der Dienst in AKS weiter ausgeführt, und die öffentliche URL bleibt verfügbar.

## <a name="update-code"></a>Aktualisieren des Codes

Zum Bereitstellen einer aktualisierten Version Ihres Diensts können Sie Dateien in Ihrem Projekt aktualisieren und den Befehl `azds up` erneut ausführen. Beispiel:

1. Drücken Sie *STRG+C*, wenn `azds up` noch ausgeführt wird.
1. Aktualisieren Sie die [Zeile 13 in `server.js`](https://github.com/Azure/dev-spaces/blob/master/samples/nodejs/getting-started/webfrontend/server.js#L13) wie folgt:
    
    ```javascript
        res.send('Hello from webfrontend in Azure');
    ```

1. Speichern Sie die Änderungen.
1. Führen Sie den Befehl `azds up` erneut aus:

    ```cmd
    $ azds up
    Using dev space 'default' with target 'MyAKS'
    Synchronizing files...1s
    Installing Helm chart...3s
    Waiting for container image build...
    ...    
    ```

1. Navigieren Sie zu Ihrem ausgeführten Dienst, und sehen Sie sich Ihre Änderungen an.
1. Drücken Sie *STRG+C*, um den Befehl `azds up` zu beenden.

## <a name="clean-up-your-azure-resources"></a>Bereinigen Ihrer Azure-Ressourcen

```azurecli
az group delete --name MyResourceGroup --yes --no-wait
```

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich darüber, wie Azure Dev Spaces Sie bei der Entwicklung komplexerer containerübergreifender Anwendungen unterstützt und wie Sie die gemeinsame Entwicklung vereinfachen können, indem Sie in verschiedenen Bereichen mit verschiedenen Versionen oder Branches Ihres Codes arbeiten.

> [!div class="nextstepaction"]
> [Schnellstart: Entwicklung im Team unter Kubernetes: Azure Dev Spaces][team-quickstart]

[java-quickstart]: quickstart-java.md
[nodejs-quickstart]: quickstart-nodejs.md
[netcore-quickstart]: quickstart-netcore.md
[team-quickstart]: quickstart-team-development.md
[supported-regions]: https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service