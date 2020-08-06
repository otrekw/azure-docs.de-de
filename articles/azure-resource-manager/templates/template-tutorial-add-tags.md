---
title: 'Tutorial: Hinzufügen von Tags zu Ressourcen in einer Vorlage'
description: Fügen Sie den Ressourcen, die Sie in Ihrer Azure Resource Manager-Vorlage bereitstellen, Tags hinzu. Mithilfe von Tags können Sie Ressourcen logisch organisieren.
author: mumian
ms.date: 03/27/2020
ms.topic: tutorial
ms.author: jgao
ms.custom: devx-track-azurecli
ms.openlocfilehash: ce25047ae93a332b62b7e785b23da5794031a98b
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2020
ms.locfileid: "87497510"
---
# <a name="tutorial-add-tags-in-your-arm-template"></a>Tutorial: Hinzufügen von Tags in Ihrer ARM-Vorlage

In diesem Tutorial wird beschrieben, wie Sie den Ressourcen in Ihrer ARM-Vorlage (Azure Resource Manager) Tags hinzufügen. [Tags](../management/tag-resources.md) helfen Ihnen dabei, Ihre Ressourcen logisch zu organisieren. Die Tagwerte werden in Kostenberichten angezeigt. Dieses Tutorial dauert ungefähr **8 Minuten**.

## <a name="prerequisites"></a>Voraussetzungen

Wir empfehlen, das [Tutorial zu Schnellstartvorlagen](template-tutorial-quickstart-template.md) durchzuführen. Dies ist jedoch keine Voraussetzung.

Sie benötigen Visual Studio Code mit der Resource Manager-Tools-Erweiterung und entweder Azure PowerShell oder die Azure-Befehlszeilenschnittstelle (Azure CLI). Weitere Informationen finden Sie unter [Vorlagentools](template-tutorial-create-first-template.md#get-tools).

## <a name="review-template"></a>Überprüfen der Vorlage

Ihre bisherige Vorlage hat ein Speicherkonto, einen App Service-Plan und eine Web-App bereitgestellt.

:::code language="json" source="~/resourcemanager-templates/get-started-with-templates/quickstart-template/azuredeploy.json":::

Nach der Bereitstellung dieser Ressourcen müssen Sie möglicherweise die Kosten nachverfolgen und nach Ressourcen suchen, die zu einer Kategorie gehören. Um dies zu ermöglichen, können Sie Tags hinzufügen.

## <a name="add-tags"></a>Hinzufügen von Tags

Sie taggen Ressourcen, um Werte hinzuzufügen, die Aufschluss über ihre Verwendung geben. Beispielsweise können Sie Tags hinzufügen, die die Umgebung und das Projekt auflisten. Sie können Tags hinzufügen, die eine Kostenstelle oder das für die Ressource zuständige Team identifizieren. Fügen Sie beliebige Werte hinzu, die für Ihre Organisation sinnvoll sind.

Im folgenden Beispiel sind die Änderungen hervorgehoben, die Sie an der Vorlage vornehmen. Kopieren Sie die gesamte Datei, und ersetzen Sie Ihre Vorlage durch den Inhalt der Datei.

:::code language="json" source="~/resourcemanager-templates/get-started-with-templates/add-tags/azuredeploy.json" range="1-118" highlight="46-52,64,78,102":::

## <a name="deploy-template"></a>Bereitstellen der Vorlage

Jetzt können Sie die Vorlage bereitstellen und sich die Ergebnisse ansehen.

Falls Sie die Ressourcengruppe noch nicht erstellt haben, folgen Sie den Anweisungen unter [Erstellen einer Ressourcengruppe](template-tutorial-create-first-template.md#create-resource-group). Dieses Beispiel setzt voraus, dass Sie die Variable **templateFile** wie im [ersten Tutorial](template-tutorial-create-first-template.md#deploy-template) beschrieben auf den Pfad zur Vorlagendatei festgelegt haben.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
New-AzResourceGroupDeployment `
  -Name addtags `
  -ResourceGroupName myResourceGroup `
  -TemplateFile $templateFile `
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
  --template-file $templateFile \
  --parameters storagePrefix=store storageSKU=Standard_LRS webAppName=demoapp
```

---

> [!NOTE]
> Wenn bei der Bereitstellung ein Fehler aufgetreten ist, verwenden Sie die Option **debug** mit dem Bereitstellungsbefehl, um die Debugprotokolle anzuzeigen.  Sie können auch die Option **verbose** verwenden, um die vollständigen Debugprotokolle anzuzeigen.

## <a name="verify-deployment"></a>Überprüfen der Bereitstellung

Sie können die Bereitstellung überprüfen, indem Sie sich die Ressourcengruppe im Azure-Portal ansehen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im linken Menü **Ressourcengruppen** aus.
1. Wählen Sie die Ressourcengruppe aus, in der Sie die Vorlage bereitgestellt haben.
1. Wählen Sie eine der Ressourcen aus, z. B. die Speicherkontoressource. Wie Sie sehen, enthält die Ressource jetzt Tags.

   ![Anzeigen von Tags](./media/template-tutorial-add-tags/show-tags.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie mit dem nächsten Tutorial fortfahren, müssen Sie die Ressourcengruppe nicht löschen.

Falls Sie keine weiteren Tutorials durchführen möchten, können Sie die bereitgestellten Ressourcen bereinigen, indem Sie die Ressourcengruppe löschen.

1. Wählen Sie im Azure-Portal im linken Menü die Option **Ressourcengruppe** aus.
2. Geben Sie den Namen der Ressourcengruppe in das Feld **Nach Name filtern** ein.
3. Klicken Sie auf den Namen der Ressourcengruppe.
4. Wählen Sie **Ressourcengruppe löschen** aus dem Menü ganz oben aus.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie den Ressourcen Tags hinzugefügt. Im nächsten Tutorial erfahren Sie, wie Sie die Übergabe von Werten an die Vorlage mithilfe von Parameterdateien vereinfachen.

> [!div class="nextstepaction"]
> [Verwenden einer Parameterdatei](template-tutorial-use-parameter-file.md)
