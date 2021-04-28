---
title: 'Tutorial: Verwenden der Parameterdatei zum Bereitstellen einer Azure Resource Manager-Bicep-Datei'
description: Verwenden Sie Parameterdateien, die die Werte enthalten, die Sie für die Bereitstellung Ihrer Azure Resource Manager-Bicep-Datei verwenden möchten.
author: mumian
ms.date: 04/27/2021
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 73af451292462d9963720924df7718116a78baaa
ms.sourcegitcommit: 2e123f00b9bbfebe1a3f6e42196f328b50233fc5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/27/2021
ms.locfileid: "108071485"
---
# <a name="tutorial-use-parameter-files-to-deploy-azure-resource-manager-bicep-file"></a>Tutorial: Verwenden von Parameterdateien zum Bereitstellen einer Azure Resource Manager-Bicep-Datei

In diesem Tutorial erfahren Sie, wie Sie [Parameterdateien](parameter-files.md) verwenden, um die Werte zu speichern, die Sie während der Bereitstellung übergeben. In den vorherigen Tutorials haben Sie Inline-Parameter mit dem Bereitstellungs Befehl verwendet. Dieser Ansatz funktionierte beim Testen Ihrer Bicep-Datei, aber bei der Automatisierung von Bereitstellungen kann es einfacher sein, eine Gruppe von Werten für Ihre Umgebung zu übergeben. Parameterdateien vereinfachen das Packen von Parameterwerten für eine bestimmte Umgebung. Beim Bereitstellen einer JSON-Vorlage verwenden Sie dieselbe JSON-Parameterdatei. In diesem Tutorial erstellen Sie Parameterdateien für Entwicklungs- und Produktionsumgebungen. Dieser Schritt dauert ungefähr **12 Minuten**.

[!INCLUDE [Bicep preview](../../../includes/resource-manager-bicep-preview.md)]

## <a name="prerequisites"></a>Voraussetzungen

Wir empfehlen, das [Tutorial zu Tags](bicep-tutorial-add-tags.md) durchzuführen. Dies ist jedoch keine Voraussetzung.

