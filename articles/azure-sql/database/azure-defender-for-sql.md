---
title: Azure Defender für SQL
description: Hier erhalten Sie Informationen zu den Funktionen zur Verwaltung von Datenbankrisiken und die Erkennung ungewöhnlicher Aktivitäten, die eine Bedrohung für Ihre Datenbank in Azure SQL-Datenbank, Azure SQL Managed Instance oder Azure Synapse darstellen können.
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
ms.date: 09/21/2020
ms.openlocfilehash: b789cd423d2260ce709a02ffb3ac5ea500997609
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90930594"
---
# <a name="azure-defender-for-sql"></a>Azure Defender für SQL
[!INCLUDE[appliesto-sqldb-sqlmi-asa](../includes/appliesto-sqldb-sqlmi-asa.md)]


Azure Defender für SQL ist ein einheitliches Paket für erweiterte SQL-Sicherheitsfunktionen. Azure Defender ist für Azure SQL-Datenbank, Azure SQL Managed Instance und Azure Synapse Analytics verfügbar. Dazu zählen die Funktionen zur Ermittlung und Klassifizierung sensibler Daten, Ermittlung und Verringerung potenzieller Datenbankschwachstellen und Erkennung ungewöhnlicher Aktivitäten, die eine Bedrohung für Ihre Datenbank darstellen können. Es bietet einen einzelnen Anlaufpunkt zum Aktivieren und Verwalten dieser Funktionen.

## <a name="overview"></a>Übersicht

Azure Defender stellt eine Reihe erweiterter SQL-Sicherheitsfunktionen bereit, darunter SQL-Risikoanalyse und Advanced Threat Protection.
- Die [Sicherheitsrisikobewertung](sql-vulnerability-assessment.md) ist ein einfach zu konfigurierender Dienst, mit dem potenzielle Schwachstellen in der Datenbank ermittelt, nachverfolgt und behoben werden können. Er bietet Einblicke in Ihren Sicherheitsstatus, enthält umsetzbare Schritte zum Beheben von Sicherheitsproblemen und verbessert Ihre Datenbanksicherheit.
- [Advanced Threat Protection](threat-detection-overview.md) erkennt Anomalien bei Aktivitäten, die auf ungewöhnliche und potenziell schädliche Versuche hinweisen, auf Ihre Datenbank zuzugreifen oder diese zu nutzen. Sie überwacht Ihre Datenbank fortlaufend auf verdächtige Aktivitäten und bietet sofortige Sicherheitswarnungen zu potenziellen Sicherheitsrisiken, Angriffen durch Einschleusung von Azure SQL-Befehlen und ungewöhnlichen Datenbankzugriffsmustern. Die Warnungen von Advanced Threat Protection enthalten Details zu verdächtigen Aktivitäten sowie Empfehlungen, wie die Bedrohung untersucht und abgewendet werden kann.

