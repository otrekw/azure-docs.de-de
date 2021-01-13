---
title: Anzeigen von Featureinformationen in Android-Karten | Microsoft Azure Maps
description: Hier erfahren Sie, wie Sie Informationen anzeigen, wenn Benutzer mit Kartenfunktionen interagieren. Verwenden Sie das Azure Maps Android SDK, um Popupmeldungen und andere Arten von Meldungen anzuzeigen.
author: rbrundritt
ms.author: richbrun
ms.date: 08/08/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.openlocfilehash: 37b5ab1c144ed81d995da40b87edeaccdcad7253
ms.sourcegitcommit: 66b0caafd915544f1c658c131eaf4695daba74c8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/18/2020
ms.locfileid: "97679986"
---
# <a name="display-feature-information"></a>Anzeigen von Featureinformationen

Räumliche Daten werden häufig mithilfe von Punkten, Linien und Polygonen dargestellt. Diesen Daten sind häufig Metadateninformationen zugeordnet. Ein Punkt kann beispielsweise den Standort eines Restaurants darstellen, und die Metadaten zu diesem Restaurant können den Namen, die Adresse und die Art des Speisenangebots umfassen. Diese Metadaten können als Eigenschaften eines GeoJSON-Features (`Feature`) hinzugefügt werden. Der folgende Code erstellt ein einfaches Punktmerkmal mit einer Eigenschaft vom Typ `title` und dem Wert „Hello World!“:

```java
//Create a data source and add it to the map.
DataSource source = new DataSource();
map.sources.add(source);

//Create a point feature.
Feature feature = Feature.fromGeometry(Point.fromLngLat(-122.33, 47.64));

//Add a property to the feature.
feature.addStringProperty("title", "Hello World!");

//Create a point feature, pass in the metadata properties, and add it to the data source.
source.add(feature);
```

Informationen zu den Möglichkeiten zum Erstellen und Hinzufügen von Daten zur Karte finden Sie in der Dokumentation [Erstellen einer Datenquelle](create-data-source-android-sdk.md).

Wenn ein Benutzer mit einem Merkmal auf der Karte interagiert, kann mithilfe von Ereignissen auf diese Aktionen reagiert werden. Ein gängiges Szenario ist das Anzeigen einer Nachricht, die sich aus den Metadateneigenschaften eines Merkmals zusammensetzt, mit dem der Benutzer interagiert hat. Das Ereignis `OnFeatureClick` ist das Hauptereignis, das verwendet wird, um zu erkennen, wann der Benutzer auf ein Merkmal auf der Karte getippt hat. Es gibt auch ein Ereignis vom Typ `OnLongFeatureClick`. Wenn der Karte das Ereignis `OnFeatureClick` hinzugefügt wird, kann es auf eine einzelne Ebene begrenzt werden, indem die ID einer Ebene übergeben wird, auf die es begrenzt werden soll. Wird keine Ebenen-ID übergeben, wird das Ereignis ausgelöst, wenn auf ein beliebiges Merkmal auf der Karte getippt wird (unabhängig von der Ebene, auf der sich das Merkmal befindet). Der folgende Code erstellt eine Symbolebene zum Rendern von Punktdaten auf der Karte. Anschließend wird ein Ereignis vom Typ `OnFeatureClick` hinzugefügt und auf diese Symbolebene beschränkt.

```java
//Create a symbol and add it to the map.
SymbolLayer layer = new SymbolLayer(source);
map.layers.add(layer);

//Add a feature click event to the map.
map.events.add((OnFeatureClick) (features) -> {
    //Retrieve the title property of the feature as a string.
    String msg = features.get(0).getStringProperty("title");

    //Do something with the message.
}, layer.getId());    //Limit this event to the symbol layer.
```

## <a name="display-a-toast-message"></a>Anzeigen einer Popupmeldung

Eine Popupmeldung ist eine der einfachsten Methoden, um dem Benutzer Informationen anzuzeigen, und steht in allen Versionen von Android zur Verfügung. Sie unterstützt keinerlei Benutzereingaben und wird nur kurz angezeigt. Wenn Sie den Benutzer schnell über etwas informieren möchten, worauf er getippt hat, ist eine Popupmeldung ggf. eine gute Option. Der folgende Code zeigt, wie eine Popupmeldung mit dem Ereignis `OnFeatureClick` verwendet werden kann:

```java
//Add a feature click event to the map.
map.events.add((OnFeatureClick) (features) -> {
    //Retrieve the title property of the feature as a string.
    String msg = features.get(0).getStringProperty("title");

    //Display a toast message with the title information.
    Toast.makeText(this, msg, Toast.LENGTH_SHORT).show();
}, layer.getId());    //Limit this event to the symbol layer.
```

![Animation: Tippen auf ein Merkmal und Anzeigen einer Popupmeldung](./media/display-feature-information-android/symbol-layer-click-toast-message.gif)

Neben Popupmeldungen gibt es aber auch noch verschiedene andere Möglichkeiten, um die Metadateneigenschaften eines Merkmals zu präsentieren:

- [Snackbar-Widget:](https://developer.android.com/training/snackbar/showing.html) `Snackbars` liefern einfaches Feedback zu einem Vorgang. Sie zeigen eine kurze Meldung am unteren Bildschirmrand (Mobilgeräte) bzw. links unten (größere Geräte) an. `Snackbars` werden im Vordergrund über allen anderen Bildschirmelementen eingeblendet, und es kann immer nur eine einzelne Snackbar angezeigt werden.
- [Dialogfelder:](https://developer.android.com/guide/topics/ui/dialogs) Ein Dialogfeld ist ein kleines Fenster, in dem der Benutzer zu einer Entscheidung oder zur Eingabe zusätzlicher Informationen aufgefordert wird. Ein Dialogfeld füllt nicht den gesamten Bildschirm aus und wird in der Regel für modale Ereignisse verwendet, bei denen der Benutzer eine Aktion ausführen muss, damit ein Vorgang fortgesetzt wird.
- Fügen Sie der aktuellen Aktivität ein [Fragment](https://developer.android.com/guide/components/fragments) hinzu.
- Navigieren Sie zu einer anderen Aktivität oder Ansicht.

## <a name="next-steps"></a>Nächste Schritte

So fügen Sie Ihrer Karte weitere Daten hinzu:

> [!div class="nextstepaction"]
> [Reagieren auf Kartenereignisse](android-map-events.md)

> [!div class="nextstepaction"]
> [Erstellen einer Datenquelle](create-data-source-android-sdk.md)

> [!div class="nextstepaction"]
> [Hinzufügen einer Symbolebene](how-to-add-symbol-to-android-map.md)

> [!div class="nextstepaction"]
> [Hinzufügen einer Blasenebene](map-add-bubble-layer-android.md)

> [!div class="nextstepaction"]
> [Hinzufügen einer Linienebene](android-map-add-line-layer.md)

> [!div class="nextstepaction"]
> [Hinzufügen einer Polygonebene](how-to-add-shapes-to-android-map.md)
