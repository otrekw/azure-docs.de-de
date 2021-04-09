---
title: PowerShell-Skript zum Ändern der Failoverpriorität für ein Azure Cosmos-Konto mit einer einzelnen Schreibregion
description: 'Azure PowerShell-Skriptbeispiel: Ändern der Failoverpriorität oder Auslösen des Failovers für ein Azure Cosmos-Konto mit einer einzelnen Schreibregion'
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 03/18/2020
ms.author: mjbrown
ms.openlocfilehash: ef9b16ae56c560d033cb18de6a3ccf3b87fbc25a
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "98684339"
---
# <a name="change-failover-priority-or-trigger-failover-for-an-azure-cosmos-account-with-single-write-region-by-using-powershell"></a>Ändern der Failoverpriorität oder Auslösen des Failovers für ein Azure Cosmos-Konto mit einer einzelnen Schreibregion mithilfe von PowerShell
[!INCLUDE[appliesto-all-apis](../../../includes/appliesto-all-apis.md)]

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

Für dieses Beispiel ist mindestens Azure PowerShell Az 5.4.0 erforderlich. Führen Sie `Get-Module -ListAvailable Az` aus, um die installierten Versionen zu ermitteln.
Wenn Sie die Installation ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-az-ps) Informationen dazu.

Führen Sie zum Anmelden bei Azure [Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount) aus.

## <a name="sample-script"></a>Beispielskript

> [!NOTE]
> Jede Änderung an einer Region mit `failoverPriority=0` löst ein manuelles Failover aus und kann nur für ein Konto ausgeführt werden, für das das manuelle Failover konfiguriert ist. Durch Änderungen an allen anderen Regionen wird einfach die Failoverpriorität für ein Cosmos-Konto geändert.
> [!NOTE]
> Dieses Beispiel veranschaulicht die Verwendung eines SQL-API-Kontos (Core-API). Wenn Sie dieses Beispiel für andere APIs verwenden möchten, kopieren Sie die zugehörigen Eigenschaften, und wenden Sie sie auf Ihr API-spezifisches Skript an.

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/common/ps-account-failover-priority-update.ps1 "Update failover priority for an Azure Cosmos account or trigger a manual failover")]

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
| [Update-AzCosmosDBAccountFailoverPriority](/powershell/module/az.cosmosdb/update-azcosmosdbaccountfailoverpriority) | Aktualisieren der Failoverprioritätenreihenfolge für die Regionen eines Cosmos DB-Kontos. |
|**Azure-Ressourcengruppen**| |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |
|||

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure PowerShell finden Sie in der [Azure PowerShell-Dokumentation](/powershell/).