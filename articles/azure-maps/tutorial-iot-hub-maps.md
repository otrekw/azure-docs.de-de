---
title: 'Tutorial: Implementieren der räumlichen IoT-Analyse mit Microsoft Azure Maps'
description: Hier wird beschrieben, wie Sie IoT Hub in Microsoft Azure Maps-Dienst-APIs integrieren.
author: anastasia-ms
ms.author: v-stharr
ms.date: 09/01/2020
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.custom: mvc
ms.openlocfilehash: 4150464b5c59b631afea0c788b1e351dee5185f9
ms.sourcegitcommit: 58d3b3314df4ba3cabd4d4a6016b22fa5264f05a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89299391"
---
# <a name="tutorial-implement-iot-spatial-analytics-using-azure-maps"></a>Tutorial: Implementieren der räumlichen IoT-Analyse mit Azure Maps

Ein gängiges IoT-Szenario ist die Erfassung und Nachverfolgung relevanter Ereignisse in Raum und Zeit. Beispielszenarien wären etwa die Verwaltung von Fuhrparks, Assetnachverfolgung, Mobilität und Smart City-Anwendungen. In diesem Tutorial werden Sie durch eine Lösung geführt, die mit den Azure Maps-APIs die Bewegung gebrauchter Mietwagen nachverfolgt.

In diesem Lernprogramm führen Sie folgende Schritte aus:

> [!div class="checklist"]
> * Erstellen eines Azure Storage-Kontos zur Protokollierung der Autonachverfolgungsdaten.
> * Hochladen eines Geofence per Datenupload-API in den Azure Maps-Datendienst.
> * Erstellen einer IoT Hub-Instanz und Registrieren eines Geräts.
> * Erstellen einer Funktion in Azure Functions und Implementieren von Geschäftslogik basierend auf einer räumlichen Azure Maps-Analyse
> * Abonnieren von IoT-Gerätetelemetrieereignissen aus der Azure-Funktion per Event Grid
> * Filtern der Telemetrieereignisse per IoT Hub-Nachrichtenrouting

