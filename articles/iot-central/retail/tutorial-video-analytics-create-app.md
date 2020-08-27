---
title: 'Tutorial: Erstellen einer Anwendung vom Typ „Videoanalyse: Objekt- und Bewegungserkennung“ in Azure IoT Central'
description: In diesem Tutorial wird veranschaulicht, wie Sie in IoT Central eine Anwendung für die Videoanalyse erstellen. Sie erstellen die Anwendung, passen sie an und stellen dafür die Verbindung mit anderen Azure-Diensten her.
services: iot-central
ms.service: iot-central
ms.subservice: iot-central-retail
ms.topic: tutorial
author: KishorIoT
ms.author: nandab
ms.date: 07/31/2020
ms.openlocfilehash: b98406984f2c9f2adfca030369a6ea3d47a786f3
ms.sourcegitcommit: e2b36c60a53904ecf3b99b3f1d36be00fbde24fb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/24/2020
ms.locfileid: "88762776"
---
# <a name="tutorial-create-a-video-analytics---object-and-motion-detection-application-in-azure-iot-central"></a>Tutorial: Erstellen einer Anwendung vom Typ „Videoanalyse: Objekt- und Bewegungserkennung“ in Azure IoT Central

Informieren Sie sich als Lösungsentwickler darüber, wie Sie mit der IoT Central-Anwendungsvorlage vom Typ *Videoanalyse: Objekt- und Bewegungserkennung*, Azure IoT Edge-Geräten und Azure Media Services eine Anwendung für die Videoanalyse erstellen. Im Rahmen der Lösung wird anhand eines Einzelhandelsgeschäfts (Filiale) veranschaulicht, wie die häufige geschäftliche Anforderung zur Überwachung von Sicherheitskameras erfüllt wird. Für die Lösung wird die automatische Objekterkennung in einem Videofeed genutzt, um interessante Ereignisse schnell identifizieren und den entsprechenden Ort ermitteln zu können.

Die Beispielanwendung umfasst zwei simulierte Geräte und ein IoT Edge-Gateway. Die folgenden Tutorials veranschaulichen zwei Ansätze zum Experimentieren mit den Funktionen des Gateways und zum besseren Verständnis:

* Erstellen Sie das IoT Edge-Gateway auf einer Azure-VM, und schließen Sie eine simulierte Kamera an.
* Erstellen Sie das IoT Edge-Gateway auf einem echten Gerät, z. B. Intel NUC, und schließen Sie eine echte Kamera an.

In diesem Tutorial lernen Sie, wie die folgenden Aufgaben ausgeführt werden:
> [!div class="checklist"]
> * Verwenden der Anwendungsvorlage für die Azure IoT Central-Videoanalyse zum Erstellen einer Filialenanwendung
> * Anpassen der Anwendungseinstellungen
> * Erstellen einer Gerätevorlage für ein IoT Edge-Gatewaygerät
> * Hinzufügen eines Gatewaygeräts zu Ihrer IoT Central-Anwendung

## <a name="prerequisites"></a>Voraussetzungen

Zum Abschließen dieses Tutorials benötigen Sie Folgendes:

* Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie auf der Seite [Azure-Anmeldeseite](https://aka.ms/createazuresubscription) eines erstellen.
* Bei Verwendung einer echten Kamera benötigen Sie Konnektivität zwischen dem IoT Edge-Gerät und der Kamera sowie den **Real-Time Streaming Protocol**-Kanal.

## <a name="initial-setup"></a>Erste Einrichtung

In diesen Tutorials aktualisieren und verwenden Sie mehrere Konfigurationsdateien. Die Anfangsversionen dieser Dateien sind im GitHub-Repository [LVA-gateway](https://github.com/Azure/live-video-analytics/tree/master/ref-apps/lva-edge-iot-central-gateway) verfügbar. Das Repository enthält auch eine Scratchpad-Textdatei, die Sie herunterladen und verwenden können, um Konfigurationswerte für die von Ihnen bereitgestellten Dienste aufzuzeichnen.

Erstellen Sie auf Ihrem lokalen Computer einen Ordner mit dem Namen *lva-configuration*, um darin Kopien dieser Dateien zu speichern. Klicken Sie anschließend mit der rechten Maustaste auf die folgenden Links, und wählen Sie **Speichern unter** aus, um die Datei im Ordner *lva-configuration* zu speichern:

- [scratchpad.txt](https://raw.githubusercontent.com/Azure/live-video-analytics/master/ref-apps/lva-edge-iot-central-gateway/setup/Scratchpad.txt)
- [deployment.amd64.json](https://raw.githubusercontent.com/Azure/live-video-analytics/master/ref-apps/lva-edge-iot-central-gateway/setup/deployment.amd64.json)
- [LvaEdgeGatewayDcm.json](https://raw.githubusercontent.com/Azure/live-video-analytics/master/ref-apps/lva-edge-iot-central-gateway/setup/LvaEdgeGatewayDcm.json)
- [state.json](https://raw.githubusercontent.com/Azure/live-video-analytics/master/ref-apps/lva-edge-iot-central-gateway/setup/state.json)

> [!NOTE]
> Das GitHub-Repository enthält auch den Quellcode für die IoT Edge-Module **LvaEdgeGatewayModule** und **lvaYolov3**. Weitere Informationen zur Verwendung des Quellcodes finden Sie unter [Ändern und Erstellen der Gatewaymodule für die Livevideoanalyse](tutorial-video-analytics-build-module.md).

## <a name="deploy-and-configure-azure-media-services"></a>Bereitstellen und Konfigurieren von Azure Media Services

Für die Lösung wird ein Azure Media Services-Konto verwendet, um die Videoclips zur Objekterkennung zu speichern, die vom IoT Edge-Gatewaygerät aufgezeichnet werden.

Bei der Erstellung des Media Services-Kontos:

- Sie müssen einen Kontonamen, ein Azure-Abonnement, einen Standort, eine Ressourcengruppe und ein Speicherkonto angeben. Erstellen Sie ein neues Speicherkonto mit den Standardeinstellungen, während Sie das Media Services-Konto erstellen.

- Wählen Sie als Standort die Region **USA, Osten** aus.

- Erstellen Sie für das Media Services- und das Speicherkonto eine neue Ressourcengruppe mit dem Namen *lva-rg* in der Region **USA, Osten**. Nachdem Sie die Tutorials durchgearbeitet haben, können Sie alle Ressourcen leicht entfernen, indem Sie die Ressourcengruppe *lva-rg* löschen.

Erstellen Sie das [Media Services-Konto im Azure-Portal](https://portal.azure.com/?r=1#create/Microsoft.MediaService).

> [!TIP]
> Verwenden Sie in diesen Tutorials für alle Beispiele die Region **USA, Osten**. Sie können Ihre nächstgelegene Region verwenden, falls Sie dies möchten.

Notieren Sie sich den Namen Ihres **Media Services**-Kontos in der Datei *scratchpad.txt*.

Navigieren Sie nach Abschluss der Bereitstellung zur Seite **Eigenschaften** Ihres **Media Services**-Kontos. Notieren Sie sich die **Ressourcen-ID** in der Datei *scratchpad.txt*. Sie benötigen diesen Wert später beim Konfigurieren des IoT Edge-Moduls.

Konfigurieren Sie als Nächstes einen Azure Active Directory-Dienstprinzipal für Ihre Media Services-Ressource. Wählen Sie die Option **API-Zugriff** und dann **Dienstprinzipalauthentifizierung** aus. Erstellen Sie eine neue Azure Active Directory-App, die den gleichen Namen wie Ihre Media Services-Ressource hat, und erstellen Sie dann ein Geheimnis mit der Beschreibung *IoT Edge-Zugriff*.

:::image type="content" source="./media/tutorial-video-analytics-create-app/media-service-authentication.png" alt-text="Konfigurieren der AAD-App für AMS":::

Scrollen Sie nach der Generierung des Geheimnisses nach unten zum Abschnitt **Kopieren Sie die Anmeldeinformationen, um eine Verbindung mit Ihrer Dienstprinzipalanwendung herzustellen**. Wählen Sie anschließend **JSON** aus. Sie können alle Anmeldeinformationen auf einmal kopieren. Notieren Sie sich diese Informationen in der Datei *scratchpad.txt*. Sie benötigen sie später beim Konfigurieren des IoT Edge-Geräts.

> [!WARNING]
> Dies ist die einzige Möglichkeit, das Geheimnis anzuzeigen und zu speichern. Wenn Sie es verlieren, müssen Sie ein neues Geheimnis generieren.

## <a name="create-the-azure-iot-central-application"></a>Erstellen der Azure IoT Central-Anwendung

In diesem Abschnitt erstellen Sie eine neue Azure IoT Central-Anwendung aus einer Vorlage. Sie verwenden diese Anwendung in der gesamten Tutorialreihe, um eine umfassende Lösung zu erstellen.

So erstellen Sie eine neue Azure IoT Central-Anwendung:

1. Navigieren Sie zur Website des [Azure IoT Central-Anwendungs-Managers](https://aka.ms/iotcentral).

1. Melden Sie sich mit den Anmeldeinformationen an, die Sie zum Zugreifen auf Ihr Azure-Abonnement verwenden.

1. Wählen Sie auf der Seite **Erstellen** die Option **Neue Anwendung** aus, um mit der Erstellung einer neuen Azure IoT Central-Anwendung zu beginnen.

1. Wählen Sie **Einzelhandel** aus. Auf der Seite „Einzelhandel“ werden verschiedene Vorlagen für Einzelhandelsanwendungen angezeigt.

Erstellen Sie wie folgt eine neue Videoanalyseanwendung:

1. Wählen Sie die Anwendungsvorlage **Videoanalyse: Objekt- und Bewegungserkennung** aus. Diese Vorlage enthält Gerätevorlagen für die im Tutorial verwendeten Geräte. Die Vorlage enthält Beispieldashboards, mit den Operatoren Aufgaben wie Überwachen und Verwalten von Kameras ausführen können.

1. Wählen Sie optional einen benutzerfreundlichen **Anzeigenamen** aus. Diese Anwendung basiert auf einem fiktiven Einzelhandelsgeschäft mit dem Namen „Northwind Traders“. In diesem Tutorial wird *Northwind Traders video analytics* (Northwind Traders-Videoanalyse) als **Anwendungsname** verwendet.

    > [!NOTE]
    > Wenn Sie einen benutzerfreundlichen **Anwendungsnamen** verwenden, müssen Sie trotzdem einen eindeutigen Wert für die **URL** der Anwendung angeben.

1. Wählen Sie bei Verwendung eines Azure-Abonnements Ihr **Verzeichnis**, **Azure-Abonnement** und als **Standort** die Option **USA** aus. Wenn Sie kein Abonnement haben, können Sie eine **7-tägige kostenlose Testversion** aktivieren und die erforderlichen Kontaktinformationen eingeben. In diesem Tutorial werden drei Geräte verwendet: zwei Kameras und ein IoT Edge-Gerät. Wenn Sie nicht die kostenlose Testversion nutzen, werden Ihnen also Kosten für die Nutzung berechnet.

    Weitere Informationen zu Verzeichnissen, Abonnements und Standorten finden Sie in der Schnellstartanleitung unter [Erstellen einer Azure IoT Central-Anwendung](../core/quick-deploy-iot-central.md).

1. Klicken Sie auf **Erstellen**.

    :::image type="content" source="./media/tutorial-video-analytics-create-app/new-application.png" alt-text="Azure IoT Central-Seite „Anwendung erstellen“":::

### <a name="retrieve-the-configuration-data"></a>Abrufen der Konfigurationsdaten

Später in diesem Tutorial beim Konfigurieren des IoT Edge-Gateways benötigen Sie einige Konfigurationsdaten aus der IoT Central-Anwendung. Das IoT Edge-Gerät benötigt diese Informationen, um die Registrierung bei der Anwendung und die zugehörige Verbindungsherstellung durchzuführen.

Wählen Sie im Abschnitt **Verwaltung** die Option **Ihre Anwendung** aus, und notieren Sie sich die **Anwendungs-URL** und die **Anwendungs-ID** in der Datei *scratchpad.txt*:

:::image type="content" source="./media/tutorial-video-analytics-create-app/administration.png" alt-text="Verwaltung":::

Wählen Sie **API-Token** aus, und generieren Sie ein neues Token mit dem Namen **LVAEdgeToken** für die Rolle **Operator**:

:::image type="content" source="./media/tutorial-video-analytics-create-app/token.png" alt-text="Token generieren":::

Notieren Sie sich das Token in der Datei *scratchpad.txt* zur späteren Verwendung. Nachdem das Dialogfeld geschlossen wurde, können Sie das Token nicht noch einmal anzeigen.

Wählen Sie im Abschnitt **Verwaltung** die Option **Geräteverbindung** und dann **SAS-IoT-Devices** aus.

Notieren Sie sich den **Primärschlüssel** für Geräte in der Datei *scratchpad.txt*. Sie verwenden dieses *SAS-Token (Shared Access Signature) für die primäre Gruppe* später beim Konfigurieren des IoT Edge-Geräts.

## <a name="edit-the-deployment-manifest"></a>Bearbeiten des Bereitstellungsmanifests

Sie stellen ein IoT Edge-Modul bereit, indem Sie ein Bereitstellungsmanifest verwenden. In IoT Central können Sie das Manifest als Gerätevorlage importieren und die Verwaltung der Modulbereitstellung dann IoT Central überlassen.

Bereiten Sie das Bereitstellungsmanifest wie folgt vor:

1. Öffnen Sie die Datei *deployment.amd64.json*, die Sie im Ordner *lva-configuration* gespeichert haben, in einem Text-Editor.

1. Suchen Sie nach den Einstellungen für `LvaEdgeGatewayModule`, und ändern Sie den Bildnamen wie im folgenden Codeausschnitt:

    ```json
    "LvaEdgeGatewayModule": {
        "settings": {
            "image": "mcr.microsoft.com/lva-utilities/lva-edge-iotc-gateway:1.0-amd64",
    ```

1. Fügen Sie den Namen Ihres Media Services-Kontos im Abschnitt `LvaEdgeGatewayModule` dem Knoten `env` hinzu. Sie haben sich diesen Kontonamen in der Datei *scratchpad.txt* notiert:

    ```json
    "env": {
        "lvaEdgeModuleId": {
            "value": "lvaEdge"
        },
        "amsAccountName": {
            "value": "<YOUR_AZURE_MEDIA_ACCOUNT_NAME>"
        }
    }
    ```

1. Über die Vorlage werden diese Eigenschaften nicht in IoT Central verfügbar gemacht. Daher müssen Sie die Media Services-Konfigurationswerte dem Bereitstellungsmanifest hinzufügen. Suchen Sie nach dem Modul `lvaEdge`, und ersetzen Sie die Platzhalter durch die Werte, die Sie sich beim Erstellen Ihres Media Services-Kontos in der Datei *scratchpad.txt* notiert haben.

    `azureMediaServicesArmId` ist die **Ressourcen-ID**, die Sie sich beim Erstellen des Media Services-Kontos in der Datei *scratchpad.txt* notiert haben.

    Sie haben sich die Angaben für `aadTenantId`, `aadServicePrincipalAppId` und `aadServicePrincipalSecret` in der Datei *scratchpad.txt* notiert, als Sie den Dienstprinzipal für Ihr Media Services-Konto erstellt haben:

    ```json
    {
        "lvaEdge":{
        "properties.desired": {
            "applicationDataDirectory": "/var/lib/azuremediaservices",
            "azureMediaServicesArmId": "[Resource ID from scratchpad]",
            "aadTenantId": "[AADTenantID from scratchpad]",
            "aadServicePrincipalAppId": "[AadClientId from scratchpad]",
            "aadServicePrincipalSecret": "[AadSecret from scratchpad]",
            "aadEndpoint": "https://login.microsoftonline.com",
            "aadResourceId": "https://management.core.windows.net/",
            "armEndpoint": "https://management.azure.com/",
            "diagnosticsEventsOutputName": "AmsDiagnostics",
            "operationalMetricsOutputName": "AmsOperational",
            "logCategories": "Application,Event",
            "AllowUnsecuredEndpoints": "true",
            "TelemetryOptOut": false
            }
        }
    }
    ```

1. Speichern Sie die Änderungen.

Optional können Sie das Yolov3-Modul durch das Modell mit Hardwareoptimierung von Intel unter [OpenVINO&trade; Model Server – Edge AI Extension Module](https://github.com/openvinotoolkit/model_server/tree/ams/extras/ams_wrapper) ersetzen. Sie können ein Beispiel für ein Bereitstellungsmanifest herunterladen: [deployment.openvino.amd64.json](https://raw.githubusercontent.com/Azure/live-video-analytics/master/ref-apps/lva-edge-iot-central-gateway/setup/deployment.openvino.amd64.json). Dieses Manifest ersetzt die Konfigurationseinstellungen für das `lvaYolov3`-Modul durch die folgende Konfiguration:

```json
"OpenVINOModelServerEdgeAIExtensionModule": {
    "settings": {
        "image": "marketplace.azurecr.io/intel_corporation/open_vino",
        "createOptions": "{\"HostConfig\":{\"PortBindings\":{\"4000/tcp\":[{\"HostPort\":\"4000\"}]}},\"Cmd\":[\"/ams_wrapper/start_ams.py\",\"--ams_port=4000\",\"--ovms_port=9000\"]}"
    },
    "type": "docker",
    "version": "1.0",
    "status": "running",
    "restartPolicy": "always"
}
```

## <a name="create-the-azure-iot-edge-gateway-device"></a>Erstellen des Azure IoT Edge-Gatewaygeräts

Die Anwendung „Videoanalyse: Objekt- und Bewegungserkennung“ enthält die Gerätevorlagen **LVA Edge Object Detector** (LVA-Edgeobjekterkennung) und **LVA Edge Motion Detection** (LVA-Edgebewegungserkennung). In diesem Abschnitt erstellen Sie eine Vorlage für ein Gatewaygerät, indem Sie das Bereitstellungsmanifest verwenden, und fügen das Gatewaygerät Ihrer IoT Central-Anwendung hinzu.

### <a name="create-a-device-template-for-the-lva-edge-gateway"></a>Erstellen einer Gerätevorlage für das LVA-Edgegateway

Gehen Sie wie folgt vor, um das Bereitstellungsmanifest zu importieren und die Gerätevorlage vom Typ **LVA Edge Gateway** (LVA-Edgegateway) zu erstellen:

1. Navigieren Sie in Ihrer IoT Central-Anwendung zu **Gerätevorlagen**, und wählen Sie **+ Neu** aus.

1. Wählen Sie auf der Seite **Vorlagentyp auswählen** die Kachel **Azure IoT Edge** aus. Wählen Sie anschließend **Next: Anpassen**.

1. Geben Sie auf der Seite **Azure IoT Edge-Bereitstellungsmanifest hochladen** den Namen *LVA-Edgegateway* als Vorlagennamen ein, und aktivieren Sie die Option **Gatewaygerät mit nachgeschalteten Geräten**.

    Suchen Sie noch nicht nach dem Bereitstellungsmanifest. Wenn Sie die Suche doch durchführen, erwartet der Bereitstellungs-Assistent eine Schnittstelle für jedes Modul. Sie müssen aber nur die Schnittstelle für **LvaEdgeGatewayModule** verfügbar machen. Sie laden das Manifest in einem späteren Schritt hoch.

    :::image type="content" source="./media/tutorial-video-analytics-create-app/upload-deployment-manifest.png" alt-text="Bereitstellungsmanifest nicht hochladen":::

    Klicken Sie auf **Weiter: Review** (Weiter: Überprüfen).

1. Wählen Sie auf der Seite **Überprüfen** die Option **Erstellen** aus.

### <a name="import-the-device-capability-model"></a>Importieren des Gerätefunktionsmodells

Die Gerätevorlage muss ein Gerätefunktionsmodell enthalten. Wählen Sie auf der Seite **LVA-Edgegateway** die Kachel **Funktionsmodell importieren** aus. Navigieren Sie zum zuvor erstellten Ordner *lva-configuration*, und wählen Sie die Datei *LvaEdgeGatewayDcm.json* aus.

Die Gerätevorlage **LVA-Edgegateway** enthält jetzt das **LVA-Edgegatewaymodul** und drei Schnittstellen: **Device information** (Geräteinformationen), **LVA Edge Gateway Settings** (LVA-Edgegatewayeinstellungen) und **LVA Edge Gateway Interface** (LVA-Edgegatewayschnittstelle).

### <a name="replace-the-manifest"></a>Ersetzen des Manifests

Wählen Sie auf der Seite **LVA-Edgegateway** die Option **+ Manifest ersetzen** aus.

:::image type="content" source="./media/tutorial-video-analytics-create-app/replace-manifest.png" alt-text="Manifest ersetzen":::

Navigieren Sie zum Ordner *lva-configuration*, und wählen Sie die Manifestdatei *deployment.amd64.json* aus, die Sie zuvor bearbeitet haben. Wählen Sie die Option **Hochladen**. Wählen Sie nach Abschluss der Validierung die Option **Ersetzen** aus.

### <a name="add-relationships"></a>Hinzufügen von Beziehungen

Wählen Sie in der Gerätevorlage **LVA-Edgegateway** unter **Modules/LVA Edge Gateway Module** (Module/LVA-Edgegatewaymodul) die Option **Beziehungen** aus. Wählen Sie **+ Beziehung hinzufügen** aus, und fügen Sie die beiden folgenden Beziehungen hinzu:

|Anzeigename               |Name          |Ziel |
|-------------------------- |------------- |------ |
|LVA Edge Motion Detector (LVA-Edgebewegungserkennung)   |Verwenden Sie den Standardwert.   |LVA Edge Motion Detector Device (Gerät für LVA-Edgebewegungserkennung) |
|LVA Edge Object Detector (LVA-Edgeobjekterkennung)   |Verwenden Sie den Standardwert.   |LVA Edge Object Detector Device (Gerät für LVA-Edgeobjekterkennung) |

Klicken Sie dann auf **Speichern**.

:::image type="content" source="media/tutorial-video-analytics-create-app/relationships.png" alt-text="Hinzufügen von Beziehungen":::

### <a name="add-views"></a>Hinzufügen von Ansichten

Die Gerätevorlage **LVA-Edgegateway** enthält keine Ansichtsdefinitionen.

So fügen Sie der Gerätevorlage eine Ansicht hinzu:

1. Navigieren Sie in der Gerätevorlage **LVA-Edgegateway** zu **Ansichten**, und wählen Sie die Kachel **Das Gerät wird visualisiert** aus.

1. Geben Sie *LVA Edge Gateway device* (Gerät für LVA-Edgegateway) als Namen für die Ansicht ein.

1. Fügen Sie der Ansicht die folgenden Kacheln hinzu:

    * Eine Kachel mit den Eigenschaften für **Geräteinformationen**: **Gerätemodell**, **Hersteller**, **Betriebssystem**, **Prozessorarchitektur**, **Softwareversion**, **Gesamter Arbeitsspeicher** und **Speicher gesamt**.
    * Eine Liniendiagramm-Kachel mit den Telemetriewerten für **Freier Arbeitsspeicher** und **System Heartbeat** (Systemheartbeat).
    * Eine Kachel für den Ereignisverlauf mit den folgenden Ereignissen: **Kamera erstellen**, **Kamera löschen**, **Modulneustart**, **Modul gestartet**, **Modul angehalten**.
    * Eine 2x1-Kachel mit dem letzten bekannten Wert, auf der die Telemetriedaten für den **IoT Central-Clientzustand** angezeigt werden.
    * Eine 2x1-Kachel mit dem letzten bekannten Wert, auf der die Telemetriedaten für den **Modulzustand** angezeigt werden.
    * Eine 1x1-Kachel mit dem letzten bekannten Wert, auf der die Telemetriedaten für den **System Heartbeat** (Systemheartbeat) angezeigt werden.
    * Eine 1x1-Kachel mit dem letzten bekannten Wert, auf der die Telemetriedaten für **Connected Cameras** (Angeschlossene Kameras) angezeigt werden.

    :::image type="content" source="media/tutorial-video-analytics-create-app/gateway-dashboard.png" alt-text="Dashboard":::

1. Wählen Sie **Speichern** aus.

### <a name="publish-the-device-template"></a>Veröffentlichen der Gerätevorlage

Bevor Sie der Anwendung ein Gerät hinzufügen können, müssen Sie die Gerätevorlage veröffentlichen:

1. Wählen Sie in der Gerätevorlage **LVA-Edgegateway** die Option **Veröffentlichen** aus.

1. Wählen Sie auf der Seite **Diese Gerätevorlage für die Anwendung veröffentlichen** die Option **Veröffentlichen** aus.

**LVA-Edgegateway** ist jetzt als Gerätetyp verfügbar, der in der Anwendung auf der Seite **Geräte** verwendet werden kann.

## <a name="add-a-gateway-device"></a>Hinzufügen eines Gatewaygeräts

Fügen Sie der Anwendung wie folgt ein Gerät vom Typ **LVA-Edgegateway** hinzu:

1. Navigieren Sie zur Seite **Geräte**, und wählen Sie die Gerätevorlage **LVA-Edgegateway** aus.

1. Wählen Sie **+ Neu** aus.

1. Ändern Sie im Dialogfeld **Neues Gerät erstellen** den Gerätenamen in *LVA-Gateway 001* und die Geräte-ID in *lva-gateway-001*.

    > [!NOTE]
    > Die Geräte-ID muss in der Anwendung eindeutig sein.

1. Klicken Sie auf **Erstellen**.

Der Gerätestatus lautet **Registriert**.

### <a name="get-the-device-credentials"></a>Abrufen der Geräteanmeldeinformationen

Sie benötigen die Anmeldeinformationen, mit denen das Gerät eine Verbindung mit Ihrer IoT Central-Anwendung herstellen kann. So rufen Sie die Anmeldeinformationen ab:

1. Wählen Sie auf der Seite **Geräte** das von Ihnen erstellte Gerät **lva-gateway-001** aus.

1. Wählen Sie **Verbinden**.

1. Notieren Sie sich auf der Seite **Geräteverbindung** in der Datei *scratchpad.txt* den **ID-Bereich**, die **Geräte-ID** und den **Primärschlüssel** des Geräts. Diese Werte werden später benötigt.

1. Stellen Sie sicher, dass die Verbindungsmethode auf **Shared Access Signature** festgelegt ist.

1. Klicken Sie auf **Schließen**.

## <a name="next-steps"></a>Nächste Schritte

Sie haben jetzt mit der Anwendungsvorlage **Videoanalyse: Objekt- und Bewegungserkennung** eine IoT Central-Anwendung und dann eine Gerätevorlage für das Gatewaygerät erstellt und der Anwendung anschließend ein Gatewaygerät hinzugefügt.

Gehen Sie wie folgt vor, falls Sie die Anwendung vom Typ „Videoanalyse: Objekt- und Bewegungserkennung“ mit IoT Edge-Modulen ausprobieren möchten, für die eine Cloud-VM mit simulierten Videodatenströmen ausgeführt wird:

> [!div class="nextstepaction"]
> [Erstellen einer IoT Edge-Instanz für die Videoanalyse (Linux-VM)](./tutorial-video-analytics-iot-edge-vm.md)

Gehen Sie wie folgt vor, falls Sie die Anwendung vom Typ „Videoanalyse: Objekt- und Bewegungserkennung“ mit IoT Edge-Modulen ausprobieren möchten, für die ein echtes Gerät mit einer echten **ONVIF**-Kamera ausgeführt wird:

> [!div class="nextstepaction"]
> [Erstellen einer IoT Edge-Instanz für die Videoanalyse (Intel NUC)](./tutorial-video-analytics-iot-edge-nuc.md)
