---
title: 'Szenario: Weiterleiten von Datenverkehr über virtuelle Netzwerkgeräte (NVAs) mithilfe von benutzerdefinierten Einstellungen'
titleSuffix: Azure Virtual WAN
description: Dieses Szenario hilft Ihnen beim Weiterleiten von Datenverkehr über NVAs mithilfe eines anderen NVA für Internetdatenverkehr.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 09/22/2020
ms.author: cherylmc
ms.custom: fasttrack-edit
ms.openlocfilehash: e1cf9faeab60264d491539256828151e496ade8f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91267498"
---
# <a name="scenario-route-traffic-through-nvas---custom-preview"></a>Szenario: Weiterleiten von Datenverkehr über virtuelle Netzwerkgeräte: benutzerdefiniert (Vorschau)

Wenn Sie mit Virtual WAN-Routing für virtuelle Hubs arbeiten, steht Ihnen eine ganze Reihe von Szenarios zur Verfügung. In diesem NVA-Szenario (Network Virtual Appliance, virtuelles Netzwerkgerät) besteht das Ziel darin, Datenverkehr über ein NVA für die Kommunikation zwischen VNETs und Branches weiterzuleiten und ein anderes NVA für Internetdatenverkehr zu verwenden. Weitere Informationen zum Routing für virtuelle Hubs finden Sie unter [Informationen zum Routing virtueller Hubs](about-virtual-hub-routing.md).

## <a name="design"></a><a name="design"></a>Entwurf

In diesem Szenario wird die folgende Benennungskonvention verwendet:

* „Dienst-VNET“ für virtuelle Netzwerke, in denen Benutzer ein NVA bereitgestellt haben (VNET 4 in **Abbildung 1**), um nicht über das Internet verlaufenden Datenverkehr zu untersuchen.
* „DMZ-VNET“ für virtuelle Netzwerke, in denen Benutzer ein NVA für die Untersuchung von Internetdatenverkehr bereitgestellt haben (VNET 5 in **Abbildung 1**).
* „NVA-Spokes“ für virtuelle Netzwerke, die mit einem NVA-VNET (VNET 1, VNET 2 und VNET 3 in **Abbildung 1**) verbunden sind.
* „Hubs“ für von Microsoft verwaltete Virtual WAN-Hubs.

In der folgenden Konnektivitätsmatrix sind die für dieses Szenario unterstützten Datenflüsse zusammengefasst:

**Konnektivitätsmatrix**

| From          | Nach:|*NVA-Spokes*|*Dienst-VNET*|*DMZ-VNET*|*Branches (statisch)*|
|---|---|---|---|---|---|
| **NVA-Spokes**| &#8594;|      X |            X |   Peering |    statischen    |
| **Dienst-VNET**| &#8594;|    X |            X |      X    |      X       |
| **DMZ-VNET** | &#8594;|       X |            X |      X    |      X       |
| **Branches** | &#8594;|  statischen |            X |      X    |      X       |

In den einzelnen Zellen der Konnektivitätsmatrix ist beschrieben, ob eine Virtual WAN-Verbindung (Von-Seite des Datenflusses bzw. Zeilenbezeichnung) ein Zielpräfix (An-Seite des Datenflusses bzw. kursive Spaltenüberschrift) für einen bestimmten Datenverkehrsfluss erlernt. Ein „X“ bedeutet, dass die Konnektivität nativ von Virtual WAN bereitgestellt wird. „Statisch“ bedeutet, dass die Konnektivität von Virtual WAN mithilfe statischer Routen bereitgestellt wird. Hier sind die einzelnen Zeilen ausführlicher beschrieben:

* NVA-Spokes:
  * Spokes können andere Spokes direkt über Virtual WAN-Hubs erreichen.
  * Spokes erhalten die Konnektivität mit Branches über eine statische Route, die auf das Dienst-VNET verweist. Sie sollten keine spezifischen Präfixe über die Branches erlernen (andernfalls wären diese spezifischer und würden die Zusammenfassung außer Kraft setzen).
  * Spokes senden Internetdatenverkehr per direktem VNET-Peering an das DMZ-VNET.
