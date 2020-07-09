---
title: Sicherheitskontrollen
description: Eine Prüfliste mit Sicherheitskontrollen zur Evaluierung von Azure SQL-Datenbank
services: sql-database
author: msmbaldwin
manager: rkarlin
ms.service: load-balancer
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: mbaldwin
ms.openlocfilehash: 97d2cd8e9ba51e4fc6ebab8459b04f4f37e876d3
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84668422"
---
# <a name="security-controls-for-azure-sql-database-and-sql-managed-instance"></a>Sicherheitskontrollen für Azure SQL-Datenbank und SQL Managed Instance
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

In diesem Artikel werden die in Azure SQL-Datenbank und Azure SQL Managed Instance integrierten Sicherheitskontrollen beschrieben.

[!INCLUDE [Security controls Header](../../../includes/security-controls-header.md)]



## <a name="network"></a>Netzwerk

| Sicherheitskontrolle | Ja/Nein | Notizen |
|---|---|--|
| Unterstützung des Dienstendpunkts| Ja | Gilt nur für [SQL-Datenbank](../index.yml). |
| Unterstützung für das Einfügen in Azure Virtual Network| Ja | Gilt nur für eine [SQL Managed Instance](../managed-instance/sql-managed-instance-paas-overview.md). |
| Netzwerkisolation und Firewallunterstützung| Ja | Firewall sowohl auf Datenbankebene als auch auf Serverebene. Netzwerkisolation wird ausschließlich für [SQL Managed Instance](../managed-instance/sql-managed-instance-paas-overview.md) angeboten. |
| Unterstützung der Tunnelerzwingung| Ja | [SQL Managed Instance](../managed-instance/sql-managed-instance-paas-overview.md) über [ExpressRoute](../expressroute/../index.yml)-VPN. |

## <a name="monitoring--logging"></a>Überwachung und Protokollierung

| Sicherheitskontrolle | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Azure-Überwachung, z. B. Protokollanalyse oder Application Insights| Ja | SecureSphere, die SIEM-Lösung von Imperva, wird ebenfalls unterstützt, und zwar mittels [Azure Event Hubs](../event-hubs/../index.yml)-Integration über [SQL-Überwachung](../../azure-sql/database/auditing-overview.md). |
| Protokollierung und Überwachung auf Steuerungs- und Verwaltungsebene| Ja | Ja, für einige Ereignisse |
| Protokollierung und Überwachung auf Datenebene | Ja | Über [SQL-Überwachung](../../azure-sql/database/auditing-overview.md) |

## <a name="identity"></a>Identity

| Sicherheitskontrolle | Ja/Nein | Notizen|
|---|---|--|
| Authentifizierung| Ja | Azure Active Directory (Azure AD) |
| Authorization| Ja | Keine |

## <a name="data-protection"></a>Schutz von Daten

| Sicherheitskontrolle | Ja/Nein | Notizen |
|---|---|--|
| Serverseitige Verschlüsselung ruhender Daten: Von Microsoft verwaltete Schlüssel | Ja | Wird als „Verschlüsselung während der Verwendung“ bezeichnet, wie im Artikel [Always Encrypted](always-encrypted-certificate-store-configure.md) beschrieben. Dienstseitige Verschlüsselung verwendet die [transparente Datenverschlüsselung](transparent-data-encryption-tde-overview.md).|
| Verschlüsselung während der Übertragung:<ul><li>Azure ExpressRoute-Verschlüsselung</li><li>Verschlüsselung in einem virtuellen Netzwerk</li><li>Verschlüsselung zwischen virtuellen Netzwerken</ul>| Ja | Mithilfe von HTTPS |
| Behandeln von Verschlüsselungsschlüsseln, wie z. B. CMK oder BYOK| Ja | Es wird sowohl die Verarbeitung von vom Dienst verwalteten Schlüsseln als auch die Verarbeitung von vom Kunden verwalteten Schlüsseln angeboten. Letztere Option wird über [Azure Key Vault](../key-vault/../index.yml) angeboten. |
| Verschlüsselung auf Spaltenebene, die von Azure Data Services bereitgestellt wird| Ja | Durch [Always Encrypted](always-encrypted-certificate-store-configure.md). |
| Verschlüsselte API-Aufrufe| Ja | Mithilfe von HTTPS/TLS. |

## <a name="configuration-management"></a>Konfigurationsverwaltung

| Sicherheitskontrolle | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Konfigurationsverwaltung, z. B. Versionsverwaltung der Konfiguration| Nein  | Keine |

## <a name="additional-security-controls-for-sql-database"></a>Zusätzliche Sicherheitskontrollen für die SQL-Datenbank

| Sicherheitskontrolle | Ja/Nein | Notizen|
|---|---|--|
| Prävention: Sicherheitsrisikobewertung | Ja | Weitere Informationen finden Sie unter [Mit dem Dienst zur SQL-Sicherheitsrisikobewertung können Sie Datenbankschwachstellen erkennen](sql-vulnerability-assessment.md). |
| Prävention: Datenermittlung und -klassifizierung  | Ja | Weitere Informationen finden Sie unter [Azure SQL-Datenbank und SQL Data Warehouse: Datenermittlung und -klassifizierung](data-discovery-and-classification-overview.md). |
| Erkennung: Bedrohungserkennung | Ja | Weitere Informationen finden Sie unter [Advanced Threat Protection für Azure SQL-Datenbank](threat-detection-overview.md). |

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die [integrierten Sicherheitskontrollen in Azure-Diensten](../../security/fundamentals/security-controls.md).
