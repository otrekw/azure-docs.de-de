---
title: Häufig gestellte Fragen (FAQ)
titleSuffix: Azure SQL Managed Instance
description: Häufig gestellte Fragen (FAQ) zu Azure SQL Managed Instance
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein, carlrab
ms.date: 03/17/2020
ms.openlocfilehash: b72952618b2d024bd2c4b445c3ea673ed523866b
ms.sourcegitcommit: 8017209cc9d8a825cc404df852c8dc02f74d584b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2020
ms.locfileid: "84247936"
---
# <a name="azure-sql-managed-instance-frequently-asked-questions-faq"></a>Häufig gestellte Fragen (FAQ) zu Azure SQL Managed Instance
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Dieser Artikel enthält die am häufigsten gestellten Fragen zu [Azure SQL Managed Instance](sql-managed-instance-paas-overview.md).

## <a name="supported-features"></a>Unterstützte Features

**Wo finde ich eine Liste der unterstützten Funktionen in SQL Managed Instance?**

Eine Liste der in SQL Managed Instance unterstützten Funktionen finden Sie unter [Azure SQL Managed Instance: Features](../database/features-comparison.md).

Informationen zu den Unterschieden in Syntax und Verhalten zwischen Azure SQL Managed Instance und SQL Server finden Sie unter [T-SQL-Unterschiede zu SQL Server](transact-sql-tsql-differences-sql-server.md).


## <a name="tech-spec--resource-limits"></a>Technische Spezifikation und Ressourcenlimits
 
**Wo finde ich technische Merkmale und Ressourceneinschränkungen für SQL Managed Instance?**

