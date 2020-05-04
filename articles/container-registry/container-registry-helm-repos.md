---
title: Speichern von Helm-Diagrammen
description: Erfahren Sie, wie Sie Helm-Charts für Ihre Kubernetes-Anwendungen mithilfe von Depots in Azure Container Registry speichern.
ms.topic: article
ms.date: 03/20/2020
ms.openlocfilehash: cf9f9667438519443f857e43c0bbc94d6c239e0a
ms.sourcegitcommit: b9d4b8ace55818fcb8e3aa58d193c03c7f6aa4f1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "82583345"
---
# <a name="push-and-pull-helm-charts-to-an-azure-container-registry"></a>Pushen und Pullen von Helm-Charts in Azure Container Registry

Sie können den [Open Source-Paket-Manager von Helm][helm] verwenden, um Anwendungen für Kubernetes ohne großen Aufwand zu verwalten und bereitzustellen. Bei Helm werden Anwendungspakete als [Charts](https://helm.sh/docs/topics/charts/) definiert, die gesammelt und in einem [Helm-Chartrepository](https://helm.sh/docs/topics/chart_repository/) gespeichert werden.

In diesem Artikel erfahren Sie, wie Sie mit einer Helm 3- oder Helm 2-Installation Helm-Chartrepositorys in einer Azure Container Registry-Instanz hosten. In vielen Szenarien würden Sie für Ihre selbst entwickelten Anwendungen eigene Charts erstellen und hochladen. Weitere Informationen zum Erstellen eigener Helm-Charts finden Sie unter [Entwicklerhandbuch für Chartvorlagen][develop-helm-charts]. Sie können auch ein vorhandenes Helm-Chart aus einem anderen Helm-Repository speichern.

> [!IMPORTANT]
> Die Unterstützung von Helm-Charts in Azure Container Registry befindet sich derzeit in der Vorschauphase. Wenn Sie Vorschauversionen nutzen möchten, müssen Sie die zusätzlichen [Nutzungsbedingungen][terms-of-use] akzeptieren. Einige Aspekte dieses Features werden bis zur allgemeinen Verfügbarkeit unter Umständen noch geändert.

## <a name="helm-3-or-helm-2"></a>Helm 3 oder Helm 2?

Zum Speichern, Verwalten und Installieren von Helm-Charts verwenden Sie einen Helm-Client und die Helm-Befehlszeilenschnittstelle. Zu den Hauptversionen des Helm-Clients zählen Helm 3 und Helm 2. Helm 3 unterstützt ein neues Chartformat, und die serverseitige Tiller-Komponente wird nicht mehr installiert. Ausführliche Informationen zu den Versionsunterschieden finden Sie in den [häufig gestellten Fragen (FAQ) zur Version](https://helm.sh/docs/faq/). Wenn Sie zuvor Helm 2-Charts bereitgestellt haben, lesen Sie [Migrating Helm v2 to v3](https://helm.sh/docs/topics/v2_v3_migration/) (Migrieren von Helm v2 zu v3).

Sie können zum Hosten von Helm-Charts in Azure Container Registry entweder Helm 3 oder Helm 2 verwenden, wobei für jede Version spezifische Workflows gelten:

* [Helm 3-Client:](#use-the-helm-3-client) Verwenden Sie Befehle vom Typ `helm chart` in der Helm-Befehlszeilenschnittstelle, um Charts in Ihrer Registrierung als [OCI-Artefakte](container-registry-image-formats.md#oci-artifacts) zu verwalten.
* [Helm 2-Client:](#use-the-helm-2-client) Verwenden Sie [az acr helm][az-acr-helm]-Befehle in der Azure-Befehlszeilenschnittstelle, um Ihre Containerregistrierung als Helm-Chartrepository hinzuzufügen und zu verwalten.

### <a name="additional-information"></a>Zusätzliche Informationen

* Für die meisten Szenarien wird der Helm 3-Workflow mit nativen Befehlen vom Typ `helm chart` empfohlen, um Charts als OCI-Artefakte zu verwalten.
* Ab Helm 3 werden [az acr helm][az-acr-helm]-Befehle aus Gründen der Kompatibilität mit dem Helm 2-Client und dem -Chartformat unterstützt. Eine zukünftige Weiterentwicklungen dieser Befehle ist derzeit nicht geplant. Siehe die [Produktroadmap](https://github.com/Azure/acr/blob/master/docs/acr-roadmap.md#acr-helm-ga).

## <a name="use-the-helm-3-client"></a>Verwenden des Helm 3-Clients

### <a name="prerequisites"></a>Voraussetzungen

- Eine **Azure Container Registry**-Instanz in Ihrem Azure-Abonnement. Erstellen Sie bei Bedarf eine Registrierung im [Azure-Portal](container-registry-get-started-portal.md) oder über die [Azure-Befehlszeilenschnittstelle](container-registry-get-started-azure-cli.md).
- **Version 3.1.0 oder höher des Helm-Clients:** Führen Sie `helm version` aus, um Ihre aktuelle Version zu ermitteln. Weitere Informationen zum Installieren und Aktualisieren von Helm finden Sie unter [Installieren von Helm][helm-install].
- **Ein Kubernetes-Cluster**, in dem Sie später ein Helm-Chart installieren. Erstellen Sie bei Bedarf einen [Azure Kubernetes Service-Cluster][aks-quickstart]. 
- **Version 2.0.71 oder höher der Azure-Befehlszeilenschnittstelle:** Führen Sie `az --version` aus, um die Version zu ermitteln. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sie bei Bedarf unter [Installieren der Azure CLI][azure-cli-install].

### <a name="high-level-workflow"></a>Allgemeiner Workflow

Mit **Helm 3** haben Sie folgende Möglichkeiten:

* Erstellen von Helm-Repositorys in Azure Container Registry
* Speichern von Helm 3-Charts in einer Registrierung als [OCI-Artefakte](container-registry-image-formats.md#oci-artifacts). Die Helm 3-Unterstützung für OCI ist momentan *experimentell*.
* Authentifizieren bei Ihrer Registrierung mithilfe des Befehls `helm registry login`
* Pushen, Pullen und Verwalten von Helm-Charts in einer Registrierung mithilfe von Befehlen vom Typ `helm chart` in der Helm-Befehlszeilenschnittstelle
* Verwenden Sie `helm install`, um Charts aus einem lokalen Repositorycache in einem Kubernetes-Cluster zu installieren.

Beispiele hierzu finden Sie in den nachfolgenden Abschnitten.

### <a name="enable-oci-support"></a>Aktivieren der OCI-Unterstützung

Legen Sie die folgende Umgebungsvariable fest, um die OCI-Unterstützung im Helm 3-Client zu aktivieren. Diese Unterstützung ist derzeit experimentell. 

```console
export HELM_EXPERIMENTAL_OCI=1
```

### <a name="create-a-sample-chart"></a>Erstellen eines Beispielcharts

Erstellen Sie mithilfe der folgenden Befehle ein Testchart:

```console
mkdir helmtest

cd helmtest
helm create hello-world
```

Wechseln Sie für dieses einfache Beispiel zunächst in den Ordner `templates`, und löschen Sie den Inhalt des Ordners:

```console
cd hello-world/templates
rm -rf *
```

Erstellen Sie im Ordner `configmap.yaml` eine Datei namens `templates` mit folgendem Inhalt:

```console
cat <<EOF > configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: hello-world-configmap
data:
  myvalue: "Hello World"
EOF
```

Weitere Informationen zum Erstellen und Ausführen dieses Beispiels finden Sie in der Helm-Dokumentation unter [Erste Schritte](https://helm.sh/docs/chart_template_guide/getting_started/).

### <a name="save-chart-to-local-registry-cache"></a>Speichern des Charts im lokalen Registrierungscache

Wechseln Sie in das Unterverzeichnis `hello-world`. Führen Sie anschließend `helm chart save` aus, um eine Kopie des Charts lokal zu speichern und einen Alias mit dem vollqualifizierten Namen der Registrierung (nur Kleinbuchstaben) sowie des Zielrepositorys und des Tags zu erstellen. 

Im folgenden Beispiel lautet der Registrierungsname *mycontainerregistry*, das Zielrepository ist *hello-world*, und das Zielcharttag ist *v1*. Ersetzen Sie diese Werte jedoch durch Werte für Ihre Umgebung:

```console
cd ..
helm chart save . hello-world:v1
helm chart save . mycontainerregistry.azurecr.io/helm/hello-world:v1
```

Führen Sie `helm chart list` aus, um zu überprüfen, ob die Charts im lokalen Registrierungscache gespeichert wurden. Die Ausgabe Die Ausgabe lautet in etwa wie folgt:

```console
REF                                                      NAME            VERSION DIGEST  SIZE            CREATED
hello-world:v1                                           hello-world       0.1.0   5899db0 3.2 KiB        2 minutes 
mycontainerregistry.azurecr.io/helm/hello-world:v1       hello-world       0.1.0   5899db0 3.2 KiB        2 minutes
```

### <a name="authenticate-with-the-registry"></a>Authentifizieren bei der Registrierung

Führen Sie in der Helm 3-Befehlszeilenschnittstelle den Befehl `helm registry login` aus, um sich [bei der Registrierung zu authentifizieren](container-registry-authentication.md), und verwenden Sie geeignete Anmeldeinformationen für Ihr Szenario.

Erstellen Sie beispielsweise einen [Azure Active Directory-Dienstprinzipal mit Pull- und Pushberechtigungen](container-registry-auth-service-principal.md#create-a-service-principal) (AcrPush-Rolle) für die Registrierung. Geben Sie anschließend die Dienstprinzipal-Anmeldeinformationen für `helm registry login` an. Im folgenden Beispiel wird das Kennwort mithilfe einer Umgebungsvariablen angegeben:

```console
echo $spPassword | helm registry login mycontainerregistry.azurecr.io \
  --username <service-principal-id> \
  --password-stdin
```

### <a name="push-chart-to-azure-container-registry"></a>Pushen von Charts an Azure Container Registry

Führen Sie den Befehl `helm chart push` in der Helm 3-Befehlszeilenschnittstelle aus, um das Chart in das vollqualifizierte Zielrepository zu pushen:

```console
helm chart push mycontainerregistry.azurecr.io/helm/hello-world:v1
```

Nach einem erfolgreichen Push ähnelt die Ausgabe der folgenden:

```output
The push refers to repository [mycontainerregistry.azurecr.io/helm/hello-world]
ref:     mycontainerregistry.azurecr.io/helm/hello-world:v1
digest:  5899db028dcf96aeaabdadfa5899db025899db025899db025899db025899db02
size:    3.2 KiB
name:    hello-world
version: 0.1.0
```

### <a name="list-charts-in-the-repository"></a>Auflisten von Diagrammen im Repository

Wie bei in Azure Container Registry gespeicherten Images können Sie mithilfe von [az acr repository][az-acr-repository]-Befehlen die Depots anzeigen, in denen Ihre Charts gehostet werden, sowie die Charttags und -manifeste. 

Führen Sie z. B. [az acr repository show][az-acr-repository-show] aus, um die Eigenschaften des im vorherigen Schritt erstellten Repositorys anzuzeigen:

```azurecli
az acr repository show \
  --name mycontainerregistry \
  --repository helm/hello-world
```

Die Ausgabe Die Ausgabe lautet in etwa wie folgt:

```output
{
  "changeableAttributes": {
    "deleteEnabled": true,
    "listEnabled": true,
    "readEnabled": true,
    "writeEnabled": true
  },
  "createdTime": "2020-03-20T18:11:37.6701689Z",
  "imageName": "helm/hello-world",
  "lastUpdateTime": "2020-03-20T18:11:37.7637082Z",
  "manifestCount": 1,
  "registry": "mycontainerregistry.azurecr.io",
  "tagCount": 1
}
```

Führen Sie den Befehl [az acr repository show-manifests][az-acr-repository-show-manifests] aus, um die Details des im Repository gespeicherten Charts anzuzeigen. Beispiel:

```azurecli
az acr repository show-manifests \
  --name mycontainerregistry \
  --repository helm/hello-world --detail
```

Die Ausgabe (in diesem Beispiel abgekürzt) zeigt `application/vnd.cncf.helm.config.v1+json` als `configMediaType` an:

```output
[
  {
    [...]
    "configMediaType": "application/vnd.cncf.helm.config.v1+json",
    "createdTime": "2020-03-20T18:11:37.7167893Z",
    "digest": "sha256:0c03b71c225c3ddff53660258ea16ca7412b53b1f6811bf769d8c85a1f0663ee",
    "imageSize": 3301,
    "lastUpdateTime": "2020-03-20T18:11:37.7167893Z",
    "mediaType": "application/vnd.oci.image.manifest.v1+json",
    "tags": [
      "v1"
    ]
```

### <a name="pull-chart-to-local-cache"></a>Pullen eines Charts in den lokalen Cache

Um ein Helm-Chart in Kubernetes zu installieren, muss sich das Chart im lokalen Cache befinden. Führen Sie in diesem Beispiel zuerst `helm chart remove` aus, um das vorhandene lokale Chart mit dem Namen `mycontainerregistry.azurecr.io/helm/hello-world:v1` zu entfernen:

```console
helm chart remove mycontainerregistry.azurecr.io/helm/hello-world:v1
```

Führen Sie `helm chart pull` aus, um das Chart aus Azure Container Registry in Ihren lokalen Cache herunterzuladen:

```console
helm chart pull mycontainerregistry.azurecr.io/helm/hello-world:v1
```

### <a name="export-helm-chart"></a>Exportieren von Helm-Charts

Wenn Sie mit dem Chart weiterarbeiten möchten, exportieren Sie es mithilfe von `helm chart export` in ein lokales Verzeichnis. Exportieren Sie z. B. das Chart, das Sie in das Verzeichnis `install` gepullt haben:

```console
helm chart export mycontainerregistry.azurecr.io/helm/hello-world:v1 \
  --destination ./install
```

Um Informationen zum exportierten Chart im Repository anzuzeigen, führen Sie den Befehl `helm show chart` in dem Verzeichnis aus, in das Sie das Chart exportiert haben.

```console
cd install
helm show chart hello-world
```

Helm gibt ausführliche Informationen zur aktuellen Version Ihres Charts zurück, wie in der folgenden Beispielausgabe zu sehen:

```output
apiVersion: v2
appVersion: 1.16.0
description: A Helm chart for Kubernetes
name: hello-world
type: application
version: 0.1.0    
```

### <a name="install-helm-chart"></a>Installieren von Helm-Charts

Führen Sie `helm install` aus, um das Helm-Chart, das Sie in den lokalen Cache gepullt und exportiert haben, zu installieren. Geben Sie einen Releasenamen (beispielsweise *myhelmtest*) an, oder übergeben Sie den Parameter `--generate-name`. Beispiel:

```console
helm install myhelmtest ./hello-world
```

Die Ausgabe nach erfolgreicher Chartinstallation sieht in etwa wie folgt aus:

```console
NAME: myhelmtest
LAST DEPLOYED: Fri Mar 20 14:14:42 2020
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None
```

Führen Sie den Befehl `helm get manifest` aus, um die Installation zu überprüfen. Der Befehl gibt die YAML-Daten in der Vorlagendatei `configmap.yaml` zurück.

Führen Sie `helm uninstall` aus, um das Chart-Release in Ihrem Cluster zu deinstallieren:

```console
helm uninstall myhelmtest
```

### <a name="delete-a-helm-chart-from-the-repository"></a>Löschen eines Helm-Diagramms aus dem Repository

Wenn Sie ein Chart aus dem Repository löschen möchten, verwenden Sie den Befehl [az acr repository delete][az-acr-repository-delete]. Führen Sie den folgenden Befehl aus, und bestätigen Sie den Vorgang, wenn Sie dazu aufgefordert werden:

```azurecli
az acr repository delete --name mycontainerregistry --image helm/hello-world:v1
```

## <a name="use-the-helm-2-client"></a>Verwenden des Helm 2-Clients

### <a name="prerequisites"></a>Voraussetzungen

- Eine **Azure Container Registry**-Instanz in Ihrem Azure-Abonnement. Erstellen Sie bei Bedarf eine Registrierung im [Azure-Portal](container-registry-get-started-portal.md) oder über die [Azure-Befehlszeilenschnittstelle](container-registry-get-started-azure-cli.md).
- **Version 2.11.0 oder höher des Helm-Clients (keine RC-Version):** Führen Sie `helm version` aus, um Ihre aktuelle Version zu ermitteln. Darüber hinaus benötigen Sie einen Helm-Server (Tiller), der in einem Kubernetes-Cluster initialisiert wird. Erstellen Sie bei Bedarf einen [Azure Kubernetes Service-Cluster][aks-quickstart]. Weitere Informationen zum Installieren und Aktualisieren von Helm finden Sie unter [Installieren von Helm][helm-install-v2].
- **Azure CLI-Version 2.0.46 oder höher** : Führen Sie `az --version` aus, um die Version zu ermitteln. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sie bei Bedarf unter [Installieren der Azure CLI][azure-cli-install].

### <a name="high-level-workflow"></a>Allgemeiner Workflow

Mit **Helm 2** haben Sie folgende Möglichkeiten:

* Konfigurieren Ihrer Azure Container Registry-Instanz als *einzelnes* Helm-Chartrepository. Azure Container Registry verwaltet die Indexdefinition, wenn Sie dem Repository Charts hinzufügen und daraus entfernen.
* Authentifizieren Sie sich über die Azure-Befehlszeilenschnittstelle bei Azure Container Registry. Ihr Helm-Client wird dann automatisch mit dem Registrierungs-URI und den Anmeldeinformationen aktualisiert. Sie müssen diese Registrierungsinformationen nicht manuell eingeben. Die Anmeldeinformationen sind daher nicht aus dem Befehlsverlauf ersichtlich.
* Verwenden Sie [az acr helm][az-acr-helm]-Befehle an der Azure-Befehlszeilenschnittstelle, um Ihre Azure Container Registry-Instanz als Helm-Chartrepository hinzuzufügen und Charts zu pushen und zu verwalten. Diese Befehle der Azure-Befehlszeilenschnittstelle umschließen Helm 2-Clientbefehle.
* Hinzufügen des Chartrepositorys in Ihrer Azure-Containerregistrierung zum Index Ihres lokalen Helm-Repositorys, um die Chartsuche zu unterstützen
* Verwenden Sie `helm install`, um Charts aus einem lokalen Repositorycache in einem Kubernetes-Cluster zu installieren.

Beispiele hierzu finden Sie in den nachfolgenden Abschnitten.

### <a name="add-repository-to-helm-client"></a>Hinzufügen eines Repositorys zum Helm-Client

Fügen Sie Ihrem Helm-Client mithilfe des Befehls [az acr helm repo add][az-acr-helm-repo-add] das Helm-Chartrepository in Ihrer Azure Container Registry-Instanz hinzu. Mit diesem Befehl wird ein Authentifizierungstoken für Ihre Azure Container Registry abgerufen, das vom Helm-Client verwendet wird. Das Authentifizierungstoken ist drei Stunden lang gültig. Ähnlich wie `docker login` können Sie diesen Befehl in zukünftigen CLI-Sitzungen ausführen, um Ihren Helm-Client bei Ihrem Helm-Repository für Diagramme in Azure Container Registry zu authentifizieren:

```azurecli
az acr helm repo add --name mycontainerregistry
```

### <a name="add-a-sample-chart-to-the-repository"></a>Hinzufügen eines Beispielcharts zum Repository

Erstellen Sie zunächst unter *~/acr-helm* ein lokales Verzeichnis, und laden Sie das vorhandene Chart *stable/wordpress* herunter:

```console
mkdir ~/acr-helm && cd ~/acr-helm
helm repo update
helm fetch stable/wordpress
```

Geben Sie `ls` ein, um das heruntergeladene Chart aufzulisten, und notieren Sie sich die im Dateinamen enthaltene WordPress-Version. Durch den Befehl `helm fetch stable/wordpress` wird keine bestimmte Version angegeben. Daher wurde die *aktuelle* Version abgerufen. In der folgenden Beispielausgabe weist das WordPress-Chart die Version *8.1.0* auf:

```output
wordpress-8.1.0.tgz
```

Pushen Sie das Chart mithilfe des Befehls [az acr helm push][az-acr-helm-push] der Azure-Befehlszeilenschnittstelle in Ihr Helm-Chartrepository in Azure Container Registry. Geben Sie den Namen des Helm-Charts an, das Sie im vorherigen Schritt heruntergeladen haben, z. B. *wordpress-8.1.0.tgz*:

```azurecli
az acr helm push --name mycontainerregistry wordpress-8.1.0.tgz
```

Nach kurzer Zeit meldet die Azure-Befehlszeilenschnittstelle, dass das Chart gespeichert wurde, wie in der folgenden Beispielausgabe gezeigt wird:

```output
{
  "saved": true
}
```

### <a name="list-charts-in-the-repository"></a>Auflisten von Diagrammen im Repository

Damit das im vorherigen Schritt hochgeladene Diagramm verwendet werden kann, muss der lokale Index des Helm-Repositorys aktualisiert werden. Sie können die Repositorys im Helm-Client neu indizieren oder den Repositoryindex über die Azure CLI aktualisieren. Jedes Mal, wenn Sie ein Diagramm zu Ihrem Repository hinzufügen, müssen Sie den folgenden Schritt ausführen:

```azurecli
az acr helm repo add --name mycontainerregistry
```

Wenn in Ihrem Repository ein Diagramm gespeichert und der aktualisierte Index lokal verfügbar ist, können Sie einen Suchvorgang oder eine Installation mithilfe der regulären Befehle des Helm-Clients durchführen. Um alle Charts in Ihrem Repository anzuzeigen, verwenden Sie den Befehl `helm search`, und geben Sie Ihren eigenen Azure Container Registry-Namen an:

```console
helm search mycontainerregistry
```

Das Wordpress-Diagramm, das im vorherigen Schritt mithilfe von Push übertragen wurde, wird aufgelistet, wie in der folgenden Beispielausgabe gezeigt wird:

```output
NAME                  CHART VERSION    APP VERSION    DESCRIPTION
helmdocs/wordpress    8.1.0           5.3.2          Web publishing platform for building blogs and websites.
```

Sie können die Charts auch über die Azure-Befehlszeilenschnittstelle auflisten. Verwenden Sie dazu den Befehl [az acr helm list][az-acr-helm-list]:

```azurecli
az acr helm list --name mycontainerregistry
```

### <a name="show-information-for-a-helm-chart"></a>Anzeigen von Informationen zu einem Helm-Diagramm

Wenn Sie im Repository Informationen zu einem bestimmten Chart anzeigen möchten, können Sie den Befehl `helm inspect` verwenden.

```console
helm inspect mycontainerregistry/wordpress
```

Wenn keine Versionsnummer angegeben ist, wird die *aktuelle* Version verwendet. Helm Gibt ausführliche Informationen zu Ihrem Diagramm zurück, wie in der folgenden verkürzten Beispielausgabe gezeigt wird:

```output
apiVersion: v1
appVersion: 5.3.2
description: Web publishing platform for building blogs and websites.
engine: gotpl
home: http://www.wordpress.com/
icon: https://bitnami.com/assets/stacks/wordpress/img/wordpress-stack-220x234.png
keywords:
- wordpress
- cms
- blog
- http
- web
- application
- php
maintainers:
- email: containers@bitnami.com
  name: Bitnami
name: wordpress
sources:
- https://github.com/bitnami/bitnami-docker-wordpress
version: 8.1.0
[...]
```

Die Informationen zu einem Chart können auch über die Azure-Befehlszeilenschnittstelle angezeigt werden. Verwenden Sie dazu den Befehl [az acr helm show][az-acr-helm-show]. Auch hier wird standardmäßig die *aktuelle* Version eines Diagramms zurückgegeben. Sie können `--version` anfügen, um eine bestimmte Version eines Charts aufzulisten, z. B. *8.1.0*:

```azurecli
az acr helm show --name mycontainerregistry wordpress
```

### <a name="install-a-helm-chart-from-the-repository"></a>Installieren eines Helm-Diagramms aus dem Repository

Das Helm-Chart in Ihrem Repository wird installiert, indem Sie den Repositorynamen und den Chartnamen angeben. Sie können das Wordpress-Diagramm mithilfe des Helm-Clients installieren:

```console
helm install mycontainerregistry/wordpress
```

> [!TIP]
> Wenn Sie das Diagramm mithilfe von Push in Ihr Helm-Repository für Diagramme in Azure Container Registry übertragen und später in einer neuen CLI-Sitzung zurückkehren, benötigt Ihr lokaler Helm-Client ein aktualisiertes Authentifizierungstoken. Mithilfe des Befehls [az acr helm repo add][az-acr-helm-repo-add] können Sie ein neues Authentifizierungstoken abrufen.

Während des Installationsprozesses werden die folgenden Schritte ausgeführt:

- Der Helm-Client durchsucht den lokalen Repositoryindex.
- Das entsprechende Diagramm wird aus dem Azure Container Registry-Repository heruntergeladen.
- Das Diagramm wird mit Tiller in Ihrem Kubernetes-Cluster bereitgestellt.

Befolgen Sie während der Installation die Anweisungen in der Befehlsausgabe, um die URLs und die Anmeldeinformationen für WordPress anzuzeigen. Sie können auch den Befehl `kubectl get pods` ausführen, um die im Helm-Chart bereitgestellten Kubernetes-Ressourcen anzuzeigen:

```output
NAME                                    READY   STATUS    RESTARTS   AGE
wordpress-1598530621-67c77b6d86-7ldv4   1/1     Running   0          2m48s
wordpress-1598530621-mariadb-0          1/1     Running   0          2m48s
[...]
```

### <a name="delete-a-helm-chart-from-the-repository"></a>Löschen eines Helm-Diagramms aus dem Repository

Wenn Sie ein Chart aus dem Repository löschen möchten, verwenden Sie den Befehl [az acr helm delete][az-acr-helm-delete]. Geben Sie den Namen des Charts, z. B. *wordpress*, und die zu löschende Version an, z. B. *8.1.0*.

```azurecli
az acr helm delete --name mycontainerregistry wordpress --version 8.1.0
```

Lassen Sie den Parameter `--version` aus, wenn alle Versionen des benannten Diagramms gelöscht werden sollen.

Das Chart wird weiterhin zurückgegeben, wenn Sie `helm search` ausführen. Auch hier aktualisiert der Helm-Client nicht automatisch die Liste der in einem Repository verfügbaren Diagramme. Mithilfe des Befehls [az acr helm repo add][az-acr-helm-repo-add] können Sie den Repositoryindex des Helm-Clients aktualisieren:

```azurecli
az acr helm repo add --name mycontainerregistry
```

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zum Erstellen und Bereitstellen von Helm-Charts finden Sie unter [Charts][develop-helm-charts].
* Weitere Informationen zum Installieren von Anwendungen mit Helm in Azure Kubernetes Service (AKS) finden Sie [hier](../aks/kubernetes-helm.md).
* Helm-Diagramme können als Teil des Containererstellungsprozesses verwendet werden. Weitere Informationen finden Sie unter [Verwenden von Azure Container Registry Tasks][acr-tasks].

<!-- LINKS - external -->
[helm]: https://helm.sh/
[helm-install]: https://helm.sh/docs/intro/install/
[helm-install-v2]: https://v2.helm.sh/docs/using_helm/#installing-helm
[develop-helm-charts]: https://helm.sh/docs/chart_template_guide/
[semver2]: https://semver.org/
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - internal -->
[azure-cli-install]: /cli/azure/install-azure-cli
[aks-quickstart]: ../aks/kubernetes-walkthrough.md
[acr-bestpractices]: container-registry-best-practices.md
[az-configure]: /cli/azure/reference-index#az-configure
[az-acr-login]: /cli/azure/acr#az-acr-login
[az-acr-helm]: /cli/azure/acr/helm
[az-acr-repository]: /cli/azure/acr/repository
[az-acr-repository-show]: /cli/azure/acr/repository#az-acr-repository-show
[az-acr-repository-delete]: /cli/azure/acr/repository#az-acr-repository-delete
[az-acr-repository-show-tags]: /cli/azure/acr/repository#az-acr-repository-show-tags
[az-acr-repository-show-manifests]: /cli/azure/acr/repository#az-acr-repository-show-manifests
[az-acr-helm-repo-add]: /cli/azure/acr/helm/repo#az-acr-helm-repo-add
[az-acr-helm-push]: /cli/azure/acr/helm#az-acr-helm-push
[az-acr-helm-list]: /cli/azure/acr/helm#az-acr-helm-list
[az-acr-helm-show]: /cli/azure/acr/helm#az-acr-helm-show
[az-acr-helm-delete]: /cli/azure/acr/helm#az-acr-helm-delete
[acr-tasks]: container-registry-tasks-overview.md
