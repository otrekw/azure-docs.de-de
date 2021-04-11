---
title: 'Microsoft Connected Cache: Geschachteltes Azure IoT Edge-Gateway mit zwei Ebenen mit ausgehendem nicht authentifiziertem Proxy | Microsoft-Dokumentation'
titleSuffix: Device Update for Azure IoT Hub
description: 'Tutorial – Microsoft Connected Cache: Geschachteltes Azure IoT Edge-Gateway mit zwei Ebenen mit ausgehendem nicht authentifiziertem Proxy'
author: andyriv
ms.author: andyriv
ms.date: 2/16/2021
ms.topic: tutorial
ms.service: iot-hub-device-update
ms.openlocfilehash: 7facb74cd407c576b2a7b119f19427dcd185f04e
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105568816"
---
# <a name="microsoft-connected-cache-preview-deployment-scenario-sample-two-level-nested-azure-iot-edge-gateway-with-outbound-unauthenticated-proxy"></a>Beispiel für ein Szenario zur Bereitstellung von Microsoft Connected Cache: Geschachteltes Azure IoT Edge-Gateway mit zwei Ebenen ausgehendem nicht authentifiziertem Proxy

In diesem Szenario gibt es ein Azure IoT Edge-Gateway und ein Azure IoT Edge-Downstreamgerät, ein Azure IoT Edge-Gateway, das einem anderen Azure IoT Edge-Gateway übergeordnet ist, und einen Proxyserver in der IT-DMZ. Nachfolgend finden Sie ein Beispiel für die Microsoft Connected Cache-Umgebungsvariablen, die auf der Benutzeroberfläche des Azure-Portals für die beiden MCC-Module, die für die Azure IoT Edge-Gateways bereitgestellt werden, festgelegt werden würden. Das gezeigte Beispiel veranschaulicht die Konfiguration für Azure IoT Edge-Gateways auf zwei Ebenen, aber es gibt keinen Grenzwert für die Tiefe der Upstreamhosts, die von Microsoft Connected Cache unterstützt werden. Es gibt keinen Unterschied bei den Optionen zum Erstellen von MCC-Containern aus den vorherigen Beispielen.

Weitere Informationen zur Konfiguration von mehrstufigen Bereitstellungen von Azure IoT Edge-Gateways finden Sie in der Dokumentation [Verbinden nachgeschalteter IoT Edge-Geräte – Azure IoT Edge](../iot-edge/how-to-connect-downstream-iot-edge-device.md?preserve-view=true&tabs=azure-portal&view=iotedge-2020-11). Beachten Sie außerdem, dass sich bei der Bereitstellung von Azure IoT Edge-, Microsoft Connected Cache- und benutzerdefinierten Modulen alle Module in derselben Containerregistrierung befinden müssen.

Das folgende Diagramm beschreibt das Szenario, in dem ein Azure IoT Edge-Gateway als direkter Zugriff auf CDN-Ressourcen als übergeordnetes Gateway für ein anderes Azure IoT Edge-Gateway fungiert, das als übergeordnetes Gerät für ein Azure IoT-Blattgerät wie Raspberry Pi fungiert. Nur das übergeordnete Azure IoT Edge-Gateway verfügt über eine Internetverbindung mit CDN-Ressourcen und sowohl das untergeordnete Azure IoT Edge-Gerät als auch das Azure IoT-Gerät sind vom Internet isoliert. 

  :::image type="content" source="media/connected-cache-overview/nested-level-proxy.png" alt-text="Microsoft Connected Cache: Geschachtelt" lightbox="media/connected-cache-overview/nested-level-proxy.png":::

## <a name="parent-gateway-configuration"></a>Übergeordnete Gatewaykonfiguration

