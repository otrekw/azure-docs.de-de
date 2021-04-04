---
title: 'Erstellen eines Azure Blockchain Service-Mitglieds: Azure PowerShell'
description: Erstellen Sie mithilfe der Azure PowerShell ein Azure Blockchain Service-Mitglied für ein Blockchainkonsortium.
ms.date: 9/22/2020
ms.topic: quickstart
ms.reviewer: ravastra
ms.custom: references_regions, devx-track-azurepowershell
ms.openlocfilehash: b57c44e79d599ab41b2c3356ee337811acdf639d
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "91948339"
---
# <a name="quickstart-create-an-azure-blockchain-service-blockchain-member-using-azure-powershell"></a>Schnellstart: Erstellen eines Blockchainmitglieds für den Azure Blockchain Service mithilfe der Azure PowerShell

In dieser Schnellstartanleitung stellen Sie mithilfe der Azure PowerShell ein neues Blockchainmitglied und -konsortium in Azure Blockchain Service bereit.

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

Falls Sie PowerShell lokal verwenden möchten, müssen Sie für diesen Artikel das Az PowerShell-Modul installieren und mit dem Cmdlet [Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount) eine Verbindung mit Ihrem Azure-Konto herstellen. Weitere Informationen zum Installieren des Az PowerShell-Moduls finden Sie unter [Installieren von Azure PowerShell](/powershell/azure/install-az-ps).

> [!IMPORTANT]
> Solange nur eine Vorschauversion des PowerShell-Moduls **Az.Blockchain** verfügbar ist, müssen Sie es separat über das Az PowerShell-Modul installieren. Verwenden Sie dazu das Cmdlet `Install-Module`. Sobald dieses PowerShell-Modul allgemein verfügbar ist, wird es in die zukünftigen Releases des Az PowerShell-Moduls integriert und in Azure Cloud Shell nativ zur Verfügung gestellt.

```azurepowershell-interactive
Install-Module -Name Az.Blockchain
```

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

## <a name="register-resource-provider"></a>Registrieren des Ressourcenanbieters

Falls Sie den Azure Blockchain Service zum ersten Mal verwenden, müssen Sie den Ressourcenanbieter **Microsoft.Blockchain** registrieren.

```azurepowershell-interactive
Register-AzResourceProvider -ProviderNamespace Microsoft.Blockchain
```

## <a name="choose-a-specific-azure-subscription"></a>Auswählen eines bestimmten Azure-Abonnements

Wenn Sie über mehrere Azure-Abonnements verfügen, müssen Sie das entsprechende Abonnement auswählen, in dem die Ressourcen fakturiert werden sollen. Wählen Sie mit dem Cmdlet [Set-AzContext](/powershell/module/az.accounts/set-azcontext) ein bestimmtes Abonnement aus.

```azurepowershell-interactive
Set-AzContext -SubscriptionId 00000000-0000-0000-0000-000000000000
```

## <a name="define-variables"></a>Definieren von Variablen

Sie verwenden mehrere Informationselemente wiederholt. Erstellen Sie Variablen zum Speichern der Informationen.

```azurepowershell-interactive
# Name of resource group used throughout this article
$resourceGroupName = 'myResourceGroup'

# Azure region
$location = 'eastus'
```

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie mit dem Cmdlet [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) eine [Azure-Ressourcengruppe](../../azure-resource-manager/management/overview.md). Eine Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen als Gruppe bereitgestellt und verwaltet werden.

Im folgenden Beispiel wird eine Ressourcengruppe basierend auf dem Namen in der Variablen `$resourceGroupName` in der Region erstellt, die in der Variablen `$location` angegeben ist.

```azurepowershell-interactive
New-AzResourceGroup -Name $resourceGroupName -Location $location
```

## <a name="create-a-blockchain-member"></a>Erstellen eines Blockchainmitglieds

Ein Azure Blockchain Service-Mitglied ist ein Blockchainknoten in einem privaten Konsortium-Blockchainnetzwerk.
Wenn Sie ein Mitglied bereitstellen, können Sie ein Konsortiumsnetzwerk erstellen oder einem Konsortiumsnetzwerk beitreten. Für ein Konsortiumsnetzwerk wird mindestens ein Mitglied benötigt. Die Anzahl der von den Teilnehmern benötigten Blockchainmitglieder hängt von Ihrem Szenario ab. Die Konsortiumsteilnehmer können über eines oder mehrere Blockchainmitglieder verfügen oder Mitglieder gemeinsam mit anderen Teilnehmern nutzen. Weitere Informationen zu Konsortien finden Sie unter [Azure Blockchain Service-Konsortium](consortium.md).

