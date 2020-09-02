---
title: Übersicht über die Bereitstellung mit Azure Firewall Manager
description: Informieren Sie sich über die allgemeinen Schritte zur Bereitstellung von Azure Firewall Manager.
author: vhorne
ms.service: firewall-manager
services: firewall-manager
ms.topic: conceptual
ms.date: 08/28/2020
ms.author: victorh
ms.openlocfilehash: ceb6e84b31067f7289b9e003a4fb273ce717de33
ms.sourcegitcommit: 656c0c38cf550327a9ee10cc936029378bc7b5a2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2020
ms.locfileid: "89079097"
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
>
> Weitere bekannte Probleme finden Sie unter [Was ist Azure Firewall Manager?](overview.md#known-issues)

## <a name="convert-virtual-networks"></a>Konvertieren von virtuellen Netzwerken

Die folgenden Informationen gelten, wenn Sie ein vorhandenes virtuelles Netzwerk in ein virtuelles Hubnetzwerk konvertieren:

- Wenn das virtuelle Netzwerk über eine bereits vorhandene Azure Firewall verfügt, können Sie eine Firewallrichtlinie auswählen, die der vorhandenen Firewall zugeordnet werden soll. Der Bereitstellungsstatus der Firewall wird aktualisiert, während die Firewallregeln von der Firewallrichtlinie ersetzt werden. Während des Bereitstellungsstatus fährt die Firewall mit der Verarbeitung des Datenverkehrs fort, und es kommt nicht zu Ausfallzeiten. Sie können vorhandene Regeln in eine Firewallrichtlinie importieren, indem Sie Firewall Manager oder Azure PowerShell verwenden.
- Wenn das virtuelle Netzwerk nicht über eine zugeordnete Azure Firewall verfügt, wird eine Firewall bereitgestellt, und die Firewallrichtlinie wird der neuen Firewall zugeordnet.

## <a name="next-steps"></a>Nächste Schritte

- [Tutorial: Schützen Ihres Cloudnetzwerks mithilfe von Azure Firewall Manager unter Verwendung des Azure-Portals](secure-cloud-network.md)