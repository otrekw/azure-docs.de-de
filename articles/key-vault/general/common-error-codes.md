---
title: Allgemeine Fehlercodes für Azure Key Vault | Microsoft-Dokumentation
description: Allgemeine Fehlercodes für Azure Key Vault
services: key-vault
author: sebansal
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: reference
ms.date: 09/29/2020
ms.author: mbaldwin
ms.openlocfilehash: 25c79229a09db912903fba825f0d48f571880745
ms.sourcegitcommit: fbb620e0c47f49a8cf0a568ba704edefd0e30f81
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91876794"
---
# <a name="common-error-codes-for-azure-key-vault"></a>Allgemeine Fehlercodes für Azure Key Vault

Die in der folgenden Tabelle aufgelisteten Fehlercodes können von einem Vorgang in Azure Key Vault zurückgegeben werden.

| Fehlercode | Meldung für den Benutzer |
|--|--|
| VaultAlreadyExists |  Der angegebene Schlüsseltresor ist bereits vorhanden (vorläufig gelöscht oder in einem anderen Abonnement). |
| VaultNameNotValid |  Der Tresorname muss 24 Zeichen lang sein, alphanumerisch sein und mit einem Buchstaben beginnen. |
| AccessDenied |  Möglicherweise fehlen in der Zugriffsrichtlinie Berechtigungen, um diesen Vorgang durchzuführen. |
| ForbiddenByFirewall |  Die Clientadresse ist nicht autorisiert, und der Aufrufer ist kein vertrauenswürdiger Dienst. |
| ConflictError |  Sie fordern mehrere Vorgänge für dasselbe Element an.  |
| RegionNotSupported |  Die angegebene Azure-Region wird für diese Ressource nicht unterstützt. |
| SkuNotSupported |  Der angegebene SKU-Typ wird für diese Ressource nicht unterstützt. |
| ResourceNotFound |  Die angegebene Azure-Ressource wurde nicht gefunden. |
| CertificateExpired |  Überprüfen Sie das Ablaufdatum und die Gültigkeitsdauer des Zertifikats. |


## <a name="next-steps"></a>Nächste Schritte

- [Entwicklerhandbuch zu Azure-Schlüsseltresor](developers-guide.md)
- Weitere Informationen zum [Authentifizieren bei Key Vault](authentication.md)
