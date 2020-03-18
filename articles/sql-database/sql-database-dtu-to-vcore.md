---
title: Migrieren von DTU zu V-Kern
description: Migrieren Sie vom DTU-Modell zum V-Kern-Modell. Das Migrieren zu V-Kern ähnelt dem Up- oder Downgrade zwischen der Standard- und Premium-Dienstebene.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: sashan, moslake, carlrab
ms.date: 03/09/2020
ms.openlocfilehash: 693065046f92e0e9eade14c43e9942772440937d
ms.sourcegitcommit: 8f4d54218f9b3dccc2a701ffcacf608bbcd393a6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2020
ms.locfileid: "78945401"
---
# <a name="migrate-from-the-dtu-based-model-to-the-vcore-based-model"></a>Migrieren vom DTU-basierten Modell zum V-Kern-basierten Modell

## <a name="migrate-a-database"></a>Migrieren einer Datenbank

Die Migration einer Datenbank vom DTU-basierten zum V-Kern-basierten Kaufmodell ist vergleichbar mit einem Up- oder Downgrade zwischen der Standard- und Premium-Dienstebene im DTU-basierten Kaufmodell.

## <a name="migrate-databases-that-use-geo-replication"></a>Migrieren von Datenbanken mit Georeplikation

Die Migration vom DTU-basierten zum V-Kern-basierten Modell ähnelt dem Up- oder Downgrade der Georeplikationsbeziehungen zwischen Datenbanken auf der Standard- und Premium-Dienstebene. Sie müssen die Georeplikation während der Migration nicht beenden, es müssen jedoch die folgenden Sequenzierungsregeln eingehalten werden:

- Bei einem Upgrade müssen Sie zuerst das Upgrade für die sekundäre Datenbank und anschließend das Upgrade für die primäre Datenbank durchführen.
- Drehen Sie bei einem Downgrade die Reihenfolge um: Führen Sie zuerst das Downgrade für die primäre und anschließend das Downgrade für die sekundäre Datenbank durch.

Bei Verwendung der Georeplikation zwischen zwei Pools für elastische Datenbanken empfiehlt es sich, einen Pool als primäres Element und den anderen als sekundäres Element festzulegen. In diesem Fall sollten Sie sich beim Migrieren von Pools für elastische Datenbanken an die gleiche Sequenzierungsanleitung halten. Falls Sie jedoch über Pools für elastische Datenbanken verfügen, die sowohl primäre als auch sekundäre Datenbanken enthalten, müssen Sie den Pool mit der höheren Auslastung als primäres Element behandeln und die Sequenzierungsregeln entsprechend befolgen.  

Die folgende Tabelle enthält eine Anleitung für spezifische Migrationsszenarien:

|Aktuelle Dienstebene|Zieldienstebene|Migrationstyp|Benutzeraktionen|
|---|---|---|---|
|Standard|Allgemeiner Zweck|Seitwärts|Die Migration ist in einer beliebigen Reihenfolge möglich, aber Sie müssen für eine geeignete V-Kern-Größe sorgen*|
|Premium|Unternehmenskritisch|Seitwärts|Die Migration ist in einer beliebigen Reihenfolge möglich, aber Sie müssen für eine geeignete V-Kern-Größe sorgen*|
|Standard|Unternehmenskritisch|Aktualisieren|Sekundäre Einheit muss zuerst migriert werden|
|Unternehmenskritisch|Standard|Downgrade|Primäre Einheit muss zuerst migriert werden|
|Premium|Allgemeiner Zweck|Downgrade|Primäre Einheit muss zuerst migriert werden|
|Allgemeiner Zweck|Premium|Aktualisieren|Sekundäre Einheit muss zuerst migriert werden|
|Unternehmenskritisch|Allgemeiner Zweck|Downgrade|Primäre Einheit muss zuerst migriert werden|
|Allgemeiner Zweck|Unternehmenskritisch|Aktualisieren|Sekundäre Einheit muss zuerst migriert werden|
||||

\* Als Faustregel gilt: Auf der Standardebene wird für jeweils 100 DTUs mindestens ein V-Kern benötigt. Auf der Premium-Ebene wird für jeweils 125 DTUs mindestens ein virtueller Kern benötigt. Weitere Informationen finden Sie unter [Auf virtuellen Kernen basierendes Erwerbsmodell](https://docs.microsoft.com/azure/sql-database/sql-database-purchase-models#vcore-based-purchasing-model).

## <a name="migrate-failover-groups"></a>Migrieren von Failovergruppen

Für die Migration von Failovergruppen mit mehreren Datenbanken ist eine individuelle Migration der primären und sekundären Datenbank erforderlich. Während dieses Prozesses gelten die gleichen Aspekte und Sequenzierungsregeln. Nachdem die Datenbanken auf das V-Kern-basierte Kaufmodell umgestellt wurden, bleibt die Failovergruppe mit den gleichen Richtlinieneinstellungen wirksam.

### <a name="create-a-geo-replication-secondary-database"></a>Erstellen einer sekundären Datenbank für die Georeplikation

Eine sekundäre Datenbank für die Georeplikation (Geo-Sekundärdatenbank) kann nur mit der gleichen Dienstebene erstellt werden, die auch für die primäre Datenbank verwendet wurde. Bei Datenbanken mit hoher Protokollgenerierungsrate empfiehlt es sich, die Geo-Sekundärdatenbank mit der gleichen Computegröße zu erstellen wie die primäre Datenbank.

Wenn Sie eine Geo-Sekundärdatenbank im Pool für elastische Datenbanken für eine einzelne primäre Datenbank erstellen, muss die Einstellung `maxVCore` für den Pool der Computegröße der primären Datenbank entsprechen. Wenn Sie eine Geo-Sekundärdatenbank für eine primäre Datenbank in einem anderen Pool für elastische Datenbanken erstellen, sollte die Einstellung `maxVCore` der Pools gleich sein.

## <a name="use-database-copy-to-convert-a-dtu-based-database-to-a-vcore-based-database"></a>Verwenden einer Datenbankkopie zum Konvertieren einer DTU-basierten Datenbank in eine V-Kern-basierte Datenbank

Sie können eine beliebige Datenbank mit einer DTU-basierten Computegröße in eine Datenbank mit einer V-Kern-basierten Computegröße kopieren, ohne dass hierfür Einschränkungen oder spezielle Sequenzierungen gelten, solange die Zielcomputegröße die maximale Datenbankgröße der Quelldatenbank unterstützt. Die Datenbankkopie erstellt eine Momentaufnahme der Daten zum Startzeitpunkt des Kopiervorgangs und synchronisiert keine Daten zwischen Quelle und Ziel.

## <a name="next-steps"></a>Nächste Schritte

- Die spezifischen Computegrößen und Speichergrößenoptionen für Einzeldatenbanken finden Sie unter [V-Kern-basierte Ressourceneinschränkungen in SQL-Datenbank für Einzeldatenbanken](sql-database-vcore-resource-limits-single-databases.md).
- Die spezifischen Computegrößen und Speichergrößenoptionen für Pools für elastische Datenbanken finden Sie unter [V-Kern-basierte Ressourceneinschränkungen in SQL-Datenbank für Pools für elastische Datenbanken](sql-database-vcore-resource-limits-elastic-pools.md).
