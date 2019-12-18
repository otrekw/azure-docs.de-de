---
title: Bereitstellen der Azure-SSIS Integration Runtime mithilfe von PowerShell
description: Dieses PowerShell-Skript erstellt eine Azure-SSIS-Integrationslaufzeit, die SSIS-Pakete in der Cloud ausführen kann.
services: data-factory
ms.service: data-factory
ms.workload: data-services
ms.topic: article
ms.author: sawinark
author: swinarko
manager: mflasko
ms.custom: seo-lt-2019
ms.date: 09/12/2017
ms.openlocfilehash: c6d9a9299fa25013a440ee6ac45f5eae407225b8
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/08/2019
ms.locfileid: "74929821"
---
# <a name="powershell-script---deploy-azure-ssis-integration-runtime"></a>PowerShell-Skript: Bereitstellen der Azure-SSIS-Integrationslaufzeit

Dieses PowerShell-Beispielskript erstellt eine Azure-SSIS-Integrationslaufzeit, die Ihre SSIS-Pakete in Azure ausführen kann.  

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh-az.md)]

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/data-factory/deploy-azure-ssis-integration-runtime/deploy-azure-ssis-integration-runtime.ps1 "Deploy Azure-SSIS Integration Runtime")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung

Nach der Ausführung des Beispielskripts können Sie den folgenden Befehl ausführen, um die Ressourcengruppe und alle damit verbundenen Ressourcen zu entfernen:

```powershell
Remove-AzResourceGroup -ResourceGroupName $resourceGroupName
```
Führen Sie den folgenden Befehl aus, um die Data Factory aus der Ressourcengruppe zu entfernen: 

```powershell
Remove-AzDataFactoryV2 -Name $dataFactoryName -ResourceGroupName $resourceGroupName
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle:

| Get-Help | Notizen |
|---|---|
| [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [Set-AzDataFactoryV2](/powershell/module/az.datafactory/set-Azdatafactoryv2) | Erstellen einer Data Factory. |
| [Set-AzDataFactoryV2IntegrationRuntime](/powershell/module/az.datafactory/set-Azdatafactoryv2integrationruntime) | Erstellt eine Azure-SSIS-Integrationslaufzeit, die SSIS-Pakete in der Cloud ausführen kann. |
| [Start-AzDataFactoryV2IntegrationRuntime](/powershell/module/az.datafactory/start-Azdatafactoryv2integrationruntime) | Startet die Azure-SSIS-Integrationslaufzeit. |
| [Get-AzDataFactoryV2IntegrationRuntime](/powershell/module/az.datafactory/get-Azdatafactoryv2integrationruntime) | Ruft Informationen zur Azure-SSIS-Integrationslaufzeit ab. |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |
|||

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure PowerShell finden Sie in der [Azure PowerShell-Dokumentation](https://docs.microsoft.com/powershell/).

Zusätzliche PowerShell-Skriptbeispiele für Azure Data Factory finden Sie unter [Azure PowerShell-Beispiele für Azure Data Factory](../samples-powershell.md).
