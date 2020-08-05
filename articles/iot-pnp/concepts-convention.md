---
title: IoT Plug & Play-Konventionen | Microsoft-Dokumentation
description: Beschreibung der Konventionen, bei denen IoT Plug & Play erwartet, dass sie von Geräten verwendet werden, wenn sie Telemetriedaten und Eigenschaften senden und Befehle und Aktualisierungen von Eigenschaften durchführen.
author: rido-min
ms.author: rmpablos
ms.date: 07/10/2020
ms.topic: conceptual
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: b06edb83e85ef6d56cf8bf8f91551fe99535eba1
ms.sourcegitcommit: 46f8457ccb224eb000799ec81ed5b3ea93a6f06f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/28/2020
ms.locfileid: "87351999"
---
# <a name="iot-plug-and-play-conventions"></a>IoT Plug & Play-Konventionen

Geräte in IoT Plug & Play (Vorschau) müssen sich beim Austauschen von Nachrichten mit einem IoT-Hub an eine Reihe von Konventionen halten. Geräte in IoT Plug & Play (Vorschau) verwenden für die Kommunikation mit IoT Hub das MQTT-Protokoll.

Die Telemetriedaten, Eigenschaften und Befehle, die ein Gerät in IoT Plug & Play implementiert, werden mit einem [Digital Twins Definition Language v2 (DTDL)](https://github.com/Azure/opendigitaltwins-dtdl)-_Modell_ beschrieben. In diesem Artikel werden zwei Modelltypen erläutert:

- **Ohne Komponenten**: Ein Modell ohne Komponenten. Bei diesem Modell werden Telemetriedaten, Eigenschaften und Befehle im Inhaltsabschnitt der Hauptschnittstelle als Eigenschaften der obersten Ebene deklariert.
- **Mit mehreren Komponenten**: Ein Modell, das aus mindestens zwei Schnittstellen besteht. Aus einer Hauptschnittstelle mit Telemetriedaten, Eigenschaften und Befehlen. Und aus mindestens einer Schnittstelle, die als Komponente mit zusätzlichen Telemetriedaten, Eigenschaften und Befehlen deklariert wird.

Weitere Informationen finden Sie unter [IoT Plug & Play-Komponenten in Modellen](concepts-components.md).

## <a name="model-discovery"></a>Modellermittlung

Zur Ankündigung des Modells, das vom IoT Plug & Play-Gerät implementiert wird, wird die Modell-ID in das MQTT-Verbindungspaket durch Hinzufügen von `model-id` zum `USERNAME`-Feld eingebunden.

Um das Modell zu ermitteln, das von einem Gerät implementiert wird, kann ein Dienst die Modell-ID an folgender Stelle abrufen:

- Vom `modelId`-Feld des Gerätezwillings
- Vom `$metadata.$model`-Feld des digitalen Zwillings
- Aus einer Änderungsbenachrichtigung bei einem digitalen Zwilling

## <a name="telemetry"></a>Telemetrie

Telemetriedaten, die von einem Gerät ohne Komponenten gesendet werden, erfordern keine zusätzlichen Metadaten. Vom System wird die `dt-dataschema`-Eigenschaft hinzugefügt.

Telemetriedaten, die von einem Gerät mit mehreren Komponenten gesendet werden, müssen zusätzlich `$.sub` als Nachrichteneigenschaft enthalten. Vom System werden die Eigenschaften `dt-subject` und `dt-dataschema` hinzugefügt.

## <a name="read-only-properties"></a>Schreibgeschützte Eigenschaften

### <a name="sample-no-component-read-only-property"></a>Beispiel für eine schreibgeschützte Eigenschaft eines Geräts ohne Komponenten

Ein Gerät kann eine gültige JSON-Nachricht senden, die den DTDL v2-Regeln entspricht.

:::row:::
   :::column span="":::
      **DTDL**

      ```json
      "@context": "dtmi:dtdl:context;2",
      "@id": "dtmi:example: Thermostat;1",
      "@type": "Interface",
      "contents": [
        {
          "@type": "Property",
          "name": "temperature",
          "schema": "double"
        }
      ]
      ```
   :::column-end:::
   :::column span="":::
      **Beispielnutzlast**

      ```json
      "reported" :
      {
        "temperature" : 21.3
      }
      ```
   :::column-end:::
:::row-end:::

### <a name="sample-multiple-components-read-only-property"></a>Beispiel für eine schreibgeschützte Eigenschaft eines Geräts mit mehreren Komponenten

Das Gerät muss zusätzlich den `{"__t": "c"}`-Marker aufweisen, um anzugeben, dass das Element auf eine Komponente verweist.

:::row:::
   :::column span="":::
      **DTDL**

      ```json
      "@context": "dtmi:dtdl:context;2",
      "@id": "dtmi:com:example:TemperatureController;1",
      "@type": "Interface",
      "displayName": "Temperature Controller",
      "contents": [
        {
          "@type" : "Component",
          "schema": "dtmi:com:example:Thermostat;1",
          "name": "thermostat1"
        }
      ]

      "@context": "dtmi:dtdl:context;2",
      "@id": "dtmi:com:example:Thermostat;1",
      "@type": "Interface",
      "contents": [
        {
          "@type": "Property",
          "name": "temperature",
          "schema": "double"
        }
      ]
      ```
   :::column-end:::
   :::column span="":::
      **Gemeldete Eigenschaft**

      ```json
      "reported": {
        "thermostat1": {
          "__t": "c",
          "temperature": 21.3
        }
      }
      ```
   :::column-end:::
:::row-end:::

## <a name="writable-properties"></a>Schreibbare Eigenschaften

Das Gerät muss bestätigen, dass es die Eigenschaft erhalten hat, indem es eine gemeldete Eigenschaft sendet. Die gemeldete Eigenschaft muss Folgendes enthalten:

- `value`: der vom Gerät empfangene Wert
- `ac`: einen Bestätigungscode, der einen HTTP-Statuscode enthält
- `av`: eine Bestätigungsversion, die auf die `$version` der gewünschten Eigenschaft verweist.
- `ad`: eine optionale Beschreibung der Bestätigung

### <a name="sample-no-component-writable-property"></a>Beispiel für eine schreibbare Eigenschaft eines Geräts ohne Komponenten

Ein Gerät kann eine gültige JSON-Nachricht senden, die den DTDL v2-Regeln entspricht:

:::row:::
   :::column span="":::
      **DTDL**

      ```json
      "@context": "dtmi:dtdl:context;2",
      "@id": "dtmi:example: Thermostat;1",
      "@type": "Interface",
      "contents": [
        {
          "@type": "Property",
          "name": "targetTemperature",
          "schema": "double",
          "writable": true
        }
      ]
      ```
   :::column-end:::
   :::column span="":::
      **Gewünschte Eigenschaft**

      ```json
      "desired" :
      {
        "targetTemperature" : 21.3
      },
      "$version" : 3
      ```
   :::column-end:::
   :::column span="":::
      **Gemeldete Eigenschaft**

      ```json
      "reported": {
        "targetTemperature": {
          "value": 21.3,
          "ac": 200,
          "av": 3,
          "ad": "complete"
       }
     }
      ```
   :::column-end:::
:::row-end:::

### <a name="sample-multiple-components-writable-property"></a>Beispiel für eine schreibbare Eigenschaft eines Geräts mit mehreren Komponenten

Das Gerät muss zusätzlich den `{"__t": "c"}`-Marker aufweisen, um anzugeben, dass das Element auf eine Komponente verweist.

Der Marker wird nur für Aktualisierungen auf Komponentenebene gesendet. Daher müssen Geräte nicht überprüfen, ob dieses Flag vorhanden ist.

Das Gerät muss bestätigen, dass es die Eigenschaft erhalten hat, indem es eine gemeldete Eigenschaft sendet:

:::row:::
   :::column span="":::
      **DTDL**

      ```json
      "@context": "dtmi:dtdl:context;2",
      "@id": "dtmi:com:example:TemperatureController;1",
      "@type": "Interface",
      "displayName": "Temperature Controller",
      "contents": [
        {
          "@type" : "Component",
          "schema": "dtmi:com:example:Thermostat;1",
          "name": "thermostat1"
        }
      ]

      "@context": "dtmi:dtdl:context;2",
      "@id": "dtmi:com:example:Thermostat;1",
      "@type": "Interface",
      "contents": [
        {
          "@type": "Property",
          "name": "targetTemperature",
          "schema": "double",
          "writable": true
        }
      ]
      ```
   :::column-end:::
   :::column span="":::
      **Gewünschte Eigenschaft**

      ```json
      "desired": {
        "thermostat1": {
          "__t": "c",
          "targetTemperature": 21.3
        }
      },
      "$version" : 3
      ```
   :::column-end:::
   :::column span="":::
      **Gemeldete Eigenschaft**

      ```json
      "reported": {
        "thermostat1": {
          "__t": "c",
          "targetTemperature": {
            "value": 23,
            "ac": 200,
            "av": 3,
            "ad": "complete"
          }
        }
      }
      ```
   :::column-end:::
:::row-end:::

## <a name="commands"></a>Befehle

Bei Schnittstellen ohne Komponenten wird der Befehlsname ohne Präfix verwendet.

Bei einem Gerät werden bei Schnittstellen mit mehreren Komponenten Befehlsnamen im folgenden Format verwendet: `componentName*commandName`.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich mit IoT Plug & Play-Konventionen vertraut gemacht haben, finden Sie weitere Informationen in folgenden Artikeln:

- [Digital Twins Definition Language (DTDL)](https://github.com/Azure/opendigitaltwins-dtdl)
- [C-Geräte-SDK](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/)
- [IoT-REST-API](https://docs.microsoft.com/rest/api/iothub/device)
- [Modellkomponenten](./concepts-components.md)
