---
title: Häufig gestellte Fragen (FAQ)
titleSuffix: Azure SQL Managed Instance
description: Häufig gestellte Fragen (FAQ) zu Azure SQL Managed Instance
services: sql-database
ms.service: sql-managed-instance
ms.subservice: operations
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein, carlrab
ms.date: 03/17/2020
ms.openlocfilehash: d2e4b07c97e09fce5cdaa034e2fe67a18ef0d7f1
ms.sourcegitcommit: 1e6c13dc1917f85983772812a3c62c265150d1e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2020
ms.locfileid: "86171158"
---
# <a name="azure-sql-managed-instance-frequently-asked-questions-faq"></a>Häufig gestellte Fragen (FAQ) zu Azure SQL Managed Instance
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Dieser Artikel enthält die am häufigsten gestellten Fragen zu [Azure SQL Managed Instance](sql-managed-instance-paas-overview.md).

## <a name="supported-features"></a>Unterstützte Features

**Wo finde ich eine Liste der unterstützten Funktionen in SQL Managed Instance?**

Eine Liste der in SQL Managed Instance unterstützten Funktionen finden Sie unter [Azure SQL Managed Instance: Features](../database/features-comparison.md).

Informationen zu den Unterschieden in Syntax und Verhalten zwischen Azure SQL Managed Instance und SQL Server finden Sie unter [T-SQL-Unterschiede zu SQL Server](transact-sql-tsql-differences-sql-server.md).


## <a name="technical-specification-resource-limits-and-other-limitations"></a>Technische Spezifikation, Ressourceneinschränkungen und andere Einschränkungen
 
**Wo finde ich technische Merkmale und Ressourceneinschränkungen für SQL Managed Instance?**

