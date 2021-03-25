---
title: 'Tutorial: Verwenden von Schnellstartvorlagen für die Azure Resource Manager-Bicep-Entwicklung'
description: Erfahren Sie, wie Sie Azure-Schnellstartvorlagen verwenden, um die Bicep-Entwicklung abzuschließen.
author: mumian
ms.date: 03/10/2021
ms.topic: tutorial
ms.author: jgao
ms.custom: ''
ms.openlocfilehash: cf655885e01fe6bca99a82c82d6bbbc4c1a080b3
ms.sourcegitcommit: e6de1702d3958a3bea275645eb46e4f2e0f011af
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102632430"
---
# <a name="tutorial-use-azure-quickstart-templates-for-azure-resource-manager-bicep-development"></a>Tutorial: Verwenden von Schnellstartvorlagen für die Azure Resource Manager-Bicep-Entwicklung

[Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/) sind ein Repository von JSON-Vorlagen, die von der Community entwickelt wurden. Sie können die Beispielvorlagen in der Bicep-Entwicklung nutzen. In diesem Tutorial finden Sie eine Website-Ressourcendefinition, dekompilieren diese in Bicep und fügen sie Ihrer Bicep-Datei hinzu. Dieser Schritt dauert ungefähr **12 Minuten**.

[!INCLUDE [Bicep preview](../../../includes/resource-manager-bicep-preview.md)]

## <a name="prerequisites"></a>Voraussetzungen

Wir empfehlen, das [Tutorial zu exportieren Vorlagen](bicep-tutorial-export-template.md) durchzuführen. Dies ist jedoch keine Voraussetzung.

