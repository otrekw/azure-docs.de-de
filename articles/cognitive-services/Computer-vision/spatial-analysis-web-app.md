---
title: Bereitstellen einer Web-App zur räumlichen Analyse
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie die räumliche Analyse in einer Webanwendung verwenden.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 06/08/2021
ms.author: pafarley
ms.openlocfilehash: ecccbb4e2741cc7f413e9b2076bd3199b00dd9a8
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122356062"
---
# <a name="how-to-deploy-a-spatial-analysis-web-application"></a>Vorgehensweise: Bereitstellen einer Webanwendung zur räumlichen Analyse

In diesem Artikel erfahren Sie, wie Sie eine Web-App bereitstellen, die räumliche Analysedaten (Erkenntnisse) von IotHub sammelt und visualisiert. Dies kann nützliche Anwendungsmöglichkeiten in einer Vielzahl von Szenarien und Branchen darstellen. Möchte ein Unternehmen z. B. die Nutzung seiner Immobilienflächen optimieren, kann es schnell eine Lösung mit verschiedenen Szenarien erstellen. 

In diesem Lernprogramm lernen Sie Folgendes:

* Bereitstellen des Containers für die räumliche Analyse
* Konfigurieren des Vorgangs und der Kamera
* Konfigurieren der IoT Hub-Verbindung in der Webanwendung
* Bereitstellen und Testen der Webanwendung

In dieser App werden die folgenden Szenarien dargestellt:

* Anzahl der Personen, die einen Raum/Laden betreten/verlassen
* Anzahl der Personen, die einen Kassenbereich/eine Kassenzone betreten und verlassen, sowie die in der Kassenschlange verbrachte Zeit (Verweildauer)
* Anzahl der Personen, die eine Gesichtsmaske tragen 
* Anzahl der Personen, die gegen Richtlinien der räumlichen Distanzierung verstoßen

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement – [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/cognitive-services/)
* Grundlegende Kenntnisse der Bereitstellungskonfigurationen von Azure IoT Edge und eine Instanz von [Azure IoT Hub](../../iot-hub/index.yml)
* Ein konfigurierter [Hostcomputer](spatial-analysis-container.md)

## <a name="deploy-the-spatial-analysis-container"></a>Bereitstellen des Containers für die räumliche Analyse

Befolgen Sie das [Hostcomputersetup](./spatial-analysis-container.md), um den Hostcomputer zu konfigurieren und ein IoT Edge-Gerät mit Azure IoT Hub zu verbinden. 

### <a name="deploy-an-azure-iot-hub-service-in-your-subscription"></a>Bereitstellen eines Azure IoT Hub-Diensts in Ihrem Abonnement

Erstellen Sie zunächst eine Instanz eines Azure IoT Hub-Diensts mit dem Standard-Tarif (S1) oder dem Free-Tarif (S0). Befolgen Sie diese Anweisungen, um diese Instanz mithilfe der Azure-Befehlszeilenschnittstelle zu erstellen.

Füllen Sie die erforderlichen Parameter aus:
* Abonnement: Der Name oder die ID Ihres Azure-Abonnements
* Ressourcengruppe: Legen Sie einen Namen für Ihre Ressourcengruppe fest.
* IoT-Hub-Name: Legen Sie einen Namen für Ihren IoT-Hub fest.
* IoT-Hub-Name: Der Name des erstellten IoT-Hubs 
* Name des Edgegeräts: Legen Sie einen Namen für Ihr Edgegerät fest.

```azurecli
az login
az account set --subscription <name or ID of Azure Subscription>
az group create --name "<Resource Group Name>" --location "WestUS"

az iot hub create --name "<IoT Hub Name>" --sku S1 --resource-group "test-resource-group"

az iot hub device-identity create --hub-name "<IoT Hub Name>" --device-id "<Edge Device Name>" --edge-enabled
```

