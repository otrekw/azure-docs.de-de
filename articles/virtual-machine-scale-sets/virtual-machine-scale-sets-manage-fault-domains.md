---
title: Verwalten von Fehlerdomänen in Azure-VM-Skalierungsgruppen
description: Erfahren Sie, wie Sie beim Erstellen einer VM-Skalierungsgruppe die richtige Anzahl von Fehlerdomänen (FDs) auswählen.
author: mimckitt
ms.author: mimckitt
ms.topic: conceptual
ms.service: virtual-machine-scale-sets
ms.subservice: availability
ms.date: 12/18/2018
ms.reviewer: jushiman
ms.custom: mimckitt, devx-track-azurecli
ms.openlocfilehash: 8c114d6260cf81bcc4fb256fc8a09947ab9ce1d8
ms.sourcegitcommit: 15d27661c1c03bf84d3974a675c7bd11a0e086e6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2021
ms.locfileid: "102502483"
---
# <a name="choosing-the-right-number-of-fault-domains-for-virtual-machine-scale-set"></a>Auswählen der richtigen Anzahl von Fehlerdomänen für VM-Skalierungsgruppen
VM-Skalierungsgruppen werden in Azure-Regionen ohne Zonen standardmäßig mit fünf Fehlerdomänen erstellt. Wenn diese Option für Regionen ausgewählt ist, die die Zonenbereitstellung von VM-Skalierungsgruppen unterstützen, lautet der Standardwert der Anzahl von Fehlerdomänen für jede Zone 1. FD = 1 bedeutet in diesem Fall, dass die zur Skalierungsgruppe gehörenden VM-Instanzen auf bestmögliche Weise über viele Racks verteilt werden.

Sie können auch die Anzahl der Fehlerdomänen für die Skalierungsgruppe an der Anzahl von Fehlerdomänen für verwaltete Datenträger ausrichten. Diese Ausrichtung kann dabei helfen, Quorumverluste zu verhindern, wenn eine gesamte Fehlerdomäne für verwaltete Datenträger ausfällt. Die FD-Anzahl kann auf die Anzahl der in allen Regionen verfügbaren Fehlerdomänen für verwaltete Datenträger oder eine kleinere Zahl festgelegt werden. In [diesem Dokument](../virtual-machines/availability.md) erfahren Sie mehr über die Anzahl von Fehlerdomänen für verwaltete Datenträger nach Region.

## <a name="rest-api"></a>REST-API
Sie können die Eigenschaft `properties.platformFaultDomainCount` auf 1, 2 oder 3 festlegen (der Standardwert bei fehlender Angabe ist 3). Weitere Informationen finden Sie in der REST-API-Dokumentation [hier](/rest/api/compute/virtualmachinescalesets/createorupdate).

## <a name="azure-cli"></a>Azure CLI
Sie können den Parameter `--platform-fault-domain-count` auf 1, 2 oder 3 festlegen (der Standardwert bei fehlender Angabe ist 3). Weitere Informationen finden Sie in der [Dokumentation der Azure-Befehlszeilenschnittstelle](/cli/azure/vmss#az-vmss-create).

```azurecli-interactive
az vmss create \
  --resource-group myResourceGroup \
  --name myScaleSet \
  --image UbuntuLTS \
  --upgrade-policy-mode automatic \
  --admin-username azureuser \
  --platform-fault-domain-count 3\
  --generate-ssh-keys
```

Die Erstellung und Konfiguration aller Ressourcen und virtuellen Computer der Skalierungsgruppe dauert einige Minuten.

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie mehr über [Verfügbarkeits-und Redundanzfeatures](../virtual-machines/availability.md) für Azure-Umgebungen.
