---
title: Verwalten des Zugriffs über öffentliche Netzwerke für Azure IoT Hub
description: Dokumentation zum Deaktivieren und Aktivieren des Zugriffs über öffentliche Netzwerke für IoT Hub
author: jlian
ms.author: jlian
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 03/22/2021
ms.openlocfilehash: fbbdaeb796dfa23906c8010a54af14eff6df0b97
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105026639"
---
# <a name="managing-public-network-access-for-your-iot-hub"></a>Verwalten des Zugriffs über öffentliche Netzwerke für Ihren IoT-Hub

Deaktivieren Sie den Zugriff über öffentliche Netzwerke, um den Zugriff auf den [privaten Endpunkt für Ihren IoT-Hub in Ihrem VNet](virtual-network-support.md) einzuschränken. Verwenden Sie hierzu das Azure-Portal oder die `publicNetworkAccess`-API. 

## <a name="turn-off-public-network-access-using-azure-portal"></a>Deaktivieren des Zugriffs über öffentliche Netzwerke im Azure-Portal

1. Besuchen Sie das [Azure-Portal](https://portal.azure.com).
2. Navigieren Sie zu Ihrem IoT Hub.
3. Wählen Sie im linken Menü die Option **Netzwerk** aus.
4. Wählen Sie unter „Zugriff über öffentliche Netzwerke zulassen auf“ die Option **Deaktiviert** aus.
5. Wählen Sie **Speichern** aus.

:::image type="content" source="media/iot-hub-publicnetworkaccess/turn-off-public-network-access.png" alt-text="Screenshot des Navigationspfads im Azure-Portal zum Deaktivieren des Zugriffs über öffentliche Netzwerke" lightbox="media/iot-hub-publicnetworkaccess/turn-off-public-network-access.png":::

Wenn Sie den Zugriff über öffentliche Netzwerke aktivieren möchten, wählen Sie **Alle Netzwerke** und dann **Speichern** aus.

## <a name="accessing-the-iot-hub-after-disabling-public-network-access"></a>Zugreifen auf den IoT Hub nach dem Deaktivieren des Zugriffs auf das öffentliche Netzwerk

Nachdem der Zugriff auf das öffentliche Netzwerk deaktiviert wurde, ist der IoT Hub nur über den [privaten VNet-Endpunkt über Azure Private Link](virtual-network-support.md) zu erreichen. Diese Einschränkung schließt den Zugriff über das Azure-Portal mit ein, da API-Aufrufe an den IoT Hub-Dienst direkt über Ihren Browser mit Ihren Anmeldeinformationen erfolgen.

## <a name="iot-hub-endpoint-ip-address-and-ports-after-disabling-public-network-access"></a>IoT Hub-Endpunkt, IP-Adresse und Ports nach Deaktivierung des Zugriffs auf öffentliche Netzwerke

Weil IoT Hub ist eine mehrinstanzenfähige Platform-as-a-Service-Instanz (PaaS) ist, können verschiedene Kunden denselben Pool an Compute-, Netzwerk- und Speicherhardwareressourcen gemeinsam nutzen. IoT Hub-Hostnamen werden einem öffentlichen Endpunkt mit einer öffentlich routingfähigen IP-Adresse über das Internet zugeordnet. Dieser öffentliche IoT Hub-Endpunkt wird von mehreren Kunden gemeinsam genutzt, und sämtliche IoT-Geräte in WANs und lokalen Netzwerken können darauf zugreifen. 

Die Deaktivierung des Zugriffs auf öffentliche Netzwerke wird für eine bestimmte IoT Hub-Ressource erzwungen und so die Isolation sichergestellt. Um den Dienst für andere Kundenressourcen über den öffentlichen Pfad aktiv zu halten, bleibt der öffentliche Endpunkt auflösbar, IP-Adressen bleiben erkennbar, und die Ports bleiben offen. Dies ist kein Problem, da Microsoft mehrere Sicherheitsebenen integriert, um eine vollständige Isolation zwischen Mandanten sicherzustellen. Weitere Informationen hierzu finden Sie unter [Isolation in der öffentlichen Azure-Cloud](../security/fundamentals/isolation-choices.md#tenant-level-isolation).

## <a name="ip-filter"></a>IP-Filter 

Wenn der Zugriff über öffentliche Netzwerke deaktiviert ist, werden alle [IP-Filterregeln](iot-hub-ip-filtering.md) ignoriert. Der Grund dafür ist, dass alle IP-Adressen aus dem öffentlichen Internet blockiert werden. Wenn Sie IP-Filter nutzen möchten, verwenden Sie die Option **Ausgewählte IP-Adressbereiche**.

## <a name="bug-fix-with-built-in-event-hub-compatible-endpoint"></a>Fehlerkorrektur für den integrierten Event Hub-kompatiblen Endpunkt

Es gibt einen IoT Hub-Fehler, bei dem der Zugriff auf den [integrierten Event Hub-kompatiblen Endpunkt](iot-hub-devguide-messages-read-builtin.md) über das öffentliche Internet weiterhin möglich ist, wenn der Zugriff über öffentliche Netzwerke auf den IoT-Hub deaktiviert ist. Wenn Sie mehr über diesen Fehler erfahren und uns diesbezüglich kontaktieren möchten, lesen Sie [Deaktivierter Zugriff auf integrierte Azure Event Hub-Endpunkte durch Deaktivierung des Zugriffs über öffentliche Netzwerke für Azure IoT Hub](https://azure.microsoft.com/updates/iot-hub-public-network-access-bug-fix).
