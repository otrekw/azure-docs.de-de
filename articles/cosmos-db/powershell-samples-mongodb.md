---
title: 'Azure PowerShell-Beispiele für Azure Cosmos DB: MongoDB-API'
description: Hier erfahren Sie, wie Sie Azure PowerShell-Beispiele zum Ausführen verschiedener gängiger Aufgaben in der MongoDB-API von Azure Cosmos DB abrufen.
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 12/02/2019
ms.author: mjbrown
ms.openlocfilehash: 810161268df405d84f6c190d4d7f3b67f2a1def8
ms.sourcegitcommit: 07d62796de0d1f9c0fa14bfcc425f852fdb08fb1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "80366206"
---
# <a name="azure-powershell-samples-for-azure-cosmos-db-mongodb-api"></a>Azure PowerShell-Beispiele für Azure Cosmos DB: MongoDB-API

Die folgende Tabelle enthält Links zu Azure PowerShell-Beispielskripts für Azure Cosmos DB für die MongoDB-API.

> [!NOTE]
> Derzeit können Sie mithilfe von PowerShell, CLI und Resource Manager-Vorlagen nur Version 3.2 der Azure Cosmos DB-API für MongoDB-Konten (d. h. Konten mit dem Endpunkt im Format `*.documents.azure.com`) erstellen. Verwenden Sie zum Erstellen von Konten der Version 3.6 stattdessen das Azure-Portal.

> [!NOTE]
> In den Beispielen werden die [Az.CosmosDB](https://docs.microsoft.com/powershell/module/az.cosmosdb)-Verwaltungs-Cmdlets verwendet. Beachten Sie, dass sich die `Az.CosmosDB`-Cmdlets noch in der Vorschauphase befinden und sich vor der Veröffentlichung ggf. noch ändern. Prüfen Sie regelmäßig auf Updates für `Az.CosmosDB`.

| | |
|---|---|
|[Konto, Datenbank und Sammlung erstellen](scripts/powershell/mongodb/ps-mongodb-create.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Erstellen eines Kontos, einer Datenbank und einer Sammlung in Azure Cosmos |
|[Datenbanken oder Sammlungen auflisten oder abrufen](scripts/powershell/mongodb/ps-mongodb-list-get.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Auflisten oder Abrufen von Datenbanken oder Sammlungen |
|[RU/s abrufen](scripts/powershell/mongodb/ps-mongodb-ru-get.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Abrufen von RU/s für eine Datenbank oder eine Sammlung |
|[RU/s aktualisieren](scripts/powershell/mongodb/ps-mongodb-ru-update.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Aktualisieren von RU/s für eine Datenbank oder eine Sammlung |
|[Ein Konto aktualisieren oder eine Region hinzufügen](scripts/powershell/common/ps-account-update.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Hinzufügen einer Region zu einem Cosmos-Konto. Kann auch zum Ändern anderer Kontoeigenschaften verwendet werden, dafür muss jedoch ein separater Vorgang ausgeführt werden. |
|[Failoverpriorität ändern oder Failover auslösen](scripts/powershell/common/ps-account-failover-priority-update.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Ändern der Priorität eines regionalen Failovers eines Azure Cosmos-Kontos oder Auslösen eines manuellen Failovers |
|[Kontoschlüssel oder Verbindungszeichenfolgen](scripts/powershell/common/ps-account-keys-connection-strings.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Abrufen von Primär- und Sekundärschlüsseln und Verbindungszeichenfolgen oder erneutes Generieren eines Kontoschlüssels eines Azure Cosmos-Kontos |
|[Cosmos-Konto mit IP-Firewall erstellen](scripts/powershell/common/ps-account-firewall-create.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Erstellen eines Azure Cosmos-Kontos mit aktivierter IP-Firewall |
|||
