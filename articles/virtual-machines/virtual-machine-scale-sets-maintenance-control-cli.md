---
title: Wartungssteuerung für Upgrades von Betriebssystemimages in Azure-VM-Skalierungsgruppen über die Azure CLI
description: Hier erfahren Sie, wie Sie mithilfe der Wartungssteuerung und der Azure CLI steuern können, wann automatische Upgrades von Betriebssystemimages in Ihren Azure-VM-Skalierungsgruppen eingeführt werden.
author: ju-shim
ms.service: virtual-machine-scale-sets
ms.topic: how-to
ms.workload: infrastructure-services
ms.date: 06/01/2021
ms.author: jushiman
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: e00341b38cca73ccd351b4e16ce71317a8d0c751
ms.sourcegitcommit: 8651d19fca8c5f709cbb22bfcbe2fd4a1c8e429f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/14/2021
ms.locfileid: "112073046"
---
# <a name="maintenance-control-for-os-image-upgrades-on-azure-virtual-machine-scale-sets-using-azure-cli"></a>Wartungssteuerung für Upgrades von Betriebssystemimages in Azure-VM-Skalierungsgruppen über die Azure CLI

Über die Wartungssteuerung können Sie entscheiden, wann automatische Gastbetriebssystemimageupgrades auf Ihre VM-Skalierungsgruppen angewendet werden sollen. In diesem Thema werden die Azure CLI-Optionen für die Wartungssteuerung behandelt. Weitere Informationen zur Verwendung der Wartungssteuerung finden Sie unter [Wartungssteuerung für Azure-VM-Skalierungsgruppen](virtual-machine-scale-sets-maintenance-control.md).


## <a name="create-a-maintenance-configuration"></a>Erstellen einer Wartungskonfiguration

Verwenden Sie `az maintenance configuration create`, um eine Wartungskonfiguration zu erstellen. In diesem Beispiel wird eine Wartungskonfiguration mit dem Namen *myConfig* erstellt, die für das Betriebssystemimage gilt. 

```azurecli-interactive
az group create \
   --location eastus \
   --name myMaintenanceRG
az maintenance configuration create \
   -g myMaintenanceRG \
   --resource-name myConfig \
   --maintenance-scope osimage\
   --location eastus
```

Kopieren Sie die Konfigurations-ID aus der Ausgabe, um sie später zu verwenden.

Durch die Verwendung von `--maintenance-scope osimage` wird sichergestellt, dass die Wartungskonfiguration zum Steuern von Updates für das Gastbetriebssystem verwendet wird.

Wenn Sie versuchen, eine Konfiguration mit dem gleichen Namen, aber an einem anderen Speicherort zu erstellen, erhalten Sie eine Fehlermeldung. Konfigurationsnamen müssen für Ihre Ressourcengruppe eindeutig sein.

Mithilfe von `az maintenance configuration list` können Sie verfügbare Wartungskonfigurationen abfragen.

```azurecli-interactive
az maintenance configuration list --query "[].{Name:name, ID:id}" -o table 
```

### <a name="create-a-maintenance-configuration-with-a-scheduled-window"></a>Erstellen einer Wartungskonfiguration mit einem geplanten Fenster
Sie können auch ein geplantes Fenster deklarieren, in dem Azure die Updates auf Ihre Ressourcen anwendet. In diesem Beispiel wird eine Wartungskonfiguration mit dem Namen *myConfig* mit einem geplanten Fenster von fünf Stunden am vierten Montag jedes Monats erstellt. Nachdem Sie ein geplantes Fenster erstellt haben, müssen Sie die Updates nicht mehr manuell anwenden.

> [!IMPORTANT]
> Die Dauer (**Duration**) der Wartung muss *5 Stunden* oder länger sein. Die Wartungswiederholung (**RecurEvery**) muss auf *Day* festgelegt werden.

```azurecli-interactive
az maintenance configuration create \
   -g myMaintenanceRG \
   --resource-name myConfig \
   --maintenance-scope osimage \
   --location eastus \
   --maintenance-window-duration "05:00" \
   --maintenance-window-recur-every "Day" \
   --maintenance-window-start-date-time "2020-12-30 08:00" \
   --maintenance-window-time-zone "Pacific Standard Time"
```

## <a name="assign-the-configuration"></a>Zuweisen der Konfiguration

Weisen Sie Ihrer VM-Skalierungsgruppe die Konfiguration mithilfe von `az maintenance assignment create` zu.


## <a name="enable-automatic-os-upgrade"></a>Aktivieren von automatischen Betriebssystemupgrades

Sie können automatische Betriebssystemupgrades für jede VM-Skalierungsgruppe aktivieren, für die die Wartungssteuerung verwendet werden soll. Weitere Informationen zum Aktivieren automatischer Betriebssystemupgrades für Ihre VM-Skalierungsgruppe finden Sie unter [Automatische Betriebssystemimageupgrades für Azure-VM-Skalierungsgruppen](../virtual-machine-scale-sets/virtual-machine-scale-sets-automatic-upgrade.md).


## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erfahren Sie mehr über Wartung und Updates für VMs in Azure](maintenance-and-updates.md)
