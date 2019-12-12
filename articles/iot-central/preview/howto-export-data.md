---
title: Export von Azure IoT Central-Daten | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Daten aus Ihrer Azure IoT Central-Anwendung in Azure Event Hubs, Azure Service Bus und Azure Blob Storage exportiert werden.
services: iot-central
author: viv-liu
ms.author: viviali
ms.date: 12/06/2019
ms.topic: conceptual
ms.service: iot-central
manager: corywink
ms.openlocfilehash: 1aac5af916e414178676a1caf42fead41109de68
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2019
ms.locfileid: "74974460"
---
# <a name="export-your-azure-iot-central-data-preview-features"></a>Exportieren von Azure IoT Central-Daten (Previewfunktionen)

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

*Dieses Thema gilt für Administratoren.*

In diesem Artikel wird beschrieben, wie Sie in Azure IoT Central das Feature für den kontinuierlichen Datenexport verwenden, um Ihre Daten in Instanzen von **Azure Event Hubs**, **Azure Service Bus** oder **Azure Blob Storage** zu exportieren. Die Daten werden im JSON-Format exportiert und können Telemetriedaten, Geräteinformationen sowie Gerätevorlageninformationen enthalten. Verwenden Sie die exportierten Daten für folgende Zwecke:

- Umsetzbare Einblicke und Analyseergebnisse. Diese Option umfasst das Auslösen benutzerdefinierter Regeln in Azure Stream Analytics, das Auslösen benutzerdefinierter Workflows in Azure Logic Apps oder deren Weiterleitung durch Azure Functions, damit sie transformiert werden.
- Analyse von kalten Datenpfaden wie Trainingsmodelle in Azure Machine Learning oder die langfristige Trendanalyse in Microsoft Power BI.

> [!Note]
> Wenn Sie den fortlaufenden Datenexport aktivieren, erhalten Sie nur die Daten ab dem jeweiligen Aktivierungszeitpunkt. Derzeit können Daten nicht für Zeiten abgerufen werden, in denen der Datenexport deaktiviert war. Aktivieren Sie den fortlaufenden Datenexport frühzeitig, um umfassendere Verlaufsdaten zu erhalten.

## <a name="prerequisites"></a>Voraussetzungen

Sie müssen Administrator in Ihrer IoT Central-Anwendung sein.

## <a name="set-up-export-destination"></a>Einrichten des Exportziels

Das Exportziel muss vorhanden sein, bevor Sie Ihren fortlaufenden Datenexport konfigurieren.

### <a name="create-event-hubs-namespace"></a>Erstellen eines Event Hubs-Namespaces

Wenn Sie keinen vorhandenen Event Hubs-Namespace als Exportziel haben, führen Sie die folgenden Schritte aus:

