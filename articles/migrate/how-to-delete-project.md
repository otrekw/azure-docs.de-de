---
title: Löschen eines Azure Migrate-Projekts
description: In diesem Artikel erfahren Sie, wie Sie mithilfe des Azure-Portals ein Azure Migrate-Projekt löschen können.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 10/22/2019
ms.author: raynew
ms.openlocfilehash: c58081e7cf88ac4820aa7d4f367b7344f3340a77
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91307873"
---
# <a name="delete-an-azure-migrate-project"></a>Löschen eines Azure Migrate-Projekts

In diesem Artikel wird beschrieben, wie Sie ein [Azure Migrate](./migrate-services-overview.md)-Projekt löschen.


## <a name="before-you-start"></a>Vorbereitung

Vor dem Löschen eines Projekts:

- Wenn Sie ein Projekt löschen, werden das Projekt und die ermittelten Computermetadaten gelöscht.
- Wenn Sie dem Serverbewertungstool für die Abhängigkeitsanalyse einen Log Analytics-Arbeitsbereich angefügt haben, müssen Sie entscheiden, ob Sie den Arbeitsbereich löschen möchten. 
    - Der Arbeitsbereich wird nicht automatisch gelöscht. Er muss manuell gelöscht werden.
    - Überprüfen Sie, wofür ein Arbeitsbereich verwendet wird, bevor Sie ihn löschen. Derselbe Log Analytics-Arbeitsbereich kann für mehrere Szenarien verwendet werden.
    - Bevor Sie das Projekt löschen, finden Sie einen Link zum Arbeitsbereich in **Azure Migrate – Server** > **Azure Migrate – Serverbewertung** unter **OMS-Arbeitsbereich**.
    - Zum Löschen eines Arbeitsbereichs, nachdem ein Projekt gelöscht wurde, suchen Sie den Arbeitsbereich in der relevanten Ressourcengruppe und befolgen Sie [diese Anweisungen](../azure-monitor/platform/delete-workspace.md).


## <a name="delete-a-project"></a>Löschen eines Projekts


1. Öffnen Sie im Azure-Portal die Ressourcengruppe, in der das Projekt erstellt wurde.
2. Wählen Sie auf der Seite für die Ressourcengruppe **Ausgeblendete Typen anzeigen** aus.
3. Wählen Sie das Projekt und die zugehörigen Ressourcen aus, die Sie löschen möchten.
    - Der Ressourcentyp für Azure Migrate-Projekte ist **Microsoft.Migrate/migrateprojects**.
    - Überprüfen Sie im nächsten Abschnitt die Ressourcen, die für die Ermittlung, Bewertung und Migration in einem Azure Migrate-Projekt erstellt wurden.
    - Wenn die Ressourcengruppe nur das Azure Migrate-Projekt enthält, können Sie die gesamte Ressourcengruppe löschen.
    - Wenn Sie ein Projekt aus der früheren Version von Azure Migrate löschen möchten, sind die Schritte identisch. Der Ressourcentyp für diese Projekte ist **Migrationsprojekt**.


## <a name="created-resources"></a>Erstellte Ressourcen

Diese Tabelle enthält eine Übersicht über die Ressourcen, die für die Ermittlung, Bewertung und Migration in einem Azure Migrate-Projekt erstellt werden.

> [!NOTE]
> Löschen Sie den Schlüsseltresor mit Vorsicht, da er Sicherheitsschlüssel enthalten kann.

### <a name="vmwarephysical-server"></a>VMware/physischer Server

**Ressource** | **Typ**
--- | ---
"Appliancename"kv | Schlüsseltresor
"Appliancename"site | Microsoft.OffAzure/VMwareSites
"ProjectName" | Microsoft.Migrate/migrateprojects
"ProjectName"project | Microsoft.Migrate/assessmentProjects
"ProjectName"rsvault | Recovery Services-Tresor
"ProjectName"-MigrateVault-* | Recovery Services-Tresor
migrateappligwsa* | Speicherkonto
migrateapplilsa* | Speicherkonto
migrateapplicsa* | Speicherkonto
migrateapplikv* | Schlüsseltresor
migrateapplisbns16041 | Service Bus- Namespace

### <a name="hyper-v-vm"></a>Virtueller Hyper-V-Computer 

**Ressource** | **Typ**
--- | ---
"ProjectName" | Microsoft.Migrate/migrateprojects
"ProjectName"project | Microsoft.Migrate/assessmentProjects
HyperV*kv | Schlüsseltresor
HyperV*Site | Microsoft.OffAzure/HyperVSites
"ProjectName"-MigrateVault-* | Recovery Services-Tresor


## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie zusätzliche Tools für [Bewertung](how-to-assess.md) und [Migration](how-to-migrate.md) hinzufügen. 
