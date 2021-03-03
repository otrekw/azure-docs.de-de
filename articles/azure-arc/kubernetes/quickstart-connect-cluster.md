---
title: 'Schnellstart: Herstellen einer Verbindung eines vorhandenen Kubernetes-Clusters mit Azure Arc'
description: In diesem Schnellstart erfahren Sie, wie Sie eine Verbindung mit einem Kubernetes-Cluster mit Azure Arc-Aktivierung herstellen.
author: mgoedtel
ms.author: magoedte
ms.service: azure-arc
ms.topic: quickstart
ms.date: 02/24/2021
ms.custom: template-quickstart
keywords: Kubernetes, Arc, Azure, Cluster
ms.openlocfilehash: 8eb177f0c80d7ed2df70c75ca476a1dfe33c8425
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2021
ms.locfileid: "101664270"
---
# <a name="quickstart-connect-an-existing-kubernetes-cluster-to-azure-arc"></a>Schnellstart: Herstellen einer Verbindung eines vorhandenen Kubernetes-Clusters mit Azure Arc 

In diesem Schnellstart werden wir die Vorteile von Kubernetes mit Azure Arc-Aktivierung nutzen und eine Verbindung von einem vorhandenen Kubernetes-Cluster mit Azure Arc herstellen. Eine konzeptionelle Übersicht über das Herstellen einer Verbindung von Clustern mit Azure Arc finden Sie im Artikel [Architektur eines Azure Arc-fähigen Kubernetes-Agents](./conceptual-agent-architecture.md).

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../includes/azure-cli-prepare-your-environment.md)]

* Überprüfen Sie, ob Folgendes vorhanden ist:
    * Ein funktionierender Kubernetes-Cluster.
    * Eine `kubeconfig`-Datei.
    * Lese- und Schreibberechtigungen für den Benutzer oder Dienstprinzipal, der die Verbindung herstellt und den Kubernetes-Ressourcentyp mit Azure Arc-Aktivierung (`Microsoft.Kubernetes/connectedClusters`) erstellt.
* Installieren Sie das [neueste Release von Helm 3](https://helm.sh/docs/intro/install).
* Installieren Sie die folgenden Kubernetes-CLI-Erweiterungen mit Azure Arc-Aktivierung der Versionen >= 1.0.0:
  
  ```azurecli
  az extension add --name connectedk8s
  az extension add --name k8s-configuration
  ```
  * Führen Sie die folgenden Befehle aus, um diese Erweiterungen auf die aktuelle Version zu aktualisieren:
  
  ```azurecli
  az extension update --name connectedk8s
  az extension update --name k8s-configuration
  ```

>[!NOTE]
>**Unterstützte Regionen:**
>* East US
>* Europa, Westen
>* USA, Westen-Mitte
>* USA Süd Mitte
>* Asien, Südosten
>* UK, Süden
>* USA, Westen 2
>* Australien (Osten)
>* USA (Ost) 2
>* Nordeuropa

## <a name="meet-network-requirements"></a>Erfüllen von Netzwerkanforderungen

>[!IMPORTANT]
>Azure Arc-Agents müssen über die folgenden Protokolle/Ports/ausgehenden URLs verfügen, um zu funktionieren:
>* TCP an Port 443: `https://:443`
>* TCP an Port 9418: `git://:9418`
  
| Endpunkt (DNS) | BESCHREIBUNG |  
| ----------------- | ------------- |  
| `https://management.azure.com`                                                                                 | Erforderlich, damit der Agent eine Verbindung mit Azure herstellen und den Cluster registrieren kann.                                                        |  
| `https://eastus.dp.kubernetesconfiguration.azure.com`, `https://westeurope.dp.kubernetesconfiguration.azure.com`, `https://westcentralus.dp.kubernetesconfiguration.azure.com`, `https://southcentralus.dp.kubernetesconfiguration.azure.com`, `https://southeastasia.dp.kubernetesconfiguration.azure.com`, `https://uksouth.dp.kubernetesconfiguration.azure.com`, `https://westus2.dp.kubernetesconfiguration.azure.com`, `https://australiaeast.dp.kubernetesconfiguration.azure.com`, `https://eastus2.dp.kubernetesconfiguration.azure.com`, `https://northeurope.dp.kubernetesconfiguration.azure.com` | Endpunkt auf Datenebene, über den der Agent Statusinformationen mithilfe von Push übermitteln und Konfigurationsinformationen abrufen kann                                      |  
| `https://login.microsoftonline.com`                                                                            | Erforderlich zum Abrufen und Aktualisieren von Azure Resource Manager-Token.                                                                                    |  
| `https://mcr.microsoft.com`                                                                            | Erforderlich zum Pullen von Containerimages für Azure Arc-Agents.                                                                  |  
| `https://eus.his.arc.azure.com`, `https://weu.his.arc.azure.com`, `https://wcus.his.arc.azure.com`, `https://scus.his.arc.azure.com`, `https://sea.his.arc.azure.com`, `https://uks.his.arc.azure.com`, `https://wus2.his.arc.azure.com`, `https://ae.his.arc.azure.com`, `https://eus2.his.arc.azure.com`, `https://ne.his.arc.azure.com` |  Erforderlich zum Pullen vom System zugewiesener Zertifikate für verwaltete Dienstidentitäten (MSI).                                                                  |


## <a name="install-the-azure-arc-enabled-kubernetes-cli-extensions"></a>Installieren der CLI-Erweiterungen für Kubernetes mit Azure Arc-Aktivierung

Geben Sie die folgenden Befehle ein:  

    ```azurecli
    az extension add --name connectedk8s
    az extension add --name k8s-configuration
    ```

## <a name="register-the-two-providers-for-azure-arc-enabled-kubernetes"></a>Registrieren der beiden Anbieter für Kubernetes mit Azure Arc-Unterstützung

1. Geben Sie die folgenden Befehle ein:
    ```azurecli
    az provider register --namespace Microsoft.Kubernetes
    az provider register --namespace Microsoft.KubernetesConfiguration
    ```
2. Überwachen Sie den Registrierungsprozess. Die Registrierung kann bis zu 10 Minuten dauern.
    ```azurecli
    az provider show -n Microsoft.Kubernetes -o table
    az provider show -n Microsoft.KubernetesConfiguration -o table    
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
> [Bereitstellen von Konfigurationen mithilfe von GitOps](use-gitops-connected-cluster.md)
