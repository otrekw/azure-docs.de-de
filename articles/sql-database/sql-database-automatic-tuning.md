---
title: Übersicht über die automatische Optimierung
description: Azure SQL-Datenbank analysiert SQL-Abfragen und führt eine automatische Anpassung an die Benutzerworkload durch.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: danimir
ms.author: danil
ms.reviewer: jrasnik, carlrab
ms.date: 03/30/2020
ms.openlocfilehash: 7488fd5f8a572788933856f03bb0ad4351885704
ms.sourcegitcommit: b0ff9c9d760a0426fd1226b909ab943e13ade330
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/01/2020
ms.locfileid: "80518222"
---
# <a name="automatic-tuning-in-azure-sql-database"></a>Automatische Optimierung in Azure SQL-Datenbank

Die automatische Optimierung von Azure SQL-Datenbank sorgt dank einer kontinuierlichen Leistungsoptimierung, die auf künstlicher Intelligenz und maschinellem Lernen basiert, für hohe Leistung und stabile Workloads.

Bei der automatischen Optimierung handelt es sich um einen vollständig verwalteten Leistungsdienst mit integrierten intelligenten Funktionen, der kontinuierlich Abfragen für eine Datenbank überwacht und automatisch deren Leistung verbessert. Dies wird durch eine dynamische Anpassung der Datenbank an die unterschiedlichen Workloads sowie durch die Anwendung von Optimierungsempfehlungen erreicht. Dank künstlicher Intelligenz lernt die automatische Optimierung horizontal von allen Datenbanken in Azure und verbessert dynamisch ihre Optimierungsmaßnahmen. Je länger die automatische Optimierung für eine Datenbank verwendet wird, desto besser ist ihre Leistung.

Die automatische Optimierung von Azure SQL-Datenbank ist vielleicht eines der wichtigsten Features, die Sie aktivieren können, um stabile Datenbankworkloads mit hoher Leistung zu erhalten.

## <a name="what-can-automatic-tuning-do-for-you"></a>Vorteile der automatischen Optimierung

- Automatisierte Optimierung der Leistung von Azure SQL-Datenbanken
- Automatisierte Überprüfung von Leistungszuwächsen
- Automatisches Rollback und automatische Korrektur
- Optimierungsverlauf
- Optimierung von T-SQL-Aktionsskripts für manuelle Bereitstellungen
- Proaktive Überwachung der Workloadleistung
- Funktion für das Aufskalieren von einigen Hunderttausend Datenbanken
- Positive Auswirkung auf DevOps-Ressourcen und die Gesamtkosten

## <a name="safe-reliable-and-proven"></a>Sicher, zuverlässig und bewährt

Die Leistung anspruchsvoller Workloads wird durch Optimierungsmaßnahmen, die auf Datenbanken unter Azure SQL-Datenbank angewendet werden, nicht beeinträchtigt. Bei der Entwicklung des Systems wurde sehr genau darauf geachtet, dass es zu keinerlei Störung der Benutzerworkloads kommt. Empfehlungen der automatisierten Optimierung werden nur bei geringer Auslastung angewendet. Zur Wahrung der Workloadleistung kann das System automatische Optimierungsmaßnahmen auch vorübergehend deaktivieren. In diesem Fall wird im Azure-Portal eine entsprechende Meldung angezeigt. Die automatische Optimierung betrachtet Workloads mit der höchsten Ressourcenpriorität.

Die Mechanismen für die automatische Optimierung sind ausgereift und wurden auf Grundlage von Millionen von Datenbanken perfektioniert, die in Azure ausgeführt werden. Angewendete automatisierte Optimierungsmaßnahmen werden automatisch überprüft, um sicherzustellen, dass sie sich positiv auf die Workloadleistung auswirken. Zurückgestellte Leistungsempfehlungen werden dynamisch erkannt und unverzüglich zurückgesetzt. Anhand des aufgezeichneten Optimierungsverlaufs wird klar ersichtlich, dass die Leistung jeder einzelnen Azure SQL-Datenbank verbessert wurde.

![Wie funktioniert die automatische Optimierung?](./media/sql-database-automatic-tuning/how-does-automatic-tuning-work.png)

