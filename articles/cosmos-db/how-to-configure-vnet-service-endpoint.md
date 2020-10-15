---
title: Konfigurieren des auf einem virtuellen Netzwerk basierenden Zugriffs für ein Azure Cosmos-Konto
description: Dieses Dokument beschreibt die Schritte, die zum Einrichten eines VNET-Dienstendpunkts für Azure Cosmos DB erforderlich sind.
author: markjbrown
ms.service: cosmos-db
ms.topic: how-to
ms.date: 06/04/2020
ms.author: mjbrown
ms.custom: devx-track-azurecli
ms.openlocfilehash: 995e5a1a87ee332c48641f42c4134e3e58f11cfa
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87495419"
---
# <a name="configure-access-from-virtual-networks-vnet"></a>Konfigurieren des Zugriffs über virtuelle Netzwerke (VNET)

Sie können Azure Cosmos DB-Konten so konfigurieren, dass der Zugriff nur aus einem bestimmten Subnetz eines virtuellen Azure-Netzwerks (VNET) zugelassen wird. Um den Zugriff auf ein Azure Cosmos DB-Konto mit Verbindungen aus einem Subnetz in einem virtuellen Netzwerk einzuschränken, sind diese Schritte erforderlich:

1. Aktivieren Sie das Subnetz, um die Identität von Subnetz und virtuellem Netzwerk an Azure Cosmos DB zu senden. Aktivieren Sie dazu einen Dienstendpunkt für Azure Cosmos DB im entsprechenden Subnetz.

1. Fügen Sie im Azure Cosmos DB-Konto eine Regel hinzu, die dieses Subnetz als Quelle für den Zugriff auf das Konto angibt.

