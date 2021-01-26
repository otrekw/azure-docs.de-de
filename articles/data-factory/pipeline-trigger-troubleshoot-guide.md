---
title: Problembehandlung bei der Pipelineorchestrierung und Pipelinetriggern in Azure Data Factory
description: Wenden Sie verschiedene Methoden an, um Probleme mit Pipelinetriggern in Azure Data Factory zu beheben.
author: ssabat
ms.service: data-factory
ms.date: 12/15/2020
ms.topic: troubleshooting
ms.author: susabat
ms.reviewer: susabat
ms.openlocfilehash: 0ceee3c65e8c4df5d843bb441fb6426a0f4eb696
ms.sourcegitcommit: d59abc5bfad604909a107d05c5dc1b9a193214a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/14/2021
ms.locfileid: "98220251"
---
# <a name="troubleshoot-pipeline-orchestration-and-triggers-in-azure-data-factory"></a>Problembehandlung bei der Pipelineorchestrierung und Pipelinetriggern in Azure Data Factory

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Eine Pipelineausführung in Azure Data Factory definiert eine Instanz einer Pipelineausführung. Beispiel: Angenommen, Sie verfügen über eine Pipeline, die um 8:00, 9:00 und 10:00 Uhr ausgeführt wird. In diesem Fall erfolgen drei separate Pipelineausführungen. Jede Pipelineausführung besitzt eine eindeutige Pipelineausführungs-ID. Eine Ausführungs-ID ist eine GUID (Globally Unique Identifier), die die jeweilige Pipelineausführung eindeutig definiert.

Zur Instanziierung von Pipelineausführungen werden in der Regel Argumente an in der Pipeline definierte Parameter übergeben. Sie können eine Pipeline entweder manuell oder mithilfe eines Triggers ausführen. Weitere Informationen finden Sie unter [Pipelineausführung und -trigger in Azure Data Factory](concepts-pipeline-execution-triggers.md).

## <a name="common-issues-causes-and-solutions"></a>Bekannte Probleme, Ursachen und Lösungen

### <a name="an-azure-functions-app-pipeline-throws-an-error-with-private-endpoint-connectivity"></a>Eine Pipeline für eine Azure Functions-App löst einen Fehler bei privaten Endpunktverbindungen aus
 
Sie führen Data Factory und eine Azure-Funktions-App auf einem privaten Endpunkt aus. Sie versuchen, eine Pipeline auszuführen, die mit der Funktions-App interagiert. Sie haben drei verschiedene Methoden ausprobiert, aber eine gibt den Fehler „Ungültige Anforderung“ zurück, und die anderen beiden Methoden geben „Fehler 103: Unzulässig“ zurück.

**Ursache:** Data Factory unterstützt für Funktions-Apps derzeit keinen Connector für private Endpunkte. Azure Functions lehnt Aufrufe ab, weil gemäß Konfiguration nur Verbindungen über eine private Verbindung zugelassen werden.

**Lösung:** Erstellen Sie einen **PrivateLinkService**-Endpunkt, und geben Sie den DNS der Funktions-App an.

### <a name="a-pipeline-run-is-canceled-but-the-monitor-still-shows-progress-status"></a>Eine Pipelineausführung wurde zwar abgebrochen, aber der Status wird weiterhin in der Überwachung angezeigt

Wenn Sie eine Pipelineausführung abbrechen, wird der Status häufig weiterhin in der Pipelineüberwachung aufgeführt. Dies geschieht aufgrund eines Browsercacheproblems. Außerdem verwenden Sie möglicherweise nicht die richtigen Überwachungsfilter.

**Lösung:** Aktualisieren Sie den Browser, und wenden Sie die richtigen Überwachungsfilter an.
 
### <a name="you-see-a-delimitedtextmorecolumnsthandefined-error-when-copying-a-pipeline"></a>Beim Kopieren einer Pipeline wird der Fehler „DelimitedTextMoreColumnsThanDefined" angezeigt
 
Wenn ein Ordner, den Sie kopieren, Dateien mit unterschiedlichen Schemas (z. B. eine variable Anzahl von Spalten, unterschiedliche Trennzeichen oder abweichende Anführungszeichen) oder Datenproblemen enthält, löst die Data Factory-Pipeline möglicherweise diesen Fehler aus:

`
Operation on target Copy_sks  failed: Failure happened on 'Sink' side.
ErrorCode=DelimitedTextMoreColumnsThanDefined,
'Type=Microsoft.DataTransfer.Common.Shared.HybridDeliveryException,
Message=Error found when processing 'Csv/Tsv Format Text' source '0_2020_11_09_11_43_32.avro' with row number 53: found more columns than expected column count 27.,
Source=Microsoft.DataTransfer.Common,'
`

