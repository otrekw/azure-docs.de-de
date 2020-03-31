---
title: Erstellen und Verwalten von Azure Cosmos DB mit Resource Manager-Vorlagen
description: Verwenden von Azure Resource Manager-Vorlagen zum Erstellen und Konfigurieren der SQL (Core)-API von Azure Cosmos DB
author: TheovanKraay
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/12/2019
ms.author: thvankra
ms.openlocfilehash: 72a87c3b23e0eed6cfbf1614388702443f4e99d0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79227274"
---
# <a name="manage-azure-cosmos-db-sql-core-api-resources-with-azure-resource-manager-templates"></a>Verwalten von Ressourcen für die SQL-API (Core) von Azure Cosmos DB mit Azure Resource Manager-Vorlagen

In diesem Artikel erfahren Sie, wie Sie Azure Resource Manager-Vorlagen verwenden, um die Verwaltung Ihrer Azure Cosmos DB-Konten, -Datenbanken und -Container zu automatisieren.

Dieser Artikel zeigt nur Azure Resource Manager-Vorlagenbeispiele für SQL-API-Konten. Es gibt jedoch auch Vorlagenbeispiele für [Cassandra-](manage-cassandra-with-resource-manager.md)-, [Gremlin](manage-gremlin-with-resource-manager.md)-, [MongoDB](manage-mongodb-with-resource-manager.md)- und [Table](manage-table-with-resource-manager.md)-APIs.

<a id="create-resource"></a>

## <a name="create-an-azure-cosmos-account-database-and-container"></a>Erstellen eines Kontos, einer Datenbank und eines Containers in Azure Cosmos DB

Diese Azure Resource Manager-Vorlage erstellt ein Azure Cosmos-Konto mit den folgenden Elementen:

* Zwei Container mit einem gemeinsamen Durchsatz von 400 Anforderungseinheiten pro Sekunde (RU/s) auf Datenbankebene.
* Ein Container mit einem dediziertem Durchsatz von 400 RU/s.