Informationen zu den verfügbaren Merkmalen der Hardwaregenerationen finden Sie unter [technische Unterschiede zwischen Hardwaregenerationen](resource-limits.md#hardware-generation-characteristics).
Informationen zu den verfügbaren Dienstebenen und ihren Merkmalen finden Sie unter [technische Unterschiede zwischen Dienstebenen](resource-limits.md#service-tier-characteristics).

## <a name="known-issues--bugs"></a>Bekannte Probleme und Fehler

**Wo finde ich bekannte Probleme und Fehler?**

Programmfehler und bekannte Probleme werden unter [Bekannte Probleme](../database/doc-changes-updates-release-notes.md#known-issues) erörtert.

## <a name="new-features"></a>Neue Funktionen

**Wo finde ich die neuesten Features sowie die Features in der öffentlichen Vorschau?**

Neue Features und Previewfunktionen finden Sie in den [Versionshinweisen](../database/doc-changes-updates-release-notes.md?tabs=managed-instance).

## <a name="deployment-times"></a>Bereitstellungszeiten 

**Wie lange dauert es, eine Instanz zu erstellen oder zu aktualisieren oder eine Datenbank wiederherzustellen?**

Die erwartete Zeit zum Erstellen einer SQL Managed Instance oder zum Ändern der Dienstebene (virtuelle Kerne, Speicher) hängt von mehreren Faktoren ab. Sehen Sie sich die [Verwaltungsvorgänge](/azure/sql-database/sql-database-managed-instance#managed-instance-management-operations) an. 

## <a name="naming-convention"></a>Benennungskonvention

**Kann eine SQL Managed Instance den gleichen Namen wie eine lokale Instanz von SQL Server haben?**

Die Namensänderung einer SQL Managed Instance wird nicht unterstützt.

Die DNS-Standardzone *.database.windows.net* von SQL Managed Instance könnte geändert werden. 

So verwenden Sie statt der Standardzone eine andere DNS-Zone, z. B. *.contoso.com*: 
- Verwenden Sie „CliConfig“ zum Definieren eines Alias. Weil das Tool nur ein Wrapper für Registrierungseinstellungen ist, könnte es auch mithilfe einer Gruppenrichtlinie oder eines Skripts ausgeführt werden.
- Verwenden Sie *CNAME* mit der Option *TrustServerCertificate=true*.

## <a name="move-db-from-mi"></a>Verschieben einer Datenbank aus einer verwalteten Instanz 

**Wie kann ich eine Datenbank von SQL Managed Instance zurück zu SQL Server oder Azure SQL-Datenbank verschieben?**

Sie können [die Datenbank in eine BACPAC-Datei exportieren](../database/database-export.md) und dann [die BACPAC-Datei importieren]( ../database/database-import.md). Dieser Ansatz wird empfohlen, wenn die Datenbank kleiner als 100 GB ist.

Transaktionsreplikation kann verwendet werden, wenn alle Tabellen in der Datenbank Primärschlüssel aufweisen.

Native `COPY_ONLY`-Sicherungen von SQL Managed Instance können nicht in SQL Server wiederhergestellt werden, da SQL Managed Instance eine höhere Datenbankversion als SQL Server aufweist.

## <a name="migrate-instance-db"></a>Migrieren einer Instanzdatenbank

**Wie kann ich meine Instanzdatenbank zu einer einzelnen Azure SQL-Datenbank migrieren?**

Eine Option ist das [Exportieren der Datenbank in eine BACPAC-Datei](../database/database-export.md) und das anschließende [Importieren der BACPAC-Datei](../database/database-import.md). 

Dieser Ansatz wird empfohlen, wenn die Datenbank kleiner als 100 GB ist. Transaktionsreplikation kann verwendet werden, wenn alle Tabellen in der Datenbank Primärschlüssel aufweisen.

## <a name="switch-hardware-generation"></a>Wechseln der Hardwaregeneration 

**Kann ich für meine SQL Managed Instance online zwischen der Hardwaregeneration Gen 4 und Gen 5 wechseln?**

Der automatische Onlinewechsel zwischen Hardwaregenerationen ist möglich, wenn beide Hardwaregenerationen in der Region verfügbar sind, in der Ihre SQL Managed Instance bereitgestellt wurde. In diesem Fall können Sie auf der [Seite mit der Übersicht über das V-Kern-Modell](../database/service-tiers-vcore.md) nachsehen, wie Sie zwischen den Hardwaregenerationen wechseln können.

Dabei handelt es sich um einen Vorgang mit langer Ausführungszeit, da eine neue SQL Managed Instance im Hintergrund bereitgestellt wird und Datenbanken automatisch zwischen der alten und neuen Instanz mit einem schnellen Failover am Ende des Prozesses übertragen werden. 

**Was geschieht, wenn die beiden Hardwaregenerationen nicht in derselben Region unterstützt werden?**

Wenn nicht beide Hardwaregenerationen in der gleichen Region unterstützt werden, kann die Hardwaregeneration nur manuell gewechselt werden. Dazu müssen Sie eine neue Instanz in der Region bereitstellen, in der die gewünschte Hardwaregeneration verfügbar ist, sowie die Daten zwischen der alten und neuen Instanz manuell sichern und wiederherstellen.

**Was geschieht, wenn nicht genügend IP-Adressen zum Ausführen des Updatevorgangs vorhanden sind?**

Falls nicht genügend IP-Adressen im Subnetz vorhanden sind, in dem die verwaltete Instanz bereitgestellt wird, müssen Sie ein neues Subnetz und darin eine neue verwaltete Instanz erstellen. Es wird auch empfohlen, ein neues Subnetz mit mehr IP-Adressen zu erstellen, damit bei zukünftigen Updatevorgängen eine ähnliche Situation vermieden werden kann. (Informationen zu geeigneten Subnetzgrößen finden Sie unter [Ermitteln der Größe von VNET-Subnetzen](vnet-subnet-determine-size.md).) Nachdem die neue Instanz bereitgestellt wurde, können Sie Daten zwischen der alten und der neuen Instanz manuell sichern und wiederherstellen. Sie können auch eine instanzenübergreifende [Point-in-Time-Wiederherstellung](point-in-time-restore.md?tabs=azure-powershell) durchführen. 


## <a name="tune-performance"></a>Optimieren der Leistung

**Wie optimiere ich die Leistung meiner SQL Managed Instance?**

Da eine universelle SQL Managed Instance Remotespeicher verwendet, wirkt sich die Größe der Daten-und Protokolldateien auf die Leistung aus. Weitere Informationen finden Sie unter [Auswirkung der Protokolldateigröße auf die Leistung einer universellen SQL Managed Instance](https://medium.com/azure-sqldb-managed-instance/impact-of-log-file-size-on-general-purpose-managed-instance-performance-21ad170c823e).

Wenn Ihre Workload aus vielen kleinen Transaktionen besteht, sollten Sie den Verbindungstyp von Proxymodus in Umleitungsmodus ändern.

## <a name="maximum-storage-size"></a>Maximale Speichergröße

**Was ist die maximale Speichergröße für SQL Managed Instance?**

Die Speichergröße für SQL Managed Instance hängt von der ausgewählten Dienstebene („Universell“ oder „Unternehmenskritisch“) ab. Informationen zu den Speichereinschränkungen dieser Dienstebenen finden Sie unter [Merkmale der Dienstebene](../database/service-tiers-general-purpose-business-critical.md).

## <a name="back-up-storage-cost"></a>Speicherkosten für Sicherungen 

**Wird der Sicherungsspeicher vom Speicher meiner SQL Managed Instance abgezogen?**

Nein, der Sicherungsspeicher wird nicht vom Speicher Ihrer SQL Managed Instance abgezogen. Der Sicherungsspeicher ist unabhängig vom Instanzspeicherplatz, und seine Größe ist nicht begrenzt. Der Sicherungsspeicher wird durch den Zeitraum zum Beibehalten der Sicherung Ihrer Instanzdatenbanken begrenzt, der auf 7 bis 35 Tage festgelegt werden kann. Weitere Informationen finden Sie unter [Automatisierte Sicherungen](../database/automated-backups-overview.md).

## <a name="track-billing"></a>Nachverfolgen der Abrechnung

**Gibt es eine Möglichkeit, meine Abrechnungskosten für meine SQL Managed Instance nachzuverfolgen?**

Sie können zu diesem Zweck die [Azure Cost Management-Lösung](/azure/cost-management/) verwenden. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu **Abonnements**, und wählen Sie **Kostenanalyse** aus. 

Verwenden Sie die Option **Kumulierte Kosten**, und filtern Sie dann nach dem **Ressourcentyp** als `microsoft.sql/managedinstances`. 
  
## <a name="inbound-nsg-rules"></a>NSG-Regeln für eingehenden Datenverkehr

**Wie kann ich NSG-Regeln für eingehenden Datenverkehr an Verwaltungsports festlegen?**

Die Steuerungsebene von SQL Managed Instance verwaltet NSG-Regeln zum Schutz von Verwaltungsports.

Verwaltungsports werden für Folgendes verwendet:

Port 9000 und 9003 werden für die Service Fabric-Infrastruktur verwendet. Die primäre Aufgabe von Service Fabric ist es, den virtuellen Cluster fehlerfrei zu halten und den Zielzustand hinsichtlich der Anzahl der Komponentenreplikate zu bewahren.

Die Ports 1438, 1440 und 1452 werden vom Knoten-Agent verwendet. Der Knoten-Agent ist eine Anwendung, die im Cluster ausgeführt wird und von der Steuerungsebene zum Ausführen von Verwaltungsbefehlen verwendet wird.

Zusätzlich zu den NSG-Regeln schützt die integrierte Firewall die Instanz auf Netzwerkebene. Auf Anwendungsebene wird die Kommunikation mit den Zertifikaten geschützt.
  
Weitere Informationen zu diesem Thema und zum Überprüfen der integrierten Firewall finden Sie unter [Azure SQL Managed Instance: integrierte Firewall](management-endpoint-verify-built-in-firewall.md).


## <a name="mitigate-data-exfiltration-risks"></a>Mindern von Risiken bei der Datenexfiltration  

**Wie kann ich Risiken bei der Datenexfiltration mindern?**

Kunden wird empfohlen, zum Mindern von Risiken bei der Datenexfiltration eine Reihe von Sicherheitseinstellungen und -kontrollen anzuwenden:

- Aktivieren Sie [Transparent Data Encryption (TDE)](https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-azure-sql) für alle Datenbanken.
- Deaktivieren Sie die Common Language Runtime (CLR). Dies wird auch für lokale Umgebungen empfohlen.
- Verwenden Sie nur Authentifizierung über Azure Active Directory (AAD).
- Greifen Sie auf die Instanz mit einem DBA-Konto mit geringen Berechtigungen zu.
- Konfigurieren Sie den Zugriff auf die JiT-Jumpbox für das Systemadministratorkonto.
- Aktivieren Sie [SQL-Überwachung](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine), und integrieren Sie sie in Warnungsmechanismen.
- Aktivieren Sie die [Bedrohungserkennung](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection) aus der [Advanced Data Security (ADS)](https://docs.microsoft.com/azure/sql-database/sql-database-advanced-data-security)-Sammlung.


## <a name="cost-saving-use-cases"></a>Anwendungsfälle für Kosteneinsparungen

**Wo finde ich Anwendungsfälle und entsprechende Kosteneinsparungen für SQL Managed Instance?**

Fallstudien zu SQL Managed Instance:

- [Komatsu](https://customers.microsoft.com/story/komatsu-australia-manufacturing-azure)
- [KMD](https://customers.microsoft.com/en-ca/story/kmd-professional-services-azure-sql-database)
- [PowerDETAILS](https://customers.microsoft.com/story/powerdetails-partner-professional-services-azure-sql-database-managed-instance)
- [Allscripts](https://customers.microsoft.com/story/allscripts-partner-professional-services-azure)   
Es gibt auch eine Forrester-Studie, die ein besseres Verständnis der Vorteile, Kosten und Risiken im Zusammenhang mit der Bereitstellung von SQL Managed Instance vermittelt: [Der Total Economic Impact der verwalteten Microsoft Azure SQL-Datenbank-Instanz](https://azure.microsoft.com/resources/forrester-tei-sql-database-managed-instance).


## <a name="dns-refresh"></a>DNS-Aktualisierung 

**Kann ich eine DNS-Aktualisierung ausführen?**

Derzeit bieten wir keine Funktion zum Aktualisieren der DNS-Serverkonfiguration für SQL Managed Instance.

Die DNS-Konfiguration wird letztendlich aktualisiert:

- Wenn die DHCP-Lease abläuft.
- Bei einem Plattformupgrade.

Als Problemumgehung können Sie ein Downgrade der SQL Managed Instance auf 4 virtuelle Kerne durchführen und danach ein Upgrade der Instanz durchführen. Dies hat die Nebenwirkung, dass die DNS-Konfiguration aktualisiert wird.


## <a name="ip-address"></a>IP-Adresse

**Kann ich mithilfe der IP-Adresse eine Verbindung mit SQL Managed Instance herstellen?**

Das Herstellen einer Verbindung mit SQL Managed Instance mithilfe der IP-Adresse wird nicht unterstützt. Der Hostname der SQL Managed Instance wird dem Lastenausgleich (Load Balancer, LB) vor dem virtuellen Cluster der SQL Managed Instance zugeordnet. Da ein einziger virtueller Cluster mehrere SQL Managed Instances hosten könnte, kann die Verbindung nicht an die richtige SQL Managed Instance geroutet werden, ohne deren Namen explizit anzugeben.

Weitere Informationen zur Architektur des virtuellen Clusters für SQL Managed Instances finden Sie unter [Verbindungsarchitektur für virtuelle Cluster](connectivity-architecture-overview.md#virtual-cluster-connectivity-architecture).

**Kann eine SQL Managed Instance eine statische IP-Adresse aufweisen?**

In seltenen, jedoch erforderlichen Situationen müssen Sie eine Onlinemigration einer SQL Managed Instance zu einem neuen virtuellen Cluster durchführen. Gegebenenfalls ist diese Migration aufgrund von Änderungen in unserem Technologiestapel erforderlich, mit denen die Sicherheit und Zuverlässigkeit des Diensts verbessert werden sollen. Die Migration zu einem neuen virtuellen Cluster führt zum Ändern der IP-Adresse, die dem Hostnamen der SQL Managed Instance zugeordnet ist. Der SQL Managed Instance-Dienst beansprucht keine Unterstützung statischer IP-Adressen und behält sich das Recht vor, die IP-Adresse im Rahmen regulärer Wartungszyklen zu ändern.

Aus diesem Grund wird dringend empfohlen, keine statische IP-Adresse zu verwenden, da dies zu unnötigen Ausfallzeiten führen kann.

## <a name="change-time-zone"></a>Ändern der Zeitzone

**Kann ich die Zeitzone für eine vorhandene SQL Managed Instance ändern?**

Die Zeitzonenkonfiguration kann festgelegt werden, wenn eine SQL Managed Instance zum ersten Mal bereitgestellt wird. Das Ändern der Zeitzone der vorhandenen SQL Managed Instance wird nicht unterstützt. Weitere Informationen finden Sie unter [Zeitzonenbeschränkungen](timezones-overview.md#limitations).

Als Problemumgehung können Sie eine neue SQL Managed Instance mit der richtigen Zeitzone erstellen und dann entweder eine Sicherung und Wiederherstellung oder eine [instanzübergreifende Point-in-Time-Wiederherstellung](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/06/07/cross-instance-point-in-time-restore-in-azure-sql-database-managed-instance/) durchführen (von uns empfohlen).


## <a name="resolve-performance-issues"></a>Beheben von Leistungsproblemen

**Wie löse ich Leistungsprobleme bei meiner SQL Managed Instance?**

Als Ausgangspunkt für einen Leistungsvergleich zwischen SQL Managed Instance und SQL Server empfiehlt sich der Artikel [Best practices for performance comparison between Azure SQL Managed Instance and SQL Server](https://techcommunity.microsoft.com/t5/Azure-SQL-Database/The-best-practices-for-performance-comparison-between-Azure-SQL/ba-p/683210) (Bewährte Methoden für den Leistungsvergleich zwischen Azure SQL Managed Instance und SQL Server, in englischer Sprache).

Das Laden von Daten erfolgt aufgrund des obligatorischen vollständigen Wiederherstellungsmodells und der [Einschränkungen](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-resource-limits#service-tier-characteristics) für den Schreibdurchsatz des Transaktionsprotokolls in SQL Managed Instance häufig langsamer als in SQL Server. Dies kann manchmal umgangen werden, indem vorübergehende Daten in tempdb statt einer Benutzerdatenbank gespeichert oder gruppierte Columnstore-Tabellen oder speicheroptimierte Tabellen verwendet werden.


## <a name="restore-encrypted-backup"></a>Wiederherstellen verschlüsselter Sicherungen

**Kann ich meine verschlüsselte Datenbank in SQL Managed Instance wiederherstellen?**

Ja. Sie müssen die Datenbank nicht entschlüsseln, um sie in SQL Managed Instance wiederherzustellen. Sie müssen ein Zertifikat/einen als Verschlüsselungsschlüssel-Schutzvorrichtung verwendeten Schlüssel im Quellsystem für die SQL Managed Instance bereitstellen, um Daten aus der verschlüsselten Sicherungsdatei lesen zu können. Dies kann auf zwei Arten erreicht werden:

- *Laden Sie die Zertifikatschutzvorrichtung in die SQL Managed Instance hoch*. Das kann nur mithilfe von PowerShell geschehen. Im [Beispielskript](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-migrate-tde-certificate) wird der gesamte Prozess beschrieben.
- *Laden Sie eine asymmetrische Schlüsselschutzvorrichtung in Azure Key Vault (AKV) hoch, und verweisen Sie in SQL Managed Instance darauf*. Dieser Ansatz ähnelt dem TDE-Anwendungsfall Bring-Your-Own-Key (BYOK), in dem ebenfalls die AKV-Integration zum Speichern des Verschlüsselungsschlüssels verwendet wird. Wenn Sie den Schlüssel nicht als Schutzvorrichtung für den Verschlüsselungsschlüssel verwenden, sondern nur für SQL Managed Instance zum Wiederherstellen verschlüsselter Datenbanken zur Verfügung stellen möchten, befolgen Sie die Anweisungen zum [Einrichten von BYOK-TDE](https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-azure-sql#manage-transparent-data-encryption), und aktivieren Sie nicht das Kontrollkästchen *Legen Sie den ausgewählten Schlüssel als TDE-Standardschutzvorrichtung fest*.

Nachdem Sie die Verschlüsselungsschutzvorrichtung für SQL Managed Instance verfügbar gemacht haben, können Sie mit dem Standardverfahren für die Datenbankwiederherstellung fortfahren.

## <a name="migrate-from-sql-database"></a>Migrieren von SQL-Datenbank 

**Wie kann ich eine Migration von Azure SQL-Datenbank zu SQL Managed Instance durchführen?**

SQL Managed Instance bietet die gleichen Leistungsstufen pro Compute- und Speichergröße wie Azure SQL-Datenbank. Wenn Sie Daten in einer einzelnen Instanz konsolidieren möchten oder einfach eine Funktion ausschließlich in SQL Managed Instance unterstützt werden soll, können Sie die Daten mithilfe der Export-/Import-Funktion (BACPAC) migrieren.

## <a name="password-policy"></a>Kennwortrichtlinie 

**Welche Kennwortrichtlinien gelten für SQL-Anmeldungen in SQL Managed Instance?**

Die SQL Managed Instance-Kennwortrichtlinie für SQL-Anmeldungen erbt Azure-Plattformrichtlinien, die auf die VMs angewendet werden, die einen virtuellen Cluster bilden, der die verwaltete Instanz enthält. Zurzeit ist es nicht möglich, irgendeine dieser Einstellungen zu ändern, da diese Einstellungen durch Azure definiert und von der verwalteten Instanz geerbt sind.

 > [!IMPORTANT]
 > Die Azure-Plattform kann die Richtlinienanforderungen ändern, ohne Dienste zu benachrichtigen, die von diesen Richtlinien abhängen.

**Was sind die aktuellen Azure Platform-Richtlinien?**

Für jeden Anmeldenamen muss das Kennwort bei der Anmeldung festgelegt und nach dem Erreichen des maximalen Alters geändert werden.

| **Richtlinie** | **Sicherheitseinstellung** |
| --- | --- |
| Maximales Kennwortalter | 42 Tage |
| Minimales Kennwortalter | 1 Tag |
| Minimale Kennwortlänge | 10 Zeichen |
| Das Kennwort muss den Komplexitätsanforderungen entsprechen | Aktiviert |

**Ist es möglich, die Kennwortkomplexität und den Ablauf in SQL Managed Instance auf Anmeldeebene zu deaktivieren?**

Ja, es ist möglich, die Felder CHECK_POLICY und CHECK_EXPIRATION auf der Anmeldeebene zu steuern. Sie können die aktuellen Einstellungen überprüfen, indem Sie den folgenden T-SQL-Befehl ausführen:

```sql
SELECT *
FROM sys.sql_logins
```

Anschließend können Sie die angegebenen Anmeldeeinstellungen ändern, indem Sie Folgendes ausführen:

```sql
ALTER LOGIN <login_name> WITH CHECK_POLICY = OFF;
ALTER LOGIN <login_name> WITH CHECK_EXPIRATION = OFF;
```

(ersetzen Sie ‚test‘ durch den gewünschten Anmeldenamen, und passen Sie Werte für Richtlinie und Ablauf an).