Informationen zu den verfügbaren Merkmalen der Hardwaregenerationen finden Sie unter [Technische Unterschiede zwischen Hardwaregenerationen](resource-limits.md#hardware-generation-characteristics).
Informationen zu den verfügbaren Dienstebenen und ihren Merkmalen finden Sie unter [Technische Unterschiede zwischen Dienstebenen](resource-limits.md#service-tier-characteristics).

**Für welche Dienstebene bin ich qualifiziert?**

Jeder Kunde ist für alle Dienstebenen qualifiziert. Falls Sie aber Ihre vorhandenen Lizenzen gegen vergünstigte Tarife in Azure SQL Managed Instance über den [Azure-Hybridvorteil](https://azure.microsoft.com/pricing/hybrid-benefit/) eintauschen möchten, sollten Sie Folgendes beachten: SQL Server Enterprise Edition-Kunden mit Software Assurance sind für die Leistungsstufen [Universell](../database/service-tier-general-purpose.md) oder [Unternehmenskritisch](../database/service-tier-business-critical.md) qualifiziert, und SQL Server Standard Edition-Kunden mit Software Assurance sind nur für die Leistungsstufe „Universell“ qualifiziert. Weitere Informationen finden Sie unter [Welche spezifischen Rechte gelten für den Azure-Hybridvorteil für SQL Server?](../azure-hybrid-benefit.md?tabs=azure-powershell#what-are-the-specific-rights-of-the-azure-hybrid-benefit-for-sql-server).

**Welche Abonnementtypen werden für SQL Managed Instance unterstützt?**

Eine Liste mit den unterstützten Abonnementtypen finden Sie unter [Unterstützte Abonnementtypen](resource-limits.md#supported-subscription-types). 

**Welche Azure-Regionen werden unterstützt?**

Verwaltete Instanzen können in den meisten Azure-Regionen erstellt werden. Weitere Informationen finden Sie auf der Seite mit den [unterstützten Regionen für SQL Managed Instance](https://azure.microsoft.com/global-infrastructure/services/?products=sql-database&regions=all). [Senden Sie eine Supportanfrage über das Azure-Portal](../database/quota-increase-request.md), wenn Sie eine verwaltete Instanz in einer Region benötigen, die derzeit nicht unterstützt wird.

**Gelten für SQL Managed Instance-Bereitstellungen Kontingenteinschränkungen?**

Die verwaltete Instanz verfügt über zwei Standardeinschränkungen: eine Einschränkung der Anzahl von Subnetzen, die Sie verwenden können, und eine Einschränkung der Anzahl von virtuellen Kernen, die Sie bereitstellen können. Die Einschränkungen variieren je nach Abonnementtyp und Region. Eine Liste mit den Einschränkungen für regionale Ressourcen nach Abonnementtyp finden Sie in der Tabelle unter [Regionale Ressourcenbeschränkungen](resource-limits.md#regional-resource-limitations). Dies sind weiche Einschränkungen, die bei Bedarf erweitert werden können. Senden Sie eine Supportanfrage zum Erhöhen des Kontingents über das Azure-Portal, wenn Sie mehr verwaltete Instanzen in Ihren aktuellen Regionen bereitstellen müssen. Weitere Informationen finden Sie unter [Anfordern von Kontingenterhöhungen für Azure SQL-Datenbank](../database/quota-increase-request.md).

**Kann ich die maximale Anzahl von Datenbanken (100) auf meiner verwalteten Instanz bedarfsgesteuert erhöhen?**

Nein. Derzeit ist nicht geplant, die Anzahl von Datenbanken für eine SQL Managed Instance zu erhöhen. 

**Wohin kann ich eine Migration durchführen, wenn ich über mehr als 8 TB an Daten verfüge?**
Sie können eine Migration zu anderen Azure-Varianten erwägen, die zu Ihrer Workload passen: [Hyperscale in Azure SQL-Datenbank](../database/service-tier-hyperscale.md) oder [SQL Server auf virtuellen Azure-Computern](../virtual-machines/windows/sql-server-on-azure-vm-iaas-what-is-overview.md).

**Wohin kann ich eine Migration durchführen, wenn ich über bestimmte Hardwareanforderungen verfüge, z. B. ein höheres Verhältnis von Arbeitsspeicher zu virtuellen Kernen oder weitere CPUs?**
Sie können erwägen, eine Migration zu [SQL Server auf virtuellen Azure-Computern](../virtual-machines/windows/sql-server-on-azure-vm-iaas-what-is-overview.md) oder zu [Azure SQL-Datenbank](../database/sql-database-paas-overview.md) mit Arbeitsspeicher- bzw. CPU-Optimierung durchzuführen.

## <a name="known-issues--bugs"></a>Bekannte Probleme und Fehler

**Wo finde ich bekannte Probleme und Fehler?**

Informationen zu Programmfehlern und bekannten Problemen finden Sie unter [Bekannte Probleme](../database/doc-changes-updates-release-notes.md#known-issues).

## <a name="new-features"></a>Neue Funktionen

**Wo finde ich die neuesten Features sowie die Features in der öffentlichen Vorschau?**

Neue Features und Previewfunktionen finden Sie in den [Versionshinweisen](../database/doc-changes-updates-release-notes.md?tabs=managed-instance).

## <a name="create-update-delete-or-move-sql-managed-instance"></a>Erstellen, Aktualisieren, Löschen oder Verschieben von SQL Managed Instance

**Wie kann ich SQL Managed Instance bereitstellen?**

Sie können eine Instanz per [Azure-Portal](instance-create-quickstart.md), [PowerShell](scripts/create-configure-managed-instance-powershell.md), [Azure CLI](https://techcommunity.microsoft.com/t5/azure-sql-database/create-azure-sql-managed-instance-using-azure-cli/ba-p/386281) und mit [ARM-Vorlagen](https://docs.microsoft.com/archive/blogs/sqlserverstorageengine/creating-azure-sql-managed-instance-using-arm-templates) bereitstellen.

**Kann ich verwaltete Instanzen in einem vorhandenen Abonnement bereitstellen?**

Ja. Sie können eine verwaltete Instanz in einem vorhandenen Abonnement bereitstellen, wenn es zu den [unterstützten Abonnementtypen](resource-limits.md#supported-subscription-types) gehört.

**Warum kann ich keine verwaltete Instanz in einem Subnetz bereitstellen, dessen Name mit einer Ziffer beginnt?**

Dies ist eine aktuelle Einschränkung der zugrunde liegenden Komponente, bei der der Subnetzname anhand regulärer Ausdrücke überprüft wird: ^[a-zA-Z_][^\\\/\:\*\?\"\<\>\|\`\'\^]*(?<![\.\s])$. Alle Namen, die die Überprüfung mit regulären Ausdrücken bestehen und gültige Subnetznamen sind, werden derzeit unterstützt.

**Wie kann ich meine verwaltete Instanz skalieren?**

Sie können Ihre verwaltete Instanz per [Azure-Portal](../database/service-tiers-vcore.md?tabs=azure-portal#selecting-a-hardware-generation), [PowerShell](https://docs.microsoft.com/archive/blogs/sqlserverstorageengine/change-size-azure-sql-managed-instance-using-powershell), [Azure CLI](https://docs.microsoft.com/cli/azure/sql/mi?view=azure-cli-latest#az-sql-mi-update) oder mit [ARM-Vorlagen](https://docs.microsoft.com/archive/blogs/sqlserverstorageengine/updating-azure-sql-managed-instance-properties-using-arm-templates) skalieren.

**Kann ich meine verwaltete Instanz aus einer Region in eine andere verschieben?**

Ja, das ist möglich. Eine Anleitung finden Sie unter [Verschieben von Ressourcen in eine neue Region](../database/move-resources-across-regions.md).

**Wie kann ich meine verwaltete Instanz löschen?**

Sie können verwaltete Instanzen per Azure-Portal, [PowerShell](https://docs.microsoft.com/powershell/module/az.sql/remove-azsqlinstance?view=azps-4.3.0), [Azure CLI](https://docs.microsoft.com/cli/azure/sql/mi?view=azure-cli-latest#az-sql-mi-delete) oder mit [Resource Manager-REST-APIs](https://docs.microsoft.com/rest/api/sql/managedinstances/delete) löschen.

**Wie lange dauert es, eine Instanz zu erstellen oder zu aktualisieren oder eine Datenbank wiederherzustellen?**

Die erwartete Dauer für die Erstellung einer neuen verwalteten Instanz oder die Änderung von Dienstebenen (virtuelle Kerne, Speicher) hängt von mehreren Faktoren ab. Weitere Informationen finden Sie unter [Verwaltungsvorgänge](sql-managed-instance-paas-overview.md#management-operations).
 
## <a name="naming-conventions"></a>Benennungskonventionen

**Kann eine verwaltete Instanz über den gleichen Namen wie eine lokale SQL Server-Instanz verfügen?**

Die Namensänderung für eine verwaltete Instanz wird nicht unterstützt.

**Kann ich das Präfix der DNS-Zone ändern?**

Ja. Die DNS-Standardzone *.database.windows.net* einer verwalteten Instanz kann geändert werden. 

So verwenden Sie statt der Standardzone eine andere DNS-Zone, z. B. *.contoso.com*: 
- Verwenden Sie „CliConfig“ zum Definieren eines Alias. Weil das Tool nur ein Wrapper für Registrierungseinstellungen ist, kann es auch mithilfe einer Gruppenrichtlinie oder eines Skripts ausgeführt werden.
- Verwenden Sie *CNAME* mit der Option *TrustServerCertificate=true*.

## <a name="move-a-database-from-sql-managed-instance"></a>Verschieben einer Datenbank aus SQL Managed Instance 

**Wie kann ich eine Datenbank aus SQL Managed Instance zurück zu SQL Server oder Azure SQL-Datenbank verschieben?**

Sie können [eine Datenbank in eine BACPAC-Datei exportieren](../database/database-export.md) und dann [die BACPAC-Datei importieren](../database/database-import.md). Dieser Ansatz wird empfohlen, wenn die Datenbank kleiner als 100 GB ist.

Transaktionsreplikation kann verwendet werden, wenn alle Tabellen in der Datenbank Primärschlüssel aufweisen.

Native `COPY_ONLY`-Sicherungen von SQL Managed Instance können nicht in SQL Server wiederhergestellt werden, da SQL Managed Instance eine höhere Datenbankversion als SQL Server aufweist.

## <a name="migrate-an-instance-database"></a>Migrieren einer Instanzdatenbank

**Wie kann ich meine Instanzdatenbank zu Azure SQL-Datenbank migrieren?**

Eine Option ist das [Exportieren der Datenbank in eine BACPAC-Datei](../database/database-export.md) und das anschließende [Importieren der BACPAC-Datei](../database/database-import.md). 

Dieser Ansatz wird empfohlen, wenn die Datenbank kleiner als 100 GB ist. Transaktionsreplikation kann verwendet werden, wenn alle Tabellen in der Datenbank Primärschlüssel aufweisen.

## <a name="switch-hardware-generation"></a>Wechseln der Hardwaregeneration 

**Kann ich für meine SQL Managed Instance online zwischen der Hardwaregeneration Gen 4 und Gen 5 wechseln?**

Der automatische Onlinewechsel zwischen Hardwaregenerationen ist möglich, wenn beide Hardwaregenerationen in der Region verfügbar sind, in der SQL Managed Instance bereitgestellt wurde. In diesem Fall können Sie auf der [Seite mit der Übersicht über das V-Kern-Modell](../database/service-tiers-vcore.md) Informationen dazu anzeigen, wie Sie zwischen den Hardwaregenerationen wechseln können.

Hierbei handelt es sich um einen Vorgang mit langer Ausführungszeit, da eine neue verwaltete Instanz im Hintergrund bereitgestellt wird und Datenbanken automatisch zwischen der alten und neuen Instanz mit einem schnellen Failover am Ende des Prozesses übertragen werden. 

**Was geschieht, wenn die beiden Hardwaregenerationen nicht in derselben Region unterstützt werden?**

Wenn nicht beide Hardwaregenerationen in der gleichen Region unterstützt werden, kann die Hardwaregeneration nur manuell gewechselt werden. Hierzu müssen Sie eine neue Instanz in der Region bereitstellen, in der die gewünschte Hardwaregeneration verfügbar ist, und die Daten zwischen der alten und neuen Instanz manuell sichern und wiederherstellen.

**Was geschieht, wenn nicht genügend IP-Adressen zum Ausführen des Updatevorgangs vorhanden sind?**

Falls nicht genügend IP-Adressen im Subnetz vorhanden sind, in dem die verwaltete Instanz bereitgestellt wird, müssen Sie ein neues Subnetz und darin eine neue verwaltete Instanz erstellen. Wir empfehlen Ihnen auch, das neue Subnetz mit mehr zugeordneten IP-Adressen zu erstellen, damit ähnliche Situationen bei zukünftigen Updatevorgängen vermieden werden können. (Informationen zur richtigen Subnetzgröße finden Sie im Artikel zum [Ermitteln der Größe eines VNET-Subnetzes](vnet-subnet-determine-size.md).) Nachdem die neue Instanz bereitgestellt wurde, können Sie Daten zwischen der alten und der neuen Instanz manuell sichern und wiederherstellen. Sie können auch eine instanzübergreifende [Point-in-Time-Wiederherstellung](point-in-time-restore.md?tabs=azure-powershell) durchführen. 


## <a name="tune-performance"></a>Optimieren der Leistung

**Wie optimiere ich die Leistung für SQL Managed Instance?**

Da für SQL Managed Instance im Tarif „Universell“ Remotespeicher verwendet wird, wirkt sich die Größe der Daten- und Protokolldateien auf die Leistung aus. Weitere Informationen finden Sie unter [Auswirkung der Protokolldateigröße auf die Leistung einer universellen SQL Managed Instance](https://medium.com/azure-sqldb-managed-instance/impact-of-log-file-size-on-general-purpose-managed-instance-performance-21ad170c823e).

Wenn Ihre Workload aus vielen kleinen Transaktionen besteht, sollten Sie den Verbindungstyp von Proxymodus in Umleitungsmodus ändern.

## <a name="maximum-storage-size"></a>Maximale Speichergröße

**Was ist die maximale Speichergröße für SQL Managed Instance?**

Die Speichergröße für SQL Managed Instance hängt von der ausgewählten Dienstebene („Universell“ oder „Unternehmenskritisch“) ab. Informationen zu den Speichereinschränkungen dieser Dienstebenen finden Sie unter [Dienstebenen für Azure SQL-Datenbank und Azure SQL Managed Instance](../database/service-tiers-general-purpose-business-critical.md).

  
## <a name="networking-requirements"></a>Netzwerkanforderungen 

**Welche Einschränkungen in Bezug auf den ein- und ausgehenden NSG-Datenverkehr gelten derzeit für Subnetze von verwalteten Instanzen?**

Die erforderlichen NSG- und UDR-Regeln sind [hier](connectivity-architecture-overview.md#mandatory-inbound-security-rules-with-service-aided-subnet-configuration) dokumentiert und werden vom Dienst automatisch festgelegt.
Beachten Sie, dass es sich bei diesen Regeln lediglich um die Regeln handelt, die wir für die Wartung des Diensts benötigen. Für die Verbindungsherstellung mit der verwalteten Instanz und die Verwendung anderer Features müssen Sie weitere featurespezifische Regeln festlegen und pflegen.

**Wie kann ich NSG-Regeln für eingehenden Datenverkehr an Verwaltungsports festlegen?**

SQL Managed Instance ist für das Festlegen von Regeln auf Verwaltungsports zuständig. Dies wird mit einer Funktion mit dem Namen [dienstgestützte Subnetzkonfiguration](connectivity-architecture-overview.md#service-aided-subnet-configuration) erreicht.
Hiermit wird der unterbrechungsfreie Fluss von Verwaltungsdatenverkehr sichergestellt, um eine SLA zu erfüllen.

**Kann ich die Quell-IP-Adressbereiche abrufen, die für den eingehenden Verwaltungsdatenverkehr verwendet werden?**

Ja. Sie können Datenverkehr analysieren, der über Ihre Netzwerksicherheitsgruppen eingeht, indem Sie [Network Watcher-Flussprotokolle konfigurieren](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview#analyze-traffic-to-or-from-a-network-security-group).

**Kann ich die NSG so festlegen, dass der Zugriff auf den Datenendpunkt (Port 1433) gesteuert wird?**

Ja. Nachdem eine verwaltete Instanz bereitgestellt wurde, können Sie eine NSG festlegen, die den Zugriff in Eingangsrichtung auf Port 1433 steuert. Wir empfehlen Ihnen, den zugehörigen IP-Adressbereich so weit wie möglich zu begrenzen.

**Kann ich das virtuelle Netzwerkgerät oder die lokale Firewall so festlegen, dass der ausgehende Verwaltungsdatenverkehr basierend auf FQDNs gefiltert wird?**

Nein. Dies wird aus verschiedenen Gründen nicht unterstützt:
-   Das Routing von Datenverkehr als Antwort auf eine eingehende Verwaltungsanforderung wäre asymmetrisch und würde nicht funktionieren.
-   Das Routing von Datenverkehr in den Speicher würde durch Durchsatzeinschränkungen und Latenz beeinträchtigt werden. Auf diese Weise ist es also nicht möglich, die erwartete Dienstqualität und -verfügbarkeit bereitzustellen.
-   Die Erfahrung hat gezeigt, dass diese Konfigurationen fehleranfällig sind und nicht unterstützt werden.

**Kann ich das virtuelle Netzwerkgerät oder die Firewall für ausgehenden Datenverkehr festlegen, bei dem es sich nicht um Verwaltungsdatenverkehr handelt?**

Ja. Dies lässt sich am einfachsten erreichen, indem eine 0/0-Regel einer UDR hinzugefügt wird, die dem Subnetz einer verwalteten Instanz zugeordnet ist. So kann Datenverkehr über das virtuelle Netzwerkgerät geleitet werden.
 
**Wie viele IP-Adressen benötige ich für eine verwaltete Instanz?**

Das Subnetz muss eine ausreichende Anzahl verfügbarer [IP-Adressen](connectivity-architecture-overview.md#network-requirements) aufweisen. Informationen zur Ermittlung der Größe von VNET-Subnetzen für SQL Managed Instance finden Sie unter [Bestimmen von Subnetzgröße und -bereich für Azure SQL Managed Instance](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-determine-size-vnet-subnet). 

**Was geschieht, wenn nicht genügend IP-Adressen zum Ausführen des Updatevorgangs für Instanzen vorhanden sind?**

Falls nicht genügend [IP-Adressen](connectivity-architecture-overview.md#network-requirements) im Subnetz vorhanden sind, in dem die verwaltete Instanz bereitgestellt wird, müssen Sie ein neues Subnetz und darin eine neue verwaltete Instanz erstellen. Wir empfehlen Ihnen auch, das neue Subnetz mit mehr zugeordneten IP-Adressen zu erstellen, damit ähnliche Situationen bei zukünftigen Updatevorgängen vermieden werden können. Nachdem die neue Instanz bereitgestellt wurde, können Sie Daten zwischen der alten und der neuen Instanz manuell sichern und wiederherstellen. Sie können auch eine instanzübergreifende [Point-in-Time-Wiederherstellung](point-in-time-restore.md?tabs=azure-powershell) durchführen.

**Benötige ich zum Erstellen einer verwalteten Instanz ein leeres Subnetz?**

Nein. Sie können entweder ein leeres Subnetz oder ein Subnetz verwenden, das bereits ein oder mehrere verwaltete Instanzen enthält. 

**Kann ich den Adressbereich des Subnetzes ändern?**

Dies ist nicht möglich, wenn darin verwaltete Instanzen enthalten sind. Dies ist eine Einschränkung der Azure-Netzwerkinfrastruktur. Es ist für Sie nur zulässig, [einem leeren Subnetz weiteren Adressraum hinzuzufügen](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-subnet#change-subnet-settings). 

**Kann ich meine verwaltete Instanz in ein anderes Subnetz verschieben?**

Nein. Dies ist eine derzeit geltende Einschränkung für das Entwerfen von verwalteten Instanzen. Sie können aber eine neue Instanz in einem anderen Subnetz bereitstellen und Daten zwischen der alten und der neuen Instanz manuell sichern und wiederherstellen. Darüber hinaus können Sie auch eine instanzübergreifende [Point-in-Time-Wiederherstellung](point-in-time-restore.md?tabs=azure-powershell) durchführen.

**Benötige ich zum Erstellen einer verwalteten Instanz ein leeres virtuelles Netzwerk?**

Dies ist nicht erforderlich. Sie können entweder [ein virtuelles Netzwerk für Azure SQL Managed Instance erstellen](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-create-vnet-subnet) oder [ein vorhandenes virtuelles Netzwerk für Azure SQL Managed Instance konfigurieren](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-configure-vnet-subnet).

**Kann ich eine verwaltete Instanz zusammen mit anderen Diensten in einem Subnetz anordnen?**

Nein. Derzeit wird das Anordnen einer verwalteten Instanz in einem Subnetz, das bereits andere Ressourcentypen enthält, nicht unterstützt.


## <a name="mitigate-data-exfiltration-risks"></a>Mindern von Risiken bei der Datenexfiltration  

**Wie kann ich Risiken bei der Datenexfiltration mindern?**

Kunden wird empfohlen, zum Mindern von Risiken bei der Datenexfiltration eine Reihe von Sicherheitseinstellungen und -kontrollen anzuwenden:

- Aktivieren Sie [Transparent Data Encryption (TDE)](https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-azure-sql) für alle Datenbanken.
- Deaktivieren Sie die Common Language Runtime (CLR). Dies wird auch für lokale Umgebungen empfohlen.
- Verwenden Sie nur die Azure AD-Authentifizierung (Azure Active Directory).
- Greifen Sie auf die Instanz mit einem DBA-Konto mit geringen Rechten zu.
- Konfigurieren Sie den Zugriff auf die JIT-Jumpbox für das Systemadministratorkonto.
- Aktivieren Sie [SQL-Überwachung](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine), und integrieren Sie sie in Warnungsmechanismen.
- Aktivieren Sie die [Bedrohungserkennung](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection) über das [Advanced Data Security](https://docs.microsoft.com/azure/sql-database/sql-database-advanced-data-security)-Paket (ADS).


## <a name="cost-saving-use-cases"></a>Anwendungsfälle für Kosteneinsparungen

**Wo finde ich Anwendungsfälle und entsprechende Kosteneinsparungen für SQL Managed Instance?**

Fallstudien zu SQL Managed Instance:

- [Komatsu](https://customers.microsoft.com/story/komatsu-australia-manufacturing-azure)
- [KMD](https://customers.microsoft.com/en-ca/story/kmd-professional-services-azure-sql-database)
- [PowerDETAILS](https://customers.microsoft.com/story/powerdetails-partner-professional-services-azure-sql-database-managed-instance)
- [Allscripts](https://customers.microsoft.com/story/allscripts-partner-professional-services-azure)

Es gibt auch eine Forrester-Studie, die ein besseres Verständnis der Vorteile, Kosten und Risiken im Zusammenhang mit der Bereitstellung von Azure SQL Managed Instance vermittelt: [The Total Economic Impact™ von Microsoft Azure SQL Database Managed Instance](https://azure.microsoft.com/resources/forrester-tei-sql-database-managed-instance).


## <a name="dns"></a>DNS

**Kann ich ein benutzerdefiniertes DNS für SQL Managed Instance konfigurieren?**

Ja. Informationen hierzu finden Sie unter [Konfigurieren eines benutzerdefinierten DNS für Azure SQL Managed Instance](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-custom-dns).

**Kann ich eine DNS-Aktualisierung ausführen?**

Derzeit bieten wir keine Funktion zum Aktualisieren der DNS-Serverkonfiguration für SQL Managed Instance.

Die DNS-Konfiguration wird letztendlich aktualisiert:

- Wenn die DHCP-Lease abläuft.
- Bei einem Plattformupgrade.

Als Problemumgehung können Sie für SQL Managed Instance ein Downgrade auf vier virtuelle Kerne und danach ein Upgrade der Instanz durchführen. Dies hat die Nebenwirkung, dass die DNS-Konfiguration aktualisiert wird.


## <a name="ip-address"></a>IP-Adresse

**Kann ich mithilfe einer IP-Adresse eine Verbindung mit SQL Managed Instance herstellen?**

Das Herstellen einer Verbindung mit SQL Managed Instance mithilfe einer IP-Adresse wird nicht unterstützt. Der Hostname von SQL Managed Instance wird einem Lastenausgleich (Load Balancer, LB) zugeordnet, der sich vor dem virtuellen Cluster von SQL Managed Instance befindet. Da von nur einem virtuellen Cluster mehrere verwaltete Instanzen gehostet werden können, kann die Verbindung nicht an die richtige verwaltete Instanz geroutet werden, ohne explizit deren Namen anzugeben.

Weitere Informationen zur Architektur des virtuellen Clusters für SQL Managed Instances finden Sie unter [Verbindungsarchitektur für virtuelle Cluster](connectivity-architecture-overview.md#virtual-cluster-connectivity-architecture).

**Kann SQL Managed Instance eine statische IP-Adresse aufweisen?**

In seltenen Fällen (die aber manchmal erforderlich sind) müssen Sie eine Onlinemigration von SQL Managed Instance zu einem neuen virtuellen Cluster durchführen. Gegebenenfalls ist diese Migration aufgrund von Änderungen in unserem Technologiestapel erforderlich, mit denen die Sicherheit und Zuverlässigkeit des Diensts verbessert werden sollen. Die Migration zu einem neuen virtuellen Cluster führt zum Ändern der IP-Adresse, die dem Hostnamen der SQL Managed Instance zugeordnet ist. Der SQL Managed Instance-Dienst beansprucht keine Unterstützung statischer IP-Adressen und behält sich das Recht vor, die IP-Adresse im Rahmen regulärer Wartungszyklen zu ändern.

Aus diesem Grund wird dringend empfohlen, keine statische IP-Adresse zu verwenden, da dies zu unnötigen Ausfallzeiten führen kann.

## <a name="change-time-zone"></a>Ändern der Zeitzone

**Kann ich die Zeitzone für eine vorhandene verwaltete Instanz ändern?**

Die Zeitzonenkonfiguration kann festgelegt werden, wenn eine verwaltete Instanz zum ersten Mal bereitgestellt wird. Das Ändern der Zeitzone einer vorhandenen verwalteten Instanz wird nicht unterstützt. Weitere Informationen finden Sie unter [Zeitzonenbeschränkungen](timezones-overview.md#limitations).

Als Problemumgehung können Sie eine neue verwaltete Instanz mit der richtigen Zeitzone erstellen und dann entweder eine Sicherung und Wiederherstellung oder eine [instanzübergreifende Point-in-Time-Wiederherstellung](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/06/07/cross-instance-point-in-time-restore-in-azure-sql-database-managed-instance/) durchführen (unsere Empfehlung).


## <a name="security-and-database-encryption"></a>Sicherheit und Datenbankverschlüsselung

**Ist die sysadmin-Serverrolle für SQL Managed Instance verfügbar?**

Ja. Kunden können Anmeldungen erstellen, bei denen es sich um Mitglieder der sysadmin-Rolle handelt.  Kunden, die über die sysadmin-Berechtigung verfügen, sind auch für den Betrieb der Instanz verantwortlich. Dies kann negative Auswirkungen auf die Erfüllung der SLA-Anforderungen haben. Informationen zum Hinzufügen der Anmeldung zur sysadmin-Serverrolle finden Sie unter [Azure AD-Authentifizierung](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-aad-security-tutorial#azure-ad-authentication).

**Wird Transparent Data Encryption für SQL Managed Instance unterstützt?**

Ja. Transparent Data Encryption wird für SQL Managed Instance unterstützt. Weitere Informationen finden Sie unter [TDE (Transparent Data Encryption) für SQL-Datenbank, SQL Managed Instance und Azure Synapse Analytics](https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-azure-sql?tabs=azure-portal).

**Kann ich das Modell „Bring Your Own Key“ für TDE nutzen?**

Ja. Das Szenario „Azure Key Vault für BYOK“ ist für Azure SQL Managed Instance verfügbar. Weitere Informationen finden Sie unter [Azure SQL Transparent Data Encryption mithilfe eines kundenseitig verwalteten Schlüssels](https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-azure-sql?view=sql-server-ver15&tabs=azure-portal#customer-managed-transparent-data-encryption---bring-your-own-key).

**Kann ich eine verschlüsselte SQL Server-Datenbank migrieren?**

Ja, das ist möglich. Um eine verschlüsselte SQL Server-Datenbank zu migrieren, müssen Sie Ihre vorhandenen Zertifikate exportieren und in die verwaltete Instanz importieren und anschließend eine vollständige Datenbanksicherung erstellen und auf der verwalteten Instanz wiederherstellen. 

Sie können auch den [Azure Database Migration Service](https://azure.microsoft.com/services/database-migration/) nutzen, um die per TDE verschlüsselten Datenbanken zu migrieren.

**Wie kann ich die Rotation der TDE-Schutzvorrichtung für SQL Managed Instance konfigurieren?**

Sie können die TDE-Schutzvorrichtung für SQL Managed Instance per Azure Cloud Shell rotieren. Eine Anleitung finden Sie unter [Transparent Data Encryption in SQL Managed Instance mithilfe eines eigenen Schlüssels aus dem Azure Key Vault](scripts/transparent-data-encryption-byok-powershell.md).

**Kann ich meine verschlüsselte Datenbank in SQL Managed Instance wiederherstellen?**

Ja. Sie müssen die Datenbank nicht entschlüsseln, um sie in SQL Managed Instance wiederherzustellen. Sie müssen ein Zertifikat bzw. einen Schlüssel als Verschlüsselungsschlüssel-Schutzvorrichtung im Quellsystem für SQL Managed Instance bereitstellen, um Daten aus der verschlüsselten Sicherungsdatei lesen zu können. Dies kann auf zwei Arten erreicht werden:

- *Laden Sie die Zertifikatschutzvorrichtung in SQL Managed Instance hoch*. Das kann nur mithilfe von PowerShell geschehen. Im [Beispielskript](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-migrate-tde-certificate) wird der gesamte Prozess beschrieben.
- *Laden Sie eine asymmetrische Schlüsselschutzvorrichtung in Azure Key Vault hoch, und verweisen Sie in SQL Managed Instance darauf*. Dieser Ansatz ähnelt dem TDE-Anwendungsfall „Bring-Your-Own-Key“ (BYOK), in dem ebenfalls die Key Vault-Integration zum Speichern des Verschlüsselungsschlüssels verwendet wird. Wenn Sie den Schlüssel nicht als Schutzvorrichtung für den Verschlüsselungsschlüssel verwenden, sondern nur für SQL Managed Instance zum Wiederherstellen verschlüsselter Datenbanken zur Verfügung stellen möchten, befolgen Sie die Anweisungen zum [Einrichten von BYOK-TDE](https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-azure-sql#manage-transparent-data-encryption), und aktivieren Sie nicht das Kontrollkästchen **Legen Sie den ausgewählten Schlüssel als TDE-Standardschutzvorrichtung fest**.

Nachdem Sie die Verschlüsselungsschutzvorrichtung für SQL Managed Instance verfügbar gemacht haben, können Sie mit dem Standardverfahren für die Datenbankwiederherstellung fortfahren.

## <a name="purchasing-models-and-benefits"></a>Kaufmodelle und Vorteile

**Welche Kaufmodelle sind für SQL Managed Instance verfügbar?**

Für SQL Managed Instance ist das [vCore-basierte Kaufmodell](sql-managed-instance-paas-overview.md#vcore-based-purchasing-model) verfügbar.

**Welche Kostenvorteile sind für SQL Managed Instance verfügbar?**

Sie können mit den Azure SQL-Vorteilen wie folgt Kosten sparen:
-   Maximieren Sie die vorhandenen Investitionen in lokale Lizenzen, um mit dem [Azure-Hybridvorteil](https://docs.microsoft.com/azure/azure-sql/azure-hybrid-benefit?tabs=azure-powershell) bis zu 55 Prozent an Kosten zu sparen. 
-   Gehen Sie eine Verpflichtung zur Reservierung von Computeressourcen ein, um mit dem [Vorteil für reservierte Instanzen](https://docs.microsoft.com/azure/sql-database/sql-database-reserved-capacity) bis zu 33 Prozent der Kosten zu sparen. Kombinieren Sie dies mit dem Azure-Hybridvorteil, um Kosten in Höhe von bis zu 82 Prozent zu sparen. 
-   Sparen Sie bis zu 55 Prozent gegenüber den Listenpreisen, indem Sie den [Vorteil in Bezug auf Preise für Azure Dev/Test](https://azure.microsoft.com/pricing/dev-test/) nutzen. Hierbei erhalten Sie vergünstigte Preise für Ihre laufenden Entwicklungs- und Testworkloads.

**Wer ist für den Vorteil für reservierte Instanzen berechtigt?**

Um sich für den Vorteil für reservierte Instanzen zu qualifizieren, muss Ihr Abonnementtyp ein Enterprise Agreement (Angebotsnummern: MS-AZR-0017P oder MS-AZR-0148P) oder eine einzelne Vereinbarung mit Preisen für nutzungsbasierte Bezahlung (Angebotsnummern: MS-AZR-0003P oder MS-AZR-0023P) sein. Weitere Informationen zu Reservierungen finden Sie unter [Sparen von Kosten für Ressourcen mit reservierter Kapazität – Azure SQL-Datenbank und SQL Managed Instance](https://docs.microsoft.com/azure/sql-database/sql-database-reserved-capacity). 

**Können Reservierungen storniert, umgetauscht oder rückerstattet werden?**

Reservierungen können mit bestimmten Einschränkungen storniert, umgetauscht oder rückerstattet werden. Weitere Informationen finden Sie unter [Self-Service-Umtausch und -Rückerstattungen für Azure-Reservierungen](https://docs.microsoft.com/azure/cost-management-billing/reservations/exchange-and-refund-azure-reservations).

## <a name="billing-for-managed-instance-and-backup-storage"></a>Abrechnung für verwaltete Instanzen und Sicherungsspeicher

**Welche Preisoptionen gibt es für SQL Managed Instance?**

Informationen zu den Preisoptionen für SQL Managed Instance finden Sie auf der [Seite mit den Preisen](https://azure.microsoft.com/pricing/details/azure-sql/sql-managed-instance/single/).

**Wie kann ich die Abrechnungskosten für meine verwaltete Instanz nachverfolgen?**

Sie können zu diesem Zweck die [Azure Cost Management-Lösung](https://docs.microsoft.com/azure/cost-management-billing/) verwenden. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu **Abonnements**, und wählen Sie **Kostenanalyse** aus. 

Verwenden Sie die Option **Kumulierte Kosten**, und filtern Sie dann nach dem **Ressourcentyp** als `microsoft.sql/managedinstances`.

**Welche Kosten fallen für automatisierte Sicherungen an?**

Die Menge an freiem Speicherplatz für Sicherungen entspricht der Menge an reserviertem Datenspeicher, den Sie erworben haben. Die gilt unabhängig vom festgelegten Aufbewahrungszeitraum für die Sicherungen. Wenn sich der Verbrauch Ihres Sicherungsspeichers innerhalb der zugeordneten Menge an freiem Speicher für Sicherungen bewegt, fallen für Sie für automatisierte Sicherungen auf der verwalteten Instanz keine weiteren Kosten an. Sie sind also kostenlos. Eine Überschreitung des kostenlosen Sicherungsspeichers führt in Regionen in den USA zu monatlichen Kosten von ca. 0,20 bis 0,24 US-Dollar pro GB. Informationen zu den Preisdetails für Ihre Region finden Sie auf der Seite mit den Preisen. Ausführlichere Informationen finden Sie auf der Seite mit der [Beschreibung der Nutzung von Sicherungsspeicher](https://techcommunity.microsoft.com/t5/azure-sql-database/backup-storage-consumption-on-managed-instance-explained/ba-p/1390923).

**Wie kann ich die Abrechnungskosten für meinen Verbrauch des Sicherungsspeichers überwachen?**

Sie können die Kosten für den Sicherungsspeicher über das Azure-Portal überwachen. Eine Anleitung finden Sie unter [Überwachen der Kosten](https://docs.microsoft.com/azure/azure-sql/database/automated-backups-overview?tabs=managed-instance#monitor-costs). 

**Wie kann ich die Kosten für Sicherungsspeicher auf der verwalteten Instanz optimieren?**

Informationen zur Optimierung Ihrer Kosten für Sicherungsspeicher finden Sie unter [Optimieren von Sicherungen für SQL Managed Instance](https://techcommunity.microsoft.com/t5/azure-sql-database/fine-tuning-backup-storage-costs-on-managed-instance/ba-p/1390935).

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

## <a name="azure-feedback-and-support"></a>Azure-Feedback und -Support

**Wohin kann ich meine Ideen für Verbesserungen von SQL Managed Instance senden?**

Sie können Ihre Stimme für ein neues Feature von SQL Managed Instance abgeben oder eine neue Idee für eine Verbesserung im [Feedbackforum von SQL Managed Instance](https://feedback.azure.com/forums/915676-sql-managed-instance) zur Abstimmung bereitstellen. Auf diese Weise können Sie an der Produktentwicklung mitwirken und uns bei der Priorisierung von potenziellen Verbesserungen unterstützen.

**Wie kann ich eine Azure-Supportanfrage erstellen?**

Informationen zum Erstellen einer Azure-Supportanfrage finden Sie unter [Erstellen einer Azure-Supportanfrage](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request).