1. Erstellen Sie einen [neuen Event Hubs-Namespace im Azure-Portal](https://ms.portal.azure.com/#create/Microsoft.EventHub). Weitere Informationen erhalten Sie in der [Azure Event Hubs-Dokumentation](../../event-hubs/event-hubs-create.md).

2. Wählen Sie ein Abonnement aus. Sie können Daten in Abonnements exportieren, die mit dem Abonnement Ihrer IoT Central-Anwendung mit nutzungsbasierter Bezahlung nicht identisch sind. In diesem Fall stellen Sie die Verbindung mithilfe einer Verbindungszeichenfolge her.

3. Erstellen Sie einen Event Hub in Ihrem Event Hubs-Namespace. Wechseln Sie zu Ihrem Namespace, und wählen Sie oben **+ Event Hub** aus, um eine Event Hub-Instanz zu erstellen.

### <a name="create-service-bus-namespace"></a>Erstellen eines Service Bus-Namespaces

Wenn Sie keinen vorhandenen Service Bus-Namespace als Exportziel haben, führen Sie die folgenden Schritte aus:

1. Erstellen Sie einen [neuen Service Bus-Namespace im Azure-Portal](https://ms.portal.azure.com/#create/Microsoft.ServiceBus.1.0.5). Weitere Informationen erhalten Sie in der [Azure Service Bus-Dokumentation](../../service-bus-messaging/service-bus-create-namespace-portal.md).
2. Wählen Sie ein Abonnement aus. Sie können Daten in Abonnements exportieren, die mit dem Abonnement Ihrer IoT Central-Anwendung mit nutzungsbasierter Bezahlung nicht identisch sind. In diesem Fall stellen Sie die Verbindung mithilfe einer Verbindungszeichenfolge her.

3. Wechseln Sie zu Ihrem Service Bus-Namespace, und wählen Sie oben **+ Warteschlange** oder **+ Thema** aus, um eine Warteschlange oder ein Thema als Exportziel zu erstellen.

Wenn Sie eine Service Bus-Instanz als Exportziel auswählen, dürfen für Warteschlangen und Themen weder „Sitzungen“ noch „Duplikaterkennung“ aktiviert sein. Wenn eine dieser Optionen aktiviert ist, werden einige Nachrichten nicht in der Warteschlange oder im Thema eingehen.

### <a name="create-storage-account"></a>Speicherkonto erstellen

Wenn Sie kein vorhandenes Azure Storage-Konto als Exportziel haben, führen Sie die folgenden Schritte aus:

1. Erstellen Sie ein [neues Speicherkonto im Azure-Portal](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM). Sie können sich zum Erstellen neuer [Azure Blob Storage-Konten](https://aka.ms/blobdocscreatestorageaccount) oder neuer [Azure Data Lake Storage v2-Speicherkonten](../../storage/blobs/data-lake-storage-quickstart-create-account.md) genauer informieren.

    - Wenn Sie Daten in ein Azure Data Lake Storage v2-Speicherkonto exportieren möchten, müssen Sie in **Kontoart** die Option **BlobStorage** auswählen.
    - Sie können Daten in Speicherkonten in anderen Abonnements als demjenigen für Ihre IoT Central-Anwendung mit nutzungsbasierter Zahlung exportieren. In diesem Fall stellen Sie die Verbindung mithilfe einer Verbindungszeichenfolge her.

2. Erstellen Sie einen Container in Ihrem Speicherkonto. Wechseln Sie zum Speicherkonto. Wählen Sie unter **Blob-Dienst** die Option **Blobs durchsuchen**. Wählen Sie oben die Option **+ Container** aus, um einen neuen Container zu erstellen.

## <a name="set-up-continuous-data-export"></a>Einrichten des fortlaufenden Datenexports

Nachdem Sie über ein Exportziel für Daten verfügen, gehen Sie jetzt folgendermaßen vor, um den kontinuierlichen Datenexport einzurichten.

1. Melden Sie sich bei Ihrer IoT Central-Anwendung an.

2. Wählen Sie im linken Bereich die Option **Datenexport** aus.

    > [!Note]
    > Wenn im linken Bereich „Datenexport“ nicht angezeigt wird, sind Sie kein Administrator in Ihrer App. Wenden Sie sich an Ihren Administrator, damit dieser den Datenexport einrichtet.

3. Wählen Sie oben rechts die Schaltfläche **+ Neu** aus. Wählen Sie einen der Einträge **Azure Event Hubs**, **Azure Service Bus** oder **Azure Blob Storage** als Ziel für Ihren Export aus. Die maximale Anzahl von Exporten pro Anwendung beträgt 5.

    ![Erstellen eine neuen kontinuierlichen Datenexports](media/howto-export-data/export-new2.png)

4. Wählen Sie im Dropdown-Listenfeld Ihren **Event Hubs-Namespace**, **Service Bus-Namespace** oder **Speicherkonto-Namespace** aus, oder **geben Sie eine Verbindungszeichenfolge ein**.

    - Ihnen werden nur Speicherkonten, Event Hubs-Namespaces und Service Bus-Namespaces aus demselben Abonnement wie dem Ihrer IoT Central-Anwendung angezeigt. Wenn Sie in ein Ziel außerhalb dieses Abonnements exportieren möchten, wählen Sie **Verbindungszeichenfolge eingeben** aus, und fahren Sie mit Schritt 5 fort.
    - Bei 7-Tage-Test-Apps kann der kontinuierliche Datenexport nur über die Eingabe einer Verbindungszeichenfolge konfiguriert werden. 7-Tage-Test-Apps ist kein Azure-Abonnement zugeordnet.

    ![Erstellen eines neuen Event Hubs](media/howto-export-data/export-eh.png)

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

8. Um den kontinuierlichen Datenexport zu aktivieren, stellen Sie sicher, dass der Umschalter **Datenexport** auf **Ein** festgelegt ist. Wählen Sie **Speichern** aus.

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

Bei Blob Storage werden Nachrichten im Batch zusammengefasst und einmal pro Minute exportiert. Für die exportierten Dateien wird dasselbe Format wie für die Nachrichtendateien verwendet, die bei der [IoT Hub-Nachrichtenweiterleitung](../../iot-hub/iot-hub-csharp-csharp-process-d2c.md) in Blob Storage exportiert wurden. 

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

Jede Nachricht bzw. jeder Datensatz in einer Momentaufnahme stellt eine oder mehrere Änderungen an einem Gerät und dessen Eigenschaften seit der letzten exportierten Nachricht dar. Dies umfasst:

- `@id` des Geräts in IoT Central
- `name` des Geräts
- `deviceId` von [Device Provisioning Service](../core/howto-connect-nodejs.md?toc=/azure/iot-central/preview/toc.json&bc=/azure/iot-central/preview/breadcrumb/toc.json)
- Gerätevorlageninformationen
- Eigenschaftswerte

Die Gerätevorlage, der die einzelnen Geräte zugewiesen sind, wird durch das Feld `instanceOf` dargestellt. Wenn Sie den Namen und zusätzliche Informationen zur Gerätevorlage abrufen möchten, müssen Sie unbedingt auch die Daten der Vorlage exportieren.

Gelöschte Geräte werden nicht exportiert. Derzeit gibt es in exportierten Nachrichten keine Kennzeichnung gelöschter Geräte.

Bei Event Hubs und Service Bus werden Nachrichten, die Gerätedaten enthalten, nahezu in Echtzeit an Ihren Event Hub, Ihre Service Bus-Warteschlange oder Ihr Thema gesendet, wie in IoT Central angezeigt wird. 

Bei Blob Storage wird eine neue Momentaufnahme, die alle Änderungen seit der letzten geschriebenen Änderung enthält, einmal pro Minute exportiert.

Die ist eine Beispielnachricht zu Geräten und Eigenschaftsdaten im Event Hub, in der Service-Bus-Warteschlange oder im Thema:

```json
{
  "body":{
    "@id":"<id>",
    "@type":"Device",
    "displayName":"Airbox - 266d30aedn5",
    "data":{
      "$cloudProperties":{
        "Color":"blue"
      },
      "EnvironmentalSensor":{
        "thsensormodel":{
          "reported":{
            "value":"A1",
            "$lastUpdatedTimestamp":"2019-10-02T18:14:49.3820326Z"
          }
        },
        "pm25sensormodel":{
          "reported":{
            "value":"P1",
            "$lastUpdatedTimestamp":"2019-10-02T18:14:49.3820326Z"
          }
        }
      },
      "urn_azureiot_DeviceManagement_DeviceInformation":{
        "totalStorage":{
          "reported":{
            "value":3088.1959855710156,
            "$lastUpdatedTimestamp":"2019-10-02T18:14:49.3820326Z"
          }
        },
        "totalMemory":{
          "reported":{
            "value":16005.703586477555,
            "$lastUpdatedTimestamp":"2019-10-02T18:14:49.3820326Z"
          }
        }
      }
    },
    "instanceOf":"<templateId>",
    "deviceId":"<deviceId>",
    "simulated":true
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
  "enqueuedTimeUtc":"2019-10-02T18:14:49.3820326Z",
  "offset":"<offset>"
}
```

Dies ist eine Beispielmomentaufnahme mit Geräten und Eigenschaftsdaten in Blob Storage. Exportierte Dateien enthalten eine einzige Zeile pro Datensatz.

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

## <a name="device-templates"></a>Gerätevorlagen

Jede Nachricht bzw. jeder Momentaufnahmen-Datensatz stellt eine oder mehrere Änderungen an einer Gerätevorlage seit der letzten exportierten Nachricht dar. Mit jeder Nachricht bzw. jedem Datensatz werden folgende Informationen gesendet:

- `@id` der Gerätevorlage, die mit der `instanceOf` des Gerätestreams oben übereinstimmt
- `name` der Gerätevorlage
- `version` der Gerätevorlage
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
> [Auslösen von Azure Functions](../core/howto-trigger-azure-functions.md?toc=/azure/iot-central/preview/toc.json&bc=/azure/iot-central/preview/breadcrumb/toc.json)
