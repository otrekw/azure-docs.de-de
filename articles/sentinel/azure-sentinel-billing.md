---
title: 'Azure Sentinel: Kosten und Abrechnung | Microsoft-Dokumentation'
description: Erfahren Sie mehr über das Azure Sentinel-Preismodell, Schätzen und Verwalten von Azure Sentinel-Kosten und Grundlegendes zu Kosten und Abrechnung.
services: sentinel
author: batamig
ms.author: bagol
ms.service: azure-sentinel
ms.topic: conceptual
ms.date: 06/03/2021
ms.openlocfilehash: 0d9c016741fcdb36a2803eaf9705e9dc2ea1782b
ms.sourcegitcommit: c072eefdba1fc1f582005cdd549218863d1e149e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111950301"
---
# <a name="azure-sentinel-costs-and-billing"></a>Azure Sentinel: Kosten und Abrechnung

Azure Sentinel nutzt eine umfangreiche Abfragesprache zum Analysieren, Interagieren mit und Ableiten von Erkenntnissen aus großen Mengen operativer Daten in Sekundenschnelle. Azure Sentinel speichert seine Daten zur Analyse in Log Analytics-Arbeitsbereichen von Azure Monitor.

Falls für einen Log Analytics-Arbeitsbereich aktiviert, analysiert Azure Sentinel automatisch alle vom Arbeitsbereich erfassten Daten und stellt die Datenmenge in Rechnung, die vom Arbeitsbereich erfasst und gespeichert wird. In diesem Artikel wird erläutert, wie Sie die Nutzung von und Kosten (sowie Einsparmöglichkeiten) für Azure Sentinel und zugehörige Log Analytics-Arbeitsbereiche überwachen und verstehen können.

## <a name="azure-sentinel-pricing-model"></a>Azure Sentinel-Preismodell

