---
title: Verwalten des Zugriffs über öffentliche Netzwerke für Azure IoT Hub
description: Dokumentation zum Deaktivieren und Aktivieren des Zugriffs über öffentliche Netzwerke für IoT Hub
author: jlian
ms.author: jlian
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/01/2020
ms.openlocfilehash: c82f98df8fb79fa10f2e30b219c1a02bb646e2de
ms.sourcegitcommit: dee7b84104741ddf74b660c3c0a291adf11ed349
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85937453"
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

Wenn Sie den Zugriff über öffentliche Netzwerke aktivieren möchten, wählen Sie **Aktiviert** und dann **Speichern** aus.

## <a name="ip-filter"></a>IP-Filter 

Wenn der Zugriff über öffentliche Netzwerke deaktiviert ist, werden alle [IP-Filterregeln](iot-hub-ip-filtering.md) ignoriert. Der Grund dafür ist, dass alle IP-Adressen aus dem öffentlichen Internet blockiert werden. Wenn Sie IP-Filter nutzen möchten, verwenden Sie die Option **Ausgewählte IP-Adressbereiche**.

## <a name="bug-fix-with-built-in-event-hub-compatible-endpoint"></a>Fehlerkorrektur für den integrierten Event Hub-kompatiblen Endpunkt

Es gibt einen IoT Hub-Fehler, bei dem der Zugriff auf den [integrierten Event Hub-kompatiblen Endpunkt](iot-hub-devguide-messages-read-builtin.md) über das öffentliche Internet weiterhin möglich ist, wenn der Zugriff über öffentliche Netzwerke auf den IoT-Hub deaktiviert ist. Wenn Sie mehr über diesen Fehler erfahren und uns diesbezüglich kontaktieren möchten, lesen Sie [Deaktivierter Zugriff auf integrierte Azure Event Hub-Endpunkte durch Deaktivierung des Zugriffs über öffentliche Netzwerke für Azure IoT Hub](https://azure.microsoft.com/updates/iot-hub-public-network-access-bug-fix).