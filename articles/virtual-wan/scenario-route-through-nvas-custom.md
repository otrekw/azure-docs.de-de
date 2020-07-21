---
title: 'Szenario: Weiterleiten von Datenverkehr über virtuelle Netzwerkgeräte (NVAs) mithilfe von benutzerdefinierten Einstellungen'
titleSuffix: Azure Virtual WAN
description: Dieses Szenario hilft Ihnen beim Weiterleiten von Datenverkehr über NVAs mithilfe eines anderen NVA für Internetdatenverkehr.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 06/29/2020
ms.author: cherylmc
ms.openlocfilehash: 3fcad59bcfb75b8de6af14eae7e0bcb4962b17eb
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85567910"
---
# <a name="scenario-route-traffic-through-nvas---custom-preview"></a>Szenario: Weiterleiten von Datenverkehr über virtuelle Netzwerkgeräte: benutzerdefiniert (Vorschau)

Wenn Sie mit Virtual WAN-Routing für virtuelle Hubs arbeiten, steht Ihnen eine ganze Reihe von Szenarios zur Verfügung. In diesem NVA-Szenario (virtuelles Netzwerkgerät) besteht das Ziel darin, den Datenverkehr über ein NVA für VNet <-> Filiale weiterzuleiten und ein anderes NVA für Internetdatenverkehr zu verwenden. Weitere Informationen zum Routing für virtuelle Hubs finden Sie unter [Informationen zum Routing virtueller Hubs](about-virtual-hub-routing.md).

## <a name="scenario-architecture"></a><a name="architecture"></a>Szenarioarchitektur

In **Abbildung 1** gibt es einen Hub, **Hub 1**.

* **Hub 1** ist direkt mit den virtuellen NVA-Netzwerken **VNET4** und **VNET5** verbunden.

* Der Datenverkehr zwischen den VNets 1, 2 und 3 und den Filialen (VPN/ER/P2S) wird über das **NVA VNET4** 10.4.0.5 erwartet.

* Der gesamte Internetdatenverkehr aus den VNets 1, 2 und 3 wird über das **NVA VNET5** 10.5.0.5 erwartet.

**Abbildung 1**

:::image type="content" source="./media/routing-scenarios/nva-custom/figure-1.png" alt-text="Abbildung 1":::

## <a name="scenario-workflow"></a><a name="workflow"></a>Szenarioworkflow

Zur Einrichtung des Routings über das virtuelle Netzwerkgerät sind die folgenden Schritte zu beachten:

1. Damit der Internetdatenverkehr über das VNET5 geleitet wird, müssen die VNets 1, 2 und 3 über das VNET-Peering eine direkte Verbindung mit VNET5 herstellen. Sie benötigen zudem ein UDR-Setup in den VNets für 0.0.0.0/0 und dem nächsten Hop 10.5.0.5. Zurzeit lässt Virtual WAN im virtuellen Hub für 0.0.0.0/0 kein NVA für den nächsten Hop zu.

1. Navigieren Sie im Azure-Portal zu Ihrem virtuellen Hub, und erstellen Sie eine benutzerdefinierte Routingtabelle **RT_Shared**, die Routen über die Weitergabe von allen VNets und Filialverbindungen erlernt. In **Abbildung 2** ist dies als leere benutzerdefinierte Routingtabelle **RT_Shared** dargestellt.

   * **Routen:** Sie müssen keine statischen Routen hinzufügen.

   * **Zuordnung:** Wählen Sie die VNets 4 und 5 aus, wodurch die Verbindungen dieser beiden VNets der Routingtabelle **RT_Shared** zugeordnet werden.

   * **Weitergabe:** Da Sie möchten, dass alle Filialen und VNet-Verbindungen ihre Routen dynamisch an diese Routingtabelle weitergeben, wählen Sie Filialen und alle VNets aus.

1. Erstellen Sie eine benutzerdefinierte Routingtabelle **RT_V2B**, um den Datenverkehr von den VNets 1, 2 und 3 an Filialen weiterzuleiten.

   * **Routen:** Fügen Sie einen aggregierten statischen Routeneintrag für Filialen (VPN/ER/P2S, 10.2.0.0/16 in **Abbildung 2**) mit dem nächsten Hop als VNET4-Verbindung hinzu. Außerdem müssen Sie eine statische Route in der VNET4-Verbindung für Filialenpräfixe konfigurieren und angeben, dass der nächste Hop die spezifische IP-Adresse des virtuellen Netzwerkgeräts in VNET4 sein soll.

   * **Zuordnung:** Wählen Sie alle VNets 1, 2 und 3 aus. Dies bedeutet, dass die VNet-Verbindungen 1, 2 und 3 dieser Routingtabelle zugeordnet werden und in dieser Routingtabelle Routen (statisch und dynamisch über die Weitergabe) erlernen können.

   * **Weitergabe:** Verbindungen geben Routen an Routingtabellen weiter. Durch die Auswahl der VNets 1, 2 und 3 wird die Weitergabe von Routen von den VNets 1, 2 und 3 an diese Routingtabelle ermöglicht. Es ist nicht erforderlich, Routen von Filialverbindungen an „RT_V2B“ weiterzugeben, da der Datenverkehr des Filial-VNets über das NVA in VNET4 geleitet wird.
  
1. Bearbeiten Sie die Standardroutingtabelle **DefaultRouteTable**.

   Alle VPN-, ExpressRoute- und Benutzer-VPN-Verbindungen sind der Standardroutingtabelle zugeordnet. Alle VPN-, ExpressRoute- und Benutzer-VPN-Verbindungen geben Routen an dieselben Routingtabellen weiter.

   * **Routen:** Fügen Sie einen aggregierten statischen Routeneintrag für die VNets 1, 2 und 3 (10.1.0.0/16 in **Abbildung 2**) mit dem nächsten Hop als VNET4-Verbindung hinzu. Außerdem müssen Sie eine statische Route sowie drei aggregierte Präfixe in der VNET4-Verbindung für die VNets 1, 2 und 3 konfigurieren und angeben, dass der nächste Hop die spezifische IP-Adresse des virtuellen Netzwerkgeräts in VNET4 sein soll.

   * **Zuordnung:** Vergewissern Sie sich, dass die Option für Filialen (VPN/ER/P2S) ausgewählt ist, und stellen Sie sicher, dass lokale Filialverbindungen der *defaultroutetable* zugeordnet sind.

   * **Weitergabe von:** Vergewissern Sie sich, dass die Option für Filialen (VPN/ER/P2S) ausgewählt ist, und stellen Sie sicher, dass lokale Verbindungen Routen an die *defaultroutetable* weitergeben.

**Abbildung 2**

:::image type="content" source="./media/routing-scenarios/nva-custom/figure-2.png" alt-text="Abbildung 2":::

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu Virtual WAN finden Sie unter [Häufig gestellte Fragen](virtual-wan-faq.md).
* Weitere Informationen zum Routing für virtuelle Hubs finden Sie unter [Informationen zum Routing virtueller Hubs](about-virtual-hub-routing.md).
