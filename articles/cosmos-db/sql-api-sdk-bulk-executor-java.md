---
title: 'Azure Cosmos DB: Java-API, SDK und Ressourcen für BulkExecutor'
description: Wichtige Informationen zur Java-API und zum SDK für BulkExecutor, einschließlich Veröffentlichungsdaten, Deaktivierungsdaten und Änderungen an den einzelnen Versionen des Azure Cosmos DB-BulkExecutor-Java-SDK.
author: anfeldma-ms
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: java
ms.topic: reference
ms.date: 05/11/2020
ms.author: anfeldma
ms.openlocfilehash: 6fe075d2679ec13217200bc8b0da132e8f4d9b57
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83660441"
---
# <a name="java-bulk-executor-library-download-information"></a>Java-BulkExecutor-Bibliothek: Informationen zum Download

> [!div class="op_single_selector"]
> * [.NET](sql-api-sdk-dotnet.md)
> * [.NET-Änderungsfeed](sql-api-sdk-dotnet-changefeed.md)
> * [.NET Core](sql-api-sdk-dotnet-core.md)
> * [Node.js](sql-api-sdk-node.md)
> * [Java SDK v4](sql-api-sdk-java-v4.md)
> * [Async Java SDK v2](sql-api-sdk-async-java.md)
> * [Sync Java SDK v2](sql-api-sdk-java.md)
> * [Python](sql-api-sdk-python.md)
> * [REST](https://docs.microsoft.com/rest/api/cosmos-db/)
> * [REST-Ressourcenanbieter](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/)
> * [SQL](sql-api-query-reference.md)
> * [Bulk Executor – .NET](sql-api-sdk-bulk-executor-dot-net.md)
> * [Bulk Executor – Java](sql-api-sdk-bulk-executor-java.md)

| |  |
|---|---|
|**Beschreibung**|Die BulkExecutor-Bibliothek ermöglicht Clientanwendungen die Ausführung von Massenvorgängen in Azure Cosmos DB-Konten. Die BulkExecutor-Bibliothek stellt die Namespaces „BulkImport“ und „BulkUpdate“ bereit. Das BulkImport-Modul kann Dokumente auf optimierte Weise per Massenimport erfassen, sodass der für eine Sammlung bereitgestellte Durchsatz maximal genutzt wird. Das BulkUpdate-Modul kann vorhandene Daten in Azure Cosmos-Containern per Massenvorgang in Form von Patches aktualisieren.|
|**SDK-Download**|[Maven](https://search.maven.org/#search%7Cga%7C1%7Cdocumentdb-bulkexecutor)|
|**Bulk Executor-Bibliothek auf GitHub**|[GitHub](https://github.com/Azure/azure-cosmosdb-bulkexecutor-java-getting-started)|
| **API-Dokumentation**| [Java-API-Referenzdokumentation](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.bulkexecutor)|
|**Erste Schritte**|[Erste Schritte mit dem Java SDK für die BulkExecutor-Bibliothek](bulk-executor-java.md)|
|**Unterstützte Mindestlaufzeit**|[Java Development Kit (JDK) 7+](/java/azure/jdk/?view=azure-java-stable)|

## <a name="release-notes"></a>Versionshinweise

### <a name="2100"></a><a name="2.10.0"/>2.10.0

* Korrektur für „DocumentAnalyzer.java“, damit Schlüsselwerte geschachtelter Partitionen ordnungsgemäß aus JSON-Code extrahiert werden.

### <a name="294"></a><a name="2.9.4"/>2.9.4

* Funktionen in Massenlöschungsvorgängen hinzugefügt, mit denen Wiederholungsversuche für bestimmte Fehler ausgeführt werden und die auch eine Liste der Fehler, die wiederholt werden könnten, an den Benutzer zurückgeben.

### <a name="293"></a><a name="2.9.3"/>2.9.3

* Update für Cosmos SDK Version 2.4.7.

### <a name="292"></a><a name="2.9.2"/>2.9.2

* Korrektur für mergeAll zum Fortsetzen nach „ID“ und Partitionsschlüsselwert, sodass alle gepatchten Dokumenteigenschaften, die nach „ID“ und dem Partitionsschlüsselwert stehen, der aktualisierten Elementliste hinzugefügt werden.

### <a name="291"></a><a name="2.9.1"/>2.9.1

* Startgrad der Parallelität auf 1 aktualisiert und Debugprotokolle für minibatch hinzugefügt.


