---
title: 'Azure Resource Manager: Erstellen einer verwalteten Azure SQL-Instanz'
description: Erfahren Sie, wie Sie mit einer Azure Resource Manager-Vorlage eine verwaltete Azure SQL-Instanz erstellen.
services: sql-database
ms.service: sql-managed-instance
ms.subservice: operations
ms.custom: subject-armqs
ms.devlang: ''
ms.topic: quickstart
author: stevestein
ms.author: sstein
ms.reviewer: carlrab
ms.date: 06/22/2020
ms.openlocfilehash: 01c6c37d31d41f88b370face372555536724adde
ms.sourcegitcommit: bf99428d2562a70f42b5a04021dde6ef26c3ec3a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2020
ms.locfileid: "85256070"
---
# <a name="quickstart-create-an-azure-sql-managed-instance-using-an-azure-resource-manager-template"></a>Schnellstart: Erstellen einer verwalteten Azure SQL-Instanz mit einer Azure Resource Manager-Vorlage

In dieser Schnellstartanleitung geht es um die Bereitstellung einer Resource Manager-Vorlage zum Erstellen einer verwalteten Azure SQL-Instanz und eines VNET.

[!INCLUDE [About Azure Resource Manager](../../../includes/resource-manager-quickstart-introduction.md)]

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto erstellen](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Voraussetzungen

Keine.

## <a name="create-an-azure-sql-managed-instance"></a>Erstellen einer verwalteten Azure SQL-Instanz

[Azure SQL Managed Instance](sql-managed-instance-paas-overview.md) ist eine intelligente, vollständig verwaltete und skalierbare Clouddatenbank mit fast 100%iger Featureparität mit der SQL Server-Datenbank-Engine.

### <a name="review-the-template"></a>Überprüfen der Vorlage

Die in dieser Schnellstartanleitung verwendete Vorlage stammt von der Seite mit den [Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/101-sqlmi-new-vnet/).

:::code language="json" source="~/quickstart-templates/101-sqlmi-new-vnet/azuredeploy.json":::

Diese Ressourcen sind in der Vorlage definiert:

- [**Microsoft.Sql/managedinstances**](/azure/templates/microsoft.sql/managedinstances)
- [**Microsoft.Network/virtualNetworks**](/azure/templates/microsoft.Network/virtualNetworks)
- [**Microsoft.Network/routeTables**](/azure/templates/microsoft.Network/routeTables)
- [**Microsoft.Network/networkSecurityGroups**](/azure/templates/microsoft.Network/networkSecurityGroups)



Weitere Vorlagenbeispiele finden Sie in den [Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Sql&pageNumber=1&sort=Popular).

## <a name="deploy-the-template"></a>Bereitstellen der Vorlage

Wählen Sie **Ausprobieren** aus dem folgenden PowerShell-Codeblock, um die Azure Cloud Shell zu öffnen.

> [!IMPORTANT]
> Die Bereitstellung einer verwalteten Instanz ist ein Vorgang mit langer Ausführungsdauer. Die Bereitstellung der ersten Instanz im Subnetz dauert normalerweise deutlich länger als die Bereitstellung in einem Subnetz, das bereits verwaltete Instanzen enthält. Informationen zur durchschnittlichen Bereitstellungsdauer finden Sie unter [Verwaltungsvorgänge für verwaltete SQL-Instanzen](sql-managed-instance-paas-overview.md#management-operations).

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter a project name that is used for generating resource names"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$templateUri = "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-sqlmi-new-vnet/azuredeploy.json"

$resourceGroupName = "${projectName}rg"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri $templateUri

Read-Host -Prompt "Press [ENTER] to continue ..."
```

# <a name="the-azure-cli"></a>[Die Azure-CLI](#tab/azure-cli)

```azurecli-interactive
read -p "Enter a project name that is used for generating resource names:" projectName &&
read -p "Enter the location (i.e. centralus):" location &&
templateUri="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-sqlmi-new-vnet/azuredeploy.json" &&
resourceGroupName="${projectName}rg" &&
az group create --name $resourceGroupName --location "$location" &&
az deployment group create --resource-group $resourceGroupName --template-uri  $templateUri &&
echo "Press [ENTER] to continue ..." &&
read
```

* * *

## <a name="review-deployed-resources"></a>Überprüfen der bereitgestellten Ressourcen

Rufen Sie das [Azure-Portal](https://portal.azure.com/#blade/HubsExtension/BrowseResourceGroups) auf, und überprüfen Sie, ob sich die verwaltete Instanz in der ausgewählten Ressourcengruppe befindet. Da das Erstellen einer verwalteten Instanz einige Zeit in Anspruch nehmen kann, müssen Sie möglicherweise den Link **Bereitstellungen** auf der Seite **Übersicht** für die Ressourcengruppe überprüfen.

- Eine Schnellstartanleitung, die zeigt, wie Sie eine Verbindung zwischen einem virtuellen Azure-Computer und Azure SQL Managed Instance herstellen, finden Sie unter [Konfigurieren einer Verbindung zu einem virtuellen Azure-Computer](connect-vm-instance-configure.md).
- Eine Schnellstartanleitung, die zeigt, wie Sie von einem lokalen Clientcomputer über eine Point-to-Site-Verbindung eine Verbindung mit Azure SQL Managed Instance herstellen, finden Sie unter [Konfigurieren einer Point-to-Site-Verbindung](point-to-site-p2s-configure.md).

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie mit [Nächste Schritte](#next-steps) fortfahren möchten, behalten Sie die verwaltete Instanz bei, doch löschen Sie nach Beendigung der weiteren Tutorials die verwaltete Instanz und zugehörige Ressourcen. Nachdem Sie eine verwaltete Instanz gelöscht haben, sehen Sie sich die Informationen unter [Löschen eines Subnetzes nach dem Löschen einer verwalteten Instanz](virtual-cluster-delete.md) an.


So löschen Sie die Ressourcengruppe:

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
Remove-AzResourceGroup -Name $resourceGroupName
```

# <a name="the-azure-cli"></a>[Die Azure-CLI](#tab/azure-cli)

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group delete --name $resourceGroupName
```

* * *

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Konfigurieren einer Azure-VM für das Herstellen einer Verbindung mit Azure SQL Managed Instance](connect-vm-instance-configure.md)
