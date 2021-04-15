---
title: 'Azure Resource Manager-Vorlagen für Azure Cosmos DB: Core-API (SQL)'
description: Verwenden Sie Azure Resource Manager-Vorlagen zum Erstellen und Konfigurieren von Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 03/24/2021
ms.author: mjbrown
ms.openlocfilehash: 7163658024d150a7c5d75c3b3ac0b6b6b29cd3cb
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105037307"
---
# <a name="azure-resource-manager-templates-for-azure-cosmos-db"></a>Azure Resource Manager-Vorlagen für Azure Cosmos DB
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

Dieser Artikel enthält nur Azure Resource Manager-Vorlagenbeispiele für Core-API-Konten (SQL). Es gibt jedoch auch Vorlagenbeispiele für [Cassandra-](templates-samples-cassandra.md)-, [Gremlin](templates-samples-gremlin.md)-, [MongoDB](templates-samples-mongodb.md)- und [Table](templates-samples-table.md)-APIs.

## <a name="core-sql-api"></a>Core-API (SQL)

|**Vorlage**|**Beschreibung**|
|---|---|
|[Erstellen eines Azure Cosmos-Kontos, einer Datenbank und eines Containers mit automatisch skaliertem Durchsatz](manage-with-templates.md#create-autoscale) | Mit dieser Vorlage werden ein Core-API-Konto (SQL) in zwei Regionen, eine Datenbank und ein Container mit automatisch skaliertem Durchsatz erstellt. |
|[Erstellen eines Azure Cosmos-Kontos, einer Datenbank und eines Containers mit Analysespeicher](manage-with-templates.md#create-analytical-store) | Mit dieser Vorlage wird ein Konto für die Core-API (SQL) in einer Region mit einem Container erstellt, für den die analytische Gültigkeitsdauer aktiviert ist, und es besteht die Möglichkeit, die manuelle oder automatische Skalierung des Durchsatzes zu verwenden. |
|[Erstellen eines Azure Cosmos-Kontos, einer Datenbank und eines Containers mit (manuell bereitgestelltem) Standarddurchsatz](manage-with-templates.md#create-manual) | Mit dieser Vorlage werden ein Core-API-Konto (SQL) in zwei Regionen, eine Datenbank und ein Container mit Standarddurchsatz erstellt. |
|[Erstellen eines Azure Cosmos-Kontos, einer Datenbank und eines Containers mit einer gespeicherten Prozedur, einem Trigger und einer UDF](manage-with-templates.md#create-sproc) | Mit dieser Vorlage werden ein Core-API-Konto (SQL) in zwei Regionen mit einer gespeicherten Prozedur, einem Trigger und einer UDF für einen Container erstellt. |
|[Erstellen eines Azure Cosmos-Kontos mit Azure AD Identität, Rollendefinitionen und Rollenzuweisung](manage-with-templates.md#create-rbac) | Mit dieser Vorlage wird ein Core (SQL)-API-Konto mit Azure AD Identität, Rollendefinitionen und Rollenzuweisung für einen Dienstprinzipal erstellt. |
|[Erstellen eines privaten Endpunkts für ein vorhandenes Azure Cosmos-Konto](how-to-configure-private-endpoints.md#create-a-private-endpoint-by-using-a-resource-manager-template) |  Mit dieser Vorlage wird ein privater Endpunkt für ein vorhandenes Azure Cosmos-Core-API-Konto (SQL) in einem vorhandenen virtuellen Netzwerk erstellt. |
|[Erstellen eines Azure Cosmos-Kontos im Free-Tarif](manage-with-templates.md#free-tier) |  Mit dieser Vorlage wird ein Azure Cosmos DB-Core-API-Konto (SQL) im Free-Tarif erstellt. |

Auf der Seite [Azure Resource Manager-Referenz für Azure Cosmos DB](/azure/templates/microsoft.documentdb/allversions) finden Sie die Referenzdokumentation.
