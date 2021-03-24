---
title: 'Tutorial: Verwenden der Parameterdatei zum Bereitstellen einer Vorlage'
description: Verwenden Sie Parameterdateien, die die Werte enthalten, die Sie für die Bereitstellung Ihrer Azure Resource Manager-Vorlage (ARM-Vorlage) verwenden möchten.
author: mumian
ms.date: 09/10/2020
ms.topic: tutorial
ms.author: jgao
ms.custom: devx-track-azurecli
ms.openlocfilehash: b6257161017afc9dab692c43fcc64e5d961a90ba
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "97368425"
---
# <a name="tutorial-use-parameter-files-to-deploy-your-arm-template"></a>Tutorial: Verwenden von Parameterdateien zum Bereitstellen Ihrer ARM-Vorlage

In diesem Tutorial erfahren Sie, wie Sie [Parameterdateien](parameter-files.md) verwenden, um die Werte zu speichern, die Sie während der Bereitstellung übergeben. In den vorherigen Tutorials haben Sie Inline-Parameter mit dem Bereitstellungs Befehl verwendet. Dieser Ansatz hat beim Testen Ihrer ARM-Vorlage (Azure Resource Manager) funktioniert, aber bei der Automatisierung von Bereitstellungen kann es einfacher sein, eine Gruppe mit Werten für Ihre Umgebung zu übergeben. Parameterdateien vereinfachen das Packen von Parameterwerten für eine bestimmte Umgebung. In diesem Tutorial erstellen Sie Parameterdateien für Entwicklungs- und Produktionsumgebungen. Dieser Schritt dauert ungefähr **12 Minuten**.

## <a name="prerequisites"></a>Voraussetzungen

Wir empfehlen, das [Tutorial zu Tags](template-tutorial-add-tags.md) durchzuführen. Dies ist jedoch keine Voraussetzung.

