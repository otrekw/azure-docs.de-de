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
ms.openlocfilehash: bdbf2a975cbdc3d06745b9375c1e6f8e751ddfd6
ms.sourcegitcommit: 3c925b84b5144f3be0a9cd3256d0886df9fa9dc0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2020
ms.locfileid: "77914081"
---
# <a name="migrate-a-web-app-from-google-maps"></a>Migrieren einer Web-App aus Google Maps

Die meisten Web-Apps, die Google Maps nutzen, verwenden das Google Maps V3 JavaScript SDK. Das Azure Maps Web SDK ist das passende Azure-basierte SDK, zu dem migriert werden kann. Mit dem Azure Maps Web SDK können Sie interaktive Karten mit eigenen Inhalten und Bildern anpassen. Ihre App kann sowohl in webbasierten als auch in mobilen Anwendungen ausgeführt werden. Dieses Steuerelement nutzt WebGL, was das Rendern umfangreicher Datasets mit hoher Leistung ermöglicht. Verwenden Sie für die Entwicklung mit dem SDK JavaScript oder TypeScript.

Wenn Sie eine vorhandene Webanwendung migrieren, überprüfen Sie, ob die Anwendung eine Open Source-basierte Kartensteuerelement-Bibliothek verwendet. Beispiele für Open Source-basierte Kartensteuerelement-Bibliotheken wären etwa Cesium, Leaflet und OpenLayers. Auch wenn Ihre Anwendung eine Open Source-basierte Kartensteuerelement-Bibliothek verwendet und Sie das Azure Maps Web SDK nicht verwenden möchten, können Sie die Migration trotzdem durchführen. Verbinden Sie Ihre Anwendung in diesem Fall mit den Azure Maps-Kacheldiensten ([Straßenkacheln](https://docs.microsoft.com/rest/api/maps/render/getmaptile) \| [Satellitenkacheln](https://docs.microsoft.com/rest/api/maps/render/getmapimagerytile)). Im Anschluss finden Sie ausführliche Informationen zur Verwendung von Azure Maps in einigen gängigen Open Source-basierten Kartensteuerelement-Bibliotheken:

- Cesium: ein 3D-Kartensteuerelement für das Web. [Codebeispiel](https://azuremapscodesamples.azurewebsites.net/index.html?sample=Raster%20Tiles%20in%20Cesium%20JS) \| [Dokumentation](https://cesiumjs.org/)
- Leaflet: schlankes 2D-Kartensteuerelement für das Web. [Codebeispiel](https://azuremapscodesamples.azurewebsites.net/index.html?sample=Azure%20Maps%20Raster%20Tiles%20in%20Leaflet%20JS) \| [Dokumentation](https://leafletjs.com/)
- OpenLayers: ein 2D-Kartensteuerelement für das Web, das Projektionen unterstützt. [Codebeispiel](https://azuremapscodesamples.azurewebsites.net/index.html?sample=Raster%20Tiles%20in%20OpenLayers) \| [Dokumentation](https://openlayers.org/)

## <a name="key-features-support"></a>Unterstützung wichtiger Funktionen

Die Tabelle enthält die wichtigsten API-Features im Google Maps V3 JavaScript SDK sowie das unterstützte API-Feature im Azure Maps Web SDK:

| Google Maps-Feature     | Azure Maps Web SDK-Unterstützung |
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

Im Anschluss finden Sie einige wichtige Unterschiede zwischen den Web-SDKs für Google Maps und Azure Maps:

- Neben einem gehosteten Endpunkt für den Zugriff auf das Azure Maps Web SDK steht auch ein NPM-Paket zur Verfügung. Betten Sie das Web SDK-Paket in Apps ein. Weitere Informationen finden Sie in dieser [Dokumentation](how-to-use-map-control.md). Dieses Paket enthält außerdem TypeScript-Definitionen.
- Erstellen Sie zunächst eine Instanz der Map-Klasse in Azure Maps. Warten Sie, bis das Kartenereignis `ready` oder `load` ausgelöst wurde, bevor Sie programmgesteuert mit der Karte interagieren. Dadurch wird sichergestellt, dass alle Kartenressourcen geladen wurden und für den Zugriff bereit sind.
- Das Kachelsystem für die Basiskarten ist bei beide Plattformen ähnlich. Die Kacheln in Google Maps haben jedoch eine Größe von 256 Pixeln, während die Kachelgröße in Azure Maps 512 Pixel beträgt. Verringern Sie daher in Azure Maps die Google Maps-Zoomstufe um den Wert 1, um in Azure Maps die gleiche Kartenansicht zu erhalten wie in Google Maps.
- Koordinaten werden in Google Maps im Format „Breitengrad, Längengrad“ angegeben. In Azure Maps wird dagegen das Format „Längengrad,Breitengrad“ verwendet. Das Format von Azure Maps entspricht dem Standardformat `[x, y]`, das von den meisten GIS-Plattformen genutzt wird.
- Formen im Azure Maps Web-SDK basieren auf dem GeoJSON-Schema. Hilfsklassen werden über den [*atlas.data*-Namespace](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data?view=azure-iot-typescript-latest) verfügbar gemacht. Darüber hinaus gibt es die Klasse [*atlas.Shape*](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape). Verwenden Sie diese Klasse, um GeoJSON-Objekten zu umschließen. Dies vereinfacht die Aktualisierung und Verwaltung auf eine für Datenbindungen geeignete Weise.
- Koordinaten werden in Azure Maps als Positionsobjekte definiert. Eine Koordinate wird als Zahlenarray im Format `[longitude,latitude]` angegeben. Alternativ kann das neue Format „atlas.data.Position(Längengrad, Breitengrad)“ verwendet werden.
    > [!TIP]
    > Die Position-Klasse weist eine statische Hilfsmethode zum Importieren von Koordinaten im Format „Breitengrad, Längengrad“ auf. Die Methode [atlas.data.Position.fromLatLng](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.position?view=azure-iot-typescript-latest) kann in vielen Fällen durch die Methode `new google.maps.LatLng` in Google Maps-Code ersetzt werden.
- Anstatt Formatierungsinformationen für jede Form anzugeben, die der Karte hinzugefügt wird, trennt Azure Maps Formatvorlagen von den Daten. Daten werden in Datenquellen gespeichert und sind mit Renderingebenen verbunden. In Azure Maps-Code werden Datenquellen zum Rendern der Daten verwendet. Dieser Ansatz bietet den Vorteil besserer Leistung. Darüber hinaus unterstützen viele Ebenen ein datengesteuertes Format, bei dem Ebenenformatoptionen mit Geschäftslogik versehen werden können. Durch diese Unterstützung ändert sich die Art und Weise, wie individuelle Formen innerhalb einer Ebene auf der Grundlage der in der Form definierten Eigenschaften gerendert werden.

## <a name="web-sdk-side-by-side-examples"></a>Beispiele zum Web SDK in der Gegenüberstellung

Diese Sammlung enthält Codebeispiele für jede Plattform sowie gängige Anwendungsfälle. Die Beispiele helfen Ihnen bei der Migration Ihrer Webanwendung vom Google Maps V3 JavaScript SDK zum Azure Maps Web SDK. Codebeispiele für Webanwendungen werden in JavaScript bereitgestellt. Darüber hinaus bietet Azure Maps aber auch TypeScript-Definitionen über ein [NPM-Modul](how-to-use-map-control.md).

### <a name="load-a-map"></a>Laden einer Karte

Die Schritte zum Laden einer Karte sind bei beiden SDKs identisch:

- Hinzufügen eines Verweises auf das Karten-SDK.
- Hinzufügen eines Tags vom Typ `div` zum Textkörper der Seite. (Das Tag fungiert als Platzhalter für die Karte.)
- Erstellen einer JavaScript-Funktion, die nach dem Laden der Seite aufgerufen wird.
- Erstellen einer Instanz der jeweiligen Kartenklasse.

**Einige wesentliche Unterschiede**

- Für Google Maps muss im Skriptverweis der API ein Kontoschlüssel angegeben werden. Authentifizierungs-Anmeldeinformationen für Azure Maps werden als Optionen der Kartenklasse angegeben. Bei diesen Anmeldeinformationen kann es sich um einen Abonnementschlüssel oder um Azure Active Directory-Informationen handeln.
- Google Maps akzeptiert eine Rückruffunktion im Skriptverweis der API, um eine Initialisierungsfunktion zum Laden der Karte aufzurufen. Bei Azure Maps sollte das onload-Ereignis der Seite verwendet werden.
- Beim Verweisen auf das `div`-Element, in dem die Karte gerendert werden soll, benötigt die `Map`-Klasse in Azure Maps lediglich den `id`-Wert, während Google Maps ein `HTMLElement`-Objekt erfordert.
- Koordinaten werden in Azure Maps als Positionsobjekte definiert, die als einfaches Zahlenarray im Format `[longitude, latitude]` angegeben werden können.
- Die Zoomstufe in Azure Maps ist eine Stufe niedriger als in Google Maps. Diese Abweichung ist auf den Größenunterschied zwischen den Kachelsystemen der beiden Plattformen zurückzuführen.
- Von Azure Maps werden der Kartencanvas standardmäßig keine Navigationssteuerelemente hinzugefügt. Eine Karte verfügt also standardmäßig über keine Schaltflächen zum Zoomen oder zum Anpassen des Kartenstils. Es gibt jedoch Steuerelementoptionen, mit denen Sie ein Auswahlelement für den Kartenstil sowie Zoomschaltflächen, ein Kompass- oder Drehsteuerelement und ein Neigungssteuerelement hinzufügen können.
- In Azure Maps wird ein Ereignishandler hinzugefügt, um das `ready`-Ereignis der Karteninstanz zu überwachen. Dieses Ereignis wird ausgelöst, wenn der WebGL-Kontext und alle benötigten Ressourcen geladen wurden. Fügen Sie diesem Ereignishandler jeglichen Code hinzu, der nach dem vollständigen Laden der Karte ausgeführt werden soll.

In den folgenden Beispielen wird unter Verwendung von Google Maps eine Karte geladen, die an bestimmten Koordinaten über New York zentriert ist (Längengrad: -73,985; Breitengrad: 40,747; Zoomfaktor der Karte: 12).

**Vorher: Google Maps**

Zeigen Sie eine über einem bestimmten Ort zentrierte und gezoomte Karte in Google Maps an.

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

Laden Sie in Azure Maps eine Karte mit der gleichen Ansicht sowie mit einem Stilsteuerelement und Zoomschaltflächen für die Karte.

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

Eine ausführliche Dokumentation zum Einrichten und Verwenden des Azure Maps-Kartensteuerelements in einer Web-App finden Sie [hier](how-to-use-map-control.md).

> [!NOTE]
> Im Gegensatz zu Google Maps ist bei Azure Maps keine Angabe einer Anfangszentrierung und einer Anfangszoomstufe erforderlich, wenn die Karte geladen wird. Wenn diese Informationen beim Laden der Karte nicht angegeben werden, versucht Azure Maps zu bestimmen, in welcher Stadt sich der Benutzer befindet. Die Karte wird dann über diesem Ort zentriert und entsprechend gezoomt.

**Zusätzliche Ressourcen:**

- Azure Maps bietet darüber hinaus Navigationssteuerelemente zum Drehen und Neigen der Kartenansicht, wie [hier](map-add-controls.md) dokumentiert.

### <a name="localizing-the-map"></a>Lokalisieren der Karte

Wenn sich Ihre Zielgruppe über mehrere Länder erstreckt oder verschiedene Sprachen spricht, ist Lokalisierung ein wichtiger Punkt.

**Vorher: Google Maps**

Lokalisieren von Google Maps durch Hinzufügen von Sprach- und Regionsparametern:

```html
<script type="text/javascript" src=" https://maps.googleapis.com/maps/api/js?callback=initMap&key=[api_key]& language=[language_code]&region=[region_code]" async defer></script>
```

Dies ist ein Beispiel für Google Maps mit der Spracheinstellung „fr-FR“.

<center>

![Lokalisierung in Google Maps](media/migrate-google-maps-web-app/google-maps-localization.png)</center>

**Nachher: Azure Maps**

Azure Maps bietet zwei verschiedene Möglichkeiten zum Festlegen der Sprache und der regionalen Ansicht für die Karte. Die erste Option besteht darin, diese Informationen dem globalen *Atlas*-Namespace hinzuzufügen. Dies führt dazu, dass alle Kartensteuerelement-Instanzen in Ihrer App standardmäßig auf diese Einstellungen festgelegt werden. Im Folgenden wird die Sprache auf Französisch („fr-FR“) und die regionale Ansicht auf „Auto“ festgelegt:

```javascript
atlas.setLanguage('fr-FR');
atlas.setView('auto');
```

Die zweite Option besteht darin, diese Informationen beim Laden der Karte an die Kartenoptionen zu übergeben. Dies sieht folgendermaßen aus:

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
> Mit Azure Maps ist es möglich, auf der gleichen Seite mehrere Karteninstanzen mit unterschiedlichen Sprach- und Regionseinstellungen zu laden. Diese Einstellungen können auch nach dem Laden der Karte aktualisiert werden. 

Eine ausführliche Liste der in Azure Maps unterstützten Sprachen finden Sie [hier](supported-languages.md).

Hier sehen Sie ein Beispiel für Azure Maps mit der Spracheinstellung „fr“ und der auf „fr-FR“ festgelegten Benutzerregion.

<center>

![Lokalisierung in Azure Maps](media/migrate-google-maps-web-app/azure-maps-localization.png)</center>

### <a name="setting-the-map-view"></a>Festlegen der Kartenansicht

Dynamische Karten können sowohl in Azure Maps als auch in Google Maps programmgesteuert an neue geografische Orte verschoben werden. Rufen Sie hierzu die entsprechenden Methoden in JavaScript auf. In den Beispielen wird gezeigt, wie Sie in Google Maps eine Karte mit Satellitenluftaufnahmen anzeigen, die Karte über einem Ort zentrieren und die Zoomstufe auf „15“ festlegen. Der Ort hat die Koordinaten „-111,0225“ (Längengrad) und „35,0272“ (Breitengrad).

> [!NOTE]
> In Google Maps werden Kacheln mit einer Größe von 256 Pixeln verwendet. In Azure Maps sind die Kacheln dagegen größer (512 Pixel). Azure Maps kann daher mit weniger Netzwerkanforderungen den gleiche Kartenbereich laden wie Google Maps. Aufgrund der Funktionsweise von Kachelpyramiden in Kartensteuerelementen muss bei Verwendung von Azure Maps die in Google Maps verwendete Zoomstufe um den Wert 1 verringert werden. Diese arithmetische Operation sorgt dafür, dass von den größeren Kacheln in Azure Maps der gleiche Kartenbereich gerendert wird wie in Google Maps.

**Vorher: Google Maps**

Verschieben Sie das Google Maps-Kartensteuerelement mithilfe der Methode `setOptions`. Diese Methode ermöglicht die Angabe des Kartenmittelpunkts und einer Zoomstufe.

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

Ändern Sie in Azure Maps die Kartenposition mithilfe der Methode `setCamera` und den Kartenstil mithilfe der Methode `setStyle`. Die Koordinaten in Azure Maps werden im Format „Längengrad, Breitengrad“ angegeben, und der Wert für die Zoomstufe ist um eine Stufe niedriger.

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

In Azure Maps können Punktdaten auf mehrere Arten auf der Karte gerendert werden:

- **HTML-Marker**: Rendering von Punkten mithilfe herkömmlicher DOM-Elemente. HTML-Markers unterstützten Ziehbewegungen.
- **Symbolebene**: Rendering von Punkten als Symbol oder Text im WebGL-Kontext.
- **Blasenebene**: Punktdaten werden als Kreise auf der Karte gerendert. Die Radien der Kreise können auf der Grundlage von Eigenschaften in den Daten skaliert werden.

Rendern Sie Symbol- und Blasenebenen innerhalb des WebGL-Kontexts. Von beiden Ebenen können große Punktmengen auf der Karte gerendert werden. Für diese Ebenen ist die Speicherung der Daten in einer Datenquelle erforderlich. Die Datenquellen und Renderingebenen sollten der Karte nach dem Auslösen des `ready`-Ereignisses hinzugefügt werden. HTML-Marker werden als DOM-Elemente auf der Seite gerendert und verwenden keine Datenquelle. Je mehr DOM-Elemente eine Seite aufweist, desto langsamer wird sie. Wenn mehr als ein paar hundert Punkte auf einer Karte gerendert werden sollen, empfiehlt es sich, stattdessen eine der Renderingebenen zu verwenden.

Zu diesem Zweck fügen wir der Karte einen Marker mit der Zahl 10 als Bezeichnungsoverlay hinzu. Verwenden Sie den Längengrad „-0,2“ und den Breitengrad „51,5“.

**Vorher: Google Maps**

Verwenden Sie bei Google Maps die Klasse `google.maps.Marker`, um der Karte Marker hinzuzufügen, und geben Sie die Karte als eine der Optionen an.

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

Verwenden Sie bei Azure Maps HTML-Marker, um einen Punkt auf der Karte anzuzeigen. HTML-Marker werden für Apps empfohlen, die nur eine geringe Anzahl von Punkten auf der Karte darstellen müssen. Wenn Sie einen HTML-Marker verwenden möchten, erstellen Sie eine Instanz der Klasse `atlas.HtmlMarker`. Legen Sie die Optionen für Text und Position fest, und fügen Sie der Karte den Marker mithilfe der Methode `map.markers.add` hinzu.

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

Wenn Sie eine Symbolebene verwenden möchten, fügen Sie die Daten einer Datenquelle hinzu. Fügen Sie die Datenquelle an die Ebene an. Außerdem sollten die Datenquelle und die Ebene der Karte nach dem Auslösen des `ready`-Ereignisses hinzugefügt werden. Zum Rendern eines eindeutigen Textwerts über einem Symbol müssen die Textinformationen als Eigenschaft des Datenpunkts gespeichert werden. Auf die Eigenschaft muss in der Option `textField` der Ebene verwiesen werden. Diese Vorgehensweise ist zwar etwas aufwendiger als die Verwendung von HTML-Markern, trägt aber zur Verbesserung der Leistung bei.

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

Sie können benutzerdefinierte Bilder verwenden, um Punkte auf einer Karte darzustellen. Im folgenden Beispiel wird ein benutzerdefiniertes Bild verwendet, um einen Punkt auf der Karte anzuzeigen. Der Punkt wird an folgenden Koordinaten angezeigt: „51,5“ (Breitengrad) und „-0,2“ (Längengrad). Die Position des Markers wird durch den Anker so angepasst, dass die Spitze des Stecknadelsymbols auf die korrekte Kartenposition ausgerichtet ist.

<center>

![Bild: gelbe Stecknadel](media/migrate-google-maps-web-app/ylw_pushpin.png)<br/>
ylw\_pushpin.png</center>

**Vorher: Google Maps**

Erstellen Sie einen benutzerdefinierten Marker durch Angeben eines Objekts vom Typ `Icon`, das die URL (`url`) des Bilds enthält. Geben Sie einen Ankerpunkt (`anchor`) an, um die Spitze des Stecknadelbilds an der Koordinate auf der Karte auszurichten. Der Ankerwert in Google Maps wird relativ zur linken oberen Ecke des Bilds angegeben.

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

Übergeben Sie zum Anpassen eines HTML-Markers ein HTML-Element vom Typ `string` oder `HTMLElement` an die Option `htmlContent` des Markers. Verwenden Sie die Option `anchor`, um die Position des Markers relativ zur Positionskoordinate anzugeben. Weisen Sie der Option `anchor` einen von neun definierten Referenzpunkten zu. Folgende Punkte sind definiert: „center“, „top“, „bottom“, „left“, „right“, „top-left“, „top-right“, „bottom-left“, „bottom-right“. Die Inhalte werden standardmäßig unten in der Mitte des HTML-Inhalts verankert. Um die Migration von Code aus Google Maps zu vereinfachen, legen Sie den `anchor` auf „top-left“ fest, und verwenden Sie dann die Option `pixelOffset` mit dem gleichen Offset, der in Google Maps verwendet wird. Die Offsets in Azure Maps und Google Maps bewegen sich in entgegengesetzte Richtungen. Multiplizieren Sie die Offsets daher mit -1.

> [!TIP]
> Fügen Sie im HTML-Inhalt `pointer-events:none` als Stil hinzu, um das Standardverhalten beim Ziehen in Microsoft Edge zu deaktivieren, durch das ein unerwünschtes Symbol angezeigt wird.

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

Benutzerdefinierte Bilder werden auch von Symbolebenen in Azure Maps unterstützt. Laden Sie zunächst das Bild in die Kartenressourcen, und weisen Sie ihm eine eindeutige ID zu. Verweisen Sie auf der Symbolebene auf das Bild. Richten Sie das Bild mithilfe der Option `offset` auf den korrekten Punkt auf der Karte aus. Verwenden Sie die Option `anchor`, um die Position des Symbols relativ zu den Positionskoordinaten anzugeben. Verwenden Sie einen der neun definierten Referenzpunkte: „center“, „top“, „bottom“, „left“, „right“, „top-left“, „top-right“, „bottom-left“, „bottom-right“. Die Inhalte werden standardmäßig unten in der Mitte des HTML-Inhalts verankert. Um die Migration von Code aus Google Maps zu vereinfachen, legen Sie den `anchor` auf „top-left“ fest, und verwenden Sie dann die Option `offset` mit dem gleichen Offset, der in Google Maps verwendet wird. Die Offsets in Azure Maps und Google Maps bewegen sich in entgegengesetzte Richtungen. Multiplizieren Sie die Offsets daher mit -1.

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
> Kombinieren Sie mehrere Renderingebenen miteinander, um komplexe benutzerdefinierte Punkte zu rendern. Ein Beispiel: Angenommen, Sie möchten mehrere Ortsmarken mit dem gleichen Symbol in Kreisen mit unterschiedlichen Farben verwenden. In diesem Fall können Sie zusätzlich zu einer Blasenebene eine Symbolebene hinzufügen, anstatt jeweils eine Reihe von Bildern für die einzelnen Farbüberlagerungen zu erstellen. Lassen Sie die Stecknadeln auf die gleiche Datenquelle verweisen. Diese Vorgehensweise ist viel effizienter als die Erstellung und Verwaltung zahlreicher verschiedener Bilder.

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

Verwenden Sie Polylinien, um eine Linie oder einen Pfad auf der Karte darzustellen. In diesem Beispiel wird eine gestrichelte Polylinie auf der Karte erstellt.

**Vorher: Google Maps**

Die Polyline-Klasse akzeptiert eine Reihe von Optionen. Übergeben Sie in der Option `path` der Polylinie ein Array mit Koordinaten.

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

Polylinien werden als Objekte vom Typ `LineString` oder `MultiLineString` bezeichnet. Diese Objekte können einer Datenquelle hinzugefügt und mithilfe einer Linienebene gerendert werden. Fügen Sie `LineString` einer Datenquelle und die Datenquelle anschließend einer Linienebene (`LineLayer`) hinzu, um sie zu rendern.

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

Die Polygonunterstützung ist in Azure Maps und Google Maps ähnlich. Polygone werden verwendet, um einen Bereich auf der Karte darzustellen. In den folgenden Beispielen wird gezeigt, wie ein Polygon erstellt wird, das ein Dreieck bildet, das auf der Mittelpunktkoordinate der Karte basiert.

**Vorher: Google Maps**

Die Polygon-Klasse akzeptiert eine Reihe von Optionen. Übergeben Sie ein Array mit Koordinaten an die Option `paths` des Polygons.

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

Fügen Sie einer Datenquelle ein Objekt vom Typ `Polygon` oder `MultiPolygon` hinzu. Rendern Sie das Objekt mithilfe von Ebenen auf der Karte. Rendern Sie den Bereich eines Polygons mithilfe einer Polygonebene. Und: Rendern Sie den Umriss eines Polygons mithilfe einer Linienebene.

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

Zusätzliche Informationen zu einer Entität können in Google Maps als Klasse vom Typ `google.maps.InfoWindow` auf der Karte angezeigt werden. In Azure Maps kann dafür die Klasse `atlas.Popup` verwendet werden. In den nächsten Beispielen wird der Karte ein Marker hinzugefügt. Beim Klicken auf den Marker wird ein Infofenster oder ein Popup angezeigt.

**Vorher: Google Maps**

Instanziieren Sie ein Infofenster mithilfe des Konstruktors `google.maps.InfoWindow`.

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

Wir verwenden ein Popup, um zusätzliche Ortsinformationen anzuzeigen. Übergeben Sie ein HTML-Objekt vom Typ `string` oder `HTMLElement` an die Option `content` des Popups. Popups können auf Wunsch unabhängig von einer Form angezeigt werden. Daher muss für Popups ein Wert vom Typ `position` angegeben werden. Geben Sie den Positionswert (`position`) an. Rufen Sie zum Anzeigen eines Popups die Methode `open` auf, und übergeben Sie die Karte (`map`), auf der das Popup angezeigt werden soll.

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
> Das Gleiche funktioniert auch mit einer Symbol-, Blasen-, Linien- oder Polygonebene. Übergeben Sie anstelle eines Markers einfach die gewünschte Ebene an den Ereigniscode der Karte.

**Zusätzliche Ressourcen:**

- [Hinzufügen eines Popups](map-add-popup.md)
- [Popup mit Medieninhalt](https://azuremapscodesamples.azurewebsites.net/index.html?sample=Popup%20with%20Media%20Content)
- [Popups für Formen](https://azuremapscodesamples.azurewebsites.net/index.html?sample=Popups%20on%20Shapes)
- [Wiederverwenden eines Popups für mehrere Ortsmarken](https://azuremapscodesamples.azurewebsites.net/index.html?sample=Reusing%20Popup%20with%20Multiple%20Pins)
- [Popup-Klasse](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest)
- [Popupoptionen](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popupoptions?view=azure-iot-typescript-latest)

### <a name="import-a-geojson-file"></a>Importieren einer GeoJSON-Datei

Google Maps unterstützt das Laden und dynamische Formatieren von GeoJSON-Daten mithilfe der `google.maps.Data`-Klasse. Die Funktionalität dieser Klasse orientiert sich viel stärker an der datengesteuerten Formatierung von Azure Maps. Es gibt jedoch einen wichtigen Unterschied: Bei Google Maps wird eine Rückruffunktion angegeben. Die Geschäftslogik zur Gestaltung des jeweiligen Features wird individuell im Benutzeroberflächenthread verarbeitet. In Azure Maps unterstützen Ebenen dagegen das Angeben von datengesteuerten Ausdrücken als Formatierungsoptionen. Diese Ausdrücke werden zur Renderzeit in einem separaten Thread verarbeitet. Die Methode von Azure Maps trägt zur Verbesserung der Renderingleistung bei. Dies macht sich durch schnelleres Rendering größerer Datasets bemerkbar.

In den folgenden Beispielen wird ein GeoJSON-Feed aller Erdbeben der letzten sieben Tage aus dem USGS geladen. Erdbebendaten werden in Form skalierter Kreise auf der Karte gerendert. Farbe und Maßstab jedes Kreises basieren auf der Magnitude der einzelnen Erdbeben, die in der Eigenschaft `"mag"` jedes Features im Dataset gespeichert ist. Ist die Magnitude größer oder gleich fünf, ist der Kreis rot. Zwischen drei und fünf ist der Kreis orange. Und unter drei ist der Kreis grün. Der Radius der einzelnen Kreise entspricht dem Exponentialwert der Magnitude, multipliziert mit 0,1.

**Vorher: Google Maps**

Geben Sie eine einzelne Rückruffunktion in der Methode `map.data.setStyle` an. Wenden Sie innerhalb der Rückruffunktion Geschäftslogik auf die einzelnen Features an. Laden Sie mithilfe der Methode `map.data.loadGeoJson` den GeoJSON-Feed.

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

GeoJSON ist der Basisdatentyp in Azure Maps. Importieren Sie ihn mithilfe der Methode `datasource.importFromUrl` in eine Datenquelle. Verwenden Sie eine Blasenebene. Die Blasenebene stellt Funktionen bereit, um basierend auf den Eigenschaften der Features in einer Datenquelle skalierte Kreise zu rendern. Statt eine Rückruffunktion zu verwenden, wird die Geschäftslogik in einen Ausdruck konvertiert und an die Formatoptionen übergeben. Mithilfe von Ausdrücken wird definiert, wie die Geschäftslogik funktioniert. Ausdrücke können an einen anderen Thread übergeben und anhand der Featuredaten ausgewertet werden. Azure Maps können mehrere Datenquellen und Ebenen mit jeweils eigener Geschäftslogik hinzugefügt werden. Dadurch können auf der Karte mehrere Datasets auf unterschiedliche Weise gerendert werden.

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

Wenn auf der Karte viele Datenpunkte visualisiert werden, kann es zu Überlappungen kommen. Dadurch wird die Karte unübersichtlich, und die Benutzerfreundlichkeit leidet. Beim Clustering von Punktdaten werden benachbarte Datenpunkte miteinander kombiniert und auf der Karte als einzelner gruppierter Datenpunkt dargestellt. Wenn der Benutzer in die Karte zoomt, werden die Cluster in ihre einzelnen Datenpunkte unterteilt. Gruppierte Datenpunkte tragen zur Verbesserung der Benutzerfreundlichkeit und der Leistung der Karte bei.

In den folgenden Beispielen wird durch den Code ein GeoJSON-Feed der Erdbebendaten der letzten Woche geladen und der Karte hinzugefügt. Cluster werden als skalierte, farbige Kreise gerendert. Skalierung und Farbe der Kreise hängen von der enthaltenen Punktanzahl ab.

> [!NOTE]
> Die Clusteringalgorithmen von Google Maps und Azure Maps unterscheiden sich geringfügig. Folglich unterscheidet sich manchmal auch die Punktverteilung in den Clustern.

**Vorher: Google Maps**

Verwenden Sie die MarkerCluster-Bibliothek, um Markercluster zu erstellen. Clustersymbole sind auf Bilder beschränkt, die mit Zahlen von Eins bis Fünf benannt sind. Sie werden im gleichen Verzeichnis gehostet.

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

Fügen Sie einer Datenquelle Daten hinzu, und verwalten Sie sie. Verbinden Sie Datenquellen und Ebenen, und rendern Sie dann die Daten. Die `DataSource`-Klasse in Azure Maps bietet mehrere Clusteringoptionen.

- `cluster`: weist die Datenquelle an, Punktdaten zu gruppieren.
- `clusterRadius`: der Radius in Pixeln zum Gruppieren von Punkten.
- `clusterMaxZoom`: die maximale Zoomstufe, bei der Clustering erfolgt. Bei Überschreiten dieser Zoomstufe werden alle Punkte als Symbole gerendert.
- `clusterProperties`: Definiert benutzerdefinierte Eigenschaften, die mithilfe von Ausdrücken für alle Punkte in jedem Cluster berechnet und den Eigenschaften jedes Clusterpunkts hinzugefügt werden.

Bei aktiviertem Clustering sendet die Datenquelle gruppierte und nicht gruppierte Datenpunkte zum Rendern an Ebenen. Die Datenquelle kann Hunderttausende von Datenpunkten gruppieren. Ein gruppierter Datenpunkt verfügt über folgende Eigenschaften:

| Eigenschaftenname             | type    | Beschreibung   |
|---------------------------|---------|---------------|
| `cluster`                 | boolean | Gibt an, ob das Feature einen Cluster darstellt. |
| `cluster_id`              | Zeichenfolge  | Eine eindeutige ID für den Cluster, die mit den DataSource-Methoden `getClusterExpansionZoom`, `getClusterChildren` und `getClusterLeaves` verwendet werden kann. |
| `point_count`             | number  | Die Anzahl der Punkte, die der Cluster enthält.  |
| `point_count_abbreviated` | Zeichenfolge  | Eine Zeichenfolge, die den `point_count`-Wert kürzt, falls zu lang. (Beispiel: 4.000 wird zu 4K)  |

Die `DataSource`-Klasse weist die folgende Hilfsfunktion für den Zugriff auf zusätzliche Informationen zu einem Cluster mithilfe der `cluster_id` auf.

| Methode | Rückgabetyp | BESCHREIBUNG |
|--------|-------------|-------------|
| `getClusterChildren(clusterId: number)` | Promise&lt;Array&lt;Feature&lt;Geometry, any&gt;\| Shape&gt;&gt; | Ruft die untergeordneten Elemente des angegebenen Clusters für den nächsten Zoomfaktor ab. Diese untergeordneten Elemente können eine Kombination aus Formen und untergeordneten Clustern sein. Die untergeordneten Cluster sind Features mit Eigenschaften, die ClusteredProperties entsprechen. |
| `getClusterExpansionZoom(clusterId: number)` | Promise&lt;number&gt; | Berechnet einen Zoomfaktor, bei dem der Cluster mit der Erweiterung oder Unterteilung beginnt. |
| `getClusterLeaves(clusterId: number, limit: number, offset: number)` | Promise&lt;Array&lt;Feature&lt;Geometry, any&gt;\| Shape&gt;&gt; | Ruft alle Punkte in einem Cluster ab. Legen Sie `limit` fest, um eine Teilmenge der Punkte zurückzugeben, und verwenden Sie `offset`, um die Punkte zu durchlaufen. |

Beim Rendern gruppierter Daten auf der Karte empfiehlt es sich häufig, zwei oder mehr Ebenen zu verwenden. Im folgenden Beispiel werden drei Ebenen verwendet: Eine Blasenebene, um skalierte farbige Kreise auf der Grundlage der Clustergröße zu zeichnen. Eine Symbolebene, um die Clustergröße als Text zu rendern. Und eine zweite Symbolebene, um die nicht gruppierten Punkte zu rendern. Gruppierte Daten können auch auf viele andere Arten gerendert werden. Weitere Informationen finden Sie in der [Dokumentation zu Clusterpunktdaten](clustering-point-data-web-sdk.md).

Importieren Sie GeoJSON-Daten mithilfe der Funktion `importDataFromUrl` der Klasse `DataSource` direkt innerhalb der Azure Maps-Karte.

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

Bei Wärmebildern (auch Punktdichtekarten genannt) handelt es sich um einen Datenvisualisierungstyp. Sie dienen dazu, die Dichte von Daten mithilfe von Farbbereichen darzustellen. Und sie werden häufig eingesetzt, um die Hotspots der Daten auf einer Karte zu zeigen. Wärmebilder eignen sich gut zum Rendern großer Punktdatasets.

In den folgenden Beispielen wird ein GeoJSON-Feed aller Erdbeben im letzten Monat von der USGS geladen und als gewichtetes Wärmebild gerendert. Als Gewichtung wird die Eigenschaft `"mag"` verwendet.

**Vorher: Google Maps**

Laden Sie zum Erstellen eines Wärmebilds die Visualisierungsbibliothek, indem Sie `&libraries=visualization` zur URL des API-Skripts hinzufügen. Die Wärmebildebene in Google Maps unterstützt GeoJSON-Daten nicht direkt. Laden Sie zunächst die Daten herunter, und konvertieren Sie sie in ein Array gewichteter Datenpunkte:

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

Laden Sie die GeoJSON-Daten in eine Datenquelle, und verbinden Sie die Datenquelle mit einer Wärmebildebene. Die Eigenschaft, die für die Gewichtung verwendet werden soll, kann der `weight`-Option mithilfe eines Ausdrucks übergeben werden. Importieren Sie GeoJSON-Daten mithilfe der Funktion `importDataFromUrl` der Klasse `DataSource` direkt in Azure Maps.

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

Die Kachelebenen aus Azure Maps werden in Google Maps als Bildüberlagerungen bezeichnet. Kachelebenen ermöglichen es Ihnen, große Bilder zu überlagern, die in kleinere gekachelte Bilder aufgeteilt wurden, die sich am Kachelsystem der Karte orientieren. Diese Methode wird häufig zur Überlagerung großer Bilder oder Datasets verwendet.

In den folgenden Beispielen wird eine Kachelebene eines Wetterradars aus dem Iowa Environmental Mesonet der Iowa State University überlagert.

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

Fügen Sie der Karte eine Kachelebene hinzu. Das funktioniert ähnlich wie bei allen anderen Ebenen. Verwenden Sie eine formatierte URL mit Platzhaltern für x, y und Zoomfaktor (`{x}`, `{y}` und `{z}`), um anzugeben, wo die Ebene auf die Kacheln zugreifen soll. Kachelebenen in Azure Maps unterstützen außerdem Platzhalter vom Typ `{quadkey}`, `{bbox-epsg-3857}` und `{subdomain}`.

> [!TIP]
> In Azure Maps können Ebenen leicht unterhalb von anderen Ebenen gerendert werden, einschließlich Basiskartenebenen. Es ist häufig wünschenswert, Kachelebenen unterhalb der Kartenbezeichnungen zu rendern, damit sie leicht zu lesen sind. Die Methode `map.layers.add` nimmt einen zweiten Parameter an, bei dem es sich um die ID der Ebene handelt, unter der die neue Ebene eingefügt werden soll. Mithilfe des folgenden Codes können Sie eine Kachelebene unterhalb der Kartenbezeichnungen einfügen: `map.layers.add(myTileLayer, "labels");`

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

Verwenden Sie die Verkehrsebene, um die Karte mit Verkehrsdaten zu überlagern.

```javascript
var trafficLayer = new google.maps.TrafficLayer();
trafficLayer.setMap(map);
```

<center>

![Verkehrsinformationen in Google Maps](media/migrate-google-maps-web-app/google-maps-traffic.png)</center>

**Nachher: Azure Maps**

Azure Maps bietet verschiedene Optionen zum Anzeigen von Verkehrsinformationen. Zeigen Sie Ereignisse wie etwa Straßensperrungen und Unfälle als Symbole auf der Karte an. Überlagern Sie die Karte mit dem Verkehrsfluss und farbcodierten Straßen. Die Farben können basierend auf der geltenden Geschwindigkeitsbegrenzung, der normalerweise zu erwartenden Verzögerung oder der absoluten Verzögerung geändert werden. Vorfallsdaten in Azure Maps werden im Minutentakt aktualisiert, Daten zum Verkehrsfluss alle zwei Minuten.

Weisen Sie für Optionen vom Typ `setTraffic` die gewünschten Werte zu.

```javascript
map.setTraffic({
    incidents: true,
    flow: 'relative'
});
```

<center>

![Verkehrsinformationen in Azure Maps](media/migrate-google-maps-web-app/azure-maps-traffic.png)</center>

Wenn Sie in Azure Maps auf eins der Verkehrssymbole klicken, werden zusätzliche Informationen in einem Popup angezeigt.

<center>

![Azure Maps-Verkehrsvorfall](media/migrate-google-maps-web-app/azure-maps-traffic-incident.png)</center>

**Zusätzliche Ressourcen:**

- [Anzeigen von Datenverkehr auf einer Karte](map-show-traffic.md)
- [Optionen für Verkehrsdatenüberlagerungen](https://azuremapscodesamples.azurewebsites.net/index.html?sample=Traffic%20Overlay%20Options)

### <a name="add-a-ground-overlay"></a>Hinzufügen einer Bodenüberlagerung

Sowohl Azure Maps als auch Google Maps unterstützt die Überlagerung der Karte mit georeferenzierten Bildern. Georeferenzierte Bilder werden beim Schwenken und Zoomen der Karte entsprechend bewegt und skaliert. In Google Maps werden georeferenzierte Bilder als Bodenüberlagerungen bezeichnet. In Azure Maps heißen sie Bildebenen. Sie eigenen sich hervorragend für Grundrisse von Gebäuden, die Überlagerung alter Karten oder Bilder von einer Drohne.

**Vorher: Google Maps**

Geben Sie die URL für das zu überlagernde Bild und einen Begrenzungsrahmen an, um das Bild auf der Karte zu binden. In diesem Beispiel wird der Karte ein Kartenbild von [Newark New Jersey von 1922](https://www.lib.utexas.edu/maps/historical/newark_nj_1922.jpg) überlagert.

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

Verwenden Sie die Klasse `atlas.layer.ImageLayer`, um georeferenzierte Bilder zu überlagern. Diese Klasse erfordert eine URL zu einem Bild und einen Satz von Koordinaten für die vier Ecken des Bilds. Das Bild muss entweder in derselben Domäne gehostet oder CORs-fähig sein.

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

## <a name="add-kml-to-the-map"></a>Hinzufügen von KML zur Karte

Sowohl für Karten von Azure als auch von Google können KML-, KMZ- und GeoRSS-Daten in die Karte importiert und gerendert werden. Azure Maps unterstützt auch GPX, GML, räumliche CSV-Dateien, GeoJSON, Well Known Text (WKT), Web Mapping Services (WMS), Web Mapping Tile Services (WMTS) und Web Feature Services (WFS). Azure Maps liest die Dateien lokal in den Arbeitsspeicher ein und kann in den meisten Fällen auch deutlich größere KML-Dateien verarbeiten. 

**Vorher: Google Maps**


```javascript
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
                center: new google.maps.LatLng(0, 0),
                zoom: 1
            });

             var layer = new google.maps.KmlLayer({
              url: 'https://googlearchive.github.io/js-v2-samples/ggeoxml/cta.kml',
              map: map
            });
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

![Google Maps-Bildüberlagerung](media/migrate-google-maps-web-app/google-maps-kml.png)</center>

**Nachher: Azure Maps**

In Azure Maps ist GeoJSON das Hauptdatenformat, das im Web-SDK genutzt wird. Zusätzliche Formate für räumliche Daten können mit dem [räumlichen E/A-Modul](https://docs.microsoft.com/javascript/api/azure-maps-spatial-io/) leicht integriert werden. Dieses Modul verfügt über Funktionen für das Lesen und Schreiben von räumlichen Daten und enthält auch eine einfache Datenschicht, über die Daten mit all diesen räumlichen Formaten gerendert werden können. Übergeben Sie zum Einlesen der Daten in eine Datei mit räumlichen Daten einfach eine URL oder Rohdaten als Zeichenfolge oder Blob an die Funktion `atlas.io.read`. Hierdurch werden alle analysierten Daten aus der Datei zurückgegeben, die dann der Karte hinzugefügt werden können. KML ist etwas komplexer als die meisten Formate für räumliche Daten, weil deutlich mehr Formatierungsinformationen vorhanden sind. Die `SpatialDataLayer`-Klasse unterstützt das Rendern eines Großteils dieser Formate. Symbolbilder müssen aber in die Karte geladen werden, bevor die Featuredaten geladen werden, und Bodenüberlagerungen müssen der Karte separat als Schichten hinzugefügt werden. Beim Laden von Daten über eine URL sollte zum Hosten ein CORS-fähiger Endpunkt verwendet werden, oder ein Proxydienst sollte als Option an die Lesefunktion übergeben werden. 

```javascript
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

    <!-- Add reference to the Azure Maps Spatial IO module. -->
    <script src="https://atlas.microsoft.com/sdk/javascript/spatial/0/atlas-spatial.js"></script>

    <script type='text/javascript'>
        var map, datasource, layer;

        function initMap() {
            //Initialize a map instance.
            map = new atlas.Map('myMap', {
                view: 'Auto',

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

                //Add a simple data layer for rendering the data.
                layer = new atlas.layer.SimpleDataLayer(datasource);
                map.layers.add(layer);

                //Read a KML file from a URL or pass in a raw KML string.
                atlas.io.read('https://googlearchive.github.io/js-v2-samples/ggeoxml/cta.kml').then(async r => {
                    if (r) {

                        //Check to see if there are any icons in the data set that need to be loaded into the map resources.
                        if (r.icons) {
                            //For each icon image, create a promise to add it to the map, then run the promises in parrallel.
                            var imagePromises = [];

                            //The keys are the names of each icon image.
                            var keys = Object.keys(r.icons);

                            if (keys.length !== 0) {
                                keys.forEach(function (key) {
                                    imagePromises.push(map.imageSprite.add(key, r.icons[key]));
                                });

                                await Promise.all(imagePromises);
                            }
                        }

                        //Load all features.
                        if (r.features && r.features.length > 0) {
                            datasource.add(r.features);
                        }

                        //Load all ground overlays.
                        if (r.groundOverlays && r.groundOverlays.length > 0) {
                            map.layers.add(r.groundOverlays);
                        }

                        //If bounding box information is known for data, set the map view to it.
                        if (r.bbox) {
                            map.setCamera({ bounds: r.bbox, padding: 50 });
                        }
                    }
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

![Azure Maps-Bildüberlagerung](media/migrate-google-maps-web-app/azure-maps-kml.png)</center>

**Zusätzliche Ressourcen:**

- [Funktion „atlas.io.read“](https://docs.microsoft.com/javascript/api/azure-maps-spatial-io/atlas.io?view=azure-maps-typescript-latest#read-string---arraybuffer---blob--spatialdatareadoptions-)
- [SimpleDataLayer](https://docs.microsoft.com/javascript/api/azure-maps-spatial-io/atlas.layer.simpledatalayer)
- [SimpleDataLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-spatial-io/atlas.simpledatalayeroptions)

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

Der folgende Anhang enthält Querverweise für die gängigsten Klassen in Google Maps V3 und die Äquivalente im Azure Maps Web SDK.

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

Das Azure Maps Web SDK enthält ein Dienstmodul, das separat geladen werden kann. Dieses Modul umschließt die Azure Maps-REST-Dienste mit einer Web-API und kann in JavaScript-, TypeScript- und Node.js-Anwendungen verwendet werden.

| Google Maps | Azure Maps  |
|-------------|-------------|
| `google.maps.Geocoder` | [atlas.service.SearchUrl](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchurl?view=azure-iot-typescript-latest)  |
| `google.maps.GeocoderRequest`  | [atlas.SearchAddressOptions](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchaddressoptions?view=azure-iot-typescript-latest)<br/>[atlas.SearchAddressRevrseOptions](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchaddressreverseoptions?view=azure-iot-typescript-latest)<br/>[atlas.SearchAddressReverseCrossStreetOptions](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchaddressreversecrossstreetoptions?view=azure-iot-typescript-latest)<br/>[atlas.SearchAddressStructuredOptions](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchaddressstructuredoptions?view=azure-iot-typescript-latest)<br/>[atlas.SearchAlongRouteOptions](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchalongrouteoptions?view=azure-iot-typescript-latest)<br/>[atlas.SearchFuzzyOptions](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchfuzzyoptions?view=azure-iot-typescript-latest)<br/>[atlas.SearchInsideGeometryOptions](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchinsidegeometryoptions?view=azure-iot-typescript-latest)<br/>[atlas.SearchNearbyOptions](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchnearbyoptions?view=azure-iot-typescript-latest)<br/>[atlas.SearchPOIOptions](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchpoioptions?view=azure-iot-typescript-latest)<br/>[atlas.SearchPOICategoryOptions](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchpoicategoryoptions?view=azure-iot-typescript-latest) |
| `google.maps.DirectionsService`  | [atlas.service.RouteUrl](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.routeurl?view=azure-iot-typescript-latest)  |
| `google.maps.DirectionsRequest`  | [atlas.CalculateRouteDirectionsOptions](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.calculateroutedirectionsoptions?view=azure-iot-typescript-latest) |
| `google.maps.places.PlacesService` | [atlas.service.SearchUrl](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchurl?view=azure-iot-typescript-latest)  |

## <a name="libraries"></a>Bibliotheken

Bibliotheken fügen der Karte zusätzliche Funktionalität hinzu. Viele dieser Bibliotheken befinden sich im Core-SDK von Azure Maps. Hier sind einige äquivalente Klassen aufgeführt, die anstelle dieser Google Maps-Bibliotheken verwendet werden können.

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

