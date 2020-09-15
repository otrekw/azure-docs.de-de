---
title: Verschieben von Regionen für Ressourcen in Microsoft.Resources
description: Erfahren Sie, wie Sie Ressourcen im Namespace Microsoft.Resources in neue Regionen verschieben.
ms.topic: conceptual
ms.date: 08/25/2020
ms.openlocfilehash: 098e5c36969da12966d793b6e1eddd4f79701ead
ms.sourcegitcommit: e69bb334ea7e81d49530ebd6c2d3a3a8fa9775c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2020
ms.locfileid: "88950810"
---
# <a name="move-microsoftresources-resources-to-new-region"></a>Verschieben von Ressourcen unter Microsoft.Resources in eine neue Region

Möglicherweise müssen Sie eine vorhandene Ressource in eine neue Region verschieben. In diesem Artikel wird gezeigt, wie zwei Ressourcentypen (templateSpecs und deploymentScripts), die sich im Namespace Microsoft.Resources befinden, verschoben werden.

## <a name="move-template-specs-to-new-region"></a>Verschieben von Vorlagenspezifikationen in eine neue Region

Wenn Sie über eine [Vorlagenspezifikation](../templates/template-specs.md) in einer Region verfügen und diese in eine neue Region verschieben möchten, können Sie die Vorlagenspezifikation exportieren und erneut bereitstellen.

1. Verwenden Sie den Befehl zum Exportieren einer vorhandenen Vorlagenspezifikation. Geben Sie die Parameterwerte entsprechend der zu exportierenden Vorlagenspezifikation an.

   Verwenden Sie für Azure PowerShell Folgendes:

   ```azurepowershell
   Export-AzTemplateSpec `
     -ResourceGroupName demoRG `
     -Name demoTemplateSpec `
     -Version 1.0 `
     -OutputFolder c:\export
   ```

   Verwenden Sie für die Azure-Befehlszeilenschnittstelle den folgenden Befehl:

   ```azurecli
   az template-specs export \
     --resource-group demoRG \
     --name demoTemplateSpec \
     --version 1.0 \
     --output-folder c:\export
   ```

1. Erstellen Sie mit der exportierten Vorlagenspezifikation eine neue Vorlagenspezifikation. In den folgenden Beispielen wird `westus` als neue Region angegeben, Sie können jedoch eine Region Ihrer Wahl angeben.

   Verwenden Sie für Azure PowerShell Folgendes:

   ```azurepowershell
   New-AzTemplateSpec `
     -Name movedTemplateSpec `
     -Version 1.0 `
     -ResourceGroupName newRG `
     -Location westus `
     -TemplateJsonFile c:\export\1.0.json
   ```

   Verwenden Sie für die Azure-Befehlszeilenschnittstelle den folgenden Befehl:

   ```azurecli
   az template-specs create \
     --name movedTemplateSpec \
     --version "1.0" \
     --resource-group newRG \
     --location "westus" \
     --template-file "c:\export\demoTemplateSpec.json"
   ```

## <a name="move-deployment-scripts-to-new-region"></a>Verschieben von Bereitstellungsskripts in eine neue Region

1. Wählen Sie die Ressourcengruppe mit dem Bereitstellungsskript aus, das Sie in eine neue Region verschieben möchten.

1. [Exportieren Sie die Vorlage](../templates/export-template-portal.md). Wählen Sie beim Exportieren das Bereitstellungsskript und alle anderen erforderlichen Ressourcen aus.

1. Löschen Sie in der exportierten Vorlage die folgenden Eigenschaften:

   * tenantId
   * principalId
   * clientId

1. Die exportierte Vorlage weist einen hartcodierten Wert für die Region des Bereitstellungsskripts auf.

   ```json
   "location": "westus2",
   ```

   Ändern Sie die Vorlage so, dass ein Parameter zum Festlegen des Standorts zugelassen wird. Weitere Informationen finden Sie unter [Festlegen des Ressourcenstandorts in einer ARM-Vorlage](../templates/resource-location.md).

   ```json
   "location": "[parameters('location')]",
   ```

1. [Stellen Sie die exportierte Vorlage bereit](../templates/deploy-powershell.md), und geben Sie eine neue Region für das Bereitstellungsskript an.

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement finden Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](move-resource-group-and-subscription.md).
* Informationen zum Verschieben von Ressourcen in eine neue Region finden Sie unter [Verschieben von Azure-Ressourcen zwischen Regionen](move-region.md).
