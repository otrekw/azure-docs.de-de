---
title: PowerShell-Skript zum Auflisten und Abrufen von Azure Cosmos DB-Tabellen-API-Vorgängen
description: 'Azure PowerShell-Skript: list- und get-Vorgänge von Azure Cosmos DB für die Tabellen-API'
author: markjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.topic: sample
ms.date: 07/31/2020
ms.author: mjbrown
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 1b11083a343e6b8fb90e8bca01a3a6f85b69729f
ms.sourcegitcommit: df574710c692ba21b0467e3efeff9415d336a7e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2021
ms.locfileid: "110679621"
---
# <a name="list-and-get-tables-for-azure-cosmos-db---table-api"></a>Auflisten und Abrufen von Tabellen für Azure Cosmos DB – Tabellen-API
[!INCLUDE[appliesto-table-api](../../../includes/appliesto-table-api.md)]

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

Für dieses Beispiel ist mindestens Azure PowerShell Az 5.4.0 erforderlich. Führen Sie `Get-Module -ListAvailable Az` aus, um die installierten Versionen zu ermitteln.
Wenn Sie die Installation ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-az-ps) Informationen dazu.

Führen Sie zum Anmelden bei Azure [Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount) aus.

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/table/ps-table-list-get.ps1 "List or get tables for Table API")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung

Nach Ausführung des Skriptbeispiels können mit dem folgenden Befehl die Ressourcengruppe und alle damit verbundenen Ressourcen entfernt werden.

```powershell
Remove-AzResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
|**Azure Cosmos DB**| |
| [Get-AzCosmosDBAccount](/powershell/module/az.cosmosdb/get-azcosmosdbaccount) | Listet Cosmos DB-Konten auf oder ruft ein bestimmtes Cosmos DB-Konto ab. |
| [Get-AzCosmosDBTable](/powershell/module/az.cosmosdb/get-azcosmosdbtable) | Listet Tabellen-API-Tabellen in einem Konto auf oder ruft eine bestimmte Tabellen-API-Tabelle in einem Konto ab. |
|**Azure-Ressourcengruppen**| |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |
|||

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure PowerShell finden Sie in der [Azure PowerShell-Dokumentation](/powershell/).