### <a name="deploy-the-container-on-azure-iot-edge-on-the-host-computer"></a>Bereitstellen des Containers in Azure IoT Edge auf dem Hostcomputer

Der nächste Schritt ist die Bereitstellung des Containers für die **räumliche Analyse** mithilfe der Azure-Befehlszeilenschnittstelle als IoT-Modul auf dem Hostcomputer. Der Bereitstellungsprozess erfordert eine Bereitstellungsmanifestdatei, in der die erforderlichen Container, Variablen und Konfigurationen für Ihre Bereitstellung beschrieben werden. Ein Beispiel für ein Bereitstellungsmanifest finden Sie unter [DeploymentManifest.js](https://github.com/Azure-Samples/cognitive-services-spatial-analysis/blob/main/deployment.json), das vorgefertigte Konfigurationen für alle Szenarien enthält.  

### <a name="set-environment-variables"></a>Festlegen von Umgebungsvariablen

Die meisten **Umgebungsvariablen** für das IoT Edge-Modul sind bereits in den unter dem Link oben angegebenen *DeploymentManifest.json*-Beispieldateien festgelegt. Suchen Sie in der Datei nach den Umgebungsvariablen `ENDPOINT` und `APIKEY`, wie unten gezeigt. Ersetzen Sie die Werte durch den Endpunkt-URI und den API-Schlüssel, die Sie zuvor erstellt haben. Stellen Sie sicher, dass der EULA-Wert auf „accept“ festgelegt ist. 

```json
"EULA": { 
    "value": "accept"
},
"BILLING":{ 
    "value": "<Use a key from your Computer Vision resource>"
},
"APIKEY":{
    "value": "<Use the endpoint from your Computer Vision resource>"
}
```

### <a name="configure-the-operation-parameters"></a>Konfigurieren der Vorgangsparameter

Wenn Sie das Beispiel [DeploymentManifest.json](https://github.com/Azure-Samples/cognitive-services-spatial-analysis/blob/main/deployment.json) verwenden, das bereits alle erforderlichen Konfigurationen (Vorgänge, URLs und Zonen für aufgezeichnete Videodateien usw.) enthält, können Sie zum Abschnitt **Ausführen der Bereitstellung** übergehen.

Nachdem die anfängliche Konfiguration des Containers für die räumliche Analyse nun abgeschlossen ist, konfigurieren Sie im nächsten Schritt die Vorgangsparameter und fügen diese der Bereitstellung hinzu. 

Der erste Schritt besteht darin, das Beispiel [DeploymentManifest.json](https://github.com/Azure-Samples/cognitive-services-spatial-analysis/blob/main/deployment.json) zu aktualisieren und den gewünschten Vorgang zu konfigurieren. Die Konfiguration für „cognitiveservices.vision.spatialanalysis-personcount“ wird z. B. unten gezeigt:

```json
"personcount": {
    "operationId": "cognitiveservices.vision.spatialanalysis-personcount",
    "version": 1,
    "enabled": true,
    "parameters": {
        "VIDEO_URL": "<Replace RTSP URL here>",
        "VIDEO_SOURCE_ID": "<Replace with friendly name>",
        "VIDEO_IS_LIVE":true,
        "DETECTOR_NODE_CONFIG": "{ \"gpu_index\": 0 }",
        "SPACEANALYTICS_CONFIG": "{\"zones\":[{\"name\":\"queue\",\"polygon\":[<Replace with your values>], \"events\": [{\"type\":\"count\"}], \"threshold\":<use 0 for no threshold.}]}"
    }
},
```

Nachdem das Bereitstellungsmanifest aktualisiert wurde, befolgen Sie die Anweisungen des Kameraherstellers, um die Kamera zu installieren und die Kamera-URL, den Benutzernamen und das Kennwort zu konfigurieren. 

Legen Sie als Nächstes `VIDEO_URL` auf die RTSP-URL der Kamera und die Anmeldeinformationen für das Herstellen einer Verbindung mit der Kamera fest.

Wenn das Edgegerät über mehrere GPUs verfügt, wählen Sie die GPU aus, auf der dieser Vorgang ausgeführt werden soll. Nehmen Sie unbedingt einen Lastenausgleich für die Vorgänge vor, sodass nicht mehr als acht Vorgänge gleichzeitig auf einer einzelnen GPU ausgeführt werden.  

Konfigurieren Sie als Nächstes die Zone, in der Sie die Personen zählen möchten. Befolgen Sie zum Konfigurieren des Zonenpolygons zunächst die Anweisungen des Herstellers, um einen Frame von der Kamera abzurufen. Um einen einzelnen Vertex des Polygons zu ermitteln, wählen Sie einen Punkt im Frame aus, nehmen die x-y-Pixelkoordinaten des Punkts relativ zur linken oberen Ecke des Frames und teilen sie durch die entsprechenden Frameabmessungen. Legen Sie die Ergebnisse als x-y-Koordinaten des Vertex fest. Sie können die Konfiguration des Zonenpolygons im Feld `SPACEANALYTICS_CONFIG` festlegen.

Dies ist ein Beispiel für einen Videoframe, in dem gezeigt wird, wie die Vertexkoordinaten für einen Frame der Größe 1.920 × 1.080 berechnet werden.
![Beispielvideoframe](./media/spatial-analysis/sample-video-frame.jpg)

Sie können auch einen Konfidenzschwellenwert auswählen, ab dem erkannte Personen gezählt und Ereignisse generiert werden. Legen Sie den Schwellenwert auf 0 fest, wenn alle Ereignisse ausgegeben werden sollen.

### <a name="execute-the-deployment"></a>Ausführen der Bereitstellung

Nachdem das Bereitstellungsmanifest fertig ist, verwenden Sie diesen Befehl in der Azure-Befehlszeilenschnittstelle, um den Container auf dem Hostcomputer als IoT Edge-Modul bereitzustellen.

```azurecli
az login
az extension add --name azure-iot
az iot edge set-modules --hub-name "<IoT Hub name>" --device-id "<IoT Edge device name>" --content DeploymentManifest.json -–subscription "<subscriptionId>"
```

Füllen Sie die erforderlichen Parameter aus:

* IoT-Hub-Name: Der Name Ihrer Azure IoT Hub-Instanz
* DeploymentManifest.json: Der Name Ihrer Bereitstellungsdatei
* Name des IoT Edge-Geräts: Der Name des IoT Edge-Geräts Ihres Hostcomputers
* Abonnement: Abonnement-ID oder -name

Mit diesem Befehl wird die Bereitstellung gestartet, und Sie können den Bereitstellungsstatus in der Azure IoT Hub-Instanz im Azure-Portal anzeigen. Als Status wird möglicherweise *417: Die Bereitstellungskonfiguration des Geräts ist nicht festgelegt* angezeigt, bis das Gerät das Herunterladen der Containerimages abgeschlossen hat und gestartet wird.

### <a name="validate-that-the-deployment-was-successful"></a>Überprüfen der erfolgreichen Bereitstellung

Suchen Sie im Azure-Portal in den Einstellungen des IoT Edge-Moduls für die räumliche Analyse in Ihrer IoT Hub-Instanz nach dem *Laufzeitstatus*. Der **gewünschte Wert** und der **gemeldete Wert** für den *Laufzeitstatus* sollten `Running` lauten. Im Folgenden finden Sie Informationen dazu, wie dies im Azure-Portal dargestellt wird.

![Beispiel für die Bereitstellungsüberprüfung](./media/spatial-analysis/deployment-verification.png)

Zu diesem Zeitpunkt führt der Container für räumliche Analysen den Vorgang aus. Er gibt KI-Erkenntnisse für die Vorgänge aus und leitet diese Erkenntnisse als Telemetriedaten an Ihre Azure IoT Hub-Instanz weiter. Um weitere Kameras zu konfigurieren, können Sie die Datei mit dem Bereitstellungsmanifest aktualisieren und die Bereitstellung erneut ausführen.

## <a name="spatial-analysis-web-application"></a>Webanwendung zur räumlichen Analyse

Mit der Webanwendung zur räumlichen Analyse können Entwickler schnell eine Beispiel-Web-App konfigurieren, sie in ihrer Azure-Umgebung hosten und die App zum Überprüfen von E2E-Ereignissen verwenden.

## <a name="build-docker-image"></a>Erstellen eines Docker-Images

Befolgen Sie den [Leitfaden](https://github.com/Azure-Samples/cognitive-services-spatial-analysis/blob/main/README.md#docker-image) zum Erstellen und Pushen des Images in eine Azure Container Registry in Ihrem Abonnement.

## <a name="setup-steps"></a>Einrichtungsschritte

Erstellen Sie zum Installieren des Containers eine neue Azure App Service-Instanz, und geben Sie die erforderlichen Parameter an. Wechseln Sie dann zur Registerkarte **Docker**, und wählen Sie **Einzelner Container** und dann **Azure Container Registry** aus. Verwenden Sie Ihre Instanz von Azure Container Registry, in die Sie das Image zuvor gepusht haben.

![Eingeben von Imagedetails](./media/spatial-analysis/solution-app-create-screen.png)

Klicken Sie nach dem Eingeben der oben genannten Parameter auf **Überprüfen + erstellen**, und erstellen Sie die App.

### <a name="configure-the-app"></a>Konfigurieren der App 

Warten Sie, bis das Setup abgeschlossen wurde, und navigieren Sie im Azure-Portal zu Ihrer Ressource. Wechseln Sie zum Abschnitt **Konfiguration**, und fügen Sie die folgenden beiden **Anwendungseinstellungen** hinzu.

* `EventHubConsumerGroup`: der Zeichenfolgenname der Consumergruppe aus Ihrer Azure IoT Hub-Instanz. Sie können eine neue Consumergruppe in Ihrem IoT-Hub erstellen oder die Standardgruppe verwenden. 
* `IotHubConnectionString`: die Verbindungszeichenfolge für Ihre Azure IoT Hub-Instanz. Sie können sie im Abschnitt „Schlüssel“ Ihrer Azure IoT Hub-Ressource unter ![Parameter konfigurieren](./media/spatial-analysis/solution-app-config-page.png) abrufen.

Nachdem diese beiden Einstellungen hinzugefügt wurden, klicken Sie auf **Speichern**. Klicken Sie dann im linken Navigationsmenü auf **Authentifizierung/Autorisierung**, und legen Sie die gewünschte Authentifizierungsebene fest. Azure Active Directory Express (Azure AD) wird empfohlen. 

### <a name="test-the-app"></a>Testen der App

Wechseln Sie zum Azure-Dienst, und vergewissern Sie sich, dass die Bereitstellung erfolgreich war und die Web-App ausgeführt wird. Navigieren Sie zur konfigurierten URL `<yourapp>.azurewebsites.net`, um die ausgeführte App anzuzeigen.

![Testen der Bereitstellung](./media/spatial-analysis/solution-app-output.png)

## <a name="get-the-personcount-source-code"></a>Abrufen des PersonCount-Quellcodes
Wenn Sie den Quellcode für diese Anwendung anzeigen oder ändern möchten, finden Sie ihn [auf GitHub](https://github.com/Azure-Samples/cognitive-services-spatial-analysis).

## <a name="next-steps"></a>Nächste Schritte

* [Konfigurieren von Vorgängen zur räumlichen Analyse](./spatial-analysis-operations.md)
* [Protokollierung und Problembehandlung](spatial-analysis-logging.md)
* [Leitfaden zur Kameraplatzierung](spatial-analysis-camera-placement.md)
* [Leitfaden zur Platzierung von Zonen und Linien](spatial-analysis-zone-line-placement.md)
