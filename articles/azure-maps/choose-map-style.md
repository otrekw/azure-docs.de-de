---
title: Ändern des Kartenstils in Azure Maps | Microsoft Azure Maps
description: In diesem Artikel erfahren Sie mehr über Stilfunktionen, die im Microsoft Azure Maps Web SDK verfügbar sind.
author: philmea
ms.author: philmea
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: b1c5d9b5cd2b6b9bfecf8a0af79699061003eec1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80335684"
---
# <a name="change-the-style-of-the-map"></a>Ändern des Kartenstils

Die Karte unterstützt mehrere verschiedene [Stiloptionen](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.styleoptions), die beim Initialisieren der Karte oder später mithilfe der `setStyle`-Funktion der Karte festgelegt werden können. In diesem Artikel wird gezeigt, wie diese Stiloptionen verwendet werden, um die Kartendarstellung anzupassen. Erfahren Sie, wie Sie beim Laden einer Karte einen Stil festlegen, und erfahren Sie, wie Sie mithilfe des Stilauswahl-Steuerelements einen neuen Kartenstil festlegen.

## <a name="set-the-style-options"></a>Festlegen der Stiloptionen 

Stiloptionen können der Karte zum Zeitpunkt der Initialisierung übergeben oder später mithilfe der `setStyle`-Funktion der Karte aktualisiert werden.

```javascript
//Set the style options when creating the map.
var map = new atlas.Map('map', {
    renderWorldCopies: false,
    showBuildingModels: true

    //Additional map options.
};

//Update the style options at anytime using setStyle function.
map.setStyle({
    renderWorldCopies: true,
    showBuildingModels: false
});
```

Das folgende Tool zeigt, wie die verschiedenen Stiloptionen die Kartendarstellung ändern. Um die 3D-Gebäude anzuzeigen, zoomen Sie stark auf eine größere Stadt herein. 

<br/>

<iframe height="700" style="width: 100%;" scrolling="no" title="Kartenstiloptionen" src="https://codepen.io/azuremaps/embed/eYNMjPb?height=700&theme-id=0&default-tab=result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/eYNMjPb'>Map style options</a> (Kartenstiloptionen) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="choose-a-base-map-style"></a>Auswählen eines grundlegenden Kartenstils

Eine der gängigsten Kartenstiloptionen wird verwendet, um den Stil der zugrundeliegenden, zu formatierenden Karte zu ändern. Viele der [unterstützten Kartenstile in Azure Maps](supported-map-styles.md) sind im Web SDK verfügbar. 

### <a name="set-base-map-style-on-map-load"></a>Festlegen des grundlegenden Kartenstils beim Laden einer Karte


Der Kartenstil kann beim Initialisieren der Karte durch Festlegen der Option `style` angegeben werden. Im folgenden Code wird die Option `style` der Karte bei der Initialisierung auf `grayscale_dark` festgelegt:

```javascript
var map = new atlas.Map('map', {
    style: 'grayscale_dark',

    //Additiona map options
);
```

<br/>

<iframe height='500' scrolling='no' title='Festlegen des Stils für eine geladene Karte' src='//codepen.io/azuremaps/embed/WKOQRq/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter <a href='https://codepen.io/azuremaps/pen/WKOQRq/'>Setting the style on map load</a> A PEN BY Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf der <a href='https://codepen.io'>CodePen-Website</a>.
</iframe>

### <a name="update-the-base-map-style"></a>Aktualisieren des grundlegenden Kartenstils

 Der Kartenstil kann mithilfe der `setStyle`-Funktion und durch Festlegen der `style`-Option auf den gewünschten Kartenstil aktualisiert werden.

```javascript
map.setStyle({ style: 'satellite' });
```

Im folgenden Code wird nach dem Laden einer Karteninstanz der Kartenstil mithilfe der Funktion [setStyle](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-maps-typescript-latest#setstyle-styleoptions-) von `road` auf `satellite` aktualisiert.

<br/>

<iframe height='500' scrolling='no' title='Aktualisieren des Stils' src='//codepen.io/azuremaps/embed/yqXYzY/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter <a href='https://codepen.io/azuremaps/pen/yqXYzY/'>Updating the style</a> A PEN BY Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf der <a href='https://codepen.io'>CodePen-Website</a>.
</iframe>

### <a name="add-the-style-picker"></a>Hinzufügen der Stilauswahl

Das Stilauswahl-Steuerelement bietet eine komfortable Schaltfläche mit Flyoutbereich, die vom Endbenutzer verwendet werden kann, um den Kartenstil zu ändern. Die Stilauswahl weist zwei verschiedene Layoutoptionen auf. Standardmäßig verwendet die Stilauswahl das `icons`-Layout und zeigt den Kartenstil als horizontale Reihe von Symbolen an. 

<center>

![Symbollayout in der Stilauswahl](media/choose-map-style/style-picker-icon-layout.png)</center>

Die zweite Layoutoption wird als `list` bezeichnet und zeigt eine scrollbare Liste der Kartenstile an.  

<center>

![Listenlayout in der Stilauswahl](media/choose-map-style/style-picker-list-layout.png)</center>


Der folgende Code zeigt, wie eine Instanz des Stilauswahl-Steuerelements erstellt und der oberen rechten Ecke der Karte hinzugefügt wird. Die Stilauswahl ist auf einen dunklen Stil und auf die Darstellung einiger ausgewählter Kartenstile mithilfe der Listenebene eingestellt.

```javascript
map.controls.add(new atlas.control.StyleControl({
    mapStyles: ['road', 'night', 'grayscale_dark', 'grayscale_light'],
    layout: 'list',
    style: 'dark'
}), {
    position: 'top-right'
}); 
```

Der folgende Code fügt der Karte ein Stilauswahl-Steuerelement mit Standardeinstellungen hinzu, sodass der Benutzer komfortabel zwischen den verschiedenen Kartenstilen umschalten kann. Schalten Sie den Kartenstil mithilfe des Kartenstil-Steuerelements in der Nähe der oberen rechten Ecke um.

<br/>

<iframe height='500' scrolling='no' title='Hinzufügen der Stilauswahl' src='//codepen.io/azuremaps/embed/OwgyvG/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter <a href='https://codepen.io/azuremaps/pen/OwgyvG/'>Adding the style picker</a> A PEN BY Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf der <a href='https://codepen.io'>CodePen-Website</a>.
</iframe>

> [!TIP]
> Standardmäßig listet das Steuerelement für die Stilauswahl alle verfügbaren Stile auf, wenn der S0-Tarif von Azure Maps verwendet wird. Soll die Liste weniger Stile enthalten, können Sie an die Option `mapStyle` der Stilauswahl ein Array mit den Stilen übergeben, die in der Liste angezeigt werden sollen. Wenn Sie S1 verwenden und alle verfügbaren Stile anzeigen möchten, legen Sie die Option `mapStyles` der Stilauswahl auf `"all"` fest.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu den in diesem Artikel verwendeten Klassen und Methoden:

> [!div class="nextstepaction"]
> [Map](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map)

> [!div class="nextstepaction"]
> [StyleOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.styleoptions)

> [!div class="nextstepaction"]
> [StyleControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.stylecontrol)

> [!div class="nextstepaction"]
> [StyleControlOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.stylecontroloptions)

Fügen Sie Ihren Karten Steuerelemente hinzu:

> [!div class="nextstepaction"]
> [Hinzufügen von Kartensteuerelementen](map-add-controls.md)

> [!div class="nextstepaction"]
> [Hinzufügen von Stecknadeln zur Karte](map-add-pin.md)
