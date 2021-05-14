---
title: Bereitstellen der Azure IoT Central-Geräte-Bridge | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie die Azure IoT Central-Geräte-Bridge bereitstellen, um andere IoT-Clouds mit Ihrer IoT Central-App zu verbinden. Beispiele für andere IoT-Clouds wären etwa Sigfox, Particle Device Cloud und The Things Network.
services: iot-central
ms.service: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 04/19/2021
ms.topic: how-to
ms.openlocfilehash: 6b535ecb80fae9f55eb6ab11751c26e0c6d0e9e5
ms.sourcegitcommit: 3ed0f0b1b66a741399dc59df2285546c66d1df38
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/19/2021
ms.locfileid: "107713718"
---
# <a name="use-the-iot-central-device-bridge-to-connect-other-iot-clouds-to-iot-central"></a>Verwenden der Azure IoT Central-Geräte-Bridge, um andere IoT-Clouds mit IoT Central zu verbinden

*Dieser Artikel gilt für Administratoren.*

## <a name="azure-iot-central-device-bridge"></a>Azure IoT Central-Geräte-Bridge

Die IoT Central-Geräte-Bridge ist eine Open-Source-Lösung, die dazu dient, andere IoT-Clouds mit Ihrer IoT Central-Anwendung zu verbinden. Beispiele für andere IoT-Clouds wären etwa [Sigfox](https://www.sigfox.com/), [Particle Device Cloud](https://www.particle.io/) und [The Things Network](https://www.thethingsnetwork.org/). Die Geräte-Bridge leitet Daten von Geräten, die mit anderen IoT-Clouds verbunden sind, an Ihre IoT Central-Anwendung weiter. Von der Geräte-Bridge werden nur Daten an IoT Central weitergeleitet. Es werden jedoch keine Befehle oder Eigenschaftenupdates von IoT Central an die Geräte zurückgesendet.

Mit der Geräte-Bridge können Sie die Funktionen von IoT Central beispielsweise mit Geräten zur Ressourcennachverfolgung, die mit dem Low Power Wide Area Network von Sigfox verbunden sind, mit Geräten zur Überwachung der Luftqualität in Particle Device Cloud oder mit Geräten zur Messung der Bodenfeuchtigkeit in The Things Network kombinieren. Sie können IoT Central-Features wie Regeln und Analysen für die Daten verwenden, Workflows in Power Automate und Azure Logic Apps erstellen oder die Daten exportieren.

Von der Geräte-Bridge-Lösung werden mehrere Azure-Ressourcen in Ihrem Azure-Abonnement bereitgestellt, die zusammenarbeiten, um Gerätenachrichten zu transformieren und an IoT Central weiterzuleiten.

## <a name="prerequisites"></a>Voraussetzungen

Um die in dieser Anleitung aufgeführten Schritte ausführen zu können, benötigen Sie ein aktives Azure-Abonnement.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

Führen Sie die Schritte der Schnellstartanleitung [Erstellen einer Azure IoT Central-Anwendung](./quick-deploy-iot-central.md) aus, um mit der Vorlage **Benutzerdefinierte App > Benutzerdefinierte Anwendung** eine IoT Central-Anwendung zu erstellen.

## <a name="overview"></a>Übersicht

Die IoT Central-Geräte-Bridge ist eine Open-Source-Lösung in GitHub. Sie verwendet eine benutzerdefinierte Azure Resource Manager-Vorlage, um mehrere Ressourcen in Ihrem Azure-Abonnement bereitzustellen. Dazu zählt auch eine Azure-Funktions-App.

Die Funktions-App ist die Kernkomponente der Geräte-Bridge. Sie empfängt HTTP POST-Anforderungen von anderen IoT-Plattformen über einen einfachen Webhook. Das [Repository für die Azure IoT Central-Geräte-Bridge](https://github.com/Azure/iotc-device-bridge) enthält Beispiele, die die Verbindungsherstellung mit Sigfox-, Particle- und The Things Network-Clouds veranschaulichen. Diese Lösung kann erweitert werden, um eine Verbindung mit Ihrer benutzerdefinierten IoT-Cloud herzustellen – vorausgesetzt, Ihre Plattform kann HTTP POST-Anforderungen an Ihre Funktions-App senden.

Die Daten werden von der Funktions-App in ein von IoT Central akzeptiertes Format transformiert und unter Verwendung des Gerätebereitstellungsdiensts und der Geräteclient-APIs weitergeleitet:

:::image type="content" source="media/howto-build-iotc-device-bridge/azure-function.png" alt-text="Screenshot: Azure Functions":::

Wenn Ihre IoT Central-Anwendung die Geräte-ID in der weitergeleiteten Nachricht erkennt, werden die Telemetriedaten des Geräts in IoT Central angezeigt. Wird die Geräte-ID von Ihrer IoT Central-Anwendung nicht erkannt, versucht die Funktions-App, ein neues Gerät mit der Geräte-ID zu registrieren. Das neue Gerät wird in Ihrer IoT Central-Anwendung auf der Seite **Geräte** als **nicht zugeordnetes Gerät** angezeigt. Auf der Seite **Geräte** können Sie das neue Gerät einer Gerätevorlage zuordnen und dann die Telemetriedaten anzeigen.

## <a name="deploy-the-device-bridge"></a>Bereitstellen der Geräte-Bridge

So stellen Sie die Geräte-Bridge in Ihrem Abonnement bereit:

1. Navigieren Sie in Ihrer IoT Central-Anwendung zur Seite **Verwaltung > Geräteverbindung**.

    1. Notieren Sie sich den Wert für **ID-Bereich**. Dieser Wert wird beim Bereitstellen der Geräte-Bridge verwendet.

    1. Öffnen Sie auf der gleichen Seite die Registrierungsgruppe **SAS-IoT-Devices.** Kopieren Sie auf der Gruppenseite **SAS-IoT-Devices** den **Primärschlüssel**. Dieser Wert wird beim Bereitstellen der Geräte-Bridge verwendet.

1. Verwenden Sie die im Anschluss angezeigte Schaltfläche **In Azure bereitstellen**, um die benutzerdefinierte Resource Manager-Vorlage zu öffnen, durch die die Funktions-App in Ihrem Abonnement bereitgestellt wird. Verwenden Sie den **ID-Bereich** und den **Primärschlüssel** aus dem vorherigen Schritt:

    [![In Azure bereitstellen](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fiotc-device-bridge%2Fmaster%2Fazuredeploy.json)

Nach Abschluss der Bereitstellung müssen die von der Funktion benötigten NPM-Pakete installiert werden:

1. Öffnen Sie im Azure-Portal die in Ihrem Abonnement bereitgestellte Funktions-App. Navigieren Sie zu **Entwicklungstools > Konsole**. Führen Sie in der Konsole die folgenden Befehle aus, um die Pakete zu installieren:

    ```bash
    cd IoTCIntegration
    npm install
    ```

    Die Ausführung dieser Befehle kann mehrere Minuten dauern. Gegebenenfalls angezeigte Warnmeldungen können ignoriert werden.

1. Wählen Sie nach Abschluss der Paketinstallation auf der Seite **Übersicht** der Funktions-App die Option **Neu starten** aus:

    :::image type="content" source="media/howto-build-iotc-device-bridge/restart.png" alt-text="Screenshot: „Neu starten“":::

1. Die Funktion kann jetzt verwendet werden. Externe Systeme können HTTP POST-Anforderungen verwenden, um Gerätedaten über die Geräte-Bridge an Ihre IoT Central-Anwendung zu senden. Navigieren Sie zum Abrufen der Funktions-URL zu **Funktionen > IoTCIntegration > Programmieren und testen > Funktions-URL abrufen**:

    :::image type="content" source="media/howto-build-iotc-device-bridge/get-function-url.png" alt-text="Screenshot: „Funktions-URL abrufen“":::

Nachrichtentext muss im folgenden Format an die Geräte-Bridge gesendet werden:

```json
"device": {
  "deviceId": "my-cloud-device"
},
"measurements": {
  "temp": 20.31,
  "pressure": 50,
  "humidity": 8.5,
  "ledColor": "blue"
}
```

Jeder Schlüssel im Objekt `measurements` muss mit dem Namen eines Telemetrietyps in der Gerätevorlage in der IoT Central-Anwendung übereinstimmen. Die Angabe der Schnittstellen-ID im Nachrichtentext wird von dieser Lösung nicht unterstützt. Wenn also zwei verschiedene Schnittstellen über einen Telemetrietyp gleichen Namens verfügen, erscheint die Messung in beiden Telemetriedatenströmen in Ihrer IoT Central-Anwendung.

Sie können ein Feld vom Typ `timestamp` in den Text einschließen, um das UTC-Datum und die UTC-Uhrzeit der Nachricht anzugeben. Für dieses Feld muss das ISO 8601-Format verwendet werden. Beispiel: `2020-06-08T20:16:54.602Z`. Wenn Sie keinen Zeitstempel einschließen, werden das aktuelle Datum und die aktuelle Uhrzeit verwendet.

Sie können ein Feld vom Typ `modelId` in den Text einschließen. Mithilfe dieses Felds kann das Gerät während der Bereitstellung einer Gerätevorlage zugeordnet werden. Diese Funktion wird nur von [V3-Anwendungen](howto-get-app-info.md) unterstützt.

Die Geräte-ID (`deviceId`) muss alphanumerisch sein und darf nur Kleinbuchstaben und ggf. Bindestriche enthalten.

Wenn Sie das Feld `modelId` nicht einschließen oder die Modell-ID von IoT Central nicht erkannt wird, wird im Falle einer Nachricht mit einer unbekannten Geräte-ID (`deviceId`) in IoT Central ein neues _nicht zugeordnetes Gerät_ erstellt. Ein Operator kann das Gerät manuell zur richtigen Gerätevorlage migrieren. Weitere Informationen finden Sie unter [Verwalten von Geräten in Ihrer Azure IoT Central-Anwendung > Migrieren von Geräten zu einer Vorlage](howto-manage-devices.md).

In [V2-Anwendungen](howto-get-app-info.md) wird das neue Gerät im Geräte-Explorer auf der Seite **Nicht zugeordnete Geräte** angezeigt. Wählen Sie **Zuordnen** und anschließend eine Gerätevorlage aus, um eingehende Telemetriedaten vom Gerät zu empfangen.

> [!NOTE]
> Solange das Gerät keiner Vorlage zugeordnet ist, wird bei allen HTTP-Aufrufen für die Funktion der Fehlerstatus 403 zurückgegeben.

Wenn Sie für die Funktions-App die Protokollierung mit Application Insights aktivieren möchten, navigieren Sie in Ihrer Funktions-App im Azure-Portal zu **Überwachung > Protokolle**. Wählen Sie **Application Insights aktivieren** aus.

## <a name="provisioned-resources"></a>Bereitgestellte Ressourcen

Durch die Resource Manager-Vorlage werden in Ihrem Azure-Abonnement folgende Ressourcen bereitgestellt:

* Funktionen-App
* App Service-Plan
* Speicherkonto
* Schlüsseltresor

Der Schlüsseltresor dient zum Speichern des SAS-Gruppenschlüssels für Ihre IoT Central-Anwendung.

Die Funktions-App wird auf der Grundlage eines [Verbrauchsplan](https://azure.microsoft.com/pricing/details/functions/) ausgeführt. Diese Option bietet zwar keine dedizierten Computeressourcen, ermöglicht es der Geräte-Bridge jedoch, Hunderte von Gerätenachrichten pro Minute zu bewältigen, was für kleinere Gerätebestände sowie für Geräte geeignet ist, von denen seltener Nachrichten gesendet werden. Wenn für Ihre Anwendung sehr viele Gerätenachrichten gestreamt werden müssen, ersetzen Sie den Verbrauchsplan durch einen dedizierten [App Service-Plan](https://azure.microsoft.com/pricing/details/app-service/windows/). Dieser Plan bietet dedizierte Computeressourcen, die schnellere Serverantwortzeiten ermöglichen. Mit einem standardmäßigen App Service-Plans wurde für die Azure-Funktion in diesem Repository eine maximale Leistung von rund 1.500 Gerätenachrichten pro Minute ermittelt. Weitere Informationen finden Sie unter [Azure Functions-Hostingoptionen](../../azure-functions/functions-scale.md).

Wenn Sie anstelle eines Verbrauchsplans einen dedizierten App Service-Plan verwenden möchten, müssen Sie die benutzerdefinierte Vorlage vor der Bereitstellung bearbeiten. Wählen Sie **Vorlage bearbeiten** aus.

:::image type="content" source="media/howto-build-iotc-device-bridge/edit-template.png" alt-text="Screenshot: „Vorlage bearbeiten“":::

Ersetzen Sie das folgende Segment:

```json
{
  "type": "Microsoft.Web/serverfarms",
  "apiVersion": "2015-04-01",
  "name": "[variables('planName')]",
  "location": "[resourceGroup().location]",
  "properties": {
    "name": "[variables('planName')]",
    "computeMode": "Dynamic",
    "sku": "Dynamic"
  }
},
```

durch

```json
{
  "type": "Microsoft.Web/serverfarms",
  "sku": {
      "name": "S1",
      "tier": "Standard",
      "size": "S1",
      "family": "S",
      "capacity": 1
  },
  "kind": "app",
  "name": "[variables('planName')]",
  "apiVersion": "2016-09-01",
  "location": "[resourceGroup().location]",
  "tags": {
      "iotCentral": "device-bridge",
      "iotCentralDeviceBridge": "app-service-plan"
  },
  "properties": {
      "name": "[variables('planName')]"
  }
},
```

Bearbeiten Sie als Nächstes die Vorlage, um `"alwaysOn": true` in die Konfiguration für die Ressource `functionapp` unter `"properties": {"SiteConfig": {...}}` einzuschließen. Durch die [alwaysOn-Konfiguration](https://github.com/Azure/Azure-Functions/wiki/Enable-Always-On-when-running-on-dedicated-App-Service-Plan) wird sichergestellt, dass die Funktions-App immer ausgeführt wird.

## <a name="examples"></a>Beispiele

In den folgenden Beispielen wird die Geräte-Bridge für verschiedene IoT-Clouds konfiguriert:

### <a name="example-1-connecting-particle-devices-through-the-device-bridge"></a>Beispiel 1: Verbinden von Particle-Geräten über die Geräte-Bridge

Wenn Sie ein Particle-Gerät über die Geräte-Bridge mit IoT Central verbinden möchten, wechseln Sie zur Particle-Konsole, und erstellen Sie eine neue Webhookintegration. Legen Sie das **Anforderungsformat** auf **JSON** fest.  Verwenden Sie unter **Erweiterte Einstellungen** das folgende benutzerdefinierte Textformat:

```json
{
  "device": {
    "deviceId": "{{{PARTICLE_DEVICE_ID}}}"
  },
  "measurements": {
    "{{{PARTICLE_EVENT_NAME}}}": {{{PARTICLE_EVENT_VALUE}}}
  }
}
```

Fügen Sie die **Funktions-URL** aus Ihrer Azure-Funktions-App ein. Daraufhin werden Particle-Geräte als nicht zugeordnete Geräte in IoT Central angezeigt. Weitere Informationen finden Sie im [Blogbeitrag zum Integrieren Particle-basierter Projekte in Azure IoT Central](https://blog.particle.io/2019/09/26/integrate-particle-with-azure-iot-central/).

### <a name="example-2-connecting-sigfox-devices-through-the-device-bridge"></a>Beispiel 2: Verbinden von Sigfox-Geräten über die Geräte-Bridge

Bei einigen Plattformen ist es möglicherweise nicht möglich, das Format von Gerätenachrichten anzugeben, die über einen Webhook gesendet werden. Für solche Systeme müssen die Nutzdaten der Nachricht vor der Verarbeitung durch die Geräte-Bridge in das erwartete Textformat konvertiert werden. Die Konvertierung kann in der gleichen Azure-Funktion durchgeführt werden, von der auch die Geräte-Bridge ausgeführt wird.

In diesem Abschnitt wird gezeigt, wie Sie die Nutzdaten einer Sigfox-Webhookintegration in das von der Geräte-Bridge erwartete Textformat konvertieren. Gerätedaten werden von der Sigfox-Cloud in einem hexadezimalen Zeichenfolgenformat übertragen. Der Einfachheit halber enthält die Geräte-Bridge eine Konvertierungsfunktion für dieses Format, die eine Teilmenge der möglichen Feldtypen in Sigfox-Gerätenutzdaten akzeptiert: `int` und `uint` mit 8, 16, 32 oder 64 Bits, `float` mit 32 oder 64 Bits sowie Little-Endian und Big-Endian. Für die Verarbeitung von Nachrichten einer Sigfox-Webhookintegration muss die Datei _IoTCIntegration/index.js_ in der Funktions-App wie im Anschluss beschrieben geändert werden.

Fügen Sie zum Konvertieren der Nachrichtennutzdaten vor dem Aufruf von `handleMessage` in Zeile 21 den folgenden Code hinzu, und ersetzen Sie dabei `payloadDefinition` durch Ihre Sigfox-Nutzdatendefinition:

```javascript
const payloadDefinition = 'gforce::uint:8 lat::uint:8 lon::uint:16'; // Replace this with your payload definition

req.body = {
    device: {
        deviceId: req.body.device
    },
    measurements: require('./converters/sigfox')(payloadDefinition, req.body.data)
};
```

Von Sigfox-Geräten wird der Antwortcode `204` erwartet. Fügen Sie nach dem Aufruf von `handleMessage` in Zeile 21 den folgenden Code hinzu:

```javascript
context.res = {
    status: 204
};
```

### <a name="example-3-connecting-devices-from-the-things-network-through-the-device-bridge"></a>Beispiel 3: Verbinden von The Things Network-Geräten über die Geräte-Bridge

So verbinden Sie The Things Network-Geräte mit IoT Central:

* Fügen Sie Ihrer Anwendung in The Things Network eine neue HTTP-Integration hinzu: **Application > Integrations > add integration > HTTP Integration** („Anwendung“ > „Integrationen“ > „Integration hinzufügen“ > „HTTP-Integration“).
* Achten Sie darauf, dass Ihre Anwendung eine Decoderfunktion enthält, durch die die Nutzdaten Ihrer Gerätenachrichten vor dem Senden an die Azure-Funktion automatisch in JSON konvertiert werden: **Application > Payload Functions > decoder** („Anwendung“ > „Nutzdatenfunktionen“ > „Decoder“).

Im folgenden Beispiel wird eine JavaScript-Decoderfunktion verwendet, mit der gängige numerische Typen aus Binärdaten decodiert werden können:

```javascript
function Decoder(bytes, port) {
  function bytesToFloat(bytes, decimalPlaces) {
    var bits = (bytes[3] << 24) | (bytes[2] << 16) | (bytes[1] << 8) | bytes[0];
    var sign = (bits >>> 31 === 0) ? 1.0 : -1.0;
    var e = bits >>> 23 & 0xff;
    var m = (e === 0) ? (bits & 0x7fffff) << 1 : (bits & 0x7fffff) | 0x800000;
    var f = Math.round((sign * m * Math.pow(2, e - 150)) * Math.pow(10, decimalPlaces)) / Math.pow(10, decimalPlaces);
    return f;
  }

  function bytesToInt32(bytes, signed) {
    var bits = bytes[0] | (bytes[1] << 8) | (bytes[2] << 16) | (bytes[3] << 24);
    var sign = 1;

    if (signed && bits >>> 31 === 1) {
      sign = -1;
      bits = bits & 0x7FFFFFFF;
    }

    return bits * sign;
  }

  function bytesToShort(bytes, signed) {
    var bits = bytes[0] | (bytes[1] << 8);
    var sign = 1;

    if (signed && bits >>> 15 === 1) {
      sign = -1;
      bits = bits & 0x7FFF;
    }

    return bits * sign;
  }

  return {
    temperature: bytesToFloat(bytes.slice(0, 4), 2),
    presscounter: bytesToInt32(bytes.slice(4, 8), true),
    blueLux: bytesToShort(bytes.slice(8, 10), false)
  };
}
```

Fügen Sie nach dem Definieren der Integration vor dem Aufruf von `handleMessage` in Zeile 21 der Datei *IoTCIntegration/index.js* Ihrer Azure-Funktions-App den folgenden Code hinzu. Durch diesen Code wird der Text Ihrer HTTP-Integration in das erwartete Format konvertiert.

```javascript
device: {
    deviceId: req.body.hardware_serial.toLowerCase()
},
measurements: req.body.payload_fields
};
```

## <a name="limitations"></a>Einschränkungen

Von der Geräte-Bridge werden nur Nachrichten an IoT Central weitergeleitet. Es werden jedoch keine Nachrichten an die Geräte zurückgesendet. Aus diesem Grund können für Geräte, die über diese Geräte-Bridge mit IoT Central verbunden werden, keine Eigenschaften und Befehle verwendet werden. Da keine Gerätezwillingsvorgänge unterstützt werden, ist es nicht möglich, Geräteeigenschaften über die Geräte-Bridge zu aktualisieren. Um diese Features verwenden zu können, muss ein Gerät mithilfe eines der [Azure IoT-Geräte-SDKs](../../iot-hub/iot-hub-devguide-sdks.md) direkt mit Azure IoT verbunden werden.

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie sich hier mit der Bereitstellung der IoT Central-Geräte-Bridge vertraut gemacht haben, können Sie mit dem folgenden Schritt fortfahren:

> [!div class="nextstepaction"]
> [Verwalten von Geräten](howto-manage-devices.md)