Die automatische Optimierung von Azure SQL-Datenbank teilt sich die Kernlogik mit der SQL Server-Engine für die automatische Optimierung. Weitere technische Informationen zum integrierten Intelligenzmechanismus finden Sie unter [Automatic tuning](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning) (Automatische Optimierung).

Im folgenden Video können Sie sich einen Überblick über die Funktionsweise der automatischen Optimierung in typischen Verwendungsszenarien verschaffen:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Improve-Azure-SQL-Database-Performance-with-automatic-tuning/player]

## <a name="enable-automatic-tuning"></a>Aktivieren der automatischen Optimierung

Sie können die [Aktivierung der automatischen Optimierung für Einzel- und Pooldatenbanken im Azure-Portal](sql-database-automatic-tuning-enable.md) oder mit der T-SQL-Anweisung [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-set-options?view=azuresqldb-current) durchführen. Sie aktivieren die automatische Optimierung für Instanzdatenbanken bei einer Bereitstellung einer verwalteten Instanz mit der T-SQL-Anweisung [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-set-options?view=azuresqldb-mi-current).

## <a name="automatic-tuning-options"></a>Optionen für die automatische Optimierung

In Azure SQL-Datenbank stehen folgende Optionen für die automatische Optimierung zur Verfügung:

| Option für die automatische Optimierung | Unterstützung für Einzel- und Pooldatenbanken | Unterstützung der Instanzdatenbank |
| :----------------------------- | ----- | ----- |
| **CREATE INDEX:** Identifiziert Indizes, die die Leistung Ihrer Workload verbessern können, erstellt Indizes und überprüft automatisch, ob die Leistung der Abfragen verbessert wurde. | Ja | Nein |
| **DROP INDEX**: Identifiziert täglich redundante und doppelte Indizes (mit Ausnahme von eindeutigen Indizes) sowie Indizes, die über einen langen Zeitraum hinweg (über 90 Tage) nicht verwendet wurden. Beachten Sie, dass diese Option nicht kompatibel mit Anwendungen ist, die Partitionswechsel und Indexhinweise verwenden. Das Löschen nicht verwendeter Indizes wird bei den Dienstebenen „Premium“ und „Unternehmenskritisch“ nicht unterstützt. | Ja | Nein |
| **FORCE LAST GOOD PLAN** (automatische Plankorrektur): Identifiziert SQL-Abfragen mit einem Ausführungsplan, der langsamer als der vorherige gute Plan ist, und Abfragen, die den letzten bekannten guten Plan anstelle des zurückgestellten Plans verwenden. | Ja | Ja |

### <a name="automatic-tuning-for-single-and-pooled-databases"></a>Automatische Optimierung für Einzel- und Pooldatenbanken

