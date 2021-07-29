---
title: 'Leitfaden zur Migration einer lokalen MySQL-Instanz zu Azure Database for MySQL: Sicherheit'
description: Der Wechsel zu einem cloudbasierten Dienst bedeutet nicht, dass das ganze Internet jederzeit Zugriff darauf hat.
ms.service: mysql
ms.subservice: migration-guide
ms.topic: how-to
author: arunkumarthiags
ms.author: arthiaga
ms.reviewer: maghan
ms.custom: ''
ms.date: 05/26/2021
ms.openlocfilehash: 55548eb40e4ac031f1c4e4a3c3aaed3b98f51e1e
ms.sourcegitcommit: c072eefdba1fc1f582005cdd549218863d1e149e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111970858"
---
# <a name="mysql-on-premises-to-azure-database-for-mysql-migration-guide-security"></a>Leitfaden zur Migration einer lokalen MySQL-Instanz zu Azure Database for MySQL: Sicherheit

Der Wechsel zu einem cloudbasierten Dienst bedeutet nicht, dass das ganze Internet jederzeit Zugriff darauf hat. Azure bietet erstklassige Sicherheitsfunktionen, die sicherstellen, dass Datenworkloads kontinuierlich vor böswilligen Akteuren und Programmen geschützt werden.

### <a name="authentication"></a>Authentifizierung

Azure Database for MySQL unterstützt die Standardauthentifizierungsmechanismen für die MySQL-Benutzerkonnektivität, aber auch die [Integration in Azure Active Directory](../concepts-azure-ad-authentication.md). Bei dieser Sicherheitsintegration werden Token ausgestellt, die bei der MySQL-Anmeldung wie Kennwörter fungieren. Die [Konfiguration der Active Directory-Integration](../howto-configure-sign-in-azure-ad-authentication.md) ist äußerst einfach und unterstützt nicht nur Benutzer, sondern auch AAD-Gruppen.

Durch diese enge Integration können Administratoren und Anwendungen die erweiterten Sicherheitsfeatures von [Azure Identity Protection](../../active-directory/identity-protection/overview-identity-protection.md) nutzen, um weitere Identitätsprobleme zu beseitigen.

> [!NOTE] 
> Dieses Sicherheitsfeature wird von MySQL 5.7 und höher unterstützt. Die meisten [Anwendungstreiber](../howto-configure-sign-in-azure-ad-authentication.md) werden unterstützt, solange die `clear-text` Option angegeben wird.

### <a name="threat-protection"></a>Threat Protection

Für den Fall, dass Benutzer- oder Anwendungsanmeldeinformationen kompromittiert werden, sind in den Protokollen wahrscheinlich keine fehlgeschlagenen Anmeldeversuche zu finden. Kompromittierte Anmeldeinformationen können böswilligen Akteuren die Möglichkeit bieten, auf die Daten zu zugreifen und diese herunterzuladen. [Azure Threat Protection](/azure/mysql/concepts-data-access-and-security-threat-protection) kann auf Anomalien bei Anmeldungen (z. B. ungewöhnliche Standorte, seltene Benutzer oder Brute-Force-Angriffe) und andere verdächtige Aktivitäten achten. Administratoren können benachrichtigt werden, wenn etwas nicht richtig aussieht.

### <a name="audit-logging"></a>Überwachungsprotokollierung

