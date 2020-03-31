---
title: Verwaltbarkeit und Überwachung – Abfrageaktivität, Ressourcennutzung
description: Hier erfahren Sie, welche Funktionen zum Verwalten und Überwachen von Azure Synapse Analytics zur Verfügung stehen. Verwenden Sie das Azure-Portal und dynamische Verwaltungssichten (DMVs), um Informationen zu Abfrageaktivität und Ressourcennutzung für Ihre Data Warehouse-Instanz zu erhalten.
services: synapse-analytics
author: kevinvngo
manager: craigg-msft
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 03/11/2020
ms.author: kevin
ms.reviewer: jrasnick
ms.custom: azure-synapse
ms.openlocfilehash: 9d2a9bb0fcaab38f897987a1922e9c95497821d4
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "80350666"
---
# <a name="monitoring-resource-utilization-and-query-activity-in-azure-synapse-analytics"></a>Überwachen der Ressourcennutzung und Abfrageaktivität in Azure Synapse Analytics
Azure Synapse Analytics bietet umfassende Überwachungsfunktionen im Azure-Portal, um Erkenntnisse zu Ihrer Data Warehouse-Workload zu gewinnen. Das Azure-Portal ist das empfohlene Tool zum Überwachen Ihrer Data Warehouse-Instanz, weil es eine konfigurierbare Aufbewahrungsdauer, Warnungen, Empfehlungen und anpassbare Diagramme und Dashboards für Metriken und Protokolle bietet. Das Portal ermöglicht außerdem eine Integration weiterer Azure-Überwachungsdienste – z. B. Azure Monitor (Protokolle) mit Log Analytics, um Ihnen eine umfassende und integrierte Überwachungsoberfläche für Data Warehouse sowie für Ihre gesamte Azure-Analyseplattform zu bieten. In dieser Dokumentation wird beschrieben, welche Überwachungsfunktionen zur Verfügung stehen, um Ihre Analyseplattform mit SQL Analytics zu optimieren und zu verwalten. 

