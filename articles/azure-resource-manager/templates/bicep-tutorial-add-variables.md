---
title: 'Tutorial: Hinzufügen von Variablen zu Azure Resource Manager-Bicep-Dateien'
description: Fügen Sie zur Vereinfachung der Syntax Variablen zu Ihrer Bicep-Datei hinzu.
author: mumian
ms.date: 03/10/2021
ms.topic: tutorial
ms.author: jgao
ms.custom: ''
ms.openlocfilehash: da2755c1f2c0f9fa891fe1a99b1fed21f64492c8
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "102632474"
---
# <a name="tutorial-add-variables-to-azure-resource-manager-bicep-file"></a>Tutorial: Hinzufügen von Variablen zu Azure Resource Manager-Bicep-Dateien

In diesem Tutorial erfahren Sie, wie Sie Ihrer Bicep-Datei eine Variable hinzufügen. Variablen vereinfachen Ihre Bicep-Datei, da sie es Ihnen ermöglichen, einen Ausdruck einmal zu schreiben und dann in der gesamten Bicep-Datei wiederzuverwenden. Dieses Tutorial dauert ungefähr **7 Minuten**.

[!INCLUDE [Bicep preview](../../../includes/resource-manager-bicep-preview.md)]

## <a name="prerequisites"></a>Voraussetzungen

Wir empfehlen, das [Tutorial zu Funktionen](bicep-tutorial-add-functions.md) durchzuführen. Dies ist jedoch keine Voraussetzung.

