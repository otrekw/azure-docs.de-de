---
title: Versionshinweise und Ressourcen zu Azure Cosmos DB Java SDK v4 für die SQL-API
description: Hier finden Sie wichtige Informationen zum Azure Cosmos DB Java SDK v4 für die SQL-API und das SDK, einschließlich Veröffentlichungsterminen, Deaktivierungsterminen und Änderungen an den einzelnen Versionen des Azure Cosmos DB SQL Async Java SDK.
author: anfeldma-ms
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: java
ms.topic: reference
ms.date: 05/20/2020
ms.author: anfeldma
ms.openlocfilehash: ccc872b24c78fbdf6e55673f9d1f78efc0647895
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86537883"
---
# <a name="azure-cosmos-db-java-sdk-v4-for-core-sql-api-release-notes-and-resources"></a>Versionshinweise und Ressourcen zu Azure Cosmos DB Java SDK v4 für die Core (SQL)-API
> [!div class="op_single_selector"]
> * [.NET SDK v3](sql-api-sdk-dotnet-standard.md)
> * [.NET SDK v2](sql-api-sdk-dotnet.md)
> * [.NET Core SDK v2](sql-api-sdk-dotnet-core.md)
> * [.NET Change Feed SDK v2](sql-api-sdk-dotnet-changefeed.md)
> * [Node.js](sql-api-sdk-node.md)
> * [Java SDK v4](sql-api-sdk-java-v4.md)
> * [Async Java SDK v2](sql-api-sdk-async-java.md)
> * [Sync Java SDK v2](sql-api-sdk-java.md)
> * [Python](sql-api-sdk-python.md)
> * [REST](/rest/api/cosmos-db/)
> * [REST-Ressourcenanbieter](/rest/api/cosmos-db-resource-provider/)
> * [SQL](sql-api-query-reference.md)
> * [Bulk Executor – .NET v2](sql-api-sdk-bulk-executor-dot-net.md)
> * [Bulk Executor – Java](sql-api-sdk-bulk-executor-java.md)