Sie müssen Azure Defender für SQL nur einmal aktivieren, um alle diese enthaltenen Funktionen zu aktivieren. Mit nur einem Klick können Sie Azure Defender für alle Datenbanken auf Ihrem [Server](logical-servers.md) in Azure oder Azure SQL Managed Instance aktivieren. Um Azure Defender-Einstellungen aktivieren oder verwalten zu können, müssen Sie der Rolle [SQL-Sicherheits-Manager](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#sql-security-manager) oder einer der Datenbank- oder Serveradministratorrollen angehören.

Weitere Informationen zu den Preisen für Azure Defender für SQL finden Sie auf der Seite [Azure Security Center – Preise](https://azure.microsoft.com/pricing/details/security-center/).

## <a name="getting-started-with-azure-defender"></a>Erste Schritte mit Azure Defender

Die folgenden Schritte bilden den Einstieg in Azure Defender.

## <a name="1-enable-azure-defender"></a>1. Aktivieren von Azure Defender

Sie können über das [Azure-Portal](https://portal.azure.com) auf Azure Defender zugreifen. Aktivieren Sie Azure Defender, indem Sie unterhalb der Überschrift **Sicherheit** für Ihren Server oder Ihre verwaltete Instanz zu **Security Center** navigieren.

> [!NOTE]
> Ein Speicherkonto wird automatisch erstellt und konfiguriert, um die Speicherkontoergebnisse der **Sicherheitsrisikobewertung** zu speichern. Wenn Sie Azure Defender bereits für einen anderen Server in derselben Ressourcengruppe und Region aktiviert haben, wird das vorhandene Speicherkonto verwendet.
>
> Die Kosten von Azure Defender orientieren sich am Azure Security Center-Standardtarif pro Knoten, wobei ein Knoten dem gesamten Server oder der verwalteten Instanz entspricht. Sie bezahlen daher nur einmal für den Schutz aller Datenbanken auf dem Server oder in der verwalteten Instanz durch Azure Defender. Sie können Azure Defender anfänglich mit einer kostenlosen Testversion testen.

## <a name="2-start-tracking-vulnerabilities-and-investigating-threat-alerts"></a>2. Beginnen mit dem Nachverfolgen von Sicherheitsrisiken und Untersuchen von Bedrohungswarnungen

Klicken Sie auf die Karte **Sicherheitsrisikobewertung**, um Sicherheitsrisikoscans und -berichte anzuzeigen und zu verwalten und Ihren Sicherheitsstatus zu verfolgen. Wenn Sie Sicherheitswarnungen empfangen haben, klicken Sie auf die Karte **Advanced Threat Protection**, um Details zu den Warnungen anzuzeigen und einen konsolidierten Bericht für alle Warnungen in Ihrem Azure-Abonnement über die Seite mit den Sicherheitswarnungen im Azure Security Center abzurufen.

## <a name="3-manage-azure-defender-settings"></a>3. Verwalten von Azure Defender-Einstellungen

Um die Azure Defender-Einstellungen anzuzeigen und zu verwalten, navigieren Sie unterhalb der Überschrift **Sicherheit** für Ihren Server oder Ihre verwaltete Instanz zu **Security Center**. Auf dieser Seite können Sie Azure Defender aktivieren oder deaktivieren und die Einstellungen für die Sicherheitsrisikoanalyse und Advanced Threat Protection für Ihren gesamten Server oder Ihre verwaltete Instanz ändern.

## <a name="4-manage-azure-defender-settings-for-a-database"></a>4. Verwalten von Azure Defender-Einstellungen für eine Datenbank

Um die Azure Defender-Einstellungen für eine bestimmte Datenbank außer Kraft zu setzen, aktivieren Sie das Kontrollkästchen **Enable Azure Defender for SQL at the database level** (Azure Defender für SQL auf Datenbankebene aktivieren). Verwenden Sie diese Option nur, wenn eine besondere Anforderung zum Empfangen von separaten Advanced Threat Protection-Warnungen oder Risikoanalyseergebnissen für diese Datenbank vorliegt, die anstelle der Warnungen für alle Datenbanken auf dem Server oder in der verwalteten Instanz oder zusätzlich zu diesen empfangen werden müssen.

Sobald das Kontrollkästchen aktiviert wurde, können Sie die relevanten Einstellungen für diese Datenbank konfigurieren.

Sie können auch über den Azure Defender-Datenbankbereich auf die Azure Defender für SQL-Einstellungen für Ihren Server oder Ihre verwaltete Instanz zugreifen. Klicken Sie im Hauptbereich von Azure Defender auf **Einstellungen** und dann auf **View Azure Defender for SQL server settings** (Einstellungen für Azure Defender für SQL anzeigen).

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zur [Sicherheitsrisikobewertung](sql-vulnerability-assessment.md)
- Weitere Informationen zu [Advanced Threat Protection](threat-detection-configure.md)
- Weitere Informationen zu [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro)
