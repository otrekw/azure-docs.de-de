---
title: Azure Resource Manager-Vorlagen für Azure Cosmos DB
description: Verwenden Sie Azure Resource Manager-Vorlagen zum Erstellen und Konfigurieren von Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/14/2020
ms.author: mjbrown
ms.openlocfilehash: 1c4f1a1920d98052231eaa3a7d5c0454441c88ee
ms.sourcegitcommit: d6e4eebf663df8adf8efe07deabdc3586616d1e4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/15/2020
ms.locfileid: "81390877"
---
# <a name="azure-resource-manager-templates-for-azure-cosmos-db"></a>Azure Resource Manager-Vorlagen für Azure Cosmos DB

Die folgenden Tabellen enthalten Links zu Azure Resource Manager-Vorlagen für Azure Cosmos DB:

## <a name="sql-core-api"></a>SQL-API (Core-API)

|**Vorlage**|**Beschreibung**|
|---|---|
|[Erstellen eines Kontos, einer Datenbank und eines Containers in Azure Cosmos](manage-sql-with-resource-manager.md#create-resource) | Diese Vorlage erstellt ein SQL (Core) API-Konto in zwei Regionen mit zwei Containern mit gemeinsamem Datenbankdurchsatz und einem Container mit dediziertem Durchsatz. Der Durchsatz kann aktualisiert werden, indem die Vorlage mit dem aktualisierten Eigenschaftswert für den Durchsatz erneut übermittelt wird. |
|[Erstellen eines Azure Cosmos-Kontos, einer Datenbank und eines Containers mit einer gespeicherten Prozedur, einem Trigger und einer UDF](manage-sql-with-resource-manager.md#create-sproc) | Diese Vorlage erstellt ein SQL(Core) API-Konto in zwei Regionen mit einer gespeicherten Prozedur, einem Trigger und einer UDF für einen Container. |
|[Erstellen eines privaten Endpunkts für ein vorhandenes Azure Cosmos-Konto](how-to-configure-private-endpoints.md#create-a-private-endpoint-by-using-a-resource-manager-template) |  Mit dieser Vorlage wird ein privater Endpunkt für ein vorhandenes Azure Cosmos-SQL-API-Konto in einem vorhandenen virtuellen Netzwerk erstellt. |

## <a name="mongodb-api"></a>MongoDB-API

|**Vorlage**|**Beschreibung**|
|---| ---|
|[Erstellen eines Kontos, einer Datenbank und einer Sammlung in Azure Cosmos](manage-mongodb-with-resource-manager.md#create-resource) | Diese Vorlage erstellt mithilfe der Azure Cosmos DB-API für MongoDB ein Konto in zwei Regionen mit aktivierter Multimasterunterstützung. Das Azure Cosmos-Konto enthält zwei Container, die den Durchsatz auf Datenbankebene gemeinsam nutzen. |

## <a name="cassandra-api"></a>Cassandra-API

|**Vorlage**|**Beschreibung**|
|---| ---|
|[Erstellen eines Kontos, eines Keyspace und einer Tabelle in Azure Cosmos](manage-cassandra-with-resource-manager.md#create-resource) | Diese Vorlage erstellt ein Cassandra-API-Konto in zwei Regionen mit aktivierter Multimasterunterstützung. Das Azure Cosmos-Konto enthält zwei Tabellen, die den Durchsatz auf Schlüsselbereichsebene gemeinsam nutzen. |

## <a name="gremlin-api"></a>Gremlin-API

|**Vorlage**|**Beschreibung**|
|---| ---|
|[Erstellen eines Kontos, einer Datenbank und eines Graphen in Azure Cosmos](manage-gremlin-with-resource-manager.md#create-resource) | Diese Vorlage erstellt ein Gremlin-API-Konto in zwei Regionen mit aktivierter Multimasterunterstützung. Das Azure Cosmos-Konto enthält zwei Graphen, die den Durchsatz auf Datenbankebene gemeinsam nutzen. |

## <a name="table-api"></a>Tabelle-API

|**Vorlage**|**Beschreibung**|
|---| ---|
|[Erstellen eines Kontos und einer Tabelle in Azure Cosmos](manage-table-with-resource-manager.md#create-resource) | Diese Vorlage erstellt ein Tabellen-API-Konto in zwei Regionen mit aktivierter Multimasterunterstützung. Das Azure Cosmos-Konto enthält eine einzelne Tabelle. |

> [!TIP]
> Um bei Verwendung der Tabellen-API die gemeinsame Nutzung des Durchsatzes zu aktivieren, aktivieren Sie im Azure-Portal den Durchsatz auf Kontoebene.

Auf der Seite [Azure Resource Manager-Referenz für Azure Cosmos DB](/azure/templates/microsoft.documentdb/allversions) finden Sie die Referenzdokumentation.
