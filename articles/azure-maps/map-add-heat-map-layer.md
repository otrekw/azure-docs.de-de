---
title: Hinzufügen einer Wärmebildebene zu einer Karte | Microsoft Azure Maps
description: In diesem Artikel erfahren Sie, wie Sie einer Karte mithilfe des Microsoft Azure Maps Web SDK eine Wärmebildebene hinzufügen.
author: rbrundritt
ms.author: richbrun
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: ead30a80a6568e72f922f355916d31121b49a93b
ms.sourcegitcommit: f9601bbccddfccddb6f577d6febf7b2b12988911
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/12/2020
ms.locfileid: "75911218"
---
# <a name="add-a-heat-map-layer"></a>Hinzufügen einer Wärmebildebene

Wärmebilder, die auch als Punktdichtekarten bezeichnet werden, stellen eine Art der Datenvisualisierung dar, die verwendet wird, um die Dichte von Daten mit Farbbereichen darzustellen. Sie werden häufig verwendet, um „Hotspots“ von Daten auf einer Karte anzuzeigen, und sind gut zum Rendern von großen Punktdatasets geeignet. 

Das Rendern von Zehntausenden von Punkten innerhalb der Kartenansicht als Symbole deckt z. B. den größten Teil des Kartenbereichs ab. Dies führt sehr wahrscheinlich dazu, dass sich viele Symbole überlappen, sodass es schwierig wird, einen Einblick in die Daten zu erhalten. Wenn Sie dasselbe Dataset jedoch als Wärmebild visualisieren, können Sie einfach erkennen, wo die Punktdaten die größte Dichte aufweisen und wie die Dichte im Vergleich zu anderen Bereichen ausfällt.

Sie können Wärmebilder in vielen verschiedenen Szenarien verwenden, z. B.:

- **Temperaturdaten:** Bietet Näherungen für die Temperatur zwischen zwei Datenpunkten.
- **Daten für Geräuschsensoren:** Zeigt nicht nur die Intensität der Geräusche am Standort des Sensors, sondern auch deren Dissipation über eine gewisse Entfernung. Der Geräuschpegel ist an einem bestimmten Standort möglicherweise nicht sehr hoch. Wenn sich aber die Geräuschebenen von mehreren Sensoren überlappen, können in diesem überlappenden Bereich höhere Geräuschpegel auftreten, die dann im Wärmebild sichtbar wären.
- **GPS-Überwachung:** Zeigt die Geschwindigkeit als gewichtete Höhenkarte, bei der die Intensität der einzelnen Datenpunkte auf der Geschwindigkeit basiert. So können Sie beispielsweise erkennen, wo ein Fahrzeug beschleunigt wurde.

> [!TIP]
> Wärmebildebenen rendern in der Standardeinstellung die Koordinaten aller Geometrien in einer Datenquelle. Legen Sie die `filter`-Eigenschaft der Ebene auf `['==', ['geometry-type'], 'Point']` fest, um die Ebene dahin gehend zu beschränken, dass nur Punktgeometriefunktionen gerendert werden. Wenn Sie auch MultiPoint-Funktionen einschließen möchten, legen Sie die `filter`-Eigenschaft der Ebene auf `['any', ['==', ['geometry-type'], 'Point'], ['==', ['geometry-type'], 'MultiPoint']]` fest.

<br/>

<iframe src="https://channel9.msdn.com/Shows/Internet-of-Things-Show/Heat-Maps-and-Image-Overlays-in-Azure-Maps/player" width="960" height="540" allowFullScreen frameBorder="0"></iframe>

## <a name="add-a-heat-map-layer"></a>Hinzufügen einer Wärmebildebene

Wenn Sie eine Datenquelle, die aus Punkten besteht, als Wärmebild rendern möchten, übergeben Sie diese an eine Instanz der `HeatMapLayer`-Klasse, und fügen Sie sie der Karte hinzu.

