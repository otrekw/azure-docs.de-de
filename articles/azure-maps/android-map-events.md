---
title: Verarbeiten von Kartenereignissen in Android-Karten | Microsoft Azure Maps
description: Erfahren Sie, welche Ereignisse ausgelöst werden, wenn Benutzer mit Karten interagieren. Zeigen Sie eine Liste aller unterstützten Kartenereignisse an. Hier erfahren Sie, wie Sie mit dem Azure Maps Android SDK Ereignisse verarbeiten können.
author: rbrundritt
ms.author: richbrun
ms.date: 12/08/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.openlocfilehash: 818d33947fa079a130c3009a34dcb9b72ad52f91
ms.sourcegitcommit: 66b0caafd915544f1c658c131eaf4695daba74c8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/18/2020
ms.locfileid: "97681293"
---
# <a name="interact-with-the-map-android-sdk"></a>Interagieren mit der Karte (Android SDK)

In diesem Artikel erfahren Sie, wie Sie den Kartenereignis-Manager verwenden.

## <a name="interact-with-the-map"></a>Interaktion mit der Karte

Die Karte verwaltet alle Ereignisse über ihre `events`-Eigenschaft. In der folgenden Tabelle sind alle unterstützten Kartenereignisse aufgelistet.

| Ereignis                  | Ereignishandlerformat | BESCHREIBUNG |
|------------------------|----------------------|-------------|
| `OnCameraIdle`         | `()`                 | <p>Wird ausgelöst, nachdem der letzte Frame gerendert wurde, jedoch bevor die Karte in den Leerlauf wechselt:<ul><li>Es werden keine Kameraübergänge ausgeführt.</li><li>Alle zurzeit angeforderten Kacheln wurden geladen.</li><li>Alle Ausblend-/Übergangsanimationen wurden abgeschlossen.</li></ul></p> |
| `OnCameraMove`         | `()`                 | Wird während eines animierten Übergangs von einer Ansicht zu einer anderen wiederholt ausgelöst, entweder infolge einer Benutzerinteraktion oder durch Methoden. |
| `OnCameraMoveCanceled` | `()`                 | Wird ausgelöst, wenn eine Bewegungsanforderung an die Kamera abgebrochen wurde. |
| `OnCameraMoveStarted`  | `(int reason)`       | Wird ausgelöst, kurz bevor die Karte den Übergang von einer Ansicht zu einer anderen beginnt, entweder infolge einer Benutzerinteraktion oder durch Methoden. Das Argument `reason` des Ereignislistener gibt einen ganzzahligen Wert zurück, der Aufschluss darüber gibt, wie die Kamerabewegung initiiert wurde. Im Folgenden finden Sie eine Liste möglicher Gründe:<ul><li>1: Geste</li><li>2: Entwickleranimation</li><li>3: API-Animation</li></ul>   |
| `OnClick`              | `(double lat, double lon)` | Wird ausgelöst, wenn die Karte an derselben Stelle gedrückt und wieder losgelassen wird. |
| `OnFeatureClick`       | `(List<Feature>)`    | Wird ausgelöst, wenn die Karte an derselben Stelle eines Features gedrückt und wieder losgelassen wird.  |
| `OnLongClick`          | `(double lat, double lon)` | Wird ausgelöst, wenn die Karte gedrückt wird, für einen Moment gehalten und dann an derselben Stelle auf der Karte wieder losgelassen wird. |
| `OnLongFeatureClick `  | `(List<Feature>)`    | Wird ausgelöst, wenn die Karte gedrückt wird, für einen Moment gehalten und dann an derselben Stelle eines Features losgelassen wird. |
| `OnReady`              | `(AzureMap map)`     | Wird ausgelöst, wenn die Karte anfänglich geladen wird oder wenn sich die App-Ausrichtung ändert und die erforderlichen Kartenressourcen geladen sind und die Karte bereit ist, programmgesteuert interaktiv genutzt zu werden. |

Der folgende Code zeigt, wie die `OnClick`-, `OnFeatureClick`- und `OnCameraMove`-Ereignisse der Karte hinzugefügt werden.

```java
map.events.add((OnClick) (lat, lon) -> {
    //Map clicked.
});

map.events.add((OnFeatureClick) (features) -> {
    //Feature clicked.
});

map.events.add((OnCameraMove) () -> {
    //Map camera moved.
});
```

Weitere Informationen zur Interaktion mit den Karten- und Auslöserereignissen finden Sie in der Dokumentation [Navigieren auf einer Karte](how-to-use-android-map-control-library.md#navigating-the-map).

## <a name="scope-feature-events-to-layer"></a>Beschränken von Featureereignissen auf eine Ebene

Beim Hinzufügen der `OnFeatureClick`- oder `OnLongFeatureClick`-Ereignisse zur Karte kann eine Ebenenkennung als zweiter Parameter übergeben werden. Beim Übergeben einer Ebenenkennung wird das Ereignis nur ausgelöst, wenn das Ereignis auf dieser Ebene auftritt. Auf Ebenen bezogene Ereignisse werden von Symbol-, Blasen-, Linien- und Polygonebenen unterstützt.

```java
//Create a data source.
DataSource source = new DataSource();
map.sources.add(source);

//Add data to the data source.
source.add(Point.fromLngLat(0, 0));

//Create a layer and add it to the map.
BubbleLayer layer = new BubbleLayer(source);
map.layers.add(layer);

//Add a feature click event to the map and pass the layer ID to limit the event to the specified layer.
map.events.add((OnFeatureClick) (features) -> {
    //One or more features clicked.
}, layer.getId());

//Add a long feature click event to the map and pass the layer ID to limit the event to the specified layer.
map.events.add((OnLongFeatureClick) (features) -> {
    //One or more features long clicked.
}, layer.getId());
```

## <a name="next-steps"></a>Nächste Schritte

Die folgenden Artikel enthalten vollständige Codebeispiele:

> [!div class="nextstepaction"]
> [Navigieren auf einer Karte](how-to-use-android-map-control-library.md#navigating-the-map)

> [!div class="nextstepaction"]
> [Hinzufügen einer Symbolebene](how-to-add-symbol-to-android-map.md)

> [!div class="nextstepaction"]
> [Hinzufügen einer Blasenebene](map-add-bubble-layer-android.md)

> [!div class="nextstepaction"]
> [Hinzufügen einer Linienebene](android-map-add-line-layer.md)

> [!div class="nextstepaction"]
> [Hinzufügen einer Polygonebene](how-to-add-shapes-to-android-map.md)
