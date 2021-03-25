---
title: 'Tutorial: Hinzufügen von Ausgaben zu Azure Resource Manager-Bicep-Dateien'
description: Fügen Sie zur Vereinfachung der Syntax Ausgaben zu Ihrer Bicep-Datei hinzu.
author: mumian
ms.date: 03/01/2021
ms.topic: tutorial
ms.author: jgao
ms.custom: ''
ms.openlocfilehash: 279417f22ded89db21abddad7a91a4cce520c6bd
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101742968"
---
# <a name="tutorial-add-outputs-to-azure-resource-manager-bicep-file"></a>Tutorial: Hinzufügen von Ausgaben zu Azure Resource Manager-Bicep-Dateien

In diesem Tutorial erfahren Sie, wie Sie einen Wert aus Ihrer Bereitstellung zurückgeben. Sie verwenden Ausgaben, wenn Sie einen Wert von einer bereitgestellten Ressource benötigen. Dieses Tutorial dauert ungefähr **7 Minuten**.

[!INCLUDE [Bicep preview](../../../includes/resource-manager-bicep-preview.md)]

## <a name="prerequisites"></a>Voraussetzungen

Wir empfehlen, das [Tutorial zu Variablen](bicep-tutorial-add-variables.md) durchzuführen. Dies ist jedoch keine Voraussetzung.

