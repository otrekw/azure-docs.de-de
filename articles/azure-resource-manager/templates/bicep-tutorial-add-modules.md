---
title: 'Tutorial: Hinzufügen von Variablen zu Azure Resource Manager-Bicep-Dateien'
description: Verwenden Sie Module, um komplexe Details der rohen Ressourcen-Deklaration zu kapseln.
author: mumian
ms.date: 03/25/2021
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 8c7ab1038cbe62d6f15faf56796193df12b38546
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105568765"
---
# <a name="tutorial-add-modules-to-azure-resource-manager-bicep-file"></a>Tutorial: Hinzufügen von Variablen zu Azure Resource Manager-Bicep-Dateien

Im [vorherigen Tutorial](bicep-tutorial-use-parameter-file.md) haben Sie gelernt, wie Sie eine Parameterdatei zum Bereitstellen Ihrer Bicep-Datei verwenden. In diesem Tutorial erfahren Sie, wie Sie mithilfe von Bicep-Modulen komplexe Details der rohen Ressourcendeklaration kapseln. Die Module können gemeinsam genutzt und in Ihrer Lösung wiederverwendet werden.  Dieser Schritt dauert ungefähr **12 Minuten**.

[!INCLUDE [Bicep preview](../../../includes/resource-manager-bicep-preview.md)]

## <a name="prerequisites"></a>Voraussetzungen

Wir empfehlen, das [Tutorial zur Parameterdatei](bicep-tutorial-use-parameter-file.md) abzuschließen. Dies ist jedoch keine Voraussetzung.

Sie benötigen Visual Studio Code mit der Bicep-Erweiterung sowie entweder Azure PowerShell oder die Azure-Befehlszeilenschnittstelle (Azure CLI). Weitere Informationen finden Sie unter [Bicep-Tools](bicep-tutorial-create-first-bicep.md#get-tools).

## <a name="review-bicep-file"></a>Bicep-Datei überprüfen

Am Ende des vorherigen Tutorials hatte Ihre Bicep-Datei folgenden Inhalt:

:::code language="json" source="~/resourcemanager-templates/get-started-with-templates/add-tags/azuredeploy.bicep":::

Diese Bicep-Datei funktioniert gut. Bei größeren Lösungen sollten Sie jedoch ihre Bicep-Datei in viele zugehörige Module unterteilen, damit Sie diese Module freigeben und wieder verwenden können. Mit der aktuellen Bicep-Datei werden ein Speicherkonto, ein App Service-Plan und eine Website bereitgestellt.  Trennen wir das Speicherkonto in ein Modul.

## <a name="create-bicep-module"></a>Erstellen eines Bicep-Moduls

Jede Bicep-Datei kann als Modul genutzt werden, sodass keine spezifische Syntax zum Definieren eines Moduls vorhanden ist. Erstellen Sie eine _storage.bicep_-Datei mit folgendem Inhalt:

:::code language="json" source="~/resourcemanager-templates/get-started-with-templates/add-module/storage.bicep":::

Dieses Modul enthält die Speicherkontoressource und die zugehörigen Parameter und Variablen. Die Werte für den _location_-Parameter und die _resourceTags_-Parameter wurden entfernt. Diese Werte werden aus der Haupt-Bicep-Datei übergeben.

## <a name="consume-bicep-module"></a>Bizep-Modul nutzen

Ersetzen Sie die Speicherkonto-Ressourcendefinition in der vorhandenen Datei _azuredeploy.bicep_ durch den folgenden Bicep-Inhalt:

```bicep
module stg './storage.bicep' = {
  name: 'storageDeploy'
  params: {
    storagePrefix: storagePrefix
    location: location
    resourceTags: resourceTags
  }
}
```

- **module**: Schlüsselwort.
- **symbolic name** (stg): Dies ist ein Bezeichner für das Modul.
- **module file**: Der Pfad zum Modul in diesem Beispiel wird unter Verwendung eines relativen Pfads (./storage.bicep) angegeben. Alle Pfade in Bicep müssen mithilfe des eines Schrägstrichs (/) als Verzeichnistrennzeichen angegeben werden, um eine plattformübergreifende konsistente Kompilierung sicherzustellen. Der Backslash von Windows (\)) wird nicht unterstützt.

Um den Speicherendpunkt abzurufen, aktualisieren Sie die Ausgabe in _azuredeploy.bicep_ auf das folgende Bicep:

```bicep
output storageEndpoint object = stg.outputs.storageEndpoint
```

Die abgeschlossene Datei azuredeploy.bicep hat folgenden Inhalt:

:::code language="json" source="~/resourcemanager-templates/get-started-with-templates/add-module/azuredeploy.bicep":::

## <a name="deploy-template"></a>Bereitstellen der Vorlage

Verwenden Sie entweder die Azure CLI oder Azure PowerShell, um die Vorlage bereitzustellen.

Falls Sie die Ressourcengruppe noch nicht erstellt haben, folgen Sie den Anweisungen unter [Erstellen einer Ressourcengruppe](bicep-tutorial-create-first-bicep.md#create-resource-group). Dieses Beispiel setzt voraus, dass Sie die Variable `bicepFile` wie im [ersten Tutorial](bicep-tutorial-create-first-bicep.md#deploy-bicep-file) beschrieben auf den Pfad zur Bicep-Datei festgelegt haben.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Für die Ausführung dieses Bereitstellungs-Cmdlets benötigen Sie die [aktuelle Version](/powershell/azure/install-az-ps) von Azure PowerShell.

```azurepowershell
New-AzResourceGroupDeployment `
  -Name addmodule `
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
  --name addmodule \
  --resource-group myResourceGroup \
  --template-file $bicepFile \
  --parameters storagePrefix=store storageSKU=Standard_LRS webAppName=demoapp
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

Herzlichen Glückwunsch, Sie haben diese Einführung zur Bereitstellung von Bicep-Dateien in Azure abgeschlossen. Informieren Sie uns im Abschnitt „Feedback“, wenn Sie Kommentare oder Vorschläge haben. Vielen Dank!

In der nächsten Tutorialreihe wird ausführlicher auf die Bereitstellung von Vorlagen eingegangen.

> [!div class="nextstepaction"]
> [Bereitstellen einer lokalen Vorlage](./deployment-tutorial-local-template.md)
