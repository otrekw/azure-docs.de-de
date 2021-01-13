---
title: Problembehandlung bei der Pipelineorchestrierung und Pipelinetriggern in Azure Data Factory
description: Wenden Sie verschiedene Methoden an, um Probleme mit Pipelinetriggern in Azure Data Factory zu beheben.
author: ssabat
ms.service: data-factory
ms.date: 12/15/2020
ms.topic: troubleshooting
ms.author: susabat
ms.reviewer: susabat
ms.openlocfilehash: 0e67a316b012eda61607c84edfd8e10d6aa3318d
ms.sourcegitcommit: d2d1c90ec5218b93abb80b8f3ed49dcf4327f7f4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/16/2020
ms.locfileid: "97589157"
---
# <a name="troubleshoot-pipeline-orchestration-and-triggers-in-azure-data-factory"></a>Problembehandlung bei der Pipelineorchestrierung und Pipelinetriggern in Azure Data Factory

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Eine Pipelineausführung in Azure Data Factory definiert eine Instanz einer Pipelineausführung. Angenommen, Sie verfügen über eine Pipeline, die um 8:00, 9:00 und 10:00 Uhr ausgeführt wird. In diesem Fall erfolgen drei separate Ausführungen der Pipeline (oder Pipelineausführungen). Jede Pipelineausführung besitzt eine eindeutige Pipelineausführungs-ID. Eine Ausführungs-ID ist ein eindeutiger Bezeichner – eine GUID (Globally Unique Identifier) –, der die jeweilige Pipelineausführung eindeutig definiert.

Zur Instanziierung von Pipelineausführungen werden in der Regel Argumente an in der Pipeline definierte Parameter übergeben. Sie können eine Pipeline entweder manuell oder mithilfe eines Triggers ausführen. Weitere Informationen finden Sie unter [Pipelineausführung und -trigger in Azure Data Factory](concepts-pipeline-execution-triggers.md).

## <a name="common-issues-causes-and-solutions"></a>Bekannte Probleme, Ursachen und Lösungen

### <a name="pipeline-with-azure-function-throws-error-with-private-end-point-connectivity"></a>Eine Pipeline mit einer Azure-Funktion löst einen Fehler bei Verbindungen mit einem privaten Endpunkt aus
 
#### <a name="issue"></a>Problem
In einem bestimmten Kontext werden Data Factory und die Azure-Funktions-App auf einem privaten Endpunkt ausgeführt. Sie versuchen, eine Pipeline auszuführen, die mit der Azure-Funktions-App interagiert. Sie haben drei verschiedene Methoden ausprobiert, aber eine gibt den Fehler `Bad Request` zurück, und die anderen beiden Methoden geben `103 Error Forbidden` zurück.

#### <a name="cause"></a>Ursache 
Data Factory unterstützt derzeit keinen Connector für private Endpunkte für Azure-Funktions-Apps. Dies sollte die Ursache dafür sein, warum die Aufrufe von der Azure-Funktions-App abgelehnt werden, da sie so konfiguriert wurde, dass sie nur Verbindungen von Private Link zulässt.

#### <a name="resolution"></a>Lösung
Wenn Sie einen privaten Endpunkt vom Typ **PrivateLinkService** erstellen und den DNS-Eintrag Ihrer Funktions-App angeben, sollte die Verbindung funktionieren.

### <a name="pipeline-run-is-killed-but-the-monitor-still-shows-progress-status"></a>Die Pipelineausführung wird zwar abgebrochen, aber in der Überwachung wird weiterhin der Fortschritt angezeigt.

#### <a name="issue"></a>Problem
Wenn Sie eine Pipelineausführung beenden, zeigt die Pipelineüberwachung häufig weiterhin den Fortschritt an. Dies geschieht aufgrund eines Cacheproblems im Browser und weil Sie nicht die richtigen Filter für die Überwachung verwenden.

#### <a name="resolution"></a>Lösung
Aktualisieren Sie den Browser, und wenden Sie die richtigen Filter für die Überwachung an.
 
### <a name="copy-pipeline-failure--found-more-columns-than-expected-column-count-delimitedtextmorecolumnsthandefined"></a>Fehler beim Kopieren der Pipeline: Es wurden mehr Spalten als die erwartete Spaltenanzahl gefunden (DelimitedTextMoreColumnsThanDefined).

#### <a name="issue"></a>Problem  
Wenn die Dateien in einem bestimmten Ordner, den Sie kopieren, Dateien mit unterschiedlichen Schemas enthalten (wie z. B. eine variable Anzahl von Spalten, unterschiedliche Trennzeichen oder abweichende Anführungszeichen) oder wenn sie andere Datenprobleme aufweisen, wird die Data Factory-Pipeline mit diesem Fehler beendet:

`
Operation on target Copy_sks  failed: Failure happened on 'Sink' side.
ErrorCode=DelimitedTextMoreColumnsThanDefined,
'Type=Microsoft.DataTransfer.Common.Shared.HybridDeliveryException,
Message=Error found when processing 'Csv/Tsv Format Text' source '0_2020_11_09_11_43_32.avro' with row number 53: found more columns than expected column count 27.,
Source=Microsoft.DataTransfer.Common,'
`

