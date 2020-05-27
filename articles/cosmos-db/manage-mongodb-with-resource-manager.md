---
title: Resource Manager-Vorlagen für die Azure Cosmos DB-API für MongoDB
description: Verwenden Sie Azure Resource Manager-Vorlagen zum Erstellen und Konfigurieren der MongoDB-API von Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/19/2020
ms.author: mjbrown
ms.openlocfilehash: 1fe2c96e3c469e41e12d64f27a6a3b3d16a1174d
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2020
ms.locfileid: "83684779"
---
# <a name="manage-azure-cosmos-db-mongodb-api-resources-using-azure-resource-manager-templates"></a>Verwalten von Ressourcen für die MongoDB-API von Azure Cosmos DB mithilfe von Azure Resource Manager-Vorlagen

In diesem Artikel erfahren Sie, wie Sie Ihre Konten, Datenbanken und Sammlungen für die Azure Cosmos DB für MongoDB-API mithilfe von Azure Resource Manager-Vorlagen bereitstellen und verwalten.

Dieser Artikel enthält Beispiele nur für Azure Cosmos DB-API-Konten für MongoDB. Beispiele für Konten anderer API-Typen finden Sie in den Artikeln zur Verwendung von Azure Resource Manager-Vorlagen mit der Azure Cosmos DB-API für [Cassandra](manage-cassandra-with-resource-manager.md), [Gremlin](manage-gremlin-with-resource-manager.md), [SQL](manage-sql-with-resource-manager.md) und [Tabellen](manage-table-with-resource-manager.md).

> [!IMPORTANT]
>
> * Die Länge von Kontonamen ist auf 44 Zeichen (ausschließlich Kleinbuchstaben) beschränkt.
> * Sie müssen die Vorlage mit aktualisierter Angabe der RU/s erneut übermitteln, um die Durchsatzwerte zu ändern.
> * Wenn Sie einem Azure Cosmos-Konto Speicherorte hinzufügen oder aus ihm entfernen, können Sie nicht gleichzeitig andere Eigenschaften ändern. Diese Vorgänge müssen separat ausgeführt werden.

Wenn Sie eine der unten aufgeführten Azure Cosmos DB-Ressourcen erstellen möchten, kopieren Sie die folgende Beispielvorlage in eine neue JSON-Datei. Optional können Sie eine JSON-Parameterdatei erstellen, die beim Bereitstellen mehrerer Instanzen derselben Ressource mit unterschiedlichen Namen und Werten verwendet werden soll. Für das Bereitstellen von Azure Resource Manager-Vorlagen stehen Ihnen viele Möglichkeiten zur Verfügung, beispielsweise das [Azure-Portal](../azure-resource-manager/templates/deploy-portal.md), die [Azure-Befehlszeilenschnittstelle](../azure-resource-manager/templates/deploy-cli.md), [Azure PowerShell](../azure-resource-manager/templates/deploy-powershell.md) und [GitHub](../azure-resource-manager/templates/deploy-to-azure-button.md).

<a id="create-autoscale"></a>

## <a name="azure-cosmos-account-for-mongodb-with-autoscale-provisioned-throughput"></a>Azure Cosmos-Konto für MongoDB mit per Autoskalierung bereitgestelltem Durchsatz

Mit dieser Vorlage wird ein Azure Cosmos-Konto für die MongoDB-API (3.2 oder 3.6) mit zwei Sammlungen erstellt, die sich auf Datenbankebene einen per Autoskalierung bereitgestellten Durchsatz teilen. Diese Vorlage ist auch für die Bereitstellung mit nur einem Klick im Katalog der Azure-Schnellstartvorlagen verfügbar.

[![In Azure bereitstellen](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-cosmosdb-mongodb-autoscale%2Fazuredeploy.json)

:::code language="json" source="~/quickstart-templates/101-cosmosdb-mongodb-autoscale/azuredeploy.json":::

<a id="create-manual"></a>

## <a name="azure-cosmos-account-for-mongodb-with-standard-provisioned-throughput"></a>Azure Cosmos-Konto für MongoDB mit standardmäßig bereitgestelltem Durchsatz

Mit dieser Vorlage wird ein Azure Cosmos-Konto für die MongoDB-API (3.2 oder 3.6) mit zwei Sammlungen erstellt, die sich auf Datenbankebene einen (manuell bereitgestellten) Durchsatz von standardmäßig 400 RU/s teilen. Diese Vorlage ist auch für die Bereitstellung mit nur einem Klick im Katalog der Azure-Schnellstartvorlagen verfügbar.

[![In Azure bereitstellen](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-cosmosdb-mongodb%2Fazuredeploy.json)

:::code language="json" source="~/quickstart-templates/101-cosmosdb-mongodb/azuredeploy.json":::

## <a name="next-steps"></a>Nächste Schritte

Hier sind einige zusätzlichen Ressourcen:

* [Dokumentation zu Azure Resource Manager](/azure/azure-resource-manager/)
* [Schema von Azure Cosmos DB-Ressourcenanbietern](/azure/templates/microsoft.documentdb/allversions)
* [Schnellstartvorlagen für Azure Cosmos DB](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.DocumentDB&pageNumber=1&sort=Popular)
* [Beheben gängiger Azure-Bereitstellungsfehler mit Azure Resource Manager](../azure-resource-manager/templates/common-deployment-errors.md)