> [!NOTE]
> Wenn ein Dienstendpunkt für Ihr Azure Cosmos DB-Konto in einem Subnetz aktiviert ist, wechselt die Quelle des in Azure Cosmos DB eingehenden Datenverkehrs von einer öffentlichen IP-Adresse zu einem virtuellen Netzwerk und Subnetz. Der Wechsel des Datenverkehrs gilt für jedes Azure Cosmos DB-Konto, auf das über dieses Subnetz zugegriffen wird. Wenn Ihre Azure Cosmos DB-Konten über eine IP-basierte Firewall verfügen, um dieses Subnetz zuzulassen, entsprechen Anfragen von vom Dienst aktivierten Subnetzen nicht mehr den IP-Firewallregeln, und sie werden abgelehnt.
>
> Weitere Informationen finden Sie in den Schritten unter [Migrieren einer IP-Firewallregel zur Zugriffssteuerungsliste eines virtuellen Netzwerks](#migrate-from-firewall-to-vnet) in diesem Artikel.

Die folgenden Abschnitte beschreiben, wie Sie den VNET-Dienstendpunkt für ein Azure Cosmos DB-Konto konfigurieren.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="configure-a-service-endpoint-by-using-the-azure-portal"></a><a id="configure-using-portal"></a>Konfigurieren eines Dienstendpunkts über das Azure-Portal

### <a name="configure-a-service-endpoint-for-an-existing-azure-virtual-network-and-subnet"></a>Konfigurieren eines Dienstendpunkts für ein vorhandenes virtuelles Azure-Netzwerk und Subnetz

1. Suchen Sie auf dem Blatt **Alle Ressourcen** das Azure Cosmos DB-Konto, das Sie schützen möchten.

1. Wählen Sie aus dem Einstellungsmenü die Option **Firewalls und virtuelle Netzwerke**, und aktivieren Sie **Ausgewählte Netzwerke**, um den Zugriff über ausgewählte Netzwerke zuzulassen.

1. Um Zugriff auf das Subnetz eines vorhandenen virtuellen Netzwerks zu gewähren, wählen Sie unter **Virtuelle Netzwerke** die Option **Vorhandenes Azure Virtual Network hinzufügen** aus.

1. Wählen Sie das **Abonnement** aus, aus dem ein virtuelles Azure-Netzwerk hinzugefügt werden soll. Wählen Sie für **Virtuelle Netzwerke** und **Subnetze** die virtuellen Azure-Netzwerke und Subnetze aus, denen Sie Zugriff auf Ihr Azure Cosmos DB-Konto gewähren möchten. Klicken Sie als Nächstes auf **Aktivieren**, um die ausgewählten Netzwerke mit Dienstendpunkten für „Microsoft.AzureCosmosDB“ zu aktivieren. Wenn der Vorgang abgeschlossen ist, klicken Sie auf **Hinzufügen**.

   :::image type="content" source="./media/how-to-configure-vnet-service-endpoint/choose-subnet-and-vnet.png" alt-text="Auswählen des virtuellen Netzwerks und des Subnetzes":::

1. Nachdem das Azure Cosmos DB-Konto für den Zugriff aus einem virtuellen Netzwerk aktiviert wurde, lässt es nur den Datenverkehr aus diesem ausgewählten Subnetz zu. Das von Ihnen hinzugefügte virtuelle Netzwerk und das Subnetz sollten wie im folgenden Screenshot angezeigt werden:

   :::image type="content" source="./media/how-to-configure-vnet-service-endpoint/vnet-and-subnet-configured-successfully.png" alt-text="Auswählen des virtuellen Netzwerks und des Subnetzes":::

> [!NOTE]
> Um VNET-Dienstendpunkte zu aktivieren, benötigen Sie die folgenden Abonnementberechtigungen:
>   * Abonnement mit virtuellem Netzwerk: Netzwerkmitwirkender
>   * Abonnement mit Azure Cosmos DB-Konto: DocumentDB-Kontomitwirkender
>   * Wenn sich Ihr virtuelles Netzwerk und Ihr Azure Cosmos DB-Konto in unterschiedlichen Abonnements befinden, stellen Sie sicher, dass für das Abonnement mit dem virtuellen Netzwerk ebenfalls ein `Microsoft.DocumentDB`-Ressourcenanbieter registriert ist. Informationen zum Registrieren eines Ressourcenanbieters finden Sie im Artikel [Azure-Ressourcenanbieter und -typen](../azure-resource-manager/management/resource-providers-and-types.md).

Im Folgenden finden Sie die Schritte zum Registrieren eines Abonnements bei einem Ressourcenanbieter.

### <a name="configure-a-service-endpoint-for-a-new-azure-virtual-network-and-subnet"></a>Konfigurieren eines Dienstendpunkts für ein neues virtuelles Azure-Netzwerk und Subnetz

1. Suchen Sie auf dem Blatt **Alle Ressourcen** das Azure Cosmos DB-Konto, das Sie schützen möchten.  

1. Wählen Sie aus dem Einstellungsmenü die Option **Firewalls und virtuelle Azure-Netzwerke**, und aktivieren Sie **Ausgewählte Netzwerke**, um den Zugriff über ausgewählte Netzwerke zuzulassen.  

1. Um Zugriff auf ein neues virtuelles Azure-Netzwerk zu gewähren, wählen Sie unter **Virtuelle Netzwerke** die Option **Neues virtuelles Netzwerk hinzufügen** aus.  

1. Geben Sie die Informationen ein, die zum Erstellen des neuen virtuellen Netzwerks erforderlich sind, und klicken Sie dann auf **Erstellen**. Das Subnetz wird mit einem für „Microsoft.AzureCosmosDB“ aktivierten Dienstendpunkt erstellt.

   :::image type="content" source="./media/how-to-configure-vnet-service-endpoint/choose-subnet-and-vnet-new-vnet.png" alt-text="Auswählen des virtuellen Netzwerks und des Subnetzes":::

Wenn Ihr Azure Cosmos DB-Konto von anderen Azure-Diensten wie z. B. Azure Cognitive Search verwendet wird oder Stream Analytics oder Power BI darauf zugreifen, gewähren Sie Zugriff, indem Sie das Kontrollkästchen **Accept connections from within global Azure datacenters** (Zugriff aus globalen Azure-Rechenzentren zulassen) aktivieren.

Um sicherzustellen, dass Sie vom Portal aus auf Azure Cosmos DB-Metriken zugreifen können, müssen Sie Optionen für **Zugriff über das Azure-Portal zulassen** aktivieren. Weitere Informationen zu diesen Optionen finden Sie im Artikel [Konfigurieren einer IP-Firewall](how-to-configure-firewall.md). Nachdem Sie den Zugriff aktiviert haben, klicken Sie auf **Speichern**, um die Einstellungen zu speichern.

## <a name="remove-a-virtual-network-or-subnet"></a><a id="remove-vnet-or-subnet"></a>Entfernen eines virtuellen Netzwerks oder eines Subnetzes

1. Suchen Sie auf dem Blatt **Alle Ressourcen** das Azure Cosmos DB-Konto, dem Sie Dienstendpunkte zugewiesen haben.  

1. Wählen Sie aus Einstellungsmenü die Option **Firewalls und virtuelle Netzwerke** aus.  

1. Um eine Regel für ein virtuelles Netzwerk oder ein Subnetz zu entfernen, klicken Sie neben dem virtuellen Netzwerk oder dem Subnetz auf **...** , und wählen Sie **Entfernen** aus.

   :::image type="content" source="./media/how-to-configure-vnet-service-endpoint/remove-a-vnet.png" alt-text="Auswählen des virtuellen Netzwerks und des Subnetzes":::

1. Klicken Sie zum Übernehmen der Änderungen auf **Speichern**.

## <a name="configure-a-service-endpoint-by-using-azure-powershell"></a><a id="configure-using-powershell"></a>Konfigurieren eines Dienstendpunkts über Azure PowerShell

> [!NOTE]
> Wenn Sie PowerShell oder die Azure CLI verwenden, stellen Sie sicher, dass Sie die vollständige Liste der IP-Filter und Zugriffssteuerungslisten des virtuellen Netzwerks in den Parametern angeben, und nicht nur diejenigen, die hinzugefügt werden müssen.

Führen Sie die folgenden Schritte aus, um über Azure PowerShell einen Dienstendpunkt für ein Azure Cosmos DB-Konto zu konfigurieren:  

1. Installieren Sie [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-Az-ps), und [melden Sie sich an](https://docs.microsoft.com/powershell/azure/authenticate-azureps).  

1. Aktivieren Sie den Dienstendpunkt für ein vorhandenes Subnetz eines Virtual Network.  

   ```powershell
   $resourceGroupName = "<Resource group name>"
   $vnetName = "<Virtual network name>"
   $subnetName = "<Subnet name>"
   $subnetPrefix = "<Subnet address range>"
   $serviceEndpoint = "Microsoft.AzureCosmosDB"

   Get-AzVirtualNetwork `
      -ResourceGroupName $resourceGroupName `
      -Name $vnetName | Set-AzVirtualNetworkSubnetConfig `
      -Name $subnetName `
      -AddressPrefix $subnetPrefix `
      -ServiceEndpoint $serviceEndpoint | Set-AzVirtualNetwork
   ```

1. Rufen Sie Informationen zum virtuellen Netzwerk ab.

   ```powershell
   $vnet = Get-AzVirtualNetwork `
      -ResourceGroupName $resourceGroupName `
      -Name $vnetName

   $subnetId = $vnet.Id + "/subnets/" + $subnetName
   ```

1. Vorbereiten einer Cosmos DB-Regel für das virtuelle Netzwerk

   ```powershell
   $vnetRule = New-AzCosmosDBVirtualNetworkRule `
      -Id $subnetId
   ```

1. Aktualisieren Sie die Azure Cosmos DB-Kontoeigenschaften mit der neuen Virtual Network-Endpunktkonfiguration: 

   ```powershell
   $accountName = "<Cosmos DB account name>"

   Update-AzCosmosDBAccount `
      -ResourceGroupName $resourceGroupName `
      -Name $accountName `
      -EnableVirtualNetwork $true `
      -VirtualNetworkRuleObject @($vnetRule)
   ```

1. Führen Sie den folgenden Befehl aus, um zu überprüfen, ob Ihr Azure Cosmos DB-Konto mit dem VNET-Dienstendpunkt aktualisiert wurde, den Sie im vorherigen Schritt konfiguriert haben:

   ```powershell
   $account = Get-AzCosmosDBAccount `
      -ResourceGroupName $resourceGroupName `
      -Name $accountName

   $account.IsVirtualNetworkFilterEnabled
   $account.VirtualNetworkRules
   ```

## <a name="configure-a-service-endpoint-by-using-the-azure-cli"></a><a id="configure-using-cli"></a>Konfigurieren eines Dienstendpunkts über die Azure CLI

Azure Cosmos-Konten können für Dienstendpunkte konfiguriert werden, wenn sie zu einem späteren Zeitpunkt erstellt oder aktualisiert werden, sofern das Subnetz bereits für sie konfiguriert wurde. Dienstendpunkte können auch für das Cosmos-Konto aktiviert werden, wenn das Subnetz noch nicht für sie konfiguriert wurde. Die Endpunkte werden aktiv, wenn das Subnetz später konfiguriert wird. Diese Flexibilität ermöglicht Administratoren, die weder Zugriff auf das Cosmos-Konto noch auf virtuelle Netzwerkressourcen haben, Konfigurationen unabhängig voneinander vorzunehmen.

### <a name="create-a-new-cosmos-account-and-connect-it-to-a-back-end-subnet-for-a-new-virtual-network"></a>Erstellen Sie ein neues Cosmos-Konto, und verbinden Sie es mit einem Back-End-Subnetz eines neuen virtuellen Netzwerks.

In diesem Beispiel erstellen Sie das virtuelle Netzwerk und das Subnetz. Zum Zeitpunkt der Erstellung sind Dienstendpunkte für beide Netze aktiviert.

```azurecli-interactive
# Create an Azure Cosmos Account with a service endpoint connected to a backend subnet

# Resource group and Cosmos account variables
resourceGroupName='MyResourceGroup'
location='West US 2'
accountName='mycosmosaccount'

# Variables for a new Virtual Network with two subnets
vnetName='myVnet'
frontEnd='FrontEnd'
backEnd='BackEnd'

# Create a resource group
az group create -n $resourceGroupName -l $location

# Create a virtual network with a front-end subnet
az network vnet create \
   -n $vnetName \
   -g $resourceGroupName \
   --address-prefix 10.0.0.0/16 \
   --subnet-name $frontEnd \
   --subnet-prefix 10.0.1.0/24

# Create a back-end subnet with service endpoints enabled for Cosmos DB
az network vnet subnet create \
   -n $backEnd \
   -g $resourceGroupName \
   --address-prefix 10.0.2.0/24 \
   --vnet-name $vnetName \
   --service-endpoints Microsoft.AzureCosmosDB

svcEndpoint=$(az network vnet subnet show -g $resourceGroupName -n $backEnd --vnet-name $vnetName --query 'id' -o tsv)

# Create a Cosmos DB account with default values and service endpoints
az cosmosdb create \
   -n $accountName \
   -g $resourceGroupName \
   --enable-virtual-network true \
   --virtual-network-rules $svcEndpoint
```

### <a name="connect-and-configure-a-cosmos-account-to-a-back-end-subnet-independently"></a>Verbinden eines Cosmos-Kontos mit einem Back-End-Subnetz und unabhängige Konfiguration

Dieses Beispiel soll veranschaulichen, wie Sie ein Azure Cosmos-Konto mit einem vorhandenen neuen virtuellen Netzwerk verbinden, in dem das Subnetz noch nicht für Dienstendpunkte konfiguriert wurde. Dies erfolgt mithilfe des `--ignore-missing-vnet-service-endpoint`-Parameters. Dadurch kann die Konfiguration für das Cosmos-Konto ohne Fehler abgeschlossen werden, bevor die Konfiguration für das Subnetz des virtuellen Netzwerks abgeschlossen ist. Sobald die Subnetzkonfiguration abgeschlossen ist, kann über das konfigurierte Subnetz auf das Cosmos-Konto zugegriffen werden.

```azurecli-interactive
# Create an Azure Cosmos Account with a service endpoint connected to a backend subnet
# that is not yet enabled for service endpoints.

# Resource group and Cosmos account variables
resourceGroupName='MyResourceGroup'
location='West US 2'
accountName='mycosmosaccount'

# Variables for a new Virtual Network with two subnets
vnetName='myVnet'
frontEnd='FrontEnd'
backEnd='BackEnd'

# Create a resource group
az group create -n $resourceGroupName -l $location

# Create a virtual network with a front-end subnet
az network vnet create \
   -n $vnetName \
   -g $resourceGroupName \
   --address-prefix 10.0.0.0/16 \
   --subnet-name $frontEnd \
   --subnet-prefix 10.0.1.0/24

# Create a back-end subnet but without configuring service endpoints (--service-endpoints Microsoft.AzureCosmosDB)
az network vnet subnet create \
   -n $backEnd \
   -g $resourceGroupName \
   --address-prefix 10.0.2.0/24 \
   --vnet-name $vnetName

svcEndpoint=$(az network vnet subnet show -g $resourceGroupName -n $backEnd --vnet-name $vnetName --query 'id' -o tsv)

# Create a Cosmos DB account with default values
az cosmosdb create -n $accountName -g $resourceGroupName

# Add the virtual network rule but ignore the missing service endpoint on the subnet
az cosmosdb network-rule add \
   -n $accountName \
   -g $resourceGroupName \
   --virtual-network $vnetName \
   --subnet svcEndpoint \
   --ignore-missing-vnet-service-endpoint true

read -p'Press any key to now configure the subnet for service endpoints'

az network vnet subnet update \
   -n $backEnd \
   -g $resourceGroupName \
   --vnet-name $vnetName \
   --service-endpoints Microsoft.AzureCosmosDB
```

## <a name="port-range-when-using-direct-mode"></a>Portbereich bei Verwendung des direkten Modus

Wenn Sie Dienstendpunkte mit einem Azure Cosmos-Konto über eine Verbindung im direkten Modus verwenden, muss der TCP-Portbereich von 10.000 bis 20.000 geöffnet sein.

## <a name="migrating-from-an-ip-firewall-rule-to-a-virtual-network-acl"></a><a id="migrate-from-firewall-to-vnet"></a>Migrieren einer IP-Firewallregel zur Zugriffssteuerungsliste eines virtuellen Netzwerks

Führen Sie die folgenden Schritte aus, um ein Azure Cosmos DB-Konto von der Verwendung von IP-Firewallregeln zur Verwendung von VNET-Dienstendpunkten zu migrieren.

Sobald ein Azure Cosmos DB-Konto für einen Dienstendpunkt für ein Subnetz konfiguriert ist, werden Anforderungen von diesem Subnetz mit Informationen zu virtuellem Netzwerk und Subnetzquelle statt an eine öffentliche IP-Adresse an Azure Cosmos DB gesendet. Diese Anforderungen stimmen nicht mehr mit einem für das Azure Cosmos DB-Konto konfigurierten IP-Filter überein. Aus diesem Grund sind die folgenden Schritte erforderlich, um Ausfallzeiten zu vermeiden.

Bevor Sie fortfahren, aktivieren Sie den Azure Cosmos DB-Dienstendpunkt im virtuellen Netzwerk und Subnetz mit dem oben unter „Aktivieren Sie den Dienstendpunkt für ein vorhandenes Subnetz eines Virtual Network“ aufgeführten Schritt.

1. Abrufen der Informationen zu virtuellem Netzwerk und Subnetz:

   ```powershell
   $resourceGroupName = "myResourceGroup"
   $accountName = "mycosmosaccount"
   $vnetName = "myVnet"
   $subnetName = "mySubnet"

   $vnet = Get-AzVirtualNetwork `
      -ResourceGroupName $resourceGroupName `
      -Name $vnetName

   $subnetId = $vnet.Id + "/subnets/" + $subnetName
   ```

1. Bereiten Sie ein neues Virtual Network-Regelobjekt für das Azure Cosmos DB-Konto vor:

   ```powershell
   $vnetRule = New-AzCosmosDBVirtualNetworkRule `
      -Id $subnetId
   ```

1. Aktualisieren Sie das Azure Cosmos DB-Konto, um den Dienstendpunktzugriff aus dem Subnetz zu aktivieren:

   ```powershell
   Update-AzCosmosDBAccount `
      -ResourceGroupName $resourceGroupName `
      -Name $accountName `
      -EnableVirtualNetwork $true `
      -VirtualNetworkRuleObject @($vnetRule)
   ```

1. Wiederholen Sie die vorherigen Schritte für alle Azure Cosmos DB-Konten, auf die Sie aus dem Subnetz zugreifen.

1. Entfernen Sie die IP-Firewallregel für das Subnetz aus den Firewallregeln des Azure Cosmos DB-Kontos.

## <a name="next-steps"></a>Nächste Schritte

* Um eine Firewall für Azure Cosmos DB zu konfigurieren, lesen Sie den Artikel [Firewallunterstützung](firewall-support.md).
