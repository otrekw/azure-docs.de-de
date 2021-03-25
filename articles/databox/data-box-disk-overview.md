---
title: Microsoft Azure Data Box-Datenträger – Übersicht | Microsoft-Dokumentation
description: Enthält eine Beschreibung der Cloudlösung für Azure Data Box-Datenträger, die zum Übertragen von großen Datenmengen nach Azure dient.
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: overview
ms.date: 06/18/2019
ms.author: alkohli
Customer intent: As an IT admin, I need to understand what Data Box Disk is and how it works so I can use it to import on-premises data into Azure.
ms.openlocfilehash: ea9353ed8c1938fa9b33585a0650b4507c671451
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "92125029"
---
# <a name="what-is-azure-data-box-disk"></a>Was ist ein Azure Data Box-Datenträger?

Mit der Lösung für Microsoft Azure Data Box-Datenträger können Sie lokale Daten im Terabyte-Bereich schnell, kostengünstig und zuverlässig an Azure senden. Die sichere Datenübertragung wird beschleunigt, indem Sie ein bis fünf SSD-Datenträger (Solid-State Disks) erhalten. Diese verschlüsselten Datenträger mit einem Speicherplatz von 8 TB werden über einen regionalen Kurierdienst an Ihr Rechenzentrum gesendet.

Sie können die Datenträger über den Data Box-Dienst im Azure-Portal schnell konfigurieren, anschließen und entsperren. Kopieren Sie Ihre Daten auf die Datenträger, und senden Sie die Datenträger zurück an Azure. Im Azure-Rechenzentrum werden Ihre Daten automatisch von Laufwerken in die Cloud hochgeladen, indem ein Link für das schnelle Hochladen ins private Netzwerk verwendet wird.

## <a name="use-cases"></a>Anwendungsfälle

Verwenden Sie Data Box-Datenträger, um Daten im TB-Bereich in Fällen zu übertragen, in denen keine oder nur eingeschränkte Netzwerkkonnektivität vorhanden ist. Die Datenverschiebung kann eine einmalige, eine periodische oder eine erste Massenübertragung von Daten sein, auf die regelmäßige Übertragungen folgen.

- **Einmalige Migration**: Wird verwendet, wenn eine große Menge von lokalen Daten in Azure verschoben wird. Ein Beispiel hierfür ist das Verschieben von Daten von Offlinebändern in den Archivdatenbereich der Speicherebene „Kalt“ von Azure.
- **Inkrementelle Übertragung**: Wird verwendet, wenn eine erste Massenübertragung per Data Box-Datenträger (Seed) durchgeführt wird, gefolgt von inkrementellen Übertragungen über das Netzwerk. Beispielsweise werden Commvault und Data Box-Datenträger verwendet, um Sicherungskopien zurück in Azure zu verschieben. Auf diesen Migrationsvorgang folgt das Kopieren von inkrementellen Daten über das Netzwerk in Azure Storage.
- **Periodische Uploads**: Werden verwendet, wenn regelmäßig große Datenmengen generiert werden und in Azure verschoben werden müssen. Ein Beispiel hierfür ist die Exploration in der Energiebranche, bei der Videodaten auf Bohrinseln und für Windfarmen generiert werden.

### <a name="ingestion-of-data-from-data-box"></a>Erfassung von Daten aus Data Box

Azure-Anbieter und Nicht-Azure-Anbieter können Daten aus Azure Data Box erfassen. Zu den Azure-Diensten, die eine Datenerfassung aus Azure Data Box bieten, gehören:

- **SharePoint Online**: Verwenden Sie Azure Data Box und das SharePoint-Migrationstool (SPMT), um Inhalte von Dateifreigaben zu SharePoint Online zu migrieren. Mit Data Box sind Sie zum Übertragen von Daten nicht mehr auf eine WAN-Verbindung angewiesen. Weitere Informationen finden Sie unter [Verwenden der Azure Data Box Heavy zum Migrieren von Dateifreigabeinhalten zu SharePoint Online](data-box-heavy-migrate-spo.md).

