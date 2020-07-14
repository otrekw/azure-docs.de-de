---
title: Erweiterter Schutz vor Bedrohungen
titleSuffix: Azure SQL Database, SQL Managed Instance, & Azure Synapse Analytics
description: Advanced Threat Protection erkennt anomale Datenbankaktivitäten, die auf potenzielle Sicherheitsrisiken in Azure SQL-Datenbank, Azure SQL Managed Instance und Azure Synapse Analytics hindeuten.
services: sql-database
ms.service: sql-db-mi
ms.subservice: security
ms.devlang: ''
ms.custom: sqldbrb=2
ms.topic: conceptual
author: monhaber
ms.author: ronmat
ms.reviewer: vanto, carlrab
ms.date: 02/05/2020
tags: azure-synapse
ms.openlocfilehash: 822d4267d5b163ddda50302e2caea5c3ade3b7c4
ms.sourcegitcommit: 93462ccb4dd178ec81115f50455fbad2fa1d79ce
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/06/2020
ms.locfileid: "85985370"
---
# <a name="advanced-threat-protection-for-azure-sql-database-sql-managed-instance-and-azure-synapse-analytics"></a>Advanced Threat Protection für Azure SQL-Datenbank, SQL Managed Instance und Azure Synapse Analytics
[!INCLUDE[appliesto-sqldb-sqlmi-asa](../includes/appliesto-sqldb-sqlmi-asa.md)]

Advanced Threat Protection für [Azure SQL-Datenbank](sql-database-paas-overview.md), [Azure SQL Managed Instance](../managed-instance/sql-managed-instance-paas-overview.md) und [Azure Synapse Analytics](../../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is.md) erkennt anomale Aktivitäten, die auf ungewöhnliche und möglicherweise schädigende Zugriffs- oder Exploitversuche auf Datenbanken hinweisen.

Advanced Threat Protection ist Teil des Pakets [Advanced Data Security](advanced-data-security.md). Dabei handelt es sich um ein vereinheitlichtes Paket für erweiterte SQL-Sicherheitsfunktionen. Der Zugriff auf Advanced Threat Protection und dessen Verwaltung sind über das zentrale SQL ADS-Portal möglich.

## <a name="overview"></a>Übersicht

