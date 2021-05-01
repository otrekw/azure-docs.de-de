---
author: asudbring
ms.service: virtual-network
ms.topic: include
ms.date: 04/08/2021
ms.author: allensu
ms.openlocfilehash: 062cdb7c442ee5556aa454c978bd4aa47058aaf5
ms.sourcegitcommit: afb79a35e687a91270973990ff111ef90634f142
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/14/2021
ms.locfileid: "107530032"
---
> [!NOTE]
> Azure stellt eine kurzlebige IP-Adresse für Azure Virtual Machines-Instanzen bereit, denen keine öffentliche IP-Adresse zugewiesen ist oder die sich im Back-End-Pool einer Azure Load Balancer-Instanz vom Typ „Basic“ befinden. Der Mechanismus für kurzlebige IP-Adressen stellt eine ausgehende IP-Adresse bereit, die nicht konfigurierbar ist. 
>
>Die kurzlebige IP-Adresse wird deaktiviert, wenn dem virtuellen Computer eine öffentliche IP-Adresse zugewiesen wird oder wenn der virtuelle Computer im Back-End-Pool eines Load Balancer Standard-Instanz mit oder ohne Ausgangsregeln platziert wird. Wird dem Subnetz des virtuellen Computers eine Gatewayressource vom Typ [Azure Virtual Network NAT](../articles/virtual-network/nat-overview.md) zugewiesen, wird die kurzlebige IP-Adresse deaktiviert.
>
> Weitere Informationen zu ausgehenden Verbindungen in Azure finden Sie unter [Verwenden von SNAT für ausgehende Verbindungen](../articles/load-balancer/load-balancer-outbound-connections.md).