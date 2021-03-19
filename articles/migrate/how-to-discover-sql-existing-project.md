---
title: Ermitteln von SQL Server-Instanzen in einem vorhandenen Azure Migrate-Projekt
description: Erfahren Sie, wie SQL Server-Instanzen in einem vorhandenen Azure Migrate-Projekt ermittelt werden.
author: rashi-ms
ms.author: rajosh
ms.manager: abhemraj
ms.topic: how-to
ms.date: 11/23/2020
ms.openlocfilehash: ca0052eebd8d3c8e80943ca8c0e0346216436800
ms.sourcegitcommit: 6386854467e74d0745c281cc53621af3bb201920
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2021
ms.locfileid: "102452763"
---
# <a name="discover-sql-server-instances-in-an-existing-project"></a>Ermitteln von SQL Server-Instanzen in einem vorhandenen Projekt 

In diesem Artikel wird beschrieben, wie Sie SQL Server-Instanzen und -Datenbanken in einem [Azure Migrate](./migrate-services-overview.md)-Projekt ermitteln, das vor der Vorschau der Azure SQL-Bewertungsfunktion erstellt wurde.

Wenn Sie SQL Server-Instanzen und -Datenbanken ermitteln, die auf lokalen Computern ausgeführt werden, können Sie einen Migrationspfad zu Azure SQL identifizieren und anpassen. Die Azure Migrate-Appliance führt diese Ermittlung mithilfe der Domänenanmeldeinformationen oder SQL Server-Anmeldeinformationen für die Authentifizierung aus, die Zugriff auf die SQL Server-Instanzen und -Datenbanken haben, die auf den Zielservern ausgeführt werden. Dieser Ermittlungsvorgang erfolgt ohne Agents, d. h. auf den Zielservern ist nichts installiert.

