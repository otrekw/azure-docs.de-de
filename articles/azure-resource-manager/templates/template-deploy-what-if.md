---
title: Was-wäre-wenn für Vorlagenbereitstellung (Preview)
description: Legen Sie vor der Bereitstellung einer Azure Resource Manager-Vorlage fest, welche Änderungen an Ihren Ressourcen vorgenommen werden.
author: mumian
ms.topic: conceptual
ms.date: 04/09/2020
ms.author: jgao
ms.openlocfilehash: b8e94d0b4f364e2873dfc21792a67f11c33483bf
ms.sourcegitcommit: ae3d707f1fe68ba5d7d206be1ca82958f12751e8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/10/2020
ms.locfileid: "81010187"
---
# <a name="arm-template-deployment-what-if-operation-preview"></a>ARM-Vorlagenbereitstellung: Was-wäre-wenn-Vorgang (Vorschau)

Vor dem Bereitstellen einer ARM-Vorlage (Azure Resource Manager) können Sie eine Vorschau der Änderungen anzeigen, die vorgenommen werden. Azure Resource Manager stellt den Was-wäre-wenn-Vorgang bereit, damit Sie sehen können, wie sich Ressourcen ändern, wenn Sie die Vorlage bereitstellen. Der Was-wäre-wenn-Vorgang nimmt keine Änderungen an vorhandenen Ressourcen vor. Stattdessen sagt er die Änderungen vorher, wenn die angegebene Vorlage bereitgestellt wird.

