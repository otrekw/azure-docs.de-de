---
title: Bereitstellen von Bicep-Dateien mit Cloud Shell
description: Verwenden Sie Azure Resource Manager und Cloud Shell, um Ressourcen in Azure bereitzustellen. Die Ressourcen sind in einer Bicep-Datei definiert.
author: mumian
ms.author: jgao
ms.topic: conceptual
ms.date: 06/01/2021
ms.openlocfilehash: fa74250a290cc3e254e5c9f3757bb3e712c020da
ms.sourcegitcommit: 7f59e3b79a12395d37d569c250285a15df7a1077
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/02/2021
ms.locfileid: "111026190"
---
# <a name="deploy-bicep-files-from-azure-cloud-shell"></a>Bereitstellen von Bicep-Dateien in Azure Cloud Shell

Sie können eine Bicep-Datei mithilfe von [Azure Cloud Shell](../../cloud-shell/overview.md) bereitstellen. Derzeit können Sie nur eine lokale Bicep-Datei aus Cloud Shell bereitstellen.

Den Umfang der Bereitstellung legen Sie selbst fest. In diesem Artikel wird gezeigt, wie Sie eine Bereitstellung in einer Ressourcengruppe durchführen.

## <a name="deploy-local-bicep-file"></a>Bereitstellen einer lokalen Bicep-Datei

Um eine lokale Bicep-Datei bereitzustellen, müssen Sie zuerst Ihre Bicep-Datei in Ihre Cloud Shell-Sitzung hochladen.

1. Melden Sie sich bei [Cloud Shell](https://shell.azure.com) an.
1. Wählen Sie entweder **PowerShell** oder **Bash** aus.

    :::image type="content" source="./media/deploy-cloud-shell/bicep-cloud-shell-bash-powershell.png" alt-text="Bash oder PowerShell auswählen":::

1. Wählen Sie **Dateien hochladen/herunterladen** und dann **Hochladen** aus.

    :::image type="content" source="./media/deploy-cloud-shell/bicep-cloud-shell-upload.png" alt-text="Hochladen einer Datei":::

1. Wählen Sie die Bicep-Datei aus, die Sie hochladen möchten, und wählen Sie dann **Öffnen** aus.
1. Führen Sie die folgenden Befehle aus, um die Bicep-Datei bereitzustellen:

   # <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

   ```azurecli-interactive
   az group create --name ExampleGroup --location "South Central US"
   az deployment group create \
     --resource-group ExampleGroup \
     --template-file azuredeploy.bicep \
     --parameters storageAccountType=Standard_GRS
   ```

   # <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

   ```azurepowershell-interactive
   New-AzResourceGroup -Name ExampleGroup -Location "Central US"
   New-AzResourceGroupDeployment `
     -DeploymentName ExampleDeployment `
     -ResourceGroupName ExampleGroup `
     -TemplateFile azuredeploy.bicep `
     -storageAccountType Standard_GRS
   ```

   ---

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu Bereitstellungsbefehlen finden Sie unter [Bereitstellen von Ressourcen mit Bicep und der Azure CLI](deploy-cli.md) und [Bereitstellen von Ressourcen mit Bicep und Azure PowerShell](deploy-powershell.md).
- Informationen zum Anzeigen einer Änderungsvorschau vor der Bereitstellung einer Bicep-Datei finden Sie unter [Was-wäre-wenn-Vorgang bei der Bereitstellung von Bicep](./deploy-what-if.md).
