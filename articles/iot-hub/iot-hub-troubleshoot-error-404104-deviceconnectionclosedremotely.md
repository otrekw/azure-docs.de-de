---
title: Problembehandlung – Azure IoT Hub-Fehler „404104 DeviceConnectionClosedRemotely“
description: Grundlegendes zum Beheben des Fehlers „404104 DeviceConnectionClosedRemotely“
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
ms.date: 01/30/2020
ms.author: jlian
ms.openlocfilehash: 686946b65b4b7540f404a291c87c5ad9c7b7a0ca
ms.sourcegitcommit: fa6fe765e08aa2e015f2f8dbc2445664d63cc591
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2020
ms.locfileid: "76960295"
---
# <a name="404104-deviceconnectionclosedremotely"></a>404104 DeviceConnectionClosedRemotely

In diesem Artikel werden die Ursachen des Fehlers **404104 DeviceConnectionClosedRemotely** und die Lösungen dafür beschrieben.

## <a name="symptoms"></a>Symptome

### <a name="symptom-1"></a>Symptom 1

Die Geräteverbindung wird in regelmäßigen Abständen (beispielsweise alle 65 Minuten) getrennt und in den IoT Hub-Diagnoseprotokollen die Meldung **404104 DeviceConnectionClosedRemotely** angezeigt. Manchmal wird auch **401003 IoTHubUnauthorized** und weniger als eine Minute später ein Ereignis des Typs „Geräteverbindung erfolgreich“ angezeigt.

### <a name="symptom-2"></a>Symptom 2

Die Verbindung von Geräten wird nach dem Zufallsprinzip getrennt und in den IoT Hub-Diagnoseprotokollen die Meldung **404104 DeviceConnectionClosedRemotely** angezeigt.

### <a name="symptom-3"></a>Symptom 3

Die Verbindung vieler Geräte wird sofort getrennt, in der [Metrik „Verbundene Geräte“](iot-hub-metrics.md) wird eine Senke angezeigt, und in den Diagnoseprotokollen gibt es mehr Meldungen **404104 DeviceConnectionClosedRemotely** und [500xxx-interne Fehler](iot-hub-troubleshoot-error-500xxx-internal-errors.md) als üblich.

## <a name="causes"></a>Ursachen

### <a name="cause-1"></a>Ursache 1

Das [zum Herstellen einer Verbindung mit IoT Hub verwendete SAS-Token](iot-hub-devguide-security.md#security-tokens) ist abgelaufen. Dies führt dazu, dass IoT Hub die Verbindung mit dem Gerät trennt. Die Verbindung wird wiederhergestellt, wenn das Token vom Gerät aktualisiert wurde. Beispielsweise [läuft das SAS-Token für C SDK standardmäßig stündlich ab](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/connection_and_messaging_reliability.md#connection-authentication). Das kann zu regulären Trennungen der Verbindung führen.

Weitere Informationen finden Sie unter [401003 IoTHubUnauthorized cause](iot-hub-troubleshoot-error-401003-iothubunauthorized.md#cause-1) (Ursache für „401003 IoTHubUnauthorized“).

### <a name="cause-2"></a>Ursache 2

Einige Möglichkeiten sind die folgenden:

- Das Gerät hat die zugrunde liegende Netzwerkkonnektivität länger als das [MQTT-Keep-Alive](iot-hub-mqtt-support.md#default-keep-alive-timeout) verloren, was zu einem Remote-Leerlauftimeout führte. Die Einstellung für das MQTT-Keep-Alive kann bei jedem Gerät unterschiedlich sein.

- Das Gerät hat eine Zurücksetzung auf TCP/IP-Ebene gesendet, aber keine Anwendungsebene `MQTT DISCONNECT`. Im Wesentlichen hat das Gerät die zugrunde liegende Socketverbindung plötzlich geschlossen. Manchmal wird dieses Problem durch Fehler in älteren Versionen des Azure IoT SDKs verursacht.

- Die geräteseitige Anwendung ist abgestürzt.

### <a name="cause-3"></a>Ursache 3

Bei IoT Hub ist möglicherweise ein vorübergehendes Problem aufgetreten. Weitere Informationen finden Sie unter [IoT Hub internal server error cause](iot-hub-troubleshoot-error-500xxx-internal-errors.md#cause) (IoT Hub – Ursache für internen Serverfehler).

## <a name="solutions"></a>Lösungen

### <a name="solution-1"></a>Lösung 1

Weitere Informationen finden Sie unter [401003 IoTHubUnauthorized – Lösung 1](iot-hub-troubleshoot-error-401003-iothubunauthorized.md#solution-1).

### <a name="solution-2"></a>Lösung 2

- Stellen Sie sicher, dass das Gerät über eine gute Konnektivität zum IoT Hub verfügt, indem Sie [die Verbindung testen](tutorial-connectivity.md). Wenn das Netzwerk unzuverlässig oder zeitweilig ist, raten wir davon ab, den Keep-Alive-Wert zu erhöhen. Dies könnte nämlich dazu führen, dass die Erkennung (z. B. über Azure Monitor-Warnungen) länger dauert. 

- Verwenden Sie die neuesten Versionen der [IoT SDKs](iot-hub-devguide-sdks.md).

### <a name="solution-3"></a>Lösung 3

Weitere Informationen finden Sie unter [solutions to IoT Hub internal server errors](iot-hub-troubleshoot-error-500xxx-internal-errors.md#solution) (Lösungen für interne IoT Hub-Serverfehler).

## <a name="next-steps"></a>Nächste Schritte

Wir empfehlen die Verwendung von Azure IoT-Geräte-SDKs, um Verbindungen zuverlässig zu verwalten. Weitere Informationen finden Sie unter [Verwalten von Konnektivität und zuverlässigem Messaging mithilfe von Azure IoT Hub-Geräte-SDKs](iot-hub-reliability-features-in-sdks.md).