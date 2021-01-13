---
title: Hinzufügen einer Linienebene zu Android-Karten | Microsoft Azure Maps
description: Erfahren Sie, wie Sie Linien zu Karten hinzufügen. Hier finden Sie Beispiele, in denen das Azure Maps Android SDK verwendet wird, um Linienebenen zu Karten hinzuzufügen und Linien mit Symbolen und Farbverläufen benutzerdefiniert anzupassen.
author: rbrundritt
ms.author: richbrun
ms.date: 12/08/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.openlocfilehash: 3e68be79a4405af103512a9009187857a0d9af39
ms.sourcegitcommit: 66b0caafd915544f1c658c131eaf4695daba74c8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/18/2020
ms.locfileid: "97681307"
---
# <a name="add-a-line-layer-to-the-map-android-sdk"></a>Hinzufügen einer Linienebene zur Karte (Android SDK)

Eine Linienebene kann verwendet werden, um `LineString`- und `MultiLineString`-Features als Pfade oder Routen auf der Karte zu rendern. Eine Linienebene kann auch verwendet werden, um die Kontur eines `Polygon`- und `MultiPolygon`-Features zu rendern. Eine Datenquelle ist mit einer Linienebene verbunden, um die Daten zum Rendern bereitzustellen.

> [!TIP]
> Linienebenen werden standardmäßig sowohl die Koordinaten von Polygonen als auch Linien in einer Datenquelle rendern. Legen Sie die Option `filter` der Ebene auf `eq(geometryType(), "LineString")` fest, um die Ebene so zu beschränken, dass nur LineString-Geometriefeatures gerendert werden. Wenn Sie auch MultiLineString-Features einschließen möchten, legen Sie die Option `filter` der Ebene auf `any(eq(geometryType(), "LineString"), eq(geometryType(), "MultiLineString"))` fest.

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie sicher, dass Sie die Schritte im Dokument [Schnellstart: Erstellen einer Android-App](quick-android-map.md) ausführen. Codeblöcke in diesem Artikel können in den `onReady`-Ereignishandler von Karten eingefügt werden.

## <a name="add-a-line-layer"></a>Hinzufügen einer Linienebene

Der folgende Code zeigt, wie Sie eine Line erstellen. Fügen Sie die Linie zu einer Datenquelle hinzu, und rendern Sie sie dann mit einer Linienebene unter Verwendung der Klasse `LineLayer`.

```java
//Create a data source and add it to the map.
DataSource source = new DataSource();
map.sources.add(source);

//Create a list of points.
List<Point> points = Arrays.asList(
    Point.fromLngLat(-73.972340, 40.743270),
    Point.fromLngLat(-74.004420, 40.756800));

//Create a LineString geometry and add it to the data source.
source.add(LineString.fromLngLats(points));

//Create a line layer and add it to the map.
LineLayer layer = new LineLayer(source,
    strokeColor("blue"),
    strokeWidth(5f)
);

map.layers.add(layer);
```

Der folgende Screenshot veranschaulicht, wie durch den oben aufgeführten Code eine Linie in einer Linienebene gerendert wird.

![Karte mit einer Linie, die unter Verwendung einer Linienebene gerendert wurde](media/android-map-add-line-layer/android-line-layer.png)

## <a name="data-drive-line-style"></a>Datengesteuerte Linienart

Mit dem folgende Code werden zwei Zeilenfeatures erstellt und jeder Zeile ein Geschwindigkeitsbegrenzungswert als Eigenschaft hinzugefügt. Eine Linienebene verwendet einen datengesteuerten Formatvorlagenausdruck, um die Linien basierend auf dem Geschwindigkeitsbegrenzungswert farbig darzustellen. Da die Liniendaten entlang von Straßen überlagert werden, wird durch den folgenden Code die Linienebene unter der Bezeichnungsebene hinzugefügt, sodass die Straßenbezeichnungen weiterhin deutlich zu lesen sind.

```java
//Create a data source and add it to the map.
DataSource source = new DataSource();
map.sources.add(source);

//Create a line feature.
Feature feature = Feature.fromGeometry(
    LineString.fromLngLats(Arrays.asList(
        Point.fromLngLat(-122.131821, 47.704033),
        Point.fromLngLat(-122.099919, 47.703678))));

//Add a property to the feature.
feature.addNumberProperty("speedLimitMph", 35);

//Add the feature to the data source.
source.add(feature);

//Create a second line feature.
Feature feature2 = Feature.fromGeometry(
    LineString.fromLngLats(Arrays.asList(
        Point.fromLngLat(-122.126662, 47.708265),
        Point.fromLngLat(-122.126877, 47.703980))));

//Add a property to the second feature.
feature2.addNumberProperty("speedLimitMph", 15);

//Add the second feature to the data source.
source.add(feature2);

//Create a line layer and add it to the map.
LineLayer layer = new LineLayer(source,
    strokeColor(
        interpolate(
            linear(),
            get("speedLimitMph"),
            stop(0, color(Color.GREEN)),
            stop(30, color(Color.YELLOW)),
            stop(60, color(Color.RED))
        )
    ),
    strokeWidth(5f)
);

map.layers.add(layer, "labels");
```

