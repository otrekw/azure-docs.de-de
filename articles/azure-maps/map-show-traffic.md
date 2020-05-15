---
title: Anzeigen von Verkehrsinfos auf einer Karte | Microsoft Azure Maps
description: In diesem Artikel erfahren Sie, wie Sie mithilfe des Microsoft Azure Maps Web SDK auf einer Karte Verkehrsinfos anzeigen.
author: Philmea
ms.author: philmea
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 9c17c3cc22d478d81ed3c2b2ae9f61c173aad6cd
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2020
ms.locfileid: "83123920"
---
# <a name="show-traffic-on-the-map"></a>Anzeigen von Datenverkehr auf einer Karte

In Azure Maps sind zwei Arten von Verkehrsdaten verfügbar:

- Vorfallsdaten: Punkt- und linienbasierte Daten, z. B. für Baustellen, Straßensperrungen und Unfälle.
- Flussdaten: Stellt Metriken zum Verkehrsfluss auf Straßen bereit. Daten zum Verkehrsfluss werden häufig verwendet, um Straßen farbig zu markieren. Die Farben basieren auf dem Verkehrsaufkommen, das aufgrund einer Geschwindigkeitsbeschränkung oder anderen Metrik zu einer Verlangsamung des Verkehrsflusses führt. Die Daten zum Verkehrsfluss in Azure Maps verfügen über drei unterschiedliche Metriken für Messungen:
    - `relative`: Geschwindigkeit relativ zur Geschwindigkeit bei ungehindertem Verkehrsfluss auf der Straße.
    - `absolute`: Absolute Geschwindigkeit aller Fahrzeuge auf der Straße.
    - `relative-delay`: Zeigt Bereiche mit einer langsameren Geschwindigkeit als der durchschnittlichen erwarteten Verzögerung an.

Der folgende Code verdeutlicht, wie Sie Verkehrsdaten auf der Karte anzeigen.

```javascript
//Show traffic on the map using the traffic options.
map.setTraffic({
    incidents: true,
    flow: 'relative'
});
```

Nachfolgend finden Sie das vollständige ausführbare Codebeispiel für die oben erläuterte Funktionalität.

<br/>

<iframe height='500' scrolling='no' title='Anzeigen von Datenverkehr auf einer Karte' src='//codepen.io/azuremaps/embed/WMLRPw/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/WMLRPw/'>Show traffic on a map</a> (Anzeigen von Datenverkehr auf einer Karte) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="traffic-overlay-options"></a>Optionen für Verkehrsdatenüberlagerungen

Mit dem folgenden Tool können Sie zwischen den unterschiedlichen Einstellungen für Verkehrsdatenüberlagerungen wechseln, um auszuprobieren, wie sich das Rendering ändert. 

<br/>

<iframe height="700" style="width: 100%;" scrolling="no" title="Optionen für Verkehrsdatenüberlagerungen" src="//codepen.io/azuremaps/embed/RwbPqRY/?height=700&theme-id=0&default-tab=result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Weitere Informationen finden Sie im Pen <a href='https://codepen.io/azuremaps/pen/RwbPqRY/'>Traffic overlay options</a> (Optionen für Verkehrsdatenüberlagerungen) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) bei <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr zu den in diesem Artikel verwendeten Klassen und Methoden:

> [!div class="nextstepaction"]
> [Map](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map)

> [!div class="nextstepaction"]
> [TrafficOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.trafficoptions)

Verbessern der Benutzerfreundlichkeit:

> [!div class="nextstepaction"]
> [Karteninteraktion mit Mausereignissen](map-events.md)

> [!div class="nextstepaction"]
> [Erstellen einer zugänglichen Karte](map-accessibility.md)

> [!div class="nextstepaction"]
> [Seite für Beispielcode](https://aka.ms/AzureMapsSamples)
