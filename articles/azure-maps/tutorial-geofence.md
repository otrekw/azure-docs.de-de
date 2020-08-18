---
title: 'Tutorial: Erstellen eines Geofence und Nachverfolgen von Geräten auf einer Microsoft Azure-Karte'
description: Hier wird erläutert, wie Sie einen Geofence einrichten. Erfahren Sie, wie Sie mithilfe des räumlichen Diensts von Azure Maps Geräte in Relation zum Geofence nachverfolgen.
author: anastasia-ms
ms.author: v-stharr
ms.date: 8/11/2020
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.custom: mvc
ms.openlocfilehash: b88d9132ec1548c9d94fc418af35b55ac2836e96
ms.sourcegitcommit: b8702065338fc1ed81bfed082650b5b58234a702
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2020
ms.locfileid: "88121237"
---
# <a name="tutorial-set-up-a-geofence-by-using-azure-maps"></a>Tutorial: Einrichten eines Geofence mit Azure Maps

In diesem Tutorial werden die Grundlagen der Erstellung und Verwendung von Azure Maps-Geofencediensten im Kontext des folgenden Szenarios vermittelt:

*Ein Baustellenleiter muss den Ein- und Ausgang von Geräten für eine Baustelle überwachen. Wenn ein Gerät den Baustellenbereich verlässt oder in den Baustellenbereich gebracht wird, erhält der Baustellenleiter eine E-Mail.*

Azure Maps bietet eine Reihe von Diensten, die die Nachverfolgung des Ein- und Ausgangs von Geräten im obigen Szenario unterstützen. In diesem Tutorial wird Folgendes beschrieben:

