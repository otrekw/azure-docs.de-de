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
ms.openlocfilehash: b222a94ee754b24192261451d8ddc429886e705c
ms.sourcegitcommit: 6fd8dbeee587fd7633571dfea46424f3c7e65169
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/21/2020
ms.locfileid: "83725651"
---
# <a name="azure-cosmos-db-java-sdk-v4-for-core-sql-api-release-notes-and-resources"></a>Versionshinweise und Ressourcen zu Azure Cosmos DB Java SDK v4 für die Core (SQL)-API
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
|**API-Dokumentation** | [Java-API-Referenzdokumentation](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-cosmos/4.0.1-beta.3/index.html) |
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

