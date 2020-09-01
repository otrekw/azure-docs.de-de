---
title: 'Datenvertrag für die gRPC-Erweiterung: Azure'
description: Dieser Artikel enthält Informationen zur Verwendung des gRPC-Protokolls zum Senden von Nachrichten zwischen dem Live Video Analytics-Modul und Ihrer benutzerdefinierten KI- oder CV-Erweiterung.
ms.topic: overview
ms.date: 09/14/2020
ms.openlocfilehash: 0221d20245a6db69791d8bf13ba9e00de3b96ecc
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2020
ms.locfileid: "88687228"
---
# <a name="grpc-extension-data-contract"></a>Datenvertrag für die gRPC-Erweiterung

Dieser Artikel enthält Informationen zur Verwendung des gRPC-Protokolls zum Senden von Nachrichten zwischen dem Live Video Analytics-Modul und Ihrer benutzerdefinierten KI- oder CV-Erweiterung.

gRPC ist ein modernes, Open-Source-basiertes, hochleistungsfähiges RPC-Framework, das in einer beliebigen Umgebung ausgeführt werden kann. Der gRPC-Transportdienst verwendet bidirektionales HTTP/2-Streaming zwischen folgenden Komponenten:

* gRPC-Client (Modul „Live Video Analytics in IoT Edge“) und 
* gRPC-Server (Ihre benutzerdefinierte Erweiterung)

Eine gRPC-Sitzung ist eine einzelne Verbindung zwischen gRPC-Client und gRPC-Server über den TCP/TLS-Port. 

