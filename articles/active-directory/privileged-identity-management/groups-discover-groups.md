---
title: Verwalten von zu Rollen zuweisbaren Gruppen als Gruppen mit privilegiertem Zugriff in Azure AD | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie von zu Rollen zuweisbaren Gruppen, die als Gruppen mit privilegiertem Zugriff verwaltet werden sollen, in Privileged Identity Management (PIM) integrieren können.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 08/17/2020
ms.author: curtand
ms.collection: M365-identity-device-management
ms.openlocfilehash: a707c66f2562fe442c58ce6292e51c4e67a20ae2
ms.sourcegitcommit: 927dd0e3d44d48b413b446384214f4661f33db04
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/26/2020
ms.locfileid: "88869529"
---
# <a name="manage-privileged-access-groups-preview-in-privileged-identity-management"></a>Verwalten von Gruppen mit privilegiertem Zugriff (Vorschau) in Privileged Identity Management

In Azure Active Directory (Azure AD) können Sie Cloudgruppen integrierte Azure AD-Rollen zuweisen, um die Verwaltung von Rollenzuweisungen zu vereinfachen. Zum Schutz von Azure AD-Rollen und für einen sicheren Zugriff können Sie nun Privileged Identity Management (PIM) verwenden, um den Just-in-Time-Zugriff für Mitglieder oder Besitzer dieser Gruppen zu verwalten. Um eine Azure AD-Gruppe, der Rollen zugewiesen werden können, als eine Gruppe mit privilegiertem Zugriff in Privileged Identity Management verwalten zu können, müssen Sie diese der Verwaltung in PIM unterstellen.

## <a name="identify-groups-to-manage"></a>Festlegen zu verwaltender Gruppen

Sie können in Azure AD eine Gruppe mit Rollenzuweisung erstellen, wie es unter [Erstellen einer Gruppe in Azure Active Directory für das Zuweisen von Rollen](../users-groups-roles/roles-groups-create-eligible.md) beschrieben ist. Sie müssen ein Besitzer der Gruppe sein, um sie der Verwaltung in Privileged Identity Management zu unterstellen.

1. [Melden Sie sich bei Azure AD](https://aad.portal.azure.com) mit der Berechtigung „Administrator für privilegierte Rollen“ an.
1. Wählen Sie **Gruppen** und dann die einer Rolle zuweisbare Gruppe aus, die Sie verwalten möchten. Sie können die Liste durchsuchen und filtern.

    ![Suchen einer Gruppe mit Rollenzuweisung für die Verwaltung in PIM](./media/groups-discover-groups/groups-list-in-azure-ad.png)

1. Öffnen Sie die Gruppe, und wählen Sie **Privilegierter Zugriff (Vorschau)** aus.

    ![Öffnen der Privileged Identity Management-Benutzeroberfläche](./media/groups-discover-groups/groups-discover-groups.png)

1. Wenn Ihre Gruppen noch nicht in PIM verwaltet werden, wählen Sie **Privilegierten Zugriff aktivieren** aus, um der Verwaltung zuzustimmen. Nur der globale Administrator oder Besitzer einer Gruppe kann diese Zustimmung erteilen.

    ![Bedarfsabhängige Zustimmung zur Verwaltung der Gruppe in Privileged Identity Management](./media/groups-discover-groups/consent-page.png)

1. Beginnen Sie mit der Verwaltung von Zuweisungen in PIM.

    ![Verwalten von Zuweisungen in Privileged Identity Management](./media/groups-discover-groups/groups-bring-under-management.png)

> [!NOTE]
> Sobald eine Gruppe mit privilegiertem Zugriff verwaltet wird, kann Sie nicht mehr aus der Verwaltung entfernt werden. Dadurch wird verhindert, dass andere Administratoren Privileged Identity Management-Einstellungen entfernen.

## <a name="next-steps"></a>Nächste Schritte

- [Konfigurieren von Zuweisungen für Gruppen mit privilegiertem Zugriff in Privileged Identity Management](pim-resource-roles-configure-role-settings.md)
- [Zuweisen von Gruppen mit privilegiertem Zugriff in Privileged Identity Management](pim-resource-roles-assign-roles.md)
