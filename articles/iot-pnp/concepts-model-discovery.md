---
title: Implementieren der IoT Plug & Play-Vorschaumodellermittlung | Microsoft-Dokumentation
description: Erfahren Sie als Lösungsentwickler, wie Sie die IoT Plug & Play-Modellermittlung in Ihre Lösung implementieren können.
author: prashmo
ms.author: prashmo
ms.date: 07/23/2020
ms.topic: conceptual
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: 364b85a8ead09858b97d5d7e6ca8c130b9960b2c
ms.sourcegitcommit: 46f8457ccb224eb000799ec81ed5b3ea93a6f06f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/28/2020
ms.locfileid: "87337380"
---
# <a name="implement-iot-plug-and-play-preview-model-discovery-in-an-iot-solution"></a>Implementieren der IoT Plug & Play-Vorschaumodellermittlung in eine IoT-Lösung

In diesem Artikel wird beschrieben, wie Sie als Lösungsentwickler die IoT Plug & Play-Vorschaumodellermittlung in eine IoT-Lösung implementieren können. Die Modellermittlung beschreibt Folgendes:

- Wie IoT Plug & Play-Geräte ihre Modell-ID registrieren.
- Wie eine IoT-Lösung die vom Gerät implementierten Schnittstellen abruft.

IoT-Lösungen lassen sich in zwei Hauptkategorien unterteilen:

- Eine *zweckgebundene IoT-Lösung* arbeitet mit einem bekannten Satz von IoT Plug & Play-Gerätemodellen.

- Eine *modellgesteuerte IoT-Lösung* kann mit jedem IoT Plug & Play-Gerät arbeiten. Die Entwicklung einer modellgesteuerten Lösung ist komplexer. Der Vorteil ist jedoch, dass Ihre Lösung mit allen zukünftig hinzugefügten Geräten funktioniert.

    Um eine modellgesteuerte IoT-Lösung zu erstellen, müssen Sie eine Logik für die IoT Plug and Play-Schnittstellenprimitive erstellen: Telemetrie, Eigenschaften und Befehle. Die Logik Ihrer Lösung stellt ein Gerät dar, indem sie mehrere Telemetrie-, Eigenschafts- und Befehlsfunktionen kombiniert.

In diesem Artikel wird beschrieben, wie Sie die Modellermittlung in beiden Lösungstypen implementieren.

## <a name="model-discovery"></a>Modellermittlung

Um das von einem Gerät implementierte Modell zu ermitteln, kann eine Lösung die Modell-ID mithilfe der ereignisbasierten Ermittlung oder der zwillingsbasierten Ermittlung abrufen:

### <a name="event-based-discovery"></a>Ereignisbasierte Ermittlung

Wenn ein IoT Plug & Play-Gerät eine Verbindung mit einem IoT-Hub herstellt, registriert es das von ihm implementierte Modell. Diese Registrierung führt zu einer Benachrichtigung über ein [Änderungsereignis bei einem digitalen Zwilling](concepts-digital-twin.md#digital-twin-change-events). Informationen zum Aktivieren des Routings für Ereignisse bei digitalen Zwillingen finden Sie unter [Verwenden des IoT Hub-Nachrichtenroutings zum Senden von D2C-Nachrichten an verschiedene Endpunkte](../iot-hub/iot-hub-devguide-messages-d2c.md#non-telemetry-events).

Die Lösung kann das im folgenden Codeausschnitt gezeigte Ereignis verwenden, um mehr über das IoT Plug & Play-Gerät zu erfahren, das eine Verbindung herstellt, und seine Modell-ID abrufen:

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

Dieses Ereignis wird ausgelöst, wenn die Gerätemodell-ID hinzugefügt oder aktualisiert wird.

### <a name="twin-based-discovery"></a>Zwillingsbasierte Ermittlung

Wenn die Lösung Informationen über die Funktionen eines bestimmten Geräts erhalten möchte, kann sie die API zum [Abrufen des digitalen Zwillings](https://docs.microsoft.com/rest/api/iothub/service/digitaltwin/getdigitaltwin) verwenden, um diese Informationen abzurufen.

Im folgenden Codeausschnitt für einen digitalen Zwilling enthält `$metadata.$model` die Modell-ID eines IoT Plug & Play-Geräts:

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

Die Lösung kann auch die API zum **Abrufen des Gerätezwillings** verwenden, um die Modell-ID aus dem Gerätezwilling abzurufen, wie es im folgenden Codeausschnitt gezeigt wird:

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

## <a name="model-resolution"></a>Modellauflösung

Eine Lösung verwendet die Modellauflösung, um Zugriff auf die Schnittstellen, aus denen sich ein Modell zusammensetzt, über die Modell-ID zu erhalten. 

- Lösungen können diese Schnittstellen als Dateien in einem lokalen Ordner speichern. 
- Lösungen können das [Modellrepository](concepts-model-repository.md) verwenden.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun mehr über die Modellermittlung einer IoT-Lösung erfahren haben, können Sie sich mit der [Azure IoT-Plattform](overview-iot-plug-and-play.md) vertraut machen, um andere Funktionen für Ihre Lösung zu verwenden.

- [Interagieren mit einem Gerät über Ihre Lösung](quickstart-service-node.md)
- [IoT-REST-API für digitale Zwillinge](https://docs.microsoft.com/rest/api/iothub/service/digitaltwin)
- [Azure IoT-Explorer](howto-use-iot-explorer.md)
