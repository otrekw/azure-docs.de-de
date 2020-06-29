---
title: 'Azure PowerShell-Beispiele für Azure Cosmos DB: SQL-API (Core-API)'
description: Hier erfahren Sie, wie Sie Azure PowerShell-Beispiele zum Ausführen verschiedener gängiger Aufgaben in Azure Cosmos DB-SQL-API-Konten abrufen.
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 06/12/2020
ms.author: mjbrown
ms.openlocfilehash: 82d709567e75540a865758162ac87f55dfa84bea
ms.sourcegitcommit: 398fecceba133d90aa8f6f1f2af58899f613d1e3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/21/2020
ms.locfileid: "85126092"
---
# <a name="azure-powershell-samples-for-azure-cosmos-db---sql-core-api"></a>Azure PowerShell-Beispiele für Azure Cosmos DB: SQL-API (Core-API)

Die unten stehende Tabelle enthält Links zu häufig verwendeten Azure PowerShell-Skripts für die SQL (Core)-API von Azure Cosmos DB. Wenn Sie diese PowerShell-Beispiele für Cosmos DB aus unserem GitHub-Repository forken möchten, sehen Sie sich die [Cosmos DB-PowerShell-Beispiele auf GitHub](https://github.com/Azure/azure-docs-powershell-samples/tree/master/cosmosdb) an.

Weitere Cosmos DB-PowerShell-Beispiele für die SQL-API (Core-API) sowie die Dokumentation finden Sie unter [Verwalten von SQL-API-Ressourcen für Azure Cosmos DB mit PowerShell](manage-with-powershell.md). Cosmos DB-PowerShell-Beispiele für andere APIs finden Sie unter [Cassandra-API](powershell-samples-cassandra.md), [MongoDB-API](powershell-samples-mongodb.md), [Gremlin-API](powershell-samples-gremlin.md) und [Tabellen-API](powershell-samples-table.md).

> [!NOTE]
> In den Beispielen werden die [Az.CosmosDB](https://docs.microsoft.com/powershell/module/az.cosmosdb)-Verwaltungs-Cmdlets verwendet. Prüfen Sie regelmäßig auf Updates für `Az.CosmosDB`.

| | |
|---|---|
|[Konto, Datenbank und Container erstellen](scripts/powershell/sql/ps-sql-create.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Erstellen eines Kontos, einer Datenbank und eines Containers in Azure Cosmos DB |
|[Container mit einem großen Partitionsschlüssel erstellen](scripts/powershell/sql/ps-sql-container-create-large-partition-key.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Erstellen eines Containers mit einem großen Partitionsschlüssel |
|[Datenbanken oder Container auflisten oder abrufen](scripts/powershell/sql/ps-sql-list-get.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Auflisten oder Abrufen von Datenbanken oder Containern |
|[RU/s abrufen](scripts/powershell/sql/ps-sql-ru-get.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Abrufen von RU/s für eine Datenbank oder einen Container |
|[RU/s aktualisieren](scripts/powershell/sql/ps-sql-ru-update.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Aktualisieren von RU/s für eine Datenbank oder einen Container |
|[Container ohne Indexrichtlinie erstellen](scripts/powershell/sql/ps-sql-container-create-index-none.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Erstellen eines Azure Cosmos DB-Containers mit deaktivierter Indexrichtlinie.|
|[Konto aktualisieren](scripts/powershell/common/ps-account-update.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Aktualisieren der Standardkonsistenzebene eines Cosmos DB-Kontos |
|[Regionen eines Kontos aktualisieren](scripts/powershell/common/ps-account-update-region.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Aktualisieren der Regionen eines Cosmos DB-Kontos |
|[Failoverpriorität ändern oder Failover auslösen](scripts/powershell/common/ps-account-failover-priority-update.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Ändern der Priorität eines regionalen Failovers eines Azure Cosmos-Kontos oder Auslösen eines manuellen Failovers |
|[Kontoschlüssel oder Verbindungszeichenfolgen](scripts/powershell/common/ps-account-keys-connection-strings.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Abrufen von Primär- und Sekundärschlüsseln und Verbindungszeichenfolgen oder erneutes Generieren eines Kontoschlüssels eines Azure Cosmos DB-Kontos |
|[Cosmos-Konto mit IP-Firewall erstellen](scripts/powershell/common/ps-account-firewall-create.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Erstellen eines Azure Cosmos DB-Kontos mit aktivierter IP-Firewall |
|[Sperren von Ressourcen für die Löschung](scripts/powershell/sql/powershell-sql-lock.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Verhindern des Löschens von Ressourcen mit Ressourcensperren. |
|||
