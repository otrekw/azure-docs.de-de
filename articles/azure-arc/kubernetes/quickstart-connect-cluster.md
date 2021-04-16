---
title: 'Schnellstart: Herstellen einer Verbindung eines vorhandenen Kubernetes-Clusters mit Azure Arc'
description: In diesem Schnellstart erfahren Sie, wie Sie eine Verbindung mit einem Kubernetes-Cluster mit Azure Arc-Aktivierung herstellen.
author: mgoedtel
ms.author: magoedte
ms.service: azure-arc
ms.topic: quickstart
ms.date: 03/03/2021
ms.custom: template-quickstart, references_regions
keywords: Kubernetes, Arc, Azure, Cluster
ms.openlocfilehash: b4cbd45f8478674c7c6bacc50f068bc0ec691a14
ms.sourcegitcommit: 56b0c7923d67f96da21653b4bb37d943c36a81d6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2021
ms.locfileid: "106449918"
---
# <a name="quickstart-connect-an-existing-kubernetes-cluster-to-azure-arc"></a>Schnellstart: Herstellen einer Verbindung eines vorhandenen Kubernetes-Clusters mit Azure Arc 

In diesem Schnellstart werden wir die Vorteile von Kubernetes mit Azure Arc-Aktivierung nutzen und eine Verbindung von einem vorhandenen Kubernetes-Cluster mit Azure Arc herstellen. Eine konzeptionelle Übersicht über das Herstellen einer Verbindung von Clustern mit Azure Arc finden Sie im Artikel [Architektur eines Azure Arc-fähigen Kubernetes-Agents](./conceptual-agent-architecture.md).

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../includes/azure-cli-prepare-your-environment.md)]

