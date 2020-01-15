---
title: 'V-Kern-Ressourcenlimits: Einzeldatenbank'
description: Auf dieser Seite werden einige allgemeine Ressourcenlimits für virtuelle Kerne für Singletons in Azure SQL-Datenbank beschrieben.
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 12/11/2019
ms.openlocfilehash: 4455181ddf69613ba07bcaeedb26273a4bb5a74d
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/03/2020
ms.locfileid: "75647848"
---
# <a name="resource-limits-for-single-databases-using-the-vcore-purchasing-model"></a>Ressourcenlimits für Singletons mit dem auf virtuellen Kernen (V-Kernen) basierenden Kaufmodell

Dieser Artikel enthält ausführliche Angaben zu Ressourcenlimits für Singletons in Azure SQL-Datenbank, die das V-Kern-basierte Kaufmodell verwenden.

Weitere Informationen zu Ressourcenlimits für Singletons auf einem SQL-Datenbank-Server mit dem DTU-basierten Kaufmodell finden Sie unter [Übersicht über Ressourcenlimits auf einem SQL-Datenbank-Server](sql-database-resource-limits-database-server.md).

Sie können mit dem [Azure-Portal](sql-database-single-databases-manage.md#manage-an-existing-sql-database-server), [Transact-SQL](sql-database-single-databases-manage.md#transact-sql-manage-sql-database-servers-and-single-databases), [PowerShell](sql-database-single-databases-manage.md#powershell-manage-sql-database-servers-and-single-databases), der [Azure CLI](sql-database-single-databases-manage.md#azure-cli-manage-sql-database-servers-and-single-databases) oder der [REST-API](sql-database-single-databases-manage.md#rest-api-manage-sql-database-servers-and-single-databases) Dienstebene, Computegröße und Speichermenge für eine einzelne Datenbank festlegen.

> [!IMPORTANT]
> Anleitungen und Überlegungen zur Skalierung finden Sie unter [Skalieren eines Singletons](sql-database-single-database-scale.md).

## <a name="general-purpose---serverless-compute---gen5"></a>Universell – serverloses Computing – Gen5

Der [serverlose Computetarif](sql-database-serverless.md) ist derzeit nur auf Gen5-Hardware verfügbar.

### <a name="gen5-compute-generation-part-1"></a>Computegeneration Gen5 (Teil 1)

|Computegröße|GP_S_Gen5_1|GP_S_Gen5_2|GP_S_Gen5_4|GP_S_Gen5_6|GP_S_Gen5_8|
|:--- | --: |--: |--: |--: |--: |
|Computegeneration|Gen5|Gen5|Gen5|Gen5|Gen5|
|Min-Max V-Kerne|0,5 - 1|0,5 - 2|0,5 - 4|0,75 - 6|1,0 - 8|
|Min-Max-Arbeitsspeicher (GB)|2,02 - 3|2,05 - 6|2,10 - 12|2,25 - 18|3,00 - 24|
|Min. Verzögerung für automatische Pause (Minuten)|60|60|60|60|60|
|Columnstore-Unterstützung|Ja|Ja|Ja|Ja|Ja|
|In-Memory-OLTP-Speicher (GB)|–|–|–|–|–|
|Maximale Datengröße (GB)|512|1024|1024|1024|1536|
|Maximale Protokollgröße (GB)|154|307|307|307|461|
|Max. Datengröße von TempDB (GB)|32|64|128|192|256|
|Speichertyp|Remote-SSD|Remote-SSD|Remote-SSD|Remote-SSD|Remote-SSD|
|E/A-Wartezeit (ungefähr)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|
|Max. Datenrate, IOPS*|320|640|1280|1920|2560|
|Max. Protokollrate (MBit/s)|3.8|7,5|15|22,5|30|
|Max. gleichzeitige Worker (Anforderungen)|75|150|300|450|600|
|Max. gleichzeitige Sitzungen|30.000|30.000|30.000|30.000|30.000|
|Anzahl von Replikaten|1|1|1|1|1|
|Multi-AZ|–|–|–|–|–|
|Horizontale Leseskalierung|–|–|–|–|–|
|Enthaltener Sicherungsspeicher|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|

\* Der maximale Wert für E/A-Größen im Bereich von 8 KB bis 64 KB. Die tatsächlichen IOPS sind von der Arbeitsauslastung abhängig. Weitere Informationen finden Sie unter [Daten-E/A-Governance](sql-database-resource-limits-database-server.md#resource-governance).

### <a name="gen5-compute-generation-part-2"></a>Computegeneration Gen5 (Teil 2)

|Computegröße|GP_S_Gen5_10|GP_S_Gen5_12|GP_S_Gen5_14|GP_S_Gen5_16|
|:--- | --: |--: |--: |--: |
|Computegeneration|Gen5|Gen5|Gen5|Gen5|
|Min-Max V-Kerne|1,25 - 10|1,50 - 12|1,75–14|2,00 - 16|
|Min-Max-Arbeitsspeicher (GB)|3,75 - 30|4,50 - 36|5,25 - 42|6,00 - 48|
|Min. Verzögerung für automatische Pause (Minuten)|60|60|60|60|
|Columnstore-Unterstützung|Ja|Ja|Ja|Ja|
|In-Memory-OLTP-Speicher (GB)|–|–|–|–|
|Maximale Datengröße (GB)|1536|3072|3072|3072|
|Maximale Protokollgröße (GB)|461|461|461|922|
|Max. Datengröße von TempDB (GB)|320|384|448|512|
|Speichertyp|Remote-SSD|Remote-SSD|Remote-SSD|Remote-SSD|
|E/A-Wartezeit (ungefähr)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|
|Max. Datenrate, IOPS*|3200|3840|4480|5120|
|Max. Protokollrate (MBit/s)|30|30|30|30|
|Max. gleichzeitige Worker (Anforderungen)|750|900|1050|1200|
|Max. gleichzeitige Sitzungen|30.000|30.000|30.000|30.000|
|Anzahl von Replikaten|1|1|1|1|
|Multi-AZ|–|–|–|–|
|Horizontale Leseskalierung|–|–|–|–|
|Enthaltener Sicherungsspeicher|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|

\* Der maximale Wert für E/A-Größen im Bereich von 8 KB bis 64 KB. Die tatsächlichen IOPS sind von der Arbeitsauslastung abhängig. Weitere Informationen finden Sie unter [Daten-E/A-Governance](sql-database-resource-limits-database-server.md#resource-governance).

## <a name="hyperscale---provisioned-compute---gen4"></a>Hyperscale – bereitgestelltes Computing – Gen4

### <a name="gen4-compute-generation-part-1"></a>Computegeneration Gen4 (Teil 1)

|Leistungsstufe|HS_Gen4_1|HS_Gen4_2|HS_Gen4_3|HS_Gen4_4|HS_Gen4_5|HS_Gen4_6|
|:--- | --: |--: |--: |---: | --: |--: |
|Computegeneration|Gen4|Gen4|Gen4|Gen4|Gen4|Gen4|
|V-Kerne|1|2|3|4|5|6|
|Arbeitsspeicher (GB)|7|14|21|28|35|42|
|[RBPEX](sql-database-service-tier-hyperscale.md#compute)-Größe|3 × Arbeitsspeicher|3 × Arbeitsspeicher|3 × Arbeitsspeicher|3 × Arbeitsspeicher|3 × Arbeitsspeicher|3 × Arbeitsspeicher|
|Columnstore-Unterstützung|Ja|Ja|Ja|Ja|Ja|Ja|
|In-Memory-OLTP-Speicher (GB)|–|–|–|–|–|–|
|Maximale Datengröße (TB)|100 |100 |100 |100 |100 |100|
|Maximale Protokollgröße (TB)|1 |1 |1 |1 |1 |1 |
|Max. Datengröße von TempDB (GB)|32|64|96|128|160|192|
|Speichertyp| [Hinweis 1](#notes) |[Hinweis 1](#notes)|[Hinweis 1](#notes) |[Hinweis 1](#notes) |[Hinweis 1](#notes) |[Hinweis 1](#notes) |
|Max. Datenrate, IOPS*|[Hinweis 2](#notes)|[Hinweis 2](#notes)|[Hinweis 2](#notes)|[Hinweis 2](#notes)|[Hinweis 2](#notes)|[Hinweis 2](#notes)|
|E/A-Wartezeit (ungefähr)|[Hinweis 3](#notes)|[Hinweis 3](#notes)|[Hinweis 3](#notes)|[Hinweis 3](#notes)|[Hinweis 3](#notes)|[Hinweis 3](#notes)|
|Max. gleichzeitige Worker (Anforderungen)|200|400|600|800|1000|1200|
|Max. gleichzeitige Sitzungen|30.000|30.000|30.000|30.000|30.000|30.000|
|Sekundäre Replikate|0–4|0–4|0–4|0–4|0–4|0–4|
|Multi-AZ|–|–|–|–|–|–|
|Horizontale Leseskalierung|Ja|Ja|Ja|Ja|Ja|Ja|
|Vermerkdauer im Sicherungsspeicher|7 Tage|7 Tage|7 Tage|7 Tage|7 Tage|7 Tage|
|||

### <a name="gen4-compute-generation-part-2"></a>Computegeneration Gen4 (Teil 2)

|Leistungsstufe|HS_Gen4_7|HS_Gen4_8|HS_Gen4_9|HS_Gen4_10|HS_Gen4_16|HS_Gen4_24|
|:--- | ---: |--: |--: | --: |--: |--: |
|Computegeneration|Gen4|Gen4|Gen4|Gen4|Gen4|Gen4|
|V-Kerne|7|8|9|10|16|24|
|Arbeitsspeicher (GB)|49|56|63|70|112|159,5|
|[RBPEX](sql-database-service-tier-hyperscale.md#compute)-Größe|3 × Arbeitsspeicher|3 × Arbeitsspeicher|3 × Arbeitsspeicher|3 × Arbeitsspeicher|3 × Arbeitsspeicher|3 × Arbeitsspeicher|
|Columnstore-Unterstützung|Ja|Ja|Ja|Ja|Ja|Ja|
|In-Memory-OLTP-Speicher (GB)|–|–|–|–|–|–|
|Maximale Datengröße (TB)|100 |100 |100 |100 |100 |100 |
|Maximale Protokollgröße (TB)|1 |1 |1 |1 |1 |1 |
|Max. Datengröße von TempDB (GB)|224|256|288|320|512|768|
|Speichertyp| [Hinweis 1](#notes) |[Hinweis 1](#notes) |[Hinweis 1](#notes) |[Hinweis 1](#notes) |[Hinweis 1](#notes) |[Hinweis 1](#notes) |
|Max. Datenrate, IOPS*|[Hinweis 2](#notes)|[Hinweis 2](#notes)|[Hinweis 2](#notes)|[Hinweis 2](#notes)|[Hinweis 2](#notes)|[Hinweis 2](#notes)|
|E/A-Wartezeit (ungefähr)|[Hinweis 3](#notes)|[Hinweis 3](#notes)|[Hinweis 3](#notes)|[Hinweis 3](#notes)|[Hinweis 3](#notes)|[Hinweis 3](#notes)|
|Max. gleichzeitige Worker (Anforderungen)|1400|1600|1800|2000|3200|4800|
|Max. gleichzeitige Sitzungen|30.000|30.000|30.000|30.000|30.000|30.000|
|Sekundäre Replikate|0–4|0–4|0–4|0–4|0–4|0–4|
|Multi-AZ|–|–|–|–|–|–|
|Horizontale Leseskalierung|Ja|Ja|Ja|Ja|Ja|Ja|
|Vermerkdauer im Sicherungsspeicher|7 Tage|7 Tage|7 Tage|7 Tage|7 Tage|7 Tage|
|||

\* Der maximale Wert für E/A-Größen im Bereich von 8 KB bis 64 KB. Die tatsächlichen IOPS sind von der Arbeitsauslastung abhängig. Weitere Informationen finden Sie unter [Daten-E/A-Governance](sql-database-resource-limits-database-server.md#resource-governance).

## <a name="hyperscale---provisioned-compute---gen5"></a>Hyperscale – bereitgestelltes Computing – Gen5

### <a name="gen5-compute-generation-part-1"></a>Computegeneration Gen5 (Teil 1)

|Leistungsstufe|HS_Gen5_2|HS_Gen5_4|HS_Gen5_6|HS_Gen_8|HS_Gen5_10|HS_Gen5_12|HS_Gen5_14|
|:--- | --: |--: |--: |--: |---: | --: |--: |--: |
|Computegeneration|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|
|V-Kerne|2|4|6|8|10|12|14|
|Arbeitsspeicher (GB)|10,4|20,8|31,1|41,5|51,9|62,3|72,7|
|[RBPEX](sql-database-service-tier-hyperscale.md#compute)-Größe|3 × Arbeitsspeicher|3 × Arbeitsspeicher|3 × Arbeitsspeicher|3 × Arbeitsspeicher|3 × Arbeitsspeicher|3 × Arbeitsspeicher|3 × Arbeitsspeicher|
|Columnstore-Unterstützung|Ja|Ja|Ja|Ja|Ja|Ja|Ja|
|In-Memory-OLTP-Speicher (GB)|–|–|–|–|–|–|–|
|Maximale Datengröße (TB)|100 |100 |100 |100 |100 |100 |100|
|Maximale Protokollgröße (TB)|1 |1 |1 |1 |1 |1 |1 |
|Max. Datengröße von TempDB (GB)|64|128|192|256|320|384|448|
|Speichertyp| [Hinweis 1](#notes) |[Hinweis 1](#notes)|[Hinweis 1](#notes) |[Hinweis 1](#notes) |[Hinweis 1](#notes) |[Hinweis 1](#notes) |[Hinweis 1](#notes) |
|Max. Datenrate, IOPS*|[Hinweis 2](#notes)|[Hinweis 2](#notes)|[Hinweis 2](#notes)|[Hinweis 2](#notes)|[Hinweis 2](#notes)|[Hinweis 2](#notes)|[Hinweis 2](#notes)|
|E/A-Wartezeit (ungefähr)|[Hinweis 3](#notes)|[Hinweis 3](#notes)|[Hinweis 3](#notes)|[Hinweis 3](#notes)|[Hinweis 3](#notes)|[Hinweis 3](#notes)|[Hinweis 3](#notes)|
|Max. gleichzeitige Worker (Anforderungen)|200|400|600|800|1000|1200|1400|
|Max. gleichzeitige Sitzungen|30.000|30.000|30.000|30.000|30.000|30.000|30.000|
|Sekundäre Replikate|0–4|0–4|0–4|0–4|0–4|0–4|0–4|
|Multi-AZ|–|–|–|–|–|–|–|
|Horizontale Leseskalierung|Ja|Ja|Ja|Ja|Ja|Ja|Ja|
|Vermerkdauer im Sicherungsspeicher|7 Tage|7 Tage|7 Tage|7 Tage|7 Tage|7 Tage|7 Tage|
|||

\* Der maximale Wert für E/A-Größen im Bereich von 8 KB bis 64 KB. Die tatsächlichen IOPS sind von der Arbeitsauslastung abhängig. Weitere Informationen finden Sie unter [Daten-E/A-Governance](sql-database-resource-limits-database-server.md#resource-governance).

### <a name="gen5-compute-generation-part-2"></a>Computegeneration Gen5 (Teil 2)

|Leistungsstufe|HS_Gen5_16|HS_Gen5_18|HS_Gen5_20|HS_Gen_24|HS_Gen5_32|HS_Gen5_40|HS_Gen5_80|
|:--- | --: |--: |--: |--: |---: |--: |--: |
|Computegeneration|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|
|V-Kerne|16|18|20|24|32|40|80|
|Arbeitsspeicher (GB)|83|93,4|103,8|124,6|166,1|207,6|415,2|
|[RBPEX](sql-database-service-tier-hyperscale.md#compute)-Größe|3 × Arbeitsspeicher|3 × Arbeitsspeicher|3 × Arbeitsspeicher|3 × Arbeitsspeicher|3 × Arbeitsspeicher|3 × Arbeitsspeicher|3 × Arbeitsspeicher|
|Columnstore-Unterstützung|Ja|Ja|Ja|Ja|Ja|Ja|Ja|
|In-Memory-OLTP-Speicher (GB)|–|–|–|–|–|–|–|
|Maximale Datengröße (TB)|100 |100 |100 |100 |100 |100 |100 |
|Maximale Protokollgröße (TB)|1 |1 |1 |1 |1 |1 |1 |
|Max. Datengröße von TempDB (GB)|512|576|640|768|1024|1280|2560|
|Speichertyp| [Hinweis 1](#notes) |[Hinweis 1](#notes)|[Hinweis 1](#notes)|[Hinweis 1](#notes) |[Hinweis 1](#notes) |[Hinweis 1](#notes) |[Hinweis 1](#notes) |
|Max. Datenrate, IOPS*|[Hinweis 2](#notes)|[Hinweis 2](#notes)|[Hinweis 2](#notes)|[Hinweis 2](#notes)|[Hinweis 2](#notes)|[Hinweis 2](#notes)|[Hinweis 2](#notes)|
|E/A-Wartezeit (ungefähr)|[Hinweis 3](#notes)|[Hinweis 3](#notes)|[Hinweis 3](#notes)|[Hinweis 3](#notes)|[Hinweis 3](#notes)|[Hinweis 3](#notes)|[Hinweis 3](#notes)|
|Max. gleichzeitige Worker (Anforderungen)|200|400|800|1600|2400|3200|8\.000|
|Max. gleichzeitige Sitzungen|30.000|30.000|30.000|30.000|30.000|30.000|30.000|
|Sekundäre Replikate|0–4|0–4|0–4|0–4|0–4|0–4|0–4|
|Multi-AZ|–|–|–|–|–|–|–|
|Horizontale Leseskalierung|Ja|Ja|Ja|Ja|Ja|Ja|Ja|
|Vermerkdauer im Sicherungsspeicher|7 Tage|7 Tage|7 Tage|7 Tage|7 Tage|7 Tage|7 Tage|
|||

\* Der maximale Wert für E/A-Größen im Bereich von 8 KB bis 64 KB. Die tatsächlichen IOPS sind von der Arbeitsauslastung abhängig. Weitere Informationen finden Sie unter [Daten-E/A-Governance](sql-database-resource-limits-database-server.md#resource-governance).

#### <a name="notes"></a>Notizen

**Hinweis 1:** Hyperscale ist eine mehrschichtige Architektur mit separaten Compute- und Speicherkomponenten: [Architektur der Dienstebene „Hyperscale“](sql-database-service-tier-hyperscale.md#distributed-functions-architecture)

**Hinweis 2:** Hyperscale ist eine mehrschichtige Architektur mit Caching auf mehreren Ebenen. Der tatsächliche IOPS-Wert hängt von der Workload ab.

**Hinweis 3:** Die Latenz beträgt 1–2 ms für Daten im SSD-basierten RBPEX-Cache auf Computereplikaten, der die am häufigsten verwendeten Datenseiten zwischenspeichert. Für Daten, die von Seitenservern abgerufen werden, gilt eine höhere Latenz.

## <a name="general-purpose---provisioned-compute---gen4"></a>Universell – bereitgestelltes Computing – Gen4

> [!IMPORTANT]
> Neue Gen4-Datenbanken werden in den Regionen „Australien, Osten“ und „Brasilien, Süden“ nicht mehr unterstützt.

### <a name="gen4-compute-generation-part-1"></a>Computegeneration Gen4 (Teil 1)

|Computegröße|GP_Gen4_1|GP_Gen4_2|GP_Gen4_3|GP_Gen4_4|GP_Gen4_5|GP_Gen4_6
|:--- | --: |--: |--: |--: |--: |--: |
|Computegeneration|Gen4|Gen4|Gen4|Gen4|Gen4|Gen4|
|V-Kerne|1|2|3|4|5|6|
|Arbeitsspeicher (GB)|7|14|21|28|35|42|
|Columnstore-Unterstützung|Ja|Ja|Ja|Ja|Ja|Ja|
|In-Memory-OLTP-Speicher (GB)|–|–|–|–|–|–|
|Maximale Datengröße (GB)|1024|1024|1536|1536|1536|3072|
|Maximale Protokollgröße (GB)|307|307|461|461|461|922|
|Max. Datengröße von TempDB (GB)|32|64|96|128|160|192|
|Speichertyp|Remote-SSD|Remote-SSD|Remote-SSD|Remote-SSD|Remote-SSD|Remote-SSD|
|E/A-Wartezeit (ungefähr)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|
|Max. Datenrate, IOPS*|320|640|960|1280|1600|1920|
|Max. Protokollrate (MBit/s)|3,75|7,5|11,25|15|18,75|22,5|
|Max. gleichzeitige Worker (Anforderungen)|200|400|600|800|1000|1200|
|Max. gleichzeitige Sitzungen|30.000|30.000|30.000|30.000|30.000|30.000|
|Anzahl von Replikaten|1|1|1|1|1|1|
|Multi-AZ|–|–|–|–|–|–|
|Horizontale Leseskalierung|–|–|–|–|–|–|
|Enthaltener Sicherungsspeicher|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|

\* Der maximale Wert für E/A-Größen im Bereich von 8 KB bis 64 KB. Die tatsächlichen IOPS sind von der Arbeitsauslastung abhängig. Weitere Informationen finden Sie unter [Daten-E/A-Governance](sql-database-resource-limits-database-server.md#resource-governance).

### <a name="gen4-compute-generation-part-2"></a>Computegeneration Gen4 (Teil 2)

|Computegröße|GP_Gen4_7|GP_Gen4_8|GP_Gen4_9|GP_Gen4_10|GP_Gen4_16|GP_Gen4_24
|:--- | --: |--: |--: |--: |--: |--: |
|Computegeneration|Gen4|Gen4|Gen4|Gen4|Gen4|Gen4|
|V-Kerne|7|8|9|10|16|24|
|Arbeitsspeicher (GB)|49|56|63|70|112|159,5|
|Columnstore-Unterstützung|Ja|Ja|Ja|Ja|Ja|Ja|
|In-Memory-OLTP-Speicher (GB)|–|–|–|–|–|–|
|Maximale Datengröße (GB)|3072|3072|3072|3072|4096|4096|
|Maximale Protokollgröße (GB)|922|922|922|922|1229|1229|
|Max. Datengröße von TempDB (GB)|224|256|288|320|512|768|
|Speichertyp|Remote-SSD|Remote-SSD|Remote-SSD|Remote-SSD|Remote-SSD|Remote-SSD|
|E/A-Wartezeit (ungefähr)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)
|Max. Datenrate, IOPS*|2240|2560|2880|3200|5120|7680|
|Max. Protokollrate (MBit/s)|26,3|30|30|30|30|30|
|Max. gleichzeitige Worker (Anforderungen)|1400|1600|1800|2000|3200|4800|
|Max. gleichzeitige Sitzungen|30.000|30.000|30.000|30.000|30.000|30.000|
|Anzahl von Replikaten|1|1|1|1|1|1|
|Multi-AZ|–|–|–|–|–|–|
|Horizontale Leseskalierung|–|–|–|–|–|–|
|Enthaltener Sicherungsspeicher|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|

\* Der maximale Wert für E/A-Größen im Bereich von 8 KB bis 64 KB. Die tatsächlichen IOPS sind von der Arbeitsauslastung abhängig. Weitere Informationen finden Sie unter [Daten-E/A-Governance](sql-database-resource-limits-database-server.md#resource-governance).

## <a name="general-purpose---provisioned-compute---gen5"></a>Universell – bereitgestelltes Computing – Gen5

### <a name="gen5-compute-generation-part-1"></a>Computegeneration Gen5 (Teil 1)

|Computegröße|GP_Gen5_2|GP_Gen5_4|GP_Gen5_6|GP_Gen5_8|GP_Gen5_10|GP_Gen5_12|GP_Gen5_14|
|:--- | --: |--: |--: |--: |---: | --: |--: |
|Computegeneration|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|
|V-Kerne|2|4|6|8|10|12|14|
|Arbeitsspeicher (GB)|10,4|20,8|31,1|41,5|51,9|62,3|72,7|
|Columnstore-Unterstützung|Ja|Ja|Ja|Ja|Ja|Ja|Ja|
|In-Memory-OLTP-Speicher (GB)|–|–|–|–|–|–|–|
|Maximale Datengröße (GB)|1024|1024|1536|1536|1536|3072|3072|
|Maximale Protokollgröße (GB)|307|307|461|461|461|922|922|
|Max. Datengröße von TempDB (GB)|64|128|192|256|320|384|384|
|Speichertyp|Remote-SSD|Remote-SSD|Remote-SSD|Remote-SSD|Remote-SSD|Remote-SSD|Remote-SSD|
|E/A-Wartezeit (ungefähr)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|
|Max. Datenrate, IOPS*|640|1280|1920|2560|3200|3840|4480|
|Max. Protokollrate (MBit/s)|7,5|15|22,5|30|30|30|30|
|Max. gleichzeitige Worker (Anforderungen)|200|400|600|800|1000|1200|1400|
|Max. gleichzeitige Sitzungen|30.000|30.000|30.000|30.000|30.000|30.000|30.000|
|Anzahl von Replikaten|1|1|1|1|1|1|1|
|Multi-AZ|–|–|–|–|–|–|–|
|Horizontale Leseskalierung|–|–|–|–|–|–|–|
|Enthaltener Sicherungsspeicher|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|

\* Der maximale Wert für E/A-Größen im Bereich von 8 KB bis 64 KB. Die tatsächlichen IOPS sind von der Arbeitsauslastung abhängig. Weitere Informationen finden Sie unter [Daten-E/A-Governance](sql-database-resource-limits-database-server.md#resource-governance).

### <a name="gen5-compute-generation-part-2"></a>Computegeneration Gen5 (Teil 2)

|Computegröße|GP_Gen5_16|GP_Gen5_18|GP_Gen5_20|GP_Gen5_24|GP_Gen5_32|GP_Gen5_40|GP_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |
|Computegeneration|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|
|V-Kerne|16|18|20|24|32|40|80|
|Arbeitsspeicher (GB)|83|93,4|103,8|124,6|166,1|207,6|415,2|
|Columnstore-Unterstützung|Ja|Ja|Ja|Ja|Ja|Ja|Ja|
|In-Memory-OLTP-Speicher (GB)|–|–|–|–|–|–|–|
|Maximale Datengröße (GB)|3072|3072|3072|4096|4096|4096|4096|
|Maximale Protokollgröße (GB)|922|922|922|1229|1229|1229|1229|
|Max. Datengröße von TempDB (GB)|512|576|640|768|1024|1280|2560|
|Speichertyp|Remote-SSD|Remote-SSD|Remote-SSD|Remote-SSD|Remote-SSD|Remote-SSD|Remote-SSD|
|E/A-Wartezeit (ungefähr)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|
|Max. Datenrate, IOPS*|5120|5760|6400|7680|10.240|12800|25600|
|Max. Protokollrate (MBit/s)|30|30|30|30|30|30|30|
|Max. gleichzeitige Worker (Anforderungen)|1600|1800|2000|2400|3200|4000|8\.000|
|Max. gleichzeitige Sitzungen|30.000|30.000|30.000|30.000|30.000|30.000|30.000|
|Anzahl von Replikaten|1|1|1|1|1|1|1|
|Multi-AZ|–|–|–|–|–|–|–|
|Horizontale Leseskalierung|–|–|–|–|–|–|–|
|Enthaltener Sicherungsspeicher|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|

\* Der maximale Wert für E/A-Größen im Bereich von 8 KB bis 64 KB. Die tatsächlichen IOPS sind von der Arbeitsauslastung abhängig. Weitere Informationen finden Sie unter [Daten-E/A-Governance](sql-database-resource-limits-database-server.md#resource-governance).

## <a name="general-purpose---provisioned-compute---fsv2-series"></a>Universell – bereitgestelltes Computing – Fsv2-Serie

### <a name="fsv2-series-compute-generation-preview"></a>Computegeneration der Fsv2-Serie (Vorschau)

|Computegröße|GP_Fsv2_72|
|:--- | --: |
|Computegeneration|Fsv2-Serie|
|V-Kerne|72|
|Arbeitsspeicher (GB)|136,2|
|Columnstore-Unterstützung|Ja|
|In-Memory-OLTP-Speicher (GB)|–|
|Maximale Datengröße (GB)|4096|
|Maximale Protokollgröße (GB)|1024|
|Max. Datengröße von TempDB (GB)|333|
|Speichertyp|Remote-SSD|
|E/A-Wartezeit (ungefähr)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|
|Max. Datenrate, IOPS*|23.040|
|Max. Protokollrate (MBit/s)|30|
|Max. gleichzeitige Worker (Anforderungen)|3600|
|Max. gleichzeitige Sitzungen|30.000|
|Anzahl von Replikaten|1|
|Multi-AZ|–|
|Horizontale Leseskalierung|–|
|Enthaltener Sicherungsspeicher|1 × Datenbankgröße|

\* Der maximale Wert für E/A-Größen im Bereich von 8 KB bis 64 KB. Die tatsächlichen IOPS sind von der Arbeitsauslastung abhängig. Weitere Informationen finden Sie unter [Daten-E/A-Governance](sql-database-resource-limits-database-server.md#resource-governance).

## <a name="business-critical---provisioned-compute---gen4"></a>Unternehmenskritisch – bereitgestelltes Computing – Gen4

> [!IMPORTANT]
> Neue Gen4-Datenbanken werden in den Regionen „Australien, Osten“ und „Brasilien, Süden“ nicht mehr unterstützt.

### <a name="gen4-compute-generation-part-1"></a>Computegeneration Gen4 (Teil 1)

|Computegröße|BC_Gen4_1|BC_Gen4_2|BC_Gen4_3|BC_Gen4_4|BC_Gen4_5|BC_Gen4_6|
|:--- | --: |--: |--: |--: |--: |--: |
|Computegeneration|Gen4|Gen4|Gen4|Gen4|Gen4|Gen4|
|V-Kerne|1|2|3|4|5|6|
|Arbeitsspeicher (GB)|7|14|21|28|35|42|
|Columnstore-Unterstützung|Ja|Ja|Ja|Ja|Ja|Ja|
|In-Memory-OLTP-Speicher (GB)|1|2|3|4|5|6|
|Speichertyp|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|
|Maximale Datengröße (GB)|1024|1024|1024|1024|1024|1024|
|Maximale Protokollgröße (GB)|307|307|307|307|307|307|
|Max. Datengröße von TempDB (GB)|32|64|96|128|160|192|
|E/A-Wartezeit (ungefähr)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|
|Max. Datenrate, IOPS*|4\.000|8\.000|12.000|16.000|20.000|24.000|
|Max. Protokollrate (MBit/s)|8|16|24|32|40|48|
|Max. gleichzeitige Worker (Anforderungen)|200|400|600|800|1000|1200|
|Maximale Anzahl gleichzeitiger Anmeldungen|200|400|600|800|1000|1200|
|Max. gleichzeitige Sitzungen|30.000|30.000|30.000|30.000|30.000|30.000|
|Anzahl von Replikaten|4|4|4|4|4|4|
|Multi-AZ|Ja|Ja|Ja|Ja|Ja|Ja|
|Horizontale Leseskalierung|Ja|Ja|Ja|Ja|Ja|Ja|
|Enthaltener Sicherungsspeicher|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|

\* Der maximale Wert für E/A-Größen im Bereich von 8 KB bis 64 KB. Die tatsächlichen IOPS sind von der Arbeitsauslastung abhängig. Weitere Informationen finden Sie unter [Daten-E/A-Governance](sql-database-resource-limits-database-server.md#resource-governance).

### <a name="gen4-compute-generation-part-2"></a>Computegeneration Gen4 (Teil 2)

|Computegröße|BC_Gen4_7|BC_Gen4_8|BC_Gen4_9|BC_Gen4_10|BC_Gen4_16|BC_Gen4_24|
|:--- | --: |--: |--: |--: |--: |--: |
|Computegeneration|Gen4|Gen4|Gen4|Gen4|Gen4|Gen4|
|V-Kerne|7|8|9|10|16|24|
|Arbeitsspeicher (GB)|49|56|63|70|112|159,5|
|Columnstore-Unterstützung|Ja|Ja|Ja|Ja|Ja|Ja|
|In-Memory-OLTP-Speicher (GB)|7|8|9.5|11|20|36|
|Speichertyp|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|
|Maximale Datengröße (GB)|1024|1024|1024|1024|1024|1024|
|Maximale Protokollgröße (GB)|307|307|307|307|307|307|
|Max. Datengröße von TempDB (GB)|224|256|288|320|512|768|
|E/A-Wartezeit (ungefähr)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|
|Max. Datenrate, IOPS |28.000|32.000|36.000|40.000|64.000|76.800|
|Max. Protokollrate (MBit/s)|56|64|64|64|64|64|
|Max. gleichzeitige Worker (Anforderungen)|1400|1600|1800|2000|3200|4800|
|Max. gleichzeitige Anmeldungen (Anforderungen)|1400|1600|1800|2000|3200|4800|
|Max. gleichzeitige Sitzungen|30.000|30.000|30.000|30.000|30.000|30.000|
|Anzahl von Replikaten|4|4|4|4|4|4|
|Multi-AZ|Ja|Ja|Ja|Ja|Ja|Ja|
|Horizontale Leseskalierung|Ja|Ja|Ja|Ja|Ja|Ja|
|Enthaltener Sicherungsspeicher|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|

\* Der maximale Wert für E/A-Größen im Bereich von 8 KB bis 64 KB. Die tatsächlichen IOPS sind von der Arbeitsauslastung abhängig. Weitere Informationen finden Sie unter [Daten-E/A-Governance](sql-database-resource-limits-database-server.md#resource-governance).

## <a name="business-critical---provisioned-compute---gen5"></a>Unternehmenskritisch – bereitgestelltes Computing – Gen5

### <a name="gen5-compute-generation-part-1"></a>Computegeneration Gen5 (Teil 1)

|Computegröße|BC_Gen5_2|BC_Gen5_4|BC_Gen5_6|BC_Gen5_8|BC_Gen5_10|BC_Gen5_12|BC_Gen5_14|
|:--- | --: |--: |--: |--: |---: | --: |--: |
|Computegeneration|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|
|V-Kerne|2|4|6|8|10|12|14|
|Arbeitsspeicher (GB)|10,4|20,8|31,1|41,5|51,9|62,3|72,7|
|Columnstore-Unterstützung|Ja|Ja|Ja|Ja|Ja|Ja|Ja|
|In-Memory-OLTP-Speicher (GB)|1,57|3,14|4.71|6,28|8,65|11,02|13,39|
|Maximale Datengröße (GB)|1024|1024|1536|1536|1536|3072|3072|
|Maximale Protokollgröße (GB)|307|307|461|461|461|922|922|
|Max. Datengröße von TempDB (GB)|64|128|192|256|320|384|448|
|Speichertyp|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|
|E/A-Wartezeit (ungefähr)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|
|Max. Datenrate, IOPS*|8\.000|16.000|24.000|32.000|40.000|48.000|56.000|
|Max. Protokollrate (MBit/s)|24|48|72|96|96|96|96|
|Max. gleichzeitige Worker (Anforderungen)|200|400|600|800|1000|1200|1400|
|Maximale Anzahl gleichzeitiger Anmeldungen|200|400|600|800|1000|1200|1400|
|Max. gleichzeitige Sitzungen|30.000|30.000|30.000|30.000|30.000|30.000|30.000|
|Anzahl von Replikaten|4|4|4|4|4|4|4|
|Multi-AZ|Ja|Ja|Ja|Ja|Ja|Ja|Ja|
|Horizontale Leseskalierung|Ja|Ja|Ja|Ja|Ja|Ja|Ja|
|Enthaltener Sicherungsspeicher|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|

\* Der maximale Wert für E/A-Größen im Bereich von 8 KB bis 64 KB. Die tatsächlichen IOPS sind von der Arbeitsauslastung abhängig. Weitere Informationen finden Sie unter [Daten-E/A-Governance](sql-database-resource-limits-database-server.md#resource-governance).

### <a name="gen5-compute-generation-part-2"></a>Computegeneration Gen5 (Teil 2)

|Computegröße|BC_Gen5_16|BC_Gen5_18|BC_Gen5_20|BC_Gen5_24|BC_Gen5_32|BC_Gen5_40|BC_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |
|Computegeneration|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|
|V-Kerne|16|18|20|24|32|40|80|
|Arbeitsspeicher (GB)|83|93,4|103,8|124,6|166,1|207,6|415,2|
|Columnstore-Unterstützung|Ja|Ja|Ja|Ja|Ja|Ja|Ja|
|In-Memory-OLTP-Speicher (GB)|15,77|18,14|20,51|25,25|37,94|52.23|131,64|
|Maximale Datengröße (GB)|3072|3072|3072|4096|4096|4096|4096|
|Maximale Protokollgröße (GB)|922|922|922|1229|1229|1229|1229|
|Max. Datengröße von TempDB (GB)|512|576|640|768|1024|1280|2560|
|Speichertyp|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|
|E/A-Wartezeit (ungefähr)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|
|Max. Datenrate, IOPS*|64.000|72.000|80.000|96.000|128.000|160.000|204.800|
|Max. Protokollrate (MBit/s)|96|96|96|96|96|96|96|
|Max. gleichzeitige Worker (Anforderungen)|1600|1800|2000|2400|3200|4000|8\.000|
|Maximale Anzahl gleichzeitiger Anmeldungen|1600|1800|2000|2400|3200|4000|8\.000|
|Max. gleichzeitige Sitzungen|30.000|30.000|30.000|30.000|30.000|30.000|30.000|
|Anzahl von Replikaten|4|4|4|4|4|4|4|
|Multi-AZ|Ja|Ja|Ja|Ja|Ja|Ja|Ja|
|Horizontale Leseskalierung|Ja|Ja|Ja|Ja|Ja|Ja|Ja|
|Enthaltener Sicherungsspeicher|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|

\* Der maximale Wert für E/A-Größen im Bereich von 8 KB bis 64 KB. Die tatsächlichen IOPS sind von der Arbeitsauslastung abhängig. Weitere Informationen finden Sie unter [Daten-E/A-Governance](sql-database-resource-limits-database-server.md#resource-governance).

## <a name="business-critical---provisioned-compute---m-series"></a>Unternehmenskritisch – bereitgestelltes Computing – M-Serie

### <a name="m-series-compute-generation-preview"></a>Computegeneration der M-Serie (Vorschau)

|Computegröße|GP_M_128|
|:--- | --: |
|Computegeneration|M-Serie|
|V-Kerne|128|
|Arbeitsspeicher (GB)|3767|
|Columnstore-Unterstützung|Ja|
|In-Memory-OLTP-Speicher (GB)|481|
|Maximale Datengröße (GB)|4096|
|Maximale Protokollgröße (GB)|2048|
|Max. Datengröße von TempDB (GB)|4096|
|Speichertyp|Lokale SSD|
|E/A-Wartezeit (ungefähr)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|
|Max. Datenrate, IOPS*|204.800|
|Max. Protokollrate (MBit/s)|192|
|Max. gleichzeitige Worker (Anforderungen)|12800|
|Max. gleichzeitige Sitzungen|30.000|
|Anzahl von Replikaten|4|
|Multi-AZ|Ja|
|Horizontale Leseskalierung|Ja|
|Enthaltener Sicherungsspeicher|1 × Datenbankgröße|

\* Der maximale Wert für E/A-Größen im Bereich von 8 KB bis 64 KB. Die tatsächlichen IOPS sind von der Arbeitsauslastung abhängig. Weitere Informationen finden Sie unter [Daten-E/A-Governance](sql-database-resource-limits-database-server.md#resource-governance).

> [!IMPORTANT]
> Unter bestimmten Umständen müssen Sie ggf. eine Datenbank verkleinern, um ungenutzten Speicherplatz freizugeben. Weitere Informationen finden Sie unter [Verwalten von Dateispeicherplatz in Azure SQL-Datenbank](sql-database-file-space-management.md).

## <a name="next-steps"></a>Nächste Schritte

- Informationen zu DTU-Ressourcenlimits für einen Singleton finden Sie unter [Ressourcenlimits für Singletons, die das DTU-Kaufmodell verwenden](sql-database-dtu-resource-limits-single-databases.md).
- Informationen zu V-Kern-Ressourcenlimits für Pools für elastische Datenbanken finden Sie unter [Ressourcenlimits für Pools für elastische Datenbanken, die das V-Kern-Kaufmodell verwenden](sql-database-vcore-resource-limits-elastic-pools.md).
- Informationen zu DTU-Ressourcenlimits für Pools für elastische Datenbanken finden Sie unter [Ressourcenlimits für Pools für elastische Datenbanken, die das DTU-Kaufmodell verwenden](sql-database-dtu-resource-limits-elastic-pools.md).
- Informationen zu den Ressourcenlimits für verwaltete Instanzen finden Sie unter [Ressourcenlimits bei verwalteten Instanzen](sql-database-managed-instance-resource-limits.md).
- Informationen zu allgemeinen Azure-Einschränkungen finden Sie unter [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](../azure-resource-manager/management/azure-subscription-service-limits.md).
- Informationen zu Ressourcenlimits auf Server- und Abonnementebene auf einem Datenbankserver finden Sie unter [Übersicht über Ressourcenlimits für einen SQL-Datenbank-Server](sql-database-resource-limits-database-server.md).
