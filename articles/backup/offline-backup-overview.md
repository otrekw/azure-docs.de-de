---
title: Übersicht über die Offlinesicherung
description: Informieren Sie sich über die verschiedenen Komponenten der Offlinesicherung einschließlich der Azure Data Box- und Azure Import/Export-basierten Offlinesicherung.
ms.topic: conceptual
ms.date: 1/28/2020
ms.openlocfilehash: 35bc15488624f3648bdc765a36d10607b4ca2de9
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2020
ms.locfileid: "77026490"
---
# <a name="overview-of-offline-backup"></a>Übersicht über die Offlinesicherung

Dieser Artikel bietet eine Übersicht über die Offlinesicherung.

Bei den ersten vollständigen Sicherungen in Azure werden meist online große Datenmengen übertragen, sodass eine höhere Netzwerkbandbreite als bei den nachfolgenden Sicherungen erforderlich ist, bei denen nur die inkrementellen Änderungen übertragen werden. Zweigstellen oder Rechenzentren in bestimmten Regionen verfügen nicht immer über ausreichende Netzwerkbandbreite. Daher nehmen diese anfänglichen Sicherungen einige Tage in Anspruch und verwenden während dieser Zeit kontinuierlich das gleiche Netzwerk, das für Anwendungen bereitgestellt wurde, die im lokalen Rechenzentrum ausgeführt werden.

Azure Backup unterstützt die „Offlinesicherung“, mit der die anfänglichen Sicherungsdaten offline übertragen werden können ohne dabei die Netzwerkbandbreite in Anspruch zu nehmen. Sie bietet einen Mechanismus zum Kopieren von Sicherungsdaten auf physische Speichergeräte, die anschließend in ein Azure-Rechenzentrum geliefert werden und in einen Recovery Services-Tresor hochgeladen werden. Durch diesen Vorgang wird eine stabile Übertragung von Sicherungsdaten ohne die Nutzung von Netzwerkbandbreite sichergestellt.

## <a name="offline-backup-options"></a>Offlinesicherungsoptionen

Die Offlinesicherung wird in zwei Modi angeboten, die auf dem Besitz der Speichergeräte basieren.

1. Azure Data Box-basierte Offlinesicherung (Vorschauversion)
2. Azure Import/Export-basierte Offlinesicherung

## <a name="azure-data-box-based-offline-backup-preview"></a>Azure Data Box-basierte Offlinesicherung (Vorschauversion)

