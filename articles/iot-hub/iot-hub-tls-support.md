---
title: Azure IoT Hub TLS-Unterstützung
description: Bewährte Methoden bei der Verwendung von sicheren TLS-Verbindungen für Geräte und Dienste, die mit IoT Hub kommunizieren
services: iot-hub
author: rezasherafat
ms.service: iot-fundamentals
ms.topic: conceptual
ms.date: 01/10/2020
ms.author: rezas
ms.openlocfilehash: 5b9f6b993b0d0f527d041b4ee055bf51fefa1253
ms.sourcegitcommit: 1f25aa993c38b37472cf8a0359bc6f0bf97b6784
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2020
ms.locfileid: "83848244"
---
# <a name="tls-support-in-iot-hub"></a>TLS-Unterstützung in IoT Hub

IoT Hub verwendet Transport Layer Security (TLS) zum Sichern von Verbindungen mit IoT-Geräten und -Diensten. Derzeit werden drei Versionen des TLS-Protokolls unterstützt, nämlich die Versionen 1.0, 1.1 und 1.2.

TLS 1.0 und 1.1 werden als Legacy betrachtet und in Kürze eingestellt. Weitere Informationen finden Sie unter [Deprecating TLS 1.0 and 1.1 for IoT Hub](iot-hub-tls-deprecating-1-0-and-1-1.md) (Einstellung von TLS 1.0 and 1.1 für IoT Hub). Es wird dringend empfohlen, beim Herstellen einer Verbindung mit IoT Hub TLS 1.2 als bevorzugte TLS-Version zu verwenden.

## <a name="restrict-connections-to-tls-12-in-your-iot-hub-resource"></a>Einschränken von Verbindungen mit TLS 1.2 in ihrer IoT Hub-Ressource

Zur weiteren Sicherheit sollten Sie Ihre IoT Hubs so konfigurieren, dass *nur* Clientverbindungen zugelassen werden, die TLS-Version 1.2 verwenden, und Sie sollten die Verwendung von [empfohlenen Verschlüsselungen](#recommended-ciphers) erzwingen.

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

> [!NOTE]
> Die Eigenschaft `minTlsVersion` ist schreibgeschützt und kann nach Erstellung Ihrer IoT Hub-Ressource nicht mehr geändert werden. Daher ist es von entscheidender Bedeutung, dass Sie im Voraus ordnungsgemäß testen und überprüfen, ob *alle* Ihre IoT-Geräte und -Dienste mit TLS 1.2 und den [empfohlenen Verschlüsselungen](#recommended-ciphers) kompatibel sind.

## <a name="supported-regions"></a>Unterstützte Regionen

IoT Hubs, bei denen TLS 1.2 verwendet werden muss, können in den folgenden Regionen erstellt werden:

* East US
* USA Süd Mitte
* USA, Westen 2
* US Gov Arizona
* US Government, Virginia

> [!NOTE]
> Nach einem Failover bleibt die Eigenschaft `minTlsVersion` Ihres IoT Hubs in der geografisch gekoppelten Region wirksam.

## <a name="recommended-ciphers"></a>Empfohlene Verschlüsselungen

IoT Hubs, die so konfiguriert sind, dass nur TLS 1.2 akzeptiert wird, erzwingen außerdem die Verwendung der folgenden empfohlenen Verschlüsselungen:

* `TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256`
* `TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384`
* `TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256`
* `TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384`

Bei IoT-Hubs, die nicht zum Erzwingen von TLS 1.2 konfiguriert sind, funktioniert TLS 1.2 weiterhin mit den folgenden Verschlüsselungen:

* `TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256`
* `TLS_DHE_RSA_WITH_AES_256_GCM_SHA384`
* `TLS_DHE_RSA_WITH_AES_128_GCM_SHA256`
* `TLS_RSA_WITH_AES_256_GCM_SHA384`
* `TLS_RSA_WITH_AES_128_GCM_SHA256`
* `TLS_RSA_WITH_AES_256_CBC_SHA256`
* `TLS_RSA_WITH_AES_128_CBC_SHA256`
* `TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA`
* `TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA`
* `TLS_RSA_WITH_AES_256_CBC_SHA`
* `TLS_RSA_WITH_AES_128_CBC_SHA`
* `TLS_RSA_WITH_3DES_EDE_CBC_SHA`

## <a name="use-tls-12-in-your-iot-hub-sdks"></a>Verwenden von TLS 1.2 in Ihren IoT Hub-SDKs

Verwenden Sie die nachstehenden Links zum Konfigurieren von TLS 1.2 und zulässigen Verschlüsselungen in IoT Hub-Client-SDKs.

| Sprache | Versionen mit Unterstützung von TLS 1.2 | Dokumentation |
|----------|------------------------------------|---------------|
| C        | Tag 2019-12-11 oder höher            | [Link](https://aka.ms/Tls_C_SDK_IoT) |
| Python   | Version 2.0.0 oder höher             | [Link](https://aka.ms/Tls_Python_SDK_IoT) |
| C#       | Version 1.21.4 oder höher            | [Link](https://aka.ms/Tls_CSharp_SDK_IoT) |
| Java     | Version 1.19.0 oder höher            | [Link](https://aka.ms/Tls_Java_SDK_IoT) |
| NodeJS   | Version 1.12.2 oder höher            | [Link](https://aka.ms/Tls_Node_SDK_IoT) |


## <a name="use-tls-12-in-your-iot-edge-setup"></a>Verwenden von TLS 1.2 in Ihrer IoT Edge-Installation

IoT Edge-Geräte können für die Verwendung von TLS 1.2 bei der Kommunikation mit IoT Hub konfiguriert werden. Verwenden Sie dazu die [IoT Edge Dokumentationsseite](https://github.com/Azure/iotedge/blob/master/edge-modules/edgehub-proxy/README.md).