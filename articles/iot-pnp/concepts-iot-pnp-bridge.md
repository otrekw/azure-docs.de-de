---
title: IoT Plug & Play-Bridge | Microsoft-Dokumentation
description: Verstehen Sie die Grundlagen der IoT Plug & Play-Bridge und wie Sie sie zum Herstellen von Verbindungen mit vorhandenen Geräten als IoT Plug & Play-Geräte verwenden können, die an ein Windows- oder Linux-Gateway angefügt sind.
author: usivagna
ms.author: ugans
ms.date: 1/20/2021
ms.topic: conceptual
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: 05b9df2c5083d24ae6a78212bdd29cd1c740e1d1
ms.sourcegitcommit: 4d48a54d0a3f772c01171719a9b80ee9c41c0c5d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2021
ms.locfileid: "98746491"
---
# <a name="iot-plug-and-play-bridge"></a>IoT Plug & Play-Brücke

Die IoT Plug & Play-Bridge ist eine Open-Source-Anwendung für das Verbinden von vorhandenen Geräten, die an ein Windows- oder Linux-Gateway angefügt sind, als IoT Plug & Play-Geräte. Nach dem Installieren und Konfigurieren der Anwendung auf Ihrem Windows- oder Linux-Computer können Sie sie verwenden, um angefügte Geräte mit einem IoT-Hub zu verbinden. Sie können die Bridge verwenden, um IoT Plug & Play-Schnittstellen den von den angefügten Geräten gesendeten Telemetriedaten zuzuordnen, mit ihren Geräteeigenschaften zu arbeiten und Befehle aufzurufen.

:::image type="content" source="media/concepts-iot-pnp-bridge/iot-pnp-bridge-high-level.png" alt-text="Auf der linken Seite ist eine Reihe vorhandener Sensoren (sowohl drahtgebundener als auch drahtloser) an einen Windows- oder Linux-PC angeschlossen, der eine IoT Plug & Play-Bridge enthält. Die IoT Plug & Play-Bridge stellt ihrerseits die Verbindung mit einem IoT-Hub auf der rechten Seite her":::

Die IoT Plug & Play-Bridge kann als eigenständige ausführbare Datei auf einem beliebigen IoT-Gerät, einem Industrie-PC, einem Server oder einem Gateway bereitgestellt werden, das bzw. der Windows 10 oder Linux ausführt. Ferner kann sie in Ihren Anwendungscode kompiliert werden. Eine einfache JSON-Konfigurationsdatei teilt der IoT Plug & Play-Bridge mit, welche angeschlossenen Geräte/Peripheriegeräte für Azure verfügbar gemacht werden sollen.

## <a name="supported-protocols-and-sensors"></a>Unterstützte Protokolle und Sensoren

Die IoT Plug & Play-Bridge unterstützt standardmäßig die folgenden Typen von Peripheriegeräten. Links zur Adapterdokumentation sind angegeben:

