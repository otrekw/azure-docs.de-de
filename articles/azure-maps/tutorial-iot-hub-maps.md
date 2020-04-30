---
title: 'Tutorial: Implementieren der räumlichen IoT-Analyse | Microsoft Azure Maps'
description: Hier wird beschrieben, wie Sie IoT Hub in Microsoft Azure Maps-Dienst-APIs integrieren.
author: philmea
ms.author: philmea
ms.date: 11/12/2019
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.custom: mvc
ms.openlocfilehash: cfea9aa7bfcc9a9698bb93bdf54797481b8539ce
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "80333963"
---
# <a name="tutorial-implement-iot-spatial-analytics-using-azure-maps"></a>Tutorial: Implementieren der räumlichen IoT-Analyse mit Azure Maps

Ein gängiges IoT-Szenario ist die Erfassung und Nachverfolgung relevanter Ereignisse in Raum und Zeit. Beispielszenarien wären etwa die Verwaltung von Fuhrparks, Assetnachverfolgung, Mobilität und Smart City-Anwendungen. In diesem Tutorial wird Schritt für Schritt ein Lösungsmuster unter Verwendung der Azure Maps-APIs erläutert. Relevante Ereignisse werden von IoT Hub unter Verwendung des von Event Grid bereitgestellten Ereignisabonnementmodells erfasst.

In diesem Lernprogramm führen Sie folgende Schritte aus:

> [!div class="checklist"]
> * Erstellen Sie einen IoT Hub.
> * Hochladen des Geofencebereichs in den Azure Maps-Datendienst per Datenupload-API
> * Erstellen einer Funktion in Azure Functions und Implementieren von Geschäftslogik basierend auf einer räumlichen Azure Maps-Analyse
> * Abonnieren von IoT-Gerätetelemetrieereignissen aus der Azure-Funktion per Event Grid
> * Filtern der Telemetrieereignisse per IoT Hub-Nachrichtenrouting
> * Erstellen eines Speicherkontos zum Speichern relevanter Ereignisdaten
> * Simulieren eines IoT-Geräts in einem Fahrzeug
    

## <a name="use-case"></a>Anwendungsfall

In diesem Tutorialszenario möchte ein Mietwagenunternehmen seine Mietfahrzeuge überwachen und Ereignisse für die Fahrzeuge protokollieren. Mietwagenunternehmen vermieten Fahrzeuge häufig für eine bestimmte geografische Region. Daher müssen sie die Position während der Vermietung nachverfolgen. Wenn ein Fahrzeug die ausgewählte geografische Region verlässt, muss dies protokolliert werden. Durch die Protokollierung der Daten wird die ordnungsgemäße Behandlung von Richtlinien, Gebühren und anderen geschäftlichen Aspekten sichergestellt.

