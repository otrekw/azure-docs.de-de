---
title: Verwalten von Azure SQL Managed Instance für Apache Cassandra-Ressourcen mithilfe der Azure CLI
description: Hier erfahren Sie mehr über die allgemeinen Befehle zum Automatisieren der Verwaltung von Azure SQL Managed Instance für Apache Cassandra mithilfe der Azure CLI.
author: TheovanKraay
ms.service: managed-instance-apache-cassandra
ms.topic: how-to
ms.date: 03/02/2021
ms.author: thvankra
ms.openlocfilehash: 3cd5fdbf6cdc504a1290c8fbd80cf89cf85ce714
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101743443"
---
# <a name="manage-azure-managed-instance-for-apache-cassandra-resources-using-azure-cli-preview"></a>Verwalten von Azure SQL Managed Instance für Apache Cassandra-Ressourcen mithilfe der Azure CLI (Vorschau)

In diesem Artikel werden allgemeine Befehle zum Automatisieren der Verwaltung von Azure SQL Managed Instance für Apache Cassandra-Cluster mithilfe der Azure CLI beschrieben.

> [!IMPORTANT]
> Azure SQL Managed Instance für Apache Cassandra befindet sich derzeit in der öffentlichen Vorschauphase.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

[!INCLUDE [azure-cli-prepare-your-environment.md](../../includes/azure-cli-prepare-your-environment.md)]

* Für diesen Artikel ist die Azure CLI-Version 2.12.1 oder höher erforderlich. Wenn Sie Azure Cloud Shell verwenden, ist die neueste Version bereits installiert.

> [!IMPORTANT]
> Ressourcen für die Verwaltung von Azure SQL Managed Instance für Apache Cassandra können nicht umbenannt werden, da dies gegen die Funktionsweise von Azure Resource Manager mit Ressourcen-URIs verstößt.

## <a name="azure-managed-instance-for-apache-cassandra-clusters"></a>Azure SQL Managed Instance für Apache Cassandra-Cluster

In den folgenden Abschnitten wird die Verwaltung von Azure SQL Managed Instance für Apache Cluster veranschaulicht, einschließlich der folgenden Szenarios:

* [Erstellen eines Managed Instance-Clusters](#create-cluster)
* [Löschen eines Managed Instance-Clusters](#delete-cluster)
* [Abrufen der Clusterdetails](#get-cluster-details)
* [Abrufen des Clusterknotenstatus](#get-cluster-status)
* [Auflisten von Clustern nach Ressourcengruppe](#list-clusters-resource-group)
* [Auflisten von Clustern nach Abonnement-ID](#list-clusters-subscription)

### <a name="create-a-managed-instance-cluster"></a><a id="create-cluster"></a>Erstellen eines Managed Instance-Clusters

So erstellen Sie einen Azure SQL Managed Instance für Apache Cassandra-Cluster:

```azurecli-interactive
resourceGroupName='MyResourceGroup'
clusterName='cassandra-hybrid-cluster'
location='West US'
delegatedManagementSubnetId='/subscriptions/<subscription id>/resourceGroups/customer-vnet-rg/providers/Microsoft.Network/virtualNetworks/customer-vnet/subnets/management'
initialCassandraAdminPassword='myPassword'

# You can override the cluster name if the original name is not legal for an Azure resource.
# overrideClusterName='ClusterNameIllegalForAzureResource'
# the default Cassandra version is v3.11

az managed-cassandra cluster create \
    --cluster-name $clusterName \
    --resource-group $resourceGroupName \
    --location $location \
    --delegated-management-subnet-id $delegatedManagementSubnetId \
    --initial-cassandra-admin-password $initialCassandraAdminPassword \
```

### <a name="delete-a-managed-instance-cluster"></a><a id="delete-cluster"></a>Löschen eines Managed Instance-Clusters

So löschen Sie einen Cluster:

```azurecli-interactive
resourceGroupName='MyResourceGroup'
clusterName='cassandra-hybrid-cluster'

az managed-cassandra cluster delete \
    --cluster-name $clusterName \
    --resource-group $resourceGroupName
```

### <a name="get-the-cluster-details"></a><a id="get-cluster-details"></a>Abrufen der Clusterdetails

So rufen Sie Clusterdetails ab:

```azurecli-interactive
resourceGroupName='MyResourceGroup'
clusterName='cassandra-hybrid-cluster'

az managed-cassandra cluster show \
    --cluster-name $clusterName \
    --resource-group $resourceGroupName
```

### <a name="get-the-cluster-node-status"></a><a id="get-cluster-status"></a>Abrufen des Clusterknotenstatus

So rufen Sie Clusterdetails ab:

```azurecli-interactive
clusterName='cassandra-hybrid-cluster'
resourceGroupName='MyResourceGroup'

az managed-cassandra cluster node-status \
    --cluster-name $clusterName \
    --resource-group $resourceGroupName
```

### <a name="list-the-clusters-by-resource-group"></a><a id="list-clusters-resource-group"></a>Auflisten von Clustern nach Ressourcengruppe

So listen Sie die Cluster nach Ressourcengruppe auf:

```azurecli-interactive
subscriptionId='MySubscriptionId'
resourceGroupName='MyResourceGroup'

az managed-cassandra cluster list\
    --resource-group $resourceGroupName
```

### <a name="list-clusters-by-subscription-id"></a><a id="list-clusters-subscription"></a>Auflisten von Clustern nach Abonnement-ID

So listen Sie die Cluster nach der Abonnement-ID auf:

```azurecli-interactive
# set your subscription id
az account set -s <subscription id>

az managed-cassandra cluster list
```

## <a name="the-managed-instance-datacenters"></a><a id="managed-instance-datacenter"></a>Rechenzentren für Azure SQL Managed Instance

In den folgenden Abschnitten wird die Verwaltung von Azure SQL Managed Instance für Apache Cassandra-Rechenzentren veranschaulicht, einschließlich der folgenden Szenarios:

* [Erstellen eines Rechenzentrums](#create-datacenter)
* [Löschen eines Rechenzentrums](#delete-datacenter)
* [Abrufen von Details zu Rechenzentren](#get-datacenter-details)
* [Aktualisieren oder Skalieren eines Rechenzentrums](#update-datacenter)
* [Abrufen von Rechenzentren in einem Cluster](#get-datacenters-cluster)

### <a name="create-a-datacenter"></a><a id="create-datacenter"></a>Erstellen eines Rechenzentrums

So erstellen Sie ein Rechenzentrum:

```azurecli-interactive
resourceGroupName='MyResourceGroup'
clusterName='cassandra-hybrid-cluster'
dataCenterName='dc1'
dataCenterLocation='eastus2'
delegatedSubnetId='/subscriptions/<Subscription_ID>/resourceGroups/customer-vnet-rg/providers/Microsoft.Network/virtualNetworks/customer-vnet/subnets/dc1-subnet'

az managed-cassandra datacenter create \
    --resource-group $resourceGroupName \
    --cluster-name $clusterName \
    --data-center-name $dataCenterName \
    --data-center-location $dataCenterLocation \
    --delegated-subnet-id $delegatedSubnetId \
    --node-count 3 
```

### <a name="delete-a-datacenter"></a><a id="delete-datacenter"></a>Löschen eines Rechenzentrums

So löschen Sie ein Rechenzentrum:

```azurecli-interactive
resourceGroupName='MyResourceGroup'
clusterName='cassandra-hybrid-cluster'
dataCenterName='dc1'

az managed-cassandra datacenter delete \
    --resource-group $resourceGroupName \
    --cluster-name $clusterName \
    --data-center-name $dataCenterName 
```

### <a name="get-datacenter-details"></a><a id="get-datacenter-details"></a>Abrufen von Details zu Rechenzentren

So rufen Sie Details zu Rechenzentren ab:

```azurecli-interactive
resourceGroupName='MyResourceGroup'
clusterName='cassandra-hybrid-cluster'
dataCenterName='dc1'

az managed-cassandra datacenter show \
    --resource-group $resourceGroupName \
    --cluster-name $clusterName \
    --data-center-name $dataCenterName 
```

### <a name="update-or-scale-a-datacenter"></a><a id="update-datacenter"></a>Aktualisieren oder Skalieren eines Rechenzentrums

So aktualisieren oder skalieren Sie ein Rechenzentrum (ändern Sie zum Skalieren den nodeCount-Wert):

```azurecli-interactive
resourceGroupName='MyResourceGroup'
clusterName='cassandra-hybrid-cluster'
dataCenterName='dc1'
dataCenterLocation='eastus'
delegatedSubnetId= '/subscriptions/<Subscription_ID>/resourceGroups/customer-vnet-rg/providers/Microsoft.Network/virtualNetworks/customer-vnet/subnets/dc1-subnet'

az managed-cassandra datacenter update \
    --resource-group $resourceGroupName \
    --cluster-name $clusterName \
    --data-center-name $dataCenterName \
    --node-count 13 
```

### <a name="get-the-datacenters-in-a-cluster"></a><a id="get-datacenters-cluster"></a>Abrufen der Rechenzentren in einem Cluster

So rufen Sie die Rechenzentren in einem Cluster ab:

```azurecli-interactive
resourceGroupName='MyResourceGroup'
clusterName='cassandra-hybrid-cluster'

az managed-cassandra datacenter list \
    --resource-group $resourceGroupName \
    --cluster-name $clusterName
```

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen eines Azure SQL Managed Instance-Clusters über das Azure-Portal](create-cluster-portal.md)
* [Bereitstellen eines verwalteten Apache Spark-Clusters mit Azure Databricks](deploy-cluster-databricks.md)