- **Azure-Dateisynchronisierung**: Replizieren Sie Dateien aus Data Box in eine Azure-Dateifreigabe. Dadurch können Sie Ihre Dateidienste in Azure zentralisieren und gleichzeitig den lokalen Zugriff auf Ihre Daten aufrechterhalten. Weitere Informationen finden Sie unter [Bereitstellen der Azure-Dateisynchronisierung](../storage/files/storage-sync-files-deployment-guide.md).

- **HDFS-Speicher**: Migrieren Sie Daten aus einem lokalen HDFS-Speicher (Hadoop Distributed File System) Ihres Hadoop-Clusters zu Azure Storage mithilfe von Data Box. Weitere Informationen finden Sie unter [Migrieren von lokalem HDFS-Speicher zu Azure Storage mit Azure Data Box](../storage/blobs/data-lake-storage-migrate-on-premises-hdfs-cluster.md).

- **Azure Backup**: Ermöglicht Ihnen das Verschieben großer Sicherungen kritischer Unternehmensdaten über Offlinemechanismen in einen Azure Recovery Services-Tresor. Weitere Informationen finden Sie in der [Übersicht zu Azure Backup](../backup/backup-overview.md).

Sie können Ihre Data Box-Daten mit vielen Nicht-Azure-Dienstanbietern verwenden. Beispiel:

