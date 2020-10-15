---
title: Hinzufügen von Migrationstools in Azure Migrate
description: Hinzufügen von Migrationstools in Azure Migrate
ms.topic: article
ms.date: 04/26/2020
ms.openlocfilehash: b6ac8ed64d3f12783cc16f0428874a19d027adf9
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "86109806"
---
# <a name="add-migration-tools"></a>Hinzufügen von Migrationstools

In diesem Artikel wird beschrieben, wie Sie in [Azure Migrate](./migrate-services-overview.md) Migrationstools hinzufügen.

- Wenn Sie ein Migrationstool hinzufügen möchten und noch kein Azure Migrate-Projekt angelegt haben, befolgen Sie die Anweisungen in diesem [Artikel](how-to-add-tool-first-time.md).
- Wenn Sie für die Migration ein ISV-Tool hinzugefügt haben, [befolgen Sie die Schritte](prepare-isv-movere.md) zur Vorbereitung auf die Arbeit mit dem Tool.

## <a name="select-a-migration-scenario"></a>Auswählen eines Datenmigrationsszenarios

1. Klicken Sie im Azure Migrate-Projekt auf **Übersicht**.
2. Wählen Sie das Migrationsszenario aus, das Sie verwenden möchten:

    - Um Computer und Workloads zu Azure zu migrieren, wählen Sie die Option **Server bewerten und migrieren** aus.
    - Um lokale SQL Server-Datenbanken zu migrieren, wählen Sie die Option **Datenbanken bewerten und migrieren** aus.
    - Um lokale Web-Apps zu Azure zu migrieren, wählen Sie die Option **Web-Apps bewerten und migrieren** aus.
    - Um große Mengen an lokalen Daten zu Azure im Offlinemodus zu migrieren, wählen Sie **Data Box bestellen** aus.

    ![Bewertungsszenario](./media/how-to-migrate/assess-scenario.png)

## <a name="select-a-server-migration-tool"></a>Auswählen eines Tools für die Servermigration

1. Klicken Sie auf **Server bewerten und migrieren**.
2. Wenn Sie in **Azure Migrate: Server** noch keine Migrationstools hinzugefügt haben, wählen Sie unter **Migrationstools** die Option **Klicken Sie hier, um ein Migrationstool hinzuzufügen** aus. Wenn Sie bereits Migrationstools hinzugefügt haben, wählen Sie unter **Weitere Migrationstools hinzufügen** die Option **Ändern** aus.

    > [!NOTE]
    > Wenn Sie zu einem anderen Projekt navigieren müssen, klicken Sie in **Azure Migrate: Server** neben **Details zu einem anderen Migrationsprojekt anzeigen** auf **Klicken Sie hier**.

3. Wählen Sie in **Azure Migrate** das gewünschte Migrationstool aus.
    - Wenn Sie Azure Migrate-Servermigration verwenden, können Sie Migrationen direkt im Azure Migrate-Projekt einrichten und ausführen.
    - Wenn Sie ein Bewertungstool eines Drittanbieters verwenden, navigieren Sie zu dem bereitgestellten Link für den ISV, und führen Sie die Migration gemäß deren Anweisungen aus.

## <a name="select-a-database-migration-tool"></a>Auswählen eines Datenbankmigrationstools

1. Klicken Sie auf **Datenbanken bewerten und migrieren**.
2. Klicken Sie unter **Datenbanken** auf **Tools hinzufügen**.
3. Wählen Sie unter „Tool hinzufügen“ > **Migrationstool auswählen** das Tool aus, das Sie zum Migrieren der Datenbank verwenden möchten.

## <a name="select-a-web-app-migration-tool"></a>Auswählen eines Web-App-Migrationstools

1. Klicken Sie auf **Web-Apps bewerten und migrieren**.
2. Folgen Sie dem Link zum Migrationstool für den Azure App Service. Verwenden Sie das Migrationstool, um folgende Aufgaben auszuführen:

    - **Onlinebewertung von Apps**: Sie können Apps mit einer öffentlichen URL mithilfe des Azure App Service-Migrations-Assistenten online bewerten und migrieren.
    - **.NET/PHP**: Für interne .NET- und PHP-Apps können Sie den Migrations-Assistenten herunterladen und ausführen.

## <a name="order-an-azure-data-box"></a>Bestellen von Azure Data Box

Um große Datenmengen zu Azure zu migrieren, können Sie eine Azure Data Box für die Offlinedatenübertragung bestellen.

1. Klicken Sie auf **Bestellen einer Data Box**.
2. Geben Sie in **Wählen Sie Ihre Azure Data Box aus** Ihr Abonnement an. 
3. Die Übertragung erfolgt als Import in Azure. Geben Sie die Datenquelle und die Azure-Zielregion für die Daten an.

## <a name="next-steps"></a>Nächste Schritte

Testen Sie eine Migration mithilfe von Azure Migrate-Servermigration für [Hyper-V](tutorial-migrate-hyper-v.md)- oder [VMware](tutorial-migrate-vmware.md)-VMs.