Zum Erstellen der Azure Cosmos DB Ressourcen kopieren Sie die folgende Beispielvorlage, und stellen Sie sie wie beschrieben entweder über [PowerShell](#deploy-via-powershell) oder die [Azure CLI](#deploy-via-azure-cli) bereit.

* Optional können Sie den [Azure-Schnellstartkatalog](https://azure.microsoft.com/resources/templates/101-cosmosdb-sql/) besuchen und die Bereitstellung über das Azure-Portal durchführen.
* Sie können die Vorlage auch auf Ihren lokalen Computer herunterladen oder eine neue Vorlage erstellen und den lokalen Pfad mit dem Parameter `--template-file` angeben.

> [!IMPORTANT]
>
> * Wenn Sie einem Azure Cosmos-Konto Speicherorte hinzufügen oder aus ihm entfernen, können Sie nicht gleichzeitig andere Eigenschaften ändern. Diese Vorgänge müssen separat ausgeführt werden.
> * Die Länge von Kontonamen ist auf 44 Zeichen (ausschließlich Kleinbuchstaben) beschränkt.
> * Um die Durchsatzwerte zu ändern, müssen Sie die Vorlage mit aktualisierter Angabe der RU/s erneut übermitteln.

:::code language="json" source="~/quickstart-templates/101-cosmosdb-sql/azuredeploy.json":::

> [!NOTE]
> Um einen Container mit großem Partitionsschlüssel zu erstellen, ändern Sie die vorherige Vorlage, um die Eigenschaft `"version":2` in das `partitionKey`-Objekt einzubeziehen.

### <a name="deploy-via-powershell"></a>Bereitstellen über PowerShell

So verwenden Sie PowerShell für die Bereitstellung der Resource Manager-Vorlage:

1. **Kopieren** Sie das Skript.
2. Wählen Sie **Ausprobieren** aus, um Azure Cloud Shell zu öffnen.
3. Klicken Sie mit der rechten Maustaste im Azure Cloud Shell-Fenster, und wählen Sie **Einfügen** aus.

```azurepowershell-interactive

$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$accountName = Read-Host -Prompt "Enter the account name"
$location = Read-Host -Prompt "Enter the location (i.e. westus2)"
$primaryRegion = Read-Host -Prompt "Enter the primary region (i.e. westus2)"
$secondaryRegion = Read-Host -Prompt "Enter the secondary region (i.e. eastus2)"
$databaseName = Read-Host -Prompt "Enter the database name"
$sharedThroughput = Read-Host -Prompt "Enter the shared database throughput (i.e. 400)"
$sharedContainer1Name = Read-Host -Prompt "Enter the first shared container name"
$sharedContainer2Name = Read-Host -Prompt "Enter the second shared container name"
$dedicatedContainer1Name = Read-Host -Prompt "Enter the dedicated container name"
$dedicatedThroughput = Read-Host -Prompt "Enter the dedicated container throughput (i.e. 400)"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-cosmosdb-sql/azuredeploy.json" `
    -accountName $accountName `
    -location $location `
    -primaryRegion $primaryRegion `
    -secondaryRegion $secondaryRegion `
    -databaseName $databaseName `
    -sharedThroughput $ $sharedThroughput `
    -sharedContainer1Name $sharedContainer1Name `
    -sharedContainer2Name $sharedContainer2Name `
    -dedicatedContainer1Name $dedicatedContainer1Name `
    -dedicatedThroughput $dedicatedThroughput

 (Get-AzResource --ResourceType "Microsoft.DocumentDb/databaseAccounts" --ApiVersion "2019-08-01" --ResourceGroupName $resourceGroupName).name
```

Anstelle von Azure Cloud Shell können Sie die Vorlage mit einer lokal installierten Version von PowerShell bereitstellen. Sie müssen das [Azure PowerShell-Modul installieren](/powershell/azure/install-az-ps). Führen Sie `Get-Module -ListAvailable Az` aus, um die erforderliche Version zu ermitteln.

### <a name="deploy-via-azure-cli"></a>Bereitstellen über Azure CLI

So verwenden Sie die Azure CLI zum Bereitstellen der Azure Resource Manager-Vorlage:

1. **Kopieren** Sie das Skript.
2. Wählen Sie **Ausprobieren** aus, um Azure Cloud Shell zu öffnen.
3. Klicken Sie mit der rechten Maustaste im Azure Cloud Shell-Fenster, und wählen Sie **Einfügen** aus.

```azurecli-interactive
read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the location (i.e. westus2): ' location
read -p 'Enter the account name: ' accountName
read -p 'Enter the primary region (i.e. westus2): ' primaryRegion
read -p 'Enter the secondary region (i.e. eastus2): ' secondaryRegion
read -p 'Enter the database name: ' databaseName
read -p 'Enter the shared database throughput: sharedThroughput
read -p 'Enter the first shared container name: ' sharedContainer1Name
read -p 'Enter the second shared container name: ' sharedContainer2Name
read -p 'Enter the dedicated container name: ' dedicatedContainer1Name
read -p 'Enter the dedicated container throughput: dedicatedThroughput

az group create --name $resourceGroupName --location $location
az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-sql/azuredeploy.json \
   --parameters accountName=$accountName \
   primaryRegion=$primaryRegion \
   secondaryRegion=$secondaryRegion \
   databaseName=$databaseName \
   sharedThroughput=$sharedThroughput \
   sharedContainer1Name=$sharedContainer1Name \
   sharedContainer2Name=$sharedContainer2Name \
   dedicatedContainer1Name=$dedicatedContainer1Name \
   dedicatedThroughput=$dedicatedThroughput

az cosmosdb show --resource-group $resourceGroupName --name accountName --output tsv
```

Der Befehl `az cosmosdb show` zeigt das neu erstellte Azure Cosmos-Konto an, nachdem es bereitgestellt wurde. Anstelle von Azure Cloud Shell können Sie die Vorlage mit einer lokal installierten Version der Azure CLI bereitstellen. Weitere Informationen finden Sie im Artikel [Azure-Befehlszeilenschnittstelle (CLI)](/cli/azure/).

<a id="create-sproc"></a>

## <a name="create-an-azure-cosmos-db-container-with-server-side-functionality"></a>Erstellen eines Azure Cosmos DB-Containers mit serverseitiger Funktionalität

Sie können eine Azure Resource Manager-Vorlage verwenden, um einen Azure Cosmos DB-Container mit einer gespeicherten Prozedur, einem Trigger und einer benutzerdefinierten Funktion zu erstellen.

Kopieren Sie die folgende Beispielvorlage, und stellen Sie sie wie beschrieben entweder mit [PowerShell](#deploy-with-powershell) oder der [Azure CLI](#deploy-with-azure-cli) bereit.

* Optional können Sie den [Azure-Schnellstartkatalog](https://azure.microsoft.com/resources/templates/101-cosmosdb-sql-container-sprocs/) besuchen und die Bereitstellung über das Azure-Portal durchführen.
* Sie können die Vorlage auch auf Ihren lokalen Computer herunterladen oder eine neue Vorlage erstellen und den lokalen Pfad mit dem Parameter `--template-file` angeben.

:::code language="json" source="~/quickstart-templates/101-cosmosdb-sql-container-sprocs/azuredeploy.json":::

### <a name="deploy-with-powershell"></a>Bereitstellen mit PowerShell

So verwenden Sie PowerShell für die Bereitstellung der Resource Manager-Vorlage:

1. **Kopieren** Sie das Skript.
1. Wählen Sie **Ausprobieren** aus, um Azure Cloud Shell zu öffnen.
1. Klicken Sie mit der rechten Maustaste im Azure Cloud Shell-Fenster, und wählen Sie **Einfügen** aus.

```azurepowershell-interactive

$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$accountName = Read-Host -Prompt "Enter the account name"
$location = Read-Host -Prompt "Enter the location (i.e. westus2)"
$primaryRegion = Read-Host -Prompt "Enter the primary region (i.e. westus2)"
$secondaryRegion = Read-Host -Prompt "Enter the secondary region (i.e. eastus2)"
$databaseName = Read-Host -Prompt "Enter the database name"
$containerName = Read-Host -Prompt "Enter the container name"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-cosmosdb-sql-container-sprocs/azuredeploy.json" `
    -accountName $accountName `
    -location $location `
    -primaryRegion $primaryRegion `
    -secondaryRegion $secondaryRegion `
    -databaseName $databaseName `
    -containerName $containerName

 (Get-AzResource --ResourceType "Microsoft.DocumentDb/databaseAccounts" --ApiVersion "2019-08-01" --ResourceGroupName $resourceGroupName).name
```

Anstelle von Azure Cloud Shell können Sie die Vorlage mit einer lokal installierten Version von PowerShell bereitstellen. Sie müssen das [Azure PowerShell-Modul installieren](/powershell/azure/install-az-ps). Führen Sie `Get-Module -ListAvailable Az` aus, um die erforderliche Version zu ermitteln.

### <a name="deploy-with-azure-cli"></a>Bereitstellen über die Azure-Befehlszeilenschnittstelle

So verwenden Sie die Azure CLI zum Bereitstellen der Azure Resource Manager-Vorlage:

1. **Kopieren** Sie das Skript.
2. Wählen Sie **Ausprobieren** aus, um Azure Cloud Shell zu öffnen.
3. Klicken Sie mit der rechten Maustaste im Azure Cloud Shell-Fenster, und wählen Sie **Einfügen** aus.

```azurecli-interactive
read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the location (i.e. westus2): ' location
read -p 'Enter the account name: ' accountName
read -p 'Enter the primary region (i.e. westus2): ' primaryRegion
read -p 'Enter the secondary region (i.e. eastus2): ' secondaryRegion
read -p 'Enter the database name: ' databaseName
read -p 'Enter the container name: ' containerName

az group create --name $resourceGroupName --location $location
az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-sql-container-sprocs/azuredeploy.json \
   --parameters accountName=$accountName primaryRegion=$primaryRegion secondaryRegion=$secondaryRegion databaseName=$databaseName \
   containerName=$containerName

az cosmosdb show --resource-group $resourceGroupName --name accountName --output tsv
```

## <a name="next-steps"></a>Nächste Schritte

Hier sind einige zusätzlichen Ressourcen:

* [Dokumentation zu Azure Resource Manager](/azure/azure-resource-manager/)
* [Schema von Azure Cosmos DB-Ressourcenanbietern](/azure/templates/microsoft.documentdb/allversions)
* [Schnellstartvorlagen für Azure Cosmos DB](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Documentdb&pageNumber=1&sort=Popular)
* [Beheben gängiger Azure-Bereitstellungsfehler mit Azure Resource Manager](../azure-resource-manager/templates/common-deployment-errors.md)
