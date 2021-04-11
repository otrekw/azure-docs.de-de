---
title: Problembehandlung – Azure IoT Hub-Fehler „409002 LinkCreationConflict“
description: Grundlegendes zum Beheben des Fehlers „409002 LinkCreationConflict“
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
ms.date: 01/30/2020
ms.author: jlian
ms.custom: amqp
ms.openlocfilehash: c354eed6e7bf9f2147f8b81a82a06e80a9337c84
ms.sourcegitcommit: 73fb48074c4c91c3511d5bcdffd6e40854fb46e5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/31/2021
ms.locfileid: "106061042"
---
# <a name="409002-linkcreationconflict"></a>409002 LinkCreationConflict

In diesem Artikel werden die Ursachen des Fehlers **409002 LinkCreationConflict** und die Lösungen dafür beschrieben.

## <a name="symptoms"></a>Symptome

Der Fehler **409002 LinkCreationConflict** wird in Protokollen zusammen mit einer Trennung von Geräteverbindungen oder einem Fehler bei C2D-Nachrichten (Cloud-to-Device, Cloud-zu-Gerät) protokolliert.

<!-- When using AMQP? -->

## <a name="cause"></a>Ursache

Dieser Fehler tritt im Allgemeinen auf, wenn IoT Hub erkennt, dass es bei einem Client mehr als eine Verbindung gibt. Wenn eine neue Verbindungsanforderung für ein Gerät mit einer vorhandenen Verbindung eintrifft, schließt IoT Hub die vorhandene Verbindung mit dieser Fehlermeldung.

### <a name="cause-1"></a>Ursache 1

Im häufigsten Fall führt ein separates Problem (z. B. [404104 DeviceConnectionClosedRemotely](iot-hub-troubleshoot-error-404104-deviceconnectionclosedremotely.md)) dazu, dass die Verbindung des Geräts getrennt wird. Das Gerät versucht, die Verbindung sofort wiederherzustellen, doch IoT Hub betrachtet das Gerät weiterhin als verbunden. IoT Hub schließt die vorherige Verbindung und protokolliert diesen Fehler.

### <a name="cause-2"></a>Ursache 2

Eine fehlerhafte geräteseitige Logik bewirkt, dass das Gerät die Verbindung herstellt, wenn eine Verbindung bereits geöffnet ist.

## <a name="solution"></a>Lösung

Dieser Fehler tritt normalerweise als Nebenwirkung eines anderen vorübergehenden Problems auf. Suchen Sie also nach anderen Fehlern in den Protokollen, um eine Problembehandlung durchführen. Erstellen Sie andernfalls nur dann eine neue Verbindungsanforderung, wenn die Verbindung unterbrochen wird.
