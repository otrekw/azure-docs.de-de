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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "79226962"
---
# <a name="co-location"></a>Zusammenstellung

Einer der wichtigsten Gründe für die Latenz zwischen VMs ist einfach die Entfernung.

## <a name="preview-proximity-placement-groups"></a>Vorschau: Näherungsplatzierungsgruppen 

[!INCLUDE [virtual-machines-common-ppg-overview](../../includes/virtual-machines-common-ppg-overview.md)]

## <a name="next-steps"></a>Nächste Schritte

Erstellen Sie eine [Näherungsplatzierungsgruppe](proximity-placement-groups.md) für Ihre Skalierungsgruppe.

