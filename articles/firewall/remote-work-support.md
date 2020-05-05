---
title: Unterstützung von Remotearbeit in Azure Firewall
description: In diesem Artikel erfahren Sie, wie Azure Firewall die Anforderungen Ihrer Remotemitarbeiter unterstützen kann.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: conceptual
ms.date: 04/29/2020
ms.author: victorh
ms.openlocfilehash: 14f52a92d57a8ef09b080f4657e93b3dcbf718cd
ms.sourcegitcommit: 856db17a4209927812bcbf30a66b14ee7c1ac777
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "82562099"
---
# <a name="azure-firewall-remote-work-support"></a>Unterstützung von Remotearbeit in Azure Firewall

Azure Firewall ist ein verwalteter, cloudbasierter Netzwerksicherheitsdienst zum Schutz Ihrer Azure Virtual Network-Ressourcen. Es ist eine vollständig zustandsbehaftete Firewall-as-a-Service mit integrierter Hochverfügbarkeit und uneingeschränkter Cloudskalierbarkeit. 

## <a name="firewall-rules"></a>Firewallregeln

Mithilfe von Azure Firewall können Sie den eingehenden RDP-Zugriff Ihrer virtuellen Desktopinfrastruktur (Virtual Desktop Infrastructure, VDI) auf Ihr virtuelles Azure-Netzwerk mithilfe der [DNAT-Regeln](rule-processing.md) von Azure Firewall sichern. Für Windows Virtual Desktop (WVD) ist es nicht erforderlich, dass Sie den eingehenden Zugriff auf Ihr virtuelles Netzwerk öffnen. Sie müssen jedoch einen Satz ausgehender Netzwerkverbindungen für die virtuellen WVD-Computer zulassen, die in Ihrem virtuellen Netzwerk ausgeführt werden. Weitere Informationen finden Sie unter [Verwenden von Azure Firewall zum Schutz von Windows Virtual Desktop-Bereitstellungen](protect-windows-virtual-desktop.md).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu [Windows Virtual Desktop](https://docs.microsoft.com/azure/virtual-desktop/).