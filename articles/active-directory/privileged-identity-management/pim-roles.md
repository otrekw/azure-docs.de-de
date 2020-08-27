---
title: Rollen, die nicht in Privileged Identity Management verwaltet werden können – Azure Active Directory | Microsoft-Dokumentation
description: In diesem Artikel werden die Rollen beschrieben, die Sie nicht in Azure AD Privileged Identity Management (PIM) verwalten können.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 05/11/2020
ms.author: curtand
ms.custom: pim ; H1Hack27Feb2017;oldportal;it-pro;
ms.collection: M365-identity-device-management
ms.openlocfilehash: 862b304d1f36567aecd4452f91f31d1a50c3198a
ms.sourcegitcommit: 9c3cfbe2bee467d0e6966c2bfdeddbe039cad029
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/24/2020
ms.locfileid: "88782480"
---
# <a name="roles-you-cant-manage-in-privileged-identity-management"></a>Rollen, die nicht in Privileged Identity Management verwaltet werden können

Mit Azure Active Directory (Azure AD) Privileged Identity Management (PIM) können Sie alle [Azure AD-Rollen](../users-groups-roles/directory-assign-admin-roles.md) und alle [Azure-Rollen](../../role-based-access-control/built-in-roles.md) verwalten. Zu diesen Azure-Rollen gehören auch benutzerdefinierten Rollen, die an Ihre Verwaltungsgruppen, Abonnements, Ressourcengruppen und Ressourcen angefügt sind. Es gibt jedoch einige Rollen, die Sie nicht verwalten können. In diesem Artikel werden die Rollen beschrieben, die Sie nicht in Privileged Identity Management verwalten können.

## <a name="classic-subscription-administrator-roles"></a>Administrator für klassisches Abonnement

Die folgenden Administratorrollen klassischer Abonnements können Sie nicht in Privileged Identity Management verwalten:

- Kontoadministrator
- Dienstadministrator
- Co-Administrator

Weitere Informationen zu Administratorrollen klassischer Abonnements finden Sie unter [Administratorrollen für klassische Abonnements, Azure-Rollen und Azure AD-Administratorrollen](../../role-based-access-control/rbac-and-directory-admin-roles.md).

## <a name="what-about-office-365-admin-roles"></a>Informationen zu Office 365-Administratorrollen

Es werden alle Office 365-Rollen in der Azure AD-Rollenumgebung und im Administratorportal unterstützt, z. B. „Exchange-Administrator“ und „SharePoint-Administrator“, aber es werden keine spezifischen Rollen innerhalb von Exchange-RBAC oder SharePoint-RBAC unterstützt. Weitere Informationen zu diesen Office 365-Diensten finden Sie unter [Office 365-Administratorrollen](/office365/admin/add-users/about-admin-roles).

> [!NOTE]
> Bei berechtigten Benutzern für die Rolle „SharePoint-Administrator“, die Rolle „Geräteadministrator“ sowie allen Rollen, die versuchen, auf das Microsoft Security and Compliance Center zuzugreifen, können nach dem Aktivieren ihrer Rolle Verzögerungen von bis zu einigen Stunden auftreten. Wir arbeiten mit diesen Teams zusammen daran, diese Probleme zu beheben.

## <a name="next-steps"></a>Nächste Schritte

- [Zuweisen von Azure AD-Rollen in PIM](pim-how-to-add-role-to-user.md)
- [Zuweisen von Azure-Ressourcenrollen in PIM](pim-resource-roles-assign-roles.md)