---
title: Gängige Fragen zu Azure Migrate
description: Hier erhalten Sie Antworten auf häufig gestellte Fragen zum Azure Migrate-Dienst.
author: snehaamicrosoft
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: snehaa
ms.openlocfilehash: baf01c0a0d5c6154305f7137c24deb0365b5e812
ms.sourcegitcommit: db2d402883035150f4f89d94ef79219b1604c5ba
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2020
ms.locfileid: "77062097"
---
# <a name="azure-migrate-common-questions"></a>Von der Azure Häufig gestellte Fragen

In diesem Artikel werden Fragen zu Azure Migrate beantwortet. Sollten Sie nach der Lektüre dieses Artikels noch Fragen haben, stellen Sie diese bitte im [Azure Migrate-Forum](https://aka.ms/AzureMigrateForum). Wenn Sie weitere Fragen haben, lesen Sie die folgenden Artikel:

- [Fragen](common-questions-appliance.md) zur Azure Migrate-Appliance.
- [Fragen](common-questions-discovery-assessment.md) zur Ermittlung, Bewertung und Abhängigkeitsvisualisierung.


## <a name="what-is-azure-migrate"></a>Was ist Azure Migrate?

Azure Migrate ist ein zentraler Hub zum Nachverfolgen der Ermittlung, Bewertung und Migration Ihrer lokalen Apps und Workloads sowie von VMs in der privaten/öffentlichen Cloud zu Azure. Der Hub stellt Azure Migrate-Tools für die Bewertung und Migration sowie Angebote von unabhängigen Drittanbietern (Independent Software Vendors, ISVs) bereit. [Weitere Informationen](migrate-services-overview.md)


## <a name="what-can-i-do-with-azure-migrate"></a>Wofür kann ich Azure Migrate verwenden?

Verwenden Sie Azure Migrate, um lokale Infrastrukturen, Anwendungen und Daten in Azure zu ermitteln, zu bewerten und zu Azure zu migrieren. Azure Migrate unterstützt die Bewertung und Migration von lokalen VMware-VMs, Hyper-V-VMs, physischen Servern, anderen virtualisierten VMs, Datenbanken, Web-Apps und virtuellen Desktops. 

## <a name="whats-the-difference-between-azure-migrate-and-site-recovery"></a>Was ist der Unterschied zwischen Azure Migrate und Site Recovery?

Azure Migrate stellt einen zentralisierten Hub für die Bewertung und Migration zu Azure bereit. [Azure Site Recovery](../site-recovery/site-recovery-overview.md) ist eine Lösung für die Notfallwiederherstellung. Azure Migrate: Das Server Migrationstool nutzt gewisse Site Recovery-Back-End-Funktionen für die Lift & Shift-Migration von einigen lokalen Computern.

## <a name="whats-the-difference-between-azure-migrate-server-assessment-and-the-map-toolkit"></a>Was ist der Unterschied zwischen der Azure Migrate-Serverbewertungen und dem MAP Toolkit?

Die Serverbewertung ermöglicht eine Bewertung zum Feststellen der Migrationsbereitschaft und eine Auswertung der Workloads für die Migration nach Azure. Das [Microsoft Assessment and Planning (MAP) Toolkit](https://www.microsoft.com/download/details.aspx?id=7826) unterstützt Sie bei anderen Aufgaben, beispielsweise der Migrationsplanung für neuere Versionen von Windows-Client-/Serverbetriebssystemen sowie die Nachverfolgung der Softwarenutzung. Verwenden Sie für diese Szenarien weiterhin das MAP Toolkit.

## <a name="whats-the-difference-between-server-assessment-and-the-site-recovery-deployment-planner"></a>Was ist der Unterschied zwischen der Serverbewertung und dem Site Recovery-Bereitstellungsplaner?

Die Serverbewertung ist ein Tool zur Migrationsplanung. Der Site Recovery-Bereitstellungsplaner ist ein Tool zur Notfallwiederherstellung.

- **Planen der lokalen Migration zu Azure**: Wenn Sie planen, Ihre lokalen Server zu Azure zu migrieren, verwenden Sie für die Migrationsplanung die Serverbewertung. Sie bewertet lokale Workloads und stellt Anleitungen und Tools zur Verfügung, die Sie bei der Migration unterstützen. Nachdem der Migrationsplan steht, können Sie Tools wie die Azure Migrate-Servermigration verwenden, um die Computer zu Azure zu migrieren.
- **Planen der Notfallwiederherstellung in Azure**: Wenn Sie planen, mit Site Recovery eine Notfallwiederherstellung lokaler Computer in Azure einzurichten, verwenden Sie die Site Recovery-Bereitstellungsplaner. Der Bereitstellungsplaner führt eine umfassende, Site Recovery-spezifische Bewertung Ihrer lokalen Umgebung für die Notfallwiederherstellung durch. Er stellt Empfehlungen für die Notfallwiederherstellung zur Verfügung, z. B. Replikation und Failover.

## <a name="how-does-server-migration-work-with-site-recovery"></a>Wie funktioniert die Servermigration mit Site Recovery?

- Wenn Sie Azure Migrate verwenden: Servermigration zum Durchführen einer Migration ohne Agent für lokale VMware-VMs: Die Migration ist dann nativ für Azure Migrate, und Site Recovery wird nicht verwendet.
- Wenn Sie Azure Migrate verwenden: Servermigration zum Durchführen einer Agent-basierten Migration von VMware-VMs oder der Migration von Hyper-V-VMs oder physischen Servern: Die Azure Migrate-Servermigration nutzt die Azure Site Recovery-Replikations-Engine.


## <a name="which-geographies-are-supported"></a>Welche geografischen Regionen werden unterstützt?

Überprüfen Sie die von Azure Migrate unterstützten geografischen Regionen für [VMware-VMs](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware) und [Hyper-V-VMs](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-hyper-v).

## <a name="how-do-i-get-started"></a>Wie fange ich an?

Sie identifizieren das Tool, das Sie benötigen, und fügen es einem Azure Migrate-Projekt hinzu. Falls Sie ein ISV-Tool oder Movere hinzufügen, gehen Sie wie folgt vor:
- Beziehen Sie zunächst eine Lizenz, oder registrieren Sie sich für eine kostenlose Testversion (gemäß der Toolrichtlinie). Die Lizenzierung für ISV-Tools erfolgt gemäß dem Lizenzierungsmodell des ISV oder Tools.
- In jedem Tool ist eine Option zum Herstellen einer Verbindung mit Azure Migrate vorhanden. Befolgen Sie die Anweisungen des Tools und seine Dokumentation, um das Tool mit Azure Migrate zu verbinden.
Sie verfolgen Ihre Migration innerhalb des Azure Migrate-Projekts zentral über Azure und andere Tools hinweg nach.

## <a name="how-do-i-delete-a-project"></a>Wie lösche ich ein Projekt?

[Informationen](how-to-delete-project.md) zum Löschen eines ein Projekts. 




## <a name="next-steps"></a>Nächste Schritte
Sehen Sie sich die [Übersicht zu Azure Migrate](migrate-services-overview.md) an.
