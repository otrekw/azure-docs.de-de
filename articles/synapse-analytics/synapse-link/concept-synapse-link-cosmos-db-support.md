---
title: Von Azure Synapse Link (Vorschau) unterstützte Features für Azure Cosmos DB
description: Aktuelle Liste der Aktionen, die von Azure Synapse Link für Azure Cosmos DB unterstützt werden
services: synapse-analytics
author: ArnoMicrosoft
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: synapse-link
ms.date: 09/15/2020
ms.author: acomet
ms.reviewer: jrasnick
ms.openlocfilehash: 336409b8b6f804b224b87d5fb11fded0654b8619
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90895533"
---
# <a name="azure-synapse-link-preview-for-azure-cosmos-db-supported-features"></a>Von Azure Synapse Link (Vorschau) unterstützte Features für Azure Cosmos DB

In diesem Artikel werden die Funktionen beschrieben, die derzeit in Azure Synapse Link für Azure Cosmos DB unterstützt werden.

## <a name="azure-synapse-support"></a>Azure Synapse-Unterstützung

In Azure Cosmos DB gibt es zwei Arten von Containern:
* HTAP-Container: ein Container mit Synapse Link-Aktivierung. Dieser Container verfügt sowohl über Transaktionsspeicher als auch über Analysespeicher. 
* OLTP-Container: Ein Container mit nicht aktiviertem Synaspe Link. Dieser Container verfügt nur über Transaktionsspeicher und nicht über Analysespeicher.

> [!IMPORTANT]
> Azure Synapse Link für Azure Cosmos DB wird derzeit für Synapse-Arbeitsbereiche unterstützt, in denen kein verwaltetes virtuelles Netzwerk aktiviert ist. 

Sie können eine Verbindung mit einem Azure Cosmos DB-Container herstellen, ohne Synapse Link zu aktivieren. In diesem Fall können Sie im Transaktionsspeicher nur Lese-/Schreibvorgänge ausführen. Nachfolgend sehen Sie die Liste der derzeit unterstützten Features in Synapse Link für Azure Cosmos DB. 

| Category              | BESCHREIBUNG |[Spark](https://docs.microsoft.com/azure/synapse-analytics/sql/on-demand-workspace-overview) | [SQL (serverlos)](https://docs.microsoft.com/azure/synapse-analytics/sql/on-demand-workspace-overview) |
| -------------------- | ----------------------------------------------------------- |----------------------------------------------------------- | ----------------------------------------------------------- |
| **Laufzeitunterstützung** |Unterstützte Azure Synapse-Runtime für den Zugriff auf Azure Cosmos DB| ✓ | [Kontaktaufnahme](mailto:cosmosdbsynapselink@microsoft.com?subject=[Enable%20Preview%20Feature]%20SQL%20serverless%20for%20Cosmos%20DB) |
| **Azure Cosmos DB-API-Unterstützung** | Unterstützte Azure Cosmos DB-API-Art | SQL/MongoDB | SQL/MongoDB |
| **Object**  |Objekte wie eine Tabelle, die erstellt werden können und direkt auf den Azure Cosmos DB-Container verweisen| Datenrahmen, Ansicht, Tabelle | Sicht |
| **Lesen**    | Typ des Azure Cosmos DB-Containers, der gelesen werden kann | OLTP/HTAP | HTAP  |
| **Schreiben**   | Kann die Azure Synapse-Runtime verwendet werden, um Daten in einen Azure Cosmos DB-Container zu schreiben? | Ja | Nein |

* Wenn Sie Daten aus Spark in einen Azure Cosmos DB-Container schreiben, erfolgt dieser Vorgang über den Transaktionsspeicher von Azure Cosmos DB und wirkt sich auf die Leistung der Transaktionsworkloads von Azure Cosmos DB und den Verbrauch von Anforderungseinheiten aus.
* Die Integration von Synapse SQL-Pools über externe Tabellen wird derzeit nicht unterstützt.

## <a name="supported-code-generated-actions-for-spark"></a>Unterstützte, durch Code generierte Aktionen für Spark

| Geste              | BESCHREIBUNG |OLTP |HTAP  |
| -------------------- | ----------------------------------------------------------- |----------------------------------------------------------- |----------------------------------------------------------- |
| **In Datenrahmen laden** |Laden und Lesen von Daten in einen Spark-Datenrahmen |✓| ✓ |
| **Spark-Tabelle erstellen** |Erstellen einer Tabelle, die auf einen Azure Cosmos DB-Container verweist|✓| ✓ |
| **Datenrahmen in Container schreiben** |Schreiben von Dateien in einen Container|✓| ✓ |
| **Streamingdatenrahmen aus Container laden** |Streamen von Daten mithilfe des Azure Cosmos DB-Änderungsfeeds|✓| ✓ |
| **Streamingdatenrahmen in Container schreiben** |Streamen von Daten mithilfe des Azure Cosmos DB-Änderungsfeeds|✓| ✓ |



## <a name="supported-code-generated-actions-for-sql-serverless"></a>Unterstützte, durch Code generierte Aktionen für SQL serverlos

| Geste              | BESCHREIBUNG |OLTP |HTAP |
| -------------------- | ----------------------------------------------------------- |----------------------------------------------------------- |----------------------------------------------------------- |
| **Durchsuchen von Daten** |Durchsuchen von Daten aus einem Container mit vertrauter T-SQL-Syntax und automatischem Schemarückschluss|X| ✓ |
| **Erstellen von Sichten und Erstellen von BI-Berichten** |Erstellen einer SQL-Sicht für den direkten Zugriff auf einen Container für BI über Synapse SQL (serverlos) |X| ✓ |
| **Verknüpfen von unterschiedlichen Datenquellen mit Cosmos DB-Daten** | Speichern von Abfrageergebnissen beim Lesen von Daten aus Cosmos DB-Containern zusammen mit Daten in Azure Blob Storage oder Azure Data Lake Storage mithilfe von CETAS |X| ✓ |

## <a name="next-steps"></a>Nächste Schritte

* [Informationen zum Herstellen einer Verbindung mit Synapse Link für Azure Cosmos DB](../quickstart-connect-synapse-link-cosmos-db.md)
* [Informationen zum Abfragen des Analysespeichers mit Spark](how-to-query-analytical-store-spark.md)
