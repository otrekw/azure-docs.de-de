---
title: Export von Azure IoT Central-Daten | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Daten aus Ihrer Azure IoT Central-Anwendung in Azure Event Hubs, Azure Service Bus und Azure Blob Storage exportiert werden.
services: iot-central
author: viv-liu
ms.author: viviali
ms.date: 01/30/2019
ms.topic: conceptual
ms.service: iot-central
manager: corywink
ms.openlocfilehash: a3d60bf38c4a9dad13dacf8ba9798c4078c1df1a
ms.sourcegitcommit: 57669c5ae1abdb6bac3b1e816ea822e3dbf5b3e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/06/2020
ms.locfileid: "77049695"
---
# <a name="export-your-azure-iot-central-data"></a>Exportieren Ihrer Azure IoT Central-Daten



*Dieses Thema gilt für Administratoren.*

In diesem Artikel wird beschrieben, wie Sie in Azure IoT Central das Feature für den kontinuierlichen Datenexport verwenden, um Ihre Daten in Instanzen von **Azure Event Hubs**, **Azure Service Bus** oder **Azure Blob Storage** zu exportieren. Die Daten werden im JSON-Format exportiert und können Telemetriedaten, Geräteinformationen sowie Gerätevorlageninformationen enthalten. Verwenden Sie die exportierten Daten für folgende Zwecke:

- Umsetzbare Einblicke und Analyseergebnisse. Diese Option umfasst das Auslösen benutzerdefinierter Regeln in Azure Stream Analytics, das Auslösen benutzerdefinierter Workflows in Azure Logic Apps oder deren Weiterleitung durch Azure Functions, damit sie transformiert werden.
- Analyse von kalten Datenpfaden wie Trainingsmodelle in Azure Machine Learning oder die langfristige Trendanalyse in Microsoft Power BI.

> [!Note]
> Wenn Sie den fortlaufenden Datenexport aktivieren, erhalten Sie nur die Daten ab dem jeweiligen Aktivierungszeitpunkt. Derzeit können Daten nicht für Zeiten abgerufen werden, in denen der Datenexport deaktiviert war. Aktivieren Sie den fortlaufenden Datenexport frühzeitig, um umfassendere Verlaufsdaten zu erhalten.

## <a name="prerequisites"></a>Voraussetzungen

Sie müssen ein Administrator für Ihre IoT Central-Anwendung sein oder über Berechtigungen für den Datenexport verfügen.

## <a name="set-up-export-destination"></a>Einrichten des Exportziels

Das Exportziel muss vorhanden sein, bevor Sie Ihren fortlaufenden Datenexport konfigurieren.

### <a name="create-event-hubs-namespace"></a>Erstellen eines Event Hubs-Namespaces

Wenn Sie keinen vorhandenen Event Hubs-Namespace als Exportziel haben, führen Sie die folgenden Schritte aus:

