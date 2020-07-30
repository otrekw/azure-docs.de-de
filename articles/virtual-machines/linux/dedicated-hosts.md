---
title: Übersicht über dedizierte Azure-Hosts für virtuelle Computer
description: Erfahren Sie mehr darüber, wie dedizierte Azure-Hosts für die Bereitstellung virtueller Computer verwendet werden können.
author: cynthn
ms.service: virtual-machines
ms.topic: article
ms.date: 07/28/2020
ms.author: cynthn
ms.openlocfilehash: 97e65528a7f049c213ec077c83da1b7672469d90
ms.sourcegitcommit: 5b8fb60a5ded05c5b7281094d18cf8ae15cb1d55
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2020
ms.locfileid: "87388168"
---
# <a name="azure-dedicated-hosts-for-virtual-machines"></a>Azure Dedicated Hosts für virtuelle Computer

Azure Dedicated Host ist ein Dienst zum Bereitstellen physischer Server, die virtuelle Computer hosten können und ausschließlich für ein Azure-Abonnement reserviert sind. Dedizierte Hosts sind die in unseren Rechenzentren verwendeten physischen Server, die als Ressource bereitgestellt werden. Sie können dedizierte Hosts in einer Region, einer Verfügbarkeitszone und einer Fehlerdomäne bereitstellen. Anschließend können Sie VMs direkt in Ihren bereitgestellten Hosts platzieren, und zwar in einer beliebigen Konfiguration, die Ihren Anforderungen am besten entspricht.



[!INCLUDE [virtual-machines-common-dedicated-hosts](../../../includes/virtual-machines-common-dedicated-hosts.md)]


## <a name="next-steps"></a>Nächste Schritte

- Sie können einen dedizierten Host über die [Azure-Befehlszeilenschnittstelle](dedicated-hosts-cli.md), das [Portal](dedicated-hosts-portal.md) und [PowerShell](../windows/dedicated-hosts-powershell.md) bereitstellen.

- Weitere Informationen finden Sie in der Übersicht zu [dedizierten Hosts](dedicated-hosts.md).

- [Hier](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vm-dedicated-hosts/README.md) finden Sie eine Beispielvorlage, die sowohl Zonen als auch Fehlerdomänen für maximale Resilienz in einer Region verwendet.

- Sie können auch mit einer [reservierten Instanz von Azure Dedicated Hosts](../prepay-dedicated-hosts-reserved-instances.md) Kosten einsparen.
