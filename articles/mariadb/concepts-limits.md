---
title: Einschränkungen – Azure Database for MariaDB
description: Dieser Artikel beschreibt die Einschränkungen in Azure Database for MariaDB, z.B. die Anzahl von Verbindungs- und Speicher-Engine-Optionen.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 6/25/2020
ms.openlocfilehash: fc5557c1b20d87d2f96559e1d41efa4576045f09
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85392776"
---
# <a name="limitations-in-azure-database-for-mariadb"></a>Einschränkungen in Azure Database for MariaDB
In den folgenden Abschnitten werden die Kapazitäts- und funktionalen Beschränkungen sowie Beschränkungen bei der Unterstützung der Speicher-Engine und von Datenmanipulationsanweisungen im Datenbankdienst beschrieben.

## <a name="server-parameters"></a>Serverparameter

> [!NOTE]
> Wenn Sie nach Minimal-/Maximalwerten für Serverparameter wie `max_connections` und `innodb_buffer_pool_size` suchen, finden Sie diese Informationen nun im Artikel zu **[Serverparametern](./concepts-server-parameters.md)** .

Azure Database for MariaDB unterstützt das Anpassen einiger Serverparameter. Die Minimal- und Maximalwerte einiger Parameter (z. B. `max_connections`, `join_buffer_size`, `query_cache_size`) werden durch den Tarif und die virtuellen Kerne des Servers bestimmt. Im Artikel [Serverparameter](./concepts-server-parameters.md) finden Sie weitere Informationen zu diesen Grenzwerten.

Bei der ersten Bereitstellung enthält ein Server für Azure for MariaDB Systemtabellen für Zeitzoneninformationen, die jedoch nicht aufgefüllt sind. Die Zeitzonentabellen können durch Aufrufen der gespeicherten Prozedur `mysql.az_load_timezone` über ein Tool wie die MySQL-Befehlszeile oder MySQL Workbench aufgefüllt werden. Informationen zum Aufrufen der gespeicherten Prozedur und zum Festlegen der globalen Zeitzonen oder Zeitzonen auf Sitzungsebene finden Sie in den Artikeln für das [Azure-Portal](howto-server-parameters.md#working-with-the-time-zone-parameter) und die [Azure CLI](howto-configure-server-parameters-cli.md#working-with-the-time-zone-parameter).

## <a name="storage-engine-support"></a>Speicher-Engine-Unterstützung

### <a name="supported"></a>Unterstützt
- [InnoDB](https://mariadb.com/kb/en/library/xtradb-and-innodb/)
- [MEMORY](https://mariadb.com/kb/en/library/memory-storage-engine/)

### <a name="unsupported"></a>Nicht unterstützt
- [MyISAM](https://mariadb.com/kb/en/library/myisam-storage-engine/)
- [BLACKHOLE](https://mariadb.com/kb/en/library/blackhole/)
- [ARCHIVE](https://mariadb.com/kb/en/library/archive/)

## <a name="privilege-support"></a>Berechtigungsunterstützung

### <a name="unsupported"></a>Nicht unterstützt
- DBA-Rolle: Viele Serverparameter und -einstellungen können unbeabsichtigterweise die Serverleistung beeinträchtigen oder ACID-Eigenschaften des DBMS verschlechtern. Um die Dienstintegrität und die SLA auf Produktebene aufrechtzuerhalten, stellt dieser Dienst die DBA-Rolle nicht zur Verfügung. Das Standardbenutzerkonto, das zusammen mit einer neuen Datenbankinstanz erstellt wird, gestattet dem Benutzer die Ausführung der meisten DDL- und DML-Anweisungen in der verwalteten Datenbankinstanz.
- SUPER-Berechtigung: Auf ähnliche Weise ist die [SUPER-Berechtigung](https://mariadb.com/kb/en/library/grant/#global-privileges) ebenfalls eingeschränkt.
- DEFINER: Erfordert erhöhte Berechtigungen zum Erstellen und ist beschränkt. Entfernen Sie beim Importieren von Daten mithilfe einer Sicherung die `CREATE DEFINER`-Befehle manuell oder mithilfe des Befehls `--skip-definer`, wenn Sie einen mysqldump ausführen.

## <a name="data-manipulation-statement-support"></a>Unterstützung von Datenmanipulationsanweisungen

### <a name="supported"></a>Unterstützt
- `LOAD DATA INFILE` wird unterstützt, jedoch muss der Parameter `[LOCAL]` angegeben und an einen UNC-Pfad (über das SMB-Protokoll eingebundene Azure Storage-Instanz) weitergeleitet werden.

### <a name="unsupported"></a>Nicht unterstützt
- `SELECT ... INTO OUTFILE`

## <a name="functional-limitations"></a>Funktionale Beschränkungen

### <a name="scale-operations"></a>Skalierungsvorgänge
- Die dynamische Skalierung auf den oder vom Basic-Tarif aus wird zurzeit nicht unterstützt.
- Die Verringerung der Größe des Serverspeichers wird nicht unterstützt.

### <a name="server-version-upgrades"></a>Upgrades von Serverversionen
- Die automatisierte Migration zwischen Hauptversionen von Datenbank-Engines wird derzeit nicht unterstützt.

### <a name="point-in-time-restore"></a>Point-in-Time-Wiederherstellung
- Wenn Sie das PITR-Feature verwenden, wird der neue Server mit den gleichen Konfigurationen erstellt wie der Server, auf dem er basiert.
- Die Wiederherstellung eines gelöschten Servers wird nicht unterstützt.

### <a name="subscription-management"></a>Abonnementverwaltung
- Die dynamische Verschiebung von vorab erstellten Servern zwischen Abonnement- und Ressourcengruppen wird derzeit nicht unterstützt.

### <a name="vnet-service-endpoints"></a>VNET-Dienstendpunkte
- VNET-Dienstendpunkte werden nur für Server vom Typ „Universell“ und „Arbeitsspeicheroptimiert“ unterstützt.

### <a name="storage-size"></a>Speichergröße
- Informationen zu den Beschränkungen der Speichergröße pro Tarif finden Sie unter [Tarife](concepts-pricing-tiers.md).

## <a name="current-known-issues"></a>Aktuelle bekannte Probleme
- Die MariaDB-Serverinstanz zeigt die falsche Serverversion an, nachdem die Verbindung hergestellt wurde. Um die richtige Engine-Version der Serverinstanzen abzurufen, verwenden Sie den Befehl `select version();`.

## <a name="next-steps"></a>Nächste Schritte
- [Verfügbaren Funktionen auf den einzelnen Dienstebenen](concepts-pricing-tiers.md)
- [Unterstützte MariaDB-Datenbankversionen](concepts-supported-versions.md)
