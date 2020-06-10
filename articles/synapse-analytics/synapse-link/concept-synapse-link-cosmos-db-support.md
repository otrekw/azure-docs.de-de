---
title: Von Azure Synapse Link (Vorschau) unterstützte Features für Azure Cosmos DB
description: Aktuelle Liste der Aktionen, die von Azure Synapse Link für Azure Cosmos DB unterstützt werden
services: synapse-analytics
author: ArnoMicrosoft
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: ''
ms.date: 04/21/2020
ms.author: acomet
ms.reviewer: jrasnick
ms.openlocfilehash: 9c90bc9527bffff55d7478150cc1f31827bacc46
ms.sourcegitcommit: f1132db5c8ad5a0f2193d751e341e1cd31989854
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/31/2020
ms.locfileid: "84235285"
---
# <a name="azure-synapse-link-preview-for-azure-cosmos-db-supported-features"></a>Von Azure Synapse Link (Vorschau) unterstützte Features für Azure Cosmos DB

In diesem Artikel werden die Funktionen beschrieben, die derzeit in Azure Synapse Link für Azure Cosmos DB unterstützt werden.

## <a name="azure-synapse-support"></a>Azure Synapse-Unterstützung

In Azure Cosmos DB gibt es zwei Arten von Containern:
* HTAP-Container: ein Container mit Synapse Link-Aktivierung. Dieser Container verfügt sowohl über Transaktionsspeicher als auch über Analysespeicher. 
* OLTP-Container: ein Container nur mit Transaktionsspeicher. Synapse Link ist nicht aktiviert. 

> [!IMPORTANT]
> Azure Synapse Link für Azure Cosmos DB wird derzeit für Arbeitsbereiche unterstützt, in denen kein verwaltetes virtuelles Netzwerk aktiviert ist. 

Sie können eine Verbindung mit einem Azure Cosmos DB-Container herstellen, ohne Synapse Link zu aktivieren. In diesem Fall können Sie im Transaktionsspeicher nur Lese-/Schreibvorgänge ausführen. Nachfolgend sehen Sie eine Liste der derzeit unterstützten Features in Synapse Link für Azure Cosmos DB. 

| Category              | BESCHREIBUNG |[Spark](https://docs.microsoft.com/azure/synapse-analytics/sql/on-demand-workspace-overview) | [SQL (serverlos)](https://docs.microsoft.com/azure/synapse-analytics/sql/on-demand-workspace-overview) |
| -------------------- | ----------------------------------------------------------- |----------------------------------------------------------- | ----------------------------------------------------------- | ----------------------------------------------------------- |
| **Laufzeitunterstützung** |Unterstützung für Lese-oder Schreibvorgänge durch die Azure Synapse-Laufzeit| ✓ | [Kontaktaufnahme](mailto:AskSynapse@microsoft.com?subject=[Enable%20Preview%20Feature]%20SQL%20serverless%20for%20Cosmos%20DB)|
| **Azure Cosmos DB-API-Unterstützung** |API-Unterstützung als Synapse Link-Instanz| SQL/MongoDB | SQL/MongoDB |
| **Object**  |Objekte wie eine Tabelle, die erstellt werden können und direkt auf den Azure Cosmos DB-Container verweisen| Anzeigen, Tabelle | Sicht |
| **Lesen**    |Lesen von Daten aus einem Azure Cosmos DB-Container| OLTP/HTAP | HTAP  |
| **Schreiben**   |Schreiben von Daten aus der Laufzeit in einen Azure Cosmos DB-Container| OLTP | – |

* Wenn Sie Daten aus Spark in einen Azure Cosmos DB-Container schreiben, erfolgt dieser Vorgang über den Transaktionsspeicher von Azure Cosmos DB und wirkt sich durch den Verbrauch von Anforderungseinheiten auf die Transaktionsleistung von Azure Cosmos DB aus.
* Die Integration von SQL-Pools über externe Tabellen wird derzeit nicht unterstützt.

## <a name="supported-code-generated-actions-for-spark"></a>Unterstützte, durch Code generierte Aktionen für Spark

| Geste              | BESCHREIBUNG |OLTP |HTAP  |
| -------------------- | ----------------------------------------------------------- |----------------------------------------------------------- |----------------------------------------------------------- |
| **In Datenrahmen laden** |Laden und Lesen von Daten in einen Spark-Datenrahmen |X| ✓ |
| **Spark-Tabelle erstellen** |Erstellen einer Tabelle, die auf einen Azure Cosmos DB-Container verweist|X| ✓ |
| **Datenrahmen in Container schreiben** |Schreiben von Dateien in einen Container|✓| ✓ |
| **Streamingdatenrahmen aus Container laden** |Streamen von Daten mithilfe des Azure Cosmos DB-Änderungsfeeds|✓| ✓ |
| **Streamingdatenrahmen in Container schreiben** |Streamen von Daten mithilfe des Azure Cosmos DB-Änderungsfeeds|✓| ✓ |



## <a name="supported-code-generated-actions-for-sql-serverless"></a>Unterstützte, durch Code generierte Aktionen für SQL serverlos

| Geste              | BESCHREIBUNG |OLTP |HTAP |
| -------------------- | ----------------------------------------------------------- |----------------------------------------------------------- |----------------------------------------------------------- |
| **Top 100 auswählen** |Vorschau der 100 obersten Elemente aus einem Container anzeigen|X| ✓ |
| **Ansicht erstellen** |Erstellen einer Ansicht, damit BI über Synapse SQL direkt Zugriff in einem Container hat|X| ✓ |

## <a name="next-steps"></a>Nächste Schritte

* [Informationen zum Herstellen einer Verbindung mit Synapse Link für Azure Cosmos DB](../quickstart-connect-synapse-link-cosmos-db.md)
* [Informationen zum Abfragen des Analysespeichers mit Spark](how-to-query-analytical-store-spark.md)