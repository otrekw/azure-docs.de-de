---
title: 'Tutorial: Hinzufügen von Funktionen zu Azure Resource Manager-Bicep-Dateien'
description: Fügen Sie Ihren Bicep-Dateien Funktionen hinzu, um Werte zu erstellen.
author: mumian
ms.date: 03/02/2021
ms.topic: tutorial
ms.author: jgao
ms.custom: references_regions
ms.openlocfilehash: 9c66ab132d3343115b1a9bd852d3eb3d83ae4ca7
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101742992"
---
# <a name="tutorial-add-functions-to-azure-resource-manager-bicep-file"></a>Tutorial: Hinzufügen von Funktionen zu Azure Resource Manager-Bicep-Dateien

In diesem Tutorial erfahren Sie, wie Sie Ihrer Bicep-Datei [Vorlagenfunktionen](template-functions.md) hinzufügen. Sie verwenden Funktionen, um dynamisch Werte zu erstellen. Zusätzlich zu diesen vom System bereitgestellten Vorlagenfunktionen können Sie auch [benutzerdefinierte Funktionen](./template-user-defined-functions.md) (User-Defined Functions, UDFs) erstellen. Dieses Tutorial dauert ungefähr **7 Minuten**.

[!INCLUDE [Bicep preview](../../../includes/resource-manager-bicep-preview.md)]

## <a name="prerequisites"></a>Voraussetzungen

Wir empfehlen, das [Tutorial zu Parametern](bicep-tutorial-add-parameters.md) durchzuführen. Dies ist jedoch keine Voraussetzung.

