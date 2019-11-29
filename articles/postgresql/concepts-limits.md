---
title: Beschränkungen in Azure Database for PostgreSQL – Einzelserver
description: In diesem Artikel werden die Beschränkungen in Azure Database for PostgreSQL für Einzelserver beschrieben, z. B. die Anzahl der Verbindungen und Optionen für die Speicher-Engine.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 06/25/2019
ms.custom: fasttrack-edit
ms.openlocfilehash: b9cef4753b6fd324b38d7254139fe288463a0c0c
ms.sourcegitcommit: 5a8c65d7420daee9667660d560be9d77fa93e9c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2019
ms.locfileid: "74123896"
---
# <a name="limits-in-azure-database-for-postgresql---single-server"></a>Beschränkungen in Azure Database for PostgreSQL – Einzelserver
In den folgenden Abschnitten werden die Kapazitäts- und funktionalen Beschränkungen im Datenbankdienst beschrieben. Informationen zu den Tarifen für Ressourcen (Compute, Arbeitsspeicher, Speicher) finden Sie im Artikel [Tarife](concepts-pricing-tiers.md).


## <a name="maximum-connections"></a>Maximale Anzahl der Verbindungen
Die folgende Tabelle enthält die maximale Anzahl von Verbindungen nach Tarif und V-Kernen: 

|**Tarif**| **vCore(s)**| **Max. Anzahl von Verbindungen** | **Max. Benutzerverbindungen** |
|---|---|---|---|
|Basic| 1| 55 | 50|
|Basic| 2| 105 | 100|
|Allgemeiner Zweck| 2| 150| 145|
|Allgemeiner Zweck| 4| 250| 245|
|Allgemeiner Zweck| 8| 480| 475|
|Allgemeiner Zweck| 16| 950| 945|
|Allgemeiner Zweck| 32| 1500| 1495|
|Allgemeiner Zweck| 64| 1\.900| 1895|
|Arbeitsspeicheroptimiert| 2| 300| 295|
|Arbeitsspeicheroptimiert| 4| 500| 495|
|Arbeitsspeicheroptimiert| 8| 960| 955|
|Arbeitsspeicheroptimiert| 16| 1\.900| 1895|
|Arbeitsspeicheroptimiert| 32| 1987| 1982|

Wenn Verbindungen den Grenzwert übersteigen, erhalten Sie möglicherweise den folgenden Fehler:
> SCHWERWIEGEND: Es sind bereits zu viele Clients vorhanden.

Das Azure-System benötigt fünf Verbindungen, um den Azure Database for PostgreSQL-Server zu überwachen. 

## <a name="functional-limitations"></a>Funktionale Beschränkungen
### <a name="scale-operations"></a>Skalierungsvorgänge
- Die dynamische Skalierung auf den oder vom Basic-Tarif aus wird zurzeit nicht unterstützt.
- Die Verringerung der Größe des Serverspeichers wird zurzeit nicht unterstützt.

### <a name="server-version-upgrades"></a>Upgrades von Serverversionen
- Die automatisierte Migration zwischen Hauptversionen von Datenbank-Engines wird derzeit nicht unterstützt. Wenn Sie auf die nächste Hauptversion upgraden möchten, führen Sie eine [Sicherung und Wiederherstellung](./howto-migrate-using-dump-and-restore.md) auf einem Server aus, der mit der neuen Engine-Version erstellt wurde.

> Beachten Sie, dass vor PostgreSQL-Version 10 ein _Hauptversionsupgrade_ von der [PostgreSQL-Versionsrichtlinie](https://www.postgresql.org/support/versioning/) als eine Erhöhung der ersten _oder_ zweiten Versionsnummer betrachtet wurde (z. B. wurde der Wechsel von 9.5 auf 9.6 als _Hauptversionsupgrade_ angesehen).
> Ab Version 10 gilt nur noch eine Änderung der ersten Versionsnummer als Hauptversionsupgrade (z. B. ist der Wechsel von 10.0 auf 10.1 ein _Nebenversionsupgrade_ und der von 10 auf 11 ein _Hauptversionsupgrade_).

### <a name="vnet-service-endpoints"></a>VNET-Dienstendpunkte
- VNET-Dienstendpunkte werden nur für Server vom Typ „Universell“ und „Arbeitsspeicheroptimiert“ unterstützt.

### <a name="restoring-a-server"></a>Wiederherstellen eines Servers
- Wenn Sie das PITR-Feature verwenden, wird der neue Server mit den gleichen Tarifkonfigurationen erstellt wie der Server, auf dem er basiert.
- Der neue Server, der während einer Wiederherstellung erstellt wird, weist nicht die Firewallregeln auf, die auf dem ursprünglichen Server vorhanden waren. Firewallregeln müssen separat für diesen neuen Server eingerichtet werden.
- Die Wiederherstellung eines gelöschten Servers wird nicht unterstützt.

### <a name="utf-8-characters-on-windows"></a>UTF-8-Zeichen in Windows
- In einigen Szenarien werden UTF-8-Zeichen nicht vollständig in Open Source PostgreSQL unter Windows unterstützt, was Azure Database for PostgreSQL beeinträchtigt. Weitere Informationen finden Sie in dem Thread zu [Fehler #15476 im PostgreSQL-Archiv](https://www.postgresql-archive.org/BUG-15476-Problem-on-show-trgm-with-4-byte-UTF-8-characters-td6056677.html).

## <a name="next-steps"></a>Nächste Schritte
- Informationen zu den [verfügbaren Funktionen in jedem Tarif](concepts-pricing-tiers.md)
- Informationen zu [unterstützten PostgreSQL-Datenbankversionen](concepts-supported-versions.md)
- Informationen zum [Sichern und Wiederherstellen eines Servers in Azure Database for PostgreSQL mithilfe des Azure-Portals](howto-restore-server-portal.md)
