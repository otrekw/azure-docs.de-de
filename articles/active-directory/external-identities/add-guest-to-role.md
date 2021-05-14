---
title: Hinzufügen eines B2B-Zusammenarbeitsbenutzers zu einer Rolle – Azure Active Directory
description: Hinzufügen eines Gastbenutzers zu einer Rolle in Azure Active Directory
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: how-to
ms.date: 05/08/2018
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.custom: it-pro, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: 455d0ee4855645104dda662785794225f5a23d2d
ms.sourcegitcommit: 38d81c4afd3fec0c56cc9c032ae5169e500f345d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2021
ms.locfileid: "109520532"
---
# <a name="grant-permissions-to-users-from-partner-organizations-in-your-azure-active-directory-tenant"></a>Gewähren von Berechtigungen für Benutzer von Partnerorganisationen in Ihrem Azure Active Directory-Mandanten

Die Benutzer der Azure Active Directory (Azure AD) B2B-Zusammenarbeit werden dem Verzeichnis als Gastbenutzer hinzugefügt, und die Gastberechtigungen im Verzeichnis sind standardmäßig eingeschränkt. Allerdings ist es in Ihrem Unternehmen möglicherweise erforderlich, dass einige Gastbenutzer Rollen mit höheren Berechtigungen in Ihrer Organisation übernehmen. Zur Unterstützung von Rollen mit höheren Berechtigungen können Gastbenutzer je nach Anforderungen Ihrer Organisation jeder gewünschten Rolle hinzugefügt werden.

Wenn einem Gastbenutzer eine Verzeichnisrolle zugewiesen ist, werden dem Gastbenutzer zusätzliche Berechtigungen gewährt, die in der Rolle enthalten sind, einschließlich grundlegender Leseberechtigungen. Weitere Informationen finden Sie unter [Integrierte Rollen in Azure AD](../roles/permissions-reference.md).

## <a name="default-role"></a>Standardrolle

![Screenshot mit der Standardverzeichnisrolle](./media/add-guest-to-role/default-role.png)

## <a name="global-administrator-role"></a>Globale Administratorrolle

![Screenshot mit der globalen Administratorrolle](./media/add-guest-to-role/global-admin-role.png)

## <a name="limited-administrator-role"></a>Administratorrolle mit eingeschränkten Berechtigungen

![Screenshot mit der eingeschränkten Administratorrolle](./media/add-guest-to-role/limited-admin-role.png)

## <a name="next-steps"></a>Nächste Schritte

- [Was ist die Azure AD B2B-Zusammenarbeit?](what-is-b2b.md)
- [Eigenschaften von B2B-Zusammenarbeitsbenutzern](user-properties.md)