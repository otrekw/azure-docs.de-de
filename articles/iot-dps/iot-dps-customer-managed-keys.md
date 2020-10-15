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
ms.openlocfilehash: e9b5a549d550f117e080fc64193fbe29658f401d
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "85298224"
---
# <a name="encryption-of-data-at-rest-with-customer-managed-keys-for-device-provisioning-service"></a>Verschlüsselung ruhender Daten über kundenseitig verwaltete Schlüssel für den Device Provisioning-Dienst

## <a name="overview"></a>Übersicht

Der Gerätebereitstellungsdienst (Device Provisioning Service, DPS) unterstützt die Verschlüsselung ruhender Daten mit kundenseitig verwalteten Schlüsseln (Customer-Managed Key, CMK), auch als „Bring Your Own Key“ (BYOK) bezeichnet. DPS bietet Verschlüsselung von ruhenden Daten und während der Übertragung, wenn diese in unseren Rechenzentren geschrieben werden, und entschlüsselt sie, wenn Sie darauf zugreifen. Standardmäßig verwendet DPS von Microsoft verwaltete Schlüssel, um die ruhenden Daten zu verschlüsseln. Mit CMK können Sie eine zusätzliche Verschlüsselungsebene zusätzlich zur Standardplattformverschlüsselung erhalten, indem Sie sich dafür entscheiden, ruhende Daten mit einem Schlüsselverschlüsselungsschlüssel zu verschlüsseln, der über [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) verwaltet wird. Dadurch erhalten Sie die Flexibilität, Schlüssel zu erstellen, zu rotieren, zu deaktivieren und zu widerrufen. Wenn CMK für Ihren DPS konfiguriert ist, bedeutet dies, dass doppelte Verschlüsselung mit zwei Schutzebenen aktiviert ist und Ihre Daten aktiv geschützt werden. 

Für diese Funktion muss ein neuer DPS erstellt werden. Um diese Funktion auszuprobieren, kontaktieren Sie uns über den [Microsoft-Support](https://azure.microsoft.com/support/create-ticket/). Geben Sie Ihren Firmennamen und Ihre Abonnement-ID an, wenn Sie sich an den Microsoft-Support wenden.


## <a name="next-steps"></a>Nächste Schritte

* [Weitere Informationen zum Device Provisioning-Dienst](https://docs.microsoft.com/azure/iot-dps/)

* [Weitere Informationen zu Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-overview)
