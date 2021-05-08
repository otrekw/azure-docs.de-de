---
title: Ereignisbenachrichtigungen
titleSuffix: Azure Digital Twins
description: Erfahren Sie, wie die verschiedenen Ereignistypen und ihre unterschiedlichen Benachrichtigungsmeldungen zu interpretieren sind.
author: baanders
ms.author: baanders
ms.date: 4/8/2021
ms.topic: conceptual
ms.service: digital-twins
ms.custom: contperf-fy21q4
ms.openlocfilehash: 625f69b0088ffe6931410afc6f2a825c3156fd70
ms.sourcegitcommit: a5dd9799fa93c175b4644c9fe1509e9f97506cc6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108209947"
---
# <a name="event-notifications"></a>Ereignisbenachrichtigungen

Unterschiedliche Ereignisse in Azure Digital Twins erzeugen **Benachrichtigungen**, die es dem Lösungs-Back-End ermöglichen, zu erkennen, wenn verschiedene Aktionen stattfinden. Diese werden dann zu verschiedenen Orten innerhalb und außerhalb von Azure Digital Twins [geleitet](concepts-route-events.md), die diese Informationen zur Ergreifung von Maßnahmen nutzen können.

Es gibt verschiedene Arten von Benachrichtigungen, die generiert werden können, und Benachrichtigungsmeldungen können unterschiedlich aussehen, je nachdem, mit welcher Art von Ereignis sie erzeugt wurden. Dieser Artikel enthält Einzelheiten zu den verschiedenen Arten von Meldungen und wie sie aussehen könnten.

Dieses Diagramm zeigt die verschiedenen Benachrichtigungstypen:

[!INCLUDE [digital-twins-notifications.md](../../includes/digital-twins-notifications.md)]

## <a name="notification-structure"></a>Benachrichtigungsstruktur

Im Allgemeinen bestehen Benachrichtigungen aus zwei Teilen: Header und Hauptteil. 

### <a name="event-notification-headers"></a>Header von Ereignisbenachrichtigungen

Header von Benachrichtigungsmeldungen werden mit Schlüssel-Wert-Paaren dargestellt. Je nach verwendetem Protokoll (MQTT, AMQP oder HTTP) werden die Meldungsheader unterschiedlich serialisiert. Dieser Abschnitt behandelt allgemeine Headerinformationen für Benachrichtigungsmeldungen, unabhängig vom gewählten bestimmten Protokoll und der gewählten Serialisierung.