In unserem Anwendungsfall sind die Mietwagen mit IoT-Geräten ausgestattet, die regelmäßig Telemetriedaten an Azure IoT Hub senden. Die Telemetriedaten beinhalten den aktuellen Standort sowie die Information, ob der Motor des Fahrzeugs läuft. Das Schema für den Gerätestandort basiert auf dem [IoT Plug & Play-Schema für Geodaten](https://github.com/Azure/IoTPlugandPlay/blob/master/Schemas/geospatial.md). Das Gerätetelemetrieschema des Mietwagens sieht wie folgt aus:

```JSON
{
    "data": {
        "properties": {
            "Engine": "ON"
        },
        "systemProperties": {
            "iothub-content-type": "application/json",
            "iothub-content-encoding": "utf-8",
            "iothub-connection-device-id": "ContosoRentalDevice",
            "iothub-connection-auth-method": "{\"scope\":\"device\",\"type\":\"sas\",\"issuer\":\"iothub\",\"acceptingIpFilterRule\":null}",
            "iothub-connection-auth-generation-id": "636959817064335548",
            "iothub-enqueuedtime": "2019-06-18T00:17:20.608Z",
            "iothub-message-source": "Telemetry"
        },
        "body": { 
            "location": { 
                "type": "Point",
                "coordinates": [ -77.025988698005662, 38.9015330523316 ]
            } 
        } 
    }
}
```

Wir verwenden die Telemetriedaten eines fahrzeuginternen Geräts, um unser Ziel zu erreichen. Wir möchten Geofencingregeln ausführen. Und wir möchten darauf reagieren, wenn durch ein empfangenes Ereignis angegeben wird, dass das Fahrzeug bewegt wurde. Zu diesem Zweck abonnieren wir über Event Grid die Gerätetelemetrieereignisse von IoT Hub. 

Event Grid-Abonnements können auf mehrere Arten erstellt werden. In diesem Tutorial verwenden wir Azure Functions. Azure Functions reagiert auf in Event Grid veröffentlichte Ereignisse. Darüber hinaus wird Logik für ein Mietwagenunternehmen implementiert (basierend auf der räumlichen Azure Maps-Analyse). 

Durch den Code in der Azure-Funktion wird überprüft, ob das Fahrzeug den Geofencebereich verlassen hat. Falls ja, werden von der Azure-Funktion weitere Informationen gesammelt (etwa die Adresse des aktuellen Standorts). Mit der Funktion wird auch Logik implementiert, um aussagekräftige Ereignisdaten in einem Datenblobspeicher zu speichern und so eine Beschreibung der Ereignisumstände zu ermöglichen. 

Die Ereignisumstände können sowohl für das Mietwagenunternehmen als auch für den Kunden hilfreich sein. Das folgende Diagramm enthält eine allgemeine Übersicht über das System.

 
  <center>

  ![Systemübersicht](./media/tutorial-iot-hub-maps/system-diagram.png)
  
  </center>

In der folgenden Abbildung ist der Geofencebereich blau eingefärbt. Die Route des Mietwagens ist als grüne Linie dargestellt.

  ![Geofenceroute](./media/tutorial-iot-hub-maps/geofence-route.png)


## <a name="prerequisites"></a>Voraussetzungen 

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Um die Schritte in diesem Tutorial ausführen zu können, müssen Sie zunächst im Azure-Portal eine Ressourcengruppe erstellen. Gehen Sie zum Erstellen einer Ressourcengruppe wie folgt vor:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Klicken Sie auf **Ressourcengruppen**.
    
   ![Ressourcengruppen](./media/tutorial-iot-hub-maps/resource-group.png)

3. Wählen Sie unter **Ressourcengruppe** die Option **Hinzufügen** aus.
    
   ![Hinzufügen von Ressourcengruppen](./media/tutorial-iot-hub-maps/add-resource-group.png) 

4. Geben Sie die folgenden Eigenschaftswerte ein:
    * **Abonnement:** Wählen Sie Ihr Azure-Abonnement.
    * **Ressourcengruppe:** Geben Sie „ContosoRental“ als Ressourcengruppennamen ein.
    * **Region:** Wählen Sie eine Region für die Ressourcengruppe aus.  

    ![Details zur Ressourcengruppe](./media/tutorial-iot-hub-maps/resource-details.png)

    Wählen Sie **Überprüfen + erstellen** und auf der nächsten Seite die Option **Erstellen** aus.

### <a name="create-an-azure-maps-account"></a>Erstellen eines Azure Maps-Kontos 

Um Geschäftslogik auf der Grundlage der räumlichen Azure Maps-Analyse implementieren zu können, muss in der erstellten Ressourcengruppe ein Azure Maps-Konto erstellt werden. Befolgen Sie die Anleitung zum [Erstellen eines Kontos](quick-demo-map-app.md#create-an-account-with-azure-maps), um ein Azure Maps-Kontoabonnement im Tarif S1 zu erstellen. Führen Sie die Schritte unter [Abrufen des Primärschlüssels](quick-demo-map-app.md#get-the-primary-key-for-your-account) aus, um den Primärschlüssel für Ihr Konto zu erhalten. Weitere Informationen zur Authentifizierung in Azure Maps finden Sie unter [Verwalten der Authentifizierung in Azure Maps](how-to-manage-authentication.md).



### <a name="create-a-storage-account"></a>Speicherkonto erstellen

Sie erstellen zum Protokollieren von Ereignisdaten das universelle Konto **v2storage**, das Zugriff auf sämtliche Azure Storage-Dienste bietet: Blobs, Dateien, Warteschlangen, Tabellen und Datenträger.  Sie müssen dieses Speicherkonto in der Ressourcengruppe „ContosoRental“ platzieren, um Daten als Blobs speichern zu können. Befolgen Sie zum Erstellen eines Speicherkontos die Anleitung unter [Speicherkonto erstellen](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?toc=%2Fazure%2Fstorage%2Fblobs%2Ftoc.json&tabs=azure-portal). Als Nächstes müssen wir einen Container zum Speichern von Blobs erstellen. Führen Sie hierzu die folgenden Schritte aus:

1. Navigieren Sie in Ihrem Speicherkonto (Blob, Datei, Tabelle, Warteschlange) zu „Container“.

    ![Blobs](./media/tutorial-iot-hub-maps/blobs.png)

2. Klicken Sie oben links auf die Containerschaltfläche, geben Sie Ihrem Container den Namen „contoso-rental-logs“, und klicken Sie auf „OK“.

    ![blob-container](./media/tutorial-iot-hub-maps/blob-container.png)

3. Navigieren Sie in Ihrem Speicherkonto zum Blatt **Zugriffsschlüssel**, und kopieren Sie den Namen des Speicherkontos und den Zugriffsschlüssel. Diese werden in einem späteren Schritt benötigt.

    ![access-keys](./media/tutorial-iot-hub-maps/access-keys.png)


Wir verfügen nun über ein Speicherkonto und einen Container zum Protokollieren von Ereignisdaten. Als Nächstes erstellen wir einen IoT-Hub.

### <a name="create-an-iot-hub"></a>Erstellen eines IoT Hubs

IoT Hub ist ein verwalteter Dienst in der Cloud. Die IoT Hub-Instanz fungiert als zentraler Nachrichtenhub für die bidirektionale Kommunikation zwischen einer IoT-Anwendung und den Geräten, die von der Anwendung verwaltet werden. Erstellen Sie eine IoT Hub-Instanz in der Ressourcengruppe „ContosoRental“, um Gerätetelemetrienachrichten an eine Event Grid-Instanz weiterzuleiten. Richten Sie eine Nachrichtenroutenintegration ein, um Nachrichten basierend auf dem Motorstatus des Fahrzeugs zu filtern. Darüber hinaus senden wir Gerätetelemetrienachrichten jeweils an die Event Grid-Instanz, wenn das Fahrzeug bewegt wird.

> [!Note] 
> Die IoT Hub-Funktionalität zum Veröffentlichen von Gerätetelemetrieereignissen über Event Grid befindet sich in der öffentlichen Vorschauphase (Public Preview). Features in der öffentlichen Vorschauphase sind in allen Regionen verfügbar, mit Ausnahme von **„USA, Osten“, „USA, Westen“, „Europa, Westen“, „Azure Government“, „Azure China 21Vianet“** und **„Azure Deutschland“** . 

Erstellen Sie eine IoT Hub-Instanz, indem Sie die Schritte im Abschnitt [Erstellen eines IoT Hubs](https://docs.microsoft.com/azure/iot-hub/quickstart-send-telemetry-dotnet#create-an-iot-hub) ausführen.


### <a name="register-a-device"></a>Registrieren eines Geräts 

Zum Herstellen einer Verbindung mit der IoT Hub-Instanz muss ein Gerät registriert werden. Führen Sie die folgenden Schritte aus, um ein Gerät bei IoT Hub zu registrieren:

1. Klicken Sie auf Ihrer IoT Hub-Instanz auf das Blatt „IoT-Geräte“ und auf „Neu“.

    ![add-device](./media/tutorial-iot-hub-maps/add-device.png)

2. Geben Sie Ihrem IoT-Gerät auf der Seite für die Geräteerstellung einen Namen, und klicken Sie auf „Speichern“.
    
    ![register-device](./media/tutorial-iot-hub-maps/register-device.png)

3. Speichern Sie den Wert für **Primäre Verbindungszeichenfolge** Ihres Geräts zur Verwendung in einem späteren Schritt, in dem Sie einen Platzhalter durch diese Verbindungszeichenfolge ersetzen müssen.

    ![add-device](./media/tutorial-iot-hub-maps/connection-string.png)

## <a name="upload-geofence"></a>Hochladen der Geofencedaten

Wir nutzen die [Postman-Anwendung](https://www.getpostman.com) zum [Hochladen der Geofencedaten](https://docs.microsoft.com/azure/azure-maps/geofence-geojson) in den Azure Maps-Dienst (unter Verwendung der Data Upload-API von Azure Maps). Alle Ereignisse, bei denen sich das Fahrzeug außerhalb dieses Geofencebereichs befindet, werden protokolliert.

Öffnen Sie die Postman-App, und führen Sie die unten angegebenen Schritte aus, um die Geofencedaten mit der Data Upload-API von Azure Maps hochzuladen.  

1. Klicken Sie in der Postman-App auf „New“ | „Create new“ („Neu“ | „Neu erstellen“), und wählen Sie anschließend „Request“ (Anfordern). Geben Sie einen Anforderungsnamen für das Hochladen von Geofencedaten ein, wählen Sie eine Sammlung oder einen Ordner zum Speichern aus, und klicken Sie auf „Save“ (Speichern).

    ![Hochladen von Geofences mit Postman](./media/tutorial-iot-hub-maps/postman-new.png)

2. Wählen Sie auf der Registerkarte „Builder“ (Generator) die POST-HTTP-Methode aus, und geben Sie die folgende URL ein, um eine POST-Anforderung zu senden.

    ```HTTP
    https://atlas.microsoft.com/mapData/upload?subscription-key={subscription-key}&api-version=1.0&dataFormat=geojson
    ```
    
    Der „geojson“-Wert für den Parameter `dataFormat` im URL-Pfad gibt das Format der hochzuladenden Daten an.

3. Klicken Sie auf **Params**, und geben Sie die folgenden Schlüssel-Wert-Paare ein, die für die POST-Anforderungs-URL verwendet werden sollen. Ersetzen Sie den Abonnementschlüsselwert durch Ihren Azure Maps-Schlüssel.
   
    ![Schlüssel-Wert-Parameter: Postman](./media/tutorial-iot-hub-maps/postman-key-vals.png)

4. Klicken Sie auf **Text**, und wählen Sie dann das Rohdaten-Eingabeformat (**raw**) aus. Wählen Sie in der Dropdownliste anschließend **JSON (application/text)** (JSON (Anwendung/Text)) als Eingabeformat aus. Öffnen Sie die [hier](https://raw.githubusercontent.com/Azure-Samples/iothub-to-azure-maps-geofencing/master/src/Data/geofence.json?token=AKD25BYJYKDJBJ55PT62N4C5LRNN4) verfügbare JSON-Datendatei, und kopieren Sie den JSON-Code als hochzuladende Daten in den Abschnitt „body“ (Text). Klicken Sie anschließend auf **Send** (Senden).
    
    ![Bereitstellen von Daten](./media/tutorial-iot-hub-maps/post-json-data.png)
    
5. Sehen Sie sich die Antwortheader an. Bei einer erfolgreichen Anforderung enthält der Adressheader (**Location**) den Status-URI, um den aktuellen Status der Uploadanforderung zu überprüfen. Der Status-URI hat das folgende Format. 

   ```HTTP
   https://atlas.microsoft.com/mapData/{uploadStatusId}/status?api-version=1.0
   ```

6. Kopieren Sie Ihren Status-URI, und fügen Sie an ihn den Parameter `subscription-key` an. Weisen Sie den Wert Ihres Azure Maps Konto-Kontoabonnementschlüssel dem Parameter `subscription-key` zu. Der Status-URI sollte das folgende Format haben, und `{Subscription-key}` muss durch Ihren Abonnementschlüssel ersetzt werden:

   ```HTTP
   https://atlas.microsoft.com/mapData/{uploadStatusId}/status?api-version=1.0&subscription-key={Subscription-key}
   ```

7. Um den Wert von `udId` abzurufen, öffnen Sie in der Postman-App eine neue Registerkarte, wählen auf der Registerkarte „Builder“ (Generator) die HTTP-Methode „GET“ aus und führen eine GET-Anforderung für den Status-URI aus. Wenn der Datenupload erfolgreich ausgeführt wurde, erhalten Sie im Antworttext einen Wert für „udId“. Kopieren Sie die udId zur späteren Verwendung.

   ```JSON
   {
    "udid" : "{udId}"
   }
   ```


Als Nächstes erstellen wir eine Azure-Funktion in der Ressourcengruppe „ContosoRental“ und richten dann in IoT Hub eine Nachrichtenroute ein, um Gerätetelemetrienachrichten zu filtern.


## <a name="create-an-azure-function-and-add-an-event-grid-subscription"></a>Erstellen einer Azure-Funktion und Hinzufügen eines Event Grid-Abonnements

Azure Functions ist ein serverloser Computedienst, mit dem wir Code bedarfsgesteuert ausführen können, ohne explizit eine Computeinfrastruktur bereitstellen oder verwalten zu müssen. Weitere Informationen zu Azure Functions finden Sie in der Dokumentation zu [Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-overview). 

Für die Logik, die wir in der Funktion implementieren, werden die Standortdaten der Gerätetelemetrie im Fahrzeug genutzt, um den Geofencestatus zu bewerten. Wenn ein Fahrzeug den Geofencebereich verlässt, erfasst die Funktion weitere Informationen (beispielsweise die Adresse des Standorts) über die [API für die umgekehrte Adresssuche](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse). Diese API wandelt eine bestimmte Standortkoordinate in eine für Menschen lesbare Adresse um. 

Alle relevanten Ereignisinformationen werden dann im Blobspeicher gespeichert. In Schritt 5 unten wird auf den ausführbaren Code verwiesen, mit dem diese Logik implementiert wird. Führen Sie die unten angegebenen Schritte aus, um eine Azure-Funktion zu erstellen, mit der Datenprotokolle an den Blobcontainer im Blobspeicherkonto gesendet werden, und fügen Sie ein Event Grid-Abonnement hinzu.

1. Wählen Sie im Dashboard des Azure-Portals die Option zum Erstellen einer Ressource aus. Wählen Sie in der Liste mit den verfügbaren Ressourcentypen die Option **Compute** und dann **Funktions-App** aus.

    ![create-resource](./media/tutorial-iot-hub-maps/create-resource.png)

2. Geben Sie Ihrer Funktions-App auf der Erstellungsseite **Funktions-App** einen Namen. Wählen Sie unter **Ressourcengruppe** die Option **Use existing** (Vorhandene verwenden) und anschließend in der Dropdownliste die Option „ContosoRental“ aus. Wählen Sie „.NET Core“ als Runtimestapel aus. Wählen Sie unter **Hosting** für **Speicherkonto** den Speicherkontonamen aus einem vorherigen Schritt aus. In diesem vorherigen Schritt haben wir dem Speicherkonto den Namen **v2storage** gegeben.  Wählen Sie dann **Bewerten + erstellen** aus.
    
    ![create-app](./media/tutorial-iot-hub-maps/rental-app.png)

2. Überprüfen Sie die Details der Funktions-App, und wählen Sie „Erstellen“ aus.

3. Nach der Erstellung der App müssen wir ihr eine Funktion hinzufügen. Navigieren Sie zur Funktions-App. Klicken Sie auf **Neue Funktion**, um eine Funktion hinzuzufügen, und wählen Sie als Entwicklungsumgebung die Option **Im Portal** aus. Wählen Sie anschließend **Weiter** aus.

    ![create-function](./media/tutorial-iot-hub-maps/function.png)

4. Wählen Sie **More templates** (Weitere Vorlagen), und klicken Sie auf **Finish and view templates** (Fertig stellen und Vorlagen anzeigen). 

5. Wählen Sie die Vorlage mit einem **Azure Event Grid-Trigger** aus. Installieren Sie die Erweiterungen, wenn Sie dazu aufgefordert werden, geben Sie der Funktion einen Namen, und wählen Sie **Erstellen** aus.

    ![function-template](./media/tutorial-iot-hub-maps/eventgrid-funct.png)
    
    Der **Azure Event Hub-Trigger** und der **Azure Event Grid-Trigger** haben ähnliche Symbole. Stellen Sie sicher, dass Sie den **Azure Event Grid-Trigger** auswählen.

6. Kopieren Sie den [C#-Code](https://github.com/Azure-Samples/iothub-to-azure-maps-geofencing/blob/master/src/Azure%20Function/run.csx) in Ihre Funktion.
 
7. Ersetzen Sie im C#-Skript die folgenden Parameter. Klicken Sie auf **Speichern**. Klicken Sie noch nicht auf **Ausführen**.
    * Ersetzen Sie **SUBSCRIPTION_KEY** durch den primären Abonnementschlüssel Ihres Azure Maps-Kontos.
    * Ersetzen Sie **UDID** durch die udId des hochgeladenen Geofencebereichs. 
    * Mit der Funktion **CreateBlobAsync** im Skript wird im Datenspeicherkonto ein Blob pro Ereignis erstellt. Ersetzen Sie **ACCESS_KEY**, **ACCOUNT_NAME** und **STORAGE_CONTAINER_NAME** durch den Zugriffsschlüssel, den Kontonamen und den Datenspeichercontainer Ihres Speicherkontos.

10. Klicken Sie auf **Event Grid-Abonnement hinzufügen**.
    
    ![add-event-grid](./media/tutorial-iot-hub-maps/add-egs.png)

11. Füllen Sie die Abonnementdetails aus. Geben Sie unter **DETAILS ZUM EREIGNISABONNEMENT** einen Namen für Ihr Ereignisabonnement ein. Wählen Sie als Ereignisschema die Option „Event Grid-Schema“ aus. Wählen Sie unter **THEMENDETAILS** den Thementyp „Azure IoT Hub-Konten“ aus. Wählen Sie das gleiche Abonnement aus, das Sie auch beim Erstellen der Ressourcengruppe verwendet haben, und wählen Sie „ContosoRental“ als Ressourcengruppe aus. Wählen Sie die von Ihnen erstellte IoT Hub-Instanz als Ressource aus. Wählen Sie **Gerätetelemetrie** als Ereignistyp aus. Nachdem Sie diese Optionen ausgewählt haben, sehen Sie, dass der Thementyp automatisch in „IoT Hub“ geändert wurde.

    ![event-grid-subscription](./media/tutorial-iot-hub-maps/af-egs.png)
 

## <a name="filter-events-using-iot-hub-message-routing"></a>Filtern von Ereignissen per IoT Hub-Nachrichtenrouting

Nachdem Sie der Azure-Funktion ein Event Grid-Abonnement hinzugefügt haben, wird auf dem Blatt **Nachrichtenrouting** von IoT Hub eine Standardnachrichtenroute zu Event Grid angezeigt. Das Nachrichtenrouting ermöglicht es Ihnen, verschiedene Datentypen an verschiedene Endpunkte weiterzuleiten. Dadurch können Sie beispielsweise Gerätetelemetrienachrichten, Gerätelebenszyklusereignisse und Änderungsereignisse für Gerätezwillinge weiterleiten. Weitere Informationen zum IoT Hub-Nachrichtenrouting finden Sie unter [Verwenden des IoT Hub-Nachrichtenroutings zum Senden von D2C-Nachrichten an verschiedene Endpunkte](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-d2c).

![hub-EG-route](./media/tutorial-iot-hub-maps/hub-route.png)

In unserem Beispielszenario möchten wir alle Nachrichten herausfiltern, bei denen der Mietwagen bewegt wird. Um diese Gerätetelemetrieereignisse für Event Grid zu veröffentlichen, filtern wird die Ereignisse, für die die Eigenschaft `Engine` auf **ON** festgelegt ist, mithilfe der Routingabfrage. Es gibt verschiedene Möglichkeiten zum Abfragen von IoT-Gerät-zu-Cloud-Nachrichten. Weitere Informationen zur Syntax für das Nachrichtenrouting finden Sie unter [Abfragesyntax für das IoT Hub-Nachrichtenrouting](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-routing-query-syntax). Klicken Sie zum Erstellen einer Routingabfrage auf die Route **RouteToEventGrid**, und ersetzen Sie die **Routingabfrage** durch **„Engine='ON'“** . Klicken Sie anschließend auf **Speichern**. IoT Hub veröffentlicht jetzt nur Gerätetelemetriedaten, wenn „Engine“ auf „ON“ festgelegt ist.

![hub-EG-filter](./media/tutorial-iot-hub-maps/hub-filter.png)


## <a name="send-telemetry-data-to-iot-hub"></a>Senden von Telemetriedaten an IoT Hub

Sobald unsere Azure-Funktion einsatzbereit ist, können wir Telemetriedaten an die IoT Hub-Instanz senden, von wo aus sie dann an Event Grid weitergeleitet werden. Wir verwenden hier eine C#-Anwendung, um Standortdaten für ein Gerät in einem Mietwagen zu simulieren. Zum Ausführen der Anwendung benötigen Sie auf Ihrem Entwicklungscomputer .NET Core SDK 2.1.0 oder höher. Führen Sie die folgenden Schritte aus, um simulierte Telemetriedaten an IoT Hub zu senden.

1. Laden Sie das C#-Projekt [rentalCarSimulation](https://github.com/Azure-Samples/iothub-to-azure-maps-geofencing/tree/master/src/rentalCarSimulation) herunter. 

2. Öffnen Sie die Datei „simulatedCar.cs“ in einem Text-Editor Ihrer Wahl, und ersetzen Sie den Wert von `connectionString` durch den Wert, den Sie beim Registrieren des Geräts gespeichert haben. Speichern Sie anschließend die Änderungen der Datei.
 
3. Vergewissern Sie sich, dass auf Ihrem Computer .NET Core installiert ist. Navigieren Sie in Ihrem lokalen Terminalfenster zum Stammordner des C#-Projekts, und führen Sie den folgenden Befehl aus, um die erforderlichen Pakete für die simulierte Geräteanwendung zu installieren:
    
    ```cmd/sh
    dotnet restore
    ```

4. Führen Sie in demselben Terminal den folgenden Befehl aus, um die Anwendung für die Mietwagensimulation zu erstellen und auszuführen:

    ```cmd/sh
    dotnet run
    ```

  Ihr lokales Terminal sollte wie folgt aussehen.

  ![Terminalausgabe](./media/tutorial-iot-hub-maps/terminal.png)

Wenn Sie den Blobspeichercontainer jetzt öffnen, sollten vier Blobs für Standorte angezeigt werden, an denen sich das Fahrzeug außerhalb des Geofencebereichs befunden hat.

![Blobeingabe](./media/tutorial-iot-hub-maps/blob.png)

In der Karte unten werden vier Punkte angezeigt, an denen sich das Fahrzeug außerhalb des Geofencebereichs befunden hat. Diese Protokollierung erfolgt in regelmäßigen Intervallen.

![Anzeige von Verstößen auf Karte](./media/tutorial-iot-hub-maps/violation-map.png)

## <a name="next-steps"></a>Nächste Schritte

Informationen zu den in diesem Tutorial verwendeten Azure Maps-APIs finden Sie unter:

* [Get Search Address Reverse](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse)
* [Get Geofence](https://docs.microsoft.com/rest/api/maps/spatial/getgeofence)

Eine vollständige Liste mit Azure Maps-REST-APIs finden Sie unter:

* [Azure Maps-REST-APIs](https://docs.microsoft.com/rest/api/maps/spatial/getgeofence)

Weitere Informationen zu IoT Plug & Play finden Sie in dem folgenden Artikel:

* [Dokumentation zu IoT Plug & Play (Vorschauversion)](https://docs.microsoft.com/azure/iot-pnp)

Eine Liste mit Geräten, die über eine Azure-Zertifizierung für IoT verfügen, finden Sie unter:

* [Geräte mit Azure-Zertifizierung](https://catalog.azureiotsolutions.com/)

Weitere Informationen zum Senden von Telemetriedaten von einem Gerät an die Cloud (und umgekehrt) finden Sie unter:

* [Senden von Telemetriedaten von einem Gerät](https://docs.microsoft.com/azure/iot-hub/quickstart-send-telemetry-dotnet)