In einer einzelnen Sitzung geschieht Folgendes: Der Client sendet einen Mediendatenstrom-Deskriptor sowie Videoframes als Nachricht vom Typ [Protobuf](https://developers.google.com/protocol-buffers) über die gRPC-Streamsitzung an den Server. Der Server überprüft den Datenstromdeskriptor, analysiert den Videoframe und gibt Rückschlussergebnisse als Protobuf-Nachricht zurück.

![Vertrag für die gRPC-Erweiterung](./media/data-contracts/grpc.png)

## <a name="implementing-grpc-protocol"></a>Implementieren des gRPC-Protokolls

### <a name="creating-a-grpc-connection"></a>Erstellen einer gRPC-Verbindung

Von der benutzerdefinierten Erweiterung muss der folgende gRPC-Dienst implementiert werden:

```
service MediaGraphExtension {
  rpc ProcessMediaStream(stream MediaStreamMessage) returns (stream MediaStreamMessage);
}
```

Dadurch wird bei einem Aufruf ein bidirektionaler Datenstrom geöffnet, sodass Nachrichten zwischen der gRPC-Erweiterung und dem Live Video Analytics-Graphen ausgetauscht werden können. Die erste Nachricht, die in diesem Datenstrom von jeder Partei gesendet wird, enthält einen Mediendatenstrom-Deskriptor (MediaStreamDescriptor), um die Informationen zu definieren, die in den folgenden Medienstichproben (MediaSamples) gesendet werden.

So kann beispielsweise die Grapherweiterung eine (hier in JSON ausgedrückte) Nachricht senden, um anzugeben, dass sie RGB24-codierte Frames im Format 416 × 416 sendet, die in die gRPC-Nachrichten an die benutzerdefinierte Erweiterung eingebettet sind.

```
 {
    "sequence_number": 1,
    "ack_sequence_number": 0,
    "media_stream_descriptor": {
        "graph_identifier": {
            "media_services_arm_id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/microsoft.media/mediaservices/mediaAccountName",
            "graph_instance_name": "mediaGraphName",
            "graph_node_name": "grpcExtension"
        },
        "media_descriptor": {
            "timescale": 90000,
            "video_frame_sample_format": {
                "encoding": "RAW",
                "pixel_format": "RGB24",
                "dimensions": {
                    "width": 416,
                    "height": 416
                },
                "stride_bytes": 1248
            }
        }
    }
}
```

Die benutzerdefinierte Erweiterung würde daraufhin mit folgender Nachricht antworten, um anzugeben, dass sie nur Rückschlüsse zurückgibt.

```
{
    "sequence_number": 1,
    "ack_sequence_number": 1,
    "media_stream_descriptor": {
        "extension_identifier": "customExtensionName"    }
}
```

Nachdem beide Seiten Mediendeskriptoren ausgetauscht haben, beginnt Live Video Analytics damit, Frames an die Erweiterung zu übertragen.

### <a name="sequence-numbers"></a>Sequenznummern

Der gRPC-Erweiterungsknoten und die benutzerdefinierte Erweiterung verfügen jeweils über eigene Sequenznummern, die ihren Nachrichten zugewiesen werden. Diese Sequenznummern sollten mit „1“ beginnen und monoton ansteigen. `ack_sequence_number` kann ignoriert werden, wenn keine Nachricht bestätigt wird. Dies kann beim Senden der ersten Nachricht vorkommen.

Eine Anforderung muss bestätigt werden, nachdem die entsprechende Nachricht vollständig verarbeitet wurde. Bei einer Übertragung mit gemeinsam genutztem Arbeitsspeicher wird dem Absender durch diese Bestätigung signalisiert, dass der gemeinsam genutzte Arbeitsspeicher freigegeben werden kann und dass der Empfänger nicht mehr darauf zugreift. Gemeinsam genutzter Arbeitsspeicher muss vom Absender bis zur Bestätigung vorgehalten werden.

### <a name="reading-embedded-content"></a>Lesen eingebetteter Inhalte

Eingebettete Inhalte können über das Feld `content_byte` direkt aus der Nachricht vom Typ `MediaSample` gelesen werden. Die Daten werden gemäß dem Mediendeskriptor (`MediaDescriptor`) codiert.

### <a name="reading-content-from-shared-memory"></a>Lesen von Inhalten aus gemeinsam genutztem Arbeitsspeicher

Wenn Inhalte über gemeinsam genutzten Arbeitsspeicher übertragen werden, schließt der Absender bei der ersten Sitzungseinrichtung `SharedMemoryBufferTransferProperties` in `MediaStreamDescriptor` ein. Das sieht dann in etwa wie folgt aus (ausgedrückt in JSON):

```
{
  "handle_name": "inference_client_share_memory_2146989006636459346"
  "length_bytes": 20971520
}
```

Der Empfänger öffnet daraufhin die Datei `/dev/shm/inference_client_share_memory_2146989006636459346`. Der Absender sendet eine Nachricht vom Typ `MediaSample` mit einem Inhaltsverweis (`ContentReference`), der auf eine bestimmte Stelle in dieser Datei verweist.

```
{
    "timestamp": 143598615750000,
    "content_reference": {
        "address_offset": 519168,
        "length_bytes": 173056
    }
}
```

Anschließend liest der Empfänger die Daten, die sich an diesem Ort in der Datei befinden.

Damit der Live Video Analytics-Container über gemeinsam genutzten Arbeitsspeicher kommunizieren kann, muss der IPC-Modus des Containers ordnungsgemäß konfiguriert werden. Dafür gibt es mehrere Möglichkeiten. Hier finden Sie einige empfohlene Konfigurationen:

* Bei der Kommunikation mit einer auf dem Hostgerät ausgeführten gRPC-Rückschlussengine sollte der IPC-Modus auf „host“ festgelegt werden.
* Bei der Kommunikation mit einem gRPC-Server, der in einem anderen IoT Edge-Modul ausgeführt wird, sollte der IPC-Modus für das Live Video Analytics-Modul auf „shareable“ festgelegt werden. Außerdem sollte `container:liveVideoAnalytics` für die benutzerdefinierte Erweiterung festgelegt werden, wobei `liveVideoAnalytics` der Name des Live Video Analytics-Moduls ist.

Das folgende Beispiel zeigt, wie dies bei Verwendung der ersten Option im Gerätezwilling aussehen kann:

```
"liveVideoAnalytics": {
  "version": "1.0",
  "type": "docker",
  "status": "running",
  "restartPolicy": "always",
  "settings": {
    "image": "mcr.microsoft.com/media/live-video-analytics:1",
    "createOptions": 
      "HostConfig": {
        "IpcMode": "host"
      }
    }
  }
}
```

Weitere Informationen zu IPC-Modi finden Sie unter https://docs.docker.com/engine/reference/run/#ipc-settings---ipc.

## <a name="media-graph-grpc-extension-contract-definitions"></a>Definitionen von gRPC-Erweiterungsverträgen für Mediengraphen

In diesem Abschnitt wird der gRPC-Vertrag definiert, der den Datenfluss definiert.

### <a name="protocol-messages"></a>Protokollnachrichten

![Protokollnachrichten](./media/data-contracts/grpc2.png)
 
### <a name="client-authentication"></a>Clientauthentifizierung

Implementierer benutzerdefinierter Erweiterungen können die Authentizität eingehender gRPC-Verbindungen überprüfen, um sicherzustellen, dass sie vom gRPC-Erweiterungsknoten stammen. Vom Knoten wird ein Eintrag in den Anforderungsheadern bereitgestellt, der für die Überprüfung verwendet werden kann.

Hierzu können Anmeldeinformationen (Benutzername/Kennwort) verwendet werden. Die Anmeldeinformationen werden bei der Erstellung eines gRPC-Erweiterungsknotens wie folgt angegeben:

```
{
  "@type": "#Microsoft.Media.MediaGraphGrpcExtension",
  "name": "{moduleIdentifier}",
  "endpoint": {
    "@type": "#Microsoft.Media.MediaGraphUnsecuredEndpoint",
    "url": "tcp://customExtension:8081",
    "credentials": {
      "@type": "#Microsoft.Media.MediaGraphUsernamePasswordCredentials",
      "username": "username",
      "password": "password"
    }
  }
  // Other fields omitted
}
```

Wenn die gRPC-Anforderung gesendet wird, wird der folgende Header in die Anforderungsmetadaten eingeschlossen, um eine HTTP-Standardauthentifizierung zu imitieren:

`x-ms-authentication: Basic (Base64 Encoded username:password)`

## <a name="using-grpc-over-tls"></a>Verwenden von gRPC über TLS

Eine für Rückschlüsse verwendete gRPC-Verbindung kann über TLS geschützt werden. Dies ist hilfreich, wenn die Sicherheit des Netzwerks zwischen Live Video Analytics und der Rückschlussengine nicht garantiert werden kann. Von TLS werden alle Inhalte verschlüsselt, die in die gRPC-Nachrichten eingebettet sind, was eine höhere CPU-Auslastung zur Folge hat, wenn Frames mit einer hohen Rate übertragen werden.

Die Überprüfungsoptionen „IgnoreHostname“ und „IgnoreSignature“ werden von gRPC nicht unterstützt. Daher muss das von der Rückschlussengine präsentierte Serverzertifikat einen CN enthalten, der exakt der IP-Adresse/dem Hostnamen in der Endpunkt-URL des GRPC-Erweiterungsknotens entspricht.

## <a name="next-steps"></a>Nächste Schritte

[Schema für Rückschlussmetadaten](inference-metadata-schema.md)
