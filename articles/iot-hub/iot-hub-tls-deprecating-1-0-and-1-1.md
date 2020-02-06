---
title: Einstellung der Unterstützung für TLS 1.0 und 1.1 in IoT Hub und Device Provisioning Service (DPS) | Microsoft-Dokumentation
description: Richtlinien für die Einstellung der Unterstützung von TLS 1.0 und 1.1 und Chiffren in IOT Hub und DPS.
author: rezasherafat
ms.author: rezas
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 12/16/2019
ms.openlocfilehash: 95733f579740f2928cda917eec0023bf00d53076
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76722658"
---
# <a name="deprecation-of-tls-10-and-11-in-iot-hub-and-device-provisioning-service"></a>Einstellung von TLS 1.0 und 1.1 in IoT Hub und Device Provisioning Service

Zur Bereitstellung einer erstklassigen Verschlüsselung wechseln IOT Hub und Device Provisioning Service (DPS) zu Transport Layer Security (TLS) 1.2 als Verschlüsselungsmechanismus der Wahl für IOT-Geräte und-Dienste. TLS 1.0 und TLS 1.1 sowie einige nicht empfohlene Legacyverschlüsselungen werden daher ab dem **1. Juli 2020** nicht mehr unterstützt.


## <a name="impact"></a>Auswirkung
Basierend auf den spezifischen Situationen und Konfigurationen der Kunden hat die Einstellung der Unterstützung von TLS 1.0 und 1.1 und nicht empfohlenen Legacyverschlüsselungen eine erhebliche Auswirkung auf ihre IoT-Geräte und -Dienste, die mit dem IoT Hub oder DPS kommunizieren. In einigen Fällen können Geräte und Dienste, die mit diesen Änderungen nicht kompatibel sind, nach dem oben erwähnten Stichtag keine Verbindung mit dem IoT Hub oder DPS herstellen.


## <a name="supported-ciphers"></a>Unterstützte Verschlüsselungen

Beim TLS-Handshake können nur die folgenden Verschlüsselungen verwendet werden:

* TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256
* TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
* TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256
* TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384


## <a name="customer-feedback"></a>Kundenfeedback

TLS 1.2 stellt eine branchenweit erstklassige Verschlüsselungsoption dar und wird wie geplant aktiviert. Dennoch würden wir gerne von unseren Kunden über ihre spezifischen Bereitstellungen und Schwierigkeiten bei der Einführung von TLS 1.2 informiert werden. Sie können Ihre Kommentare gerne an [iot_tls1_deprecation@microsoft.com](mailto:iot_tls1_deprecation@microsoft.com) senden.