Sie benötigen Visual Studio Code mit der Bicep-Erweiterung sowie entweder Azure PowerShell oder die Azure-Befehlszeilenschnittstelle (Azure CLI). Weitere Informationen finden Sie unter [Bicep-Tools](bicep-tutorial-create-first-bicep.md#get-tools).

## <a name="review-bicep-file"></a>Bicep-Datei überprüfen

Am Ende des vorherigen Tutorials hatte Ihre Bicep-Datei folgenden Inhalt:

:::code language="bicep" source="~/resourcemanager-templates/get-started-with-templates/add-variable/azuredeploy.bicep":::

Der Code stellt ein Speicherkonto bereit, gibt aber keine Informationen zu diesem Konto zurück. Möglicherweise müssen Sie Eigenschaften von einer neuen Ressource erfassen, damit sie später zur Referenz verfügbar sind.

## <a name="add-outputs"></a>Hinzufügen von Ausgaben

Sie können Ausgaben verwenden, um Werte aus der Bereitstellung zurückzugeben. Beispielsweise kann es hilfreich sein, die Endpunkte für Ihr neues Speicherkonto abzurufen.

Im folgenden Beispiel ist die Änderung hervorgehoben, die Sie vornehmen, um Ihrer Bicep-Datei einen Ausgabewert hinzuzufügen. Kopieren Sie die gesamte Datei, und ersetzen Sie Ihre Bicep-Datei durch den Inhalt der Datei.

:::code language="bicep" source="~/resourcemanager-templates/get-started-with-templates/add-outputs/azuredeploy.bicep" range="1-33" highlight="33":::

Beachten Sie bezüglich des von Ihnen hinzugefügten Ausgabewerts folgende wichtige Punkte.

Der Typ des Rückgabewerts ist auf `object` festgelegt, d. h. es wird ein Vorlagenobjekt zurückgegeben.

Verwenden Sie den symbolischen Namen des Speicherkontos, um die Eigenschaft `primaryEndpoints` aus dem Speicherkonto zu erhalten.

## <a name="deploy-bicep-file"></a>Bereitstellen der Bicep-Datei

Jetzt können Sie die Bicep-Datei bereitstellen und sich den zurückgegebenen Wert ansehen.

Falls Sie die Ressourcengruppe noch nicht erstellt haben, folgen Sie den Anweisungen unter [Erstellen einer Ressourcengruppe](bicep-tutorial-create-first-bicep.md#create-resource-group). Dieses Beispiel setzt voraus, dass Sie die Variable `bicepFile` wie im [ersten Tutorial](bicep-tutorial-create-first-bicep.md#deploy-bicep-file) beschrieben auf den Pfad zur Bicep-Datei festgelegt haben.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
New-AzResourceGroupDeployment `
  -Name addoutputs `
  -ResourceGroupName myResourceGroup `
  -TemplateFile $bicepFile `
  -storagePrefix "store" `
  -storageSKU Standard_LRS
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Für die Ausführung dieses Bereitstellungsbefehls müssen Sie über die [aktuelle Version](/cli/azure/install-azure-cli) der Azure CLI verfügen.

```azurecli
az deployment group create \
  --name addoutputs \
  --resource-group myResourceGroup \
  --template-file $bicepFile \
  --parameters storagePrefix=store storageSKU=Standard_LRS
```

---

Die Ausgabe für den Bereitstellungsbefehl enthält ein ähnliches Objekt wie im folgenden Beispiel (allerdings nur, wenn die Ausgabe im JSON-Format vorliegt):

```json
{
  "dfs": "https://storeluktbfkpjjrkm.dfs.core.windows.net/",
  "web": "https://storeluktbfkpjjrkm.z19.web.core.windows.net/",
  "blob": "https://storeluktbfkpjjrkm.blob.core.windows.net/",
  "queue": "https://storeluktbfkpjjrkm.queue.core.windows.net/",
  "table": "https://storeluktbfkpjjrkm.table.core.windows.net/",
  "file": "https://storeluktbfkpjjrkm.file.core.windows.net/"
}
```

> [!NOTE]
> Wenn bei der Bereitstellung ein Fehler aufgetreten ist, verwenden Sie den Schalter `verbose`, um Informationen zu den erstellten Ressourcen abzurufen. Verwenden Sie den Schalter `debug`, um weitere Informationen zum Debuggen zu erhalten.

## <a name="review-your-work"></a>Überprüfen Ihrer Arbeit

In den letzten sechs Tutorials haben Sie viele Aufgaben erledigt. Nehmen wir uns einen Moment Zeit, um uns Ihre Arbeit anzusehen. Sie haben eine Bicep-Datei mit Parametern erstellt, die einfach angegeben werden können. Die Bicep-Datei kann in unterschiedlichen Umgebungen wiederverwendet werden, da sie Anpassungen ermöglicht und erforderliche Werte dynamisch erstellt. Außerdem gibt sie Informationen zum Speicherkonto zurück, die Sie in Ihrem Skript verwenden können.

Sehen wir uns nun die Ressourcengruppe und den Bereitstellungsverlauf an.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im linken Menü **Ressourcengruppen** aus.
1. Wählen Sie die Ressourcengruppe aus, in der Sie die Vorlage bereitgestellt haben.
1. Je nachdem, welche Schritte Sie ausgeführt haben, sollten Sie mindestens ein oder vielleicht mehrere Speicherkonten in der Ressourcengruppe haben.
1. Zudem sollten im Verlauf mehrere erfolgreiche Bereitstellungen angezeigt werden. Klicken Sie auf diesen Link.

   ![Auswählen von Bereitstellungen](./media/bicep-tutorial-add-outputs/select-deployments.png)

1. Im Verlauf werden all Ihre Bereitstellungen angezeigt. Wählen Sie die Bereitstellung **addoutputs** aus.

   ![Anzeigen des Bereitstellungsverlaufs](./media/bicep-tutorial-add-outputs/show-history.png)

1. Sie können die Eingaben überprüfen.

   ![Anzeigen der Eingaben](./media/bicep-tutorial-add-outputs/show-inputs.png)

1. Sie können die Ausgaben überprüfen.

   ![Anzeigen der Ausgaben](./media/bicep-tutorial-add-outputs/show-outputs.png)

1. Sie können die JSON-Vorlage überprüfen.

   ![Anzeigen der Vorlage](./media/bicep-tutorial-add-outputs/show-template.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie mit dem nächsten Tutorial fortfahren, müssen Sie die Ressourcengruppe nicht löschen.

Falls Sie keine weiteren Tutorials durchführen möchten, können Sie die bereitgestellten Ressourcen bereinigen, indem Sie die Ressourcengruppe löschen.

1. Wählen Sie im Azure-Portal im linken Menü die Option **Ressourcengruppe** aus.
2. Geben Sie den Namen der Ressourcengruppe in das Feld **Nach Name filtern** ein.
3. Klicken Sie auf den Namen der Ressourcengruppe.
4. Wählen Sie **Ressourcengruppe löschen** aus dem Menü ganz oben aus.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie der Bicep-Datei einen Rückgabewert hinzugefügt. Im nächsten Tutorial erfahren Sie, wie Sie eine JSON-Vorlage exportieren und Teile dieser exportierten Vorlage in Ihrer Bicep-Datei verwenden.

> [!div class="nextstepaction"]
> [Verwenden einer exportierten Vorlage](bicep-tutorial-export-template.md)
