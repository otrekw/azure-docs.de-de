---
title: Implementieren dynamischer Stile für Azure Maps Creator-Gebäudepläne (Vorschau)
description: Erfahren Sie, wie Sie dynamische Stile für Creator-Gebäudepläne (Vorschau) implementieren.
author: anastasia-ms
ms.author: v-stharr
ms.date: 12/07/2020
ms.topic: how-to
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: a23c492d4a81703c0dc6612928a56b5b31d52cae
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "101726313"
---
# <a name="implement-dynamic-styling-for-creator-preview-indoor-maps"></a>Implementieren dynamischer Stile für Creator-Gebäudepläne (Vorschau)

> [!IMPORTANT]
> Azure Maps Creator-Dienste befinden sich derzeit in der öffentlichen Vorschau.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Mit dem [Featurezustandsdienst](/rest/api/maps/featurestate) von Azure Maps-Ersteller können Sie Stile basierend auf den dynamischen Eigenschaften der Features von Gebäudeplandaten anwenden.  Beispielsweise können Sie Besprechungsräume der Einrichtung mit einer bestimmten Farbe rendern, um den Belegungsstatus anzuzeigen. In diesem Artikel erfahren Sie, wie Sie Features von Gebäudeplänen mithilfe des [Featurezustandsdiensts](/rest/api/maps/featurestate) und des [Indoor-Webmoduls](how-to-use-indoor-module.md) rendern.

## <a name="prerequisites"></a>Voraussetzungen

