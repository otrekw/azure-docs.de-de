---
title: Advanced Data Security
description: Erfahren Sie etwas über die Funktionen zur Ermittlung und Klassifizierung vertraulicher Daten, die Verwaltung von Datenbankrisiken und die Erkennung ungewöhnlicher Aktivitäten, die eine Bedrohung für Ihre Datenbank in Azure SQL-Datenbank, Azure SQL Managed Instance oder Azure Synapse darstellen können.
services: sql-database
ms.service: sql-db-mi
ms.subservice: security
ms.devlang: ''
ms.custom: sqldbrb=2
ms.topic: conceptual
ms.author: memildin
manager: rkarlin
author: memildin
ms.reviewer: vanto
ms.date: 04/23/2020
ms.openlocfilehash: 53765ee97f0f253db4df4ecca3c1c90d6068fb07
ms.sourcegitcommit: 93462ccb4dd178ec81115f50455fbad2fa1d79ce
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/06/2020
ms.locfileid: "85983993"
---
# <a name="advanced-data-security"></a>Advanced Data Security
[!INCLUDE[appliesto-sqldb-sqlmi-asa](../includes/appliesto-sqldb-sqlmi-asa.md)]


Advanced Data Security (ADS) ist ein einheitliches Paket für erweiterte SQL-Sicherheitsfunktionen. ADS ist für Azure SQL-Datenbank, Azure SQL Managed Instance und Azure Synapse Analytics verfügbar. Dazu zählen die Funktionen zur Ermittlung und Klassifizierung sensibler Daten, Ermittlung und Verringerung potenzieller Datenbankschwachstellen und Erkennung ungewöhnlicher Aktivitäten, die eine Bedrohung für Ihre Datenbank darstellen können. Es bietet einen einzelnen Anlaufpunkt zum Aktivieren und Verwalten dieser Funktionen.

## <a name="overview"></a>Übersicht

ADS stellt eine Reihe erweiterter SQL-Sicherheitsfunktionen bereit, darunter Datenermittlung und -klassifizierung, SQL-Risikoanalyse und Advanced Threat Protection.
- [Datenermittlung und -klassifizierung](data-discovery-and-classification-overview.md) bietet in Azure SQL-Datenbank, Azure SQL Managed Instance und Azure Synapse integrierte Funktionen zum Ermitteln, Klassifizieren, Bezeichnen und Melden vertraulicher Daten in Ihren Datenbanken. Das Feature kann Einblicke in den Zustand Ihrer Datenbankklassifizierung bereitstellen und den Zugriff auf sensible Daten innerhalb der Datenbank und außerhalb ihrer Grenzen verfolgen.
- Die [Sicherheitsrisikobewertung](sql-vulnerability-assessment.md) ist ein einfach zu konfigurierender Dienst, mit dem potenzielle Schwachstellen in der Datenbank ermittelt, nachverfolgt und behoben werden können. Er bietet Einblicke in Ihren Sicherheitsstatus, enthält umsetzbare Schritte zum Beheben von Sicherheitsproblemen und verbessert Ihre Datenbanksicherheit.
- [Advanced Threat Protection](threat-detection-overview.md) erkennt Anomalien bei Aktivitäten, die auf ungewöhnliche und potenziell schädliche Versuche hinweisen, auf Ihre Datenbank zuzugreifen oder diese zu nutzen. Sie überwacht Ihre Datenbank fortlaufend auf verdächtige Aktivitäten und bietet sofortige Sicherheitswarnungen zu potenziellen Sicherheitsrisiken, Angriffen durch Einschleusung von Azure SQL-Befehlen und ungewöhnlichen Datenbankzugriffsmustern. Die Warnungen von Advanced Threat Protection enthalten Details zu verdächtigen Aktivitäten sowie Empfehlungen, wie die Bedrohung untersucht und abgewendet werden kann.

