---
title: Automatische Instanzreparaturen mit Azure-VM-Skalierungsgruppen
description: Erfahren Sie, wie Sie Richtlinien für automatische Reparaturen für VM-Instanzen in einer Skalierungsgruppe konfigurieren
author: avirishuv
manager: vashan
tags: azure-resource-manager
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm
ms.topic: conceptual
ms.date: 02/28/2020
ms.author: avverma
ms.openlocfilehash: f335b0fb3396103c321d740bcf6d125e60e95086
ms.sourcegitcommit: d45fd299815ee29ce65fd68fd5e0ecf774546a47
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2020
ms.locfileid: "78274578"
---
# <a name="preview-automatic-instance-repairs-for-azure-virtual-machine-scale-sets"></a>Vorschau: Automatische Instanzreparaturen für Azure-VM-Skalierungsgruppen

Durch das Aktivieren von automatischen Instanzreparaturen für Azure-VM-Skalierungsgruppen können Sie Hochverfügbarkeit für Anwendungen erzielen, indem Sie eine Reihe von fehlerfreien Instanzen verwalten. Wenn eine Instanz in der Skalierungsgruppe fehlerhaft ist, wie von der [Anwendungsintegritätserweiterung](./virtual-machine-scale-sets-health-extension.md) oder [Load Balancer-Integritätstests](../load-balancer/load-balancer-custom-probe-overview.md) gemeldet, führt diese Funktion automatisch eine Instanzreparatur aus und erstellt eine neue Instanz, um sie zu ersetzen.

> [!NOTE]
> Diese Vorschaufunktion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen.

## <a name="requirements-for-using-automatic-instance-repairs"></a>Anforderungen für die Verwendung automatischer Instanzreparaturen

**Abonnieren automatischer Instanzreparaturen (Vorschau)**

Verwenden der REST-API oder von Azure PowerShell zum Abonnieren der Vorschau automatischer Instanzreparaturen. Mit diesen Schritten registrieren Sie Ihr Abonnement für die Vorschaufunktion. Beachten Sie, dass für die Verwendung dieser Funktion nur eine einmalige Einrichtung erforderlich ist. Wenn Ihr Abonnement bereits für automatische Instanzreparaturen (Vorschau) registriert ist, müssen Sie sich nicht erneut registrieren. 

Verwenden der REST-API 

1. Registrieren für das Feature mit [Features > Registrieren](/rest/api/resources/features/register) 

```
POST on '/subscriptions/{subscriptionId}/providers/Microsoft.Features/providers/Microsoft.Compute/features/RepairVMScaleSetInstancesPreview/register?api-version=2015-12-01'
```

```json
{
  "properties": {
    "state": "Registering"
  },
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Features/providers/Microsoft.Compute/features/RepairVMScaleSetInstancesPreview",
  "type": "Microsoft.Features/providers/features",
  "name": "Microsoft.Compute/RepairVMScaleSetInstancesPreview"
}
```

2. Warten Sie einige Minuten, bis sich der *Status* in *Registriert* ändert. Sie können die folgende API verwenden, um dies zu bestätigen.

```
GET on '/subscriptions/{subscriptionId}/providers/Microsoft.Features/providers/Microsoft.Compute/features/RepairVMScaleSetInstancesPreview?api-version=2015-12-01'
```

```json
{
  "properties": {
    "state": "Registered"
  },
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Features/providers/Microsoft.Compute/features/RepairVMScaleSetInstancesPreview",
  "type": "Microsoft.Features/providers/features",
  "name": "Microsoft.Compute/RepairVMScaleSetInstancesPreview"
}
```

3. Nachdem sich der *Status* in *Registriert* geändert hat, führen Sie Folgendes aus.

```
POST on '/subscriptions/{subscriptionId}/providers/Microsoft.Compute/register?api-version=2015-12-01'
```

Verwenden von Azure PowerShell

1. Registrieren für das Feature mit dem Cmdlet [Register-AzureRmResourceProvider](/powershell/module/azurerm.resources/register-azurermresourceprovider), gefolgt von [Register-AzureRmProviderFeature](/powershell/module/azurerm.resources/register-azurermproviderfeature)

