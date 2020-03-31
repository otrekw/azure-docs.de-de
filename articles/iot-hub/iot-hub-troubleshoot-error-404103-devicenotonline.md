---
title: Problembehandlung – Azure IoT Hub-Fehler „404103 DeviceNotOnline“
description: Grundlegendes zum Beheben des Fehlers „404103 DeviceNotOnline“
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
ms.date: 01/30/2020
ms.author: jlian
ms.openlocfilehash: e648428f924cfc33421c8591c41f7ac85b05a033
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "76960299"
---
# <a name="404103-devicenotonline"></a>404103 DeviceNotOnline

In diesem Artikel werden die Ursachen des Fehlers **404103 DeviceNotOnline** und die Lösungen dafür beschrieben.

## <a name="symptoms"></a>Symptome

Eine direkte Methode auf einem Gerät schlägt mit der Meldung **404103 DeviceNotOnline** selbst dann fehl, wenn das Gerät online ist. 

## <a name="cause"></a>Ursache

Wenn Sie wissen, dass das Gerät online ist und die Fehlermeldung weiterhin angezeigt wird, liegt es wahrscheinlich daran, dass der Rückruf der direkten Methode auf dem Gerät nicht registriert ist.

## <a name="solution"></a>Lösung

Informationen zum ordnungsgemäßen Konfigurieren Ihres Geräts für Rückrufe der direkten Methode finden Sie unter [Behandeln einer direkten Methode auf einem Gerät](iot-hub-devguide-direct-methods.md#handle-a-direct-method-on-a-device).