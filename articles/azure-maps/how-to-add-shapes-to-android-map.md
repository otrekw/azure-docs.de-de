---
title: Hinzufügen einer Polygonebene zu Android-Karten | Microsoft Azure Maps
description: Hier wird erläutert, wie Sie Karten Polygone oder Kreise hinzufügen. Hier erfahren Sie, wie Sie mit dem Azure Maps Android SDK geometrische Formen anpassen und wie sich diese einfach aktualisieren und verwalten lassen.
author: rbrundritt
ms.author: richbrun
ms.date: 12/08/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.openlocfilehash: 1712cedab9cef23108fcc48b8e09bdc3e33065c4
ms.sourcegitcommit: 66b0caafd915544f1c658c131eaf4695daba74c8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/18/2020
ms.locfileid: "97679482"
---
# <a name="add-a-polygon-layer-to-the-map-android-sdk"></a>Hinzufügen einer Polygonebene zur Karte (Android SDK)

In diesem Artikel erfahren Sie, wie Sie mithilfe einer Polygonebene die Bereiche der `Polygon`- und `MultiPolygon`-Featuregeometrien auf einer Karte rendern können.

## <a name="prerequisites"></a>Voraussetzungen

Führen Sie unbedingt die Schritte im Dokument [Schnellstart: Erstellen einer Android-App](quick-android-map.md) aus. Codeblöcke in diesem Artikel können in den `onReady`-Ereignishandler von Karten eingefügt werden.

## <a name="use-a-polygon-layer"></a>Verwenden einer Polygonebene

Wenn eine Polygonebene mit einer Datenquelle verbunden ist und in die Karte geladen wird, werden die Bereiche mit `Polygon`- und `MultiPolygon`-Features gerendert. Fügen Sie zum Erstellen eines Polygons dieses zu einer Datenquelle hinzu und rendern Sie es mit einer Polygonebene unter Verwendung der `PolygonLayer`-KIasse.

```java
//Create a data source and add it to the map.
DataSource source = new DataSource();
map.sources.add(source);

//Create a rectangular polygon.
source.add(Polygon.fromLngLats(
    Arrays.asList(
        Arrays.asList(
            Point.fromLngLat(-73.98235, 40.76799),
            Point.fromLngLat(-73.95785, 40.80044),
            Point.fromLngLat(-73.94928, 40.79680),
            Point.fromLngLat(-73.97317, 40.76437),
            Point.fromLngLat(-73.98235, 40.76799)
        )
    )
));

//Create and add a polygon layer to render the polygon on the map, below the label layer.
map.layers.add(new PolygonLayer(source, 
    fillColor("red"),
    fillOpacity(0.7f)
), "labels");
```

Der folgende Screenshot zeigt den obigen Code, der den Bereich eines Polygons mithilfe einer Polygonebene rendert.

![Polygon mit gerendertem Füllbereich](media/how-to-add-shapes-to-android-map/android-polygon-layer.png)

## <a name="use-a-polygon-and-line-layer-together"></a>Verwenden von Polygon- und Linienebene

Eine Linienebene wird verwendet, um die Kontur von Polygonen zu rendern. Das folgende Codebeispiel rendert ebenso wie das vorherige Beispiel ein Polygon, fügt aber eine Linienebene hinzu. Diese Linienebene ist eine zweite Ebene, die mit der Datenquelle verbunden ist.  

```java
//Create a data source and add it to the map.
DataSource source = new DataSource();
map.sources.add(source);

//Create a rectangular polygon.
source.add(Polygon.fromLngLats(
    Arrays.asList(
        Arrays.asList(
            Point.fromLngLat(-73.98235, 40.76799),
            Point.fromLngLat(-73.95785, 40.80044),
            Point.fromLngLat(-73.94928, 40.79680),
            Point.fromLngLat(-73.97317, 40.76437),
            Point.fromLngLat(-73.98235, 40.76799)
        )
    )
));

//Create and add a polygon layer to render the polygon on the map, below the label layer.
map.layers.add(new PolygonLayer(source,
    fillColor("rgba(0, 200, 200, 0.5)")
), "labels");

//Create and add a line layer to render the outline of the polygon.
map.layers.add(new LineLayer(source,
    strokeColor("red"),
    strokeWidth(2f)
));
```

Der folgende Screenshot zeigt den obigen Code, der ein Polygon mit gerenderter Kontur mithilfe einer Linienebene rendert.

![Polygon, dessen Füllfläche und Kontur gerendert wurden](media/how-to-add-shapes-to-android-map/android-polygon-and-line-layer.png)

> [!TIP]
> Wenn Sie ein Polygon mit einer Linienebene gliedern, achten Sie darauf, alle Ringe in Polygonen zu schließen, sodass jedes Array von Punkten denselben Start- und Endpunkt aufweist. Wenn dies nicht erfolgt, stellt die Linienebene möglicherweise keine Verbindung zwischen dem letzten Punkt des Polygons und dem ersten Punkt her.

## <a name="next-steps"></a>Nächste Schritte

In den folgenden Artikeln finden Sie weitere Codebeispiele, die Sie Ihren Karten hinzufügen können:

> [!div class="nextstepaction"]
> [Erstellen einer Datenquelle](create-data-source-android-sdk.md)

> [!div class="nextstepaction"]
> [Verwenden von datengesteuerten Formatvorlagenausdrücken](data-driven-style-expressions-android-sdk.md)

> [!div class="nextstepaction"]
> [Hinzufügen einer Linienebene](android-map-add-line-layer.md)