```azurepowershell-interactive
Register-AzureRmResourceProvider `
 -ProviderNamespace Microsoft.Compute

Register-AzureRmProviderFeature `
 -ProviderNamespace Microsoft.Compute `
 -FeatureName RepairVMScaleSetInstancesPreview
```

2. Warten Sie einige Minuten, bis sich *RegistrationState* in *Registriert* ändert. Sie können das folgende Cmdlet verwenden, um dies zu bestätigen.

```azurepowershell-interactive
Get-AzureRmProviderFeature `
 -ProviderNamespace Microsoft.Compute `
 -FeatureName RepairVMScaleSetInstancesPreview
 ```

 Die Antwort sollte wie folgt aussehen.

| FeatureName                           | ProviderName            | RegistrationState       |
|---------------------------------------|-------------------------|-------------------------|
| RepairVMScaleSetInstancesPreview      | Microsoft.Compute       | Registriert              |

3. Nachdem sich *RegistrationState* in *Registriert* geändert hat, führen Sie das folgende Cmdlet aus.

```azurepowershell-interactive
Register-AzureRmResourceProvider `
 -ProviderNamespace Microsoft.Compute
```

**Aktivieren der Anwendungsintegritätsüberwachung für eine Skalierungsgruppe**

Für die Skalierungsgruppe sollte die Anwendungsintegritätsüberwachung für Instanzen aktiviert sein. Verwenden Sie zu diesem Zweck die [Erweiterung für Anwendungsintegrität](./virtual-machine-scale-sets-health-extension.md) oder [Lastenausgleichs-Integritätstests](../load-balancer/load-balancer-custom-probe-overview.md). Nur eine dieser beiden Optionen kann gleichzeitig aktiviert werden. Die Erweiterung für Anwendungsintegrität oder der Lastenausgleich testet den für die VM-Instanzen konfigurierten Anwendungsendpunkt mit Ping, um den Integritätsstatus der Anwendung zu ermitteln. Dieser Integritätsstatus wird vom Orchestrator der Skalierungsgruppe verwendet, um die Instanzintegrität zu überwachen und bei Bedarf Reparaturen auszuführen.

**Konfigurieren des Endpunkts zum Bereitstellen des Integritätsstatus**

Vergewissern Sie sich vor dem Aktivieren der Richtlinie für automatische Instanzreparatur, dass für die Skalierungsgruppeninstanzen ein Anwendungsendpunkt konfiguriert ist, der den Integritätsstatus der Anwendung ausgeben kann. Wenn eine Instanz den Status 200 (OK) für diesen Anwendungsendpunkt zurückgibt, wird die Instanz als „Fehlerfrei“ gekennzeichnet. In allen anderen Fällen wird die Instanz als „Fehlerhaft“ gekennzeichnet, beispielsweise auch in den folgenden Szenarien:

- Wenn kein Anwendungsendpunkt innerhalb der VM-Instanzen konfiguriert ist, um den Integritätsstatus der Anwendung bereitzustellen.
- Wenn der Anwendungsendpunkt nicht ordnungsgemäß konfiguriert ist.
- Wenn der Anwendungsendpunkt nicht erreichbar ist.

Für Instanzen, die als "Fehlerhaft" markiert sind, werden automatische Reparaturen durch die Skalierungsgruppe ausgelöst. Stellen Sie sicher, dass der Anwendungsendpunkt ordnungsgemäß konfiguriert ist, bevor Sie die Richtlinie für automatische Reparaturen aktivieren, um unbeabsichtigte Instanzreparaturen zu vermeiden, während der Endpunkt konfiguriert wird.

**Aktivieren einer einzelnen Platzierungsgruppe**

