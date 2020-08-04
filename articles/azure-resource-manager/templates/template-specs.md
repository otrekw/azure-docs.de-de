---
title: Übersicht der Vorlagenspezifikationen
description: Beschreibt, wie Sie Vorlagenspezifikationen erstellen und diese mit anderen Benutzern in Ihrer Organisation teilen.
ms.topic: conceptual
ms.date: 07/20/2020
ms.author: tomfitz
author: tfitzmac
ms.openlocfilehash: 47dcf44b35ad5c0b77dd0b88d683071a7f2f4ecb
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87094351"
---
# <a name="azure-resource-manager-template-specs-preview"></a>Azure Resource Manager-Vorlagenspezifikationen (Vorschau)

Eine Vorlagenspezifikation ist ein neuer Ressourcentyp zum Speichern einer Azure Resource Manager-Vorlage (ARM-Vorlage) in Azure für die spätere Bereitstellung. Mit diesem Ressourcentyp können Sie ARM-Vorlagen mit anderen Benutzer in Ihrer Organisation teilen. Wie bei jeder anderen Azure-Ressource können Sie die rollenbasierte Zugriffssteuerung (RBAC) verwenden, um die Vorlagenspezifikation zu teilen. Benutzer benötigen nur Lesezugriff auf die Vorlagenspezifikation, um die zugehörige Vorlage bereitzustellen, sodass Sie die Vorlage teilen können, ohne anderen Benutzern das Ändern zu gestatten.

