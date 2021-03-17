---
title: 'Schnellstart: Erstellen eines Clusters vom Typ „Azure Managed Instance for Apache Cassandra“ mithilfe der CLI'
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie mithilfe der Azure CLI einen Cluster vom Typ „Azure Managed Instance for Apache Cassandra“ erstellen.
author: TheovanKraay
ms.author: thvankra
ms.service: managed-instance-apache-cassandra
ms.topic: quickstart
ms.date: 03/02/2021
ms.openlocfilehash: 86fa7e2e45dacb86b6601b699dca46b1b909fd08
ms.sourcegitcommit: ba676927b1a8acd7c30708144e201f63ce89021d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2021
ms.locfileid: "102424698"
---
# <a name="quickstart-create-an-azure-managed-instance-for-apache-cassandra-cluster-using-azure-cli-preview"></a>Schnellstart: Erstellen eines Clusters vom Typ „Azure Managed Instance for Apache Cassandra“ mithilfe der Azure CLI (Vorschau)

Azure Managed Instance for Apache Cassandra bietet automatisierte Bereitstellungs- und Skalierungsvorgänge für verwaltete Open-Source-basierte Apache Cassandra-Rechenzentren. Dieser Dienst unterstützt Sie dabei, Hybridszenarien zu beschleunigen und die laufende Wartung zu verringern.

> [!IMPORTANT]
> Azure Managed Instance for Apache Cassandra befindet sich derzeit in der Public Preview-Phase.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

In dieser Schnellstartanleitung erfahren Sie, wie Sie mithilfe der Azure CLI einen Cluster mit Azure Managed Instance for Apache Cassandra erstellen. Außerdem wird gezeigt, wie Sie ein Rechenzentrum erstellen und Knoten im Rechenzentrum hoch- oder herunterskalieren.

[!INCLUDE [azure-cli-prepare-your-environment.md](../../includes/azure-cli-prepare-your-environment.md)]

* Für diesen Artikel ist mindestens die Azure CLI-Version 2.12.1 erforderlich. Wenn Sie Azure Cloud Shell verwenden, ist die neueste Version bereits installiert.

* [Azure Virtual Network](../virtual-network/virtual-networks-overview.md) mit einer Verbindung mit Ihrer selbstgehosteten bzw. lokalen Umgebung. Weitere Informationen zum Herstellen einer Verbindung für lokale Umgebungen mit Azure finden Sie im Artikel [Verbinden eines lokalen Netzwerks mit Azure](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/).

* Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.


## <a name="create-a-managed-instance-cluster"></a><a id="create-cluster"></a>Erstellen eines Managed Instance-Clusters

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)

1. Legen Sie Ihre Abonnement-ID über die Azure CLI fest:

   ```azurecli-interactive
   az account set -s <Subscription_ID>
   ```

1. Erstellen Sie als Nächstes ein virtuelles Netzwerk mit einem dedizierten Subnetz in Ihrer Ressourcengruppe:

   ```azurecli-interactive
   az network vnet create -n <VNet_Name> -l eastus2 -g <Resource_Group_Name> --subnet-name <Subnet Name>
   ```

1. Wenden Sie einige spezielle Berechtigungen auf das virtuelle Netzwerk und das Subnetz an, die für die verwaltete Instanz erforderlich sind. Verwenden Sie den Befehl `az role assignment create`, und ersetzen Sie `<subscription ID>`, `<resource group name>`, `<VNet name>` und `<subnet name>` durch die entsprechenden Werte:

   ```azurecli-interactive
   az role assignment create --assignee e5007d2c-4b13-4a74-9b6a-605d99f03501 --role 4d97b98b-1d4f-4787-a291-c67834d212e7 --scope /subscriptions/<subscription ID>/resourceGroups/<resource group name>/providers/Microsoft.Network/virtualNetworks/<VNet name>/subnets/<subnet name>
   ```

   > [!NOTE]
   > Die Werte `assignee` und `role` im vorherigen Befehl sind feste Dienstprinzipal- bzw. Rollenbezeichner.

