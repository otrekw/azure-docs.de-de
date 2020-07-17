---
title: Gewähren von rollenbasiertem Zugriff für Benutzer
titleSuffix: Azure Maps
description: Verwenden der rollenbasierten Zugriffssteuerung, um Benutzern Autorisierung für Azure Maps zu gewähren
author: philmea
ms.author: philmea
ms.date: 06/17/2020
ms.topic: include
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 86f89397f3685443071788580253ee11ce4b70be
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84988373"
---
## <a name="grant-role-based-access-for-users-to-azure-maps"></a>Gewähren von rollenbasierter Zugriffssteuerung für Benutzer in Azure Maps

Sie gewähren die *rollenbasierte Zugriffssteuerung* (RBAC), indem Sie mindestens einer Azure Maps-Rollendefinition für die Zugriffssteuerung eine Azure AD-Gruppe oder Sicherheitsprinzipale zuweisen. Informationen über die für Azure Maps verfügbaren Rollendefinitionen für die rollenbasierte Zugriffssteuerung finden Sie unter **Zugriffssteuerung (IAM)** . Wählen Sie **Rollen** aus, und suchen Sie dann nach Rollen, die mit *Azure Maps* beginnen.

* Informationen zur effizienten Verwaltung des Zugriffs einer großen Menge von Benutzern auf Azure Maps finden Sie unter [Azure AD-Gruppen](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-manage-groups).
* Damit Benutzer sich bei der Anwendung authentifizieren können, müssen die Benutzer in Azure AD erstellt werden. Siehe [Hinzufügen oder Löschen von Benutzern mithilfe von Azure AD](https://docs.microsoft.com/azure/active-directory/fundamentals/add-users-azure-active-directory).

Weitere Informationen zum effektiven Verwalten eines Verzeichnisses für Benutzer finden Sie unter [Azure AD](https://docs.microsoft.com/azure/active-directory/fundamentals/).

1. Navigieren Sie zu Ihrem **Azure Maps-Konto**. Wählen Sie **Zugriffssteuerung (IAM)**  > **Rollenzuweisungen** aus.

    ![Gewähren von RBAC](../media/how-to-manage-authentication/how-to-grant-rbac.png)

2. Wählen Sie auf der Registerkarte **Rollenzuweisungen** unter **Rolle** eine integrierte Azure Maps-Rollendefinition aus, z. B. **Azure Maps-Datenleser** oder **Azure Maps-Datenmitwirkender**. Wählen Sie unter **Zugriff zuweisen zu** die Option **Azure AD-Benutzer, -Gruppe oder -Dienstprinzipal** aus. Wählen Sie den Prinzipal nach Namen aus. Klicken Sie dann auf **Speichern**.

   * Lesen Sie die Details zum [Hinzufügen oder Entfernen von Rollenzuweisungen](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal).

> [!WARNING]
> Integrierte Azure Maps-Rollendefinitionen bieten einen sehr umfangreichen Autorisierungszugriff auf viele Azure Maps-REST-APIs. Informationen, wie Sie APIs für Benutzer auf ein Mindestmaß beschränken, finden Sie unter [Erstellen einer benutzerdefinierten Rollendefinition und Zuweisen von Benutzern](https://docs.microsoft.com/azure/role-based-access-control/custom-roles) zur benutzerdefinierten Rollendefinition. Dadurch werden Benutzer in die Lage versetzt, die geringsten Berechtigungen zu erhalten, die für die Anwendung erforderlich sind.