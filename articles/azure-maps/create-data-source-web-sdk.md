---
title: Erstellen einer Datenquelle für eine Karte | Microsoft Azure Maps
description: In diesem Artikel erfahren Sie, wie Sie mithilfe des Microsoft Azure Maps Web SDK eine Datenquelle erstellen und einer Karte hinzufügen.
author: rbrundritt
ms.author: richbrun
ms.date: 08/08/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.custom: codepen
ms.openlocfilehash: 1675d63fd3a65beda46042f4a78535bb4e066e62
ms.sourcegitcommit: b07964632879a077b10f988aa33fa3907cbaaf0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2020
ms.locfileid: "77190235"
---
# <a name="create-a-data-source"></a>Erstellen einer Datenquelle

Das Azure Maps Web SDK speichert Daten in Datenquellen. Die Verwendung von Datenquellen optimiert die Datenvorgänge zum Abfragen und Rendern. Derzeit gibt es zwei Arten von Datenquellen:

**GeoJSON-Datenquelle**

Mithilfe der `DataSource`-Klasse speichert und lädt eine GeoJSON-basierte Datenquelle Daten lokal. GeoJSON-Daten können manuell oder mithilfe der Hilfsklassen im [Atlas. Data](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data)-Namespace erstellt werden. Die `DataSource`-Klasse stellt Funktionen zum Importieren von lokalen oder Remote-GeoJSON-Dateien bereit. Remote-GeoJSON-Dateien müssen auf einem CORs-fähigen Endpunkt gehostet werden. Die `DataSource`-Klasse bietet Funktionen für das Clustering von Punktdaten. Außerdem können Daten ganz einfach mit der `DataSource`-Klasse hinzugefügt, entfernt und aktualisiert werden.


> [!TIP]
> Angenommen, Sie möchten alle Daten in einer `DataSource` überschreiben. Wenn Sie zunächst die Funktion `clear` und dann `add` aufrufen, wird die Karte möglicherweise zweimal gerendert, was zu einer Verzögerung führen kann. Verwenden Sie stattdessen die `setShapes`-Funktion, die alle Daten in der Datenquelle entfernt und ersetzt und nur ein einziges erneutes Rendering der Karte auslöst.

**Vektorkachelquelle**

Eine Vektorkachelquelle beschreibt, wie auf eine Vektorkachelebene zugegriffen wird. Verwenden Sie die [VectorTileSource-](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.vectortilesource)-Klasse, um eine Vektorkachelquelle zu instanziieren. Vektorkachelebenen ähneln Kachelebenen, sind aber nicht identisch. Eine Kachelebene ist ein Rasterbild. Vektorkachelebenen sind eine komprimierte Datei im PBF-Format. Diese komprimierte Datei enthält Vektorkartendaten und eine oder mehrere Ebenen. Die Datei kann auf dem Client gerendert und formatiert werden, basierend auf dem Stil jeder einzelnen Ebene. Die Daten in einer Vektorkachel enthalten geografische Merkmale in Form von Punkten, Linien und Polygonen. Vektorkachelebenen haben gegenüber Rasterkachelebenen mehrere Vorteile:

 - Eine Dateigröße einer Vektorkachel ist in der Regel wesentlich kleiner als eine vergleichbare Rasterkachel. Somit wird auch weniger Bandbreite verwendet. Dies bedeutet geringere Wartezeiten, eine schnellere Karte und eine bessere Benutzererfahrung.
 - Da Vektorkacheln auf dem Client gerendert werden, passen sie sich an die Auflösung des Geräts an, auf dem sie angezeigt werden. Hieraus resultiert, dass die gerenderten Karten definierter und mit sehr klaren Bezeichnungen dargestellt werden.
 - Um den Stil der Daten in den Vektorkarten zu ändern, müssen Sie die Daten nicht erneut herunterladen, da der neue Stil auf den Client angewendet werden kann. Im Gegensatz dazu erfordert das Ändern des Stils einer Rasterkachelebene in der Regel das Laden von Kacheln vom Server und das anschließende Anwenden des neuen Stils.
 - Da die Daten in Vektorform bereitgestellt werden, ist zur Aufbereitung der Daten weniger serverseitige Verarbeitung erforderlich. Als Folge hieraus können die neueren Daten schneller zur Verfügung gestellt werden.

Alle Ebenen, die eine Vektorquelle verwenden, müssen einen `sourceLayer`-Wert angeben.

Nach der Erstellung können Datenquellen über die `map.sources`-Eigenschaft, die ein [SourceManager](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.sourcemanager) ist, der Karte hinzugefügt werden. Der folgende Code zeigt, wie Sie eine `DataSource` erstellen und der Karte hinzufügen.

```javascript
//Create a data source and add it to the map.
var dataSource = new atlas.source.DataSource();
map.sources.add(dataSource);
```

