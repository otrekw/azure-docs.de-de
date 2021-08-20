---
title: Problembehandlung – Azure IoT Hub-Fehler „404001 DeviceNotFound“
description: Grundlegendes zum Beheben des Fehlers „404001 DeviceNotFound“
author: jlian
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
ms.date: 07/07/2021
ms.author: jlian
ms.openlocfilehash: 127f4546d674eb425440a8e0f395ca8ff0a6b6f4
ms.sourcegitcommit: 555ea0d06da38dea1de6ecbe0ed746cddd4566f5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2021
ms.locfileid: "113515523"
---
# <a name="404001-devicenotfound"></a>404001 DeviceNotFound

In diesem Artikel werden die Ursachen des Fehlers **404001 DeviceNotFound** und die Lösungen dafür beschrieben.

## <a name="symptoms"></a>Symptome

Während einer C2D-Kommunikation (Cloud-to-Device, Cloud-zu-Gerät), z. B. C2D-Nachricht, Zwillingsupdate oder direkte Methode, schlägt der Vorgang mit der Meldung **404001 DeviceNotFound** fehl.

## <a name="cause"></a>Ursache

Der Vorgang ist fehlgeschlagen, weil das Gerät von IoT Hub nicht gefunden werden kann. Das Gerät ist entweder nicht registriert oder deaktiviert.

## <a name="solution"></a>Lösung

Registrieren Sie die verwendete Geräte-ID, und wiederholen Sie dann den Vorgang.