Der folgende Screenshot zeigt, wie durch den oben aufgeführten Code zwei Linien in einer Linienebene gerendert werden, deren Farbe basierend auf einer Eigenschaft in den Linienfeatures aus einem datengesteuerten Formatvorlagenausdruck abgerufen wird.

![Karte mit Linien, die auf Grundlage einer datengesteuerten Formatvorlage in einer Linienebene gerendert wurden](media/android-map-add-line-layer/android-line-layer-data-drive-style.png)

## <a name="add-symbols-along-a-line"></a>Hinzufügen von Symbolen entlang einer Linie

In diesem Beispiel wird gezeigt, wie Pfeilsymbole entlang einer Linie auf der Karte hinzugefügt werden. Wenn Sie eine Symbolebene verwenden, legen Sie die Option `symbolPlacement` auf `SymbolPlacement.LINE` fest. Diese Option rendert die Symbole entlang der Linie und dreht die Symbole (0 Grad = rechts).

```java
//Create a data source and add it to the map.
DataSource source = new DataSource();
map.sources.add(source);

//Load a image of an arrow into the map image sprite and call it "arrow-icon".
map.images.add("arrow-icon", R.drawable.purple-arrow-right);

//Create and add a line to the data source.
source.add(LineString.fromLngLats(Arrays.asList(
        Point.fromLngLat(-122.18822, 47.63208),
        Point.fromLngLat(-122.18204, 47.63196),
        Point.fromLngLat(-122.17243, 47.62976),
        Point.fromLngLat(-122.16419, 47.63023),
        Point.fromLngLat(-122.15852, 47.62942),
        Point.fromLngLat(-122.15183, 47.62988),
        Point.fromLngLat(-122.14256, 47.63451),
        Point.fromLngLat(-122.13483, 47.64041),
        Point.fromLngLat(-122.13466, 47.64422),
        Point.fromLngLat(-122.13844, 47.65440),
        Point.fromLngLat(-122.13277, 47.66515),
        Point.fromLngLat(-122.12779, 47.66712),
        Point.fromLngLat(-122.11595, 47.66712),
        Point.fromLngLat(-122.11063, 47.66735),
        Point.fromLngLat(-122.10668, 47.67035),
        Point.fromLngLat(-122.10565, 47.67498))));

//Create a line layer and add it to the map.
map.layers.add(new LineLayer(source,
    strokeColor("DarkOrchid"),
    strokeWidth(5f)
));

//Create a symbol layer and add it to the map.
map.layers.add(new SymbolLayer(source,
    //Space symbols out along line.
    symbolPlacement(SymbolPlacement.LINE),

    //Spread the symbols out 100 pixels apart.
    symbolSpacing(100f),
    
    //Use the arrow icon as the symbol.
    iconImage("arrow-icon"),

    //Allow icons to overlap so that they aren't hidden if they collide with other map elements.
    iconAllowOverlap(true),

    //Center the symbol icon.
    iconAnchor(AnchorType.CENTER),

    //Scale the icon size.
    iconSize(0.8f)
));
```

In diesem Beispiel wurde die folgende Abbildung in den Ordner „drawable“ der App geladen.

| ![Lilafarbenes Pfeilsymbol](media/android-map-add-line-layer/purple-arrow-right.png)|
|:-----------------------------------------------------------------------:|
|                                                  |

Der folgende Screenshot zeigt, wie durch den oben aufgeführten Code eine Linie mit Pfeilsymbolen entlang der Linie gerendert werden.

![Karte, die Linien mit Pfeilen enthält, die auf Grundlage einer datengesteuerten Formatvorlage in einer Linienebene gerendert wurden](media/android-map-add-line-layer/android-symbols-along-line-path.png)

## <a name="next-steps"></a>Nächste Schritte

In den folgenden Artikeln finden Sie weitere Codebeispiele, die Sie Ihren Karten hinzufügen können:

> [!div class="nextstepaction"]
> [Erstellen einer Datenquelle](create-data-source-android-sdk.md)

> [!div class="nextstepaction"]
> [Verwenden von datengesteuerten Formatvorlagenausdrücken](data-driven-style-expressions-android-sdk.md)

> [!div class="nextstepaction"]
> [Hinzufügen einer Polygonebene](how-to-add-shapes-to-android-map.md)
