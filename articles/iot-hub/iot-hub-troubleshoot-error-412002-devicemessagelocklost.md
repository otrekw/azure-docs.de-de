---
title: Problembehandlung – Azure IoT Hub-Fehler „412002 DeviceMessageLockLost“
description: Grundlegendes zum Beheben des Fehlers „412002 DeviceMessageLockLost“
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
ms.date: 01/30/2020
ms.author: jlian
ms.openlocfilehash: 53364009f9b9c041c39728e438c3e24eacfd1665
ms.sourcegitcommit: ba676927b1a8acd7c30708144e201f63ce89021d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2021
ms.locfileid: "102435476"
---
# <a name="412002-devicemessagelocklost"></a>412002 DeviceMessageLockLost

In diesem Artikel werden die Ursachen des Fehlers **412002 DeviceMessageLockLost** und die Lösungen dafür beschrieben.

## <a name="symptoms"></a>Symptome

Beim Versuch, eine C2D-Nachricht (Cloud-to-Device, Cloud-zu-Gerät) zu senden, schlägt die Anforderung mit der Meldung **412002DeviceMessageLockLost** fehl.

## <a name="cause"></a>Ursache

Wenn ein Gerät eine C2D-Nachricht aus der Warteschlange empfängt (z. B. über [`ReceiveAsync()`](/dotnet/api/microsoft.azure.devices.client.deviceclient.receiveasync)), wird die Nachricht von IoT Hub für eine Sperrtimeoutdauer von einer Minute gesperrt. Wenn das Gerät versucht, die Nachricht nach Ablauf des Sperrtimeouts abzuschließen, löst IoT Hub diese Ausnahme aus.

## <a name="solution"></a>Lösung

Wenn IoT Hub die Benachrichtigung nicht innerhalb der Sperrtimeoutdauer von einer Minute erhält, setzt es die Nachricht zurück auf den Status *Enqueued* (In die Warteschlange eingereiht). Das Gerät kann versuchen, die Nachricht erneut zu empfangen. Wenn Sie ein erneutes Auftreten dieses Fehlers in Zukunft verhindern möchten, implementieren Sie geräteseitige Logik, damit die Nachricht innerhalb von einer Minute nach dem Empfang abgeschlossen wird. Dieses Timeout von einer Minute Dauer kann nicht geändert werden.