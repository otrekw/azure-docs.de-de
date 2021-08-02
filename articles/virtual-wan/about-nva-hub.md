---
title: 'Azure Virtual WAN: Virtuelles Netzwerkgerät im Hub'
description: Erfahren Sie mehr über virtuelle Netzwerkgeräte auf dem Virtual WAN-Hub.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 06/02/2021
ms.author: scottnap
ms.openlocfilehash: 68e5216257fd32237f3d67f05f0e17a0b8e16dbd
ms.sourcegitcommit: c385af80989f6555ef3dadc17117a78764f83963
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2021
ms.locfileid: "111411903"
---
# <a name="about-network-virtual-appliance-in-an-azure-virtual-wan-hub"></a>Virtuelles Netzwerkgerät in einem Azure Virtual WAN-Hub

Für Azure Virtual WAN wurde in Zusammenarbeit mit Netzwerkpartnern eine Automatisierung erstellt, mit der Geräte in der Kundenumgebung (Customer Premises Equipment, CPE) ganz einfach mit einem Azure-VPN-Gateway im virtuellen Hub verbunden werden können. Azure arbeitet mit ausgewählten Netzwerkpartnern zusammen, um Kunden die direkte Bereitstellung eines virtuellen Netzwerkgeräts (Network Virtual Appliance, NVA) von einem Drittanbieter im virtuellen Hub zu ermöglichen. Dadurch können Kunden ihr Zweigniederlassungs-CPE mit dem gleichen Marken-NVA im virtuellen Hub verbinden und so proprietäre End-to-End-SD-WAN-Funktionen nutzen.

