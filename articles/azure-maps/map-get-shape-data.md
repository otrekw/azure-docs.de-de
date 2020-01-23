---
title: Abrufen von Daten aus Formen auf einer Karte | Microsoft Azure Maps
description: In diesem Artikel erfahren Sie, wie Sie mithilfe des Microsoft Azure Maps Web SDK Formendaten abrufen, die auf einer Karte gezeichnet wurden.
author: walsehgal
ms.author: v-musehg
ms.date: 09/04/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: ca1b9854b118d359167873313f1fcb11bfecee52
ms.sourcegitcommit: f9601bbccddfccddb6f577d6febf7b2b12988911
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/12/2020
ms.locfileid: "75911010"
---
# <a name="get-shape-data"></a>Abrufen von Formdaten

In diesem Artikel erfahren Sie, wie Sie mithilfe der **drawingManager.getSource()** -Funktion des [Zeichnungs-Managers](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.drawing.drawingmanager?view=azure-node-latest#getsource--) auf der Karte gezeichnete Formdaten abrufen. Es gibt verschiedene Szenarien, in denen Sie GeoJSON-Daten einer gezeichneten Form extrahieren und an anderer Stelle verwenden möchten.  


## <a name="get-data-from-drawn-shape"></a>Abrufen von Daten aus gezeichneten Formen

Die folgende Funktion ruft die Quelldaten einer gezeichneten Form ab und gibt sie auf dem Bildschirm aus. 

```Javascript
function getDrawnShapes() {
    var source = drawingManager.getSource();

    document.getElementById('CodeOutput').value = JSON.stringify(source.toJson(), null, '    ');
}
```

Nachstehend finden Sie das vollständige, ausführbare Codebeispiel, in dem Sie eine Form zeichnen können, um die Funktion zu testen:

<br/>

<iframe height="686" title="Abrufen von Formdaten" src="//codepen.io/azuremaps/embed/xxKgBVz/?height=265&theme-id=0&default-tab=result" frameborder="no" allowtransparency="true" allowfullscreen="true" style='width: 100%;'>Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/xxKgBVz/'>Get shape data</a> (Abrufen von Formdaten) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>


## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie die weiteren Funktionen des Zeichentools-Moduls verwendet werden:

> [!div class="nextstepaction"]
> [Reagieren auf Zeichnungsereignisse](drawing-tools-events.md)

> [!div class="nextstepaction"]
> [Interaktionstypen und Tastenkombinationen](drawing-tools-interactions-keyboard-shortcuts.md)

Erfahren Sie mehr zu den in diesem Artikel verwendeten Klassen und Methoden:

> [!div class="nextstepaction"]
> [Map](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [Zeichnungs-Manager](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.drawing.drawingmanager?view=azure-node-latest)

> [!div class="nextstepaction"]
> [Zeichnungssymbolleiste](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.control.drawingtoolbar?view=azure-node-latest)
