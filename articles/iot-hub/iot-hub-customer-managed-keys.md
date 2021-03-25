---
title: Verschlüsselung ruhender Daten mit Azure IoT Hub über kundenseitig verwaltete Schlüssel | Microsoft-Dokumentation
description: Verschlüsselung ruhender Daten mit vom Kunden verwalteten Schlüsseln für IoT Hub
author: ash2017
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 06/17/2020
ms.author: asrastog
ms.openlocfilehash: 352da24b36124ff0446a81c1ecbc584da545bb16
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "92142205"
---
# <a name="encryption-of-data-at-rest-with-customer-managed-keys-for-iot-hub"></a>Verschlüsselung ruhender Daten mit vom Kunden verwalteten Schlüsseln für IoT Hub

IoT Hub unterstützt die Verschlüsselung ruhender Daten mit kundenseitig verwalteten Schlüsseln (Customer-Managed Key, CMK), auch bezeichnet als Unterstützung von „Bring Your Own Key“ (BYOK). Azure IoT Hub bietet Verschlüsselung von ruhenden Daten und während der Übertragung, wenn diese in unseren Rechenzentren geschrieben werden, und entschlüsselt sie, wenn Sie darauf zugreifen. Standardmäßig verwendet IoT Hub von Microsoft verwaltete Schlüssel, um die ruhenden Daten zu verschlüsseln. Mit CMK können Sie eine weitere Verschlüsselungsebene zusätzlich zur Standardverschlüsselung erhalten, indem Sie sich dafür entscheiden, ruhende Daten mit einem Schlüsselverschlüsselungsschlüssel zu verschlüsseln, der über [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) verwaltet wird. Dadurch erhalten Sie die Flexibilität, Zugriffssteuerungen zu erstellen, zu rotieren, zu deaktivieren und zu widerrufen. Wenn für Ihren IoT Hub BYOK konfiguriert ist, bieten wir darüber hinaus doppelte Verschlüsselung an, mit der eine zweite Schutzebene zur Verfügung steht, deren Verschlüsselungsschlüssel Sie über Ihren Azure Key Vault kontrollieren können.

Diese Funktion erfordert die Erstellung eines neuen IoT-Hubs (Tarif Basic oder Standard): Um diese Funktion auszuprobieren, kontaktieren Sie uns über den [Microsoft-Support](https://azure.microsoft.com/support/create-ticket/). Geben Sie Ihren Firmennamen und Ihre Abonnement-ID an, wenn Sie sich an den Microsoft-Support wenden.


## <a name="next-steps"></a>Nächste Schritte

* [Weitere Informationen zu IoT Hub](./about-iot-hub.md)

* [Weitere Informationen zu Azure Key Vault](../key-vault/general/overview.md)