Diese Vorschau ist derzeit nur für Skalierungsgruppen verfügbar, die als einzelne Platzierungsgruppe bereitgestellt werden. Die Eigenschaft *SinglePlacementGroup* muss auf *TRUE* festgelegt sein, damit Ihre Skalierungsgruppe das Feature für automatische Instanzreparaturen verwendet. Weitere Informationen zu [Platzierungsgruppen](./virtual-machine-scale-sets-placement-groups.md#placement-groups).

**API-Version**

Die Richtlinie für automatische Reparaturen wird für die API-Computeversion 2018-10-01 oder höher unterstützt.

**Einschränkungen beim Verschieben von Ressourcen oder Abonnements**

Im Rahmen dieser Vorschau werden Ressourcen- oder Abonnementverschiebungen für Skalierungsgruppen derzeit nicht unterstützt, wenn die Richtlinie für automatische Reparaturen aktiviert ist.

**Einschränkung für Service Fabric-Skalierungsgruppen**

Dieses Vorschaufeature wird für Service Fabric-Skalierungsgruppen zurzeit nicht unterstützt.

## <a name="how-do-automatic-instance-repairs-work"></a>Wie funktionieren automatische Instanzreparaturen?

Das Feature für automatische Instannreparatur basiert auf der Integritätsüberwachung einzelner Instanzen in einer Skalierungsgruppe. VM-Instanzen in einer Skalierungsgruppe können so konfiguriert werden, dass der Integritätsstatus der Anwendung entweder mit der [Erweiterung für die Anwendungsintegrität](./virtual-machine-scale-sets-health-extension.md) oder mit [Lastenausgleichs-Integritätstests](../load-balancer/load-balancer-custom-probe-overview.md) ausgegeben wird. Wenn eine Instanz als fehlerhaft eingestuft wird, führt die Skalierungsgruppe eine Reparaturaktion aus, indem sie die fehlerhafte Instanz löscht und eine neue Instanz erstellt, um sie zu ersetzen. Diese Funktion kann im VM-Skalierungsgruppenmodell mithilfe des *automaticRepairsPolicy*-Objekts aktiviert werden.

### <a name="batching"></a>Batching

Die automatischen Instanzreparaturvorgänge werden in Batches ausgeführt. Zu jedem Zeitpunkt werden nicht mehr als 5 % der Instanzen in der Skalierungsgruppe über die Richtlinie für automatische Reparaturen repariert. Dadurch wird verhindert, dass eine große Anzahl von Instanzen gleichzeitig gelöscht und neu erstellt wird, wenn diese gleichzeitig als fehlerhaft eingestuft werden.

### <a name="grace-period"></a>Toleranzperiode

Wenn eine Instanz aufgrund einer PUT-, PATCH- oder POST-Aktion, die für die Skalierungsgruppe ausgeführt wird (z. B. Reimaging, erneute Bereitstellung, Aktualisierung usw.), einen Statusänderungsvorgang durchläuft, wird jede Reparaturaktion für diese Instanz erst ausgeführt, nachdem die Toleranzperiode abgewartet wurde. Die Toleranzperiode ist die Zeitspanne, die es der Instanz ermöglicht, in einen fehlerfreien Zustand zurückzukehren. Die Toleranzperiode startet, nachdem die Statusänderung abgeschlossen wurde. Dadurch werden vorzeitige oder versehentliche Reparaturvorgänge vermieden. Die Toleranzperiode wird für alle neu erstellten Instanzen in der Skalierungsgruppe (einschließlich der als Ergebnis eines Reparaturvorgangs erstellten Instanz) eingehalten. Die Toleranzperiode wird im ISO 8601-Format in Minuten angegeben und kann mithilfe der Eigenschaft *automaticRepairsPolicy.gracePeriod* festgelegt werden. Die Toleranzperiode kann zwischen 30 Minuten und 90 Minuten betragen und weist einen Standardwert von 30 Minuten auf.

Der automatische Instannreparaturvorgang funktioniert wie folgt:

1. Die [Erweiterung für Anwendungsintegrität](./virtual-machine-scale-sets-health-extension.md) oder [Lastenausgleichs-Integritätstests](../load-balancer/load-balancer-custom-probe-overview.md) pingen den Anwendungsendpunkt innerhalb der einzelnen virtuellen Computer in der Skalierungsgruppe pingen, um den Integritätsstatus der Anwendung für jede Instanz abzurufen.
2. Wenn der Endpunkt mit dem Status 200 (OK) antwortet, wird die Instanz als „Fehlerfrei“ gekennzeichnet. In allen anderen Fällen (auch wenn der Endpunkt nicht erreichbar ist) wird die Instanz als „Fehlerhaft“ markiert.
3. Wenn eine Instanz als fehlerhaft eingestuft wird, löst die Skalierungsgruppe eine Reparaturaktion aus, indem sie die fehlerhafte Instanz löscht und eine neue Instanz erstellt, um sie zu ersetzen.
4. Instanzreparaturen werden in Batches ausgeführt. Zu jedem Zeitpunkt werden nicht mehr als 5 % der Gesamtzahl der Instanzen in der Skalierungsgruppe repariert. Wenn eine Skalierungsgruppe über weniger als 20 Instanzen verfügt, werden die Reparaturen für jeweils eine fehlerhafte Instanz gleichzeitig durchgeführt.
5. Der oben beschriebene Vorgang wird fortgesetzt, bis alle fehlerhaften Instanzen in der Skalierungsgruppe repariert wurden.

## <a name="instance-protection-and-automatic-repairs"></a>Instanzschutz und automatische Reparaturen

Wenn eine Instanz in einer Skalierungsgruppe geschützt ist, indem Sie die *[Schutzrichtlinie „Vor Aktionen für Skalierungsgruppen schützen“](./virtual-machine-scale-sets-instance-protection.md#protect-from-scale-set-actions)* anwenden, werden automatische Reparaturen für diese Instanz nicht ausgeführt.

## <a name="enabling-automatic-repairs-policy-when-creating-a-new-scale-set"></a>Aktivieren der Richtlinie für automatische Reparaturen beim Erstellen einer neuen Skalierungsgruppe

Um die Richtlinie für automatische Reparaturen beim Erstellen einer neuen Skalierungsgruppe zu aktivieren, stellen Sie sicher, dass alle [Anforderungen](#requirements-for-using-automatic-instance-repairs) erfüllt sind, die für das Abonnieren dieses Features erforderlich sind. Der Anwendungsendpunkt sollte für Skalierungsgruppeninstanzen ordnungsgemäß konfiguriert werden, um zu vermeiden, dass unbeabsichtigte Reparaturen ausgelöst werden, während der Endpunkt konfiguriert wird. Bei neu erstellten Skalierungsgruppen werden alle Instanzreparaturen ausgeführt, nachdem für die Dauer der Toleranzperiode gewartet wurde. Um die automatische Instanzreparatur in einer Skalierungsgruppe zu aktivieren, verwenden Sie das *automaticRepairsPolicy*-Objekt im VM-Skalierungsgruppenmodell.

### <a name="rest-api"></a>REST-API

Im folgenden Beispiel wird gezeigt, wie die automatische Instanzreparatur in einem Skalierungsgruppenmodell aktiviert wird. Verwenden Sie die API-Version 2018-10-01 oder höher.

```
PUT or PATCH on '/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}?api-version=2019-07-01'
```

```json
{
  "properties": {
    "automaticRepairsPolicy": {
            "enabled": "true",
            "gracePeriod": "PT30M"
        }
    }
}
```

### <a name="azure-powershell"></a>Azure PowerShell

Die Funktion für automatische Instanzreparatur kann beim Erstellen einer neuen Skalierungsgruppe mithilfe des Cmdlets [New-AzVmssConfig](/powershell/module/az.compute/new-azvmssconfig) aktiviert werden. Dieses Beispielskript führt Sie schrittweise durch die Erstellung einer Skalierungsgruppe und zugehöriger Ressourcen mithilfe der Konfigurationsdatei: [Erstellen Sie eine vollständige VM-Skalierungsgruppe](./scripts/powershell-sample-create-complete-scale-set.md). Sie können die Richtlinie für automatische Instanzreparaturen konfigurieren, indem Sie die Parameter *EnableAutomaticRepair* und *AutomaticRepairGracePeriod* dem Konfigurationsobjekt hinzufügen, um die Skalierungsgruppe zu erstellen. Das folgende Beispiel aktiviert das Feature mit einer Toleranzperiode von 30 Minuten.

```azurepowershell-interactive
New-AzVmssConfig `
 -Location "EastUS" `
 -SkuCapacity 2 `
 -SkuName "Standard_DS2" `
 -UpgradePolicyMode "Automatic" `
 -EnableAutomaticRepair $true `
 -AutomaticRepairGracePeriod "PT30M"
```

## <a name="enabling-automatic-repairs-policy-when-updating-an-existing-scale-set"></a>Aktivieren der Richtlinie für automatische Reparaturen beim Aktualisieren einer vorhandenen Skalierungsgruppe

Bevor Sie die Richtlinie für automatische Reparaturen in einer vorhandenen Skalierungsgruppe aktivieren, stellen Sie sicher, dass alle [Anforderungen](#requirements-for-using-automatic-instance-repairs) erfüllt sind, die für das Abonnieren dieses Features erforderlich sind. Der Anwendungsendpunkt sollte für Skalierungsgruppeninstanzen ordnungsgemäß konfiguriert werden, um zu vermeiden, dass unbeabsichtigte Reparaturen ausgelöst werden, während der Endpunkt konfiguriert wird. Um die automatische Instanzreparatur in einer Skalierungsgruppe zu aktivieren, verwenden Sie das *automaticRepairsPolicy*-Objekt im VM-Skalierungsgruppenmodell.

### <a name="rest-api"></a>REST-API

Das folgende Beispiel aktiviert die Richtlinie mit einer Toleranzperiode von 40 Minuten. Verwenden Sie die API-Version 2018-10-01 oder höher.

```
PUT or PATCH on '/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}?api-version=2019-07-01'
```

```json
{
  "properties": {
    "automaticRepairsPolicy": {
            "enabled": "true",
            "gracePeriod": "PT40M"
        }
    }
}
```

### <a name="azure-powershell"></a>Azure PowerShell

Verwenden Sie das Cmdlet [Update-AzVmss](/powershell/module/az.compute/update-azvmss), um die Konfiguration des automatischen Instanzreparaturfeatures in einer vorhandenen Skalierungsgruppe zu ändern. Im folgenden Beispiel wird die Toleranzperiode auf 40 Minuten aktualisiert.

```azurepowershell-interactive
Update-AzVmss `
 -ResourceGroupName "myResourceGroup" `
 -VMScaleSetName "myScaleSet" `
 -EnableAutomaticRepair $true `
 -AutomaticRepairGracePeriod "PT40M"
```

## <a name="troubleshoot"></a>Problembehandlung

**Fehler beim Aktivieren der Richtlinie für automatische Reparaturen**

Wenn Sie einen Fehler „BadRequest“ mit der Meldung „Could not find member 'automaticRepairsPolicy' on object of type 'properties'“ (Member „automaticRepairsPolicy“ für Objekt vom Typ „properties“ wurde nicht gefunden) erhalten, überprüfen Sie die API-Version, die für die VM-Skalierungsgruppe verwendet wird. Für dieses Feature ist API-Version 2018-10-01 oder höher erforderlich.

**Instanz wird auch bei aktivierter Richtlinie nicht repariert**

Die Instanz kann sich in der Toleranzperiode befinden. Dies ist die Zeitspanne, die gewartet werden soll, nachdem eine beliebige Zustandsänderung der Instanz erfolgt ist, bevor Reparaturen durchgeführt werden. Dadurch werden vorzeitige oder versehentliche Reparaturvorgänge vermieden. Die Reparaturaktion sollte durchgeführt werden, sobald die Toleranzperiode für die Instanz abgeschlossen ist.

**Anzeigen des Integritätsstatus der Anwendung für Skalierungsgruppeninstanzen**

Zum Anzeigen des Integritätsstatus der Anwendung können Sie die [API zum Abrufen der Instanzenansicht](/rest/api/compute/virtualmachinescalesetvms/getinstanceview) für Instanzen in einer VM-Skalierungsgruppe verwenden. Mit Azure PowerShell können Sie das Cmdlet [Get-AzVmssVM](/powershell/module/az.compute/get-azvmssvm) mit dem Flag *-InstanceView* verwenden. Der Integritätsstatus der Anwendung wird unter der Eigenschaft *vmHealth* bereitgestellt.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie die [Erweiterung für Anwendungsintegrität](./virtual-machine-scale-sets-health-extension.md) oder [Lastenausgleichs-Integritätstests](../load-balancer/load-balancer-custom-probe-overview.md) für Ihre Skalierungsgruppen konfiguriert werden.
