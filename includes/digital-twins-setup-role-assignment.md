---
author: baanders
description: Includedatei für den Schritt „Zugriffsberechtigungen“ bei der Einrichtung von Azure Digital Twins
ms.service: digital-twins
ms.topic: include
ms.date: 7/17/2020
ms.author: baanders
ms.openlocfilehash: 6f43a55b3a409a84e3baf99dae24af7616ea4385
ms.sourcegitcommit: 42107c62f721da8550621a4651b3ef6c68704cd3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2020
ms.locfileid: "87408162"
---
In Azure Digital Twins wird [Azure Active Directory (Azure AD)](../articles/active-directory/fundamentals/active-directory-whatis.md) für die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) verwendet. Dies bedeutet, dass einem Benutzer eine Rolle mit entsprechenden Berechtigungen zugewiesen werden muss, bevor er Aufrufe auf Datenebene an Ihre Azure Digital Twins-Instanz richten kann.

Bei Azure Digital Twins ist dies die Rolle _**Azure Digital Twins-Besitzer (Vorschau)**_. Weitere Informationen zu Rollen und Sicherheit finden Sie unter [ *-Konzepte: Sicherheit für Azure Digital Twins-Lösungen*](../articles/digital-twins/concepts-security.md).

In diesem Abschnitt erfahren Sie, wie Sie eine Rollenzuweisung für einen Benutzer in Ihrer Azure Digital Twins-Instanz erstellen, indem Sie die E-Mail-Adresse dieses Benutzers im Azure AD-Mandanten in Ihrem Azure-Abonnement verwenden. Je nach Ihrer Rolle in Ihrer Organisation können Sie diese Berechtigung für sich selbst oder für eine andere Person einrichten, die die Azure Digital Twins-Instanz verwaltet.

### <a name="assign-the-role"></a>Zuweisen der Rolle

Um einem Benutzer Berechtigungen zum Verwalten einer Azure Digital Twins-Instanz zu gewähren, müssen Sie ihm die Rolle _**Azure Digital Twins-Besitzer (Vorschau)**_ innerhalb der Instanz zuweisen.

> [!NOTE]
> Beachten Sie, dass sich diese Rolle von der Azure AD-Rolle *Besitzer* unterscheidet, die ebenfalls im Bereich der Azure Digital Twins-Instanz zugewiesen werden kann. Es handelt sich hierbei um zwei unterschiedliche Verwaltungsrollen. Die Azure AD-Rolle *Besitzer* gewährt keinen Zugriff auf Features der Datenebene, während dieser Zugriff mit *Azure Digital Twins-Besitzer (Vorschau)* gewährt wird.