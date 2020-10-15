---
title: Zusammenstellen von VMs für verbesserte Latenz
description: Erfahren Sie, wie das Zusammenstellen von Azure-VM-Ressourcen die Latenz verbessern kann.
author: cynthn
ms.service: virtual-machines
ms.topic: conceptual
ms.workload: infrastructure-services
ms.date: 10/30/2019
ms.author: zivr
ms.openlocfilehash: 9c981e9b829fa10f094370e1e1ffd8b6df967d4b
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2020
ms.locfileid: "91965776"
---
# <a name="co-locate-resource-for-improved-latency"></a>Zusammenstellen von Ressourcen für verbesserte Latenzen

Wenn Sie Ihre Anwendung in Azure bereitstellen und die Instanzen auf Regionen oder Verfügbarkeitszonen verteilen, führt dies zu einer Netzwerklatenz, die sich möglicherweise auf die Gesamtleistung Ihrer Anwendung auswirkt. 


## <a name="proximity-placement-groups"></a>Näherungsplatzierungsgruppen 

[!INCLUDE [virtual-machines-common-ppg-overview](../../../includes/virtual-machines-common-ppg-overview.md)]

## <a name="next-steps"></a>Nächste Schritte

Stellen Sie mithilfe von Azure PowerShell eine VM in einer [Näherungsplatzierungsgruppe](proximity-placement-groups.md) bereit.

Erfahren Sie, wie Sie [die Netzwerklatenz testen](../../virtual-network/virtual-network-test-latency.md?toc=%252fazure%252fvirtual-machines%252fwindows%252ftoc.json).

Erfahren Sie, wie Sie [den Netzwerkdurchsatz optimieren](../../virtual-network/virtual-network-optimize-network-bandwidth.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).  

Erfahren Sie, wie Sie [Näherungsplatzierungsgruppen mit SAP-Anwendungen verwenden](../workloads/sap/sap-proximity-placement-scenarios.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).