## <a name="resource-utilization"></a>Ressourcenverwendung 
Im Azure-Portal stehen die folgenden Metriken für SQL Analytics zur Verfügung. Diese Metriken werden über [Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/data-collection#metrics) angegeben.


| Metrikname             | BESCHREIBUNG                                                  | Aggregationstyp |
| ----------------------- | ------------------------------------------------------------ | ---------------- |
| CPU-Prozentsatz          | CPU-Auslastung für alle Knoten der Data Warehouse-Instanz      | Durchschnitt, Minimum, Maximum    |
| E/A-Prozentsatz für Daten      | E/A-Auslastung für alle Knoten der Data Warehouse-Instanz       | Durchschnitt, Minimum, Maximum    |
| Arbeitsspeicherprozentsatz       | Arbeitsspeicherauslastung (SQL Server) für alle Knoten der Data Warehouse-Instanz | Durchschnitt, Minimum, Maximum   |
| Aktive Abfragen          | Anzahl aktiver Abfragen, die auf dem System ausgeführt werden             | SUM              |
| Abfragen in Warteschlange          | Anzahl von Abfragen in der Warteschlange, die auf die Ausführung warten          | SUM              |
| Erfolgreiche Verbindungen  | Anzahl von erfolgreichen Datenverbindungen                 | Summe, Anzahl       |
| Verbindungsfehler      | Anzahl von Fehlern bei der Verbindungsherstellung mit Data Warehouse           | Summe, Anzahl       |
| Von der Firewall blockiert     | Anzahl von blockierten Anmeldungen bei Data Warehouse     | Summe, Anzahl       |
| DWU-Grenzwert               | Servicelevelziel für Data Warehouse                | Durchschnitt, Minimum, Maximum    |
| DWU in Prozent          | Höchstwert zwischen CPU-Prozentsatz und E/A-Prozentsatz für Daten        | Durchschnitt, Minimum, Maximum    |
| DWU-Verbrauch                | DWU-Limit × DWU-Prozentsatz                                   | Durchschnitt, Minimum, Maximum    |
| Prozentsatz der Cachetreffer    | (Cachetreffer/Cachefehler) × 100, wobei die Cachetreffer die Summe aller Columnstore-Segmenttreffer im lokalen SSD-Cache und die Cachefehler die Columnstore-Segmentfehler im lokalen SSD-Cache repräsentieren, summiert über alle Knoten | Durchschnitt, Minimum, Maximum    |
| Cacheverwendung in Prozent   | (Cacheverwendung/Cachekapazität) × 100, wobei die Cacheverwendung die Summe aller Bytes im lokalen SSD-Cache für alle Knoten darstellt und die Cachekapazität die Summe der Speicherkapazität im lokalen SSD-Cache für alle Knoten repräsentiert | Durchschnitt, Minimum, Maximum    |
| Lokaler tempdb-Prozentsatz | Lokale tempdb-Auslastung für alle Computeknoten, Werte werden alle fünf Minuten ausgegeben | Durchschnitt, Minimum, Maximum    |
| Datenspeichergröße | Gesamtgröße der Daten, die in die Datenbank geladen wurden. Dies schließt Daten ein, die sich in CCI- und Nicht-CCI-Tabellen befinden, wobei die Größe der Nicht-CCI-Tabellen anhand der Gesamtgröße der Datenbankdatei gemessen wird. | SUM |
| Notfallwiederherstellungsgröße | Gesamtgröße der Geosicherung, die alle 24 Stunden erfolgt | SUM |
| Momentaufnahmen-Speichergröße | Gesamtgröße der Momentaufnahmen, die zur Bereitstellung von Datenbankwiederherstellungspunkten angefertigt werden. Dies umfasst automatisierte und benutzerdefinierte Momentaufnahmen. | SUM |

Beim Anzeigen von Metriken und Festlegen von Warnungen zu berücksichtigende Aspekte:

- Die verwendete DWU stellt nur eine **allgemeine Darstellung der Nutzung** im SQL-Pool dar und soll nicht als umfassender Indikator der Auslastung dienen. Um zu ermitteln, ob zentral hoch- oder herunterskaliert werden soll, sollten Sie alle Faktoren berücksichtigen, die von DWU betroffen sein können, etwa Parallelität, Arbeitsspeicher, tempdb und adaptive Cachekapazität. Es wird empfohlen, [ihre Workload mit verschiedenen DWU-Einstellungen](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-manage-compute-overview#finding-the-right-size-of-data-warehouse-units) auszuführen, um zu ermitteln, was für Ihre Geschäftsziele am besten funktioniert.
- Fehlerhafte und erfolgreiche Verbindungen werden für ein bestimmtes Data Warehouse gemeldet – nicht für den logischen Server.
- Der Prozentsatz des Arbeitsspeichers spiegelt die Auslastung auch dann wider, wenn sich das Data Warehouse im Leerlauf befindet – er gibt nicht die Speichernutzung durch die aktive Workload wieder. Verwenden Sie diese Metrik zusammen mit anderen (tempdb, Gen2-Cache), und verfolgen Sie sie, um eine ganzheitliche Entscheidung darüber zu treffen, ob eine Skalierung auf zusätzliche Cache Kapazität die Workloadleistung entsprechend Ihren Anforderungen steigert.


## <a name="query-activity"></a>Abfrageaktivität
Für eine programmgesteuerte Benutzeroberfläche bei der Überwachung von SQL Analytics über T-SQL bietet der Dienst einen Satz an dynamischen Verwaltungssichten (Dynamic Management Views, DMVs). Diese Sichten sind nützlich für die aktive Problembehandlung und das Identifizieren von Leistungsengpässen in Ihrer Workload.

Eine Liste der von SQL Analytics bereitgestellten DMVs finden Sie in dieser [Dokumentation](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-reference-tsql-system-views#sql-data-warehouse-dynamic-management-views-dmvs). 

## <a name="metrics-and-diagnostics-logging"></a>Protokollierung von Metriken und Diagnosedaten
Sowohl Metriken als auch Protokolle können in Azure Monitor exportiert werden, insbesondere die Komponente [Azure Monitor-Protokolle](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview), und auf sie kann programmgesteuert über [Protokollabfragen](https://docs.microsoft.com/azure/log-analytics/log-analytics-tutorial-viewdata) zugegriffen werden. Die Protokolllatenz für SQL Analytics beträgt ca. 10 – 15 Minuten. Weitere Informationen zu den Faktoren, die sich auf die Wartezeit auswirken, finden Sie in der folgenden Dokumentation.


## <a name="next-steps"></a>Nächste Schritte
Die folgenden Anleitungen beschreiben gängige Szenarios und Anwendungsfälle bei der Überwachung und Verwalten Ihrer Data Warehouse-Instanz:

- [Überwachen Ihrer Data Warehouse-Workload mit DMVs](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-manage-monitor)
