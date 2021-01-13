---
title: Supportmatrix für Backup Center
description: Dieser Artikel enthält eine Zusammenfassung der Szenarien, die Backup Center für jeden Workloadtyp unterstützt.
ms.topic: conceptual
ms.date: 09/07/2020
ms.openlocfilehash: 40582ddc1187082230e2887edcd24b3716811be2
ms.sourcegitcommit: 2989396c328c70832dcadc8f435270522c113229
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2020
ms.locfileid: "92173991"
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
| Aktionen | Konfigurieren der Sicherung | <li> Virtueller Azure-Computer <br><br> <li> Azure Database for PostgreSQL-Server | In den Supportmatrizen finden Sie Informationen zu [Sicherungen von virtuellen Azure-Computern](./backup-support-matrix-iaas.md) und [Azure Database for PostgreSQL-Servern](backup-azure-database-postgresql.md#support-matrix). |
| Aktionen | Wiederherstellen von Sicherungsinstanzen | <li> Virtueller Azure-Computer <br><br> <li> Azure Database for PostgreSQL-Server | In den Supportmatrizen finden Sie Informationen zu [Sicherungen von virtuellen Azure-Computern](./backup-support-matrix-iaas.md) und [Azure Database for PostgreSQL-Servern](backup-azure-database-postgresql.md#support-matrix). |
| Aktionen | Tresor erstellen | <li> Virtueller Azure-Computer <br><br> <li> Azure Database for PostgreSQL-Server | In den Supportmatrizen finden Sie Informationen zum [Recovery Services-Tresor](./backup-support-matrix.md#vault-support). |
| Aktionen | Erstellen der Sicherungsrichtlinie | <li> Virtueller Azure-Computer <br><br> <li> Azure Database for PostgreSQL-Server | In den Supportmatrizen finden Sie Informationen zum [Recovery Services-Tresor](./backup-support-matrix.md#vault-support). |
| Aktionen | Ausführen bedarfsgesteuerter Sicherung für eine Sicherungsinstanz | <li> Virtueller Azure-Computer <br><br> <li> Azure Database for PostgreSQL-Server | In den Supportmatrizen finden Sie Informationen zu [Sicherungen von virtuellen Azure-Computern](./backup-support-matrix-iaas.md) und [Azure Database for PostgreSQL-Servern](backup-azure-database-postgresql.md#support-matrix). |
| Aktionen | Beenden der Sicherung für eine Sicherungsinstanz | <li> Virtueller Azure-Computer <br><br> <li> Azure Database for PostgreSQL-Server | In den Supportmatrizen finden Sie Informationen zu [Sicherungen von virtuellen Azure-Computern](./backup-support-matrix-iaas.md) und [Azure Database for PostgreSQL-Servern](backup-azure-database-postgresql.md#support-matrix). |
| Einblicke | Anzeigen von Sicherungsberichten | <li> Virtueller Azure-Computer <br><br> <li> SQL in Azure Virtual Machine <br><br> <li> SAP HANA in Azure Virtual Machine <br><br> <li> Azure Files <br><br> <li> System Center Data Protection Manager <br><br> <li> Azure Backup-Agent (MARS) <br><br> <li> Azure Backup Server (MABS) | Weitere Informationen finden Sie in den [unterstützten Szenarien für Sicherungsberichte](./configure-reports.md#supported-scenarios) |
| Governance | Anzeigen und Zuweisen integrierter und benutzerdefinierter Azure-Richtlinien unter der Kategorie „Sicherung“ | – | – |
| Governance | Anzeigen von Datenquellen, die nicht für die Sicherung konfiguriert sind | <li> Virtueller Azure-Computer <br><br> <li> Azure Database for PostgreSQL-Server | – |

## <a name="unsupported-scenarios"></a>Nicht unterstützte Szenarien

| **Kategorie** | **Szenario**  |
|--------------|---------------|
| Überwachung | Anzeigen von Warnungen im großen Stil |
| Aktionen | Konfigurieren von Tresoreinstellungen im großen Stil |
| Aktionen | Ausführen eines regionsübergreifenden Wiederherstellungsauftrags von Backup Center aus |

## <a name="next-steps"></a>Nächste Schritte

* Sehen Sie sich die [Supportmatrix für Azure Backup](./backup-support-matrix.md) an.
* Sehen Sie sich die [Unterstützungsmatrix für die Sicherung virtueller Azure-Computer](./backup-support-matrix-iaas.md) an.
* Sehen Sie sich die [Supportmatrix für die Sicherung des Azure Database for PostgreSQL-Servers](backup-azure-database-postgresql.md#support-matrix) an.