---
title: Problembehandlung – Azure IoT Hub-Fehler „404001 DeviceNotFound“
description: Grundlegendes zum Beheben des Fehlers „404001 DeviceNotFound“
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
ms.date: 01/30/2020
ms.author: jlian
ms.openlocfilehash: 09f3c00dadc6e95aae01fb8082fb746e155d4688
ms.sourcegitcommit: 73fb48074c4c91c3511d5bcdffd6e40854fb46e5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/31/2021
ms.locfileid: "106061280"
---
# <a name="404001-devicenotfound"></a>404001 DeviceNotFound

In diesem Artikel werden die Ursachen des Fehlers **404001 DeviceNotFound** und die Lösungen dafür beschrieben.

## <a name="symptoms"></a>Symptome

Während einer C2D-Kommunikation (Cloud-to-Device, Cloud-zu-Gerät), z. B. C2D-Nachricht, Zwillingsupdate oder direkte Methode, schlägt der Vorgang mit der Meldung **404001 DeviceNotFound** fehl.

## <a name="cause"></a>Ursache

Der Vorgang ist fehlgeschlagen, weil das Gerät von IoT Hub nicht gefunden werden kann. Das Gerät ist entweder nicht registriert oder deaktiviert.

## <a name="solution"></a>Lösung

Registrieren Sie die verwendete Geräte-ID, und wiederholen Sie dann den Vorgang.