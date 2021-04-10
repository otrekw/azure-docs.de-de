---
title: 'Tutorial: Hinzufügen von Parametern zur Azure Resource Manager-Bicep-Datei'
description: Fügen Sie Parameter zu Ihrer Bicep-Datei hinzu, um sie wiederverwenden zu können.
author: mumian
ms.date: 03/10/2021
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 00df2ffc6272011127c5a1eb0c1e302011f8de5f
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "102632780"
---
# <a name="tutorial-add-parameters-to-azure-resource-manager-bicep-file"></a>Tutorial: Hinzufügen von Parametern zur Azure Resource Manager-Bicep-Datei

Im [vorherigen Tutorial](bicep-tutorial-create-first-bicep.md) haben Sie erfahren, wie Sie ein Speicherkonto bereitstellen. In diesem Tutorial wird gezeigt, wie Sie die Bicep-Datei durch Hinzufügen von Parametern verbessern. Das Tutorial dauert ungefähr **14 Minuten**.

[!INCLUDE [Bicep preview](../../../includes/resource-manager-bicep-preview.md)]

## <a name="prerequisites"></a>Voraussetzungen

Es wird empfohlen, das [Erstellen der ersten Bicep-Datei](bicep-tutorial-create-first-bicep.md) abzuschließen. Dies ist jedoch keine Voraussetzung.

