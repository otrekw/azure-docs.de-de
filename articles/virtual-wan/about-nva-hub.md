---
title: 'Azure Virtual WAN: Virtuelles Netzwerkgerät im Hub'
description: In diesem Artikel erfahren Sie mehr über virtuelle Netzwerkgeräte im Virtual WAN-Hub.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 07/06/2020
ms.author: scottnap
Customer intent: As someone with a networking background, I want to learn about Network Virtual Appliances in the Virtual WAN hub.
ms.openlocfilehash: ad7c7fb5111ce700a5725336b7c9db788e178c4c
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87094832"
---
# <a name="about-network-virtual-appliance-in-an-azure-virtual-wan-hub-preview"></a>Virtuelles Netzwerkgerät in einem Azure Virtual WAN-Hub (Vorschauversion)

Für Azure Virtual WAN wurde in Zusammenarbeit mit Netzwerkpartnern eine Automatisierung erstellt, mit der Geräte in der Kundenumgebung (Customer Premises Equipment, CPE) ganz einfach mit einem Azure-VPN-Gateway im virtuellen Hub verbunden werden können. Azure arbeitet mit ausgewählten Netzwerkpartnern zusammen, um Kunden die direkte Bereitstellung eines virtuellen Netzwerkgeräts (Network Virtual Appliance, NVA) von einem Drittanbieter im virtuellen Hub zu ermöglichen. Dadurch können Kunden ihr Zweigniederlassungs-CPE mit dem gleichen Marken-NVA im virtuellen Hub verbinden und so proprietäre End-to-End-SD-WAN-Funktionen nutzen.

Barracuda Networks ist der erste Partner, der mit seinem Produkt [Barracuda CloudGen WAN](https://www.barracuda.com/products/cloudgenwan) ein NVA-Angebot bietet, das direkt im Virtual WAN-Hub bereitgestellt werden kann. Azure arbeitet mit mehreren Partnern zusammen, sodass weitere Angebote folgen werden.

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

NVAs im virtuellen Hub sind in den folgenden Regionen als Vorschauversion verfügbar:

|Geopolitische Region | Azure-Regionen|
|---|---|
| Nordamerika| USA, Westen; USA, Süden-Mitte; USA, Osten 2   |
| Südamerika | Brasilien Süd |
| Europa | Europa, Westen; Vereinigtes Königreich, Süden|
|  Naher Osten | Vereinigte Arabische Emirate, Norden |
| Asia | Japan, Osten |
| Australien | Australien (Osten) |

## <a name="faq"></a>Häufig gestellte Fragen

### <a name="i-am-a-network-appliance-partner-and-want-to-get-our-nva-in-the-hub--can-i-join-this-partner-program"></a>Ich bin ein Partner für Netzwerkgeräte und möchte die Bereitstellung unseres NVA im Hub ermöglichen.  Kann ich an diesem Partnerprogramm teilnehmen?

Leider haben wir zurzeit keine Kapazität für neue Partnerangebote. Versuchen Sie es bitte im November nochmal.

### <a name="can-i-deploy-any-nva-from-azure-marketplace-into-the-virtual-wan-hub"></a>Kann ich ein beliebiges NVA aus dem Azure Marketplace im Virtual WAN-Hub bereitstellen?

Nein. Zurzeit ist nur [Barracuda CloudGen WAN](https://aka.ms/BarracudaMarketPlaceOffer) zur Bereitstellung im Virtual WAN-Hub verfügbar.

### <a name="what-is-the-cost-of-the-nva"></a>Welche Kosten fallen für das NVA an?

Sie müssen eine Lizenz für Ihr Barracuda CloudGen WAN-NVA von Barracuda erwerben. Weitere Informationen zur Lizenzierung finden Sie auf der [Seite zu CloudGen WAN von Barracuda](https://www.barracuda.com/products/cloudgenwan). Außerdem stellt Ihnen Microsoft Gebühren für die NVA-Infrastruktureinheiten und anderen Ressourcen in Rechnung, die Sie nutzen. Weitere Informationen finden Sie unter [Informationen zu den Preisen von Virtual WAN](pricing-concepts.md).

### <a name="can-i-deploy-an-nva-to-a-basic-hub"></a>Kann ich ein NVA in einem Basic-Hub bereitstellen?

Nein. Sie müssen einen Standard-Hub verwenden, wenn Sie ein NVA bereitstellen möchten.

### <a name="can-i-deploy-an-nva-into-a-secure-hub"></a>Kann ich ein NVA in einem geschützten Hub bereitstellen?

Ja. Barracuda CloudGen WAN kann in einem Hub mit Azure Firewall bereitgestellt werden.

### <a name="can-i-connect-any-cpe-device-in-my-branch-office-to-barracuda-cloudgen-wan-nva-in-the-hub"></a>Kann ich ein beliebiges CPE-Gerät in meiner Zweigstelle mit dem Barracuda CloudGen WAN-NVA im Hub verbinden?

Nein. Barracuda CloudGen WAN ist nur mit CPE-Geräten von Barracuda kompatibel. Weitere Informationen zu den Anforderungen von CloudGen WAN finden Sie auf der [Seite zu CloudGen WAN von Barracuda](https://www.barracuda.com/products/cloudgenwan).

### <a name="what-routing-scenarios-are-supported-with-nva-in-the-hub"></a>Welche Routingszenarien werden mit NVAs im Hub unterstützt?

Alle von Virtual WAN unterstützten Routingszenarien werden mit NVAs im Hub unterstützt.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Virtual WAN finden Sie in der [Übersicht über Virtual WAN](virtual-wan-about.md).