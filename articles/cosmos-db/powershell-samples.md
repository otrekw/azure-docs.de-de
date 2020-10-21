---
title: Azure PowerShell-Beispiele für Azure Cosmos DB
description: Hier erfahren Sie, wie Sie Azure PowerShell-Beispiele zum Ausführen gängiger Aufgaben in Azure Cosmos DB abrufen.
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 10/07/2020
ms.author: mjbrown
ms.openlocfilehash: 0c2d13ef49676917bc48dd5a2e682471558567f3
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91842970"
---
# <a name="azure-powershell-samples-for-azure-cosmos-db"></a>Azure PowerShell-Beispiele für Azure Cosmos DB

Die unten stehende Tabelle enthält Links zu häufig verwendeten Azure PowerShell-Skripts für Azure Cosmos DB. Navigieren Sie über die Links auf der rechten Seite zu API-spezifischen Beispielen. Allgemeine Beispiele sind in allen APIs identisch. Referenzseiten für alle Azure Cosmos DB-PowerShell-Cmdlets finden Sie in der [Referenz zu Azure PowerShell](/powershell/module/az.cosmosdb). Prüfen Sie regelmäßig auf Updates für `Az.CosmosDB`. Sie können diese PowerShell-Beispiele für Cosmos DB auch aus unserem GitHub-Repository forken. Informationen dazu finden Sie in den [Cosmos DB-PowerShell-Beispielen auf GitHub](https://github.com/Azure/azure-docs-powershell-samples/tree/master/cosmosdb).

## <a name="common-samples"></a>Allgemeine Beispiele

|Aufgabe | BESCHREIBUNG |
|---|---|
|[Konto aktualisieren](scripts/powershell/common/account-update.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Aktualisieren der Standardkonsistenzebene eines Cosmos DB-Kontos |
|[Regionen eines Kontos aktualisieren](scripts/powershell/common/update-region.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Aktualisieren der Regionen eines Cosmos DB-Kontos |
|[Failoverpriorität ändern oder Failover auslösen](scripts/powershell/common/failover-priority-update.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Ändern der Priorität eines regionalen Failovers eines Azure Cosmos-Kontos oder Auslösen eines manuellen Failovers |
|[Kontoschlüssel oder Verbindungszeichenfolgen](scripts/powershell/common/keys-connection-strings.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Abrufen von Primär- und Sekundärschlüsseln und Verbindungszeichenfolgen oder erneutes Generieren eines Kontoschlüssels eines Azure Cosmos DB-Kontos |
|[Cosmos-Konto mit IP-Firewall erstellen](scripts/powershell/common/firewall-create.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Erstellen eines Azure Cosmos DB-Kontos mit aktivierter IP-Firewall |
|||

## <a name="core-sql-api-samples"></a>Core-API-Beispiele (SQL)

|Aufgabe | BESCHREIBUNG |
|---|---|
|[Konto, Datenbank und Container erstellen](scripts/powershell/sql/create.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Erstellen eines Kontos, einer Datenbank und eines Containers in Azure Cosmos DB |
|[Konto, Datenbank und Container mit Autoskalierung erstellen](scripts/powershell/sql/autoscale.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Erstellen eines Kontos, einer Datenbank und eines Containers mit Autoskalierung in Azure Cosmos DB |
|[Container mit einem großen Partitionsschlüssel erstellen](scripts/powershell/sql/create-large-partition-key.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Erstellen eines Containers mit einem großen Partitionsschlüssel |
|[Container ohne Indexrichtlinie erstellen](scripts/powershell/sql/create-index-none.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Erstellen eines Azure Cosmos DB-Containers mit deaktivierter Indexrichtlinie.|
|[Datenbanken oder Container auflisten oder abrufen](scripts/powershell/sql/list-get.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Auflisten oder Abrufen von Datenbanken oder Containern |
|[Durchsatzvorgänge](scripts/powershell/sql/throughput.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Durchsatzvorgänge für eine Datenbank oder einen Container, z. B. Abrufen, Aktualisieren und Migrieren zwischen automatisch skaliertem und standardmäßigem Durchsatz. |
|[Sperren von Ressourcen für die Löschung](scripts/powershell/sql/lock.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Verhindern des Löschens von Ressourcen mit Ressourcensperren. |
|||

## <a name="cassandra-api-samples"></a>Cassandra-API-Beispiele

|Aufgabe | BESCHREIBUNG |
|---|---|
|[Konto, Keyspace und Tabelle erstellen](scripts/powershell/cassandra/create.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Erstellen eines Kontos, eines Keyspace und einer Tabelle in Azure Cosmos |
|[Konto, Keyspace und Tabelle mit Autoskalierung erstellen](scripts/powershell/cassandra/autoscale.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Erstellen eines Kontos, Keyspace und einer Tabelle mit Autoskalierung in Azure Cosmos |
|[Keyspaces oder Tabellen auflisten oder abrufen](scripts/powershell/cassandra/list-get.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Auflisten oder Abrufen von Keyspaces oder Tabellen |
|[Durchsatzvorgänge](scripts/powershell/cassandra/throughput.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Durchsatzvorgänge für einen Keyspace oder eine Tabelle, z. B. Abrufen, Aktualisieren und Migrieren zwischen automatisch skaliertem und standardmäßigem Durchsatz. |
|[Sperren von Ressourcen für die Löschung](scripts/powershell/cassandra/lock.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Verhindern des Löschens von Ressourcen mit Ressourcensperren. |
|||

## <a name="mongo-db-api-samples"></a>Mongo DB-API-Beispiele

|Aufgabe | BESCHREIBUNG |
|---|---|
|[Konto, Datenbank und Sammlung erstellen](scripts/powershell/mongodb/create.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Erstellen eines Kontos, einer Datenbank und einer Sammlung in Azure Cosmos |
|[Konto, Datenbank und Sammlung mit Autoskalierung erstellen](scripts/powershell/mongodb/autoscale.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Erstellen eines Kontos, einer Datenbank und einer Sammlung mit Autoskalierung in Azure Cosmos |
|[Datenbanken oder Sammlungen auflisten oder abrufen](scripts/powershell/mongodb/list-get.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Auflisten oder Abrufen von Datenbanken oder Sammlungen |
|[Durchsatzvorgänge](scripts/powershell/mongodb/throughput.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Durchsatzvorgänge für eine Datenbank oder eine Sammlung, z. B. Abrufen, Aktualisieren und Migrieren zwischen automatisch skaliertem und standardmäßigem Durchsatz. |
|[Sperren von Ressourcen für die Löschung](scripts/powershell/mongodb/lock.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Verhindern des Löschens von Ressourcen mit Ressourcensperren. |
|||

## <a name="gremlin-api-samples"></a>Gremlin-API-Beispiele

|Aufgabe | BESCHREIBUNG |
|---|---|
|[Konto, Datenbank und Graph erstellen](scripts/powershell/gremlin/create.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Erstellen eines Kontos, einer Datenbank und eines Graphs in Azure Cosmos |
|[Konto, Datenbank und Graph mit Autoskalierung erstellen](scripts/powershell/gremlin/autoscale.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Erstellen eines Kontos, einer Datenbank und eines Graphen mit Autoskalierung in Azure Cosmos |
|[Datenbanken oder Graphen auflisten oder abrufen](scripts/powershell/gremlin/list-get.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Auflisten oder Abrufen einer Datenbank oder eines Graphs |
|[Durchsatzvorgänge](scripts/powershell/gremlin/throughput.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Durchsatzvorgänge für eine Datenbank oder einen Graphen, z. B. Abrufen, Aktualisieren und Migrieren zwischen automatisch skaliertem und standardmäßigem Durchsatz. |
|[Sperren von Ressourcen für die Löschung](scripts/powershell/gremlin/lock.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Verhindern des Löschens von Ressourcen mit Ressourcensperren. |
|||

## <a name="table-api-samples"></a>Tabellen-API-Beispiele

|Aufgabe | BESCHREIBUNG |
|---|---|
|[Konto und Tabelle erstellen](scripts/powershell/table/create.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Erstellen eines Kontos und einer Tabelle in Azure Cosmos |
|[Konto und Tabelle mit Autoskalierung erstellen](scripts/powershell/table/autoscale.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Erstellen eines Kontos und einer Tabelle mit Autoskalierung in Azure Cosmos |
|[Auflisten oder Abrufen von Tabellen](scripts/powershell/table/list-get.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Auflisten oder Abrufen von Tabellen |
|[Durchsatzvorgänge](scripts/powershell/table/throughput.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Durchsatzvorgänge für eine Tabelle, z. B. Abrufen, Aktualisieren und Migrieren zwischen automatisch skaliertem und standardmäßigem Durchsatz. |
|[Sperren von Ressourcen für die Löschung](scripts/powershell/table/lock.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Verhindern des Löschens von Ressourcen mit Ressourcensperren. |
|||
