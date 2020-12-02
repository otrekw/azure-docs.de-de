---
title: Zusammenstellen von Linux-VMs
description: Erfahren Sie, wie das Zusammenstellen von Azure-VM-Ressourcen für Linux die Latenz verbessern kann.
ms.service: virtual-machines
ms.topic: conceptual
ms.date: 10/30/2019
ms.author: zivr
ms.openlocfilehash: 304623ca50fd030ab6e016b940f8be52819c161a
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/02/2020
ms.locfileid: "96500630"
---
# <a name="co-locate-resources-for-improved-latency"></a>Zusammenstellen von Ressourcen für geringere Latenz

Wenn Sie Ihre Anwendung in Azure bereitstellen und die Instanzen auf Regionen oder Verfügbarkeitszonen verteilen, führt dies zu einer Netzwerklatenz, die sich möglicherweise auf die Gesamtleistung Ihrer Anwendung auswirkt. 

## <a name="proximity-placement-groups"></a>Näherungsplatzierungsgruppen

[!INCLUDE [virtual-machines-common-ppg-overview](../../../includes/virtual-machines-common-ppg-overview.md)]

## <a name="next-steps"></a>Nächste Schritte

Stellen Sie mithilfe der Azure CLI eine VM in einer [Näherungsplatzierungsgruppe](proximity-placement-groups.md) bereit.

Erfahren Sie, wie Sie [die Netzwerklatenz testen](../../virtual-network/virtual-network-test-latency.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Erfahren Sie, wie Sie [den Netzwerkdurchsatz optimieren](../../virtual-network/virtual-network-optimize-network-bandwidth.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).  

Erfahren Sie, wie Sie [Näherungsplatzierungsgruppen mit SAP-Anwendungen verwenden](../workloads/sap/sap-proximity-placement-scenarios.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).