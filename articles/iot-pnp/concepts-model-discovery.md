---
title: Verwenden von IoT Plug & Play-Modellen in einer Lösung | Microsoft-Dokumentation
description: Erfahren Sie als Lösungsentwickler, wie Sie IoT Plug & Play-Modelle in Ihrer IoT-Lösung verwenden.
author: arunmannengal
ms.author: arunmann
ms.date: 07/23/2020
ms.topic: conceptual
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: 4cdd6f63c9e5e717a533b88702b2886387fe3e39
ms.sourcegitcommit: 5f7b75e32222fe20ac68a053d141a0adbd16b347
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2020
ms.locfileid: "87475242"
---
# <a name="use-iot-plug-and-play-models-in-an-iot-solution"></a>Verwenden von IoT Plug & Play-Modellen in einer IoT-Lösung

In diesem Artikel wird beschrieben, wie Sie in einer IoT-Lösung die Modell-ID eines IoT Plug & Play Geräts identifizieren und dann seine Modelldefinition abrufen.

IoT-Lösungen können in zwei Hauptkategorien unterteilt werden:

- Eine *zweckgebundene Lösung* funktioniert mit einem bekannten Satz von Modellen für die IoT Plug & Play Geräte, die eine Verbindung mit der Lösung herstellen. Diese Modelle werden bei der Entwicklung der Lösung verwendet.

- Eine *modellgesteuerte IoT-Lösung* funktioniert mit dem Modell jedes IoT Plug & Play-Geräts. Die Entwicklung einer modellgesteuerten Lösung ist komplexer. Der Vorteil ist jedoch, dass Ihre Lösung mit allen zukünftig hinzugefügten Geräten funktioniert. Eine modellgesteuerte IoT-Lösung ruft ein Modell ab und ermittelt mit ihm die vom Gerät implementierten Telemetriedaten, Eigenschaften und Befehle.

Zum Verwenden eines IoT Plug & Play-Modells geht eine IoT-Lösung folgendermaßen vor:

1. Sie identifiziert die Modell-ID des Modells, das von dem mit der Lösung verbundenen IoT Plug & Play-Gerät implementiert wird.

1. Sie verwendet die Modell-ID zum Abrufen der Modelldefinition des verbundenen Geräts aus einem Modellrepository oder einem benutzerdefinierten Speicher.

## <a name="identify-model-id"></a>Identifizieren der Modell-ID

Wenn ein IoT Plug & Play-Gerät eine Verbindung mit IoT-Hub herstellt, registriert es die Modell-ID des von ihm implementierten Modells bei IoT Hub.

IoT Hub benachrichtigt die Lösung im Rahmen des Geräteverbindungsflows mit der Gerätemodell-ID.

Eine Lösung kann die Modell-ID des IoT Plug & Play-Geräts mithilfe einer der folgenden drei Methoden erhalten:

### <a name="get-device-twin-api"></a>API zum Abrufen von Gerätezwillingen

