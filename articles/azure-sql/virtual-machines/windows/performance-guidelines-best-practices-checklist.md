---
title: 'Checkliste: Bewährte Methoden und Richtlinien zur Leistung'
description: Bietet bewährte Methoden und Richtlinien für den Speicher, um die Leistung Ihrer SQL Server-Instanz auf Azure Virtual Machines (VM) zu optimieren.
services: virtual-machines-windows
documentationcenter: na
author: dplessMSFT
editor: ''
tags: azure-service-management
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/06/2021
ms.author: dpless
ms.custom: contperf-fy21q3
ms.reviewer: jroth
ms.openlocfilehash: 3649c593fcc7a0d27ed7326efa03b1325763649d
ms.sourcegitcommit: a9f131fb59ac8dc2f7b5774de7aae9279d960d74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2021
ms.locfileid: "110190897"
---
# <a name="checklist-performance-best-practices-for-sql-server-on-azure-vms"></a>Checkliste: Bewährte Methoden zur Leistung für SQL Server auf Azure-VMs
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

Bietet bewährte Methoden und Richtlinien für den Speicher, um die Leistung Ihrer SQL Server-Instanz auf Azure Virtual Machines (VM) zu optimieren. 

Ausführliche Informationen finden Sie in den anderen Artikeln dieser Reihe: [VM-Größe](performance-guidelines-best-practices-vm-size.md), [Speicher](performance-guidelines-best-practices-storage.md), [Baseline sammeln](performance-guidelines-best-practices-collect-baseline.md). 


## <a name="overview"></a>Übersicht

Es empfiehlt sich, beim Ausführen von SQL Server auf Azure Virtual Machines weiterhin die gleichen Optionen zur Optimierung der Datenbankleistung zu verwenden, die für SQL Server in lokalen Serverumgebungen gelten. Die Leistung einer relationalen Datenbank in einer öffentlichen Cloud hängt jedoch von vielen Faktoren ab, z. B. der Größe eines virtuellen Computers und der Konfiguration der Datenträger für Daten.

In der Regel kommt es zu einem Kompromiss zwischen einer Kostenoptimierung und einer Leistungsoptimierung. Diese Folge von bewährten Methoden zur Leistung konzentriert sich darauf, die *beste* Leistung für SQL Server auf Azure Virtual Machines zu erzielen. Wenn Ihre Workload weniger anspruchsvoll ist, sind möglicherweise nicht alle empfohlenen Optimierungen erforderlich. Berücksichtigen Sie bei der Evaluierung dieser Empfehlungen Ihre Leistungsanforderungen und -kosten sowie Ihre Workloadmuster.

## <a name="vm-size"></a>Größe des virtuellen Computers

Im folgenden finden Sie eine kurze Checkliste mit bewährten Methoden für die VM-Größe für die Ausführung Ihrer SQL Server auf Azure-VM: 

