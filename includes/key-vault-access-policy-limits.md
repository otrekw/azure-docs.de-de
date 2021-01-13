---
author: msmbaldwin
ms.service: key-vault
ms.topic: include
ms.date: 08/27/2020
ms.author: msmbaldwin
ms.openlocfilehash: 9e4d60e501ffc5b61c87a80b8dd13698cca28737
ms.sourcegitcommit: 2aa52d30e7b733616d6d92633436e499fbe8b069
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/06/2021
ms.locfileid: "97934532"
---
Key Vault unterstützt bis zu 1.024 Zugriffsrichtlinieneinträge, wobei jeder Eintrag einen eindeutigen Satz von Berechtigungen für einen bestimmten Sicherheitsprinzipal erteilt. Aufgrund dieser Einschränkung empfiehlt es sich, Zugriffsrichtlinien möglichst Gruppen von Benutzern anstelle von einzelnen Benutzern zuzuweisen. Das Verwenden von Gruppen vereinfacht die Verwaltung von Berechtigungen für mehrere Personen in Ihrer Organisation deutlich. Weitere Informationen finden Sie unter [Verwalten des Zugriffs auf Apps und Ressourcen mithilfe von Azure Active Directory-Gruppen](../articles/active-directory/fundamentals/active-directory-manage-groups.md).

Ausführliche Informationen zur Key Vault-Zugriffssteuerung finden Sie unter [Azure Key Vault – Sicherheit: Identitäts- und Zugriffsverwaltung](../articles/key-vault/general/security-overview.md#identity-management).