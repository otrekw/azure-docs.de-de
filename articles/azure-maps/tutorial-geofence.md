---
title: 'Tutorial: Erstellen eines Geofence und Nachverfolgen von Geräten auf einer Karte | Microsoft Azure Maps'
description: Es wird beschrieben, wie Sie mit dem räumlichen Dienst von Microsoft Azure Maps einen Geofence einrichten und Geräte in Relation zum Geofence nachverfolgen.
author: philmea
ms.author: philmea
ms.date: 1/15/2020
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 126829f12d71e40511c26e781cb191988c1d031e
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "80333863"
---
# <a name="tutorial-set-up-a-geofence-by-using-azure-maps"></a>Tutorial: Einrichten eines Geofence mit Azure Maps

In diesem Tutorial werden die grundlegenden Schritte beschrieben, die zum Einrichten eines Geofence mit Azure Maps ausgeführt werden müssen. Stellen Sie sich ein Szenario vor, bei dem ein Baustellenleiter potenziell gefährliche Ausrüstung überwachen muss. Der Baustellenleiter muss sicherstellen, dass die Ausrüstung den festgelegten Baustellenbereich nicht verlässt. Dieser Baustellenbereich ist ein fester Parameter. Gemäß der gesetzlichen Bestimmungen muss die Ausrüstung innerhalb des Bereichs dieses Parameters bleiben, und Verstöße werden an den Betriebsleiter gemeldet.  

