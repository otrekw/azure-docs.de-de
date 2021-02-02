---
title: PowerShell-Skript zum Erstellen eines Azure Cosmos DB-Kontos mit IP-Firewall
description: 'Azure PowerShell-Skriptbeispiel: Erstellen eines Azure Cosmos DB-Kontos mit IP-Firewall'
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 03/18/2020
ms.author: mjbrown
ms.openlocfilehash: c29d131948be42a53bf568aa4048fe4e7d61c36e
ms.sourcegitcommit: b39cf769ce8e2eb7ea74cfdac6759a17a048b331
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2021
ms.locfileid: "98684313"
---
# <a name="create-an-azure-cosmos-db-account-with-ip-firewall"></a>Erstellen eines Azure Cosmos DB-Kontos mit IP-Firewall
[!INCLUDE[appliesto-all-apis](../../../includes/appliesto-all-apis.md)]

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

Für dieses Beispiel ist mindestens Azure PowerShell Az 5.4.0 erforderlich. Führen Sie `Get-Module -ListAvailable Az` aus, um die installierten Versionen zu ermitteln.
Wenn Sie die Installation ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-az-ps) Informationen dazu.

Führen Sie zum Anmelden bei Azure [Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount) aus.

## <a name="sample-script"></a>Beispielskript

> [!NOTE]
> Dieses Beispiel veranschaulicht die Verwendung eines SQL-API-Kontos (Core-API). Wenn Sie dieses Beispiel für andere APIs verwenden möchten, kopieren Sie die zugehörigen Eigenschaften, und wenden Sie sie auf Ihr API-spezifisches Skript an.

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/common/ps-account-firewall-create.ps1 "Create an Azure Cosmos account with IP Firewall")]

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
| [New-AzCosmosDBAccount](/powershell/module/az.cosmosdb/new-azcosmosdbaccount) | Erstellt ein neues Cosmos DB-Konto. |
|**Azure-Ressourcengruppen**| |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |
|||

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure PowerShell finden Sie in der [Azure PowerShell-Dokumentation](/powershell/).