Die Lösung kann die API zum [Abrufen von Gerätezwillingen](https://docs.microsoft.com/rest/api/iothub/service/twin/getdevicetwin) verwenden, um die Modell-ID des IoT Plug & Play-Geräts abzurufen.

Im folgenden Antwortausschnitt eines Gerätezwillings enthält `modelId` die Modell-ID eines IoT Plug & Play-Geräts:

```json
{
    "deviceId": "sample-device",
    "etag": "AAAAAAAAAAc=",
    "deviceEtag": "NTk0ODUyODgx",
    "status": "enabled",
    "statusUpdateTime": "0001-01-01T00:00:00Z",
    "connectionState": "Disconnected",
    "lastActivityTime": "2020-07-17T06:12:26.8402249Z",
    "cloudToDeviceMessageCount": 0,
    "authenticationType": "sas",
    "x509Thumbprint": {
        "primaryThumbprint": null,
        "secondaryThumbprint": null
    },
    "modelId": "dtmi:com:example:TemperatureController;1",
    "version": 15,
    "properties": {...}
    }
}
```

### <a name="get-digital-twin-api"></a>API zum Abrufen von digitalen Zwillingen

Die Lösung kann die API zum [Abrufen von digitalen Zwillingen](https://docs.microsoft.com/rest/api/iothub/service/digitaltwin/getdigitaltwin) verwenden, um die Modell-ID des Modells abzurufen, das vom IoT Plug & Play-Gerät implementiert wurde.

Im folgenden Antwortausschnitt eines digitalen Zwillings enthält `$metadata.$model` die Modell-ID eines IoT Plug & Play-Geräts:

```json
{
    "$dtId": "sample-device",
    "$metadata": {
        "$model": "dtmi:com:example:TemperatureController;1",
        "serialNumber": {
            "lastUpdateTime": "2020-07-17T06:10:31.9609233Z"
        }
    }
}
```

### <a name="digital-twin-change-event-notification"></a>Benachrichtigung über Änderungsereignis für einen digitalen Zwilling

Eine Geräteverbindung führt zu einer Benachrichtigung über ein [Änderungsereignis für einen digitalen Zwilling](concepts-digital-twin.md#digital-twin-change-events). Eine Lösung muss diese Ereignisbenachrichtigungen abonnieren. Informationen zum Aktivieren des Routings für Ereignisse bei digitalen Zwillingen finden Sie unter [Verwenden des IoT Hub-Nachrichtenroutings zum Senden von D2C-Nachrichten an verschiedene Endpunkte](../iot-hub/iot-hub-devguide-messages-d2c.md#non-telemetry-events).

Die Lösung kann das im folgenden Codeausschnitt gezeigte Ereignis verwenden, um mehr über das IoT Plug & Play-Gerät zu erfahren, das eine Verbindung herstellt, und seine Modell-ID abzurufen:

```json
iothub-connection-device-id:sample-device
iothub-enqueuedtime:7/22/2020 8:02:27 PM
iothub-message-source:digitalTwinChangeEvents
correlation-id:100f322dc2c5
content-type:application/json-patch+json
content-encoding:utf-8
[
  {
    "op": "replace",
    "path": "/$metadata/$model",
    "value": "dtmi:com:example:TemperatureController;1"
  }
]
```

## <a name="retrieve-a-model-definition"></a>Abrufen einer Modelldefinition

Eine Lösung verwendet die oben identifizierte Modell-ID, um die zugehörige Modelldefinition abzurufen.

Eine Lösung kann die Modelldefinition mithilfe einer der folgenden Optionen erhalten:

### <a name="model-repository"></a>Modellrepository

Lösungen können das [Modellrepository](concepts-model-repository.md) zum Abrufen von Modellen verwenden. Die Gerätegeneratoren oder die Lösungsgeneratoren müssen ihre Modelle vorab in das Repository hochladen, damit die Lösung sie abrufen kann.

Nachdem Sie die Modell-ID für eine neue Geräteverbindung identifiziert haben, führen Sie die folgenden Schritte aus:

1. Rufen Sie die Modelldefinition mithilfe der Modell-ID aus dem Modellrepository ab. Weitere Informationen finden Sie unter [Abrufen von Modellen](https://docs.microsoft.com/rest/api/iothub/digitaltwinmodelrepositoryservice/getmodelasync/getmodelasync).

1. Mithilfe der Modelldefinition des verbundenen Geräts können Sie die Funktionen des Geräts auflisten.

1. Mithilfe der aufgelisteten Funktionen des Geräts können Sie Benutzern gestatten, [mit dem Gerät zu interagieren](quickstart-service-node.md).

### <a name="custom-store"></a>Benutzerdefinierter Speicher

Lösungen können diese Modelldefinitionen in einem lokalen Dateisystem oder in einem öffentlichen Dateispeicher speichern oder eine benutzerdefinierte Implementierung nutzen.

Nachdem Sie die Modell-ID für eine neue Geräteverbindung identifiziert haben, führen Sie die folgenden Schritte aus:

1. Rufen Sie die Modelldefinition mithilfe der Modell-ID aus Ihrem benutzerdefinierten Speicher ab.

1. Mithilfe der Modelldefinition des verbundenen Geräts können Sie die Funktionen des Geräts auflisten. 

1. Mithilfe der aufgelisteten Funktionen des Geräts können Sie Benutzern gestatten, [mit dem Gerät zu interagieren](quickstart-service-node.md).  

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie gelernt haben, wie IoT Plug & Play-Modelle in eine IoT-Lösung integriert werden, finden Sie hier einige der empfohlenen nächsten Schritte:

- [Interagieren mit einem Gerät über Ihre Lösung](quickstart-service-node.md)
- [IoT-REST-API für digitale Zwillinge](https://docs.microsoft.com/rest/api/iothub/service/digitaltwin)
- [Azure IoT-Explorer](howto-use-iot-explorer.md)