Es gibt mehrere Parameter und Eigenschaften, die Sie übergeben müssen. Ersetzen Sie die Beispielparameter durch eigene Werte.

```azurepowershell-interactive
$passwd = Read-Host -Prompt 'Enter the members default transaction node password' -AsSecureString
$csPasswd = Read-Host -Prompt 'Enter  the consortium account password' -AsSecureString

$memberParams = @{
  Name = 'myblockchainmember'
  ResourceGroupName = $resourceGroupName
  Consortium = 'myconsortium'
  ConsortiumManagementAccountPassword = $csPasswd
  Location = $location
  Password = $passwd
  Protocol = 'Quorum'
  Sku = 'S0'
}
New-AzBlockchainMember @memberParams
```

| Parameter | Beschreibung |
|---------|-------------|
| **ResourceGroupName** | Name der Ressourcengruppe, in der die Ressourcen des Azure Blockchain-Diensts erstellt werden. Verwenden Sie die Ressourcengruppe aus, die Sie im vorherigen Abschnitt erstellt haben.
| **Name** | Ein eindeutiger Name, der Ihr Blockchainmitglied in Azure Blockchain identifiziert. Der Name wird für die Adresse eines öffentlichen Endpunkts verwendet. Beispiel: `myblockchainmember.blockchain.azure.com`.
| **Standort** | Die Azure-Region, in der das Blockchainmitglied erstellt wird. Beispiel: `westus2`. Wählen Sie den Standort aus, der Ihren Benutzern oder anderen Azure-Anwendungen am nächsten liegt. Features sind unter Umständen in einigen Regionen nicht verfügbar. Azure Blockchain Data Manager ist in den folgenden Azure-Regionen verfügbar: („USA, Osten“ und „Europa, Westen“) ausgeführt werden.
| **Kennwort** | Das Kennwort für den Standardtransaktionsknoten des Mitglieds. Verwenden Sie das Kennwort für die Standardauthentifizierung, wenn Sie eine Verbindung mit dem öffentlichen Endpunkt des Standardtransaktionsknotens des Blockchainmitglieds herstellen.
| **Protokoll** | Blockchain-Protokoll. Derzeit wird das _Quorum_-Protokoll unterstützt.
| **Konsortium** | Name des Konsortiums, dem beigetreten oder das erstellt werden soll. Weitere Informationen zu Konsortien finden Sie unter [Azure Blockchain Service-Konsortium](consortium.md).
| **ConsortiumManagementAccountPassword** | Das Kennwort für das Konsortium wird auch als Mitgliedskontokennwort bezeichnet. Das Mitgliedskontokennwort wird zum Verschlüsseln des privaten Schlüssels für das Ethereum-Konto verwendet, das für Ihr Mitglied erstellt wird. Sie verwenden das Mitgliedskonto und das Mitgliedskontokennwort für die Verwaltung des Konsortiums.
| **sku** | Tarifart. **S0** für Standard oder **B0** für Basic. Verwenden Sie _Basic_ für die Entwicklung, das Testen und Proof of Concept-Vorgänge. Verwenden Sie _Standard_ für Bereitstellungen für die Produktion. Sie sollten den Tarif _Standard_ auch wählen, wenn Sie Blockchain Data Manager verwenden oder eine große Menge privater Transaktionen senden. Das Wechseln zwischen den Tarifen „Basic“ und „Standard“ nach der Erstellung eines Mitglieds wird nicht unterstützt.

Es dauert etwa 10 Minuten, das Blockchainmitglied und die zugehörigen Ressourcen zu erstellen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Das von Ihnen erstellte Blockchainmitglied können Sie für die nächste Schnellstartanleitung oder das nächste Tutorial verwenden. Wenn Ressourcen nicht mehr benötigt werden, können Sie diese löschen, indem Sie die für die Schnellstartanleitung erstellte Ressourcengruppe `myResourceGroup` löschen.

> [!CAUTION]
> Im folgenden Beispiel werden die angegebene Ressourcengruppe und alle darin enthaltenen Ressourcen gelöscht.
> Falls in der angegebenen Ressourcengruppe Ressourcen enthalten sind, die nicht zum Umfang dieses Artikels gehören, werden sie ebenfalls gelöscht.

```azurepowershell-interactive
Remove-AzResourceGroup -Name $resourceGroupName
```

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie ein Azure Blockchain Service-Mitglied und ein neues Konsortium bereitgestellt. In der nächsten Schnellstartanleitung erfahren Sie, wie Sie das Azure Blockchain Development Kit für Ethereum zum Anfügen an ein Azure Blockchain Service-Mitglied verwenden.

> [!div class="nextstepaction"]
> [Herstellen einer Verbindung mit Azure Blockchain Service mithilfe von Visual Studio Code](connect-vscode.md)