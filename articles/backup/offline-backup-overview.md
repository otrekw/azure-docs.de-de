---
title: Übersicht über die Offlinesicherung
description: Erfahren Sie mehr über die Komponenten der Offlinesicherung. Dazu zählen Offlinesicherungen basierend auf Azure Data Box und Offlinesicherungen basierend auf dem Azure Import/Export-Dienst.
ms.topic: conceptual
ms.date: 1/28/2020
ms.openlocfilehash: 4dae68c46e0072a726bba13139e405b44be0f008
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "78196157"
---
# <a name="overview-of-offline-backup"></a>Übersicht über die Offlinesicherung

Dieser Artikel bietet eine Übersicht über die Offlinesicherung.

Bei den ersten vollständigen Sicherungen in Azure werden meist online große Datenmengen übertragen, sodass eine höhere Netzwerkbandbreite als bei den nachfolgenden Sicherungen erforderlich ist, bei denen nur die inkrementellen Änderungen übertragen werden. Zweigstellen oder Rechenzentren in bestimmten Regionen verfügen nicht immer über ausreichende Netzwerkbandbreite. Daher nehmen diese anfänglichen Sicherungen einige Tage in Anspruch. Während dieser Zeit verwenden die Sicherungen kontinuierlich das gleiche Netzwerk, das für Anwendungen bereitgestellt wurde, die im lokalen Rechenzentrum ausgeführt werden.

Azure Backup unterstützt die „Offlinesicherung“, mit der die anfänglichen Sicherungsdaten offline übertragen werden, ohne dabei Netzwerkbandbreite in Anspruch zu nehmen. Es stellt einen Mechanismus zum Kopieren von Sicherungsdaten auf physische Speichergeräte bereit. Die Geräte werden dann an ein Azure-Rechenzentrum in der Nähe übertragen und in einen Recovery Services-Tresor hochgeladen. Durch diesen Vorgang wird eine stabile Übertragung von Sicherungsdaten ohne die Nutzung von Netzwerkbandbreite sichergestellt.

## <a name="offline-backup-options"></a>Offlinesicherungsoptionen

Die Offlinesicherung wird in zwei Modi angeboten, die auf dem Besitz der Speichergeräte basieren:

- Offlinesicherung basierend auf Azure Data Box (Vorschau)
- Offlinesicherung basierend auf dem Azure Import/Export-Dienst

## <a name="offline-backup-based-on-azure-data-box-preview"></a>Offlinesicherung basierend auf Azure Data Box (Vorschau)

