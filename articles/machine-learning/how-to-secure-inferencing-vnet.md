---
title: Schützen von Rückschlussumgebungen mit virtuellen Netzwerken
titleSuffix: Azure Machine Learning
description: Verwenden Sie eine isolierte Azure Virtual Network-Instanz, um Ihre Azure Machine Learning-Rückschlussumgebung zu schützen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
ms.reviewer: larryfr
ms.author: aashishb
author: aashishb
ms.date: 07/16/2020
ms.custom: contperfq4, tracking-python
ms.openlocfilehash: 359c2a27099ca298076edc255b8c30e226af0a18
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90882954"
---
# <a name="secure-an-azure-machine-learning-inferencing-environment-with-virtual-networks"></a>Schützen einer Azure Machine Learning-Rückschlussumgebung mit virtuellen Netzwerken

In diesem Artikel erfahren Sie, wie Sie Rückschlussumgebungen mit einem virtuellen Netzwerk in Azure Machine Learning schützen.

Dieser Artikel ist der vierte Teil einer fünfteiligen Artikelreihe, in der Sie durch die Absicherung eines Azure Machine Learning-Workflows geführt werden. Es wird dringend empfohlen, zunächst [Teil 1: Virtuelle Netzwerke im Überblick](how-to-network-security-overview.md) zu lesen, um die Gesamtarchitektur besser zu verstehen. 

Sehen Sie sich auch die anderen Artikel in dieser Reihe an:

[1. Virtuelle Netzwerke im Überblick](how-to-network-security-overview.md) > [Schützen von Arbeitsbereichsressourcen](how-to-secure-workspace-vnet.md) > [3. Schützen der Trainingsumgebung](how-to-secure-training-vnet.md) > **4. Schützen der Rückschlussumgebung** > [5. Verwenden von Studio in einem virtuellen Netzwerk](how-to-enable-studio-virtual-network.md)

In diesem Artikel erfahren Sie, wie Sie die folgenden Rückschlussressourcen in einem virtuellen Netzwerk schützen:
> [!div class="checklist"]
> - Standard-AKS-Cluster (Azure Kubernetes Service)
> - Privater AKS-Cluster
> - Azure Container Instances (ACI)


## <a name="prerequisites"></a>Voraussetzungen

+ Im Artikel [Übersicht über die Netzwerksicherheit](how-to-network-security-overview.md) finden Sie Informationen zu gängigen Szenarien im Zusammenhang mit virtuellen Netzwerken sowie zur Gesamtarchitektur virtueller Netzwerke.

+ Ein vorhandenes virtuelles Netzwerk und Subnetz, die mit Ihren Computeressourcen verwendet werden können