Sie benötigen Visual Studio Code mit der Bicep-Erweiterung sowie entweder Azure PowerShell oder die Azure-Befehlszeilenschnittstelle (Azure CLI). Weitere Informationen finden Sie unter [Bicep-Tools](bicep-tutorial-create-first-bicep.md#get-tools).

## <a name="review-bicep-file"></a>Bicep-Datei überprüfen

Am Ende des vorherigen Tutorials hatte Ihre Bicep-Datei folgenden Inhalt:

:::code language="bicep" source="~/resourcemanager-templates/get-started-with-templates/export-template/azuredeploy.bicep":::

Diese Vorlage funktioniert für die Bereitstellung von Speicherkonten und App Service-Plänen, Sie sollten ihr jedoch eine Website hinzufügen. Sie können vorgefertigte Vorlagen verwenden, um schnell den JSON-Code zu ermitteln, der für die Bereitstellung einer Ressource erforderlich ist. Bevor Azure-Schnellstartvorlagen Bicep-Beispiele bieten, können Sie mithilfe von Konvertierungstools JSON-Vorlagen in Bicep-Dateien konvertieren.

## <a name="find-template"></a>Suchen nach Vorlagen

Derzeit bieten die Azure-Schnellstartvorlagen nur JSON-Vorlagen. Es gibt Tools, mit denen sie JSON-Vorlagen in Bicep-Vorlagen dekompilieren können.

1. Öffnen Sie [Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/).
1. Geben Sie _Linux-Web-App bereitstellen_ in die **Suche** ein.
1. Wählen Sie die Kachel mit dem Titel **Deploy a basic Linux web app** (Grundlegende Linux-Web-App bereitstellen) aus. Hier ist der [direkte Link](https://azure.microsoft.com/resources/templates/101-webapp-basic-linux/), falls Sie sie nicht finden können.
1. Wählen Sie **Auf GitHub suchen** aus.
1. Wählen Sie _azuredeploy.json_ aus. Dies ist die Vorlage, die Sie verwenden können.
1. Wählen Sie **Raw** aus, und erstellen Sie dann eine Kopie der URL.
1. Navigieren Sie zu **https://bicepdemo.z22.web.core.windows.net/** , wählen Sie die Option **Dekompilieren** aus, und geben Sie dann die URL der rohen Vorlage an.
1. Überprüfen der Bicep-Vorlage. Suchen Sie insbesondere nach der Ressource `Microsoft.Web/sites`.

    ![Website: Resource Manager-Schnellstartvorlage](./media/bicep-tutorial-quickstart-template/resource-manager-template-quickstart-template-web-site.png)

    Es gibt einige wichtige Bicep-Features, die in dieser neuen Ressource zu beachten sind, wenn Sie an JSON-Vorlagen gearbeitet haben.

    In ARM-JSON-Vorlagen müssen Sie manuell Ressourcenabhängigkeiten mit der Eigenschaft _dependsOn_ angeben. Die Websiteressource ist abhängig von der Ressource des App Service-Plans. Wenn Sie mit Bicep auf eine Eigenschaft der Ressource mithilfe des symbolischen Namens verweisen, wird die dependsOn-Eigenschaft automatisch hinzugefügt.

    Sie können auf einfache Weise auf die Ressourcen-ID des symbolischen Namens des App Service-Plans (appServicePlanName.id) verweisen, der in die Funktion resourceId(...) in der kompilierten JSON-Vorlage übersetzt wird.

## <a name="revise-existing-bicep-file"></a>Vorhandene Bicep-Datei überarbeiten

Zusammenführen der dekompilierten Schnellstartvorlage mit der vorhandenen Bicep-Datei. Wie im vorherigen Tutorial aktualisieren Sie den symbolischen Namen der Ressource und den Ressourcennamen entsprechend ihrer Namenskonvention.

:::code language="bicep" source="~/resourcemanager-templates/get-started-with-templates/quickstart-template/azuredeploy.bicep" range="1-73" highlight="20-25,28,61-71":::

Der Web-App-Name muss in Azure eindeutig sein. Zum Vermeiden von doppelten Namen wurde die Variable `webAppPortalName` von `var webAppPortalName_var = '${webAppName}-webapp'` zu `var webAppPortalName = '${webAppName}${uniqueString(resourceGroup().id)}'` aktualisiert.

## <a name="deploy-bicep-file"></a>Bereitstellen der Bicep-Datei

Verwenden Sie zum Bereitstellen einer Bicep-Datei entweder die Azure CLI oder Azure PowerShell.

Falls Sie die Ressourcengruppe noch nicht erstellt haben, folgen Sie den Anweisungen unter [Erstellen einer Ressourcengruppe](bicep-tutorial-create-first-bicep.md#create-resource-group). Diese Beispiel setzt voraus, dass Sie die **bicepFile**-Variable wie im [ersten Tutorial](bicep-tutorial-create-first-bicep.md#deploy-bicep-file) beschrieben auf den Pfad der Bicep-Datei festgelegt haben.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Für die Ausführung dieses Bereitstellungs-Cmdlets benötigen Sie die [aktuelle Version](/powershell/azure/install-az-ps) von Azure PowerShell.

```azurepowershell
New-AzResourceGroupDeployment `
  -Name addwebapp `
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
  --name addwebapp \
  --resource-group myResourceGroup \
  --template-file $bicepFile \
  --parameters storagePrefix=store storageSKU=Standard_LRS webAppName=demoapp
```

---

> [!NOTE]
> Wenn bei der Bereitstellung ein Fehler aufgetreten ist, verwenden Sie den Schalter `verbose`, um Informationen zu den erstellten Ressourcen abzurufen. Verwenden Sie den Schalter `debug`, um weitere Informationen zum Debuggen zu erhalten.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie mit dem nächsten Tutorial fortfahren, müssen Sie die Ressourcengruppe nicht löschen.

Falls Sie keine weiteren Tutorials durchführen möchten, können Sie die bereitgestellten Ressourcen bereinigen, indem Sie die Ressourcengruppe löschen.

1. Wählen Sie im Azure-Portal im linken Menü die Option **Ressourcengruppe** aus.
2. Geben Sie den Namen der Ressourcengruppe in das Feld **Nach Name filtern** ein.
3. Klicken Sie auf den Namen der Ressourcengruppe.
4. Wählen Sie **Ressourcengruppe löschen** aus dem Menü ganz oben aus.

## <a name="next-steps"></a>Nächste Schritte

Sie haben gelernt, wie Sie eine Schnellstartvorlage für Ihre Bicep-Entwicklung verwenden. Im nächsten Tutorial fügen Sie den Ressourcen Tags hinzu.

> [!div class="nextstepaction"]
> [Hinzufügen von Tags](bicep-tutorial-add-tags.md)
