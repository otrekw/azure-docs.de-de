---
title: 'Azure Cosmos DB: Java-API, SDK und Ressourcen für BulkExecutor'
description: Wichtige Informationen zur Java-API und zum SDK für BulkExecutor, einschließlich Veröffentlichungsdaten, Deaktivierungsdaten und Änderungen an den einzelnen Versionen des Azure Cosmos DB-BulkExecutor-Java-SDK.
author: milismsft
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: java
ms.topic: reference
ms.date: 11/21/2018
ms.author: adrianmi
ms.openlocfilehash: 0030f974a36dc80dc8c4112000aa5934126a2482
ms.sourcegitcommit: 11572a869ef8dbec8e7c721bc7744e2859b79962
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/05/2020
ms.locfileid: "82836443"
---
# <a name="java-bulk-executor-library-download-information"></a>Java-BulkExecutor-Bibliothek: Informationen zum Download

> [!div class="op_single_selector"]
> * [.NET](sql-api-sdk-dotnet.md)
> * [.NET-Änderungsfeed](sql-api-sdk-dotnet-changefeed.md)
> * [.NET Core](sql-api-sdk-dotnet-core.md)
> * [Node.js](sql-api-sdk-node.md)
> * [Async Java](sql-api-sdk-async-java.md)
> * [Java](sql-api-sdk-java.md)
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