Einige Benachrichtigungen entsprechen dem [CloudEvents](https://cloudevents.io/)-Standard. Die CloudEvents-Konformität sieht wie folgt aus.
* Von Geräten ausgegebene Benachrichtigungen folgen weiterhin den bestehenden Spezifikationen für Benachrichtigungen.
* Die von IoT Hub verarbeiteten und ausgegebenen Benachrichtigungen folgen weiterhin den bestehenden Spezifikationen für Benachrichtigungen, es sei denn, IoT Hub entscheidet sich für die Unterstützung von CloudEvents, z. B. durch Event Grid.
* Benachrichtigungen, die von [Digital Twins](concepts-twins-graph.md) mit einem [Modell](concepts-models.md) ausgegeben werden, entsprechen CloudEvents.
* Die von Azure Digital Twins verarbeiteten und ausgegebenen Benachrichtigungen entsprechen CloudEvents.

Die Dienste müssen allen Benachrichtigungen eine Sequenznummer hinzufügen, um ihre Reihenfolge anzugeben, oder auf andere Weise ihre eigene Reihenfolge beibehalten. 

Benachrichtigungen, die von Azure Digital Twins an Event Grid gesendet werden, werden automatisch entweder in das CloudEvents-Schema oder das EventGridEvent-Schema formatiert. Dies ist abhängig vom Schematyp, der im Thema „Event Grid“ definiert ist. 

Erweiterungsattribute für Header werden als Eigenschaften im Event Grid-Schema innerhalb der Nutzlast hinzugefügt. 

### <a name="event-notification-bodies"></a>Hauptteile der Ereignisbenachrichtigung

Die Hauptteile der Benachrichtigungsmeldungen werden hier in JSON beschrieben. Je nach gewünschter Serialisierung des Nachrichtentexts (z. B. mit JSON, CBOR, Protobuf usw.) kann der Nachrichtentext unterschiedlich serialisiert werden.

Die Auswahl von Feldern, die der Hauptteil enthält, variiert je nach Benachrichtigungstyp.

In den folgenden Abschnitten wird näher auf die verschiedenen Arten von Benachrichtigungen eingegangen, die von IoT Hub und Azure Digital Twins (oder anderen Azure IoT-Diensten) gesendet werden. Sie werden über die Auslöser für die einzelnen Benachrichtigungstypen und die Auswahl von Feldern lesen, die bei jeder Art von Benachrichtigungshauptteil enthalten sind.

## <a name="digital-twin-change-notifications"></a>Änderungsbenachrichtigungen bei digitalen Zwillingen

**Änderungsbenachrichtigungen bei digitalen Zwillingen** werden ausgelöst, wenn ein digitaler Zwilling aktualisiert wird. Beispiel:
* Wenn sich Eigenschaftswerte oder Metadaten ändern.
* Wenn sich die Metadaten von digitalen Zwillingen oder Komponenten ändern. Ein Beispiel für dieses Szenario ist die Änderung des Modells eines digitalen Zwillings.

### <a name="properties"></a>Eigenschaften

Hier folgen die Felder des Hauptteils einer Änderungsbenachrichtigung für digitale Zwillinge.

| Name    | Wert |
| --- | --- |
| `id` | Bezeichner der Benachrichtigung, z. B. eine UUID oder ein vom Dienst verwalteter Zähler. `source` + `id` ist für jedes einzelne Ereignis eindeutig. |
| `source` | Name der IoT Hub- oder Azure Digital Twins-Instanz, z. B. *myhub.azure-devices.net* oder *mydigitaltwins.westus2.azuredigitaltwins.net*. |
| `data` | Ein JSON-Patchdokument, in dem das Update für den Zwilling beschrieben wird. Weitere Informationen finden Sie weiter unten unter [Details zum Hauptteil](#body-details). |
| `specversion` | *1.0*<br>Die Meldung entspricht dieser Version der [CloudEvents-Spezifikation](https://github.com/cloudevents/spec). |
| `type` | `Microsoft.DigitalTwins.Twin.Update` |
| `datacontenttype` | `application/json` |
| `subject` | ID des digitalen Zwillings. |
| `time` | Zeitstempel für den Zeitpunkt, an dem der Vorgang beim digitalen Zwilling erfolgt ist. |
| `traceparent` | Ein W3C-Ablaufverfolgungskontext für das Ereignis. |

### <a name="body-details"></a>Details zum Hauptteil

Innerhalb der Nachricht enthält das Feld `data` ein JSON-Patchdokument, das das Update des digitalen Zwillings enthält.

Nehmen wir zum Beispiel an, dass ein digitaler Zwilling mit dem folgenden Patch aktualisiert wurde.

:::code language="json" source="~/digital-twins-docs-samples/models/patch-component-2.json":::

Die entsprechende Benachrichtigung (wenn sie vom Dienst synchron ausgeführt wird, z. B. Azure Digital Twins beim Update eines digitalen Zwillings) hätte einen Hauptteil wie den folgenden:

```json
{
    "modelId": "dtmi:example:com:floor4;2",
    "patch": [
      {
        "value": 40,
        "path": "/Temperature",
        "op": "replace"
      },
      {
        "value": 30,
        "path": "/comp1/prop1",
        "op": "add"
      }
    ]
  }
```

Dies sind die Informationen, die im Feld `data` der Benachrichtigungsmeldung zum Lebenszyklus angezeigt werden.

## <a name="digital-twin-lifecycle-notifications"></a>Benachrichtigungen zum Lebenszyklus von digitalen Zwillingen

Alle [digitalen Zwillinge](concepts-twins-graph.md) senden Benachrichtigungen, unabhängig davon, ob sie [IoT Hub-Geräte in Azure Digital Twins](how-to-ingest-iot-hub-data.md) darstellen oder nicht. Das liegt an den **Lebenszyklusbenachrichtigungen**, die den digitalen Zwilling selbst betreffen.

Lebenszyklusbenachrichtigungen werden in folgenden Situationen ausgelöst:
* Ein digitaler Zwilling wird erstellt.
* Ein digitaler Zwilling wird gelöscht.

### <a name="properties"></a>Eigenschaften

Hier werden die Felder des Hauptteils einer Lebenszyklusbenachrichtigung aufgeführt.

| Name | Wert |
| --- | --- |
| `id` | Bezeichner der Benachrichtigung, z. B. eine UUID oder ein vom Dienst verwalteter Zähler. `source` + `id` ist für jedes einzelne Ereignis eindeutig. |
| `source` | Name der IoT Hub- oder Azure Digital Twins-Instanz, z. B. *myhub.azure-devices.net* oder *mydigitaltwins.westus2.azuredigitaltwins.net*. |
| `data` | Die Daten des Zwillings, für den das Lebenszyklusereignis auftritt. Weitere Informationen finden Sie weiter unten unter [Details zum Hauptteil](#body-details-1). |
| `specversion` | *1.0*<br>Die Meldung entspricht dieser Version der [CloudEvents-Spezifikation](https://github.com/cloudevents/spec). |
| `type` | `Microsoft.DigitalTwins.Twin.Create`<br>`Microsoft.DigitalTwins.Twin.Delete` |
| `datacontenttype` | `application/json` |
| `subject` | ID des digitalen Zwillings. |
| `time` | Zeitstempel für den Zeitpunkt, an dem der Vorgang beim Zwilling erfolgt ist. |
| `traceparent` | Ein W3C-Ablaufverfolgungskontext für das Ereignis. |

### <a name="body-details"></a>Details zum Hauptteil

Hier ist ein Beispiel für eine Benachrichtigungsmeldung zum Lebenszyklus: 

```json
{
  "specversion": "1.0",
  "id": "d047e992-dddc-4a5a-b0af-fa79832235f8",
  "type": "Microsoft.DigitalTwins.Twin.Create",
  "source": "contoso-adt.api.wus2.digitaltwins.azure.net",
  "data": {
    "$dtId": "floor1",
    "$etag": "W/\"e398dbf4-8214-4483-9d52-880b61e491ec\"",
    "$metadata": {
      "$model": "dtmi:example:Floor;1"
    }
  },
  "subject": "floor1",
  "time": "2020-06-23T19:03:48.9700792Z",
  "datacontenttype": "application/json",
  "traceparent": "00-18f4e34b3e4a784aadf5913917537e7d-691a71e0a220d642-01"
}
```

Innerhalb der Nachricht enthält das Feld `data` die Daten des betreffenden digitalen Zwillings, dargestellt im JSON-Format. Das Schema hierfür ist *Digital Twins Resource 7.1*.

Bei Erstellungsereignissen spiegelt die `data`-Payload den Zustand des Zwillings nach der Erstellung der Ressource wider, sodass sie wie ein `GET`-Aufruf alle vom System generierten Elemente enthalten sollte.

Hier folgt ein Beispiel der Daten für ein [IoT Plug and Play (PnP)](../iot-pnp/overview-iot-plug-and-play.md)-Gerät mit Komponenten und ohne Eigenschaften der obersten Ebene. Eigenschaften, die für Geräte nicht sinnvoll sind (z. B. gemeldete Eigenschaften), sollten weggelassen werden. Dies sind die Informationen, die im Feld `data` der Benachrichtigungsmeldung zum Lebenszyklus angezeigt werden.

```json
{
  "$dtId": "device-digitaltwin-01",
  "$etag": "W/\"e59ce8f5-03c0-4356-aea9-249ecbdc07f9\"",
  "thermostat": {
    "temperature": 80,
    "humidity": 45,
    "$metadata": {
      "$model": "dtmi:com:contoso:Thermostat;1",
      "temperature": {
        "desiredValue": 85,
        "desiredVersion": 3,
        "ackVersion": 2,
        "ackCode": 200,
        "ackDescription": "OK"
      },
      "humidity": {
        "desiredValue": 40,
        "desiredVersion": 1,
        "ackVersion": 1,
        "ackCode": 200,
        "ackDescription": "OK"
      }
    }
  },
  "$metadata": {
    "$model": "dtmi:com:contoso:Thermostat_X500;1",
  }
}
```

Hier ist ein weiteres Beispiel für Daten eines digitalen Zwillings. Dieses basiert auf einem [Modell](concepts-models.md) und unterstützt keine Komponenten:

```json
{
  "$dtId": "logical-digitaltwin-01",
  "$etag": "W/\"e59ce8f5-03c0-4356-aea9-249ecbdc07f9\"",
  "avgTemperature": 70,
  "comfortIndex": 85,
  "$metadata": {
    "$model": "dtmi:com:contoso:Building;1",
    "avgTemperature": {
      "desiredValue": 72,
      "desiredVersion": 5,
      "ackVersion": 4,
      "ackCode": 200,
      "ackDescription": "OK"
    },
    "comfortIndex": {
      "desiredValue": 90,
      "desiredVersion": 1,
      "ackVersion": 3,
      "ackCode": 200,
      "ackDescription": "OK"
    }
  }
}
```

## <a name="digital-twin-relationship-change-notifications"></a>Benachrichtigungen zur Beziehungsänderung bei digitalen Zwillingen

**Benachrichtigungen zur Beziehungsänderung** werden ausgelöst, wenn eine Beziehung eines digitalen Zwillings erstellt, aktualisiert oder gelöscht wird. 

### <a name="properties"></a>Eigenschaften

Hier folgen die Felder des Hauptteils einer Änderungsbenachrichtigung zu einer Beziehung.

| Name    | Wert |
| --- | --- |
| `id` | Bezeichner der Benachrichtigung, z. B. eine UUID oder ein vom Dienst verwalteter Zähler. `source` + `id` ist für jedes einzelne Ereignis eindeutig. |
| `source` | Name der Azure Digital Twins-Instanz, z. B. *mydigitaltwins.westus2.azuredigitaltwins.net* |
| `data` | Die Payload der geänderten Beziehung. Weitere Informationen finden Sie weiter unten unter [Details zum Hauptteil](#body-details-2). |
| `specversion` | *1.0*<br>Die Meldung entspricht dieser Version der [CloudEvents-Spezifikation](https://github.com/cloudevents/spec). |
| `type` | `Microsoft.DigitalTwins.Relationship.Create`<br>`Microsoft.DigitalTwins.Relationship.Update`<br>`Microsoft.DigitalTwins.Relationship.Delete` |
| `datacontenttype` | `application/json` |
| `subject` | ID der Beziehung, z. B. `<twinID>/relationships/<relationshipID>`. |
| `time` | Zeitstempel für den Zeitpunkt, an dem der Vorgang in der Beziehung aufgetreten ist. |
| `traceparent` | Ein W3C-Ablaufverfolgungskontext für das Ereignis. |

### <a name="body-details"></a>Details zum Hauptteil

Innerhalb der Nachricht enthält das Feld `data` die Payload einer Beziehung im JSON-Format. Es verwendet dasselbe Format wie eine `GET`-Anforderung für eine Beziehung über die [DigitalTwins-API](/rest/api/digital-twins/dataplane/twins). 

Hier sehen Sie ein Beispiel der Daten für eine Benachrichtigung zum Aktualisieren einer Beziehung. „Aktualisieren einer Beziehung“ bedeutet, dass die Eigenschaften der Beziehung geändert wurden, sodass die Daten die aktualisierte Eigenschaft und ihren neuen Wert anzeigen. Dies sind die Informationen, die im Feld `data` der Benachrichtigungsmeldung zur Beziehung zwischen digitalen Zwillingen angezeigt werden.

```json
{
    "modelId": "dtmi:example:Floor;1",
    "patch": [
      {
        "value": "user3",
        "path": "/ownershipUser",
        "op": "replace"
      }
    ]
  }
```

Hier folgt ein Beispiel der Daten für eine Benachrichtigung zum Erstellen oder Löschen einer Beziehung. Für `Relationship.Delete` entspricht der Hauptteil der Anforderung `GET`, und er erhält den letzten Zustand vor dem Löschvorgang.

```json
{
    "$relationshipId": "device_to_device",
    "$etag": "W/\"72479873-0083-41a8-83e2-caedb932d881\"",
    "$relationshipName": "Connected",
    "$targetId": "device2",
    "connectionType": "WIFI"
}
```

## <a name="digital-twin-telemetry-messages"></a>Telemetrienachrichten für digitale Zwillinge

**Telemetrienachrichten** werden in Azure Digital Twins von verbundenen Geräten empfangen, die Messungen erfassen und senden.

### <a name="properties"></a>Eigenschaften

Hier folgen die Felder des Hauptteils einer Telemetrienachricht.

| Name    | Wert |
| --- | --- |
| `id` | Bezeichner der Benachrichtigung, die vom Kunden beim Aufrufen der Telemetrie-API bereitgestellt wird. |
| `source` | Vollqualifizierte Name des Zwillings, an den das Telemetrieereignis gesendet wurde. Verwenden Sie das folgende Format: `<yourDigitalTwinInstance>.api.<yourRegion>.digitaltwins.azure.net/<twinId>`. |
| `specversion` | *1.0*<br>Die Meldung entspricht dieser Version der [CloudEvents-Spezifikation](https://github.com/cloudevents/spec). |
| `type` | `microsoft.iot.telemetry` |
| `data` | Die Telemetrienachricht, die an Zwillinge gesendet wurde. Die Payload ist unverändert und entspricht möglicherweise nicht dem Schema des Zwillings, dem die Telemetriedaten gesendet wurden. |
| `dataschema` | Das Datenschema ist die Modell-ID des Zwillings oder der Komponente, der bzw. die die Telemetrie ausgibt. Beispiel: `dtmi:example:com:floor4;2`. |
| `datacontenttype` | `application/json` |
| `traceparent` | Ein W3C-Ablaufverfolgungskontext für das Ereignis. |

### <a name="body-details"></a>Details zum Hauptteil

Der Text enthält die Telemetriemessdaten zusammen mit einigen Kontextinformationen zum Gerät.

Hier ist ein Beispiel für eine Telemetrienachricht: 

```json
{
  "specversion": "1.0",
  "id": "df5a5992-817b-4e8a-b12c-e0b18d4bf8fb",
  "type": "microsoft.iot.telemetry",
  "source": "contoso-adt.api.wus2.digitaltwins.azure.net/digitaltwins/room1",
  "data": {
    "Temperature": 10
  },
  "dataschema": "dtmi:example:com:floor4;2",
  "datacontenttype": "application/json",
  "traceparent": "00-7e3081c6d3edfb4eaf7d3244b2036baa-23d762f4d9f81741-01"
}
```

## <a name="next-steps"></a>Nächste Schritte

Hier erfahren Sie mehr über das Bereitstellen von Ereignissen an verschiedene Ziele mithilfe von Endpunkten und Routen:
* [Konzepte: Ereignisrouten](concepts-route-events.md)