> [!div class="checklist"]
> * Hochladen von [GeoJSON-Geofencingdaten](geofence-geojson.md), die die zu überwachenden Baustellenbereiche definieren. Hier wird die [Datenupload-API](https://docs.microsoft.com/rest/api/maps/data/uploadpreview) verwendet, um Geofences als Polygonkoordinaten in Ihr Azure Maps-Konto hochzuladen.
> * Einrichten von zwei [Logik-Apps](https://docs.microsoft.com/azure/event-grid/handler-webhooks#logic-apps), die ausgelöst werden können, um E-Mail-Benachrichtigungen an den Baustellenleiter zu senden, wenn Geräte in den Geofencebereich gebracht oder daraus entfernt wird.
> * Verwenden von [Azure Event Grid](https://docs.microsoft.com/azure/event-grid/overview), um Eingangs- und Ausgangsereignisse für den Azure Maps-Geofence zu abonnieren. In diesem Tutorial werden zwei Webhook-Ereignisabonnements eingerichtet, von denen die in den beiden Logik-Apps definierten HTTP-Endpunkte aufgerufen werden. Von den Logik-Apps werden dann die entsprechenden E-Mail-Benachrichtigungen für Geräte gesendet, die in den Geofencebereich gebracht oder daraus entfernt werden.
> * Verwenden Sie die [GET-API für die Geofencesuche](https://docs.microsoft.com/rest/api/maps/spatial/getgeofence), um Benachrichtigungen zu erhalten, wenn ein Gerät in die Geofencebereiche gebracht oder daraus entfernt wird.

## <a name="prerequisites"></a>Voraussetzungen

1. [Erstellen eines Azure Maps-Kontos](quick-demo-map-app.md#create-an-azure-maps-account)
2. [Abrufen eines Primärschlüssels](quick-demo-map-app.md#get-the-primary-key-for-your-account) (auch primärer Schlüssel oder Abonnementschlüssel genannt)

In diesem Tutorial wird die Anwendung [Postman](https://www.postman.com/) verwendet. Sie können aber auch eine andere API-Entwicklungsumgebung verwenden.

## <a name="upload-geofencing-geojson-data"></a>Hochladen von GeoJSON-Geofencingdaten

In diesem Tutorial werden GeoJSON-Geofencingdaten hochgeladen, die eine Merkmalsauswahl (`FeatureCollection`) enthalten. Die Merkmalsauswahl (`FeatureCollection`) enthält zwei Geofences, durch die polygonale Bereiche innerhalb der Baustelle definiert werden. Für den ersten Geofence sind keine Ablaufzeit und keine Einschränkungen festgelegt. Der zweite Geofence kann nur während der Geschäftszeiten (9–17 Uhr PST) abgefragt werden und ist nach dem 1. Januar 2022 nicht mehr gültig. Weitere Informationen zum GeoJSON-Format finden Sie unter [Geofencing von GeoJSON-Daten](geofence-geojson.md).

>[!TIP]
>Die Geofencingdaten können jederzeit aktualisiert werden. Weitere Informationen zum Aktualisieren Ihrer Daten finden Sie im [Artikel zur Datenupload-API](https://docs.microsoft.com/rest/api/maps/data/uploadpreview).

1. Öffnen Sie die Postman-App. Wählen Sie oben in der Postman-App **New** (Neu) aus. Wählen Sie im Fenster **Create New** (Neu erstellen) die Option **Collection** (Sammlung) aus.  Geben Sie einen Namen für die Sammlung ein, und klicken Sie dann auf **Create** (Erstellen).

2. Klicken Sie erneut auf **New** (Neu), um die Anforderung zu erstellen. Wählen Sie im Fenster **Create New** (Neu erstellen) die Option **Request** (Anforderung) aus. Geben Sie einen Anforderungsnamen (**Request name**) ein. Wählen Sie die im vorherigen Schritt erstellte Sammlung und anschließend **Save** (Speichern) aus.

3. Wählen Sie auf der Registerkarte „Builder“ (Generator) die HTTP-Methode **POST** aus, und geben Sie die folgende URL ein, um die Geofencingdaten in den Azure Maps-Dienst hochzuladen. Ersetzen Sie bei dieser Anforderung sowie bei den anderen in diesem Artikel angegebenen Anforderungen jeweils `{Azure-Maps-Primary-Subscription-key}` durch Ihren primären Abonnementschlüssel.

    ```HTTP
    https://atlas.microsoft.com/mapData/upload?subscription-key={Azure-Maps-Primary-Subscription-key}&api-version=1.0&dataFormat=geojson
    ```

    Der Parameter _geojson_ im URL-Pfad steht für das Format der Daten, die hochgeladen werden.

4. Klicken Sie auf die Registerkarte **Text**. Wählen Sie **Rohdaten** und anschließend **JSON** als Eingabeformat aus. Kopieren Sie die folgenden GeoJSON-Daten, und fügen Sie sie in den Bereich **Text** ein:

   ```JSON
   {
      "type": "FeatureCollection",
      "features": [
        {
          "type": "Feature",
          "geometry": {
            "type": "Polygon",
            "coordinates": [
              [
                [
                  -122.13393688201903,
                  47.63829579223815
                ],
                [
                  -122.13389128446579,
                  47.63782047131512
                ],
                [
                  -122.13240802288054,
                  47.63783312249837
                ],
                [
                  -122.13238388299942,
                  47.63829037035086
                ],
                [
                  -122.13393688201903,
                  47.63829579223815
                ]
              ]
            ]
          },
          "properties": {
            "geometryId": "1"
          }
        },
        {
          "type": "Feature",
          "geometry": {
            "type": "Polygon",
            "coordinates": [
              [
                [
                  -122.13374376296996,
                  47.63784758098976
                ],
                [
                  -122.13277012109755,
                  47.63784577367854
                ],
                [
                  -122.13314831256866,
                  47.6382813338708
                ],
                [
                  -122.1334782242775,
                  47.63827591198201
                ],
                [
                  -122.13374376296996,
                  47.63784758098976
                ]
              ]
            ]
          },
          "properties": {
            "geometryId": "2",
            "validityTime": {
            "expiredTime": "2022-01-01T00:00:00",
            "validityPeriod": [
                {
                  "startTime": "2020-07-15T16:00:00",
                  "endTime": "2020-07-15T24:00:00",
                  "recurrenceType": "Daily",
                  "recurrenceFrequency": 1,
                  "businessDayOnly": true
                }
              ]
            }
          }
        }
      ]
   }
   ```

5. Klicken Sie auf die blaue Schaltfläche **Send** (Senden), und warten Sie, bis die Anforderung verarbeitet wurde. Navigieren Sie nach Abschluss der Anforderung zur Registerkarte **Headers** (Header) der Antwort. Kopieren Sie den Wert des Schlüssels **Location** (Speicherort). Hierbei handelt es sich um die Status-URL (`status URL`).

    ```http
    https://atlas.microsoft.com/mapData/operations/<operationId>?api-version=1.0
    ```

6. Erstellen Sie zum Überprüfen des Status des API-Aufrufs eine **GET**-HTTP-Anforderung für `status URL`. An die URL muss zur Authentifizierung der primäre Abonnementschlüssel angefügt werden. Die **GET**-Anforderung sollte wie die folgende URL aussehen:

   ```HTTP
   https://atlas.microsoft.com/mapData/<operationId>/status?api-version=1.0&subscription-key={Subscription-key}
   ```

7. Wenn die **GET**-HTTP-Anforderung erfolgreich abgeschlossen wurde, wird ein `resourceLocation` zurückgegeben. Der `resourceLocation` enthält die eindeutige `udid` für den hochgeladenen Inhalt. Speichern Sie die `udid`. Sie wird im letzten Abschnitt dieses Tutorials zum Abfragen der API „GET Geofence“ benötigt. Optional können Sie im nächsten Schritt mithilfe der `resourceLocation`-URL Metadaten aus dieser Ressource abzurufen.

      ```json
      {
          "status": "Succeeded",
          "resourceLocation": "https://atlas.microsoft.com/mapData/metadata/{udid}?api-version=1.0"
      }
      ```

8. Erstellen Sie zum Abrufen von Inhaltsmetadaten eine **GET**-HTTP-Anforderung für die `resourceLocation`-URL, die Sie in Schritt 7 kopiert haben. Stellen Sie sicher, dass Sie für die Authentifizierung den primären Abonnementschlüssel an die URL anfügen. Die **GET**-Anforderung sollte wie die folgende URL aussehen:

    ```http
   https://atlas.microsoft.com/mapData/metadata/{udid}?api-version=1.0&subscription-key={Azure-Maps-Primary-Subscription-key}
    ```

9. Wenn die **GET**-HTTP-Anforderung erfolgreich ausgeführt wird, enthält der Antworttext die in Schritt 7 im `resourceLocation` angegebene `udid`, den Speicherort für den Zugriff bzw. das Herunterladen des Inhalts sowie einige weitere Metadaten zum Inhalt wie beispielsweise das Erstellungs- und Aktualisierungsdatum, die Größe und Ähnliches. Im Anschluss sehen Sie ein Beispiel für eine vollständige Antwort:

    ```json
    {
        "udid": "{udid}",
        "location": "https://atlas.microsoft.com/mapData/{udid}?api-version=1.0",
        "created": "7/15/2020 6:11:43 PM +00:00",
        "updated": "7/15/2020 6:11:45 PM +00:00",
        "sizeInBytes": 1962,
        "uploadStatus": "Completed"
    }
    ```

## <a name="create-logic-app-workflows"></a>Erstellen von Logik-App-Workflows

In diesem Abschnitt werden zwei [Logik-App](https://docs.microsoft.com/azure/event-grid/handler-webhooks#logic-apps)-Endpunkte zum Auslösen einer E-Mail-Benachrichtigung erstellt. Hier zeigen wir Ihnen, wie Sie den ersten Trigger erstellen, durch den bei jedem Aufruf des zugehörigen Endpunkts eine E-Mail-Benachrichtigung gesendet wird.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)

2. Klicken Sie im [Azure-Portal](https://portal.azure.com) links oben auf **Ressource erstellen**.

3. Geben Sie im Feld *Marketplace durchsuchen* den Suchbegriff **Logik-App** ein.

4. Wählen Sie unter *Ergebnisse* die Option **Logik-App** aus. Klicken Sie auf die Schaltfläche **Erstellen**.

5. Geben Sie auf der Seite **Logik-App** die folgenden Werte ein:
    * Das *Abonnement*, das Sie für diese Logik-App verwenden möchten.
    * Den Namen der *Ressourcengruppe* für diese Logik-App. Sie können für die Ressourcengruppe die Option *Neu erstellen* oder die Option *Vorhandene verwenden* auswählen.
    * Den *Namen Ihrer Logik-App*. Hier wird der Name `Equipment-Enter` verwendet.

    Lassen Sie die restlichen Werte für dieses Tutorial unverändert.

    :::image type="content" source="./media/tutorial-geofence/logic-app-create.png" alt-text="Erstellen einer Logik-App":::

6. Klicken Sie auf die Schaltfläche **Überprüfen + erstellen**. Überprüfen Sie Ihre Einstellungen, und klicken Sie auf **Erstellen**, um die Bereitstellung zu übermitteln. Klicken Sie nach erfolgreicher Bereitstellung auf **Zu Ressource wechseln**. Daraufhin wird der **Logik-App-Designer** angezeigt.

7. Als Nächstes wählen wir einen Triggertyp aus. Scrollen Sie etwas nach unten, um zum Abschnitt *Starten Sie mit einem gängigen Trigger** zu gelangen. Klicken Sie auf **Beim Empfang einer HTTP-Anforderung**.

     :::image type="content" source="./media/tutorial-geofence/logic-app-trigger.png" alt-text="Erstellen eines HTTP-Triggers für eine Logik-App":::

8. Klicken Sie rechts oben im Designer auf **Speichern**. Die **HTTP-POST-URL** wird automatisch generiert. Speichern Sie die URL. Sie wird im nächsten Abschnitt zum Erstellen eines Ereignisendpunkts benötigt.

    :::image type="content" source="./media/tutorial-geofence/logic-app-httprequest.png" alt-text="Logik-App: HTTP-Anforderungs-URL und JSON-Code":::

9. Wählen Sie **+ Neuer Schritt** aus. Als Nächstes wählen wir eine Aktion aus. Geben Sie im Suchfeld als Suchbegriff `outlook.com email` ein. Scrollen Sie in der Liste **Aktionen** nach unten, und klicken Sie auf **E-Mail senden (V2)** .
  
    :::image type="content" source="./media/tutorial-geofence/logic-app-designer.png" alt-text="Erstellen einer Logik-App: Designer":::

10. Melden Sie sich bei Ihrem Outlook.com-Konto an. Klicken Sie auf **Ja**, damit die Logik-App auf das Konto zugreifen kann. Füllen Sie die Felder für das Senden einer E-Mail aus.

    :::image type="content" source="./media/tutorial-geofence/logic-app-email.png" alt-text="Erstellen einer Logik-App: Schritt zum Senden einer E-Mail":::

    >[!TIP]
    > Sie können GeoJSON-Antwortdaten wie `geometryId` oder `deviceId` in Ihren E-Mail-Benachrichtigungen abrufen, indem Sie die Logik-App so konfigurieren, dass sie die von Event Grid gesendeten Daten liest. Wie Sie eine Logik-App für die Nutzung und Übergabe von Ereignisdaten in E-Mail-Benachrichtigungen konfigurieren, erfahren Sie unter [Tutorial: Senden von E-Mail-Benachrichtigungen zu Azure IoT Hub-Ereignissen mit Event Grid und Logic Apps](https://docs.microsoft.com/azure/event-grid/publish-iot-hub-events-to-logic-apps).

11. Klicken Sie links oben im Designer für Logik-Apps auf **Speichern**.

12. Wiederholen Sie die Schritte 3 bis 11, um eine zweite Logik-App zu erstellen, die den Baustellenleiter benachrichtigt, wenn Geräte den Baustellenbereich verlassen. Nennen Sie die Logik-App `Equipment-Exit`.

## <a name="create-azure-maps-events-subscriptions"></a>Erstellen von Abonnements für Azure Maps-Ereignisse

Azure Maps unterstützt drei Ereignistypen. Sie können sich die von Azure Maps unterstützten Ereignistypen [hier](https://docs.microsoft.com/azure/event-grid/event-schema-azure-maps) ansehen.  Wir müssen zwei verschiedene Ereignisabonnements erstellen: eins für Geofence-Eingangsereignisse und eins für Geofence-Ausgangsereignisse.

Führen Sie die unten angegebenen Schritte aus, um ein Ereignisabonnement für die enter-Ereignisse von Geofences zu erstellen. Geofence-Ausgangsereignisse können auf ähnliche Weise abonniert werden.

1. Navigieren Sie zu Ihrem Azure Maps-Konto. Wählen Sie im Dashboard die Option **Abonnements** aus. Klicken Sie auf den Namen Ihres Abonnements, und wählen Sie im Menü „Einstellungen“ die Option **Ereignisse** aus.

    :::image type="content" source="./media/tutorial-geofence/events-tab.png" alt-text="Navigieren zu Azure Maps-Kontoereignissen":::

2. Wählen Sie zum Erstellen eines Ereignisabonnements auf der Ereignisseite die Option **+ Ereignisabonnement** aus.

    :::image type="content" source="./media/tutorial-geofence/create-event-subscription.png" alt-text="Erstellen eines Abonnements für Azure Maps-Ereignisse":::

3. Geben Sie auf der Seite **Ereignisabonnement erstellen** die folgenden Werte ein:
    * Den *Namen* des Ereignisabonnements.
    * Das *Ereignisschema* muss *Event Grid-Schema* lauten.
    * Den *Namen des Systemthemas* für dieses Ereignisabonnement. Hier wird `Contoso-Construction` verwendet.
    * Wählen Sie unter *Nach Ereignistypen filtern* die Option `Geofence Entered` als Ereignistyp aus.
    * Wählen Sie für den *Endpunkttyp* die Option `Web Hook` aus.
    * Kopieren Sie für den *Endpunkt* die HTTP-POST-URL für den Logik-App-Endpunkt, den Sie im vorherigen Abschnitt erstellt haben. Sollten Sie die URL nicht gespeichert haben, können Sie zum Logik-App-Designer zurückkehren und sie im Schritt für den HTTP-Trigger kopieren.

    :::image type="content" source="./media/tutorial-geofence/events-subscription.png" alt-text="Details zum Azure Maps-Ereignisabonnement":::

4. Klicken Sie auf **Erstellen**.

5. Wiederholen Sie die Schritte 1 bis 4 für den Logik-App-Ausgangsendpunkt, den Sie im vorherigen Abschnitt erstellt haben. Wählen Sie im dritten Schritt `Geofence Exited` als Ereignistyp aus.

## <a name="use-search-geofence-get-api"></a>Verwenden der GET-API für die Geofencesuche

Als Nächstes verwenden wir die [GET-API für die Geofencesuche](https://docs.microsoft.com/rest/api/maps/spatial/getgeofence), um E-Mail-Benachrichtigungen an den Baustellenleiter zu senden, wenn ein Gerät in den Geofencebereich gebracht oder daraus entfernt wird.

Jedes Gerät verfügt über eine Geräte-ID (`deviceId`). In diesem Tutorial wird ein einzelnes Gerät mit der eindeutigen ID `device_1` nachverfolgt.

Das folgende Diagramm zeigt die fünf Positionen des Geräts im Zeitverlauf – beginnend mit der *Startposition*, die sich außerhalb der Geofences befindet. Die *Startposition* ist in diesem Tutorial nicht definiert, da das Gerät dort nicht abgefragt wird.

Wenn die [GET-API für die Geofencesuche](https://docs.microsoft.com/rest/api/maps/spatial/getgeofence) mit einer Geräteposition abgefragt wird, die angibt, dass das Gerät erstmals in den Geofencebereich gebracht oder daraus entfernt wurde, wird von Event Grid der entsprechende Logik-App-Endpunkt aufgerufen, um eine E-Mail-Benachrichtigung an den Baustellenleiter zu senden.

In den folgenden Abschnitten werden jeweils HTTP GET-Anforderungen mit den fünf verschiedenen Positionskoordinaten des Geräts an die Geofencing-API übermittelt.

![Geofence-Zuordnung in Azure Maps](./media/tutorial-geofence/geofence.png)

### <a name="equipment-location-1-47638237-122132483"></a>Geräteposition 1 (47.638237,-122.132483)

1. Wählen Sie oben in der Postman-App **New** (Neu) aus. Wählen Sie im Fenster **Create New** (Neu erstellen) die Option **Request** (Anforderung) aus.  Geben Sie einen Anforderungsnamen (**Request name**) ein. Hier wird der Name *Location 1* verwendet. Wählen Sie die Sammlung aus, die Sie im Abschnitt [Hochladen von GeoJSON-Geofencingdaten](#upload-geofencing-geojson-data) erstellt haben, und wählen Sie anschließend **Speichern** aus.

2. Wählen Sie auf der Registerkarte „Builder“ (Generator) die HTTP-Methode **GET** aus, und geben Sie die folgende URL ein. Ersetzen Sie dabei `{Azure-Maps-Primary-Subscription-key}` durch Ihren primären Abonnementschlüssel und `{udid}` durch die `udid`, die Sie im Abschnitt [Hochladen von GeoJSON-Geofencingdaten](#upload-geofencing-geojson-data) gespeichert haben.

   ```HTTP
   https://atlas.microsoft.com/spatial/geofence/json?subscription-key={subscription-key}&api-version=1.0&deviceId=device_01&udid={udid}&lat=47.638237&lon=-122.1324831&searchBuffer=5&isAsync=True&mode=EnterAndExit
   ```

3. Klicken Sie auf die Schaltfläche **Senden**. Im Antwortfenster wird der folgende GeoJSON-Code angezeigt:

    ```json
    {
      "geometries": [
        {
          "deviceId": "device_1",
          "udId": "64f71aa5-bbee-942d-e351-651a6679a7da",
          "geometryId": "1",
          "distance": -999.0,
          "nearestLat": 47.638291,
          "nearestLon": -122.132483
        },
        {
          "deviceId": "device_1",
          "udId": "64f71aa5-bbee-942d-e351-651a6679a7da",
          "geometryId": "2",
          "distance": 999.0,
          "nearestLat": 47.638053,
          "nearestLon": -122.13295
        }
      ],
      "expiredGeofenceGeometryId": [],
      "invalidPeriodGeofenceGeometryId": [],
      "isEventPublished": true
    }
    ```

4. In der obigen GeoJSON-Antwort bedeutet die negative Entfernung zum Geofence für den Hauptbereich, dass sich das Gerät innerhalb des Geofencebereichs befindet. Die positive Entfernung vom Geofence-Unterbereich (subsite) bedeutet, dass sich die Ausrüstung außerhalb des Unterbereichs befindet. Da sich dieses Gerät zum ersten Mal innerhalb des Geofence für den Hauptbereich befindet, wird der Parameter `isEventPublished` auf `true` festgelegt, und der Baustellenleiter erhält eine E-Mail-Benachrichtigung mit dem Hinweis, dass sich das Gerät nun innerhalb des Geofencebereichs befindet.

### <a name="location-2-4763800-122132531"></a>Position 2 (47.63800,-122.132531)

1. Wählen Sie oben in der Postman-App **New** (Neu) aus. Wählen Sie im Fenster **Create New** (Neu erstellen) die Option **Request** (Anforderung) aus.  Geben Sie einen Anforderungsnamen (**Request name**) ein. Hier wird der Name *Location 2* verwendet. Wählen Sie die Sammlung aus, die Sie im Abschnitt [Hochladen von GeoJSON-Geofencingdaten](#upload-geofencing-geojson-data) erstellt haben, und wählen Sie anschließend **Speichern** aus.

2. Wählen Sie auf der Registerkarte „Builder“ (Generator) die HTTP-Methode **GET** aus, und geben Sie die folgende URL ein. Ersetzen Sie dabei `{Azure-Maps-Primary-Subscription-key}` durch Ihren primären Abonnementschlüssel und `{udid}` durch die `udid`, die Sie im Abschnitt [Hochladen von GeoJSON-Geofencingdaten](#upload-geofencing-geojson-data) gespeichert haben.

   ```HTTP
   https://atlas.microsoft.com/spatial/geofence/json?subscription-key={subscription-key}&api-version=1.0&deviceId=device_01&udId={udId}&lat=47.63800&lon=-122.132531&searchBuffer=5&isAsync=True&mode=EnterAndExit
   ```

3. Klicken Sie auf die Schaltfläche **Senden**. Im Antwortfenster wird der folgende GeoJSON-Code angezeigt:

    ```json
    {
      "geometries": [
        {
          "deviceId": "device_01",
          "udId": "64f71aa5-bbee-942d-e351-651a6679a7da",
          "geometryId": "1",
          "distance": -999.0,
          "nearestLat": 47.637997,
          "nearestLon": -122.132399
        },
        {
          "deviceId": "device_01",
          "udId": "64f71aa5-bbee-942d-e351-651a6679a7da",
          "geometryId": "2",
          "distance": 999.0,
          "nearestLat": 47.63789,
          "nearestLon": -122.132809
        }
      ],
      "expiredGeofenceGeometryId": [],
      "invalidPeriodGeofenceGeometryId": [],
      "isEventPublished": false
    }
    ````

4. In der obigen GeoJSON-Antwort befindet sich das Gerät weiterhin innerhalb des Geofence für den Hauptbereich und wurde nicht in den Geofence für den Unterbereich gebracht. Der Parameter `isEventPublished` wird daher auf `false` festgelegt, und der Baustellenleiter erhält keine E-Mail-Benachrichtigung.

### <a name="location-3-4763810783315048-12213336020708084"></a>Position 3 (47.63810783315048,-122.13336020708084)

1. Wählen Sie oben in der Postman-App **New** (Neu) aus. Wählen Sie im Fenster **Create New** (Neu erstellen) die Option **Request** (Anforderung) aus.  Geben Sie einen Anforderungsnamen (**Request name**) ein. Hier wird der Name *Location 3* verwendet. Wählen Sie die Sammlung aus, die Sie im Abschnitt [Hochladen von GeoJSON-Geofencingdaten](#upload-geofencing-geojson-data) erstellt haben, und wählen Sie anschließend **Speichern** aus.

2. Wählen Sie auf der Registerkarte „Builder“ (Generator) die HTTP-Methode **GET** aus, und geben Sie die folgende URL ein. Ersetzen Sie dabei `{Azure-Maps-Primary-Subscription-key}` durch Ihren primären Abonnementschlüssel und `{udid}` durch die `udid`, die Sie im Abschnitt [Hochladen von GeoJSON-Geofencingdaten](#upload-geofencing-geojson-data) gespeichert haben.

    ```HTTP
      https://atlas.microsoft.com/spatial/geofence/json?subscription-key={subscription-key}&api-version=1.0&deviceId=device_01&udid={udid}&lat=47.63810783315048&lon=-122.13336020708084&searchBuffer=5&isAsync=True&mode=EnterAndExit
      ```

3. Klicken Sie auf die Schaltfläche **Senden**. Im Antwortfenster wird der folgende GeoJSON-Code angezeigt:

    ```json
    {
      "geometries": [
        {
          "deviceId": "device_01",
          "udId": "64f71aa5-bbee-942d-e351-651a6679a7da",
          "geometryId": "1",
          "distance": -999.0,
          "nearestLat": 47.638294,
          "nearestLon": -122.133359
        },
        {
          "deviceId": "device_01",
          "udId": "64f71aa5-bbee-942d-e351-651a6679a7da",
          "geometryId": "2",
          "distance": -999.0,
          "nearestLat": 47.638161,
          "nearestLon": -122.133549
        }
      ],
      "expiredGeofenceGeometryId": [],
      "invalidPeriodGeofenceGeometryId": [],
      "isEventPublished": true
    }
    ````

4. In der obigen GeoJSON-Antwort befindet sich das Gerät weiterhin innerhalb des Geofence für den Hauptbereich, wurde aber in den Geofence für den Unterbereich gebracht. Der Parameter wird daher `isEventPublished` auf `true` festgelegt, und der Baustellenleiter erhält eine E-Mail-Benachrichtigung mit dem Hinweis, dass das Gerät in einen Geofencebereich gebracht wurde.

    >[!NOTE]
    >Falls das Gerät nach Geschäftsschluss in den Unterbereich bewegt wurde, wird kein Ereignis veröffentlicht, und der Baustellenleiter erhält keine Benachrichtigung.  

### <a name="location-4-47637988-1221338344"></a>Position 4 (47.637988,-122.1338344)

1. Wählen Sie oben in der Postman-App **New** (Neu) aus. Wählen Sie im Fenster **Create New** (Neu erstellen) die Option **Request** (Anforderung) aus.  Geben Sie einen Anforderungsnamen (**Request name**) ein. Hier wird der Name *Location 4* verwendet. Wählen Sie die Sammlung aus, die Sie im Abschnitt [Hochladen von GeoJSON-Geofencingdaten](#upload-geofencing-geojson-data) erstellt haben, und wählen Sie anschließend **Speichern** aus.

2. Wählen Sie auf der Registerkarte „Builder“ (Generator) die HTTP-Methode **GET** aus, und geben Sie die folgende URL ein. Ersetzen Sie dabei `{Azure-Maps-Primary-Subscription-key}` durch Ihren primären Abonnementschlüssel und `{udid}` durch die `udid`, die Sie im Abschnitt [Hochladen von GeoJSON-Geofencingdaten](#upload-geofencing-geojson-data) gespeichert haben.

    ```HTTP
    https://atlas.microsoft.com/spatial/geofence/json?subscription-key={subscription-key}&api-version=1.0&deviceId=device_01&udid={udid}&lat=47.637988&userTime=2023-01-16&lon=-122.1338344&searchBuffer=5&isAsync=True&mode=EnterAndExit
    ```

3. Klicken Sie auf die Schaltfläche **Senden**. Im Antwortfenster wird der folgende GeoJSON-Code angezeigt:

    ```json
    {
      "geometries": [
        {
          "deviceId": "device_01",
          "udId": "64f71aa5-bbee-942d-e351-651a6679a7da",
          "geometryId": "1",
          "distance": -999.0,
          "nearestLat": 47.637985,
          "nearestLon": -122.133907
        }
      ],
      "expiredGeofenceGeometryId": [
        "2"
      ],
      "invalidPeriodGeofenceGeometryId": [],
      "isEventPublished": false
    }
    ````

4. In der obigen GeoJSON-Antwort befindet sich das Gerät weiterhin innerhalb des Geofence für den Hauptbereich, wurde aber aus dem Geofence für den Unterbereich entfernt. Der Wert `userTime` liegt allerdings nach dem in den Geofencedaten definierten Ablauftermin (`expiredTime`). Der Parameter `isEventPublished` wird deshalb auf `false` festgelegt, und der Baustellenleiter erhält keine E-Mail-Benachrichtigung.

### <a name="location-547637988-1221338344"></a>Position 5 (47.637988,-122.1338344)

1. Wählen Sie oben in der Postman-App **New** (Neu) aus. Wählen Sie im Fenster **Create New** (Neu erstellen) die Option **Request** (Anforderung) aus.  Geben Sie einen Anforderungsnamen (**Request name**) ein. Hier wird der Name *Location 4* verwendet. Wählen Sie die Sammlung aus, die Sie im Abschnitt [Hochladen von GeoJSON-Geofencingdaten](#upload-geofencing-geojson-data) erstellt haben, und wählen Sie anschließend **Speichern** aus.

2. Wählen Sie auf der Registerkarte „Builder“ (Generator) die HTTP-Methode **GET** aus, und geben Sie die folgende URL ein. Ersetzen Sie dabei `{Azure-Maps-Primary-Subscription-key}` durch Ihren primären Abonnementschlüssel und `{udid}` durch die `udid`, die Sie im Abschnitt [Hochladen von GeoJSON-Geofencingdaten](#upload-geofencing-geojson-data) gespeichert haben.

    ```HTTP
    https://atlas.microsoft.com/spatial/geofence/json?subscription-key={subscription-key}&api-version=1.0&deviceId=device_01&udid={udid}&lat=47.637988&lon=-122.1338344&searchBuffer=5&isAsync=True&mode=EnterAndExit
    ```

3. Klicken Sie auf die Schaltfläche **Senden**. Im Antwortfenster wird der folgende GeoJSON-Code angezeigt:

    ```json
    {
      "geometries": [
      {
        "deviceId": "device_01",
        "udId": "64f71aa5-bbee-942d-e351-651a6679a7da",
        "geometryId": "1",
        "distance": -999.0,
        "nearestLat": 47.637985,
        "nearestLon": -122.133907
      },
      {
        "deviceId": "device_01",
        "udId": "64f71aa5-bbee-942d-e351-651a6679a7da",
        "geometryId": "2",
        "distance": 999.0,
        "nearestLat": 47.637945,
        "nearestLon": -122.133683
      }
      ],
      "expiredGeofenceGeometryId": [],
      "invalidPeriodGeofenceGeometryId": [],
      "isEventPublished": true
    }
    ````

4. In der obigen GeoJSON-Antwort wurde das Gerät aus dem Geofence für den Hauptbereich entfernt. Der Parameter wird daher `isEventPublished` auf `true` festgelegt, und der Baustellenleiter erhält eine E-Mail-Benachrichtigung mit dem Hinweis, dass das Gerät aus einem Geofencebereich entfernt wurde.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Behandeln von Inhaltstypen in Azure Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-content-type)

> [!div class="nextstepaction"]
> [Tutorial: Senden von E-Mail-Benachrichtigungen zu Azure IoT Hub-Ereignissen mit Event Grid und Logic Apps](https://docs.microsoft.com/azure/event-grid/publish-iot-hub-events-to-logic-apps)

> [!div class="nextstepaction"]
> [Ereignishandler in Azure Event Grid](https://docs.microsoft.com/azure/event-grid/event-handlers)
