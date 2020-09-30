---
title: 'Azure Defender für SQL: Vorteile und Features'
description: Enthält eine Beschreibung der Vorteile und Features von Azure Defender für SQL.
author: memildin
ms.author: memildin
ms.date: 9/22/2020
ms.topic: overview
ms.service: security-center
ms.custom: references_regions
manager: rkarlin
ms.openlocfilehash: 328a565dbb22a13c71a7001d43941e7be062dff9
ms.sourcegitcommit: 3792cf7efc12e357f0e3b65638ea7673651db6e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/29/2020
ms.locfileid: "91449049"
---
# <a name="introduction-to-azure-defender-for-sql"></a>Einführung in Azure Defender für SQL

Azure Defender for SQL verfügt über zwei Azure Defender-Pläne als Erweiterung des [Datensicherheitspakets](../azure-sql/database/advanced-data-security.md) von Azure Security Center, um Ihre Datenbanken und die zugehörigen Daten unabhängig vom Speicherort zu schützen. 

## <a name="availability"></a>Verfügbarkeit

|Aspekt|Details|
|----|:----|
|Status des Release:|**Azure Defender für Azure SQL-Datenbank-Server**: Allgemein verfügbar (GA)<br>**Azure Defender für SQL-Server auf Computern**: Vorschauversion|
|Preise:|Die beiden Pläne, die **Azure Defender für SQL** bilden, werden gemäß den Angaben auf der [Preisseite](security-center-pricing.md) berechnet.|
|Geschützte SQL-Versionen:|Azure SQL-Datenbank <br>Verwaltete Azure SQL-Instanz<br>Azure Synapse Analytics (ehemals SQL DW)<br>SQL Server (alle unterstützten Versionen)|
|Clouds:|![Ja](./media/icons/yes-icon.png) Kommerzielle Clouds<br>![Ja](./media/icons/yes-icon.png) US Gov<br>![Nein](./media/icons/no-icon.png) China Gov/andere Gov-Clouds|
|||

## <a name="what-does-azure-defender-for-sql-protect"></a>Was wird mit Azure Defender für SQL geschützt?

**Azure Defender für SQL** verfügt über zwei separate Azure Defender-Pläne:

- Mit **Azure Defender für Azure SQL-Datenbank-Server** wird Folgendes geschützt:
  - [Azure SQL-Datenbank](../azure-sql/database/sql-database-paas-overview.md)
  - [Verwaltete Azure SQL-Datenbank-Instanz](../azure-sql/managed-instance/sql-managed-instance-paas-overview.md)
  - [Azure Synapse Analytics](../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is.md)

- Mit **Azure Defender für SQL-Server auf Computern (Vorschau)** wird der Schutz für Ihre nativen Azure SQL Server-Instanzen erweitert, um Hybridumgebungen vollständig zu unterstützen und SQL Server-Instanzen (alle unterstützten Versionen), die in Azure, anderen Cloudumgebungen oder sogar auf lokalen Computern gehostet werden, schützen zu können.


## <a name="what-are-the-benefits-of-azure-defender-for-sql"></a>Welche Vorteile hat die Nutzung von Azure Defender für SQL?

Diese beiden Pläne verfügen über Funktionen zur Ermittlung und Verringerung potenzieller Datenbankschwachstellen und Erkennung ungewöhnlicher Aktivitäten, die Bedrohungen für Ihre Datenbanken darstellen können:

- [Sicherheitsrisikobewertung](../azure-sql/database/sql-vulnerability-assessment.md): der Scandienst, mit dem potenzielle Schwachstellen in der Datenbank ermittelt, nachverfolgt und behoben werden. Bewertungsscans bieten einen Überblick über den Sicherheitszustand Ihrer SQL-Computer und Einzelheiten zu allen Sicherheitsergebnissen.

- [Advanced Threat Protection](../azure-sql/database/threat-detection-overview.md): Der Erkennungsdienst, der Ihre SQL Server-Instanzen kontinuierlich auf Bedrohungen wie die Einschleusung von SQL-Befehlen, Brute-Force-Angriffe und Berechtigungsmissbrauch überwacht. Dieser Dienst bietet aktionsorientierte Sicherheitswarnungen in Azure Security Center mit Details zur verdächtigen Aktivität, Anleitungen zum Minimieren der Bedrohungen und Optionen für die Fortsetzung ihrer Untersuchungen mit Azure Sentinel.


## <a name="what-kind-of-alerts-does-azure-defender-for-sql-provide"></a>Über welche Art von Warnungen verfügt Azure Defender für SQL?

Sicherheitswarnungen werden ausgelöst, wenn Folgendes vorliegt:

- **Potenzielle Angriffe durch Einschleusung von SQL-Befehlen**: Hierzu gehören auch Sicherheitsrisiken, die erkannt werden, wenn von Anwendungen in der Datenbank eine fehlerhafte SQL-Anweisung generiert wird.
- **Anomale Muster für Datenbankzugriff und -abfrage**: Ein Beispiel hierfür sind eine ungewöhnlich hohe Anzahl von fehlgeschlagenen Anmeldeversuchen mit unterschiedlichen Anmeldeinformationen (Brute-Force-Angriff).
- **Verdächtige Datenbankaktivität**: Dies kann beispielsweise eine Änderung des Exportspeicherziels für einen SQL-Import- und -Exportvorgang sein.

Warnungen enthalten Details zum Incident, durch den sie ausgelöst wurden, sowie Empfehlungen zur Untersuchung und Eindämmung von Bedrohungen.



## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie mehr über Azure Defender für SQL erfahren.

Weitere Informationen finden Sie in den folgenden Artikeln: 

- [Aktivieren von Azure Defender für SQL-Servern auf Computern](defender-for-sql-usage.md)
- [Aktivieren von Azure Defender für SQL-Datenbank-Servern](../azure-sql/database/advanced-data-security.md)
- [Liste mit Azure Defender-Warnungen für SQL](alerts-reference.md#alerts-sql-db-and-warehouse)