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
ms.openlocfilehash: 15aa21c2ec2c11bb251f7208fd22c92ceb859d6d
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "76960303"
---
# <a name="404001-devicenotfound"></a>404001 DeviceNotFound

In diesem Artikel werden die Ursachen des Fehlers **404001 DeviceNotFound** und die Lösungen dafür beschrieben.

## <a name="symptoms"></a>Symptome

Während einer C2D-Kommunikation (Cloud-to-Device, Cloud-zu-Gerät), z. B. C2D-Nachricht, Zwillingsupdate oder direkte Methode, schlägt der Vorgang mit der Meldung **404001 DeviceNotFound** fehl.

## <a name="cause"></a>Ursache

Der Vorgang ist fehlgeschlagen, weil das Gerät von IoT Hub nicht gefunden werden kann. Das Gerät ist entweder nicht registriert oder deaktiviert.

## <a name="solution"></a>Lösung

Registrieren Sie die verwendete Geräte-ID, und wiederholen Sie dann den Vorgang.