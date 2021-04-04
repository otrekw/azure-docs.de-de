---
title: 'Azure ExpressRoute: Herstellen einer Verbindung mit Microsoft Cloud mithilfe von Global Reach'
description: Erfahren Sie, wie Azure ExpressRoute Global Reach ExpressRoute-Leitungen miteinander verbinden kann, um ein privates Netzwerk zwischen Ihren lokalen Netzwerken aufzubauen.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: conceptual
ms.date: 09/18/2020
ms.author: duau
ms.custom: references_regions
ms.openlocfilehash: 50679e11697a4227af69b8568c5f3cd23fe26cbe
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "98662796"
---
# <a name="expressroute-global-reach"></a>ExpressRoute Global Reach
ExpressRoute ist eine private und zuverlässige Möglichkeit, Ihre lokalen Netzwerke mit der Microsoft Cloud zu verbinden. Sie können aus Ihrem privaten Rechenzentrum oder Ihrem Unternehmensnetzwerk auf viele Microsoft-Clouddienste wie beispielsweise Azure und Microsoft 365 zugreifen. Angenommen, Sie haben eine Niederlassung in San Francisco mit einer ExpressRoute-Leitung im Silicon Valley und eine weitere Niederlassung in London mit einer ExpressRoute-Leitung in derselben Stadt. Beide Niederlassungen erhalten Highspeedkonnektivität mit Azure-Ressourcen in den Regionen „USA, Westen“ und „Vereinigtes Königreich, Süden“. Die Niederlassungen können jedoch keine Verbindung herstellen und nicht direkt Daten untereinander austauschen. Anders ausgedrückt: 10.0.1.0/24 kann Daten an die Netzwerke 10.0.3.0/24 und 10.0.4.0/24 senden, jedoch NICHT an das Netzwerk 10.0.2.0/24.

![Diagramm der nicht miteinander verbundenen Verbindungen mit ExpressRoute Global Reach][1]

Mithilfe von **ExpressRoute Global Reach** können Sie ExpressRoute-Leitungen miteinander verbinden, um ein privates Netzwerk zwischen Ihren lokalen Netzwerken aufzubauen. Im obigen Beispiel kann Ihre Niederlassung in San Francisco (10.0.1.0/24) dank ExpressRoute Global Reach direkt Daten mit Ihrer Niederlassung in London (10.0.2.0/24) über die bestehenden ExpressRoute-Leitungen und über das globale Netzwerk von Microsoft austauschen. 

![Diagramm der miteinander verbundenen Verbindungen mit ExpressRoute Global Reach][2]

## <a name="use-case"></a>Anwendungsfall
ExpressRoute Global Reach dient zur Ergänzung der WAN-Implementierung des Dienstanbieters und vernetzt Ihre Niederlassungen auf der ganzen Welt. Wenn Ihr Dienstanbieter beispielsweise in erster Linie in den USA tätig ist und sämtliche Niederlassungen in den USA vernetzt hat, der Dienstanbieter jedoch nicht in Japan und Hongkong agiert, können Sie mit ExpressRoute Global Reach mit einem lokalen Dienstanbieter arbeiten. Microsoft vernetzt Ihre Niederlassungen dann über ExpressRoute und unser globales Netzwerk mit denen in den USA.

![Diagramm eines Anwendungsfalls für ExpressRoute Global Reach][3]

## <a name="availability"></a>Verfügbarkeit 
ExpressRoute Global Reach wird an den folgenden Standorten unterstützt. 

> [!NOTE] 
> Um ExpressRoute Global Reach zwischen [verschiedenen geopolitischen Regionen](expressroute-locations-providers.md#locations) zu aktivieren, ist für Ihre Verbindungen eine **Premium-SKU** erforderlich.

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
* Südafrika (nur Johannesburg)
* Schweden
* Schweiz
* United Kingdom
* USA

## <a name="next-steps"></a>Nächste Schritte
- Sehen Sie sich die [häufig gestellten Fragen zu Global Reach](expressroute-faqs.md#globalreach) an.
- Erfahren Sie, wie Sie [Global Reach aktivieren](expressroute-howto-set-global-reach.md).
- Erfahren Sie, wie Sie [eine ExpressRoute-Verbindung mit Ihrem virtuellen Netzwerk verknüpfen](expressroute-howto-linkvnet-arm.md).

<!--Image References-->
[1]: ./media/expressroute-global-reach/1.png "Diagramm ohne Global Reach"
[2]: ./media/expressroute-global-reach/2.png "Diagramm mit Global Reach"
[3]: ./media/expressroute-global-reach/3.png "Anwendungsfall zu Global Reach"