> [!NOTE]
> Der Was-wäre-wenn-Vorgang befindet sich zurzeit in der Preview-Phase. Als Previewrelease können die Ergebnisse mitunter anzeigen, dass sich eine Ressource ändert, wenn tatsächlich gar keine Änderung stattfindet. Wir arbeiten daran, diese Probleme zu verringern, aber hierzu benötigen wir Ihre Hilfe. Melden Sie diese Probleme bitte unter [https://aka.ms/whatifissues](https://aka.ms/whatifissues).

Sie können den Was-wäre-wenn-Vorgang mit PowerShell-Befehlen oder REST-API-Vorgänge verwenden.

## <a name="install-powershell-module"></a>Installieren des PowerShell-Moduls

Um Was-wäre-wenn in PowerShell zu verwenden, installieren Sie eine Vorschauversion des Az.Resources-Moduls aus dem PowerShell-Katalog.

### <a name="install-preview-version"></a>Installieren der Vorschauversion

Verwenden Sie zum Installieren des Vorschaumoduls:

```powershell
Install-Module Az.Resources -RequiredVersion 1.12.1-preview -AllowPrerelease
```

### <a name="uninstall-alpha-version"></a>Deinstallieren einer Alphaversion

Wenn Sie zuvor eine Alphaversion des Was-wäre-wenn-Moduls installiert hatten, deinstallieren Sie dieses Modul. Die Alphaversion war nur für Benutzer verfügbar, die sich für eine frühe Vorschau registriert hatten. Wenn Sie diese Vorschau nicht installiert haben, können Sie diesen Abschnitt überspringen.

1. Ausführen von PowerShell als Administrator
1. Überprüfen Sie Ihre installierten Versionen des Az.Resources-Moduls.

   ```powershell
   Get-InstalledModule -Name Az.Resources -AllVersions | select Name,Version
   ```

1. Wenn Sie über eine installierte Version mit einer Versionsnummer im Format **2.x.x-alpha** verfügen, deinstallieren Sie diese Version.

   ```powershell
   Uninstall-Module Az.Resources -RequiredVersion 2.0.1-alpha5 -AllowPrerelease
   ```

1. Heben Sie die Registrierung des Was-wäre-wenn-Repositorys auf, das Sie für die Installation der Vorschau verwendet haben.

   ```powershell
   Unregister-PSRepository -Name WhatIfRepository
   ```

Sie sind für die Verwendung von Was-wäre-wenn bereit.

## <a name="see-results"></a>Anzeigen der Ergebnisse

In PowerShell enthält die Ausgabe farbcodierte Ergebnisse, die Ihnen helfen, die verschiedenen Arten von Änderungen anzuzeigen.

![Resource Manager-Vorlagenbereitstellung: Was-wäre-wenn-Vorgang fullresourcepayload und Änderungstypen](./media/template-deploy-what-if/resource-manager-deployment-whatif-change-types.png)

Die Textausgabe lautet:

```powershell
Resource and property changes are indicated with these symbols:
  - Delete
  + Create
  ~ Modify

The deployment will update the following scope:

Scope: /subscriptions/./resourceGroups/ExampleGroup

  ~ Microsoft.Network/virtualNetworks/vnet-001 [2018-10-01]
    - tags.Owner: "Team A"
    ~ properties.addressSpace.addressPrefixes: [
      - 0: "10.0.0.0/16"
      + 0: "10.0.0.0/15"
      ]
    ~ properties.subnets: [
      - 0:

          name:                     "subnet001"
          properties.addressPrefix: "10.0.0.0/24"

      ]

Resource changes: 1 to modify.
```

## <a name="what-if-commands"></a>Was-wäre-wenn-Befehle

Sie können Azure PowerShell oder die Azure-REST-API für den Was-wäre-wenn-Vorgang verwenden.

### <a name="azure-powershell"></a>Azure PowerShell

Wenn Sie vor der Bereitstellung einer Vorlage eine Vorschau der Änderungen anzeigen möchten, fügen Sie dem Bereitstellungsbefehl den Parameterschalter `-Whatif` hinzu.

* `New-AzResourceGroupDeployment -Whatif` für Bereitstellungen von Ressourcengruppen
* `New-AzSubscriptionDeployment -Whatif` und `New-AzDeployment -Whatif` für Bereitstellungen auf Abonnementebene

Sie können auch den Parameterschalter `-Confirm` verwenden, um eine Vorschau der Änderungen anzuzeigen und dann eine Aufforderung zum Fortsetzen der Bereitstellung zu erhalten.

* `New-AzResourceGroupDeployment -Confirm` für Bereitstellungen von Ressourcengruppen
* `New-AzSubscriptionDeployment -Confirm` und `New-AzDeployment -Confirm` für Bereitstellungen auf Abonnementebene

Die vorangehenden Befehle geben eine Textzusammenfassung zurück, die Sie manuell überprüfen können. Um ein Objekt zu erhalten, das Sie programmgesteuert auf Änderungen überprüfen können, verwenden Sie Folgendes:

* `$results = Get-AzResourceGroupDeploymentWhatIfResult` für Bereitstellungen von Ressourcengruppen
* `$results = Get-AzSubscriptionDeploymentWhatIfResult` oder `$results = Get-AzDeploymentWhatIfResult` für Bereitstellungen auf Abonnementebene

### <a name="azure-rest-api"></a>Azure-REST-API

Verwenden Sie für die REST-API Folgendes:

* [Bereitstellungen – Was-wäre-wenn](/rest/api/resources/deployments/whatif) für Bereitstellungen von Ressourcengruppen
* [Bereitstellungen – Was-wäre-wenn im Abonnementbereich](/rest/api/resources/deployments/whatifatsubscriptionscope) für Bereitstellungen auf Abonnementebene

## <a name="change-types"></a>Änderungstypen

Der Was-wäre-wenn-Vorgang listet sechs verschiedene Typen von Änderungen auf:

- **Erstellen**: Die Ressource ist zurzeit nicht vorhanden, aber in der Vorlage definiert. Die Ressource wird erstellt.

- **Löschen:** Dieser Änderungstyp gilt nur, wenn der [Modus „Vollständig“](deployment-modes.md) für die Bereitstellung verwendet wird. Die Ressource ist vorhanden, aber nicht in der Vorlage definiert. Im Modus „Vollständig“ wird die Ressource gelöscht. In diesem Änderungstyp werden nur Ressourcen eingeschlossen, die [das Löschen im Modus „Vollständig“ unterstützen](complete-mode-deletion.md).

- **Ignorieren**: Die Ressource ist vorhanden, aber nicht in der Vorlage definiert. Die Ressource wird weder bereitgestellt noch geändert.

- **NoChange**: Die Ressource ist vorhanden und in der Vorlage definiert. Die Ressource wird erneut bereitgestellt, wobei sich die Eigenschaften der Ressource aber nicht ändern. Dieser Änderungstyp wird zurückgegeben, wenn [ResultFormat](#result-format) auf `FullResourcePayloads` festgelegt ist, wobei es sich um den Standardwert handelt.

- **Ändern**: Die Ressource ist vorhanden und in der Vorlage definiert. Die Ressource wird erneut bereitgestellt, und die Eigenschaften der Ressource ändern sich. Dieser Änderungstyp wird zurückgegeben, wenn [ResultFormat](#result-format) auf `FullResourcePayloads` festgelegt ist, wobei es sich um den Standardwert handelt.

- **Bereitstellen**: Die Ressource ist vorhanden und in der Vorlage definiert. Die Ressource wird erneut erstellt. Die Eigenschaften der Ressource können sich ändern oder auch nicht. Der Vorgang gibt diesen Änderungstyp zurück, wenn er nicht über genügend Informationen verfügt, um zu bestimmen, ob sich Eigenschaften ändern. Diese Bedingung wird nur angezeigt, wenn [ResultFormat](#result-format) auf `ResourceIdOnly` festgelegt ist.

## <a name="result-format"></a>Ergebnisformat

Sie können den Grad der Detailliertheit der Informationen steuern, die in Bezug auf die vorhergesagten Änderungen zurückgegeben werden. Verwenden Sie in den Bereitstellungsbefehlen (`New-Az*Deployment`) den Parameter **-WhatIfResultFormat**. Verwenden Sie in den Befehlen für programmgesteuerte Objekte (`Get-Az*DeploymentWhatIf`) den Parameter **ResultFormat**.

Legen Sie den Formatparameter auf **FullResourcePayloads** fest, um eine Liste der Ressourcen, die sich ändern, sowie Details zu den Eigenschaften, die sich ändern, zu erhalten. Legen Sie den Formatparameter auf **ResourceIdOnly** fest, um eine Liste der Ressourcen zu erhalten, die sich ändern. Der Standardwert ist **FullResourcePayloads**.  

Die folgenden Ergebnisse zeigen die zwei unterschiedlichen Ausgabeformate:

- Vollständige Ressourcennutzlasten

  ```powershell
  Resource and property changes are indicated with these symbols:
    - Delete
    + Create
    ~ Modify

  The deployment will update the following scope:

  Scope: /subscriptions/./resourceGroups/ExampleGroup

    ~ Microsoft.Network/virtualNetworks/vnet-001 [2018-10-01]
      - tags.Owner: "Team A"
      ~ properties.addressSpace.addressPrefixes: [
        - 0: "10.0.0.0/16"
        + 0: "10.0.0.0/15"
        ]
      ~ properties.subnets: [
        - 0:

          name:                     "subnet001"
          properties.addressPrefix: "10.0.0.0/24"

        ]

  Resource changes: 1 to modify.
  ```

- Nur Ressourcen-ID

  ```powershell
  Resource and property changes are indicated with this symbol:
    ! Deploy

  The deployment will update the following scope:

  Scope: /subscriptions/./resourceGroups/ExampleGroup

    ! Microsoft.Network/virtualNetworks/vnet-001

  Resource changes: 1 to deploy.
  ```

## <a name="run-what-if-operation"></a>Ausführen des Was-wäre-wenn-Vorgangs

### <a name="set-up-environment"></a>Einrichten der Umgebung

Um zu sehen, wie Was-wäre-wenn funktioniert, führen wir nun einige Tests aus. Stellen Sie zunächst eine [Vorlage zum Erstellen eines virtuellen Netzwerks](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/what-if/what-if-before.json) bereit. Sie verwenden dieses virtuelle Netzwerk, um zu testen, wie Änderungen bei Was-wäre-wenn gemeldet werden.

```azurepowershell
New-AzResourceGroup `
  -Name ExampleGroup `
  -Location centralus
New-AzResourceGroupDeployment `
  -ResourceGroupName ExampleGroup `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/what-if/what-if-before.json"
```

### <a name="test-modification"></a>Testen der Änderung

Nach Abschluss der Bereitstellung, sind Sie bereit, um den Was-wäre-wenn-Vorgang zu testen. Stellen Sie dieses Mal eine [Vorlage zum Ändern des virtuellen Netzwerks](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/what-if/what-if-after.json) bereit. Es fehlt eins der ursprünglichen Tags, ein Subnetz wurde entfernt, und das Adresspräfix wurde geändert.

```azurepowershell
New-AzResourceGroupDeployment `
  -Whatif `
  -ResourceGroupName ExampleGroup `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/what-if/what-if-after.json"
```

Die Was-wäre-wenn-Ausgabe ähnelt der folgenden:

![Resource Manager-Vorlagenbereitstellung: Was-wäre-wenn-Ausgabe](./media/template-deploy-what-if/resource-manager-deployment-whatif-change-types.png)

Die Textausgabe lautet:

```powershell
Resource and property changes are indicated with these symbols:
  - Delete
  + Create
  ~ Modify

The deployment will update the following scope:

Scope: /subscriptions/./resourceGroups/ExampleGroup

  ~ Microsoft.Network/virtualNetworks/vnet-001 [2018-10-01]
    - tags.Owner: "Team A"
    ~ properties.addressSpace.addressPrefixes: [
      - 0: "10.0.0.0/16"
      + 0: "10.0.0.0/15"
      ]
    ~ properties.subnets: [
      - 0:

        name:                     "subnet001"
        properties.addressPrefix: "10.0.0.0/24"

      ]

Resource changes: 1 to modify.
```

Beachten Sie am Anfang der Ausgabe, dass Farben so definiert sind, dass der Typ der Änderungen angegeben wird.

Im unteren Bereich der Ausgabe wird angezeigt, dass das Tag „Owner“ gelöscht wurde. Das Adresspräfix wurde von 10.0.0.0/16 in 10.0.0.0/15 geändert. Das Subnetz mit dem Namen „subnet001“ wurde gelöscht. Beachten Sie, dass diese Änderungen nicht tatsächlich bereitgestellt wurden. Sie sehen eine Vorschau der Änderungen, die durchgeführt werden, wenn Sie die Vorlage bereitstellen.

Einige der als gelöscht aufgeführten Eigenschaften werden sich tatsächlich nicht ändern. Eigenschaften können fälschlicherweise als gelöscht gemeldet werden, wenn sie nicht in der Vorlage vorhanden sind, sondern während der Bereitstellung automatisch als Standardwerte festgelegt werden. Dieses Ergebnis wird in der Was-wäre-wenn-Antwort als „Rauschen“ (Noise) betrachtet. Für die endgültige bereitgestellte Ressource werden die Werte der Eigenschaften festgelegt. Mit der fortschreitenden Entwicklung des Was-wäre-wenn-Vorgangs werden diese Eigenschaften aus dem Ergebnis herausgefiltert.

## <a name="programmatically-evaluate-what-if-results"></a>Programmgesteuertes Auswerten von Was-wäre-wenn-Ergebnissen

Nun können Sie die Was-wäre-wenn-Ergebnisse programmgesteuert auswerten, indem Sie den Befehl auf eine Variable festlegen.

```azurepowershell
$results = Get-AzResourceGroupDeploymentWhatIfResult `
  -ResourceGroupName ExampleGroup `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/what-if/what-if-after.json"
```

Eine Zusammenfassung der einzelnen Änderungen wird angezeigt.

```azurepowershell
foreach ($change in $results.Changes)
{
  $change.Delta
}
```

## <a name="confirm-deletion"></a>Löschvorgang bestätigen

Der Was-wäre-wenn-Vorgang unterstützt die Verwendung von [Bereitstellungsmodi](deployment-modes.md). Wenn er auf den Modus „Vollständig“ festgelegt ist, werden Ressourcen, die nicht in der Vorlage enthalten sind, gelöscht. Im folgenden Beispiel wird eine [Vorlage im Modus „Vollständig“ bereitgestellt, für die keine Ressourcen definiert sind](https://github.com/Azure/azure-docs-json-samples/blob/master/empty-template/azuredeploy.json).

Wenn Sie vor der Bereitstellung einer Vorlage eine Vorschau der Änderungen anzeigen möchten, verwenden Sie den Parameterschalter `-Confirm` für den Bereitstellungsbefehl. Wenn die Änderungen Ihren Erwartungen entsprechen, bestätigen Sie, dass Sie die Bereitstellung fertig stellen möchten.

```azurepowershell
New-AzResourceGroupDeployment `
  -Confirm `
  -ResourceGroupName ExampleGroup `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/empty-template/azuredeploy.json" `
  -Mode Complete
```

Da in der Vorlage keine Ressourcen definiert sind und der Bereitstellungsmodus auf „Vollständig“ festgelegt ist, wird das virtuelle Netzwerk gelöscht.

![Resource Manager-Vorlagenbereitstellung: Was-wäre-wenn-Ausgabe Bereitstellungsmodus „Vollständig“](./media/template-deploy-what-if/resource-manager-deployment-whatif-output-mode-complete.png)

Die Textausgabe lautet:

```powershell
Resource and property changes are indicated with this symbol:
  - Delete

The deployment will update the following scope:

Scope: /subscriptions/./resourceGroups/ExampleGroup

  - Microsoft.Network/virtualNetworks/vnet-001

      id:
"/subscriptions/./resourceGroups/ExampleGroup/providers/Microsoft.Network/virtualNet
works/vnet-001"
      location:        "centralus"
      name:            "vnet-001"
      tags.CostCenter: "12345"
      tags.Owner:      "Team A"
      type:            "Microsoft.Network/virtualNetworks"

Resource changes: 1 to delete.

Are you sure you want to execute the deployment?
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"):
```

Die erwarteten Änderungen werden angezeigt, und Sie können bestätigen, dass die Bereitstellung ausgeführt werden soll.

## <a name="next-steps"></a>Nächste Schritte

- Wenn Sie falsche Ergebnisse von Was-wäre-wenn aus der Previewversion bemerken, melden Sie die Probleme unter [https://aka.ms/whatifissues](https://aka.ms/whatifissues).
- Informationen zum Bereitstellen von Vorlagen mit Azure PowerShell finden Sie unter [Bereitstellen von Ressourcen mit ARM-Vorlagen und Azure PowerShell](deploy-powershell.md).
- Informationen zum Bereitstellen von Vorlagen mit REST finden Sie unter [Bereitstellen von Ressourcen mit ARM-Vorlagen und der Resource Manager-REST-API](deploy-rest.md).