Sie benötigen Visual Studio Code mit der Bicep-Erweiterung sowie entweder Azure PowerShell oder die Azure-Befehlszeilenschnittstelle (Azure CLI). Weitere Informationen finden Sie unter [Bicep-Tools](bicep-tutorial-create-first-bicep.md#get-tools).

## <a name="review-bicep-file"></a>Bicep-Datei überprüfen

Ihre Bicep-Datei verfügt über zahlreiche Parameter, die Sie während der Bereitstellung angeben können. Am Ende des vorherigen Tutorials sah Ihre Bicep-Datei wie folgt aus:

:::code language="json" source="~/resourcemanager-templates/get-started-with-templates/add-tags/azuredeploy.bicep":::

Diese Bicep-Datei funktioniert gut, jetzt möchten Sie jedoch problemlos die Parameter verwalten, die Sie für die Bicep-Datei übergeben.

## <a name="add-parameter-files"></a>Parameterdateien hinzufügen

Parameterdateien sind JSON-Dateien mit einer Struktur, die JSON-Vorlagen ähnelt. In der Datei geben Sie die Parameterwerte an, die Sie während der Bereitstellung übergeben möchten.

In der Parameterdatei geben Sie Werte für die Parameter in Ihrer Bicep-Datei an. Der Name jedes Parameters in der Parameterdatei muss mit dem Namen eines Parameters in der Bicep-Datei identisch sein. Beim Namen wird die Groß-/Kleinschreibung nicht beachtet. Um die übereinstimmenden Werte leichter zu erkennen, sollte jedoch die Groß-/Kleinschreibung aus der Bicep-Datei verwendet werden.

Sie müssen keinen Wert für jeden Parameter angeben. Verfügt ein nicht spezifizierter Parameter über einen Standardwert, wird dieser Wert während der Bereitstellung verwendet. Wenn ein Parameter keinen Standardwert hat und nicht in der Parameterdatei angegeben ist, werden Sie während der Bereitstellung zur Angabe eines Werts aufgefordert.

Sie können in der Parameterdatei nur Parameternamen angeben, die mit einem Parameternamen in der Bicep-Datei identisch sind. Wenn unbekannte Parameter angegeben werden, tritt ein Fehler auf.

Erstellen Sie in Visual Studio Code eine neue Datei mit folgendem Inhalt. Speichern Sie die Datei mit dem Namen _azuredeploy.parameters.dev.json_.

:::code language="json" source="~/resourcemanager-templates/get-started-with-templates/add-tags/azuredeploy.parameters.dev.json":::

Diese Datei ist Ihre Parameterdatei für die Entwicklungsumgebung. Beachten Sie, dass **Standard_LRS** als Speicherkonto verwendet wird, Ressourcen mit dem Präfix **dev** benannt werden, und das `Environment`-Tag auf **Dev** festgelegt wird.

Erstellen Sie erneut eine neue Datei mit dem folgenden Inhalt. Speichern Sie die Datei mit dem Namen _azuredeploy.parameters.prod.json_.

:::code language="json" source="~/resourcemanager-templates/get-started-with-templates/add-tags/azuredeploy.parameters.prod.json":::

Diese Datei ist Ihre Parameterdatei für die Produktionsumgebung. Beachten Sie, dass **Standard_GRS** als Speicherkonto verwendet wird, Ressourcen mit dem Präfix **contoso** benannt werden, und das `Environment`-Tag auf **Production** festgelegt wird. In einer realen Produktionsumgebung würden Sie auch einen App-Dienst mit einer anderen SKU als „Free“ verwenden, für dieses Tutorial verwenden wir diese SKU aber weiterhin.

## <a name="deploy-bicep-file"></a>Bereitstellen der Bicep-Datei

Verwenden Sie entweder die Azure CLI oder Azure PowerShell zum Bereitstellen der Bicep-Datei.

In diesem Tutorial erstellen wir zwei neue Ressourcengruppen. Eine für die Entwicklungsumgebung und eine für die Produktionsumgebung.

Ersetzen Sie für die Vorlagen- und Parametervariablen `{path-to-the-bicep-file}`, `{path-to-azuredeploy.parameters.dev.json}`, `{path-to-azuredeploy.parameters.prod.json}` und die geschweiften Klammern `{}` durch Ihre Bicep-Datei- und Parameterdateipfade.

Zuerst führen wir die Bereitstellung in der Entwicklungsumgebung durch.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Für die Ausführung dieses Bereitstellungs-Cmdlets benötigen Sie die [aktuelle Version](/powershell/azure/install-az-ps) von Azure PowerShell.

```azurepowershell
$bicepFile = "{path-to-the-bicep-file}"
$parameterFile="{path-to-azuredeploy.parameters.dev.json}"
New-AzResourceGroup `
  -Name myResourceGroupDev `
  -Location "East US"
New-AzResourceGroupDeployment `
  -Name devenvironment `
  -ResourceGroupName myResourceGroupDev `
  -TemplateFile $bicepFile `
  -TemplateParameterFile $parameterFile
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Für die Ausführung dieses Bereitstellungsbefehls müssen Sie über die [aktuelle Version](/cli/azure/install-azure-cli) der Azure CLI verfügen.

```azurecli
bicepFile="{path-to-the-bicep-file}"
devParameterFile="{path-to-azuredeploy.parameters.dev.json}"
az group create \
  --name myResourceGroupDev \
  --location "East US"
az deployment group create \
  --name devenvironment \
  --resource-group myResourceGroupDev \
  --template-file $bicepFile \
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
  -TemplateFile $bicepFile `
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
  --template-file $bicepFile \
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
2. Geben Sie den Namen der Ressourcengruppe in das Feld **Nach Name filtern** ein.
3. Klicken Sie auf den Namen der Ressourcengruppe.
4. Wählen Sie **Ressourcengruppe löschen** aus dem Menü ganz oben aus.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial verwenden Sie eine Parameterdatei zum Bereitstellen der Bicep-Datei. Im nächsten Tutorial erfahren Sie, wie Sie Ihre Bicep-Dateien modularisieren.

> [!div class="nextstepaction"]
> [Hinzufügen von Modulen](./bicep-tutorial-add-modules.md)
