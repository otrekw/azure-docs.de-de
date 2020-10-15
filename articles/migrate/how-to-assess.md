---
title: Hinzufügen von Bewertungstools in Azure Migrate
description: Hier erfahren Sie, wie Sie Bewertungstools in Azure Migrate hinzufügen.
ms.topic: how-to
ms.date: 04/26/2020
ms.openlocfilehash: 5151135a57b7c2780485d85299ead5df83ea851e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "86122114"
---
# <a name="add-assessment-tools"></a>Hinzufügen von Bewertungstools

In diesem Artikel wird beschrieben, wie Sie in [Azure Migrate](./migrate-services-overview.md) Bewertungstools hinzufügen. 

- Wenn Sie ein Bewertungstool hinzufügen möchten und noch nicht über ein Azure Migrate-Projekt verfügen, hilft Ihnen [dieser Artikel](how-to-add-tool-first-time.md) weiter.
- Wenn Sie für die Bewertung ein ISV-Tool oder Movere hinzugefügt haben, [befolgen Sie die Schritte](prepare-isv-movere.md) zur Vorbereitung auf die Arbeit mit dem Tool.

## <a name="select-an-assessment-scenario"></a>Auswählen eines Bewertungsszenarios

1. Klicken Sie im Azure Migrate-Projekt auf **Übersicht**.
2. Wählen Sie das Bewertungsszenario aus, das Sie verwenden möchten:

    - Wählen Sie **Server bewerten und migrieren**, um Computer und Workloads zu ermitteln und für die Migration zu Azure zu bewerten.
    - Um lokale SQL Server-Datenbanken zu bewerten, wählen Sie die Option **Datenbanken bewerten und migrieren** aus.
    - Wählen Sie die Option **Web-Apps bewerten und migrieren**, um lokale Web-Apps zu bewerten.

    ![Bewertungsszenario](./media/how-to-assess/assess-scenario.png)

## <a name="select-a-server-assessment-tool"></a>Auswählen eines Tools für die Serverbewertung 

1. Klicken Sie auf **Server bewerten und migrieren**.
2. Wenn Sie in **Azure Migrate: Server** noch kein Bewertungstool hinzugefügt haben, wählen Sie unter **Bewertungstools** die Option **Klicken Sie hier, um ein Bewertungstool hinzuzufügen**. Falls Sie bereits Bewertungstools hinzugefügt haben, wählen Sie unter **Weitere Bewertungstools hinzufügen** die Option **Ändern**.

    > [!NOTE]
    > Wenn Sie zu einem anderen Projekt navigieren müssen, klicken Sie in **Azure Migrate: Server** neben **Details zu einem anderen Migrationsprojekt anzeigen** auf **Klicken Sie hier**.

3. Wählen Sie in **Azure Migrate** das gewünschte Bewertungstool aus.

    - Bei Verwendung der Azure Migrate-Serverbewertung können Sie Bewertungen direkt im Azure Migrate-Projekt einrichten, ausführen und anzeigen.
    - Navigieren Sie bei Verwendung eines anderen Bewertungstools über den bereitgestellten Link zur entsprechenden Site, und führen Sie die Bewertung gemäß der angegebenen Anleitung durch.


## <a name="select-a-database-assessment-tool"></a>Auswählen eines Tools für die Datenbankbewertung

1. Klicken Sie auf **Datenbanken bewerten und migrieren**.
2. Klicken Sie unter **Datenbanken** auf **Tools hinzufügen**.
3. Wählen Sie unter „Tool hinzufügen“ > **Bewertungstool auswählen** das Tool aus, das Sie zum Bewerten der Datenbank verwenden möchten.

## <a name="select-a-web-app-assessment-tool"></a>Auswählen eines Tools für die Web-App-Bewertung

1. Klicken Sie auf **Web-Apps bewerten und migrieren**.
2. Folgen Sie dem Link zum Migrationstool für den Azure App Service. Verwenden Sie das Migrationstool, um folgende Aufgaben auszuführen:

    - **Onlinebewertung von Apps**: Sie können Apps mit einer öffentlichen URL online bewerten, indem Sie den Azure App Service-Migrations-Assistenten verwenden.
    - **.NET/PHP**: Für interne .NET- und PHP-Apps können Sie den Migrations-Assistenten herunterladen und ausführen.



## <a name="next-steps"></a>Nächste Schritte

Probieren Sie eine Bewertung aus, indem Sie die Azure Migrate-Serverbewertung für [VMware](tutorial-prepare-vmware.md)-VMs, [Hyper-V](tutorial-prepare-hyper-v.md) oder [physische Server](tutorial-prepare-physical.md) verwenden.