- Verwenden Sie VM-Größen mit vier oder mehr vCPUs, z. B. [Standard_M8-4ms](../../../virtual-machines/m-series.md), [E4ds_v4](../../../virtual-machines/edv4-edsv4-series.md#edv4-series) und [DS12_v2](../../../virtual-machines/dv2-dsv2-series-memory.md#dsv2-series-11-15) oder höher. 
- Verwenden Sie VM-Größen mit [Arbeitsspeicheroptimierung](../../../virtual-machines/sizes-memory.md), um die bestmögliche Leistung für SQL Server-Workloads zu erzielen. 
- Die Serien [DSv2 11-15](../../../virtual-machines/dv2-dsv2-series-memory.md), [Edsv4](../../../virtual-machines/edv4-edsv4-series.md), [M](../../../virtual-machines/m-series.md) und [Mv2](../../../virtual-machines/mv2-series.md) führen zu dem optimalen Verhältnis von Arbeitsspeicher zu virtuellen Kernen, das für OLTP-Workloads benötigt wird. Beide VMs der M-Serie bieten das beste Verhältnis von Arbeitsspeicher zu virtuellen Kernen für unternehmenskritische Workloads und sind zudem ideal für Data Warehouse-Workloads geeignet. 
- Für unternehmenskritische und Data Warehouse-Workloads ist unter Umständen ein höheres Verhältnis von Arbeitsspeicher zu virtuellen Kernen erforderlich. 
- Nutzen Sie die Azure Virtual Machines-Marketplace-Images, da die SQL Server-Einstellungen und Speicheroptionen für eine optimale SQL Server-Leistung konfiguriert sind. 
- Erfassen Sie die Leistungsmerkmale des Zielworkloads, und nutzen Sie sie zum Ermitteln der passenden VM-Größe für Ihr Unternehmen.

Weitere Informationen finden Sie in den [bewährten Methoden für die VM-Größe](performance-guidelines-best-practices-vm-size.md). 

## <a name="storage"></a>Storage

Im folgenden finden Sie eine kurze Checkliste mit bewährten Methoden für die Speicherkonfiguration für die Ausführung Ihrer SQL Server auf Azure-VM: 

- Überwachen Sie die Anwendung, und [bestimmen Sie die Anforderungen an die Speicherbandbreite und Wartezeit](../../../virtual-machines/premium-storage-performance.md#counters-to-measure-application-performance-requirements) für SQL Server-Daten-, Protokoll- und tempdb-Dateien, bevor Sie den Datenträgertyp auswählen. 
- Um die Speicherleistung zu optimieren, planen Sie für die höchsten verfügbaren nicht zwischengespeicherten IOPS, und verwenden Sie Datenzwischenspeicherung als Leistungsmerkmal für Datenlesevorgänge, während Sie die [Begrenzung virtueller Computer und Datenträger](../../../virtual-machines/premium-storage-performance.md#throttling) vermeiden.
- Platzieren Sie Daten-, Protokoll- und tempdb-Dateien auf separaten Laufwerken.
    - Verwenden Sie für das Datenlaufwerk nur [Premium P30- und P40-Datenträger](../../../virtual-machines/disks-types.md#premium-ssd), um die Verfügbarkeit der Cacheunterstützung sicherzustellen.
    - Planen Sie für das Protokolllaufwerk die Kapazität und testen die Leistung im Verhältnis zu den Kosten, während Sie die [Premium-Datenträger P30-P80](../../../virtual-machines/disks-types.md#premium-ssd) bewerten.
      - Wenn eine Speicherwartezeit von weniger als einer Millisekunde erforderlich ist, verwenden Sie [Azure Ultra-Datenträger](../../../virtual-machines/disks-types.md#ultra-disk) für das Transaktionsprotokoll. 
      - Für Bereitstellungen von virtuellen Computern der M-Serie sollten Sie eine [Schreibbeschleunigung](../../../virtual-machines/how-to-enable-write-accelerator.md) der Verwendung von Azure-Ultra-Datenträgern vorziehen.
    - Platzieren Sie [tempdb](/sql/relational-databases/databases/tempdb-database) auf dem lokalen SSD- `D:\` Laufwerk für die meisten SQL Server Workloads, nachdem Sie die optimale VM-Größe ausgewählt haben. 
      - Wenn die Kapazität des lokalen Laufwerks für „tempdb“ nicht ausreicht, können Sie die Vergrößerung der VM in Erwägung ziehen. Weitere Informationen finden Sie unter [Richtlinien für das Zwischenspeichern von Datendateien](performance-guidelines-best-practices-storage.md#data-file-caching-policies).
- Erstellen Sie ein Stripeset mehrerer Azure-Datenträger mithilfe von [Speicherplätzen](/windows-server/storage/storage-spaces/overview), um die E/A-Bandbreite bis zum IOPS- und Durchsatzlimit des virtuellen Zielcomputers zu erhöhen.
- Legen Sie [Hostzwischenspeicherung](../../../virtual-machines/disks-performance.md#virtual-machine-uncached-vs-cached-limits) für Datenträger mit Datendateien auf „Schreibgeschützt“ fest.
- Legen Sie [Hostzwischenspeicherung](../../../virtual-machines/disks-performance.md#virtual-machine-uncached-vs-cached-limits) für Protokolldateidatenträger auf „Keine“ fest.
    - Aktivieren Sie nicht das Zwischenspeichern von Lese-/Schreibzugriff auf Datenträgern mit SQL Server-Dateien. 
    - Vor dem Ändern der Cacheeinstellungen Ihres Datenträgers sollten Sie den SQL Server-Dienst immer anhalten.
- Für die Entwicklung und Test-Arbeitsauslastungen sollten Sie den Standardspeicher verwenden. Es wird nicht empfohlen, HDD Standard/SDD für Produktionsworkloads zu verwenden.
- [Auf Guthaben basierendes Datenträgerbursting](../../../virtual-machines/disk-bursting.md#credit-based-bursting) (P1-P20) sollte nur für kleinere Dev-/Test-Workloads und Abteilungssysteme in Betracht gezogen werden.
- Formatieren Sie den Datenträger, um die Größe der Zuordnungseinheiten für 64 KB für alle Datendateien zu verwenden, die auf einem anderen Laufwerk als dem temporären Laufwerk abgelegt werden `D:\` (Standardwert: 4 KB). SQL Server über Azure Marketplace bereitgestellten virtuellen Computer verfügen über Datenträger, die mit der Größe der Zuordnungseinheit formatiert sind, und Interleave für den Speicherpool auf 64 KB. 

Weitere Informationen finden Sie in den [bewährten Methoden für den Speicher](performance-guidelines-best-practices-storage.md). 

## <a name="sql-server-features"></a>SQL Server-Funktionen

Im Folgenden finden Sie eine kurze Checkliste der Best Practices für SQL Server-Konfigurationseinstellungen, wenn Sie Ihre SQL Server-Instanzen auf einem virtuellen Azure-Computer in der Produktion ausführen: 

- Aktivieren Sie ggf. die [Datenbankseitenkomprimierung](/sql/relational-databases/data-compression/data-compression).
- Aktivieren Sie die [Sicherungskomprimierung](/sql/relational-databases/backup-restore/backup-compression-sql-server).
- Aktivieren Sie die [sofortige Dateiinitialisierung](/sql/relational-databases/databases/database-instant-file-initialization) für Datendateien.
- Begrenzen Sie die [automatische Vergrößerung](/troubleshoot/sql/admin/considerations-autogrow-autoshrink#considerations-for-autogrow) der Datenbank.
- Deaktivieren Sie die [automatische Verkleinerung](/troubleshoot/sql/admin/considerations-autogrow-autoshrink#considerations-for-auto_shrink) der Datenbank.
- Deaktivieren Sie das automatische Schließen der Datenbank.
- Verschieben Sie alle Datenbanken, einschließlich der [Systemdatenbanken](/sql/relational-databases/databases/move-system-databases) auf Datenträger für Daten.
- Verschieben Sie die Verzeichnisse für das SQL Server-Fehlerprotokoll und die Ablaufverfolgungsdateien auf die Datenträger für Daten.
- Konfigurieren Sie standardmäßige Dateispeicherorte für Sicherungen und Datenbanken.
- Legen Sie ein [Arbeitsspeicherlimit für SQL Server](/sql/database-engine/configure-windows/server-memory-server-configuration-options#use-) fest, damit dem Betriebssystem genügend Arbeitsspeicher zur Verfügung steht. ([Verwenden Sie „Arbeitsspeicher: Verfügbare Bytes“](/sql/relational-databases/performance-monitor/monitor-memory-usage), um den Arbeitsspeicherzustand des Betriebssystems zu überwachen.)
- Aktivieren Sie [Seiten im Arbeitsspeicher sperren](/sql/database-engine/configure-windows/enable-the-lock-pages-in-memory-option-windows).
- Aktivieren Sie [Für Ad-hoc-Arbeitsauslastungen optimieren](/sql/database-engine/configure-windows/optimize-for-ad-hoc-workloads-server-configuration-option) für OLTP-Umgebungen mit hoher Auslastung.
- Evaluieren Sie die [aktuellen kumulativen Updates](/sql/database-engine/install-windows/latest-updates-for-microsoft-sql-server) für die installierte Version von SQL Server, und wenden Sie sie an.
- Aktivieren Sie [Abfragespeicher](/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store) für alle SQL Server-Produktionsdatenbanken [gemäß den Best Practices](/sql/relational-databases/performance/best-practice-with-the-query-store).
- Aktivieren Sie die [automatische Optimierung](/sql/relational-databases/automatic-tuning/automatic-tuning) für unternehmenskritische Anwendungsdatenbanken.
- Stellen Sie sicher, dass alle [Best Practices für tempdb](/sql/relational-databases/databases/tempdb-database#optimizing-tempdb-performance-in-sql-server) befolgt werden.
- Platzieren Sie tempdb auf dem kurzlebigen Laufwerk D:/.
- [Verwenden Sie die empfohlene Anzahl von Dateien](/troubleshoot/sql/performance/recommendations-reduce-allocation-contention#resolution). Beginnen Sie bei mehreren tempdb-Datendateien mit einer Datei pro Kern, und verwenden Sie nicht mehr als 8 Dateien.
- Planen Sie SQL Server-Agent-Aufträge für die Ausführung von [DBCC CHECKDB](/sql/t-sql/database-console-commands/dbcc-checkdb-transact-sql#a-checking-both-the-current-and-another-database)-, [index reorganize](/sql/relational-databases/indexes/reorganize-and-rebuild-indexes#reorganize-an-index)-, [index rebuild](/sql/relational-databases/indexes/reorganize-and-rebuild-indexes#rebuild-an-index)- und [update statistics](/sql/t-sql/statements/update-statistics-transact-sql#examples)-Vorgängen.
- Überwachen und verwalten Sie die Integrität und Größe der SQL Server-[Transaktionsprotokolldatei](/sql/relational-databases/logs/manage-the-size-of-the-transaction-log-file#Recommendations).
- Nutzen Sie die neuen [SQL Server-Features](/sql/sql-server/what-s-new-in-sql-server-ver15), die für die verwendete Version verfügbar sind.
- Beachten Sie die Unterschiede bei den [unterstützten Features](/sql/sql-server/editions-and-components-of-sql-server-version-15) der jeweiligen Editionen, die Sie bereitstellen möchten.

## <a name="azure-features"></a>Azure-Features

Im Folgenden finden Sie eine kurze Checkliste der Best Practices zur Azure-spezifischen Anleitung bei der Ausführung Ihrer SQL Server-Instanzen auf Azure-VM:

- Registrieren Sie sich für die [SQL Server-IaaS-Agent-Erweiterung](sql-agent-extension-manually-register-single-vm.md), um eine Reihe von [Featurevorteilen](sql-server-iaas-agent-extension-automate-management.md#feature-benefits) zu nutzen.
- Nutzen Sie die beste [Sicherungs- und Wiederherstellungsstrategie](backup-restore.md#decision-matrix) für Ihre SQL Server-Workload.
- Stellen Sie sicher, dass der [beschleunigte Netzwerkbetrieb](../../../virtual-network/create-vm-accelerated-networking-cli.md#portal-creation) auf dem virtuellen Computer aktiviert ist.
- Nutzen Sie [Azure Security Center](../../../security-center/index.yml), um den Sicherheitsstatus Ihres bereitgestellten virtuellen Computers insgesamt zu verbessern.
- Nutzen Sie [Azure Defender](../../../security-center/azure-defender.md) in [Azure Security Center](https://azure.microsoft.com/services/security-center/) für bestimmte [SQL Server-VM-Abdeckungen](../../../security-center/defender-for-sql-introduction.md), einschließlich Sicherheitsrisikobewertungen und Just-in-Time-Zugriff, wodurch der Angriffsdienst reduziert wird, während berechtigte Benutzer bei Bedarf auf virtuelle Computer zugreifen können. Weitere Informationen finden Sie unter [Sicherheitsrisikobewertungen](../../../security-center/defender-for-sql-on-machines-vulnerability-assessment.md), [Aktivieren von Sicherheitsrisikobewertungen für SQL Server-VMs](sql-vulnerability-assessment-enable.md) und [Just-in-Time-Zugriff](../../../security-center/just-in-time-explained.md). 
- Verwenden Sie [Azure Advisor](../../../advisor/advisor-overview.md), um Empfehlungen für [Leistung](../../../advisor/advisor-performance-recommendations.md), [Kosten](../../../advisor/advisor-cost-recommendations.md), [Zuverlässigkeit](../../../advisor/advisor-high-availability-recommendations.md), [Operational Excellence](../../../advisor/advisor-operational-excellence-recommendations.md) und [Sicherheit](../../../advisor/advisor-security-recommendations.md) zu erfüllen.
- Verwenden Sie [Azure Monitor](../../../azure-monitor/vm/quick-monitor-azure-vm.md), um Telemetriedaten aus Ihrer SQL Server-Umgebung zu sammeln, zu analysieren und darauf zu reagieren. Dies umfasst das Identifizieren von Infrastrukturproblemen mit [VM-Erkenntnissen](../../../azure-monitor/vm/vminsights-overview.md) und das Überwachen von Daten mit [Log Analytics](../../../azure-monitor/logs/log-query-overview.md) für eine ausführlichere Diagnose.
- Aktivieren Sie die Option [Automatisches Herunterfahren](../../../automation/automation-solution-vm-management.md) für Entwicklungs- und Testumgebungen. 
- Implementieren Sie eine HADR-Lösung (High Availability and Disaster Recovery), die Ihren SLAs für Geschäftskontinuität entspricht. Weitere Informationen finden Sie unter [HADR-Optionen](business-continuity-high-availability-disaster-recovery-hadr-overview.md#deployment-architectures), die für SQL Server auf Azure-VMs verfügbar sind. 
- Verwenden Sie das Azure-Portal (Support und Problembehandlung), um die Integrität und den Verlauf von [Ressourcen](../../../service-health/resource-health-overview.md) zu bewerten. Übermitteln Sie bei Bedarf neue Supportanfragen.

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich auch die anderen Artikel in dieser Reihe an, um mehr zu erfahren:
- [Größe des virtuellen Computers](performance-guidelines-best-practices-vm-size.md)
- [Storage](performance-guidelines-best-practices-storage.md)
- [Baseline auswählen](performance-guidelines-best-practices-collect-baseline.md)

Bewährte Methoden für die Sicherheit finden Sie unter [Sicherheitsüberlegungen für SQL Server auf Azure Virtual Machines](security-considerations-best-practices.md).

Weitere Artikel zu virtuellen SQL Server-Computern finden Sie unter [Übersicht zu SQL Server auf virtuellen Azure-Computern](sql-server-on-azure-vm-iaas-what-is-overview.md). Falls Sie Fragen zu SQL Server-VMs haben, finden Sie in den [häufig gestellten Fragen](frequently-asked-questions-faq.md) weitere Informationen.