MySQL verfügt über ein stabiles integriertes Überwachungsprotokollfeature. Standardmäßig ist dieses [Überwachungsprotokollfeature](../concepts-audit-logs.md) in Azure Database for MySQL deaktiviert. Die Protokollierung auf Serverebene kann durch Ändern des `audit\_log\_enabled`-Serverparameters aktiviert werden. Nach der Aktivierung können Sie über [Azure Monitor](../../azure-monitor/overview.md) und [Log Analytics](../../azure-monitor/logs/design-logs-deployment.md) auf Protokolle zugreifen, indem Sie die [Diagnoseprotokollierung](../howto-configure-audit-logs-portal.md#set-up-diagnostic-logs) aktivieren.

Führen Sie die folgende KQL-Abfrage aus, um Ereignisse in Zusammenhang mit Benutzerverbindungen abzufragen:

```
AzureDiagnostics  
| where ResourceProvider =="MICROSOFT.DBFORMYSQL" 
| where Category == 'MySqlAuditLogs' and event\_class\_s == "connection\_log"  
| project TimeGenerated, LogicalServerName\_s, event\_class\_s, event\_subclass\_s  
, event\_time\_t, user\_s , ip\_s , sql\_text\_s  
| order by TimeGenerated asc
```

### <a name="encryption"></a>Verschlüsselung

Daten in der MySQL-Instanz werden im Ruhezustand standardmäßig verschlüsselt. Alle automatisierten Sicherungen werden ebenfalls verschlüsselt, um mögliche Datenlecks an Unbefugte zu verhindern. Diese Verschlüsselung erfolgt in der Regel mit einem Schlüssel, der beim Erstellen der Instanz erstellt wird. Zusätzlich zu diesem Standardverschlüsselungsschlüssel können Administratoren [eigene Schlüssel verwenden (BYOK, Bring Your Own Key)](../concepts-data-encryption-mysql.md).

Wenn Sie eine Strategie mit kundenseitig verwalteten Schlüsseln verwenden, müssen Sie die Zuständigkeiten für die Schlüssellebenszyklusverwaltung kennen. Kundenschlüssel werden in einem [Azure Key Vault](../../key-vault/general/basic-concepts.md) gespeichert und dann über Richtlinien aufgerufen. Es ist wichtig, alle Empfehlungen für die Schlüsselverwaltung zu befolgen. Der Verlust des Verschlüsselungsschlüssels ist gleichwertig mit dem Verlust des Datenzugriffs.

Verwenden Sie zusätzlich zu kundenseitig verwalteten Schlüsseln Schlüssel auf Dienstebene, um eine [doppelte Verschlüsselung](../concepts-infrastructure-double-encryption.md) zu implementieren. Durch die Implementierung dieses Features werden ruhende Daten hochgradig verschlüsselt, die Verschlüsselungsleistung wird jedoch beeinträchtigt. Es sollten entsprechende Tests durchgeführt werden.

Daten können bei der Übertragung mit SSL/TLS verschlüsselt werden. Wie bereits erwähnt, kann es erforderlich sein, die [Anwendungen zu ändern](../howto-configure-ssl.md), damit diese Änderung unterstützt wird, und auch die entsprechenden TLS-Überprüfungseinstellungen zu konfigurieren.

### <a name="firewall"></a>Firewall

Sobald Benutzer eingerichtet sind und ruhende Daten verschlüsselt sind, sollte das Migrationsteam die Datenflüsse im Netzwerk überprüfen. Azure Database for MySQL bietet mehrere Mechanismen zum Schützen der Netzwerkebenen, indem der Zugriff nur auf autorisierte Benutzer, Anwendungen und Geräte beschränkt wird.

Die erste Verteidigungslinie zum Schutz der MySQL-Instanz besteht in der Implementierung von [Firewallregeln](../concepts-firewall-rules.md). IP-Adressen können beim Zugriff auf die Instanz über interne oder externe IP-Adressen auf gültige Standorte beschränkt werden. Wenn die MySQL-Instanz nur für interne Anwendungen verwendet wird, sollten Sie den [öffentlichen Zugriff einschränken](../howto-deny-public-network-access.md).

Bei der Verlagerung einer Anwendung zusammen mit der MySQL-Workload nach Azure ist es wahrscheinlich, dass mehrere virtuelle Netzwerke in einem Hub-and-Spoke-Muster eingerichtet werden, für sodass [ein Peering virtueller Netzwerke](../../virtual-network/virtual-network-peering-overview.md) konfiguriert werden muss.

### <a name="private-link"></a>Private Link

Aktivieren Sie [Private Link](../concepts-data-access-security-private-link.md), um den Zugriff auf Azure Database for MySQL auf interne Azure-Ressourcen zu beschränken. Private Link stellt sicher, dass der MySQL-Instanz eine private IP-Adresse anstelle einer öffentlichen IP-Adresse zugewiesen wird.

> [!NOTE]
> Es gibt viele andere [grundlegende Überlegungen zu Azure-Netzwerken](../concepts-data-access-and-security-vnet.md), die berücksichtigt werden müssen, aber nicht in diesem Leitfaden behandelt werden.

Informieren Sie sich über die möglichen Aufgaben zum Erstellen einer [Sicherheitsbaseline](/azure/mysql/security-baseline), die für alle Azure-Ressourcen implementiert werden können. Nicht alle im Referenzlink beschriebenen Elemente gelten für alle Datenworkloads oder Azure-Ressourcen.

### <a name="security-checklist"></a>Sicherheitsprüfliste

  - Verwenden Sie nach Möglichkeit die Azure AD-Authentifizierung.

  - Aktivieren Sie Advanced Threat Protection.

  - Aktivieren Sie alle Überwachungsfeatures.

  - Erwägen Sie eine Bring-Your-Own-Key-Strategie (BYOK).

  - Implementieren Sie Firewallregeln.

  - Nutzen Sie private Endpunkte für Workloads, die nicht über das Internet übertragen werden.  


> [!div class="nextstepaction"]
> [Zusammenfassung](./summary.md)