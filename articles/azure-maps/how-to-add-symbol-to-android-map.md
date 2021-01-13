---
title: Hinzufügen einer Symbolebene zu Android-Karten | Microsoft Azure Maps
description: Erfahren Sie, wie Sie einer Karte einen Marker hinzufügen. Sehen Sie sich ein Beispiel an, in dem das Azure Maps Android SDK verwendet wird, um eine Symbolebene hinzuzufügen, die punktbasierte Daten aus einer Datenquelle enthält.
author: rbrundritt
ms.author: richbrun
ms.date: 12/08/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.openlocfilehash: 040fcde35707074ffaf102ed6c224b2f47a084bb
ms.sourcegitcommit: 66b0caafd915544f1c658c131eaf4695daba74c8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/18/2020
ms.locfileid: "97679354"
---
# <a name="add-a-symbol-layer-android-sdk"></a>Hinzufügen einer Symbolebene (Android SDK)

Dieser Artikel zeigt Ihnen, wie Sie Punktdaten aus einer Datenquelle als Symbolebene auf einer Karte mit dem Android SDK für Azure Maps rendern können. Mit Symbolebenen werden Punkte als Bild und Text auf der Karte gerendert.

> [!TIP]
> Symbolebenen rendern in der Standardeinstellung die Koordinaten aller Geometrien in einer Datenquelle. Legen Sie die Option `filter` der Ebene auf `eq(geometryType(), "Point")` fest, um die Ebene so zu beschränken, dass nur Punktgeometriefeatures gerendert werden. Wenn Sie auch MultiPoint-Features einschließen möchten, legen Sie die Option `filter` der Ebene auf `any(eq(geometryType(), "Point"), eq(geometryType(), "MultiPoint"))` fest.

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie sicher, dass Sie die Schritte im Dokument [Schnellstart: Erstellen einer Android-App](quick-android-map.md) ausführen. Codeblöcke in diesem Artikel können in den `onReady`-Ereignishandler von Karten eingefügt werden.

## <a name="add-a-symbol-layer"></a>Hinzufügen einer Symbolebene

Bevor Sie der Karte eine Symbolebene hinzufügen können, müssen Sie einige Schritte ausführen. Erstellen Sie zunächst eine Datenquelle, und fügen Sie sie der Karte hinzu. Erstellen Sie eine Symbolebene. Übergeben Sie dann die Datenquelle an die Symbolebene, um die Daten aus der Datenquelle abzurufen. Abschließend fügen Sie Daten in die Datenquelle ein, damit diese gerendert werden können.

Der folgende Code zeigt, was der Karte hinzugefügt werden soll, nachdem sie geladen wurde. Dieses Beispiel rendert einen einzelnen Punkt auf der Karte mithilfe einer Symbolebene.

```java
//Create a data source and add it to the map.
DataSource source = new DataSource();
map.sources.add(source);

//Create a point and add it to the data source.
source.add(Point.fromLngLat(0, 0));

//Create a symbol layer to render icons and/or text at points on the map.
SymbolLayer layer = new SymbolLayer(source);

//Add the layer to the map.
map.layers.add(layer);
```

Es gibt drei verschiedene Arten von Punktdaten, die der Karte hinzugefügt werden können:

- GeoJSON-Point-Geometrie: Dieses Objekt enthält lediglich eine Koordinate eines Punkts. Mit der statischen `Point.fromLngLat`-Methode können diese Objekte ganz einfach erstellt werden.
- GeoJSON-MultiPoint-Geometrie: Dieses Objekt enthält lediglich die Koordinaten mehrerer Punkte. Übergeben Sie ein Array von Punkten an die `MultiPoint`-Klasse, um diese Objekte zu erstellen.
- GeoJSON-Feature: Dieses Objekt besteht aus einer beliebigen GeoJSON-Geometrie und einer Reihe von Eigenschaften, die Metadaten der Geometrie enthalten.

Weitere Informationen zum Erstellen und Hinzufügen von Daten zur Karte finden Sie im Dokument [Erstellen einer Datenquelle](create-data-source-android-sdk.md).

Mit dem folgenden Codebeispiel wird eine GeoJSON-Punktgeometrie erstellt und an das GeoJSON-Feature übergeben. Außerdem wird den Eigenschaften der `title`-Wert hinzugefügt. Die `title`-Eigenschaft wird über dem Symbol auf der Karte als Text angezeigt.

```java
//Create a data source and add it to the map.
DataSource source = new DataSource();
map.sources.add(source);

//Create a point feature.
Feature feature = Feature.fromGeometry(Point.fromLngLat(0, 0));

//Add a property to the feature.
feature.addStringProperty("title", "Hello World!");

//Add the feature to the data source.
source.add(feature);

//Create a symbol layer to render icons and/or text at points on the map.
SymbolLayer layer = new SymbolLayer(source, 
    //Get the title property of the feature and display it on the map.
    textField(get("title"))
);

//Add the layer to the map.
map.layers.add(layer);
```