Bei der automatischen Optimierung für Einzel- und Pooldatenbanken werden die Empfehlungen **CREATE INDEX**, **DROP INDEX** und **FORCE LAST GOOD PLAN** des Datenbankratgebers verwendet, um Ihre Datenbankleistung zu optimieren. Weitere Informationen finden Sie unter [Empfehlungen des Datenbankratgebers im Azure-Portal](sql-database-advisor-portal.md), in [PowerShell](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabaserecommendedaction) und in der [REST-API](https://docs.microsoft.com/rest/api/sql/serverautomatictuning).

Sie können die Optimierungsempfehlungen entweder manuell über das Azure-Portal anwenden, oder es der automatischen Optimierung überlassen, diese Empfehlungen autonom für Sie anzuwenden. Wenn Sie es dem System überlassen, Optimierungsempfehlungen autonom anzuwenden, profitieren Sie davon, dass das System automatisch überprüft, ob eine Empfehlung die Workloadleistung tatsächlich positiv beeinflusst. Sollte keine erhebliche Leistungsverbesserung erkannt werden, macht das System die Optimierungsempfehlung automatisch rückgängig. Beachten Sie, dass bei Abfragen, auf die sich Optimierungsempfehlungen auswirken und die nicht häufig ausgeführt werden, die Überprüfungsphase bis zu 72 Stunden dauern kann.

Wenn Sie Optimierungsempfehlungen über T-SQL anwenden, sind die automatische Leistungsüberprüfung und die Mechanismen für die Umkehrung nicht verfügbar. Auf diese Weise angewendete Empfehlungen bleiben zwischen 24 und 48 Stunden lang aktiv und werden in der Liste mit den Optimierungsempfehlungen angezeigt. Danach zieht sie das System automatisch zurück. Falls Sie vorher eine Empfehlung entfernen möchten, können Sie sie über das Azure-Portal verwerfen.

Die Optionen für die automatische Optimierung können unabhängig pro Datenbank aktiviert oder deaktiviert werden. Sie können auch auf SQL-Datenbank-Servern konfiguriert und auf jede Datenbank angewandt werden, die Einstellungen von diesem Server erbt. SQL-Datenbank-Server können Azure-Standardwerte für die Einstellungen der automatischen Optimierung erben. In den Azure-Standardwerten sind derzeit FORCE_LAST_GOOD_PLAN und CREATE_INDEX aktiviert und DROP_INDEX deaktiviert.

> [!IMPORTANT]
> Ab März 2020 werden Änderungen an den Azure-Standardeinstellungen für die automatische Optimierung wie folgt wirksam:
>
> - Neue Azure-Standardwerte werden „FORCE_LAST_GOOD_PLAN“ = aktiviert, „CREATE_INDEX“ = deaktiviert und „DROP_INDEX“ = deaktiviert.
> - Vorhandene Server ohne konfigurierte automatische Optimierungseinstellungen werden automatisch so konfiguriert, dass sie die neuen Azure-Standardeinstellungen ERBEN. Dies gilt für alle Kunden, bei denen sich die automatische Optimierung von Servereinstellungen in einem nicht definierten Zustand befindet.
> - Neu erstellte Server werden automatisch so konfiguriert, dass sie die neuen Azure-Standardeinstellungen ERBEN (im Gegensatz zu früher, wo sich die automatische Optimierungskonfiguration bei der Erstellung neuer Server in einem nicht definierten Zustand befand).

Das Konfigurieren der Optionen für die automatische Optimierung auf dem Server und das Erben von Einstellungen durch die Datenbanken auf dem übergeordneten Server wird für die Konfiguration der automatischen Optimierung empfohlen, weil hierdurch die Verwaltung der Optionen für die automatische Optimierung bei einer großen Anzahl von Datenbanken vereinfacht wird.

Informationen zum Erstellen von E-Mail-Benachrichtigungen für automatische Optimierungsempfehlungen finden Sie unter [E-Mail-Benachrichtigungen zur automatischen Optimierung](sql-database-automatic-tuning-email-notifications.md).

### <a name="automatic-tuning-for-instance-databases"></a>Automatische Optimierung für Instanzdatenbanken

Von der automatischen Optimierung für Instanzdatenbanken bei einer Bereitstellung einer verwalteten Instanz wird nur **FORCE LAST GOOD PLAN** unterstützt. Weitere Informationen zur Konfiguration von automatischen Optimierungsoptionen per T-SQL finden Sie unter [Automatisches Optimieren führt zu einer automatischen Plankorrektur](https://azure.microsoft.com/blog/automatic-tuning-introduces-automatic-plan-correction-and-t-sql-management/) und [Automatische Plankorrektur](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning?view=sql-server-ver15#automatic-plan-correction).

## <a name="next-steps"></a>Nächste Schritte

- Informationen zur integrierten Intelligenz, die bei der automatischen Optimierung zum Einsatz kommt, finden Sie unter [Optimierung von Azure SQL-Datenbanken durch künstliche Intelligenz](https://azure.microsoft.com/blog/artificial-intelligence-tunes-azure-sql-databases/).
- Informationen dazu, wie die automatische Optimierung hinter den Kulissen funktioniert, finden Sie unter [Automatisches Indizieren von Millionen von Datenbanken in Microsoft Azure SQL-Datenbank](https://www.microsoft.com/en-us/research/uploads/prod/2019/02/autoindexing_azuredb.pdf).
