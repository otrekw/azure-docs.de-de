---
title: Verstehen der Komponenten in IoT Plug & Play-Modellen | Microsoft-Dokumentation
description: Verstehen Sie den Unterschied zwischen IoT Plug & Play-DTDL-Modellen, die Komponenten verwenden, und Modellen, die keine Komponenten verwenden.
author: ericmitt
ms.author: ericmitt
ms.date: 07/07/2020
ms.topic: conceptual
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: eef8179567d83e3727c3ab949eef2706ce2a9b16
ms.sourcegitcommit: 24a12d4692c4a4c97f6e31a5fbda971695c4cd68
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2021
ms.locfileid: "102175803"
---
# <a name="iot-plug-and-play-components-in-models"></a>IoT Plug & Play-Komponenten in Modellen

Laut den IoT Plug & Play-Konventionen ist ein Gerät ein IoT Plug & Play-Gerät, wenn es beim Herstellen einer Verbindung mit einem IoT-Hub seine DTDL-Modell-ID (Digital Twins Definition Language) angibt.

Der folgende Codeausschnitt zeigt Beispiele für Modell-IDs:

```json
 "@id": "dtmi:com:example:TemperatureController;1"
 "@id": "dtmi:com:example:Thermostat;1",
```

## <a name="no-components"></a>Keine Komponenten

Ein einfaches Modell verwendet keine eingebetteten oder weitergegebenen Komponenten. Es enthält Headerinformationen und einen Inhaltsabschnitt zum Definieren von Telemetriedaten, Eigenschaften und Befehlen.

Das folgende Beispiel zeigt einen Teil eines einfachen Modells, das keine Komponenten verwendet:

```json
{
  "@context": "dtmi:dtdl:context;2",
  "@id": "dtmi:com:example:Thermostat;1",
  "@type": "Interface",
  "displayName": "Thermostat",
  "description": "Reports current temperature and provides desired temperature control.",
  "contents": [
    {
      "@type": [
        "Telemetry",
        "Temperature"
      ],
      "name": "temperature",
      "displayName": "Temperature",
      "description": "Temperature in degrees Celsius.",
      "schema": "double",
      "unit": "degreeCelsius"
    },
    {
      "@type": [
        "Property",
...
```

Obwohl das Modell nicht explizit eine Komponente definiert, verhält es sich so, als gäbe es eine einzelne _Standardkomponente_ mit allen Telemetrie-, Eigenschafts- und Befehlsdefinitionen.

Der folgende Screenshot zeigt, wie das Modell im Azure IoT-Explorertool angezeigt wird:

:::image type="content" source="media/concepts-components/default-component.png" alt-text="Standardkomponente im Azure IoT-Explorer":::

Die Modell-ID wird in einer Eigenschaft für Gerätezwillinge gespeichert, wie der folgenden Screenshot zeigt:

:::image type="content" source="media/concepts-components/twin-model-id.png" alt-text="Modell-ID in Eigenschaft für digitalen Zwilling":::

Ein DTDL-Modell ohne Komponenten ist eine nützliche Vereinfachung für Geräte oder IoT Edge-Module mit nur einer Gruppe von Telemetriedaten, Eigenschaften und Befehlen. Mit einem Modell, das keine Komponenten verwendet, können Sie aus einem vorhandenen Gerät oder Modul ganz einfach ein IoT Plug & Play-Gerät oder -Modul machen. Sie erstellen ein DTDL-Modell, das Ihr tatsächliches Gerät oder Modul beschreibt, ohne dass dabei Komponenten definiert werden müssen.

> [!TIP]
> Ein Modul kann ein [Gerätemodul](../iot-hub/iot-hub-devguide-module-twins.md) oder ein [IoT Edge-Modul](../iot-edge/about-iot-edge.md) sein.

## <a name="multiple-components"></a>Mehrere Komponenten

Mithilfe von Komponenten können Sie eine Modellschnittstelle als eine Assembly aus anderen Schnittstellen erstellen.

Die [Thermostatschnittstelle](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/samples/Thermostat.json) ist zum Beispiel als Modell definiert. Sie können diese Schnittstelle als eine oder mehrere Komponenten einbinden, wenn Sie das [Temperature Controller-Modell](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/samples/TemperatureController.json) definieren. Im folgenden Beispiel werden diese Komponenten als `thermostat1` und `thermostat2` bezeichnet.

Ein DTDL-Modell mit mehreren Komponenten verfügt über mindestens zwei Komponentenabschnitte. In jedem Abschnitt ist `@type` auf `Component` festgelegt und ein expliziter Verweis auf ein Schema enthalten, wie im folgenden Codeausschnitt zu sehen:

```json
{
  "@context": "dtmi:dtdl:context;2",
  "@id": "dtmi:com:example:TemperatureController;1",
  "@type": "Interface",
  "displayName": "Temperature Controller",
  "description": "Device with two thermostats and remote reboot.",
  "contents": [
...
    {
      "@type" : "Component",
      "schema": "dtmi:com:example:Thermostat;1",
      "name": "thermostat1",
      "displayName": "Thermostat One",
      "description": "Thermostat One of Two."
    },
    {
      "@type" : "Component",
      "schema": "dtmi:com:example:Thermostat;1",
      "name": "thermostat2",
      "displayName": "Thermostat Two",
      "description": "Thermostat Two of Two."
    },
    {
      "@type": "Component",
      "schema": "dtmi:azure:DeviceManagement:DeviceInformation;1",
      "name": "deviceInformation",
      "displayName": "Device Information interface",
      "description": "Optional interface with basic device hardware information."
    }
...
```

Für dieses Modell sind im Inhaltsabschnitt drei Komponenten definiert: zwei `Thermostat`-Komponenten und eine `DeviceInformation`-Komponente. Darüber hinaus enthält es eine Standardkomponente.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich nun mit Modellkomponenten vertraut gemacht haben, finden Sie weitere Informationen in den folgenden Artikeln:

- [Installieren und Verwenden der DTDL-Erstellungstools](howto-use-dtdl-authoring-tools.md)
- [Digital Twins Definition Language](https://github.com/Azure/opendigitaltwins-dtdl)
- [Azure IoT-Modellrepository](./concepts-model-repository.md)
