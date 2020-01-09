---
title: Was-wäre-wenn für Vorlagenbereitstellung (Preview)
description: Legen Sie vor der Bereitstellung einer Azure Resource Manager-Vorlage fest, welche Änderungen an Ihren Ressourcen vorgenommen werden.
author: mumian
ms.topic: conceptual
ms.date: 11/20/2019
ms.author: jgao
ms.openlocfilehash: edb9f5e35008b1270031d8e2d5c8a5efa37cb554
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75476272"
---
# <a name="resource-manager-template-deployment-what-if-operation-preview"></a>Resource Manager-Vorlagenbereitstellung: Was-wäre-wenn-Vorgang (Preview)

Vor dem Bereitstellen einer Vorlage können Sie eine Vorschau der Änderungen anzeigen, die vorgenommen werden. Azure Resource Manager stellt den Was-wäre-wenn-Vorgang bereit, damit Sie sehen können, wie sich Ressourcen ändern, wenn Sie die Vorlage bereitstellen. Der Was-wäre-wenn-Vorgang nimmt keine Änderungen an vorhandenen Ressourcen vor. Stattdessen sagt er die Änderungen vorher, wenn die angegebene Vorlage bereitgestellt wird.

> [!NOTE]
> Der Was-wäre-wenn-Vorgang befindet sich zurzeit in der Preview-Phase. Um ihn zu verwenden, müssen Sie sich [für die Preview registrieren](https://aka.ms/armtemplatepreviews). Als Previewrelease können die Ergebnisse mitunter anzeigen, dass sich eine Ressource ändert, wenn tatsächlich gar keine Änderung stattfindet. Wir arbeiten daran, diese Probleme zu verringern, aber hierzu benötigen wir Ihre Hilfe. Melden Sie diese Probleme bitte unter [https://aka.ms/whatifissues](https://aka.ms/whatifissues).

Sie können den Was-wäre-wenn-Vorgang mit dem PowerShell-Befehl `New-AzDeploymentWhatIf` oder dem REST-Vorgang [Deployments – What If](/rest/api/resources/deployments/whatif) (Bereitstellungen – Was-wäre-wenn) verwenden.

In PowerShell sieht die Ausgabe wie folgt aus:

![Resource Manager-Vorlagenbereitstellung: Was-wäre-wenn-Vorgang fullresourcepayload und Änderungstypen](./media/template-deploy-what-if/resource-manager-deployment-whatif-change-types.png)

## <a name="change-types"></a>Änderungstypen

Der Was-wäre-wenn-Vorgang listet sechs verschiedene Typen von Änderungen auf:

- **Erstellen**: Die Ressource ist zurzeit nicht vorhanden, aber in der Vorlage definiert. Die Ressource wird erstellt.

- **Löschen:** Dieser Änderungstyp gilt nur, wenn der [Modus „Vollständig“](deployment-modes.md) für die Bereitstellung verwendet wird. Die Ressource ist vorhanden, aber nicht in der Vorlage definiert. Im Modus „Vollständig“ wird die Ressource gelöscht. In diesem Änderungstyp werden nur Ressourcen eingeschlossen, die [das Löschen im Modus „Vollständig“ unterstützen](complete-mode-deletion.md).

- **Ignorieren**: Die Ressource ist vorhanden, aber nicht in der Vorlage definiert. Die Ressource wird weder bereitgestellt noch geändert.

- **NoChange**: Die Ressource ist vorhanden und in der Vorlage definiert. Die Ressource wird erneut bereitgestellt, wobei sich die Eigenschaften der Ressource aber nicht ändern. Dieser Änderungstyp wird zurückgegeben, wenn [ResultFormat](#result-format) auf `FullResourcePayloads` festgelegt ist, wobei es sich um den Standardwert handelt.

- **Ändern**: Die Ressource ist vorhanden und in der Vorlage definiert. Die Ressource wird erneut bereitgestellt, und die Eigenschaften der Ressource ändern sich. Dieser Änderungstyp wird zurückgegeben, wenn [ResultFormat](#result-format) auf `FullResourcePayloads` festgelegt ist, wobei es sich um den Standardwert handelt.

- **Bereitstellen**: Die Ressource ist vorhanden und in der Vorlage definiert. Die Ressource wird erneut erstellt. Die Eigenschaften der Ressource können sich ändern oder auch nicht. Der Vorgang gibt diesen Änderungstyp zurück, wenn er nicht über genügend Informationen verfügt, um zu bestimmen, ob sich Eigenschaften ändern. Diese Bedingung wird nur angezeigt, wenn [ResultFormat](#result-format) auf `ResourceIdOnly` festgelegt ist.

## <a name="deployment-scope"></a>Bereitstellungsumfang

Sie können den Was-wäre-wenn-Vorgang für Bereitstellungen auf Ebene des Abonnements oder der Ressourcengruppe verwenden. Den Bereitstellungsumfang legen Sie mit dem `-ScopeType`-Parameter fest. Zulässige Werte sind `Subscription` und `ResourceGroup`. In diesem Artikel verwenden Ressourcengruppenbereitstellungen demonstriert.

Informationen zu Bereitstellungen auf Abonnementebene finden Sie unter [Erstellen von Ressourcengruppen und Ressourcen auf Abonnementebene](deploy-to-subscription.md#).

## <a name="result-format"></a>Ergebnisformat

Sie können den Grad der Detailliertheit der Informationen steuern, die in Bezug auf die vorhergesagten Änderungen zurückgegeben werden. Legen Sie den `ResultFormat`-Parameter auf `FullResourcePayloads` fest, um eine Liste der Ressourcen zu erhalten, die sich ändern, und Details zu den Eigenschaften, die sich ändern. Legen Sie den `ResultFormat`-Parameter auf `ResourceIdOnly` fest, um eine Liste der Ressourcen zu erhalten, die sich ändern. Standardwert: `FullResourcePayloads`.  

Die folgenden Screenshots zeigen die zwei unterschiedlichen Ausgabeformate:

- Vollständige Ressourcennutzlasten

    ![Resource Manager-Vorlagenbereitstellung: Was-wäre-wenn-Vorgang fullresourcepayloads-Ausgabe](./media/template-deploy-what-if/resource-manager-deployment-whatif-output-fullresourcepayload.png)

- Nur Ressourcen-ID

    ![Resource Manager-Vorlagenbereitstellung: Was-wäre-wenn-Vorgang resourceidonly-Ausgabe](./media/template-deploy-what-if/resource-manager-deployment-whatif-output-resourceidonly.png)

## <a name="run-what-if-operation"></a>Ausführen des Was-wäre-wenn-Vorgangs

### <a name="set-up-environment"></a>Einrichten der Umgebung

Um zu sehen, wie Was-wäre-wenn funktioniert, führen wir nun einige Tests aus. Stellen Sie als Erstes eine Vorlage aus den [Azure-Schnellstartvorlagen bereit, die ein Speicherkonto erstellt](https://github.com/Azure/azure-quickstart-templates/blob/master/101-storage-account-create/azuredeploy.json). Der Standardspeicherkontotyp ist `Standard_LRS`. Sie verwenden dieses Speicherkonto, um zu testen, wie Änderungen von Was-wäre-wenn gemeldet werden.

```azurepowershell-interactive
New-AzResourceGroup `
  -Name ExampleGroup `
  -Location centralus
New-AzResourceGroupDeployment `
  -ResourceGroupName ExampleGroup `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json"
```

### <a name="test-modification"></a>Testen der Änderung

Nach Abschluss der Bereitstellung, sind Sie bereit, um den Was-wäre-wenn-Vorgang zu testen. Führen Sie den What-if-Befehl aus, aber ändern Sie dabei den Speicherkontotyp in `Standard_GRS`.

```azurepowershell-interactive
New-AzDeploymentWhatIf `
  -ScopeType ResourceGroup `
  -ResourceGroupName ExampleGroup `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json" `
  -storageAccountType Standard_GRS
```

Die Was-wäre-wenn-Ausgabe sieht in etwa wie folgt aus:

![Resource Manager-Vorlagenbereitstellung: Was-wäre-wenn-Ausgabe](./media/template-deploy-what-if/resource-manager-deployment-whatif-output.png)

Beachten Sie am Anfang der Ausgabe, dass Farben so definiert sind, dass der Typ der Änderungen angegeben wird.

Am unteren Rand der Ausgabe wird angezeigt, dass der SKU-Name (Speicherkontotyp) von **Standard_LRS** in **Standard_GRS** geändert wird.

Einige der als gelöscht aufgeführten Eigenschaften werden sich tatsächlich nicht ändern. In der obigen Abbildung handelt es sich bei diesen Eigenschaften um „accessTier“, „encryption.keySource“ und andere in diesem Abschnitt. Eigenschaften können fälschlicherweise als gelöscht gemeldet werden, wenn sie nicht in der Vorlage vorhanden sind, sondern während der Bereitstellung automatisch als Standardwerte festgelegt werden. Dieses Ergebnis wird in der Was-wäre-wenn-Antwort als „Rauschen“ (Noise) betrachtet. Für die endgültige bereitgestellte Ressource werden die Werte der Eigenschaften festgelegt. Mit der fortschreitenden Entwicklung des Was-wäre-wenn-Vorgangs werden diese Eigenschaften aus dem Ergebnis herausgefiltert.

### <a name="test-deletion"></a>Testen des Löschens

Der Was-wäre-wenn-Vorgang unterstützt die Verwendung von [Bereitstellungsmodi](deployment-modes.md). Wenn er auf den Modus „Vollständig“ festgelegt ist, werden Ressourcen, die nicht in der Vorlage enthalten sind, gelöscht. Im folgenden Beispiel wird eine [Vorlage im Modus „Vollständig“ bereitgestellt, für die keine Ressourcen definiert sind](https://github.com/Azure/azure-docs-json-samples/blob/master/empty-template/azuredeploy.json).

```azurepowershell-interactive
New-AzDeploymentWhatIf `
  -ScopeType ResourceGroup `
  -ResourceGroupName ExampleGroup `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/empty-template/azuredeploy.json" `
  -Mode Complete
```

Da keine Ressourcen in der Vorlage definiert sind und der Bereitstellungsmodus auf „Vollständig“ festgelegt ist, wird das Speicherkonto gelöscht.

![Resource Manager-Vorlagenbereitstellung: Was-wäre-wenn-Ausgabe Bereitstellungsmodus „Vollständig“](./media/template-deploy-what-if/resource-manager-deployment-whatif-output-mode-complete.png)

Es ist wichtig, daran zu denken, dass Was-wäre-wenn keine tatsächlichen Änderungen vornimmt. Das Speicherkonto ist weiterhin in der Ressourcengruppe vorhanden.

## <a name="next-steps"></a>Nächste Schritte

- Wenn Sie falsche Ergebnisse von Was-wäre-wenn aus der Previewversion bemerken, melden Sie die Probleme unter [https://aka.ms/whatifissues](https://aka.ms/whatifissues).
- Informationen zum Bereitstellen von Vorlagen mit Azure PowerShell finden Sie unter [Bereitstellen von Ressourcen mit Resource Manager-Vorlagen und Azure PowerShell](deploy-powershell.md).
- Informationen zum Bereitstellen mit REST finden Sie unter [Bereitstellen von Ressourcen mit Resource Manager-Vorlagen und Resource Manager-REST-API](deploy-rest.md).
- Informationen zum Rollback zu einer erfolgreiche Bereitstellung, wenn ein Fehler auftritt, finden Sie unter [Rollback bei Fehler zu erfolgreicher Bereitstellung](rollback-on-error.md).
