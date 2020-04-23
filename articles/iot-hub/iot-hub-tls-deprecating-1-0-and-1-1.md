---
title: Einstellung der Unterstützung von TLS 1.0 und 1.1 in IoT Hub | Microsoft-Dokumentation
description: Enthält Richtlinien zur Einstellung der Unterstützung von TLS 1.0 und 1.1 und der zugehörigen Verschlüsselungen in IoT Hub.
author: jlian
ms.author: jlian
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/14/2020
ms.openlocfilehash: a887dd4df44ba58b0e6646ffb1c10eb21edf3e69
ms.sourcegitcommit: ea006cd8e62888271b2601d5ed4ec78fb40e8427
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/14/2020
ms.locfileid: "81381297"
---
# <a name="deprecation-of-tls-10-and-11-in-iot-hub"></a>Einstellung der Unterstützung von TLS 1.0 und 1.1 in IoT Hub

Um die bestmögliche Verschlüsselung zu erzielen, erfolgt für IoT Hub die Umstellung auf Transport Layer Security 1.2 (TLS) als Verschlüsselungsmechanismus für IoT-Geräte und -Dienste. 

## <a name="timeline"></a>Zeitachse

TLS 1.0/1.1 wird für IoT Hub bis auf Weiteres noch unterstützt. Wir empfehlen aber allen Kunden, so bald wie möglich die Migration zu TLS 1.2 durchzuführen.

## <a name="supported-ciphers"></a>Unterstützte Verschlüsselungen

Die Zeitachse für die Verfügbarkeit verschiedener Verschlüsselungen, die im TLS-Handshake verwendet werden, lautet wie folgt:

* TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256 (derzeit unterstützt)
* TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384 (wird in der zweiten Hälfte von 2020 unterstützt)
* TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 (wird in der zweiten Hälfte von 2020 unterstützt)
* TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384 (wird in der zweiten Hälfte von 2020 unterstützt)

## <a name="customer-feedback"></a>Kundenfeedback

TLS 1.2 stellt eine branchenweit erstklassige Verschlüsselungsoption dar und wird wie geplant aktiviert. Dennoch würden wir gerne von unseren Kunden über ihre spezifischen Bereitstellungen und Schwierigkeiten bei der Einführung von TLS 1.2 informiert werden. Sie können Ihre Kommentare gerne an [iot_tls1_deprecation@microsoft.com](mailto:iot_tls1_deprecation@microsoft.com) senden.
