---
title: 'Tutorial: Exportieren einer JSON-Vorlage aus dem Azure-Portal für die Bicep-Entwicklung'
description: Erfahren Sie, wie Sie eine exportierte JSON-Vorlage verwenden, um die Bicep-Entwicklung abzuschließen.
author: mumian
ms.date: 03/10/2021
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 3bc7ed4ada4f7810e9864778c7f76a0573c9dc89
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "102632548"
---
# <a name="tutorial-use-exported-json-template-from-the-azure-portal"></a>Tutorial: Verwenden einer exportieren JSON-Vorlage aus dem Azure-Portal

In dieser Tutorialreihe haben Sie eine Bicep-Datei zum Bereitstellen eines Azure Storage-Kontos erstellt. In den nächsten beiden Tutorials fügen Sie einen *App Service-Plan* und eine *Website* hinzu. Anstatt Vorlagen von Grund auf neu zu erstellen, erfahren Sie, wie Sie JSON-Vorlagen aus dem Azure-Portal exportieren und Beispielvorlagen aus den [Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/) verwenden. Sie passen diese Vorlagen für ihre Verwendung an. Dieses Tutorial dreht sich vor allem um das Exportieren von Vorlagen und das Anpassen des Ergebnisses für ihre Bicep-Datei. Dieser Schritt dauert ungefähr **14 Minuten**.

[!INCLUDE [Bicep preview](../../../includes/resource-manager-bicep-preview.md)]

## <a name="prerequisites"></a>Voraussetzungen

Wir empfehlen, das [Tutorial zu Ausgaben](bicep-tutorial-add-outputs.md) durchzuführen. Dies ist jedoch keine Voraussetzung.

