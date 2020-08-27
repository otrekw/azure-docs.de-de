---
title: 'Azure Resource Manager: Erstellen einer Einzeldatenbank'
description: Erstellen einer Einzeldatenbank in Azure SQL-Datenbank mithilfe einer Azure Resource Manager-Vorlage.
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: subject-armqs sqldbrb=1, devx-track-azurecli
ms.devlang: ''
ms.topic: quickstart
author: mumian
ms.author: jgao
ms.reviewer: carlrab
ms.date: 06/24/2020
ms.openlocfilehash: 2c1ae6bd69a87ad0ae941fba66785344d91e8268
ms.sourcegitcommit: d18a59b2efff67934650f6ad3a2e1fe9f8269f21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2020
ms.locfileid: "88660828"
---
# <a name="quickstart-create-a-single-database-in-azure-sql-database-using-an-arm-template"></a>Schnellstart: Erstellen einer Einzeldatenbank in Azure SQL-Datenbank mithilfe einer ARM-Vorlage

Die Erstellung einer [Einzeldatenbank](single-database-overview.md) ist die schnellste und einfachste Option zum Erstellen einer Datenbank in Azure SQL-Datenbank. In dieser Schnellstartanleitung wird gezeigt, wie Sie mit einer Azure Resource Manager-Vorlage (ARM-Vorlage) eine Einzeldatenbank erstellen.

[!INCLUDE [About Azure Resource Manager](../../../includes/resource-manager-quickstart-introduction.md)]

Wenn Ihre Umgebung die Voraussetzungen erfüllt und Sie mit der Verwendung von ARM-Vorlagen vertraut sind, klicken Sie auf die Schaltfläche **In Azure bereitstellen**. Die Vorlage wird im Azure-Portal geöffnet.

[![In Azure bereitstellen](../../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-sql-database%2Fazuredeploy.json)

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto erstellen](https://azure.microsoft.com/free/).

## <a name="review-the-template"></a>Überprüfen der Vorlage

Eine Einzeldatenbank enthält einen definierten Satz von Compute-, Arbeitsspeicher-, EA- und Speicherressourcen und verwendet eins von zwei möglichen [Kaufmodellen](purchasing-models.md). Wenn Sie eine Einzeldatenbank erstellen, legen Sie auch einen [Server](logical-servers.md) für ihre Verwaltung fest und platzieren ihn in einer [Azure-Ressourcengruppe](../../active-directory-b2c/overview.md) in einer bestimmten Region.

Die in dieser Schnellstartanleitung verwendete Vorlage stammt von der Seite mit den [Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/101-sql-database/).

:::code language="json" source="~/quickstart-templates/101-sql-database/azuredeploy.json":::

Diese Ressourcen sind in der Vorlage definiert:

- [**Microsoft.Sql/servers**](/azure/templates/microsoft.sql/servers)
- [**Microsoft.Sql/servers/databases**](/azure/templates/microsoft.sql/servers/databases)

Weitere Vorlagenbeispiele für Azure SQL-Datenbank finden Sie in [Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Sql&pageNumber=1&sort=Popular).

## <a name="deploy-the-template"></a>Bereitstellen der Vorlage

Wählen Sie **Ausprobieren** aus dem folgenden PowerShell-Codeblock, um die Azure Cloud Shell zu öffnen.

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter a project name that is used for generating resource names"
$location = Read-Host -Prompt "Enter an Azure location (i.e. centralus)"
$adminUser = Read-Host -Prompt "Enter the SQL server administrator username"
$adminPassword = Read-Host -Prompt "Enter the SQl server administrator password" -AsSecureString

$resourceGroupName = "${projectName}rg"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-sql-database/azuredeploy.json" -administratorLogin $adminUser -administratorLoginPassword $adminPassword

Read-Host -Prompt "Press [ENTER] to continue ..."
```

## <a name="validate-the-deployment"></a>Überprüfen der Bereitstellung

Informationen zum Abfragen der Datenbank finden Sie unter [Abfragen der Datenbank](single-database-create-quickstart.md#query-the-database).

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Behalten Sie die Ressourcengruppe, den Server und die Einzeldatenbank, wenn Sie mit [Nächste Schritte](#next-steps) fortfahren möchten. Die nächsten Schritte zeigen, wie Sie mithilfe verschiedener Methoden eine Verbindung mit Ihrer Datenbank herstellen und die Datenbank abfragen.

So löschen Sie die Ressourcengruppe:

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
Remove-AzResourceGroup -Name $resourceGroupName
```

## <a name="next-steps"></a>Nächste Schritte

- Erstellen Sie eine Firewallregel auf Serverebene, um über lokale Tools oder Remotetools eine Verbindung mit der Einzeldatenbank herstellen zu können. Weitere Informationen finden Sie unter [Erstellen einer Firewallregel auf Serverebene](firewall-create-server-level-portal-quickstart.md).
- Nachdem Sie eine Firewallregel auf Serverebene erstellt haben, können Sie mit verschiedenen Tools und Programmiersprachen eine [Verbindung mit Ihrer Datenbank herstellen und Abfragen ausführen](connect-query-content-reference-guide.md).
  - [Verbinden und Abfragen mit SQL Server Management Studio (SSMS)](connect-query-ssms.md)
  - [Verbinden und Abfragen mit Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/quickstart-sql-database?toc=/azure/sql-database/toc.json)
- Informationen zum Erstellen einer Einzeldatenbank mit der Azure CLI finden Sie unter [Azure CLI-Beispiele für Azure SQL-Datenbank und verwaltete SQL-Instanzen](az-cli-script-samples-content-guide.md).
- Informationen zum Erstellen einer Einzeldatenbank mit Azure PowerShell finden Sie unter [Azure PowerShell-Beispiele für Azure SQL-Datenbank](powershell-script-content-guide.md).
- Informationen zum Erstellen von ARM-Vorlagen finden Sie unter [Erstellen Ihrer ersten Vorlage](../../azure-resource-manager/templates/template-tutorial-create-first-template.md).
