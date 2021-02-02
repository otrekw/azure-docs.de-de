---
title: PowerShell-Skript zum Abrufen von Schlüssel- und Verbindungszeichenfolgenvorgängen für ein Azure Cosmos DB-Konto
description: 'Azure PowerShell-Skriptbeispiel: Kontoschlüssel- und Verbindungszeichenfolgenvorgänge für ein Azure Cosmos DB-Konto'
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 03/18/2020
ms.author: mjbrown
ms.openlocfilehash: eb84684201ddf79f2a73fd1bcba1a0f69899bea8
ms.sourcegitcommit: b39cf769ce8e2eb7ea74cfdac6759a17a048b331
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2021
ms.locfileid: "98684314"
---
# <a name="connection-string-and-account-key-operations-for-an-azure-cosmos-db-account-using-powershell"></a>Verbindungszeichenfolgen- und Kontoschlüsselvorgänge für ein Azure Cosmos DB-Konto mithilfe von PowerShell
[!INCLUDE[appliesto-all-apis](../../../includes/appliesto-all-apis.md)]

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

Für dieses Beispiel ist mindestens Azure PowerShell Az 5.4.0 erforderlich. Führen Sie `Get-Module -ListAvailable Az` aus, um die installierten Versionen zu ermitteln.
Wenn Sie die Installation ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-az-ps) Informationen dazu.

Führen Sie zum Anmelden bei Azure [Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount) aus.

## <a name="sample-script"></a>Beispielskript

> [!NOTE]
> Dieses Beispiel veranschaulicht die Verwendung eines SQL-API-Kontos. Wenn Sie dieses Beispiel für andere APIs verwenden möchten, kopieren Sie die zugehörigen Eigenschaften, und wenden Sie sie auf Ihr API-spezifisches Skript an.

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/common/ps-account-keys-connection-strings.ps1 "Connection strings and account keys for Azure Cosmos account")]

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
| [Get-AzCosmosDBAccountKey](/powershell/module/az.cosmosdb/get-azcosmosdbaccountkey) | Ruft die Verbindungszeichenfolge oder den Schlüssel (Lesen/Schreiben oder schreibgeschützt) für ein Cosmos DB-Konto ab. |
| [New-AzCosmosDBAccountKey](/powershell/module/az.cosmosdb/new-azcosmosdbaccountkey) | Generiert den angegebenen Schlüssel für ein Cosmos DB-Konto erneut. |
|**Azure-Ressourcengruppen**| |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |
|||

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure PowerShell finden Sie in der [Azure PowerShell-Dokumentation](/powershell/).