Azure Sentinel bietet ein flexibles und vorhersagbares Preismodell. Weitere Informationen finden Sie auf der Seite [Azure Sentinel – Preise](https://azure.microsoft.com/pricing/details/azure-sentinel/). Die zugehörigen Log Analytics-Gebühren finden Sie unter [Azure Monitor Log Analytics – Preise](https://azure.microsoft.com/pricing/details/log-analytics/).

### <a name="pay-as-you-go-and-commitment-tiers"></a>Die Tarife „Nutzungsbasierte Zahlung“ und „Mindestabnahme“

Für den Azure Sentinel-Dienst gibt es zwei Zahlungsoptionen: Nutzungsbasierte Zahlung und Mindestabnahme.

„Nutzungsbasierte Zahlung“ ist das Standardmodell, das auf dem tatsächlich gespeicherten Datenvolumen basiert und optional eine Datenaufbewahrung über 90 Tage hinaus vorsieht. Das Datenvolumen wird in GB (10^9 Bytes) gemessen.

Log Analytics und Azure Sentinel bieten auch den Tarif „Mindestabnahme“ (zuvor als „Kapazitätsreservierungen“ bezeichnet), der besser kalkulierbar ist und im Vergleich zu „Nutzungsbasierte Zahlung“ eine Ersparnis bis zu 65 % bringt. Mit dem Tarif „Mindestabnahme“ können Sie Kapazität ab 100 GB/Tag buchen. Sämtliche Nutzung, die die gebuchte Kapazität überschreitet, wird mit dem von Ihnen für die Mindestabnahme gewählten Satz in Rechnung gestellt. Bei einer Mindestabnahme von 100 GB/Tag wird beispielsweise das zugesagte Datenvolumen von 100 GB/Tag plus jedes weitere GB/Tag zum ermäßigten Satz für diesen Tarif in Rechnung gestellt.

Sie können Ihre Mindestabnahme jederzeit erhöhen und alle 31 Tage verringern, um die Kosten zu optimieren, sobald Ihr Datenvolumen zu- oder abnimmt. Um Ihren aktuellen Azure Sentinel-Tarif einzusehen, wählen Sie im linken Navigationsbereich von Azure Sentinel **Einstellungen** und dann die Registerkarte **Preise** aus. Ihr aktueller Tarif ist als **Aktueller Tarif** markiert.

Informationen zum Festlegen und Ändern Ihrer Mindestabnahme finden Sie unter [Festlegen oder Ändern des Tarifs](#set-or-change-pricing-tier).

### <a name="costs-for-other-services"></a>Kosten anderer Dienste

Azure Sentinel kann in viele andere Azure-Dienste integriert werden, um erweiterte Funktionen bereitzustellen. Zu diesen Diensten gehören Azure Logic Apps, Azure Notebooks und BYOML-Modelle (Bring Your Own Machine Learning). Für einige dieser Dienste fallen möglicherweise zusätzliche Gebühren an. Einige der Datenconnectors und -lösungen von Azure Sentinel verwenden für die Datenerfassung Azure Functions, wodurch ebenfalls gesonderte Kosten anfallen.

Preisdetails für diese Dienste finden Sie unter:

- [Azure Logic Apps – Preise](https://azure.microsoft.com/pricing/details/logic-apps/)
- [Azure Machine Learning – Preise](https://azure.microsoft.com/pricing/details/machine-learning/)
- [Machine Learning Studio (Classic) – Preise](https://azure.microsoft.com/pricing/details/machine-learning-studio/)
- [Azure Functions – Preise](https://azure.microsoft.com/en-us/pricing/details/functions/)

Für alle anderen Dienste, die Sie nutzen, können Kosten anfallen.

### <a name="data-retention-costs"></a>Datenaufbewahrungskosten

Nachdem Sie Azure Sentinel in einem Log Analytics-Arbeitsbereich aktiviert haben, können Sie alle im Arbeitsbereich erfassten Daten in den ersten 90 Tagen kostenlos aufbewahren. Eine Aufbewahrung über 90 Tage hinaus wird gemäß den standardmäßigen [Log Analytics-Aufbewahrungspreisen](https://azure.microsoft.com/pricing/details/monitor/) in Rechnung gestellt.

Sie können für einzelne Datentypen unterschiedliche Aufbewahrungseinstellungen angeben. Weitere Informationen finden Sie unter [Aufbewahrung nach Datentyp](../azure-monitor/logs/manage-cost-storage.md#retention-by-data-type).

### <a name="free-trial"></a>Kostenlose Testversion

Sie können Azure Sentinel für einen neuen oder vorhandenen Log Analytics-Arbeitsbereich in den ersten 31 Tagen ohne zusätzliche Kosten aktivieren. Gebühren im Zusammenhang mit Log Analytics, Azure Automation und BYOML fallen im kostenlosen Testzeitraum dennoch an. Die Nutzung über die ersten 31 Tage hinaus wird gemäß den [Preisen für Azure Sentinel](https://azure.microsoft.com/pricing/details/azure-sentinel) in Rechnung gestellt. 

### <a name="free-data-sources"></a>Kostenlose Datenquellen

Die folgenden Datenquellen lassen sich kostenlos mit Azure Sentinel nutzen:

- Azure-Aktivitätsprotokolle.
- Office 365-Überwachungsprotokolle, einschließlich aller SharePoint-Aktivitäten, Exchange-Administratoraktivitäten und Teams.
- Microsoft Defender-Warnungen, einschließlich Warnungen von Azure Defender, Microsoft 365 Defender, Microsoft Defender für Office 365, Microsoft Defender for Identity und Microsoft Defender für Endpunkt.
- Warnungen von Azure Security Center and Microsoft Cloud App Security (MCAS). Rohprotokolle für einige Datentypen von Microsoft 365 Defender, MCAS, Azure Active Directory (Azure AD) und Azure Information Protection (AIP) sind jedoch kostenpflichtig.

In der folgenden Tabelle sind die kostenlosen Datenquellen aufgeführt, die Sie in Azure Sentinel aktivieren können. Einige der Datenconnectors, z. B. Microsoft 365 Defender und MCAS, enthalten sowohl kostenlose als auch kostenpflichtige Datentypen.

| Azure Sentinel-Datenconnector   | Datentyp | Kostenlos oder kostenpflichtig |
|-------------------------------------|--------------------------------|------------------|
| **Azure-Aktivitätsprotokolle**         | AzureActivity                  | Kostenlos             |
| **Office 365**                     | OfficeActivity (SharePoint)    | Kostenlos|
|| OfficeActivity (Exchange)|Kostenlos|
|| OfficeActivity (Teams)          | Kostenlos|
| **Azure Defender**                  | SecurityAlert (ASC)             | Kostenlos             |
| **Azure Defender für IoT**          | SecurityAlert (ASC for IoT)     | Kostenlos             |
| **Microsoft 365 Defender**          | SecurityIncident | Kostenlos|
||SecurityAlert| Kostenlos|
||DeviceEvents                    | Kostenpflichtig|
||DeviceFileEvents                | Kostenpflichtig|
||DeviceImageLoadEvents           | Kostenpflichtig|
||DeviceInfo                      | Kostenpflichtig|
||DeviceLogonEvents               | Kostenpflichtig|
||DeviceNetworkEvents             | Kostenpflichtig|
||DeviceNetworkInfo               | Kostenpflichtig|
||DeviceProcessEvents             | Kostenpflichtig|
||DeviceRegistryEvents            | Kostenpflichtig|
||DeviceFileCertificateInfo       | Kostenpflichtig|
| **Microsoft Defender für den Endpunkt** | SecurityAlert (MDATP)          | Kostenlos             |
| **Microsoft Defender for Identity** | SecurityAlert (AATP)           | Kostenlos             |
| **Microsoft Cloud App Security**   | SecurityAlert (MCAS)           | Kostenlos             |
||MCASShadowITReporting           | Kostenpflichtig|

Für Datenconnectors, die sowohl kostenlose als auch kostenpflichtige Datentypen enthalten, können Sie auswählen, welche Datentypen Sie aktivieren möchten.

![Screenshot der Seite „Datenconnector“ für MCAS, auf der die kostenlosen Sicherheitswarnungen ausgewählt sind und die kostenpflichtige Option MCASShadowITReporting nicht ausgewählt ist](media/billing/data-types.png)

## <a name="estimate-azure-sentinel-costs"></a>Schätzen der Azure Sentinel-Kosten

Wenn Sie Azure Sentinel noch nicht einsetzen, können Sie mithilfe des [Azure Sentinel-Preisrechners](https://azure.microsoft.com/pricing/calculator/?service=azure-sentinel) die potenziellen Kosten der Nutzung von Azure Sentinel schätzen. Geben Sie *Azure Sentinel* in das Feld „Suchen“ ein, und wählen Sie die resultierende Kachel „Azure Sentinel“ aus. Mit dem Preisrechner können Sie Ihre wahrscheinlichen Kosten auf Grundlage Ihres erwarteten Datenerfassungs- und -aufbewahrungsaufkommens schätzen.

Beispielsweise können Sie das tägliche Datenaufkommen (in GB), das Sie in Azure Sentinel erfassen möchten, und die Region für Ihren Arbeitsbereich eingeben. Der Rechner gibt die aggregierten monatlichen Kosten für diese Komponenten aus:

- Log Analytics-Datenerfassung 
- Azure Sentinel-Datenanalyse 
- Log Analytics-Datenaufbewahrung 

## <a name="manage-azure-sentinel-costs"></a>Verwalten von Azure Sentinel-Kosten

Es gibt mehrere Möglichkeiten, die Azure Sentinel-Nutzung und -Kosten zu verstehen und zu verwalten.

Verwalten von Datenerfassung und -aufbewahrung:

- [Wählen des Tarifs „Mindestabnahme“, um die Kosten basierend auf Ihrem Datenerfassungsvolumen zu optimieren](#set-or-change-pricing-tier)
- [Festlegen einer Obergrenze für das Log Analytics-Datenvolumen](#define-a-data-volume-cap-in-log-analytics), um die Erfassung zu verwalten, wobei die Obergrenze nicht für sicherheitsrelevante Daten gilt
- [Optimieren der Log Analytics-Kosten mithilfe dedizierter Cluster](#optimize-log-analytics-costs-with-dedicated-clusters)
- [Ablegen nicht sicherheitsrelevanter Daten in einem anderen Arbeitsbereich](#separate-non-security-data-in-a-different-workspace)
- [Senken langfristiger Datenaufbewahrungskosten mit Azure Data Explorer](#reduce-long-term-data-retention-costs-with-adx)

Verstehen von, Überwachen auf und Warnen bei Datenerfassungs- und Kostenänderungen:

- [Ausführen von Abfragen zum Verstehen Ihrer Datenerfassung](#run-queries-to-understand-your-data-ingestion)
- [Bereitstellen einer Arbeitsmappe zum Visualisieren der Datenerfassung](#deploy-a-workbook-to-visualize-data-ingestion)
- [Verwenden eines Cost Management-Playbooks](#use-a-playbook-for-cost-management-alerts), das eine Warnung senden kann, wenn die Erfassung einen vordefinierten Schwellenwert überschreitet
- [Grundlegendes zur CEF-Datenerfassung (Common Event Format)](#understand-cef-ingestion-volume)

### <a name="manage-data-ingestion-and-retention"></a>Verwalten von Datenerfassung und -aufbewahrung

Verwalten Sie mit den folgenden Methoden die Datenerfassung und -aufbewahrung für Ihren Azure Sentinel-Arbeitsbereich.

#### <a name="set-or-change-pricing-tier"></a>Festlegen oder Ändern des Tarifs

Zum Erzielen der höchsten Einsparungen überwachen Sie Ihr Erfassungsvolumen, um sicherzustellen, dass Sie die Mindestabnahme gebucht haben, die am ehesten mit Ihren Erfassungsvolumenmustern übereinstimmt. Sie können Ihre Mindestabnahme erhöhen oder verringern, um sie an sich ändernde Datenvolumen anzupassen.

Sie können Ihre Mindestabnahme jederzeit erhöhen, wodurch der zugehörige Zeitraum von 31 Tagen neu gestartet wird. Wenn Sie jedoch zur nutzungsbasierten Zahlung oder zu einer niedrigeren Mindestabnahme zurückkehren möchten, müssen Sie warten, bis der 31-tägige Mindestabnahmezeitraum abgelaufen ist. Die Abrechnung für Mindestabnahmen erfolgt täglich.

Um Ihren aktuellen Azure Sentinel-Tarif einzusehen, wählen Sie im linken Navigationsbereich von Azure Sentinel **Einstellungen** und dann die Registerkarte **Preise** aus. Ihr aktueller Tarif ist als **Aktueller Tarif** markiert.

Um die gebuchte Mindestabnahme zu ändern, wählen Sie auf der Preisseite einen der anderen Tarife und dann **Übernehmen** aus. Sie müssen in Azure Sentinel die Rolle **Mitwirkender** oder **Besitzer** haben, um den Tarif zu ändern.

![Screenshot der Seite „Preise“ in den Azure Sentinel-Einstellungen, wobei „Nutzungsbasierte Zahlung“ als aktueller Tarif angegeben ist.](media/billing/pricing.png)

> [!NOTE]
> Datenerfassungsvolumen von Azure Sentinel werden in einigen Nutzungsdiagrammen im Portal unter **Einblicke in die Sicherheit** angezeigt.

In den Azure Sentinel-Tarifen sind Log Analytics-Gebühren nicht inbegriffen. Informationen zum Ändern Ihres gebuchten Tarifs für Log Analytics finden Sie unter [Ändern des Tarifs](../azure-monitor/logs/manage-cost-storage.md#changing-pricing-tier).

#### <a name="define-a-data-volume-cap-in-log-analytics"></a>Festlegen einer Obergrenze für das Datenvolumen in Log Analytics

In Log Analytics können Sie eine tägliche Volumenobergrenze aktivieren, die die tägliche Erfassung für Ihren Arbeitsbereich einschränkt. Die tägliche Obergrenze kann Ihnen helfen, unerwartete Anstiege des Datenvolumens zu bewältigen, ihr Budget einzuhalten und ungeplante Gebühren zu begrenzen.

Um eine tägliche Volumenobergrenze festzulegen, wählen Sie im linken Navigationsbereich Ihres Log Analytics-Arbeitsbereichs **Nutzungs- und geschätzte Kosten** und dann Tägliche **Obergrenze** aus. Wählen Sie **Ein** aus, geben Sie eine tägliche Volumenobergrenze ein, und wählen Sie dann **OK** aus.

![Screenshot: Bildschirm „Nutzungs- und geschätzte Kosten“ und Fenster „Tägliche Obergrenze“.](media/billing/daily-cap.png)

Auf dem Bildschirm **Nutzung und geschätzte Kosten** sind auch der Trend des erfassten Datenvolumens in den letzten 31 Tagen und das gesamte gespeicherte Datenvolumen zu sehen.

> [!IMPORTANT]
> Die tägliche Obergrenze begrenzt nicht die Sammlung aller Datentypen. Weitere Informationen zum Verwalten der täglichen Obergrenze in Log Analytics finden Sie unter [Verwalten des maximalen täglichen Datenvolumens](../azure-monitor/logs/manage-cost-storage.md#manage-your-maximum-daily-data-volume).

#### <a name="optimize-log-analytics-costs-with-dedicated-clusters"></a>Optimieren der Log Analytics-Kosten mithilfe dedizierter Cluster

Wenn Sie in Ihrem Azure Sentinel-Arbeitsbereich bzw. in Arbeitsbereichen in derselben Region mindestens 1 TB pro Tag erfassen, sollten Sie zur Kostensenkung einen Umstieg auf einen dedizierten Log Analytics-Cluster in Betracht ziehen. Eine Mindestabnahme für einen dedizierten Log Analytics-Cluster aggregiert das Datenvolumen mehrerer Arbeitsbereiche, die zusammen mindestens 1 TB pro Tag erfassen.

Für dedizierte Log Analytics-Cluster gelten nicht die Azure Sentinel-Mindestabnahmen. Azure Sentinel-Kosten fallen im dedizierten Cluster weiter arbeitsbereichsbezogen an.

Sie können einem dedizierten Log Analytics-Cluster mehrere Azure Sentinel-Arbeitsbereiche hinzufügen. Der Einsatz eines dedizierten Log Analytics-Clusters für Azure Sentinel bietet verschiedene Vorteile:

- Arbeitsbereichsübergreifende Abfragen erfolgen schneller, wenn sich alle an der Abfrage beteiligten Arbeitsbereiche im dedizierten Cluster befinden. Es empfiehlt sich nach wie vor, so wenig Arbeitsbereiche wie möglich in Ihrer Umgebung zu betreiben, und für einen dedizierten Cluster gilt unverändert die [Obergrenze von 100 Arbeitsbereichen](../azure-monitor/logs/cross-workspace-query.md) für die Einbeziehung in eine einzelne arbeitsbereichsübergreifende Abfrage.

- Alle Arbeitsbereiche im dedizierten Cluster können die für den Cluster festgelegten Log Analytics-Mindestabnahme gemeinsam nutzen. Dadurch, dass keine separaten Log Analytics-Mindestabnahmen für jeden Arbeitsbereich verpflichtend sind, lassen sich Kosten einsparen und mehr Effizienz erzielen. Durch Aktivieren eines dedizierten Clusters verpflichten Sie sich zu einer Mindestabnahme für Log Analytics von 1 TB pro Tag.

Im Folgenden finden Sie einige weitere Überlegungen zum Umstieg auf einen dedizierten Cluster zur Kostenoptimierung:

- Die maximale Anzahl aktiver Cluster pro Region und Abonnement ist 2.
- Alle mit einem Cluster verknüpften Arbeitsbereiche müssen sich in der gleichen Region befinden.
- Mit einem Cluster können maximal 1000 Arbeitsbereiche verknüpft sein.
- Sie können die Verknüpfung eines Arbeitsbereichs mit Ihrem Cluster aufheben. Die Anzahl der Verknüpfungsvorgänge in einem bestimmten Arbeitsbereich ist innerhalb eines Zeitraums von 30 Tagen auf 2 begrenzt.
- Sie können einen vorhandenen Arbeitsbereich nicht in einen Cluster mit kundenseitig verwaltetem Schlüssel verschieben. Sie müssen den Arbeitsbereich im Cluster erstellen.
- Das Verschieben eines Clusters in eine andere Ressourcengruppe oder ein anderes Abonnement wird derzeit nicht unterstützt.
- Eine Verknüpfung eines Arbeitsbereichs mit einem Cluster schlägt fehl, wenn der Arbeitsbereich mit einem anderen Cluster verknüpft ist.

Weitere Informationen zu dedizierten Clustern finden Sie unter [Dedizierte Log Analytics-Cluster](../azure-monitor/logs/manage-cost-storage.md#log-analytics-dedicated-clusters).

#### <a name="separate-non-security-data-in-a-different-workspace"></a>Ablegen nicht sicherheitsrelevanter Daten in einem anderen Arbeitsbereich

Azure Sentinel analysiert alle Daten, die in für Azure Sentinel aktivierten Log Analytics-Arbeitsbereichen erfasst werden. Es empfiehlt sich ein separater Arbeitsbereich für nicht sicherheitsrelevante Betriebsdaten, um sicherzustellen, dass für diese keine Azure Sentinel-Kosten anfallen.

Wenn Sie Bedrohungen in Azure Sentinel suchen oder untersuchen, müssen Sie möglicherweise auf operative Daten zugreifen, die in diesen eigenständigen Azure Log Analytics-Arbeitsbereichen gespeichert sind. Sie können auf diese Daten zugreifen, indem Sie bei der Protokolluntersuchung und in Arbeitsmappen arbeitsbereichsübergreifende Abfragen verwenden. Sie können allerdings nur dann arbeitsbereichsübergreifende Analyseregeln und Huntingabfragen nutzen, wenn Azure Sentinel für alle Arbeitsbereiche aktiviert ist.

#### <a name="reduce-long-term-data-retention-costs-with-adx"></a>Senken langfristiger Datenaufbewahrungskosten mit Azure Data Explorer

Die Azure Sentinel-Datenaufbewahrung ist in den ersten 90 Tagen kostenlos. Um den Datenaufbewahrungszeitraum in Log Analytics zu ändern, wählen Sie im linken Navigationsbereich **Nutzungs- und geschätzte Kosten** und dann **Datenaufbewahrung** aus. Passen Sie anschließend den Schieberegler an.

Azure Sentinel-Sicherheitsdaten verlieren nach einigen Monaten möglicherweise einen Teil ihres Nutzens. SOC-Benutzer (Security Operations Center) müssen möglicherweise nicht so häufig auf ältere Daten zugreifen wie auf neuere, benötigen aber möglicherweise dennoch den Zugriff auf die Daten für sporadische Untersuchungen oder zu Prüfzwecken. Um Azure Sentinel-Kosten für Datenaufbewahrung zu senken, können Sie Azure Data Explorer für die langfristige Datenaufbewahrung zu geringeren Kosten verwenden. Azure Data Explorer bietet das richtige Gleichgewicht von Kosten und Nutzen älterer Daten, für die die Sicherheitsfunktionen von Azure Sentinel nicht mehr erforderlich sind.

Mit Azure Data Explorer können Sie Daten zu niedrigeren Kosten speichern, die Daten aber dennoch mit den gleichen KQL-Abfragen (Kusto Query Language) wie in Azure Sentinel erkunden. Sie können mit dem Proxyfeature von Azure Data Explorer auch plattformübergreifende Abfragen durchführen. Diese Abfragen aggregieren und korrelieren Daten, die auf Azure Data Explorer, Application Insights, Azure Sentinel und Log Analytics verteilt sind.

Weitere Informationen finden Sie unter [Integrieren von Azure Data Explorer für die langfristige Protokollaufbewahrung](store-logs-in-azure-data-explorer.md).

### <a name="understand-monitor-and-alert-for-changes-in-data-ingestion-and-costs"></a>Verstehen von, Überwachen auf und Warnen bei Datenerfassungs- und Kostenänderungen

Mithilfe der folgenden Methoden können Sie Änderungen in Ihrem Azure Sentinel-Arbeitsbereich verstehen, überwachen und entsprechende Warnungen ausgeben.

#### <a name="run-queries-to-understand-your-data-ingestion"></a>Ausführen von Abfragen zum Verstehen Ihrer Datenerfassung

Es folgen einige Abfragen, mit deren Hilfe Sie das Datenerfassungsvolumen verstehen können.

Führen Sie die folgende Abfrage aus, um das Datenerfassungsvolumen nach Lösung anzuzeigen: 

```kusto
Usage
| where StartTime >= startofday(ago(31d)) and EndTime < startofday(now())
| where IsBillable == true
| summarize BillableDataGB = sum(Quantity) / 1000. by bin(StartTime, 1d), Solution
| extend Solution = iif(Solution == "SecurityInsights", "AzureSentinel", Solution)
| render columnchart
```

Führen Sie die folgende Abfrage aus, um das Datenerfassungsvolumen nach Datentyp anzuzeigen: 

```kusto
Usage
| where StartTime >= startofday(ago(31d)) and EndTime < startofday(now())
| where IsBillable == true
| summarize BillableDataGB = sum(Quantity) / 1000. by bin(StartTime, 1d), DataType
| render columnchart
```
Führen Sie die folgende Abfrage aus, um das Datenerfassungsvolumen nach Lösung und Datentyp anzuzeigen: 

```kusto
Usage
| where TimeGenerated > ago(32d)
| where StartTime >= startofday(ago(31d)) and EndTime < startofday(now())
| where IsBillable == true
| summarize BillableDataGB = sum(Quantity) by Solution, DataType
| extend Solution = iif(Solution == "SecurityInsights", "AzureSentinel", Solution)
| sort by Solution asc, DataType asc
```

#### <a name="deploy-a-workbook-to-visualize-data-ingestion"></a>Bereitstellen einer Arbeitsmappe zum Visualisieren der Datenerfassung

Die **Arbeitsmappe „Bericht zur Arbeitsbereichsnutzung“** stellt Datennutzungs-, Kosten- und Nutzungsstatistiken für Ihren Arbeitsbereich bereit. Die Arbeitsmappe gibt den Status der Datenerfassung im Arbeitsbereich und die Menge der kostenlosen und abrechenbaren Daten an. Sie können mithilfe der Arbeitsmappenlogik Datenerfassung und Kosten überwachen sowie benutzerdefinierte Ansichten und regelbasierte Warnungen erstellen.

Diese Arbeitsmappe bietet auch detaillierte Erfassungsdetails. In der Arbeitsmappe sind die Daten in Ihrem Arbeitsbereich nach Datentabellen aufgeschlüsselt, und es werden Volumen pro Tabelle und Eintrag angegeben, damit Sie Ihre Erfassungsmuster besser verstehen können.

So aktivieren Sie die Arbeitsmappe „Bericht zur Arbeitsbereichsnutzung“

1. Wählen Sie in Azure Sentinel im linken Navigationsbereich **Bedrohungsmanagement** > **Arbeitsmappen** aus.
1. Geben Sie in der Suchleiste *Arbeitsbereichsnutzung* ein, und wählen Sie dann **Bericht zur Arbeitsbereichsnutzung** aus.
1. Wählen Sie **Vorlage anzeigen** aus, um die Arbeitsmappe unverändert zu verwenden, oder wählen Sie **Speichern** aus, um eine bearbeitbare Kopie der Arbeitsmappe zu erstellen. Wenn Sie eine Kopie speichern, wählen Sie **Gespeicherte Arbeitsmappe anzeigen** aus.
1. Wählen Sie in der Arbeitsmappe das **Abonnement** und den **Arbeitsbereich** aus, den Sie anzeigen möchten, und legen Sie dann **TimeRange** auf den Zeitrahmen fest, den Sie einsehen möchten. Sie können den Umschalter **Hilfe anzeigen** auf **Ja** festlegen, um in der Arbeitsmappe Erklärungen anzuzeigen.

#### <a name="use-a-playbook-for-cost-management-alerts"></a>Verwenden eines Playbooks für Kostenverwaltungswarnungen

Sie können ein Playbook zur Kostenverwaltung erstellen, um Ihr Azure Sentinel-Budget zu steuern. Das Playbook sendet Ihnen eine Warnung, wenn der Azure Sentinel-Arbeitsbereich innerhalb eines bestimmten Zeitraums ein von Ihnen definiertes Budget überschreitet.

Die GitHub-Community für Azure Sentinel stellt das Playbook [Send-IngestionCostAlert](https://github.com/iwafula025/Azure-Sentinel/tree/master/Playbooks/Send-IngestionCostAlert) zur Kostenverwaltung auf GitHub zur Verfügung. Dieses Playbook wird durch einen Wiederholungstrigger aktiviert und bietet ein hohes Maß an Flexibilität. Sie können die Ausführungshäufigkeit, das Erfassungsvolumen und die auszulösende Meldung basierend auf Ihren Anforderungen steuern.

#### <a name="understand-cef-ingestion-volume"></a>Verstehen des CEF-Erfassungsvolumens

CEF ist ein unterstütztes Syslog-Ereignisformat in Azure Sentinel. Sie mithilfe von CEF nützliche Sicherheitsinformationen aus einer Vielzahl von Quellen in Ihrem Azure Sentinel-Arbeitsbereich erfassen. CEF-Protokolle werden in die Tabelle CommonSecurityLog in Azure Sentinel gesammelt, die alle standardmäßigen aktuellen CEF-Felder enthält.

Viele Geräte und Datenquellen lassen Protokollierungsfelder zu, die über das CEF-Standardschema hinausgehen. Diese zusätzlichen Felder werden in der Tabelle AdditionalExtensions angezeigt. Diese Felder können höhere Erfassungsvolumen als die CEF-Standardfelder haben, da der Ereignisinhalt in diesen Feldern variabel sein kann.

## <a name="understand-your-azure-sentinel-costs-and-bill"></a>Grundlegendes zu Azure Sentinel-Kosten und -Abrechnung

Es ist wichtig, Ihre Azure Sentinel-Kosten zu verstehen und im Blick zu behalten. Der Hub [Azure Cost Management + Billing](../cost-management-billing/costs/quick-acm-cost-analysis.md) bietet nützliche Funktionen. Nachdem Sie im Azure-Portal **Cost Management + Billing** geöffnet haben, wählen Sie im linken Navigationsbereich **Cost Management** aus. Wählen Sie dann den [Bereich](..//cost-management-billing/costs/understand-work-scopes.md) oder die Gruppe der zu untersuchenden Ressourcen aus, z. B. ein Azure-Abonnement oder eine Ressourcengruppe.

Um Ihre Azure-Rechnung einzusehen, wählen Sie im linken Navigationsbereich von **Cost Management + Billing** die Option **Kostenanalyse** aus. Wählen Sie auf dem Bildschirm **Kostenanalyse** den Dropdownpfeil im Feld **Ansicht** und dann **Rechnungsdetails** aus.

> [!NOTE]
> Die in diesem Bild gezeigten Kosten dienen nur als Beispiel. Sie sollen keine tatsächlichen Kosten widerspiegeln.

![Screenshot des Azure Sentinel-Abschnitts mit einer Azure-Beispielrechnung.](media/billing/sample-bill.png)

Azure Sentinel- und Log Analytics-Gebühren werden auf Ihrer Azure-Rechnung basierend auf Ihrem ausgewählten Tarif als separate Posten ausgewiesen. Wenn Sie die Mindestabnahme für Ihren Arbeitsbereich in einem bestimmten Monat überschreiten, weist die Azure-Rechnung einen Einzelposten für die Mindestabnahme mit den damit verbundenen Fixkosten und einen separaten Einzelposten für die über die Mindestabnahme hinausgehende Erfassung aus, die zum gleichen Mindestabnahmesatz abgerechnet wird.

Die folgende Tabelle zeigt, wie Azure Sentinel- und Log Analytics-Kosten in den Spalten **Dienstname** und **Verbrauchseinheit** Ihrer Azure-Rechnung ausgewiesen werden:

|Cost|Dienstname|Zähler|
|----|------------|----------------|
|Azure Sentinel-Mindestabnahme|**sentinel**|**`n` GB Mindestabnahme**|
|Log Analytics-Mindestabnahme|**Azure Monitor**|**`n` GB Mindestabnahme**|
|Azure Sentinel-Überschreitung der Mindestabnahme oder Nutzungsbasierte Zahlung|**sentinel**|**Analyse**|
|Log Analytics-Überschreitung der Mindestabnahme oder Nutzungsbasierte Zahlung|**Protokollanalyse**|**Datenerfassung**|

Weitere Informationen zum Anzeigen und Herunterladen Ihrer Azure-Rechnung finden Sie unter [Anzeigen und Herunterladen der Azure-Nutzung und -Gebühren](../cost-management-billing/understand/download-azure-daily-usage.md).

Auf dem Bildschirm **Kostenanalyse** werden auch detaillierte Ansichten Ihrer Azure-Nutzung und -Kosten angezeigt, auf die Sie eine Vielzahl von Steuerelementen und Filtern anwenden können.

So können Sie z. B. Diagramme Ihrer täglichen Kosten für einen bestimmten Zeitrahmen einsehen:
1. Wählen Sie im Feld **Ansicht** den Dropdownpfeil und dann **Akkumulierte Kosten** oder **Tägliche Kosten** aus.
1. Wählen Sie den Dropdownpfeil im Datumsfeld und dann einen Datumsbereich aus.
1. Wählen Sie den Dropdownpfeil neben **Granularität** und dann **Täglich** aus.

> [!NOTE]
> Die in diesem Bild gezeigten Kosten dienen nur als Beispiel. Sie sollen keine tatsächlichen Kosten widerspiegeln.

![Screenshot des Bildschirms „Kostenanalyse“ in „Cost Management + Billing“.](media/billing/cost-management.png)

Sie können auch weitere Steuerelemente anwenden. Um beispielsweise nur die mit Azure Sentinel verbundenen Kosten anzuzeigen, wählen Sie **Filter hinzufügen**, anschließend **Dienstname** und dann die Dienstnamen **Sentinel**, **Log Analytics** und **Azure Monitor** aus.

## <a name="next-steps"></a>Nächste Schritte
Weitere Tipps zum Reduzieren des Log Analytics-Datenvolumens finden Sie unter [Tipps zum Reduzieren der Datenmenge](../azure-monitor/logs/manage-cost-storage.md#tips-for-reducing-data-volume).