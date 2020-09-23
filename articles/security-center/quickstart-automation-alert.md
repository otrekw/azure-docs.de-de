---
title: Erstellen einer Sicherheitsautomatisierung für bestimmte Sicherheitswarnungen mithilfe einer ARM-Vorlage (Azure Resource Manager)
description: Hier erfahren Sie, wie Sie mithilfe einer ARM-Vorlage (Azure Resource Manager) eine Azure Security Center-Automatisierung zum Auslösen einer Logik-App erstellen.
services: azure-resource-manager
author: memildin
ms.service: azure-resource-manager
ms.topic: quickstart
ms.custom: subject-armqs
ms.author: memildin
ms.date: 08/20/2020
ms.openlocfilehash: 6c8a3d6c291435a379a637707a42f9ad7fe8dc00
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90906353"
---
# <a name="quickstart-create-an-automatic-response-to-a-specific-security-alert-using-an-azure-resource-manager-template-arm-template"></a>Schnellstart: Erstellen einer automatischen Antwort auf eine bestimmte Sicherheitswarnung mithilfe einer ARM-Vorlage (Azure Resource Manager)

In dieser Schnellstartanleitung wird beschrieben, wie Sie mithilfe einer ARM-Vorlage (Azure Resource Manager) eine Workflowautomatisierung erstellen, die eine Logik-App auslöst, wenn von Azure Security Center bestimmte Sicherheitswarnungen empfangen werden.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Wenn Ihre Umgebung die Voraussetzungen erfüllt und Sie mit der Verwendung von ARM-Vorlagen vertraut sind, klicken Sie auf die Schaltfläche **In Azure bereitstellen**. Die Vorlage wird im Azure-Portal geöffnet.

[![In Azure bereitstellen](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3a%2f%2fraw.githubusercontent.com%2fAzure%2fazure-quickstart-templates%2fmaster%2f101-securitycenter-create-automation-for-alertnamecontains%2fazuredeploy.json)


## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

Eine Liste der Rollen und Berechtigungen, die für die Arbeit mit dem Azure Security Center-Feature für die Workflowautomatisierung erforderlich sind, finden Sie unter [Workflowautomatisierung](workflow-automation.md).


## <a name="review-the-template"></a>Überprüfen der Vorlage

Die in dieser Schnellstartanleitung verwendete Vorlage stammt von der Seite mit den [Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/101-securitycenter-create-automation-for-alertnamecontains/).

:::code language="json" source="~/quickstart-templates/101-securitycenter-create-automation-for-alertnamecontains/azuredeploy.json":::

### <a name="relevant-resources"></a>Relevante Ressourcen (möglicherweise nur in englischer Sprache)

- [**Microsoft.Security/automations**](https://github.com/Azure/azure-quickstart-templates/tree/master/101-securitycenter-create-automation-for-alertnamecontains/#microsoftsecurity-resource-provider): Die Automatisierung, die bei Eingang einer Azure Security Center-Warnung mit einer bestimmten Zeichenfolge die Logik-App auslöst
- [**Microsoft.Logic/workflows**](https://github.com/Azure/azure-quickstart-templates/tree/master/101-securitycenter-create-automation-for-alertnamecontains/#microsoftlogic-resource-provider): Eine leere auslösbare Logik-App.

Weitere von der Community bereitgestellte Security Center-Schnellstartvorlagen finden Sie [hier](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Security).

## <a name="deploy-the-template"></a>Bereitstellen der Vorlage

  - **PowerShell**:
    ```azurepowershell-interactive
    New-AzResourceGroup -Name <resource-group-name> -Location <resource-group-location> #use this command when you need to create a new resource group for your deployment
    New-AzResourceGroupDeployment -ResourceGroupName <resource-group-name> -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-securitycenter-create-automation-for-alertnamecontains/azuredeploy.json
    ```

  - **CLI**:
    ```azurecli-interactive
    az group create --name <resource-group-name> --location <resource-group-location> #use this command when you need to create a new resource group for your deployment
    az group deployment create --resource-group <my-resource-group> --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-securitycenter-create-automation-for-alertnamecontains/azuredeploy.json
    ```

  - **Portal**:

    [![In Azure bereitstellen](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3a%2f%2fraw.githubusercontent.com%2fAzure%2fazure-quickstart-templates%2fmaster%2f101-securitycenter-create-automation-for-alertnamecontains%2fazuredeploy.json)

    Weitere Informationen zu dieser Bereitstellungsoption finden Sie unter [Verwenden einer Bereitstellungsschaltfläche zum Bereitstellen von Vorlagen aus einem GitHub-Repository](https://docs.microsoft.com/azure/azure-resource-manager/templates/deploy-to-azure-button).

## <a name="review-deployed-resources"></a>Überprüfen der bereitgestellten Ressourcen

Überprüfen Sie mithilfe des Azure-Portals, ob die Workflowautomatisierung bereitgestellt wurde. 

1. Öffnen Sie **Security Center** über das [Azure-Portal](https://portal.azure.com).
1. Wählen Sie auf der oberen Menüleiste das Filtersymbol aus, und wählen Sie das Abonnement aus, in dem Sie die neue Workflowautomatisierung bereitgestellt haben.
1. Öffnen Sie auf der Seitenleiste von Security Center **Workflowautomatisierung**, und suchen Sie nach der neuen Automatisierung.
    :::image type="content" source="./media/quickstart-automation-alert/validating-template-run.png" alt-text="Liste konfigurierter Automatisierungen" lightbox="./media/quickstart-automation-alert/validating-template-run.png":::
    >[!TIP]
    > Wenn Ihr Abonnement viele Workflowautomatisierungen enthält, verwenden Sie die Option **Nach Name filtern**. 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Workflowautomatisierung mithilfe des Azure-Portals, wenn Sie sie nicht mehr benötigen.

1. Öffnen Sie **Security Center** über das [Azure-Portal](https://portal.azure.com).
1. Wählen Sie auf der oberen Menüleiste das Filtersymbol aus, und wählen Sie das Abonnement aus, in dem Sie die neue Workflowautomatisierung bereitgestellt haben.
1. Öffnen Sie auf der Seitenleiste von Security Center **Workflowautomatisierung**, und suchen Sie nach der zu löschenden Automatisierung.
    :::image type="content" source="./media/quickstart-automation-alert/deleting-workflow-automation.png" alt-text="Schritte zum Entfernen einer Workflowautomatisierung" lightbox="./media/quickstart-automation-alert/deleting-workflow-automation.png":::
1. Aktivieren Sie das Kontrollkästchen für das zu löschende Element.
1. Wählen Sie auf der Symbolleiste die Option **Löschen** aus.


## <a name="next-steps"></a>Nächste Schritte

Ein Schritt-für-Schritt-Tutorial mit Anleitungen zum Erstellen einer Vorlage finden Sie unter folgendem Link:

> [!div class="nextstepaction"]
> [Tutorial: Erstellen und Bereitstellen Ihrer ersten ARM-Vorlage](/azure/azure-resource-manager/templates/template-tutorial-create-first-template)