* Branches:
  * Branches erreichen Spokes per statischem Routing, wobei auf das Dienst-VNET verwiesen wird. Hierbei sollten keine spezifischen Präfixe von den VNETs erlernt werden, die die zusammengefasste statische Route außer Kraft setzen.
* Das Dienst-VNET ähnelt einem VNET für gemeinsame Dienste, das für jedes VNET und jeden Branch erreichbar sein muss.
* Das DMZ-VNET benötigt nicht unbedingt Konnektivität per Virtual WAN, da der einzige Datenverkehr, der von diesem VNET unterstützt wird, über direkte VNET-Peerings eingeht. Wir verwenden aber das gleiche Konnektivitätsmodell wie für das DMZ-VNET, um die Konfiguration zu vereinfachen.

Mit unserer Konnektivitätsmatrix erhalten wir also drei separate Konnektivitätsmuster, und dies ergibt drei Routingtabellen. Die Zuordnungen zu den verschiedenen VNETs lauten wie folgt:

* NVA-Spokes:
  * Zugeordnete Routingtabelle: **RT_V2B**
  * Weitergabe an Routingtabellen: **RT_V2B** und **RT_SHARED**
* NVA-VNETs (intern und Internet):
  * Zugeordnete Routingtabelle: **RT_SHARED**
  * Weitergabe an Routingtabellen: **RT_SHARED**
* Branches:
  * Zugeordnete Routingtabelle: **Standard**
  * Weitergabe an Routingtabellen: **RT_SHARED** und **Default**

Wir benötigen diese statischen Routen, um sicherzustellen, dass VNET-zu-Branch- und Branch-zu-VNET-Datenverkehr über das NVA im Dienst-VNET verläuft (VNET 4):

| BESCHREIBUNG | Routingtabelle | Statische Route              |
| ----------- | ----------- | ------------------------- |
| Branches    | RT_V2B      | 10.2.0.0/16 -> vnet4conn  |
| NVA-Spokes  | Standard     | 10.1.0.0/16 -> vnet4conn  |

Das Virtual WAN verfügt jetzt über die Information, an welche Verbindung die Pakete gesendet werden sollen. Die Verbindung muss aber auch wissen, was zu tun ist, wenn diese Pakete empfangen werden: Hierfür werden die Routingtabellen für Verbindungen verwendet.

| BESCHREIBUNG | Verbindung | Statische Route            |
| ----------- | ---------- | ----------------------- |
| VNet2Branch | vnet4conn  | 10.2.0.0/16 -> 10.4.0.5 |
| Branch2VNet | vnet4conn  | 10.1.0.0/16 -> 10.4.0.5 |

Es sollte nun alles wie gewünscht eingerichtet sein.

Weitere Informationen zum Routing für virtuelle Hubs finden Sie unter [Informationen zum Routing virtueller Hubs](about-virtual-hub-routing.md).

## <a name="architecture"></a><a name="architecture"></a>Architektur

In **Abbildung 1** gibt es einen Hub, **Hub 1**.

* **Hub 1** ist direkt mit den NVA-VNETs **VNET 4** und **VNET 5** verbunden.

* Der Datenverkehr zwischen den VNETs 1, 2 und 3 und den Branches (VPN/ER/P2S) wird über **NVA-VNET 4** (10.4.0.5) erwartet.

* Der gesamte Internetdatenverkehr aus den VNETs 1, 2 und 3 wird über das **NVA-VNET 5** (10.5.0.5) erwartet.

**Abbildung 1**

:::image type="content" source="./media/routing-scenarios/nva-custom/figure-1.png" alt-text="Abbildung 1":::

## <a name="workflow"></a><a name="workflow"></a>Workflow

Zur Einrichtung des Routings über das virtuelle Netzwerkgerät sind die folgenden Schritte zu beachten:

1. Damit der Internetdatenverkehr über VNET 5 geleitet wird, müssen die VNETs 1, 2 und 3 per VNET-Peering eine direkte Verbindung mit VNET 5 herstellen. Sie benötigen zudem ein UDR-Setup in den VNets für 0.0.0.0/0 und dem nächsten Hop 10.5.0.5. Zurzeit lässt Virtual WAN im virtuellen Hub für 0.0.0.0/0 kein NVA für den nächsten Hop zu.

