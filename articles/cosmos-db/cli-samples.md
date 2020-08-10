---
title: Azure CLI-Beispiele für Azure Cosmos DB
description: Azure CLI-Beispiele für Azure Cosmos DB
author: markjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: sample
ms.date: 07/29/2020
ms.author: mjbrown
ms.custom: devx-track-azurecli
ms.openlocfilehash: 954215f04525e850151fdad93af6e7272b41b3df
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2020
ms.locfileid: "87498462"
---
# <a name="azure-cli-samples-for-azure-cosmos-db"></a>Azure CLI-Beispiele für Azure Cosmos DB

Die folgende Tabelle enthält Links zu Azure CLI-Beispielskripts für Azure Cosmos DB. Navigieren Sie über die Links auf der rechten Seite zu API-spezifischen Beispielen. Allgemeine Beispiele sind in allen APIs identisch. Referenzseiten für alle CLI-Befehle für Azure Cosmos DB sind in der [Referenz zur Azure CLI](/cli/azure/cosmosdb) verfügbar. Die CLI-Skriptbeispiele für Azure Cosmos DB finden Sie auch im [GitHub-Repository zur Azure Cosmos DB-CLI](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb).

Für diese Beispiele wird mindestens Version 2.9.1 der Azure-Befehlszeilenschnittstelle benötigt. Führen Sie `az --version` aus, um die Version zu ermitteln. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sie bei Bedarf unter [Installieren der Azure CLI](/cli/azure/install-azure-cli).

## <a name="common-samples"></a>Allgemeine Beispiele

Diese Beispiele gelten für alle Azure Cosmos DB-APIs.

|Aufgabe | BESCHREIBUNG |
|---|---|
| [Hinzufügen von Failoverregionen](scripts/cli/common/regions.md?toc=%2fcli%2fazure%2ftoc.json) | Hinzufügen einer Region, Ändern der Failoverpriorität und Auslösen eines manuellen Failovers|
| [Kontoschlüssel und Verbindungszeichenfolgen](scripts/cli/common/keys.md?toc=%2fcli%2fazure%2ftoc.json) | Auflisten von Kontoschlüsseln und schreibgeschützten Schlüsseln, erneutes Generieren von Schlüsseln und Auflisten von Verbindungszeichenfolgen|
| [Sichern mit einer IP-Firewall](scripts/cli/common/ipfirewall.md?toc=%2fcli%2fazure%2ftoc.json)| Erstellen eines Cosmos-Kontos mit konfigurierter IP-Firewall|
| [Sichern eines neuen Kontos mit Dienstendpunkten](scripts/cli/common/service-endpoints.md?toc=%2fcli%2fazure%2ftoc.json)| Erstellen eines Cosmos-Kontos und Sichern des Kontos mit Dienstendpunkten|
| [Sichern eines vorhandenen Kontos mit Dienstendpunkten](scripts/cli/common/service-endpoints-ignore-missing-vnet.md?toc=%2fcli%2fazure%2ftoc.json)| Aktualisieren eines Cosmos-Kontos beim Konfigurieren des Subnetzes, um es mit Dienstendpunkten zu sichern|
|||

## <a name="core-sql-api-samples"></a>Core-API-Beispiele (SQL)

|Aufgabe | BESCHREIBUNG |
|---|---|
| [Erstellen eines Kontos, einer Datenbank und eines Containers in Azure Cosmos DB](scripts/cli/sql/create.md?toc=%2fcli%2fazure%2ftoc.json)| Erstellen eines Kontos, einer Datenbank und eines Containers in Azure Cosmos DB für die Core-API (SQL) |
| [Konto, Datenbank und Container mit Autoskalierung in Azure Cosmos erstellen](scripts/cli/sql/autoscale.md?toc=%2fcli%2fazure%2ftoc.json)| Erstellen eines Kontos, einer Datenbank und eines Containers mit Autoskalierung in Azure Cosmos DB für die Core-API (SQL) |
| [Ändern des Durchsatzes](scripts/cli/sql/throughput.md?toc=%2fcli%2fazure%2ftoc.json) | Aktualisieren der Anforderungseinheiten pro Sekunde (RU/s) für eine Datenbank und einen Container|
| [Sperren von Ressourcen für die Löschung](scripts/cli/sql/lock.md?toc=%2fcli%2fazure%2ftoc.json)| Verhindern des Löschens von Ressourcen mit Ressourcensperren|
|||

## <a name="mongodb-api-samples"></a>MongoDB-API-Beispiele