> [!Note]
> Das Feature für die Ermittlung und Bewertung von SQL Server-Instanzen und -Datenbanken, die in Ihrer VMware-Umgebung ausgeführt werden, befindet sich nun in der Vorschauphase. Verwenden Sie [**diesen Link**](https://aka.ms/AzureMigrate/SQL), und erstellen Sie ein Projekt in der Region **Australien, Osten**, um dieses Feature zu testen. Falls Sie bereits über ein Projekt in „Australien, Osten“ verfügen und dieses Feature ausprobieren möchten, sollten Sie sicherstellen, dass Sie die in diesem Artikel beschriebenen [**Voraussetzungen**](how-to-discover-sql-existing-project.md) erfüllt haben.

## <a name="before-you-start"></a>Vorbereitung

- Stellen Sie Folgendes sicher: 
    - Vor der Ankündigung der SQL-Bewertungsfunktion für Ihre Region wurde ein [Azure Migrate-Projekt](./create-manage-projects.md) erstellt.
    - Das Azure [Migrate-Serverbewertungstool](./how-to-assess.md) wurde einem Projekt hinzugefügt.
- Sie haben die Unterstützung und die Anforderungen für die App-Ermittlung [überprüft](./migrate-support-matrix-vmware.md#vmware-requirements).
-  Stellen Sie sicher, dass auf den Servern, auf denen Sie die App-Ermittlung ausführen, PowerShell Version 2.0 oder höher sowie die VMware Tools (höher als 10.2.0) installiert sind.
- Überprüfen Sie die [Anforderungen](./migrate-appliance.md) für die Bereitstellung der Azure Migrate-Appliance.
- Stellen Sie sicher, dass Sie über die [erforderlichen Rollen](./create-manage-projects.md#verify-permissions) im Abonnement verfügen, um Ressourcen zu erstellen.
- Stellen Sie sicher, dass Ihr Gerät Zugriff auf das Internet hat.

## <a name="enable-discovery-of-sql-server-instances-and-databases"></a>Aktivieren Sie die Ermittlung von SQL Server-Instanzen und -Datenbanken.

1. Wählen Sie in Ihrem Azure Migrate-Projekt eine der folgenden Optionen aus:
    - Wählen Sie auf der Hub-Kachel **Nicht aktiviert** oder die :::image type="content" source="./media/how-to-discover-sql-existing-project/hub-not-enabled.png" alt-text="Azure Migrate Hub-Kachel mit nicht aktivierter SQL-Ermittlung"::: aus.
    - Wählen Sie **Nicht aktiviert** für einen beliebigen Eintrag auf der Seite „Serverermittlung“ unter der Spalte „SQL-Instanzen“ aus. :::image type="content" source="./media/how-to-discover-sql-existing-project/discovery-not-enabled.png" alt-text="Blatt „Von Azure Migrate ermittelte Server“ mit nicht aktivierter SQL-Ermittlung":::.
2. Führen Sie unter „SQL Server-Instanzen und -Datenbanken ermitteln“ die folgenden Schritte aus:
    - Wählen Sie **Upgrade** aus, um die erforderliche Ressource zu erstellen.
        :::image type="content" source="./media/how-to-discover-sql-existing-project/discovery-upgrade-appliance.png" alt-text="Schaltfläche zum Aktualisieren der Azure Migrate-Appliance":::
    - Bestätigen Sie, dass die auf der Appliance ausgeführten Dienste auf die neuesten Versionen aktualisiert wurden. Starten Sie hierzu den Appliancekonfigurations-Manager auf Ihrem Applianceserver, und wählen Sie im Bereich „Erforderliche Komponenten“ die Option „Appliancedienste anzeigen“ aus.
        - Die Appliance und zugehörige Komponenten werden automatisch aktualisiert. :::image type="content" source="./media/how-to-discover-sql-existing-project/appliance-services-version.png" alt-text="Überprüfen Sie die Geräteversion":::.
    - Fügen Sie im Bereich „Anmeldeinformationen und Ermittlungsquellen verwalten“ des Appliancekonfigurations-Managers Domänen- oder SQL Server-Authentifizierungsanmeldeinformationen hinzu, die über Sysadmin-Zugriff auf die SQL Server-Instanz und die zu ermittelnden Datenbanken verfügen. 
    Sie können entweder die automatische Zuordnung von Anmeldeinformationen der Appliance nutzen oder die Anmeldeinformationen manuell dem jeweiligen Server zuordnen, wie [hier](/azure/migrate/tutorial-discover-vmware#start-continuous-discovery) gezeigt wird.
        
    Hinweise:
    - Stellen Sie sicher, dass der Softwarebestand bereits aktiviert ist, oder stellen Sie Domänen- oder Nicht-Domänenanmeldeinformationen zur Aktivierung bereit. Zum Ermitteln von SQL Server-Instanzen muss der Softwarebestand ausgeführt werden.
    - Die Appliance versucht, die Domänenanmeldeinformationen mit AD zu validieren, wenn sie hinzugefügt werden. Stellen Sie sicher, dass der Applianceserver über eine Netzwerksichtverbindung mit dem AD-Server verfügt, dem die Anmeldeinformationen zugeordnet sind. Anmeldeinformationen, die SQL Server-Authentifizierung zugeordnet sind, werden nicht überprüft. 

3. Nachdem die gewünschten Anmeldeinformationen hinzugefügt wurden, wählen Sie „Ermittlung starten“ aus, um die Überprüfung zu starten.

> [!Note] 
>Sehen Sie für die Ausführung der SQL-Ermittlung einige Zeit vor, bevor Sie Bewertungen für Azure SQL erstellen. Wenn die Ermittlung von SQL Server-Instanzen und -Datenbanken nicht abgeschlossen werden kann, werden die entsprechenden Instanzen im Bewertungsbericht als **Unbekannt** markiert.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum Erstellen einer [Azure SQL-Bewertung](./how-to-create-azure-sql-assessment.md)
- Weitere Informationen zur [Azure SQL-Bewertungen](./concepts-azure-sql-assessment-calculation.md)