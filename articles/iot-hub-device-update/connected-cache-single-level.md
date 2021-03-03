---
title: Microsoft Connected Cache (Vorschau) – Beispiele für ein Bereitstellungsszenario | Microsoft-Dokumentation
titleSuffix: Device Update for Azure IoT Hub
description: Microsoft Connected Cache (Vorschau) – Beispiele für ein Bereitstellungsszenario – Tutorials
author: andyriv
ms.author: andyriv
ms.date: 2/16/2021
ms.topic: tutorial
ms.service: iot-hub-device-update
ms.openlocfilehash: ae07926d7d8c768170e945e916367bee41999571
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2021
ms.locfileid: "101658740"
---
# <a name="microsoft-connected-cache-preview-deployment-scenario-samples"></a>Microsoft Connected Cache (Vorschau) – Beispiele für ein Bereitstellungsszenario

## <a name="single-level-azure-iot-edge-gateway-no-proxy"></a>Azure IoT Edge-Gateway mit einer Ebene – kein Proxy

Im folgenden Diagramm wird das Szenario beschrieben, in dem ein Azure IoT Edge-Gateway direkten Zugriff auf CDN-Ressourcen hat. Außerdem gibt es ein Azure IoT-Blattgerät (z. B. ein Raspberry Pi). Dies ist ein vom Internet getrenntes untergeordnetes Gerät des Azure IoT Edge-Gateways. 

  :::image type="content" source="media/connected-cache-overview/disconnected-device-update.png" alt-text="Microsoft Connected Cache – Getrennte Geräteaktualisierung" lightbox="media/connected-cache-overview/disconnected-device-update.png":::

1. Fügen Sie das Microsoft Connected Cache-Modul zu Ihrer Azure IoT Edge Gateway-Gerätebereitstellung in Azure IoT Hub hinzu (Einzelheiten zum Aufrufen des Moduls finden Sie unter `MCC concepts`).
2. Fügen Sie die Umgebungsvariablen für die Bereitstellung hinzu. Nachstehend finden Sie ein Beispiel für die Umgebungsvariablen.

    **Umgebungsvariablen**
    
    | Name                 | Wert                                       |
    | ----------------------------- | --------------------------------------------| 
    | CACHE_NODE_ID                 | Siehe die Beschreibung der Umgebungsvariablen oben. |
    | CUSTOMER_ID                   | Siehe die Beschreibung der Umgebungsvariablen oben. |
    | CUSTOMER_KEY                  | Siehe die Beschreibung der Umgebungsvariablen oben. |
    | STORAGE_ *N* _SIZE_GB           | N = 5                                       |

3. Fügen Sie die Optionen zum Erstellen von Containern für die Bereitstellung hinzu. Nachstehend finden Sie ein Beispiel für die Optionen zum Erstellen von Containern.

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

Um eine Prüfung des ordnungsgemäß funktionierenden Microsoft Connected Cache durchzuführen, führen Sie den folgenden Befehl im Terminal des IoT Edge-Geräts, auf dem das Modul gehostet wird, oder auf einem beliebigen Gerät im Netzwerk aus.

```bash
    wget "http://<IOT Edge Gateway IP>/mscomtest/wuidt.gif?cacheHostOrigin=au.download.windowsupdate.com
```

## <a name="single-level-azure-iot-edge-gateway-with-outbound-unauthenticated-proxy"></a>Azure IoT Edge-Gateway mit einer Ebene und ausgehendem, nicht authentifiziertem Proxy

In diesem Szenario gibt es ein Azure IoT Edge-Gateway, das über einen ausgehenden, nicht authentifizierten Proxy auf CDN-Ressourcen zugreifen kann. Microsoft Connected Cache wird so konfiguriert, dass Inhalt aus einem benutzerdefinierten Repository zwischengespeichert wird und der Zusammenfassungsbericht für alle Benutzer im Netzwerk sichtbar gemacht wurde. Nachstehend finden Sie ein Beispiel für die MCC-Umgebungsvariablen, die festgelegt würden.

  :::image type="content" source="media/connected-cache-overview/single-level-proxy.png" alt-text="Microsoft Connected Cache – Proxy mit einer Ebene" lightbox="media/connected-cache-overview/single-level-proxy.png":::

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

```json
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

Um eine Prüfung des ordnungsgemäß funktionierenden Microsoft Connected Cache durchzuführen, führen Sie den folgenden Befehl im Terminal des Azure IoT Edge-Geräts, auf dem das Modul gehostet wird, oder auf einem beliebigen Gerät im Netzwerk aus.

```bash
    wget "http://<Azure IOT Edge Gateway IP>/mscomtest/wuidt.gif?cacheHostOrigin=au.download.windowsupdate.com
```