Sie benötigen Visual Studio Code mit der Bicep-Erweiterung sowie entweder Azure PowerShell oder die Azure-Befehlszeilenschnittstelle (Azure CLI). Weitere Informationen finden Sie unter [Bicep-Tools](bicep-tutorial-create-first-bicep.md#get-tools).

## <a name="review-bicep-file"></a>Bicep-Datei überprüfen

Am Ende des vorherigen Tutorials hatte Ihre Bicep-Datei folgenden Inhalt:

:::code language="bicep" source="~/resourcemanager-templates/get-started-with-templates/add-location/azuredeploy.bicep":::

Die Verwendung des Parameters für den Speicherkontonamen ist schwierig, da Sie einen eindeutigen Namen angeben müssen. Wenn Sie die vorherigen Tutorials in dieser Reihe durchgeführt haben, sind Sie es wahrscheinlich leid, einen eindeutigen Namen erraten zu müssen. Um dieses Problem zu lösen, fügen Sie eine Variable hinzu, die einen eindeutigen Namen für das Speicherkonto erstellt.

## <a name="use-variable"></a>Verwenden einer Variablen

Im folgenden Beispiel sind die Änderungen gezeigt, die Sie vornehmen, um Ihrer Bicep-Datei eine Variable hinzuzufügen, die einen eindeutigen Speicherkontonamen erstellt. Kopieren Sie die gesamte Datei, und ersetzen Sie Ihre Bicep-Datei durch den Inhalt der Datei.

:::code language="bicep" source="~/resourcemanager-templates/get-started-with-templates/add-variable/azuredeploy.bicep" range="1-31" highlight="1-3,19,22":::

Beachten Sie, dass die Datei eine Variable namens `uniqueStorageName` enthält. Diese Variable verwendet drei Funktionen, um einen Zeichenfolgenwert zu erstellen.

Sie kennen auch die Funktion [resourceGroup](template-functions-resource.md#resourcegroup). Im vorherigen Tutorial haben Sie die `location`-Eigenschaft abgerufen. In diesem Fall rufen Sie stattdessen die `id`-Eigenschaft ab. Die `id`-Eigenschaft gibt den vollständigen Bezeichner der Ressourcengruppe zurück, einschließlich der Abonnement-ID und des Ressourcengruppennamens.

Die Funktion [uniqueString](template-functions-string.md#uniquestring) erstellt einen Hashwert mit 13 Zeichen. Der zurückgegebene Wert hängt von den Parametern ab, die Sie übergeben. In diesem Tutorial verwenden Sie die Ressourcengruppen-ID als Eingabe für den Hashwert. Sie könnten die Bicep-Datei also in anderen Ressourcengruppen bereitstellen und einen anderen eindeutigen Zeichenfolgenwert abrufen. Wenn Sie die Vorlage in derselben Ressourcengruppe bereitstellen, erhalten Sie jedoch den gleichen Wert.

Bicep unterstützt eine [Zeichenfolgeninterpolation](https://en.wikipedia.org/wiki/String_interpolation#)-Syntax. Bei dieser Variable akzeptiert sie die Zeichenfolge aus dem Parameter und die Zeichenfolge aus der `uniqueString`-Funktion und kombiniert sie zu einer Zeichenfolge.

Mit dem Parameter `storagePrefix` können Sie ein Präfix übergeben, das Ihnen die Identifizierung von Speicherkonten erleichtert. Sie können eine eigene Benennungskonvention erstellen, mit der Sie Speicherkonten nach der Bereitstellung in einer langen Liste von Ressourcen leichter erkennen können.

Beachten Sie auch, dass der Speichername jetzt nicht mehr auf einen Parameter, sondern auf die Variable festgelegt ist.

## <a name="deploy-bicep-file"></a>Bereitstellen der Bicep-Datei

Stellen wir nun die Bicep-Datei bereit. Die Bereitstellung dieser Bicep-Datei ist einfacher als die der vorherigen Vorlagen, da Sie nur das Präfix für den Speichernamen angeben.

Falls Sie die Ressourcengruppe noch nicht erstellt haben, folgen Sie den Anweisungen unter [Erstellen einer Ressourcengruppe](bicep-tutorial-create-first-bicep.md#create-resource-group). Dieses Beispiel setzt voraus, dass Sie die Variable `bicepFile` wie im [ersten Tutorial](bicep-tutorial-create-first-bicep.md#deploy-bicep-file) beschrieben auf den Pfad zur Bicep-Datei festgelegt haben.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Für die Ausführung dieses Bereitstellungs-Cmdlets benötigen Sie die [aktuelle Version](/powershell/azure/install-az-ps) von Azure PowerShell.

```azurepowershell
New-AzResourceGroupDeployment `
  -Name addnamevariable `
  -ResourceGroupName myResourceGroup `
  -TemplateFile $bicepFile `
  -storagePrefix "store" `
  -storageSKU Standard_LRS
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Für die Ausführung dieses Bereitstellungsbefehls müssen Sie über die [aktuelle Version](/cli/azure/install-azure-cli) der Azure CLI verfügen.

```azurecli
az deployment group create \
  --name addnamevariable \
  --resource-group myResourceGroup \
  --template-file $bicepFile \
  --parameters storagePrefix=store storageSKU=Standard_LRS
```

---

> [!NOTE]
> Wenn bei der Bereitstellung ein Fehler aufgetreten ist, verwenden Sie den Schalter `verbose`, um Informationen zu den erstellten Ressourcen abzurufen. Verwenden Sie den Schalter `debug`, um weitere Informationen zum Debuggen zu erhalten.

## <a name="verify-deployment"></a>Überprüfen der Bereitstellung

Sie können die Bereitstellung überprüfen, indem Sie sich die Ressourcengruppe im Azure-Portal ansehen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im linken Menü **Ressourcengruppen** aus.
1. Wählen Sie die Ressourcengruppe aus, in der Sie die Vorlage bereitgestellt haben.
1. Sie sehen, dass eine Speicherkontoressource bereitgestellt wurde. Der Name des Speicherkontos lautet **store** gefolgt von einer Folge zufälliger Zeichen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie mit dem nächsten Tutorial fortfahren, müssen Sie die Ressourcengruppe nicht löschen.

Falls Sie keine weiteren Tutorials durchführen möchten, können Sie die bereitgestellten Ressourcen bereinigen, indem Sie die Ressourcengruppe löschen.

1. Wählen Sie im Azure-Portal im linken Menü die Option **Ressourcengruppe** aus.
2. Geben Sie den Namen der Ressourcengruppe in das Feld **Nach Name filtern** ein.
3. Klicken Sie auf den Namen der Ressourcengruppe.
4. Wählen Sie **Ressourcengruppe löschen** aus dem Menü ganz oben aus.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie eine Variable hinzugefügt, die einen eindeutigen Namen für ein Speicherkonto erstellt. Im nächsten Tutorial geben Sie einen Wert aus dem bereitgestellten Speicherkonto zurück.

> [!div class="nextstepaction"]
> [Hinzufügen von Ausgaben](bicep-tutorial-add-outputs.md)
