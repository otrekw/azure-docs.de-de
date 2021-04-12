---
title: Netzwerkanforderungen von Device Update for IoT Hub | Microsoft-Dokumentation
description: Device Update for IoT Hub verwendet verschiedene Netzwerkports für verschiedene Zwecke.
author: philmea
ms.author: philmea
ms.date: 1/11/2021
ms.topic: conceptual
ms.service: iot-hub-device-update
ms.openlocfilehash: 0512308fbaa0a725c6ecca573c70c90d8c04e247
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105558379"
---
# <a name="ports-used-with-device-update-for-iot-hub"></a>Mit Device Update for IoT Hub verwendete Ports
ADU verwendet verschiedene Netzwerkports für verschiedene Zwecke.

## <a name="default-ports"></a>Standardports

Zweck|Portnummer |
---|---
Herunterladen aus Netzwerken/CDNs  | 80 (HTTP-Protokoll)
Herunterladen aus MCC/CDNs | 80 (HTTP-Protokoll)
ADU-Agent-Verbindung mit Azure IoT Hub  | 8883 (MQTT-Protokoll)

## <a name="use-azure-iot-hub-supported-protocols"></a>Verwenden von Protokollen, die von IoT Hub unterstützt werden
Der ADU-Agent kann so geändert werden, dass er die unterstützten Azure IoT Hub-Protokolle verwendet.

[Erfahren Sie mehr](../iot-hub/iot-hub-devguide-protocols.md) über die aktuelle Liste der unterstützten Protokolle.
