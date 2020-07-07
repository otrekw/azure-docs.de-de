---
title: Konfigurieren von Advanced Threat Protection
description: Advanced Threat Protection identifiziert anomale Datenbankaktivitäten, die auf potenzielle Sicherheitsbedrohungen für die Datenbank in Azure SQL-Datenbank hinweisen.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: seo-dt-2019, sqldbrb=1
ms.topic: conceptual
author: rmatchoro
ms.author: ronmat
ms.reviewer: vanto, carlrab
ms.date: 08/05/2019
ms.openlocfilehash: 2f4e9841a44252829fae283a12ba804219204022
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84321545"
---
# <a name="configure-advanced-threat-protection-for-azure-sql-database"></a>Konfigurieren von Advanced Threat Protection für Azure SQL-Datenbank
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

[Advanced Threat Protection](threat-detection-overview.md) für Azure SQL-Datenbank erkennt Anomalien bei Aktivitäten, die auf ungewöhnliche und potenziell schädliche Versuche hinweisen, auf Datenbanken zuzugreifen oder diese zu missbrauchen. Mithilfe von Advanced Threat Protection können folgende Aktivitäten identifiziert werden: **potenzielle SQL-Einschleusung**, **Zugriff von einem ungewöhnlichen Standort oder Rechenzentrum**, **Zugriff über einen unbekannten Prinzipal oder eine potenziell schädliche Anwendung** und **Brute-Force-SQL-Anmeldeinformationen**. Weitere Informationen finden Sie im Abschnitt zu den [Advanced Threat Protection-Warnungen](threat-detection-overview.md#alerts).

Sie können sich über [E-Mail-Benachrichtigungen](threat-detection-overview.md#explore-detection-of-a-suspicious-event) oder im [Azure-Portal](threat-detection-overview.md#explore-alerts-in-the-azure-portal) über die erkannten Bedrohungen benachrichtigen lassen.

[Advanced Threat Protection](threat-detection-overview.md) ist Teil des [Advanced Data Security](advanced-data-security.md)-Angebots. Dabei handelt es sich um ein vereinheitlichtes Paket für erweiterte SQL-Sicherheitsfunktionen. Der Zugriff auf Advanced Threat Protection und die Verwaltung erfolgen über das zentrale SQL Advanced Data Security-Portal.

## <a name="set-up-advanced-threat-protection-in-the-azure-portal"></a>Einrichten von Advanced Threat Protection im Azure-Portal

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Navigieren Sie zur Konfigurationsseite des Servers, den Sie schützen möchten. Wählen Sie in den Sicherheitseinstellungen **Advanced Data Security** aus.
3. Wählen Sie auf der Seite für die **Advanced Data Security**-Konfiguration

   - die Option „Advanced Data Security auf dem Server aktivieren“ aus.
   - Geben Sie unter **Advanced Threat Protection Settings** (Advanced Threat Protection-Einstellungen) im Textfeld **Warnungen senden an** eine Liste von E-Mail-Adressen an, die Sicherheitswarnungen bei der Erkennung von anomalen Datenbankaktivitäten empfangen sollen.
  
   ![Einrichten von Advanced Threat Protection](./media/threat-detection/set_up_threat_detection.png)

## <a name="set-up-advanced-threat-protection-using-powershell"></a>Einrichten von Advanced Threat Protection mit PowerShell

Ein Skriptbeispiel finden Sie unter [Konfigurieren von Überwachung und Advanced Threat Protection mit PowerShell](scripts/auditing-threat-detection-powershell-configure.md).

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [Advanced Threat Protection](threat-detection-overview.md).
- Erfahren Sie mehr über [Advanced Threat Protection in SQL Managed Instance](../managed-instance/threat-detection-configure.md).  
- Erfahren Sie mehr über [Advanced Data Security](advanced-data-security.md).
- Weitere Informationen zur [Überwachung](../../azure-sql/database/auditing-overview.md).
- Weitere Informationen zu [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro).
- Weitere Informationen zu den Preisen finden Sie unter [SQL-Datenbank: Preise](https://azure.microsoft.com/pricing/details/sql-database/).  
