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
ms.openlocfilehash: 83d2fa59654e038586a7f23eedbe7c656873f35c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84976573"
---
# <a name="encryption-of-data-at-rest-with-customer-managed-keys-for-iot-hub"></a>Verschlüsselung ruhender Daten mit vom Kunden verwalteten Schlüsseln für IoT Hub

IoT Hub unterstützt die Verschlüsselung ruhender Daten mit kundenseitig verwalteten Schlüsseln (Customer-Managed Key, CMK), auch bezeichnet als Unterstützung von „Bring Your Own Key“ (BYOK). Azure IoT Hub bietet Verschlüsselung von ruhenden Daten und während der Übertragung, wenn diese in unseren Rechenzentren geschrieben werden, und entschlüsselt sie, wenn Sie darauf zugreifen. Standardmäßig verwendet IoT Hub von Microsoft verwaltete Schlüssel, um die ruhenden Daten zu verschlüsseln. Mit CMK können Sie eine weitere Verschlüsselungsebene zusätzlich zur Standardverschlüsselung erhalten, indem Sie sich dafür entscheiden, ruhende Daten mit einem Schlüsselverschlüsselungsschlüssel zu verschlüsseln, der über [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) verwaltet wird. Dadurch erhalten Sie die Flexibilität, Zugriffssteuerungen zu erstellen, zu rotieren, zu deaktivieren und zu widerrufen. Wenn für Ihren IoT Hub BYOK konfiguriert ist, bieten wir darüber hinaus doppelte Verschlüsselung an, mit der eine zweite Schutzebene zur Verfügung steht, deren Verschlüsselungsschlüssel Sie über Ihren Azure Key Vault kontrollieren können.

Diese Funktion erfordert die Erstellung eines neuen IoT-Hubs (Tarif Basic oder Standard): Um diese Funktion auszuprobieren, kontaktieren Sie uns über den [Microsoft-Support](https://azure.microsoft.com/support/create-ticket/). Geben Sie Ihren Firmennamen und Ihre Abonnement-ID an, wenn Sie sich an den Microsoft-Support wenden.


## <a name="next-steps"></a>Nächste Schritte

* [Weitere Informationen zu IoT Hub](https://docs.microsoft.com/azure/iot-hub/about-iot-hub)

* [Weitere Informationen zu Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-overview)
