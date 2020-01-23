---
title: Verschlüsselung ruhender Daten mit Azure IoT Hub über kundenseitig verwaltete Schlüssel | Microsoft-Dokumentation
description: Verschlüsselung ruhender Daten mit vom Kunden verwalteten Schlüsseln für IoT Hub
author: ash2017
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/08/2020
ms.author: asrastog
ms.openlocfilehash: 1bb55d593878026bb3e57014a317b4fc0158d734
ms.sourcegitcommit: e9776e6574c0819296f28b43c9647aa749d1f5a6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/13/2020
ms.locfileid: "75913181"
---
# <a name="encryption-of-data-at-rest-with-customer-managed-keys-for-iot-hub"></a>Verschlüsselung ruhender Daten mit vom Kunden verwalteten Schlüsseln für IoT Hub

IoT Hub unterstützt die Verschlüsselung ruhender Daten mit kundenseitig verwalteten Schlüsseln (Customer-Managed Key, CMK), auch bezeichnet als Unterstützung von „Bring Your Own Key“ (BYOK) in Azure IoT Hub. Azure IoT Hub ermöglicht die Verschlüsselung von ruhenden Daten und während der Übertragung. Standardmäßig verwendet IoT Hub von Microsoft verwaltete Schlüssel, um die Daten zu verschlüsseln. Dank der CMK-Unterstützung haben Kunden jetzt die Möglichkeit, die ruhenden Daten mit einem Schlüsselverschlüsselungsschlüssel zu verschlüsseln, den sie mithilfe von [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) verwalten.

Diese Funktion erfordert die Erstellung eines neuen IoT-Hubs (Tarif Basic oder Standard) in einer der folgenden Regionen: „USA, Osten“, „USA, Westen 2“ oder „USA, Süden-Mitte“. Um diese Funktion auszuprobieren, kontaktieren Sie uns über den [Microsoft-Support](https://azure.microsoft.com/support/create-ticket/). Geben Sie Ihren Firmennamen und Ihre Abonnement-ID an, wenn Sie sich an den Microsoft-Support wenden.

## <a name="next-steps"></a>Nächste Schritte

* [Weitere Informationen zu IoT Hub](https://docs.microsoft.com/azure/iot-hub/about-iot-hub)

* [Weitere Informationen zu Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-overview)