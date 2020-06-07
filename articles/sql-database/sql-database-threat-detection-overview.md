---
title: Erweiterter Schutz vor Bedrohungen
description: Advanced Threat Protection erkennt anomale Datenbankaktivitäten, die auf potenzielle Sicherheitsrisiken in Azure SQL-Datenbank hindeuten.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: monhaber
ms.author: ronmat
ms.reviewer: vanto, carlrab
ms.date: 02/05/2020
tags: azure-synapse
ms.openlocfilehash: 0b231adafabff8414b1cf742e1441e938a3fa212
ms.sourcegitcommit: 1f25aa993c38b37472cf8a0359bc6f0bf97b6784
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2020
ms.locfileid: "83848193"
---
# <a name="advanced-threat-protection-for-azure-sql-database"></a>Advanced Threat Protection für Azure SQL-Datenbank

Advanced Threat Protection für [Azure SQL-Datenbank](sql-database-technical-overview.md) und [Azure Synapse Analytics](../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is.md) erkennt anomale Aktivitäten, die auf ungewöhnliche und möglicherweise schädigende Versuche hinweisen, auf Datenbanken zuzugreifen oder diese zu auszunutzen.

Advanced Threat Protection ist Teil des Angebots [Advanced Data Security](sql-database-advanced-data-security.md) (ADS), bei dem es sich um ein vereinheitlichtes Paket für erweiterte SQL-Sicherheitsfunktionen handelt. Der Zugriff auf Advanced Threat Protection und dessen Verwaltung sind über das zentrale SQL ADS-Portal möglich.

> [!NOTE]
> Dieses Thema gilt für Azure SQL-Server sowie für Datenbanken von SQL-Datenbank und Azure Synapse, die auf dem Azure SQL-Server erstellt werden. Der Einfachheit halber wird „SQL-Datenbank“ verwendet, wenn sowohl auf SQL-Datenbank als auch auf Azure Synapse verwiesen wird.

