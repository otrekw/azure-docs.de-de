---
title: Zusammenstellen von Linux-VMs
description: Erfahren Sie, wie das Zusammenstellen von Azure-VM-Ressourcen die Latenz verbessern kann.
ms.service: virtual-machines
ms.topic: conceptual
ms.date: 10/30/2019
ms.author: zivr
ms.openlocfilehash: 000c8c8955314060ae9bdedb96b086a509fb3e96
ms.sourcegitcommit: f353fe5acd9698aa31631f38dd32790d889b4dbb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2020
ms.locfileid: "87368641"
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
