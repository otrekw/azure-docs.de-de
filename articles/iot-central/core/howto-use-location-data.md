---
title: Verwenden von Standortdaten in einer Azure IoT Central-Lösung
description: Hier erfahren Sie, wie Sie Standortdaten verwenden, die von einem mit Ihrer IoT Central Anwendung verbundenen Gerät gesendet werden. Zeichnen Sie Standortdaten auf einer Karte, oder erstellen Sie Geofencingregeln.
author: dominicbetts
ms.author: dobett
ms.date: 01/08/2021
ms.topic: how-to
ms.service: iot-central
services: iot-central
ms.openlocfilehash: c909fd1438488e3625f3674dd26f959cf6fad79f
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "98127973"
---
# <a name="use-location-data-in-an-azure-iot-central-solution"></a>Verwenden von Standortdaten in einer Azure IoT Central-Lösung

In diesem Artikel wird gezeigt, wie Sie Standortdaten in einer IoT Central-Anwendung verwenden. Ein mit IoT Central verbundenes Gerät kann Standortdaten als Telemetriedatenstrom senden oder sie mithilfe einer Geräteeigenschaft melden.

Ein Lösungsentwickler kann die Standortdaten für Folgendes verwenden:

* Zeichnen des gemeldeten Standorts auf einer Karte.
* Zeichnen des Telemetrie-Standortverlaufs in einer Karte.
* Erstellen von Geofencingregeln zum Benachrichtigen eines Operators, wenn ein Gerät in einen bestimmten Bereich kommt oder ihn verlässt.

## <a name="add-location-capabilities-to-a-device-template"></a>Hinzufügen von Standortfunktionen zu einer Gerätevorlage

Der folgende Screenshot zeigt eine Gerätevorlage mit Beispielen für eine Geräteeigenschaft und einen Telemetrietyp, die Standortdaten verwenden. Die Definitionen verwenden den semantischen Typ **location** und den Schematyp **geolocation**:

:::image type="content" source="media/howto-use-location-data/location-device-template.png" alt-text="Screenshot der Definition für Standorteigenschaften in der Gerätevorlage":::

Zur Referenz: Die [Digital Twins Definition Language (DTDL)](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/dtdlv2.md)-Definitionen für diese Funktionen sehen wie der folgende Codeausschnitt aus:

```json
{
  "@type": [
    "Property",
    "Location"
  ],
  "displayName": {
    "en": "DeviceLocation"
  },
  "name": "DeviceLocation",
  "schema": "geopoint",
  "writable": false
},
{
  "@type": [
    "Telemetry",
    "Location"
  ],
  "displayName": {
    "en": "Tracking"
  },
  "name": "Tracking",
  "schema": "geopoint"
}
```

> [!NOTE]
> Der Schematyp **geopoint** gehört nicht zur [DTDL-Spezifikation](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/dtdlv2.md). IoT Central unterstützt zurzeit den Schematyp **geopoint** und den semantischen Typ **location** aus Gründen der Abwärtskompatibilität.

## <a name="send-location-data-from-a-device"></a>Senden von Standortdaten von einem Gerät

Wenn ein Gerät Daten für die Eigenschaft **DeviceLocation** sendet, die im vorhergehenden Abschnitt gezeigt wurde, sehen die Nutzdaten wie der folgende JSON-Codeausschnitt aus:

```json
{
  "DeviceLocation": {
    "lat": 47.64263,
    "lon": -122.13035,
    "alt": 0
  }
}
```

Wenn ein Gerät Daten für die Telemetrie **Tracking** sendet, die im vorhergehenden Abschnitt gezeigt wurde, sehen die Nutzdaten wie der folgende JSON-Codeausschnitt aus:

```json
{
  "Tracking": {
    "lat": 47.64263,
    "lon": -122.13035,
    "alt": 0
  }
}
```

## <a name="display-device-location"></a>Anzeigen des Gerätestandorts

Sie können Standortdaten an mehreren Stellen in Ihrer IoT Central-Anwendung anzeigen. Beispiel: In Ansichten, die einem einzelnen Gerät zugeordnet sind, oder auf Dashboards.

Beim Erstellen einer Ansicht für ein Gerät können Sie auswählen, ob die Position in einer Karte gezeichnet oder die einzelnen Werte angezeigt werden soll(en):

:::image type="content" source="media/howto-use-location-data/location-views.png" alt-text="Screenshot einer Beispielansicht mit Standortdaten":::

Sie können einem Dashboard Kartenkacheln hinzufügen, um den Standort von einem oder mehreren Gerät(en) zu zeichnen. Wenn Sie eine Kartenkachel zum Anzeigen von Standorttelemetrie hinzufügen, können Sie den Standort über einen bestimmten Zeitraum darstellen. Der folgende Screenshot zeigt den Standort, der während der letzten 30 Minuten von einem simulierten Gerät gemeldet wurde:

:::image type="content" source="media/howto-use-location-data/location-dashboard.png" alt-text="Screenshot eines Beispieldashboards mit Standortdaten":::

## <a name="create-a-geofencing-rule"></a>Erstellen einer Geofencingregel

Sie können mithilfe von Standorttelemetrie eine Geofencingregel erstellen, die eine Warnung generiert, wenn ein Gerät in einen oder aus einem rechteckigen Bereich bewegt wird. Der folgende Screenshot zeigt eine Regel, die mit vier Bedingungen anhand von Breiten- und Längengradwerten einen rechteckigen Bereich definiert. Die Regel generiert eine E-Mail, wenn das Gerät in den rechteckigen Bereich bewegt wird:

:::image type="content" source="media/howto-use-location-data/geofence-rule.png" alt-text="Screenshot einer Definition für eine Geofencingregel":::

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie erfahren haben, wie Sie Eigenschaften in Ihrer Azure IoT Central-Anwendung verwenden können, lesen Sie jetzt:

* [Payloads](concepts-telemetry-properties-commands.md)
* [Tutorial: Erstellen einer Node.js-Clientanwendung und Verbinden der Anwendung mit Ihrer Azure IoT Central-Anwendung (Node.js)](tutorial-connect-device.md)