1. Navigieren Sie im Azure-Portal zu Ihrem virtuellen Hub, und erstellen Sie eine benutzerdefinierte Routingtabelle **RT_Shared**, die Routen über die Weitergabe von allen VNets und Filialverbindungen erlernt. In **Abbildung 2** ist dies als leere benutzerdefinierte Routingtabelle **RT_Shared** dargestellt.

   * **Routen:** Sie müssen keine statischen Routen hinzufügen.

   * **Zuordnung:** Wählen Sie die VNETs 4 und 5 aus. Dies bedeutet, dass die Verbindungen dieser beiden VNETs der Routingtabelle **RT_Shared** zugeordnet werden.

   * **Weitergabe:** Da Sie möchten, dass alle Filialen und VNet-Verbindungen ihre Routen dynamisch an diese Routingtabelle weitergeben, wählen Sie Filialen und alle VNets aus.

1. Erstellen Sie eine benutzerdefinierte Routingtabelle **RT_V2B**, um den Datenverkehr von den VNETs 1, 2 und 3 an Branches weiterzuleiten.

   * **Routen:** Fügen Sie einen aggregierten statischen Routeneintrag für Branches (VPN/ER/P2S, 10.2.0.0/16 in **Abbildung 2**) mit dem nächsten Hop als VNET 4-Verbindung hinzu. Außerdem müssen Sie eine statische Route in der VNET 4-Verbindung für Branchpräfixe konfigurieren und angeben, dass der nächste Hop die spezifische IP-Adresse des virtuellen Netzwerkgeräts in VNET 4 sein soll.

   * **Zuordnung:** Wählen Sie alle VNETs aus (1, 2 und 3). Dies bedeutet, dass die VNet-Verbindungen 1, 2 und 3 dieser Routingtabelle zugeordnet werden und in dieser Routingtabelle Routen (statisch und dynamisch über die Weitergabe) erlernen können.

   * **Weitergabe:** Verbindungen geben Routen an Routingtabellen weiter. Durch die Auswahl der VNETs 1, 2 und 3 wird die Weitergabe von Routen von den VNETs 1, 2 und 3 an diese Routingtabelle ermöglicht. Es ist nicht erforderlich, Routen von Branchverbindungen an „RT_V2B“ weiterzugeben, da der Datenverkehr des Branch-VNET über das NVA in VNET 4 geleitet wird.
  
1. Bearbeiten Sie die Standardroutingtabelle **DefaultRouteTable**.

   Alle VPN-, ExpressRoute- und Benutzer-VPN-Verbindungen sind der Standardroutingtabelle zugeordnet. Alle VPN-, ExpressRoute- und Benutzer-VPN-Verbindungen geben Routen an dieselben Routingtabellen weiter.

   * **Routen:** Fügen Sie einen aggregierten statischen Routeneintrag für die VNETs 1, 2 und 3 (10.1.0.0/16 in **Abbildung 2**) mit dem nächsten Hop als VNET 4-Verbindung hinzu. Außerdem müssen Sie eine statische Route sowie drei aggregierte Präfixe in der VNET 4-Verbindung für die VNETs 1, 2 und 3 konfigurieren und angeben, dass der nächste Hop die spezifische IP-Adresse des virtuellen Netzwerkgeräts in VNET 4 sein soll.

   * **Zuordnung:** Vergewissern Sie sich, dass die Option für Filialen (VPN/ER/P2S) ausgewählt ist, und stellen Sie sicher, dass lokale Filialverbindungen der *defaultroutetable* zugeordnet sind.

   * **Weitergabe von:** Vergewissern Sie sich, dass die Option für Filialen (VPN/ER/P2S) ausgewählt ist, und stellen Sie sicher, dass lokale Verbindungen Routen an die *defaultroutetable* weitergeben.

**Abbildung 2**

:::image type="content" source="./media/routing-scenarios/nva-custom/figure-2.png" alt-text="Abbildung 1" lightbox="./media/routing-scenarios/nva-custom/figure-2.png":::

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu Virtual WAN finden Sie unter [Häufig gestellte Fragen](virtual-wan-faq.md).
* Weitere Informationen zum Routing für virtuelle Hubs finden Sie unter [Informationen zum Routing virtueller Hubs](about-virtual-hub-routing.md).
