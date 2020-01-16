---
title: Hinzufügen einer Bildebene zu Azure Maps | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie dem Azure Maps Web SDK eine Bildebene hinzufügen.
author: rbrundritt
ms.author: richbrun
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 631a9e2d44b798404ee7567d3ccfed90628d2f8b
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75432866"
---
# <a name="add-an-image-layer-to-a-map"></a>Hinzufügen einer Bildebene zu einer Karte

In diesem Artikel erfahren Sie, wie Sie einen festen Satz an Koordinaten auf der Karte mit einem Bild überlagern können. Hier sind einige Beispiele für Bildtypen, mit denen Karten häufig überlagert werden:

* Von Drohnen aufgenommene Bilder
* Gebäudegrundrisse
* Bilder von historischen oder anderen Spezialkarten
* Blaupausen von Auftragsstandorten
* Wetterradarbilder

> [!TIP]
> Ein [ImageLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.imagelayer?view=azure-iot-typescript-latest)-Element (Bildebene) ist eine einfache Möglichkeit, eine Karte mit einem Bild zu überlagern. Beachten Sie, dass bei Browsern beim Laden großer Bilder Probleme auftreten können. In diesem Fall ist die Aufteilung des Bilds in Kacheln sinnvoll, die Sie dann als [TileLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.tilelayer?view=azure-iot-typescript-latest) (Kachelebene) in die Karte laden können.

Die Bildebene unterstützt folgende Bildformate:

- JPEG
- PNG
- BMP
- GIF (keine Animationen)

## <a name="add-an-image-layer"></a>Hinzufügen einer Bildebene

Der folgende Code legt ein Bild einer [Karte von Newark, New Jersey, von 1922](https://www.lib.utexas.edu/maps/historical/newark_nj_1922.jpg) über die Karte. Ein [ImageLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.imagelayer?view=azure-iot-typescript-latest)-Element wird erstellt, indem eine URL zu einem Bild und Koordinaten für die vier Ecken im Format `[Top Left Corner, Top Right Corner, Bottom Right Corner, Bottom Left Corner]` übergeben werden.

```javascript
//Create an image layer and add it to the map.
map.layers.add(new atlas.layer.ImageLayer({
    url: 'newark_nj_1922.jpg',
    coordinates: [
        [-74.22655, 40.773941], //Top Left Corner
        [-74.12544, 40.773941], //Top Right Corner
        [-74.12544, 40.712216], //Bottom Right Corner
        [-74.22655, 40.712216]  //Bottom Left Corner
    ]
}));
```

Hier finden Sie das vollständige, ausführbare Codebeispiel für den oben erläuterten Code.

<br/>

<iframe height='500' scrolling='no' title='Einfache Bildebene' src='//codepen.io/azuremaps/embed/eQodRo/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/eQodRo/'>Simple Image Layer</a> (Einfache Bildebene) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="import-a-kml-ground-overlay"></a>Importieren einer KML-Bodenüberlagerung

Im nächsten Beispiel sehen Sie, wie die Karte mit einer KML-Bodenüberlagerung als Bildebene überlagert wird. KML-Bodenüberlagerungen verfügen über Nord-, Süd-, Ost- und Westkoordinaten und ermöglichen eine Drehung gegen den Uhrzeigersinn, wohingegen die Bildebene Koordinaten für jede Ecke des Bilds erwartet. Bei der KML-Bodenüberlagerung in diesem Beispiel handelt es sich um die Kathedrale von Chartres (Quelle: [Wikimedia](https://commons.wikimedia.org/wiki/File:Chartres.svg/overlay.kml)).

Der folgende Code verwendet die statische `getCoordinatesFromEdges`-Funktion der [Imagelayer-](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.imagelayer?view=azure-iot-typescript-latest)-Klasse. Er berechnet die vier Ecken des Bilds aus den Nord-, Süd-, Ost-, West- und Drehungsinformationen der KML-Bodenüberlagerung.

<br/>

<iframe height='500' scrolling='no' title='KML-Bodenüberlagerung als Bildebene' src='//codepen.io/azuremaps/embed/EOJgpj/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/EOJgpj/'>KML Ground Overlay as Image Layer</a> (KML-Bodenüberlagerung als Bildebene) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="customize-an-image-layer"></a>Anpassen einer Bildebene

Die Bildebene bietet zahlreiche Optionen für die Formatierung. Mit dem folgenden Tool können Sie diese ausprobieren.

<br/>

<iframe height='700' scrolling='no' title='Bildebenenoptionen' src='//codepen.io/azuremaps/embed/RqOGzx/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/RqOGzx/'>Image Layer Options</a> (Bildebenenoptionen) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr zu den in diesem Artikel verwendeten Klassen und Methoden:

> [!div class="nextstepaction"]
> [ImageLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.imagelayer?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [ImageLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.imagelayeroptions?view=azure-iot-typescript-latest)

In den folgenden Artikeln finden Sie weitere Codebeispiele, die Sie Ihren Karten hinzufügen können:

> [!div class="nextstepaction"]
> [Hinzufügen einer Kachelebene](./map-add-tile-layer.md)