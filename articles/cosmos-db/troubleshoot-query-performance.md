---
title: Behandeln von Problemen bei Verwendung von Azure Cosmos DB
description: Erfahren Sie, wie Sie Probleme mit SQL-Abfragen in Azure Cosmos DB identifizieren, diagnostizieren und beheben.
author: ginamr
ms.service: cosmos-db
ms.topic: troubleshooting
ms.date: 07/10/2019
ms.author: girobins
ms.subservice: cosmosdb-sql
ms.reviewer: sngun
ms.openlocfilehash: 1859fa8f71b5c4c44d6e5da1b6a36ca9d9399516
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75444721"
---
# <a name="troubleshoot-query-performance-for-azure-cosmos-db"></a>Behandlung von Problemen mit der Abfrageleistung für Azure Cosmos DB
In diesem Artikel erfahren Sie, wie Sie Probleme mit SQL-Abfragen in Azure Cosmos DB identifizieren, diagnostizieren und beheben. Führen Sie die folgenden Schritte zur Problembehandlung aus, um eine optimale Leistung für Azure Cosmos DB-Abfragen zu erzielen. 

## <a name="collocate-clients-in-same-azure-region"></a>Anordnen von Clients in derselben Azure-Region 
Die geringste Latenz erzielen Sie, wenn sich die aufrufende Anwendung in der gleichen Azure-Region wie der bereitgestellte Azure Cosmos DB-Endpunkt befindet. Eine Liste mit den verfügbaren Regionen finden Sie im Artikel [Azure-Regionen](https://azure.microsoft.com/global-infrastructure/regions/#services).

## <a name="check-consistency-level"></a>Überprüfen der Konsistenzebene
Die [Konsistenzebene](consistency-levels.md) kann sich auf die Leistung und Gebühren auswirken. Stellen Sie sicher, dass die Konsistenzebene für das jeweilige Szenario geeignet ist. Weitere Informationen finden Sie unter [Auswählen der Konsistenzebene](consistency-levels-choosing.md).

## <a name="log-the-executed-sql-query"></a>Protokollieren der ausgeführten SQL-Abfrage 

Sie können die ausgeführte SQL-Abfrage in einem Speicherkonto oder in der Diagnoseprotokolltabelle protokollieren. [SQL-Abfrageprotokolle über Diagnoseprotokolle](cosmosdb-monitor-resource-logs.md) ermöglichen die Protokollierung der verschleierten Abfrage in einem Speicherkonto Ihrer Wahl. So können Sie die Protokolle prüfen und die Abfrage ermitteln, die höhere RUs nutzt. Später können Sie die die eigentliche Abfrage in QueryRuntimeStatistics anhand der Aktivitäts-ID zuordnen. Die Abfrage wird aus Sicherheitsgründen verschleiert, und die Namen und Werte der Abfrageparameter in WHERE-Klauseln unterscheiden sich von den tatsächlichen Namen und Werten. Sie können die Protokollierung für das Speicherkonto verwenden, um die Langzeitaufbewahrung der ausgeführten Abfragen beizubehalten.  

## <a name="log-query-metrics"></a>Metrik zur Protokollabfrage

Verwenden Sie `QueryMetrics`, um Probleme mit langsamen oder teuren Abfragen zu beheben. 

  * Legen Sie `FeedOptions.PopulateQueryMetrics = true` so fest, dass `QueryMetrics` in der Antwort enthalten ist.
  * Die `QueryMetrics`-Klasse verfügt über eine überladene `.ToString()`-Funktion, die aufgerufen werden kann, um die Zeichenfolgendarstellung von `QueryMetrics` zu erhalten. 
  * Die Metriken können verwendet werden, um u.a. die folgenden Erkenntnisse zu gewinnen: 
  
      * Ob die Ausführung einer bestimmten Komponente der Abfragepipeline ungewöhnlich lange gedauert hat (in der Größenordnung von Hunderten von Millisekunden oder mehr). 

          * Sehen Sie sich `TotalExecutionTime` an.
          * Wenn das `TotalExecutionTime`-Element der Abfrage kleiner als die End-to-End-Ausführungszeit ist, dann wird die Zeit auf der Clientseite oder im Netzwerk aufgewendet. Überprüfen Sie noch einmal, ob der Client und die Azure-Region angeordnet sind.
      
      * Ob es in den analysierten Dokumenten falsch positive Ergebnisse gab (wenn die Anzahl der Ausgabedokumente viel geringer ist als die Anzahl der abgerufenen Dokumente).  

          * Sehen Sie sich `Index Utilization` an.
          * `Index Utilization` = (Anzahl der zurückgegebenen Dokumente / Anzahl der geladenen Dokumente)
          * Wenn die Anzahl der zurückgegebenen Dokumente wesentlich kleiner ist als die Anzahl der geladenen, werden falsch positive Ergebnisse analysiert.
          * Begrenzen Sie die Anzahl der abgerufenen Dokumente mit engeren Filtern.  

      * Wie die einzelnen Roundtrips verliefen (siehe `Partition Execution Timeline` aus der Zeichenfolgendarstellung von `QueryMetrics`). 
      * Ob die Abfrage eine hohe Gebühr für die Anforderung in Anspruch genommen hat. 

Weitere Informationen finden Sie im Artikel [Abrufen von SQL-Abfrageausführungsmetriken und Analysieren der Abfrageleistung mit dem .NET SDK](profile-sql-api-query.md).
      
## <a name="tune-query-feed-options-parameters"></a>Optimieren von Feedoptionen für Abfrageparameter 
Die Abfrageleistung kann über die [Feedoptionen](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.feedoptions?view=azure-dotnet)-Parameter der Anforderung optimiert werden. Versuchen Sie, die folgenden Optionen einzustellen:

  * Legen Sie zuerst `MaxDegreeOfParallelism` auf „-1“ fest, und vergleichen Sie dann die Leistung für verschiedene Werte. 
  * Legen Sie zuerst `MaxBufferedItemCount` auf „-1“ fest, und vergleichen Sie dann die Leistung für verschiedene Werte. 
  * Legen Sie `MaxItemCount` auf „-1“ fest.

Verwenden Sie Werte wie 2, 4, 8, 16 und andere, wenn Sie die Leistung verschiedener Werte vergleichen.
 
## <a name="read-all-results-from-continuations"></a>Lesen aller Ergebnisse aus Fortsetzungen
Wenn Sie der Meinung sind, dass Sie nicht alle Ergebnisse erhalten, sollten Sie sicherstellen, dass die Fortsetzung vollständig ausgeglichen wird. Anders ausgedrückt: Setzen Sie das Lesen der Ergebnisse fort, solange das Fortsetzungstoken noch über weitere Dokumente verfügt.

Der vollständige Ausgleich wird mit einem der folgenden Muster erreicht:

  * Setzen Sie die Verarbeitung der Ergebnisse fort, solange die Fortsetzung nicht leer ist.
  * Setzen Sie die Verarbeitung fort, wenn die Abfrage noch mehr Ergebnisse liefert. 

    ```csharp
    // using AsDocumentQuery you get access to whether or not the query HasMoreResults
    // If it does, just call ExecuteNextAsync until there are no more results
    // No need to supply a continuation token here as the server keeps track of progress
    var query = client.CreateDocumentQuery<Family>(collectionLink, options).AsDocumentQuery();
    while (query.HasMoreResults)
    {
        foreach (Family family in await query.ExecuteNextAsync())
        {
            families.Add(family);
        }
    }
    ```

## <a name="choose-system-functions-that-utilize-index"></a>Auswahl von Systemfunktionen, die den Index verwenden
Wenn der Ausdruck in einen Bereich von Zeichenfolgenwerten übersetzt werden kann, kann der Index genutzt werden. Andernfalls ist dies nicht möglich. 

Hier ist die Liste mit den Zeichenfolgenfunktionen angegeben, die den Index verwenden können: 
    
  * STARTSWITH(str_expr, str_expr) 
  * LEFT(str_expr, num_expr) = str_expr 
  * SUBSTRING(str_expr, num_expr, num_expr) = str_expr, aber nur, wenn der erste „num_expr“-Wert „0“ ist 
    
    Nachstehend finden Sie einige Abfragebeispiele: 
    
    ```sql

    -- If there is a range index on r.name, STARTSWITH will utilize the index while ENDSWITH won't 
    SELECT * 
    FROM c 
    WHERE STARTSWITH(c.name, 'J') AND ENDSWITH(c.name, 'n')

    ```
    
    ```sql

    -- LEFT will utilize the index while RIGHT won't 
    SELECT * 
    FROM c 
    WHERE LEFT(c.name, 2) = 'Jo' AND RIGHT(c.name, 2) = 'hn'

    ```

  * Vermeiden Sie Systemfunktionen im Filter (oder in der WHERE-Klausel), die nicht vom Index bedient werden. Einige Beispiele für solche Systemfunktionen sind „Contains“, „Upper“, „Lower“.
  * Schreiben Sie nach Möglichkeit Abfragen, um für einen Partitionsschlüssel einen Filter zu verwenden.
  * Um leistungsfähige Abfragen zu erhalten, sollten Sie den Aufruf von UPPER/LOWER im Filter vermeiden. Normalisieren Sie stattdessen die Groß- und Kleinschreibung der Werte beim Einfügen. Geben Sie für jeden der Werte den Wert in der gewünschten Schreibweise ein oder geben Sie sowohl den ursprünglichen Wert als auch den Wert in der gewünschten Schreibweise ein. 

    Beispiel:
    
    ```sql

    SELECT * FROM c WHERE UPPER(c.name) = "JOE"

    ```
    
    In diesem Fall wird „JOE“ großgeschrieben oder es wird sowohl der ursprüngliche Wert „Joe“ als auch „JOE“ gespeichert. 
    
    Wenn die Schreibweise der JSON-Daten normalisiert ist, sieht die Abfrage wie folgt aus:
    
    ```sql

    SELECT * FROM c WHERE c.name = "JOE"

    ```

    Die zweite Abfrage ist leistungsfähiger, da es nicht erforderlich ist, jeden einzelnen Wert umzuwandeln, um die Werte mit „JOE“ zu vergleichen.

Weitere Details zu Systemfunktionen finden Sie im Artikel [Systemfunktionen](sql-query-system-functions.md).

## <a name="check-indexing-policy"></a>Überprüfen der Indizierungsrichtlinie
Überprüfen Sie wie folgt, ob die aktuelle [Indizierungsrichtlinie](index-policy.md) optimal ist:

  * Stellen Sie sicher, dass alle in den Abfragen verwendeten JSON-Pfade in der Indizierungsrichtlinie enthalten sind, um schnellere Lesevorgänge zu erzielen.
  * Schließen Sie Pfade aus, die nicht in Abfragen verwendet werden, um die Leistung für die Schreibvorgänge zu erhöhen.

Weitere Informationen finden Sie im Artikel [Verwalten der Indizierungsrichtlinie](how-to-manage-indexing-policy.md).

## <a name="spatial-data-check-ordering-of-points"></a>Räumliche Daten: Überprüfen der Reihenfolge von Punkten
Punkte innerhalb eines Polygons müssen gegen den Uhrzeigersinn nacheinander angegeben werden. Ein Polygon, das im Uhrzeigersinn angegeben wird, stellt die Umkehrung der darin enthaltenen Region dar.

## <a name="optimize-join-expressions"></a>Optimieren von JOIN-Ausdrücken
`JOIN`-Ausdrücke können zu großen Kreuzprodukten erweitert werden. Fragen Sie nach Möglichkeit einen kleineren Suchbereich über einen engeren Filter ab.

Mehrwertige Unterabfragen können `JOIN`-Ausdrücke optimieren, indem Prädikate nach jedem „Select-Many“-Ausdruck statt nach allen Kreuzprodukten in der `WHERE`-Klausel gepusht werden. Ein ausführliches Beispiel finden Sie im Artikel [Optimieren von JOIN-Ausdrücken](https://docs.microsoft.com/azure/cosmos-db/sql-query-subquery#optimize-join-expressions).

## <a name="optimize-order-by-expressions"></a>Optimieren von ORDER BY-Ausdrücken 
Die Abfrageleistung von `ORDER BY` kann beeinträchtigt werden, wenn die Felder eine geringe Dichte aufweisen oder nicht in der Indexrichtlinie enthalten sind.

  * Für Felder mit einer geringen Dichte, wie z.B. Zeit, verringern Sie den Suchbereich so weit wie möglich, indem Sie Filter einsetzen. 
  * Fügen Sie für eine einzelne `ORDER BY`-Eigenschaft diese in die Indexrichtlinien ein. 
  * Definieren Sie für mehrere `ORDER BY`-Eigenschaftsausdrücke einen [zusammengesetzten Index](https://docs.microsoft.com/azure/cosmos-db/index-policy#composite-indexes) für Felder, die sortiert werden sollen.  

## <a name="many-large-documents-being-loaded-and-processed"></a>Laden und Verarbeiten vieler großer Dokumente
Die Zeit und die RUs, die für eine Abfrage benötigt werden, sind nicht nur von der Größe der Antwort abhängig, sondern auch von der Leistung, die von der Abfrageverarbeitungspipeline erbracht wird. Zeitaufwand und RUs steigen proportional zum Arbeitsaufwand der gesamten Abfrageverarbeitungspipeline an. Für große Dokumente ist auch die Arbeitslast größer, weshalb für diese mehr Zeit und RUs für das Laden und Verarbeiten benötigt werden.

## <a name="low-provisioned-throughput"></a>Geringer bereitgestellter Durchsatz
Stellen Sie sicher, dass der bereitgestellte Durchsatz die anfallenden Workloads verarbeiten kann. Erhöhen Sie das RU-Budget für betroffene Sammlungen.

## <a name="try-upgrading-to-the-latest-sdk-version"></a>Upgrade auf die neuste SDK-Version
Informationen zum Ermitteln des neuesten SDK finden Sie im Artikel [Azure Cosmos DB .NET SDK für SQL-API: Download und Anmerkungen zum Release](sql-api-sdk-dotnet.md).

## <a name="next-steps"></a>Nächste Schritte
Die unten angegebenen Dokumente enthalten u.a. Informationen dazu, wie Sie RUs pro Abfrage messen und Ausführungsstatistiken zum Optimieren Ihrer Abfragen erhalten:

* [Abrufen von SQL-Abfrageausführungsmetriken und Analysieren der Abfrageleistung mit dem .NET SDK](profile-sql-api-query.md)
* [Optimieren der Abfrageleistung mit Azure Cosmos DB](sql-api-sql-query-metrics.md)
* [Tipps zur Leistungssteigerung für das .NET SDK](performance-tips.md)
