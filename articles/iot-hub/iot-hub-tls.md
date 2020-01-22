---
title: Azure IoT Hub TLS-Unterstützung
description: Bewährte Methoden bei der Verwendung von sicheren TLS-Verbindungen für Geräte und Dienste, die mit IoT Hub kommunizieren
services: iot-hub
author: rezasherafat
ms.service: iot-fundamentals
ms.topic: conceptual
ms.date: 01/10/2020
ms.author: rezas
ms.custom: Azure IoT Hub TLS
ms.openlocfilehash: eb6b04a476ac6100962e1103af37d75b719dd546
ms.sourcegitcommit: f34165bdfd27982bdae836d79b7290831a518f12
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/13/2020
ms.locfileid: "75921242"
---
# <a name="tls-support-in-iot-hub"></a>TLS-Unterstützung in IoT Hub

IoT Hub verwendet Transport Layer Security (TLS) zum Sichern von Verbindungen mit IoT-Geräten und -Diensten. Derzeit werden drei Versionen des TLS-Protokolls unterstützt, nämlich die Versionen 1.0, 1.1 und 1.2.

TLS 1.0 und 1.1 werden als Legacy betrachtet und [in Kürze eingestellt](./tls-1.2-everywhere.md). Daher wird dringend empfohlen, beim Herstellen einer Verbindung mit IoT Hub TLS 1.2 als bevorzugte TLS-Version zu verwenden.


## <a name="restrict-connections-to-tls-12-in-your-iot-hub-resource"></a>Einschränken von Verbindungen mit TLS 1.2 in ihrer IoT Hub-Ressource

Zur weiteren Sicherheit sollten Sie Ihre IoT Hubs so konfigurieren, dass _nur_ Clientverbindungen zugelassen werden, die TLS-Version 1.2 verwenden, und Sie sollten die Verwendung von [empfohlenen Verschlüsselungen](#recommended-ciphers) erzwingen.

Stellen Sie zu diesem Zweck einen neuen IoT Hub in einer der [unterstützten Regionen](#supported-regions) bereit, und legen Sie die Eigenschaft `minTlsVersion` in der IoT Hub-Ressourcenspezifikation Ihrer Azure Resource Manager-Vorlage auf `1.2` fest:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "type": "Microsoft.Devices/IotHubs",
            "apiVersion": "2020-01-01",
            "name": "<provide-a-valid-resource-name>",
            "location": "<any-of-supported-regions-below>",
            "properties": {
                "minTlsVersion": "1.2"
            },
            "sku": {
                "name": "<your-hubs-SKU-name>",
                "tier": "<your-hubs-SKU-tier>",
                "capacity": 1
            }
        }
    ]
}
```

Die erstellte IoT Hub-Ressource, die diese Konfiguration verwendet, wird Geräte- und Dienstclients ablehnen, die versuchen, eine Verbindung mit den TLS-Versionen 1.0 und 1.1 herzustellen. Analog dazu wird der TLS-Handshake abgelehnt, wenn die HELLO-Nachricht des Clients keine der [empfohlenen Verschlüsselungen](#recommended-ciphers) enthält.

Beachten Sie, dass die Eigenschaft `minTlsVersion` schreibgeschützt ist und nach Erstellung Ihrer IoT Hub-Ressource nicht mehr geändert werden kann. Daher ist es von entscheidender Bedeutung, dass Sie im Voraus ordnungsgemäß testen und überprüfen, ob _alle_ Ihre IoT-Geräte und -Dienste mit TLS 1.2 und den [empfohlenen Verschlüsselungen](#recommended-ciphers) kompatibel sind.


### <a name="supported-regions"></a>Unterstützte Regionen

IoT Hubs, bei denen TLS 1.2 verwendet werden muss, können in den folgenden Regionen erstellt werden:

* East US
* USA Süd Mitte
* USA, Westen 2

> [!NOTE]
> Nach einem Failover bleibt die Eigenschaft `minTlsVersion` Ihres IoT Hubs in der geografisch gekoppelten Region wirksam.



### <a name="recommended-ciphers"></a>Empfohlene Verschlüsselungen

IoT Hubs, die so konfiguriert sind, dass nur TLS 1.2 akzeptiert wird, erzwingen außerdem die Verwendung der folgenden empfohlenen Verschlüsselungen:

* `TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256`
* `TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384`
* `TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256`
* `TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384`


### <a name="use-tls-12-in-your-iot-hub-sdks"></a>Verwenden von TLS 1.2 in Ihren IoT Hub-SDKs

Verwenden Sie die nachstehenden Links zum Konfigurieren von TLS 1.2 und zulässigen Verschlüsselungen in IoT Hub-Client-SDKs.

| Sprache | TLS 1.2 wird unterstützt | Dokumentation |
|----------|-------------------|---------------|
| C        | Ja               | [Link](https://aka.ms/Tls_C_SDK_IoT) |
| Python   | Ja               | [Link](https://aka.ms/Tls_Python_SDK_IoT) |
| C#       | Ja               | [Link](https://aka.ms/Tls_CSharp_SDK_IoT) |
| Java     | Ja               | [Link](https://aka.ms/Tls_Java_SDK_IoT) |
| NodeJS   | Ja               | [Link](https://aka.ms/Tls_Node_SDK_IoT) |


### <a name="use-tls-12-in-your-iot-edge-setup"></a>Verwenden von TLS 1.2 in Ihrer IoT Edge-Installation

IoT Edge-Geräte können für die Verwendung von TLS 1.2 bei der Kommunikation mit IoT Hub konfiguriert werden. Verwenden Sie dazu die [IoT Edge Dokumentationsseite](https://github.com/Azure/iotedge/blob/master/edge-modules/edgehub-proxy/README.md).