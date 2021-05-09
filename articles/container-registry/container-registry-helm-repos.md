---
title: Speichern von Helm-Diagrammen
description: Erfahren Sie, wie Sie Helm-Charts für Ihre Kubernetes-Anwendungen mithilfe von Depots in Azure Container Registry speichern.
ms.topic: article
ms.date: 04/15/2021
ms.openlocfilehash: c7dcdf222e9628daedb7e1c3617efb0b9c7af185
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/20/2021
ms.locfileid: "107772377"
---
# <a name="push-and-pull-helm-charts-to-an-azure-container-registry"></a>Pushen und Pullen von Helm-Charts in Azure Container Registry

Sie können den [Open Source-Paket-Manager von Helm][helm] verwenden, um Anwendungen für Kubernetes ohne großen Aufwand zu verwalten und bereitzustellen. Bei Helm werden Anwendungspakete als [Charts](https://helm.sh/docs/topics/charts/) definiert, die gesammelt und in einem [Helm-Chartrepository](https://helm.sh/docs/topics/chart_repository/) gespeichert werden.

In diesem Artikel erfahren Sie, wie Sie mithilfe von Helm 3-Befehlen Helm-Chartrepositorys in einer Azure Container Registry-Instanz hosten. In vielen Szenarien würden Sie für Ihre selbst entwickelten Anwendungen eigene Charts erstellen und hochladen. Weitere Informationen zum Erstellen eigener Helm-Charts finden Sie unter [Entwicklerhandbuch für Chartvorlagen][develop-helm-charts]. Sie können auch ein vorhandenes Helm-Chart aus einem anderen Helm-Repository speichern.

## <a name="helm-3-or-helm-2"></a>Helm 3 oder Helm 2?

Zum Speichern, Verwalten und Installieren von Helm-Charts verwenden Sie einen Helm-Client und die Helm-Befehlszeilenschnittstelle. Zu den Hauptversionen des Helm-Clients zählen Helm 3 und Helm 2. Ausführliche Informationen zu den Versionsunterschieden finden Sie in den [häufig gestellten Fragen (FAQ) zur Version](https://helm.sh/docs/faq/). 

Helm 3 sollte zum Hosten von Helm-Charts in Azure Container Registry verwendet werden. Mit Helm 3 haben Sie folgende Möglichkeiten:

* Erstellen von Helm-Repositorys in Azure Container Registry
* Speichern von Helm 3-Charts in einer Registrierung als [OCI-Artefakte](container-registry-image-formats.md#oci-artifacts). Azure Container Registry bietet allgemeine Unterstützung für [OCI-Artefakte](container-registry-oci-artifacts.md), einschließlich Helm-Charts.
* Authentifizieren bei Ihrer Registrierung mithilfe des Befehls `helm registry login`
* Pushen, Pullen und Verwalten von Helm-Charts in einer Registrierung mithilfe von Befehlen vom Typ `helm chart` in der Helm-Befehlszeilenschnittstelle
* Verwenden Sie `helm install`, um Charts aus einem lokalen Repositorycache in einem Kubernetes-Cluster zu installieren.
> [!NOTE]
> Ab Helm 3 gelten [az acr helm][az-acr-helm]-Befehle für die Verwendung mit dem Helm 2-Client als veraltet. Sie werden mindestens drei Monate im Voraus über das Entfernen eines Befehls informiert. Wenn Sie zuvor Helm 2-Charts bereitgestellt haben, lesen Sie [Migrating Helm v2 to v3](https://helm.sh/docs/topics/v2_v3_migration/) (Migrieren von Helm v2 zu v3).

## <a name="prerequisites"></a>Voraussetzungen

Die folgenden Ressourcen sind für das Szenario in diesem Artikel erforderlich:

- Eine **Azure Container Registry**-Instanz in Ihrem Azure-Abonnement. Erstellen Sie bei Bedarf eine Registrierung im [Azure-Portal](container-registry-get-started-portal.md) oder über die [Azure-Befehlszeilenschnittstelle](container-registry-get-started-azure-cli.md).
- **Version 3.1.0 oder höher des Helm-Clients:** Führen Sie `helm version` aus, um Ihre aktuelle Version zu ermitteln. Weitere Informationen zum Installieren und Aktualisieren von Helm finden Sie unter [Installieren von Helm][helm-install].
- **Ein Kubernetes-Cluster**, in dem Sie später ein Helm-Chart installieren. Erstellen Sie bei Bedarf einen [Azure Kubernetes Service-Cluster][aks-quickstart]. 
- **Version 2.0.71 oder höher der Azure-Befehlszeilenschnittstelle:** Führen Sie `az --version` aus, um die Version zu ermitteln. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sie bei Bedarf unter [Installieren der Azure CLI][azure-cli-install].

## <a name="enable-oci-support"></a>Aktivieren der OCI-Unterstützung

Verwenden Sie den Befehl `helm version`, um zu überprüfen, ob Sie Helm 3 installiert haben:

```console
helm version
```

Legen Sie die folgende Umgebungsvariable fest, um die OCI-Unterstützung im Helm 3-Client zu aktivieren. Diese Unterstützung ist derzeit experimentell. 

```console
export HELM_EXPERIMENTAL_OCI=1
```

## <a name="create-a-sample-chart"></a>Erstellen eines Beispielcharts

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

Erstellen Sie im Ordner `templates` eine Datei namens `configmap.yaml`, indem Sie den folgenden Befehl ausführen:

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

## <a name="save-chart-to-local-registry-cache"></a>Speichern des Charts im lokalen Registrierungscache

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

## <a name="authenticate-with-the-registry"></a>Authentifizieren bei der Registrierung

Führen Sie in der Helm 3-Befehlszeilenschnittstelle den Befehl `helm registry login` aus, um sich [bei der Registrierung zu authentifizieren](container-registry-authentication.md), und verwenden Sie geeignete Anmeldeinformationen für Ihr Szenario.

Erstellen Sie beispielsweise einen [Azure Active Directory-Dienstprinzipal mit Pull- und Pushberechtigungen](container-registry-auth-service-principal.md#create-a-service-principal) (AcrPush-Rolle) für die Registrierung. Geben Sie anschließend die Dienstprinzipal-Anmeldeinformationen für `helm registry login` an. Im folgenden Beispiel wird das Kennwort mithilfe einer Umgebungsvariablen angegeben:

```console
echo $spPassword | helm registry login mycontainerregistry.azurecr.io \
  --username <service-principal-id> \
  --password-stdin
```

## <a name="push-chart-to-registry"></a>Pushen eines Charts in die Registrierung

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

## <a name="list-charts-in-the-repository"></a>Auflisten von Diagrammen im Repository

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

## <a name="pull-chart-to-local-cache"></a>Pullen eines Charts in den lokalen Cache

Um ein Helm-Chart in Kubernetes zu installieren, muss sich das Chart im lokalen Cache befinden. Führen Sie in diesem Beispiel zuerst `helm chart remove` aus, um das vorhandene lokale Chart mit dem Namen `mycontainerregistry.azurecr.io/helm/hello-world:v1` zu entfernen:

```console
helm chart remove mycontainerregistry.azurecr.io/helm/hello-world:v1
```

Führen Sie `helm chart pull` aus, um das Chart aus Azure Container Registry in Ihren lokalen Cache herunterzuladen:

```console
helm chart pull mycontainerregistry.azurecr.io/helm/hello-world:v1
```

## <a name="export-helm-chart"></a>Exportieren von Helm-Charts

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

## <a name="install-helm-chart"></a>Installieren von Helm-Charts

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

Führen Sie den Befehl `helm get manifest` aus, um die Installation zu überprüfen. 

```console
helm get manifest myhelmtest
```

Der Befehl gibt die YAML-Daten in der Vorlagendatei `configmap.yaml` zurück.

Führen Sie `helm uninstall` aus, um das Chart-Release in Ihrem Cluster zu deinstallieren:

```console
helm uninstall myhelmtest
```

## <a name="delete-chart-from-the-registry"></a>Löschen eines Charts aus der Registrierung

Wenn Sie ein Chart aus der Containerregistrierung löschen möchten, verwenden Sie den Befehl [az acr repository delete][az-acr-repository-delete]. Führen Sie den folgenden Befehl aus, und bestätigen Sie den Vorgang, wenn Sie dazu aufgefordert werden:

```azurecli
az acr repository delete --name mycontainerregistry --image helm/hello-world:v1
```

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zum Erstellen und Bereitstellen von Helm-Charts finden Sie unter [Charts][develop-helm-charts].
* Weitere Informationen zum Installieren von Anwendungen mit Helm in Azure Kubernetes Service (AKS) finden Sie [hier](../aks/kubernetes-helm.md).
* Helm-Diagramme können als Teil des Containererstellungsprozesses verwendet werden. Weitere Informationen finden Sie unter [Verwenden von Azure Container Registry Tasks][acr-tasks].

<!-- LINKS - external -->
[helm]: https://helm.sh/
[helm-install]: https://helm.sh/docs/intro/install/
[develop-helm-charts]: https://helm.sh/docs/chart_template_guide/
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - internal -->
[azure-cli-install]: /cli/azure/install-azure-cli
[aks-quickstart]: ../aks/kubernetes-walkthrough.md
[acr-bestpractices]: container-registry-best-practices.md
[az-configure]: /cli/azure/reference-index#az_configure
[az-acr-login]: /cli/azure/acr#az_acr_login
[az-acr-helm]: /cli/azure/acr/helm
[az-acr-repository]: /cli/azure/acr/repository
[az-acr-repository-show]: /cli/azure/acr/repository#az_acr_repository_show
[az-acr-repository-delete]: /cli/azure/acr/repository#az_acr_repository_delete
[az-acr-repository-show-tags]: /cli/azure/acr/repository#az_acr_repository_show_tags
[az-acr-repository-show-manifests]: /cli/azure/acr/repository#az_acr_repository_show_manifests
[acr-tasks]: container-registry-tasks-overview.md