## <a name="prerequisites"></a>Voraussetzungen

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. [Erstellen Sie ein Azure Maps-Konto](quick-demo-map-app.md#create-an-azure-maps-account).

3. [Abrufen eines Primärschlüssels](quick-demo-map-app.md#get-the-primary-key-for-your-account) (auch primärer Schlüssel oder Abonnementschlüssel genannt) Weitere Informationen zur Authentifizierung in Azure Maps finden Sie unter [Verwalten der Authentifizierung in Azure Maps](how-to-manage-authentication.md).

4. [Erstellen Sie eine Ressourcengruppe](https://docs.microsoft.com/azure/azure-resource-manager/management/manage-resource-groups-portal#create-resource-groups). In diesem Tutorial nennen wir unsere Ressourcengruppe *ContosoRental*, aber Sie können einen beliebigen Namen auswählen.

5. Laden Sie das [C#-Projekt rentalCarSimulation](https://github.com/Azure-Samples/iothub-to-azure-maps-geofencing/tree/master/src/rentalCarSimulation) herunter.

In diesem Tutorial wird die Anwendung [Postman](https://www.postman.com/) verwendet. Sie können aber auch eine andere API-Entwicklungsumgebung verwenden.

## <a name="use-case-rental-car-tracking"></a>Anwendungsfall: Nachverfolgung von Mietwagen

Dieses Tutorial veranschaulicht die folgenden Szenarios: Ein Autoverleihunternehmen möchte Standortinformationen, zurückgelegte Entfernung und Betriebsstatus seiner Mietfahrzeuge protokollieren. Außerdem möchte das Unternehmen diese Informationen immer dann speichern, wenn ein Auto die richtige autorisierte geografische Region verlässt.

In unserem Anwendungsfall sind die Mietwagen mit IoT-Geräten ausgestattet, die regelmäßig Telemetriedaten an Azure IoT Hub senden. Die Telemetriedaten beinhalten den aktuellen Standort sowie die Information, ob der Motor des Fahrzeugs läuft. Das Schema für den Gerätestandort basiert auf dem [IoT Plug & Play-Schema für Geodaten](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v1-preview/schemas/geospatial.md). Das Gerätetelemetrieschema des Mietwagens sieht wie der folgende JSON-Code aus:

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

In diesem Tutorial wird nur ein Fahrzeug nachverfolgt. Nachdem wir die Azure-Dienste eingerichtet haben, müssen Sie das [C#-Projekt rentalCarSimulation](https://github.com/Azure-Samples/iothub-to-azure-maps-geofencing/tree/master/src/rentalCarSimulation) herunterladen, um den Fahrzeugsimulator auszuführen. Der gesamte Prozess vom Ereignis bis zur Funktionsausführung wird in den folgenden Schritten zusammengefasst:

1. Das Gerät im Fahrzeug sendet Telemetriedaten an den IoT-Hub.

2. Wenn der Fahrzeugmotor läuft, veröffentlicht der IoT-Hub die Telemetriedaten im Event Grid.

3. Eine Azure-Funktion wird aufgrund ihres Ereignisabonnements für Gerätetelemetrieereignisse ausgelöst.

4. Die Funktion protokolliert die Koordinaten der Position des Fahrzeuggeräts, den Zeitpunkt des Ereignisses und die Geräte-ID. Anschließend wird anhand der [Spatial Geofence-GET-API](https://docs.microsoft.com/rest/api/maps/spatial/getgeofence) bestimmt, ob das Fahrzeug außerhalb des Geofence gefahren wurde. Wenn es die Geofencegrenzen überschritten hat, speichert die Funktion die vom Ereignis empfangenen Positionsdaten in unserem Blobcontainer. Außerdem fragt unsere Funktion die [inverse Adresssuche](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) ab, um die Koordinatenposition in eine Straße zu übersetzen und mit den restlichen Gerätestandortdaten zu speichern.

Das folgende Diagramm enthält eine allgemeine Übersicht über das System.

   :::image type="content" source="./media/tutorial-iot-hub-maps/system-diagram.png" border="false" alt-text="Systemübersicht":::

In der folgenden Abbildung ist der Geofencebereich blau eingefärbt. Die Route des Mietwagens ist als grüne Linie dargestellt.

   :::image type="content" source="./media/tutorial-iot-hub-maps/geofence-route.png" border="false" alt-text="Geofenceroute":::

## <a name="create-an-azure-storage-account"></a>Erstellen eines Azure-Speicherkontos

Zum Speichern von Nachverfolgungsdaten zu Verstößen des Fahrzeugs erstellen wir ein [universelles v2-Speicherkonto](https://docs.microsoft.com/azure/storage/common/storage-account-overview#general-purpose-v2-accounts) in Ihrer Ressourcengruppe. Wenn Sie noch keine Ressourcengruppe erstellt haben, befolgen Sie die Anweisungen in [Erstellen von Ressourcengruppe](https://docs.microsoft.com/azure/azure-resource-manager/management/manage-resource-groups-portal#create-resource-groups). In diesem Tutorial nennen wir die Ressourcengruppe *ContosoRental*.

Befolgen Sie zum Erstellen eines Speicherkontos die Anweisungen unter [Speicherkonto erstellen](https://docs.microsoft.com/azure/storage/common/storage-account-create?tabs=azure-portal). In diesem Tutorial nennen wir das Speicherkonto *contosorentalstorage*, Sie können es jedoch beliebig benennen.

Nachdem das Speicherkonto erfolgreich erstellt wurde, müssen wir einen Container zum Speichern von Protokollierungsdaten erstellen.

1. Navigieren Sie zum neu erstellten Speicherkonto. Klicken Sie im Abschnitt „Zusammenfassung“ auf den Link **Container**.

    :::image type="content" source="./media/tutorial-iot-hub-maps/containers.png" alt-text="Container für Blobspeicher":::

2. Klicken Sie in der linken oberen Ecke auf die Schaltfläche **+ Container**. Rechts im Browser wird ein Fenster angezeigt. Nennen Sie Ihren Container *contoso-rental-logs*, und klicken Sie auf **Erstellen**.

     :::image type="content" source="./media/tutorial-iot-hub-maps/container-new.png" alt-text="Erstellen eines Blobcontainers":::

3. Navigieren Sie in Ihrem Speicherkonto zum Blatt **Zugriffsschlüssel**, und kopieren Sie den **Namen des Speicherkontos** und den Wert des **Schlüssels** in den Abschnitt **key1**. Wir benötigen beide Werte im Abschnitt [Erstellen einer Azure-Funktion und Hinzufügen eines Event Grid-Abonnements](#create-an-azure-function-and-add-an-event-grid-subscription).

    :::image type="content" source="./media/tutorial-iot-hub-maps/access-keys.png" alt-text="Kopieren des Speicherkontonamens und -schlüssels":::

## <a name="upload-a-geofence"></a>Hochladen eines Geofence

Wir verwenden jetzt die [Postman-App](https://www.getpostman.com) zum [Hochladen des Geofence](https://docs.microsoft.com/azure/azure-maps/geofence-geojson) in den Azure Maps-Dienst. Der Geofence definiert den autorisierten geografischen Bereich für unser Mietfahrzeug.  Wir verwenden den Geofence in unserer Azure-Funktion, um zu bestimmen, ob ein Fahrzeug außerhalb des Geofencebereichs bewegt wurde.

Öffnen Sie die Postman-App, und führen Sie die unten angegebenen Schritte aus, um die Geofencedaten mit der Datenupload-API von Azure Maps hochzuladen.  

1. Öffnen Sie die Postman-App. Wählen Sie oben in der Postman-App **New** (Neu) aus. Wählen Sie im Fenster **Create New** (Neu erstellen) die Option **Collection** (Sammlung) aus.  Geben Sie einen Namen für die Sammlung ein, und klicken Sie dann auf **Create** (Erstellen).

2. Klicken Sie erneut auf **New** (Neu), um die Anforderung zu erstellen. Wählen Sie im Fenster **Create New** (Neu erstellen) die Option **Request** (Anforderung) aus. Geben Sie einen Anforderungsnamen (**Request name**) ein. Wählen Sie die im vorherigen Schritt erstellte Sammlung und anschließend **Save** (Speichern) aus.

3. Wählen Sie auf der Registerkarte „Builder“ (Generator) die HTTP-Methode **POST** aus, und geben Sie die folgende URL ein, um die Geofencedaten in die Datenupload-API hochzuladen. Stellen Sie sicher, dass Sie `{subscription-key}` durch Ihren primären Abonnementschlüssel ersetzen.

    ```HTTP
    https://atlas.microsoft.com/mapData/upload?subscription-key={subscription-key}&api-version=1.0&dataFormat=geojson
    ```

    Der „geojson“-Wert für den Parameter `dataFormat` im URL-Pfad gibt das Format der hochzuladenden Daten an.

4. Klicken Sie auf **Text**, und wählen Sie dann das Eingabeformat **roh** aus. Wählen Sie in der Dropdownliste anschließend **JSON** als Eingabeformat. Öffnen Sie die JSON-Datendatei [hier](https://raw.githubusercontent.com/Azure-Samples/iothub-to-azure-maps-geofencing/master/src/Data/geofence.json?token=AKD25BYJYKDJBJ55PT62N4C5LRNN4), und kopieren Sie den JSON-Code in den Textabschnitt. Klicken Sie auf **Send**.

5. Klicken Sie auf die blaue Schaltfläche **Send** (Senden), und warten Sie, bis die Anforderung verarbeitet wurde. Navigieren Sie nach Abschluss der Anforderung zur Registerkarte **Headers** (Header) der Antwort. Kopieren Sie den Wert des Schlüssels **Location** (Speicherort). Hierbei handelt es sich um die Status-URL (`status URL`).

    ```http
    https://atlas.microsoft.com/mapData/operations/<operationId>?api-version=1.0
    ```

6. Erstellen Sie zum Überprüfen des Status des API-Aufrufs eine **GET**-HTTP-Anforderung für `status URL`. An die URL muss zur Authentifizierung der primäre Abonnementschlüssel angefügt werden. Die **GET**-Anforderung sollte wie die folgende URL aussehen:

   ```HTTP
   https://atlas.microsoft.com/mapData/<operationId>/status?api-version=1.0&subscription-key={subscription-key}

7. When the **GET** HTTP request completes successfully, it will return a `resourceLocation`. The `resourceLocation` contains the unique `udid` for the uploaded content. You'll need to copy this `udid` for later use in this tutorial.

      ```json
      {
          "status": "Succeeded",
          "resourceLocation": "https://atlas.microsoft.com/mapData/metadata/{udid}?api-version=1.0"
      }
      ```

## <a name="create-an-azure-iot-hub"></a>Erstellen einer Azure IoT Hub-Instanz

Azure IoT Hub ermöglicht eine äußerst sichere und zuverlässige bidirektionale Kommunikation zwischen Ihrer IoT-Anwendung und den verwalteten Geräten.  In unserem Szenario möchten wir Informationen von unserem Gerät im Fahrzeug erhalten, um die Position des Mietfahrzeugs zu ermitteln. In diesem Abschnitt erstellen wir einen IoT-Hub innerhalb der *ContosoRental*-Ressourcengruppe. Der IoT-Hub ist für das Veröffentlichen von Gerätetelemetrieereignissen verantwortlich.

> [!NOTE]
> Die Funktionalität des IoT-Hubs zum Veröffentlichen von Gerätetelemetrieereignissen über Event Grid befindet sich in der öffentlichen Vorschauphase (Public Preview). Features in der öffentlichen Vorschauphase sind in allen Regionen verfügbar, mit Ausnahme von **„USA, Osten“, „USA, Westen“, „Europa, Westen“, „Azure Government“, „Azure China 21Vianet“** und **„Azure Deutschland“**.

Führen Sie zum Erstellen eines IoT-Hubs in der Ressourcengruppe *ContosoRental* die Schritte unter [Erstellen eines IoT-Hubs](https://docs.microsoft.com/azure/iot-hub/quickstart-send-telemetry-dotnet#create-an-iot-hub) aus.

## <a name="register-a-device-in-iot-hub"></a>Registrieren eines Geräts in einem IoT-Hub

Geräte können nur dann eine Verbindung mit dem IoT-Hub herstellen, wenn sie in der IoT-Hub-Identitätsregistrierung registriert sind. In unserem Szenario erstellen wir ein einzelnes Gerät mit dem Namen *InVehicleDevice*. Um das Gerät im IoT-Hub zu erstellen und zu registrieren, führen Sie die Schritte in [Registrieren eines neuen Geräts beim IoT-Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-create-through-portal#register-a-new-device-in-the-iot-hub) aus. Stellen Sie sicher, dass Sie die **Primäre Verbindungszeichenfolge** Ihres Geräts kopieren, da wir sie in einem späteren Schritt verwenden werden.

## <a name="create-an-azure-function-and-add-an-event-grid-subscription"></a>Erstellen einer Azure-Funktion und Hinzufügen eines Event Grid-Abonnements

Azure Functions ist ein serverloser Computedienst, mit dem Sie kleine Codeabschnitte („Funktionen“) ausführen können, ohne explizit eine Computeinfrastruktur bereitstellen oder verwalten zu müssen. Weitere Informationen zu Azure Functions finden Sie in der Dokumentation zu [Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-overview).

Eine Funktion wird durch ein bestimmtes Ereignis „ausgelöst“. In unserem Szenario erstellen wir eine Funktion, die durch einen Event Grid-Trigger ausgelöst wird. Wir erstellen die Beziehung zwischen Trigger und Funktion, indem wir ein Ereignisabonnement für IoT-Hub-Gerätetelemetrieereignisse erstellen. Wenn ein Gerätetelemetrieereignis auftritt, wird unsere Funktion als Endpunkt aufgerufen und empfängt die relevanten Daten für das [Gerät, das wir zuvor im IoT-Hub registriert haben](#register-a-device-in-iot-hub).

Der C#-Skriptcode, den die Funktion enthält, ist [hier](https://github.com/Azure-Samples/iothub-to-azure-maps-geofencing/blob/master/src/Azure%20Function/run.csx) zu sehen.

Jetzt richten wir unsere Azure-Funktion ein.

1. Klicken Sie im Dashboard des Azure-Portals auf **Ressource erstellen**. Geben Sie im Suchtextfeld **Funktionen-App** ein. Klicken Sie auf **Funktionen-App**. Klicken Sie auf **Erstellen**.

2. Geben Sie Ihrer Funktions-App auf der Erstellungsseite **Funktions-App** einen Namen. Wählen Sie unter **Ressourcengruppe** die Option *ContosoRental* in der Dropdownliste aus.  Wählen Sie *.NET Core* als **Runtimestapel** aus. Klicken Sie auf **Weiter: Hosting** im unteren Bereich der Seite.

    :::image type="content" source="./media/tutorial-iot-hub-maps/rental-app.png" alt-text="Erstellen einer Funktions-App":::

3. Wählen Sie als **Speicherkonto** das Speicherkonto aus, das Sie unter [Erstellen eines Azure-Speicherkontos](#create-an-azure-storage-account) erstellt haben. Klicken Sie auf **Überprüfen + erstellen**.

4. Überprüfen Sie die Details der Funktions-App, und klicken Sie auf **Erstellen**.

5. Nach der Erstellung der App müssen wir ihr eine Funktion hinzufügen. Navigieren Sie zur Funktions-App. Klicken Sie auf das Blatt **Funktionen**. Klicken Sie am oberen Seitenrand auf **+ Hinzufügen**. Der Funktionsvorlagenbereich wird angezeigt. Scrollen Sie im Bereich nach unten. Klicken Sie auf **Azure Event Grid-Trigger**.

     >[!WARNING]
    > Die Vorlagen **Azure Event Hub-Trigger** und **Azure Event Grid-Trigger** haben ähnliche Namen. Stellen Sie sicher, dass Sie die Vorlage **Azure Event Grid-Trigger** auswählen.

    :::image type="content" source="./media/tutorial-iot-hub-maps/function-create.png" alt-text="Erstellen einer Funktion":::

6. Geben Sie der Funktion einen Namen. In diesem Tutorial verwenden wir den Namen *GetGeoFunction*, aber Sie können einen beliebigen Namen verwenden. Klicken Sie auf **Funktion erstellen**.

7. Klicken Sie im Menü auf der linken Seite auf das Blatt **Code + Test**. Kopieren Sie das [C#-Skript](https://github.com/Azure-Samples/iothub-to-azure-maps-geofencing/blob/master/src/Azure%20Function/run.csx), und fügen Sie es in das Codefenster ein.

     :::image type="content" source="./media/tutorial-iot-hub-maps/function-code.png" alt-text="Kopieren/Einfügen von Code in das Funktionsfenster":::

8. Ersetzen Sie im C#-Code die folgenden Parameter. Klicken Sie auf **Speichern**. Klicken Sie noch nicht auf **Testen/Ausführen**.
    * Ersetzen Sie **SUBSCRIPTION_KEY** durch den primären Abonnementschlüssel Ihres Azure Maps-Kontos.
    * Ersetzen Sie **UDID** durch die `udid` des in [Hochladen eines Geofence](#upload-a-geofence) hochgeladenen Geofencebereichs.
    * Mit der Funktion **CreateBlobAsync** im Skript wird im Datenspeicherkonto ein Blob pro Ereignis erstellt. Ersetzen Sie **ACCESS_KEY**, **ACCOUNT_NAME** und **STORAGE_CONTAINER_NAME** durch den Zugriffsschlüssel, den Kontonamen und den Datenspeichercontainer Ihres Speicherkontos. Diese Werte wurden generiert, als Sie in [Erstellen eines Azure-Speicherkontos](#create-an-azure-storage-account) ein Speicherkonto erstellt haben.

9. Klicken Sie im Menü auf der linken Seite auf das Blatt **Integration**. Klicken Sie im Diagramm auf **Event Grid-Trigger**. Geben Sie einen Namen für den Trigger ein, z. B. *eventCarTelemetry*, und klicken Sie auf **Event Grid-Abonnement erstellen**.

     :::image type="content" source="./media/tutorial-iot-hub-maps/function-integration.png" alt-text="Hinzufügen eines Ereignisabonnements":::

10. Tragen Sie die Abonnementdetails ein. Benennen Sie das Ereignisabonnement. Wählen Sie für *Ereignisschema* die Option *Event Grid-Schema* aus. Wählen Sie für **Thementypen** die Option *Azure IoT Hub-Konten* aus. Wählen Sie für **Ressourcengruppe** die Ressourcengruppe aus, die Sie zu Beginn dieses Tutorials erstellt haben. Wählen Sie für **Ressource** den IoT-Hub aus, den Sie in [Erstellen einer Azure IoT Hub-Instanz](#create-an-azure-iot-hub) erstellt haben. Wählen Sie für **Nach Ereignistypen filtern** die Option *Gerätetelemetrie* aus. Nachdem Sie diese Optionen ausgewählt haben, sehen Sie, dass der **Thementyp** automatisch in *IoT Hub* geändert wurde. Für **Name des Systemthemas** können Sie denselben Namen wie für Ihre Ressource verwenden.  Klicken Sie abschließend auf **Endpunkt auswählen** im Abschnitt **Endpunktdetails**. Akzeptieren Sie alle Einstellungen, und klicken Sie auf **Auswahl bestätigen**.

    :::image type="content" source="./media/tutorial-iot-hub-maps/function-create-event-subscription.png" alt-text="Erstellen eines Ereignisabonnements":::

11. Überprüfen Sie Ihre Einstellungen. Stellen Sie sicher, dass der Endpunkt die Funktion angibt, die Sie am Anfang dieses Abschnitts erstellt haben. Klicken Sie auf **Erstellen**.

    :::image type="content" source="./media/tutorial-iot-hub-maps/function-create-event-subscription-confirm.png" alt-text="Bestätigung des Erstellens eines Ereignisabonnements":::

12. Nun befinden Sie sich wieder im Fenster **Trigger bearbeiten**. Klicken Sie auf **Speichern**.

## <a name="filter-events-using-iot-hub-message-routing"></a>Filtern von Ereignissen per IoT Hub-Nachrichtenrouting

Wenn Sie der Azure-Funktion ein Event Grid-Abonnement hinzufügen, wird automatisch eine Nachrichtenroute im angegebenen IoT-Hub erstellt. Das Nachrichtenrouting ermöglicht Ihnen, verschiedene Datentypen an verschiedene Endpunkte weiterzuleiten. Dadurch können Sie beispielsweise Gerätetelemetrienachrichten, Gerätelebenszyklusereignisse und Änderungsereignisse für Gerätezwillinge weiterleiten. Weitere Informationen zum IoT Hub-Nachrichtenrouting finden Sie unter [Verwenden des IoT Hub-Nachrichtenroutings zum Senden von D2C-Nachrichten an verschiedene Endpunkte](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-d2c).

:::image type="content" source="./media/tutorial-iot-hub-maps/hub-route.png" alt-text="Nachrichtenrouting im IoT-Hub":::

In unserem Beispielszenario möchten wir nur dann Nachrichten empfangen, wenn das Mietfahrzeug bewegt wird. Wir erstellen eine Routingabfrage, um die Ereignisse zu filtern, bei denen die `Engine`-Eigenschaft gleich **ON** ist. Klicken Sie zum Erstellen einer Routingabfrage auf die Route **RouteToEventGrid**, und ersetzen Sie die **Routingabfrage** durch **„Engine='ON'“**. Klicken Sie anschließend auf **Speichern**. IoT Hub veröffentlicht jetzt nur Gerätetelemetriedaten, wenn „Engine“ auf „ON“ festgelegt ist.

:::image type="content" source="./media/tutorial-iot-hub-maps/hub-filter.png" alt-text="Filtern von Routingnachrichten":::

>[!TIP]
>Es gibt verschiedene Möglichkeiten zum Abfragen von IoT-Gerät-zu-Cloud-Nachrichten. Weitere Informationen zur Syntax für das Nachrichtenrouting finden Sie unter [Abfragesyntax für das IoT Hub-Nachrichtenrouting](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-routing-query-syntax).

## <a name="send-telemetry-data-to-iot-hub"></a>Senden von Telemetriedaten an IoT Hub

Sobald unsere Azure-Funktion einsatzbereit ist, können wir Telemetriedaten an die IoT Hub-Instanz senden, von wo aus sie dann an Event Grid weitergeleitet werden. Wir verwenden hier eine C#-Anwendung, um Standortdaten für ein Gerät in einem Mietwagen zu simulieren. Zum Ausführen der Anwendung benötigen Sie auf Ihrem Entwicklungscomputer .NET Core SDK 2.1.0 oder höher. Führen Sie die folgenden Schritte aus, um simulierte Telemetriedaten an IoT Hub zu senden.

1. Wenn Sie dies noch nicht getan haben, laden Sie das C#-Projekt [rentalCarSimulation](https://github.com/Azure-Samples/iothub-to-azure-maps-geofencing/tree/master/src/rentalCarSimulation) herunter.

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

:::image type="content" source="./media/tutorial-iot-hub-maps/terminal.png" alt-text="Terminalausgabe":::

Wenn Sie den Blobspeichercontainer jetzt öffnen, können Sie vier Blobs für Standorte sehen, an denen sich das Fahrzeug außerhalb des Geofencebereichs befunden hat.

:::image type="content" source="./media/tutorial-iot-hub-maps/blob.png" alt-text="Anzeigen von Blobs in einem Container":::

Die folgende Karte zeigt vier Fahrzeugpositionspunkte außerhalb des Geofence. Jede Position wurde in regelmäßigen Zeitabständen protokolliert.

:::image type="content" source="./media/tutorial-iot-hub-maps/violation-map.png" alt-text="Karte der Verstöße":::

## <a name="explore-azure-maps-and-iot"></a>Erkunden von Azure Maps und IoT

Informationen zu den in diesem Tutorial verwendeten Azure Maps-APIs finden Sie unter:

* [Get Search Address Reverse (Suchadresse invers abrufen)](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse)
* [Get Geofence](https://docs.microsoft.com/rest/api/maps/spatial/getgeofence)

Eine vollständige Liste mit Azure Maps-REST-APIs finden Sie unter:

* [Azure Maps-REST-APIs](https://docs.microsoft.com/rest/api/maps/spatial/getgeofence)

Weitere Informationen zu IoT Plug & Play finden Sie in dem folgenden Artikel:

* [Dokumentation zu IoT Plug & Play (Vorschauversion)](https://docs.microsoft.com/azure/iot-pnp)

Eine Liste mit Geräten, die über eine Azure-Zertifizierung für IoT verfügen, finden Sie unter:

* [Geräte mit Azure-Zertifizierung](https://catalog.azureiotsolutions.com/)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Senden von Telemetriedaten von einem Gerät an die Cloud (und umgekehrt) finden Sie unter:

> [!div class="nextstepaction"]
> [Senden von Telemetriedaten von einem Gerät](https://docs.microsoft.com/azure/iot-hub/quickstart-send-telemetry-dotnet)
