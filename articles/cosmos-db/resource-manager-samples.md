---
title: Azure Resource Manager-Vorlagen für Azure Cosmos DB
description: Verwenden Sie Azure Resource Manager-Vorlagen zum Erstellen und Konfigurieren von Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/08/2020
ms.author: mjbrown
ms.openlocfilehash: a98aee61c3a330344b5b7fa0a5f7f53a65e5ecdb
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83586149"
---
# <a name="azure-resource-manager-templates-for-azure-cosmos-db"></a>Azure Resource Manager-Vorlagen für Azure Cosmos DB

Die folgenden Tabellen enthalten Links zu Azure Resource Manager-Vorlagen für Azure Cosmos DB:

## <a name="core-sql-api"></a>Core-API (SQL)

|**Vorlage**|**Beschreibung**|
|---|---|
|[Erstellen eines Azure Cosmos-Kontos, einer Datenbank und eines Containers mit automatisch skaliertem Durchsatz](manage-sql-with-resource-manager.md#create-autoscale) | Mit dieser Vorlage werden ein Core-API-Konto (SQL) in zwei Regionen, eine Datenbank und ein Container mit automatisch skaliertem Durchsatz erstellt. |
|[Erstellen eines Azure Cosmos-Kontos, einer Datenbank und eines Containers mit Analysespeicher](manage-sql-with-resource-manager.md#create-analytical-store) | Mit dieser Vorlage wird ein Konto für die Core-API (SQL) in einer Region mit einem Container erstellt, für den die analytische Gültigkeitsdauer aktiviert ist, und es besteht die Möglichkeit, die manuelle oder automatische Skalierung des Durchsatzes zu verwenden. |
|[Erstellen eines Azure Cosmos-Kontos, einer Datenbank und eines Containers mit (manuell bereitgestelltem) Standarddurchsatz](manage-sql-with-resource-manager.md#create-manual) | Mit dieser Vorlage werden ein Core-API-Konto (SQL) in zwei Regionen, eine Datenbank und ein Container mit Standarddurchsatz erstellt. |
|[Erstellen eines Azure Cosmos-Kontos, einer Datenbank und eines Containers mit einer gespeicherten Prozedur, einem Trigger und einer UDF](manage-sql-with-resource-manager.md#create-sproc) | Mit dieser Vorlage werden ein Core-API-Konto (SQL) in zwei Regionen mit einer gespeicherten Prozedur, einem Trigger und einer UDF für einen Container erstellt. |
|[Erstellen eines privaten Endpunkts für ein vorhandenes Azure Cosmos-Konto](how-to-configure-private-endpoints.md#create-a-private-endpoint-by-using-a-resource-manager-template) |  Mit dieser Vorlage wird ein privater Endpunkt für ein vorhandenes Azure Cosmos-Core-API-Konto (SQL) in einem vorhandenen virtuellen Netzwerk erstellt. |
|[Erstellen eines Azure Cosmos-Kontos im Free-Tarif](manage-sql-with-resource-manager.md#free-tier) |  Mit dieser Vorlage wird ein Azure Cosmos DB-Core-API-Konto (SQL) im Free-Tarif erstellt. |

## <a name="mongodb-api"></a>MongoDB-API

|**Vorlage**|**Beschreibung**|
|---| ---|
|[Erstellen eines Azure Cosmos-Kontos, einer Datenbank und einer Sammlung mit automatisch skaliertem Durchsatz](manage-mongodb-with-resource-manager.md#create-autoscale) | Mit dieser Vorlage wird ein Konto mit Azure Cosmos DB-API für MongoDB in zwei Regionen mit zwei Containern erstellt, die automatisch skalierten Durchsatz auf Datenbankebene gemeinsam nutzen. |
|[Erstellen eines Azure Cosmos-Kontos, einer Datenbank und einer Sammlung mit (manuell bereitgestelltem) Standarddurchsatz](manage-mongodb-with-resource-manager.md#create-manual) | Mit dieser Vorlage wird ein Konto mit Azure Cosmos DB-API für MongoDB in zwei Regionen mit zwei Containern erstellt, die den Standarddurchsatz auf Datenbankebene gemeinsam nutzen. |

## <a name="cassandra-api"></a>Cassandra-API

|**Vorlage**|**Beschreibung**|
|---| ---|
|[Erstellen eines Azure Cosmos-Kontos, eines Keyspace und einer Tabelle mit automatisch skaliertem Durchsatz](manage-cassandra-with-resource-manager.md#create-autoscale) | Mit dieser Vorlage wird ein Cassandra-API-Konto in zwei Regionen mit einem Keyspace und einer Tabelle mit automatisch skaliertem Durchsatz erstellt. |
|[Erstellen eines Azure Cosmos-Kontos, eines Keyspace und einer Tabelle mit (manuell bereitgestelltem) Standarddurchsatz](manage-cassandra-with-resource-manager.md#create-manual) | Mit dieser Vorlage wird ein Cassandra-API-Konto in zwei Regionen mit einem Keyspace und einer Tabelle mit manuellem Durchsatz erstellt. |

## <a name="gremlin-api"></a>Gremlin-API

|**Vorlage**|**Beschreibung**|
|---| ---|
|[Erstellen eines Azure Cosmos-Kontos, einer Datenbank und eines Graphen mit automatisch skaliertem Durchsatz](manage-gremlin-with-resource-manager.md#create-autoscale) | Mit dieser Vorlage wird ein Gremlin-API-Konto in zwei Regionen mit einer Datenbank und einem Graphen mit automatisch skaliertem Durchsatz erstellt. |
|[Erstellen eines Azure Cosmos-Kontos, einer Datenbank und eines Graphen mit (manuell bereitgestelltem) Standarddurchsatz](manage-gremlin-with-resource-manager.md#create-manual) | Mit dieser Vorlage wird ein Gremlin-API-Konto in zwei Regionen mit einer Datenbank und einem Graphen mit Standarddurchsatz erstellt. |

## <a name="table-api"></a>Tabelle-API

|**Vorlage**|**Beschreibung**|
|---| ---|
|[Erstellen eines Azure Cosmos-Kontos und einer Tabelle mit automatisch skaliertem Durchsatz](manage-table-with-resource-manager.md#create-autoscale) | Mit dieser Vorlage werden ein Tabellen-API-Konto in zwei Regionen und eine Tabelle mit automatisch skaliertem Durchsatz erstellt. |
|[Erstellen eines Azure Cosmos-Kontos und einer Tabelle mit (manuell bereitgestelltem) Standarddurchsatz](manage-table-with-resource-manager.md#create-manual) | Mit dieser Vorlage werden ein Tabellen-API-Konto in zwei Regionen und eine Tabelle mit Standarddurchsatz erstellt. |

Auf der Seite [Azure Resource Manager-Referenz für Azure Cosmos DB](/azure/templates/microsoft.documentdb/allversions) finden Sie die Referenzdokumentation.
