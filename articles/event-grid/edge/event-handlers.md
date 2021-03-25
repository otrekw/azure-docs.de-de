---
title: Ereignishandler und Ziele – Azure Event Grid und IoT Edge | Microsoft-Dokumentation
description: Ereignishandler und Ziele in Event Grid in Edge
ms.date: 07/08/2020
ms.topic: article
ms.openlocfilehash: 18e2b944f60ebdf8a1b0c60ba446a13df13134c5
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "86171583"
---
# <a name="event-handlers-and-destinations-in-event-grid-on-edge"></a>Ereignishandler und Ziele in Event Grid in Edge

Ein Ereignishandler ist der Ort, an dem das Ereignis für weitere Aktionen vorbereitet oder verarbeitet wird. Mit dem Modul Event Grid in Edge kann sich der Ereignishandler auf demselben Edgegerät, einem anderen Gerät oder in der Cloud befinden. Sie können Ereignisse mit einem beliebigen Webhook behandeln, oder Sie können sie an einen der nativen Handler wie Azure Event Grid senden.

Dieser Artikel enthält Informationen zum Konfigurieren der einzelnen Optionen.

## <a name="webhook"></a>WebHook

Legen Sie zum Veröffentlichen eines Webhook-Endpunkts `endpointType` auf `WebHook` fest, und geben Sie Folgendes an:

* endpointUrl: Die URL des Webhook-Endpunkts

    ```json
        {
          "properties": {
            "destination": {
              "endpointType": "WebHook",
              "properties": {
                "endpointUrl": "<your-webhook-endpoint>"
              }
            }
          }
        }
    ```

## <a name="azure-event-grid"></a>Azure Event Grid

Legen Sie zum Veröffentlichen auf einem Azure Event Grid-Cloudendpunkt `endpointType` auf `eventGrid` fest, und geben Sie Folgendes an:

* endpointUrl: Event Grid-Thema-URL in der Cloud
* sasKey: SAS-Schlüssel des Event Grid-Themas
* topicName: Name, mit dem alle ausgehenden Ereignisse an Event Grid versehen werden. Der Themaname ist nützlich, wenn die Veröffentlichung in einem Event Grid-Domänenthema erfolgt.

   ```json
        {
          "properties": {
            "destination": {
              "endpointType": "eventGrid",
              "properties": {
                 "endpointUrl": "<your-event-grid-cloud-topic-endpoint-url>?api-version=2018-01-01",
                 "sasKey": "<your-event-grid-topic-saskey>",
                 "topicName": null
              }
            }
          }
    }
   ```

## <a name="iot-edge-hub"></a>IoT Edge-Hub

Legen Sie zum Veröffentlichen in einem Edge Hub-Modul `endpointType` auf `edgeHub` fest, und geben Sie Folgendes an:

* outputName: Die Ausgabe, an die das Event Grid-Modul Ereignisse weiterleitet, die dem edgeHub-Abonnement entsprechen. Beispielsweise werden Ereignisse, die dem folgenden Abonnement entsprechen, in „/messages/modules/eventgridmodule/outputs/sampleSub4“ geschrieben.

    ```json
        {
          "properties": {
            "destination": {
              "endpointType": "edgeHub",
              "properties": {
                "outputName": "sampleSub4"
              }
            }
          }
        }
    ```

## <a name="event-hubs"></a>Event Hubs

Legen Sie zum Veröffentlichen in einem Event Hub `endpointType` auf `eventHub` fest, und geben Sie Folgendes an:

