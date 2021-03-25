---
title: Netzwerkanforderungen von Device Update for IoT Hub | Microsoft-Dokumentation
description: Device Update for IoT Hub verwendet verschiedene Netzwerkports für verschiedene Zwecke.
author: philmea
ms.author: philmea
ms.date: 1/11/2021
ms.topic: conceptual
ms.service: iot-hub-device-update
ms.openlocfilehash: e72ff144a56f44ccaa695b7dab328e42052fce39
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "101678501"
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

[Erfahren Sie mehr](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-protocols#:~:text=Table%202%20%20%20,%201%20more%20rows) über die aktuelle Liste der unterstützten Protokolle.
