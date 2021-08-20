---
title: 'Schnellstart: Erste Schritte zum Hinzufügen eines Administrators'
description: In diesem Tutorial erfahren Sie, wie Sie Ihrem Arbeitsbereich einen weiteren Administrator hinzufügen.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.subservice: workspace
ms.topic: tutorial
ms.date: 04/02/2021
ms.custom: subject-rbac-steps
ms.openlocfilehash: 7a03715a89b5319e341d1704719b020e1b61ef1d
ms.sourcegitcommit: 6bd31ec35ac44d79debfe98a3ef32fb3522e3934
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2021
ms.locfileid: "113219418"
---
# <a name="add-an-administrator-to-your-synapse-workspace"></a>Fügen Sie Ihrem Synapse-Arbeitsbereich einen Administrator hinzu.

In diesem Tutorial erfahren Sie, wie Sie Ihrem Synapse-Arbeitsbereich einen Administrator hinzufügen. Dieser Benutzer verfügt über vollständige Kontrolle über den Arbeitsbereich.

## <a name="overview"></a>Übersicht

Bisher haben wir im Leitfaden für die ersten Schritte im Mittelpunkt mit den Aktivitäten *den Aktivitäten die Sie im* Arbeitsbereich machen begonnen. Da Sie den Arbeitsbereich in Schritt 1 erstellt haben, sind Sie Administrator des Arbeitsbereichs Synapse. Nun werden wir ein anderer Benutzer Ryan () zu `ryan@contoso.com` einem Administrator machen. Nachdem der Vorgang abgeschlossen ist, kann Ryan alles ausführen, was Sie im Arbeitsbereich erledigen können.

## <a name="azure-rbac-owner-role-for-the-workspace"></a>Azure RBAC: Besitzer-Rolle für den Arbeitsbereich

1. Öffnen Sie das Azure-Portal, und öffnen Sie Ihren Synapse-Arbeitsbereich.
1. Wählen Sie auf der linken Seite **Zugriffssteuerung (IAM)** aus.
1. Wählen Sie **Hinzufügen** > **Rollenzuweisung hinzufügen** aus, um den Bereich „Rollenzuweisung hinzufügen“ zu öffnen.
1. Weisen Sie die folgende Rolle zu. Ausführliche Informationen finden Sie unter [Zuweisen von Azure-Rollen über das Azure-Portal](../role-based-access-control/role-assignments-portal.md).
    
    | Einstellung | Wert |
    | --- | --- |
    | Rolle | Besitzer |
    | Zugriff zuweisen zu | USER |
    | Member | ryan@contoso.com |

    ![Seite „Rollenzuweisung hinzufügen“ im Azure-Portal](../../includes/role-based-access-control/media/add-role-assignment-page.png)

1. Wählen Sie **Speichern** aus. 
 
 
## <a name="synapse-rbac-synapse-administrator-role-for-the-workspace"></a>Synapse RBAC: Synapse-Administrator Rolle für den Arbeitsbereich

Weisen Sie `ryan@contoso.com` der Synapse RBAC **Synapse-Administrator** Rolle im Arbeitsbereich zu.

1. Öffnen Sie Ihren Arbeitsbereich in Azure Synapse Studio.
1. Klicken Sie auf der linken Seite auf **Verwalten**, um den Verwaltungshub zu öffnen.
1. Wählen Sie unter **Sicherheit** die Option **Zugriffssteuerung** aus.
1. Wählen Sie **Hinzufügen**.
1. Lassen Sie den **Bereich** auf **Arbeitsbereich** eingestellt.
1. Fügen Sie die Rolle `ryan@contoso.com`**Synapse-Administrator** dazu. 
1. Wählen Sie dann **Anwenden** aus.
 
## <a name="azure-rbac-role-assignments-on-the-workspaces-primary-storage-account"></a>Azure RBAC: Rollenzuweisungen für das primäre Speicherkonto des Arbeitsbereichs

1. Öffnen Sie das Speicherkonto des primären Arbeitsbereichs im Azure-Portal.
1. Wählen Sie auf der linken Seite **Zugriffssteuerung (IAM)** aus.
1. Wählen Sie **Hinzufügen** > **Rollenzuweisung hinzufügen** aus, um den Bereich „Rollenzuweisung hinzufügen“ zu öffnen.
1. Weisen Sie die folgende Rolle zu. Ausführliche Informationen finden Sie unter [Zuweisen von Azure-Rollen über das Azure-Portal](../role-based-access-control/role-assignments-portal.md).
    
    | Einstellung | Wert |
    | --- | --- |
    | Rolle 1 | Besitzer |
    | Rolle 2| Mitwirkender an Azure Storage Blob-Daten|
    | Zugriff zuweisen zu | USER |
    | Member | ryan@contoso.com |

    ![Seite „Rollenzuweisung hinzufügen“ im Azure-Portal](../../includes/role-based-access-control/media/add-role-assignment-page.png)

## <a name="dedicated-sql-pools-db_owner-role"></a>Dedizierte SQL-Pools: db_owner Rolle

Weisen `ryan@contoso.com` Sie den **db_owner** für jeden dedizierten SQL-Pool im Arbeitsbereich zu.

```
CREATE USER [ryan@contoso.com] FROM EXTERNAL PROVIDER; 
EXEC sp_addrolemember 'db_owner', 'ryan@contoso.com'
```

## <a name="next-steps"></a>Nächste Schritte

* [Erste Schritte mit Azure Synapse Analytics](get-started.md)
* [Erstellen eines Arbeitsbereichs](quickstart-create-workspace.md)
* [Verwenden eines serverlosen SQL-Pools](quickstart-sql-on-demand.md)