* Ein funktionierender Kubernetes-Cluster. Wenn Sie über keinen verfügen, haben Sie die folgenden drei Möglichkeiten, um einen Cluster zu erstellen:
    * [Kubernetes in Docker (KIND)](https://kind.sigs.k8s.io/)
    * Erstellen eines Kubernetes-Clusters mithilfe von Docker für [Mac](https://docs.docker.com/docker-for-mac/#kubernetes) oder [Windows](https://docs.docker.com/docker-for-windows/#kubernetes)
    * Selbst verwalteter Kubernetes-Cluster mithilfe der [Cluster-API](https://cluster-api.sigs.k8s.io/user/quick-start.html)

    >[!NOTE]
    > Der Cluster muss mindestens einen Knoten des Betriebssystems und des Architektur Typs `linux/amd64` aufweisen. Cluster mit nur `linux/arm64`-Knoten werden noch nicht unterstützt.
    
* Eine `kubeconfig`-Datei und ein Kontext, der auf Ihren Cluster verweist.
* Lese- und Schreibberechtigungen für den Azure Arc-aktivierten Kubernetes-Ressourcentyp (`Microsoft.Kubernetes/connectedClusters`).

* Installieren Sie das [neueste Release von Helm 3](https://helm.sh/docs/intro/install).

- [Installieren oder aktualisieren Sie Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) auf eine Version >= 2.16.0
* Installieren Sie die Azure CLI-Erweiterung `connectedk8s` in einer Version >= 1.0.0:
  
  ```azurecli
  az extension add --name connectedk8s
  ```

>[!TIP]
> Wenn die Erweiterung `connectedk8s` bereits installiert ist, können Sie sie mit dem folgenden Befehl auf die neueste Version aktualisieren: `az extension update --name connectedk8s`


>[!NOTE]
>Die Liste der Regionen, die von Azure Arc-aktiviertem Kubernetes unterstützt werden, finden Sie [hier](https://azure.microsoft.com/global-infrastructure/services/?products=azure-arc).

>[!NOTE]
> Wenn Sie benutzerdefinierte Speicherorte im Cluster verwenden möchten, verwenden Sie die Regionen „USA, Osten“ oder „Europa, Westen“, um Ihren Cluster zu verbinden, da benutzerdefinierte Speicherorte derzeit nur in diesen Regionen verfügbar sind. Alle anderen Azure Arc-aktivierten Kubernetes-Funktionen sind in allen oben aufgeführten Regionen verfügbar.

## <a name="meet-network-requirements"></a>Erfüllen von Netzwerkanforderungen

>[!IMPORTANT]
>Azure Arc-Agents müssen über die folgenden Protokolle/Ports/ausgehenden URLs verfügen, um zu funktionieren:
>* TCP an Port 443: `https://:443`
>* TCP an Port 9418: `git://:9418`
  
| Endpunkt (DNS) | BESCHREIBUNG |  
| ----------------- | ------------- |  
| `https://management.azure.com`                                                                                 | Erforderlich, damit der Agent eine Verbindung mit Azure herstellen und den Cluster registrieren kann.                                                        |  
| `https://<region>.dp.kubernetesconfiguration.azure.com` | Endpunkt auf Datenebene, über den der Agent Statusinformationen mithilfe von Push übermitteln und Konfigurationsinformationen abrufen kann                                      |  
| `https://login.microsoftonline.com`                                                                            | Erforderlich zum Abrufen und Aktualisieren von Azure Resource Manager-Token.                                                                                    |  
| `https://mcr.microsoft.com`                                                                            | Erforderlich zum Pullen von Containerimages für Azure Arc-Agents.                                                                  |  
| `https://eus.his.arc.azure.com`, `https://weu.his.arc.azure.com`, `https://wcus.his.arc.azure.com`, `https://scus.his.arc.azure.com`, `https://sea.his.arc.azure.com`, `https://uks.his.arc.azure.com`, `https://wus2.his.arc.azure.com`, `https://ae.his.arc.azure.com`, `https://eus2.his.arc.azure.com`, `https://ne.his.arc.azure.com` |  Erforderlich zum Pullen vom System zugewiesener Zertifikate für verwaltete Dienstidentitäten (MSI).                                                                  |

## <a name="register-the-two-providers-for-azure-arc-enabled-kubernetes"></a>Registrieren der beiden Anbieter für Kubernetes mit Azure Arc-Unterstützung

1. Geben Sie die folgenden Befehle ein:
    ```azurecli
    az provider register --namespace Microsoft.Kubernetes
    az provider register --namespace Microsoft.KubernetesConfiguration
    az provider register --namespace Microsoft.ExtendedLocation
    ```
2. Überwachen Sie den Registrierungsprozess. Die Registrierung kann bis zu 10 Minuten dauern.
    ```azurecli
    az provider show -n Microsoft.Kubernetes -o table
    az provider show -n Microsoft.KubernetesConfiguration -o table
    az provider show -n Microsoft.ExtendedLocation -o table
    ```

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie eine Ressourcengruppe:  

```console
az group create --name AzureArcTest -l EastUS -o table
```

```output
Location    Name
----------  ------------
eastus      AzureArcTest
```

## <a name="connect-an-existing-kubernetes-cluster"></a>Herstellen der Verbindung mit einem vorhandenen Kubernetes-Cluster

1. Verbinden Sie Ihren Kubernetes-Cluster mithilfe des folgenden Befehls mit Azure Arc:
    ```console
    az connectedk8s connect --name AzureArcTest1 --resource-group AzureArcTest
    ```

    ```output
    Helm release deployment succeeded

    {
      "aadProfile": {
        "clientAppId": "",
        "serverAppId": "",
        "tenantId": ""
      },
      "agentPublicKeyCertificate": "xxxxxxxxxxxxxxxxxxx",
      "agentVersion": null,
      "connectivityStatus": "Connecting",
      "distribution": "gke",
      "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/AzureArcTest/providers/Microsoft.Kubernetes/connectedClusters/AzureArcTest1",
      "identity": {
        "principalId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
        "tenantId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
        "type": "SystemAssigned"
      },
      "infrastructure": "gcp",
      "kubernetesVersion": null,
      "lastConnectivityTime": null,
      "location": "eastus",
      "managedIdentityCertificateExpirationTime": null,
      "name": "AzureArcTest1",
      "offering": null,
      "provisioningState": "Succeeded",
      "resourceGroup": "AzureArcTest",
      "tags": {},
      "totalCoreCount": null,
      "totalNodeCount": null,
      "type": "Microsoft.Kubernetes/connectedClusters"
    }
    ```

> [!TIP]
> Mit dem obigen Befehl ohne den angegebenen location-Parameter wird die Kubernetes-Ressource mit Azure Arc-Unterstützung am gleichen Standort wie die Ressourcengruppe erstellt. Wenn Sie die Kubernetes-Ressource mit Azure Arc-Unterstützung an einem anderen Standort erstellen möchten, geben Sie bei Ausführung des Befehls `az connectedk8s connect` entweder `--location <region>` oder `-l <region>` an.

## <a name="verify-cluster-connection"></a>Überprüfen der Clusterverbindung

Zeigen Sie mit dem folgenden Befehl eine Liste Ihrer verbundenen Cluster an:  

```console
az connectedk8s list -g AzureArcTest -o table
```

```output
Name           Location    ResourceGroup
-------------  ----------  ---------------
AzureArcTest1  eastus      AzureArcTest
```

> [!NOTE]
> Nach dem Onboarding des Clusters dauert es ungefähr 5 bis 10 Minuten, bis die Clustermetadaten (Clusterversion, Agent-Version, Anzahl der Knoten usw.) auf der Übersichtsseite der Azure Arc-fähigen Kubernetes-Ressource im Azure-Portal angezeigt wird.

## <a name="connect-using-an-outbound-proxy-server"></a>Herstellen einer Verbindung mit einem ausgehenden Proxyserver

Wenn sich Ihr Cluster hinter einem ausgehenden Proxyserver befindet, müssen die Azure-Befehlszeilenschnittstelle und die Kubernetes-Agents mit Azure Arc-Aktivierung ihre Anforderungen über den ausgehenden Proxyserver weiterleiten. 


1. Legen Sie die für Azure CLI erforderlichen Umgebungsvariablen fest, um den ausgehenden Proxyserver zu verwenden:

    * Wenn Sie die Bash verwenden, führen Sie den folgenden Befehl mit entsprechenden Werten aus:

        ```bash
        export HTTP_PROXY=<proxy-server-ip-address>:<port>
        export HTTPS_PROXY=<proxy-server-ip-address>:<port>
        export NO_PROXY=<cluster-apiserver-ip-address>:<port>
        ```

    * Wenn Sie PowerShell verwenden, führen Sie den folgenden Befehl mit entsprechenden Werten aus:

        ```powershell
        $Env:HTTP_PROXY = "<proxy-server-ip-address>:<port>"
        $Env:HTTPS_PROXY = "<proxy-server-ip-address>:<port>"
        $Env:NO_PROXY = "<cluster-apiserver-ip-address>:<port>"
        ```

2. Führen Sie den connect-Befehl mit den angegebenen Proxyparametern aus:

    ```console
    az connectedk8s connect -n <cluster-name> -g <resource-group> --proxy-https https://<proxy-server-ip-address>:<port> --proxy-http http://<proxy-server-ip-address>:<port> --proxy-skip-range <excludedIP>,<excludedCIDR> --proxy-cert <path-to-cert-file>
    ```

> [!NOTE]
> * Geben Sie `excludedCIDR` unter `--proxy-skip-range` an, um sicherzustellen, dass die clusterinterne Kommunikation für die Agents nicht unterbrochen wird.
> * `--proxy-http`, `--proxy-https` und `--proxy-skip-range` werden für die meisten ausgehenden Proxyumgebungen erwartet. `--proxy-cert` ist *nur* erforderlich, wenn Sie vertrauenswürdige Zertifikate, die vom Proxy erwartet werden, in den vertrauenswürdigen Zertifikatspeicher der Agent-Pods einfügen müssen.

## <a name="view-azure-arc-agents-for-kubernetes"></a>Anzeigen von Azure Arc-Agents für Kubernetes

Kubernetes mit Azure Arc-Aktivierung stellt einige Operatoren im Namespace `azure-arc` bereit. 

1. Zeigen Sie diese Bereitstellungen und Pods wie folgt an:

    ```console
    kubectl -n azure-arc get deployments,pods
    ```

1. Vergewissern Sie sich, dass alle Pods den Zustand `Running` aufweisen.

    ```output
    NAME                                        READY      UP-TO-DATE  AVAILABLE  AGE
    deployment.apps/cluster-metadata-operator     1/1             1        1      16h
    deployment.apps/clusteridentityoperator       1/1             1        1      16h
    deployment.apps/config-agent                  1/1             1        1      16h
    deployment.apps/controller-manager            1/1             1        1      16h
    deployment.apps/flux-logs-agent               1/1             1        1      16h
    deployment.apps/metrics-agent                 1/1             1        1      16h
    deployment.apps/resource-sync-agent           1/1             1        1      16h

    NAME                                           READY    STATUS   RESTART AGE
    pod/cluster-metadata-operator-7fb54d9986-g785b  2/2     Running  0       16h
    pod/clusteridentityoperator-6d6678ffd4-tx8hr    3/3     Running  0       16h
    pod/config-agent-544c4669f9-4th92               3/3     Running  0       16h
    pod/controller-manager-fddf5c766-ftd96          3/3     Running  0       16h
    pod/flux-logs-agent-7c489f57f4-mwqqv            2/2     Running  0       16h
    pod/metrics-agent-58b765c8db-n5l7k              2/2     Running  0       16h
    pod/resource-sync-agent-5cf85976c7-522p5        3/3     Running  0       16h
    ```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Mit dem folgenden Befehl können Sie die Kubernetes-Ressource mit Azure Arc-Aktivierung, alle zugeordneten Konfigurationsressourcen *und* alle Agents, die auf dem Cluster ausgeführt werden, mit der Azure CLI löschen:

```azurecli
az connectedk8s delete --name AzureArcTest1 --resource-group AzureArcTest
```

>[!NOTE]
>Das Löschen der Kubernetes-Ressource mit Azure Arc-Aktivierung mithilfe des Azure-Portals entfernt alle zugehörigen Konfigurationsressourcen, entfernt jedoch *keine* Agents, die auf dem Cluster ausgeführt werden. Die bewährte Methode besteht darin, die Kubernetes-Ressource mit Azure Arc-Aktivierung mithilfe von `az connectedk8s delete` anstatt über das Azure-Portal zu löschen.

## <a name="next-steps"></a>Nächste Schritte

Fahren Sie mit dem nächsten Artikel fort, um zu erfahren, wie Sie Konfigurationen mithilfe von GitOps für Ihren verbundenen Kubernetes-Cluster bereitstellen.
> [!div class="nextstepaction"]
> [Bereitstellen von Konfigurationen mithilfe von GitOps](tutorial-use-gitops-connected-cluster.md)