Azure Maps hält den offenen Standard [Mapbox Vector Tile Specification](https://github.com/mapbox/vector-tile-spec) ein.

## <a name="connecting-a-data-source-to-a-layer"></a>Verbinden einer Datenquelle mit einer Ebene

Daten werden mithilfe von Renderebenen auf der Karte gerendert. Auf eine einzelne Datenquelle kann von einer oder mehreren Renderebene verwiesen werden. Die folgenden Renderebenen erfordern eine Datenquelle:

- [Blasenebene](map-add-bubble-layer.md) – Punktdaten werden als skalierte Kreise mit Pixelradius gerendert.
- [Symbolebene](map-add-pin.md) – Punktdaten werden als Symbole oder Text gerendert.
- [Wärmebildebene](map-add-heat-map-layer.md) – Punktdaten werden als Dichtewärmebild gerendert.
- [Linienebene](map-add-shape.md) – Rendert eine Linie und/oder die Kontur von Polygonen. 
- [Polygonebene](map-add-shape.md) – füllt den Bereich eines Polygons mit einer Volltonfarbe oder einem Bildmuster aus.

Der folgende Code zeigt, wie Sie eine Datenquelle erstellen, sie zur Karte hinzufügen und mit einer Blasenebene verbinden. Und dann importieren Sie GeoJSON-Punktdaten von einem Remoteort in die Datenquelle. 

```javascript
//Create a data source and add it to the map.
var datasource = new atlas.source.DataSource();
map.sources.add(datasource);

//Create a layer that defines how to render points in the data source and add it to the map.
map.layers.add(new atlas.layer.BubbleLayer(datasource));

//Load the earthquake data.
datasource.importDataFromUrl('https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/significant_month.geojson');
```

Es gibt zusätzliche Renderebenen, die sich nicht mit diesen Datenquellen verbinden, die die Daten zum Rendern aber direkt laden. 

- [Bildebene](map-add-image-layer.md) – überlagert ein einzelnes Bild oberhalb der Karte und bindet seine Ecken an einen Satz angegebener Koordinaten.
- [Kachelebene](map-add-tile-layer.md) – überlagert eine Rasterkachelebene oberhalb der Karte.

## <a name="one-data-source-with-multiple-layers"></a>Eine Datenquelle mit mehreren Ebenen

Es können mehrere Ebenen mit einer Datenquelle verbunden sein. Es gibt viele verschiedene Szenarien, in denen diese Option nützlich ist. Nehmen wir als Beispiel ein Szenario, in dem ein Benutzer ein Polygon zeichnet. Wir sollten den Polygonbereich rendern und ausfüllen, während der Benutzer der Karte Punkte hinzufügt. Durch Hinzufügen einer formatierten Linie als Umriss des Polygons lassen sich die Kanten des Polygons besser erkennen, während der Benutzer ihn zeichnet. Um eine einzelne Position im Polygon bequem zu bearbeiten, können wir oberhalb von jeder Position einen Handle, z. B. eine Stecknadel oder einen Marker, hinzufügen.

![Karte mit mehreren Ebenen, die Daten aus einer einzigen Datenquelle rendern](media/create-data-source-web-sdk/multiple-layers-one-datasource.png)

Auf den meisten Kartenplattformen müssten Sie ein Polygonobjekt, ein Linienobjekt und für jede Position auf dem Polygon eine Stecknadel erstellen. Wenn das Polygon verändert wird, müssen Sie die Linien und Pins manuell aktualisieren. Dies kann sehr schnell zu einem komplexen Vorgang werden.

Mit Azure Maps benötigen Sie nur ein einzelnes Polygon in einer Datenquelle, wie im Code unten gezeigt.

```javascript
//Create a data source and add it to the map.
var dataSource = new atlas.source.DataSource();
map.sources.add(dataSource);

//Create a polygon and add it to the data source.
dataSource.add(new atlas.data.Polygon([[[/* Coordinates for polygon */]]]));

//Create a polygon layer to render the filled in area of the polygon.
var polygonLayer = new atlas.layer.PolygonLayer(dataSource, 'myPolygonLayer', {
     fillColor: 'rgba(255,165,0,0.2)'
});

//Create a line layer for greater control of rendering the outline of the polygon.
var lineLayer = new atlas.layer.LineLayer(dataSource, 'myLineLayer', {
     color: 'orange',
     width: 2
});

//Create a bubble layer to render the vertices of the polygon as scaled circles.
var bubbleLayer = new atlas.layer.BubbleLayer(dataSource, 'myBubbleLayer', {
     color: 'orange',
     radius: 5,
     outlineColor: 'white',
     outlineWidth: 2
});

//Add all layers to the map.
map.layers.add([polygonLayer, lineLayer, bubbleLayer]);
```

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr zu den in diesem Artikel verwendeten Klassen und Methoden:

> [!div class="nextstepaction"]
> [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-maps-typescript-latest)

> [!div class="nextstepaction"]
> [DataSourceOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.datasourceoptions?view=azure-maps-typescript-latest)

> [!div class="nextstepaction"]
> [VectorTileSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.vectortilesource?view=azure-maps-typescript-latest)

> [!div class="nextstepaction"]
> [VectorTileSourceOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.vectortilesourceoptions?view=azure-maps-typescript-latest)

In den folgenden Artikeln finden Sie weitere Codebeispiele, die Sie Ihren Karten hinzufügen können:

> [!div class="nextstepaction"]
> [Hinzufügen eines Popups](map-add-popup.md)

> [!div class="nextstepaction"]
> [Verwenden von datengesteuerten Formatvorlagenausdrücken](data-driven-style-expressions-web-sdk.md)

> [!div class="nextstepaction"]
> [Hinzufügen einer Symbolebene](map-add-pin.md)

> [!div class="nextstepaction"]
> [Hinzufügen einer Blasenebene](map-add-bubble-layer.md)

> [!div class="nextstepaction"]
> [Hinzufügen einer Linienebene](map-add-line-layer.md)

> [!div class="nextstepaction"]
> [Hinzufügen einer Polygonebene](map-add-shape.md)

> [!div class="nextstepaction"]
> [Hinzufügen eines Wärmebilds](map-add-heat-map-layer.md)

> [!div class="nextstepaction"]
> [Codebeispiele](https://docs.microsoft.com/samples/browse/?products=azure-maps)