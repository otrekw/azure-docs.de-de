---
title: Versionshinweise und Ressourcen zu Spring Data Azure Cosmos DB v2 für die SQL-API
description: Hier finden Sie wichtige Informationen zu Spring Data Azure Cosmos DB v2 für die SQL-API, z. B. Veröffentlichungstermine, Deaktivierungstermine und Änderungen an den einzelnen Versionen des Azure Cosmos DB SQL Async Java SDK.
author: anfeldma-ms
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: java
ms.topic: reference
ms.date: 08/12/2020
ms.author: anfeldma
ms.custom: devx-track-java
ms.openlocfilehash: 47d9a8ff884e29dc5692c97d5e7867a856d01063
ms.sourcegitcommit: 02ca0f340a44b7e18acca1351c8e81f3cca4a370
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/19/2020
ms.locfileid: "88590509"
---
# <a name="spring-data-azure-cosmos-db-v2-for-core-sql-api-release-notes-and-resources"></a>Spring Data Azure Cosmos DB v2 für Core-API (SQL): Versionshinweise und Ressourcen
> [!div class="op_single_selector"]
> * [.NET SDK v3](sql-api-sdk-dotnet-standard.md)
> * [.NET SDK v2](sql-api-sdk-dotnet.md)
> * [.NET Core SDK v2](sql-api-sdk-dotnet-core.md)
> * [.NET Change Feed SDK v2](sql-api-sdk-dotnet-changefeed.md)
> * [Node.js](sql-api-sdk-node.md)
> * [Java SDK v4](sql-api-sdk-java-v4.md)
> * [Async Java SDK v2](sql-api-sdk-async-java.md)
> * [Sync Java SDK v2](sql-api-sdk-java.md)
> * [Spring Data v2](sql-api-sdk-java-spring-v2.md)
> * [Spring Data v3](sql-api-sdk-java-spring-v3.md)
> * [Spark-Connector](sql-api-sdk-java-spark.md)
> * [Python](sql-api-sdk-python.md)
> * [REST](/rest/api/cosmos-db/)
> * [REST-Ressourcenanbieter](/rest/api/cosmos-db-resource-provider/)
> * [SQL](sql-api-query-reference.md)
> * [Bulk Executor – .NET v2](sql-api-sdk-bulk-executor-dot-net.md)
> * [Bulk Executor – Java](sql-api-sdk-bulk-executor-java.md)

Mit Spring Data Azure Cosmos DB v2 für Core (SQL) können Entwickler Azure Cosmos DB in Spring-Anwendungen nutzen. Bei Spring Data Azure Cosmos DB wird die Spring Data-Schnittstelle für die Bearbeitung von Datenbanken und Sammlungen, die Verwendung von Dokumenten und die Ausgabe von Abfragen verfügbar gemacht. Für dasselbe Maven-Artefakt werden sowohl synchrone als auch asynchrone (reaktive) APIs unterstützt. 

