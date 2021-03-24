---
title: Einstellung der Unterstützung von TLS 1.0 und 1.1 in IoT Hub | Microsoft-Dokumentation
description: Enthält Richtlinien zur Einstellung der Unterstützung von TLS 1.0 und 1.1 und der zugehörigen Verschlüsselungen in IoT Hub.
author: jlian
ms.author: jlian
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/14/2020
ms.openlocfilehash: fcf7620f53c9bfdb51eb62598f2c8b441574eca6
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "90006079"
---
# <a name="deprecation-of-tls-10-and-11-in-iot-hub"></a>Einstellung der Unterstützung von TLS 1.0 und 1.1 in IoT Hub

Um die bestmögliche Verschlüsselung zu erzielen, erfolgt für IoT Hub die Umstellung auf Transport Layer Security 1.2 (TLS) als Verschlüsselungsmechanismus für IoT-Geräte und -Dienste. 

## <a name="timeline"></a>Zeitachse

TLS 1.0/1.1 wird für IoT Hub bis auf Weiteres noch unterstützt. Wir empfehlen aber allen Kunden, so bald wie möglich die Migration zu TLS 1.2 durchzuführen.

## <a name="deprecating-tls-11-ciphers"></a>Einstellung der Unterstützung von TLS 1.1-Verschlüsselungen

* `TLS_ECDHE_RSA_WITH_AES_256_CBC_SH`
* `TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA`
* `TLS_RSA_WITH_AES_256_CBC_SHA`
* `TLS_RSA_WITH_AES_128_CBC_SHA`
* `TLS_RSA_WITH_3DES_EDE_CBC_SHA`

## <a name="deprecating-tls-10-ciphers"></a>Einstellung der Unterstützung von TLS 1.0-Verschlüsselungen

* `TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA`
* `TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA`
* `TLS_RSA_WITH_AES_256_CBC_SHA`
* `TLS_RSA_WITH_AES_128_CBC_SHA`
* `TLS_RSA_WITH_3DES_EDE_CBC_SHA`

## <a name="tls-12-cipher-suites"></a>TLS 1.2-Verschlüsselungssammlungen

Weitere Informationen finden Sie unter [IoT Hub TLS 1.2-Verschlüsselungssammlungen](iot-hub-tls-support.md#cipher-suites).
 
## <a name="customer-feedback"></a>Kundenfeedback

TLS 1.2 stellt eine branchenweit erstklassige Verschlüsselungsoption dar und wird wie geplant aktiviert. Dennoch würden wir gerne von unseren Kunden über ihre spezifischen Bereitstellungen und Schwierigkeiten bei der Einführung von TLS 1.2 informiert werden. Sie können Ihre Kommentare gerne an [iot_tls1_deprecation@microsoft.com](mailto:iot_tls1_deprecation@microsoft.com) senden.
