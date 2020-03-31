---
title: Bewerten einer großen Anzahl physischer Server für die Migration zu Azure mit Azure Migrate | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine große Anzahl von physischen Servern mithilfe des Azure Migrate-Diensts für die Migration zu Azure bewerten.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 01/19/2020
ms.author: hamusa
ms.openlocfilehash: a19a1b6e7416667079ab07fc5440ee8828c26bf4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "76294836"
---
# <a name="assess-large-numbers-of-physical-servers-for-migration-to-azure"></a>Bewerten einer großen Anzahl physischer Server für die Migration zu Azure

In diesem Artikel wird beschrieben, wie eine große Anzahl lokaler physischer Server mithilfe des Azure Migrate-Serverbewertungstools für die Migration zu Azure bewertet wird.

[Azure Migrate](migrate-services-overview.md) stellt einen Hub mit Tools bereit, die Ihnen dabei helfen, Apps, Infrastrukturen und Workloads zu ermitteln, zu bewerten und zu Microsoft Azure zu migrieren. Der Hub umfasst Azure Migrate-Tools sowie Angebote von unabhängigen Drittanbietern (Independent Software Vendors, ISVs). 


In diesem Artikel werden folgende Vorgehensweisen behandelt:
> [!div class="checklist"]
> * Planen Sie die Bewertung in gewünschtem Umfang.
> * Konfigurieren Sie Azure-Berechtigungen, und bereiten Sie die physischen Server für die Bewertung vor.
> * Erstellen Sie ein Azure Migrate-Projekt, und erstellen Sie eine Bewertung.
> * Überprüfen Sie die Bewertung beim Planen der Migration.


> [!NOTE]
> Wenn Sie einen Proof of Concept ausprobieren möchten, um einige Server zu bewerten, bevor Sie eine Bewertung im gewünschtem Umfang durchführen, lesen Sie unsere [Tutorialreihe](tutorial-prepare-physical.md).

## <a name="plan-for-assessment"></a>Planen für die Bewertung

Bei der Planung für die Bewertung einer großen Anzahl von physischen Servern sind einige Dinge zu berücksichtigen:

- **Planen von Azure Migrate-Projekten**: Finden Sie heraus, wie Azure Migrate-Projekte bereitgestellt werden. Wenn sich Ihre Rechenzentren beispielsweise in verschiedenen geografischen Regionen befinden oder Sie ermittlungs-, bewertungs- oder migrationsspezifische Metadaten in einer anderen geografischen Region speichern müssen, benötigen Sie möglicherweise mehrere Projekte.
- **Planen von Appliances**: Azure Migrate verwendet eine lokale Azure Migrate-Appliance, die auf einem Windows-Computer bereitgestellt wird, um Server kontinuierlich für die Bewertung und Migration zu identifizieren. Die Appliance überwacht Umgebungsänderungen wie z.B. das Hinzufügen von VMs, Datenträgern oder Netzwerkadaptern. Sie sendet außerdem deren Meta- und Leistungsdaten an Azure. Sie müssen herausfinden, wie viele Appliances bereitgestellt werden sollen.


## <a name="planning-limits"></a>Planen von Einschränkungen
 
Orientieren Sie sich bei der Planung an den in dieser Tabelle zusammengefassten Einschränkungen.

**Planung** | **Einschränkungen**
--- | --- 
**Azure Migrate-Projekte** | Bewerten Sie bis zu 35.000 Server in einem Projekt.
**Azure Migrate-Appliance** | Eine einzelne Appliance kann bis zu 250 Server ermitteln.<br/> Eine einzelne Appliance kann nur einer einzelnen Azure Migrate-Ressource zugeordnet werden.<br/> Einer einzelnen Azure Migrate-Ressource können beliebig viele Appliances zugeordnet werden. <br/><br/> 
**Gruppe** | Sie können einer einzelnen Gruppe bis zu 35.000 Server hinzufügen.
**Azure Migrate-Bewertung** | Sie können bis zu 35.000 Server in einem einzelnen Vorgang bewerten.


## <a name="other-planning-considerations"></a>Weitere Überlegungen zur Planung

- Um die Ermittlung von der Appliance aus zu starten, müssen Sie jeden physischen Server auswählen. 

## <a name="prepare-for-assessment"></a>Vorbereiten für die Bewertung

Bereiten Sie Azure und die physischen Server auf die Serverbewertung vor. 

1. Überprüfen Sie die [Unterstützungsanforderungen und -einschränkungen für physische Server](migrate-support-matrix-physical.md).
2. Richten Sie Berechtigungen für Ihr Azure-Konto zur Interaktion mit Azure Migrate ein.
3. Bereiten Sie die physischen Server vor.

Befolgen Sie die Anweisungen in [diesem Tutorial](tutorial-prepare-physical.md), um diese Einstellungen zu konfigurieren.

## <a name="create-a-project"></a>Erstellen eines Projekts

Führen Sie in Übereinstimmung mit Ihren Planungsanforderungen folgende Schritte aus:

1. Erstellen eines Azure Migrate-Projekts
2. Fügen Sie den Projekten das Azure Migrate-Serverbewertungstool hinzu.

[Weitere Informationen](how-to-add-tool-first-time.md)

## <a name="create-and-review-an-assessment"></a>Erstellen und Überprüfen einer Bewertung

1. Erstellen Sie Bewertungen für physische Server.
1. Überprüfen Sie die Bewertungen in Vorbereitung auf die Migrationsplanung.

[Erfahren Sie mehr](tutorial-assess-physical.md) über das Erstellen und Überprüfen von Bewertungen.
    

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel führen Sie folgende Schritte aus:
 
> [!div class="checklist"] 
> * Planen von Azure Migrate-Bewertungen für physische Server im gewünschten Umfang.
> * Vorbereiten von Azure und physischen Servern auf die Bewertung.
> * Erstellen eines Azure Migrate-Projekts und Ausführen von Bewertungen.
> * Überprüfen der Bewertungen in Vorbereitung auf die Migrationsplanung.

[Erfahren Sie nun](concepts-assessment-calculation.md), wie Bewertungen berechnet und [geändert](how-to-modify-assessment.md) werden.
