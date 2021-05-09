---
title: Löschen eines Azure Migrate-Projekts
description: In diesem Artikel erfahren Sie, wie Sie mithilfe des Azure-Portals ein Azure Migrate-Projekt löschen können.
author: ms-psharma
ms.author: panshar
ms.manager: abhemraj
ms.topic: how-to
ms.date: 10/22/2019
ms.openlocfilehash: 8c94bb23f5d514fef5cdacb855657efdf5219631
ms.sourcegitcommit: 3ed0f0b1b66a741399dc59df2285546c66d1df38
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/19/2021
ms.locfileid: "107714738"
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
    - Zum Löschen eines Arbeitsbereichs, nachdem ein Projekt gelöscht wurde, suchen Sie den Arbeitsbereich in der relevanten Ressourcengruppe und befolgen Sie [diese Anweisungen](../azure-monitor/logs/delete-workspace.md).


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

### <a name="projects-with-public-endpoint-connectivity"></a>Projekte mit Verbindungen über öffentliche Endpunkte

#### <a name="vmwarephysical-server"></a>VMware/physischer Server

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
migrateapplisbns* | Service Bus- Namespace

#### <a name="hyper-v-vm"></a>Virtueller Hyper-V-Computer

**Ressource** | **Typ**
--- | ---
"ProjectName" | Microsoft.Migrate/migrateprojects
"ProjectName"project | Microsoft.Migrate/assessmentProjects
HyperV*kv | Schlüsseltresor
HyperV*Site | Microsoft.OffAzure/HyperVSites
"ProjectName"-MigrateVault-* | Recovery Services-Tresor

<br/>
In den folgenden Tabellen sind die Ressourcen zusammengefasst, die von Azure Migrate zum Ermitteln, Bewerten und Migrieren von Servern über ein privates Netzwerk mithilfe von [Azure Private Link](./how-to-use-azure-migrate-with-private-endpoints.md) erstellt werden.

### <a name="projects-with-private-endpoint-connectivity"></a>Projekte mit Verbindungen über private Endpunkte

#### <a name="vmware-vms---agentless-migrations"></a>VMware-VMs: Migrationen ohne Agent

**Typ** | **Ressource** | **Privater Endpunkt <br/>** |
--- | --- | ---
Microsoft.Migrate/migrateprojects | "ProjectName" | "ProjectName"\*pe 
Ermittlungsstandort (Hauptstandort) | "ProjectName"*mastersite | "ProjectName"\*mastersite\*pe 
Microsoft.Migrate/assessmentProjects | "ApplianceName"*project | "ApplianceName"\*project\*pe 
Schlüsseltresor | "ProjectName"*kv | "ProjectName"\*kv\*pe
Microsoft.OffAzure/VMwareSites | "ApplianceName"*site | Nicht verfügbar
Recovery Services-Tresor | "ApplianceName"*vault | Nicht verfügbar
Speicherkonto | "ApplianceName"*usa | "ApplianceName"\*usa\*pe
Recovery Services-Tresor | "ProjectName"-MigrateVault-* | Nicht verfügbar
Speicherkonto | migrateappligwsa* | Nicht verfügbar
Speicherkonto | migrateapplilsa* | Nicht verfügbar
Schlüsseltresor | migrateapplikv* | Nicht verfügbar
Service Bus- Namespace | migrateapplisbns* | Nicht verfügbar

#### <a name="hyper-v-vms"></a>Virtuelle Hyper-V-Computer 

**Typ** | **Ressource** | **Privater Endpunkt <br/>** |
--- | --- | ---
Microsoft.Migrate/migrateprojects | "ProjectName" | "ProjectName"\*pe 
Ermittlungsstandort (Hauptstandort) | "ProjectName"*mastersite | "ProjectName"\*mastersite\*pe 
Microsoft.Migrate/assessmentProjects | "ApplianceName"*project | "ApplianceName"\*project\*pe 
Schlüsseltresor | "ProjectName"*kv | "ProjectName"\*kv\*pe
Microsoft.OffAzure/HyperVSites | "ApplianceName"*site | Nicht verfügbar
Recovery Services-Tresor | "ProjectName"-MigrateVault-* | "ProjectName"-MigrateVault-*pe

#### <a name="physical-servers--aws-vms--gcp-vms"></a>Physische Server/AWS-VMs/GCP-VMs 

**Typ** | **Ressource** | **Privater Endpunkt <br/>** |
--- | --- | ---
Microsoft.Migrate/migrateprojects | "ProjectName" | "ProjectName"\*pe 
Ermittlungsstandort (Hauptstandort) | "ProjectName"*mastersite | "ProjectName"\*mastersite\*pe 
Microsoft.Migrate/assessmentProjects | "ApplianceName"*project | "ApplianceName"\*project\*pe 
Schlüsseltresor | "ProjectName"*kv | "ProjectName"\*kv\*pe
Microsoft.OffAzure/serversites | "ApplianceName"*site | Nicht verfügbar
Recovery Services-Tresor | "ProjectName"-MigrateVault-* | "ProjectName"-MigrateVault-*pe


## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie zusätzliche Tools für [Bewertung](how-to-assess.md) und [Migration](how-to-migrate.md) hinzufügen. 