1. Erstellen Sie einen [neuen Event Hubs-Namespace im Azure-Portal](https://ms.portal.azure.com/#create/Microsoft.EventHub). Weitere Informationen erhalten Sie in der [Azure Event Hubs-Dokumentation](../../event-hubs/event-hubs-create.md).

2. Wählen Sie ein Abonnement aus. Sie können Daten in Abonnements exportieren, die mit dem Abonnement Ihrer IoT Central-Anwendung nicht identisch sind. In diesem Fall stellen Sie die Verbindung mithilfe einer Verbindungszeichenfolge her.

3. Erstellen Sie einen Event Hub in Ihrem Event Hubs-Namespace. Wechseln Sie zu Ihrem Namespace, und wählen Sie oben **+ Event Hub** aus, um eine Event Hub-Instanz zu erstellen.

### <a name="create-service-bus-namespace"></a>Erstellen eines Service Bus-Namespaces

Wenn Sie keinen vorhandenen Service Bus-Namespace als Exportziel haben, führen Sie die folgenden Schritte aus:

1. Erstellen Sie einen [neuen Service Bus-Namespace im Azure-Portal](https://ms.portal.azure.com/#create/Microsoft.ServiceBus.1.0.5). Weitere Informationen erhalten Sie in der [Azure Service Bus-Dokumentation](../../service-bus-messaging/service-bus-create-namespace-portal.md).
2. Wählen Sie ein Abonnement aus. Sie können Daten in Abonnements exportieren, die mit dem Abonnement Ihrer IoT Central-Anwendung nicht identisch sind. In diesem Fall stellen Sie die Verbindung mithilfe einer Verbindungszeichenfolge her.

3. Wechseln Sie zu Ihrem Service Bus-Namespace, und wählen Sie oben **+ Warteschlange** oder **+ Thema** aus, um eine Warteschlange oder ein Thema als Exportziel zu erstellen.

Wenn Sie eine Service Bus-Instanz als Exportziel auswählen, dürfen für Warteschlangen und Themen weder „Sitzungen“ noch „Duplikaterkennung“ aktiviert sein. Wenn eine dieser Optionen aktiviert ist, werden einige Nachrichten nicht in der Warteschlange oder im Thema eingehen.

### <a name="create-storage-account"></a>Speicherkonto erstellen

Wenn Sie kein vorhandenes Azure Storage-Konto als Exportziel haben, führen Sie die folgenden Schritte aus:

1. Erstellen Sie ein [neues Speicherkonto im Azure-Portal](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM). Sie können sich zum Erstellen neuer [Azure Blob Storage-Konten](https://aka.ms/blobdocscreatestorageaccount) oder neuer [Azure Data Lake Storage v2-Speicherkonten](../../storage/blobs/data-lake-storage-quickstart-create-account.md) genauer informieren. Beim Datenexport können Daten nur in Speicherkonten geschrieben werden, die Blockblobs unterstützen. Im Folgenden finden Sie eine Liste bekannter kompatibler Typen von Speicherkonten: 

    |Leistungsstufe|Kontotyp|
    |-|-|
    |Standard|Universell v2|
    |Standard|Universell v1|
    |Standard|Blob Storage|
    |Premium|Block Blob Storage|

2. Erstellen Sie einen Container in Ihrem Speicherkonto. Wechseln Sie zum Speicherkonto. Wählen Sie unter **Blob-Dienst** die Option **Blobs durchsuchen**. Wählen Sie oben die Option **+ Container** aus, um einen neuen Container zu erstellen.

## <a name="set-up-continuous-data-export"></a>Einrichten des fortlaufenden Datenexports

Nachdem Sie über ein Exportziel für Daten verfügen, gehen Sie jetzt folgendermaßen vor, um den kontinuierlichen Datenexport einzurichten.

1. Melden Sie sich bei Ihrer IoT Central-Anwendung an.

2. Wählen Sie im linken Bereich die Option **Datenexport** aus.

    > [!Note]
    > Falls „Datenexport“ im linken Bereich nicht angezeigt wird, verfügen Sie nicht über die Berechtigungen zum Konfigurieren des Datenexports in Ihrer App. Wenden Sie sich an Ihren Administrator, damit dieser den Datenexport einrichtet.

3. Wählen Sie oben rechts die Schaltfläche **+ Neu** aus. Wählen Sie einen der Einträge **Azure Event Hubs**, **Azure Service Bus** oder **Azure Blob Storage** als Ziel für Ihren Export aus. Die maximale Anzahl von Exporten pro Anwendung beträgt 5.

    ![Erstellen eine neuen kontinuierlichen Datenexports](media/howto-export-data/new-export-definition.png)

4. Wählen Sie im Dropdown-Listenfeld Ihren **Event Hubs-Namespace**, **Service Bus-Namespace** oder **Speicherkonto-Namespace** aus, oder **geben Sie eine Verbindungszeichenfolge ein**.

    - Ihnen werden nur Speicherkonten, Event Hubs-Namespaces und Service Bus-Namespaces aus demselben Abonnement wie dem Ihrer IoT Central-Anwendung angezeigt. Wenn Sie in ein Ziel außerhalb dieses Abonnements exportieren möchten, wählen Sie **Verbindungszeichenfolge eingeben** aus, und fahren Sie mit Schritt 5 fort.
    - Für Apps, die mit dem Free-Tarif erstellt werden, ist die einzige Möglichkeit zum Konfigurieren des fortlaufenden Datenexports die Verwendung einer Verbindungszeichenfolge. Apps unter dem Free-Tarif ist kein Azure-Abonnement zugeordnet.

    ![Erstellen eines neuen Event Hubs](media/howto-export-data/export-event-hub.png)

5. (Optional) Wenn Sie **Verbindungszeichenfolge eingeben** ausgewählt haben, wird ein neues Feld angezeigt, in das Sie Ihre Verbindungszeichenfolge einfügen können. Um die Verbindungszeichenfolge für
    - Ihren Event Hubs oder Service Bus abzurufen, wechseln Sie im Azure-Portal zu dem Namespace.
        - Wählen Sie unter **Einstellungen** die Option **Freigegebene Zugriffsrichtlinien** aus.
        - Wählen Sie die Standardrichtlinie **RootManageSharedAccessKey** aus, oder erstellen Sie eine neue.
        - Kopieren Sie die primäre oder die sekundäre Verbindungszeichenfolge.
    - Um Ihr Speicherkonto abzurufen, wechseln Sie im Azure-Portal zu dem Konto:
        - Wählen Sie unter **Einstellungen** die Option **Zugriffsschlüssel** aus.
        - Kopieren Sie entweder die Verbindungszeichenfolge „key1“ oder die Verbindungszeichenfolge „key2“.

6. Wählen Sie im Dropdown-Listenfeld einen Event Hub, eine Warteschlange, ein Thema oder einen Container aus.

7. Wählen Sie unter **Zu exportierende Daten** die Typen der zu exportierenden Daten aus, indem Sie den Typ jeweils auf **Ein** festlegen.

8. Um den kontinuierlichen Datenexport zu aktivieren, müssen Sie sicherstellen, dass der Umschalter **Aktiviert** auf **Ein** festgelegt ist. Wählen Sie **Speichern** aus.

9. Nach einigen Minuten werden Ihre Daten im ausgewählten Ziel angezeigt.

## <a name="export-contents-and-format"></a>Exportieren von Inhalten und des Formats

Exportierte Telemetriedaten enthalten die gesamte Nachricht, die Ihre Geräte an IoT Central gesendet haben, und nicht nur die eigentlichen Telemetriewerte. Exportierte Gerätedaten enthalten Änderungen an Eigenschaften und Metadaten aller Geräte, und exportierte Gerätevorlagendaten enthalten Änderungen an allen Gerätevorlagen.

Bei Event Hubs und Service Bus werden Daten nahezu in Echtzeit exportiert. Die Daten befinden sich in der Texteigenschaft und sind im JSON-Format (sehen Sie sich dazu die nachstehenden Beispiele an).

Bei Blob Storage werden Daten einmal pro Minute exportiert, wobei jede Datei den Batch der Änderungen seit der letzten exportierten Datei enthält. Exportierte Daten werden in drei Ordnern im JSON-Format gespeichert. Die Standardpfade in Ihrem Speicherkonto sind:

- Telemetriedaten: _{container}/{app-id}/telemetry/{YYYY}/{MM}/{dd}/{hh}/{mm}/{filename}_
- Geräte: _{container}/{app-id}/devices/{YYYY}/{MM}/{dd}/{hh}/{mm}/{filename}_
- Gerätevorlagen: _{container}/{app-id}/deviceTemplates/{YYYY}/{MM}/{dd}/{hh}/{mm}/{filename}_

Sie können die exportierten Dateien im Azure-Portal durchsuchen, indem Sie zu der jeweiligen Datei navigieren und die Registerkarte **Blob bearbeiten** auswählen.


## <a name="telemetry"></a>Telemetrie

Bei Event Hubs und Service Bus wird eine Nachricht schnell exportiert, nachdem IoT Central sie von einem Gerät empfangen hat. Jede exportierte Nachricht enthält die vom Gerät gesendete vollständige Nachricht in der Texteigenschaft im JSON-Format.

Bei Blob Storage werden Nachrichten im Batch zusammengefasst und einmal pro Minute exportiert. Für die exportierten Dateien wird dasselbe Format wie für die Nachrichtendateien verwendet, die bei der [IoT Hub-Nachrichtenweiterleitung](../../iot-hub/tutorial-routing.md) in Blob Storage exportiert wurden. 

> [!NOTE]
> Stellen Sie bei Blob Storage sicher, dass Ihre Geräte Nachrichten senden, die `contentType: application/JSON` und `contentEncoding:utf-8` (oder `utf-16`, `utf-32`) enthalten. Ein Beispiel finden Sie in der [Dokumentation zu IoT Hub](../../iot-hub/iot-hub-devguide-routing-query-syntax.md#message-routing-query-based-on-message-body).

Das Gerät, das die Telemetriedaten gesendet hat, wird durch die Geräte-ID dargestellt (siehe die folgenden Abschnitte). Wenn Sie die Namen der Geräte abrufen möchten, exportieren Sie Gerätedaten. Korrelieren Sie anschließend die einzelnen Nachrichten, indem Sie jeweils die **connectionDeviceId** verwenden, die mit der **deviceId** der Gerätenachricht übereinstimmt.

Dies ist eine Beispielnachricht, die in einem Event Hub, einer Service Bus-Warteschlange oder einem Thema empfangen wurde:

```json
{
  "body":{
    "temp":67.96099945281145,
    "humid":58.51139305465015,
    "pm25":36.91162432340187
  },
  "annotations":{
    "iothub-connection-device-id":"<deviceId>",
    "iothub-connection-auth-method":"{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\",\"acceptingIpFilterRule\":null}",
    "iothub-connection-auth-generation-id":"<generationId>",
    "iothub-enqueuedtime":1539381029965,
    "iothub-message-source":"Telemetry",
    "x-opt-sequence-number":25325,
    "x-opt-offset":"<offset>",
    "x-opt-enqueued-time":1539381030200
  },
  "sequenceNumber":25325,
  "enqueuedTimeUtc":"2018-10-12T21:50:30.200Z",
  "offset":"<offset>",
  "properties":{
    "content_type":"application/json",
    "content_encoding":"utf-8"
  }
}
```

Dies ist ein Beispieldatensatz, der in Blob Storage exportiert wurde:

```json
{
  "EnqueuedTimeUtc":"2019-09-26T17:46:09.8870000Z",
  "Properties":{

  },
  "SystemProperties":{
    "connectionDeviceId":"<deviceid>",
    "connectionAuthMethod":"{\"scope\":\"device\",\"type\":\"sas\",\"issuer\":\"iothub\",\"acceptingIpFilterRule\":null}",
    "connectionDeviceGenerationId":"637051167384630591",
    "contentType":"application/json",
    "contentEncoding":"utf-8",
    "enqueuedTime":"2019-09-26T17:46:09.8870000Z"
  },
  "Body":{
    "temp":49.91322758395974,
    "humid":49.61214852573155,
    "pm25":25.87332214661367
  }
}
```

## <a name="devices"></a>Geräte

Jede Nachricht bzw. jeder Datensatz in einer Momentaufnahme stellt eine oder mehrere Änderungen an einem Gerät und den zugehörigen Geräte- und Cloudeigenschaften seit der letzten exportierten Nachricht dar. Dies schließt Folgendes ein:

- `id` des Geräts in IoT Central
- `displayName` des Geräts
- Gerätevorlagen-ID in `instanceOf`
- `simulated`-Flag: „true“, wenn es sich um ein simuliertes Gerät handelt
- `provisioned`-Flag: „true“, wenn das Gerät bereitgestellt wurde
- `approved`-Flag: „true“, wenn für das Gerät das Senden von Daten genehmigt wurde
- Eigenschaftswerte
- `properties`, einschließlich Werte für Geräte- und Cloudeigenschaften

Gelöschte Geräte werden nicht exportiert. Derzeit gibt es in exportierten Nachrichten keine Kennzeichnung gelöschter Geräte.

Bei Event Hubs und Service Bus werden Nachrichten, die Gerätedaten enthalten, nahezu in Echtzeit an Ihren Event Hub, Ihre Service Bus-Warteschlange oder Ihr Thema gesendet, wie in IoT Central angezeigt wird. 

Bei Blob Storage wird eine neue Momentaufnahme, die alle Änderungen seit der letzten geschriebenen Änderung enthält, einmal pro Minute exportiert.

Die ist eine Beispielnachricht zu Geräten und Eigenschaftsdaten im Event Hub, in der Service-Bus-Warteschlange oder im Thema:

```json
{
  "body":{
    "id": "<device Id>",
    "etag": "<etag>",
    "displayName": "Sensor 1",
    "instanceOf": "<device template Id>",
    "simulated": false,
    "provisioned": true,
    "approved": true,
    "properties": {
        "sensorComponent": {
            "setTemp": "30",
            "fwVersion": "2.0.1",
            "status": { "first": "first", "second": "second" },
            "$metadata": {
                "setTemp": {
                    "desiredValue": "30",
                    "desiredVersion": 3,
                    "desiredTimestamp": "2020-02-01T17:15:08.9284049Z",
                    "ackVersion": 3
                },
                "fwVersion": { "ackVersion": 3 },
                "status": {
                    "desiredValue": {
                        "first": "first",
                        "second": "second"
                    },
                    "desiredVersion": 2,
                    "desiredTimestamp": "2020-02-01T17:15:08.9284049Z",
                    "ackVersion": 2
                }
            },
            
        }
    },
    "installDate": { "installDate": "2020-02-01" }
},
  "annotations":{
    "iotcentral-message-source":"devices",
    "x-opt-partition-key":"<partitionKey>",
    "x-opt-sequence-number":39740,
    "x-opt-offset":"<offset>",
    "x-opt-enqueued-time":1539274959654
  },
  "partitionKey":"<partitionKey>",
  "sequenceNumber":39740,
  "enqueuedTimeUtc":"2020-02-01T18:14:49.3820326Z",
  "offset":"<offset>"
}
```

Dies ist eine Beispielmomentaufnahme mit Geräten und Eigenschaftsdaten in Blob Storage. Exportierte Dateien enthalten eine einzige Zeile pro Datensatz.

```json
{
  "id": "<device Id>",
  "etag": "<etag>",
  "displayName": "Sensor 1",
  "instanceOf": "<device template Id>",
  "simulated": false,
  "provisioned": true,
  "approved": true,
  "properties": {
      "sensorComponent": {
          "setTemp": "30",
          "fwVersion": "2.0.1",
          "status": { "first": "first", "second": "second" },
          "$metadata": {
              "setTemp": {
                  "desiredValue": "30",
                  "desiredVersion": 3,
                  "desiredTimestamp": "2020-02-01T17:15:08.9284049Z",
                  "ackVersion": 3
              },
              "fwVersion": { "ackVersion": 3 },
              "status": {
                  "desiredValue": {
                      "first": "first",
                      "second": "second"
                  },
                  "desiredVersion": 2,
                  "desiredTimestamp": "2020-02-01T17:15:08.9284049Z",
                  "ackVersion": 2
              }
          },
          
      }
  },
  "installDate": { "installDate": "2020-02-01" }
}
```

## <a name="device-templates"></a>Gerätevorlagen

Jede Nachricht bzw. jeder Momentaufnahmen-Datensatz stellt eine oder mehrere Änderungen an einer veröffentlichten Gerätevorlage seit der letzten exportierten Nachricht dar. Mit jeder Nachricht bzw. jedem Datensatz werden folgende Informationen gesendet:

- `id` der Gerätevorlage, die mit der `instanceOf` des Gerätestreams oben übereinstimmt
- `displayName` der Gerätevorlage
- Das `capabilityModel` des Geräts, einschließlich seiner `interfaces`, sowie die Definitionen für Telemetriedaten, Eigenschaften und Befehle
- Definitionen für `cloudProperties`
- Außerkraftsetzungen und Anfangswerte, zusammen mit dem `capabilityModel`

Gelöschte Gerätevorlagen werden nicht exportiert. Derzeit gibt es in exportierten Nachrichten keine Kennzeichnung gelöschter Gerätevorlagen.

Bei Event Hubs und Service Bus werden Nachrichten, die Gerätevorlagendaten enthalten, nahezu in Echtzeit an Ihren Event Hub, Ihre Service Bus-Warteschlange oder Ihr Thema gesendet, wie in IoT Central angezeigt wird. 

Bei Blob Storage wird eine neue Momentaufnahme, die alle Änderungen seit der letzten geschriebenen Änderung enthält, einmal pro Minute exportiert.

Die ist eine Beispielnachricht zu Gerätevorlagendaten im Event Hub, in der Service-Bus-Warteschlange oder im Thema:

```json
{
  "body":{
      "id": "<device template id>",
      "etag": "<etag>",
      "types": ["DeviceModel"],
      "displayName": "Sensor template",
      "capabilityModel": {
          "@id": "<capability model id>",
          "@type": ["CapabilityModel"],
          "contents": [],
          "implements": [
              {
                  "@id": "<component Id>",
                  "@type": ["InterfaceInstance"],
                  "name": "sensorComponent",
                  "schema": {
                      "@id": "<interface Id>",
                      "@type": ["Interface"],
                      "displayName": "Sensor interface",
                      "contents": [
                          {
                              "@id": "<id>",
                              "@type": ["Telemetry"],
                              "displayName": "Humidity",
                              "name": "humidity",
                              "schema": "double"
                          },
                          {
                              "@id": "<id>",
                              "@type": ["Telemetry", "SemanticType/Event"],
                              "displayName": "Error event",
                              "name": "error",
                              "schema": "integer"
                          },
                          {
                              "@id": "<id>",
                              "@type": ["Property"],
                              "displayName": "Set temperature",
                              "name": "setTemp",
                              "writable": true,
                              "schema": "integer",
                              "unit": "Units/Temperature/fahrenheit",
                              "initialValue": "30"
                          },
                          {
                              "@id": "<id>",
                              "@type": ["Property"],
                              "displayName": "Firmware version read only",
                              "name": "fwversion",
                              "schema": "string"
                          },
                          {
                              "@id": "<id>",
                              "@type": ["Property"],
                              "displayName": "Display status",
                              "name": "status",
                              "writable": true,
                              "schema": {
                                  "@id": "urn:testInterface:status:obj:ka8iw8wka:1",
                                  "@type": ["Object"]
                              }
                          },
                          {
                              "@id": "<id>",
                              "@type": ["Command"],
                              "commandType": "synchronous",
                              "request": {
                                  "@id": "<id>",
                                  "@type": ["SchemaField"],
                                  "displayName": "Configuration",
                                  "name": "config",
                                  "schema": "string"
                              },
                              "response": {
                                  "@id": "<id>",
                                  "@type": ["SchemaField"],
                                  "displayName": "Response",
                                  "name": "response",
                                  "schema": "string"
                              },
                              "displayName": "Configure sensor",
                              "name": "sensorConfig"
                          }
                      ]
                  }
              }
          ],
          "displayName": "Sensor capability model"
      },
      "solutionModel": {
          "@id": "<id>",
          "@type": ["SolutionModel"],
          "cloudProperties": [
              {
                  "@id": "<id>",
                  "@type": ["CloudProperty"],
                  "displayName": "Install date",
                  "name": "installDate",
                  "schema": "dateTime",
                  "valueDetail": {
                      "@id": "<id>",
                      "@type": ["ValueDetail/DateTimeValueDetail"]
                  }
              }
          ]
      }
  },
    "annotations":{
      "iotcentral-message-source":"deviceTemplates",
      "x-opt-partition-key":"<partitionKey>",
      "x-opt-sequence-number":25315,
      "x-opt-offset":"<offset>",
      "x-opt-enqueued-time":1539274985085
    },
    "partitionKey":"<partitionKey>",
    "sequenceNumber":25315,
    "enqueuedTimeUtc":"2019-10-02T16:23:05.085Z",
    "offset":"<offset>"
  }
}
```

Dies ist eine Beispielmomentaufnahme mit Geräten und Eigenschaftsdaten in Blob Storage. Exportierte Dateien enthalten eine einzige Zeile pro Datensatz.

```json
{
      "id": "<device template id>",
      "etag": "<etag>",
      "types": ["DeviceModel"],
      "displayName": "Sensor template",
      "capabilityModel": {
          "@id": "<capability model id>",
          "@type": ["CapabilityModel"],
          "contents": [],
          "implements": [
              {
                  "@id": "<component Id>",
                  "@type": ["InterfaceInstance"],
                  "name": "Sensor component",
                  "schema": {
                      "@id": "<interface Id>",
                      "@type": ["Interface"],
                      "displayName": "Sensor interface",
                      "contents": [
                          {
                              "@id": "<id>",
                              "@type": ["Telemetry"],
                              "displayName": "Humidity",
                              "name": "humidity",
                              "schema": "double"
                          },
                          {
                              "@id": "<id>",
                              "@type": ["Telemetry", "SemanticType/Event"],
                              "displayName": "Error event",
                              "name": "error",
                              "schema": "integer"
                          },
                          {
                              "@id": "<id>",
                              "@type": ["Property"],
                              "displayName": "Set temperature",
                              "name": "setTemp",
                              "writable": true,
                              "schema": "integer",
                              "unit": "Units/Temperature/fahrenheit",
                              "initialValue": "30"
                          },
                          {
                              "@id": "<id>",
                              "@type": ["Property"],
                              "displayName": "Firmware version read only",
                              "name": "fwversion",
                              "schema": "string"
                          },
                          {
                              "@id": "<id>",
                              "@type": ["Property"],
                              "displayName": "Display status",
                              "name": "status",
                              "writable": true,
                              "schema": {
                                  "@id": "urn:testInterface:status:obj:ka8iw8wka:1",
                                  "@type": ["Object"]
                              }
                          },
                          {
                              "@id": "<id>",
                              "@type": ["Command"],
                              "commandType": "synchronous",
                              "request": {
                                  "@id": "<id>",
                                  "@type": ["SchemaField"],
                                  "displayName": "Configuration",
                                  "name": "config",
                                  "schema": "string"
                              },
                              "response": {
                                  "@id": "<id>",
                                  "@type": ["SchemaField"],
                                  "displayName": "Response",
                                  "name": "response",
                                  "schema": "string"
                              },
                              "displayName": "Configure sensor",
                              "name": "sensorconfig"
                          }
                      ]
                  }
              }
          ],
          "displayName": "Sensor capability model"
      },
      "solutionModel": {
          "@id": "<id>",
          "@type": ["SolutionModel"],
          "cloudProperties": [
              {
                  "@id": "<id>",
                  "@type": ["CloudProperty"],
                  "displayName": "Install date",
                  "name": "installDate",
                  "schema": "dateTime",
                  "valueDetail": {
                      "@id": "<id>",
                      "@type": ["ValueDetail/DateTimeValueDetail"]
                  }
              }
          ]
      }
  }
```
## <a name="data-format-change-notice"></a>Änderungshinweis zum Datenformat

> [!Note]
> Das Datenformat des Telemetriedatenstroms wird von dieser Änderung nicht beeinträchtigt. Nur die Geräte- und Gerätevorlagenstreams von Daten sind hiervon betroffen.

Wenn Sie in Ihrer Vorschauanwendung über einen vorhandenen Datenexport verfügen, für den die Datenströme für *Geräte* und *Gerätevorlagen* aktiviert sind, müssen Sie Ihren Export bis zum **30. Juni 2020** aktualisieren. Dies gilt für Exporte in Azure Blob Storage, Azure Event Hubs und Azure Service Bus.

Ab dem 3. Februar 2020 verfügen alle neuen Exporte in Anwendungen, für die Geräte und Gerätevorlagen aktiviert sind, über das oben beschriebene Datenformat. Für alle zuvor erstellten Exporte wird bis zum 30. Juni 2020 das alte Datenformat beibehalten. Anschließend werden diese Exporte automatisch zum neuen Datenformat migriert. Das neue Datenformat stimmt mit den Objekten [device](https://docs.microsoft.com/rest/api/iotcentral/devices/get) (Gerät), [device property](https://docs.microsoft.com/rest/api/iotcentral/devices/getproperties) (Geräteeigenschaft), [device cloud property](https://docs.microsoft.com/rest/api/iotcentral/devices/getcloudproperties) (Gerätecloudeigenschaft) und [device template](https://docs.microsoft.com/rest/api/iotcentral/devicetemplates/get) (Gerätevorlage) in der öffentlichen IoT Central-API überein. 
 
Wichtige Unterschiede zwischen dem alten und dem neuen Datenformat für **Devices** (Geräte) sind:
- `@id` für Gerät entfernt, `deviceId` in `id` umbenannt 
- `provisioned`-Flag hinzugefügt, um den Bereitstellungsstatus des Geräts zu beschreiben
- `approved`-Flag hinzugefügt, um den Genehmigungsstatus des Geräts zu beschreiben
- `properties` mit Geräte- und Cloudeigenschaften, Übereinstimmung mit Entitäten in der öffentlichen API

Wichtige Unterschiede zwischen dem alten und dem neuen Datenformat für **Device templates** (Gerätevorlagen) sind:

- `@id` für Gerätevorlage in `id` umbenannt
- `@type` für die Gerätevorlage in `types` umbenannt (ist jetzt ein Array)

### <a name="devices-format-deprecated-as-of-3-february-2020"></a>Devices (Geräte): Format gilt ab dem 3. Februar 2020 als veraltet
```json
{
  "@id":"<id-value>",
  "@type":"Device",
  "displayName":"Airbox",
  "data":{
    "$cloudProperties":{
        "Color":"blue"
    },
    "EnvironmentalSensor":{
      "thsensormodel":{
        "reported":{
          "value":"Neque quia et voluptatem veritatis assumenda consequuntur quod.",
          "$lastUpdatedTimestamp":"2019-09-30T20:35:43.8478978Z"
        }
      },
      "pm25sensormodel":{
        "reported":{
          "value":"Aut alias odio.",
          "$lastUpdatedTimestamp":"2019-09-30T20:35:43.8478978Z"
        }
      }
    },
    "urn_azureiot_DeviceManagement_DeviceInformation":{
      "totalStorage":{
        "reported":{
          "value":27900.9730905171,
          "$lastUpdatedTimestamp":"2019-09-30T20:35:43.8478978Z"
        }
      },
      "totalMemory":{
        "reported":{
          "value":4667.82916715811,
          "$lastUpdatedTimestamp":"2019-09-30T20:35:43.8478978Z"
        }
      }
    }
  },
  "instanceOf":"<template-id>",
  "deviceId":"<device-id>",
  "simulated":true
}
```

### <a name="device-templates-format-deprecated-as-of-3-february-2020"></a>Device templates (Gerätevorlagen): Format gilt ab dem 3. Februar 2020 als veraltet
```json
{
  "@id":"<template-id>",
  "@type":"DeviceModelDefinition",
  "displayName":"Airbox",
  "capabilityModel":{
    "@id":"<id>",
    "@type":"CapabilityModel",
    "implements":[
      {
        "@id":"<id>",
        "@type":"InterfaceInstance",
        "name":"EnvironmentalSensor",
        "schema":{
          "@id":"<id>",
          "@type":"Interface",
          "comment":"Requires temperature and humidity sensors.",
          "description":"Provides functionality to report temperature, humidity. Provides telemetry, commands and read-write properties",
          "displayName":"Environmental Sensor",
          "contents":[
            {
              "@id":"<id>",
              "@type":"Telemetry",
              "description":"Current temperature on the device",
              "displayName":"Temperature",
              "name":"temp",
              "schema":"double",
              "unit":"Units/Temperature/celsius",
              "valueDetail":{
                "@id":"<id>",
                "@type":"ValueDetail/NumberValueDetail",
                "minValue":{
                  "@value":"50"
                }
              },
              "visualizationDetail":{
                "@id":"<id>",
                "@type":"VisualizationDetail"
              }
            },
            {
              "@id":"<id>",
              "@type":"Telemetry",
              "description":"Current humidity on the device",
              "displayName":"Humidity",
              "name":"humid",
              "schema":"integer"
            },
            {
              "@id":"<id>",
              "@type":"Telemetry",
              "description":"Current PM2.5 on the device",
              "displayName":"PM2.5",
              "name":"pm25",
              "schema":"integer"
            },
            {
              "@id":"<id>",
              "@type":"Property",
              "description":"T&H Sensor Model Name",
              "displayName":"T&H Sensor Model",
              "name":"thsensormodel",
              "schema":"string"
            },
            {
              "@id":"<id>",
              "@type":"Property",
              "description":"PM2.5 Sensor Model Name",
              "displayName":"PM2.5 Sensor Model",
              "name":"pm25sensormodel",
              "schema":"string"
            }
          ]
        }
      },
      {
        "@id":"<id>",
        "@type":"InterfaceInstance",
        "name":"urn_azureiot_DeviceManagement_DeviceInformation",
        "schema":{
          "@id":"<id>",
          "@type":"Interface",
          "displayName":"Device information",
          "contents":[
            {
              "@id":"<id>",
              "@type":"Property",
              "comment":"Total available storage on the device in kilobytes. Ex. 20480000 kilobytes.",
              "displayName":"Total storage",
              "name":"totalStorage",
              "displayUnit":"kilobytes",
              "schema":"long"
            },
            {
              "@id":"<id>",
              "@type":"Property",
              "comment":"Total available memory on the device in kilobytes. Ex. 256000 kilobytes.",
              "displayName":"Total memory",
              "name":"totalMemory",
              "displayUnit":"kilobytes",
              "schema":"long"
            }
          ]
        }
      }
    ],
    "displayName":"AAEONAirbox52"
  },
  "solutionModel":{
    "@id":"<id>",
    "@type":"SolutionModel",
    "cloudProperties":[
      {
        "@id":"<id>",
        "@type":"CloudProperty",
        "displayName":"Color",
        "name":"Color",
        "schema":"string",
        "valueDetail":{
          "@id":"<id>",
          "@type":"ValueDetail/StringValueDetail"
        },
        "visualizationDetail":{
          "@id":"<id>",
          "@type":"VisualizationDetail"
        }
      }
    ]
  }
}
```
## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie jetzt mit dem Exportieren Ihrer Daten in Azure Event Hubs, Azure Service Bus und Azure Blob Storage vertraut sind, können Sie mit dem nächsten Schritt fortfahren:

> [!div class="nextstepaction"]
> [Erstellen von Webhooks](./howto-create-webhooks.md)
