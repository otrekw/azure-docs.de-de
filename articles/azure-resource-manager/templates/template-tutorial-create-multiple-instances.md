---
title: Erstellen mehrerer Ressourceninstanzen
description: Erfahren Sie, wie Sie eine Azure Resource Manager-Vorlage erstellen, um mehrere Azure-Ressourceninstanzen bereitzustellen.
author: mumian
ms.date: 04/08/2020
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 83afff3aa15caa1743f66eea9eaee541492b8d1c
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "81260835"
---
# <a name="tutorial-create-multiple-resource-instances-with-arm-templates"></a>Tutorial: Erstellen mehrerer Ressourceninstanzen mit ARM-Vorlagen

Es wird beschrieben, wie Sie die ARM-Vorlage (Azure Resource Manager) durchlaufen können, um mehrere Instanzen einer Azure-Ressource zu erstellen. In diesem Tutorial ändern Sie eine Vorlage, um drei Speicherkontoinstanzen zu erstellen.

![Diagramm: Erstellen mehrerer Instanzen in Azure Resource Manager](./media/template-tutorial-create-multiple-instances/resource-manager-template-create-multiple-instances-diagram.png)

Dieses Tutorial enthält die folgenden Aufgaben:

> [!div class="checklist"]
> * Öffnen einer Schnellstartvorlage
> * Bearbeiten der Vorlage
> * Bereitstellen der Vorlage

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Damit Sie die Anweisungen in diesem Artikel ausführen können, benötigen Sie Folgendes:

* Visual Studio Code mit der Erweiterung „Azure Resource Manager-Tools“. Weitere Informationen finden Sie unter [Verwenden von Visual Studio Code für die Erstellung von ARM-Vorlagen](use-vs-code-to-create-template.md).

## <a name="open-a-quickstart-template"></a>Öffnen einer Schnellstartvorlage

[Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/) ist ein Repository für ARM-Vorlagen. Statt eine Vorlage von Grund auf neu zu erstellen, können Sie eine Beispielvorlage verwenden und diese anpassen. Die in dieser Schnellstartanleitung verwendete Vorlage heißt [Standardspeicherkonto erstellen](https://azure.microsoft.com/resources/templates/101-storage-account-create/). Die Vorlage definiert eine Azure Storage-Kontoressource.

1. Wählen Sie in Visual Studio Code **Datei**>**Datei öffnen** aus.
2. Fügen Sie in **Dateiname** die folgende URL ein:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
    ```
3. Wählen Sie **Öffnen** aus, um die Datei zu öffnen.
4. In der Vorlage ist die Ressource „Microsoft.Storage/storageAccounts“ definiert. Vergleichen Sie die Vorlage mit der [Vorlagenreferenz](https://docs.microsoft.com/azure/templates/Microsoft.Storage/storageAccounts). Bevor Sie die Vorlage anpassen, sollten Sie sich zunächst grundlegend damit vertraut machen.
5. Wählen Sie **Datei**>**Speichern unter** aus, um die Datei als **azuredeploy.json** auf dem lokalen Computer zu speichern.

## <a name="edit-the-template"></a>Bearbeiten der Vorlage

Die vorhandene Vorlage erstellt ein einzelnes Speicherkonto. Sie passen die Vorlage an, um drei Speicherkonten zu erstellen.

Nehmen Sie von Visual Studio Code aus die folgenden vier Änderungen vor:

![Erstellen mehrerer Instanzen in Azure Resource Manager](./media/template-tutorial-create-multiple-instances/resource-manager-template-create-multiple-instances.png)

1. Fügen Sie der Ressourcendefinition des Speicherkontos ein `copy`-Element hinzu. Im copy-Element geben Sie die Anzahl von Iterationen und eine Variable für diese Schleife an. Der count-Wert muss eine positive ganze Zahl sein und darf 800 nicht überschreiten.
2. Die `copyIndex()`-Funktion gibt die aktuelle Iteration in der Schleife zurück. Verwenden Sie den Index als Namenspräfix. `copyIndex()` ist nullbasiert. Zum Versetzen des Indexwerts können Sie einen Wert in der copyIndex()-Funktion übergeben. Beispiel: *copyIndex(1)* .
3. Löschen Sie das **variables**-Element, da es nicht mehr verwendet wird.
4. Löschen Sie das **outputs**-Element. Es wird nicht mehr benötigt.

Die fertige Vorlage sieht folgendermaßen aus:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccountType": {
      "type": "string",
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS",
        "Standard_ZRS",
        "Premium_LRS"
      ],
      "metadata": {
        "description": "Storage Account type"
      }
    },
    "location": {
      "type": "string",
      "defaultValue": "[resourceGroup().location]",
      "metadata": {
        "description": "Location for all resources."
      }
    }
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2019-04-01",
      "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
      "location": "[parameters('location')]",
      "sku": {
        "name": "[parameters('storageAccountType')]"
      },
      "kind": "StorageV2",
      "copy": {
        "name": "storagecopy",
        "count": 3
      },
      "properties": {}
    }
  ]
}
```

Weitere Informationen zum Erstellen mehrerer Instanzen finden Sie unter [Bereitstellen mehrerer Instanzen einer Ressource oder Eigenschaft in ARM-Vorlagen](./copy-resources.md).

## <a name="deploy-the-template"></a>Bereitstellen der Vorlage

Informationen zum Bereitstellungsverfahren finden Sie in der Visual Studio Code-Schnellstartanleitung im Abschnitt [Bereitstellen der Vorlage](quickstart-create-templates-use-visual-studio-code.md#deploy-the-template).

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Lassen Sie zum Auflisten aller drei Speicherkonten den --name-Parameter aus:

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli
echo "Enter a project name that is used to generate resource group name:" &&
read projectName &&
resourceGroupName="${projectName}rg" &&
az storage account list --resource-group $resourceGroupName &&
echo "Press [ENTER] to continue ..."
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
$projectName = Read-Host -Prompt "Enter a project name that is used to generate resource group name"
$resourceGroupName = "${projectName}rg"
Get-AzStorageAccount -ResourceGroupName $resourceGroupName
Write-Host "Press [ENTER] to continue ..."
```

---

Vergleichen Sie die Speicherkontennamen mit der Namensdefinition in der Vorlage.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Azure-Ressourcen nicht mehr benötigen, löschen Sie die Ressourcengruppe, um die bereitgestellten Ressourcen zu bereinigen.

1. Wählen Sie im Azure-Portal im linken Menü die Option **Ressourcengruppe** aus.
2. Geben Sie den Namen der Ressourcengruppe in das Feld **Nach Name filtern** ein.
3. Klicken Sie auf den Namen der Ressourcengruppe.  Es werden insgesamt drei Ressourcen in der Ressourcengruppe angezeigt.
4. Wählen Sie **Ressourcengruppe löschen** aus dem Menü ganz oben aus.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, mehrere Instanzen von Speicherkonten zu erstellen.  Im nächsten Tutorial entwickeln Sie eine Vorlage mit mehreren Ressourcen und Ressourcentypen. Einige der Ressourcen verfügen über abhängige Ressourcen.

> [!div class="nextstepaction"]
> [Tutorial: Erstellen von Azure Resource Manager-Vorlagen mit abhängigen Ressourcen](./template-tutorial-create-templates-with-dependent-resources.md)
