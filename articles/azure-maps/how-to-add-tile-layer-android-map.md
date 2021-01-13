---
title: Hinzufügen einer Kachelebene zu Android-Karten | Microsoft Azure Maps
description: Erfahren Sie, wie Sie einer Karte eine Kachelebene hinzufügen. Sehen Sie sich ein Beispiel an, in dem das Azure Maps Android SDK verwendet wird, um einer Karte eine Wetterradarüberlagerung hinzuzufügen.
author: rbrundritt
ms.author: richbrun
ms.date: 12/08/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.openlocfilehash: 8ea6f44c47c5cd4d223b053640f65827f46db482
ms.sourcegitcommit: 66b0caafd915544f1c658c131eaf4695daba74c8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/18/2020
ms.locfileid: "97679297"
---
# <a name="add-a-tile-layer-to-a-map-android-sdk"></a>Hinzufügen einer Kachelebene zu einer Karte (Android SDK)

In diesem Artikel wird veranschaulicht, wie Sie mit dem Android SDK für Azure Maps eine Kachelebene auf einer Karte rendern können. Mithilfe von Kachelebenen lassen sich Bilder über die Azure Maps-Basiskartenkacheln legen. Weitere Informationen zum Azure Maps-Kachelsystem finden Sie in der Dokumentation [Zoomfaktoren und Linienraster](zoom-levels-and-tile-grid.md).

Eine Kachelebene wird in Kacheln von einem Server geladen. Diese Bilder können wie jedes andere Bild vorab gerendert und auf einem Server gespeichert werden, wobei eine Namenskonvention verwendet wird, die von der Kachelebene verstanden wird. Sie können diese Bilder aber auch mit einem dynamischen Dienst rendern, der die Bilder nahezu in Echtzeit generiert. Es gibt drei verschiedene Namenskonventionen für Kacheldienste, die von der Azure Maps-Klasse TileLayer unterstützt werden:

* Notation von X, Y, Zoomfaktor: Basierend auf dem Zoomfaktor ist X die Spalten- und Y die Zeilenposition der Kachel im Kachelraster.
* Quadkey-Notation: Kombination der Informationen X, Y und Zoomfaktor in einem einzelnen Zeichenfolgenwert, der ein eindeutiger Bezeichner für eine Kachel ist.
* Begrenzungsrahmen: Koordinaten des Begrenzungsrahmens können verwendet werden, um ein Bild im Format `{west},{south},{east},{north}` anzugeben, welches häufig von [Web Mapping Services (WMS)](https://www.opengeospatial.org/standards/wms) verwendet wird.

> [!TIP]
> Ein TileLayer-Element (Kachelebene) ist eine gute Möglichkeit, große Datasets auf der Karte zu visualisieren. Eine Kachelebene lässt sich nicht nur aus einem Bild generieren, sondern auch Vektordaten können als Kachelebene gerendert werden. Durch das Rendern von Vektordaten als Kachelebene muss das Kartensteuerelement nur die Kacheln laden. Diese können eine viel kleinere Dateigröße aufweisen als die Vektordaten, die sie darstellen. Diese Technik wird von vielen verwendet, die Millionen von Datenzeilen auf der Karte rendern müssen.

Die in eine Kachelebene übergebene Kachel-URL muss eine HTTP/HTTPS-URL zu einer TileJSON-Ressource oder eine Kachel-URL-Vorlage sein, die die folgenden Parameter verwendet: 

* `{x}`: X-Position der Kachel. Benötigt auch `{y}` und `{z}`.
* `{y}`: Y-Position der Kachel. Benötigt auch `{x}` und `{z}`.
* `{z}`: Zoomfaktor der Kachel. Benötigt auch `{x}` und `{y}`.
* `{quadkey}`: Kachel-Quadkey-Bezeichner basierend auf der Namenskonvention des Bing Maps-Kachelsystems.
* `{bbox-epsg-3857}`: Eine Begrenzungsrahmen-Zeichenfolge mit dem Format `{west},{south},{east},{north}` im Raumbezugssystem EPSG 3857.
* `{subdomain}`: Ein Platzhalter für die Unterdomänenwerte, sofern angegeben.

## <a name="prerequisites"></a>Voraussetzungen

Um den Vorgang in diesem Artikel abzuschließen, müssen Sie das [Android SDK für Azure Maps](how-to-use-android-map-control-library.md) installieren, um eine Karte zu laden.

## <a name="add-a-tile-layer-to-the-map"></a>Hinzufügen einer Kachelebene zur Karte

In diesem Beispiel wird das Erstellen einer Kachelebene veranschaulicht, die auf mehrere Kacheln verweisen. In diesem Beispiel wird das Kachelsystem „X, Y, Zoom“ verwendet. Die Quelle dieser Kachelebene stellt das [OpenSeaMap-Projekt](https://openseamap.org/index.php) dar, das per Crowdsourcing erworbene Seekarten enthält. Bei der Anzeige von Kachelebenen ist es oft wünschenswert, dass die Bezeichnungen der Städte auf der Karte deutlich zu sehen sind. Dieses Verhalten wird erreicht, indem die Kachelebene unter den Kartenbezeichnungsebenen eingefügt wird.

```java
TileLayer layer = new TileLayer(
    tileUrl("https://tiles.openseamap.org/seamark/{z}/{x}/{y}.png"),
    opacity(0.8f),
    tileSize(256),
    minSourceZoom(7),
    maxSourceZoom(17)
);

map.layers.add(layer, "labels");
```

Der folgende Screenshot zeigt, wie durch den oben aufgeführten Code eine Kachelebene mit nautischen Informationen auf einer Karte mit dem Stil „Graustufen dunkel“ angezeigt wird.

![Android-Karte mit einer Kachelebene](media/how-to-add-tile-layer-android-map/xyz-tile-layer-android.png)

## <a name="next-steps"></a>Nächste Schritte

Im folgenden Artikel erfahren Sie mehr zum Festlegen von Kartenstilen.

> [!div class="nextstepaction"]
> [Ändern des Kartenstils](set-android-map-styles.md)

> [!div class="nextstepaction"]
> [Hinzufügen eines Wärmebilds](map-add-heat-map-layer-android.md)