Der folgende Screenshot veranschaulicht, wie durch den oben aufgeführten Code unter Verwendung eines Symbols und einer Beschriftung mit einer Symbolebene ein Punktfeature gerendert wird.

![Karte mit gerendertem Punkt. Dabei wird eine Symbolebene zur Darstellung eines Symbols und einer Beschriftung für ein Punktfeature verwendet.](media/how-to-add-symbol-to-android-map/android-map-pin.png)

> [!TIP]
> Standardmäßig optimieren Symbolebenen das Rendering von Symbolen, indem sie überlappende Symbole ausblenden. Beim Zoomen werden die ausgeblendeten Symbole angezeigt. Legen Sie die Optionen `iconAllowOverlap` und `textAllowOverlap` auf `true` fest, um dieses Feature zu deaktivieren und immer alle Symbole zu rendern.

## <a name="add-a-custom-icon-to-a-symbol-layer"></a>Hinzufügen eines benutzerdefinierten Zeichens zu einer Symbolebene

Symbolebenen werden mit WebGL gerendert. Daher müssen alle Ressourcen wie etwa Zeichenbilder in den WebGL-Kontext geladen werden. Dieses Beispiel zeigt, wie Sie ein benutzerdefiniertes Symbol zu den Kartenressourcen hinzufügen können. Dieses Symbol wird dann zum Rendern von Punktdaten mit einem benutzerdefinierten Symbol auf der Karte verwendet. Die `textField`-Eigenschaft der Symbolebene erfordert, dass ein Ausdruck angegeben wird. In diesem Fall möchten wir die temperature-Eigenschaft rendern. Da die Temperatur eine Zahl ist, muss Sie in eine Zeichenfolge konvertiert werden. Außerdem soll „°F“ angefügt werden. Diese Verkettung ist mit folgendem Ausdruck möglich: `concat(Expression.toString(get("temperature")), literal("°F"))`.

```java
//Load a custom icon image into the image sprite of the map.
map.images.add("my-custom-icon", R.drawable.showers);

//Create a data source and add it to the map.
DataSource source = new DataSource();
map.sources.add(source);

//Create a point feature.
Feature feature = Feature.fromGeometry(Point.fromLngLat(-73.985708, 40.75773));

//Add a property to the feature.
feature.addNumberProperty("temperature", 64);

//Add the feature to the data source.
source.add(feature);

//Create a symbol layer to render icons and/or text at points on the map.
SymbolLayer layer = new SymbolLayer(source,
    iconImage("my-custom-icon"),
    iconSize(0.5f),

    //Get the title property of the feature and display it on the map.
    textField(concat(Expression.toString(get("temperature")), literal("°F"))),
    textOffset(new Float[]{0f, -1.5f})
);
```

In diesem Beispiel wurde die folgende Abbildung in den Ordner „drawable“ der App geladen.

| ![Wettersymbolbild für Regenschauer](media/how-to-add-symbol-to-android-map/showers.png)|
|:-----------------------------------------------------------------------:|
| showers.png                                                  |

Der folgende Screenshot veranschaulicht, wie durch den oben aufgeführten Code unter Verwendung eines benutzerdefinierten Symbols und einer formatierten Beschriftung mit einer Symbolebene ein Punktfeature gerendert wird.

![Karte mit gerendertem Punkt. Dabei wird eine Symbolebene zur Darstellung eines benutzerdefinierten Symbols und einer formatierten Beschriftung für ein Punktfeature verwendet.](media/how-to-add-symbol-to-android-map/android-custom-symbol-layer.png)

> [!TIP]
> Wenn Sie nur Text mit einer Symbolebene rendern möchten, können Sie das Symbol ausblenden, indem Sie die `iconImage`-Eigenschaft der Symboloptionen auf `"none"` setzen.

## <a name="next-steps"></a>Nächste Schritte

In den folgenden Artikeln finden Sie weitere Codebeispiele, die Sie Ihren Karten hinzufügen können:

> [!div class="nextstepaction"]
> [Erstellen einer Datenquelle](create-data-source-android-sdk.md)

> [!div class="nextstepaction"]
> [Hinzufügen einer Blasenebene](map-add-bubble-layer-android.md)

> [!div class="nextstepaction"]
> [Verwenden von datengesteuerten Formatvorlagenausdrücken](data-driven-style-expressions-android-sdk.md)

> [!div class="nextstepaction"]
> [Anzeigen von Featureinformationen](display-feature-information-android.md)