+ Ihr Benutzerkonto muss über die rollenbasierte Zugriffssteuerung von Azure (Role-Based Access Control, RBAC) zu den folgenden Aktionen berechtigt werden, um Ressourcen in einem virtuellen Netzwerk oder Subnetz bereitstellen zu können:

    - „Microsoft.Network/virtualNetworks/join/action“ auf der virtuellen Netzwerkressource
    - „Microsoft.Network/virtualNetworks/subnet/join/action“ auf der Subnetzressource

    Weitere Informationen zur RBAC in Netzwerken finden Sie unter [Integrierte Netzwerkrollen](/azure/role-based-access-control/built-in-roles#networking).

<a id="aksvnet"></a>

## <a name="azure-kubernetes-service"></a>Azure Kubernetes Service

Wenn Sie einen AKS-Cluster in einem virtuellen Netzwerk verwenden möchten, müssen die folgenden Anforderungen an das Netzwerk erfüllt sein:

> [!div class="checklist"]
> * Orientieren Sie sich an den Voraussetzungen unter [Konfigurieren von erweiterten Netzwerken in Azure Kubernetes Service (AKS)](../aks/configure-azure-cni.md#prerequisites).
> * Die AKS-Instanz und das virtuelle Netzwerk müssen sich in derselben Region befinden. Wenn Sie die Azure Storage-Konten, die vom Arbeitsbereich verwendet werden, in einem virtuellen Netzwerk sichern, müssen diese sich im selben virtuellen Netzwerk befinden wie die AKS-Instanz.


Gehen Sie folgendermaßen vor, um AKS in einem virtuellen Netzwerk zu Ihrem Arbeitsbereich hinzuzufügen:

1. Melden Sie sich bei [Azure Machine Learning Studio](https://ml.azure.com/) an, und wählen Sie dann Ihr Abonnement und den Arbeitsbereich aus.

1. Wählen Sie links __Compute__ aus.

1. Wählen Sie in der Mitte __Rückschlusscluster__ aus, und wählen Sie dann __+__ aus.

1. Wählen Sie im Dialogfeld __Neuer Rückschlusscluster__ unter __Netzwerkkonfiguration__ die Option __Erweitert__ aus.

1. Gehen Sie zum Konfigurieren dieser Computeressource für die Verwendung eines virtuellen Netzwerks wie folgt vor:

    1. Wählen Sie in der Dropdownliste __Ressourcengruppe__ die Ressourcengruppe aus, die das virtuelle Netzwerk enthält.
    1. Wählen Sie in der Dropdownliste __Virtuelles Netzwerk__ das virtuelle Netzwerk aus, das das Subnetzwerk enthält.
    1. Wählen Sie in der Dropdownliste __Subnetz__ das Subnetz aus.
    1. Geben Sie in das Feld __Kubernetes Service Address Range__ (Kubernetes-Dienstadressbereich) den Kubernetes-Dienstadressbereich ein. Dieser Adressbereich verwendet einen IP-Adressbereich in CIDR-Notation (Classless Inter-Domain Routing), um die für den Cluster verfügbaren IP-Adressen zu definieren. Er darf sich mit keinem IP-Adressbereich eines Subnetzes überschneiden (z. B. 10.0.0.0/16).
    1. Geben Sie in das Feld __Kubernetes DNS Service IP Address__ (Kubernetes-DNS-Dienst-IP-Adresse) die Kubernetes-DNS-Dienst-IP-Adresse ein. Diese IP-Adresse wird dem Kubernetes-DNS-Dienst zugewiesen. Sie muss innerhalb des Kubernetes-Dienstadressbereichs liegen (z. B. 10.0.0.10).
    1. Geben Sie in das Feld __Adresse der Docker-Brücke__ die Adresse der Docker-Brücke ein. Diese IP-Adresse wird der Docker-Brücke zugewiesen. Sie darf weder dem IP-Adressbereich eines Subnetzes noch dem Adressbereich des Kubernetes-Diensts angehören (z. B. 172.17.0.1/16).

   ![Azure Machine Learning: VNET-Einstellungen für Machine Learning Compute](./media/how-to-enable-virtual-network/aks-virtual-network-screen.png)

1. Stellen Sie sicher, dass für die Netzwerksicherheitsgruppe, die das virtuelle Netzwerk steuert, eine Sicherheitsregel für eingehenden Datenverkehr für den Bewertungsendpunkt aktiviert ist, damit sie von außerhalb des virtuellen Netzwerks aus aufgerufen werden kann.
   > [!IMPORTANT]
   > Behalten Sie die Standardausgangsregeln für die NSG bei. Weitere Informationen finden Sie unter [Sicherheitsgruppen](https://docs.microsoft.com/azure/virtual-network/security-overview#default-security-rules) bei den Standardsicherheitsregeln.

   [![Eine Eingangssicherheitsregel](./media/how-to-enable-virtual-network/aks-vnet-inbound-nsg-scoring.png)](./media/how-to-enable-virtual-network/aks-vnet-inbound-nsg-scoring.png#lightbox)

Sie können auch das Azure Machine Learning SDK verwenden, um Azure Kubernetes Service einem virtuellen Netzwerk hinzuzufügen. Wenn Sie bereits über einen AKS-Cluster in einem virtuellen Netzwerk verfügen, können Sie ihn dem Arbeitsbereich anfügen (siehe [Bereitstellen von Modellen mit dem Azure Machine Learning-Dienst](how-to-deploy-and-where.md)). Der folgende Code erstellt eine neue AKS-Instanz im Subnetz `default` eines virtuellen Netzwerks namens `mynetwork`:

```python
from azureml.core.compute import ComputeTarget, AksCompute

# Create the compute configuration and set virtual network information
config = AksCompute.provisioning_configuration(location="eastus2")
config.vnet_resourcegroup_name = "mygroup"
config.vnet_name = "mynetwork"
config.subnet_name = "default"
config.service_cidr = "10.0.0.0/16"
config.dns_service_ip = "10.0.0.10"
config.docker_bridge_cidr = "172.17.0.1/16"

# Create the compute target
aks_target = ComputeTarget.create(workspace=ws,
                                  name="myaks",
                                  provisioning_configuration=config)
```

Nach Abschluss des Erstellungsprozesses können Sie Rückschlüsse für einen AKS-Cluster hinter einem virtuellen Netzwerk ziehen oder das Modell bewerten. Weitere Informationen finden Sie unter [Bereitstellen im AKS](how-to-deploy-and-where.md).

## <a name="private-aks-cluster"></a>Privater AKS-Cluster

AKS-Standardcluster weisen standardmäßig eine Steuerungsebene (API-Server) mit öffentlichen IP-Adressen auf. Sie können AKS so konfigurieren, dass eine private Steuerungsebene verwendet wird, indem Sie einen privaten AKS-Cluster erstellen. Weitere Informationen finden Sie unter [Erstellen eines privaten Azure Kubernetes Service-Clusters](../aks/private-clusters.md).

Nachdem Sie den privaten AKS-Cluster erstellt haben, [fügen Sie den Cluster an das virtuelle Netzwerk an](how-to-create-attach-kubernetes.md), um ihn mit Azure Machine Learning zu verwenden.

## <a name="internal-aks-load-balancer"></a>Interner AKS-Lastenausgleich

Standardmäßig verwenden AKS-Bereitstellungen einen [öffentlichen Lastenausgleich](../aks/load-balancer-standard.md). In diesem Abschnitt erfahren Sie, wie Sie AKS für die Verwendung eines internen Lastenausgleichs konfigurieren. Ein internes (oder privates) Lastenausgleichsmodul wird verwendet, wenn nur private IP-Adressen als Front-End zulässig sind. Interne Lastenausgleichsmodule werden verwendet, um einen Lastausgleich für Datenverkehr innerhalb eines virtuellen Netzwerks vorzunehmen.

Ein privater Lastenausgleich wird aktiviert, indem AKS zur Verwendung eines _internen Lastenausgleichs_ konfiguriert wird. 

### <a name="network-contributor-role"></a>Rolle „Netzwerkmitwirkender“

> [!IMPORTANT]
> Wenn Sie einen AKS-Cluster erstellen oder anfügen, indem Sie ein zuvor erstelltes virtuelles Netzwerk bereitstellen, müssen Sie dem Dienst Prinzipal (Service Principal, SP) oder der verwalteten Identität für den AKS-Cluster die Rolle _Netzwerkmitwirkender_ der Ressourcengruppe zuweisen, die das virtuelle Netzwerk enthält. Dies muss geschehen, bevor Sie versuchen, den internen Load Balancer auf eine private IP-Adresse umzustellen.
>
> Um die Identität als Netzwerkmitwirkenden hinzuzufügen, führen Sie die folgenden Schritte aus:

1. Verwenden Sie zum Ermitteln der ID des Dienstprinzipals oder der verwalteten Identität für AKS die folgenden Azure CLI Befehle. Ersetzen Sie `<aks-cluster-name>` durch den Namen des Clusters. Ersetzen Sie `<resource-group-name>` durch den Namen der Ressourcengruppe, die den _AKS-Cluster enthält_:

    ```azurecli-interactive
    az aks show -n <aks-cluster-name> --resource-group <resource-group-name> --query servicePrincipalProfile.clientId
    ``` 

    Wenn dieser Befehl den Wert `msi` zurückgibt, verwenden Sie den folgenden Befehl, um die Prinzipal-ID für die verwaltete Identität zu bestimmen:

    ```azurecli-interactive
    az aks show -n <aks-cluster-name> --resource-group <resource-group-name> --query identity.principalId
    ```

1. Verwenden Sie den folgenden Befehl, um die ID der Ressourcengruppe zu ermitteln, die das virtuelle Netzwerk enthält. Ersetzen Sie `<resource-group-name>` durch den Namen der Ressourcengruppe, die das _virtuelle Azure-Netzwerk enthält_:

    ```azurecli-interactive
    az group show -n <resource-group-name> --query id
    ```

1. Um den Dienstprinzipal oder die verwaltete Identität als Netzwerkmitwirkenden hinzuzufügen, verwenden Sie den folgenden Befehl. Ersetzen Sie `<SP-or-managed-identity>` durch die für den Dienstprinzipal oder die verwaltete Identität zurückgegebene ID. Ersetzen Sie `<resource-group-id>` durch die ID, die für die Ressourcengruppe mit dem virtuellen Netzwerk zurückgegeben wurde:

    ```azurecli-interactive
    az role assignment create --assignee <SP-or-managed-identity> --role 'Network Contributor' --scope <resource-group-id>
    ```
Weitere Informationen zur Verwendung des internen Lastenausgleichs mit AKS finden Sie unter [Verwenden des internen Lastenausgleichs mit Azure Kubernetes Service](/azure/aks/internal-lb).

### <a name="enable-private-load-balancer"></a>Aktivieren eines privaten Lastenausgleichs

> [!IMPORTANT]
> Sie können beim Erstellen des Azure Kubernetes Service-Clusters keine private IP-Adresse aktivieren. Er muss als Update eines bestehenden Clusters aktiviert werden.

Der folgende Codeausschnitt veranschaulicht, wie Sie __einen neuen AKS-Cluster erstellen__ und ihn dann aktualisieren, um eine private IP-Adresse bzw. einen internen Lastenausgleich zu verwenden:

```python
import azureml.core
from azureml.core.compute.aks import AksUpdateConfiguration
from azureml.core.compute import AksCompute, ComputeTarget

# Verify that cluster does not exist already
try:
    aks_target = AksCompute(workspace=ws, name=aks_cluster_name)
    print("Found existing aks cluster")

except:
    print("Creating new aks cluster")

    # Subnet to use for AKS
    subnet_name = "default"
    # Create AKS configuration
    prov_config = AksCompute.provisioning_configuration(location = "eastus2")
    # Set info for existing virtual network to create the cluster in
    prov_config.vnet_resourcegroup_name = "myvnetresourcegroup"
    prov_config.vnet_name = "myvnetname"
    prov_config.service_cidr = "10.0.0.0/16"
    prov_config.dns_service_ip = "10.0.0.10"
    prov_config.subnet_name = subnet_name
    prov_config.docker_bridge_cidr = "172.17.0.1/16"

    # Create compute target
    aks_target = ComputeTarget.create(workspace = ws, name = "myaks", provisioning_configuration = prov_config)
    # Wait for the operation to complete
    aks_target.wait_for_completion(show_output = True)
    
    # Update AKS configuration to use an internal load balancer
    update_config = AksUpdateConfiguration(None, "InternalLoadBalancer", subnet_name)
    aks_target.update(update_config)
    # Wait for the operation to complete
    aks_target.wait_for_completion(show_output = True)
```

__Azure-Befehlszeilenschnittstelle__

```azurecli-interactive
az rest --method put --uri https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.MachineLearningServices/workspaces/<workspace>/computes/<compute>?api-version=2018-11-19 --body @body.json
```

Der Inhalt der Datei `body.json`, auf die der Befehl verweist, ist dem folgenden JSON-Dokument ähnlich:

```json
{ 
    "location": "<region>", 
    "properties": { 
        "resourceId": "/subscriptions/<subscription-id>/resourcegroups/<resource-group>/providers/Microsoft.ContainerService/managedClusters/<aks-resource-name>", 
        "computeType": "AKS", 
        "provisioningState": "Succeeded", 
        "properties": { 
            "loadBalancerType": "InternalLoadBalancer", 
            "agentCount": <agent-count>, 
            "agentVmSize": "vm-size", 
            "clusterFqdn": "<cluster-fqdn>" 
        } 
    } 
} 
```

Wenn Sie einen __vorhandenen Cluster__ an den Arbeitsbereich anfügen, müssen Sie nach dem Anfügevorgang mit der Load Balancer-Konfiguration warten.

Weitere Informationen zum Anfügen eines Clusters finden Sie unter [Anfügen eines vorhandenen AKS-Clusters](how-to-create-attach-kubernetes.md).

Nachdem Sie den vorhandenen Cluster angefügt haben, können Sie den Cluster so aktualisieren, dass er eine private IP verwendet.

```python
import azureml.core
from azureml.core.compute.aks import AksUpdateConfiguration
from azureml.core.compute import AksCompute

# ws = workspace object. Creation not shown in this snippet
aks_target = AksCompute(ws,"myaks")

# Change to the name of the subnet that contains AKS
subnet_name = "default"
# Update AKS configuration to use an internal load balancer
update_config = AksUpdateConfiguration(None, "InternalLoadBalancer", subnet_name)
aks_target.update(update_config)
# Wait for the operation to complete
aks_target.wait_for_completion(show_output = True)
```

## <a name="enable-azure-container-instances-aci"></a>Aktivieren von Azure Container Instances (ACI)

Azure Container Instances werden beim Bereitstellen eines Modells dynamisch erstellt. Damit Azure Machine Learning ACIs innerhalb des virtuellen Netzwerks erstellen kann, müssen Sie __Subnetzdelegierung__ für das von der Bereitstellung verwendete Subnetz aktivieren.

> [!WARNING]
> Wenn Sie Azure Container Instances in einem virtuellen Netzwerk verwenden, muss sich das virtuelle Netzwerk in derselben Ressourcengruppe wie der Azure Machine Learning-Arbeitsbereich befinden.
>
> Wenn Sie Azure Container Instances innerhalb des virtuellen Netzwerks verwenden, darf sich die Azure Container Registry (ACR) für Ihren Arbeitsbereich nicht ebenfalls im virtuellen Netzwerk befinden.

Gehen Sie folgendermaßen vor, um ACIs in einem virtuellen Netzwerk zu Ihrem Arbeitsbereich hinzuzufügen:

1. Anweisungen zum Aktivieren der Subnetzdelegierung in einem virtuellen Netzwerk finden Sie im Artikel [Hinzufügen oder Entfernen einer Subnetzdelegierung](../virtual-network/manage-subnet-delegation.md). Sie können die Delegierung beim Erstellen eines virtuellen Netzwerks aktivieren oder einem bestehenden Netzwerk hinzufügen.

    > [!IMPORTANT]
    > Wenn Sie Delegierung aktivieren, verwenden Sie `Microsoft.ContainerInstance/containerGroups` als Wert für __Subnetz an einen Dienst delegieren__.

2. Stellen Sie das Modell mithilfe von [AciWebservice.deploy_configuration()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aci.aciwebservice?view=azure-ml-py#deploy-configuration-cpu-cores-none--memory-gb-none--tags-none--properties-none--description-none--location-none--auth-enabled-none--ssl-enabled-none--enable-app-insights-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--ssl-cname-none--dns-name-label-none--primary-key-none--secondary-key-none--collect-model-data-none--cmk-vault-base-url-none--cmk-key-name-none--cmk-key-version-none--vnet-name-none--subnet-name-none-&preserve-view=true) unter Angabe der Parameter `vnet_name` und `subnet_name` bereit. Legen Sie diese Parameter auf den Namen und das Subnetz des virtuellen Netzwerks fest, in dem Sie Delegierung aktiviert haben.


## <a name="next-steps"></a>Nächste Schritte

Dieser Artikel ist der dritte Teil einer vierteiligen Serie zu virtuellen Netzwerken. Weitere Informationen zum Schützen eines virtuellen Netzwerks finden Sie in den verbleibenden Artikeln:

* [Teil 1: Virtuelle Netzwerke im Überblick](how-to-network-security-overview.md)
* [Teil 2: Schützen von Arbeitsbereichsressourcen](how-to-secure-workspace-vnet.md)
* [Teil 3: Schützen der Trainingsumgebung](how-to-secure-training-vnet.md)
* [Teil 5: Verwenden von Studio in einem virtuellen Netzwerk](how-to-enable-studio-virtual-network.md)