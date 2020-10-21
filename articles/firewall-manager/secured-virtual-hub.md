---
title: Was ist ein geschützter virtueller Hub?
description: Informationen zu geschützten virtuellen Hubs
author: vhorne
ms.service: firewall-manager
services: firewall-manager
ms.topic: conceptual
ms.date: 10/12/2020
ms.author: victorh
ms.openlocfilehash: c840bf9e82b8dcdb1fbf9b380ea847b3d1b08dd9
ms.sourcegitcommit: a2d8acc1b0bf4fba90bfed9241b299dc35753ee6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/12/2020
ms.locfileid: "91948067"
---
# <a name="what-is-a-secured-virtual-hub"></a>Was ist ein geschützter virtueller Hub?

Ein virtueller Hub ist ein von Microsoft verwaltetes virtuelles Netzwerk, das Verbindungen von anderen Ressourcen unterstützt. Wenn ein virtueller Hub von einem Virtual WAN im Azure-Portal erstellt wird, werden ein VNET für den virtuellen Hub und (optional) Gateways als dessen Komponenten erstellt.

Ein *geschützter* virtueller Hub ist ein [Azure Virtual WAN-Hub](../virtual-wan/virtual-wan-about.md#resources) mit zugehörigen Sicherheits- und Routingrichtlinien, die durch Azure Firewall Manager konfiguriert wurden. Verwenden Sie geschützte virtuelle Hubs, um auf einfache Weise Hub-and-Spoke- und transitive Architekturen mit nativen Sicherheitsdiensten für die Kontrolle und den Schutz des Datenverkehrs zu erstellen. 

Sie können einen geschützten virtuellen Hub verwenden, um den Datenverkehr zwischen virtuellen Netzwerken (V2V), virtuellen Netzwerken und Zweigstellen (B2V) und den Datenverkehr zum Internet (B2I/V2I) zu filtern. Ein geschützter virtueller Hub bietet automatisiertes Routing. Es ist nicht erforderlich, Ihre eigenen UDRs (benutzerdefinierte Routen) zu konfigurieren, um Datenverkehr durch Ihre Firewall zu leiten.

Sie können die erforderlichen Sicherheitsanbieter auswählen, um Ihren Datenverkehr im Netzwerk zu schützen und zu regulieren, einschließlich der Azure Firewall, anderer SECaaS-Drittanbieter (Security-as-a-Service) bzw. beide Optionen. Derzeit unterstützt ein gesicherter Hub keine Filterung von Zweigstelle zu Zweigstelle (B2B) und keine mehrere Hubs umfassende Filterung. Weitere Informationen finden Sie unter [Was ist Azure Firewall Manager?](overview.md#known-issues). 

## <a name="create-a-secured-virtual-hub"></a>Erstellen eines geschützten virtuellen Hubs

Mithilfe von Firewall Manager im Azure-Portal können Sie entweder einen neuen geschützten virtuellen Hub erstellen oder einen vorhandenen virtuellen Hub konvertieren, den Sie zuvor mithilfe von Azure Virtual WAN erstellt haben.

## <a name="next-steps"></a>Nächste Schritte

Informationen dazu, wie Sie einen geschützten virtuellen Hub erstellen und verwenden, um ein Hub-and-Spoke-Netzwerk zu schützen und zu kontrollieren, finden Sie im [Tutorial: Schützen Ihres Cloudnetzwerks mithilfe von Azure Firewall Manager unter Verwendung des Azure-Portals](secure-cloud-network.md).