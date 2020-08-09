---
title: Erstellen und Verwalten von Azure Migrate-Projekten
description: Suchen, erstellen, verwalten und löschen Sie Projekte in Azure Migrate.
ms.topic: how-to
ms.date: 07/23/2020
ms.openlocfilehash: d60868f9d0d4c60291cfd92a9e8d11fd3f9a42b9
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87071806"
---
# <a name="create-and-manage-azure-migrate-projects"></a>Erstellen und Verwalten von Azure Migrate-Projekten

In diesem Artikel wird beschrieben, wie Sie [Azure Migrate](migrate-services-overview.md)-Projekte erstellen, verwalten und löschen.


## <a name="create-a-project-for-the-first-time"></a>Erstmaliges Erstellen eines Projekts

Wenn Sie Azure Migrate zum ersten Mal einrichten, erstellen Sie ein Projekt und fügen ein Bewertungs- oder Migrationstool hinzu. [Befolgen Sie diese Anweisungen](how-to-add-tool-first-time.md) für die erstmalige Einrichtung.

## <a name="create-additional-projects"></a>Erstellen zusätzlicher Projekte

Wenn Sie bereits über ein Azure Migrate-Projekt verfügen und ein zusätzliches Projekt erstellen möchten, gehen Sie wie folgt vor:  

1. Suchen Sie im [öffentlichen Azure-Portal](https://portal.azure.com) oder in [Azure Government](https://portal.azure.us) nach **Azure Migrate**.
2. Rufen Sie im Dashboard von Azure Migrate **Server** auf, und wählen Sie oben rechts in der Ecke **Ändern** aus.

   ![Ändern des Azure Migrate-Projekts](./media/create-manage-projects/switch-project.png)

3. Wählen Sie zum Erstellen eines neuen Projekts **Hier klicken** aus.

   ![Erstellen eines zweiten Azure Migrate-Projekts](./media/create-manage-projects/create-new-project.png)


## <a name="find-a-project"></a>Suchen eines Projekts

Suchen Sie ein Projekt wie folgt:

1. Suchen Sie im [Azure-Portal](https://portal.azure.com) nach **Azure Migrate**.
2. Rufen Sie im Dashboard von Azure Migrate **Server** auf, und wählen Sie oben rechts in der Ecke **Ändern** aus.

    ![Wechseln zu einem vorhandenen Azure Migrate-Projekt](./media/create-manage-projects/switch-project.png)

3. Wählen Sie das entsprechende Abonnement und das Azure Migrate-Projekt aus.


Wenn Sie das Projekt in der [vorherigen Version](migrate-services-overview.md#azure-migrate-versions) von Azure Migrate erstellt haben, finden Sie es wie folgt:

1. Suchen Sie im [Azure-Portal](https://portal.azure.com) nach **Azure Migrate**.
2. Wenn Sie ein Projekt in der vorherigen Version erstellt haben, wird im Dashboard von Azure Migrate ein Banner mit einem Verweis auf ältere Projekte angezeigt. Wählen Sie das Banner aus.

    ![Zugreifen auf vorhandene Projekte](./media/create-manage-projects/access-existing-projects.png)

3. Überprüfen Sie die Liste der alten Projekte.


## <a name="delete-a-project"></a>Löschen eines Projekts

Gehen Sie beim Löschen wie folgt vor:

1. Öffnen Sie die Azure-Ressourcengruppe, in der das Projekt erstellt wurde.
2. Wählen Sie auf der Seite für die Ressourcengruppe **Ausgeblendete Typen anzeigen** aus.
3. Wählen Sie das zu löschende Migrationsprojekt und die zugehörigen Ressourcen aus.
    - Der Ressourcentyp ist **Microsoft.Migrate/migrateprojects**.
    - Wenn die Ressourcengruppe ausschließlich vom Azure Migrate-Projekt verwendet wird, können Sie die gesamte Ressourcengruppe löschen.

Beachten Sie dabei Folgendes:

- Beim Löschen werden sowohl das Projekt als auch die Metadaten zu den gefundenen Computern gelöscht.
- Wenn Sie die ältere Version von Azure Migrate verwenden, öffnen Sie die Azure-Ressourcengruppe, in der das Projekt erstellt wurde. Wählen Sie das Migrationsprojekt aus, das Sie löschen möchten (der Ressourcentyp ist **Migrationsprojekt**).
- Wenn Sie die Abhängigkeitsanalyse mit einem Azure Log Analytics-Arbeitsbereich verwenden:
    - Wenn Sie dem Serverbewertungstool einen Log Analytics-Arbeitsbereich angefügt hatten, wird dieser nicht automatisch gelöscht. Derselbe Log Analytics-Arbeitsbereich kann für mehrere Szenarien verwendet werden.
    - Wenn Sie den Log Analytics-Arbeitsbereich löschen möchten, gehen Sie manuell vor.
- Das Löschen eines Projekts kann nicht rückgängig gemacht werden. Gelöschte Objekte können nicht wiederhergestellt werden.

### <a name="delete-a-workspace-manually"></a>Manuelles Löschen eines Arbeitsbereichs

1. Navigieren Sie zu dem Log Analytics-Arbeitsbereich, der dem Projekt angefügt ist.

    - Wenn Sie das Azure Migrate-Projekt nicht gelöscht haben, finden Sie den Link zum Arbeitsbereich unter **Essentials** > **Serverbewertung**.
       ![LA-Arbeitsbereich](./media/create-manage-projects/loganalytics-workspace.png).
       
    - Wenn Sie das Azure Migrate-Projekt bereits gelöscht haben, wählen Sie **Ressourcengruppen** im linken Bereich des Azure-Portals aus, und suchen Sie den Arbeitsbereich.
       
2. [Befolgen Sie die Anweisungen](../azure-monitor/platform/delete-workspace.md), um den Arbeitsbereich zu löschen.

## <a name="next-steps"></a>Nächste Schritte

Fügen Sie den Azure Migrate-Projekten Tools für die [Bewertung](how-to-assess.md) oder [Migration](how-to-migrate.md) hinzu.
