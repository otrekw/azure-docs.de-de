---
title: Überwachen und Verbessern der Leistung
description: Die Azure SQL-Datenbank bietet Leistungstools, mit denen Sie Bereiche identifizieren können, in denen die aktuelle Abfrageleistung verbessert werden kann.
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: danimir
ms.author: danil
ms.reviewer: jrasnik, carlrab
ms.date: 11/14/2019
ms.openlocfilehash: 474c2f4f00374ce785b81fe048e11cb353b3078a
ms.sourcegitcommit: 5cfe977783f02cd045023a1645ac42b8d82223bd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/17/2019
ms.locfileid: "74151212"
---
# <a name="monitor-and-improve-performance"></a>Überwachen und Verbessern der Leistung

Azure SQL-Datenbank stellt intelligente Aktionen zur Optimierung und Empfehlungen bereit, mit denen potenzielle Probleme in Ihrer Datenbank identifiziert werden und Aktionen zur Leistungsverbesserung Ihrer Workload vorgeschlagen werden.

## <a name="performance-tuning-options"></a>Optionen für die Leistungsoptimierung

Leistungsoptimierungsoptionen für Azure SQL-Datenbank finden Sie im Datenbanknavigationsmenü unter „Intelligente Leistung“:

| Option für die Leistungsoptimierung | Unterstützung für Einzel- und Pooldatenbanken | Unterstützung der Instanzdatenbank |
| :----------------------------- | ----- | ----- |
| **Leistungsübersicht**: Ermöglicht die Überwachung aller Leistungsaktivitäten für Ihre Datenbank. | Ja | Nein | 
| **Leistungsempfehlungen**: Zeigt Leistungsempfehlungen an, die zur Verbesserung der Workloadleistung beitragen können. | Ja | Nein | 
| **Statistik zur Abfrageleistung**: Zeigt die Leistung der Abfragen mit dem höchsten Verbrauch der Datenbank an. | Ja | Nein | 
| **Automatische Optimierung**: Verwenden Sie Azure SQL-Datenbank zur automatischen Optimierung der Datenbankleistung. | Ja | Nein | 

## <a name="performance-overview"></a>Leistungsübersicht

Diese Ansicht bietet eine Übersicht über die Leistung der Datenbank und unterstützt Sie bei der Leistungsoptimierung und Fehlerbehebung. 

![Leistungsübersicht für Azure SQL-Datenbank](./media/sql-database-performance/performance-overview-annotated.png)

* Die Kachel **Empfehlungen** bietet eine Aufschlüsselung der Optimierungsempfehlungen für Ihre Datenbank (bei mehreren Empfehlungen werden die wichtigsten drei angezeigt). Durch Klicken auf diese Kachel gelangen Sie zu **[Empfehlungen zur Leistung](#performance-recommendations)** . 
* Die Kachel **Optimierungsaktivität** bietet eine Zusammenfassung der laufenden und abgeschlossenen Optimierungsaktionen für die Datenbank, sodass Sie einen schnellen Überblick über den Verlauf der Optimierungsaktivitäten erhalten. Wenn Sie auf diese Kachel klicken, gelangen Sie zur Ansicht des vollständigen Optimierungsverlaufs für Ihre Datenbank.
* Die Kachel **Automatische Optimierung** zeigt die Konfiguration der [automatischen Optimierung für Ihre Datenbank](sql-database-automatic-tuning-enable.md) an (die Optimierungsaktionen, die automatisch auf die Datenbank angewendet werden). Durch Klicken auf diese Kachel öffnen Sie das Dialogfeld zur Automatisierungskonfiguration.
* Die Kachel **Datenbankabfragen** zeigt die Zusammenfassung der Abfrageleistung für Ihre Datenbank (DTU-Nutzung insgesamt und die Abfragen mit dem größten Ressourcenverbrauch). Durch Klicken auf diese Kachel gelangen Sie zur **[Statistik zur Abfrageleistung](#query-performance-insight)** .

## <a name="performance-recommendations"></a>Empfehlungen zur Leistung

Diese Seite bietet intelligente [Empfehlungen zur Optimierung](sql-database-advisor.md), mit denen Sie die Leistung Ihrer Datenbank verbessern können. Auf dieser Seite finden Sie die folgenden Arten von Empfehlungen:

* Empfehlungen, welche Indizes erstellt oder abgelegt werden sollen.
* Empfehlungen bei Schemaproblemen in der Datenbank.
* Empfehlungen, wenn Abfragen von parametrisierten Abfragen profitieren können.

![Leistungsempfehlungen für Azure SQL-Datenbank](./media/sql-database-performance/performance-recommendations-annotated.png)

Sie können auch den vollständigen Verlauf der Aktionen zur Leistungsoptimierung suchen, die in der Vergangenheit angewendet wurden.

Im Artikel [Find and apply performance recommendations (Suchen und Anwenden von Empfehlungen zur Leistung)](sql-database-advisor-portal.md) finden Sie Informationen zur Anwendung von Empfehlungen zur Leistung.

## <a name="query-performance-insight"></a>Query Performance Insight

Mithilfe von [Query Performance Insight](sql-database-query-performance.md) können Sie den Zeitaufwand für die Problembehandlung der Datenbankleistung reduzieren. Dieses Tool stellt Folgendes bereit:

* Tiefere Einblicke in den Verbrauch von Datenbankressourcen (DTU). 
* Informationen zu Abfragen mit der höchsten CPU-Auslastung, für die unter Umständen eine Leistungssteigerung erzielt werden kann. 
* Die Möglichkeit zum Durchführen eines Drilldowns in die Details einer Abfrage.​ 

  ![Statistik zur Abfrageleistung für Azure SQL-Datenbank](./media/sql-database-performance/query-performance-insights-annotated.png)

Weitere Informationen zu dieser Seite finden Sie im Artikel **[How to use Query Performance Insight (Anleitung zur Verwendung der Statistik zur Abfrageleistung)](sql-database-query-performance.md)** .

## <a name="automatic-tuning"></a>Automatische Optimierung

Azure SQL-Datenbanken können automatisch die Leistung der Datenbank optimieren, indem sie [Empfehlungen zur Leistung](sql-database-advisor.md) anwenden. Lesen Sie zum Aktivieren den Artikel [Aktivieren der automatischen Optimierung](sql-database-automatic-tuning-enable.md).

  ![Automatische Optimierung für Azure SQL-Datenbank](./media/sql-database-performance/automatic-tuning-annotated.png)

Weitere Informationen finden Sie unter [Automatische Optimierung](sql-database-automatic-tuning.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Leitfaden zur Azure SQL-Datenbankleistung für Einzeldatenbanken](sql-database-performance-guidance.md)
* [Wann sollte ein Pool für elastische Datenbanken verwendet werden?](sql-database-elastic-pool-guidance.md)