**Microsoft.Resources/templateSpecs** ist der neue Ressourcentyp für Vorlagenspezifikationen. Er besteht aus einer Hauptvorlage und einer beliebigen Anzahl von verknüpften Vorlagen. Azure speichert Vorlagenspezifikationen sicher in Ressourcengruppen. Vorlagenspezifikationen unterstützen [Versionsverwaltung](#versioning).

Zum Bereitstellen der Vorlagenspezifikation verwenden Sie Azure-Standardtools wie PowerShell, Azure CLI, Azure-Portal, REST und andere unterstützte SDKs und Clients. Sie verwenden dieselben Befehle und übergeben dieselben Parameter als Eingabe für die Vorlage.

Der Vorteil der Verwendung von Vorlagenspezifikationen besteht darin, dass Teams in Ihrer Organisation keine Vorlagen für gängige Szenarien neu erstellen oder kopieren müssen. Sie erstellen kanonische Vorlagen und teilen diese. Die Vorlagen, die Sie in eine Vorlagenspezifikation aufnehmen, sollten von Administratoren in Ihrer Organisation überprüft werden, damit sie die Anforderungen und Richtlinien der Organisation einhalten.

> [!NOTE]
> Vorlagenspezifikationen befinden sich derzeit in der Vorschauphase. Um sie zu verwenden, müssen Sie sich [für die Warteliste registrieren](https://aka.ms/templateSpecOnboarding).

## <a name="create-template-spec"></a>Erstellen von Vorlagenspezifikationen

Das folgende Beispiel zeigt eine einfache Vorlage zum Erstellen eines Speicherkonto in Azure.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "resources": [
    {
      "name": "[concat('storage', uniqueString(resourceGroup().id))]",
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2019-06-01",
      "location": "[resourceGroup().location]",
      "kind": "StorageV2",
      "sku": {
        "name": "Premium_LRS",
        "tier": "Premium"
      }
    }
  ]
}
```

Wenn Sie die Vorlagenspezifikation erstellen, werden die PowerShell- oder CLI-Befehle an die Hauptvorlagendatei übergeben. Wenn die Hauptvorlage auf verknüpfte Vorlagen verweist, finden die Befehle diese und verpacken Sie, um die Vorlagenspezifikation zu erstellen. Weitere Informationen finden Sie unter [Erstellen einer Vorlagenspezifikation mit verknüpften Vorlagen](#create-a-template-spec-with-linked-templates).

Erstellen einer Vorlagenspezifikation mittels:

```azurepowershell
New-AzTemplateSpec -Name storageSpec -Version 1.0 -ResourceGroupName templateSpecsRg -TemplateJsonFile ./mainTemplate.json
```

Sie können alle Vorlagenspezifikationen in Ihrem Abonnement anzeigen mithilfe von:

```azurepowershell
Get-AzTemplateSpec
```

Sie können Details einer Vorlagenspezifikation anzeigen, einschließlich ihrer Versionen, mittels:

```azurepowershell
Get-AzTemplateSpec -ResourceGroupName templateSpecsRG -Name storageSpec
```

## <a name="deploy-template-spec"></a>Bereitstellen von Vorlagenspezifikationen

Nachdem Sie die Vorlagenspezifikation erstellt haben, können Benutzer mit **Lese**zugriff auf die Vorlagenspezifikation diese bereitstellen. Informationen zum Erteilen von Zugriffsberechtigungen finden Sie unter [Tutorial: Gewähren des Zugriffs auf Azure-Ressourcen für eine Gruppe mithilfe von Azure PowerShell](../../role-based-access-control/tutorial-role-assignments-group-powershell.md).

Vorlagenspezifikationen können über das Portal, mittels PowerShell, Azure CLI oder als verknüpfte Vorlage in einer größeren Vorlagenbereitstellung bereitgestellt werden. Benutzer in einer Organisation können eine Vorlagenspezifikation in einem beliebigen Bereich in Azure (Ressourcengruppe, Abonnement, Verwaltungsgruppe oder Mandant) bereitstellen.

Anstatt einen Pfad oder URI als Eingabe für eine Vorlage zu übergeben, stellen Sie eine Vorlagenspezifikation bereit, indem Sie deren Ressourcen-ID angeben. Die Ressourcen-ID hat folgendes Format:

**/subscriptions/{Abonnement-ID}/resourceGroups/{Ressourcengruppe}/providers/Microsoft.Resources/templateSpecs/{Vorlagenspezifikation-Name}/versions/{Vorlagenspezifikation-Version}**

Beachten Sie, dass die Ressourcen-ID eine Versionsnummer für die Vorlagenspezifikation enthält.

Beispielsweise stellen Sie eine Vorlagenspezifikation mit dem folgenden PowerShell-Befehl bereit.

```azurepowershell
$id = "/subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/templateSpecsRG/providers/Microsoft.Resources/templateSpecs/storageSpec/versions/1.0"

New-AzResourceGroupDeployment `
  -TemplateSpecId $id `
  -ResourceGroupName demoRG
```

In der Praxis führen Sie in der Regel `Get-AzTemplateSpec` aus, um die ID der Vorlagenspezifikation abzurufen, die Sie bereitstellen möchten.

```azurepowershell
$id = (Get-AzTemplateSpec -Name storageSpec -ResourceGroupName templateSpecsRg -Version 1.0).Version.Id

New-AzResourceGroupDeployment `
  -TemplateSpecId $id `
  -ResourceGroupName demoRG
```

## <a name="create-a-template-spec-with-linked-templates"></a>Erstellen einer Vorlagenspezifikation mit verknüpften Vorlagen

Wenn die Hauptvorlage für Ihre Vorlagenspezifikation auf verknüpfte Vorlagen verweist, können die PowerShell- und CLI-Befehle die verknüpften Vorlagen auf Ihrem lokalen Laufwerk automatisch suchen und verpacken. Sie müssen keine Speicherkonten oder Repositorys manuell konfigurieren, um die Vorlagenspezifikationen zu hosten – alles befindet sich eigenständig in der Vorlagenspezifikationsressource.

Das folgende Beispiel besteht aus einer Hauptvorlage mit zwei verknüpften Vorlagen. Das Beispiel ist nur ein Auszug aus der Vorlage. Beachten Sie, dass es eine Eigenschaft namens `relativePath` verwendet, um die Verknüpfung mit den anderen Vorlagen herzustellen. Sie müssen `apiVersion` von `2020-06-01` oder höher für die Bereitstellungsressource verwenden.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    ...
    "resources": [
        {
            "type": "Microsoft.Resources/deployments",
            "apiVersion": "2020-06-01",
            ...
            "properties": {
                "mode": "Incremental",
                "templateLink": {
                    "relativePath": "artifacts/webapp.json"
                }
            }
        },
        {
            "type": "Microsoft.Resources/deployments",
            "apiVersion": "2020-06-01",
            ...
            "properties": {
                "mode": "Incremental",
                "templateLink": {
                    "relativePath": "artifacts/database.json"
                }
            }
        }
    ],
    "outputs": {}
}

```

Wenn der PowerShell- oder CLI-Befehl zum Erstellen der Vorlagenspezifikation für das vorherige Beispiel ausgeführt wird, findet der Befehl drei Dateien: die Hauptvorlage, die Web-App-Vorlage (`webapp.json`) und die Datenbankvorlage (`database.json`) und verpackt diese in die Vorlagenspezifikation.

Weitere Informationen finden Sie im [Tutorial: Erstellen einer Vorlagenspezifikation mit verknüpften Vorlagen](template-specs-create-linked.md).

## <a name="deploy-template-spec-as-a-linked-template"></a>Bereitstellen einer Vorlagenspezifikation als verknüpfte Vorlage

Nachdem Sie eine Vorlagenspezifikation erstellt haben, ist es einfach, sie aus einer ARM-Vorlage oder einer anderen Vorlagenspezifikation wiederzuverwenden. Sie stellen eine Verknüpfung mit einer Vorlagenspezifikation her, indem Sie deren Ressourcen-ID zu Ihrer Vorlage hinzufügen. Die verknüpfte Vorlagenspezifikation wird automatisch bereitgestellt, wenn Sie die Hauptvorlage bereitstellen. Mit diesem Verhalten können Sie modulare Vorlagenspezifikationen entwickeln und diese ganz nach Bedarf wiederverwenden.

Beispielsweise können Sie eine Vorlagenspezifikation erstellen, die Netzwerkressourcen bereitstellt, sowie eine weitere Vorlagenspezifikation, die Speicherressourcen bereitstellt. In ARM-Vorlagen stellen Sie jederzeit eine Verknüpfung mit diesen beiden Vorlagenspezifikationen her, wenn Sie Netzwerk- oder Speicherressourcen konfigurieren müssen.

Das folgende Beispiel ähnelt dem vorherigen Beispiel, aber Sie verwenden die `id`-Eigenschaft, um eine Verknüpfung mit einer Vorlagenspezifikation herzustellen, anstatt mit der `relativePath`-Eigenschaft eine Verknüpfung miteiner lokalen Vorlage herzustellen. Verwenden Sie `2020-06-01` als API-Version für die Bereitstellungsressource. In dem Beispiel befinden sich die Vorlagenspezifikationen in einer Ressourcengruppe namens **templateSpecsRG**.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    ...
    "resources": [
        {
            "type": "Microsoft.Resources/deployments",
            "apiVersion": "2020-06-01",
            "name": "networkingDeployment",
            ...
            "properties": {
                "mode": "Incremental",
                "templateLink": {
                    "id": "[resourceId('templateSpecsRG', 'Microsoft.Resources/templateSpecs/versions', 'networkingSpec', '1.0')]"
                }
            }
        },
        {
            "type": "Microsoft.Resources/deployments",
            "apiVersion": "2020-06-01",
            "name": "storageDeployment",
            ...
            "properties": {
                "mode": "Incremental",
                "templateLink": {
                    "id": "[resourceId('templateSpecsRG', 'Microsoft.Resources/templateSpecs/versions', 'storageSpec', '1.0')]"
                }
            }
        }
    ],
    "outputs": {}
}
```

Weitere Informationen zum Verknüpfen von Vorlagenspezifikationen finden Sie unter [Tutorial: Bereitstellen einer Vorlagenspezifikation als verknüpfte Vorlage](template-specs-deploy-linked-template.md).

## <a name="versioning"></a>Versionsverwaltung

Wenn Sie eine Vorlagenspezifikation erstellen, geben Sie eine Versionsnummer dafür an. Wenn Sie den Vorlagencode durchlaufen, können Sie entweder eine vorhandene Version aktualisieren (für Hotfixes) oder eine neue Version veröffentlichen. Die Version ist eine Textzeichenfolge. Sie können sich für ein beliebiges Versionskontrollsystem entscheiden, einschließlich einer semantischen Versionsverwaltung. Benutzer der Vorlagenspezifikation können die Versionsnummer angeben, die sie bei der Bereitstellung verwenden möchten.

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Erstellen und Bereitstellen einer Vorlagenspezifikation finden Sie unter [Schnellstart: Erstellen und Bereitstellen von Vorlagenspezifikationen](quickstart-create-template-specs.md).

* Weitere Informationen zum Verknüpfen von Vorlagen in Vorlagenspezifikationen finden Sie unter [Tutorial: Erstellen einer Vorlagenspezifikation mit verknüpften Vorlagen](template-specs-create-linked.md).

* Weitere Informationen zum Bereitstellen einer Vorlagenspezifikation als verknüpfte Vorlage finden Sie unter [Tutorial: Bereitstellen einer Vorlagenspezifikation als verknüpfte Vorlage](template-specs-deploy-linked-template.md).
