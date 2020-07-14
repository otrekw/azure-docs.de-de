---
title: Ausführen von Schnelltasks mit einer Vorlage
description: Einstellen einer ACR-Taskausführung in die Warteschlange, um ein Image mithilfe einer Azure Resource Manager-Vorlage zu erstellen
ms.topic: article
ms.date: 04/22/2020
ms.openlocfilehash: 7ad40d2e925d5e1443af9bce4115d45b0e8c06e1
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "82927767"
---
# <a name="run-acr-tasks-using-resource-manager-templates"></a>Ausführen von ACR Tasks mithilfe von Resource Manager-Vorlagen

[ACR Tasks](container-registry-tasks-overview.md) ist eine Suite mit Features in Azure Container Registry, die Sie beim Verwalten und Ändern von Containerimages während des gesamten Containerlebenszyklus unterstützt. 

In diesem Artikel werden Beispiele für Azure Resource Manager-Vorlagen gezeigt, um eine Schnelltaskausführung in die Warteschlange zu stellen, ähnlich wie bei einer, die Sie manuell mithilfe des Befehls [az acr build][az-acr-build] erstellen können.

Eine Resource Manager-Vorlage zum Einstellen einer Taskausführung in die Warteschlange ist in Automatisierungsszenarien nützlich und erweitert die Funktionalität von `az acr build`. Beispiel:

* Verwenden Sie eine Vorlage zum Erstellen einer Containerregistrierung und um eine Taskausführung sofort in die Warteschlange zu stellen, um ein Containerimage zu erstellen und zu pushen.
* Erstellen oder aktivieren Sie zusätzliche Ressourcen, die Sie in einer Schnelltaskausführung verwenden können, z. B. eine verwaltete Identität für Azure-Ressourcen.

## <a name="limitations"></a>Einschränkungen