Das Azure Cosmos DB Java SDK v4 für Core (SQL) kombiniert eine asynchrone API und eine synchrone API in ein Maven-Artefakt. Das SDK v4 bietet verbesserte Leistung, neue API-Features und asynchrone Unterstützung basierend auf Project Reactor und der [Netty-Bibliothek](https://netty.io/). Benutzer können gegenüber [Azure Cosmos DB Async Java SDK v2](sql-api-sdk-async-java.md) und [Azure Cosmos DB Sync Java SDK v2](sql-api-sdk-java.md) eine bessere Leistung mit Azure Cosmos DB Java SDK v4 erwarten.

> [!IMPORTANT]  
> Diese Versionshinweise gelten nur für Azure Cosmos DB Java SDK v4. Wenn Sie aktuell eine ältere Version als v4 verwenden, lesen Sie den Leitfaden [Migrieren zum Azure Cosmos DB Java SDK v4](migrate-java-v4-sdk.md), um Hilfe beim Upgrade auf v4 zu erhalten.
>
> Mit den folgenden drei Schritten können Sie schnell einsteigen.
> 1. Installieren Sie die [mindestens unterstützte Java-Runtime (JDK 8)](/java/azure/jdk/?view=azure-java-stable), damit Sie das SDK verwenden können.
> 2. Durchlaufen Sie die [Schnellstartanleitung für Azure Cosmos DB Java SDK v4](https://docs.microsoft.com/azure/cosmos-db/create-sql-api-java), um Zugriff auf das Maven-Artefakt zu erhalten und die Verwendung grundlegender Azure Cosmos DB-Anforderungen zu erlernen.
> 3. Lesen Sie die Leitfaden mit [Leistungstipps](performance-tips-java-sdk-v4-sql.md) und zur [Problembehandlung](troubleshoot-java-sdk-v4-sql.md) für Azure Cosmos DB Java SDK v4, um das SDK für Ihre Anwendung zu optimieren.
>
> Die [Azure Cosmos DB-Workshops und -Labs](https://aka.ms/cosmosworkshop) sind eine weitere hervorragende Ressource, um die Verwendung des Azure Cosmos DB Java SDK v4 zu erlernen!
>

| |  |
|---|---|
| **SDK-Download** | [Maven](https://mvnrepository.com/artifact/com.azure/azure-cosmos) |
|**API-Dokumentation** | [Java-API-Referenzdokumentation](https://docs.microsoft.com/java/api/overview/azure/cosmosdb/client?view=azure-java-stable) |
|**Am SDK mitwirken** | [Azure SDK für das zentrale Java-Repository auf GitHub](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/cosmos) | 
|**Erste Schritte** | [Schnellstart: Erstellen einer Java-App zum Verwalten von Daten der Azure Cosmos DB SQL-API](https://docs.microsoft.com/azure/cosmos-db/create-sql-api-java) [GitHub-Repository mit Schnellstartcode](https://github.com/Azure-Samples/azure-cosmos-java-getting-started) | 
|**Einfache Codebeispiele** | [Azure Cosmos DB: Java-Beispiele für die SQL-API](sql-api-java-sdk-samples.md) [GitHub-Repository mit Beispielcode](https://github.com/Azure-Samples/azure-cosmos-java-sql-api-samples)|
|**Konsolen-App mit Änderungsfeed**| [Änderungsfeed: Java SDK v4-Beispiel](create-sql-api-java-changefeed.md) [GitHub-Repository mit Beispielcode](https://github.com/Azure-Samples/azure-cosmos-java-sql-app-example)| 
|**Web-App-Beispiel**| [Erstellen einer Web-App mit dem Java SDK v4](sql-api-java-application.md) [GitHub-Repository mit Beispielcode](https://github.com/Azure-Samples/azure-cosmos-java-sql-api-todo-app)|
| **Leistungstipps**| [Leistungstipps für das Java SDK v4](performance-tips-java-sdk-v4-sql.md)| 
| **Problembehandlung** | [Problembehandlung für das Java SDK v4](troubleshoot-java-sdk-v4-sql.md) |
| **Migrieren zu v4 von einem älteren SDK** | [Migrieren zum Java SDK v4](migrate-java-v4-sdk.md) |
| **Unterstützte Mindestlaufzeit**|[JDK 8](/java/azure/jdk/?view=azure-java-stable) | 
| **Azure Cosmos DB-Workshops und -Labs** |[Homepage der Cosmos DB-Workshops](https://aka.ms/cosmosworkshop)

## <a name="release-history"></a>Releaseverlauf

### <a name="420-2020-07-14"></a>4.2.0 (2020-07-14)
* Die API für die Skriptprotokollierung wurde `CosmosStoredProcedureRequestOptions` hinzugefügt.
* Für `DirectConnectionConfig` wurde die Standardeinstellung von `idleEndpointTimeout` auf „1 Stunde“ und die Standardeinstellung von `connectTimeout` auf „5 Sekunden“ aktualisiert.
#### <a name="key-bug-fixes"></a>Wichtige Fehlerbehebungen
* Es wurde ein Problem behoben, bei dem `DirectConnectionConfig` `idleConnectionTimeout` von `GatewayConnectionConfig` `idleConnectionTimeout` außer Kraft gesetzt wurde.
* Die APIs für das Abrufen und Festlegen für `responseContinuationTokenLimitInKb` wurden in `CosmosQueryRequestOptions` korrigiert.
* Es wurde ein Problem im Abfrage- und Änderungsfeed behoben, das beim Neuerstellen der Sammlung gleichen Namens aufgetreten ist.
* Es wurde ein Problem behoben, bei dem für die oberste Abfrage „ClassCastException“ ausgelöst wurde.
* Es wurde ein Problem behoben, bei dem für die Abfrage „NullPointerException“ ausgelöst wurde.
* Es wurde ein Problem mit der Verarbeitung von abgebrochenen Anforderungen im direkten Modus behoben, bei dem der Reaktor `onErrorDropped` aufgerufen wurde. 

### <a name="410-2020-06-25"></a>4.1.0 (2020-06-25)
#### <a name="new-features"></a>Neue Funktionen
* Unterstützung für `GROUP BY`-Abfrage wurde hinzugefügt.
* Standardwert von „maxConnectionsPerEndpoint“ wurde in „DirectConnectionConfig“ auf 130 erhöht.
* Standardwert von „maxRequestsPerConnection“ wurde in „DirectConnectionConfig“ auf 30 erhöht.
#### <a name="key-bug-fixes"></a>Wichtige Fehlerbehebungen
* Probleme mit ORDER BY-Abfrage wurden behoben, die beim Fortsetzen mit dem Fortsetzungstoken doppelte Ergebnisse zurückgab. 
* Probleme mit VALUE-Abfrage wurden behoben, die für geschachtelte Objekte NULL-Werte zurückgab.
* NULL-Zeigerausnahme beim Anforderungs-Manager in „RntbdClientChannelPool“ wurde behoben.

### <a name="401-2020-06-10"></a>4.0.1 (2020-06-10)
#### <a name="new-features"></a>Neue Funktionen
* `QueryRequestOptions` wurde in `CosmosQueryRequestOptions` umbenannt.
* `ChangeFeedProcessorBuilder` wurde auf Generatormuster aktualisiert.
* `CosmosPermissionProperties` wurde mit neuen APIs für Containername und untergeordnete Ressourcen aktualisiert.
* Weitere Beispiele und erweiterte Dokumente wurden zu `CosmosClientBuilder` hinzugefügt. 
* `CosmosDatabase`- und `CosmosContainer`-APIs wurden mit Durchsatzeigenschaften zur Unterstützung von Autoskalierung/Autopilot aktualisiert. 
* `CosmosClientException` wurde in `CosmosException` umbenannt. 
* `AccessCondition` und `AccessConditionType` wurden durch `ifMatchETag()`- und `ifNoneMatchETag()`-APIs ersetzt. 
* Alle `Cosmos*AsyncResponse`- und `CosmosResponse`-Typen wurden zu einem einzelnen `CosmosResponse`-Typ zusammengeführt.
* `CosmosResponseDiagnostics` wurde in `CosmosDiagnostics` umbenannt.  
* `FeedResponseDiagnostics` wurde in `CosmosDiagnostics` eingeschlossen. 
* `jackson`-Abhängigkeit wurde aus „azure-cosmos“ und Verwendung von „azure-core“ entfernt. 
* `CosmosKeyCredential` wurde durch `AzureKeyCredential`-Typ ersetzt. 
* `ProxyOptions`-APIs wurden zu `GatewayConnectionConfig` hinzugefügt. 
* SDK wurde auf Verwendung des `Instant`-Typs anstelle von `OffsetDateTime` aktualisiert. 
* Neuer Enumerationstyp `OperationKind` wurde hinzugefügt. 
* `FeedOptions` wurde in `QueryRequestOptions` umbenannt. 
* `getETag()`- und `getTimestamp()`-APIs wurden zu `Cosmos*Properties`-Typen hinzugefügt. 
* `userAgent`-Informationen wurden in `CosmosException` und `CosmosDiagnostics` hinzugefügt. 
* Zeichen für neue Zeile in `Diagnostics` wurde auf Zeichen für neue Zeile des Systems aktualisiert. 
* `readAll*`-APIs wurden entfernt, stattdessen wird eine Abfrage mit „Alle APIs auswählen“ verwendet.
* API zum Schätzen der Verzögerung für `ChangeFeedProcessor` wurde hinzugefügt.   
* Unterstützung für Durchsatzbereitstellung für Autoskalierung/Autopilot wurde zum SDK hinzugefügt.  
* `ConnectionPolicy` wurde durch neue Verbindungskonfigurationen ersetzt. `DirectConnectionConfig`- und `GatewayConnectionConfig`-APIs wurden über `CosmosClientBuilder` für Verbindungskonfigurationen im direkten Modus und Gatewaymodus verfügbar gemacht.
* `JsonSerializable` und `Resource` wurden in das Implementierungspaket verschoben. 
* `contentResponseOnWriteEnabled`-API wurde zu „CosmosClientBuilder“ hinzugefügt, wodurch vollständiger Antwortinhalt bei Schreibvorgängen deaktiviert wird.
* `getETag()`-APIs wurden für Antworttypen verfügbar gemacht.
* `CosmosAuthorizationTokenResolver` wurde in die Implementierung verschoben. 
* `preferredLocations`- und `multipleWriteLocations`-APIs wurden in  &  und `preferredRegions` umbenannt. 
* `reactor-core` wurde auf 3.3.5.RELEASE, `reactor-netty` auf 0.9.7.RELEASE und `netty` auf 4.1.49.Final aktualisiert. 
* Unterstützung für `analyticalStoreTimeToLive` wurde im SDK hinzugefügt.     
* `CosmosClientException` erweitert `AzureException`. 
* `maxItemCount`- und `requestContinuationToken`-APIs wurden aus `FeedOptions` entfernt, stattdessen werden `byPage()`-APIs aus `CosmosPagedFlux` und `CosmosPagedIterable` verwendet.
* `CosmosPermissionProperties` wurde auf der öffentlichen Oberfläche für `Permission`-APIs eingeführt.
* `SqlParameterList`-Typ wurde entfernt und durch `List` ersetzt.
* Mehrere Speicherverluste beim Direct TCP-Client wurden behoben. 
* Unterstützung für `DISTINCT`-Abfragen wurde hinzugefügt. 
* Externe Abhängigkeiten von `fasterxml.uuid, guava, commons-io, commons-collection4, commons-text` wurden entfernt.  
* `CosmosPagedFlux` und `CosmosPagedIterable` wurden in `utils`-Paket verschoben. 
* Netty wurde auf 4.1.45.Final und Project Reactor auf Version 3.3.3 aktualisiert.
* Öffentliche REST-Verträge wurden auf `Final`-Klassen aktualisiert.
* Unterstützung für erweiterte Diagnose für Punktvorgänge wurde hinzugefügt.
* Paket wurde auf `com.azure.cosmos` aktualisiert.
* `models`-Paket wurde für Modell-/REST-Verträge hinzugefügt.
* `utils`-Paket wurde für `CosmosPagedFlux`- und `CosmosPagedIterable`-Typen hinzugefügt. 
* Öffentliche APIs wurden auf die Verwendung von `Duration` im gesamten SDK aktualisiert.
* Alle REST-Verträge wurden dem `models`-Paket hinzugefügt.
* `RetryOptions` wurde in `ThrottlingRetryOptions` umbenannt.
* Paginierungstypen `CosmosPagedFlux` und `CosmosPagedIterable` wurden für Abfrage-APIs hinzugefügt. 
* Unterstützung für Freigabe von TransportClient für mehrere Instanzen von CosmosClients wurde mithilfe einer neuen API in `CosmosClientBuilder#connectionSharingAcrossClientsEnabled(true)` hinzugefügt.
* Abfrageoptimierungen durch Entfernen doppelter Serialisierung/Deserialisierung. 
* Optimierungen der Antwortheader durch Entfernen von unnötigem Hin- und Herkopieren. 
* Optimierte `ByteBuffer`-Serialisierung/Deserialisierung durch Entfernen von Zwischeninstanziierungen der Zeichenfolge.
#### <a name="key-bug-fixes"></a>Wichtige Fehlerbehebungen
* NULL-Zeigerausnahme bei `toString()` in „ConnectionPolicy“ wurde behoben.
* Problem beim Analysieren von Abfrageergebnisse im Fall von ORDER BY-Abfragen für VALUE wurde behoben. 
* Probleme mit Socketverlusten beim Direct TCP-Client wurden behoben.
* `orderByQuery` mit Fehler beim Fortsetzungstoken wurde behoben.
* `ChangeFeedProcessor`-Fehlerbehebung für die Verarbeitung von Partitionsteilungen und wenn die Partition nicht gefunden wird.
* `ChangeFeedProcessor`-Fehlerbehebung beim Synchronisieren von Lease-Updates für verschiedene Threads.
* Racebedingung behoben, die eine `ArrayIndexOutOfBound`-Ausnahme in StoreReader verursachte.

## <a name="faq"></a>Häufig gestellte Fragen
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Weitere Informationen
Weitere Informationen zu Cosmos DB finden Sie auf der Seite zum Dienst [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).