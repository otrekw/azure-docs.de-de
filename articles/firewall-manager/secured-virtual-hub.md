---
title: Was ist ein geschützter virtueller Hub?
description: Informationen zu geschützten virtuellen Hubs
author: vhorne
ms.service: firewall-manager
services: firewall-manager
ms.topic: conceptual
ms.date: 06/30/2020
ms.author: victorh
ms.openlocfilehash: 56e0d40bcbfb97f57b63dc82da1a6604f83dffad
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85563632"
---
# <a name="what-is-a-secured-virtual-hub"></a>Was ist ein geschützter virtueller Hub?

Ein virtueller Hub ist ein von Microsoft verwaltetes virtuelles Netzwerk, das Verbindungen von anderen Ressourcen unterstützt. Wenn ein virtueller Hub von einem Virtual WAN im Azure-Portal erstellt wird, werden ein VNET für den virtuellen Hub und (optional) Gateways als dessen Komponenten erstellt.

Ein *geschützter* virtueller Hub ist ein [Azure Virtual WAN-Hub](../virtual-wan/virtual-wan-about.md#resources) mit zugehörigen Sicherheits- und Routingrichtlinien, die durch Azure Firewall Manager konfiguriert wurden. Verwenden Sie geschützte virtuelle Hubs, um auf einfache Weise Hub-and-Spoke- und transitive Architekturen mit nativen Sicherheitsdiensten für die Kontrolle und den Schutz des Datenverkehrs zu erstellen. 

Sie können einen geschützten virtuellen Hub als verwaltetes zentrales VNET ohne lokale Konnektivität verwenden. Dieses ersetzt das zentrale VNET, das zuvor für eine Azure Firewall-Bereitstellung erforderlich war. Da der geschützte virtuelle Hub das automatisierte Routing ermöglicht, müssen Sie keine eigenen benutzerdefinierten Routen (UDRs) konfigurieren, um Datenverkehr durch die Firewall zu leiten.

Es ist auch möglich, geschützte virtuelle Hubs als Teil einer vollständigen Virtual WAN-Architektur zu verwenden. Diese Architektur unterstützt geschützte, optimierte und automatisierte Branchverbindungen mit und über Azure. Sie können die Dienste zum Schutz und zur Kontrolle des Netzwerkdatenverkehrs auswählen, einschließlich der Azure Firewall und anderer SECaaS (Security-as-a-Service)-Drittanbieter.

## <a name="create-a-secured-virtual-hub"></a>Erstellen eines geschützten virtuellen Hubs

Mithilfe von Firewall Manager im Azure-Portal können Sie entweder einen neuen geschützten virtuellen Hub erstellen oder einen vorhandenen virtuellen Hub konvertieren, den Sie zuvor mithilfe von Azure Virtual WAN erstellt haben.

## <a name="next-steps"></a>Nächste Schritte

Informationen dazu, wie Sie einen geschützten virtuellen Hub erstellen und verwenden, um ein Hub-and-Spoke-Netzwerk zu schützen und zu kontrollieren, finden Sie im [Tutorial: Schützen Ihres Cloudnetzwerks mithilfe von Azure Firewall Manager unter Verwendung des Azure-Portals](secure-cloud-network.md).