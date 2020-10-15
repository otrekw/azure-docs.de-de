---
title: Supportmatrix für Backup Center
description: Dieser Artikel enthält eine Zusammenfassung der Szenarien, die Backup Center für jeden Workloadtyp unterstützt.
ms.topic: conceptual
ms.date: 09/07/2020
ms.openlocfilehash: 8effc2514abf1cac55abc28b625b869810536baf
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "90993224"
---
# <a name="support-matrix-for-backup-center"></a>Supportmatrix für Backup Center

Backup Center bietet eine zentralisierte Benutzeroberfläche, mit der Unternehmen [Sicherungen in großem Maßstab steuern, überwachen, betreiben und analysieren](backup-center-overview.md) können. Dieser Artikel enthält eine Zusammenfassung der Szenarien, die Backup Center für jeden Workloadtyp unterstützt.

## <a name="supported-scenarios"></a>Unterstützte Szenarien

| **Kategorie** | **Szenario**  | **Unterstützte Workloads**  | **Einschränkungen** |
| -------------| ------------- | ----------------------- |------------|
| Überwachung   | Anzeigen aller Aufträge | <li> Virtueller Azure-Computer <br><br> <li> Azure Database for PostgreSQL-Server | <li> Aufträge im Umfang von 7 Tagen sind standardmäßig verfügbar. <br> <li> Jeder Filter bzw. jedes Dropdownelement unterstützt maximal 1000 Elemente. Das Backup Center kann also zum Überwachen von maximal 1000 Abonnements und 1000 Tresoren über Mandanten hinweg verwendet werden. |
| Überwachung | Anzeigen aller Sicherungsinstanzen | <li> Virtueller Azure-Computer <br><br> <li> Azure Database for PostgreSQL-Server | Wie oben |
| Überwachung | Anzeigen aller Sicherungsrichtlinien | <li> Virtueller Azure-Computer <br><br> <li> Azure Database for PostgreSQL-Server | Wie oben |
| Überwachung | Anzeigen aller Tresore | <li> Virtueller Azure-Computer <br><br> <li> Azure Database for PostgreSQL-Server | Wie oben |
| Aktionen | Konfigurieren der Sicherung | <li> Virtueller Azure-Computer <br><br> <li> Azure Database for PostgreSQL-Server | In den Supportmatrizen finden Sie Informationen zu [Sicherungen von virtuellen Azure-Computern](https://docs.microsoft.com/azure/backup/backup-support-matrix-iaas) und [Azure Database for PostgreSQL-Servern](backup-azure-database-postgresql.md#support-matrix). |
| Aktionen | Wiederherstellen von Sicherungsinstanzen | <li> Virtueller Azure-Computer <br><br> <li> Azure Database for PostgreSQL-Server | In den Supportmatrizen finden Sie Informationen zu [Sicherungen von virtuellen Azure-Computern](https://docs.microsoft.com/azure/backup/backup-support-matrix-iaas) und [Azure Database for PostgreSQL-Servern](backup-azure-database-postgresql.md#support-matrix). |
| Aktionen | Tresor erstellen | <li> Virtueller Azure-Computer <br><br> <li> Azure Database for PostgreSQL-Server | In den Supportmatrizen finden Sie Informationen zum [Recovery Services-Tresor](https://docs.microsoft.com/azure/backup/backup-support-matrix#vault-support). |
| Aktionen | Erstellen der Sicherungsrichtlinie | <li> Virtueller Azure-Computer <br><br> <li> Azure Database for PostgreSQL-Server | In den Supportmatrizen finden Sie Informationen zum [Recovery Services-Tresor](https://docs.microsoft.com/azure/backup/backup-support-matrix#vault-support). |
| Aktionen | Ausführen bedarfsgesteuerter Sicherung für eine Sicherungsinstanz | <li> Virtueller Azure-Computer <br><br> <li> Azure Database for PostgreSQL-Server | In den Supportmatrizen finden Sie Informationen zu [Sicherungen von virtuellen Azure-Computern](https://docs.microsoft.com/azure/backup/backup-support-matrix-iaas) und [Azure Database for PostgreSQL-Servern](backup-azure-database-postgresql.md#support-matrix). |
| Aktionen | Beenden der Sicherung für eine Sicherungsinstanz | <li> Virtueller Azure-Computer <br><br> <li> Azure Database for PostgreSQL-Server | In den Supportmatrizen finden Sie Informationen zu [Sicherungen von virtuellen Azure-Computern](https://docs.microsoft.com/azure/backup/backup-support-matrix-iaas) und [Azure Database for PostgreSQL-Servern](backup-azure-database-postgresql.md#support-matrix). |
| Einblicke | Anzeigen von Sicherungsberichten | <li> Virtueller Azure-Computer <br><br> <li> SQL in Azure Virtual Machine <br><br> <li> SAP HANA in Azure Virtual Machine <br><br> <li> Azure Files <br><br> <li> System Center Data Protection Manager <br><br> <li> Azure Backup-Agent (MARS) <br><br> <li> Azure Backup Server (MABS) | Weitere Informationen finden Sie in den [unterstützten Szenarien für Sicherungsberichte](https://docs.microsoft.com/azure/backup/configure-reports#supported-scenarios) |
| Governance | Anzeigen und Zuweisen integrierter und benutzerdefinierter Azure-Richtlinien unter der Kategorie „Sicherung“ | – | – |
| Governance | Anzeigen von Datenquellen, die nicht für die Sicherung konfiguriert sind | <li> Virtueller Azure-Computer <br><br> <li> Azure Database for PostgreSQL-Server | – |

## <a name="unsupported-scenarios"></a>Nicht unterstützte Szenarien

| **Kategorie** | **Szenario**  |
|--------------|---------------|
| Überwachung | Anzeigen von Warnungen im großen Stil |
| Aktionen | Konfigurieren von Tresoreinstellungen im großen Stil |
| Aktionen | Ausführen eines regionsübergreifenden Wiederherstellungsauftrags von Backup Center aus |

## <a name="next-steps"></a>Nächste Schritte

* Sehen Sie sich die [Supportmatrix für Azure Backup](https://docs.microsoft.com/azure/backup/backup-support-matrix) an.
* Sehen Sie sich die [Unterstützungsmatrix für die Sicherung virtueller Azure-Computer](https://docs.microsoft.com/azure/backup/backup-support-matrix-iaas) an.
* Sehen Sie sich die [Supportmatrix für die Sicherung des Azure Database for PostgreSQL-Servers](backup-azure-database-postgresql.md#support-matrix) an.
