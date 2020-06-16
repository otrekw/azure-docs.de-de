---
title: Was ist Azure SQL Managed Instance?
description: Erfahren Sie mehr über Azure SQL Managed Instance, die Ihnen nahezu 100 %ige Kompatibilität mit der neuesten SQL Server-Datenbankengine (Enterprise Edition) bietet
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: bonova
ms.author: bonova
ms.reviewer: sstein, carlrab, vanto
ms.date: 04/02/2020
ms.openlocfilehash: 886653bcfa255c7929f7e23082b74f171c97c7a1
ms.sourcegitcommit: 1f48ad3c83467a6ffac4e23093ef288fea592eb5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2020
ms.locfileid: "84195449"
---
# <a name="what-is-azure-sql-managed-instance"></a>Was ist Azure SQL Managed Instance?
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Azure SQL Managed Instance ist Teil der Azure SQL-Produktfamilie und stellt einen intelligenten, skalierbaren, cloudbasierten Datenbankdienst dar, der die breiteste Kompatibilität mit der SQL Server-Datenbankengine mit allen Vorzügen einer vollständig verwalteten und stets aktuellen Platform-as-a-Service kombiniert. Azure SQL Managed Instance zeichnet sich durch nahezu uneingeschränkte Kompatibilität mit der aktuellen SQL Server-Datenbank-Engine (Enterprise Edition) aus. Darüber hinaus bietet sie eine native Implementierung eines [virtuellen Netzwerks (VNET)](../../virtual-network/virtual-networks-overview.md) zur Behebung allgemeiner Sicherheitsrisiken sowie ein vorteilhaftes [Geschäftsmodell](https://azure.microsoft.com/pricing/details/sql-database/) für SQL Server-Bestandskunden. Über SQL Managed Instance können bestehende SQL Server-Kunden ihre lokalen Anwendungen mit minimalen Änderungen an den Anwendungen und Datenbanken per Lift & Shift zur Cloud migrieren. Gleichzeitig behält SQL Managed Instance alle PaaS-Funktionen (automatisches Patchen und automatische Versionsupdates, [automatische Sicherungen](../database/automated-backups-overview.md), [Hochverfügbarkeit](../database/high-availability-sla.md)) bei, die den Verwaltungsaufwand und die Gesamtkosten drastisch reduzieren.

> [!IMPORTANT]
> Eine Liste der Regionen, in denen SQL Managed Instance derzeit verfügbar ist, finden Sie unter [Unterstützte Regionen](resource-limits.md#supported-regions).

In der folgenden Abbildung sind die wichtigsten Features von SQL Managed Instances dargestellt:

![Wichtigste Features](./media/sql-managed-instance-paas-overview/key-features.png)

Azure SQL Managed Instance ist für Kunden konzipiert, die eine große Anzahl von Apps aus einer selbst erstellten oder über ISVs bereitgestellten lokalen Umgebung oder IaaS-Umgebung mit möglichst geringem Migrationsaufwand in eine vollständig verwaltete PaaS-Cloudumgebung migrieren möchten. Unter Verwendung des vollständig automatisierten [Data Migration Service (DMS)](../../dms/tutorial-sql-server-to-managed-instance.md#create-an-azure-database-migration-service-instance) in Azure können Kunden ihre vorhandenen SQL Server-Instanzen per Lift & Shift zu einer SQL Managed Instance migrieren, die Kompatibilität mit SQL Server und vollständige Isolation von Kundeninstanzen mit nativer VNET-Unterstützung bietet.  Mit Software Assurance können Sie Ihre vorhandenen Lizenzen mit dem [Azure-Hybridvorteil für SQL Server](https://azure.microsoft.com/pricing/hybrid-benefit/) zu ermäßigten Preisen gegen eine SQL Managed Instance austauschen. Eine SQL Managed Instance ist das beste Migrationsziel in der Cloud für SQL Server-Instanzen, die hohe Sicherheit und eine umfangreiche Programmieroberfläche erfordern.

## <a name="key-features-and-capabilities"></a>Wichtige Features und Funktionen

SQL Managed Instance kombiniert die besten Features aus Azure SQL-Datenbank und der SQL Server-Datenbank-Engine.

> [!IMPORTANT]
> SQL Managed Instance wird mit allen Features der neuesten Version von SQL Server ausgeführt – einschließlich Onlinevorgängen, automatischer Plankorrekturen und anderer Leistungsverbesserungen für Unternehmen. Einen Vergleich der verfügbaren Features finden Sie unter [Funktionsvergleich: Azure SQL Managed Instance im Vergleich mit SQL Server](../database/features-comparison.md).

| **PaaS-Vorteile** | **Geschäftskontinuität** |
| --- | --- |
|Kein Kauf und keine Verwaltung von Hardware <br>Kein zusätzlicher Aufwand für die Verwaltung der zugrunde liegenden Infrastruktur <br>Schnelle Bereitstellung und Dienstskalierung <br>Automatische Patches und Versionsupgrades <br>Integration in andere PaaS-Datendienste |Betriebszeit-SLA von 99,99 %  <br>Integrierte [Hochverfügbarkeit](../database/high-availability-sla.md) <br>Schutz der Daten durch [automatische Sicherungen](../database/automated-backups-overview.md) <br>Vom Kunden konfigurierbare Aufbewahrungsdauer für Sicherungen <br>Vom Benutzer initiierte [Sicherungen](https://docs.microsoft.com/sql/t-sql/statements/backup-transact-sql?view=azuresqldb-mi-current) <br>Funktion für [Point-in-Time-Datenbankwiederherstellung](../database/recovery-using-backups.md#point-in-time-restore) |
|**Sicherheit und Konformität** | **Verwaltung**|
|Isolierte Umgebung ([VNET-Integration](connectivity-architecture-overview.md), Dienst mit einzelnem Mandanten, dedizierte Compute- und Speicherressourcen) <br>[Transparent Data Encryption (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql)<br>[Azure AD-Authentifizierung](../database/authentication-aad-overview.md), Unterstützung für einmaliges Anmelden <br> <a href="/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current">Azure AD-Serverprinzipale (Anmeldungen)</a>  <br>Gleiche Kompatibilitätsstandards wie für Azure SQL-Datenbank <br>[SQL-Überwachung](auditing-configure.md) <br>[Advanced Threat Protection für Azure SQL-Datenbank](threat-detection-configure.md) |Azure Resource Manager-API zur Automatisierung der Dienstbereitstellung und -skalierung <br>Funktionen des Azure-Portals für die manuelle Dienstbereitstellung und -skalierung <br>Data Migration Service

> [!IMPORTANT]
> Azure SQL Managed Instance wurde anhand einer Reihe von Konformitätsstandards zertifiziert. Weitere Informationen finden Sie in den [Microsoft Azure Complianceangeboten](https://servicetrust.microsoft.com/ViewPage/MSComplianceGuideV3?command=Download&downloadType=Document&downloadId=44bbae63-bf4d-4e3b-9d3d-c96fb25ec363&tab=7027ead0-3d6b-11e9-b9e1-290b1eb4cdeb&docTab=7027ead0-3d6b-11e9-b9e1-290b1eb4cdeb_FAQ_and_White_Papers), unter denen die aktuelle Liste der Compliancezertifizierungen für SQL Managed Instance unter **SQL-Datenbank** aufgeführt ist.

Die wichtigsten Features von SQL Managed Instance sind in der folgenden Tabelle angegeben:

|Funktion | BESCHREIBUNG|
|---|---|
| SQL Server-Version/-Build | SQL Server-Datenbank-Engine (letzte stabile Version) |
| Verwaltete automatisierte Sicherungen | Ja |
| Integrierte Überwachung und Metriken von Instanzen und Datenbanken | Ja |
| Automatische Softwarepatches | Ja |
| Neueste Features der Datenbank-Engine | Ja |
| Anzahl der Datendateien (ROWS) pro Datenbank | Mehrere |
| Anzahl der Protokolldateien (LOG) pro Datenbank | 1 |
| VNET – Azure Resource Manager-Bereitstellung | Ja |
| VNET – Klassisches Bereitstellungsmodell | Nein |
| Portal-Unterstützung | Ja|
| Integrierter Integrationsdienst (SSIS) | Nein – SSIS ist Teil von [Azure Data Factory PaaS](https://docs.microsoft.com/azure/data-factory/tutorial-deploy-ssis-packages-azure) |
| Integrierter Analysedienst (SSAS) | Nein – SSAS ist ein separater [PaaS](https://docs.microsoft.com/azure/analysis-services/analysis-services-overview) |
| Integrierter Berichterstellungsdienst (SSRS) | Nein – verwenden Sie stattdessen [paginierte Berichte in Power BI ](https://docs.microsoft.com/power-bi/paginated-reports/paginated-reports-report-builder-power-bi), oder hosten Sie SSRS auf einer Azure-VM. In einer verwalteten Instanz kann SSRS nicht als Dienst ausgeführt werden, eine verwaltete Instanz kann jedoch mithilfe der SQL Server-Authentifizierung SSRS 2019-Katalogdatenbanken für einen externen Berichtsserver hosten. |
|||

## <a name="vcore-based-purchasing-model"></a>vCore-basiertes Kaufmodell

Das auf [virtuellen Kernen basierende Kaufmodell](../database/service-tiers-vcore.md) für SQL Managed Instance bietet Ihnen Flexibilität, Kontrolle und Transparenz sowie eine unkomplizierte Möglichkeit, Ihre lokalen Workloadanforderungen in der Cloud zu realisieren. Mit diesem Modell können Sie Computeressourcen, Arbeitsspeicher und Speicher entsprechend Ihren Workloadanforderungen ändern. Das auf virtuellen Kernen basierende Modell ermöglicht mit dem [Azure-Hybridvorteil für SQL Server](https://azure.microsoft.com/pricing/hybrid-benefit/) zudem Einsparungen von bis zu 55 Prozent.

Beim V-Kern-Modell können Sie verschiedene Hardwaregenerationen auswählen.

- **Gen4**: Logische CPUs basierend auf Intel-Prozessoren vom Typ E5-2673 v3 (Haswell) mit 2,4 GHz, angefügter SSD, physischen Kernen, 7 GB RAM pro Kern und Computegrößen zwischen 8 und 24 virtuellen Kernen.
- **Gen5**: Logische CPUs basieren auf Intel-Prozessoren vom Typ E5-2673 v4 (Broadwell) mit 2,3 GHz und Intel-Prozessoren vom Typ SP-8160 (Skylake), schnellem NVMe-SSD, einem logischem Kern mit Hyperthreading und Computegrößen zwischen vier und 80 Kernen.

Weitere Informationen zu den Unterschieden zwischen Hardwaregenerationen finden Sie unter [Ressourceneinschränkungen für SQL Managed Instance](resource-limits.md#hardware-generation-characteristics).

## <a name="service-tiers"></a>Dienstebenen

SQL Managed Instance ist in zwei Dienstebenen verfügbar:

- **Universell**: Entwickelt für Anwendungen mit typischen Leistungs- und E/A-Latenzanforderungen.
- **Unternehmenskritisch**: Entwickelt für Anwendungen mit niedrigen E/A-Latenzanforderungen und minimalen Auswirkungen auf zugrunde liegende Wartungsvorgänge für die Workload.

Beide Dienstebenen garantieren eine Verfügbarkeit von 99,99 Prozent und ermöglichen es Ihnen, die Speichergröße und Computekapazität unabhängig voneinander auszuwählen. Weitere Informationen zur Hochverfügbarkeitsarchitektur von Azure SQL Managed Instance finden Sie unter [Hochverfügbarkeit und Azure SQL Managed Instance](../database/high-availability-sla.md).

### <a name="general-purpose-service-tier"></a>Universelle Dienstebene

Folgende wichtige Merkmale kennzeichnen die universelle Dienstebene:

- Entwurf für den Großteil der Geschäftsanwendungen mit typischen Leistungsanforderungen
- Azure Blob Storage mit hoher Leistung (8 TB)
- Integrierte [Hochverfügbarkeit](../database/high-availability-sla.md#basic-standard-and-general-purpose-service-tier-availability) basierend auf zuverlässigem Azure Blob Storage und [Azure Service Fabric](../../service-fabric/service-fabric-overview.md)

Weitere Informationen finden Sie unter [Speicherebene im Tarif „Universell“](https://medium.com/azure-sqldb-managed-instance/file-layout-in-general-purpose-azure-sql-managed-instance-cf21fff9c76c) und [Bewährte Methoden und Aspekte der Speicherleistung für SQL Managed Instance (Universell)](https://blogs.msdn.microsoft.com/sqlcat/2018/07/20/storage-performance-best-practices-and-considerations-for-azure-sql-db-managed-instance-general-purpose/).

Weitere Informationen zu den Unterschieden zwischen Dienstebenen finden Sie unter [Ressourceneinschränkungen für SQL Managed Instance](resource-limits.md#service-tier-characteristics).

### <a name="business-critical-service-tier"></a>Dienstebene „Unternehmenskritisch“

Die unternehmenskritische Dienstebene wurde für Anwendungen mit hohen E/A-Anforderungen konzipiert. Sie bietet höchste Resilienz gegenüber Ausfällen durch mehrere isolierte Replikate.

Folgende wichtige Merkmale kennzeichnen die Dienstebene „Unternehmenskritisch“:

- Konzipiert für Geschäftsanwendungen mit höchster Leistung und Hochverfügbarkeitsanforderungen
- Enthält einen äußerst schnellen lokalen SSD-Speicher (bis zu 1 TB bei Gen 4 und bis zu 4 TB bei Gen 5)
- Integrierte [Hochverfügbarkeit](../database/high-availability-sla.md#premium-and-business-critical-service-tier-availability) basierend auf [Always On-Verfügbarkeitsgruppen](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server) und [Azure Service Fabric](../../service-fabric/service-fabric-overview.md)
- Integriertes zusätzliches [Datenbankreplikat mit Schreibschutz](../database/read-scale-out.md), das für Berichte und andere schreibgeschützte Workloads verwendet werden kann
- [In-Memory-OLTP](../in-memory-oltp-overview.md), das für Workloads mit hohen Leistungsanforderungen verwendet werden kann  

Weitere Informationen zu den Unterschieden zwischen Dienstebenen finden Sie unter [Ressourceneinschränkungen für SQL Managed Instance](resource-limits.md#service-tier-characteristics).

## <a name="management-operations"></a>Verwaltungsvorgänge

Azure SQL Managed Instance bietet Verwaltungsvorgänge, die Sie zum automatischen Bereitstellen von neuen verwalteten Instanzen, Aktualisieren von Instanzeigenschaften und Löschen von nicht mehr benötigten Instanzen verwenden können. Dieser Abschnitt enthält Informationen zu Verwaltungsvorgängen und deren typischer Dauer.

Zur Unterstützung von [Bereitstellungen in virtuellen Azure-Netzwerken (VNETs)](../../virtual-network/virtual-network-for-azure-services.md) und zur Sicherstellung der Isolation und Sicherheit für Kunden werden für SQL Managed Instance [virtuelle Cluster](connectivity-architecture-overview.md#high-level-connectivity-architecture) verwendet. Hierbei handelt es sich um einen dedizierten Satz isolierter virtueller Computer, die im VNET-Subnetz des Kunden bereitgestellt werden. Praktisch jede Bereitstellung einer verwalteten Instanz in einem leeren Subnetz führt dazu, dass ein neuer virtueller Cluster erstellt wird.

Nachfolgende Vorgänge für bereitgestellte verwaltete Instanzen können auch Auswirkungen auf den zugrunde liegenden virtuellen Cluster haben. Dies wirkt sich auf die Dauer von Verwaltungsvorgängen aus, da die Bereitstellung von zusätzlichen virtuellen Computern mit Mehraufwand verbunden ist. Dieser muss berücksichtigt werden, wenn Sie neue Bereitstellungen oder Updates vorhandener verwalteter Instanzen planen.

Alle Verwaltungsvorgänge können wie folgt kategorisiert werden:

- Instanzbereitstellung (Erstellung neuer Instanzen)
- Instanzupdate (Änderung von Instanzeigenschaften, z. B. V-Kerne oder reservierter Speicher)
- Instanzlöschung

Normalerweise dauern Vorgänge in virtuellen Clustern am längsten. Die Dauer der Vorgänge in virtuellen Clustern variiert. Unten sind die Werte angegeben, die für vorhandene Diensttelemetriedaten normalerweise zu erwarten sind:

- **Erstellung von virtuellen Clustern**:  Dies ist ein synchroner Schritt der Vorgänge für die Instanzverwaltung. **90 % der Vorgänge werden innerhalb von vier Stunden abgeschlossen**.
- **Änderung der Größe von virtuellen Clustern (Vergrößerung oder Verkleinerung)** : Die Vergrößerung ist ein synchroner Schritt, und die Verkleinerung wird asynchron durchgeführt (ohne Auswirkung auf die Dauer von Instanzverwaltungsvorgängen). **90 % der Clustervergrößerungen werden in weniger als 2,5 Stunden abgeschlossen**.
- **Löschung von virtuellen Clustern**: Der Löschvorgang ist ein asynchroner Schritt, der in einem leeren virtuellen Cluster aber auch [manuell initiiert](virtual-cluster-delete.md) werden kann. In diesem Fall wird der Vorgang synchron durchgeführt. **90 % der Löschungen virtueller Cluster werden innerhalb von 1,5 Stunden abgeschlossen**.

Darüber hinaus kann die Verwaltung von Instanzen auch einen der Vorgänge in gehosteten Datenbanken umfassen. Dies führt zu längeren Dauern:

- **Anfügen von Datenbankdateien aus Azure Storage**:  Dies ist ein synchroner Schritt, z. B. Compute (V-Kern) oder das zentrale Hoch- oder Herunterskalieren im Tarif „Universell“. **90 % dieser Vorgänge werden innerhalb von fünf Minuten abgeschlossen**.
- **Seeding der Always On-Verfügbarkeitsgruppe**: Dies ist ein synchroner Schritt, z. B. Compute (V-Kern) oder die Skalierung des Speichers im Tarif „Unternehmenskritisch“ sowie die Änderung der Dienstebene von „Universell“ in „Unternehmenskritisch“ (oder umgekehrt). Die Dauer dieses Vorgangs ist proportional zur Gesamtgröße der Datenbank sowie zur aktuellen Datenbankaktivität (Anzahl aktiver Transaktionen). Aufgrund der Datenbankaktivität beim Aktualisieren einer Instanz kann die Gesamtdauer erheblich variieren. **90 % dieser Vorgänge werden mindestens mit 220 GB pro Stunde durchgeführt**.

In der folgenden Tabelle sind die Vorgänge und die typischen Gesamtdauern zusammengefasst:

|Category  |Vorgang  |Segment mit langer Ausführungsdauer  |Geschätzte Dauer  |
|---------|---------|---------|---------|
|**Bereitstellung** |Erste Instanz in einem leeren Subnetz|Erstellung eines virtuellen Clusters|90 % der Vorgänge werden innerhalb von vier Stunden abgeschlossen|
|Bereitstellung |Erste Instanz einer anderen Hardwaregeneration in einem nicht leeren Subnetz (z. B. erste Gen5-Instanz in einem Subnetz mit Gen4-Instanzen)|Erstellung eines virtuellen Clusters*|90 % der Vorgänge werden innerhalb von vier Stunden abgeschlossen|
|Bereitstellung |Erstellung der ersten Instanz mit vier V-Kernen in einem leeren oder nicht leeren Subnetz|Erstellung eines virtuellen Clusters**|90 % der Vorgänge werden innerhalb von vier Stunden abgeschlossen|
|Bereitstellung |Nachfolgende Instanzerstellung im nicht leeren Subnetz (2. Instanz, 3. Instanz usw.)|Änderung der Größe eines virtuellen Clusters|90 % der Vorgänge werden innerhalb von 2,5 Stunden abgeschlossen|
|**Aktualisieren** |Änderung der Instanzeigenschaft (Administratorkennwort, Azure AD-Anmeldung, Azure-Hybridvorteil-Flag)|–|Bis zu 1 Minute|
|Aktualisieren |Zentrales Hoch-/Herunterskalieren des Instanzspeichers (Dienstebene „Universell“)|Anfügung von Datenbankdateien|90 % der Vorgänge werden innerhalb von fünf Minuten abgeschlossen.|
|Aktualisieren |Zentrales Hoch-/Herunterskalieren des Instanzspeichers (Tarif „Unternehmenskritisch“)|- Änderung der Größe eines virtuellen Clusters<br>- Seeding der Always On-Verfügbarkeitsgruppe|90 % der Vorgänge werden innerhalb von 2,5 Stunden zzgl. der Zeit für das Seeding aller Datenbanken (220 GB/Stunde) abgeschlossen|
|Aktualisieren |Zentrales Hoch-/Herunterskalieren der Computekapazität (V-Kerne) (Universell)|- Änderung der Größe eines virtuellen Clusters<br>- Anfügung von Datenbankdateien|90 % der Vorgänge werden innerhalb von 2,5 Stunden abgeschlossen|
|Aktualisieren |Zentrales Hoch-/Herunterskalieren der Computekapazität (V-Kerne) (Unternehmenskritisch)|- Änderung der Größe eines virtuellen Clusters<br>- Seeding der Always On-Verfügbarkeitsgruppe|90 % der Vorgänge werden innerhalb von 2,5 Stunden zzgl. der Zeit für das Seeding aller Datenbanken (220 GB/Stunde) abgeschlossen|
|Aktualisieren |Zentrales Herunterskalieren einer Instanz auf vier V-Kerne (Universell)|- Änderung der Größe eines virtuellen Clusters (beim ersten Mal ist ggf. die Erstellung eines virtuellen Clusters erforderlich**)<br>- Anfügung von Datenbankdateien|90 % der Vorgänge werden innerhalb von 4 Stunden und 5 Minuten abgeschlossen**|
|Aktualisieren |Zentrales Herunterskalieren einer Instanz auf vier V-Kerne (Unternehmenskritisch)|- Änderung der Größe eines virtuellen Clusters (beim ersten Mal ist ggf. die Erstellung eines virtuellen Clusters erforderlich**)<br>- Seeding der Always On-Verfügbarkeitsgruppe|90 % der Vorgänge werden innerhalb von 4 Stunden zzgl. der Zeit für das Seeding aller Datenbanken (220 GB/Stunde) abgeschlossen|
|Aktualisieren |Änderung der Instanzdienstebene („Universell“ in „Unternehmenskritisch“ und umgekehrt)|- Änderung der Größe eines virtuellen Clusters<br>- Seeding der Always On-Verfügbarkeitsgruppe|90 % der Vorgänge werden innerhalb von 2,5 Stunden zzgl. der Zeit für das Seeding aller Datenbanken (220 GB/Stunde) abgeschlossen|
|**Löschung**|Instanzlöschung|Log Tail-Sicherung für alle Datenbanken|90 % der Vorgänge werden innerhalb einer Minute abgeschlossen.<br>Hinweis: Wenn die letzte Instanz im Subnetz gelöscht wird, wird bei diesem Vorgang das Löschen des virtuellen Clusters nach 12 Stunden eingeplant.***|
|Löschen|Löschung eines virtuellen Clusters (als vom Benutzer initiierter Vorgang)|Löschung eines virtuellen Clusters|90 % der Vorgänge werden innerhalb von 1,5 Stunden abgeschlossen|

\* Der virtuelle Cluster wird pro Hardwaregeneration erstellt.

\*\* Die Option für vier virtuelle Kerne wurde im Juni 2019 eingeführt und erfordert eine neue Version des virtuellen Clusters. Falls Sie im Zielsubnetz über Instanzen verfügen, die alle vor dem 12. Juni erstellt wurden, wird automatisch ein neuer virtueller Cluster bereitgestellt, um vier V-Kern-Instanzen zu hosten.

\*\*\* Bei der aktuellen Konfiguration wird „12 Stunden“ verwendet, aber da sich dies in Zukunft ändern kann, sollten Sie hierfür keine festen Abhängigkeiten einrichten. Falls Sie einen virtuellen Cluster früher löschen müssen (z. B. um das Subnetz freizugeben), helfen Ihnen die Informationen unter [Löschen eines Subnetzes nach Löschen einer Azure SQL Managed Instance](virtual-cluster-delete.md) weiter.

### <a name="instance-availability-during-management-operations"></a>Instanzverfügbarkeit während Verwaltungsvorgängen

SQL Managed Instance ist während Bereitstellungs- und Löschvorgängen für Clientanwendungen nicht verfügbar.

SQL Managed Instance ist bei Updatevorgängen verfügbar, aber es kommt zu einem kurzen Ausfall aufgrund des Failovers, das am Ende von Updates ausgeführt wird. Er dauert dank der [beschleunigten Datenbankwiederherstellung](../accelerated-database-recovery.md) in der Regel bis zu 10 Sekunden, auch bei unterbrochenen Transaktionen mit langer Ausführungszeit.

> [!IMPORTANT]
> Es wird nicht empfohlen, die Compute- oder Speicherkapazität der Azure SQL Managed Instance zu skalieren oder die Dienstebene gleichzeitig mit den Transaktionen mit langer Ausführungsdauer (Datenimport, Datenverarbeitungsaufträge, Neuerstellung des Index usw.) zu ändern. Beim Datenbankfailover, das am Ende des Vorgangs ausgeführt wird, werden alle laufenden Transaktionen abgebrochen.

### <a name="management-operations-cross-impact"></a>Wechselseitige Auswirkungen von Verwaltungsvorgängen

Verwaltungsvorgänge für SQL Managed Instance können sich auf andere Verwaltungsvorgänge der Instanzen auswirken, die sich im gleichen virtuellen Cluster befinden. Dies umfasst Folgendes:

- **Wiederherstellungsvorgänge mit langer Ausführungszeit** in einem virtuellen Cluster stellen Instanzerstellungs- oder Skalierungsvorgänge anderer Instanzen im gleichen Subnetz zurück.<br/>**Beispiel:** Wenn ein Wiederherstellungsvorgang mit langer Laufzeit ausgeführt wird und eine Anforderung zum Erstellen oder Skalieren im gleichen Subnetz vorhanden ist, dauert es bis zum Abschluss länger, weil gewartet wird, bis der Wiederherstellungsvorgang beendet ist.
    
- **Nachfolgende Instanzerstellungs- oder Skalierungsvorgänge** werden durch zuvor initiierte Instanzerstellungen oder -skalierungen zurückgestellt, die die Größenänderung des virtuellen Clusters veranlasst haben.<br/>**Beispiel:** Wenn es in demselben Subnetz unter demselben virtuellen Cluster mehrere Anforderungen zum Erstellen und/oder Skalieren gibt und eine davon die Größenänderung des virtuellen Clusters initiiert, dauern alle Anforderungen, die mehr als 5 Minuten nach der Anforderung der Größenänderung des virtuellen Clusters übermittelt wurden, länger als erwartet, da diese Anforderungen auf den Abschluss der Größenänderung warten müssen, bevor sie fortgesetzt werden können.

- **Erstellungs-/Skalierungsvorgänge, die im 5-minütigen Fenster übermittelt werden**, werden in einem Batch zusammengefasst und parallel ausgeführt.<br/>**Beispiel:** Für alle Vorgänge, die in einem 5-minütigen Fenster übermittelt werden, wird nur eine Größenänderung des virtuellen Clusters ausgeführt (gemessen ab dem Zeitpunkt der Ausführung der ersten Vorgangsanforderung). Wenn eine andere Anforderung mehr als 5 Minuten nach der Übermittlung der ersten Anforderung übermittelt wird, wartet sie, bis die Größenänderung des virtuellen Clusters abgeschlossen ist, bevor die Ausführung beginnt.

> [!IMPORTANT]
> Verwaltungsvorgänge, die aufgrund eines anderen aktuell ausgeführten Vorgangs zurückgestellt werden, werden automatisch fortgesetzt, sobald die Bedingungen für eine Fortsetzung erfüllt sind. Es ist keine Benutzeraktion erforderlich, um den temporär angehaltenen Verwaltungsvorgang fortzusetzen.

### <a name="canceling-management-operations"></a>Abbrechen von Verwaltungsvorgängen

In der folgenden Tabelle sind die Möglichkeit zum Abbrechen von speziellen Verwaltungsvorgängen und die typischen Gesamtdauern zusammengefasst:

Category  |Vorgang  |Kann abgebrochen werden  |Geschätzte Abbruchdauer  |
|---------|---------|---------|---------|
|Bereitstellung |Erstellen einer Instanz |Nein |  |
|Aktualisieren |Zentrales Hoch-/Herunterskalieren des Instanzspeichers (universell) |Nein |  |
|Aktualisieren |Zentrales Hoch-/Herunterskalieren des Instanzspeichers (unternehmenskritisch) |Ja |90 % der Vorgänge werden innerhalb von fünf Minuten abgeschlossen. |
|Aktualisieren |Zentrales Hoch-/Herunterskalieren der Computekapazität (V-Kerne) (Universell) |Ja |90 % der Vorgänge werden innerhalb von fünf Minuten abgeschlossen. |
|Aktualisieren |Zentrales Hoch-/Herunterskalieren der Computekapazität (V-Kerne) (Unternehmenskritisch) |Ja |90 % der Vorgänge werden innerhalb von fünf Minuten abgeschlossen. |
|Aktualisieren |Änderung der Instanzdienstebene („Universell“ in „Unternehmenskritisch“ und umgekehrt) |Ja |90 % der Vorgänge werden innerhalb von fünf Minuten abgeschlossen. |
|Löschen |Instanzlöschung |Nein |  |
|Löschen |Löschung eines virtuellen Clusters (als vom Benutzer initiierter Vorgang) |Nein |  |

Um den Verwaltungsvorgang abzubrechen, wechseln Sie zum Blatt „Übersicht“, und klicken Sie auf das Benachrichtigungsfeld des laufenden Vorgangs. Auf der rechten Seite wird ein Bildschirm mit dem laufenden Vorgang und einer Schaltfläche zum Abbrechen des Vorgangs angezeigt. Nach dem ersten Klick werden Sie aufgefordert, erneut zu klicken und zu bestätigen, dass Sie den Vorgang abbrechen möchten.

[![Abbrechen eines Vorgangs](./media/sql-managed-instance-paas-overview/canceling-operation.png)](./media/sql-managed-instance-paas-overview/canceling-operation.png#lightbox)

Nachdem die Anforderung zum Abbrechen übermittelt und verarbeitet wurde, erhalten Sie eine Benachrichtigung, wenn die Übermittlung erfolgreich war.

Im Fall eines Erfolgs beim Abbrechen wird der Verwaltungsvorgang innerhalb weniger Minuten abgebrochen und ein Fehler verursacht.

![Ergebnis des Abbruchs des Vorgangs](./media/sql-managed-instance-paas-overview/canceling-operation-result.png)

Wenn bei der Abbruchanforderung ein Fehler auftritt oder die Schaltfläche „Abbrechen“ nicht aktiv ist, bedeutet dies, dass der Verwaltungsvorgang nicht abgebrochen werden konnte und in wenigen Minuten beendet wird. Der Verwaltungsvorgang wird so lange ausgeführt, bis er abgeschlossen wurde.

> [!IMPORTANT]
> Abbruchvorgänge werden zurzeit nur über das Portal unterstützt.

## <a name="advanced-security-and-compliance"></a>Erweiterte Sicherheit und Konformität

SQL Managed Instance enthält standardmäßig erweiterte Sicherheitsfeatures, die von der Azure-Cloud und der SQL Server-Datenbank-Engine bereitgestellt werden.

### <a name="security-isolation"></a>Sicherheitsisolierung

SQL Managed Instance bietet zusätzliche Sicherheitsisolation von anderen Mandanten in der Azure-Cloud. Die Sicherheitsisolation umfasst Folgendes:

- [Native Implementierung von und Konnektivität mit virtuellen Netzwerken](connectivity-architecture-overview.md) in der lokalen Umgebung unter Verwendung von Azure ExpressRoute oder VPN Gateway.
- Der SQL-Endpunkt wird in einer Standardbereitstellung nur über eine private IP-Adresse verfügbar gemacht, sodass sichere Verbindungen von privaten Azure-Netzwerken oder hybriden Netzwerken möglich sind.
- Einzelner Mandant mit dedizierter zugrunde liegender Infrastruktur (Compute, Speicher).

Das folgende Diagramm zeigt verschiedene Konnektivitätsoptionen für Ihre Anwendungen:

![Hochverfügbarkeit](./media/sql-managed-instance-paas-overview/application-deployment-topologies.png)  

Weitere Informationen zur VNET-Integration und Durchsetzung von Netzwerkrichtlinien auf Subnetzebene finden Sie unter [VNET-Architektur für eine verwaltete Instanz](connectivity-architecture-overview.md) und [Herstellen einer Verbindung zwischen einer Anwendung und einer verwalteten Instanz](connect-application-instance.md).

> [!IMPORTANT]
> Platzieren Sie mehrere verwaltete Instanzen im gleichen Subnetz, wo immer dies durch Ihre Sicherheitsanforderungen erlaubt ist, da Sie dadurch zusätzliche Vorteile erzielen können. Das Zusammenstellen von Instanzen im gleichen Subnetz wird die Wartung der Netzwerkinfrastruktur erheblich vereinfachen und die Zeit für die Instanzbereitstellung verkürzen, da eine lange Bereitstellungsdauer mit den Kosten für die Bereitstellung der ersten verwalteten Instanz in einem Subnetz verbunden ist.

### <a name="security-features"></a>Sicherheitsfeatures

Azure SQL Managed Instance bietet eine Reihe von erweiterten Sicherheitsfeatures, die zum Schutz Ihrer Daten verwendet werden können.

- Die [Überwachung von SQL Managed Instance](auditing-configure.md) verfolgt Datenbankereignisse nach und schreibt sie in eine Überwachungsprotokolldatei in Ihrem Azure-Speicherkonto. Die Überwachung kann dabei helfen, gesetzliche Bestimmungen einzuhalten, die Datenbankaktivität zu verstehen und Einblicke in Abweichungen und Anomalien zu erhalten, die auf geschäftliche Probleme oder mutmaßliche Sicherheitsverstöße hinweisen können.
- Datenverschlüsselung in Aktion: Eine SQL Managed Instance schützt Ihre Daten durch die Verschlüsselung für Daten während der Übertragung mit Transport Layer Security. Neben Transport Layer Security bietet SQL Managed Instance mit [Always Encrypted](/sql/relational-databases/security/encryption/always-encrypted-database-engine) einen Schutz für sensible Daten bei der Übertragung, im Ruhezustand und während der Abfrageverarbeitung. Always Encrypted ist das branchenweit erste System, das einen beispiellosen Schutz von Daten gegen Sicherheitsverletzungen wie dem Diebstahl wichtiger Daten bietet. Beispielsweise werden mit Always Encrypted Kreditkartennummern immer verschlüsselt in der Datenbank gespeichert, selbst während der Abfrageverarbeitung. Gleichzeitig wird autorisierten Mitarbeitern oder Anwendungen, die diese Daten verarbeiten müssen, die Entschlüsselung zum Zeitpunkt der Verwendung erlaubt.
- Mit [Advanced Threat Protection](threat-detection-configure.md) wird die [Überwachung](auditing-configure.md) vervollständigt, indem der Dienst um eine zusätzliche Security Intelligence-Ebene erweitert wird, die ungewöhnliche und eventuell schädliche Versuche, auf Datenbanken zuzugreifen oder sie zu missbrauchen, erkennt. Sie werden vor verdächtigen Aktivitäten, potenziellen Sicherheitsrisiken sowie Angriffen mit Einschleusung von SQL-Befehlen und anomalen Datenbankzugriffsmustern gewarnt. Advanced Threat Protection-Warnungen können in [Azure Security Center](https://azure.microsoft.com/services/security-center/) angezeigt werden und bieten Detailinformationen zu verdächtigen Aktivitäten sowie Empfehlungen, wie die Bedrohung untersucht und abgewendet werden kann.  
- Die [dynamische Datenmaskierung](/sql/relational-databases/security/dynamic-data-masking) schränkt die Offenlegung sensibler Daten ein, indem sie für nicht berechtigte Benutzer maskiert werden. Mit der dynamischen Datenmaskierung können Sie mit minimalen Auswirkungen auf die Anwendungsschicht festlegen, wie viel von den sensiblen Daten offengelegt werden soll. Dies trägt zur Verhinderung des unbefugten Zugriffs auf sensible Daten bei. Es handelt sich um eine richtlinienbasierte Sicherheitsfunktion, die die sensiblen Daten im Resultset einer Abfrage in festgelegten Datenbankfeldern ausblendet, während die Daten in der Datenbank nicht geändert werden.
- Bei der [Sicherheit auf Zeilenebene](/sql/relational-databases/security/row-level-security) können Sie den Zugriff auf Zeilen in einer Datenbanktabelle basierend auf den Merkmalen des Benutzers steuern, der eine Abfrage ausführt (z.B. Gruppenmitgliedschaft oder Ausführungskontext). Die Sicherheit auf Zeilenebene (Row-Level Security, RLS) vereinfacht das Entwerfen und Programmieren der Sicherheit in Ihrer Anwendung. Mit RLS können Sie den Zugriff auf Datenzeilen einschränken. So können Sie beispielsweise sicherstellen, dass Mitarbeiter nur auf die Datenzeilen zugreifen können, die für ihre Abteilung relevant sind, oder den Datenzugriff ausschließlich auf die relevanten Daten beschränken.
- [Transparent Data Encryption (TDE)](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql) verschlüsselt Datendateien von SQL Managed Instance. Dies wird als Verschlüsselung ruhender Daten bezeichnet. TDE führt die E/A-Verschlüsselung und -Entschlüsselung der Daten- und Protokolldateien in Echtzeit durch. Die Verschlüsselung verwendet einen Datenbank-Verschlüsselungsschlüssel (DEK), der im Startdatensatz der Datenbank gespeichert wird und während der Wiederherstellung zur Verfügung steht. Sie können alle Ihre Datenbanken in der verwalteten Instanz mit transparenter Datenverschlüsselung schützen. TDE ist die bewährte SQL Server-Verschlüsselungstechnologie für ruhende Daten, die für viele Konformitätsstandards zum Schutz vor Diebstahl von Speichermedien erforderlich ist.

Die Migration einer verschlüsselten Datenbank zur einer SQL Managed Instance wird über den Azure Database Migration Service (DMS) oder die native Wiederherstellung unterstützt. Wenn Sie planen, eine verschlüsselte Datenbank mittels nativer Wiederherstellung zu migrieren, ist die Migration des vorhandenen TDE-Zertifikats von der SQL Server-Instanz zu einer SQL Managed Instance erforderlich. Weitere Informationen zu den Migrationsoptionen finden Sie unter [Migration von SQL Server zu SQL Managed Instance](migrate-to-instance-from-sql-server.md).

## <a name="azure-active-directory-integration"></a>Azure Active Directory-Integration

SQL Managed Instance unterstützt herkömmliche SQL Server-Datenbank-Engine-Anmeldungen und in Azure Active Directory (Azure AD) integrierte Anmeldungen. Azure AD-Serverprinzipale (Anmeldungen) (**öffentliche Vorschau**) sind die Azure-Cloudversion von lokalen Datenbankanmeldungen, die Sie in Ihrer lokalen Umgebung verwenden. Azure AD-Serverprinzipale (Anmeldungen) ermöglichen Ihnen das Angeben von Benutzern und Gruppen von Ihrem Azure Active Directory-Mandanten als Prinzipale, die auch tatsächlich im Bereich der Instanz liegen. So können Sie jeden Vorgang auf Instanzebene ausführen – auch datenbankübergreifende Abfragen innerhalb derselben verwalteten Instanz.

Eine neue Syntax wird eingeführt, um Azure AD-Serverprinzipale (Anmeldungen) zu erstellen: **FROM EXTERNAL PROVIDER**. Weitere Informationen zur Syntax finden Sie unter <a href="/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current">CREATE LOGIN</a>, und lesen Sie auch den Artikel [Bereitstellen eines Azure Active Directory-Administrators für Ihre SQL Managed Instance](../database/authentication-aad-configure.md#provision-azure-ad-admin-sql-managed-instance).

### <a name="azure-active-directory-integration-and-multi-factor-authentication"></a>Azure Active Directory-Integration und Multi-Factor Authentication

SQL Managed Instance ermöglicht über die [Azure Active Directory-Integration](../database/authentication-aad-overview.md) eine zentrale Verwaltung von Identitäten der Datenbankbenutzer und anderer Microsoft-Dienste. Diese Funktion vereinfacht die Berechtigungsverwaltung und erhöht die Sicherheit. Azure Active Directory unterstützt [Multi-Factor Authentication](../database/authentication-mfa-ssms-configure.md) (MFA), um die Daten- und Anwendungssicherheit zu erhöhen, während gleichzeitig einmaliges Anmelden unterstützt wird.

### <a name="authentication"></a>Authentifizierung

Die Authentifizierung von SQL Managed Instance bezieht sich darauf, wie Benutzer ihre Identität beim Herstellen der Verbindung mit der Datenbank nachweisen. SQL Managed Instance unterstützt zwei Arten der Authentifizierung:  

- **SQL-Authentifizierung**:

  Diese Authentifizierungsmethode verwendet einen Benutzernamen und ein Kennwort.
- **Azure Active Directory-Authentifizierung**:

  Diese Authentifizierungsmethode verwendet von Azure Active Directory verwaltete Identitäten und wird für verwaltete und integrierte Domänen unterstützt. Verwenden Sie immer die Active Directory-Authentifizierung (integrierte Sicherheit), [sofern dies möglich ist](https://docs.microsoft.com/sql/relational-databases/security/choose-an-authentication-mode).

### <a name="authorization"></a>Authorization

Autorisierung bezieht sich darauf, welche Aufgaben ein Benutzer innerhalb einer Datenbank in Azure SQL Managed Instance ausführen kann. Dies wird durch datenbankbezogene Rollenmitgliedschaften und Objektebenenberechtigungen Ihres Benutzerkontos gesteuert. Eine SQL Managed Instance enthält die gleichen Autorisierungsfunktionen wie SQL Server 2017.

## <a name="database-migration"></a>Datenbankmigration

SQL Managed Instance zielt auf Benutzerszenarien mit einer Massenmigration der Datenbank von lokalen oder IaaS-Datenbankimplementierungen ab. SQL Managed Instance unterstützt mehrere Optionen zur Datenbankmigration:

### <a name="backup-and-restore"></a>Sichern und Wiederherstellen  

Beim Migrationsansatz werden SQL-Sicherungen in Azure Blob Storage genutzt. In Azure Blob Storage gespeicherte Sicherungen können mithilfe des [T-SQL RESTORE-Befehls](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-transact-sql?view=azuresqldb-mi-current) direkt in SQL Managed Instance gespeichert werden.

- Eine Schnellstartanleitung zum Wiederherstellen der Standardsicherungsdatei für die Wide World Importers-Datenbank finden Sie unter [Wiederherstellen einer Datenbanksicherung in einer verwalteten Azure SQL-Datenbank-Instanz](restore-sample-database-quickstart.md). In diesem Schnellstart wird gezeigt, wie eine Sicherungsdatei in den Azure Blob Storage hochgeladen und mit einem SAS-Schlüssel (Shared Access Signature) geschützt wird.
- Weitere Informationen zur Wiederherstellung über eine URL finden Sie unter [Native RESTORE FROM URL-Option](migrate-to-instance-from-sql-server.md#native-restore-from-url).

> [!IMPORTANT]
> Sicherungen von einer SQL Managed Instance können nur in einer anderen SQL Managed Instance wiederhergestellt werden. Sie können nicht in einer SQL Server-Instanz oder in Azure SQL-Datenbank wiederhergestellt werden.

### <a name="data-migration-service"></a>Data Migration Service

Azure Database Migration Service ist ein vollständig verwalteter Dienst, der die nahtlose Migration von mehreren Datenbankquellen zu Azure-Datenplattformen mit minimaler Downtime ermöglicht. Dieser Dienst optimiert die Aufgaben, die erforderlich sind, um bestehende Drittanbieter- und SQL Server-Datenbanken nach Azure SQL-Datenbank, Azure SQL Managed Instance und SQL Server auf einer Azure-VM zu verschieben. Siehe [Migrieren Ihrer lokalen Datenbank zu SQL Managed Instance mithilfe von DNS](https://aka.ms/migratetoMIusingDMS).

## <a name="sql-features-supported"></a>Unterstützte SQL-Features

SQL Managed Instance strebt mithilfe eines gestaffelten Releaseplans eine nahezu 100 %ige Oberflächenkompatibilität mit der neuesten SQL Server-Version an. Eine Liste der Funktionen und einen Funktionsvergleich finden Sie unter [Funktionsvergleich: SQL Managed Instance](../database/features-comparison.md), und eine Liste der T-SQL-Unterschiede in SQL Managed Instance im Vergleich zu SQL Server finden Sie unter [SQL Managed Instance: T-SQL-Unterschiede gegenüber SQL Server](transact-sql-tsql-differences-sql-server.md).

SQL Managed Instance unterstützt die Abwärtskompatibilität mit SQL 2008-Datenbanken. Die direkte Migration von SQL 2005-Datenbankservern wird unterstützt, der Kompatibilitätsgrad für migrierte SQL 2005-Datenbanken wird auf SQL 2008 aktualisiert.
  
In der folgenden Abbildung ist die Oberflächenkompatibilität in SQL Managed Instance aufgeführt:  

![Migration](./media/sql-managed-instance-paas-overview/migration.png)

### <a name="key-differences-on-premises-and-sql-managed-instance"></a>Wichtige Unterschiede zwischen lokalen Bereitstellungen und SQL Managed Instance

SQL Managed Instance ist in der Cloud immer auf dem neuesten Stand, d. h., einige Features in der SQL Server-Instanz sind möglicherweise entweder veraltet oder außer Kraft gesetzt oder weisen Alternativen auf. In bestimmten Fällen müssen Tools erkennen, dass ein bestimmtes Feature auf leicht abweichende Weise ausgeführt wird oder der Dienst in einer Umgebung ausgeführt wird, die Sie nicht vollständig steuern können.

Einige wesentliche Unterschiede:

- Hochverfügbarkeit ist anhand einer ähnlichen Technologie wie [AlwaysOn-Verfügbarkeitsgruppen](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server) integriert und vorkonfiguriert.
- Es gibt nur automatisierte Sicherungen und Point-in-Time-Wiederherstellungen. Kunden können `copy-only`-Sicherungen initiieren, die die automatische Sicherungskette nicht beeinträchtigen.
- Die Angabe von vollständigen physischen Pfaden wird nicht unterstützt. Daher müssen alle entsprechenden Szenarien unterschiedlich unterstützt werden: RESTORE DB unterstützt WITH MOVE nicht; CREATE DB lässt keine physischen Pfade zu; BULK INSERT kann nur mit Azure-Blobs ausgeführt werden usw.
- SQL Managed Instanz unterstützt die [Azure AD-Authentifizierung](../database/authentication-aad-overview.md) als Cloudalternative zur Windows-Authentifizierung.
- SQL Managed Instance verwaltet automatisch XTP-Dateigruppen und -Dateien für Datenbanken mit In-Memory-OLTP-Objekten.
- SQL Managed Instance unterstützt SQL Server Integration Services (SSIS) und kann den SSIS-Katalog (SSISDB) hosten, in dem SSIS-Pakete gespeichert werden, sie wird jedoch in einer verwalteten Azure SSIS Integration Runtime (IR) in Azure Data Factory (ADF) ausgeführt. Siehe dazu [Erstellen der Azure SSIS Integration Runtime in Azure Data Factory](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime). Informationen zum Vergleichen der SSIS-Funktionen finden Sie unter [Vergleich von SQL-Datenbank mit SQL Managed Instance](../../data-factory/create-azure-ssis-integration-runtime.md#comparison-of-sql-database-and-sql-managed-instance).

### <a name="administration-features"></a>Verwaltungsfunktionen

Dank SQL Managed Instance muss ein Systemadministrator weniger Zeit für Verwaltungsaufgaben aufwenden, da der SQL Managed Instance-Dienst diese für ihn ausführt oder diese Aufgaben vereinfacht. Beispiele: [Installation und Patches des Betriebssystems und des RDBMS](../database/high-availability-sla.md), [dynamische Größenänderung und Konfiguration von Instanzen](../database/single-database-scale.md), [Sicherungen](../database/automated-backups-overview.md), [Datenbankreplikation](replication-between-two-instances-configure-tutorial.md) (einschließlich Systemdatenbanken), [Konfiguration der Hochverfügbarkeit](../database/high-availability-sla.md) und der Integrität und [Leistungsüberwachung](../../azure-monitor/insights/azure-sql.md) der Datenströme.

Weitere Informationen finden Sie in einer [Liste der unterstützten und nicht unterstützten SQL Managed Instance-Features](../database/features-comparison.md) und in [T-SQL-Unterschiede zwischen SQL Managed Instance und SQL Server](transact-sql-tsql-differences-sql-server.md)

### <a name="programmatically-identify-a-managed-instance"></a>Programmgesteuertes Identifizieren einer verwalteten Instanz

In der folgenden Tabelle sind verschiedene Eigenschaften aufgeführt, auf die über Transact-SQL zugegriffen werden kann und über die Sie ermitteln können, ob Ihre Anwendung mit SQL Managed Instance ausgeführt wird, und mit denen Sie wichtige Eigenschaften abrufen können.

|Eigenschaft|Wert|Comment|
|---|---|---|
|`@@VERSION`|Microsoft SQL Azure (RTM) - 12.0.2000.8 2018-03-07 Copyright (C) 2018 Microsoft Corporation|Dieser Wert ist mit dem Wert in SQL-Datenbank identisch. Dies gibt **nicht** SQL-Engine-Version 12 (SQL Server 2014) an. SQL Managed Instance führt immer die neueste stabile SQL-Engine-Version aus, die mindestens der neuesten verfügbaren RTM-Version von SQL Server entspricht.  |
|`SERVERPROPERTY ('Edition')`|SQL Azure|Dieser Wert ist mit dem Wert in SQL-Datenbank identisch.|
|`SERVERPROPERTY('EngineEdition')`|8|Durch diesen Wert wird eine verwaltete Instanz eindeutig identifiziert.|
|`@@SERVERNAME`, `SERVERPROPERTY ('ServerName')`|Vollständiger DNS-Instanzname im folgenden Format: `<instanceName>`.`<dnsPrefix>`.database.windows.net, wobei `<instanceName>` der vom Kunden angegebene Name und `<dnsPrefix>` der automatisch generierte Teil des Namens ist, der die Eindeutigkeit des globalen DNS-Namens gewährleistet (z.B. „wcus17662feb9ce98“)|Beispiel: meine-verwaltete-instanz.wcus17662feb9ce98.database.windows.net|

## <a name="next-steps"></a>Nächste Schritte

- Im [Schnellstarthandbuch](instance-create-quickstart.md) erfahren Sie, wie Sie Ihre erste SQL Managed Instance erstellen.
- Eine Liste der Features und einen Funktionsvergleich finden Sie unter [Allgemeine SQL-Features](../database/features-comparison.md).
- Weitere Informationen zur VNET-Konfiguration finden Sie unter [Konfigurieren eines VNET für SQL Managed Instance](connectivity-architecture-overview.md).
- Eine Schnellstartanleitung, in der eine SQL Managed Instance erstellt und eine Datenbank von einer Sicherungsdatei wiederhergestellt wird, finden Sie unter [Erstellen einer SQL Managed Instance](instance-create-quickstart.md).
- Ein Tutorial mit Verwendung des Azure Database Migration Service (DMS) für die Migration finden Sie unter [Migration zu SQL Managed Instance mithilfe von DMS](../../dms/tutorial-sql-server-to-managed-instance.md).
- Informationen zur erweiterten Überwachung der Datenbankleistung von SQL Managed Instance mit integrierten Problembehandlungsfunktionen finden Sie unter [Überwachen von Azure SQL-Datenbank mithilfe von Azure SQL-Analyse (Vorschauversion)](../../azure-monitor/insights/azure-sql.md).
- Preisinformationen finden Sie unter [Preise – verwaltete Azure SQL-Datenbank-Instanzen ](https://azure.microsoft.com/pricing/details/sql-database/managed/).