- **[Commvault](http://documentation.commvault.com/commvault/v11/article?p=97276.htm)** : Ermöglicht Ihnen das Migrieren großer Datenmengen zu Microsoft Azure mithilfe von Azure Data Box.
- **[Veeam](https://helpcenter.veeam.com/docs/backup/hyperv/osr_adding_data_box.html?ver=100)** : Ermöglicht Ihnen das Sichern und Replizieren großer Datenmengen von Ihrem Hyper-V-Computer in Data Box.

Eine Liste weiterer Nicht-Azure-Dienstanbieter, die eine Integration mit Data Box bieten, finden Sie unter [Azure Data Box Partners](https://cloudchampions.blob.core.windows.net/db-partners/PartnersTable.pdf).

## <a name="the-workflow"></a>Workflow

Der Workflow umfasst üblicherweise die folgenden Schritte:

1. **Auftrag**: Erstellen Sie im Azure-Portal einen Auftrag, und geben Sie die Versandinformationen und das Azure Storage-Zielkonto für Ihre Daten an. Wenn Datenträger verfügbar sind, werden in Azure die Verschlüsselung, die Vorbereitung und der Versand der Datenträger mit einer ID für die Nachverfolgung durchgeführt.

2. **Empfang**: Nachdem Sie die Datenträger erhalten haben, können Sie sie auspacken und an den Computer anschließen, auf dem sich die zu kopierenden Daten befinden. Entsperren Sie die Datenträger.

3. **Datenkopiervorgang**: Kopieren Sie die Daten per Drag & Drop auf die Datenträger.

4. **Rücksendung**: Bereiten Sie die Datenträger vor, und senden Sie sie zurück an das Azure-Rechenzentrum.

5. **Upload**: Die Daten werden automatisch von den Datenträgern in den Speicher von Azure kopiert. Die Datenträger werden gemäß den NIST-Richtlinien (National Institute of Standards and Technology) auf sichere Weise gelöscht.

Bei diesem Prozess werden Sie per E-Mail über alle Statusänderungen informiert. Weitere Informationen zu den Details des Datenflusses finden Sie unter [Bereitstellen von Data Box-Datenträgern im Azure-Portal](data-box-disk-quickstart-portal.md).

## <a name="benefits"></a>Vorteile

Data Box-Datenträger sind so ausgelegt, dass große Datenmengen ohne negative Auswirkungen auf das Netzwerk in Azure verschoben werden können. Diese Lösung hat die folgenden Vorteile:

- **Geschwindigkeit**: Für den Data Box-Datenträger wird eine USB 3.0-Verbindung genutzt, um bis zu 35 TB an Daten in weniger als einer Woche in Azure zu verschieben.

- **Benutzerfreundlichkeit**: Data Box ist eine benutzerfreundliche Lösung.

  - Für die Datenträger wird die USB-Konnektivität genutzt, bei der fast kein Einrichtungsaufwand anfällt.
  - Die Datenträger haben einen kleinen Formfaktor und sind daher leicht verwendbar.
  - Die Datenträger benötigen keine externe Stromversorgung.
  - Die Datenträger können auf einem Server des Rechenzentrums, einem Desktopcomputer oder einem Laptop verwendet werden.
  - Die Lösung ermöglicht die End-to-End-Nachverfolgung über das Azure-Portal.

- **Schutz**: Data Box-Datenträger verfügen für die Datenträger, die Daten und den Dienst über einen integrierten Sicherheitsschutz.
  - Die Datenträger sind manipulationssicher und unterstützen eine Funktion für sichere Updates.
  - Die Daten auf den Datenträgern sind ununterbrochen durch eine AES 128-Bit-Verschlüsselung geschützt.
  - Die Datenträger können nur mit einem Schlüssel entsperrt werden, der über das Azure-Portal bereitgestellt wird.
  - Der Dienst ist durch Azure-Sicherheitsfunktionen geschützt.
  - Nachdem Ihre Daten in Azure hochgeladen wurden, werden die Datenträger gemäß den NIST-Standards (800-88r1) vollständig bereinigt.  

Weitere Informationen finden Sie unter [Azure Data Box-Datenträger – Sicherheit und Schutz von Daten (Vorschauversion)](data-box-disk-security.md).

## <a name="features-and-specifications"></a>Funktionen und Spezifikationen

| Spezifikationen                                          | BESCHREIBUNG              |
|---------------------------------------------------------|--------------------------|
| Weight                                                  | Weniger als ca. 900 g pro Paket; Paket enthält maximal fünf Datenträger.                |
| Dimensionen                                              | Datenträger – SSD 2,5 Zoll |
| Kabel                                                  | 1 USB-3.1-Kabel pro Datenträger|
| Speicherkapazität pro Auftrag                              | 40 TB (ca. 35 TB nutzbar)|
| Kapazität des Datenträgerspeichers                                   | 8 TB (ca. 7 TB nutzbar)|
| Datenschnittstelle                                          | USB   |
| Sicherheit                                                | Vorverschlüsselt per BitLocker und sichere Updatefunktion <br> Per Hauptschlüssel geschützte Datenträger <br> Ununterbrochene Verschlüsselung der Daten  |
| Datenübertragungsrate                                      | bis zu 430 MBit/s je nach Dateigröße      |
|Verwaltung                                               | Azure-Portal |

## <a name="region-availability"></a>Regionale Verfügbarkeit

Informationen zur regionalen Verfügbarkeit finden Sie unter [Verfügbare Produkte nach Region](https://azure.microsoft.com/global-infrastructure/services/?products=databox&regions=all). Data Box Disk kann auch in der Azure Government-Cloud bereitgestellt werden. Weitere Informationen finden Sie unter [What is Azure Government?](../azure-government/documentation-government-welcome.md) (Was ist Azure Government?).

## <a name="pricing"></a>Preise

Informationen zu den Preisen finden Sie in der [Preisübersicht](https://azure.microsoft.com/pricing/details/databox/disk/).

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich über die [Anforderungen für Data Box-Datenträger](data-box-disk-system-requirements.md).
- Machen Sie sich mit den [Einschränkungen für Data Box-Datenträger](data-box-disk-limits.md) vertraut.
- Informieren Sie sich über die schnelle Bereitstellung von [Azure Data Box-Datenträgern](data-box-disk-quickstart-portal.md) im Azure-Portal.