Dieser Modus wird derzeit für den Microsoft Azure Recovery Services (MARS)-Agent in der Vorschauversion unterstützt. Bei dieser Option wird [Azure Data Box](https://azure.microsoft.com/services/databox/) genutzt, um sichere und vor Manipulation geschützte Microsoft-Geräte für die Datenübertragung mit USB-Connectors an Ihr Rechenzentrum oder Remotebüro zu senden. Die Sicherungsdaten werden dabei direkt auf diese Geräte geschrieben. Somit müssen Sie also keine Azure-kompatiblen Datenträger und Connectors erwerben oder temporären Speicher als Stagingspeicherort bereitstellen. Microsoft übernimmt zudem die End-to-End-Transportlogistik, die Sie über das Azure-Portal verfolgen können. 

Nachfolgend ist eine Architektur dargestellt, die die Verschiebung von Sicherungsdaten mithilfe dieser Option veranschaulicht.

![Azure Data Box-Architektur für Azure Backup](./media/offline-backup-overview/azure-backup-databox-architecture.png)

Hier finden Sie einen Überblick über die Architektur:

1. Azure Backup kopiert Sicherungsdaten direkt auf diese vorkonfigurierten Geräte.
2. Sie können diese Geräte dann an ein Azure-Rechenzentrum senden.
3. Azure Data Box kopiert die Daten in das Speicherkonto eines Kunden.
4. Azure Backup kopiert Sicherungsdaten automatisch aus dem Speicherkonto in den designierten Recovery Services-Tresor. Inkrementelle Onlinesicherungen werden geplant.

Informationen zur Verwendung der Offlinesicherung basierend auf Azure Data Box finden Sie unter [Offlinesicherung mit Azure Data Box](offline-backup-azure-data-box.md).

## <a name="offline-backup-based-on-the-azure-importexport-service"></a>Offlinesicherung basierend auf dem Azure Import/Export-Dienst

Diese Option wird von Microsoft Azure Backup Server (MABS), System Center Data Protection Manager (DPM) DPM-A und dem MARS-Agent unterstützt. Sie verwendet den [Azure Import/Export-Dienst](https://docs.microsoft.com/azure/storage/common/storage-import-export-service). Sie können die anfänglichen Sicherungsdaten mithilfe Ihrer eigenen Azure-kompatiblen Datenträger und Connectors nach Azure übertragen. Diese Vorgehensweise erfordert die Bereitstellung eines temporären Speichers, der als Stagingspeicherort bezeichnet wird, sowie die Verwendung von vorgefertigten Dienstprogrammen zum Formatieren und Kopieren der Sicherungsdaten auf Datenträger von Kunden. 

Nachfolgend ist eine Architektur dargestellt, die die Verschiebung von Sicherungsdaten mithilfe dieser Option veranschaulicht.

![Import/Export-Dienstarchitektur für Azure Backup](./media/offline-backup-overview/azure-backup-import-export.png)

Hier finden Sie einen Überblick über die Architektur:

1. Azure Backup sendet die Sicherungsdaten nicht über das Netzwerk, sondern schreibt sie in einen Stagingspeicherort.
2. Die Daten im Stagingspeicherort werden mithilfe eines benutzerdefinierten Hilfsprogramms auf einen oder mehrere SATA-Datenträger geschrieben.
3. Das Hilfsprogramm erstellt zur Vorbereitung einen Azure-Importauftrag. Die SATA-Laufwerke werden an das nächstgelegene Azure-Datencenter gesendet. Geben Sie den Importauftrag an, um die Aktivitäten zu verknüpfen.
4. Im Azure-Datencenter werden die Daten auf den Datenträgern in ein Azure-Speicherkonto kopiert.
5. Azure Backup kopiert die Sicherungsdaten aus dem Speicherkonto in den Recovery Services-Tresor. Inkrementelle Sicherungen werden geplant.

Informationen zur Verwendung einer auf dem Azure Import/Export-Dienst basierenden Offlinesicherung mit dem MARS-Agent finden Sie unter [Offlinesicherungsworkflow in Azure Backup](https://docs.microsoft.com/azure/backup/backup-azure-backup-import-export).

Wenn Sie die Offlinesicherung mit MABS oder DPM-A verwenden möchten, finden Sie weitere Informationen unter [Offlinesicherungsworkflow für DPM und Azure Backup Server](https://docs.microsoft.com/azure/backup/backup-azure-backup-server-import-export-).

## <a name="offline-backup-support-summary"></a>Zusammenfassung der Unterstützung für die Offlinesicherung

In der folgenden Tabelle werden die beiden verfügbaren Optionen verglichen, sodass Sie die entsprechenden Optionen basierend auf Ihrem Szenario treffen können.

| **Aspekt**                                            | **Offlinesicherung basierend auf Azure Data Box**                     | **Offlinesicherung basierend auf dem Azure Import/Export-Dienst**                |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Azure Backup-Bereitstellungsmodelle                              | MARS-Agent (Vorschau)                                              | MARS-Agent, MABS, DPM-A                                           |
| Maximale Sicherungsdaten pro Server (MARS) oder pro Schutzgruppe (MABS, DPM-A) | [Azure Data Box Disk:](https://docs.microsoft.com/azure/databox/data-box-disk-overview) 7,2 TB <br> [Azure Data Box:](https://docs.microsoft.com/azure/databox/data-box-overview) 80 TB       | 80 TB (bis zu 10 Datenträger mit jeweils 8 TB)                          |
| Sicherheit (Daten, Gerät und Dienst)                           | [Daten:](https://docs.microsoft.com/azure/databox/data-box-security#data-box-data-protection) AES-256-Bit-Verschlüsselung <br> [Gerät:](https://docs.microsoft.com/azure/databox/data-box-security#data-box-device-protection) robustes Gehäuse, proprietäre und auf Anmeldeinformationen basierende Schnittstelle zum Kopieren von Daten <br> [Dienst:](https://docs.microsoft.com/azure/databox/data-box-security#data-box-service-protection) Schutz durch Azure-Sicherheitsfeatures | Daten: BitLocker-verschlüsselt                                 |
| Bereitstellung von temporärem Stagingspeicherort                     | Nicht erforderlich                                                | Umfang größer oder gleich der geschätzten Größe der Sicherungsdaten        |
| Unterstützte Regionen                                           | [Azure Data Box Disk-Regionen](https://docs.microsoft.com/azure/databox/data-box-disk-overview#region-availability) <br> [Azure Data Box-Regionen](https://docs.microsoft.com/azure/databox/data-box-disk-overview#region-availability) | [Azure Import/Export-Dienst: Regionen](https://docs.microsoft.com/azure/storage/common/storage-import-export-service#region-availability) |
| Länderübergreifender Versand                                     | Nicht unterstützt  <br>    Die Quelladresse und das gewünschte Azure-Rechenzentrum müssen sich im gleichen Land/in der gleichen Region befinden* | Unterstützt                                                    |
| Transportlogistik (Lieferung, Transport und Abholung)           | Vollständig von Microsoft verwaltet                                     | Vom Kunden verwaltet                                            |
| Preise                                                      | [Azure Data Box: Preise](https://azure.microsoft.com/pricing/details/databox/) <br> [Azure Data Box Disk: Preise](https://azure.microsoft.com/pricing/details/databox/disk/) | [Azure Import/Export-Dienst: Preise](https://azure.microsoft.com/pricing/details/storage-import-export/) |

*Wenn es in Ihrem Land/Ihrer Region kein Azure-Rechenzentrum gibt, müssen Sie Ihre Datenträger an ein Azure-Rechenzentrum in einem anderen Land schicken.

## <a name="next-steps"></a>Nächste Schritte

* [Offlinesicherung in Azure Backup unter Verwendung von Azure Data Box](offline-backup-azure-data-box.md#backup-data-size-and-supported-data-box-skus)
* [Workflow zur Offlinesicherung in Azure Backup](backup-azure-backup-import-export.md) 
* [Offlinesicherungsworkflow für DPM und Azure Backup Server](backup-azure-backup-server-import-export-.md)