Sie benötigen Visual Studio Code mit der Resource Manager-Tools-Erweiterung und entweder Azure PowerShell oder die Azure-Befehlszeilenschnittstelle (Azure CLI). Weitere Informationen finden Sie unter [Vorlagentools](template-tutorial-create-first-template.md#get-tools).

## <a name="review-template"></a>Überprüfen der Vorlage

Ihre Vorlage verfügt über zahlreiche Parameter, die Sie während der Bereitstellung angeben können. Am Ende des vorherigen Tutorials sah Ihre Vorlage wie folgt aus:

:::code language="json" source="~/resourcemanager-templates/get-started-with-templates/add-tags/azuredeploy.json":::

Diese Vorlage funktioniert gut, jetzt möchten Sie jedoch problemlos die Parameter verwalten, die Sie für die Vorlage übergeben.

## <a name="add-parameter-files"></a>Parameterdateien hinzufügen

Parameterdateien sind JSON-Dateien mit einer Struktur, die ihrer Vorlage ähnelt. In der Datei geben Sie die Parameterwerte an, die Sie während der Bereitstellung übergeben möchten.

In der Parameterdatei geben Sie Werte für die Parameter in Ihrer Vorlage an. Der Name jedes Parameters in der Parameterdatei muss mit dem Namen eines Parameters in der Vorlage identisch sein. Beim Namen wird die Groß-/Kleinschreibung nicht beachtet. Um die übereinstimmenden Werte einfach anzuzeigen, sollte jedoch die Groß-/Kleinschreibung aus der Vorlage verwendet werden.

Sie müssen keinen Wert für jeden Parameter angeben. Verfügt ein nicht spezifizierter Parameter über einen Standardwert, wird dieser Wert während der Bereitstellung verwendet. Wenn ein Parameter keinen Standardwert hat und nicht in der Parameterdatei angegeben ist, werden Sie während der Bereitstellung zur Angabe eines Werts aufgefordert.

Sie können in der Parameterdatei nur Parameternamen angeben, die mit einem Parameternamen in der Vorlage identisch sind. Wenn unbekannte Parameter angegeben werden, tritt ein Fehler auf.

Erstellen Sie in Visual Studio Code eine neue Datei mit folgendem Inhalt. Speichern Sie die Datei mit dem Namen _azuredeploy.parameters.dev.json_.

:::code language="json" source="~/resourcemanager-templates/get-started-with-templates/add-tags/azuredeploy.parameters.dev.json":::

Diese Datei ist Ihre Parameterdatei für die Entwicklungsumgebung. Beachten Sie, dass **Standard_LRS** als Speicherkonto verwendet wird, Ressourcen mit dem Präfix **dev** benannt werden, und das `Environment`-Tag auf **Dev** festgelegt wird.

Erstellen Sie erneut eine neue Datei mit dem folgenden Inhalt. Speichern Sie die Datei mit dem Namen _azuredeploy.parameters.prod.json_.

:::code language="json" source="~/resourcemanager-templates/get-started-with-templates/add-tags/azuredeploy.parameters.prod.json":::

Diese Datei ist Ihre Parameterdatei für die Produktionsumgebung. Beachten Sie, dass **Standard_GRS** als Speicherkonto verwendet wird, Ressourcen mit dem Präfix **contoso** benannt werden, und das `Environment`-Tag auf **Production** festgelegt wird. In einer realen Produktionsumgebung würden Sie auch einen App-Dienst mit einer anderen SKU als „Free“ verwenden, für dieses Tutorial verwenden wir diese SKU aber weiterhin.

## <a name="deploy-template"></a>Bereitstellen der Vorlage

Verwenden Sie entweder die Azure CLI oder Azure PowerShell, um die Vorlage bereitzustellen.

Als abschließenden Test Ihrer Vorlage erstellen wir zwei neue Ressourcengruppen. Eine für die Entwicklungsumgebung und eine für die Produktionsumgebung.

Ersetzen Sie für die Vorlagen- und Parametervariablen `{path-to-the-template-file}`, `{path-to-azuredeploy.parameters.dev.json}`, `{path-to-azuredeploy.parameters.prod.json}` und die geschweiften Klammern `{}` durch Ihre Vorlagen- und Parameterdateipfade.

Zuerst führen wir die Bereitstellung in der Entwicklungsumgebung durch.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
$templateFile = "{path-to-the-template-file}"
$parameterFile="{path-to-azuredeploy.parameters.dev.json}"
New-AzResourceGroup `
  -Name myResourceGroupDev `
  -Location "East US"
New-AzResourceGroupDeployment `
  -Name devenvironment `
  -ResourceGroupName myResourceGroupDev `
  -TemplateFile $templateFile `
  -TemplateParameterFile $parameterFile
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Für die Ausführung dieses Bereitstellungsbefehls müssen Sie über die [aktuelle Version](/cli/azure/install-azure-cli) der Azure CLI verfügen.

```azurecli
templateFile="{path-to-the-template-file}"
devParameterFile="{path-to-azuredeploy.parameters.dev.json}"
az group create \
  --name myResourceGroupDev \
  --location "East US"
az deployment group create \
  --name devenvironment \
  --resource-group myResourceGroupDev \
  --template-file $templateFile \
  --parameters $devParameterFile
```

---

Jetzt stellen wir in der Produktionsumgebung bereit.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
$parameterFile="{path-to-azuredeploy.parameters.prod.json}"
New-AzResourceGroup `
  -Name myResourceGroupProd `
  -Location "West US"
New-AzResourceGroupDeployment `
  -Name prodenvironment `
  -ResourceGroupName myResourceGroupProd `
  -TemplateFile $templateFile `
  -TemplateParameterFile $parameterFile
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli
prodParameterFile="{path-to-azuredeploy.parameters.prod.json}"
az group create \
  --name myResourceGroupProd \
  --location "West US"
az deployment group create \
  --name prodenvironment \
  --resource-group myResourceGroupProd \
  --template-file $templateFile \
  --parameters $prodParameterFile
```

---

> [!NOTE]
> Wenn bei der Bereitstellung ein Fehler aufgetreten ist, verwenden Sie den Schalter `verbose`, um Informationen zu den erstellten Ressourcen abzurufen. Verwenden Sie den Schalter `debug`, um weitere Informationen zum Debuggen zu erhalten.

## <a name="verify-deployment"></a>Überprüfen der Bereitstellung

Sie können die Bereitstellung überprüfen, indem Sie sich die Ressourcengruppen im Azure-Portal ansehen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im linken Menü **Ressourcengruppen** aus.
1. Sie sehen die beiden neuen Ressourcengruppen, die Sie in diesem Tutorial bereitgestellt haben.
1. Wählen Sie eine davon aus, und zeigen Sie die bereitgestellten Ressourcen an. Beachten Sie, dass Sie mit den Werten übereinstimmen, die Sie in der Parameterdatei für diese Umgebung angegeben haben.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

1. Wählen Sie im Azure-Portal im linken Menü die Option **Ressourcengruppe** aus.
2. Geben Sie den Namen der Ressourcengruppe in das Feld **Nach Name filtern** ein. Wenn Sie diese Reihe abgeschlossen haben, verfügen Sie über drei Ressourcengruppen zum Löschen: **myResourceGroup**, **myResourceGroupDev** und **myResourceGroupProd**.
3. Klicken Sie auf den Namen der Ressourcengruppe.
4. Wählen Sie **Ressourcengruppe löschen** aus dem Menü ganz oben aus.

## <a name="next-steps"></a>Nächste Schritte

Herzlichen Glückwunsch, Sie haben diese Einführung in die Bereitstellung von Vorlagen in Azure abgeschlossen. Informieren Sie uns im Abschnitt „Feedback“, wenn Sie Kommentare oder Vorschläge haben. Vielen Dank!

In der nächsten Tutorialreihe wird ausführlicher auf die Bereitstellung von Vorlagen eingegangen.

> [!div class="nextstepaction"]
> [Bereitstellen einer lokalen Vorlage](./deployment-tutorial-local-template.md)