Sie müssen Advanced Data Security nur einmal aktivieren, um alle diese enthaltenen Features zu aktivieren. Mit nur einem Klick können Sie ADS für alle Datenbanken auf Ihrem [Server](logical-servers.md) in Azure oder Azure SQL Managed Instance aktivieren. Um ADS-Einstellungen aktivieren oder verwalten zu können, müssen Sie der Rolle [SQL-Sicherheits-Manager](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#sql-security-manager) oder einer der Datenbank- oder Serveradministratorrollen angehören.

Die Preise für ADS richten sich nach dem Standardtarif für Azure Security Center, wobei jeder geschützte Server bzw. jede verwaltete Instanz als ein Knoten abgerechnet wird. Für neue zu schützende Ressourcen kann der Standardtarif von Security Center kostenlos getestet werden. Weitere Informationen finden Sie auf der Seite mit den [Preisen für Azure Security Center](https://azure.microsoft.com/pricing/details/security-center/).

## <a name="getting-started-with-ads"></a>Erste Schritte mit ADS

Die folgenden Schritte bilden den Einstieg in ADS.

## <a name="1-enable-ads"></a>1. Aktivieren von ADS

Aktivieren Sie ADS, indem Sie unterhalb der Überschrift **Sicherheit** für Ihren Server oder Ihre verwaltete Instanz zu **Advanced Data Security** navigieren.

> [!NOTE]
> Ein Speicherkonto wird automatisch erstellt und konfiguriert, um die Speicherkontoergebnisse der **Sicherheitsrisikobewertung** zu speichern. Wenn Sie ADS bereits für einen anderen Server in derselben Ressourcengruppe und Region aktiviert haben, wird das vorhandene Speicherkonto verwendet.

![Aktivieren von ADS](./media/advanced-data-security/enable_ads.png)

> [!NOTE]
> Die Kosten von ADS orientieren sich am Azure Security Center-Standardtarif pro Knoten, wobei ein Knoten dem gesamten Server oder der verwalteten Instanz entspricht. Sie bezahlen daher nur einmal für den Schutz aller Datenbanken auf dem Server oder in der verwalteten Instanz durch ADS. Sie können ADS anfänglich mit einer kostenlosen Testversion testen.

## <a name="2-start-classifying-data-tracking-vulnerabilities-and-investigating-threat-alerts"></a>2. Beginnen mit dem Klassifizieren von Daten, Nachverfolgen von Sicherheitsrisiken und Untersuchen von Bedrohungswarnungen

Klicken Sie auf die Karte **Datenermittlung und -klassifizierung**, um empfohlene sensible Spalten anzuzeigen und Ihre Daten mit permanenten Sensibilitätsbezeichnungen zu klassifizieren. Klicken Sie auf die Karte **Sicherheitsrisikobewertung**, um Sicherheitsrisikoscans und -berichte anzuzeigen und zu verwalten und Ihren Sicherheitsstatus zu verfolgen. Wenn Sie Sicherheitswarnungen empfangen haben, klicken Sie auf die Karte **Advanced Threat Protection**, um Details zu den Warnungen anzuzeigen und einen konsolidierten Bericht für alle Warnungen in Ihrem Azure-Abonnement über die Seite mit den Sicherheitswarnungen im Azure Security Center abzurufen.

## <a name="3-manage-ads-settings"></a>3. Verwalten von ADS-Einstellungen

Um die ADS-Einstellungen anzuzeigen und zu verwalten, navigieren Sie unterhalb der Überschrift **Sicherheit** für Ihren Server oder Ihre verwaltete Instanz zu **Advanced Data Security**. Auf dieser Seite können Sie ADS aktivieren oder deaktivieren und die Einstellungen für die Sicherheitsrisikoanalyse und Advanced Threat Protection für Ihren gesamten Server oder Ihre verwaltete Instanz ändern.

![Servereinstellungen](./media/advanced-data-security/server_settings.png)

## <a name="4-manage-ads-settings-for-a-database"></a>4. Verwalten von ADS-Einstellungen für eine Datenbank

Um die ADS-Einstellungen für eine bestimmte Datenbank außer Kraft zu setzen, aktivieren Sie das Kontrollkästchen **Advanced Data Security auf Datenbankebene aktivieren**. Verwenden Sie diese Option nur, wenn eine besondere Anforderung zum Empfangen von separaten Advanced Threat Protection-Warnungen oder Risikoanalyseergebnissen für diese Datenbank vorliegt, die anstelle der Warnungen für alle Datenbanken auf dem Server oder in der verwalteten Instanz oder zusätzlich zu diesen empfangen werden müssen.

Sobald das Kontrollkästchen aktiviert wurde, können Sie die relevanten Einstellungen für diese Datenbank konfigurieren.

![Datenbank- und Advanced Threat Protection-Einstellungen](./media/advanced-data-security/database_threat_detection_settings.png)

Sie können auch über den ADS-Datenbankbereich auf die Advanced Data Security-Einstellungen für Ihren Server oder Ihre verwaltete Instanz zugreifen. Klicken Sie im ADS-Bereich auf **Einstellungen**, und klicken Sie dann auf **Advanced Data Security-Servereinstellungen anzeigen**.

![Datenbankeinstellungen](./media/advanced-data-security/database_settings.png)

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zur [Datenermittlung und -klassifizierung](data-discovery-and-classification-overview.md)
- Weitere Informationen zur [Sicherheitsrisikobewertung](sql-vulnerability-assessment.md)
- Weitere Informationen zu [Advanced Threat Protection](threat-detection-configure.md)
- Weitere Informationen zu [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro)