Das [Spring-Framework](https://spring.io/projects/spring-framework) ist ein Programmier- und Konfigurationsmodell, mit dem die Java-Anwendungsentwicklung optimiert wird. Die Website der Organisation enthält die Information, dass mit Spring die „Verknüpfung“ (Englisch: „plumbing“) von Anwendungen durch Abhängigkeitsinjektion optimiert wird. Spring ist bei vielen Entwicklern beliebt, weil das Erstellen und Testen von Anwendungen damit vereinfacht wird. Mit [Spring Boot](https://spring.io/projects/spring-boot) wird diese „Verknüpfung“ auf die Entwicklung von Webanwendungen und Microservices ausgedehnt. [Spring Data](https://spring.io/projects/spring-data) ist ein Programmiermodell für den Zugriff auf Datenspeicher, z. B. Azure Cosmos DB, im Kontext einer Spring- bzw. Spring Boot-Anwendung. 

Sie können Spring Data Azure Cosmos DB in Ihren [Azure Spring Cloud](https://azure.microsoft.com/services/spring-cloud/)-Anwendungen verwenden.

> [!IMPORTANT]  
> Diese Versionshinweise gelten für v2 von Spring Data Azure Cosmos DB. Die v3-Versionshinweise finden Sie [hier](sql-api-sdk-java-spring-v3.md). 
>
> Spring Data Azure Cosmos DB unterstützt nur die SQL-API.
>
> Die folgenden Leitfäden enthalten Informationen zur Unterstützung von Spring Data für andere Azure Cosmos DB-APIs:
> * [Spring Data für Apache Cassandra mit Azure Cosmos DB](https://docs.microsoft.com/azure/developer/java/spring-framework/configure-spring-data-apache-cassandra-with-cosmos-db)
> * [Spring Data MongoDB mit Azure Cosmos DB](https://docs.microsoft.com/azure/developer/java/spring-framework/configure-spring-data-mongodb-with-cosmos-db)
> * [Spring Data Gremlin Starter mit Azure Cosmos DB](https://docs.microsoft.com/azure/developer/java/spring-framework/configure-spring-data-gremlin-java-app-with-cosmos-db)
>
> Möchten Sie schnell loslegen?
> 1. Installieren Sie die [mindestens unterstützte Java-Runtime (JDK 8)](/java/azure/jdk/?view=azure-java-stable), damit Sie das SDK verwenden können.
> 2. Erstellen Sie eine Spring Data Azure Cosmos DB-App mit dem „Starter“ – [das ist ganz einfach](https://docs.microsoft.com/azure/developer/java/spring-framework/configure-spring-boot-starter-java-app-with-cosmos-db)!
> 3. Arbeiten Sie das [Entwicklerhandbuch zu Spring Data Azure Cosmos DB](https://docs.microsoft.com/azure/developer/java/spring-framework/how-to-guides-spring-data-cosmosdb) durch. Darin werden einfache Azure Cosmos DB-Anforderungen Schritt für Schritt beschrieben.
>
> Sie können Spring Boot Starter-Apps schnell erstellen, indem Sie [Spring Initializr](https://start.spring.io/) nutzen!
>

## <a name="helpful-content"></a>Nützliche Inhalte

| Inhalt | Link |
|---|---|
| **SDK-Download** | [Maven](https://mvnrepository.com/artifact/com.microsoft.azure/spring-data-cosmosdb) |
|**API-Dokumentation** | [Referenzdokumentation zu Spring Data Azure Cosmos DB]() |
|**Am SDK mitwirken** | [Spring Data Azure Cosmos DB-Repository auf GitHub](https://github.com/microsoft/spring-data-cosmosdb) | 
|**Spring Boot Starter**| [Azure Cosmos DB Spring Boot Starter: Clientbibliothek für Java](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/spring/azure-spring-boot-starter-cosmosdb) |
|**Spring-TODO-Beispiel-App mit Azure Cosmos DB**| [End-to-End-Java-Benutzeroberfläche in App Service unter Linux (Teil 2)](https://github.com/Azure-Samples/e2e-java-experience-in-app-service-linux-part-2) |
|**Entwicklerhandbuch** | [Entwicklerhandbuch zu Spring Data Azure Cosmos DB](https://docs.microsoft.com/azure/developer/java/spring-framework/how-to-guides-spring-data-cosmosdb) | 
|**Anleitung zur Verwendung von Starter** | [Verwendung von Spring Boot Starter mit der SQL-API von Azure Cosmos DB](https://docs.microsoft.com/azure/developer/java/spring-framework/configure-spring-boot-starter-java-app-with-cosmos-db) <br> [GitHub-Repository für Azure Spring Boot Starter Cosmos DB](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/spring/azure-spring-boot-starter-cosmosdb) |
|**Beispiel mit App Services** | [Verwenden von Spring und Azure Cosmos DB mit App Service für Linux](https://docs.microsoft.com/azure/developer/java/spring-framework/configure-spring-app-with-cosmos-db-on-app-service-linux) <br> [Beispiel für TODO-App](https://github.com/Azure-Samples/e2e-java-experience-in-app-service-linux-part-2.git) |

## <a name="release-history"></a>Releaseverlauf

### <a name="230-2020-05-21"></a>2.3.0 (21.05.2020)
#### <a name="new-features"></a>Neue Funktionen
* Update der Spring Boot-Version auf 2.3.0 
#### <a name="key-bug-fixes"></a>Wichtige Fehlerbehebungen

### <a name="225-2020-05-19"></a>2.2.5 (19.05.2020)
#### <a name="new-features"></a>Neue Funktionen
* Update von Azure Cosmos DB-Version auf v3.7.3
#### <a name="key-bug-fixes"></a>Wichtige Fehlerbehebungen
* Fehlerbehebungen für Arbeitsspeicherverlust und Upgrades der Netty-Version von Cosmos SDK v3.7.3 

### <a name="224-2020-04-06"></a>2.2.4 (06.04.2020)
#### <a name="new-features"></a>Neue Funktionen
#### <a name="key-bug-fixes"></a>Wichtige Fehlerbehebungen
* Fehlerkorrektur für „allowTelemetry“-Flag zur Berücksichtigung aus CosmosDbConfig
* Fehlerkorrektur der TTL-Eigenschaft für Container

### <a name="223-2020-02-25"></a>2.2.3 (25.02.2020)
#### <a name="new-features"></a>Neue Funktionen
* Neue API für findAll-Funktion nach Partitionsschlüssel hinzugefügt
* Update von azure-cosmos-Version auf 3.7.0
#### <a name="key-bug-fixes"></a>Wichtige Fehlerbehebungen
* Fehlerbehebung: Änderung von collectionName in containerName
* Fehlerbehebung: Änderung von entityClass und domainClass in domainType
* Fehlerbehebung: „Rückgabe der im Repository gespeicherten Entitätssammlung (anstelle der Eingabeentität)“

### <a name="2110-2020-02-25"></a>2.1.10 (25.02.2020)
#### <a name="new-features"></a>Neue Funktionen
#### <a name="key-bug-fixes"></a>Wichtige Fehlerbehebungen
* Zurückportierung für Fehlerbehebung: „Rückgabe der im Repository gespeicherten Entitätssammlung (anstelle der Eingabeentität)“

### <a name="222-2020-01-15"></a>2.2.2 (15.01.2020)
#### <a name="new-features"></a>Neue Funktionen
* Update von azure-cosmos-Version auf v3.6.0
#### <a name="key-bug-fixes"></a>Wichtige Fehlerbehebungen

### <a name="221-2019-12-31"></a>2.2.1 (31.12.2019)
#### <a name="new-features"></a>Neue Funktionen
* Update von Cosmos DB SDK-Version auf 3.5.0
* Anmerkungsfeld zum Aktivieren/Deaktivieren der automatischen Sammlungserstellung hinzugefügt
* Bessere Ausnahmebehandlung, Bereitstellung von CosmosClientException über CosmosDBAccessException
* requestCharge und activityId über ResponseDiagnostics bereitgestellt
#### <a name="key-bug-fixes"></a>Wichtige Fehlerbehebungen
* SDK 3.5.0-Update: Fehlerbehebung für „Ausnahme, wenn Cosmos DB-HTTP-Antwortheader größer als 8.192 Byte ist“ und „Fehler für ConsistencyPolicy.defaultConsistencyLevel() bei begrenzter Veraltung und Präfixkonsistenz“
* Fehlerbehebung für Verhalten von findById-APIs, leere Rückgabe bei „Nicht gefunden“ anstelle der Auslösung einer Ausnahme
* Behebung eines Fehlers, bei dem die Sortierung bei Verwendung von CosmosPageRequest nicht auf die nächste Seite angewendet wurde

### <a name="219-2019-12-26"></a>2.1.9 (26.12.2019)
#### <a name="new-features"></a>Neue Funktionen
* Anmerkungsfeld zum Aktivieren/Deaktivieren der automatischen Sammlungserstellung hinzugefügt
#### <a name="key-bug-fixes"></a>Wichtige Fehlerbehebungen
* Fehlerbehebung für Verhalten von findById-APIs, leere Rückgabe bei „Nicht gefunden“ anstelle der Auslösung einer Ausnahme

### <a name="220-2019-10-21"></a>2.2.0 (21.10.2019)
#### <a name="new-features"></a>Neue Funktionen
* Unterstützung für vollständiges reaktives Cosmos-Repository
* Unterstützung für Cosmos DB-Anforderungsdiagnose-Zeichenfolge und -Abfragemetriken
* Update des Cosmos DB SDK auf Version 3.3.1
* Upgrade der Spring Framework-Version auf 5.2.0.RELEASE
* Upgrade der Spring Data Commons-Version auf 2.2.0.RELEASE
* APIs findByIdAndPartitionKey und deleteByIdAndPartitionKey hinzugefügt
* Abhängigkeit aus azure-documentdb entfernt
* DocumentDb in Cosmos umbenannt
#### <a name="key-bug-fixes"></a>Wichtige Fehlerbehebungen
* Fehlerbehebung: „Bei der Sortierung wird ein Fehler ausgelöst, wenn pageSize kleiner als die Gesamtmenge der Elemente im Repository ist“

### <a name="218-2019-10-18"></a>2.1.8 (18.10.2019)
#### <a name="new-features"></a>Neue Funktionen
* Document DB-APIs als veraltet eingestuft
* APIs findByIdAndPartitionKey und deleteByIdAndPartitionKey hinzugefügt.
* Optimistische Sperre basierend auf _etag hinzugefügt
* SPeL-Ausdruck für Namen der Dokumentsammlung aktiviert
* ObjectMapper-Verbesserungen.
#### <a name="key-bug-fixes"></a>Wichtige Fehlerbehebungen

### <a name="217-2019-10-18"></a>2.1.7 (18.10.2019)
#### <a name="new-features"></a>Neue Funktionen
* Cosmos SDK v3-Abhängigkeit hinzugefügt
* Reaktives Cosmos-Repository hinzugefügt
* Update der Implementierung von DocumentDbTemplate für die Verwendung von Cosmos SDK v3.
* Weitere Konfigurationsänderungen für Unterstützung des reaktiven Cosmos-Repositorys.
#### <a name="key-bug-fixes"></a>Wichtige Fehlerbehebungen

### <a name="212-2019-03-19"></a>2.1.2 (19.03.2019)
#### <a name="new-features"></a>Neue Funktionen
#### <a name="key-bug-fixes"></a>Wichtige Fehlerbehebungen
* Entfernung der applicationInsights-Abhängigkeit für
    * Potenzielles Risiko einer Verunreinigung von Abhängigkeiten
    * Java 11-Inkompatibilität
    * Vermeidung potenzieller Leistungsbeeinträchtigungen für CPU bzw. Arbeitsspeicher.

### <a name="207-2019-03-20"></a>2.0.7 (20.03.2019)
#### <a name="new-features"></a>Neue Funktionen
#### <a name="key-bug-fixes"></a>Wichtige Fehlerbehebungen
* Entfernung der applicationInsights-Abhängigkeit durch Zurückportierung für
    * Potenzielles Risiko einer Verunreinigung von Abhängigkeiten
    * Java 11-Inkompatibilität
    * Vermeidung potenzieller Leistungsbeeinträchtigungen für CPU bzw. Arbeitsspeicher.

### <a name="211-2019-03-07"></a>2.1.1 (07.03.2019)
#### <a name="new-features"></a>Neue Funktionen
* Update der Masterversion auf 2.1.1
#### <a name="key-bug-fixes"></a>Wichtige Fehlerbehebungen

### <a name="206-2019-03-07"></a>2.0.6 (07.03.2019)
#### <a name="new-features"></a>Neue Funktionen
* Ignorierung aller Ausnahmen der Telemetrie
#### <a name="key-bug-fixes"></a>Wichtige Fehlerbehebungen

### <a name="210-2018-12-17"></a>2.1.0 (17.12.2018)
#### <a name="new-features"></a>Neue Funktionen
* Update der Version auf 2.1.0 zur Problembehebung
#### <a name="key-bug-fixes"></a>Wichtige Fehlerbehebungen

### <a name="205-2018-09-13"></a>2.0.5 (13.09.2018)
#### <a name="new-features"></a>Neue Funktionen
* Schlüsselwörter „exists“, „startsWith“ hinzugefügt
* Update der Infodatei
#### <a name="key-bug-fixes"></a>Wichtige Fehlerbehebungen
* Fehlerbehebung: „Direkter Aufruf von self href für Entität nicht möglich“
* Fehlerhebung: „Fehler für findAll, wenn Sammlung nicht erstellt wird“

### <a name="204-pre-release-2018-08-23"></a>2.0.4 (Vorabversion) (23.08.2018)
#### <a name="new-features"></a>Neue Funktionen
* Umbenennung eines Pakets aus documentdb in cosmosdb,
* Neues Feature für Schlüsselwort für Abfragemethode hinzugefügt, Unterstützung von 16 Schlüsselwörtern über SQL-API.
* Neues Feature für Abfragen mit Seitenverwaltung und Sortierung hinzugefügt.
* Konfiguration von spring-data-cosmosdb vereinfacht.
* APIs für deleteCollection und deleteAll hinzugefügt.

#### <a name="key-bug-fixes"></a>Wichtige Fehlerbehebungen
* Fehlerkorrektur und -behebung.

## <a name="faq"></a>Häufig gestellte Fragen
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Cosmos DB finden Sie auf der Seite zum Dienst [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).

Weitere Informationen zum Spring-Framework finden Sie auf der [Homepage des Projekts](https://spring.io/projects/spring-framework).

Weitere Informationen zu Spring Boot finden Sie auf der [Homepage des Projekts](https://spring.io/projects/spring-boot).

Weitere Informationen zu Spring Data finden Sie auf der [Homepage des Projekts](https://spring.io/projects/spring-data).