---
title: Einstellung der Unterstützung für TLS 1.0 und 1.1 in IoT Hub und Device Provisioning Service (DPS) | Microsoft-Dokumentation
description: Richtlinien für die Einstellung der Unterstützung von TLS 1.0 und 1.1 und Chiffren in IOT Hub und DPS.
author: rezasherafat
ms.author: rezas
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 12/16/2019
ms.openlocfilehash: d61ca8fe7c6f5e7cc400714d7c31a0a7e50b8a88
ms.sourcegitcommit: 05b36f7e0e4ba1a821bacce53a1e3df7e510c53a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78402788"
---
# <a name="deprecation-of-tls-10-and-11-in-iot-hub-and-device-provisioning-service"></a>Einstellung von TLS 1.0 und 1.1 in IoT Hub und Device Provisioning Service

Zur Bereitstellung einer erstklassigen Verschlüsselung wechseln IOT Hub und Device Provisioning Service (DPS) zu Transport Layer Security (TLS) 1.2 als Verschlüsselungsmechanismus der Wahl für IOT-Geräte und-Dienste. 

## <a name="supported-ciphers"></a>Unterstützte Verschlüsselungen

Die Zeitachse für die Verfügbarkeit verschiedener Verschlüsselungen, die im TLS-Handshake verwendet werden, lautet wie folgt:

* TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256 (derzeit unterstützt)
* TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384 (wird in der zweiten Hälfte von 2020 unterstützt)
* TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 (wird in der zweiten Hälfte von 2020 unterstützt)
* TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384 (wird in der zweiten Hälfte von 2020 unterstützt)


## <a name="customer-feedback"></a>Kundenfeedback

TLS 1.2 stellt eine branchenweit erstklassige Verschlüsselungsoption dar und wird wie geplant aktiviert. Dennoch würden wir gerne von unseren Kunden über ihre spezifischen Bereitstellungen und Schwierigkeiten bei der Einführung von TLS 1.2 informiert werden. Sie können Ihre Kommentare gerne an [iot_tls1_deprecation@microsoft.com](mailto:iot_tls1_deprecation@microsoft.com) senden.
