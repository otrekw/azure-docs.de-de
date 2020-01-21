---
title: 'Tutorial: Migrieren einer Web-App aus Google Maps | Microsoft Azure Maps'
description: Hier erfahren Sie, wie Sie eine Web-App aus Google Maps zu Microsoft Azure Maps migrieren.
author: rbrundritt
ms.author: richbrun
ms.date: 12/17/2019
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.custom: ''
ms.openlocfilehash: 08566283181a4bb15f77016834c4dc0dffc184b7
ms.sourcegitcommit: f9601bbccddfccddb6f577d6febf7b2b12988911
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/12/2020
ms.locfileid: "75910867"
---
# <a name="migrate-a-web-app-from-google-maps"></a>Migrieren einer Web-App aus Google Maps

Die meisten Web-Apps, die Google Maps verwenden, verwenden das Google Maps V3 JavaScript SDK. Das Azure Maps Web SDK ist das passende Azure-basierte SDK, zu dem migriert werden kann. Mit dem Azure Maps Web SDK können Sie interaktive Karten mit eigenen Inhalten und Bildern anpassen, die in Ihren webbasierten oder mobilen Anwendungen angezeigt werden sollen. Dieses Steuerelement nutzt WebGL, was das Rendern umfangreicher Datasets mit hoher Leistung ermöglicht. Verwenden Sie für die Entwicklung mit dem SDK JavaScript oder TypeScript.

