---
title: Gewähren von rollenbasiertem Zugriff für Benutzer
titleSuffix: Azure Maps
description: Verwenden der rollenbasierten Zugriffssteuerung, um Benutzern Autorisierung für Azure Maps zu gewähren
author: anastasia-ms
ms.author: v-stharr
ms.date: 06/17/2020
ms.topic: include
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 234c3358b98b7af677793fba58c1602a8bd976f0
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "101102817"
---
## <a name="grant-role-based-access-for-users-to-azure-maps"></a>Gewähren von rollenbasierter Zugriffssteuerung für Benutzer in Azure Maps

Sie gewähren die *rollenbasierte Zugriffssteuerung in Azure (Azure RBAC)* , indem Sie mindestens einer Azure Maps-Rollendefinition eine Azure AD-Gruppe oder Sicherheitsprinzipale zuweisen. Informationen über die für Azure Maps verfügbaren Rollendefinitionen für die rollenbasierte Zugriffssteuerung von Azure finden Sie unter **Zugriffssteuerung (IAM)** . Wählen Sie **Rollen** aus, und suchen Sie dann nach Rollen, die mit *Azure Maps* beginnen.

* Informationen zur effizienten Verwaltung des Zugriffs einer großen Menge von Benutzern auf Azure Maps finden Sie unter [Azure AD-Gruppen](../../active-directory/fundamentals/active-directory-manage-groups.md).
* Damit Benutzer sich bei der Anwendung authentifizieren können, müssen die Benutzer in Azure AD erstellt werden. Siehe [Hinzufügen oder Löschen von Benutzern mithilfe von Azure AD](../../active-directory/fundamentals/add-users-azure-active-directory.md).

Weitere Informationen zum effektiven Verwalten eines Verzeichnisses für Benutzer finden Sie unter [Azure AD](../../active-directory/fundamentals/index.yml).

1. Navigieren Sie zu Ihrem **Azure Maps-Konto**. Wählen Sie **Zugriffssteuerung (IAM)**  > **Rollenzuweisungen** aus.

    ![Gewähren von Zugriff mithilfe von Azure RBAC](../media/how-to-manage-authentication/how-to-grant-rbac.png)

2. Wählen Sie auf der Registerkarte **Rollenzuweisungen** unter **Rolle** eine integrierte Azure Maps-Rollendefinition aus, z. B. **Azure Maps-Datenleser** oder **Azure Maps-Datenmitwirkender**. Wählen Sie unter **Zugriff zuweisen zu** die Option **Azure AD-Benutzer, -Gruppe oder -Dienstprinzipal** aus. Wählen Sie den Prinzipal nach Namen aus. Klicken Sie dann auf **Speichern**.

   * Weitere Informationen finden Sie unter [Zuweisen von Azure-Rollen](../../role-based-access-control/role-assignments-portal.md).

> [!WARNING]
> Integrierte Azure Maps-Rollendefinitionen bieten einen sehr umfangreichen Autorisierungszugriff auf viele Azure Maps-REST-APIs. Informationen, wie Sie APIs für Benutzer auf ein Mindestmaß beschränken, finden Sie unter [Erstellen einer benutzerdefinierten Rollendefinition und Zuweisen von Benutzern](../../role-based-access-control/custom-roles.md) zur benutzerdefinierten Rollendefinition. Dadurch werden Benutzer in die Lage versetzt, die geringsten Berechtigungen zu erhalten, die für die Anwendung erforderlich sind.