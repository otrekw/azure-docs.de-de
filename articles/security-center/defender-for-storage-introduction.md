---
title: Azure Defender für Storage – Vorteile und Features
description: Hier erfahren Sie mehr über die Vorteile und Features von Azure Defender für Storage.
author: memildin
ms.author: memildin
ms.date: 9/22/2020
ms.topic: overview
ms.service: security-center
manager: rkarlin
ms.openlocfilehash: 9b2855f82927b6f1707fd748f097dd357818ac4b
ms.sourcegitcommit: f88074c00f13bcb52eaa5416c61adc1259826ce7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2020
ms.locfileid: "92341973"
---
# <a name="introduction-to-azure-defender-for-storage"></a>Einführung in Azure Defender für Storage

**Azure Defender für Storage** erkennt potenziell schädliche Aktivitäten in Ihren Azure Storage-Konten. Ihre Daten können geschützt werden, und zwar unabhängig davon, ob Sie als Blobcontainer, Dateifreigaben oder Data Lakes gespeichert werden.

Dank dieser Schutzebene können Sie auch *ohne* fundierte Sicherheitskenntnisse etwas gegen Bedrohungen unternehmen und Ihre Systeme für die Sicherheitsüberwachung verwalten.


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

- **Verdächtige Aktivitäten**: Auf das Speicherkonto wurde z. B. erfolgreich über eine IP-Adresse zugegriffen, bei der es sich um einen bekannten aktiven Exitknoten von Tor handelt.
- **Anormales Verhalten**: Beispielsweise Änderungen des Zugriffsmusters für ein Speicherkonto.
- **Upload potenzieller Malware**: Hashzuverlässigkeitsanalyse zeigt an, dass eine hochgeladene Datei Schadsoftware enthält.

Warnungen enthalten Details zum Incident, durch den sie ausgelöst wurden, sowie Empfehlungen zur Untersuchung und Eindämmung von Bedrohungen.

> [!TIP]
> Sie können Speicherwarnungen simulieren, indem Sie die Anweisungen in [diesem Blogbeitrag](https://techcommunity.microsoft.com/t5/azure-security-center/validating-atp-for-azure-storage-detections-in-azure-security/ba-p/1068131) befolgen.


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