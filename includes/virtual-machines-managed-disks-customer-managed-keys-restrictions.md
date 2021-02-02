---
title: include file
description: include file
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 08/24/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 75acbb30c2bf811b7ae72d6939b9f164554fdd32
ms.sourcegitcommit: fc8ce6ff76e64486d5acd7be24faf819f0a7be1d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/26/2021
ms.locfileid: "98860113"
---
- Es werden ausschließlich [Software- und HSM RSA-Schlüssel](../articles/key-vault/keys/about-keys.md) der Größen 2.048 Bit, 3.072 Bit und 4.096 Bit unterstützt.
    - Für [HSM](../articles/key-vault/keys/hsm-protected-keys.md)-Schlüssel ist der **Premium**-Tarif von Azure Key Vault erforderlich.
- Datenträger, die aus benutzerdefinierten Images erstellt werden, die mit serverseitiger Verschlüsselung und vom Kunden verwalteten Schlüsseln verschlüsselt wurden, müssen mit denselben vom Kunden verwalteten Schlüsseln verschlüsselt werden und sich im selben Abonnement befinden.
- Momentaufnahmen, die von Datenträgern erstellt werden, die mit serverseitiger Verschlüsselung und vom Kunden verwalteten Schlüsseln verschlüsselt wurden, müssen mit denselben vom Kunden verwalteten Schlüsseln verschlüsselt werden.
- Alle Ressourcen, die sich auf Ihre vom Kunden verwalteten Schlüssel (Azure Key Vaults, Datenträgerverschlüsselungssätze, VMs, Datenträger und Momentaufnahmen) beziehen, müssen sich im selben Abonnement und in derselben Region befinden.
- Mit vom Kunden verwalteten Schlüsseln verschlüsselte Datenträger, Momentaufnahmen und Images können nicht in andere Ressourcengruppen und Abonnements verschoben werden.
- Verwaltete Datenträger, die bisher oder aktuell mit Azure Disk Encryption verschlüsselt wurden, können nicht mit kundenseitig verwalteten Schlüsseln verschlüsselt werden.
- Pro Region und Abonnement können nur bis zu 1.000 Datenträgerverschlüsselungssätze erstellt werden.
- Informationen über die Verwendung von kundenseitig verwalteter Schlüssel mit Shared Image Gallerys finden Sie unter [Vorschau: Verwenden von kundenseitig verwalteten Schlüsseln zum Verschlüsseln von Images](../articles/virtual-machines/image-version-encryption.md).
