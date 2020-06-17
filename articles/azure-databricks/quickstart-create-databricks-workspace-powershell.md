---
title: 'Schnellstart: Erstellen eines Azure Databricks-Arbeitsbereichs mit PowerShell'
description: In diesem Schnellstart erfahren Sie, wie Sie mit PowerShell einen Azure Databricks-Arbeitsbereich erstellen.
services: azure-databricks
ms.service: azure-databricks
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.workload: big-data
ms.topic: quickstart
ms.date: 06/02/2020
ms.custom: mvc
ms.openlocfilehash: 445832119a113f92dc1da51eb5b5e77dff176fa3
ms.sourcegitcommit: f57fa5f3ce40647eda93f8be4b0ab0726d479bca
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/07/2020
ms.locfileid: "84485688"
---
# <a name="quickstart-create-an-azure-databricks-workspace-using-powershell"></a>Schnellstart: Erstellen eines Azure Databricks-Arbeitsbereichs mit PowerShell

Dieser Schnellstart beschreibt, wie Sie mit PowerShell einen Azure Databricks-Arbeitsbereich erstellen. Sie können PowerShell verwenden, um Azure-Ressourcen interaktiv oder in Skripts zu erstellen und zu verwalten.

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

Falls Sie PowerShell lokal verwenden möchten, müssen Sie für diesen Artikel das Az PowerShell-Modul installieren und mit dem Cmdlet [Connect-AzAccount](/powershell/module/az.accounts/Connect-AzAccount) eine Verbindung mit Ihrem Azure-Konto herstellen. Weitere Informationen zum Installieren des Az PowerShell-Moduls finden Sie unter [Installieren von Azure PowerShell](/powershell/azure/install-az-ps).

> [!IMPORTANT]
> Solange sich das PowerShell-Modul „Az.Databricks“ in der Vorschauphase befindet, müssen Sie es mit dem folgenden Befehl getrennt vom PowerShell-Modul „Az“ installieren: `Install-Module -Name Az.Databricks -AllowPrerelease`.
> Sobald das PowerShell-Modul „Az.Databricks“ allgemein verfügbar ist, wird es in künftige Releases des PowerShell-Moduls „Az“ integriert und in Azure Cloud Shell nativ zur Verfügung gestellt.

Wenn Sie Azure Databricks zum ersten Mal nutzen, müssen Sie den Ressourcenanbieter **Microsoft.Databricks** registrieren.

```azurepowershell-interactive
Register-AzResourceProvider -ProviderNamespace Microsoft.Databricks
```

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

Wenn Sie über mehrere Azure-Abonnements verfügen, müssen Sie das entsprechende Abonnement auswählen, in dem die Ressourcen fakturiert werden sollen. Wählen Sie mit dem Cmdlet [Set-AzContext](/powershell/module/az.accounts/set-azcontext) eine bestimmte Abonnement-ID aus.

```azurepowershell-interactive
Set-AzContext -SubscriptionId 00000000-0000-0000-0000-000000000000
```

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie mit dem Cmdlet [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) eine [Azure-Ressourcengruppe](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview). Eine Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen als Gruppe bereitgestellt und verwaltet werden.

Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen **myresourcegroup** in der Region **USA, Westen 2** erstellt.

```azurepowershell-interactive
New-AzResourceGroup -Name myresourcegroup -Location westus2
```

## <a name="create-an-azure-databricks-workspace"></a>Erstellen eines Azure Databricks-Arbeitsbereichs

In diesem Abschnitt erstellen Sie einen Azure Databricks-Arbeitsbereich mit PowerShell.

```azurepowershell-interactive
New-AzDatabricksWorkspace -Name mydatabricksws -ResourceGroupName myresourcegroup -Location westus2 -ManagedResourceGroupName databricks-group -Sku standard
```

Geben Sie außerdem die folgenden Werte an:

|       **Eigenschaft**       |                                                                                **Beschreibung**                                                                                 |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Name                     | Geben Sie einen Namen für Ihren Databricks-Arbeitsbereich an.                                                                                                                                   |
| ResourceGroupName        | Geben Sie den Namen einer vorhandenen Ressourcengruppe an.                                                                                                                                        |
| Standort                 | Wählen Sie **USA, Westen 2** aus. Informationen zu weiteren verfügbaren Regionen finden Sie unter [Verfügbare Produkte nach Region](https://azure.microsoft.com/regions/services/).                                     |
| ManagedResourceGroupName | Geben Sie an, ob Sie eine neue verwaltete Ressourcengruppe erstellen oder eine vorhandene verwenden möchten.                                                                                        |
| Sku                      | Wählen Sie zwischen **Standard**, **Premium** oder **Testversion**. Weitere Informationen zu diesen Tarifen finden Sie unter [Azure Databricks – Preise](https://azure.microsoft.com/pricing/details/databricks/). |

Die Erstellung des Arbeitsbereichs dauert einige Minuten. Nachdem dieser Prozess abgeschlossen ist, wird Ihr Benutzerkonto automatisch als Administratorbenutzer im Arbeitsbereich hinzugefügt.

Wenn bei der Bereitstellung eines Arbeitsbereichs ein Fehler auftritt, wird der Arbeitsbereich mit dem Fehlerzustand trotzdem erstellt. Löschen Sie den fehlerhaften Arbeitsbereich, und erstellen Sie einen neuen, um die Bereitstellungsfehler zu beheben. Wenn Sie den fehlerhaften Arbeitsbereich löschen, werden auch die verwaltete Ressourcengruppe und alle Ressourcen gelöscht, für die die Bereitstellung erfolgreich war.

## <a name="determine-the-provisioning-state-of-a-databricks-workspace"></a>Bestimmen des Bereitstellungsstatus eines Databricks-Arbeitsbereichs

Mithilfe des Cmdlets `Get-AzDatabricksWorkspace` können Sie feststellen, ob ein Databricks-Arbeitsbereich erfolgreich bereitgestellt wurde.

```azurepowershell-interactive
Get-AzDatabricksWorkspace -Name mydatabricksws -ResourceGroupName myresourcegroup |
  Select-Object -Property Name, SkuName, Location, ProvisioningState
```

```Output
Name            SkuName   Location  ProvisioningState
----            -------   --------  -----------------
mydatabricksws  standard  westus2   Succeeded
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die in dieser Schnellstartanleitung erstellten Ressourcen nicht für eine andere Schnellstartanleitung oder ein anderes Tutorial benötigen, können Sie sie löschen, indem Sie das folgende Beispiel ausführen.

> [!CAUTION]
> Im folgenden Beispiel werden die angegebene Ressourcengruppe und alle darin enthaltenen Ressourcen gelöscht.
> Falls in der angegebenen Ressourcengruppe Ressourcen enthalten sind, die nicht zum Umfang dieser Schnellstartanleitung gehören, werden sie ebenfalls gelöscht.

```azurepowershell-interactive
Remove-AzResourceGroup -Name myresourcegroup
```

Verwenden Sie das Cmdlet `Remove-AzDatabricksWorkspace`, um nur den in dieser Schnellstartanleitung erstellten Server ohne die Ressourcengruppe zu löschen.

```azurepowershell-interactive
Remove-AzDatabricksWorkspace -Name mydatabricksws -ResourceGroupName myresourcegroup
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen eines Spark-Clusters in Databricks](quickstart-create-databricks-workspace-portal.md#create-a-spark-cluster-in-databricks)
