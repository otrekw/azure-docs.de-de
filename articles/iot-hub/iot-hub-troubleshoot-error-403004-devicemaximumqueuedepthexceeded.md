---
title: Problembehandlung – Azure IoT Hub-Fehler „403004 DeviceMaximumQueueDepthExceeded“
description: Grundlegendes zum Beheben des Fehlers „403004 DeviceMaximumQueueDepthExceeded“
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
ms.date: 01/30/2020
ms.author: jlian
ms.custom:
- amqp
- mqtt
ms.openlocfilehash: 421066ef30e23a79b26f97939cdfffb5be83afb5
ms.sourcegitcommit: dbe434f45f9d0f9d298076bf8c08672ceca416c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2020
ms.locfileid: "92148235"
---
# <a name="403004-devicemaximumqueuedepthexceeded"></a>403004 DeviceMaximumQueueDepthExceeded

In diesem Artikel werden die Ursachen des Fehlers **403004 DeviceMaximumQueueDepthExceeded** und die Lösungen dafür beschrieben.

## <a name="symptoms"></a>Symptome

Beim Versuch, eine C2D-Nachricht (Cloud-to-Device, Cloud-zu-Gerät) zu senden, schlägt die Anforderung mit der Meldung **403004** oder **DeviceMaximumQueueDepthExceeded** fehl.

## <a name="cause"></a>Ursache

Die zugrunde liegende Ursache ist, dass die Anzahl von Nachrichten, die für das Gerät in die Warteschlange eingereiht wurden, das [Warteschlangenlimit (50)](./iot-hub-devguide-quotas-throttling.md#other-limits) überschreitet.

Der wahrscheinlichste Grund für die Überschreitung dieses Grenzwerts: Sie verwenden HTTPS zum Empfangen der Nachricht. Dies führt zu kontinuierlichem Abrufen mithilfe von `ReceiveAsync` und letztlich dazu, dass die IoT Hub-Anforderung gedrosselt wird.

## <a name="solution"></a>Lösung

Das unterstützte Muster für C2D-Nachrichten mit HTTPS entspricht Geräten, die nicht permanent verbunden sind und weniger häufig Nachrichten abrufen (d.h. seltener als alle 25 Minuten). Um die Wahrscheinlichkeit zu verringern, dass das Warteschlangenlimit erreicht wird, wechseln Sie zu AMQP oder MQTT für C2D-Nachrichten.

Als Alternative erweitern Sie die Logik auf Geräteseite, damit Nachrichten in der Warteschlange schnell abgeschlossen, abgelehnt oder abgebrochen werden, verkürzen Sie die Gültigkeitsdauer, oder erwägen Sie, weniger Nachrichten zu senden. Weitere Informationen finden Sie unter [Nachrichtenablauf (Gültigkeitsdauer)](./iot-hub-devguide-messages-c2d.md#message-expiration-time-to-live).

Und schließlich: Überlegen Sie eine Verwendung der [Purge Queue API](/azure/iot-hub/iot-c-sdk-ref/iothub-registrymanager-h/iothubregistrymanager-deletedevice) (API zum endgültigen Löschen des Warteschlangeninhalts), damit ausstehende Nachrichten regelmäßig bereinigt werden, bevor das Limit erreicht wird.