Wenn Sie eine vorhandene Webanwendung migrieren, überprüfen Sie, ob sie eine Open Source-Kartensteuerelement-Bibliothek wie Cesium, Leaflet oder OpenLayers verwendet. Wenn dies der Fall ist und Sie das Azure Maps Web SDK nicht verwenden möchten, besteht eine weitere Option zum Migrieren der Anwendung darin, das Open Source-Kartensteuerelement weiterhin zu verwenden und es mit den Kacheldiensten von Azure Maps zu verbinden ([Straßenkacheln](https://docs.microsoft.com/rest/api/maps/render/getmaptile) \| [Satellitenkacheln](https://docs.microsoft.com/rest/api/maps/render/getmapimagerytile)). Nachfolgend finden Sie ausführliche Informationen zur Verwendung von Azure Maps in einigen häufig verwendeten Open Source-Kartensteuerelement-Bibliotheken.

- Cesium: ein 3D-Kartensteuerelement für das Web. [Codebeispiel](https://azuremapscodesamples.azurewebsites.net/index.html?sample=Raster%20Tiles%20in%20Cesium%20JS) \| [Dokumentation](https://cesiumjs.org/)
- Leaflet: schlankes 2D-Kartensteuerelement für das Web. [Codebeispiel](https://azuremapscodesamples.azurewebsites.net/index.html?sample=Azure%20Maps%20Raster%20Tiles%20in%20Leaflet%20JS) \| [Dokumentation](https://leafletjs.com/)
- OpenLayers: ein 2D-Kartensteuerelement für das Web, das Projektionen unterstützt. [Codebeispiel](https://azuremapscodesamples.azurewebsites.net/index.html?sample=Raster%20Tiles%20in%20OpenLayers) \| [Dokumentation](https://openlayers.org/)

## <a name="key-features-support"></a>Unterstützung wichtiger Funktionen

In der folgenden Tabelle sind die wichtigsten API-Funktionen im Google Maps V3 JavaScript SDK und die Unterstützung einer ähnlichen API im Azure Maps Web SDK aufgeführt.

| Google Maps-Funktion     | Azure Maps Web SDK-Unterstützung |
|-------------------------|:--------------------------:|
| Marker                 | ✓                          |
| Markerclustering       | ✓                          |
| Polylinien und Polygone    | ✓                          |
| Datenebenen             | ✓                          |
| Bodenüberlagerungen         | ✓                          |
| Wärmebilder               | ✓                          |
| Kachelebenen             | ✓                          |
| KML-Ebene               | ✓                          |
| Zeichentools           | ✓                          |
| Geocoder-Dienst        | ✓                          |
| Wegbeschreibungsdienst      | ✓                          |
| Entfernungsmatrixdienst | ✓                          |
| Höhenangabendienst       | Geplant                    |

## <a name="notable-differences-in-the-web-sdks"></a>Wichtige Unterschiede in den Web-SDKs

Nachfolgend finden Sie einige der wichtigsten Unterschiede zwischen den Google Maps und Azure Maps Web SDKs, die Sie beachten sollten:

- Zusätzlich zur Bereitstellung eines gehosteten Endpunkts für den Zugriff auf das Azure Maps Web SDK hinaus steht außerdem ein NPM-Paket zum Einbetten des Web SDKs in Apps zur Verfügung. Weitere Informationen finden Sie in dieser [Dokumentation](how-to-use-map-control.md). Dieses Paket enthält außerdem TypeScript-Definitionen.
- Nachdem Sie eine Instanz der Map-Klasse in Azure Maps erstellt haben, sollte der Code warten, bis das Kartenereignis `ready` oder `load` ausgelöst wird, bevor er mit der Zuordnung interagiert. Dadurch wird sichergestellt, dass alle Kartenressourcen geladen wurden und für den Zugriff bereit sind.
- Beide Plattformen verwenden ein ähnliches System der Anordnung für die Basiskarten, jedoch messen die Kacheln in Google Maps 256 Pixel, während die Kacheln in Azure Maps 512 Pixel groß sind. Daher muss, um in Azure Maps die gleiche Kartenansicht wie in Google Maps zu erreichen, eine in Google Maps verwendete Zoomstufe von der Zoomstufe in Azure Maps subtrahiert werden.
- Koordinaten werden in Google Maps als "Breitengrad, Längengrad" angegeben, während Azure Maps "Längengrad, Breitengrad" verwendet. Dies richtet sich nach dem Standard `[x, y]`, dem die meisten GIS-Plattformen folgen.
- Formen im Azure Maps Web-SDK basieren auf dem GeoJSON-Schema. Hilfsklassen werden über den [*atlas.data*-Namespace](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data?view=azure-iot-typescript-latest) verfügbar gemacht. Es gibt ferner die [*atlas.Shape*](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape)-Klasse, die zum Umschließen von GeoJSON-Objekten verwendet werden kann, damit sie sich leicht in einer Weise aktualisieren und warten lassen, die Datenbindungen zulässt.
- Koordinaten sind in Azure Maps als Position-Objekte definiert, die als einfaches Zahlenarray im Format `[longitude, latitude]` oder als neue atlas.data.Position(Längengrad, Breitengrad) angegeben werden können.
    > [!TIP]
    > Die Position-Klasse weist eine statische Hilfsmethode zum Importieren von Koordinaten im Format „Breitengrad, Längengrad“ auf. Die Methode [atlas.data.Position.fromLatLng](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.position?view=azure-iot-typescript-latest) kann die Methode `new google.maps.LatLng` in Google Maps-Code in vielen Fällen ersetzen.
- Anstatt Formatierungsinformationen für jede Form anzugeben, die der Karte hinzugefügt wird, trennt Azure Maps Formatvorlagen von den Daten. Daten werden in Datenquellen gespeichert und sind mit Renderingebenen verbunden, die vom Azure-Code für das Rendern von Daten verwendet werden. Dieser Ansatz bietet den Vorteil besserer Leistung. Darüber hinaus unterstützen viele Ebenen datengesteuerte Formatierungen, bei denen den Optionen der Ebenenformatvorlage Geschäftslogik hinzugefügt werden kann, die das Rendering einzelner Formen auf einer Ebene auf der Grundlage der in der Form definierten Eigenschaften ändert.

## <a name="web-sdk-side-by-side-examples"></a>Beispiele zum Web SDK in der Gegenüberstellung

Nachfolgend finden Sie eine Zusammenstellung von Codebeispielen für jede Plattform, die häufige Anwendungsfälle abdecken und Ihnen die Migration Ihrer Webanwendung vom Google Maps V3 JavaScript SDK zum Azure Maps Web SDK erleichtern sollen. Codebeispiele für Webanwendungen werden in JavaScript zur Verfügung gestellt; Azure Maps bietet darüber hinaus über ein [NPM-Modul](how-to-use-map-control.md) aber außerdem TypeScript-Definitionen.

### <a name="load-a-map"></a>Laden einer Karte

Das Laden einer Karte folgt in SDKs der gleichen Schrittfolge:

- Hinzufügen eines Verweises auf das Karten-SDK.
- Hinzufügen eines `div`-Tags zum Textkörper der Seite, das als Platzhalter für die Karte fungiert.
- Erstellen einer JavaScript-Funktion, die nach dem Laden der Seite aufgerufen wird.
- Erstellen einer Instanz der jeweiligen Kartenklasse.

**Einige wesentliche Unterschiede**

- Für Google Maps muss im Skriptverweis der API ein Kontoschlüssel angegeben werden. Authentifizierungs-Anmeldeinformationen für Azure Maps werden als Optionen der Kartenklasse angegeben. Dabei kann es sich um einen Abonnementschlüssel oder um Azure Active Directory-Informationen handeln.
- Google Maps akzeptiert eine Rückruffunktion im Skriptverweis der API, die verwendet wird, um eine Initialisierungsfunktion zum Laden der Karte aufzurufen. Bei Azure Maps sollte das onload-Ereignis der Seite verwendet werden.
- Beim Verweisen auf das `div`-Element, in dem die Karte gerendert werden soll, benötigt die `Map`-Klasse in Azure Maps lediglich den `id`-Wert, während Google Maps ein `HTMLElement`-Objekt erfordert.
- Koordinaten sind in Azure Maps als Position-Objekte definiert, die als einfaches Zahlenarray im Format `[longitude, latitude]` angegeben werden können.
- Die Zoomstufe in Azure Maps ist aufgrund der unterschiedlichen Größen im Anordnungssystem der Kacheln bei beiden Plattformen eine Stufe niedriger als im Google Maps-Beispiel.
- Standardmäßig fügt Azure Maps dem Kartenzeichenbereich keine Navigationssteuerelemente hinzu, wie etwa Zoomschaltflächen und Kartenstil-Schaltflächen. Es gibt jedoch Steuerelemente zum Hinzufügen eines Auswahlelements für den Kartenstil, von Zoomschaltflächen oder eines Drehungssteuerelements sowie eines Gradsteuerelements.
- In Azure Maps wird ein Ereignishandler hinzugefügt, um das `ready`-Ereignis der Karteninstanz zu überwachen. Dieses wird ausgelöst, wenn die Karte das Laden des WebGL-Kontexts und aller benötigten Ressourcen abgeschlossen hat. Jeglicher nach dem Laden auszuführende Code kann diesem Ereignishandler hinzugefügt werden.

Die Beispiele unten zeigen das Laden einer einfachen Karte, die über New York an den Koordinaten (Längengrad: -73.985, Breitengrad: 40,747) zentriert ist.

**Vorher: Google Maps**

Der folgende Code bildet ein Beispiel für die zentrierte und über einem Standort vergrößerte Darstellung einer Google Maps-Karte.

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <meta charset="utf-8" />
    <meta http-equiv="x-ua-compatible" content="IE=Edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />

    <script type='text/javascript'>
        var map;

        function initMap() {
            map = new google.maps.Map(document.getElementById('myMap'), {
                center: new google.maps.LatLng(40.747, -73.985),
                zoom: 12
            });
        }
    </script>

    <!-- Google Maps Script Reference -->
    <script src="https://maps.googleapis.com/maps/api/js?callback=initMap&key=[Your Google Maps Key]" async defer></script>
</head>
<body>
    <div id='myMap' style='position:relative;width:600px;height:400px;'></div>
</body>
</html>
```

Beim Ausführen dieses Codes in einem Browser wird eine Karte angezeigt, die wie die folgende Abbildung aussieht:

<center>

![Einfache Google Maps-Karte](media/migrate-google-maps-web-app/simple-google-map.png)</center>

**Nachher: Azure Maps**

Der folgende Code zeigt das Laden einer Karte mit der gleichen Ansicht in Azure Maps zusammen mit einem Stilsteuerelement und Zoomschaltflächen für die Karte.

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <meta charset="utf-8" />
    <meta http-equiv="x-ua-compatible" content="IE=Edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />

    <!-- Add references to the Azure Maps Map control JavaScript and CSS files. -->
    <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.css" type="text/css" />
    <script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.js"></script>

    <script type='text/javascript'>
        var map;

        function initMap() {
            map = new atlas.Map('myMap', {
                center: [-73.985, 40.747],  //Format coordinates as longitude, latitude.
                zoom: 11,   //Subtract the zoom level by one.

                //Specify authentication information when loading the map.
                authOptions: {
                    authType: 'subscriptionKey',
                    subscriptionKey: '<Your Azure Maps Key>'
                }
            });

            //Wait until the map resources are ready.
            map.events.add('ready', function () {
                //Add zoom controls to bottom right of map.
                map.controls.add(new atlas.control.ZoomControl(), {
                    position: 'bottom-right'
                });

                //Add map style control in top left corner of map.
                map.controls.add(new atlas.control.StyleControl(), {
                    position: 'top-left'
                });
            });
        }
    </script>
</head>
<body onload="initMap()">
    <div id='myMap' style='position:relative;width:600px;height:400px;'></div>
</body>
</html>
```

Beim Ausführen dieses Codes in einem Browser wird eine Karte angezeigt, die wie die folgende Abbildung aussieht:

<center>

![Einfache Azure Maps-Karte](media/migrate-google-maps-web-app/simple-azure-maps.png)</center>

Eine ausführliche Dokumentation zum Einrichten und Verwenden des Azure Maps-Steuerelements finden Sie [hier](how-to-use-map-control.md).

> [!NOTE]
> Im Gegensatz zu Google Maps ist bei Azure Maps keine Angabe einer Anfangszentrierung und einer Anfangszoomstufe beim Laden der Karte erforderlich. Wenn diese Informationen beim Laden der Karte nicht angegeben werden, versucht die Karte zu bestimmen, in welcher Stadt sich der Benutzer befindet und zentriert und vergrößert sich entsprechend.

**Zusätzliche Ressourcen:**

- Azure Maps bietet darüber hinaus Navigationssteuerelemente zum Drehen und Anordnen der Kartenansicht, wie [hier](map-add-controls.md) dokumentiert.

### <a name="localizing-the-map"></a>Lokalisieren der Karte

Wenn sich Ihre Zielgruppe über mehrere Länder erstreckt oder verschiedene Sprachen spricht, ist Lokalisierung ein wichtiger Punkt.

**Vorher: Google Maps**

Zum Lokalisieren von Google Maps-Karten werden dem Code Sprach- und Regionsparameter hier hinzugefügt:

```html
<script type="text/javascript" src=" https://maps.googleapis.com/maps/api/js?callback=initMap&key=[api_key]& language=[language_code]&region=[region_code]" async defer></script>
```

Dies ist ein Beispiel für Google Maps mit der Spracheinstellung „fr-FR“.

<center>

![Lokalisierung in Google Maps](media/migrate-google-maps-web-app/google-maps-localization.png)</center>

**Nachher: Azure Maps**

Azure Maps bietet zwei verschiedene Möglichkeiten zum Festlegen der Sprache und der regionalen Ansicht für die Karte. Die erste Option besteht darin, diese Informationen dem globalen *atlas*-Namespace hinzuzufügen. Dies führt dazu, dass alle Kartensteuerelementinstanzen in Ihrer App diese Einstellungen als Standard verwenden. Im Folgenden wird die Sprache auf Französisch („fr-FR“) und die regionale Ansicht auf „Auto“ festgelegt:

```javascript
atlas.setLanguage('fr-FR');
atlas.setView('auto');
```

Die zweite Option besteht darin, diese Informationen beim Laden der Karte folgendermaßen an die Kartenoptionen zu übergeben:

```javascript
map = new atlas.Map('myMap', {
    language: 'fr-FR',
    view: 'auto',

    authOptions: {
        authType: 'subscriptionKey',
        subscriptionKey: '<Your Azure Maps Key>'
    }
});
```

> [!NOTE]
> Mit Azure Maps ist es möglich, mehrere Karteninstanzen auf derselben Seite mit unterschiedlichen Sprach- und Regionseinstellungen zu laden. Außerdem ist es möglich, diese Einstellungen in der Karte auch nach dem Laden zu aktualisieren. [Hier](supported-languages.md) finden Sie eine ausführliche Liste der in Azure Maps unterstützten Sprachen.

Hier sehen Sie ein Beispiel für Azure Maps mit der Spracheinstellung „fr“ und der auf „fr-FR“ festgelegten Benutzerregion.

<center>

![Lokalisierung in Azure Maps](media/migrate-google-maps-web-app/azure-maps-localization.png)</center>

### <a name="setting-the-map-view"></a>Festlegen der Kartenansicht

Dynamische Karten können sowohl in Azure als auch in Google Maps programmgesteuert durch Aufrufen der passenden Funktionen in JavaScript an neue geografische Standorte verschoben werden. Die Beispiele unten demonstrieren das Festlegen der Karte auf Satellitenluftbilder, das Zentrieren der Karte über einem Standort mit den Koordinaten (Längengrad: -111,0225, Breitengrad: 35,0272) und das Ändern der Zoomstufe auf 15 in Google Maps.

> [!NOTE]
> Google Maps verwendet Kacheln mit der Abmessung 256 Pixel, während Azure Maps größere 512 Pixel-Kacheln verwendet. Dadurch wird die Anzahl der Netzwerkanforderungen reduziert, die Azure Maps zum Laden des gleichen Kartenbereichs wie Google Maps benötigt. Aufgrund der Funktionsweise von Kachelpyramiden in den Kartensteuerelementen, erfordern die größeren Kacheln in Azure Maps zum Erreichen des gleichen sichtbaren Bereichs einer Karte in Google Maps, dass Sie die in Google Maps verwendete Zoomstufe in Azure Maps um 1 verringern müssen.

**Vorher: Google Maps**

Das Kartensteuerelement in Google Maps kann mithilfe der `setOptions`-Methode programmgesteuert verschoben werden, was es Ihnen ermöglicht, den Mittelpunkt der Karte und eine Zoomstufe anzugeben.

```javascript
map.setOptions({
    mapTypeId: google.maps.MapTypeId.SATELLITE,
    center: new google.maps.LatLng(35.0272, -111.0225),
    zoom: 15
});
```

<center>

![Festgelegte Google Maps-Ansicht](media/migrate-google-maps-web-app/google-maps-set-view.png)</center>

**Nachher: Azure Maps**

In Azure Maps kann die Kartenposition programmgesteuert mithilfe der `setCamera`-Methode der Karte und der Kartenstil mithilfe der `setStyle`-Methode geändert werden. Beachten Sie, dass die Koordinaten in Azure Maps im Format „Längengrad, Breitengrad“ angegeben werden und der Wert für die Zoomstufe um 1 verringert ist.

```javascript
map.setCamera({
    center: [-111.0225, 35.0272],
    zoom: 14
});

map.setStyle({
    style: 'satellite'
});
```

<center>

![Festgelegte Azure Maps-Ansicht](media/migrate-google-maps-web-app/azure-maps-set-view.jpeg)</center>

**Zusätzliche Ressourcen:**

- [Auswählen eines Kartenstils](choose-map-style.md)
- [Unterstützte Kartenstile](supported-map-styles.md)

### <a name="adding-a-marker"></a>Hinzufügen eines Markers

In Azure Maps gibt es mehrere Möglichkeiten, wie Punktdaten auf der Karte gerendert werden können.

- **HTML-Marker**: Rendering von Punkten mithilfe herkömmlicher DOM-Elemente. HTML-Markers unterstützten Ziehbewegungen.
- **Symbolebene**: Rendering von Punkten als Symbol und/oder Text im WebGL-Kontext.
- **Blasenebene**: Punktdaten werden als Kreise auf der Karte gerendert. Die Radien der Kreise können auf der Grundlage von Eigenschaften in den Daten skaliert werden.

Sowohl die Symbol- als auch die Blasenebene wird innerhalb des WebGL-Kontexts gerendert, und beide können sehr große Punktmengen auf der Karte darstellen. Für diese Ebenen ist die Speicherung der Daten in einer Datenquelle erforderlich. Die Datenquellen und Renderingebenen sollten der Karte nach dem Auslösen des `ready`-Ereignisses hinzugefügt werden. HTML-Marker werden als DOM-Elemente auf der Seite gerendert und benötigen keine Datenquelle. Je mehr DOM-Elemente eine Seite aufweist, desto langsamer wird sie. Wenn mehr als ein paar hundert Punkte auf einer Karte gerendert werden sollen, empfiehlt es sich, stattdessen eine der Renderingebenen zu verwenden.

In den folgenden Beispielen wird der Karte ein Marker bei (Längengrad: -0,2, Breitengrad: 51,5) mit der als Bezeichnung überlagerten Zahl „10“ hinzugefügt.

**Vorher: Google Maps**

Bei Google Maps werden der Karte Marker mithilfe der `google.maps.Marker`-Klasse und durch Angeben der Karte als eine der Optionen hinzugefügt.

```javascript
//Create a marker and add it to the map.
var marker = new google.maps.Marker({
    position: new google.maps.LatLng(51.5, -0.2),
    label: '10',
    map: map
});
```

<center>

![Google Maps-Marker](media/migrate-google-maps-web-app/google-maps-marker.png)</center>

**Nachher: Azure Maps unter Verwendung von HTML-Markern**

In Azure Maps können HTML-Marker verwendet werden, um einen Punkt auf der Karte anzuzeigen, und werden für einfache Apps empfohlen, die nur eine kleine Anzahl Punkte auf der Karte darstellen müssen. Um einen HTML-Marker zu verwenden, erstellen Sie einfach eine Instanz der `atlas.HtmlMarker`-Klasse, legen Sie die Optionen für Text und Position fest, und fügen Sie der Karte den Marker mithilfe der `map.markers.add`-Methode hinzu.

```javascript
//Create a HTML marker and add it to the map.
map.markers.add(new atlas.HtmlMarker({
    text: '10',
    position: [-0.2, 51.5]
}));
```

<center>

![HTML-Marker in Azure Maps](media/migrate-google-maps-web-app/azure-maps-html-marker.png)</center>

**Nachher: Azure Maps unter Verwendung einer Symbolebene**

Beim Verwenden einer Symbolebene müssen die Daten einer Datenquelle hinzugefügt werden, und die Datenquelle muss an die Ebene angefügt werden. Außerdem sollten die Datenquelle und die Ebene der Karte nach dem Auslösen des `ready`-Ereignisses hinzugefügt werden. Zum Rendern eines eindeutigen Textwerts oberhalb eines Symbols müssen die Textinformationen als Eigenschaft des Datenpunkts gespeichert werden, und in der `textField`-Option der Ebene muss auf diese Eigenschaft verweisen werden. Das ist etwas arbeitsaufwändiger als die Verwendung von HTML-Markern, bietet aber viele Leistungsvorteile.

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <meta charset="utf-8" />
    <meta http-equiv="x-ua-compatible" content="IE=Edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />

    <!-- Add references to the Azure Maps Map control JavaScript and CSS files. -->
    <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.css" type="text/css" />
    <script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.js"></script>

    <script type='text/javascript'>
        var map, datasource;

        function initMap() {
            map = new atlas.Map('myMap', {
                center: [-0.2, 51.5],
                zoom: 9,
                
                authOptions: {
                    authType: 'subscriptionKey',
                    subscriptionKey: '<Your Azure Maps Key>'
                }
            });

            //Wait until the map resources are ready.
            map.events.add('ready', function () {

                //Create a data source and add it to the map.
                datasource = new atlas.source.DataSource();
                map.sources.add(datasource);

                //Create a point feature, add a property to store a label for it, and add it to the data source.
                datasource.add(new atlas.data.Feature(new atlas.data.Point([-0.2, 51.5]), {
                    label: '10'
                }));

                //Add a layer for rendering point data as symbols.
                map.layers.add(new atlas.layer.SymbolLayer(datasource, null, {
                    textOptions: {
                        //Use the label property to populate the text for the symbols.
                        textField: ['get', 'label'],
                        color: 'white',
                        offset: [0, -1]
                    }
                }));
            });
        }
    </script>
</head>
<body onload="initMap()">
    <div id='myMap' style='position:relative;width:600px;height:400px;'></div>
</body>
</html>
```

<center>

![Azure Maps-Symbolebene](media/migrate-google-maps-web-app/azure-maps-symbol-layer.png)</center>

**Zusätzliche Ressourcen:**

- [Erstellen einer Datenquelle](create-data-source-web-sdk.md)
- [Hinzufügen einer Symbolebene](map-add-pin.md)
- [Hinzufügen einer Blasenebene](map-add-bubble-layer.md)
- [Clusterpunktdaten](clustering-point-data-web-sdk.md)
- [Hinzufügen von HTML-Markern](map-add-custom-html.md)
- [Verwenden von datengesteuerten Formatvorlagenausdrücken](data-driven-style-expressions-web-sdk.md)
- [Symboloptionen der Symbolebene](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.iconoptions?view=azure-iot-typescript-latest)
- [Textoption der Symbolebene](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.textoptions?view=azure-iot-typescript-latest)
- [HTML-Markerklasse](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarker?view=azure-iot-typescript-latest)
- [HTML-Markeroptionen](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarkeroptions?view=azure-iot-typescript-latest)

### <a name="adding-a-custom-marker"></a>Hinzufügen eines benutzerdefinierten Markers

Benutzerdefinierte Bilder können zur Darstellung von Punkten auf einer Karte verwendet werden. Das folgende Bild wird in den Beispielen unten zur Verwendung eines benutzerdefinierten Bilds zum Darstellen eines Punkts auf der Karte an (Breitengrad: 51,5, Längengrad: -0,2) verwendet und versetzt die Position des Markers so, dass der Punkt des Ortsmarkensymbols sich an der richtigen Position auf der Karte ausrichtet.

<center>

![Bild von gelber Ortsmarke](media/migrate-google-maps-web-app/ylw_pushpin.png)<br/>
ylw\_pushpin.png</center>

**Vorher: Google Maps**

In Google Maps wird ein benutzerdefinierter Marker durch Angeben eines `Icon`-Objekts erstellt, das die `url` des Bilds und einen `anchor`-Punkt zum Ausrichten des Punkts des Ortsmarkenbilds an der Koordinate auf der Karte enthält. Der Ankerwert in Google Maps ist relativ zur oberen linken Ecke des Bilds.

```javascript
var marker = new google.maps.Marker({
    position: new google.maps.LatLng(51.5, -0.2),
    icon: {
        url: 'https://azuremapscodesamples.azurewebsites.net/Common/images/icons/ylw-pushpin.png',
        anchor: new google.maps.Point(5, 30)
    },
    map: map
});
```

<center>

![Benutzerdefinierter Marker in Google Maps](media/migrate-google-maps-web-app/google-maps-custom-marker.png)</center>

**Nachher: Azure Maps unter Verwendung von HTML-Markern**

Zum Anpassen eines HTML-Markers kann in Azure Maps der `htmlContent`-Option des Markers ein HTML-`string` oder -`HTMLElement` übergeben werden. In Azure Maps wird eine `anchor`-Option verwendet, um die relative Position des Markers bezogen auf die Positionskoordinate mithilfe eines von neun definierten Referenzpunkten anzugeben: „center“, „top“, „bottom“, „left“, „right“, „top-left“, „top-right“, „bottom-left“, „bottom-right“. Die Inhalte werden standardmäßig unten in der Mitte des HTML-Inhalts verankert. Um die Migration von Code aus Google Maps zu vereinfachen, legen Sie den `anchor` auf „top-left“ fest, und verwenden Sie dann die Option `pixelOffset` mit dem gleichen Offset, der in Google Maps verwendet wird. Die Offsets in Azure Maps bewegen sich in der zu Google Maps entgegengesetzten Richtung, Sie müssen sie daher mit minus Eins multiplizieren.

> [!TIP]
> Fügen Sie `pointer-events:none` im HTML-Inhalt als Formatvorlage hinzu, um das Standardverhalten beim Ziehen in Microsoft Edge zu deaktivieren, bei dem ein unerwünschtes Symbol angezeigt wird.

```javascript
map.markers.add(new atlas.HtmlMarker({
    htmlContent: '<img src="https://azuremapscodesamples.azurewebsites.net/Common/images/icons/ylw-pushpin.png" style="pointer-events: none;" />',
    anchor: 'top-left',
    pixelOffset: [-5, -30],
    position: [-0.2, 51.5]
}));
```

<center>

![Benutzerdefinierter HTML-Marker in Azure Maps](media/migrate-google-maps-web-app/azure-maps-custom-html-marker.png)</center>

**Nachher: Azure Maps unter Verwendung einer Symbolebene**

Symbolebenen in Azure Maps unterstützen ebenfalls benutzerdefinierte Bilder, das Bild muss jedoch zuerst in die Kartenressourcen geladen, und ihm muss eine eindeutige ID zugewiesen werden. Anschließend kann die Symbolebene auf diese ID verweisen. Für das Symbol kann zur Ausrichtung am richtigen Punkt des Bilds ein Offset mithilfe der Symboloption `offset` festgelegt werden. In Azure Maps wird eine `anchor`-Option verwendet, um die relative Position des Symbols bezogen auf die Positionskoordinate mithilfe eines von neun definierten Referenzpunkten anzugeben: „center“, „top“, „bottom“, „left“, „right“, „top-left“, „top-right“, „bottom-left“, „bottom-right“. Die Inhalte werden standardmäßig unten in der Mitte des HTML-Inhalts verankert. Um die Migration von Code aus Google Maps zu vereinfachen, legen Sie den `anchor` auf „top-left“ fest, und verwenden Sie dann die Option `offset` mit dem gleichen Offset, der in Google Maps verwendet wird. Die Offsets in Azure Maps bewegen sich in der zu Google Maps entgegengesetzten Richtung, Sie müssen sie daher mit minus Eins multiplizieren.

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <meta charset="utf-8" />
    <meta http-equiv="x-ua-compatible" content="IE=Edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />

    <!-- Add references to the Azure Maps Map control JavaScript and CSS files. -->
    <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.css" type="text/css" />
    <script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.js"></script>

    <script type='text/javascript'>
        var map, datasource;

        function initMap() {
            map = new atlas.Map('myMap', {
                center: [-0.2, 51.5],
                zoom: 9,
                authOptions: {
                    authType: 'subscriptionKey',
                    subscriptionKey: '<Your Azure Maps Key>'
                }
            });

            //Wait until the map resources are ready.
            map.events.add('ready', function () {

                //Load the custom image icon into the map resources.
                map.imageSprite.add('my-yellow-pin', 'https://azuremapscodesamples.azurewebsites.net/Common/images/icons/ylw-pushpin.png').then(function () {

                    //Create a data source and add it to the map.
                    datasource = new atlas.source.DataSource();
                    map.sources.add(datasource);

                    //Create a point and add it to the data source.
                    datasource.add(new atlas.data.Point([-0.2, 51.5]));

                    //Add a layer for rendering point data as symbols.
                    map.layers.add(new atlas.layer.SymbolLayer(datasource, null, {
                        iconOptions: {
                            //Set the image option to the id of the custom icon that was loaded into the map resources.
                            image: 'my-yellow-pin',
                            anchor: 'top-left',
                            offset: [-5, -30]
                        }
                    }));
                });
            });
        }
    </script>
</head>
<body onload="initMap()">
    <div id='myMap' style='position:relative;width:600px;height:400px;'></div>
</body>
</html>
```

<center>

![Symbolebene für benutzerdefinierte Symbole in Azure Maps](media/migrate-google-maps-web-app/azure-maps-custom-icon-symbol-layer.png)</center>

> [!TIP]
> Um ein erweitertes benutzerdefiniertes Rendering von Punkten zu erreichen, können mehrere Renderingebenen in Kombination verwendet werden. Wenn Sie beispielsweise mehrere Ortsmarken mit dem gleichen Symbol in Kreisen mit verschiedenen Farben verwenden möchten, können Sie, statt eine Gruppe Bilder für die einzelnen Farben zu erstellen, eine Symbolebene oberhalb einer Blasenebene überlagern und beide auf die gleiche Datenquelle verweisen lassen. Dies ist viel effizienter, als die genannte Gruppe verschiedener Bilder zu erstellen und sie durch die Karte verwalten zu lassen.

**Zusätzliche Ressourcen:**

- [Erstellen einer Datenquelle](create-data-source-web-sdk.md)
- [Hinzufügen einer Symbolebene](map-add-pin.md)
- [Hinzufügen von HTML-Markern](map-add-custom-html.md)
- [Verwenden von datengesteuerten Formatvorlagenausdrücken](data-driven-style-expressions-web-sdk.md)
- [Symboloptionen der Symbolebene](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.iconoptions?view=azure-iot-typescript-latest)
- [Textoption der Symbolebene](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.textoptions?view=azure-iot-typescript-latest)
- [HTML-Markerklasse](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarker?view=azure-iot-typescript-latest)
- [HTML-Markeroptionen](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarkeroptions?view=azure-iot-typescript-latest)

### <a name="adding-a-polyline"></a>Hinzufügen einer Polylinie

Polylinien werden verwendet, um eine Linie oder einen Pfad auf der Karte darzustellen. In den folgenden Beispielen wird gezeigt, wie Sie eine gestrichelte Polylinie auf der Karte erstellen.

**Vorher: Google Maps**

In Google Maps akzeptiert die Polyline-KLasse eine Reihe von Optionen. Ein Array aus Koordinaten wird in der `path`-Option der Polylinie übergeben.

```javascript
//Get the center of the map.
var center = map.getCenter();

//Define a symbol using SVG path notation, with an opacity of 1.
var lineSymbol = {
    path: 'M 0,-1 0,1',
    strokeOpacity: 1,
    scale: 4
};

//Create the polyline.
var line = new google.maps.Polyline({
    path: [
        center,
        new google.maps.LatLng(center.lat() - 0.5, center.lng() - 1),
        new google.maps.LatLng(center.lat() - 0.5, center.lng() + 1)
    ],
    strokeColor: 'red',
    strokeOpacity: 0,
    strokeWeight: 4,
    icons: [{
        icon: lineSymbol,
        offset: '0',
        repeat: '20px'
    }]
});

//Add the polyline to the map.
line.setMap(map);
```

<center>

![Google Maps-Polylinie](media/migrate-google-maps-web-app/google-maps-polyline.png)</center>

**Nachher: Azure Maps**

In Azure Maps werden Polylinien als LineString- oder MultiLineString-Objekte bezeichnet. Diese Objekte können einer Datenquelle hinzugefügt und mithilfe einer Linienebene gerendert werden.

```javascript
//Get the center of the map.
var center = map.getCamera().center;

//Create a data source and add it to the map.
var datasource = new atlas.source.DataSource();
map.sources.add(datasource);

//Create a line and add it to the data source.
datasource.add(new atlas.data.LineString([
    center,
    [center[0] - 1, center[1] - 0.5],
    [center[0] + 1, center[1] - 0.5]
]));

//Add a layer for rendering line data.
map.layers.add(new atlas.layer.LineLayer(datasource, null, {
    strokeColor: 'red',
    strokeWidth: 4,
    strokeDashArray: [3, 3]
}));
```

<center>

![Azure Maps-Polylinie](media/migrate-google-maps-web-app/azure-maps-polyline.png)</center>

**Zusätzliche Ressourcen:**

- [Hinzufügen von Linien zur Karte](map-add-line-layer.md)
- [Linienebenenoptionen](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.linelayeroptions?view=azure-iot-typescript-latest)
- [Verwenden von datengesteuerten Formatvorlagenausdrücken](data-driven-style-expressions-web-sdk.md)

### <a name="adding-a-polygon"></a>Hinzufügen eines Polygons

Polygone werden verwendet, um einen Bereich auf der Karte darzustellen. Azure Maps und Google Maps bieten sehr ähnliche Unterstützung für Polygone. In den folgenden Beispielen wird gezeigt, wie ein Polygon erstellt wird, das ein Dreieck bildet, das auf der Mittelpunktkoordinate der Karte basiert.

**Vorher: Google Maps**

In Google Maps akzeptiert die Polygon-KLasse eine Reihe von Optionen. Ein Array aus Koordinaten wird in der `paths`-Option des Polygons übergeben.

```javascript
//Get the center of the map.
var center = map.getCenter();

//Create a polygon.
var polygon = new google.maps.Polygon({
    paths: [
        center,
        new google.maps.LatLng(center.lat() - 0.5, center.lng() - 1),
        new google.maps.LatLng(center.lat() - 0.5, center.lng() + 1),
        center
    ],
    strokeColor: 'red',
    strokeWeight: 2,
    fillColor: 'rgba(0, 255, 0, 0.5)'
});

//Add the polygon to the map
polygon.setMap(map);
```

<center>

![Google Maps-Polygon](media/migrate-google-maps-web-app/google-maps-polygon.png)</center>

**Nachher: Azure Maps**

In Azure Maps können einer Datenquelle Polygon- und MultiPolygon-Objekte hinzugefügt und mithilfe von Ebenen auf der Karte gerendert werden. Der Bereich eines Polygons kann in einer Polygonebene gerendert werden. Der Umriss eines Polygons kann mithilfe einer Linienebene gerendert werden.

```javascript
//Get the center of the map.
var center = map.getCamera().center;

//Create a data source and add it to the map.
datasource = new atlas.source.DataSource();
map.sources.add(datasource);

//Create a polygon and add it to the data source.
datasource.add(new atlas.data.Polygon([
    center,
    [center[0] - 1, center[1] - 0.5],
    [center[0] + 1, center[1] - 0.5],
    center
]));

//Add a polygon layer for rendering the polygon area.
map.layers.add(new atlas.layer.PolygonLayer(datasource, null, {
    fillColor: 'rgba(0, 255, 0, 0.5)'
}));

//Add a line layer for rendering the polygon outline.
map.layers.add(new atlas.layer.LineLayer(datasource, null, {
    strokeColor: 'red',
    strokeWidth: 2
}));
```

<center>

![Azure Maps-Polygon](media/migrate-google-maps-web-app/azure-maps-polygon.png)</center>

**Zusätzliche Ressourcen:**

- [Hinzufügen eines Polygons zur Karte](map-add-shape.md)
- [Hinzufügen eines Kreises zur Karte](map-add-shape.md#add-a-circle-to-the-map)
- [Polygonebenenoptionen](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.polygonlayeroptions?view=azure-iot-typescript-latest)
- [Linienebenenoptionen](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.linelayeroptions?view=azure-iot-typescript-latest)
- [Verwenden von datengesteuerten Formatvorlagenausdrücken](data-driven-style-expressions-web-sdk.md)

### <a name="display-an-info-window"></a>Anzeigen eines Infofensters

Zusätzliche Informationen zu einer Entität auf einer Karte können in Google Maps als `google.maps.InfoWindow`-Klasse dargestellt werden, in Azure Maps kann dies mithilfe der `atlas.Popup`-Klasse erreicht werden. In den folgenden Beispielen wird der Karte ein Marker hinzugefügt, der ein Infofenster/Popup anzeigt, wenn darauf geklickt wird.

**Vorher: Google Maps**

In Google Maps wird ein Infofenster mithilfe des `google.maps.InfoWindow`-Konstruktors erstellt.

```javascript
//Add a marker in which to display an infowindow for.
var marker = new google.maps.Marker({
    position: new google.maps.LatLng(47.6, -122.33),
    map: map
});

//Create an infowindow.
var infowindow = new google.maps.InfoWindow({
    content: '<div style="padding:5px"><b>Hello World!</b></div>'
});

//Add a click event to the marker to open the infowindow.
marker.addListener('click', function () {
    infowindow.open(map, marker);
});
```

<center>

![Google Maps-Popup](media/migrate-google-maps-web-app/google-maps-popup.png)</center>

**Nachher: Azure Maps**

In Azure Maps kann ein Popup verwendet werden, um zusätzliche Informationen zu einem Ort anzuzeigen. Ein `string`- oder `HTMLElement`-HTML-Objekt kann der `content`-Option des Popups übergeben werden. Popups können bei Bedarf unabhängig von jeder Form angezeigt werden. Daher muss ein `position`-Wert angegeben werden. Um ein Popup anzuzeigen, rufen Sie die `open`-Methode auf, und übergeben Sie die `map`, in der das Popup angezeigt werden soll.

```javascript
//Add a marker to the map in which to display a popup for.
var marker = new atlas.HtmlMarker({
    position: [-122.33, 47.6]
});

//Add the marker to the map.
map.markers.add(marker);

//Create a popup.
var popup = new atlas.Popup({
    content: '<div style="padding:5px"><b>Hello World!</b></div>',
    position: [-122.33, 47.6],
    pixelOffset: [0, -35]
});

//Add a click event to the marker to open the popup.
map.events.add('click', marker, function () {
    //Open the popup
    popup.open(map);
});
```

<center>

![Azure Maps-Popup](media/migrate-google-maps-web-app/azure-maps-popup.png)</center>

> [!NOTE]
> Um das gleiche mit einem Symbol-, einer Blasen-, einer Linien- oder einer Polygonebene zu erreichen, übergeben Sie die Ebene einfach anstelle eines Markers an den Maps-Ereigniscode.

**Zusätzliche Ressourcen:**

- [Hinzufügen eines Popups](map-add-popup.md)
- [Popup mit Medieninhalt](https://azuremapscodesamples.azurewebsites.net/index.html?sample=Popup%20with%20Media%20Content)
- [Popups für Formen](https://azuremapscodesamples.azurewebsites.net/index.html?sample=Popups%20on%20Shapes)
- [Wiederverwenden eines Popups für mehrere Ortsmarken](https://azuremapscodesamples.azurewebsites.net/index.html?sample=Reusing%20Popup%20with%20Multiple%20Pins)
- [Popup-Klasse](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest)
- [Popupoptionen](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popupoptions?view=azure-iot-typescript-latest)

### <a name="import-a-geojson-file"></a>Importieren einer GeoJSON-Datei

Google Maps unterstützt das Laden und dynamische Formatieren von GeoJSON-Daten mithilfe der `google.maps.Data`-Klasse. Die Funktionalität dieser Klasse orientiert sich viel stärker an der datengesteuerten Formatierung von Azure Maps. Ein wichtiger Unterschied besteht darin, dass Sie bei Google Maps eine Rückruffunktion und die entsprechende Geschäftslogik zum Formatieren jedes Features angeben, die einzeln im UI-Thread verarbeitet werden. In Azure Maps unterstützen Ebenen das Angeben von datengesteuerten Ausdrücken als Formatierungsoptionen. Diese Ausdrücke werden zur Renderingzeit in einem separaten Thread verarbeitet, was für höhere Renderingleistung sorgt und ein schnelleres Rendering größerer Datasets ermöglicht.

In den folgenden Beispielen wird ein GeoJSON-Feed aller Erdbeben der letzten sieben Tage aus dem USGS geladen und in Form skalierter Kreise auf der Karte dargestellt. Farbe und Maßstab jedes Kreises basieren auf der Magnitude der einzelnen Erdbeben, die in der `"mag"`-Eigenschaft jedes Features im Dataset gespeichert ist. Wenn die Magnitude größer als oder gleich 5 ist, ist der Kreis rot, ist sie größer als oder gleich 3, aber kleiner als 5, ist der Kreis orange, ist sie kleiner als 3, ist der Kreis grün. Der Radius der einzelnen Kreise entspricht dem Exponentialwert der Magnitude, multipliziert mit 0,1.

**Vorher: Google Maps**

In Google Maps kann in der `map.data.setStyle`-Methode eine einzelne Rückruffunktion angegeben werden, die zum Anwenden von Geschäftslogik auf jedes Feature verwendet wird, das mithilfe der `map.data.loadGeoJson`-Methode aus dem GeoJSON-Feed geladen wird.

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <meta charset="utf-8" />
    <meta http-equiv="x-ua-compatible" content="IE=Edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />

    <script type='text/javascript'>
        var map;
        var earthquakeFeed = 'https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/all_week.geojson';

        function initMap() {
            map = new google.maps.Map(document.getElementById('myMap'), {
                center: new google.maps.LatLng(20, -160),
                zoom: 2
            });

            //Define a callback to style each feature.
            map.data.setStyle(function (feature) {

                //Extract the 'mag' property from the feature.
                var mag = parseFloat(feature.getProperty('mag'));

                //Set the color value to 'green' by default.
                var color = 'green';

                //If the mag value is greater than 5, set the color to 'red'.
                if (mag >= 5) {
                    color = 'red';
                }
                //If the mag value is greater than 3, set the color to 'orange'.
                else if (mag >= 3) {
                    color = 'orange';
                }

                return /** @type {google.maps.Data.StyleOptions} */({
                    icon: {
                        path: google.maps.SymbolPath.CIRCLE,

                        //Scale the radius based on an exponential of the 'mag' value.
                        scale: Math.exp(mag) * 0.1,
                        fillColor: color,
                        fillOpacity: 0.75,
                        strokeWeight: 2,
                        strokeColor: 'white'
                    }
                });
            });

            //Load the data feed.
            map.data.loadGeoJson(earthquakeFeed);
        }
    </script>

    <!-- Google Maps Script Reference -->
    <script src="https://maps.googleapis.com/maps/api/js?callback=initMap&key=[Your Google Maps Key]" async defer></script>
</head>
<body>
    <div id='myMap' style='position:relative;width:600px;height:400px;'></div>
</body>
</html>
```

<center>

![Google Maps GeoJSON](media/migrate-google-maps-web-app/google-maps-geojson.png)</center>

**Nachher: Azure Maps**

GeoJSON ist der Basisdatentyp in Azure Maps, der leicht mithilfe der `datasource.importFromUrl`-Methode in eine Datenquelle importiert werden kann. Eine Blasenebene stellt Funktionen zum Rendern von skalierten Kreisen auf der Grundlage der Eigenschaften der Features in einer Datenquelle bereit. Statt eine Rückruffunktion zu verwenden, wird die Geschäftslogik in einen Ausdruck konvertiert und an die Formatoptionen übergeben. Mithilfe von Ausdrücken wird definiert, wie die Geschäftslogik funktioniert, damit sie an einen anderen Thread übergeben und anhand der Featuredaten ausgewertet werden kann. Azure Maps können mehrere Datenquellen und Ebenen hinzugefügt werden, jeweils mit einer anderen Geschäftslogik, sodass mehrere Datasets auf der Karte auf unterschiedliche Weise gerendert werden können.

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <meta charset="utf-8" />
    <meta http-equiv="x-ua-compatible" content="IE=Edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />

    <!-- Add references to the Azure Maps Map control JavaScript and CSS files. -->
    <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.css" type="text/css" />
    <script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.js"></script>

    <script type='text/javascript'>
        var map;
        var earthquakeFeed = 'https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/all_week.geojson';

        function initMap() {
            //Initialize a map instance.
            map = new atlas.Map('myMap', {
                center: [-160, 20],
                zoom: 1,

                //Add your Azure Maps subscription key to the map SDK. Get an Azure Maps key at https://azure.com/maps
                authOptions: {
                    authType: 'subscriptionKey',
                    subscriptionKey: '<Your Azure Maps Key>'
                }
            });

            //Wait until the map resources are ready.
            map.events.add('ready', function () {

                //Create a data source and add it to the map.
                datasource = new atlas.source.DataSource();
                map.sources.add(datasource);

                //Load the earthquake data.
                datasource.importDataFromUrl(earthquakeFeed);

                //Create a layer to render the data points as scaled circles.
                map.layers.add(new atlas.layer.BubbleLayer(datasource, null, {
                    //Make the circles semi-transparent.
                    opacity: 0.75,

                    color: [
                        'case',

                        //If the mag value is greater than 5, return 'red'.
                        ['>=', ['get', 'mag'], 5],
                        'red',

                        //If the mag value is greater than 3, return 'orange'.
                        ['>=', ['get', 'mag'], 3],
                        'orange',

                        //Return 'green' as a fallback.
                        'green'
                    ],

                    //Scale the radius based on an exponential of the 'mag' value.
                    radius: ['*', ['^', ['e'], ['get', 'mag']], 0.1]
                }));
            });
        }
    </script>
</head>
<body onload="initMap()">
    <div id='myMap' style='position:relative;width:600px;height:400px;'></div>
</body>
</html>
```

<center>

![Azure Maps GeoJSON](media/migrate-google-maps-web-app/azure-maps-geojson.png)</center>

**Zusätzliche Ressourcen:**

- [Hinzufügen einer Symbolebene](map-add-pin.md)
- [Hinzufügen einer Blasenebene](map-add-bubble-layer.md)
- [Clusterpunktdaten](clustering-point-data-web-sdk.md)
- [Verwenden von datengesteuerten Formatvorlagenausdrücken](data-driven-style-expressions-web-sdk.md)

### <a name="marker-clustering"></a>Markerclustering

Bei der Visualisierung vieler Datenpunkte auf der Karte überlappen sich die Punkte. Die Karte wirkt überladen, und es wird schwierig, die Karte zu überblicken und zu verwenden. Das Clustering von Punktdaten kann dazu verwendet werden, die Benutzerfreundlichkeit zu erhöhen und zugleich die Leistung zu verbessern. Das Clustering von Punktdaten ist der Prozess, bei dem benachbarte Punktdaten kombiniert und auf der Karte als ein einzelner gruppierter Datenpunkt dargestellt werden. Wenn der Benutzer in die Karte zoomt, werden die Cluster in ihre einzelnen Datenpunkte unterteilt.

In den folgenden Beispielen wird ein GeoJSON-Feed aus den Erdbebendaten der letzten Woche geladen und der Karte hinzugefügt. Cluster werden als skalierte und farbige Kreise dargestellt, abhängig von der Anzahl der Punkte, die sie enthalten.

> [!NOTE]
> Es gibt eine Reihe verschiedener Algorithmen, die für das Clustering von Markern verwendet werden. Google und Azure Maps verwenden leicht abweichende Algorithmen. Daher kann sich die Punktverteilung in den Clustern gelegentlich unterscheiden.

**Vorher: Google Maps**

In Google Maps können Marker durch Laden in die MarkerClusterer-Bibliothek gruppiert werden. Clustersymbole sind auf Bilder beschränkt, die die Zahlen von 1 bis 5 als Name aufweisen und im gleichen Verzeichnis gehostet werden.

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <meta charset="utf-8" />
    <meta http-equiv="x-ua-compatible" content="IE=Edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />

    <script type='text/javascript'>
        var map;
        var earthquakeFeed = 'https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/all_week.geojson';

        function initMap() {
            map = new google.maps.Map(document.getElementById('myMap'), {
                center: new google.maps.LatLng(20, -160),
                zoom: 2
            });

            //Download the GeoJSON data.
            fetch(earthquakeFeed)
                .then(function (response) {
                    return response.json();
                }).then(function (data) {
                    //Loop through the GeoJSON data and create a marker for each data point.
                    var markers = [];

                    for (var i = 0; i < data.features.length; i++) {

                        markers.push(new google.maps.Marker({
                            position: new google.maps.LatLng(data.features[i].geometry.coordinates[1], data.features[i].geometry.coordinates[0])
                        }));
                    }

                    //Create a marker clusterer instance and tell it where to find the cluster icons.
                    var markerCluster = new MarkerClusterer(map, markers,
                        { imagePath: 'https://developers.google.com/maps/documentation/javascript/examples/markerclusterer/m' });
                });
        }
    </script>

    <!-- Load the marker cluster library. -->
    <script src="https://developers.google.com/maps/documentation/javascript/examples/markerclusterer/markerclusterer.js"></script>

    <!-- Google Maps Script Reference -->
    <script src="https://maps.googleapis.com/maps/api/js?callback=initMap&key=[Your Google Maps Key]" async defer></script>
</head>
<body>
    <div id='myMap' style='position:relative;width:600px;height:400px;'></div>
</body>
</html>
```

<center>

![Google Maps-Clustering](media/migrate-google-maps-web-app/google-maps-clustering.png)</center>

**Nachher: Azure Maps**

In Azure Maps werden Daten aus einer Datenquelle hinzugefügt und verwaltet. Ebenen stellen eine Verbindung mit Datenquellen her und übernehmen das Rendering der enthaltenen Daten. Die `DataSource`-Klasse in Azure Maps bietet mehrere Clusteringoptionen.

- `cluster`: weist die Datenquelle an, Punktdaten zu gruppieren.
- `clusterRadius`: der Radius in Pixeln zum Gruppieren von Punkten.
- `clusterMaxZoom`: die maximale Zoomstufe, bei der Clustering erfolgt. Wenn Sie über diesen Wert hinaus vergrößern, werden alle Punkte als Symbole dargestellt.
- `clusterProperties`: Definiert benutzerdefinierte Eigenschaften, die mithilfe von Ausdrücken für alle Punkte in jedem Cluster berechnet und den Eigenschaften jedes Clusterpunkts hinzugefügt werden.

Bei aktiviertem Clustering sendet die Datenquelle gruppierte und nicht gruppierte Datenpunkte zum Rendern an Ebenen. Die Datenquelle kann Hunderttausende von Datenpunkten gruppieren. Ein gruppierter Datenpunkt weist die folgenden Eigenschaften auf:

| Eigenschaftenname             | type    | Beschreibung   |
|---------------------------|---------|---------------|
| `cluster`                 | boolean | Gibt an, ob das Feature einen Cluster darstellt. |
| `cluster_id`              | string  | Eine eindeutige ID für den Cluster, die mit den DataSource-Methoden `getClusterExpansionZoom`, `getClusterChildren` und `getClusterLeaves` verwendet werden kann. |
| `point_count`             | number  | Die Anzahl der Punkte, die der Cluster enthält.  |
| `point_count_abbreviated` | string  | Eine Zeichenfolge, die den `point_count`-Wert kürzt, falls zu lang. (Beispiel: 4.000 wird zu 4K)  |

Die `DataSource`-Klasse weist die folgende Hilfsfunktion für den Zugriff auf zusätzliche Informationen zu einem Cluster mithilfe der `cluster_id` auf.

| Methode | Rückgabetyp | Beschreibung |
|--------|-------------|-------------|
| `getClusterChildren(clusterId: number)` | Promise&lt;Array&lt;Feature&lt;Geometry, any&gt;\| Shape&gt;&gt; | Ruft die untergeordneten Elemente des angegebenen Clusters für den nächsten Zoomfaktor ab. Diese untergeordneten Elemente können eine Kombination aus Formen und untergeordneten Clustern sein. Die untergeordneten Cluster sind Features mit Eigenschaften, die ClusteredProperties entsprechen. |
| `getClusterExpansionZoom(clusterId: number)` | Promise&lt;number&gt; | Berechnet einen Zoomfaktor, bei dem der Cluster mit der Erweiterung oder Unterteilung beginnt. |
| `getClusterLeaves(clusterId: number, limit: number, offset: number)` | Promise&lt;Array&lt;Feature&lt;Geometry, any&gt;\| Shape&gt;&gt; | Ruft alle Punkte in einem Cluster ab. Legen Sie `limit` fest, um eine Teilmenge der Punkte zurückzugeben, und verwenden Sie `offset`, um die Punkte zu durchlaufen. |

Beim Rendern von gruppierten Daten auf der Karte ist es oft am einfachsten, zwei oder mehr Ebenen zu verwenden. Im folgenden Beispiel werden drei Ebenen verwendet, eine Blasenebene zum Zeichnen von skalierten, farbigen, auf der Größe der Cluster basierenden Kreisen, eine Symbolebene zum Rendern der Clustergröße als Text und eine zweite Symbolebene zum Rendern der nicht gruppierten Punkte. Es gibt viele weitere Möglichkeiten, gruppierte Daten in Azure Maps darzustellen, die in der [Clusterpunktdaten](clustering-point-data-web-sdk.md)-Dokumentation beleuchtet werden.

GeoJSON-Daten können mithilfe der `importDataFromUrl`-Funktion der `DataSource`-Klasse direkt in Azure Maps importiert werden.

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <meta charset="utf-8" />
    <meta http-equiv="x-ua-compatible" content="IE=Edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />

    <!-- Add references to the Azure Maps Map control JavaScript and CSS files. -->
    <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.css" type="text/css" />
    <script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.js"></script>

    <script type='text/javascript'>
        var map, datasource;
        var earthquakeFeed = 'https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/all_week.geojson';

        function initMap() {
            //Initialize a map instance.
            map = new atlas.Map('myMap', {
                center: [-160, 20],
                zoom: 1,

                //Add your Azure Maps subscription key to the map SDK. Get an Azure Maps key at https://azure.com/maps
                authOptions: {
                    authType: 'subscriptionKey',
                    subscriptionKey: '<Your Azure Maps Key>'
                }
            });

            //Wait until the map resources are ready.
            map.events.add('ready', function () {

                //Create a data source and add it to the map.
                datasource = new atlas.source.DataSource(null, {
                    //Tell the data source to cluster point data.
                    cluster: true
                });
                map.sources.add(datasource);

                //Create layers for rendering clusters, their counts and unclustered points and add the layers to the map.
                map.layers.add([
                    //Create a bubble layer for rendering clustered data points.
                    new atlas.layer.BubbleLayer(datasource, null, {
                        //Scale the size of the clustered bubble based on the number of points inthe cluster.
                        radius: [
                            'step',
                            ['get', 'point_count'],
                            20,         //Default of 20 pixel radius.
                            100, 30,    //If point_count >= 100, radius is 30 pixels.
                            750, 40     //If point_count >= 750, radius is 40 pixels.
                        ],

                        //Change the color of the cluster based on the value on the point_cluster property of the cluster.
                        color: [
                            'step',
                            ['get', 'point_count'],
                            'lime',            //Default to lime green. 
                            100, 'yellow',     //If the point_count >= 100, color is yellow.
                            750, 'red'         //If the point_count >= 750, color is red.
                        ],
                        strokeWidth: 0,
                        filter: ['has', 'point_count'] //Only rendered data points which have a point_count property, which clusters do.
                    }),

                    //Create a symbol layer to render the count of locations in a cluster.
                    new atlas.layer.SymbolLayer(datasource, null, {
                        iconOptions: {
                            image: 'none' //Hide the icon image.
                        },
                        textOptions: {
                            textField: ['get', 'point_count_abbreviated'],
                            offset: [0, 0.4]
                        }
                    }),

                    //Create a layer to render the individual locations.
                    new atlas.layer.SymbolLayer(datasource, null, {
                        filter: ['!', ['has', 'point_count']] //Filter out clustered points from this layer.
                    })
                ]);

                //Retrieve a GeoJSON data set and add it to the data source. 
                datasource.importDataFromUrl(earthquakeFeed);
            });
        }
    </script>
</head>
<body onload="initMap()">
    <div id='myMap' style='position:relative;width:600px;height:400px;'></div>
</body>
</html>
```

<center>

![Azure Maps-Clustering](media/migrate-google-maps-web-app/azure-maps-clustering.png)</center>

**Zusätzliche Ressourcen:**

- [Hinzufügen einer Symbolebene](map-add-pin.md)
- [Hinzufügen einer Blasenebene](map-add-bubble-layer.md)
- [Clusterpunktdaten](clustering-point-data-web-sdk.md)
- [Verwenden von datengesteuerten Formatvorlagenausdrücken](data-driven-style-expressions-web-sdk.md)

### <a name="add-a-heat-map"></a>Hinzufügen eines Wärmebilds

Wärmebilder, die auch als Punktdichtekarten bezeichnet werden, stellen eine Art der Datenvisualisierung dar, die verwendet wird, um die Dichte von Daten mit Farbbereichen darzustellen. Sie werden häufig verwendet, um die „Hotspots“ von Daten auf einer Karte anzuzeigen und sind gut zum Rendern von großen Punktdatasets geeignet.

In den folgenden Beispielen wird ein GeoJSON-Feed aller Erdbeben im letzten Monat von der USGS geladen; diese werden dann als gewichtetes Wärmebild gerendert, wobei die `"mag"`-Eigenschaft als Gewichtung dient.

**Vorher: Google Maps**

In Google Maps muss zum Erstellen eines Wärmebilds die Visualisierungsbibliothek geladen werden, indem der URL des API-Skripts `&libraries=visualization` hinzugefügt wird. Die Wärmebildebene in Google Maps unterstützt GeoJSON-Daten nicht direkt, vielmehr müssen die Daten zuerst heruntergeladen und in ein Array gewichteter Datenpunkte konvertiert werden.

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <meta charset="utf-8" />
    <meta http-equiv="x-ua-compatible" content="IE=Edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />

    <script type='text/javascript'>
        var map;
        var earthquakeFeed = 'https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/all_month.geojson';

        function initMap() {

            var map = new google.maps.Map(document.getElementById('myMap'), {
                center: new google.maps.LatLng(20, -160),
                zoom: 2,
                mapTypeId: 'satellite'
            });

            //Download the GeoJSON data.
            fetch(url).then(function (response) {
                return response.json();
            }).then(function (res) {
                var points = getDataPoints(res);

                var heatmap = new google.maps.visualization.HeatmapLayer({
                    data: points
                });
                heatmap.setMap(map);
            });
        }

        function getDataPoints(geojson, weightProp) {
            var points = [];

            for (var i = 0; i < geojson.features.length; i++) {
                var f = geojson.features[i];

                if (f.geometry.type === 'Point') {
                    points.push({
                        location: new google.maps.LatLng(f.geometry.coordinates[1], f.geometry.coordinates[0]),
                        weight: f.properties[weightProp]
                    });
                }
            }

            return points;
        } 
    </script>

    <!-- Google Maps Script Reference -->
    <script src="https://maps.googleapis.com/maps/api/js?callback=initMap&key=[Your Google Maps Key]&libraries=visualization" async defer></script>
</head>
<body>
    <div id='myMap' style='position:relative;width:600px;height:400px;'></div>
</body>
</html>
```

<center>

![Google Maps-Wärmebild](media/migrate-google-maps-web-app/google-maps-heatmap.png)</center>

**Nachher: Azure Maps**

Laden Sie in Azure Maps die GeoJSON-Daten in eine Datenquelle, und verbinden Sie die Datenquelle mit einer Wärmebildebene. Die Eigenschaft, die für die Gewichtung verwendet werden soll, kann der `weight`-Option mithilfe eines Ausdrucks übergeben werden. GeoJSON-Daten können mithilfe der `importDataFromUrl`-Funktion der `DataSource`-Klasse direkt in Azure Maps importiert werden.

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <meta charset="utf-8" />
    <meta http-equiv="x-ua-compatible" content="IE=Edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />

    <!-- Add references to the Azure Maps Map control JavaScript and CSS files. -->
    <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.css" type="text/css" />
    <script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.js"></script>

    <script type='text/javascript'>
        var map;
        var earthquakeFeed = 'https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/all_month.geojson';

        function initMap() {
            //Initialize a map instance.
            map = new atlas.Map('myMap', {
                center: [-160, 20],
                zoom: 1,
                style: 'satellite',

                //Add your Azure Maps subscription key to the map SDK. Get an Azure Maps key at https://azure.com/maps
                authOptions: {
                    authType: 'subscriptionKey',
                    subscriptionKey: '<Your Azure Maps Key>'
                }
            });

            //Wait until the map resources are ready.
            map.events.add('ready', function () {

                //Create a data source and add it to the map.
                datasource = new atlas.source.DataSource();
                map.sources.add(datasource);

                //Load the earthquake data.
                datasource.importDataFromUrl(earthquakeFeed);

                //Create a layer to render the data points as a heat map.
                map.layers.add(new atlas.layer.HeatMapLayer(datasource, null, {
                    weight: ['get', 'mag'],
                    intensity: 0.005,
                    opacity: 0.65,
                    radius: 10
                }));
            });
        }
    </script>
</head>
<body onload="initMap()">
    <div id='myMap' style='position:relative;width:600px;height:400px;'></div>
</body>
</html>
```

<center>

![Azure Maps-Wärmebild](media/migrate-google-maps-web-app/azure-maps-heatmap.png)</center>

**Zusätzliche Ressourcen:**

- [Hinzufügen einer Wärmebildebene](map-add-heat-map-layer.md)
- [Wärmebildebenen-Klasse](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.heatmaplayer?view=azure-iot-typescript-latest)
- [Optionen für Wärmebildebenen](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.heatmaplayeroptions?view=azure-iot-typescript-latest)
- [Verwenden von datengesteuerten Formatvorlagenausdrücken](data-driven-style-expressions-web-sdk.md)

### <a name="overlay-a-tile-layer"></a>Überlagern einer Kachelebene

Kachelebenen, die in Google Maps auch als Bildüberlagerungen bezeichnet werden, ermöglichen es Ihnen, große Bilder zu überlagern, die in kleinere gekachelte Bilder aufgeteilt wurden, die sich am Kartenkachel-Positionierungssystem orientieren. Dies ist eine gängige Methode zum Überlagern großer Bilder oder sehr großer Datasets.

In den folgenden Beispielen wird eine Wetterradar-Kachelebene aus dem Iowa Environmental Mesonet der Iowa State University überlagert.

**Vorher: Google Maps**

In Google Maps können Kachelebenen mithilfe der `google.maps.ImageMapType`-Klasse erstellt werden.

```javascript
map.overlayMapTypes.insertAt(0, new google.maps.ImageMapType({
    getTileUrl: function (coord, zoom) {
        return "https://mesonet.agron.iastate.edu/cache/tile.py/1.0.0/nexrad-n0q-900913/" + zoom + "/" + coord.x + "/" + coord.y;
    },
    tileSize: new google.maps.Size(256, 256),
    opacity: 0.8
}));
```

<center>

![Google Maps-Kachelebene](media/migrate-google-maps-web-app/google-maps-tile-layer.png)</center>

**Nachher: Azure Maps**

In Azure Maps kann eine Kachel Ebene einer Karte auf ziemlich die gleiche Weise wie jede andere Ebene hinzugefügt werden. Eine formatierte URL, die x, y und Zoomplatzhalter bzw. `{x}`, `{y}`, `{z}` aufweist, wird dazu verwendet, die Ebene anzuweisen, an welcher Position sie auf die Kacheln zugreifen soll. Kachelebenen in Azure Maps unterstützen außerdem `{quadkey}`-, `{bbox-epsg-3857}`- und `{subdomain}`-Platzhalter.

> [!TIP]
> In Azure Maps können Ebenen leicht unterhalb von anderen Ebenen gerendert werden, einschließlich Basiskartenebenen. Es ist häufig wünschenswert, Kachelebenen unterhalb der Kartenbezeichnungen zu rendern, damit sie leicht zu lesen sind. Die `map.layers.add`-Methode nimmt einen zweiten Parameter an, bei dem es sich um die ID der Ebene handelt, unterhalb der die neue Ebene eingefügt werden soll. Um eine Kachelebene unterhalb der Kartenbezeichnungen einzufügen, kann der folgende Code verwendet werden: `map.layers.add(myTileLayer, "labels");`

```javascript
//Create a tile layer and add it to the map below the label layer.
map.layers.add(new atlas.layer.TileLayer({
    tileUrl: 'https://mesonet.agron.iastate.edu/cache/tile.py/1.0.0/nexrad-n0q-900913/{z}/{x}/{y}.png',
    opacity: 0.8,
    tileSize: 256
}), 'labels');
```

<center>

![Azure Maps-Kachelebene](media/migrate-google-maps-web-app/azure-maps-tile-layer.png)</center>

> [!TIP]
> Kachelanforderungen können mithilfe der `transformRequest`-Option der Karte erfasst werden. Auf diese Weise können Sie der Anforderung bei Bedarf Header hinzufügen oder diese ändern.

**Zusätzliche Ressourcen:**

- [Hinzufügen von Kachelebenen](map-add-tile-layer.md)
- [Kachelebenenklasse](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.tilelayer?view=azure-iot-typescript-latest)
- [Kachelebenenoptionen](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.tilelayeroptions?view=azure-iot-typescript-latest)

### <a name="show-traffic"></a>Anzeigen des Verkehrs

Sowohl Azure- als auch Google-Karten können mit Verkehrsdaten überlagert werden.

**Vorher: Google Maps**

In Google Maps kann die Karte mithilfe der Verkehrsebene mit Verkehrsdaten überlagert werden.

```javascript
var trafficLayer = new google.maps.TrafficLayer();
trafficLayer.setMap(map);
```

<center>

![Verkehrsinformationen in Google Maps](media/migrate-google-maps-web-app/google-maps-traffic.png)</center>

**Nachher: Azure Maps**

Azure Maps bietet verschiedene Optionen zum Anzeigen von Verkehr. Verkehrsvorfälle, wie etwa Straßensperrungen und Unfälle, können als Symbole auf der Karte angezeigt werden. Der Verkehrsfluss und farbig codierte Straßen können der Karte überlagert werden; die Farben können geändert werden und auf dem veröffentlichten Tempolimit, der normalerweise zu erwartenden Verzögerung oder der absoluten Verzögerung basieren. Vorfallsdaten in Azure Maps werden jede Minute aktualisiert, Daten zum Verkehrsfluss alle zwei Minuten.

```javascript
map.setTraffic({
    incidents: true,
    flow: 'relative'
});
```

<center>

![Azure Maps-Verkehrsdarstellung](media/migrate-google-maps-web-app/azure-maps-traffic.png)</center>

Wenn Sie in Azure Maps auf eins der Verkehrssymbole klicken, werden zusätzliche Informationen in einem Popup angezeigt.

<center>

![Azure Maps-Verkehrsvorfall](media/migrate-google-maps-web-app/azure-maps-traffic-incident.png)</center>

**Zusätzliche Ressourcen:**

- [Anzeigen von Datenverkehr auf einer Karte](map-show-traffic.md)
- [Optionen für Verkehrsdatenüberlagerungen](https://azuremapscodesamples.azurewebsites.net/index.html?sample=Traffic%20Overlay%20Options)

### <a name="add-a-ground-overlay"></a>Hinzufügen einer Bodenüberlagerung

Sowohl Azure- als auch Google-Karten unterstützen das Überlagern von georeferenzierten Bildern auf der Karte, die beim Schwenken und Zoomen der Karte mit bewegt werden. In Google Maps werden diese als Bodenüberlagerungen bezeichnet, während sie in Azure Maps als Bildebenen bezeichnet werden. Diese eigenen sich hervorragend für Grundrisse von Gebäuden, die Überlagerung alter Karten oder Bilder von einer Drohne.

**Vorher: Google Maps**

Beim Erstellen einer Bodenüberlagerung in Google Maps müssen Sie die URL für das zu überlagernde Bild und einen Begrenzungsrahmen für das Anbinden des Bilds an die Karte angeben. In diesem Beispiel wird der Karte ein Kartenbild von [Newark New Jersey von 1922](https://www.lib.utexas.edu/maps/historical/newark_nj_1922.jpg) überlagert.

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <meta charset="utf-8" />
    <meta http-equiv="x-ua-compatible" content="IE=Edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />

    <script type='text/javascript'>
        var map, historicalOverlay;

        function initMap() {
            map = new google.maps.Map(document.getElementById('myMap'), {
                center: new google.maps.LatLng(40.740, -74.18),
                zoom: 12
            });

            var imageBounds = {
                north: 40.773941,
                south: 40.712216,
                east: -74.12544,
                west: -74.22655
            };

            historicalOverlay = new google.maps.GroundOverlay(
                'https://www.lib.utexas.edu/maps/historical/newark_nj_1922.jpg',
                imageBounds);
            historicalOverlay.setMap(map);
        }
    </script>

    <!-- Google Maps Script Reference -->
    <script src="https://maps.googleapis.com/maps/api/js?callback=initMap&key=[Your Google Maps Key]" async defer></script>
</head>
<body>
    <div id="myMap" style="position:relative;width:600px;height:400px;"></div>
</body>
</html>
```

Beim Ausführen dieses Codes in einem Browser wird eine Karte angezeigt, die wie die folgende Abbildung aussieht:

<center>

![Google Maps-Bildüberlagerung](media/migrate-google-maps-web-app/google-maps-image-overlay.png)</center>

**Nachher: Azure Maps**

In Azure Maps können georeferzierte Bilder mithilfe der `atlas.layer.ImageLayer`-Klasse überlagert werden. Diese Klasse erfordert eine URL zu einem Bild und einen Satz von Koordinaten für die vier Ecken des Bilds. Das Bild muss entweder in derselben Domäne gehostet oder CORs-fähig sein.

> [!TIP]
> Wenn Sie nur die Informationen „Nord“, „Süd“, „Ost“, „West“ und „Drehung“, aber keine Koordinaten für die einzelnen Ecken des Bilds haben, können Sie die statische Methode [`atlas.layer.ImageLayer.getCoordinatesFromEdges`](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.imagelayer?view=azure-iot-typescript-latest#getcoordinatesfromedges-number--number--number--number--number-) verwenden.

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <meta charset="utf-8" />
    <meta http-equiv="x-ua-compatible" content="IE=Edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />

    <!-- Add references to the Azure Maps Map control JavaScript and CSS files. -->
    <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.css" type="text/css" />
    <script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.js"></script>

    <script type='text/javascript'>
        var map;

        function initMap() {
            //Initialize a map instance.
            map = new atlas.Map('myMap', {
                center: [-74.18, 40.740],
                zoom: 12,

                //Add your Azure Maps subscription key to the map SDK. Get an Azure Maps key at https://azure.com/maps
                authOptions: {
                    authType: 'subscriptionKey',
                    subscriptionKey: '<Your Azure Maps Key>'
                }
            });

            //Wait until the map resources are ready.
            map.events.add('ready', function () {

                //Create an image layer and add it to the map.
                map.layers.add(new atlas.layer.ImageLayer({
                    url: 'newark_nj_1922.jpg',
                    coordinates: [
                        [-74.22655, 40.773941], //Top Left Corner
                        [-74.12544, 40.773941], //Top Right Corner
                        [-74.12544, 40.712216], //Bottom Right Corner
                        [-74.22655, 40.712216]  //Bottom Left Corner
                    ]
                }));
            });
        }
    </script>
</head>
<body onload="initMap()">
    <div id='myMap' style='position:relative;width:600px;height:400px;'></div>
</body>
</html>
```

<center>

![Azure Maps-Bildüberlagerung](media/migrate-google-maps-web-app/azure-maps-image-overlay.png)</center>

**Zusätzliche Ressourcen:**

- [Überlagern eines Bilds](map-add-image-layer.md)
- [Bildebenenklasse](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.imagelayer?view=azure-iot-typescript-latest)

## <a name="additional-code-samples"></a>Weitere Codebeispiele

Nachfolgend finden Sie einige zusätzliche Codebeispiele im Zusammenhang mit der Google Maps-Migration:

- [Zeichentools](map-add-drawing-toolbar.md)
- [Karte auf Schwenken mit zwei Fingern einschränken](https://azuremapscodesamples.azurewebsites.net/index.html?sample=Limit%20Map%20to%20Two%20Finger%20Panning)
- [Zoom über das Scrollrad einschränken](https://azuremapscodesamples.azurewebsites.net/index.html?sample=Limit%20Scroll%20Wheel%20Zoom)
- [Erstellen eines Vollbild-Steuerelements](https://azuremapscodesamples.azurewebsites.net/index.html?sample=Create%20a%20Fullscreen%20Control)

**Dienste:**

- [Verwenden des Moduls „Dienste“ von Azure Maps](how-to-use-services-module.md)
- [Suchen nach interessanten Punkten](map-search-location.md)
- [Abrufen von Informationen von einer Koordinate (reverse Geocoding)](map-get-information-from-coordinate.md)
- [Anzeigen einer Wegbeschreibung von A nach B](map-route.md)
- [Automatische Suchvorschläge mit der JQuery-Benutzeroberfläche](https://azuremapscodesamples.azurewebsites.net/index.html?sample=Search%20Autosuggest%20and%20JQuery%20UI)

## <a name="google-maps-v3-to-azure-maps-web-sdk-class-mapping"></a>Klassenzuordnung von Google Maps V3 zum Azure Maps Web SDK

Der folgende Anhang bietet eine Zuordnung der am häufigsten verwendeten Klassen in Google Maps V3 zu ihren Äquivalenten im Azure Maps Web SDK in Form von Querverweisen.

### <a name="core-classes"></a>Core-Klassen

| Google Maps   | Azure Maps  |
|---------------|-------------|
| `google.maps.Map` | [atlas.Map](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)  |
| `google.maps.InfoWindow` | [atlas.Popup](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest)  |
| `google.maps.InfoWindowOptions` | [atlas.PopupOptions](https://docs.microsoft.com/) |
| `google.maps.LatLng`  | [atlas.data.Position](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.position?view=azure-iot-typescript-latest)  |
| `google.maps.LatLngBounds` | [atlas.data.BoundingBox](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.boundingbox?view=azure-iot-typescript-latest) |
| `google.maps.MapOptions`  | [atlas.CameraOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.cameraoptions?view=azure-iot-typescript-latest)<br/>[atlas.CameraBoundsOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.cameraboundsoptions?view=azure-iot-typescript-latest)<br/>[atlas.ServiceOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.serviceoptions?view=azure-iot-typescript-latest)<br/>[atlas.StyleOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.styleoptions?view=azure-iot-typescript-latest)<br/>[atlas.UserInteractionOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.userinteractionoptions?view=azure-iot-typescript-latest) |
| `google.maps.Point`  | [atlas.Pixel](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.pixel?view=azure-iot-typescript-latest)   |

## <a name="overlay-classes"></a>Überlagerungsklassen

| Google Maps  | Azure Maps  |
|--------------|-------------|
| `google.maps.Marker` | [atlas.HtmlMarker](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarker?view=azure-iot-typescript-latest)<br/>[atlas.data.Point](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest)  |
| `google.maps.MarkerOptions`  | [atlas.HtmlMarkerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarkeroptions?view=azure-iot-typescript-latest)<br/>[atlas.layer.SymbolLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest)<br/>[atlas.SymbolLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.symbollayeroptions?view=azure-iot-typescript-latest)<br/>[atlas.IconOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.iconoptions?view=azure-iot-typescript-latest)<br/>[atlas.TextOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.textoptions?view=azure-iot-typescript-latest)<br/>[atlas.layer.BubbleLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.bubblelayer?view=azure-iot-typescript-latest)<br/>[atlas.BubbleLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.bubblelayeroptions?view=azure-iot-typescript-latest) |
| `google.maps.Polygon`  | [atlas.data.Polygon](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.polygon?view=azure-iot-typescript-latest)               |
| `google.maps.PolygonOptions` |[atlas.layer.PolygonLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonlayer?view=azure-iot-typescript-latest)<br/> [atlas.PolygonLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.polygonlayeroptions?view=azure-iot-typescript-latest)<br/> [atlas.layer.LineLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.linelayer?view=azure-iot-typescript-latest)<br/> [atlas.LineLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.linelayeroptions?view=azure-iot-typescript-latest)|
| `google.maps.Polyline` | [atlas.data.LineString](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.linestring?view=azure-iot-typescript-latest)         |
| `google.maps.PolylineOptions` | [atlas.layer.LineLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.linelayer?view=azure-maps-typescript-latest)<br/>[atlas.LineLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.linelayeroptions?view=azure-maps-typescript-latest) |
| `google.maps.Circle`  | Mehr dazu finden Sie unter [Hinzufügen eines Kreises zur Karte](map-add-shape.md#add-a-circle-to-the-map)                                     |
| `google.maps.ImageMapType`  | [atlas.TileLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.tilelayer?view=azure-iot-typescript-latest)         |
| `google.maps.ImageMapTypeOptions` | [atlas.TileLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.tilelayeroptions?view=azure-iot-typescript-latest) |
| `google.maps.GroundOverlay`  | [atlas.layer.ImageLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.imagelayer?view=azure-iot-typescript-latest)<br/>[atlas.ImageLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.imagelayeroptions?view=azure-iot-typescript-latest) |

## <a name="service-classes"></a>Dienstklassen

Das Azure Maps Web-SDK enthält ein [Dienstmodul](how-to-use-services-module.md) das separat geladen werden kann. Dieses Modul umschließt die Azure Maps REST-Dienste mit einer Web-API und kann in JavaScript-, TypeScript- und Node.js-Anwendungen verwendet werden.

| Google Maps | Azure Maps  |
|-------------|-------------|
| `google.maps.Geocoder` | [atlas.service.SearchUrl](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchurl?view=azure-iot-typescript-latest)  |
| `google.maps.GeocoderRequest`  | [atlas.SearchAddressOptions](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchaddressoptions?view=azure-iot-typescript-latest)<br/>[atlas.SearchAddressRevrseOptions](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchaddressreverseoptions?view=azure-iot-typescript-latest)<br/>[atlas.SearchAddressReverseCrossStreetOptions](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchaddressreversecrossstreetoptions?view=azure-iot-typescript-latest)<br/>[atlas.SearchAddressStructuredOptions](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchaddressstructuredoptions?view=azure-iot-typescript-latest)<br/>[atlas.SearchAlongRouteOptions](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchalongrouteoptions?view=azure-iot-typescript-latest)<br/>[atlas.SearchFuzzyOptions](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchfuzzyoptions?view=azure-iot-typescript-latest)<br/>[atlas.SearchInsideGeometryOptions](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchinsidegeometryoptions?view=azure-iot-typescript-latest)<br/>[atlas.SearchNearbyOptions](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchnearbyoptions?view=azure-iot-typescript-latest)<br/>[atlas.SearchPOIOptions](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchpoioptions?view=azure-iot-typescript-latest)<br/>[atlas.SearchPOICategoryOptions](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchpoicategoryoptions?view=azure-iot-typescript-latest) |
| `google.maps.DirectionsService`  | [atlas.service.RouteUrl](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.routeurl?view=azure-iot-typescript-latest)  |
| `google.maps.DirectionsRequest`  | [atlas.CalculateRouteDirectionsOptions](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.calculateroutedirectionsoptions?view=azure-iot-typescript-latest) |
| `google.maps.places.PlacesService` | [atlas.service.SearchUrl](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchurl?view=azure-iot-typescript-latest)  |

## <a name="libraries"></a>Bibliotheken

Bibliotheken fügen der Karte zusätzliche Funktionalität hinzu. Viele davon befinden sich im Core-SDK von Azure Maps. Hier sind einige äquivalente Klassen aufgeführt, die anstelle dieser Google Maps-Bibliotheken verwendet werden können.

| Google Maps           | Azure Maps   |
|-----------------------|--------------|
| Zeichnungsbibliothek       | [Zeichentools-Modul](set-drawing-options.md) |
| Geometriebibliothek      | [atlas.math](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.math?view=azure-iot-typescript-latest)   |
| Visualisierungsbibliothek | [Wärmebildebene](map-add-heat-map-layer.md) |

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über das Azure Maps Web SDK.

> [!div class="nextstepaction"]
> [Verwenden des Kartensteuerelements](how-to-use-map-control.md)

> [!div class="nextstepaction"]
> [Verwenden des Dienstmoduls](how-to-use-services-module.md)

> [!div class="nextstepaction"]
> [Verwenden des Zeichentools-Moduls](set-drawing-options.md)

> [!div class="nextstepaction"]
> [Codebeispiele](https://docs.microsoft.com/samples/browse/?products=azure-maps)