1. Fügen Sie das Microsoft Connected Cache-Modul zu Ihrer Azure IoT Edge Gateway-Gerätebereitstellung in Azure IoT Hub hinzu.
2. Fügen Sie die Umgebungsvariablen für die Bereitstellung hinzu. Nachstehend finden Sie ein Beispiel für die Umgebungsvariablen.

    **Umgebungsvariablen**

    | Name                 | Wert                                       |
    | ----------------------------- | --------------------------------------------| 
    | CACHE_NODE_ID                 | Siehe die Beschreibung der Umgebungsvariablen oben. |
    | CUSTOMER_ID                   | Siehe die Beschreibung der Umgebungsvariablen oben. |
    | CUSTOMER_KEY                  | Siehe die Beschreibung der Umgebungsvariablen oben. |
    | STORAGE_ *N* _SIZE_GB           | N = 5                                       |
    | CACHEABLE_CUSTOM_1_HOST       | Packagerepo.com:80                          |
    | CACHEABLE_CUSTOM_1_CANONICAL  | Packagerepo.com                             |
    | IS_SUMMARY_ACCESS_UNRESTRICTED| true                                        |
    | UPSTREAM_PROXY                | IP-Adresse oder FQDN des Proxyservers                     |

3. Fügen Sie die Optionen zum Erstellen von Containern für die Bereitstellung hinzu. Es gibt keinen Unterschied bei den Optionen zum Erstellen von MCC-Containern aus dem vorherigen Beispiel. Nachstehend finden Sie ein Beispiel für die Optionen zum Erstellen von Containern.

### <a name="container-create-options"></a>Optionen für die Containererstellung

```markdown
{
    "HostConfig": {
        "Binds": [
            "/MicrosoftConnectedCache1/:/nginx/cache1/"
        ],
        "PortBindings": {
            "8081/tcp": [
                {
                    "HostPort": "80"
                }
            ],
            "5000/tcp": [
                {
                    "HostPort": "5100"
                }
            ]
        }
    }
```

## <a name="child-gateway-configuration"></a>Untergeordnete Gatewaykonfiguration

>[!Note]
>Wenn Sie in Ihrer Konfiguration replizierte Container in Ihrer eigenen privaten Registrierung verwendet haben, ist eine Änderung der config.toml-Einstellungen und Runtimeeinstellungen in Ihrer Modulbereitstellung erforderlich. Weitere Informationen finden Sie unter [Tutorial: Erstellen einer Hierarchie für IoT Edge-Geräte – Azure IoT Edge](../iot-edge/tutorial-nested-iot-edge.md?preserve-view=true&tabs=azure-portal&view=iotedge-2020-11#deploy-modules-to-the-lower-layer-device).

1. Ändern Sie den Imagepfad für den Edge-Agent wie im folgenden Beispiel gezeigt:

```markdown
[agent]
name = "edgeAgent"
type = "docker"
env = {}
[agent.config]
image = "<parent_device_fqdn_or_ip>:8000/iotedge/azureiotedge-agent:1.2.0-rc2"
auth = {}
```
2. Ändern Sie die Runtimeeinstellungen für Edge Hub und Edge-Agent in der Bereitstellung von Azure IoT Edge wie in diesem Beispiel veranschaulicht:
    
    * Geben Sie unter „Edge Hub“ im Imagefeld die Zeichenfolge ```$upstream:8000/iotedge/azureiotedge-hub:1.2.0-rc2``` ein.
    * Geben Sie unter „Edge-Agent“ im Imagefeld die Zeichenfolge ```$upstream:8000/iotedge/azureiotedge-agent:1.2.0-rc2``` ein.

3. Fügen Sie das Microsoft Connected Cache-Modul zu Ihrer Azure IoT Edge Gateway-Gerätebereitstellung in Azure IoT Hub hinzu.

   * Wählen Sie einen Namen für das Modul aus: ```ConnectedCache```
   * Ändern Sie den Image-URI: ```$upstream:8000/mcc/linux/iot/mcc-ubuntu-iot-amd64:latest```

4. Fügen Sie dieselben Umgebungsvariablen und Optionen zum Erstellen von Containern hinzu, die in der übergeordneten Bereitstellung verwendet werden.

Um zu überprüfen, ob Microsoft Connected Cache ordnungsgemäß funktioniert, führen Sie den folgenden Befehl im Terminal des IoT Edge-Geräts, auf dem das Modul installiert ist, oder eines beliebigen Geräts im Netzwerk aus.

```bash
    wget "http://<CHILD Azure IoT Edge Gateway IP>/mscomtest/wuidt.gif?cacheHostOrigin=au.download.windowsupdate.com
```