|Aufgabe | BESCHREIBUNG |
|---|---|
| [Erstellen eines Kontos, einer Datenbank und einer Sammlung in Azure Cosmos](scripts/cli/mongodb/create.md?toc=%2fcli%2fazure%2ftoc.json)| Erstellen eines Kontos, einer Datenbank und einer Sammlung in Azure Cosmos DB für die MongoDB-API |
| [Konto, Datenbank mit Autoskalierung und zwei Sammlungen mit gemeinsam genutztem Durchsatz in Azure Cosmos erstellen](scripts/cli/mongodb/autoscale.md?toc=%2fcli%2fazure%2ftoc.json)| Erstellen eines Kontos, einer Datenbank mit Autoskalierung und von zwei Sammlungen mit gemeinsam genutztem Durchsatz in Azure Cosmos DB für die MongoDB-API |
| [Ändern des Durchsatzes](scripts/cli/mongodb/throughput.md?toc=%2fcli%2fazure%2ftoc.json) | Aktualisieren der Anforderungseinheiten pro Sekunde (RU/s) für eine Datenbank und Sammlung|
| [Sperren von Ressourcen für die Löschung](scripts/cli/mongodb/lock.md?toc=%2fcli%2fazure%2ftoc.json)| Verhindern des Löschens von Ressourcen mit Ressourcensperren|
|||

## <a name="cassandra-api-samples"></a>Cassandra-API-Beispiele

|Aufgabe | BESCHREIBUNG |
|---|---|
| [Erstellen eines Kontos, eines Keyspace und einer Tabelle in Azure Cosmos](scripts/cli/cassandra/create.md?toc=%2fcli%2fazure%2ftoc.json)| Erstellen eines Kontos, eines Keyspace und einer Tabelle in Azure Cosmos DB für die Cassandra-API |
| [Konto, Keyspace und Tabelle mit Autoskalierung in Azure Cosmos erstellen](scripts/cli/cassandra/autoscale.md?toc=%2fcli%2fazure%2ftoc.json)| Erstellen eines Kontos, eines Keyspace und einer Tabelle mit Autoskalierung in Azure Cosmos DB für die Cassandra-API |
| [Ändern des Durchsatzes](scripts/cli/cassandra/throughput.md?toc=%2fcli%2fazure%2ftoc.json) | Aktualisieren der Anforderungseinheiten pro Sekunde (RU/s) für einen Keyspace und eine Tabelle|
| [Sperren von Ressourcen für die Löschung](scripts/cli/cassandra/lock.md?toc=%2fcli%2fazure%2ftoc.json)| Verhindern des Löschens von Ressourcen mit Ressourcensperren|
|||

## <a name="gremlin-api-samples"></a>Gremlin-API-Beispiele

|Aufgabe | BESCHREIBUNG |
|---|---|
| [Erstellen eines Kontos, einer Datenbank und eines Graphen in Azure Cosmos](scripts/cli/gremlin/create.md?toc=%2fcli%2fazure%2ftoc.json)| Erstellen Sie ein Azure Cosmos DB-Konto, eine Datenbank und einen Graphen für die Gremlin-API. |
| [Konto, Datenbank und Graph mit Autoskalierung in Azure Cosmos erstellen](scripts/cli/gremlin/autoscale.md?toc=%2fcli%2fazure%2ftoc.json)| Erstellen eines Kontos, einer Datenbank und eines Graphen mit Autoskalierung in Azure Cosmos DB für die Gremlin-API |
| [Ändern des Durchsatzes](scripts/cli/gremlin/throughput.md?toc=%2fcli%2fazure%2ftoc.json) | Aktualisieren Sie die Anforderungseinheiten pro Sekunde (RU/s) für eine Datenbank und einen Graphen.|
| [Sperren von Ressourcen für die Löschung](scripts/cli/gremlin/lock.md?toc=%2fcli%2fazure%2ftoc.json)| Verhindern des Löschens von Ressourcen mit Ressourcensperren|
|||

## <a name="table-api-samples"></a>Tabellen-API-Beispiele

|Aufgabe | BESCHREIBUNG |
|---|---|
| [Erstellen eines Kontos und einer Tabelle in Azure Cosmos](scripts/cli/table/create.md?toc=%2fcli%2fazure%2ftoc.json)| Erstellen eines Kontos und einer Tabelle in Azure Cosmos DB für die Tabellen-API |
| [Konto und Tabelle mit Autoskalierung in Azure Cosmos erstellen](scripts/cli/table/autoscale.md?toc=%2fcli%2fazure%2ftoc.json)| Erstellen eines Kontos und einer Tabelle mit Autoskalierung in Azure Cosmos DB für die Tabellen-API |
| [Ändern des Durchsatzes](scripts/cli/table/throughput.md?toc=%2fcli%2fazure%2ftoc.json) | Aktualisieren der Anforderungseinheiten pro Sekunde (RU/s) für eine Tabelle|
| [Sperren von Ressourcen für die Löschung](scripts/cli/table/lock.md?toc=%2fcli%2fazure%2ftoc.json)| Verhindern des Löschens von Ressourcen mit Ressourcensperren.|
|||
