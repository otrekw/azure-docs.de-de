---
title: 'SQL-API: Python-Beispiele für Azure Cosmos DB'
description: Python-Beispiele für allgemeine Aufgaben in Azure Cosmos DB (einschließlich CRUD-Vorgänge) finden Sie auf GitHub.
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: python
ms.topic: sample
ms.date: 05/20/2020
ms.author: sngun
ms.custom: devx-track-python
ms.openlocfilehash: 6602d757e210837aadadd431b27bb7cf76f06715
ms.sourcegitcommit: dea88d5e28bd4bbd55f5303d7d58785fad5a341d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2020
ms.locfileid: "87873346"
---
# <a name="azure-cosmos-db-python-examples"></a>Python-Beispiele für Azure Cosmos DB

> [!div class="op_single_selector"]
> * [.NET V2 SDK-Beispiele](sql-api-dotnet-samples.md)
> * [.NET V3 SDK-Beispiele](sql-api-dotnet-v3sdk-samples.md)
> * [Java v4 SDK-Beispiele](sql-api-java-sdk-samples.md)
> * [Node.js-Beispiele](sql-api-nodejs-samples.md)
> * [Python-Beispiele](sql-api-python-samples.md)
> * [Katalog mit Azure-Codebeispielen](https://azure.microsoft.com/resources/samples/?sort=0&service=cosmos-db)

Beispiellösungen, in denen CRUD-Vorgänge und andere gängige Vorgänge für Azure Cosmos DB-Ressourcen ausgeführt werden, finden Sie im GitHub-Repository [azure-documentdb-python](https://github.com/Azure/azure-documentdb-python). Dieser Artikel enthält Folgendes:

* Links zu den Aufgaben in den einzelnen Python-Beispielprojektdateien.
* Links zum zugehörigen API-Referenzinhalt.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

- Sie können [Visual Studio-Abonnementvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio): Ihr Visual Studio-Abonnement beinhaltet ein monatliches Guthaben, das Sie für zahlungspflichtige Azure-Dienste verwenden können.

[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

Sie benötigen zudem das [Python SDK](sql-api-sdk-python.md).

   > [!NOTE]
   > Jedes Beispiel ist eigenständig mit eigener Einrichtung und Bereinigung. In den Beispielen werden mehrere Aufrufe an `CosmosClient.CreateContainer` ausgegeben. Jedes Mal, wenn dies geschieht, wird für Ihr Abonnement eine Nutzungsdauer von einer Stunde in Rechnung gestellt. Weitere Informationen zur Abrechnung für Azure Cosmos DB finden Sie unter [Azure Cosmos DB – Preise](https://azure.microsoft.com/pricing/details/cosmos-db/).

## <a name="database-examples"></a>Datenbankbeispiele

Das Python-Beispiel [database_management.py](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/cosmos/azure-cosmos/samples/database_management.py) zeigt, wie die folgenden Aufgaben ausgeführt werden. Um mehr über die Azure Cosmos-Datenbanken zu erfahren, lesen Sie vor dem Ausführen der folgenden Beispiele den Konzeptartikel [Arbeiten mit Datenbanken, Containern und Elementen](databases-containers-items.md).

| Aufgabe | API-Referenz |
| --- | --- |
| [Erstellen einer Datenbank](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/cosmos/azure-cosmos/samples/database_management.py#L48-L56) |CosmosClient.create_database|
| [Lesen einer Datenbank nach ID](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/cosmos/azure-cosmos/samples/database_management.py#L59-L67) |CosmosClient.get_database_client|
| [Abfragen der Datenbanken](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/cosmos/azure-cosmos/samples/database_management.py#L32-L67) |CosmosClient.query_databases|
| [Auflisten von Datenbanken für ein Konto](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/cosmos/azure-cosmos/samples/database_management.py#L70-L81) |CosmosClient.list_databases|
| [Löschen einer Datenbank](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/cosmos/azure-cosmos/samples/database_management.py#L84-L93) |CosmosClient.delete_database|

## <a name="container-examples"></a>Containerbeispiele

Das Python-Beispiel [container_management.py](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/cosmos/azure-cosmos/samples/container_management.py) zeigt, wie die folgenden Aufgaben ausgeführt werden. Um mehr über die Azure Cosmos-Sammlungen zu erfahren, lesen Sie vor dem Ausführen der folgenden Beispiele den Konzeptartikel [Arbeiten mit Datenbanken, Containern und Elementen](databases-containers-items.md).

| Aufgabe | API-Referenz |
| --- | --- |
| [Abfrage nach einem Container](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/cosmos/azure-cosmos/samples/container_management.py#L51-L66) |database.query_containers |
| [Container erstellen](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/cosmos/azure-cosmos/samples/container_management.py#L69-L163) |database.create_container |
| [Auflisten aller Container in einer Datenbank](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/cosmos/azure-cosmos/samples/container_management.py#L206-L217) |database.list_containers |
| [Abrufen eines Containers anhand seiner ID](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/cosmos/azure-cosmos/samples/container_management.py#L195-L203) |database.get_container_client |
| [Verwalten des bereitgestellten Durchsatzes für einen Container](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/cosmos/azure-cosmos/samples/container_management.py#L166-L192) |container.read_offer, container.replace_throughput|
| [Löschen eines Containers](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/cosmos/azure-cosmos/samples/container_management.py#L220-L229) |database.delete_container |

## <a name="item-examples"></a>Elementbeispiele

Das Python-Beispiel [item_management.py](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/cosmos/azure-cosmos/samples/document_management.py) zeigt, wie die folgenden Aufgaben ausgeführt werden. Um mehr über die Azure Cosmos-Dokumente zu erfahren, lesen Sie vor dem Ausführen der folgenden Beispiele den Konzeptartikel [Arbeiten mit Datenbanken, Containern und Elementen](databases-containers-items.md).

| Aufgabe | API-Referenz |
| --- | --- |
| [Erstellen von Elementen in einem Container](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/cosmos/azure-cosmos/samples/document_management.py#L26-L38) |container.create_item |
| [Lesen eines Elements anhand seiner ID](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/cosmos/azure-cosmos/samples/document_management.py#L41-L49) |container.read_item |
| [Lesen aller Elemente in einem Container](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/cosmos/azure-cosmos/samples/document_management.py#L52-L63) |container.read_all_items |
| [Abfragen eines Elements anhand seiner ID](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/cosmos/azure-cosmos/samples/document_management.py#L66-L78) |container.query_items |
| [Ersetzen eines Elements](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/cosmos/azure-cosmos/samples/document_management.py#L81-L88) |container.replace_items |
| [Aktualisieren/Einfügen (Upsert) eines Elements](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/cosmos/azure-cosmos/samples/document_management.py#L91-L98) |container.upsert_item |
| [Löschen eines Elements](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/cosmos/azure-cosmos/samples/document_management.py#L101-L106) |container.delete_item |
| [Abrufen des Änderungsfeeds von Elementen in einem Container](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/cosmos/azure-cosmos/samples/change_feed_management.py) |container.query_items_change_feed |

## <a name="indexing-examples"></a>Indizierungsbeispiele

Das Python-Beispiel [index_management.py](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/cosmos/azure-cosmos/samples/index_management.py) zeigt, wie die folgenden Aufgaben ausgeführt werden. Um mehr über die Indizierung in Azure Cosmos DB zu erfahren, lesen Sie vor dem Ausführen der folgenden Beispiele die Konzeptartikel [Indizierungsrichtlinien](index-policy.md), [Indizierungstypen](index-types.md) und [Indizierungspfade](index-paths.md).

| Aufgabe | API-Referenz |
| --- | --- |
| [Ausschließen eines bestimmten Elements von der Indizierung](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/cosmos/azure-cosmos/samples/index_management.py#L145-L201) | documents.IndexingDirective.Exclude|
| [Verwenden der manuellen Indizierung mit bestimmten indizierten Elementen](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/cosmos/azure-cosmos/samples/index_management.py#L204-L263) | documents.IndexingDirective.Include |
| [Ausschließen von Pfaden von der Indizierung](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/cosmos/azure-cosmos/samples/index_management.py#L266-L336) |Definieren von Pfaden, die in der `IndexingPolicy`-Eigenschaft ausgeschlossen werden sollen |
| [Verwenden von Bereichsindizes in Zeichenfolgen](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/cosmos/azure-cosmos/samples/index_management.py#L401-L485) | Definieren der Indizierungsrichtlinie mit Bereichsindizes für den Datentyp „String“. `'kind': documents.IndexKind.Range`, `'dataType': documents.DataType.String`|
| [Durchführen einer Indextransformation](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/cosmos/azure-cosmos/samples/index_management.py#L488-L544) |database.replace_container (Verwenden der aktualisierten Indizierungsrichtlinie)|
| [Verwenden von Scans, wenn nur ein Hashindex im Pfad vorhanden ist](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/cosmos/azure-cosmos/samples/index_management.py#L339-L398) | Festlegen von `enable_scan_in_query=True` und `enable_cross_partition_query=True` beim Abfragen der Elemente |
