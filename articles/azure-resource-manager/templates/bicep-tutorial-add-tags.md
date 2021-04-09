---
title: 'Tutorial: Hinzufügen von Tags zu Ressourcen in einer Azure Resource Manager-Bicep-Datei'
description: Fügen Sie Tags zu Ressourcen hinzu, die Sie in Ihren Bicep-Dateien bereitstellen. Mithilfe von Tags können Sie Ressourcen logisch organisieren.
author: mumian
ms.date: 03/10/2021
ms.topic: tutorial
ms.author: jgao
ms.custom: ''
ms.openlocfilehash: ea5e078eb692d002b3f86cd43663dd042d692611
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "102632600"
---
# <a name="tutorial-add-tags-in-azure-resource-manager-bicep-files"></a>Tutorial: Hinzufügen von Tags in einer Azure Resource Manager-Bicep-Datei

In diesem Tutorial erfahren Sie, wie Sie Tags zu Ressourcen in Ihren Bicep-Dateien hinzufügen. [Tags](../management/tag-resources.md) helfen Ihnen dabei, Ihre Ressourcen logisch zu organisieren. Die Tagwerte werden in Kostenberichten angezeigt. Dieses Tutorial dauert ungefähr **8 Minuten**.

[!INCLUDE [Bicep preview](../../../includes/resource-manager-bicep-preview.md)]

## <a name="prerequisites"></a>Voraussetzungen

Wir empfehlen, das [Tutorial zu Schnellstartvorlagen](bicep-tutorial-quickstart-template.md) durchzuführen. Dies ist jedoch keine Voraussetzung.

Sie benötigen Visual Studio Code mit der Bicep-Erweiterung sowie entweder Azure PowerShell oder die Azure-Befehlszeilenschnittstelle (Azure CLI). Weitere Informationen finden Sie unter [Bicep-Tools](bicep-tutorial-create-first-bicep.md#get-tools).

## <a name="review-bicep-file"></a>Bicep-Datei überprüfen

Ihre bisherige Bicep-Datei hat ein Speicherkonto, einen App Service-Plan und eine Web-App bereitgestellt.

:::code language="bicep" source="~/resourcemanager-templates/get-started-with-templates/quickstart-template/azuredeploy.bicep":::

Nach der Bereitstellung dieser Ressourcen müssen Sie möglicherweise die Kosten nachverfolgen und nach Ressourcen suchen, die zu einer Kategorie gehören. Um dies zu ermöglichen, können Sie Tags hinzufügen.

## <a name="add-tags"></a>Hinzufügen von Tags

Sie taggen Ressourcen, um Werte hinzuzufügen, die Aufschluss über ihre Verwendung geben. Beispielsweise können Sie Tags hinzufügen, die die Umgebung und das Projekt auflisten. Sie können Tags hinzufügen, die eine Kostenstelle oder das für die Ressource zuständige Team identifizieren. Fügen Sie beliebige Werte hinzu, die für Ihre Organisation sinnvoll sind.

Das folgende Beispiel zeigt die Änderungen an der Bicep-Datei. Kopieren Sie die gesamte Datei, und ersetzen Sie Ihre Bicep-Datei durch den Inhalt der Datei.

:::code language="bicep" source="~/resourcemanager-templates/get-started-with-templates/add-tags/azuredeploy.bicep" range="1-81" highlight="27-30,38,51,71":::

## <a name="deploy-bicep-file"></a>Bereitstellen der Bicep-Datei

Jetzt können Sie die Bicep-Datei bereitstellen und sich die Ergebnisse ansehen.

Falls Sie die Ressourcengruppe noch nicht erstellt haben, folgen Sie den Anweisungen unter [Erstellen einer Ressourcengruppe](bicep-tutorial-create-first-bicep.md#create-resource-group). Dieses Beispiel setzt voraus, dass Sie die Variable `bicepFile` wie im [ersten Tutorial](bicep-tutorial-create-first-bicep.md#deploy-bicep-file) beschrieben auf den Pfad zur Bicep-Datei festgelegt haben.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Für die Ausführung dieses Bereitstellungs-Cmdlets benötigen Sie die [aktuelle Version](/powershell/azure/install-az-ps) von Azure PowerShell.

```azurepowershell
New-AzResourceGroupDeployment `
  -Name addtags `
  -ResourceGroupName myResourceGroup `
  -TemplateFile $bicepFile `
  -storagePrefix "store" `
  -storageSKU Standard_LRS `
  -webAppName demoapp
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Für die Ausführung dieses Bereitstellungsbefehls müssen Sie über die [aktuelle Version](/cli/azure/install-azure-cli) der Azure CLI verfügen.

```azurecli
az deployment group create \
  --name addtags \
  --resource-group myResourceGroup \
  --template-file $bicepFile \
  --parameters storagePrefix=store storageSKU=Standard_LRS webAppName=demoapp
```

---

> [!NOTE]
> Wenn bei der Bereitstellung ein Fehler aufgetreten ist, verwenden Sie den Schalter `verbose`, um Informationen zu den erstellten Ressourcen abzurufen. Verwenden Sie den Schalter `debug`, um weitere Informationen zum Debuggen zu erhalten.

## <a name="verify-deployment"></a>Überprüfen der Bereitstellung

Sie können die Bereitstellung überprüfen, indem Sie sich die Ressourcengruppe im Azure-Portal ansehen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im linken Menü **Ressourcengruppen** aus.
1. Wählen Sie die Ressourcengruppe aus, in der Sie die Vorlage bereitgestellt haben.
1. Wählen Sie eine der Ressourcen aus, z. B. die Speicherkontoressource. Wie Sie sehen, enthält die Ressource jetzt Tags.

   ![Anzeigen von Tags](./media/bicep-tutorial-add-tags/show-tags.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie mit dem nächsten Tutorial fortfahren, müssen Sie die Ressourcengruppe nicht löschen.

Falls Sie keine weiteren Tutorials durchführen möchten, können Sie die bereitgestellten Ressourcen bereinigen, indem Sie die Ressourcengruppe löschen.

1. Wählen Sie im Azure-Portal im linken Menü die Option **Ressourcengruppe** aus.
2. Geben Sie den Namen der Ressourcengruppe in das Feld **Nach Name filtern** ein.
3. Klicken Sie auf den Namen der Ressourcengruppe.
4. Wählen Sie **Ressourcengruppe löschen** aus dem Menü ganz oben aus.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie den Ressourcen Tags hinzugefügt. Im nächsten Tutorial erfahren Sie, wie Sie die Übergabe von Werten zur Bereitstellung mithilfe von Parameterdateien vereinfachen.

> [!div class="nextstepaction"]
> [Verwenden einer Parameterdatei](bicep-tutorial-use-parameter-file.md)
