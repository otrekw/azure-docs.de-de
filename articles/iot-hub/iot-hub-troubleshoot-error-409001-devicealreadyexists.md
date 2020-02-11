---
title: Problembehandlung – Azure IoT Hub-Fehler „409001 DeviceAlreadyExists“
description: Grundlegendes zum Beheben des Fehlers „409001 DeviceAlreadyExists“
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
ms.date: 01/30/2020
ms.author: jlian
ms.openlocfilehash: 93ab2ecc8e820c461a7c79082ac1d50c24f0ba8b
ms.sourcegitcommit: fa6fe765e08aa2e015f2f8dbc2445664d63cc591
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2020
ms.locfileid: "76960291"
---
# <a name="409001-devicealreadyexists"></a>409001 DeviceAlreadyExists

In diesem Artikel werden die Ursachen des Fehlers **409001 DeviceAlreadyExists** und die Lösungen dafür beschrieben.

## <a name="symptoms"></a>Symptome

Wenn Sie versuchen, ein Gerät in IoT Hub zu registrieren, schlägt die Anforderung mit der Meldung **409001 DeviceAlreadyExists** fehl.

## <a name="cause"></a>Ursache

Es gibt im IoT-Hub bereits ein Gerät mit derselben Geräte-ID. 

## <a name="solution"></a>Lösung

Verwenden Sie eine andere Geräte-ID, und wiederholen Sie den Vorgang.