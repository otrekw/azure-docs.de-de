---
title: Neuerungen in Azure Backup
description: Erfahren Sie mehr über die neuen Features in Azure Backup.
ms.topic: conceptual
ms.date: 11/11/2020
ms.openlocfilehash: 62a6146990863c339917777b2624fee76ebe60d8
ms.sourcegitcommit: 9d9221ba4bfdf8d8294cf56e12344ed05be82843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2021
ms.locfileid: "98569418"
---
# <a name="whats-new-in-azure-backup"></a>Neuerungen in Azure Backup

Azure Backup wird fortlaufend verbessert und um neue Features ergänzt, die den Schutz Ihrer Daten in Azure optimieren. Diese neuen Features erweitern den Datenschutz auf neue Workloadtypen, erhöhen die Sicherheit und verbessern die Verfügbarkeit der Sicherungsdaten. Außerdem werden neue Verwaltungs-, Überwachungs- und Automatisierungsfunktionen hinzugefügt.

Weitere Informationen zu den neuen Releases erhalten Sie, indem Sie diese Seite als Lesezeichen speichern oder [hier Updates abonnieren](https://azure.microsoft.com/updates/?query=backup).

## <a name="updates-summary"></a>Übersicht über Updates

- Januar 2021
  - [Azure Disk Backup (Vorschau)](disk-backup-overview.md)
  - [Verschlüsselung ruhender Daten mit von Kunden verwalteten Schlüsseln jetzt allgemein verfügbar](encryption-at-rest-with-cmk.md)
- November 2020
  - [Azure Resource Manager-Vorlage für die Sicherung der Azure-Dateifreigabe](#azure-resource-manager-template-for-afs-backup)
  - [Inkrementelle Sicherungen für SAP HANA-Datenbanken auf Azure-VMs](#incremental-backups-for-sap-hana-databases)
- September 2020
  - [Backup Center](#backup-center)
  - [Sichern von Azure Database for PostgreSQL](#backup-azure-database-for-postgresql)
  - [Selektive Datenträgersicherung und -wiederherstellung](#selective-disk-backup-and-restore)
  - [Regionsübergreifende Wiederherstellung für SQL Server- und SAP HANA-Datenbanken auf Azure-VMs](#cross-region-restore-for-sql-server-and-sap-hana)
  - [Unterstützung für die Sicherung von VMs mit bis zu 32 Datenträgern](#support-for-backup-of-vms-with-up-to-32-disks)
  - [Vereinfachte Konfiguration von Sicherungen für SQL auf Azure-VMs](#simpler-backup-configuration-for-sql-in-azure-vms)
  - [Sichern von SAP HANA in Azure Virtual Machines unter RHEL](#backup-sap-hana-in-rhel-azure-virtual-machines)
  - [Zonenredundanter Speicher (ZRS) für Sicherungsdaten](#zone-redundant-storage-zrs-for-backup-data)
  - [Vorläufiges Löschen für SQL Server- und SAP HANA-Workloads auf Azure-VMs](#soft-delete-for-sql-server-and-sap-hana-workloads)

## <a name="azure-disk-backup-in-preview"></a>Azure Disk Backup (Vorschau)

Azure Disk Backup bietet eine schlüsselfertige Lösung, die eine Verwaltung des Lebenszyklus von Momentaufnahmen für [Azure Managed Disks](https://docs.microsoft.com/azure/virtual-machines/managed-disks-overview) durch Automatisierung einer periodischen Erstellung von Momentaufnahmen und Aufbewahrung der Momentaufnahmen für eine konfigurierte Dauer unter Verwendung der Sicherungsrichtlinie bereitstellt. Sie können die Datenträgermomentaufnahmen ohne Infrastrukturkosten und ohne die Notwendigkeit von benutzerdefinierten Skripts oder jeglichem Verwaltungsaufwand verwalten. Es handelt sich um eine absturzsichere Sicherungslösung, die eine zeitpunktgenaue Sicherung eines verwalteten Datenträgers mit [inkrementellen Momentaufnahmen](https://docs.microsoft.com/azure/virtual-machines/windows/disks-incremental-snapshots) mit Unterstützung für mehrere Sicherungen pro Tag vornimmt. Es handelt sich außerdem um eine Lösung ohne Agent, die die Leistung der Produktionsanwendung nicht beeinträchtigt. Sie unterstützt die Sicherung und Wiederherstellung der Betriebssystem- und regulären Datenträger (einschließlich gemeinsam genutzter Datenträger), unabhängig davon, ob sie derzeit mit einem ausgeführten virtuellen Azure-Computer verbunden sind oder nicht.

Weitere Informationen finden Sie unter [Azure Disk Backup (Vorschau)](disk-backup-overview.md).

## <a name="encryption-at-rest-using-customer-managed-keys"></a>Verschlüsselung im Ruhezustand mithilfe von Kunden verwalteter Schlüssel

Unterstützung für die Verschlüsselung ruhender Daten mit von Kunden verwalteten Schlüsseln ist jetzt allgemein verfügbar. Dies bietet Ihnen die Möglichkeit, die Sicherungsdaten in Ihren Recovery Services-Tresoren mit Ihren eigenen, in Azure Key Vaults gespeicherten Schlüsseln zu verschlüsseln. Der Verschlüsselungsschlüssel, der zum Verschlüsseln von Sicherungen im Recovery Services-Tresor verwendet wird, kann sich von den für die Verschlüsselung der Quelle verwendeten Verschlüsselungsschlüssel unterscheiden. Die Daten werden mithilfe eines AES-256 basierten Datenverschlüsselungsschlüssels (DEK) geschützt, der wiederum mit Ihren im Schlüsseltresor gespeicherten Schlüsseln geschützt wird. Im Vergleich zur Verschlüsselung mit plattformverwalteten Schlüsseln (die standardmäßig verfügbar ist), haben Sie so mehr Kontrolle über Ihre Schlüssel und können Ihre Complianceanforderungen besser erfüllen.

Weitere Informationen finden Sie unter [Verschlüsselung von Sicherungsdaten mit von Kunden verwalteten Schlüsseln](encryption-at-rest-with-cmk.md).

## <a name="azure-resource-manager-template-for-afs-backup"></a>Azure Resource Manager-Vorlage für die Sicherung der Azure-Dateifreigabe

Azure Backup unterstützt jetzt das Konfigurieren von Sicherungen für bestehende Azure-Dateifreigaben mithilfe einer Azure Resource Manager-Vorlage (ARM-Vorlage). Mit der Vorlage wird der Schutz für eine vorhandene Azure-Dateifreigabe konfiguriert, indem entsprechende Details für den Recovery Services-Tresor und die Sicherungsrichtlinie angegeben werden. Optional erstellt sie einen neuen Recovery Services-Tresor und eine neue Sicherungsrichtlinie und registriert das Speicherkonto, das die Dateifreigabe enthält, im Recovery Services-Tresor.

Weitere Informationen finden Sie unter [Azure Resource Manager-Vorlagen für Azure Backup](backup-rm-template-samples.md).

## <a name="incremental-backups-for-sap-hana-databases"></a>Inkrementelle Sicherungen für SAP HANA-Datenbanken

Azure Backup unterstützt jetzt inkrementelle Sicherungen für SAP HANA-Datenbanken, die auf Azure-VMs gehostet werden. Dies ermöglicht schnellere und kostengünstigere Sicherungen Ihrer SAP HANA-Daten.

Weitere Informationen finden Sie unter den [verschiedenen Optionen, die während der Erstellung einer Sicherungsrichtlinie verfügbar sind](sap-hana-faq-backup-azure-vm.md#policy), und unter [Erstellen einer Sicherungsrichtlinie für SAP HANA-Datenbanken](tutorial-backup-sap-hana-db.md#creating-a-backup-policy).

## <a name="backup-center"></a>Backup Center

Azure Backup bietet eine neue native Verwaltungsfunktion, mit der Sie die gesamte Sicherungsumgebung über eine zentrale Konsole verwalten können. Backup Center bietet Ihnen die Möglichkeit, den Schutz von Daten im großen Stil und auf einheitliche Weise zu überwachen, auszuführen, zu steuern und zu optimieren, und zwar konsistent mit den nativen Verwaltungsfunktionen von Azure.

Mit Backup Center erhalten Sie eine aggregierte Ansicht Ihres Bestands über Abonnements, Standorte, Ressourcengruppen, Tresore und mit Azure Lighthouse sogar über Mandanten hinweg. Backup Center ermöglicht auch das zentrale Auslösen von Sicherungsaktivitäten, z. B. das Konfigurieren von Sicherung und Wiederherstellung oder dem Erstellen von Richtlinien und Tresoren. Außerdem können Sie dank der nahtlosen Integration mit Azure Policy jetzt Ihre Umgebung steuern und die Compliance aus Sicherungsperspektive verfolgen. Integrierte Azure-Richtlinien, die speziell für Azure Backup gelten, ermöglichen ebenfalls das Konfigurieren von Sicherungen im großen Stil.

Weitere Informationen finden Sie in der [Übersicht über Backup Center](backup-center-overview.md).

## <a name="backup-azure-database-for-postgresql"></a>Sichern von Azure Database for PostgreSQL

Azure Backup und die Azure-Datenbankdienste wurden zusammengeführt, um eine Sicherungslösung für Unternehmen mit Azure PostgreSQL zu entwickeln (jetzt in der Vorschauphase). Jetzt können Sie Ihre Datenschutz- und Complianceanforderungen mit einer vom Kunden gesteuerten Sicherungsrichtlinie erfüllen, die die Beibehaltung von Sicherungen für bis zu 10 Jahre ermöglicht. Damit können Sie die Sicherungs- und Wiederherstellungsvorgänge auf der Ebene der einzelnen Datenbanken präzise steuern. Ebenso können Sie problemlos Wiederherstellungen verschiedener PostgreSQL-Versionen oder Wiederherstellungen in Blob Storage durchführen.

Weitere Informationen finden Sie unter [Sicherung von Azure Database for PostgreSQL](backup-azure-database-postgresql.md).

## <a name="selective-disk-backup-and-restore"></a>Selektive Datenträgersicherung und -wiederherstellung

Azure Backup unterstützt die Sicherung aller Datenträger (Betriebssystem und Daten) einer VM mithilfe der VM-Sicherungslösung. Mithilfe der Funktionalität der selektiven Datenträgersicherung und -wiederherstellung können Sie nun eine Teilmenge der Datenträger auf einem virtuellen Computer sichern. Dies stellt eine effiziente und kostengünstige Lösung für Ihre Sicherungs- und Wiederherstellungsanforderungen dar. Jeder Wiederherstellungspunkt enthält nur die Datenträger, die im Sicherungsvorgang enthalten sind.

Weitere Informationen finden Sie unter [Selektives Sichern und Wiederherstellen von Datenträgern auf Azure-VMs](selective-disk-backup-restore.md).

## <a name="cross-region-restore-for-sql-server-and-sap-hana"></a>Regionsübergreifende Wiederherstellung für SQL Server und SAP HANA

Mit der Einführung der regionsübergreifenden Wiederherstellung können Sie nun beliebig Wiederherstellungen in einer sekundären Region initiieren. So können Sie tatsächliche Probleme aufgrund von Ausfällen in einer primären Region für Ihre Umgebung verringern. Dadurch werden die Wiederherstellungen in der sekundären Region vollständig vom Kunden gesteuert. Azure Backup verwendet für solche Wiederherstellungen die gesicherten Daten, die in die sekundäre Region repliziert wurden.

Zusätzlich zur Unterstützung regionsübergreifender Wiederherstellungen für Azure-VMs wurde das Feature auch erweitert, sodass nun auch SQL- und SAP HANA-Datenbanken auf Azure-VMs wiederhergestellt werden können.

Weitere Informationen finden Sie unter [Regionsübergreifende Wiederherstellung für SQL-Datenbanken](restore-sql-database-azure-vm.md#cross-region-restore) und [Regionsübergreifende Wiederherstellung für SAP HANA-Datenbanken](sap-hana-db-restore.md#cross-region-restore).

## <a name="support-for-backup-of-vms-with-up-to-32-disks"></a>Unterstützung für die Sicherung von VMs mit bis zu 32 Datenträgern

Bisher unterstützte Azure Backup 16 verwaltete Datenträger pro VM. Nun unterstützt Azure Backup die Sicherung von bis zu 32 verwalteten Datenträgern pro VM.

Weitere Informationen finden Sie in der [Unterstützungsmatrix für VM-Speicher](backup-support-matrix-iaas.md#vm-storage-support).

## <a name="simpler-backup-configuration-for-sql-in-azure-vms"></a>Einfachere Sicherungskonfiguration für SQL auf Azure-VMs

Das Konfigurieren von Sicherungen für Ihre SQL Server-Instanzen auf Azure-VMs ist jetzt noch einfacher, da die Konfiguration von Inlinesicherungen in den VM-Bereich des Azure-Portals integriert wurde. In nur wenigen Schritten können Sie die Sicherung Ihrer SQL Server-Instanzen aktivieren, um alle vorhandenen Datenbanken sowie die in Zukunft hinzugefügten Datenbanken zu schützen.

Weitere Informationen finden Sie unter [Sichern einer SQL Server-Instanz über den VM-Bereich](backup-sql-server-vm-from-vm-pane.md).

## <a name="backup-sap-hana-in-rhel-azure-virtual-machines"></a>Sichern von SAP HANA in Azure Virtual Machines unter RHEL

Azure Backup ist die native Sicherungslösung für Azure und von SAP als BackInt zertifiziert. Azure Backup bietet jetzt Unterstützung für Red Hat Enterprise Linux (RHEL), eines der am häufigsten verwendeten Linux-Betriebssysteme, auf denen SAP HANA ausgeführt wird.

Weitere Informationen finden Sie in der [Unterstützungsmatrix für die Sicherung von SAP HANA-Datenbanken auf virtuellen Azure-Computern](sap-hana-backup-support-matrix.md#scenario-support).

## <a name="zone-redundant-storage-zrs-for-backup-data"></a>Zonenredundanter Speicher (ZRS) für Sicherungsdaten

Azure Storage bietet über verschiedene Redundanzoptionen eine hervorragende Balance zwischen hoher Leistung, Hochverfügbarkeit und hoher Datenresilienz. Mit Azure Backup können Sie diese Vorteile auch auf die Sicherungsdaten ausweiten. So haben Sie z. B. die Möglichkeit, Ihre Sicherungen in lokal redundantem Speicher (LRS) und georedundantem Speicher (GRS) zu speichern. Darüber hinaus gibt es nun zusätzliche Dauerhaftigkeitsoptionen mit zusätzlicher Unterstützung für zonenredundanten Speicher (ZRS).

Weitere Informationen finden Sie unter [Festlegen der Speicherredundanz für den Recovery Services-Tresor](backup-create-rs-vault.md#set-storage-redundancy).

## <a name="soft-delete-for-sql-server-and-sap-hana-workloads"></a>Vorläufiges Löschen für SQL Server- und SAP HANA-Workloads

Die Sorgen bezüglich Sicherheitsproblemen wie Schadsoftware, Ransomware und Eindringlingen werden immer größer. Diese Sicherheitsprobleme können erhebliche Daten- und finanzielle Verluste mit sich bringen. Zum Schutz gegen solche Angriffe verfügt Azure Backup über Sicherheitsfeatures für den Schutz von Sicherungsdaten auch nach dem Löschen.

Eines dieser Features ist das vorläufige Löschen. Beim vorläufigen Löschen werden die Sicherungsdaten 14 Tage länger aufbewahrt, damit das jeweilige Sicherungselement auch dann ohne Datenverluste wiederhergestellt werden kann, wenn ein böswilliger Akteur eine Sicherung löscht oder die Sicherungsdaten versehentlich gelöscht werden. Für die zusätzlichen 14 Tage der Aufbewahrung von Sicherungsdaten mit dem Status „Vorläufiges Löschen“ fallen für Sie keine Kosten an.

Zusätzlich zur Unterstützung für vorläufiges Löschen von Azure-VMs werden nun auch SQL Server- und SAP HANA-Workloads auf Azure-VMs durch vorläufiges Löschen geschützt.

Weitere Informationen finden Sie unter [Vorläufiges Löschen für SQL Server auf Azure-VMs und SAP HANA in Azure-VM-Workloads](soft-delete-sql-saphana-in-azure-vm.md).

## <a name="next-steps"></a>Nächste Schritte

- [Azure Backup: Leitfaden und bewährte Methoden](guidance-best-practices.md)
