---
title: Resource Manager-Vorlagen für die Cassandra-API von Azure Cosmos DB
description: Verwenden Sie Azure Resource Manager-Vorlagen zum Erstellen und Konfigurieren der Cassandra-API von Azure Cosmos DB.
author: TheovanKraay
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/12/2019
ms.author: thvankra
ms.openlocfilehash: 54d1c27148f20c89e17b343f0ac87295f978c140
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75445280"
---
# <a name="manage-azure-cosmos-db-cassandra-api-resources-using-azure-resource-manager-templates"></a>Verwalten von Ressourcen für die Cassandra-API von Azure Cosmos DB mithilfe von Azure Resource Manager-Vorlagen

In diesem Artikel wird beschrieben, wie Sie verschiedene Vorgänge durchführen können, um die Verwaltung Ihrer Azure Cosmos DB-Konten, -Datenbanken und -Container mithilfe von Azure Resource Manager-Vorlagen zu automatisieren. Dieser Artikel enthält nur Beispiele für SQL-API-Konten. Weitere Beispiele für andere API-Typen finden Sie in den Artikeln zur Verwendung von Azure Resource Manager-Vorlagen mit der Azure Cosmos DB-API für [SQL](manage-sql-with-resource-manager.md), [Gremlin](manage-gremlin-with-resource-manager.md), [MongoDB](manage-mongodb-with-resource-manager.md) und [Tabellen](manage-table-with-resource-manager.md).

## Erstellen von Azure Cosmos-Konto, -Keyspace und -Tabelle <a id="create-resource"></a>

Erstellen Sie Azure Cosmos DB-Ressourcen mithilfe einer Azure Resource Manager-Vorlage. Mit dieser Vorlage wird ein Azure Cosmos-Konto für die Cassandra-API mit zwei Tabellen erstellt, die sich auf Keyspace-Ebene einen Durchsatz von 400 RU/s teilen. Kopieren Sie die Vorlage, und stellen Sie sie wie unten gezeigt bereit, oder besuchen Sie den [Azure-Schnellstartkatalog](https://azure.microsoft.com/resources/templates/101-cosmosdb-cassandra/), und führen Sie die Bereitstellung über das Azure-Portal durch. Sie können die Vorlage auch auf Ihren lokalen Computer herunterladen oder eine neue Vorlage erstellen und den lokalen Pfad mit dem Parameter `--template-file` angeben.

> [!NOTE]
> Kontonamen müssen Kleinbuchstaben enthalten; ihre Länge darf maximal 44 Zeichen betragen.
> Zum Aktualisieren der RU/s müssen Sie die Vorlage mit den aktualisierten Eigenschaftswerten für den Durchsatz erneut übermitteln.

[!code-json[create-cosmos-Cassandra](~/quickstart-templates/101-cosmosdb-cassandra/azuredeploy.json)]

## <a name="deploy-with-the-azure-cli"></a>Bereitstellen mit der Azure-Befehlszeilenschnittstelle (Azure CLI)

Wenn Sie die Azure Resource Manager-Vorlage mit der Azure CLI bereitstellen möchten, **kopieren** Sie das Skript, und wählen Sie **Ausprobieren** aus, um Azure Cloud Shell zu öffnen. Klicken Sie zum Einfügen des Skripts mit der rechten Maustaste auf die Shell, und wählen Sie **Einfügen** aus:

```azurecli-interactive

read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the location (i.e. westus2): ' location
read -p 'Enter the account name: ' accountName
read -p 'Enter the primary region (i.e. westus2): ' primaryRegion
read -p 'Enter the secondary region (i.e. eastus2): ' secondaryRegion
read -p 'Enter the keyset name: ' keysetName
read -p 'Enter the first table name: ' table1Name
read -p 'Enter the second table name: ' table2Name

az group create --name $resourceGroupName --location $location
az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-cassandra/azuredeploy.json \
   --parameters accountName=$accountName primaryRegion=$primaryRegion secondaryRegion=$secondaryRegion keysetName=$keysetName \
   table1Name=$table1Name table2Name=$table2Name

az cosmosdb show --resource-group $resourceGroupName --name accountName --output tsv
```

Der Befehl `az cosmosdb show` zeigt das neu erstellte Azure Cosmos-Konto, nachdem es bereitgestellt wurde. Wenn Sie eine lokal installierte Version der Azure CLI statt Cloud Shell verwenden möchten, lesen Sie den Artikel [Azure-Befehlszeilenschnittstelle (CLI)](/cli/azure/).


## <a name="next-steps"></a>Nächste Schritte

Hier sind einige zusätzlichen Ressourcen:

- [Dokumentation zu Azure Resource Manager](/azure/azure-resource-manager/)
- [Schema von Azure Cosmos DB-Ressourcenanbietern](/azure/templates/microsoft.documentdb/allversions)
- [Schnellstartvorlagen für Azure Cosmos DB](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.DocumentDB&pageNumber=1&sort=Popular)
- [Beheben gängiger Azure-Bereitstellungsfehler mit Azure Resource Manager](../azure-resource-manager/resource-manager-common-deployment-errors.md)