* Sie müssen einen Remotekontext, z. B. ein GitHub-Repository, als [Quellspeicherort](container-registry-tasks-overview.md#context-locations) für ihre Taskausführung angeben. Sie können keinen lokalen Quellkontext verwenden.
* Für Taskausführungen, die eine verwaltete Identität verwenden, ist nur eine *benutzerseitig zugewiesene*, verwaltete Identität zulässig.

## <a name="prerequisites"></a>Voraussetzungen

* **Ein GitHub-Konto:** Falls Sie noch keins besitzen, erstellen Sie ein Konto unter https://github.com. 
* **Forken eines Beispielrepositorys**: Verwenden Sie für die hier gezeigten Taskbeispiel die GitHub-Benutzeroberfläche, um das folgende Beispielrepository in Ihr GitHub-Konto zu forken: https://github.com/Azure-Samples/acr-build-helloworld-node. Dieses Repository enthält Beispiel-Dockerfiles und Quellcode, um kleine Containerimages zu erstellen.

## <a name="example-create-registry-and-queue-task-run"></a>Beispiel: Erstellen einer Registrierung und Einstellen einer Taskausführung in die Warteschlange

In diesem Beispiel wird eine [Beispielvorlage](https://github.com/Azure/acr/tree/master/docs/tasks/run-as-deployment/quickdockerbuild) verwendet, um eine Containerregistrierung zu erstellen und um eine Taskausführung in die Warteschlange zu stellen, die ein Image erstellt und pusht. 

### <a name="template-parameters"></a>Vorlagenparameter

Geben Sie in diesem Beispiel Werte für die folgenden Vorlagenparameter an:

|Parameter  |Wert  |
|---------|---------|
|registryName     |Eindeutiger Name der erstellten Registrierung.         |
|repository     |Zielrepository für Buildtask.        |
|taskRunName     |Name der Taskausführung, der das Imagetag angibt. |
|sourceLocation     |Der Remotekontext für den Buildtask, z. B. https://github.com/Azure-Samples/acr-build-helloworld-node. Die Dockerfile im Repositorystamm erstellt ein Containerimage für eine kleine Node.js-Web-App. Verwenden Sie bei Bedarf Ihren Fork des Repositorys als Buildkontext.         |

### <a name="deploy-the-template"></a>Bereitstellen der Vorlage

Stellen Sie die Vorlage mit dem Befehl [az deployment group create][az-deployment-group-create] bereit. In diesem Beispiel wird das Image *helloworld-node:testrun* erstellt und in eine Registrierung mit dem Namen *mycontainerregistry* gepusht.

```azurecli
az deployment group create \
  --resource-group myResourceGroup \
  --template-uri https://raw.githubusercontent.com/Azure/acr/master/docs/tasks/run-as-deployment/quickdockerbuild/azuredeploy.json \
  --parameters \
    registryName=mycontainerregistry \
    repository=helloworld-node \
    taskRunName=testrun \
    sourceLocation=https://github.com/Azure-Samples/acr-build-helloworld-node.git
 ```

Der vorherige Befehl übergibt die Parameter in der Befehlszeile. Übergeben Sie sie bei Bedarf in einer [Parameterdatei](../azure-resource-manager/templates/parameter-files.md).

### <a name="verify-deployment"></a>Überprüfen der Bereitstellung

Nachdem die Bereitstellung erfolgreich abgeschlossen wurde, überprüfen Sie, ob das Image erstellt wurde, indem Sie [az acr repository show-tags][az-acr-repository-show-tags] ausführen:

```azurecli
az acr repository show-tags \
  --name mycontainerregistry \
  --repository helloworld-node --output table
```

Ausgabe:

```console
Result
--------
testrun
```

### <a name="view-run-log"></a>Anzeigen eines Ausführungsprotokolls

Um Details zur Taskausführung anzuzeigen, zeigen Sie das Ausführungsprotokoll an.

Rufen Sie zuerst mit [az acr task list-runs][az-acr-task-list-runs] die Ausführungs-ID ab.
```azurecli
az acr task list-runs \
  --registry mycontainerregistry --output table
```

Die Ausgabe Die Ausgabe lautet in etwa wie folgt:

```console
RUN ID    TASK    PLATFORM    STATUS     TRIGGER    STARTED               DURATION
--------  ------  ----------  ---------  ---------  --------------------  ----------
ca1               linux       Succeeded  Manual     2020-03-23T17:54:28Z  00:00:48
```

Führen Sie [az acr task logs][az-acr-task-logs] aus, um die Taskausführungsprotokolle für die Ausführungs-ID anzuzeigen, in diesem Fall *ca1*:

```azurecli
az acr task logs \
  --registry mycontainerregistry \
  --run-id ca1
```

In der Ausgabe wird das Taskausführungsprotokoll angezeigt.

Sie können das Taskausführungsprotokoll auch im Azure-Portal anzeigen. 

1. Navigieren Sie zu Ihrer Containerregistrierung
2. Wählen Sie unter **Dienste** die Option **Aufgaben** > **Ausführungen** aus.
3. Wählen Sie die Ausführungs-ID aus, in diesem Fall *ca1*. 

Im Portal wird das Taskausführungsprotokoll angezeigt.

## <a name="example-task-run-with-managed-identity"></a>Beispiel: Taskausführung mit verwalteter Identität

Verwenden Sie eine [Beispielvorlage](https://github.com/Azure/acr/tree/master/docs/tasks/run-as-deployment/quickdockerbuildwithidentity), um eine Taskausührung in eine Warteschlange zu stellen, die eine benutzerseitig zugewiesene verwaltete Identität ermöglicht. Während der Taskausführung wird die Identität authentifiziert, um ein Image aus einer anderen Azure-Containerregistrierung abzurufen. 

Dieses Szenario ähnelt [Registrierungsübergreifende Authentifizierung in einer ACR-Aufgabe unter Verwendung einer in Azure verwalteten Identität](container-registry-tasks-cross-registry-authentication.md). Beispielsweise kann eine Organisation eine zentralisierte Registrierung mit Basisimages verwalten, auf die mehrere Entwicklungsteams zugreifen.

### <a name="prepare-base-registry"></a>Vorbereiten der Basisregistrierung

Erstellen Sie zu Demonstrationszwecken eine gesonderte Containerregistrierung als Ihre Basisregistrierung, und pushen Sie ein vom Docker Hub abgerufenes Node.js-Basisimage.

1. Erstellen Sie eine zweite Containerregistrierung, z. B. *mybaseregistry*, um Basisimages zu speichern.
1. Pullen Sie das `node:9-alpine`-Image vom Docker Hub, markieren Sie es für Ihre Basisregistrierung, und pushen Sie es in die Basisregistrierung:

  ```azurecli
  docker pull node:9-alpine
  docker tag node:9-alpine mybaseregistry.azurecr.io/baseimages/node:9-alpine
  az acr login -n mybaseregistry
  docker push mybaseregistry.azurecr.io/baseimages/node:9-alpine
  ```

### <a name="create-new-dockerfile"></a>Erstellen eines neuen Dockerfile

Erstellen Sie ein Dockerfile, das Basisimage aus ihrer Basisregistrierung abruft. Führen Sie die folgenden Schritte in Ihrem lokalen Fork des GitHub-Repository aus, z. B. `https://github.com/myGitHubID/acr-build-helloworld-node.git`.

1. Wählen Sie in der GitHub-Benutzeroberfläche **Neue Datei erstellen** aus.
1. Nennen Sie Ihre Datei *Dockerfile-test*, und fügen Sie den folgenden Inhalt ein. Ersetzen Sie *mybaseregistry* durch den Namen Ihrer Registrierung.
    ```
    FROM mybaseregistry.azurecr.io/baseimages/node:9-alpine
    COPY . /src
    RUN cd /src && npm install
    EXPOSE 80
    CMD ["node", "/src/server.js"]
    ```
 1. Wählen Sie **Commit new file** (Neue Datei committen) aus.

[!INCLUDE [container-registry-tasks-user-assigned-id](../../includes/container-registry-tasks-user-assigned-id.md)]

### <a name="give-identity-pull-permissions-to-the-base-registry"></a>Erteilen von Basisregistrierungs-Pullberechtigungen für die Identität

Erteilen Sie der verwalteten Identität Berechtigungen zum Pullen von *mybaseregistry* aus der Basisregistrierung.

Verwenden Sie den Befehl [az acr show][az-acr-show], um die Ressourcen-ID der Basisregistrierung abzurufen und in einer Variablen zu speichern:

```azurecli
baseregID=$(az acr show \
  --name mybaseregistry \
  --query id --output tsv)
```

Verwenden Sie den Befehl [az role assignment create][az-role-assignment-create], um der Identität die Rolle „Acrpull“ für die Basisregistrierung zuzuweisen. Diese Rolle ist nur zum Pullen von Images aus der Registrierung berechtigt.

```azurecli
az role assignment create \
  --assignee $principalID \
  --scope $baseregID \
  --role acrpull
```

### <a name="template-parameters"></a>Vorlagenparameter

Geben Sie in diesem Beispiel Werte für die folgenden Vorlagenparameter an:

|Parameter  |Wert  |
|---------|---------|
|registryName     |Der Name der Registrierung, in der das Image erstellt wird.  |
|repository     |Zielrepository für Buildtask.        |
|taskRunName     |Name der Taskausführung, der das Imagetag angibt. |
|userAssignedIdentity |Die Ressourcen-ID der benutzerseitig zugewiesenen Identität, die in dem Task aktiviert ist.|
|customRegistryIdentity | Die Client-ID der benutzerseitig zugewiesenen Identität, die in dem Task aktiviert ist, die für die Authentifizierung mit der benutzerdefinierten Registrierung verwendet wird. |
|customRegistry |Der Name des Anmeldeservers der benutzerdefinierten Registrierung, auf die in dem Task zugegriffen wird, z. B. *mybaseregistry.azurecr.io*|
|sourceLocation     |Der Remotekontext für den Buildtask, z. B. *https://github.com/\<your-GitHub-ID\> /acr-build-helloworld-node.* |
|dockerFilePath | Der Pfad zur Dockerfile im Remotekontext, die zum Erstellen des Image verwendet wird. |

### <a name="deploy-the-template"></a>Bereitstellen der Vorlage

Stellen Sie die Vorlage mit dem Befehl [az deployment group create][az-deployment-group-create] bereit. In diesem Beispiel wird das Image *helloworld-node:testrun* erstellt und in eine Registrierung mit dem Namen *mycontainerregistry* gepusht. Das Basisimage wird von *mybaseregistry.azurecr.io* abgerufen.

```azurecli
az deployment group create \
  --resource-group myResourceGroup \
  --template-uri https://raw.githubusercontent.com/Azure/acr/master/docs/tasks/run-as-deployment/quickdockerbuildwithidentity/azuredeploy.json \
  --parameters \
    registryName=mycontainerregistry \
    repository=helloworld-node \
    taskRunName=basetask \
    userAssignedIdentity=$resourceID \
    customRegistryIdentity=$clientID \
    sourceLocation=https://github.com/<your-GitHub-ID>/acr-build-helloworld-node.git \
    dockerFilePath=Dockerfile-test \
    customRegistry=mybaseregistry.azurecr.io
```

Der vorherige Befehl übergibt die Parameter in der Befehlszeile. Übergeben Sie sie bei Bedarf in einer [Parameterdatei](../azure-resource-manager/templates/parameter-files.md).

### <a name="verify-deployment"></a>Überprüfen der Bereitstellung

Nachdem die Bereitstellung erfolgreich abgeschlossen wurde, überprüfen Sie, ob das Image erstellt wurde, indem Sie [az acr repository show-tags][az-acr-repository-show-tags] ausführen:

```azurecli
az acr repository show-tags \
  --name mycontainerregistry \
  --repository helloworld-node --output table
```

Ausgabe:

```console
Result
--------
basetask
```

### <a name="view-run-log"></a>Anzeigen eines Ausführungsprotokolls

Informationen zum Anzeigen des Ausführungsprotokolls finden Sie im [vorangehenden Abschnitt](#view-run-log).

## <a name="next-steps"></a>Nächste Schritte

 * Weitere Vorlagenbeispiele finden Sie im [ACR GitHub-Repository](https://github.com/Azure/acr/tree/master/docs/tasks/run-as-deployment).
 * Ausführliche Informationen zu Vorlageneigenschaften finden Sie in der Vorlagenreferenz für [Taskausführungen](/azure/templates/microsoft.containerregistry/2019-06-01-preview/registries/taskruns) und [Tasks](/azure/templates/microsoft.containerregistry/2019-06-01-preview/registries/tasks).


<!-- LINKS - Internal -->
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-build]: /cli/azure/acr#az-acr-build
[az-acr-show]: /cli/azure/acr#az-acr-show
[az-acr-task-run]: /cli/azure/acr/task#az-acr-task-run
[az-acr-task-logs]: /cli/azure/acr/task#az-acr-task-logs
[az-acr-repository-show-tags]: /cli/azure/acr/repository#az-acr-repository-show-tags
[az-acr-task-list-runs]: /cli/azure/acr/task#az-acr-task-list-runs
[az-deployment-group-create]: /cli/azure/deployment/group#az-deployment-group-create
[az-identity-create]: /cli/azure/identity#az-identity-create
[az-identity-show]: /cli/azure/identity#az-identity-show
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
