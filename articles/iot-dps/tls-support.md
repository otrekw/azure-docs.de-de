---
title: Azure IoT Device Provisioning Service (DPS) – TLS-Unterstützung
description: Bewährte Methoden bei der Verwendung sicherer TLS-Verbindungen für Geräte und Dienste, die mit dem IoT Device Provisioning Service (DPS) kommunizieren
services: iot-dps
author: wesmc7777
ms.service: iot-dps
ms.topic: conceptual
ms.date: 06/18/2020
ms.author: wesmc
ms.openlocfilehash: 9a90180fa606b14b06c94d3211fdf492add0350d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85564967"
---
# <a name="tls-support-in-azure-iot-hub-device-provisioning-service-dps"></a>TLS-Unterstützung in Azure IoT Hub Device Provisioning Service (DPS)

DPS verwendet [Transport Layer Security (TLS)](http://wikipedia.org/wiki/Transport_Layer_Security) zum Sichern von Verbindungen mit IoT-Geräten. 

Diese aktuellen TLS-Protokollversionen werden von DPS unterstützt: 
* TLS 1.2

TLS 1.0 und 1.1 werden als Legacy betrachtet und in Kürze eingestellt. Weitere Informationen finden Sie unter [Deprecating TLS 1.0 and 1.1 for IoT Hub](../iot-hub/iot-hub-tls-deprecating-1-0-and-1-1.md) (Einstellung von TLS 1.0 and 1.1 für IoT Hub). 

## <a name="restrict-connections-to-tls-12"></a>Beschränken von Verbindungen auf TLS 1.2

Zur weiteren Sicherheit sollten Sie Ihre DPS-Instanzen so konfigurieren, dass *nur* Geräteclientverbindungen zugelassen werden, die TLS-Version 1.2 verwenden, und Sie sollten die Verwendung von [empfohlenen Verschlüsselungen](#recommended-ciphers) erzwingen.

Legen Sie dazu eine neue DPS-Ressourceneinstellung der `minTlsVersion`-Eigenschaft für `1.2` in der DPS-Ressourcenspezifikation Ihrer Azure Resource Manager-Vorlage fest. Im folgenden Beispiel einer JSON-Vorlage wird die Eigenschaft `minTlsVersion` für eine neue DPS-Instanz angegeben.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "type": "Microsoft.Devices/ProvisioningServices",
            "apiVersion": "2020-01-01",
            "name": "<provide-a-valid-DPS-resource-name>",
            "location": "<any-region>",
            "properties": {
                "minTlsVersion": "1.2"
            },
            "sku": {
                "name": "S1",
                "capacity": 1
            },
        }     
    ]
}
```

Sie können die Vorlage mit dem folgenden Azure CLI-Befehl bereitstellen. 

```azurecli
az deployment group create -g <your resource group name> --template-file template.json
```

Weitere Informationen zum Erstellen von DPS-Ressourcen mit Resource Manager-Vorlagen finden Sie unter [Einrichten von DPS mit einer Azure Resource Manager-Vorlage](quick-setup-auto-provision-rm.md).

Die mit dieser Konfiguration erstellte DPS-Ressource lehnt Geräte ab, die versuchen, mithilfe der TLS-Versionen 1.0 und 1.1 eine Verbindung herzustellen. Analog dazu wird der TLS-Handshake abgelehnt, wenn die HELLO-Nachricht des Geräteclients keine der [empfohlenen Verschlüsselungen](#recommended-ciphers) enthält.

> [!NOTE]
> Die Eigenschaft `minTlsVersion` ist schreibgeschützt und kann nach Erstellung Ihrer DPS-Ressource nicht mehr geändert werden. Daher ist es von entscheidender Bedeutung, dass Sie im Voraus ordnungsgemäß testen und überprüfen, ob *alle* Ihre IoT-Geräte mit TLS 1.2 und den [empfohlenen Verschlüsselungen](#recommended-ciphers) kompatibel sind.


> [!NOTE]
> Nach einem Failover bleibt die Eigenschaft `minTlsVersion` Ihres DPS in der geografisch gekoppelten Region wirksam.

## <a name="recommended-ciphers"></a>Empfohlene Verschlüsselungen

DPS-Instanzen, die so konfiguriert sind, dass nur TLS 1.2 akzeptiert wird, erzwingen außerdem die Verwendung der folgenden Verschlüsselungssammlung:

### <a name="tls-12-cipher-suites"></a>TLS 1.2-Verschlüsselungssammlungen

| Minimum |
| :--- |
| `TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256`<br>`TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384`<br>`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256`<br>`TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384`<br>`TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256`<br>`TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384`<br>`TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256`<br>`TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384` |

| Chance für herausragende Leistung |
| :--- |
| `TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384`<br>`TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256`<br>`TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384`<br>`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256`<br>`TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384`<br>`TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256`<br>`TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384`<br>`TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256` |

### <a name="cipher-suite-ordering-prior-to-windows-10"></a>Reihenfolge der SSL-Verschlüsselungssammlungen vor Windows 10

| Minimum |
| :--- |
| `TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256_P256`<br>`TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384_P384`<br>`TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256_P256`<br>`TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384_P384`<br>`TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA_P256`<br>`TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA_P384`<br>`TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA_P256`<br>`TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA_P384` |

| Chance für herausragende Leistung |
| :--- |
| `TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384_P384`<br>`TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256_P256`<br>`TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384_P384`<br>`TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256_P256`<br>`TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA_P384`<br>`TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA_P256`<br>`TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA_P384`<br>`TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA_P256` |

### <a name="legacy-cipher-suites"></a>Legacy-Verschlüsselungssammlungen 

| Option 1 (höhere Sicherheit) |
| :--- |
| `TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA_P384 (uses SHA-1)`<br>`TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA_P256 (uses SHA-1)`<br>`TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA_P384   (uses SHA-1)`<br>`TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA_P256   (uses SHA-1)`<br>`TLS_RSA_WITH_AES_256_GCM_SHA384           (lack of Perfect Forward Secrecy)`<br>`TLS_RSA_WITH_AES_128_GCM_SHA256           (lack of Perfect Forward Secrecy)`<br>`TLS_RSA_WITH_AES_256_CBC_SHA256           (lack of Perfect Forward Secrecy)`<br>`TLS_RSA_WITH_AES_128_CBC_SHA256           (lack of Perfect Forward Secrecy)`<br>`TLS_RSA_WITH_AES_256_CBC_SHA              (uses SHA-1, lack of Perfect Forward Secrecy)`<br>`TLS_RSA_WITH_AES_128_CBC_SHA              (uses SHA-1, lack of Perfect Forward Secrecy)` |

| Option 2 (höhere Leistung) |
| :--- |
| `TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA_P256 (uses SHA-1)`<br>`TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA_P384 (uses SHA-1)`<br>`TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA_P256   (uses SHA-1)`<br>`TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA_P384   (uses SHA-1)`<br>`TLS_RSA_WITH_AES_128_GCM_SHA256           (lack of Perfect Forward Secrecy)`<br>`TLS_RSA_WITH_AES_256_GCM_SHA384           (lack of Perfect Forward Secrecy)`<br>`TLS_RSA_WITH_AES_128_CBC_SHA256           (lack of Perfect Forward Secrecy)`<br>`TLS_RSA_WITH_AES_256_CBC_SHA256           (lack of Perfect Forward Secrecy)`<br>`TLS_RSA_WITH_AES_128_CBC_SHA              (uses SHA-1, lack of Perfect Forward Secrecy)`<br>`TLS_RSA_WITH_AES_256_CBC_SHA              (uses SHA-1, lack of Perfect Forward Secrecy)` |


## <a name="use-tls-12-in-the-iot-sdks"></a>Verwenden von TLS 1.2 in den IoT-SDKs

Verwenden Sie die nachstehenden Links zum Konfigurieren von TLS 1.2 und zulässigen Verschlüsselungen in den Azure IoT-Client-SDKs.

| Sprache | Versionen mit Unterstützung von TLS 1.2 | Dokumentation |
|----------|------------------------------------|---------------|
| C        | Tag 2019-12-11 oder höher            | [Link](https://aka.ms/Tls_C_SDK_IoT) |
| Python   | Version 2.0.0 oder höher             | [Link](https://aka.ms/Tls_Python_SDK_IoT) |
| C#       | Version 1.21.4 oder höher            | [Link](https://aka.ms/Tls_CSharp_SDK_IoT) |
| Java     | Version 1.19.0 oder höher            | [Link](https://aka.ms/Tls_Java_SDK_IoT) |
| NodeJS   | Version 1.12.2 oder höher            | [Link](https://aka.ms/Tls_Node_SDK_IoT) |

## <a name="use-tls-12-with-iot-hub"></a>Verwenden von TLS 1.2 mit IoT Hub

IoT Hub kann für die Verwendung von TLS 1.2 bei der Kommunikation mit Geräten konfiguriert werden. Weitere Informationen finden Sie unter [Deprecating TLS 1.0 and 1.1 for IoT Hub](../iot-hub/iot-hub-tls-deprecating-1-0-and-1-1.md) (Einstellung von TLS 1.0 and 1.1 für IoT Hub).

## <a name="use-tls-12-with-iot-edge"></a>Verwenden von TLS 1.2 bei IoT Edge

IoT Edge-Geräte können für die Verwendung von TLS 1.2 bei der Kommunikation mit IoT Hub und DPS konfiguriert werden. Weitere Informationen finden Sie auf der [Dokumentationsseite zu IoT Edge](https://github.com/Azure/iotedge/blob/master/edge-modules/edgehub-proxy/README.md).