Dieser Modus wird derzeit für den MARS-Agent in der Vorschauversion unterstützt. Bei dieser Option wird der [Azure Data Box-Dienst](https://azure.microsoft.com/services/databox/) genutzt, um sichere und vor Manipulation geschützte Microsoft-Geräte für die Datenübertragung mit USB-Connectors an Ihr Rechenzentrum oder Remotebüro zu senden. Die Sicherungsdaten werden dabei direkt auf diese Geräte geschrieben. **Somit müssen Sie also keine Azure-kompatiblen Datenträger und Connectors erwerben oder temporären Speicher als Stagingspeicherort bereitstellen.** Außerdem übernimmt Microsoft die End-to-End-Transportlogistik, die Sie über das Azure-Portal verfolgen können. Nachfolgend ist eine Architektur dargestellt, die die Verschiebung von Sicherungsdaten mithilfe dieser Option veranschaulicht.

![Azure Data Box-Architektur für Azure Backup](./media/offline-backup-overview/azure-backup-databox-architecture.png)

Hier finden Sie einen Überblick über die Architektur:

1. Azure Backup kopiert Sicherungsdaten direkt auf diese vorkonfigurierten Geräte.
2. Sie können diese Geräte dann an ein Azure-Rechenzentrum senden.
3. Der Azure Data Box-Dienst kopiert die Daten in das Speicherkonto eines Kunden.
4. Azure Backup kopiert Sicherungsdaten automatisch aus dem Speicherkonto in den designierten Recovery Services-Tresor, und inkrementelle Onlinesicherungen werden geplant.

Informationen zur Verwendung der Azure Data Box-basierten Offlinesicherung finden Sie in [diesem Artikel](offline-backup-azure-data-box.md).

## <a name="azure-importexport-based-offline-backup"></a>Azure Import/Export-basierte Offlinesicherung

Diese Option wird von Azure Backup Server (MABS), DPM-A und dem MARS-Agent unterstützt. Dabei wird der [Azure Import/Export-Dienst](https://docs.microsoft.com/azure/storage/common/storage-import-export-service) genutzt, mit dem Sie anfängliche Sicherungsdaten mithilfe Ihrer eigenen Azure-kompatiblen Datenträger und Connectors in Azure übertragen können. Diese Vorgehensweise erfordert die Bereitstellung eines temporären Speichers, der als **Stagingspeicherort** bezeichnet wird, sowie die Verwendung von vorgefertigten Dienstprogrammen zum Formatieren und Kopieren der Sicherungsdaten auf Datenträger von Kunden. Nachfolgend ist eine Architektur dargestellt, die die Verschiebung von Sicherungsdaten mithilfe dieser Option veranschaulicht:

![Import/Export-Architektur für Azure Backup](./media/offline-backup-overview/azure-backup-import-export.png)

Hier finden Sie einen Überblick über die Architektur:

1. Azure Backup sendet die Sicherungsdaten nicht über das Netzwerk, sondern schreibt sie in einen Stagingspeicherort.
2. Die Daten im Stagingspeicherort werden mithilfe eines benutzerdefinierten Hilfsprogramms auf einen oder mehrere SATA-Datenträger geschrieben.
3. Das Hilfsprogramm erstellt zur Vorbereitung einen Azure-Importauftrag. Die SATA-Laufwerke werden an das nächstgelegene Azure-Datencenter gesendet. Geben Sie den Importauftrag an, um die Aktivitäten zu verknüpfen.
4. Im Azure-Datencenter werden die Daten auf den Datenträgern in ein Azure-Speicherkonto kopiert.
5. Azure Backup kopiert die Sicherungsdaten aus dem Speicherkonto in den Recovery Services-Tresor, und inkrementelle Sicherungen werden geplant.

Informationen zur Verwendung der Azure Import/Export-basierten Offlinesicherung mit dem MARS-Agent finden Sie in [diesem Artikel](https://docs.microsoft.com/azure/backup/backup-azure-backup-import-export).

Details zur Nutzung des gleichen Vorgangs mit MABS/DPM-A finden Sie in [diesem Artikel](https://docs.microsoft.com/azure/backup/backup-azure-backup-server-import-export-).

## <a name="offline-backup-support-summary"></a>Zusammenfassung der Unterstützung für die Offlinesicherung

In der folgenden Tabelle werden die beiden verfügbaren Optionen verglichen, sodass Sie die entsprechenden Optionen basierend auf Ihrem Szenario treffen können.

| **Aspekt**                                            | **Azure Data Box-basierte Offlinesicherung**                     | **Azure Import/Export-basierte Offlinesicherung**                |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Azure Backup-Bereitstellungsmodelle                              | MARS-Agent (Vorschauversion)                                              | MARS-Agent, Azure Backup Server (MABS), DPM-A                                           |
| Maximale Sicherungsdaten pro Server (MARS) oder pro Schutzgruppe (MABS, DPM-A) | [Azure Data Box Disk:](https://docs.microsoft.com/azure/databox/data-box-disk-overview) 7,2 TB <br> [Azure Data Box:](https://docs.microsoft.com/azure/databox/data-box-overview) 80 TB       | 80 TB (bis zu 10 Datenträger mit jeweils 8 TB)                          |
| Sicherheit (Daten, Gerät und Dienst)                           | [Daten:](https://docs.microsoft.com/azure/databox/data-box-security#data-box-data-protection) AES-256-Bit-Verschlüsselung <br> [Gerät:](https://docs.microsoft.com/azure/databox/data-box-security#data-box-device-protection) robustes Gehäuse, proprietäre und auf Anmeldeinformationen basierende Schnittstelle zum Kopieren von Daten <br> [Dienst:](https://docs.microsoft.com/azure/databox/data-box-security#data-box-service-protection) Schutz durch Azure-Sicherheitsfeatures | Daten: BitLocker-verschlüsselt                                 |
| Bereitstellung von temporärem Stagingspeicherort                     | Nicht erforderlich                                                | Umfang größer oder gleich der geschätzten Größe der Sicherungsdaten        |
| Unterstützte Regionen                                           | [Azure Data Box Disk-Regionen](https://docs.microsoft.com/azure/databox/data-box-disk-overview#region-availability) <br> [Azure Data Box-Regionen](https://docs.microsoft.com/azure/databox/data-box-disk-overview#region-availability) | [Azure Import/Export-Regionen](https://docs.microsoft.com/azure/storage/common/storage-import-export-service#region-availability) |
| Länderübergreifender Versand*                                     | **Nicht unterstützt**  <br>    *Die Quelladresse und das gewünschte Azure-Rechenzentrum müssen sich im gleichen Land befinden.* | Unterstützt                                                    |
| Transportlogistik (Lieferung, Transport und Abholung)           | Vollständig von Microsoft verwaltet                                     | Vom Kunden verwaltet                                            |
| Preise                                                      | [Azure Data Box: Preise](https://azure.microsoft.com/pricing/details/databox/) <br> [Azure Data Box Disk: Preise](https://azure.microsoft.com/pricing/details/databox/disk/) | [Azure Import/Export: Preise](https://azure.microsoft.com/pricing/details/storage-import-export/) |

* *Wenn es in Ihrem Land kein Azure-Rechenzentrum gibt, müssen Sie Ihre Datenträger an ein Azure-Rechenzentrum in einem anderen Land schicken.*

## <a name="next-steps"></a>Nächste Schritte

* [Azure Data Box-basierte Offlinesicherung für den MARS-Agent](offline-backup-azure-data-box.md#backup-data-size-and-supported-data-box-skus)
* [Azure Import/Export-basierte Offlinesicherung für den MARS-Agent](backup-azure-backup-import-export.md)  
* [Azure Import/Export-basierte Offlinesicherung für MABS/DPM-A](backup-azure-backup-server-import-export-.md)
