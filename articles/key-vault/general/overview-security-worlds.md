---
title: Azure Key Vault-Sicherheitsumgebungen (Security Worlds) | Microsoft Docs
description: Azure Key Vault ist ein mehrinstanzenfähiger Dienst. Er verwendet einen Pool von HSMs in jeder Azure-Region. Alle Regionen in einer geografischen Region haben eine gemeinsame kryptografische Grenze.
ms.service: key-vault
ms.subservice: general
ms.topic: conceptual
author: msmbaldwin
ms.author: mbaldwin
ms.date: 07/03/2017
ms.openlocfilehash: 0d82a3cb4c08d47b6827072378b9827037d32412
ms.sourcegitcommit: 6686a3d8d8b7c8a582d6c40b60232a33798067be
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/20/2021
ms.locfileid: "107751812"
---
# <a name="azure-key-vault-security-worlds-and-geographic-boundaries"></a>Sicherheitsumgebungen und geografische Grenzen von Azure Key Vault

Azure-Produkte sind in einer Reihe von [Azure-Geografien](https://azure.microsoft.com/en-us/global-infrastructure/geographies/)verfügbar, wobei jede Azure-Geografie mindestens eine Region enthält. Die Geografie Europa enthält z. B. zwei Regionen : „Europa, Norden“ und „Europa, Westen“, während die einzige Region in der Geografie „Brasilien“ „Brasilien, Süden“ ist.

Azure Key Vault ist ein mehrinstanzenfähiger Dienst, der einen Pool von Hardwaresicherheitsmodulen (Hardware Security Modules, HSMs) verwendet. Alle HSMs in einer Geografie haben dieselbe kryptografische Grenze, die auch als „Sicherheitsumgebung“ bezeichnet wird. Jede Geografie entspricht einer einzelnen Sicherheitsumgebung und umgekehrt.

„USA, Osten“ und „USA, Westen“ verwenden dieselbe Sicherheitsumgebung, weil sie der Geografie (USA) angehören. Ebenso verwenden alle Azure-Regionen in Japan wie alle Azure-Regionen in Australien und so weiter jeweils die gleiche Sicherheitsumgebung.

>[!NOTE]
> Ausnahmen bilden USA DOD OSTEN und USA DOD MITTE, die jeweils über eigene Sicherheitsumgebungen verfügen.

## <a name="backup-and-restore-behavior"></a>Sichern und Wiederherstellen – Verhalten

Eine Sicherung, die von einem Schlüssel aus einem Schlüsseltresor in einer Azure-Region erstellt wurde, kann in einem Schlüsseltresor in einer anderen Azure-Region wiederhergestellt werden, sofern die zwei folgenden Bedingungen erfüllt sind:

- Beide Azure-Regionen gehören derselben Geografie an.
- Beide Schlüsseltresore gehören zum selben Azure-Abonnement.

Beispielsweise kann eine Sicherung, die von einem Schlüssel in einem Schlüsseltresor für „Indien, Westen“ erstellt wurde, in einem anderen Schlüsseltresor im selben Abonnement in der Geografischen Region Indien (die Regionen „Indien, Westen“, „Indien, Mitte“ und „Indien, Süden“) wiederhergestellt werden.

## <a name="next-steps"></a>Nächste Schritte

- Siehe [Microsoft-Produkte nach Region](https://azure.microsoft.com/regions/services/)