1. [Erstellen eines Azure Maps-Kontos](quick-demo-map-app.md#create-an-azure-maps-account)
2. [Abrufen eines Primärschlüssels](quick-demo-map-app.md#get-the-primary-key-for-your-account) (auch primärer Schlüssel oder Abonnementschlüssel genannt)
3. [Erstellen einer Creator-Ressource (Vorschau)](how-to-manage-creator.md)
4. Laden Sie das [Beispielzeichenpaket](https://github.com/Azure-Samples/am-creator-indoor-data-examples) herunter.
5. [Erstellen Sie einen Gebäudeplan](tutorial-creator-indoor-maps.md), um eine `tilesetId` und `statesetId` zu erhalten.
6. Erstellen Sie eine Webanwendung, indem Sie die Schritte in [Verwenden des Moduls für Gebäudepläne](how-to-use-indoor-module.md) ausführen.

In diesem Tutorial wird die [Postman](https://www.postman.com/)-Anwendung verwendet, Sie können aber auch eine andere API-Entwicklungsumgebung auswählen.

## <a name="implement-dynamic-styling"></a>Implementieren dynamischer Stile

Nachdem Sie die Voraussetzungen erfüllt haben, sollten Sie über eine einfache Webanwendung verfügen, die mit Ihrem Abonnementschlüssel sowie der `tilesetId` und `statesetId` konfiguriert ist.

### <a name="select-features"></a>Auswählen von Features

Um dynamische Stile zu implementieren, muss ein Feature wie z. B. ein Besprechungs- oder Konferenzraum über die `id` seines Features referenziert werden. Sie verwenden die `id` des Features, um die dynamische Eigenschaft oder den *Zustand* dieses Features zu aktualisieren. Um die in einem Dataset definierten Features anzuzeigen, können Sie eine der folgenden Methoden verwenden:

* WFS-API (Web Feature Service). Datasets können mithilfe der WFS-API abgefragt werden. WFS hält die Features der OGC-API (Open Geospatial Consortium) ein. Die WFS-API ist hilfreich beim Abfragen von Features innerhalb eines Datasets. Beispielsweise können Sie WFS verwenden, um alle mittelgroßen Besprechungsräume einer bestimmten Einrichtung und in einem bestimmten Stockwerk zu finden.

* Implementieren Sie benutzerdefinierten Code, der es einem Benutzer ermöglicht, mithilfe Ihrer Webanwendung Features auf einer Karte auszuwählen. In diesem Artikel verwenden wir diese Option.  

Das folgende Skript implementiert das Mausklick-Ereignis. Der Code ruft die `id` des Features basierend auf dem angeklickten Punkt ab. In Ihrer Anwendung können Sie den Code unterhalb des Blocks mit Ihrem Indoor-Manager-Code einfügen. Führen Sie Ihre Anwendung aus, und überprüfen Sie die Konsole, um die `id` des Features des angeklickten Punkts zu erhalten.

```javascript
/* Upon a mouse click, log the feature properties to the browser's console. */
map.events.add("click", function(e){

    var features = map.layers.getRenderedShapes(e.position, "indoor");

    features.forEach(function (feature) {
        if (feature.layer.id == 'indoor_unit_office') {
            console.log(feature);
        }
    });
});
```

Im Tutorial [Erstellen eines Gebäudeplans](tutorial-creator-indoor-maps.md) wurde das Featurezustandsset für die Annahme von Zustandsaktualisierungen für `occupancy` konfiguriert.

Im nächsten Abschnitt legen wir den *Zustand* von „occupancy“ (Belegung) von Büro `UNIT26` auf `true` fest. Während `UNIT27` auf `false` festgelegt wird.

### <a name="set-occupancy-status"></a>Festlegen des Belegungsstatus

 Wir aktualisieren jetzt den Zustand der beiden Büros `UNIT26` und `UNIT27`:

1. Wählen Sie in der Postman-Anwendung **Neu** aus. Wählen Sie im Fenster **Create New** (Neu erstellen) die Option **Request** (Anforderung) aus. Geben Sie einen **Anforderungsnamen** ein, und wählen Sie eine Sammlung aus. Klicken Sie unten auf der Seite auf **Speichern**.

2. Verwenden Sie die [Featureupdatezustände-API](/rest/api/maps/featurestate/updatestatespreview), um den Zustand zu aktualisieren. Übergeben Sie die Zustandsset-ID und `UNIT26` für eine der beiden Einheiten. Fügen Sie Ihren Azure Maps-Abonnementschlüssel an. Hier sehen Sie die URL einer **POST**-Anforderung zum Aktualisieren des Zustands:

    ```http
    https://atlas.microsoft.com/featureState/state?api-version=1.0&statesetID={statesetId}&featureID=UNIT26&subscription-key={Azure-Maps-Primary-Subscription-key}
    ```

3. Legen Sie in den **Headern** der **POST**-Anforderung `Content-Type` auf `application/json` fest. Schreiben Sie im **TEXT** der **POST**-Anforderung den folgenden unformatierten JSON-Code mit den Featureupdates. Das Update wird nur gespeichert, wenn der gepostete Zeitstempel nach dem Zeitstempel liegt, der in früheren Featurezustandsaktualisierungs-Anforderungen für dieselbe `ID` des Features verwendet wurde. Übergeben Sie den `keyName` „occupied“ (ausgelastet), um seinen Wert zu aktualisieren.

    ```json
    {
        "states": [
            {
                "keyName": "occupied",
                "value": true,
                "eventTimestamp": "2019-11-14T17:10:20"
            }
        ]
    }
    ```

4. Wiederholen Sie Schritt 2 und 3 unter Verwendung von `UNIT27` mit dem folgenden JSON-Code.

    ``` json
    {
        "states": [
            {
                "keyName": "occupied",
                "value": false,
                "eventTimestamp": "2019-11-14T17:10:20"
            }
        ]
    }
    ```

### <a name="visualize-dynamic-styles-on-a-map"></a>Visualisieren dynamischer Stile auf einer Karte

Die Webanwendung, die Sie zuvor in einem Browser geöffnet haben, sollte jetzt den aktualisierten Zustand der Kartenfeatures widerspiegeln. `UNIT27`(142) sollte grün angezeigt werden, und `UNIT26`(143) sollte rot angezeigt werden.

![Freier Raum in Grün und belegter Raum in Rot](./media/indoor-map-dynamic-styling/room-state.png)

[Livedemo](https://azuremapscodesamples.azurewebsites.net/?sample=Creator%20indoor%20maps)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter:

> [!div class="nextstepaction"]
> [Creator (Vorschau) für Gebäudepläne](creator-indoor-maps.md)

Informationen finden Sie in den API-Referenzen, die in diesem Artikel erwähnt werden:

> [!div class="nextstepaction"]
> [Datenupload](creator-indoor-maps.md#upload-a-drawing-package)

> [!div class="nextstepaction"]
> [Datenkonvertierung](creator-indoor-maps.md#convert-a-drawing-package)

> [!div class="nextstepaction"]
> [Dataset](creator-indoor-maps.md#datasets)

> [!div class="nextstepaction"]
> [Kachelset](creator-indoor-maps.md#tilesets)

> [!div class="nextstepaction"]
> [Featurezustandsset](creator-indoor-maps.md#feature-statesets)

> [!div class="nextstepaction"]
> [WFS-Dienst](creator-indoor-maps.md#web-feature-service-api)