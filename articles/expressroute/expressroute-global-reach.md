---
title: 'Azure ExpressRoute: Herstellen einer Verbindung mit Microsoft Cloud mithilfe von Global Reach'
description: In diesem Artikel wird ExpressRoute Global Reach erläutert.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 06/09/2020
ms.author: cherylmc
ms.openlocfilehash: 5022efacf33a2c07f1388666b5de26ee86322739
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84636732"
---
# <a name="expressroute-global-reach"></a>ExpressRoute Global Reach
ExpressRoute ist eine private und zuverlässige Möglichkeit, Ihre lokalen Netzwerke mit der Microsoft Cloud zu verbinden. Sie können aus Ihrem privaten Rechenzentrum oder Ihrem Unternehmensnetzwerk auf viele Microsoft-Clouddienste wie beispielsweise Azure und Office 365 zugreifen. Angenommen, Sie haben eine Niederlassung in San Francisco mit einer ExpressRoute-Leitung im Silicon Valley und eine weitere Niederlassung in London mit einer ExpressRoute-Leitung in derselben Stadt. Beide Niederlassungen erhalten Highspeedkonnektivität mit Azure-Ressourcen in den Regionen „USA, Westen“ und „Vereinigtes Königreich, Süden“. Allerdings können die Niederlassungen nicht direkt Daten miteinander austauschen. Anders ausgedrückt: 10.0.1.0/24 kann Daten an 10.0.3.0/24 und 10.0.4.0/24 senden, jedoch NICHT an 10.0.2.0/24.

![ohne][1]

Mithilfe von **ExpressRoute Global Reach** können Sie ExpressRoute-Leitungen miteinander verbinden, um ein privates Netzwerk zwischen Ihren lokalen Netzwerken aufzubauen. Im obigen Beispiel kann Ihre Niederlassung in San Francisco (10.0.1.0/24) dank ExpressRoute Global Reach direkt Daten mit Ihrer Niederlassung in London (10.0.2.0/24) über die bestehenden ExpressRoute-Leitungen und über das globale Netzwerk von Microsoft austauschen. 

![mit][2]

## <a name="use-case"></a>Anwendungsfall
ExpressRoute Global Reach dient zur Ergänzung der WAN-Implementierung des Dienstanbieters und vernetzt Ihre Niederlassungen auf der ganzen Welt. Wenn Ihr Dienstanbieter beispielsweise in erster Linie in den USA tätig ist und sämtliche Niederlassungen in den USA vernetzt hat, der Dienstanbieter jedoch nicht in Japan und Hongkong agiert, können Sie mit ExpressRoute Global Reach mit einem lokalen Dienstanbieter arbeiten. Microsoft vernetzt Ihre Niederlassungen dann über ExpressRoute und unser globales Netzwerk mit denen in den USA.

![Anwendungsfall][3]

## <a name="availability"></a>Verfügbarkeit 
ExpressRoute Global Reach wird derzeit in den folgenden Ländern unterstützt:

* Australien
* Canada
* Frankreich
* Deutschland
* Hongkong (SAR)
* Irland
* Japan
* Korea
* Niederlande
* Neuseeland
* Norwegen
* Singapur
* Schweden
* Schweiz
* United Kingdom
* USA

Ihre ExpressRoute-Leitungen müssen an den [ExpressRoute-Peeringstandorten](expressroute-locations.md) in den oben aufgeführten Ländern oder Regionen eingerichtet werden. Um ExpressRoute Global Reach zwischen [verschiedenen geopolitischen Regionen](expressroute-locations.md) zu erstellen, ist für Ihre Leitungen die SKU „Premium“ erforderlich.

## <a name="next-steps"></a>Nächste Schritte
1. [Sehen Sie sich die Global Reach-FAQ an.](expressroute-faqs.md#globalreach)
2. [Erfahren Sie, wie Sie Global Reach aktivieren.](expressroute-howto-set-global-reach.md)
3. [Erfahren Sie, wie Sie eine ExpressRoute-Verbindung mit Ihrem virtuellen Netzwerk verknüpfen.](expressroute-howto-linkvnet-arm.md)


<!--Image References-->
[1]: ./media/expressroute-global-reach/1.png "Diagramm ohne Global Reach"
[2]: ./media/expressroute-global-reach/2.png "Diagramm mit Global Reach"
[3]: ./media/expressroute-global-reach/3.png "Anwendungsfall zu Global Reach"