Im folgenden Code weist jeder Wärmepunkt in allen Zoomfaktoren einen Radius von 10 Pixeln auf. Beim Hinzufügen der Wärmebildebene zur Karte wird sie in diesem Beispiel unter der Bezeichnungsebene hinzugefügt. Dies erhöht die Benutzerfreundlichkeit, da die Bezeichnungen deutlich über dem Wärmebild zu erkennen sind. Die Daten für dieses Beispiel stammen vom [USGS Earthquake Hazards Program](https://earthquake.usgs.gov/) und stellen relevante Erdbeben der letzten 30 Tage dar.

```javascript
//Create a data source and add it to the map.
var datasource = new atlas.source.DataSource();
map.sources.add(datasource);

//Load a dataset of points, in this case earthquake data from the USGS.
datasource.importDataFromUrl('https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/all_week.geojson');

//Create a heatmap and add it to the map.
map.layers.add(new atlas.layer.HeatMapLayer(datasource, null, {
  radius: 10,
  opacity: 0.8
}), 'labels');
```

Hier finden Sie das vollständige, ausführbare Codebeispiel für den oben erläuterten Code.

<br/>

<iframe height='500' scrolling='no' title='Einfache Wärmebildebene' src='//codepen.io/azuremaps/embed/gQqdQB/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/gQqdQB/'>Simple Heat Map Layer</a> (Einfache Wärmebildebene) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="customize-the-heat-map-layer"></a>Anpassen der Wärmebildebene

Im vorherigen Beispiel wurde das Wärmebild angepasst, indem der Radius und die Deckkraft angepasst wurden. Auf der Wärmebildebene können verschiedene Optionen angepasst werden, z. B.:

* `radius`: Definiert einen Pixelradius, in dem die einzelnen Datenpunkte gerendert werden sollen. Der Radius kann als feste Zahl oder als Ausdruck festgelegt werden. Mithilfe eines Ausdrucks können Sie den Radius auf der Grundlage des Zoomfaktors skalieren. Er stellt dann ein zusammenhängendes räumliches Gebiet auf der Karte dar (beispielsweise einen 5-Meilen-Radius).
* `color`: Gibt an, welche Farben für das Wärmebild verwendet werden. Farbverläufe werden auf Wärmebildern häufig verwendet. Sie können diesen Effekt mit einem `interpolate`-Ausdruck erzielen. Sie können auch einen `step`-Ausdruck für die farbige Darstellung des Wärmebilds verwenden. In diesem Fall wird die Dichte visuell in Bereiche gegliedert, die einer Kontur- oder Radarkarte ähneln. Durch diese Farbpaletten werden die Farben kleinsten bis zum größten Dichtewert definiert. 

  Sie geben die Farbwerte für Wärmebilder als Ausdruck im `heatmap-density`-Wert an. Die Farbe bei Index „0“ in einem interpolation-Ausdruck bzw. die Standardfarbe eines step-Ausdrucks definiert die Farbe in einem Bereich ohne Daten. Sie können damit eine Hintergrundfarbe definieren. Häufig wird hierfür ein transparenter Wert oder ein halbtransparentes Schwarz festgelegt. 
   
  Nachfolgend finden Sie Beispiele für Farbausdrücke:

  | Ausdruck für Farbinterpolation | Ausdruck für eine abgestufte Farbpalette | 
  |--------------------------------|--------------------------|
  | \[<br/>&nbsp;&nbsp;&nbsp;&nbsp;'interpolate',<br/>&nbsp;&nbsp;&nbsp;&nbsp;\['linear'\],<br/>&nbsp;&nbsp;&nbsp;&nbsp;\['heatmap-density'\],<br/>&nbsp;&nbsp;&nbsp;&nbsp;0, 'transparent',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0.01, 'purple',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0.5, '#fb00fb',<br/>&nbsp;&nbsp;&nbsp;&nbsp;1, '#00c3ff'<br/>\] | \[<br/>&nbsp;&nbsp;&nbsp;&nbsp;'step',<br/>&nbsp;&nbsp;&nbsp;&nbsp;\['heatmap-density'\],<br/>&nbsp;&nbsp;&nbsp;&nbsp;'transparent',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0.01, 'navy',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0.25, 'green',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0.50, 'yellow',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0.75, 'red'<br/>\] |   

- `opacity`: Gibt an, wie undurchsichtig oder transparent die Wärmebildebene ist.
- `intensity`: Wendet einen Multiplikator auf die Gewichtung jedes Datenpunkts an, um die Gesamtintensität des Wärmebilds zu erhöhen. Dadurch können kleine Unterschiede in der Gewichtung von Datenpunkten einfacher visualisiert werden.
- `weight`: Standardmäßig weisen alle Datenpunkte eine Gewichtung von 1 auf, sodass alle Datenpunkte gleichmäßig gewichtet werden. Die Option „weight“ fungiert als Multiplikator und kann als Zahl oder als Ausdruck festgelegt werden. Wenn eine Zahl (z. B. 2) als Gewichtung festgelegt wird, entspricht das der zweifachen Platzierung jedes Datenpunkts auf der Karte und somit einer Verdoppelung der Dichte. Wenn Sie die Option „weight“ auf eine Zahl festlegen, wird das Wärmebild ähnlich wie mit der Option „intensity“ gerendert. 

  Wenn Sie jedoch einen Ausdruck verwenden, kann die Gewichtung der einzelnen Datenpunkte auf den Eigenschaften der einzelnen Datenpunkte basieren. Gehen Sie beispielsweise davon aus, dass jeder Datenpunkt ein Erdbeben darstellt. Eine wichtige Metrik, die jeder Erdbeben-Datenpunkt aufweist, ist der Magnitudenwert. Es kommt ständig zu Erdbeben. Die meisten weisen jedoch eine so geringe Stärke auf, dass sie nicht spürbar sind. Das Verwenden des Magnitudenwerts in einem Ausdruck, mit dem jedem Datenpunkt eine Gewichtung zugewiesen wird, ermöglicht eine bessere Darstellung der Bedeutung von Erdbeben im Wärmebild.
- `source` und `source-layer`: Ermöglicht Ihnen das Aktualisieren der Datenquelle.

Mit diesem Tool können Sie die verschiedenen Optionen für Wärmebildebenen testen.

<br/>

<iframe height='700' scrolling='no' title='Optionen für Wärmebildebenen' src='//codepen.io/azuremaps/embed/WYPaXr/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/WYPaXr/'>Line Layer Options</a> (Linienebeneoptionen) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="consistent-zoomable-heat-map"></a>Konsistentes zoombares Wärmebild

Standardmäßig ist für die in der Wärmebildebene gerenderten Daten ein fester Pixelradius für alle Zoomfaktoren definiert. Wenn Sie die Karte zoomen, werden die Daten aggregiert und die Wärmebildebene ändert sich. 

Zum Skalieren des Radius für jede Zoomebene können Sie einen `zoom`-Ausdruck verwenden, sodass jeder Datenpunkt den gleichen physischen Bereich der Karte abdeckt. Hierdurch sieht die Wärmebildebene statischer und konsistenter aus. Jede Zoomebene der Karte hat vertikal und horizontal doppelt so viele Pixel wie die vorherige Zoomebene. 

Wenn der Radius so skaliert wird, dass er sich mit jeder Zoomebene verdoppelt, wird ein Wärmebild erstellt, das auf allen Zoomebenen einheitlich aussieht. Verwenden Sie dazu `zoom` mit einem `exponential interpolation`-Ausdruck zur Basis 2, wie im folgenden Beispiel gezeigt. Zoomen Sie die Karte, um zu sehen, wie das Wärmebild mit der Zoomebene skaliert wird.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Konsistentes zoombares Wärmebild" src="//codepen.io/azuremaps/embed/OGyMZr/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Anzeigen des <a href='https://codepen.io/azuremaps/pen/OGyMZr/'>konsistenten zoombaren Wärmebilds</a> von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) in <a href='https://codepen.io'>CodePen</a>.
</iframe>

> [!TIP]
> Wenn Sie das Clustering für die Datenquelle aktivieren, werden die Punkte, die nah beieinander liegen, zu einem Punkt gruppiert. Sie können die Anzahl der Punkte von jedem Cluster als weight-Ausdruck für das Wärmebild verwenden und damit die Anzahl der Punkte reduzieren, die gerendert werden müssen. Die Anzahl der Punkte in einem Cluster wird wie im Folgenden veranschaulicht in einer `point_count`-Eigenschaft der point-Funktion gespeichert. 
> ```JavaScript
> var layer = new atlas.layer.HeatMapLayer(datasource, null, {
>    weight: ['get', 'point_count']
> });
> ```
> Wenn der Radius für das Clustering nur wenige Pixel beträgt, gibt es beim Rendern nur geringe sichtbare Unterschiede. Bei einem größeren Radius werden mehr Punkte zusammengefasst und damit die Leistung des Wärmebilds verbessert.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr zu den in diesem Artikel verwendeten Klassen und Methoden:

> [!div class="nextstepaction"]
> [HtmlMarker-Klasse](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarker?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [HeatMapLayerOptions-Schnittstelle](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.heatmaplayeroptions?view=azure-iot-typescript-latest)

Weitere Codebeispiele, die Sie zu Ihren Karten hinzufügen können, finden Sie in den folgenden Artikeln:

> [!div class="nextstepaction"]
> [Erstellen einer Datenquelle](create-data-source-web-sdk.md)

> [!div class="nextstepaction"]
> [Verwenden von datengesteuerten Formatvorlagenausdrücken](data-driven-style-expressions-web-sdk.md)
