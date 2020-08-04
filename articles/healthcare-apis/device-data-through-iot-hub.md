---
title: 'Tutorial: Empfangen von Gerätedaten über Azure IoT Hub'
description: In diesem Tutorial erfahren Sie, wie Sie das Routen von Gerätedaten von IoT Hub über IoT Connector zu Azure API for FHIR aktivieren.
services: healthcare-apis
author: ms-puneet-nagpal
ms.service: healthcare-apis
ms.subservice: iomt
ms.topic: tutorial
ms.date: 06/27/2020
ms.author: punagpal
ms.openlocfilehash: aeed0c90eeecd59e23a1d87a3ebc1e1a836a84ec
ms.sourcegitcommit: 0820c743038459a218c40ecfb6f60d12cbf538b3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87116838"
---
# <a name="tutorial-receive-device-data-through-azure-iot-hub"></a>Tutorial: Empfangen von Gerätedaten über Azure IoT Hub

IoT Connector ermöglicht die Erfassung der Daten von IoMT-Geräten (Internet of Medical Things) in Azure API for FHIR. Die Schnellstartanleitung [Bereitstellen von IoT Connector (Vorschau) über das Azure-Portal](iot-fhir-portal-quickstart.md) enthält ein Beispiel mit einem Gerät, das von Azure IoT Central verwaltet wird und [Telemetriedaten an IoT Connector sendet](iot-fhir-portal-quickstart.md#connect-your-devices-to-iot). IoT Connector kann auch mit Geräten verwendet werden, die über Azure IoT Hub bereitgestellt und verwaltet werden. In diesem Tutorial erfahren Sie, wie Sie eine Verbindung herstellen und Gerätedaten von Azure IoT Hub an IoT Connector weiterleiten.

## <a name="prerequisites"></a>Voraussetzungen

- Ein aktives Azure-Abonnement. ([Kostenloses Abonnement erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F))
- Azure API for FHIR-Ressource mit mindestens einer IoT Connector-Instanz. Weitere Informationen finden Sie unter [Bereitstellen von IoT Connector (Vorschau) über das Azure-Portal](iot-fhir-portal-quickstart.md).
- Azure IoT Hub-Ressource, die mit mindestens einem echten oder simulierten Gerät verbunden ist. Weitere Informationen finden Sie unter [Schnellstart: Senden von Telemetriedaten von einem Gerät an einen IoT-Hub und Lesen der Telemetriedaten mit einer Back-End-Anwendung (.NET)](https://docs.microsoft.com/azure/iot-hub/quickstart-send-telemetry-dotnet).

> [!TIP]
> Bei Verwendung einer simulierten Azure IoT Hub-Geräteanwendung können Sie aus verschiedenen unterstützten Sprachen und Systemen die gewünschte Anwendung auswählen.

## <a name="get-connection-string-for-iot-connector-preview"></a>Abrufen der Verbindungszeichenfolge für IoT Connector (Vorschau)

Azure IoT Hub benötigt eine Verbindungszeichenfolge, um eine sichere Verbindung mit Ihrer IoT Connector-Instanz herstellen zu können. Erstellen Sie eine neue Verbindungszeichenfolge für Ihre IoT Connector-Instanz. Eine entsprechende Anleitung finden Sie unter [Generieren einer Verbindungszeichenfolge](iot-fhir-portal-quickstart.md#generate-a-connection-string). Speichern Sie diese Verbindungszeichenfolge für den nächsten Abschnitt.

Von IoT Connector wird im Hintergrund eine Azure Event Hub-Instanz verwendet, um Gerätenachrichten zu empfangen. Die oben erstellte Verbindungszeichenfolge ist im Grunde die Verbindungszeichenfolge für diesen zugrunde liegenden Event Hub.

## <a name="connect-azure-iot-hub-with-the-iot-connector-preview"></a>Herstellen einer Verbindung zwischen Azure IoT Hub und IoT Connector (Vorschau)

Azure IoT Hub unterstützt das so genannte [Nachrichtenrouting](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-d2c). Mit diesem Feature können Gerätedaten an verschiedene Azure-Dienste wie Event Hub, Speicherkonto und Service Bus gesendet werden. IoT Connector nutzt dieses Feature, um eine Verbindung herzustellen und Gerätedaten von Azure IoT Hub an den zugehörigen Event Hub-Endpunkt zu senden.

> [!NOTE] 
> Aktuell kann zum [Erstellen des Nachrichtenroutings](https://docs.microsoft.com/azure/iot-hub/tutorial-routing) nur PowerShell oder ein CLI-Befehl verwendet werden, da der Event Hub von IoT Connector nicht im Kundenabonnement gehostet und somit nicht im Azure-Portal angezeigt wird. Nachdem die Nachrichtenroutenobjekte mithilfe von PowerShell oder über die Befehlszeilenschnittstelle hinzugefügt wurden, sind Sie jedoch im Azure-Portal verfügbar und können dort verwaltet werden.

Die Einrichtung eines Nachrichtenroutings umfasst zwei Schritte:

### <a name="add-an-endpoint"></a>Hinzufügen eines Endpunkts
In diesem Schritt wird ein Endpunkt definiert, an den die Daten durch die IoT Hub-Instanz weitergeleitet werden. Dieser Endpunkt kann nach Belieben mithilfe des PowerShell-Befehls [Add-AzIotHubRoutingEndpoint](https://docs.microsoft.com/powershell/module/az.iothub/Add-AzIotHubRoutingEndpoint) oder mithilfe des CLI-Befehls [az iot hub routing-endpoint create](https://docs.microsoft.com/cli/azure/iot/hub/routing-endpoint?#az-iot-hub-routing-endpoint-create) erstellt werden.

Im Anschluss sind die Parameter für den Endpunkterstellungsbefehl aufgeführt:

|PowerShell-Parameter|CLI-Parameter|BESCHREIBUNG|
|---|---|---|
|ResourceGroupName|resource-group|Der Ressourcengruppenname Ihrer IoT Hub-Instanz.|
|Name|hub-name|Der Name Ihrer IoT Hub-Ressource.|
|EndpointName|endpoint-name|Verwenden Sie einen Namen, den Sie dem Endpunkt zuweisen möchten, den Sie gerade erstellen.|
|EndpointType|endpoint-type|Die Art des Endpunkts, mit dem von IoT Hub eine Verbindung hergestellt werden muss. Verwenden Sie den Literalwert „EventHub“ (PowerShell) oder „eventhub“ (Befehlszeilenschnittstelle).|
|EndpointResourceGroup|endpoint-resource-group|Der Name der Ressourcengruppe für die Azure API for FHIR-Ressource Ihrer IoT Connector-Instanz. Diesen Wert finden Sie auf der Übersichtsseite von Azure API for FHIR.|
|EndpointSubscriptionId|endpoint-subscription-id|Die Abonnement-ID für die Azure API for FHIR-Ressource Ihrer IoT Connector-Instanz. Diesen Wert finden Sie auf der Übersichtsseite von Azure API for FHIR.|
|ConnectionString|connection-string|Die Verbindungszeichenfolge für Ihre IoT Connector-Instanz. Verwenden Sie den Wert aus dem vorherigen Schritt.|

### <a name="add-a-message-route"></a>Hinzufügen einer Nachrichtenroute
In diesem Schritt wird eine Nachrichtenroute mit dem oben erstellten Endpunkt definiert. Für die Routenerstellung können Sie entweder den PowerShell-Befehl [Add-AzIotHubRoute](https://docs.microsoft.com/powershell/module/az.iothub/Add-AzIoTHubRoute) oder den CLI-Befehl [az iot hub route create](https://docs.microsoft.com/cli/azure/iot/hub/route#az-iot-hub-route-create) verwenden.

Im Anschluss sind die Parameter für den Endpunkterstellungsbefehl aufgeführt:

|PowerShell-Parameter|CLI-Parameter|BESCHREIBUNG|
|---|---|---|
|ResourceGroupName|g|Der Ressourcengruppenname Ihrer IoT Hub-Instanz.|
|Name|hub-name|Der Name Ihrer IoT Hub-Ressource.|
|EndpointName|endpoint-name|Der Name des Endpunkts, den Sie weiter oben erstellt haben.|
|RouteName|route-name|Ein Name, den Sie der Nachrichtenroute zuweisen möchten, die Sie gerade erstellen.|
|`Source`|source-type|Die Art von Daten, die an den Endpunkt gesendet werden sollen. Verwenden Sie den Literalwert „DeviceMessages“ (PowerShell) oder „devicemessages“ (Befehlszeilenschnittstelle).|

## <a name="send-device-message-to-iot-hub"></a>Senden einer Gerätenachricht an IoT Hub

Verwenden Sie Ihr (echtes oder simuliertes) Gerät, um die unten gezeigte exemplarische Herzfrequenznachricht an Azure IoT Hub zu senden. Diese Nachricht wird an IoT Connector weitergeleitet. Dort wird Nachricht dann in eine FHIR-Überwachungsressource transformiert und in Azure API for FHIR gespeichert.

```json
{
  "HeartRate": 80,
  "RespiratoryRate": 12,
  "HeartRateVariability": 64,
  "BodyTemperature": 99.08839032397609,
  "BloodPressure": {
    "Systolic": 23,
    "Diastolic": 34
  },
  "Activity": "walking"
}
```
> [!IMPORTANT]
> Die gesendete Gerätenachricht muss den für Ihre IoT Connector-Instanz konfigurierten [Zuordnungsvorlagen](iot-mapping-templates.md) entsprechen.

## <a name="view-device-data-in-azure-api-for-fhir"></a>Anzeigen von Gerätedaten in Azure API for FHIR

Mithilfe von Postman können Sie die FHIR-Überwachungsressourcen anzeigen, die von IoT Connector für Azure API for FHIR erstellt wurden. Richten Sie [Postman für den Zugriff auf Azure API for FHIR](access-fhir-postman-tutorial.md) ein, und senden Sie eine Anforderung vom Typ `GET` an `https://your-fhir-server-url/Observation?code=http://loinc.org|8867-4`, um in der obigen Beispielnachricht übermittelte FHIR-Überwachungsressourcen mit Herzfrequenz anzuzeigen.

> [!TIP]
> Vergewissern Sie sich, dass Ihr Benutzer über die erforderlichen Zugriffsrechte für die Azure API for FHIR-Datenebene verfügt. Verwenden Sie die [rollenbasierte Zugriffssteuerung in Azure](configure-azure-rbac.md), um die erforderlichen Datenebenenrollen zuzuweisen.


## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung wurde Azure IoT Hub für die Weiterleitung von Gerätedaten an IoT Connector eingerichtet. Weitere Informationen zu IoT Connector finden Sie in den folgenden Schritten:

Machen Sie sich mit verschiedenen Datenflussphasen in IoT Connector vertraut:

>[!div class="nextstepaction"]
>[IoT Connector (Vorschau): Datenfluss](iot-data-flow.md)

Erfahren Sie, wie Sie IoT Connector mithilfe von Geräte- und FHIR-Zuordnungsvorlagen konfigurieren:

>[!div class="nextstepaction"]
>[IoT Connector (Vorschau): Zuordnungsvorlagen](iot-mapping-templates.md)

FHIR ist ein eingetragenes Markenzeichen von HL7 und wird mit Erlaubnis von HL7 verwendet.

