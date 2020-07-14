---
title: Übersicht über die Bereitstellung mit Azure Firewall Manager
description: Informieren Sie sich über die allgemeinen Schritte zur Bereitstellung von Azure Firewall Manager.
author: vhorne
ms.service: firewall-manager
services: firewall-manager
ms.topic: overview
ms.date: 06/30/2020
ms.author: victorh
ms.openlocfilehash: 90f817ac3bbd475d8a84df44bc284f09fcd19ce3
ms.sourcegitcommit: 73ac360f37053a3321e8be23236b32d4f8fb30cf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/30/2020
ms.locfileid: "85565799"
---
# <a name="azure-firewall-manager-deployment-overview"></a>Übersicht über die Bereitstellung mit Azure Firewall Manager

Es gibt mehrere Möglichkeiten, Azure Firewall Manager bereitzustellen, jedoch sollten Sie den folgenden allgemeinen Prozess nutzen.

## <a name="general-deployment-process"></a>Allgemeiner Bereitstellungsprozess

### <a name="hub-virtual-networks"></a>Virtuelle Hubnetzwerke

1.  Erstellen einer Firewallrichtlinie

    - Erstellen einer neuen Richtlinie
<br>*or*<br>
    - Leiten Sie eine Basisrichtlinie ab, und passen Sie eine lokale Richtlinie an.
<br>*or*<br>
    - Importieren Sie Regeln aus einer vorhandenen Azure Firewall-Instanz. Entfernen Sie unbedingt die NAT-Regeln aus Richtlinien, die auf mehrere Firewalls angewendet werden sollen.
1. Erstellen Ihrer Hub-and-Spoke-Architektur
   - Erstellen Sie mit Azure Firewall Manager ein virtuelles Hubnutzwerk, und verknüpfen Sie unter Verwendung des Peerings virtueller Netzwerke virtuelle Spoke-Netzwerke damit.
<br>*or*<br>
    - Erstellen Sie ein virtuelles Netzwerk, fügen Sie virtuelle Netzwerkverbindungen hinzu, und verknüpfen Sie unter Verwendung des Peerings virtueller Netzwerke virtuelle Spoke-Netzwerke damit.

3. Auswählen von Sicherheitsanbietern und Zuweisen der Firewallrichtlinie. Derzeit wird nur Azure Firewall als Anbieter unterstützt.

   - Dieser Schritt wird während der Erstellung eines virtuellen Hubnetzwerks ausgeführt.
<br>*or*<br>
    - Konvertieren Sie ein vorhandenes virtuelles Netzwerk in ein virtuelles Hubnetzwerk. Es können auch mehrere virtuelle Netzwerke konvertiert werden.

4. Konfigurieren benutzerdefinierter Routen zum Weiterleiten von Datenverkehr an die Firewall Ihres virtuellen Hubnetzwerks


### <a name="secured-virtual-hubs"></a>Geschützte virtuelle Hubs

1. Erstellen Ihrer Hub-and-Spoke-Architektur

   - Erstellen Sie mit Azure Firewall Manager einen geschützten virtuellen Hub, und fügen Sie virtuelle Netzwerkverbindungen hinzu.<br>*or*<br>
   - Erstellen Sie einen virtuellen WAN-Hub, und fügen Sie virtuelle Netzwerkverbindungen hinzu.
2. Auswählen von Sicherheitsanbietern

   - Erfolgt beim Erstellen eines geschützten virtuellen Hubs.<br>*or*<br>
   - Konvertieren Sie einen vorhandenen virtuellen WAN-Hub in einen geschützten virtuellen Hub.
3. Erstellen einer Firewallrichtlinie und deren Zuordnung zu Ihrem Hub

   - Gilt nur, wenn Azure Firewall verwendet wird.
   - SECaaS-Richtlinien (Security-as-a-Service, Sicherheit als Dienst) von Drittanbietern werden über die Verwaltungsbenutzeroberfläche von Partnern konfiguriert.
4. Konfigurieren von Routeneinstellungen zum Weiterleiten von Datenverkehr an Ihren geschützten Hub

   - Leiten Sie mithilfe der Seite zur Routeneinstellung für geschützte virtuelle Hubs den Datenverkehr zur Filterung und Protokollierung ohne benutzerdefinierte Routen (UDR) auf virtuellen Spoke-Netzwerken einfach an Ihren geschützten Hub weiter.

> [!NOTE]
> - Sie können nicht mehr als einen Hub pro virtuellem WAN verwenden. Aber Sie können mehrere virtuelle WANs in der Region hinzufügen, um dies zu erreichen.
> - Überlappende IP-Adressräume für Hubs sind in einem VWAN nicht möglich.
> - Ihre Hub-VNET-Verbindungen müssen sich in der gleichen Region wie der Hub befinden.

## <a name="next-steps"></a>Nächste Schritte

- [Tutorial: Schützen Ihres Cloudnetzwerks mithilfe von Azure Firewall Manager unter Verwendung des Azure-Portals](secure-cloud-network.md)