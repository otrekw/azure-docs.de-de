---
title: Zusammenstellen von Azure-VM-Skalierungsgruppen
description: Erfahren Sie, wie Sie die Leistung von Ressourcen in Azure-VM-Skalierungsgruppen durch das Zusammenstellen verbessern können.
author: cynthn
ms.service: virtual-machine-scale-sets
ms.topic: conceptual
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 05/14/2019
ms.author: cynthn
ms.openlocfilehash: 9d6e4e15c552cc4467dd17221a1ba901fd8e4f58
ms.sourcegitcommit: 5397b08426da7f05d8aa2e5f465b71b97a75550b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2020
ms.locfileid: "76275202"
---
# <a name="co-location"></a>Zusammenstellung

Einer der wichtigsten Gründe für die Latenz zwischen VMs ist einfach die Entfernung.

## <a name="preview-proximity-placement-groups"></a>Vorschau: Näherungsplatzierungsgruppen 

[!INCLUDE [virtual-machines-common-ppg-overview](../../includes/virtual-machines-common-ppg-overview.md)]

## <a name="next-steps"></a>Nächste Schritte

Erstellen Sie eine [Näherungsplatzierungsgruppe](proximity-placement-groups.md) für Ihre Skalierungsgruppe.

