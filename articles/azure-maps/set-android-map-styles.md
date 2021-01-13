---
title: Festlegen eines Kartenstils in Android-Karten | Microsoft Azure Maps
description: Erlernen Sie zwei Möglichkeiten, den Stil einer Karte festzulegen. Erfahren Sie, wie Sie den Stil mit dem Azure Maps Android SDK in der Layoutdatei oder der „Activity“-Klasse anpassen.
author: rbrundritt
ms.author: richbrun
ms.date: 04/26/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.openlocfilehash: 1cce355c8ffbcd4704bd32b0e4d1739c77c2b623
ms.sourcegitcommit: 66b0caafd915544f1c658c131eaf4695daba74c8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/18/2020
ms.locfileid: "97678474"
---
# <a name="set-map-style-android-sdk"></a>Festlegen eines Kartenstils (Android SDK)

In diesem Artikel werden Ihnen zwei Möglichkeiten gezeigt, Kartenstile mit dem Android SDK von Azure Maps festzulegen. In Azure Maps stehen sechs Kartenstile zur Auswahl. Weitere Informationen zu unterstützten Kartenstilen finden Sie unter [Unterstützte Kartenstile in Azure Maps](supported-map-styles.md).

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie sicher, dass Sie die Schritte im Dokument [Schnellstart: Erstellen einer Android-App](quick-android-map.md) ausführen.

## <a name="set-map-style-in-the-layout"></a>Festlegen des Kartenstils im Layout

Sie können einen Kartenstil In der Layoutdatei für Ihre Activity-Klasse festlegen, wenn Sie das Kartensteuerelement hinzufügen. Im folgenden Code werden der Mittelpunkt, der Zoomfaktor und der Kartenstil festgelegt.

```XML
<com.microsoft.azure.maps.mapcontrol.MapControl
    android:id="@+id/mapcontrol"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    app:mapcontrol_centerLat="47.602806"
    app:mapcontrol_centerLng="-122.329330"
    app:mapcontrol_zoom="12"
    app:mapcontrol_style="grayscale_dark"
    />
```

Der folgende Screenshot veranschaulicht, wie durch den oben aufgeführten Code eine Straßenkarte mit dem Stil „Graustufen dunkel“ dargestellt wird.

![Straßenkarte mit dem Stil „Graustufen dunkel“](media/set-android-map-styles/android-grayscale-dark.png)

## <a name="set-map-style-in-code"></a>Festlegen des Kartenstils im Code

Der Kartenstil kann mithilfe der `setStyle`-Methode der Karte programmgesteuert im Code festgelegt werden. Mit dem folgenden Code werden der Mittelpunkt und Zoomfaktor der Karte mithilfe der `setCamera`-Methode festgelegt. Außerdem wird der Kartenstil auf `SATELLITE_ROAD_LABELS` festgelegt.

```java
mapControl.onReady(map -> {

    //Set the camera of the map.
    map.setCamera(center(Point.fromLngLat(-122.33, 47.64)), zoom(14));

    //Set the style of the map.
    map.setStyle(style(MapStyle.SATELLITE_ROAD_LABELS));
});
```

Der folgende Screenshot veranschaulicht, wie durch den oben aufgeführten Code eine Karte mit dem Stil „Straßenbezeichnungen in Satellitenansicht“ dargestellt wird.

![Karte mit dem Stil „Straßenbezeichnungen in Satellitenansicht“](media/set-android-map-styles/android-satellite-road-labels.png)

## <a name="setting-the-map-camera"></a>Festlegen der Kartenkamera

Mit der Kartenkamera wird gesteuert, welcher Teil der Karte in der Karte angezeigt wird. Die Kamera kann im Layout oder programmgesteuert im Code festgelegt werden. Beim Festlegen im Code gibt es zwei Hauptmethoden zum Festlegen der Kartenposition: Sie verwenden „center“ und „zoom“, oder Sie übergeben einen Begrenzungsrahmen. Der folgende Code zeigt, wie Sie bei Verwendung von `center` und `zoom` alle optionalen Kameraoptionen festlegen.

```java
//Set the camera of the map using center and zoom.
map.setCamera(
    center(Point.fromLngLat(-122.33, 47.64)), 

    //The zoom level. Typically a value between 0 and 22.
    zoom(14),

    //The amount of tilt in degrees the map where 0 is looking straight down.
    pitch(45),

    //Direction the top of the map is pointing in degrees. 0 = North, 90 = East, 180 = South, 270 = West
    bearing(90),

    //The minimum zoom level the map will zoom-out to when animating from one location to another on the map.
    minZoom(10),
    
    //The maximium zoom level the map will zoom-in to when animating from one location to another on the map.
    maxZoom(14)
);
```

Häufig ist es wünschenswert, den Fokus der Karte auf einen Satz von Daten zu richten. Ein Begrenzungsrahmen kann mithilfe der `MapMath.fromData`-Methode aus Features berechnet und an die Option `bounds` der Kartenkamera übergeben werden. Beim Festlegen einer Kartenansicht, die auf einem Begrenzungsrahmen basiert, ist es häufig sinnvoll, einen `padding`-Wert anzugeben, um die Pixelgröße der Punkte zu berücksichtigen, die als Blasen oder Symbole gerendert werden. Der folgende Code zeigt, wie Sie alle optionalen Kameraoptionen festlegen, wenn Sie einen Begrenzungsrahmen zum Festlegen der Kameraposition verwenden.

```java
//Set the camera of the map using a bounding box.
map.setCamera(
    //The area to focus the map on.
    bounds(BoundingBox.fromLngLats(
        //West
        -122.4594,

        //South
        47.4333,
        
        //East
        -122.21866,
        
        //North
        47.75758
    )),

    //Amount of pixel buffer around the bounding box to provide extra space around the bounding box.
    padding(20),

    //The maximium zoom level the map will zoom-in to when animating from one location to another on the map.
    maxZoom(14)
);
```

Beachten Sie, dass das Seitenverhältnis eines Begrenzungsrahmens u. U. nicht mit dem Seitenverhältnis der Karte identisch ist, da auf der Karte häufig der vollständige Bereich des Begrenzungsrahmens zu sehen ist, während sie häufig auf einen kleineren vertikalen oder horizontalen Ausschnitt beschränkt sein kann.

## <a name="next-steps"></a>Nächste Schritte

In den folgenden Artikeln finden Sie weitere Codebeispiele, die Sie Ihren Karten hinzufügen können:

> [!div class="nextstepaction"]
> [Hinzufügen einer Symbolebene](how-to-add-symbol-to-android-map.md)

> [!div class="nextstepaction"]
> [Hinzufügen einer Blasenebene](map-add-bubble-layer-android.md)
