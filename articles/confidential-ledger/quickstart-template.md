---
title: Erstellen einer Microsoft Azure Confidential Ledger-Ressource mithilfe einer Azure Resource Manager-Vorlage
description: Erfahren Sie, wie Sie eine Microsoft Azure Confidential Ledger-Ressource mithilfe einer Azure Resource Manager-Vorlage erstellen.
services: azure-resource-manager
author: msmbaldwin
ms.service: azure-resource-manager
ms.topic: quickstart
ms.custom: subject-armqs, devx-track-azurepowershell
ms.author: mbaldwin
ms.date: 04/15/2021
ms.openlocfilehash: 1110d1441c63f3af1f7c31b9ac090c9693b36be1
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110385993"
---
# <a name="quickstart-create-an-microsoft-azure-confidential-ledger-with-an-arm-template"></a>Schnellstartanleitung: Erstellen einer Microsoft Azure Confidential Ledger-Ressource mit einer Azure Resource Manager-Vorlage

[Microsoft Azure Confidential Ledger ist](overview.md) ein neuer und äußerst sicherer Dienst für die Verwaltung vertraulicher Datensätze. In dieser Schnellstartanleitung geht es um die Bereitstellung einer Azure Resource Manager-Vorlage (ARM-Vorlage) zum Erstellen eines neuen Ledgers.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Wenn Ihre Umgebung die Voraussetzungen erfüllt und Sie mit der Verwendung von ARM-Vorlagen vertraut sind, klicken Sie auf die Schaltfläche **In Azure bereitstellen**. Die Vorlage wird im Azure-Portal geöffnet.

[![In Azure bereitstellen](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-confidential-ledger-create%2Fazuredeploy.json)

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="review-the-template"></a>Überprüfen der Vorlage

Die in dieser Schnellstartanleitung verwendete Vorlage stammt von der Seite mit den [Azure-Schnellstartvorlagen](https://github.com/Azure/azure-quickstart-templates).

In der Vorlage sind die folgenden Azure-Ressourcen definiert:

- Microsoft.ConfidentialLedger/ledgers

## <a name="deploy-the-template"></a>Bereitstellen der Vorlage

1. Wählen Sie das folgende Image aus, um sich bei Azure anzumelden und die Vorlage zu öffnen.

    [![In Azure bereitstellen](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-confidential-ledger-create%2Fazuredeploy.json)

1. Wählen Sie die folgenden Werte aus, bzw. geben Sie sie ein.

    Verwenden Sie den Standardwert, um den Confidential Ledger zu erstellen, sofern kein anderer Wert angegeben ist.

    - **Ledgername**: Wählen Sie einen Namen für Ihren Ledger aus. Namen für Ledger müssen global eindeutig sein.
    - **Standort**: Wählen Sie einen Standort aus. Beispiel: **USA, Osten**

1. Wählen Sie die Option **Kaufen**. Nachdem die Confidential Ledger-Ressource erfolgreich bereitgestellt wurde, erhalten Sie eine Benachrichtigung.

Zum Bereitstellen der Vorlage wird das Azure-Portal verwendet. Neben dem Azure-Portal können Sie auch Azure PowerShell, die Azure-Befehlszeilenschnittstelle (Azure CLI) und die REST-API verwenden. Informationen zu anderen Bereitstellungsmethoden finden Sie unter [Bereitstellen von Vorlagen](../azure-resource-manager/templates/deploy-powershell.md).

## <a name="review-deployed-resources"></a>Überprüfen der bereitgestellten Ressourcen

Sie können die Ledger-Ressource im Azure-Portal überprüfen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Weitere Artikel zu Microsoft Azure Confidential Ledger können auf dieser Schnellstartanleitung aufbauen. Falls Sie mit weiteren Schnellstartanleitungen und Tutorials fortfahren möchten, sollten Sie die Ressourcen nicht bereinigen.

Wenn Sie die Ressource nicht mehr benötigen, löschen Sie die Ressourcengruppe. Dadurch wird auch die Ledger-Ressource gelöscht. Die Ressourcengruppe kann über die Azure-Befehlszeilenschnittstelle oder mithilfe von Azure PowerShell gelöscht werden:

# <a name="cli"></a>[BEFEHLSZEILENSCHNITTSTELLE (CLI)](#tab/CLI)

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group delete --name $resourceGroupName &&
echo "Press [ENTER] to continue ..."
```

# <a name="powershell"></a>[PowerShell](#tab/PowerShell)

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
Remove-AzResourceGroup -Name $resourceGroupName
Write-Host "Press [ENTER] to continue..."
```

---

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie mithilfe einer ARM-Vorlage eine Confidential Ledger-Ressource erstellt und die Bereitstellung überprüft. Weitere Informationen zum Dienst finden Sie unter [Übersicht über Microsoft Azure Confidential Ledger](overview.md).