## <a name="what-is-advanced-threat-protection"></a>Was ist Advanced Threat Protection?

 Dank der neuen Sicherheitsebene von Advanced Threat Protection können Kunden potenzielle Bedrohungen erkennen und darauf reagieren, sobald diese auftreten, indem bei anomalen Aktivitäten Sicherheitswarnungen ausgegeben werden. Benutzer erhalten Warnungen zu verdächtigen Datenbankaktivitäten, potenziellen Sicherheitsrisiken sowie Angriffen durch Einschleusung von SQL-Befehlen und anomalen Datenbankzugriffs- und -abfragemustern. Advanced Threat Protection integriert Warnungen in [Azure Security Center](https://azure.microsoft.com/services/security-center/). Dabei werden Detailinformationen zu verdächtigen Aktivitäten sowie Empfehlungen bereitgestellt, wie die Bedrohung untersucht und entschärft werden kann. Advanced Threat Protection vereinfacht den Umgang mit potenziellen Bedrohungen der Datenbank, ohne das Fachwissen eines Sicherheitsexperten besitzen oder komplexe Sicherheitsüberwachungssysteme verwalten zu müssen.

Zur vollständigen Untersuchung empfiehlt es sich, die [SQL-Datenbanküberwachung](sql-database-auditing.md) zu aktivieren, bei der Datenbankereignisse in ein Überwachungsprotokoll in Ihrem Azure Storage-Konto geschrieben werden.  

## <a name="advanced-threat-protection-alerts"></a>Advanced Threat Protection-Warnungen

Advanced Threat Protection für Azure SQL-Datenbank erkennt Anomalien bei Aktivitäten, die auf ungewöhnliche und potenziell schädliche Versuche hinweisen, auf Ihre Datenbanken zuzugreifen oder diese zu nutzen. Eine Liste der Warnungen für Azure SQL-Datenbank finden Sie in den [Warnungen für SQL-Datenbank und SQL Data Warehouse in Azure Security Center](https://docs.microsoft.com/azure/security-center/alerts-reference#alerts-sql-db-and-warehouse).

## <a name="explore-anomalous-database-activities-upon-detection-of-a-suspicious-event"></a>Untersuchen anormaler Datenbankaktivitäten bei Erkennung eines verdächtigen Ereignisses

Bei Erkennung anormaler Datenbankaktivitäten erhalten Sie eine E-Mail-Benachrichtigung. Die E-Mail enthält Informationen zum verdächtigen Sicherheitsereignis (dazu gehören Art der anomalen Aktivitäten, Datenbankname, Servername, Anwendungsname und Zeit des Ereignisses). Darüber hinaus enthält die E-Mail Angaben zu möglichen Ursachen und empfohlenen Maßnahmen zur Untersuchung und Abwehr der potenziellen Bedrohung für die Datenbank.

![Bericht zu anomalen Aktivitäten](./media/sql-database-threat-detection/anomalous_activity_report.png)

1. Klicken Sie in der E-Mail auf den Link **View recent SQL alerts** (Aktuelle SQL-Warnungen anzeigen), um das Azure-Portal zu starten und die Azure Security Center-Seite für Warnungen zu öffnen, auf der eine Übersicht über die aktiven Bedrohungen angezeigt wird, die in der SQL-Datenbank erkannt wurden.

   ![Aktivitätsbedrohungen](./media/sql-database-threat-detection/active_threats.png)

2. Klicken Sie auf eine bestimmte Warnung, um weitere Details und Aktionen zum Untersuchen der entsprechenden Bedrohung und Abwehren zukünftiger Bedrohungen anzuzeigen.

   Beispielsweise ist die Einschleusung von SQL-Befehlen das häufigste Sicherheitsproblem für Webanwendungen im Internet, das für Angriffe auf datengesteuerte Anwendungen verwendet wird. Die Angreifer nutzen Sicherheitslücken der Anwendung, um böswillige SQL-Anweisungen in Eingabefelder der Anwendung einzuschleusen, sodass Daten in der Datenbank manipuliert oder verändert werden können. Bei Warnungen in Bezug auf die Einschleusung von SQL-Befehlen schließen die Details der Warnung die anfällige missbräuchlich genutzte SQL-Anweisung ein.

   ![Spezifische Warnung](./media/sql-database-threat-detection/specific_alert.png)

## <a name="explore-advanced-threat-protection-alerts-for-your-database-in-the-azure-portal"></a>Auswerten von Advanced Threat Protection-Warnungen für Ihre Datenbank im Azure-Portal

Warnungen von Advanced Threat Protection sind in [Azure Security Center](https://azure.microsoft.com/services/security-center/) integriert. Auf Livekacheln von SQL Advanced Threat Protection innerhalb der Datenbank und auf den Blättern von SQL ADS im Azure-Portal lässt sich der Status aktiver Bedrohungen nachverfolgen.

Klicken Sie auf **Advanced Threat Protection-Warnung**, um die Azure Security Center-Seite für Warnungen zu öffnen und eine Übersicht über die aktiven SQL-Bedrohungen zu erhalten, die in der Datenbank erkannt wurden.

   ![Advanced Threat Protection-Warnung](./media/sql-database-threat-detection/threat_detection_alert.png)

   ![Advanced Threat Protection-Warnung2](./media/sql-database-threat-detection/threat_detection_alert_atp.png)

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [Advanced Threat Protection für einzelne und in Pools zusammengefasste Datenbanken](sql-database-threat-detection.md).
- Erfahren Sie mehr über [Advanced Threat Protection in einer verwalteten Instanz](sql-database-managed-instance-threat-detection.md).
- Erfahren Sie mehr über [Advanced Data Security](sql-database-advanced-data-security.md).
- Weitere Informationen zu [Überwachung von Azure SQL-Datenbank](sql-database-auditing.md)
- Weitere Informationen zu [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro).
- Weitere Informationen zu den Preisen finden Sie unter [SQL-Datenbank: Preise](https://azure.microsoft.com/pricing/details/sql-database/).  