* connectionString: Die Verbindungszeichenfolge für den jeweiligen Event Hub, den Sie als Ziel verwenden, die über eine SAS-Richtlinie generiert wird.

    >[!NOTE]
    > Die Verbindungszeichenfolge muss für die Entität spezifisch sein. Die Verwendung einer Namespace-Verbindungszeichenfolge funktioniert nicht. Sie können eine entitätsspezifische Verbindungszeichenfolge generieren, indem Sie zum jeweiligen Event Hub navigieren, in dem Sie im Azure-Portal veröffentlichen möchten, und dann auf **SAS-Richtlinien** klicken, um eine neue entitätsspezifische Verbindungszeichenfolge zu generieren.

    ```json
        {
          "properties": {
            "destination": {
              "endpointType": "eventHub",
              "properties": {
                "connectionString": "<your-event-hub-connection-string>"
              }
            }
          }
        }
    ```

## <a name="service-bus-queues"></a>Service Bus-Warteschlangen

Legen Sie zum Veröffentlichen in einer Service Bus-Warteschlange `endpointType` auf `serviceBusQueue` fest, und geben Sie Folgendes an:

* connectionString: Die Verbindungszeichenfolge für die jeweilige Service Bus-Warteschlange, die Sie als Ziel verwenden, die über eine SAS-Richtlinie generiert wird.

    >[!NOTE]
    > Die Verbindungszeichenfolge muss für die Entität spezifisch sein. Die Verwendung einer Namespace-Verbindungszeichenfolge funktioniert nicht. Generieren Sie eine entitätsspezifische Verbindungszeichenfolge, indem Sie zu der jeweiligen Service Bus-Warteschlange navigieren, in der Sie im Azure-Portal veröffentlichen möchten. Klicken Sie dann auf **SAS-Richtlinien**, um eine neue entitätsspezifische Verbindungszeichenfolge zu generieren.

    ```json
        {
          "properties": {
            "destination": {
              "endpointType": "serviceBusQueue",
              "properties": {
                "connectionString": "<your-service-bus-queue-connection-string>"
              }
            }
          }
        }
    ```

## <a name="service-bus-topics"></a>Service Bus-Themen

Legen Sie zum Veröffentlichen in einem Service Bus-Thema `endpointType` auf `serviceBusTopic` fest, und geben Sie Folgendes an:

* connectionString: Die Verbindungszeichenfolge für das jeweilige Service Bus-Thema, das Sie als Ziel verwenden, die über eine SAS-Richtlinie generiert wird.

    >[!NOTE]
    > Die Verbindungszeichenfolge muss für die Entität spezifisch sein. Die Verwendung einer Namespace-Verbindungszeichenfolge funktioniert nicht. Generieren Sie eine entitätsspezifische Verbindungszeichenfolge, indem Sie zum jeweiligen Service Bus-Thema navigieren, in dem Sie im Azure-Portal veröffentlichen möchten, und klicken Sie dann auf **SAS-Richtlinien**, um eine neue entitätsspezifische Verbindungszeichenfolge zu generieren.

    ```json
        {
          "properties": {
            "destination": {
              "endpointType": "serviceBusTopic",
              "properties": {
                "connectionString": "<your-service-bus-topic-connection-string>"
              }
            }
          }
        }
    ```

## <a name="storage-queues"></a>Storage-Warteschlangen

Legen Sie zum Veröffentlichen in einer Storage-Warteschlange `endpointType` auf `storageQueue` fest, und geben Sie Folgendes an:

* queueName: Der Name der Storage-Warteschlange, in der Sie veröffentlichen.
* connectionString: Die Verbindungszeichenfolge für das Speicherkonto, in dem sich die Storage-Warteschlange befindet.

    >[!NOTE]
    > Im Gegensatz zu Event Hubs, Service Bus-Warteschlangen und Service Bus-Themen ist die für Storage-Warteschlangen verwendete Verbindungszeichenfolge nicht entitätsspezifisch. Stattdessen muss dies die Verbindungszeichenfolge für das Speicherkonto sein.

    ```json
        {
          "properties": {
            "destination": {
              "endpointType": "storageQueue",
              "properties": {
                "queueName": "<your-storage-queue-name>",
                "connectionString": "<your-storage-account-connection-string>"
              }
            }
          }
        }
    ```