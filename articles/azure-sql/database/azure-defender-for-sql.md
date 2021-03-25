---
title: Azure Defender für SQL
description: Hier erhalten Sie Informationen zu den Funktionen zur Verwaltung von Datenbankrisiken und die Erkennung ungewöhnlicher Aktivitäten, die eine Bedrohung für Ihre Datenbank in Azure SQL-Datenbank, Azure SQL Managed Instance oder Azure Synapse darstellen können.
ms.service: sql-db-mi
ms.subservice: security
ms.devlang: ''
ms.custom: sqldbrb=2
ms.topic: conceptual
ms.author: memildin
manager: rkarlin
author: memildin
ms.date: 03/08/2021
ms.openlocfilehash: 27f17b3d1060e8b693c2a34cdb4643840f1bfd13
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102452310"
---
# <a name="azure-defender-for-sql"></a>Azure Defender für SQL

[!INCLUDE[appliesto-sqldb-sqlmi-asa](../includes/appliesto-sqldb-sqlmi-asa.md)]

Azure Defender für SQL ist ein einheitliches Paket für erweiterte SQL-Sicherheitsfunktionen. Azure Defender ist für Azure SQL-Datenbank, Azure SQL Managed Instance und Azure Synapse Analytics verfügbar. Dazu zählen die Funktionen zur Ermittlung und Klassifizierung sensibler Daten, Ermittlung und Verringerung potenzieller Datenbankschwachstellen und Erkennung ungewöhnlicher Aktivitäten, die eine Bedrohung für Ihre Datenbank darstellen können. Es bietet einen einzelnen Anlaufpunkt zum Aktivieren und Verwalten dieser Funktionen.

## <a name="what-are-the-benefits-of-azure-defender-for-sql"></a>Welche Vorteile hat die Nutzung von Azure Defender für SQL?

Azure Defender stellt eine Reihe erweiterter SQL-Sicherheitsfunktionen bereit, darunter SQL-Risikoanalyse und Advanced Threat Protection.
- Die [Sicherheitsrisikobewertung](sql-vulnerability-assessment.md) ist ein einfach zu konfigurierender Dienst, mit dem potenzielle Schwachstellen in der Datenbank ermittelt, nachverfolgt und behoben werden können. Er bietet Einblicke in Ihren Sicherheitsstatus, enthält umsetzbare Schritte zum Beheben von Sicherheitsproblemen und verbessert Ihre Datenbanksicherheit.
- [Advanced Threat Protection](threat-detection-overview.md) erkennt Anomalien bei Aktivitäten, die auf ungewöhnliche und potenziell schädliche Versuche hinweisen, auf Ihre Datenbank zuzugreifen oder diese zu nutzen. Sie überwacht Ihre Datenbank fortlaufend auf verdächtige Aktivitäten und bietet sofortige Sicherheitswarnungen zu potenziellen Sicherheitsrisiken, Angriffen durch Einschleusung von Azure SQL-Befehlen und ungewöhnlichen Datenbankzugriffsmustern. Die Warnungen von Advanced Threat Protection enthalten Details zu verdächtigen Aktivitäten sowie Empfehlungen, wie die Bedrohung untersucht und abgewendet werden kann.