Sie benötigen Visual Studio Code mit der Bicep-Erweiterung sowie entweder Azure PowerShell oder die Azure-Befehlszeilenschnittstelle (Azure CLI). Weitere Informationen finden Sie unter [Bicep-Tools](bicep-tutorial-create-first-bicep.md#get-tools).

## <a name="review-bicep-file"></a>Bicep-Datei überprüfen

Am Ende des vorherigen Tutorials sah Ihre Bicep-Datei wie folgt aus:

:::code language="bicep" source="~/resourcemanager-templates/get-started-with-templates/add-storage/azuredeploy.bicep":::

Möglicherweise haben Sie bemerkt, dass es bei dieser Bicep-Datei ein Problem gibt. Der Name des Speicherkontos ist hartcodiert. Sie können mit dieser Bicep-Datei immer nur das gleiche Speicherkonto bereitstellen. Um ein Speicherkonto mit einem anderen Namen bereitzustellen, müssten Sie eine neue Bicep-Datei erstellen, was natürlich nicht praktisch ist, wenn Sie Ihre Bereitstellungen automatisieren möchten.

## <a name="make-bicep-file-reusable"></a>Eine Bicep-Datei wiederverwendbar machen

Damit Ihre Bicep-Datei wiederverwendet werden kann, fügen Sie einen Parameter hinzu, mit dem Sie einen Speicherkontonamen übergeben können. Die folgende Bicep-Datei zeigt, was sich in der Datei geändert hat. Der Parameter `storageName` wird als Zeichenfolge identifiziert. Die maximale Länge ist auf 24 Zeichen festgelegt, damit keine zu langen Namen verwendet werden.

Kopieren Sie die gesamte Datei und ersetzen Sie sie durch den folgenden Inhalt.

:::code language="bicep" source="~/resourcemanager-templates/get-started-with-templates/add-name/azuredeploy.bicep" range="1-15" highlight="1-3,6":::

Beachten Sie, dass die Parameter direkt mithilfe ihrer Namen in Bicep referenziert werden können, im Vergleich zur Notwendigkeit von [parameters ('storagename')] in ARM-JSON-Vorlagen.

## <a name="deploy-bicep-file"></a>Bereitstellen der Bicep-Datei

Stellen wir nun die Bicep-Datei bereit. Im folgenden Beispiel wird die Bicep-Datei mit der Azure CLI oder PowerShell bereitgestellt. Dabei geben Sie den Namen des Speicherkontos als einen der Werte im Bereitstellungsbefehl an. Geben Sie für das Speicherkonto den Namen an, den Sie im vorherigen Tutorial verwendet haben.

Falls Sie die Ressourcengruppe noch nicht erstellt haben, folgen Sie den Anweisungen unter [Erstellen einer Ressourcengruppe](bicep-tutorial-create-first-bicep.md#create-resource-group). Dieses Beispiel setzt voraus, dass Sie die Variable `bicepFile` wie im [ersten Tutorial](bicep-tutorial-create-first-bicep.md#deploy-bicep-file) beschrieben auf den Pfad zur Bicep-Datei festgelegt haben.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Für die Ausführung dieses Bereitstellungs-Cmdlets benötigen Sie die [aktuelle Version](/powershell/azure/install-az-ps) von Azure PowerShell.

```azurepowershell
New-AzResourceGroupDeployment `
  -Name addnameparameter `
  -ResourceGroupName myResourceGroup `
  -TemplateFile $bicepFile `
  -storageName "{your-unique-name}"
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Für die Ausführung dieses Bereitstellungsbefehls müssen Sie über die [aktuelle Version](/cli/azure/install-azure-cli) der Azure CLI verfügen.

```azurecli
az deployment group create \
  --name addnameparameter \
  --resource-group myResourceGroup \
  --template-file $bicepFile \
  --parameters storageName={your-unique-name}
```

---

## <a name="understand-resource-updates"></a>Grundlegendes zu Ressourcenaktualisierungen

Im vorherigen Abschnitt haben Sie ein Speicherkonto mit dem Namen bereitgestellt, den Sie zuvor verwendet haben. Vielleicht fragen Sie sich, wie sich die erneute Bereitstellung auf die Ressource auswirkt.

Wenn die Ressource bereits vorhanden ist und in den Eigenschaften keine Änderungen erkannt werden, wird keine Aktion ausgeführt. Ist die Ressource bereits vorhanden und eine Eigenschaft wurde geändert, wird die Ressource aktualisiert. Wenn die Ressource nicht vorhanden ist, wird sie erstellt.

Da Aktualisierungen auf diese Weise verarbeitet werden, kann Ihre Bicep-Datei alle Ressourcen enthalten, die Sie für eine Azure-Lösung benötigen. Mit dem Wissen, dass Ressourcen nur bei Bedarf geändert oder erstellt werden, können Sie die Bicep-Datei sicher erneut bereitstellen. Wenn Sie Ihrem Speicherkonto beispielsweise Dateien hinzugefügt haben, können Sie das Speicherkonto erneut bereitstellen, ohne dass diese Dateien verloren gehen.

## <a name="customize-by-environment"></a>Anpassen nach Umgebung

Mit Parametern können Sie die Bereitstellung anpassen, indem Sie Werte angeben, die für eine bestimmte Umgebung maßgeschneidert sind. Je nachdem, ob Sie die Vorlage in einer Entwicklungs-, Test- oder Produktionsumgebung bereitstellen, können Sie beispielsweise unterschiedliche Werte übergeben.

Mit der vorherigen Bicep-Datei wurde immer ein **Standard_LRS**-Speicherkonto bereitgestellt. Möglicherweise möchten Sie in der Lage sein, je nach Umgebung flexibel verschiedene SKUs bereitzustellen. Das folgende Beispiel zeigt die Änderungen, die Sie zum Hinzufügen eines Parameters für die SKU vornehmen. Kopieren Sie die gesamte Datei, und überschreiben Sie damit Ihre Bicep-Datei.

:::code language="bicep" source="~/resourcemanager-templates/get-started-with-templates/add-sku/azuredeploy.bicep" range="1-27" highlight="5-15,21":::

Der Parameter `storageSKU` hat einen Standardwert. Dieser Wert wird verwendet, wenn während der Bereitstellung kein Wert angegeben wird. Außerdem verfügt er über eine Liste zulässiger Werte. Diese entsprechen den Werten, die zum Erstellen eines Speicherkontos erforderlich sind, und verhindern, dass Benutzer Ihrer Bicep-Datei SKUs übergeben, die nicht funktionieren.

## <a name="redeploy-bicep-file"></a>Erneutes Bereitstellen der Bicep-Datei

Jetzt können Sie Ihre Vorlage erneut bereitstellen. Da die Standard-SKU auf **Standard_LRS** festgelegt ist, müssen Sie keinen Wert für diesen Parameter angeben.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
New-AzResourceGroupDeployment `
  -Name addskuparameter `
  -ResourceGroupName myResourceGroup `
  -TemplateFile $bicepFile `
  -storageName "{your-unique-name}"
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli
az deployment group create \
  --name addskuparameter \
  --resource-group myResourceGroup \
  --template-file $bicepFile \
  --parameters storageName={your-unique-name}
```

---

> [!NOTE]
> Wenn bei der Bereitstellung ein Fehler aufgetreten ist, verwenden Sie den Schalter `verbose`, um Informationen zu den erstellten Ressourcen abzurufen. Verwenden Sie den Schalter `debug`, um weitere Informationen zum Debuggen zu erhalten.

Damit Sie sehen, wie flexibel Ihre Bicep-Datei ist, stellen Sie sie nun nochmal bereit. Dieses Mal legen Sie den SKU-Parameter auf **Standard_GRS** fest. Sie können entweder einen neuen Namen übergeben, um ein anderes Speicherkonto zu erstellen, oder den gleichen Namen verwenden, um Ihr vorhandenes Speicherkonto zu aktualisieren. Beide Optionen funktionieren.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
New-AzResourceGroupDeployment `
  -Name usenondefaultsku `
  -ResourceGroupName myResourceGroup `
  -TemplateFile $bicepFile `
  -storageName "{your-unique-name}" `
  -storageSKU Standard_GRS
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli
az deployment group create \
  --name usenondefaultsku \
  --resource-group myResourceGroup \
  --template-file $bicepFile \
  --parameters storageSKU=Standard_GRS storageName={your-unique-name}
```

---

Zum Schluss überprüfen Sie mithilfe eines weiteren Tests, was passiert, wenn Sie eine SKU übergeben, die nicht in der Liste zulässiger Werte enthalten ist. In diesem Fall testen Sie das Szenario, in dem ein Benutzer Ihrer Bicep-Datei glaubt, dass **basic** eine der zulässigen SKUs ist.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
New-AzResourceGroupDeployment `
  -Name testskuparameter `
  -ResourceGroupName myResourceGroup `
  -TemplateFile $bicepFile `
  -storageName "{your-unique-name}" `
  -storageSKU basic
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli
az deployment group create \
  --name testskuparameter \
  --resource-group myResourceGroup \
  --template-file $bicepFile \
  --parameters storageSKU=basic storageName={your-unique-name}
```

---

Der Befehl schlägt sofort fehl, und es wird eine Fehlermeldung mit den zulässigen Werten angezeigt. Resource Manager erkennt den Fehler vor dem Start der Bereitstellung.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie mit dem nächsten Tutorial fortfahren, müssen Sie die Ressourcengruppe nicht löschen.

Falls Sie keine weiteren Tutorials durchführen möchten, können Sie die bereitgestellten Ressourcen bereinigen, indem Sie die Ressourcengruppe löschen.

1. Wählen Sie im Azure-Portal im linken Menü die Option **Ressourcengruppe** aus.
2. Geben Sie den Namen der Ressourcengruppe in das Feld **Nach Name filtern** ein.
3. Klicken Sie auf den Namen der Ressourcengruppe.
4. Wählen Sie **Ressourcengruppe löschen** aus dem Menü ganz oben aus.

## <a name="next-steps"></a>Nächste Schritte

Sie haben die im [ersten Tutorial](bicep-tutorial-create-first-bicep.md) erstellte Bicep-Datei verbessert, indem Sie ihr Parameter hinzugefügt haben. Im nächsten Tutorial befassen Sie sich mit Bicep-Funktionen.

> [!div class="nextstepaction"]
> [Funktionen hinzufügen](bicep-tutorial-add-functions.md)