**Lösung:** Wählen Sie beim Erstellen der Copy-Aktivität die Option **Binärkopie** aus. Auf diese Weise werden die Dateien bei Massenkopien oder bei der Migration Ihrer Daten von einem Data Lake zu einem anderen nicht von Data Factory geöffnet, um das Schema zu lesen. Stattdessen behandelt Data Factory jede Datei als Binärdatei und kopiert sie in den anderen Speicherort.

### <a name="a-pipeline-run-fails-when-you-reach-the-capacity-limit-of-the-integration-runtime"></a>Eine Pipelineausführung führt zu einem Fehler, wenn Sie die Kapazitätsgrenze der Integration Runtime erreichen

Fehlermeldung:

`
Type=Microsoft.DataTransfer.Execution.Core.ExecutionException,Message=There are substantial concurrent MappingDataflow executions which is causing failures due to throttling under Integration Runtime 'AutoResolveIntegrationRuntime'.
`

**Ursache:** Sie haben die Kapazitätsgrenze der Integration Runtime erreicht. Möglicherweise führen Sie große Mengen an Datenflüssen gleichzeitig mit derselben Integration Runtime aus. Weitere Informationen finden Sie unter [Grenzwerte für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#version-2).

**Lösung:**
 
- Führen Sie Ihre Pipelines zu verschiedenen Triggerzeiten aus.
- Erstellen Sie eine neue Integration Runtime, und teilen Sie Ihre Pipelines auf mehrere Integration Runtimes auf.

### <a name="you-have-activity-level-errors-and-failures-in-pipelines"></a>Fehler auf Aktivitätsebene und in Pipelines

Die Azure Data Factory-Orchestrierung erlaubt bedingte Logik und ermöglicht es, basierend auf dem Ergebnis einer vorherigen Aktivität verschiedene Pfade zu nutzen. Vier bedingte Pfade sind zulässig: **Bei Erfolg** (Standardpfad), **Bei Fehlern**, **Nach Abschluss** und **Bei Überspringen**. 

Azure Data Factory wertet das Ergebnis aller Aktivitäten auf Blattebene aus. Die Pipelineergebnisse gelten nur dann als erfolgreich, wenn alle Blätter erfolgreich waren. Wenn eine Blattaktivität übersprungen wurde, wird stattdessen die übergeordnete Aktivität ausgewertet. 

**Lösung**

1. Implementieren Sie Überprüfungen auf Aktivitätsebene. Lesen Sie dazu [Informationen zu Pipelinefehlern und zur Fehlerbehandlung](https://techcommunity.microsoft.com/t5/azure-data-factory/understanding-pipeline-failures-and-error-handling/ba-p/1630459).
1. Verwenden Sie Azure Logic Apps, um Pipelines in regelmäßigen Abständen wie unter [Abfragen nach Factory](https://docs.microsoft.com/rest/api/datafactory/pipelineruns/querybyfactory) beschrieben zu überwachen.

## <a name="monitor-pipeline-failures-in-regular-intervals"></a>Überwachen von Pipelinefehlern in regelmäßigen Abständen

Es ist häufig erforderlich, fehlerhafte Data Factory-Pipelines in Intervallen zu überwachen, z. B. alle 5 Minuten. Sie können die Pipelineausführungen einer Data Factory mithilfe des Endpunkts abfragen und filtern. 

Richten Sie eine Azure-Logik-App ein, um alle fehlerhaften Pipelines alle 5 Minuten abzufragen. Dieser Vorgang wird unter [Abfragen nach Factory](https://docs.microsoft.com/rest/api/datafactory/pipelineruns/querybyfactory) beschrieben. Anschließend können Sie Incidents in unserem Ticketsystem melden.

Weitere Informationen finden Sie unter [Senden von Benachrichtigungen von Data Factory, Teil 2](https://www.mssqltips.com/sqlservertip/5962/send-notifications-from-an-azure-data-factory-pipeline--part-2/).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Problembehandlung finden Sie in diesen Ressourcen:

*  [Data Factory-Blog](https://azure.microsoft.com/blog/tag/azure-data-factory/)
*  [Data Factory-Funktionsanfragen](https://feedback.azure.com/forums/270578-data-factory)
*  [Azure-Videos](https://azure.microsoft.com/resources/videos/index/?sort=newest&services=data-factory)
*  [Frageseite von Microsoft Q&A (Fragen und Antworten)](/answers/topics/azure-data-factory.html)
*  [Twitter-Informationen über Data Factory](https://twitter.com/hashtag/DataFactory)
