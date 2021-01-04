---
title: Azure Defender für Storage – Vorteile und Features
description: Hier erfahren Sie mehr über die Vorteile und Features von Azure Defender für Storage.
author: memildin
ms.author: memildin
ms.date: 9/22/2020
ms.topic: overview
ms.service: security-center
manager: rkarlin
ms.openlocfilehash: 8979e315f188a5c21cce206c24f195f72096d438
ms.sourcegitcommit: 66479d7e55449b78ee587df14babb6321f7d1757
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/15/2020
ms.locfileid: "97516515"
---
# <a name="introduction-to-azure-defender-for-storage"></a>Einführung in Azure Defender für Storage


Bei **Azure Defender für Storage** handelt es sich um eine native intelligente Azure-Sicherheitsebene, die ungewöhnliche und potenziell schädliche Versuche erkennt, auf Ihre Speicherkonten zuzugreifen oder diese unbefugt zu nutzen. Das Tool verwendet die erweiterten Funktionen von künstlicher Sicherheitsintelligenz und [Microsoft Threat Intelligence](https://go.microsoft.com/fwlink/?linkid=2128684), um kontextbezogene Sicherheitswarnungen und entsprechende Empfehlungen bereitzustellen.

Bei Anomalien im Rahmen von Aktivitäten werden Sicherheitswarnungen ausgelöst. Diese Warnungen sind in Azure Security Center integriert und werden mit Informationen zu verdächtigen Aktivitäten und Empfehlungen zur Untersuchung und Beseitigung von Bedrohungen auch per E-Mail an Abonnementadministratoren gesendet.


## <a name="availability"></a>Verfügbarkeit

|Aspekt|Details|
|----|:----|
|Status des Release:|Allgemein verfügbar (Generally Available, GA)|
|Preise:|**Azure Defender für Storage** wird gemäß den Angaben in der [Preisübersicht](security-center-pricing.md) abgerechnet.|
|Geschützte Speichertypen:|[Blob Storage](https://azure.microsoft.com/services/storage/blobs/)<br>[Azure Files](../storage/files/storage-files-introduction.md)<br>[Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-introduction.md)|
|Clouds:|![Ja](./media/icons/yes-icon.png) Kommerzielle Clouds<br>![Ja](./media/icons/yes-icon.png) US Gov<br>![Nein](./media/icons/no-icon.png) China Gov/andere Gov-Clouds|
|||


## <a name="what-kind-of-alerts-does-azure-defender-for-storage-provide"></a>Welche Art von Warnungen bietet Azure Defender für Storage?

Sicherheitswarnungen werden ausgelöst, wenn Folgendes vorliegt:

- **Verdächtige Zugriffsmuster**, z. B. der erfolgreiche Zugriff von einem Tor-Exitknoten oder von einer IP-Adresse, die von Microsoft Threat Intelligence als verdächtig eingestuft wird
- **Verdächtige Aktivitäten**, z. B. anormale Datenextraktionen oder ungewöhnliche Änderungen der Zugriffsberechtigungen
- **Upload schädlicher Inhalte**, z. B. das Hochladen von potenziellen Malware-Dateien (basierend auf Hashzuverlässigkeitsanalysen) oder das Hosting von Phishinginhalten

Warnungen enthalten Details zum Incident, durch den sie ausgelöst wurden, sowie Empfehlungen zur Untersuchung und Eindämmung von Bedrohungen. Warnungen können in Azure Sentinel oder in ein anderes SIEM-Tool eines Drittanbieters oder in ein anderes externes Tool exportiert werden.

> [!TIP]
> Eine bewährte Methode ist das [Konfigurieren von Azure Defender für Storage](https://docs.microsoft.com/azure/storage/common/azure-defender-storage-configure?tabs=azure-security-center) auf Abonnementebene. Sie können das Tool jedoch auch [für einzelne Speicherkonten konfigurieren](https://docs.microsoft.com/azure/storage/common/azure-defender-storage-configure?tabs=azure-portal).


## <a name="what-is-hash-reputation-analysis-for-malware"></a>Was ist Hashzuverlässigkeitsanalyse für Schadsoftware?

Um zu ermitteln, ob eine hochgeladene Datei verdächtig ist, verwendet Azure Defender für Storage von [Microsoft Threat Intelligence](https://go.microsoft.com/fwlink/?linkid=2128684) unterstützte Hashzuverlässigkeitsanalyse. Die Tools zum Schutz vor Bedrohungen überprüfen die hochgeladenen Dateien nicht, sondern überprüfen die Speicherprotokolle und vergleichen die Hashes von neu hochgeladenen Dateien mit denjenigen bekannter Viren, Trojaner, Spyware und Ransomware. 

Wenn der Verdacht besteht, dass eine Datei Schadsoftware enthält, zeigt Security Center eine Warnung an und kann optional den Speicherbesitzer per E-Mail um Genehmigung bitten, die verdächtige Datei zu löschen. Um diese automatische Entfernung von Dateien festzulegen, die laut Hashzuverlässigkeitsanalyse Schadsoftware enthalten, stellen Sie [Workflowautomatisierung bereit, die bei Warnungen ausgelöst wird, die die Angabe „Upload potenzieller Schadsoftware in ein Speicherkonto“ enthalten](https://techcommunity.microsoft.com/t5/azure-security-center/how-to-respond-to-potential-malware-uploaded-to-azure-storage/ba-p/1452005).

> [!NOTE]
> Wenn Sie die Security Center-Features zum Schutz vor Bedrohungen aktivieren möchten, müssen Sie Azure Defender für das Abonnement mit den entsprechenden Workloads aktivieren.
>
> Sie können **Azure Defender für Storage** entweder auf Abonnement- oder auf Ressourcenebene aktivieren.



## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie mehr über Azure Defender für Storage erfahren.

Weitere Informationen finden Sie in den folgenden Artikeln: 

- Sie können Warnungen exportieren, und zwar unabhängig davon, ob sie von Security Center generiert oder von Security Center über ein anderes Sicherheitsprodukt empfangen wurden. Befolgen Sie die Anweisungen unter [Exportieren von Sicherheitswarnungen und -empfehlungen (Vorschau)](continuous-export.md), um die Warnungen zu Azure Sentinel (oder ein Drittanbieter-SIEM) bzw. ein beliebiges anderes externes Tool zu exportieren.
- [Aktivieren von Azure Defender für Storage](../storage/common/azure-defender-storage-configure.md)
- [Liste der Warnungen von Azure Defender für Storage](alerts-reference.md#alerts-azurestorage)
- [Microsoft Threat Intelligence-Funktionen](https://go.microsoft.com/fwlink/?linkid=2128684)
