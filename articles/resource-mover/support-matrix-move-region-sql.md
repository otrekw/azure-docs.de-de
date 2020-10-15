---
title: Unterstützung beim Verschieben von Azure SQL-Ressourcen zwischen Regionen mit Azure Resource Mover
description: Informieren Sie sich über die Unterstützung beim Verschieben von Azure SQL-Ressourcen zwischen Regionen mit Azure Resource Mover.
author: rayne-wiselman
manager: evansma
ms.service: resource-move
ms.topic: conceptual
ms.date: 09/07/2020
ms.author: raynew
ms.openlocfilehash: 4925f6ffd2383c21f8ff9b0e3196d44fc15bb657
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "89652620"
---
# <a name="support-for-moving-azure-sql-resources-between-azure-regions"></a>Unterstützung beim Verschieben von Azure SQL-Ressourcen zwischen Azure-Regionen

Dieser Artikel enthält eine Zusammenfassung der Unterstützung und Voraussetzungen im Zusammenhang mit dem Verschieben von Azure SQL-Ressourcen zwischen Azure-Regionen mit Azure Resource Mover.

## <a name="requirements"></a>Anforderungen

Die Anforderungen werden in der folgenden Tabelle zusammengefasst.

**Feature** | **Unterstützt/Nicht unterstützt.** | **Details**
--- | --- | ---
**Hyperscale in Azure SQL-Datenbank** | Nicht unterstützt | Mit Resource Mover können keine Datenbanken der Azure SQL Hyperscale-Dienstebene verschoben werden.
**Zonenredundanz** | Unterstützt |  Unterstützte Optionen für den Verschiebungsvorgang:<br/><br/> - Zwischen Regionen, die Zonenredundanz unterstützen<br/><br/> - Zwischen Regionen, die keine Zonenredundanz unterstützen<br/><br/> - Zwischen einer Region, die Zonenredundanz unterstützt, und einer Region, die keine Zonenredundanz unterstützt<br/><br/> - Zwischen einer Region, die keine Zonenredundanz unterstützt, und einer Region, die Zonenredundanz unterstützt 
**Datensynchronisierung** | Hub-/Synchronisierungsdatenbank: Nicht unterstützt<br/><br/> Synchronisierungsmitglied: Unterstützt. | Wird ein Synchronisierungsmitglied verschoben, müssen Sie die Datensynchronisierung mit der neuen Zieldatenbank einrichten.
**Vorhandene Georeplikation** | Unterstützt | Vorhandene Georeplikate werden der neuen primären Datenbank in der Zielregion zugeordnet.<br/><br/> Nach dem Verschieben muss das Seeding initialisiert werden. [Weitere Informationen](/azure/sql-database/sql-database-active-geo-replication-portal)
**Transparent Data Encryption (TDE) mit Bring Your Own Key (BYOK)** | Unterstützt | Weitere Informationen zum Verschieben von Schlüsseltresoren zwischen Regionen finden Sie [hier](../key-vault/general/move-region.md).
**TDE mit dienstseitig verwaltetem Schlüssel** | Unterstützt. |  Weitere Informationen zum Verschieben von Schlüsseltresoren zwischen Regionen finden Sie [hier](../key-vault/general/move-region.md).
**Regeln für die dynamische Datenmaskierung** | Unterstützt. | Regeln werden im Rahmen der Verschiebung automatisch in die Zielregion kopiert. [Weitere Informationen](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started-portal)
**Erweiterte Datensicherheit** | Wird nicht unterstützt. | Problemumgehung: Führen Sie die Einrichtung auf der SQL Server-Ebene in der Zielregion durch. [Weitere Informationen](https://docs.microsoft.com/azure/sql-database/sql-database-advanced-data-security)
**Firewallregeln** | Wird nicht unterstützt. | Problemumgehung: Richten Sie Firewallregeln für SQL Server in der Zielregion ein. Firewallregeln auf der Datenbankebene werden vom Quellserver auf den Zielserver kopiert. [Weitere Informationen](https://docs.microsoft.com/azure/sql-database/sql-database-server-level-firewall-rule)
**Überwachungsrichtlinien** | Wird nicht unterstützt. | Richtlinien werden nach dem Verschieben auf die Standardeinstellung zurückgesetzt. Informationen zum Zurücksetzen finden Sie [hier](https://docs.microsoft.com/azure/sql-database/sql-database-auditing).
**die Aufbewahrung der Sicherung** | Unterstützt. | Sicherungsaufbewahrungsrichtlinien für die Quelldatenbank werden in die Zieldatenbank übertragen. Informationen zum Ändern von Einstellungen nach dem Verschieben finden Sie [hier](/azure/sql-database/sql-database-long-term-backup-retention-configure).
**Automatische Optimierung** | Wird nicht unterstützt. | Problemumgehung: Legen Sie nach dem Verschieben Einstellungen für die automatische Optimierung fest. [Weitere Informationen](https://docs.microsoft.com/azure/sql-database/sql-database-automatic-tuning-enable)
**Datenbankwarnungen** | Wird nicht unterstützt. | Problemumgehung: Richten Sie nach dem Verschieben Warnungen ein. [Weitere Informationen](https://docs.microsoft.com/azure/sql-database/sql-database-insights-alerts-portal)
**Azure SQL Server Stretch Database** | Nicht unterstützt | SQL Server Stretch Database-Instanzen können nicht mit Resource Mover verschoben werden.
**Azure Synapse Analytics** | Nicht unterstützt | Synapse Analytics (ehemals Azure SQL Data Warehouse) kann nicht mit Resource Mover verschoben werden.
## <a name="next-steps"></a>Nächste Schritte

Versuchen Sie, [Azure SQL-Ressourcen](tutorial-move-region-sql.md) mit Resource Mover in eine andere Region zu verschieben.