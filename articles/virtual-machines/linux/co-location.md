---
title: Zusammenstellen von Linux-VMs
description: Erfahren Sie, wie das Zusammenstellen von Azure-VM-Ressourcen die Latenz verbessern kann.
ms.service: virtual-machines
ms.topic: article
ms.date: 10/30/2019
ms.author: zivr
ms.openlocfilehash: 0a28b238701eeb0572149fb55e1633cdd40ec0a4
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86511023"
---
# <a name="co-locate-resources-for-improved-latency"></a>Zusammenstellen von Ressourcen für geringere Latenz

Wenn Sie Ihre Anwendung in Azure bereitstellen und die Instanzen auf Regionen oder Verfügbarkeitszonen verteilen, führt dies zu einer Netzwerklatenz, die sich möglicherweise auf die Gesamtleistung Ihrer Anwendung auswirkt. 

## <a name="proximity-placement-groups"></a>Näherungsplatzierungsgruppen

[!INCLUDE [virtual-machines-common-ppg-overview](../../../includes/virtual-machines-common-ppg-overview.md)]

## <a name="next-steps"></a>Nächste Schritte

Stellen Sie mithilfe der Azure CLI eine VM in einer [Näherungsplatzierungsgruppe](proximity-placement-groups.md) bereit.

Erfahren Sie, wie Sie [die Netzwerklatenz testen](https://aka.ms/TestNetworkLatency?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Erfahren Sie, wie Sie [den Netzwerkdurchsatz optimieren](../../virtual-network/virtual-network-optimize-network-bandwidth.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).  

Erfahren Sie, wie Sie [Näherungsplatzierungsgruppen mit SAP-Anwendungen verwenden](../workloads/sap/sap-proximity-placement-scenarios.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
