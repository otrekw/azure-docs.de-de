---
author: baanders
description: Includedatei für den Schritt „Zugriffsberechtigungen“ bei der Einrichtung von Azure Digital Twins
ms.service: digital-twins
ms.topic: include
ms.date: 7/17/2020
ms.author: baanders
ms.openlocfilehash: e49abce74f5f4ab3a624def8ec1eb1d674711462
ms.sourcegitcommit: dd425ae91675b7db264288f899cff6add31e9f69
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/01/2021
ms.locfileid: "108759924"
---
In Azure Digital Twins wird [Azure Active Directory (Azure AD)](../articles/active-directory/fundamentals/active-directory-whatis.md) für die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) verwendet. Dies bedeutet, dass einem Benutzer eine Rolle mit entsprechenden Berechtigungen zugewiesen werden muss, bevor er Aufrufe auf Datenebene an Ihre Azure Digital Twins-Instanz richten kann.

Bei Azure Digital Twins ist dies die Rolle **Azure Digital Twins Data Owner** (Azure Digital Twins-Datenbesitzer). Weitere Informationen zu Rollen und Sicherheit finden Sie unter  [-Konzepte: Sicherheit für Azure Digital Twins-Lösungen](../articles/digital-twins/concepts-security.md).

> [!NOTE]
> Diese Rolle unterscheidet sich von der Azure AD-Rolle **Besitzer**, die ebenfalls im Bereich der Azure Digital Twins-Instanz zugewiesen werden kann. Es handelt sich hierbei um zwei unterschiedliche Verwaltungsrollen. Die Rolle „Besitzer“ gewährt keinen Zugriff auf Features der Datenebene, während dieser Zugriff mit **Azure Digital Twins-Datenbesitzer** gewährt wird.

In diesem Abschnitt erfahren Sie, wie Sie eine Rollenzuweisung für einen Benutzer in Ihrer Azure Digital Twins-Instanz erstellen, indem Sie die E-Mail-Adresse dieses Benutzers im Azure AD-Mandanten in Ihrem Azure-Abonnement verwenden. Je nach Ihrer Rolle in Ihrer Organisation können Sie diese Berechtigung für sich selbst oder für eine andere Person einrichten, die die Azure Digital Twins-Instanz verwaltet.