#### <a name="resolution"></a>Lösung
Wählen Sie beim Erstellen der Aktion „Daten kopieren“ die Option „Binärkopie“ aus. Mit der Option **Binärkopie** wird Data Factory beim Massenkopieren oder dem Migrieren Ihrer Daten zwischen Data Lakes die Dateien nicht öffnen und auch nicht das Schema lesen, sondern jede Datei wie eine Binärdatei behandeln und an den neuen Speicherort kopieren.

### <a name="pipeline-run-fails-when-capacity-limit-of-integration-runtime-is-reached"></a>Die Pipelineausführung führt zu einem Fehler, wenn die Kapazitätsgrenze der Integration Runtime erreicht ist.

#### <a name="issue"></a>Problem
Fehlermeldung:

`
Type=Microsoft.DataTransfer.Execution.Core.ExecutionException,Message=There are substantial concurrent MappingDataflow executions which is causing failures due to throttling under Integration Runtime 'AutoResolveIntegrationRuntime'.
`

Der Fehler gibt die Beschränkung pro Integration Runtime an, die derzeit 50 beträgt. Weitere Informationen finden Sie unter [Grenzwerte](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#version-2).

Wenn Sie große Mengen an Datenflüssen gleichzeitig mit derselben Integration Runtime ausführen, kann dies zu diesem Fehlertyp führen.

#### <a name="resolution"></a>Lösung 
- Trennen Sie diese Pipelines, um ihre Ausführung zu unterschiedlichen Zeiten auszulösen.
- Erstellen Sie eine neue Integration Runtime, und teilen Sie diese Pipelines auf mehrere Integration Runtimes auf.

### <a name="how-to-monitor-pipeline-failures-on-regular-interval"></a>Überwachen von Pipelinefehlern in regelmäßigen Abständen

#### <a name="issue"></a>Problem
Es ist häufig erforderlich, Data Factory-Pipelines in Intervallen zu überwachen, z. B. alle 5 Minuten. Sie können die Pipelineausführungen einer Data Factory mithilfe des Endpunkts abfragen und filtern. 

#### <a name="recommendation"></a>Empfehlung
1. Richten Sie eine Azure-Logik-App ein, um alle fehlerhaften Pipelines alle 5 Minuten abzufragen.
2. Anschließend können Sie dem Ticketsystem Vorfälle nach [QueryByFactory](https://docs.microsoft.com/rest/api/datafactory/pipelineruns/querybyfactory) melden.

#### <a name="reference"></a>Referenz
- [Senden von Benachrichtigungen aus einer Azure Data Factory-Pipeline](https://www.mssqltips.com/sqlservertip/5962/send-notifications-from-an-azure-data-factory-pipeline--part-2/)

### <a name="how-to-handle-activity-level-errors-and-failures-in-pipelines"></a>Behandeln von Fehlern auf Aktivitätsebene und in Pipelines

#### <a name="issue"></a>Problem
Die Azure Data Factory-Orchestrierung erlaubt bedingte Logik und ermöglicht es, basierend auf den Ergebnissen einer vorherigen Aktivität verschiedene Pfade zu nutzen. Vier bedingte Pfade sind zulässig: „Bei Erfolg (Standardpfad)“, „Bei Fehlern“, „Nach Abschluss“ und „Beim Überspringen“. Die Verwendung verschiedener Pfade ist zulässig.

Azure Data Factory definiert Erfolg und Misserfolg der Pipelineausführung wie folgt:

- Zunächst wird das Ergebnis für alle Aktivitäten auf Blattebene ausgewertet. Wenn eine Blattaktivität übersprungen wurde, wird stattdessen die übergeordnete Aktivität ausgewertet.
- Das Pipelineergebnis gilt nur dann als erfolgreich, wenn alle Blätter erfolgreich waren.

#### <a name="recommendation"></a>Empfehlung
- Implementieren Sie Überprüfungen auf Aktivitätsebene. Lesen Sie dazu [Understanding Pipeline Failures and Error Handling](https://techcommunity.microsoft.com/t5/azure-data-factory/understanding-pipeline-failures-and-error-handling/ba-p/1630459) (Informationen zu Pipelinefehlern und zur Fehlerbehandlung).
- Verwenden Sie eine Azure-Logik-App, um Pipelines wie in [Abfragen nach Data Factory]( https://docs.microsoft.com/rest/api/datafactory/pipelineruns/querybyfactory) beschrieben in regelmäßigen Abständen zu überwachen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Problembehandlung finden Sie in diesen Ressourcen:

*  [Data Factory-Blog](https://azure.microsoft.com/blog/tag/azure-data-factory/)
*  [Data Factory-Funktionsanfragen](https://feedback.azure.com/forums/270578-data-factory)
*  [Azure-Videos](https://azure.microsoft.com/resources/videos/index/?sort=newest&services=data-factory)
*  [Frageseite von Microsoft Q&A (Fragen und Antworten)](/answers/topics/azure-data-factory.html)
*  [Twitter-Informationen über Data Factory](https://twitter.com/hashtag/DataFactory)