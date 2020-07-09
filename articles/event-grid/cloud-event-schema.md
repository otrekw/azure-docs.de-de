---
title: Verwenden von Azure Event Grid mit Ereignissen im CloudEvents-Schema
description: Beschreibt die Verwendung des CloudEvents-Schemas für Ereignisse in Azure Event Grid. Der Dienst unterstützt Ereignisse in der JSON-Implementierung von Cloudereignissen.
services: event-grid
author: femila
ms.service: event-grid
ms.topic: conceptual
ms.date: 01/21/2020
ms.author: femila
ms.openlocfilehash: 57827b1c5a43a3408d374e8bddb7b91113b2929a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84560690"
---
# <a name="cloudevents-v10-schema-with-event-grid"></a>CloudEvents 1.0-Schema mit Event Grid

Zusätzlich zu seinem [Standardereignisschema](event-schema.md) unterstützt Azure Event Grid nativ Ereignisse in der [JSON-Implementierung von CloudEvents 1.0](https://github.com/cloudevents/spec/blob/v1.0/json-format.md) und in der [HTTP-Bindung](https://github.com/cloudevents/spec/blob/v1.0/http-protocol-binding.md). [CloudEvents](https://cloudevents.io/) ist eine [offene Spezifikation](https://github.com/cloudevents/spec/blob/v1.0/spec.md) zur Beschreibung von Ereignisdaten.

CloudEvents vereinfacht die Interoperabilität, indem ein allgemeines Ereignisschema für die Veröffentlichung bereitgestellt wird und cloudbasierte Ereignisse genutzt werden. Dieses Schema ermöglicht einheitliche Tools und Standardmöglichkeiten für Routing und Behandlung von Ereignissen sowie universelle Möglichkeiten, das äußere Ereignisschema zu deserialisieren. Mit einem allgemeinen Schema können Sie Arbeit leichter plattformübergreifend integrieren.

CloudEvents wird von mehreren [Partnern](https://github.com/cloudevents/spec/blob/master/community/contributors.md), einschließlich Microsoft, über die [Cloud Native Computing Foundation](https://www.cncf.io/) entwickelt. Es ist derzeit in Version 1.0 verfügbar.

In diesem Artikel wird das CloudEvents-Schema mit Event Grid beschrieben.

## <a name="sample-event-using-cloudevents-schema"></a>Beispielereignis mit CloudEvents-Schema

Hier ist ein Beispiel für ein Azure Blob Storage-Ereignis im CloudEvents-Format:

``` JSON
{
    "specversion": "1.0",
    "type": "Microsoft.Storage.BlobCreated",  
    "source": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-account}",
    "id": "9aeb0fdf-c01e-0131-0922-9eb54906e209",
    "time": "2019-11-18T15:13:39.4589254Z",
    "subject": "blobServices/default/containers/{storage-container}/blobs/{new-file}",
    "dataschema": "#",
    "data": {
        "api": "PutBlockList",
        "clientRequestId": "4c5dd7fb-2c48-4a27-bb30-5361b5de920a",
        "requestId": "9aeb0fdf-c01e-0131-0922-9eb549000000",
        "eTag": "0x8D76C39E4407333",
        "contentType": "image/png",
        "contentLength": 30699,
        "blobType": "BlockBlob",
        "url": "https://gridtesting.blob.core.windows.net/testcontainer/{new-file}",
        "sequencer": "000000000000000000000000000099240000000000c41c18",
        "storageDiagnostics": {
            "batchId": "681fe319-3006-00a8-0022-9e7cde000000"
        }
    }
}
```

[Hier](https://github.com/cloudevents/spec/blob/v1.0/spec.md#required-attributes) finden Sie eine ausführliche Beschreibung der verfügbaren Felder, Typen und Definitionen in CloudEvents v1.0.

Die im CloudEvents- und Event Grid-Schema übermittelten Headerwerte für Ereignisse sind mit Ausnahme von `content-type` identisch. Für das CloudEvents-Schema ist dieser Headerwert `"content-type":"application/cloudevents+json; charset=utf-8"`. Für das Event Grid-Schema ist dieser Headerwert `"content-type":"application/json; charset=utf-8"`.

## <a name="event-grid-for-cloudevents"></a>Event Grid für CloudEvents

Sie können Event Grid für die Eingabe und Ausgabe von Ereignissen im CloudEvents-Schema verwenden. Sie können CloudEvents für Systemereignisse wie Blob Storage-Ereignisse und IoT Hub-Ereignisse sowie benutzerdefinierte Ereignisse verwenden. Diese Ereignisse können auch bei der Übertragung hin und her transformiert werden.


| Eingabeschema       | Ausgabeschema
|--------------------|---------------------
| CloudEvents-Format | CloudEvents-Format
| Event Grid-Format  | CloudEvents-Format
| Event Grid-Format  | Event Grid-Format

Für alle Ereignisschemas setzt Event Grid beim Veröffentlichen in einem Event Grid-Thema und Erstellen eines Ereignisabonnements eine Überprüfung voraus. Weitere Informationen finden Sie unter [Event Grid – Sicherheit und Authentifizierung](security-authentication.md).

## <a name="next-steps"></a>Nächste Schritte
Lesen Sie [Verwenden des CloudEvents 1.0-Schemas mit Event Grid](cloudevents-schema.md).  
