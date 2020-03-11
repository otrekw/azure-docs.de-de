---
title: Einstellung der Unterstützung für TLS 1.0 und 1.1 in IoT Hub und Device Provisioning Service (DPS) | Microsoft-Dokumentation
description: Richtlinien für die Einstellung der Unterstützung von TLS 1.0 und 1.1 und Chiffren in IOT Hub und DPS.
author: rezasherafat
ms.author: rezas
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 12/16/2019
ms.openlocfilehash: 8e2f8fd7f99c359949b02959cc442f2f3d3ebfff
ms.sourcegitcommit: 3c925b84b5144f3be0a9cd3256d0886df9fa9dc0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2020
ms.locfileid: "77912153"
---
# <a name="deprecation-of-tls-10-and-11-in-iot-hub-and-device-provisioning-service"></a>Einstellung von TLS 1.0 und 1.1 in IoT Hub und Device Provisioning Service

Zur Bereitstellung einer erstklassigen Verschlüsselung wechseln IOT Hub und Device Provisioning Service (DPS) zu Transport Layer Security (TLS) 1.2 als Verschlüsselungsmechanismus der Wahl für IOT-Geräte und-Dienste. TLS 1.0 und TLS 1.1 sowie einige nicht empfohlene Legacyverschlüsselungen werden daher ab dem **1. Juli 2020** nicht mehr unterstützt.


## <a name="impact"></a>Auswirkung
Basierend auf den spezifischen Situationen und Konfigurationen der Kunden hat die Einstellung der Unterstützung von TLS 1.0 und 1.1 und nicht empfohlenen Legacyverschlüsselungen eine erhebliche Auswirkung auf ihre IoT-Geräte und -Dienste, die mit dem IoT Hub oder DPS kommunizieren. In einigen Fällen können Geräte und Dienste, die mit diesen Änderungen nicht kompatibel sind, nach dem oben erwähnten Stichtag keine Verbindung mit dem IoT Hub oder DPS herstellen.


## <a name="supported-ciphers"></a>Unterstützte Verschlüsselungen

Die Zeitachse für die Verfügbarkeit verschiedener Verschlüsselungen, die im TLS-Handshake verwendet werden, lautet wie folgt:

* TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256 (derzeit unterstützt)
* TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384 (wird in der zweiten Hälfte von 2020 unterstützt)
* TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 (wird in der zweiten Hälfte von 2020 unterstützt)
* TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384 (wird in der zweiten Hälfte von 2020 unterstützt)


## <a name="customer-feedback"></a>Kundenfeedback

TLS 1.2 stellt eine branchenweit erstklassige Verschlüsselungsoption dar und wird wie geplant aktiviert. Dennoch würden wir gerne von unseren Kunden über ihre spezifischen Bereitstellungen und Schwierigkeiten bei der Einführung von TLS 1.2 informiert werden. Sie können Ihre Kommentare gerne an [iot_tls1_deprecation@microsoft.com](mailto:iot_tls1_deprecation@microsoft.com) senden.