Sie müssen Azure Defender für SQL nur einmal aktivieren, um alle diese enthaltenen Funktionen zu aktivieren. Mit nur einem Klick können Sie Azure Defender für alle Datenbanken auf Ihrem [Server](logical-servers.md) in Azure oder Azure SQL Managed Instance aktivieren. Um Azure Defender-Einstellungen aktivieren oder verwalten zu können, müssen Sie der Rolle [SQL-Sicherheits-Manager](../../role-based-access-control/built-in-roles.md#sql-security-manager) oder einer der Datenbank- oder Serveradministratorrollen angehören.

Weitere Informationen zu den Preisen für Azure Defender für SQL finden Sie auf der Seite [Azure Security Center – Preise](https://azure.microsoft.com/pricing/details/security-center/).

## <a name="enable-azure-defender"></a>Aktivieren von Azure Defender 
Zum Aktivieren von Azure Defender-Plänen gibt es mehrere Möglichkeiten. Sie können Azure Defender wie folgt auf Abonnementebene aktivieren (**empfohlen**):

- [Azure Security Center](#enable-azure-defender-for-azure-sql-database-at-the-subscription-level-from-azure-security-center)
- [Programmgesteuert mit der REST-API, der Azure-Befehlszeilenschnittstelle, mit PowerShell oder Azure Policy](#enable-azure-defender-plans-programatically)

Alternativ können Sie Azure Defender auf Ressourcenebene aktivieren, wie unter [Aktivieren von Azure Defender für Azure SQL-Datenbank auf Ressourcenebene](#enable-azure-defender-for-azure-sql-database-at-the-resource-level) beschrieben.

### <a name="enable-azure-defender-for-azure-sql-database-at-the-subscription-level-from-azure-security-center"></a>Aktivieren von Azure Defender für Azure SQL-Datenbank auf Abonnementebene über Azure Security Center
Gehen Sie wie folgt vor, um Azure Defender für Azure SQL-Datenbank auf Abonnementebene über Azure Security Center zu aktivieren:

1. Öffnen Sie im [Azure-Portal](https://portal.azure.com) das **Security Center**.
1. Wählen Sie im Menü von Security Center die Option **Preise und Einstellungen** aus.
1. Wählen Sie das relevante Abonnement aus.
1. Ändern Sie die Planeinstellung in **Ein**.

    :::image type="content" source="media/azure-defender-for-sql/enable-azure-defender-sql-subscription-level.png" alt-text="Aktivieren von Azure Defender für Azure SQL-Datenbank auf Abonnementebene":::

1. Wählen Sie **Speichern** aus.


### <a name="enable-azure-defender-plans-programatically"></a>Programmgesteuertes Aktivieren von Azure Defender-Plänen 

Die Flexibilität von Azure ermöglicht eine Reihe programmgesteuerter Methoden zum Aktivieren von Azure Defender-Plänen. 

Verwenden Sie eines der folgenden Tools, um Azure Defender für Ihr Abonnement zu aktivieren: 

| Methode       | Instructions                                                                                                                                       |
|--------------|----------------------------------------------------------------------------------------------------------------------------------------------------|
| REST-API     | [Pricings-API](/rest/api/securitycenter/pricings)                                                                                                  |
| Azure CLI    | [az security pricing](/cli/azure/security/pricing)                                                                                                 |
| PowerShell   | [Set-AzSecurityPricing](/powershell/module/az.security/set-azsecuritypricing)                                                                      |
| Azure Policy | [Bundle-Preise](https://github.com/Azure/Azure-Security-Center/blob/master/Pricing%20%26%20Settings/ARM%20Templates/Set-ASC-Bundle-Pricing.json) |
|              |                                                                                                                                                    |

### <a name="enable-azure-defender-for-azure-sql-database-at-the-resource-level"></a>Aktivieren von Azure Defender für Azure SQL-Datenbank auf Ressourcenebene

Es wird empfohlen, Azure Defender-Pläne auf Abonnementebene zu aktivieren. Dies kann die Erstellung ungeschützter Ressourcen erleichtern. Wenn Sie jedoch Azure Defender aus organisatorischen Gründen auf Serverebene aktivieren möchten, führen Sie die folgenden Schritte aus:

1. Öffnen Sie im [Azure-Portal](https://portal.azure.com) Ihren Server oder Ihre verwaltete Instanz.
1. Wählen Sie unter der Überschrift **Sicherheit** die Option **Security Center** aus.
1. Wählen Sie **Azure Defender für SQL aktivieren** aus.

    :::image type="content" source="media/azure-defender-for-sql/enable-azure-defender.png" alt-text="Aktivieren von Azure Defender für SQL in Azure SQL-Datenbanken":::

> [!NOTE]
> Ein Speicherkonto wird automatisch erstellt und konfiguriert, um die Speicherkontoergebnisse der **Sicherheitsrisikobewertung** zu speichern. Wenn Sie Azure Defender bereits für einen anderen Server in derselben Ressourcengruppe und Region aktiviert haben, wird das vorhandene Speicherkonto verwendet.
>
> Die Kosten von Azure Defender orientieren sich am Azure Security Center-Standardtarif pro Knoten, wobei ein Knoten dem gesamten Server oder der verwalteten Instanz entspricht. Sie bezahlen daher nur einmal für den Schutz aller Datenbanken auf dem Server oder in der verwalteten Instanz durch Azure Defender. Sie können Azure Defender anfänglich mit einer kostenlosen Testversion testen.


## <a name="manage-azure-defender-settings"></a>Verwalten von Azure Defender-Einstellungen

Zum Anzeigen und Verwalten von Azure Defender-Einstellungen führen Sie die folgenden Schritte aus:

1. Wählen Sie im Bereich **Sicherheit** Ihres Servers oder der verwalteten Instanz die Option **Security Center** aus.

    Auf dieser Seite wird der Status von Azure Defender für SQL angezeigt:

    :::image type="content" source="media/azure-defender-for-sql/status-of-defender-for-sql.png" alt-text="Überprüfen des Status von Azure Defender für SQL in Azure SQL-Datenbanken":::

1. Wenn Azure Defender für SQL aktiviert ist, wird ein Link **Konfigurieren** wie in der vorherigen Abbildung angezeigt. Wählen Sie **Konfigurieren** aus, um die Einstellungen für Azure Defender für SQL zu bearbeiten.

    :::image type="content" source="media/azure-defender-for-sql/security-server-settings.png" alt-text="Einstellungen für Azure Defender für SQL":::

1. Nehmen Sie die gewünschten Änderungen vor, und wählen Sie **Speichern** aus.


## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zur [Sicherheitsrisikobewertung](sql-vulnerability-assessment.md)
- Weitere Informationen zu [Advanced Threat Protection](threat-detection-configure.md)
- Weitere Informationen zu [Azure Security Center](../../security-center/security-center-introduction.md)