Barracuda Networks und Cisco Systems sind die ersten Partner, die NVAs bereitstellen, die direkt auf dem Virtual WAN-Hub bereitgestellt werden können.  Die jeweilige Produktdokumentation finden Sie unter [Barracuda CloudGen WAN](https://www.barracuda.com/products/cloudgenwan), [Cisco Cloud OnRamp for Multi-Cloud](https://www.cisco.com/c/en/us/td/docs/routers/sdwan/configuration/cloudonramp/ios-xe-17/cloud-onramp-book-xe/cloud-onramp-multi-cloud.html#Cisco_Concept.dita_c61e0e7a-fff8-4080-afee-47b81e8df701) und [VMware SD-WAN](https://kb.vmware.com/s/article/82746). Azure arbeitet mit mehreren Partnern zusammen, sodass weitere Angebote folgen werden.

> [!NOTE]
> Nur NVA-Angebote, die zur Bereitstellung im Virtual WAN-Hub verfügbar sind, können im Virtual WAN-Hub bereitgestellt werden. Sie können nicht in einem beliebigen virtuellen Netzwerk in Azure bereitgestellt werden.

## <a name="how-does-it-work"></a><a name="how"></a>Funktionsweise

Die zur direkten Bereitstellung im Azure Virtual WAN-Hub verfügbaren NVAs werden speziell für die Verwendung im virtuellen Hub entwickelt. Das NVA-Angebot wird im Azure Marketplace als verwaltete Anwendung veröffentlicht. Kunden können es direkt über den Azure Marketplace oder im Azure-Portal über den virtuellen Hub bereitstellen.

:::image type="content" source="./media/about-nva-hub/high-level-process.png" alt-text="Übersicht über den Prozess":::

Die Funktionen der NVA-Angebote der einzelnen Partner unterscheiden sich je nach Bereitstellungsanforderungen geringfügig. Einige Dinge sind jedoch bei allen Partnerangeboten für NVAs im Virtual WAN-Hub gleich.

* Es handelt sich um eine verwaltete Anwendung, die über den Azure Marketplace angeboten wird.
* Die Kapazität und Abrechnung der NVA-Infrastruktur basieren auf Einheiten.
* Integritätsmetriken werden in Azure Monitor angezeigt.

### <a name="managed-application"></a><a name="managed"></a>Verwaltete Anwendung

Alle NVA-Angebote zur Bereitstellung im Virtual WAN-Hub verfügen über eine **verwaltete Anwendung**, die im Azure Marketplace erhältlich ist. Verwaltete Anwendungen ermöglichen Partnern Folgendes:

* Erstellen einer benutzerdefinierten Bereitstellungsfunktion für ihr NVA
* Bereitstellen einer spezialisierten Resource Manager-Vorlage, mit der das NVA direkt im Virtual WAN-Hub erstellt werden kann
* Direkte Abrechnung der Kosten für die Softwarelizenzierung oder Abrechnung über den Azure Marketplace
* Verfügbarmachen von benutzerdefinierten Eigenschaften und Verbrauchseinheiten für Ressourcen

NVA-Partner können abhängig von ihren Anforderungen bezüglich der Bereitstellung, Konfigurationslizenzierung und Verwaltung des NVA verschiedene Ressourcen erstellen. Wenn ein Kunde ein NVA im Virtual WAN-Hub erstellt, werden wie bei allen verwalteten Anwendungen zwei Ressourcengruppen in seinem Abonnement erstellt.

* **Kundenressourcengruppe**: Diese Ressourcengruppe enthält einen Anwendungsplatzhalter für die verwaltete Anwendung. Partner können damit beliebige benutzerdefinierte Eigenschaften verfügbar machen.
* **Verwaltete Ressourcengruppe**: Kunden können Ressourcen nicht direkt in dieser Ressourcengruppe konfigurieren oder ändern, da sie vom Herausgeber der verwalteten Anwendung gesteuert wird. Diese Ressourcengruppe enthält die Ressource **NetworkVirtualAppliances**.

:::image type="content" source="./media/about-nva-hub/managed-app.png" alt-text="Ressourcengruppen der verwalteten Anwendung":::

### <a name="nva-infrastructure-units"></a><a name="units"></a>NVA-Infrastruktureinheiten

Wenn Sie ein NVA im Virtual WAN-Hub erstellen, müssen Sie die Anzahl von NVA-Infrastruktureinheiten auswählen, die Sie mit dem NVA bereitstellen möchten. Eine **NVA-Infrastruktureinheit** ist eine Einheit, die die aggregierte Bandbreitenkapazität für ein NVA im Virtual WAN-Hub angibt. Hinsichtlich Kapazität und Dimensionierung ist eine **NVA-Infrastruktureinheit** mit einer VPN-[Skalierungseinheit](pricing-concepts.md#scale-unit) vergleichbar.

* Eine Infrastruktureinheit entspricht 500 MBit/s aggregierte Bandbreite für alle eingehenden Zweigstellenverbindungen mit dem NVA (zu einem Preis von 0,25 US-Dollar/Stunde).
* Azure unterstützt für eine Bereitstellung eines NVA in einem virtuellen Hub 1 – 80 NVA-Infrastruktureinheiten.
* Jeder Partner kann NVA-Infrastruktureinheiten in verschiedenen Paketen anbieten, die eine Teilmenge aller unterstützten Konfigurationen von NVA-Infrastruktureinheiten darstellen.

Ähnlich wie bei VPN-Skalierungseinheiten werden bei der Auswahl von *1 NVA-Infrastruktureinheit = 500 MBit/s* aus Gründen der Redundanz zwei Instanzen mit einem maximalen Durchsatz von jeweils 500 MBit/s erstellt. Wenn Sie z. B. fünf Verzweigungen mit jeweils 10 Mbit/s an der Verzweigung hatten, benötigen Sie ein Aggregat von 50 Mbit/s am Headend. Die Planung der aggregierten Kapazität des NVA sollte nach der Bewertung der Kapazität erfolgen, die zur Unterstützung der Anzahl von Zweigstellenverbindungen mit dem Hub benötigt wird.

## <a name="network-virtual-appliance-configuration-process"></a><a name="configuration"></a>NVA-Konfigurationsprozess

Unsere Partner haben eine Funktion entwickelt, die das NVA im Rahmen des Bereitstellungsprozesses automatisch konfiguriert. Nachdem das NVA im virtuellen Hub bereitgestellt wurde, müssen alle zusätzlichen Konfigurationsschritte, die für das NVA erforderlich sind, über das NVA-Partnerportal oder die Verwaltungsanwendung durchgeführt werden. Der direkte Zugriff auf das NVA ist nicht möglich.

## <a name="site-and-connection-resources-with-nvas"></a><a name="resources"></a>Standort- und Verbindungsressourcen mit NVAs

Anders als bei Azure VPN Gateway-Konfigurationen müssen Sie keine **Standortressourcen**, **Site-to-Site-Verbindungsressourcen** oder **Point-to-Site-Verbindungsressourcen** erstellen, um Ihre Zweigstellen mit dem NVA im Virtual WAN-Hub zu verbinden. All diese Ressourcen werden vom NVA-Partner verwaltet.

Sie müssen jedoch Hub-to-VNET-Verbindungen erstellen, um den Virtual WAN-Hub mit Ihren virtuellen Azure-Netzwerken zu verbinden.

## <a name="supported-regions"></a><a name="regions"></a>Unterstützte Regionen

NVAs im virtuellen Hub sind in den folgenden Regionen verfügbar:

|Geopolitische Region | Azure-Regionen|
|---|---|
| Nordamerika| Kanada, Mitte; Kanada, Osten; USA, Mitte; USA, Osten; USA, Osten 2; USA, Süden-Mitte; USA, Norden-Mitte; USA, Westen-Mitte; USA, Westen; USA, Westen 2 |
| Südamerika | „Brasilien, Süden“, „Brasilien, Südosten“ |
| Europa | „Frankreich, Mitte“, „Frankreich, Süden“, „Deutschland, Norden“, „Deutschland, Westen-Mitte“, „Europa, Norden“, „Norwegen, Osten“, „Norwegen, Westen“, „Schweiz, Norden“, „Schweiz, Westen“, „Vereinigtes Königreich, Süden“, „Vereinigtes Königreich, Westen“, „Europa, Westen“|
|  Naher Osten | Vereinigte Arabische Emirate, Norden |
| Asia |  „Asien, Osten“, „Japan, Osten“, „Japan, Westen“, „Südkorea, Mitte“, „Südkorea, Süden“, „Asien, Südosten“ | 
| Australien | „Australien, Südosten“, „Australien, Osten“, „Australien, Mitte“, „Australien, Mitte 2“|
| Afrika | Südafrika, Norden |
| Indien | „Indien, Süden“, „Indien, Westen“, „Indien, Mitte“ | 

## <a name="nva-faq"></a>Häufig gestellte Fragen zu NVAs

[!INCLUDE [NVA FAQ](../../includes/virtual-wan-nva-hub-faq.md)]

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Virtual WAN finden Sie in der [Übersicht über Virtual WAN](virtual-wan-about.md).
