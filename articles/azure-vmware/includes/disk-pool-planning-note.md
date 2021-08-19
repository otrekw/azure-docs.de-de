---
title: Hinweis zur Datenträgerpoolplanung für VNet
description: Wichtiger Hinweis dazu, wie wichtig es ist, ein VNet in geringerer Entfernung zu Azure VMware Solution-Hosts bereitzustellen.
ms.topic: include
ms.date: 07/14/2021
ms.openlocfilehash: 39bcf823a71852396c1c05db4902abf000433499
ms.sourcegitcommit: 75ad40bab1b3f90bb2ea2a489f8875d4b2da57e4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2021
ms.locfileid: "113640568"
---
Wenn Sie planen, Ihre Azure VMware Solution-Hosts mithilfe von [Azure-Datenträgerpools](../../virtual-machines/disks-pools.md) zu skalieren, ist es äußerst wichtig, das VNet in der Nähe Ihrer Hosts mit einem virtuellen ExpressRoute-Netzwerkgateway bereitzustellen.  Je näher der Speicher sich an Ihren Hosts befindet, desto besser ist die Leistung.