Sie benötigen Visual Studio Code mit der Bicep-Erweiterung sowie entweder Azure PowerShell oder die Azure-Befehlszeilenschnittstelle (Azure CLI). Weitere Informationen finden Sie unter [Bicep-Tools](bicep-tutorial-create-first-bicep.md#get-tools).

## <a name="review-bicep-file"></a>Bicep-Datei überprüfen

Am Ende des vorherigen Tutorials hatte Ihre Bicep-Datei folgenden Inhalt:

:::code language="bicep" source="~/resourcemanager-templates/get-started-with-templates/add-outputs/azuredeploy.bicep":::

Diese Bicep-Datei eignet sich gut für die Bereitstellung von Speicherkonten, Sie sollten sie jedoch mit weiteren Ressourcen ergänzen. Sie können eine JSON-Vorlage aus einer vorhandenen Ressource exportieren, um den JSON-Code für diese Ressource schnell zu erhalten. Konvertieren Sie die JSON-Datei dann in Bicep, um sie dann der Bicep-Datei hinzufügen zu können.

## <a name="create-app-service-plan"></a>App Service-Plan erstellen

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie **Ressource erstellen**.
1. Geben Sie in **Marketplace durchsuchen** den Suchbegriff **App Service-Plan** ein, und wählen Sie dann **App Service-Plan** aus.  Wählen Sie nicht **App Service-Plan (klassisch)** aus.
1. Klicken Sie auf **Erstellen**.
1. Geben Sie Folgendes ein:

    - **Abonnement**: Wählen Sie Ihr Azure-Abonnement aus.
    - **Ressourcengruppe**: Wählen Sie **Neu erstellen** aus, und geben Sie einen Namen an. Geben Sie einen anderen Ressourcengruppennamen an, als Sie bisher in dieser Tutorialreihe verwendet haben.
    - **Name**: Geben Sie einen Namen für den App Service-Plan ein.
    - **Betriebssystem**: Wählen Sie **Linux** aus.
    - **Region**: Wählen Sie eine Azure-Region aus. Beispiel: **USA, Mitte**.
    - **Tarif**: Ändern Sie die SKU (unter „Dev/Test“) in **Basic B1**, um Kosten zu sparen.

    ![Exportieren einer Resource Manager-Vorlage aus dem Portal](./media/bicep-tutorial-export-template/resource-manager-template-export.png)
1. Klicken Sie auf **Überprüfen und erstellen**.
1. Klicken Sie auf **Erstellen**. Es dauert einen Moment, bis die Ressource erstellt wird.

## <a name="export-template"></a>Exportieren der Vorlage

Derzeit unterstützt der Azure-Portal nur das Exportieren von JSON-Vorlagen. Es gibt Tools, mit denen sie JSON-Vorlagen in Bicep-Dateien dekompilieren können.

1. Wählen Sie **Zu Ressource wechseln** aus.

    ![Zu Ressource wechseln](./media/bicep-tutorial-export-template/resource-manager-template-export-go-to-resource.png)

1. Wählen Sie **Vorlage exportieren** aus.

    ![Exportieren einer Resource Manager-Vorlage](./media/bicep-tutorial-export-template/resource-manager-template-export-template.png)

   Die Funktion zum Exportieren von Vorlagen erfasst den aktuellen Zustand einer Ressource und generiert eine Vorlage für deren Bereitstellung. Das Exportieren einer Vorlage kann hilfreich sein, um schnell den JSON-Code zu erhalten, den Sie benötigen, um eine Ressource bereitzustellen.

1. Die `Microsoft.Web/serverfarms` Definition und die Parameterdefinition sind die benötigten Teile.

    ![Exportieren einer Resource Manager-Vorlage aus dem Portal: exportierte Vorlage](./media/bicep-tutorial-export-template/resource-manager-template-exported-template.png)

    > [!IMPORTANT]
    > In der Regel ist die exportierte Vorlage ausführlicher als bei der Erstellung einer Vorlage. Beispielsweise verfügt das SKU-Objekt in der exportierten Vorlage über fünf Eigenschaften. Diese Vorlage funktioniert, aber Sie könnten einfach die `name`-Eigenschaft verwenden. Sie können mit der exportierten Vorlage beginnen und Sie dann wie gewünscht ändern, um Sie an Ihre Anforderungen anzupassen.

1. Wählen Sie **Herunterladen** aus.  Die heruntergeladene ZIP-Datei enthält einmal **template.json** und einmal **parameters.json**. Entzippen Sie die Dateien.
1. Navigieren Sie zu **https://bicepdemo.z22.web.core.windows.net/** , wählen Sie die Option **Dekompilieren** aus und öffnen Sie dann **template.json**. Sie erhalten die Vorlage in Bicep.

## <a name="revise-existing-bicep-file"></a>Vorhandene Bicep-Datei überarbeiten

Mit der exportierten Vorlage erhalten Sie den größten Teil des benötigten Bicep, müssen ihn aber für Ihre Bicep-Datei anpassen. Achten Sie besonders auf Unterschiede in den Parametern und Variablen zwischen Ihrer Bicep-Datei und der exportierten Bicep-Datei. Der Exportprozess kennt selbstverständlich nicht die Parameter und Variablen, die Sie bereits in ihrer Bicep-Datei definiert haben.

Im folgenden Beispiel sind die Ergänzungen zu Ihrer Bicep-Datei gezeigt. Sie enthält den exportierten Code sowie einige Änderungen. Zuerst wird der Name des Parameters entsprechend ihrer Namenskonvention geändert. Zweitens wird der location-Parameter für den Standort des App Service-Plans verwendet. Drittens werden einige der Eigenschaften entfernt, bei denen der Standardwert in Ordnung ist.

Kopieren Sie die gesamte Datei, und ersetzen Sie Ihre Bicep-Datei durch den Inhalt der Datei.

:::code language="bicep" source="~/resourcemanager-templates/get-started-with-templates/export-template/azuredeploy.bicep" range="1-53" highlight="18,34-51":::

## <a name="deploy-bicep-file"></a>Bereitstellen der Bicep-Datei

Zum Bereitstellen einer Bicep-Datei verwenden Sie entweder die Azure CLI oder Azure PowerShell.

Falls Sie die Ressourcengruppe noch nicht erstellt haben, folgen Sie den Anweisungen unter [Erstellen einer Ressourcengruppe](bicep-tutorial-create-first-bicep.md#create-resource-group). Dieses Beispiel setzt voraus, dass Sie die Variable `bicepFile` wie im [ersten Tutorial](bicep-tutorial-create-first-bicep.md#deploy-bicep-file) beschrieben auf den Pfad zur Bicep-Datei festgelegt haben.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Für die Ausführung dieses Bereitstellungs-Cmdlets benötigen Sie die [aktuelle Version](/powershell/azure/install-az-ps) von Azure PowerShell.

```azurepowershell
New-AzResourceGroupDeployment `
  -Name addappserviceplan `
  -ResourceGroupName myResourceGroup `
  -TemplateFile $bicepFile `
  -storagePrefix "store" `
  -storageSKU Standard_LRS
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Für die Ausführung dieses Bereitstellungsbefehls müssen Sie über die [aktuelle Version](/cli/azure/install-azure-cli) der Azure CLI verfügen.

```azurecli
az deployment group create \
  --name addappserviceplan \
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
1. Die Ressourcengruppe enthält ein Speicherkonto und einen App Service-Plan.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie mit dem nächsten Tutorial fortfahren, müssen Sie die Ressourcengruppe nicht löschen.

Falls Sie keine weiteren Tutorials durchführen möchten, können Sie die bereitgestellten Ressourcen bereinigen, indem Sie die Ressourcengruppe löschen.

1. Wählen Sie im Azure-Portal im linken Menü die Option **Ressourcengruppe** aus.
2. Geben Sie den Namen der Ressourcengruppe in das Feld **Nach Name filtern** ein.
3. Klicken Sie auf den Namen der Ressourcengruppe.
4. Wählen Sie **Ressourcengruppe löschen** aus dem Menü ganz oben aus.

## <a name="next-steps"></a>Nächste Schritte

Sie haben gelernt, wie Sie eine JSON-Vorlage aus dem Azure-Portal exportieren, wie Sie die JSON-Vorlage in eine Bicep-Datei konvertieren und wie Sie die exportierte Vorlage für Ihre Bicep-Entwicklung verwenden. Sie können auch die Azure-Schnellstartvorlagen verwenden, um die Bicep-Entwicklung zu vereinfachen.

> [!div class="nextstepaction"]
> [Verwenden von Azure-Schnellstartvorlagen](bicep-tutorial-quickstart-template.md)
