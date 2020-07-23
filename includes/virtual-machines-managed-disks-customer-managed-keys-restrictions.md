---
title: include file
description: include file
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 07/10/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: c303527c7411ead585e70fc2e31db4dd2d35e996
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2020
ms.locfileid: "86259823"
---
- Es werden ausschließlich [Software- und HSM RSA-Schlüssel](../articles/key-vault/keys/about-keys.md) der Größen 2.048 Bit, 3.072 Bit und 4.096 Bit unterstützt.
    - Für [HSM](../articles/key-vault/keys/hsm-protected-keys.md)-Schlüssel ist der **Premium**-Tarif von Azure Key Vault erforderlich.
- Datenträger, die aus benutzerdefinierten Images erstellt werden, die mit serverseitiger Verschlüsselung und vom Kunden verwalteten Schlüsseln verschlüsselt wurden, müssen mit denselben vom Kunden verwalteten Schlüsseln verschlüsselt werden und sich im selben Abonnement befinden.
- Momentaufnahmen, die von Datenträgern erstellt werden, die mit serverseitiger Verschlüsselung und vom Kunden verwalteten Schlüsseln verschlüsselt wurden, müssen mit denselben vom Kunden verwalteten Schlüsseln verschlüsselt werden.
- Alle Ressourcen, die sich auf Ihre vom Kunden verwalteten Schlüssel (Azure Key Vaults, Datenträgerverschlüsselungssätze, VMs, Datenträger und Momentaufnahmen) beziehen, müssen sich im selben Abonnement und in derselben Region befinden.
- Mit vom Kunden verwalteten Schlüsseln verschlüsselte Datenträger, Momentaufnahmen und Images können nicht in ein anderes Abonnement verschoben werden.
- Verwaltete Datenträger, die mithilfe von kundenseitig verwalteten Schlüsseln verschlüsselt werden, können nicht auch mit Azure Disk Encryption verschlüsselt werden.
- Pro Region und Abonnement können nur bis zu 50 Datenträgerverschlüsselungssätze erstellt werden.
- Informationen über die Verwendung von kundenseitig verwalteter Schlüssel mit Shared Image Gallerys finden Sie unter [Vorschau: Verwenden von kundenseitig verwalteten Schlüsseln zum Verschlüsseln von Images](../articles/virtual-machines/image-version-encryption.md).
