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
ms.openlocfilehash: ed4341c1c8df588bf735bdc9c531c2165514d610
ms.sourcegitcommit: 73fb48074c4c91c3511d5bcdffd6e40854fb46e5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/31/2021
ms.locfileid: "106061263"
---
# <a name="404103-devicenotonline"></a>404103 DeviceNotOnline

In diesem Artikel werden die Ursachen des Fehlers **404103 DeviceNotOnline** und die Lösungen dafür beschrieben.

## <a name="symptoms"></a>Symptome

Eine direkte Methode auf einem Gerät schlägt mit der Meldung **404103 DeviceNotOnline** selbst dann fehl, wenn das Gerät online ist. 

## <a name="cause"></a>Ursache

Wenn Sie wissen, dass das Gerät online ist und die Fehlermeldung weiterhin angezeigt wird, liegt es wahrscheinlich daran, dass der Rückruf der direkten Methode auf dem Gerät nicht registriert ist.

## <a name="solution"></a>Lösung

Informationen zum ordnungsgemäßen Konfigurieren Ihres Geräts für Rückrufe der direkten Methode finden Sie unter [Behandeln einer direkten Methode auf einem Gerät](iot-hub-devguide-direct-methods.md#handle-a-direct-method-on-a-device).