Dank der neuen Sicherheitsebene von Advanced Threat Protection können Kunden potenzielle Bedrohungen erkennen und darauf reagieren, sobald diese auftreten, indem bei anomalen Aktivitäten Sicherheitswarnungen ausgegeben werden. Benutzer erhalten Warnungen zu verdächtigen Datenbankaktivitäten, potenziellen Sicherheitsrisiken sowie Angriffen durch Einschleusung von SQL-Befehlen und anomalen Datenbankzugriffs- und -abfragemustern. Advanced Threat Protection integriert Warnungen in [Azure Security Center](https://azure.microsoft.com/services/security-center/). Dabei werden Detailinformationen zu verdächtigen Aktivitäten sowie Empfehlungen bereitgestellt, wie die Bedrohung untersucht und entschärft werden kann. Advanced Threat Protection vereinfacht den Umgang mit potenziellen Bedrohungen der Datenbank, ohne das Fachwissen eines Sicherheitsexperten besitzen oder komplexe Sicherheitsüberwachungssysteme verwalten zu müssen.

Zur vollständigen Untersuchung empfiehlt es sich, die Datenbanküberwachung zu aktivieren, bei der Datenbankereignisse in ein Überwachungsprotokoll in Ihrem Azure Storage-Konto geschrieben werden.  Informationen zum Aktivieren der Überwachung finden Sie unter [Überwachung für Azure SQL-Datenbank und Azure-Synapse](../../azure-sql/database/auditing-overview.md) oder [Überwachung für Azure SQL Managed Instance](../managed-instance/auditing-configure.md).

## <a name="alerts"></a>Alerts

Advanced Threat Protection für Azure SQL-Datenbank erkennt Anomalien bei Aktivitäten, die auf ungewöhnliche und potenziell schädliche Versuche hinweisen, auf Ihre Datenbanken zuzugreifen oder diese zu nutzen. Eine Liste der Warnungen für Azure SQL-Datenbank finden Sie in den [Warnungen für SQL-Datenbank und SQL Data Warehouse im Azure Security Center](https://docs.microsoft.com/azure/security-center/alerts-reference#alerts-sql-db-and-warehouse).

## <a name="explore-detection-of-a-suspicious-event"></a>Erkennen eines verdächtigen Ereignisses

Bei Erkennung anormaler Datenbankaktivitäten erhalten Sie eine E-Mail-Benachrichtigung. Die E-Mail enthält Informationen zum verdächtigen Sicherheitsereignis (dazu gehören Art der anomalen Aktivitäten, Datenbankname, Servername, Anwendungsname und Zeit des Ereignisses). Darüber hinaus enthält die E-Mail Angaben zu möglichen Ursachen und empfohlenen Maßnahmen zur Untersuchung und Abwehr der potenziellen Bedrohung für die Datenbank.

![Bericht zu anomalen Aktivitäten](./media/threat-detection-overview/anomalous_activity_report.png)

1. Klicken Sie in der E-Mail auf den Link **View recent SQL alerts** (Aktuelle SQL-Warnungen anzeigen), um das Azure-Portal zu starten und die Azure Security Center-Seite für Warnungen zu öffnen, auf der eine Übersicht über die aktiven Bedrohungen angezeigt wird, die in der Datenbank erkannt wurden.

   ![Aktivitätsbedrohungen](./media/threat-detection-overview/active_threats.png)

1. Klicken Sie auf eine bestimmte Warnung, um weitere Details und Aktionen zum Untersuchen der entsprechenden Bedrohung und Abwehren zukünftiger Bedrohungen anzuzeigen.

   Beispielsweise ist die Einschleusung von SQL-Befehlen das häufigste Sicherheitsproblem für Webanwendungen im Internet, das für Angriffe auf datengesteuerte Anwendungen verwendet wird. Die Angreifer nutzen Sicherheitslücken der Anwendung, um böswillige SQL-Anweisungen in Eingabefelder der Anwendung einzuschleusen, sodass Daten in der Datenbank manipuliert oder verändert werden können. Bei Warnungen in Bezug auf die Einschleusung von SQL-Befehlen schließen die Details der Warnung die anfällige missbräuchlich genutzte SQL-Anweisung ein.

   ![Spezifische Warnung](./media/threat-detection-overview/specific_alert.png)

## <a name="explore-alerts-in-the-azure-portal"></a>Warnungen im Azure-Portal

Warnungen von Advanced Threat Protection sind in [Azure Security Center](https://azure.microsoft.com/services/security-center/) integriert. Auf Livekacheln von SQL Advanced Threat Protection innerhalb der Datenbank und auf den Blättern von SQL ADS im Azure-Portal lässt sich der Status aktiver Bedrohungen nachverfolgen.

Klicken Sie auf **Advanced Threat Protection-Warnung**, um die Azure Security Center-Seite für Warnungen zu öffnen und eine Übersicht über die aktiven SQL-Bedrohungen zu erhalten, die in der Datenbank erkannt wurden.

   ![Advanced Threat Protection-Warnung](./media/threat-detection-overview/threat_detection_alert.png)

   ![Advanced Threat Protection-Warnung2](./media/threat-detection-overview/threat_detection_alert_atp.png)

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen finden Sie unter [Advanced Threat Protection in Azure SQL-Datenbank und Azure Synapse](threat-detection-configure.md).
- Weitere Informationen finden Sie unter [Advanced Threat Protection in Azure SQL Managed Instance](../managed-instance/threat-detection-configure.md).
- Erfahren Sie mehr über [Advanced Data Security](advanced-data-security.md).
- Weitere Informationen zu [Überwachung von Azure SQL-Datenbank](../../azure-sql/database/auditing-overview.md)
- Weitere Informationen zu [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro).
- Weitere Informationen zu den Preisen finden Sie unter [Azure SQL-Datenbank: Preise](https://azure.microsoft.com/pricing/details/sql-database/).  
