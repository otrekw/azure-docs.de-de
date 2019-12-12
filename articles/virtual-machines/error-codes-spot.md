---
title: Fehlercodes für Azure Spot-VMs und Skalierungsgruppeninstanzen
description: Erfahren Sie mehr über Fehlercodes, die möglicherweise bei der Verwendung von Spot-VMs und Skalierungsgruppeninstanzen angezeigt werden.
services: virtual-machines-windows
author: cynthn
manager: gwallace
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.topic: article
ms.date: 11/20/2019
ms.author: cynthn
ms.openlocfilehash: 97e68f4d0d671ffa0f697c484c502b9070a3f20f
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/03/2019
ms.locfileid: "74781834"
---
# <a name="preview-error-messages-for-spot-vms-and-scale-sets"></a>Vorschau: Fehlermeldungen für Spot-VMs und Skalierungsgruppen


> [!IMPORTANT]
> Spot-VMs und VM-Skalierungsgruppen befindet sich derzeit in der öffentlichen Vorschauversion.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).


Im Folgenden finden Sie einige mögliche Fehlercodes, die Sie bei der Verwendung von Spot-VMs und Skalierungsgruppen erhalten können.


| Schlüssel | `Message` | BESCHREIBUNG |
|-----|---------|-------------|
| SkuNotAvailable | Der angeforderte Tarif für die Ressource \<Ressource\> ist im Speicherort \<Speicherort\> für das Abonnement \<Abonnement-ID\> derzeit nicht verfügbar. Versuchen Sie es mit einem anderen Tarif, oder führen Sie die Bereitstellung an einem anderen Standort durch. | An diesem Standort ist nicht genügend Azure Spot-Kapazität vorhanden, um Ihre VM- oder Skalierungsgruppeninstanz zu erstellen. |
| EvictionPolicyCanBeSetOnlyOnAzureSpotVirtualMachines  |  Die Entfernungsrichtlinie kann nur für Azure Spot-VMs festgelegt werden. | Diese VM ist keine Spot-VM, sodass Sie die Entfernungsrichtlinie nicht festlegen können. |
| AzureSpotVMNotSupportedInAvailabilitySet  |  Eine Azure Spot-VM wird in der Verfügbarkeitsgruppe nicht unterstützt. | Sie müssen entweder eine Spot-VM oder eine VM in einer Verfügbarkeitsgruppe verwenden, Sie können nicht beide auswählen. |
| AzureSpotFeatureNotEnabledForSubscription  |  Das Abonnement ist nicht mit dem Azure Spot-Feature aktiviert. | Sie benötigen ein Abonnement, das Spot-VMS unterstützt. |
| VMPriorityCannotBeApplied  |  Der angegebene Prioritätswert {0} kann nicht auf die VM {1} angewendet werden, weil bei der Erstellung der VM keine Priorität angegeben wurde. | Sie müssen bei der Erstellung der VM die Priorität angeben. |
| SpotPriceGreaterThanProvidedMaxPrice  |  Der Vorgang {0} kann nicht ausgeführt werden, weil der angegebene Höchstpreis von {1} USD niedriger ist als der aktuelle Spot-Preis von {2} USD für die Azure Spot-VM-Größe {3}. | Wählen Sie einen höheren Höchstpreis aus. Weitere Informationen finden Sie in den Preisinformationen für [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) bzw. für [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/).|
| MaxPriceValueInvalid  |  Ungültiger Wert für Höchstpreis. Die einzigen unterstützten Werte für den Höchstpreis sind -1 oder eine Dezimalzahl größer als 0. Der Wert „-1“ für den Höchstpreis gibt an, dass die Azure Spot-VM nicht aus Preisgründen entfernt wird. | Geben Sie einen gültigen Höchstpreis ein. Weitere Informationen finden Sie bei den Preisen für [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) bzw. [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/). |
| MaxPriceChangeNotAllowedForAllocatedVMs | Eine Änderung des Höchstpreises ist nicht erlaubt, wenn die VM {0} derzeit zugeordnet ist. Heben Sie die Zuordnungen auf, und versuchen Sie es nochmal. | Beenden Sie die VM, oder heben Sie deren Zuordnung auf, damit Sie den Höchstpreis ändern können. |
| MaxPriceChangeNotAllowed | Eine Änderung des Höchstpreises ist nicht zulässig. | Sie können den Höchstpreis für diese VM nicht ändern. |
| AzureSpotIsNotSupportedForThisAPIVersion  |  Azure Spot wird für diese API-Version nicht unterstützt. | Die API-Version muss 2019-03-01 lauten. |
| AzureSpotIsNotSupportedForThisVMSize  |  Azure Spot wird für diese VM-Größe {0} nicht unterstützt. | Wählen Sie eine andere VM-Größe aus. Weitere Informationen finden Sie unter [Spot-VMs](./linux/spot-vms.md). |
| MaxPriceIsSupportedOnlyForAzureSpotVirtualMachines  |  Der Höchstpreis wird nur für Azure Spot-VMs unterstützt. | Weitere Informationen finden Sie unter [Spot-VMs](./linux/spot-vms.md). |
| MoveResourcesWithAzureSpotVMNotSupported  |  Die Anforderung zum Verschieben von Ressourcen enthält eine Azure Spot-VM. Dies wird derzeit nicht unterstützt. Überprüfen Sie die Fehlerdetails für die IDs der virtuellen Computer. | Spot-VMs können nicht verschoben werden. |
| MoveResourcesWithAzureSpotVmssNotSupported  |  Die Anforderung zum Verschieben von Ressourcen enthält eine Azure Spot-VM-Skalierungsgruppe. Dies wird derzeit nicht unterstützt. Überprüfen Sie die Fehlerdetails für die IDs der VM-Skalierungsgruppen. | Spot-Skalierungsgruppeninstanzen können nicht verschoben werden. |
| EphemeralOSDisksNotSupportedForSpotVMs | Kurzlebige Betriebssystemdatenträger werden für Spot-VMs nicht unterstützt. | Sie müssen einen regulären Betriebssystemdatenträger für Ihre Spot-VM verwenden. |
| AzureSpotVMNotSupportedInVmssWithVMOrchestrationMode | Azure Spot-VM wird in der VM-Skalierungsgruppe mit dem VM-Orchestrierungsmodus nicht unterstützt. | Legen Sie den Orchestrierungsmodus auf die VM-Skalierungsgruppe fest, um Spot-Instanzen zu verwenden. |


**Nächste Schritte**: Weitere Informationen finden Sie unter [Spot-VMs](./linux/spot-vms.md).