Sie benötigen Visual Studio Code mit der Bicep-Erweiterung sowie entweder Azure PowerShell oder die Azure-Befehlszeilenschnittstelle (Azure CLI). Weitere Informationen finden Sie unter [Bicep-Tools](bicep-tutorial-create-first-bicep.md#get-tools).

## <a name="review-bicep-file"></a>Bicep-Datei überprüfen

Am Ende des vorherigen Tutorials hatte Ihre Bicep-Datei folgenden Inhalt:

:::code language="bicep" source="~/resourcemanager-templates/get-started-with-templates/add-sku/azuredeploy.bicep":::

Der Standort des Speicherkontos ist auf **USA, Osten** hartcodiert. Möglicherweise müssen Sie das Speicherkonto jedoch in anderen Regionen bereitstellen. Auch hier erweist sich die mangelnde Flexibilität Ihrer Bicep-Datei als Problem. Sie könnten einen Parameter für den Standort hinzufügen, es wäre aber von Vorteil, wenn sein Standardwert nicht nur ein hartcodierter Wert wäre.

## <a name="use-function"></a>Verwenden einer Funktion

Funktionen rufen während der Bereitstellung dynamisch Werte ab und erhöhen dadurch die Flexibilität Ihrer Bicep-Datei. In diesem Tutorial verwenden Sie eine Funktion, um den Standort der Ressourcengruppe abzurufen, die Sie für die Bereitstellung verwenden.

Das folgende Beispiel zeigt die Änderungen, die Sie vornehmen, um einen Parameter namens `location` hinzuzufügen. Der Standardwert des Parameters ruft die Funktion [resourceGroup](template-functions-resource.md#resourcegroup) auf. Diese Funktion gibt ein Objekt mit Informationen zu der für die Bereitstellung verwendeten Ressourcengruppe zurück. Eine der Eigenschaften im Objekt ist eine Standorteigenschaft. Wenn Sie den Standardwert verwenden, wird für das Speicherkonto der gleiche Standort verwendet wie für die Ressourcengruppe. Die Ressourcen in einer Ressourcengruppe müssen nicht den gleichen Standort haben. Sie können bei Bedarf auch einen anderen Standort angeben.

Kopieren Sie die gesamte Datei, und ersetzen Sie Ihre Bicep-Datei durch den Inhalt der Datei.

:::code language="bicep" source="~/resourcemanager-templates/get-started-with-templates/add-location/azuredeploy.bicep" range="1-30" highlight="18,22":::

## <a name="deploy-bicep-file"></a>Bereitstellen der Bicep-Datei

In den vorherigen Tutorials haben Sie ein Speicherkonto in der Region „USA, Osten“ erstellt, Ihre Ressourcengruppe wurde aber in „USA, Mitte“ erstellt. In diesem Tutorial wird Ihr Speicherkonto in der gleichen Region erstellt wie die Ressourcengruppe. Verwenden Sie den Standardwert für den Standort, damit Sie diesen Parameterwert nicht angeben müssen. Da Sie ein Speicherkonto an einem anderen Standort erstellen, müssen Sie einen neuen Namen für das Speicherkonto angeben. Verwenden Sie beispielsweise anstelle von **store1** das Präfix **store2**.

Falls Sie die Ressourcengruppe noch nicht erstellt haben, folgen Sie den Anweisungen unter [Erstellen einer Ressourcengruppe](bicep-tutorial-create-first-bicep.md#create-resource-group). Dieses Beispiel setzt voraus, dass Sie die Variable `bicepFile` wie im [ersten Tutorial](bicep-tutorial-create-first-bicep.md#deploy-bicep-file) beschrieben auf den Pfad zur Bicep-Datei festgelegt haben.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Für die Ausführung dieses Bereitstellungs-Cmdlets benötigen Sie die [aktuelle Version](/powershell/azure/install-az-ps) von Azure PowerShell.

```azurepowershell
New-AzResourceGroupDeployment `
  -Name addlocationparameter `
  -ResourceGroupName myResourceGroup `
  -TemplateFile $bicepFile `
  -storageName "{new-unique-name}"
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Für die Ausführung dieses Bereitstellungsbefehls müssen Sie über die [aktuelle Version](/cli/azure/install-azure-cli) der Azure CLI verfügen.

```azurecli
az deployment group create \
  --name addlocationparameter \
  --resource-group myResourceGroup \
  --template-file $bicepFile \
  --parameters storageName={new-unique-name}
```

---

> [!NOTE]
> Wenn bei der Bereitstellung ein Fehler aufgetreten ist, verwenden Sie den Schalter `verbose`, um Informationen zu den erstellten Ressourcen abzurufen. Verwenden Sie den Schalter `debug`, um weitere Informationen zum Debuggen zu erhalten.

## <a name="verify-deployment"></a>Überprüfen der Bereitstellung

Sie können die Bereitstellung überprüfen, indem Sie sich die Ressourcengruppe im Azure-Portal ansehen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im linken Menü **Ressourcengruppen** aus.
1. Wählen Sie die Ressourcengruppe aus, in der Sie die Vorlage bereitgestellt haben.
1. Sie sehen, dass eine Speicherkontoressource bereitgestellt wurde, die den gleichen Standort aufweist wie die Ressourcengruppe.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie mit dem nächsten Tutorial fortfahren, müssen Sie die Ressourcengruppe nicht löschen.

Falls Sie keine weiteren Tutorials durchführen möchten, können Sie die bereitgestellten Ressourcen bereinigen, indem Sie die Ressourcengruppe löschen.

1. Wählen Sie im Azure-Portal im linken Menü die Option **Ressourcengruppe** aus.
2. Geben Sie den Namen der Ressourcengruppe in das Feld **Nach Name filtern** ein.
3. Klicken Sie auf den Namen der Ressourcengruppe.
4. Wählen Sie **Ressourcengruppe löschen** aus dem Menü ganz oben aus.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie den Standardwert für einen Parameter mithilfe einer Funktion definiert. In den nächsten Tutorials dieser Reihe werden Sie weitere Funktionen verwenden. Am Ende der Reihe fügen Sie jedem Abschnitt der Bicep-Datei Funktionen hinzu.

> [!div class="nextstepaction"]
> [Hinzufügen von Variablen](bicep-tutorial-add-variables.md)
