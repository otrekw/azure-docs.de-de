---
author: msmbaldwin
ms.service: key-vault
ms.topic: include
ms.date: 08/27/2020
ms.author: msmbaldwin
ms.openlocfilehash: 20ef9a4f30aafee562096e584c4b80fef236b062
ms.sourcegitcommit: 260a2541e5e0e7327a445e1ee1be3ad20122b37e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/21/2021
ms.locfileid: "108749721"
---
Key Vault unterstützt bis zu 1.024 Zugriffsrichtlinieneinträge, wobei jeder Eintrag einen eindeutigen Satz von Berechtigungen für einen bestimmten Sicherheitsprinzipal erteilt. Aufgrund dieser Einschränkung empfiehlt es sich, Zugriffsrichtlinien möglichst Gruppen von Benutzern anstelle von einzelnen Benutzern zuzuweisen. Das Verwenden von Gruppen vereinfacht die Verwaltung von Berechtigungen für mehrere Personen in Ihrer Organisation deutlich. Weitere Informationen finden Sie unter [Verwalten des Zugriffs auf Apps und Ressourcen mithilfe von Azure Active Directory-Gruppen](../articles/active-directory/fundamentals/active-directory-manage-groups.md).

Ausführliche Informationen zur Key Vault-Zugriffssteuerung finden Sie unter [Azure Key Vault – Sicherheitsfunktionen: Identitäts- und Zugriffsverwaltung](../articles/key-vault/general/security-features.md#identity-management).