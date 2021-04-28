---
title: Resource Manager-Vorlagen für die Cassandra-API von Azure Cosmos DB
description: Verwenden Sie Azure Resource Manager-Vorlagen zum Erstellen und Konfigurieren der Cassandra-API von Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: how-to
ms.date: 10/14/2020
ms.author: mjbrown
ms.openlocfilehash: 4fd93aec24c752c176d790ca9fdc8a438e5c257a
ms.sourcegitcommit: ad921e1cde8fb973f39c31d0b3f7f3c77495600f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2021
ms.locfileid: "107948235"
---
# <a name="manage-azure-cosmos-db-cassandra-api-resources-using-azure-resource-manager-templates"></a>Verwalten von Ressourcen für die Cassandra-API von Azure Cosmos DB mithilfe von Azure Resource Manager-Vorlagen
[!INCLUDE[appliesto-cassandra-api](includes/appliesto-cassandra-api.md)]

In diesem Artikel erfahren Sie, wie Sie Vorlagen für Azure Resource Manager verwenden, um Ihre Azure Cosmos DB-Konten, -Keyspaces und -Tabellen bereitzustellen und zu verwalten.

Dieser Artikel enthält nur Beispiele für Cassandra-API-Konten. Beispiele für Konten anderer API-Typen finden Sie in den Artikeln zur Verwendung von Azure Resource Manager-Vorlagen mit der Azure Cosmos DB-API für [SQL](templates-samples-sql.md), [Gremlin](templates-samples-gremlin.md), [MongoDB](templates-samples-mongodb.md) und [Tabellen](templates-samples-table.md).

> [!IMPORTANT]
>
> * Die Länge von Kontonamen ist auf 44 Zeichen (ausschließlich Kleinbuchstaben) beschränkt.
> * Sie müssen die Vorlage mit aktualisierter Angabe der RU/s erneut bereitstellen, um die Durchsatzwerte ändern zu können.
> * Wenn Sie einem Azure Cosmos-Konto Speicherorte hinzufügen oder aus ihm entfernen, können Sie nicht gleichzeitig andere Eigenschaften ändern. Diese Vorgänge müssen separat ausgeführt werden.

Wenn Sie eine der unten aufgeführten Azure Cosmos DB-Ressourcen erstellen möchten, kopieren Sie die folgende Beispielvorlage in eine neue JSON-Datei. Optional können Sie eine JSON-Parameterdatei erstellen, die beim Bereitstellen mehrerer Instanzen derselben Ressource mit unterschiedlichen Namen und Werten verwendet werden soll. Es gibt viele Möglichkeiten, Azure Resource Manager-Vorlagen bereitzustellen, u. a. ist dies über das [Azure-Portal](../azure-resource-manager/templates/deploy-portal.md), die [Azure CLI](../azure-resource-manager/templates/deploy-cli.md), [Azure PowerShell](../azure-resource-manager/templates/deploy-powershell.md) und [GitHub](../azure-resource-manager/templates/deploy-to-azure-button.md) möglich.

<a id="create-autoscale"></a>

## <a name="azure-cosmos-account-for-cassandra-with-autoscale-provisioned-throughput"></a>Azure Cosmos-Konto für Cassandra mit über Autoskalierung bereitgestelltem Durchsatz

Mit dieser Vorlage wird ein Azure Cosmos-Konto in zwei Regionen mit Optionen für Konsistenz und Failover erstellt, wobei ein Keyspace und eine Tabelle für den Autoskalierungsdurchsatz konfiguriert sind. Diese Vorlage ist auch für die Bereitstellung mit nur einem Klick im Katalog der Azure-Schnellstartvorlagen verfügbar.

[:::image type="content" source="../media/template-deployments/deploy-to-azure.svg" alt-text="Bereitstellen in Azure":::](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fquickstarts%2Fmicrosoft.documentdb%2Fcosmosdb-cassandra-autoscale%2Fazuredeploy.json)

:::code language="json" source="~/quickstart-templates/quickstarts/microsoft.documentdb/cosmosdb-cassandra-autoscale/azuredeploy.json":::

<a id="create-manual"></a>

## <a name="azure-cosmos-account-for-cassandra-with-standard-provisioned-throughput"></a>Azure Cosmos-Konto für Cassandra mit standardmäßig bereitgestelltem Durchsatz

Mit dieser Vorlage wird ein Azure Cosmos-Konto in zwei Regionen mit Optionen für Konsistenz und Failover erstellt, wobei ein Keyspace und eine Tabelle für den Standarddurchsatz konfiguriert sind. Diese Vorlage ist auch für die Bereitstellung mit nur einem Klick im Katalog der Azure-Schnellstartvorlagen verfügbar.

[:::image type="content" source="../media/template-deployments/deploy-to-azure.svg" alt-text="Bereitstellen in Azure":::](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fquickstarts%2Fmicrosoft.documentdb%2Fcosmosdb-cassandra%2Fazuredeploy.json)

:::code language="json" source="~/quickstart-templates/quickstarts/microsoft.documentdb/cosmosdb-cassandra/azuredeploy.json":::

## <a name="next-steps"></a>Nächste Schritte

Hier sind einige zusätzlichen Ressourcen:

* [Dokumentation zu Azure Resource Manager](../azure-resource-manager/index.yml)
* [Schema von Azure Cosmos DB-Ressourcenanbietern](/azure/templates/microsoft.documentdb/allversions)
* [Schnellstartvorlagen für Azure Cosmos DB](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.DocumentDB&pageNumber=1&sort=Popular)
* [Beheben gängiger Azure-Bereitstellungsfehler mit Azure Resource Manager](../azure-resource-manager/templates/common-deployment-errors.md)