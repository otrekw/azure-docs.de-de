---
title: Resource Manager-Vorlagen für die Gremlin-API von Azure Cosmos DB
description: Verwenden Sie Azure Resource Manager-Vorlagen zum Erstellen und Konfigurieren der Gremlin-API von Azure Cosmos DB.
author: TheovanKraay
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/12/2019
ms.author: thvankra
ms.openlocfilehash: e4c18d93f07cb2143dcc5bf9c93b9ac7298d2f7b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79225706"
---
# <a name="manage-azure-cosmos-db-gremlin-api-resources-using-azure-resource-manager-templates"></a>Verwalten von Ressourcen für die Gremlin-API von Azure Cosmos DB mithilfe von Azure Resource Manager-Vorlagen

In diesem Artikel wird beschrieben, wie Sie verschiedene Vorgänge durchführen können, um die Verwaltung Ihrer Azure Cosmos DB-Konten, -Datenbanken und -Container mithilfe von Azure Resource Manager-Vorlagen zu automatisieren. Dieser Artikel enthält Beispiele nur für Gremlin-API-Konten. Beispiele für Konten anderer API-Typen finden Sie in den Artikeln zur Verwendung von Azure Resource Manager-Vorlagen mit der Azure Cosmos DB-API für [Cassandra](manage-cassandra-with-resource-manager.md), [SQL](manage-sql-with-resource-manager.md), [MongoDB](manage-mongodb-with-resource-manager.md) und [Tabellen](manage-table-with-resource-manager.md).

## <a name="create-azure-cosmos-db-api-for-mongodb-account-database-and-collection"></a>Erstellen der Azure Cosmos DB-API für MongoDB-Konto, -Datenbank und -Sammlung <a id="create-resource"></a>

Erstellen Sie Azure Cosmos DB-Ressourcen mithilfe einer Azure Resource Manager-Vorlage. Mit dieser Vorlage wird ein Azure Cosmos-Konto für die Gremlin-API mit zwei Graphen erstellt, die sich auf Datenbankebene einen Durchsatz von 400 RU/s teilen. Kopieren Sie die Vorlage, und stellen Sie sie wie unten gezeigt bereit, oder besuchen Sie den [Azure-Schnellstartkatalog](https://azure.microsoft.com/resources/templates/101-cosmosdb-gremlin/), und führen Sie die Bereitstellung über das Azure-Portal durch. Sie können die Vorlage auch auf Ihren lokalen Computer herunterladen oder eine neue Vorlage erstellen und den lokalen Pfad mit dem Parameter `--template-file` angeben.

> [!NOTE]
> Kontonamen müssen Kleinbuchstaben enthalten; ihre Länge darf maximal 44 Zeichen betragen.
> Zum Aktualisieren der RU/s müssen Sie die Vorlage mit den aktualisierten Eigenschaftswerten für den Durchsatz erneut übermitteln.

:::code language="json" source="~/quickstart-templates/101-cosmosdb-gremlin/azuredeploy.json":::

## <a name="deploy-with-the-azure-cli"></a>Bereitstellen mit der Azure-Befehlszeilenschnittstelle (Azure CLI)

Wenn Sie die Azure Resource Manager-Vorlage mit der Azure CLI bereitstellen möchten, **kopieren** Sie das Skript, und wählen Sie **Ausprobieren** aus, um Azure Cloud Shell zu öffnen. Klicken Sie zum Einfügen des Skripts mit der rechten Maustaste auf die Shell, und wählen Sie **Einfügen** aus:

```azurecli-interactive

read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the location (i.e. westus2): ' location
read -p 'Enter the account name: ' accountName
read -p 'Enter the primary region (i.e. westus2): ' primaryRegion
read -p 'Enter the secondary region (i.e. eastus2): ' secondaryRegion
read -p 'Enter the database name: ' databaseName
read -p 'Enter the first graph name: ' graph1Name
read -p 'Enter the second graph name: ' graph2Name

az group create --name $resourceGroupName --location $location
az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-gremlin/azuredeploy.json \
   --parameters accountName=$accountName primaryRegion=$primaryRegion secondaryRegion=$secondaryRegion databaseName=$databaseName \
   graph1Name=$graph1Name graph2Name=$graph2Name

az cosmosdb show --resource-group $resourceGroupName --name accountName --output tsv
```

Der Befehl `az cosmosdb show` zeigt das neu erstellte Azure Cosmos-Konto, nachdem es bereitgestellt wurde. Wenn Sie eine lokal installierte Version der Azure CLI statt Cloud Shell verwenden möchten, lesen Sie den Artikel [Azure-Befehlszeilenschnittstelle (CLI)](/cli/azure/).

## <a name="next-steps"></a>Nächste Schritte

Hier sind einige zusätzlichen Ressourcen:

- [Dokumentation zu Azure Resource Manager](/azure/azure-resource-manager/)
- [Schema von Azure Cosmos DB-Ressourcenanbietern](/azure/templates/microsoft.documentdb/allversions)
- [Schnellstartvorlagen für Azure Cosmos DB](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.DocumentDB&pageNumber=1&sort=Popular)
- [Beheben gängiger Azure-Bereitstellungsfehler mit Azure Resource Manager](../azure-resource-manager/templates/common-deployment-errors.md)