---
title: Was ist Azure Synapse Analytics (früher SQL DW)?
description: Azure Synapse Analytics (früher SQL DW) ist ein unbegrenzter Analysedienst, der Data Warehousing für Unternehmen mit Big Data-Analysen vereint.
services: sql-data-warehouse
author: mlee3gsd
manager: craigg
ms.service: sql-data-warehouse
ms.topic: overview
ms.subservice: design
ms.date: 11/04/2019
ms.author: martinle
ms.reviewer: igorstan
ms.openlocfilehash: 68d39b4f363794d50fd05c2067502fc55d5d0170
ms.sourcegitcommit: 812bc3c318f513cefc5b767de8754a6da888befc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2020
ms.locfileid: "77153246"
---
# <a name="what-is-azure-synapse-analytics-formerly-sql-dw"></a>Was ist Azure Synapse Analytics (früher SQL DW)?

Azure Synapse ist ein unbegrenzter Analysedienst, der Data Warehousing für Unternehmen mit Big Data-Analysen vereint. Er ermöglicht flexible Datenabfragen nach Ihren Vorstellungen, indem serverlose On-Demand-Ressourcen oder bereitgestellten Ressourcen im gewünschten Umfang verwendet werden. Azure Synapse vereint diese beiden Ansätze in einer vereinheitlichten Oberfläche für die Erfassung, Aufbereitung, Verwaltung und Verarbeitung von Daten für dringende BI- und Machine Learning-Anforderungen.

Azure Synapse besteht aus vier Komponenten:
- SQL-Analyse: Umfassende T-SQL-basierte Analyse – allgemein verfügbar
    - SQL-Pool (Bezahlung pro bereitgestellter DWU) 
    - SQL On-Demand (Bezahlung pro verarbeitetem TB) – Vorschau
- Spark: Fest integriertes Apache Spark (Vorschau) 
- Datenintegration: Hybrid-Datenintegration (Vorschau)
- Studio: Einheitliche Benutzeroberfläche.  (Vorschau)

> [!NOTE]
> Fordern Sie [hier](https://aka.ms/synapsepreview) den Zugriff an, um auf die Previewfunktionen von Azure Synapse zugreifen zu können. Microsoft verarbeitet alle Anforderungen und antwortet so schnell wie möglich.

## <a name="sql-analytics-and-sql-pool-in-azure-synapse"></a>SQL-Analyse und SQL-Pool in Azure Synapse

SQL Analytics bezieht sich auf die Data Warehousing-Features für Unternehmen, die in Azure Synapse allgemein zur Verfügung stehen. 

Der SQL-Pool ist eine Sammlung von Analyseressourcen, die bei Verwendung der SQL-Analyse bereitgestellt werden. Die Größe des SQL-Pools wird durch die Data Warehouse-Einheiten (DWUs) bestimmt.

Führen Sie den Import großer Datenmengen mit einfachen [PolyBase](/sql/relational-databases/polybase/polybase-guide?view=sql-server-2017&viewFallbackFrom=azure-sqldw-latest)-T-SQL-Abfragen durch, und nutzen Sie anschließend die Vorteile von MPP für Hochleistungsanalysen. Bei der Integration und Analyse wird die SQL-Analyse für Ihr Unternehmen somit zur alleinigen zuverlässigen Quelle für die Gewinnung schneller und besser fundierter Erkenntnisse.  

## <a name="key-component-of-a-big-data-solution"></a>Schlüsselkomponente einer Big Data-Lösung

Data Warehousing ist eine Schlüsselkomponente einer cloudbasierten Komplettlösung für Big Data.

![Data Warehouse-Lösung](media/sql-data-warehouse-overview-what-is/data-warehouse-solution.png) 

In einer Clouddatenlösung werden Daten aus verschiedensten Quellen in Big Data-Speichern erfasst. Nach der Speicherung in einem Big Data-Speicher werden die Daten durch Hadoop, Spark und Machine Learning-Algorithmen vorbereitet und trainiert. Wenn die Daten für komplexe Analysen bereit sind, fragt die SQL-Analyse die Big Data-Speicher mithilfe von PolyBase ab. PolyBase verwendet T-SQL-Standardabfragen, um die Daten an Tabellen der SQL-Analyse zu übermitteln.
 
Bei der SQL-Analyse werden Daten in relationalen Tabellen mit Speicherung in Spalten vorgehalten. Dieses Format zeichnet sich durch erheblich geringere Datenspeicherkosten und eine bessere Abfrageleistung aus. Nachdem die Daten gespeichert wurden, können Sie Analysen in großem Umfang durchführen. Im Vergleich zu herkömmlichen Datenbanksystemen dauern Analyseabfragen nur noch Sekunden statt Minuten (oder Stunden statt Tage). 

Die Analyseergebnisse können an Berichtsdatenbanken oder Anwendungen auf der ganzen Welt übermittelt werden. Business Analysts können so Erkenntnisse gewinnen und fundierte Geschäftsentscheidungen treffen.

## <a name="next-steps"></a>Nächste Schritte

- Erkunden Sie die [Architektur von Azure Synapse](massively-parallel-processing-mpp-architecture.md).
- Führen Sie eine schnelle [Erstellung eines SQL-Pools](create-data-warehouse-portal.md) durch.
- [Laden von Beispieldaten](sql-data-warehouse-load-sample-databases.md)
- Ansehen von [Videos](https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse)

Oder sehen Sie sich einige der folgenden weiteren Azure Synapse-Ressourcen an:  
* Suche in [Blogs](https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/)
* Übermitteln von [Funktionsanfragen](https://feedback.azure.com/forums/307516-sql-data-warehouse)
* [Erstellen eines Supporttickets](sql-data-warehouse-get-started-create-support-ticket.md)
* Suche im [MSDN-Forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureSQLDataWarehouse)
* Suche im [Stack Overflow-Forum](https://stackoverflow.com/questions/tagged/azure-sqldw)