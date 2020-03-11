---
title: Verschlüsselung ruhender Daten mit dem Azure Device Provisioning-Dienst über kundenseitig verwaltete Schlüssel | Microsoft-Dokumentation
description: Verschlüsselung ruhender Daten über kundenseitig verwaltete Schlüssel für den Device Provisioning-Dienst
author: chrissie926
manager: nberdy
ms.service: iot-dps
services: iot-dps
ms.topic: conceptual
ms.date: 02/24/2020
ms.author: menchi
ms.openlocfilehash: b065135715868094b7e248aebfca35f3b55a4c48
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2020
ms.locfileid: "77674784"
---
# <a name="encryption-of-data-at-rest-with-customer-managed-keys-for-device-provisioning-service"></a>Verschlüsselung ruhender Daten über kundenseitig verwaltete Schlüssel für den Device Provisioning-Dienst

## <a name="overview"></a>Übersicht

Der Device Provisioning-Dienst unterstützt die Verschlüsselung ruhender Daten mit kundenseitig verwalteten Schlüsseln (Customer-Managed Key, CMK), auch als „Bring Your Own Key“ (BYOK) bezeichnet. Der Device Provisioning-Dienst ermöglicht die Verschlüsselung von ruhenden Daten und von Daten während der Übertragung. Standardmäßig verwendet der Dienst von Microsoft verwaltete Schlüssel, um die Daten zu verschlüsseln. Dank der CMK-Unterstützung haben Kunden jetzt die Möglichkeit, die ruhenden Daten mit einem Schlüsselverschlüsselungsschlüssel zu verschlüsseln, den sie mithilfe von [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) verwalten.

Für diese Funktion muss in einer der folgenden Regionen ein neuer Device Provisioning-Dienst erstellt werden: „USA, Osten“, „USA, Westen 2“ oder „USA, Süden-Mitte“. Um diese Funktion auszuprobieren, kontaktieren Sie uns über den [Microsoft-Support](https://azure.microsoft.com/support/create-ticket/). Geben Sie Ihren Firmennamen und Ihre Abonnement-ID an, wenn Sie sich an den Microsoft-Support wenden.

## <a name="next-steps"></a>Nächste Schritte

* [Weitere Informationen zum Device Provisioning-Dienst](https://docs.microsoft.com/azure/iot-dps/)

* [Weitere Informationen zu Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-overview)