Wir nutzen die Datenupload-API, um einen Geofence zu speichern, und die Geofence-API, um den Standort der Ausrüstung relativ zum Geofence zu überprüfen. Sowohl die Datenupload-API als auch die Geofence-API basieren auf Azure Maps. Darüber hinaus verwenden wir Azure Event Grid, um die Ergebnisse für den Geofence zu streamen und basierend darauf eine Benachrichtigung einzurichten. Weitere Informationen zu Event Grid finden Sie unter [Azure Event Grid](https://docs.microsoft.com/azure/event-grid/overview).

In diesem Tutorial wird Folgendes beschrieben:

> [!div class="checklist"]
> * Hochladen des Geofencebereichs in den Azure Maps-Datendienst per Datenupload-API
> *   Einrichten einer Event Grid-Instanz zum Verarbeiten von Geofence-Ereignissen
> *   Einrichten eines Geofence-Ereignishandlers
> *   Einrichten von Warnungen als Antwort auf Geofence-Ereignisse mit Logic Apps
> *   Verwenden von Azure Maps-Geofencedienst-APIs zum Nachverfolgen, ob sich eine bestimmte Baustellenressource innerhalb des Baustellenbereichs befindet


## <a name="prerequisites"></a>Voraussetzungen

### <a name="create-an-azure-maps-account"></a>Erstellen eines Azure Maps-Kontos 

Befolgen Sie die Anleitung zum [Erstellen eines Kontos](quick-demo-map-app.md#create-an-account-with-azure-maps), um ein Azure Maps-Kontoabonnement im Tarif S1 zu erstellen. Die Schritte unter [Abrufen des Primärschlüssels für Ihr Konto](quick-demo-map-app.md#get-the-primary-key-for-your-account) veranschaulichen, wie Sie den Primärschlüssel Ihres Kontos abrufen. Weitere Informationen zur Authentifizierung in Azure Maps finden Sie unter [Verwalten der Authentifizierung in Azure Maps](./how-to-manage-authentication.md).

## <a name="upload-geofences"></a>Hochladen von Geofences

Wir treffen die Annahme, dass „subsite1“ der Hauptbereich des Geofence ist. Er verfügt über eine festgelegte Ablaufzeit. Sie können je nach Ihren Anforderungen weitere geschachtelte Geofences erstellen. Diese Geofences können genutzt werden, um einzelne Bereiche des gesamten Baustellenbereichs nachzuverfolgen. „subsite1“ kann beispielsweise der Unterbereich sein, in dem in Woche 1 bis 4 des Zeitplans gearbeitet wird. „subsite2“ kann der Unterbereich sein, in dem in Woche 5 bis 7 gearbeitet wird. Alle Geofences können zu Beginn des Projekts als Gesamtdataset geladen werden. Diese Geofences werden genutzt, um Regeln anhand von Zeit und Ort nachzuverfolgen. 

Wir nutzen die Anwendung Postman, um den Geofence für die Baustelle mit der Datenupload-API hochzuladen. Installieren Sie die [Anwendung Postman](https://www.getpostman.com/), und erstellen Sie ein kostenloses Konto. 

Führen Sie nach der Installation der Postman-App die unten angegebenen Schritte aus, um den Geofence der Baustelle mit der Datenupload-API von Azure Maps hochzuladen.

1. Öffnen Sie die App Postman, und klicken Sie auf „New“ > „Create new“ („Neu“ > „Neu erstellen“). Wählen Sie anschließend die Option „Request“ (Anfordern). Geben Sie einen Anforderungsnamen für das Hochladen von Geofencedaten ein, wählen Sie eine Sammlung oder einen Ordner zum Speichern aus, und klicken Sie auf „Save“ (Speichern).

    ![Hochladen von Geofences mit Postman](./media/tutorial-geofence/postman-new.png)

2. Wählen Sie auf der Registerkarte „Builder“ (Generator) die POST-HTTP-Methode aus, und geben Sie die folgende URL ein, um eine POST-Anforderung zu senden.

    ```HTTP
    https://atlas.microsoft.com/mapData/upload?subscription-key={subscription-key}&api-version=1.0&dataFormat=geojson
    ```
    
    Der Parameter GEOJSON im URL-Pfad steht für das Format der Daten, die hochgeladen werden.

3. Klicken Sie auf **Params**, und geben Sie die folgenden Schlüssel-Wert-Paare ein, die für die POST-Anforderungs-URL verwendet werden sollen. Ersetzen Sie {subscription-key} durch Ihren Azure Maps-Abonnementschlüssel (auch als Primärschlüssel bezeichnet).
   
    ![Parameter für Uploaddaten (Geofence) in Postman](./media/tutorial-geofence/postman-key-vals.png)

4. Klicken Sie auf **Text**, und wählen Sie dann das Rohdaten-Eingabeformat aus. Wählen Sie in der Dropdownliste anschließend „JSON“ als Eingabeformat aus. Geben Sie den folgenden JSON-Code als hochzuladende Daten ein:

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
              "expiredTime": "2019-01-15T00:00:00",
              "validityPeriod": [
                {
                  "startTime": "2019-01-08T01:00:00",
                  "endTime": "2019-01-08T17:00:00",
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

5. Klicken Sie auf „Senden“, und sehen Sie sich den Antwortheader an. Bei einer erfolgreichen Anforderung enthält der Header **Standort** den Status-URI. Der Status-URI hat das unten angegebene Format. Der Wert von „uploadStatusId“ ist nicht in geschweifte Klammern („{ }“) gesetzt. Die Verwendung von geschweiften Klammern ist eine gängige Vorgehensweise, um Werte anzugeben, die vom Benutzer eingegeben werden müssen oder die sich für unterschiedliche Benutzer unterscheiden.

   ```HTTP
   https://atlas.microsoft.com/mapData/{uploadStatusId}/status?api-version=1.0
   ```

6. Kopieren Sie Ihren Status-URI, und fügen Sie den Abonnementschlüssel an. Das Format des Status-URI sollte wie unten dargestellt aussehen. Beachten Sie, dass Sie im unten angegebenen Format „{subscription-key}“ durch Ihren Abonnementschlüssel ersetzen würden (ohne geschweifte Klammern).

   ```HTTP
   https://atlas.microsoft.com/mapData/{uploadStatusId}/status?api-version=1.0&subscription-key={Subscription-key}
   ```

7. Öffnen Sie zum Abrufen der `udId` in der Postman-App eine neue Registerkarte, und wählen Sie auf der Registerkarte „Builder“ (Generator) die GET HTTP-Methode aus. Senden Sie eine GET-Anforderung an den Status-URI aus dem vorherigen Schritt. Wenn der Datenupload erfolgreich ausgeführt wurde, wird im Antworttext ein Wert für „udId“ ausgegeben. Kopieren Sie die udId zur späteren Verwendung.

   ```JSON
   {
    "status": "Succeeded",
    "resourceLocation": "https://atlas.microsoft.com/mapData/metadata/{udId}?api-version=1.0"
   }
   ```

## <a name="set-up-an-event-handler"></a>Einrichten eines Ereignishandlers

In diesem Abschnitt erstellen wir einen Ereignishandler, der Benachrichtigungen empfängt. Dieser Ereignishandler sollte den Betriebsleiter darüber benachrichtigen, wenn Ausrüstung im Baustellenbereich ankommt (enter) oder diesen verlässt (exit).

Wir erstellen zwei [Logic Apps](https://docs.microsoft.com/azure/event-grid/event-handlers#logic-apps)-Dienste für die Behandlung von enter- und exit-Ereignissen. Wenn die Ereignisse in der Logik-App ausgelöst werden, werden nacheinander weitere Ereignisse ausgelöst. Das Ziel ist, dass Warnungen (in diesem Fall in Form von E-Mails) an den Betriebsleiter gesendet werden. In der folgenden Abbildung ist die Erstellung einer Logik-App für das Geofence-Ereignis „enter“ dargestellt. Auf ähnliche Weise können Sie auch eine Logik-App für das Ereignis „exit“ erstellen. Sie können auch die [Informationen zu allen unterstützten Ereignishandlern anzeigen](https://docs.microsoft.com/azure/event-grid/event-handlers).

1. Erstellen Sie eine Logik-App im Azure-Portal. Wählen Sie die Logik-App im Azure Marketplace aus. Wählen Sie dann die Schaltfläche **Erstellen** aus.

   ![Erstellen einer Azure Logic Apps-Instanz zum Verarbeiten von Geofence-Ereignissen](./media/tutorial-geofence/logic-app.png)

2. Navigieren Sie im Menü „Einstellungen“ für die Logik-App zu **Logik-App-Designer**.

3. Wählen Sie einen HTTP-Anforderungstrigger und dann die Option „Neuer Schritt“ aus. Wählen Sie im Outlook-Connector die Option „E-Mail senden“ als Aktion aus.
  
   ![Logic Apps-Schema](./media/tutorial-geofence/logic-app-schema.png)

4. Füllen Sie die Felder für das Senden einer E-Mail aus. Lassen Sie das Feld für die HTTP-URL leer. Sie wird automatisch generiert, nachdem Sie auf „Speichern“ geklickt haben.

   ![Generieren eines Logic Apps-Endpunkts](./media/tutorial-geofence/logic-app-endpoint.png)

5. Speichern Sie die Logik-App, um den HTTP-URL-Endpunkt zu generieren, und kopieren Sie die HTTP-URL.

## <a name="create-an-azure-maps-events-subscription"></a>Erstellen eines Abonnements für Azure Maps-Ereignisse

Azure Maps unterstützt drei Ereignistypen. Sie können sich die von Azure Maps unterstützten Ereignistypen [hier](https://docs.microsoft.com/azure/event-grid/event-schema-azure-maps) ansehen. Wir benötigen zwei verschiedene Ereignisabonnements: eins für enter-Ereignisse und eins für exit-Ereignisse.

Führen Sie die unten angegebenen Schritte aus, um ein Ereignisabonnement für die enter-Ereignisse von Geofences zu erstellen. Sie können exit-Ereignisse von Geofences auf ähnliche Weise abonnieren.

1. Navigieren Sie zu Ihrem Azure Maps-Konto. Wählen Sie im Dashboard die Option „Abonnements“ aus. Klicken Sie auf den Namen Ihres Abonnements, und wählen Sie im Menü „Einstellungen“ die Option **Ereignisse** aus.

   ![Navigieren zu Azure Maps-Kontoereignissen](./media/tutorial-geofence/events-tab.png)

2. Wählen Sie zum Erstellen eines Ereignisabonnements auf der Seite „Ereignisse“ die Option „Ereignisabonnement“.

   ![Erstellen eines Abonnements für Azure Maps-Ereignisse](./media/tutorial-geofence/create-event-subscription.png)

3. Geben Sie dem Ereignisabonnement einen Namen, und abonnieren Sie den Ereignistyp „enter“. Wählen Sie jetzt „Webhook“ als „Endpunkttyp“ aus. Klicken Sie auf „Endpunkt auswählen“, und kopieren Sie den HTTP-URL-Endpunkt für Ihre Logik-App in „{Endpoint}“.

   ![Details zum Azure Maps-Ereignisabonnement](./media/tutorial-geofence/events-subscription.png)


## <a name="use-geofence-api"></a>Verwenden der Geofence-API

Sie können mit der Geofence-API überprüfen, ob sich ein Gerät (**device**) – in diesem Fall ein Ausrüstungsteil – innerhalb oder außerhalb eines Geofencebereichs befindet. Wir führen für die Geofence-GET-API nun Abfragen für verschiedene Standorte durch, an denen sich bestimmte Ausrüstungsteile im Laufe der Zeit befunden haben. In der folgenden Abbildung sind fünf Standorte mit fünf Ausrüstungsteilen für die Baustelle dargestellt. 

> [!Note]
> Das Szenario und Verhalten basiert auf derselben Geräte-ID (**device id**) und spiegelt somit die fünf verschiedenen Standorte wider, die in der Abbildung unten dargestellt sind.

Die „deviceId“ ist eine eindeutige ID, die Sie für Ihr Gerät in der GET-Anforderung angeben, wenn Sie den zugehörigen Standort abfragen. Wenn Sie eine asynchrone Anforderung an die **GET-API für die Geofencesuche** senden, dient die „deviceId“ als Hilfe beim Veröffentlichen von Geofence-Ereignissen für das Gerät (relativ zum angegebenen Geofence). In diesem Tutorial haben wir asynchrone Anforderungen mit einer eindeutigen „deviceId“ an die API gesendet. Die Anforderungen in diesem Tutorial werden in chronologischer Reihenfolge gesendet, wie dies im Diagramm dargestellt ist. Die „isEventPublished“-Eigenschaft in der Antwort wird jeweils veröffentlicht, wenn ein Gerät im Geofencebereich ankommt oder diesen verlässt. Sie müssen kein Gerät registrieren, um dieses Tutorial durcharbeiten zu können.

Wir kehren zum Diagramm zurück. Jeder dieser fünf Standorte wird genutzt, um zu bewerten, wie sich der Geofencestatus „enter“ und „exit“ basierend auf dem Fence jeweils ändert. Bei einer Statusänderung löst der Geofencedienst ein Ereignis aus, das von Event Grid an die Logik-App gesendet wird. Der Baustellenleiter erhält dann per E-Mail die entsprechende Benachrichtigung zum enter- bzw. exit-Ereignis.

![Geofence-Zuordnung in Azure Maps](./media/tutorial-geofence/geofence.png)

Öffnen Sie in der App Postman eine neue Registerkarte in derselben Sammlung, die Sie oben erstellt haben. Wählen Sie auf der Registerkarte „Builder“ (Generator) die HTTP-GET-Methode aus:

Im Folgenden sind fünf HTTP GET-Anforderungen der Geofencing-API angegeben, die jeweils über andere Standortkoordinaten für die Ausrüstung verfügen. Die Koordinaten sind gemäß ihrer Ermittlung in chronologischer Reihenfolge aufgeführt. Auf jede Anforderung folgt jeweils der Antworttext.
 
1. Standort 1:
    
   ```HTTP
   https://atlas.microsoft.com/spatial/geofence/json?subscription-key={subscription-key}&api-version=1.0&deviceId=device_01&udId={udId}&lat=47.638237&lon=-122.1324831&searchBuffer=5&isAsync=True&mode=EnterAndExit
   ```
   ![Geofence-Abfrage 1](./media/tutorial-geofence/geofence-query1.png)

   In der obigen Antwort bedeutet die negative Entfernung zum Geofence-Hauptbereich, dass sich die Ausrüstung innerhalb des Geofencebereichs befindet. Die positive Entfernung vom Geofence-Unterbereich (subsite) bedeutet, dass sich die Ausrüstung außerhalb des Unterbereichs befindet. 

2. Standort 2: 
   
   ```HTTP
   https://atlas.microsoft.com/spatial/geofence/json?subscription-key={subscription-key}&api-version=1.0&deviceId=device_01&udId={udId}&lat=47.63800&lon=-122.132531&searchBuffer=5&isAsync=True&mode=EnterAndExit
   ```
    
   ![Geofence-Abfrage 2](./media/tutorial-geofence/geofence-query2.png)

   Wenn Sie sich die obige JSON-Antwort genau ansehen, erkennen Sie, dass sich die Ausrüstung außerhalb des Unterbereichs und innerhalb des Hauptbereichs befindet. Es wird kein Ereignis ausgelöst und keine E-Mail gesendet.

3. Standort 3: 
  
   ```HTTP
   https://atlas.microsoft.com/spatial/geofence/json?subscription-key={subscription-key}&api-version=1.0&deviceId=device_01&udId={udId}&lat=47.63810783315048&lon=-122.13336020708084&searchBuffer=5&isAsync=True&mode=EnterAndExit
   ```

   ![Geofence-Abfrage 3](./media/tutorial-geofence/geofence-query3.png)

   Eine Zustandsänderung ist eingetreten, und jetzt befindet sich die Ausrüstung sowohl innerhalb des Hauptbereichs als auch innerhalb des Unterbereichs. Diese Änderung bewirkt die Veröffentlichung eines Ereignisses, und eine Benachrichtigungs-E-Mail wird an den Baustellenleiter gesendet.

4. Standort 4: 

   ```HTTP
   https://atlas.microsoft.com/spatial/geofence/json?subscription-key={subscription-key}&api-version=1.0&deviceId=device_01&udId={udId}&lat=47.637988&lon=-122.1338344&searchBuffer=5&isAsync=True&mode=EnterAndExit
   ```
  
   ![Geofence-Abfrage 4](./media/tutorial-geofence/geofence-query4.png)

   Wenn Sie sich die entsprechende Antwort genau ansehen, erkennen Sie, dass hier kein Ereignis veröffentlicht wird, obwohl die Ausrüstung den Geofence des Unterbereichs verlassen hat. Anhand der vom Benutzer angegebenen Uhrzeit in der GET-Anforderung können Sie erkennen, dass der Geofence-Unterbereich für diesen Zeitraum abgelaufen ist. Die Ausrüstung befindet sich immer noch innerhalb des Geofence-Hauptbereichs. Im Antworttext unter `expiredGeofenceGeometryId` sehen Sie auch die Geometrie-ID des Geofence-Unterbereichs.


5. Standort 5:
      
   ```HTTP
   https://atlas.microsoft.com/spatial/geofence/json?subscription-key={subscription-key}&api-version=1.0&deviceId=device_01&udId={udId}&lat=47.63799&lon=-122.134505&userTime=2019-01-16&searchBuffer=5&isAsync=True&mode=EnterAndExit
   ```

   ![Geofence-Abfrage 5](./media/tutorial-geofence/geofence-query5.png)

   Sie können sehen, dass die Ausrüstung den Geofence des Hauptbereichs der Baustelle verlassen hat. Ein Ereignis wird veröffentlicht, und eine Benachrichtigungs-E-Mail wird an den Betriebsleiter gesendet.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurde beschrieben, wie Sie einen Geofence einrichten, indem Sie ihn mit der Datenupload-API in Azure Maps und den Datendienst hochladen. Sie haben auch erfahren, wie Sie Azure Maps Event Grid nutzen, um Geofence-Ereignisse zu abonnieren und zu verarbeiten. 

* Unter [Behandeln von Inhaltstypen in Azure Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-content-type) finden Sie Informationen dazu, wie Sie Logic Apps zum Analysieren von JSON-Code nutzen, um eine komplexere Logik zu erstellen.
* Weitere Informationen zu Ereignishandlern in Event Grid finden Sie unter [Ereignishandler in Azure Event Grid](https://docs.microsoft.com/azure/event-grid/event-handlers).
