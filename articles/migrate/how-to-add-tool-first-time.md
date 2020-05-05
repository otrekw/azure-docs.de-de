---
title: Hinzufügen eines Bewertungs-/Migrationstools in Azure Migrate
description: Hier wird beschrieben, wie Sie ein Azure Migrate-Projekt erstellt und ein Bewertungs-/Migrationstool hinzufügen.
ms.topic: how-to
ms.date: 04/16/2020
ms.openlocfilehash: 48bdea31d17ea1ddf0b983af962dce30b22d8dcf
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "81537728"
---
# <a name="add-an-assessmentmigration-tool-for-the-first-time"></a>Erstes Hinzufügen eines Bewertungs-/Migrationstools

In diesem Artikel wird beschrieben, wie Sie zum ersten Mal ein Bewertungs- oder Migrationstool zu einem [Azure Migrate](migrate-overview.md)-Projekt hinzufügen.  
Azure Migrate ist ein zentraler Hub zum Nachverfolgen der Ermittlung, Bewertung und Migration Ihrer lokalen Apps und Workloads sowie von VMs in der privaten/öffentlichen Cloud zu Azure. Der Hub stellt Azure Migrate-Tools für die Bewertung und Migration sowie andere Tools und [Angebote](migrate-services-overview.md#isv-integration) von unabhängigen Drittanbietern (Independent Software Vendors, ISVs) bereit. 

## <a name="create-a-project-and-add-a-tool"></a>Erstellen eines Projekts und Hinzufügen eines Tools

Richten Sie ein neues Azure Migrate-Projekt in einem Azure-Abonnement ein, und fügen Sie ein Tool hinzu.

- Ein Azure Migrate-Projekt wird zum Speichern von Ermittlungs-, Bewertungs- und Migrationsmetadaten verwendet, die in der von Ihnen bewerteten oder migrierten Umgebung gesammelt werden. 
- In einem Projekt können Sie ermittelte Objekte nachverfolgen und die Bewertung und Migration orchestrieren.

1. Wählen Sie im Azure-Portal **Alle Dienste** aus, und suchen Sie nach **Azure Migrate**.
2. Wählen Sie unter **Dienste** die Option **Azure Migrate** aus.

    ![Einrichten von Azure Migrate](./media/how-to-add-tool-first-time/azure-migrate-search.png)

3. Klicken Sie in der **Übersicht** auf **Server bewerten und migrieren**.
4. Klicken Sie unter **Server ermitteln, bewerten und migrieren** auf **Server bewerten und migrieren**.

    ![Ermitteln und Bewerten von Servern](./media/how-to-add-tool-first-time/assess-migrate.png)

1. Klicken Sie unter **Server ermitteln, bewerten und migrieren** auf **Tools hinzufügen**.
2. Wählen Sie unter **Projekt migrieren** Ihr Azure-Abonnement aus, und erstellen Sie bei Bedarf eine Ressourcengruppe.
3. Geben Sie unter **Projektdetails** den Projektnamen und die geografische Region an, in der Sie das Projekt erstellen möchten.  Beachten Sie die unterstützten geografischen Regionen für [öffentliche Clouds](migrate-support-matrix.md#supported-geographies-public-cloud) und [Azure Government-Clouds](migrate-support-matrix.md#supported-geographies-azure-government).

    ![Erstellen eines Azure Migrate-Projekts](./media/how-to-add-tool-first-time/migrate-project.png)

    - Die für das Projekt angegebene Region wird nur zum Speichern der Metadaten verwendet, die von den lokalen VMs erfasst werden. Bei der tatsächlichen Migration kann eine beliebige Zielregion ausgewählt werden.
    - Wenn Sie ein Projekt innerhalb einer bestimmten geografischen Region bereitstellen müssen, verwenden Sie zum Erstellen eines Projekts die folgende API. Geben Sie neben dem Standort die Abonnement-ID, den Ressourcengruppennamen und Projektnamen an. Prüfen Sie die geografischen Regionen für [öffentliche](migrate-support-matrix.md#supported-geographies-public-cloud) und [Government-Clouds](migrate-support-matrix.md#supported-geographies-azure-government).

        `PUT /subscriptions/<subid>/resourceGroups/<rg>/providers/Microsoft.Migrate/MigrateProjects/<mymigrateprojectname>?api-version=2018-09-01-preview "{location: 'centralus', properties: {}}"`   


4. Klicken Sie auf **Weiter**, und fügen Sie ein Bewertungs- oder Migrationstool hinzu.

    > [!NOTE]
    > Wenn Sie ein Projekt erstellen, müssen Sie mindestens ein Bewertungs- oder Migrationstool hinzufügen.

5. Fügen Sie unter **Bewertungstool auswählen** ein Bewertungstool hinzu. Wenn Sie kein Bewertungstool benötigen, wählen Sie die Option **Hinzufügen eines Bewertungstools vorerst überspringen** > **Weiter** aus. 
2. Fügen Sie unter **Migrationstool auswählen** nach Bedarf ein Migrationstool hinzu. Wenn Sie zum jetzigen Zeitpunkt kein Migrationstool benötigen, wählen Sie die Option **Hinzufügen eines Migrationstools vorerst überspringen** > **Weiter** aus.
3. Überprüfen Sie die Einstellungen unter **Review + add tools** (Überprüfen und Tools hinzufügen), und klicken Sie auf **Tools hinzufügen**.

Nachdem Sie das Projekt erstellt haben, können Sie zusätzliche Tools für Bewertung und Migration von Servern, Workloads, Datenbanken und Web-Apps auswählen.

## <a name="create-additional-projects"></a>Erstellen zusätzlicher Projekte

Unter bestimmten Umständen müssen Sie möglicherweise zusätzliche Azure Migrate-Projekte erstellen. Beispielsweise, wenn Sie über Rechenzentren in unterschiedlichen geografischen Regionen verfügen oder wenn Sie Metadaten in einer anderen geografischen Region speichern müssen. Erstellen Sie wie folgt ein zusätzliches Projekt:

1. Klicken Sie im Azure Migrate-Projekt auf **Server** oder **Datenbanken**.
2. Klicken Sie in der oberen rechten Ecke neben dem Namen des aktuellen Projekts auf **Ändern**.
3. Wählen Sie unter **Einstellungen** die Option **Hier klicken, um ein neues Projekt zu erstellen** aus.
4. Erstellen Sie ein neues Projekt, und fügen Sie ein neues Tool hinzu, wie im vorherigen Verfahren beschrieben.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie zusätzliche Tools für [Bewertung](how-to-assess.md) und [Migration](how-to-migrate.md) hinzufügen. 