|Periperiegerät|Windows|Linux|
|---------|---------|---------|
|Der [Bluetooth-Sensoradapter](https://github.com/Azure/iot-plug-and-play-bridge/blob/master/pnpbridge/docs/bluetooth_sensor_adapter.md) verbindet erkannte BLE-fähige Sensoren (Bluetooth Low Energy).       |Ja|Nein|
|Der [Kameraadapter](https://github.com/Azure/iot-plug-and-play-bridge/blob/master/pnpbridge/docs/camera_adapter.md) verbindet Kameras auf einem Windows 10-Gerät.               |Ja|Nein|
|Der [Modbus-Adapter](https://github.com/Azure/iot-plug-and-play-bridge/blob/master/pnpbridge/docs/modbus_adapters.md) verbindet Sensoren auf einem Modbus-Gerät.              |Ja|Ja|
|Der [MQTT-Adapter](https://github.com/Azure/iot-plug-and-play-bridge/blob/master/pnpbridge/docs/mqtt_adapter.md) verbindet Geräte, die einen MQTT-Broker verwenden.                  |Ja|Ja|
|Der [SerialPnP-Adapter](https://github.com/Azure/iot-plug-and-play-bridge/blob/master/serialpnp/Readme.md) verbindet Geräte, die über eine serielle Verbindung kommunizieren.               |Ja|Ja|
|Das [Windows-USB-Peripheriegerät](https://github.com/Azure/iot-plug-and-play-bridge/blob/master/pnpbridge/docs/coredevicehealth_adapter.md) verwendet eine Liste von Geräteschnittstellenklassen, die vom Adapter unterstützt werden, zum Verbinden von Geräten (mit einer bestimmten Hardware-ID).  |Ja|Nicht zutreffend|

Informationen dazu, wie Sie die IoT Plug & Play-Bridge erweitern können, um zusätzliche Geräteprotokolle zu unterstützen, finden Sie unter [Erweitern der IoT Plug & Play-Bridge](howto-author-pnp-bridge-adapter.md). Informationen zum Erstellen und Bereitstellen der IoT-Plug & Play-Bridge finden Sie unter [Erstellen und Bereitstellen der IoT Plug & Play-Bridge](howto-build-deploy-extend-pnp-bridge.md).

## <a name="iot-plug-and-play-bridge-architecture"></a>Architektur der IoT Plug & Play-Bridge

:::image type="content" source="media/concepts-iot-pnp-bridge/iot-pnp-bridge-components.png" alt-text="Auf der linken Seite gibt es mehrere Felder, die verschiedene an einen Windows- oder Linux-PC mit einer IoT Plug & Play-Bridge angeschlossene Peripheriegeräte anzeigen. Von oben verweist ein Feld mit der Bezeichnung „Konfigurationspunkte“ auf die Bridge. Die Bridge stellt dann eine Verbindung mit einem IoT-Hub auf der rechten Seite des Diagramms her.":::

### <a name="iot-plug-and-play-bridge-adapters"></a>Adapter der IoT Plug & Play-Bridge

IoT Plug & Play-Bridge unterstützt eine Reihe von IoT Plug & Play-Bridge-Adaptern für verschiedene Gerätetypen. Ein *Adaptermanifest* definiert statisch die Adapter für eine Bridge.

Der Bridge-Adapter-Manager verwendet das Manifest zum Identifizieren und Aufrufen von Adapterfunktionen. Der Adapter-Manager ruft die Funktion zum Erstellen nur auf den Bridge-Adaptern auf, die für die in der Konfigurationsdatei aufgeführten Schnittstellenkomponenten erforderlich sind. Eine Adapterinstanz wird für jede IoT Plug & Play-Komponente erstellt.

Ein Bridge-Adapter erstellt ein Schnittstellenhandle für digitale Zwillinge und ruft es ab. Der Adapter bindet mithilfe dieses Handles die Gerätefunktionalität an den digitalen Zwilling.

Anhand der Informationen in der Konfigurationsdatei verwendet der Bridge-Adapter die folgenden Verfahren, um eine vollständige Kommunikation zwischen Geräten und digitalen Zwillingen über die Bridge zu ermöglichen:

- Richtet einen Kommunikationskanal direkt ein.
- Erstellt einen Geräte-Watcher, der darauf wartet, dass ein Kommunikationskanal verfügbar wird.

### <a name="configuration-file"></a>Konfigurationsdatei

Die IoT Plug & Play-Bridge verwendet eine JSON-basierte Konfigurationsdatei, die Folgendes angibt:

- Wie eine Verbindung mit einem IoT-Hub oder einer IoT Central-Anwendung hergestellt wird: Zu den Optionen gehören Verbindungszeichenfolgen, Authentifizierungsparameter oder IoT Hub Device Provisioning Service (DPS).
- Der Speicherort der IoT Plug & Play-Funktionsmodelle, die von der Bridge verwendet werden. Das Modell definiert die Funktionen eines IoT Plug & Play-Geräts, ist statisch und unveränderlich.
- Eine Liste der IoT Plug & Play-Schnittstellenkomponenten und der folgenden Informationen für jede Komponente:
- Die Schnittstellen-ID und der Komponentenname.
- Der Bridge-Adapter, der für die Interaktion mit der Komponente erforderlich ist.
- Geräteinformationen, die der Bridge-Adapter benötigt, um die Kommunikation mit dem Gerät herzustellen. Beispielsweise die Hardware-ID oder spezifische Informationen für einen Adapter, eine Schnittstelle oder ein Protokoll.
- Ein optionaler Bridge-Adapter-Untertyp oder eine Schnittstellenkonfiguration, wenn der Adapter mehrere Kommunikationstypen bei ähnlichen Geräten unterstützt. Das Beispiel zeigt, wie eine Bluetooth-Sensorkomponente konfiguriert werden könnte:

    ```json
    {
      "_comment": "Component BLE sensor",
      "pnp_bridge_component_name": "blesensor1",
      "pnp_bridge_adapter_id": "bluetooth-sensor-pnp-adapter",
      "pnp_bridge_adapter_config": {
        "bluetooth_address": "267541100483311",
        "blesensor_identity" : "Blesensor1"
      }
    }
    ```

- Eine optionale Liste von globalen Parametern für den Bridge-Adapter. So enthält beispielsweise der Bluetooth-Sensor-Bridge-Adapter ein Wörterbuch von unterstützten Konfigurationen. Eine Schnittstellenkomponente, für die der Bluetooth-Sensoradapter erforderlich ist, kann eine dieser Konfigurationen als `blesensor_identity` auswählen:

    ```json
    {
      "pnp_bridge_adapter_global_configs": {
        "bluetooth-sensor-pnp-adapter": {
          "Blesensor1" : {
            "company_id": "0x499",
            "endianness": "big",
            "telemetry_descriptor": [
              {
                "telemetry_name": "humidity",
                "data_parse_type": "uint8",
                "data_offset": 1,
                "conversion_bias": 0,
                "conversion_coefficient": 0.5
              },
              {
                "telemetry_name": "temperature",
                "data_parse_type": "int8",
                "data_offset": 2,
                "conversion_bias": 0,
                "conversion_coefficient": 1.0
              },
              {
                "telemetry_name": "pressure",
                "data_parse_type": "int16",
                "data_offset": 4,
                "conversion_bias": 0,
                "conversion_coefficient": 1.0
              },
              {
                "telemetry_name": "acceleration_x",
                "data_parse_type": "int16",
                "data_offset": 6,
                "conversion_bias": 0,
                "conversion_coefficient": 0.00980665
              },
              {
                "telemetry_name": "acceleration_y",
                "data_parse_type": "int16",
                "data_offset": 8,
                "conversion_bias": 0,
                "conversion_coefficient": 0.00980665
              },
              {
                "telemetry_name": "acceleration_z",
                "data_parse_type": "int16",
                "data_offset": 10,
                "conversion_bias": 0,
                "conversion_coefficient": 0.00980665
              }
            ]
          }
        }
      }
    }
    ```

## <a name="download-iot-plug-and-play-bridge"></a>Herunterladen der IoT Plug & Play-Bridge

Sie können eine vorgefertigte Version der Bridge mit unterstützten Adaptern in [IoT Plug & Play-Bridge – Versionen](https://github.com/Azure/iot-plug-and-play-bridge/releases) herunterladen und die Liste der Ressourcen für die aktuellste Version aufklappen. Laden Sie die neueste Version der Anwendung für Ihr Betriebssystem herunter.

Sie können außerdem den Quellcode von [IoT Plug & Play Bridge auf GitHub](https://github.com/Azure/iot-plug-and-play-bridge) herunterladen und anzeigen.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun einen Überblick über die Architektur einer IoT Plug & Play-Bridge erhalten haben, können Sie in den nächsten Schritten mehr über die folgenden Themen erfahren:

- [Herstellen einer Verbindung zwischen einem IoT Plug & Play-Bridgebeispiel unter Linux oder Windows und IoT Hub](./howto-use-iot-pnp-bridge.md)
- [Erstellen und Bereitstellen der IoT Plug &amp; Play-Bridge](howto-build-deploy-extend-pnp-bridge.md)
- [Erweitern der IoT Plug &amp; Play-Bridge](howto-build-deploy-extend-pnp-bridge.md)
- [IoT Plug & Play-Bridge auf GitHub](https://github.com/Azure/iot-plug-and-play-bridge)