1. Erstellen Sie als Nächstes den Cluster in Ihrem neu erstellten virtuellen Netzwerk. Führen Sie den folgenden Befehl aus, und verwenden Sie dabei den im vorherigen Befehl abgerufenen Wert des Parameters `Resource ID` als Wert der Variablen `delegatedManagementSubnetId`:

   ```azurecli-interactive
   resourceGroupName='<Resource_Group_Name>'
   clusterName='<Cluster_Name>'
   location='eastus2'
   delegatedManagementSubnetId='<Resource_ID>'
   initialCassandraAdminPassword='myPassword'
    
   az managed-cassandra cluster create \
      --cluster-name $clusterName \
      --resource-group $resourceGroupName \
      --location $location \
      --delegated-management-subnet-id $delegatedManagementSubnetId \
      --initial-cassandra-admin-password $initialCassandraAdminPassword \
      --debug
   ```

1. Erstellen Sie für den Cluster abschließend ein Rechenzentrum mit drei Knoten:

   ```azurecli-interactive
   dataCenterName='dc1'
   dataCenterLocation='eastus2'
   delegatedSubnetId='<Resource_ID>'
    
   az managed-cassandra datacenter create \
      --resource-group $resourceGroupName \
      --cluster-name $clusterName \
      --data-center-name $dataCenterName \
      --data-center-location $dataCenterLocation \
      --delegated-subnet-id $delegatedSubnetId \
      --node-count 3 
   ```

1. Die Knoten des erstellten Rechenzentrums können bei Bedarf mithilfe des folgenden Befehls hoch- oder herunterskaliert werden. Ändern Sie den Wert des Parameters `node-count` in den gewünschten Wert:

   ```azurecli-interactive
   resourceGroupName='<Resource_Group_Name>'
   clusterName='<Cluster Name>'
   dataCenterName='dc1'
   dataCenterLocation='eastus2'
   delegatedSubnetId= '<Resource_ID>'
    
   az managed-cassandra datacenter update \
      --resource-group $resourceGroupName \
      --cluster-name $clusterName \
      --data-center-name $dataCenterName \
      --node-count 9 
   ```

## <a name="connect-to-your-cluster"></a>Herstellen einer Clusterverbindung

Von Azure Managed Instance for Apache Cassandra werden keine Knoten mit öffentlicher IP-Adresse erstellt. Wenn Sie eine Verbindung mit dem neu erstellten Cassandra-Cluster herstellen möchten, müssen Sie eine weitere Ressource innerhalb des virtuellen Netzwerks erstellen. Bei dieser Ressource kann es sich um eine Anwendung handeln oder um einen virtuellen Computer, auf dem das Open-Source-Abfragetool [CQLSH](https://cassandra.apache.org/doc/latest/tools/cqlsh.html) von Apache installiert ist. Sie können eine [Resource Manager-Vorlage](https://azure.microsoft.com/resources/templates/101-vm-simple-linux/) verwenden, um einen virtuellen Ubuntu-Computer bereitzustellen. Stellen Sie nach dessen Bereitstellung eine SSH-Verbindung mit dem Computer her, und installieren Sie CQLSH, wie in den folgenden Befehlen gezeigt:

```bash
# Install default-jre and default-jdk
sudo apt update
sudo apt install openjdk-8-jdk openjdk-8-jre

# Install the Cassandra libraries in order to get CQLSH:
echo "deb http://www.apache.org/dist/cassandra/debian 311x main" | sudo tee -a /etc/apt/sources.list.d/cassandra.sources.list
curl https://downloads.apache.org/cassandra/KEYS | sudo apt-key add -
sudo apt-get update
sudo apt-get install cassandra

# Export the SSL variables:
export SSL_VERSION=TLSv1_2
export SSL_VALIDATE=false

# Connect to CQLSH (replace <IP> with the private IP addresses of the nodes in your Datacenter):
host=("<IP>" "<IP>" "<IP>")
cqlsh $host 9042 -u cassandra -p cassandra --ssl
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Ressourcen nicht mehr benötigen, führen Sie den Befehl `az group delete` aus, um die Ressourcengruppe, die verwaltete Instanz und alle zugehörigen Ressourcen zu löschen:

```azurecli-interactive
az group delete --name <Resource_Group_Name>
```

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie gelernt, wie Sie mithilfe der Azure CLI einen Cluster vom Typ „Azure Managed Instance for Apache Cassandra“ erstellen. Nun können Sie mit der Nutzung des Clusters beginnen:

> [!div class="nextstepaction"]
> [Bereitstellen eines verwalteten Apache Spark-Clusters mit Azure Databricks](deploy-cluster-databricks.md)
