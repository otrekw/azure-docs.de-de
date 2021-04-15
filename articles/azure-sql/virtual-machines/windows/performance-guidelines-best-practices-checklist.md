---
title: 'Checkliste: Bewährte Methoden und Richtlinien zur Leistung'
description: Bietet bewährte Methoden und Richtlinien für den Speicher, um die Leistung Ihrer SQL Server-Instanz auf Azure Virtual Machines (VM) zu optimieren.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
editor: ''
tags: azure-service-management
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 03/25/2021
ms.author: mathoma
ms.custom: contperf-fy21q3
ms.reviewer: jroth
ms.openlocfilehash: 0b88884576a47db871c78b874104d4973ee9ba9a
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105572229"
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

- Verwenden Sie VM-Größen mit vier oder mehr vCPUs, z. B. [Standard_M8-4ms](/../../virtual-machines/m-series), [E4ds_v4](../../../virtual-machines/edv4-edsv4-series.md#edv4-series) und [DS12_v2](../../../virtual-machines/dv2-dsv2-series-memory.md#dsv2-series-11-15) oder höher. 
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
      - Wenn die Kapazität des lokalen Laufwerks für tempdb nicht ausreicht, können Sie die Größenanpassung der VM in Erwägung gezogen. Weitere Informationen finden Sie unter [Caching-Richtlinien für Datendateien](performance-guidelines-best-practices-storage.md#data-file-caching-policies).
- Stripen Sie mehrere Azure-Datenträger mithilfe von [Speicherplätzen](/windows-server/storage/storage-spaces/overview) , um die E/A-Bandbreite bis zum IOPS-und Durchsatzlimit des virtuellen Zielcomputers zu erhöhen.
- Legen Sie [Host-Caching](../../../virtual-machines/disks-performance.md#virtual-machine-uncached-vs-cached-limits) für Datendateien auf schreibgeschützt fest.
- Legen Sie [Host-Caching](../../../virtual-machines/disks-performance.md#virtual-machine-uncached-vs-cached-limits) für Protokolldatei-Datenträger auf keine fest.
    - Aktivieren Sie nicht das Zwischenspeichern von Lese-/Schreibzugriff auf Datenträgern mit SQL Server Dateien. 
    - Vor dem Ändern der Cacheeinstellungen Ihres Datenträgers sollten Sie den SQL Server-Dienst immer anhalten.
- Für die Entwicklung und Test-Arbeitsauslastungen sollten Sie den Standardspeicher verwenden. Es wird nicht empfohlen, HDD Standard/SDD für Produktionsworkloads zu verwenden.
- Für kleinere dev/Test-Workloads und Abteilungssysteme sollte nur ein [auf Guthaben basierendes Datenträgerbursting](../../../virtual-machines/disk-bursting.md#credit-based-bursting) (P1-P20) berücksichtigt werden.
- Befindet sich das Speicherkonto in derselben Region wie Azure SQL Server? 
- Deaktivieren Sie den georedundanten Azure-Speicher (georeplikation), und verwenden Sie LRS (lokal redundanter Speicher) für das Speicherkonto.
- Formatieren Sie den Datenträger, um die Größe der Zuordnungseinheiten für 64 KB für alle Datendateien zu verwenden, die auf einem anderen Laufwerk als dem temporären Laufwerk abgelegt werden `D:\` (Standardwert: 4 KB). SQL Server über Azure Marketplace bereitgestellten virtuellen Computer verfügen über Datenträger, die mit der Größe der Zuordnungseinheit formatiert sind, und Interleave für den Speicherpool auf 64 KB. 

Weitere Informationen finden Sie in den [bewährten Methoden für den Speicher](performance-guidelines-best-practices-storage.md). 


## <a name="azure--sql-feature-specific"></a>Azure und SQL Funktionsspezifika

Im folgenden finden Sie eine kurze Checkliste der bewährten Methoden für SQL Server- und Azure-spezifische Konfigurationen bei der Ausführung Ihrer SQL Server auf Azure-VM: 

- Registrieren Sie sich bei der [SQL-IaaS-Agent-Erweiterung](sql-agent-extension-manually-register-single-vm.md) , um eine Reihe von [funktionalen Vorteilen](sql-server-iaas-agent-extension-automate-management.md#feature-benefits)zu entsperren. 
- Aktivieren Sie die Datenbankseitenkomprimierung.
- Aktivieren Sie die sofortige Dateiinitialisierung für Datendateien.
- Begrenzen Sie die automatische Vergrößerung der Datenbank
- Deaktivieren Sie die automatische Verkleinerung der Datenbank
- Verschieben Sie alle Datenbanken, einschließlich der Systemdatenbanken, auf Datenträger für Daten.
- Verschieben Sie die Verzeichnisse für das SQL Server-Fehlerprotokoll und die Ablaufverfolgungsdateien auf die Datenträger für Daten.
- Konfigurieren Sie standardmäßige Dateispeicherorte für Sicherungen und Datenbanken.
- [Aktivieren Sie gesperrte Seiten im Arbeitsspeicher](/sql/database-engine/configure-windows/enable-the-lock-pages-in-memory-option-windows).
- Evaluieren Sie die [aktuellen kumulativen Updates](/sql/database-engine/install-windows/latest-updates-for-microsoft-sql-server) für die installierte Version von SQL Server, und wenden Sie sie an.
- Sichern Sie direkt in Azure Blob Storage.
- Verwenden Sie mehrere [tempdb](/sql/relational-databases/databases/tempdb-database#optimizing-tempdb-performance-in-sql-server)-Dateien, 1 Datei pro Kern, bis zu 8 Dateien.



## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich auch die anderen Artikel in dieser Reihe an, um mehr zu erfahren:
- [Größe des virtuellen Computers](performance-guidelines-best-practices-vm-size.md)
- [Storage](performance-guidelines-best-practices-storage.md)
- [Baseline auswählen](performance-guidelines-best-practices-collect-baseline.md)

Bewährte Methoden für die Sicherheit finden Sie unter [Sicherheitsüberlegungen für SQL Server auf Azure Virtual Machines](security-considerations-best-practices.md).

Weitere Artikel zu virtuellen SQL Server-Computern finden Sie unter [Übersicht zu SQL Server auf virtuellen Azure-Computern](sql-server-on-azure-vm-iaas-what-is-overview.md). Falls Sie Fragen zu SQL Server-VMs haben, finden Sie in den [häufig gestellten Fragen](frequently-asked-questions-faq.md) weitere Informationen.
