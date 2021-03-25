---
title: Zuordnen von Azure-Datenträgern zu Linux-VM-Gastdatenträgern
description: Erfahren Sie, wie Sie die Azure-Datenträger ermitteln, die den Gastdatenträgern einer Linux-VM zugrunde liegen.
author: timbasham
ms.service: virtual-machines
ms.subservice: disks
ms.workload: infrastructure-services
ms.topic: how-to
ms.date: 11/17/2020
ms.author: tibasham
ms.collection: linux
ms.openlocfilehash: bc6c6273ab3d1a4403763e4ed0a8c491995fb2df
ms.sourcegitcommit: e6de1702d3958a3bea275645eb46e4f2e0f011af
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102556722"
---
# <a name="how-to-map-azure-disks-to-linux-vm-guest-disks"></a>Zuordnen von Azure-Datenträgern zu Linux-VM-Gastdatenträgern

Möglicherweise müssen Sie die Azure-Datenträger ermitteln, die den Gastdatenträger einer VM zugrunde liegen. In einigen Szenarien können Sie die Datenträger- oder Volumegröße mit der Größe der angefügten Azure-Datenträger vergleichen. In Szenarien, in denen an die VM mehrere Azure-Datenträger mit derselben Größe angefügt sind, müssen Sie die logische Gerätenummer (Logical Unit Number, LUN) der Datenträger verwenden. 

## <a name="what-is-a-lun"></a>Was ist eine logische Gerätenummer?

Eine logische Gerätenummer (Logical Unit Number, LUN) ist eine Zahl, mit der ein bestimmtes Speichergerät identifiziert werden kann. Jedem Speichergerät wird ein eindeutiger numerischer Bezeichner zugewiesen – beginnend bei null. Der vollständige Pfad zu einem Gerät wird durch die Busnummer, die Ziel-ID und die logische Gerätenummer (LUN) dargestellt. 

Beispiel: ***Busnummer 0, Ziel-ID 0, LUN 3***

Für diese Übung benötigen Sie nur die LUN.

## <a name="finding-the-lun"></a>Ermitteln der logischen Gerätenummer

Im Folgenden finden Sie zwei Methoden zum Ermitteln der logischen Gerätenummer eines Datenträgers unter Linux.

### <a name="lsscsi"></a>lsscsi

1. Herstellen der Verbindung zur VM
1. `sudo lsscsi`

Die erste aufgelistete Spalte enthält die LUN. Das Format lautet: [Host:Kanal:Ziel:**LUN**].

### <a name="listing-block-devices"></a>Auflisten der Blockgeräte

1. Herstellen der Verbindung zur VM
1. `sudo ls -l /sys/block/*/device`

Die letzte aufgelistete Spalte enthält die LUN. Das Format ist: [Host:Kanal:Ziel:**LUN**].

## <a name="finding-the-lun-for-the-azure-disks"></a>Ermitteln der logischen Gerätenummer von Azure-Datenträgern

Sie können die logische Gerätenummer eines Azure-Datenträgers mithilfe des Azure-Portals oder der Azure-Befehlszeilenschnittstelle ermitteln.

### <a name="finding-an-azure-disks-lun-in-the-azure-portal"></a>Ermitteln der logischen Gerätenummer eines Azure-Datenträgers im Azure-Portal

1. Wählen Sie im Azure-Portal „Virtuelle Computer“ aus, um eine Liste Ihrer VMs anzuzeigen.
1. Wählen Sie den virtuellen Computer aus.
1. Wählen Sie „Datenträger“ aus.
1. Wählen Sie in der Liste der angefügten Datenträger einen aus.
1. Die logische Gerätenummer des Datenträgers wird im Detailbereich des Datenträgers angezeigt. Die hier angezeigte logische Gerätenummer entspricht den logischen Gerätenummern, die Sie im Gast mithilfe von lsscsi oder durch Auflisten der Blockgeräte ermittelt haben.

### <a name="finding-an-azure-disks-lun-using-azure-cli"></a>Ermitteln der logischen Gerätenummer eines Azure-Datenträgers mithilfe der Azure-Befehlszeilenschnittstelle

```azurecli-interactive
az vm show -g myResourceGroup -n myVM --query "storageProfile.dataDisks"
```
