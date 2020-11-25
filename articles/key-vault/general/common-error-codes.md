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
ms.openlocfilehash: a543f03cb73d9eae8eaa81eeb3a37fd59e4e6a81
ms.sourcegitcommit: c157b830430f9937a7fa7a3a6666dcb66caa338b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/17/2020
ms.locfileid: "94685799"
---
# <a name="common-error-codes-for-azure-key-vault"></a>Allgemeine Fehlercodes für Azure Key Vault

Die in der folgenden Tabelle aufgelisteten Fehlercodes können von einem Vorgang in Azure Key Vault zurückgegeben werden.

| Fehlercode | Meldung für den Benutzer |
|--|--|
| VaultAlreadyExists |  Ihr neuer Schlüsseltresor konnte nicht mit dem angegebenen Namen erstellt werden, weil der Name bereits verwendet wird. Wenn Sie kürzlich einen Schlüsseltresor mit diesem Namen gelöscht haben, befindet er sich möglicherweise noch im vorläufig gelöschten Zustand. [Hier](https://docs.microsoft.com/azure/key-vault/general/key-vault-recovery?tabs=azure-portal#list-recover-or-purge-a-soft-deleted-key-vault) können Sie überprüfen, ob der Tresor noch im vorläufig gelöschten Zustand vorhanden ist. |
| VaultNameNotValid |  Der Tresorname muss 24 Zeichen lang sein, alphanumerisch sein und mit einem Buchstaben beginnen. |
| AccessDenied |  Möglicherweise fehlen in der Zugriffsrichtlinie Berechtigungen, um diesen Vorgang durchzuführen. |
| ForbiddenByFirewall |  Die Clientadresse ist nicht autorisiert, und der Aufrufer ist kein vertrauenswürdiger Dienst. |
| ConflictError |  Sie fordern mehrere Vorgänge für dasselbe Element an.  |
| RegionNotSupported |  Die angegebene Azure-Region wird für diese Ressource nicht unterstützt. |
| SkuNotSupported |  Der angegebene SKU-Typ wird für diese Ressource nicht unterstützt. |
| ResourceNotFound |  Die angegebene Azure-Ressource wurde nicht gefunden. |
| ResourceGroupNotFound | Die angegebene Azure-Ressourcengruppe wurde nicht gefunden. |
| CertificateExpired |  Überprüfen Sie das Ablaufdatum und die Gültigkeitsdauer des Zertifikats. |


## <a name="next-steps"></a>Nächste Schritte

- [Entwicklerhandbuch zu Azure-Schlüsseltresor](developers-guide.md)
- Weitere Informationen zum [Authentifizieren bei Key Vault](authentication.md)
