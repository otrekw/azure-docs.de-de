---
title: Verwalten von Nutzung und Kosten für Azure Monitor-Protokolle
description: Hier erfahren Sie, wie Sie in Azure Monitor den Tarif ändern und das Datenvolumen sowie die Aufbewahrungsrichtlinie für Ihren Log Analytics-Arbeitsbereich verwalten.
services: azure-monitor
documentationcenter: azure-monitor
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 03/28/2021
ms.author: bwren
ms.openlocfilehash: b76812abb80c466480e9982a6c8c4ace64fab624
ms.sourcegitcommit: 4a54c268400b4158b78bb1d37235b79409cb5816
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108139667"
---
# <a name="manage-usage-and-costs-with-azure-monitor-logs"></a>Verwalten von Nutzung und Kosten mit Azure Monitor-Protokollen    

> [!NOTE]
> In diesem Artikel wird beschrieben, wie Sie die Kosten für Azure Monitor-Protokolle ermitteln und steuern. In einem verwandten Artikel, [Überwachen der Nutzung und der geschätzten Kosten](../usage-estimated-costs.md), wird erläutert, wie die Nutzung und geschätzten Kosten über mehrere Azure-Überwachungsfeatures hinweg für unterschiedliche Preismodelle angezeigt werden. Alle in diesem Artikel gezeigten Preise und Kosten dienen nur zu Beispielzwecken. 

Azure Monitor-Protokolle sind für die Skalierung und Unterstützung der täglichen Sammlung, Indizierung und Speicherung enormer Datenmengen aus beliebigen Quellen in Ihrem Unternehmen oder aus in Azure bereitgestellten Quellen konzipiert.  Dies ist zwar ggf. die primäre Motivation für die Verwendung in Ihrem Unternehmen, letztendlich geht es jedoch um Kosteneffizienz. In diesem Zusammenhang ist es wichtig zu wissen, dass die Kosten eines Log Analytics-Arbeitsbereichs nicht nur auf dem Umfang der gesammelten Daten basieren, sondern auch davon abhängen, welcher Tarif gewählt wurde und wie lange die von den verbundenen Quellen generierten Daten gespeichert werden sollen.  

In diesem Artikel erfahren Sie, wie Sie das erfasste Datenvolumen und Speicherwachstum proaktiv überwachen und Grenzwerte festlegen, um die damit verbundenen Kosten zu steuern. 

## <a name="pricing-model"></a>Preismodell

Die Standardpreise für Log Analytics werden gemäß der **nutzungsbasierten Zahlung** berechnet und basieren auf dem verbrauchten Datenvolumen, und optional wird die längere Datenaufbewahrung berücksichtigt. Das Datenvolumen wird als Größe der gespeicherten Daten in GB (10^9 Bytes) gemessen. Jeder Log Analytics-Arbeitsbereich wird als separater Dienst abgerechnet und auf der Rechnung für Ihr Azure-Abonnement aufgeführt. Die bei der Erfassung anfallende Datenmenge kann erheblich sein und hängt von den folgenden Faktoren ab: 

  - Anzahl der aktivierten Verwaltungslösungen und deren Konfiguration
  - Anzahl der überwachten VMs
  - Typ der Daten, die von jeder überwachten VM gesammelt werden 
  
Zusätzlich zum Modell der nutzungsbasierten Bezahlung bietet Log Analytics Tarife für die **Kapazitätsreservierung**. Mit diesen Tarifen können Sie Einsparungen von 25 % gegenüber der nutzungsbasierten Zahlung erzielen. Die Preise der Kapazitätsreservierung ermöglichen Ihnen den Kauf einer Reservierung ab 100 GB/Tag. Jeder über die Reservierung hinausgehende Verbrauch wird entsprechend der nutzungsbasierten Bezahlung berechnet. Die Tarife für die Kapazitätsreservierung umfassen einen Verpflichtungszeitraum von 31 Tagen. Während dieses Verpflichtungszeitraums können Sie in einen höheren Tarif für die Kapazitätsreservierung wechseln (hierbei wird der 31-tägige Verpflichtungszeitraum neu gestartet), aber es ist erst nach Ablauf des Verpflichtungszeitraums möglich, zum nutzungsbasierten Modell oder zu einem niedrigeren Tarif für die Kapazitätsreservierung zurückzukehren. Bei den Tarifen für die Kapazitätsreservierung erfolgt die Abrechnung täglich. [Erfahren Sie mehr](https://azure.microsoft.com/pricing/details/monitor/) über Preise der nutzungsbasierten Bezahlung und Kapazitätsreservierung für Log Analytics. 

In allen Tarifen wird die Datengröße eines Ereignisses anhand einer Zeichenfolgendarstellung der Eigenschaften berechnet, die in Log Analytics für dieses Ereignis gespeichert werden, unabhängig davon, ob die Daten von einem Agent gesendet oder während des Erfassungsvorgangs hinzugefügt werden. Dies schließt alle [benutzerdefinierten Felder](custom-fields.md) ein, die beim Sammeln von Daten hinzugefügt und anschließend in Log Analytics gespeichert werden. Mehrere Eigenschaften, die alle Datentypen gemein haben, einschließlich einiger [Log Analytics-Standardeigenschaften](./log-standard-columns.md), werden bei der Berechnung der Ereignisgröße nicht berücksichtigt. Dazu gehören `_ResourceId`, `_SubscriptionId`, `_ItemId`, `_IsBillable`, `_BilledSize` und `Type`. Alle anderen Eigenschaften, die in Log Analytics gespeichert werden, sind in der Berechnung der Ereignisgröße enthalten. Für einige Datentypen fallen keinerlei Gebühren für die Datenerfassung an, wie z. B. AzureActivity, Heartbeat and Usage. Um zu ermitteln, ob ein Ereignis von der Abrechnung für die Datenerfassung ausgeschlossen wurde, können Sie die `_IsBillable`-Eigenschaft verwenden, wie es [unten](#data-volume-for-specific-events) gezeigt ist. Die Nutzung wird in GB (1.0E9 Bytes) angegeben. 

Beachten Sie außerdem, dass für einige Lösungen eigene Preismodelle gelten. Beispiele wären etwa [Azure Defender (Security Center)](https://azure.microsoft.com/pricing/details/azure-defender/), [Azure Sentinel](https://azure.microsoft.com/pricing/details/azure-sentinel/) und [Konfigurationsverwaltung](https://azure.microsoft.com/pricing/details/automation/). 

### <a name="log-analytics-dedicated-clusters"></a>Dedizierte Log Analytics-Cluster

Dedizierte Log Analytics-Cluster sind Sammlungen von Arbeitsbereichen in einem einzelnen verwalteten Azure Data Explorer-Cluster, die erweiterte Szenarien unterstützen, beispielsweise [vom Kunden verwaltete Schlüssel](customer-managed-keys.md).  Dedizierte Log Analytics-Cluster verwenden ein Preismodell für die Kapazitätsreservierung, das für mindestens 1000 GB/Tag konfiguriert sein muss. Diese Kapazitätsebene weist im Vergleich zu nutzungsbasierten Preisen einen Rabatt von 25 % auf. Jeder über die Reservierung hinausgehende Verbrauch wird entsprechend der nutzungsbasierten Bezahlung berechnet. Die Clusterkapazitätsreservierung hat einen Verpflichtungszeitraum von 31 Tagen, nachdem die Reservierungsebene erhöht wurde. Während des Verpflichtungszeitraums kann die Kapazitätsreservierungsebene nicht herabgesetzt, aber jederzeit erhöht werden. Wenn Arbeitsbereiche einem Cluster zugeordnet sind, erfolgt die Abrechnung der Datenerfassung für diese Arbeitsbereiche auf Clusterebene anhand der konfigurierten Kapazitätsreservierungsebene. Erfahren Sie mehr über das [Erstellen eines Log Analytics-Clusters](customer-managed-keys.md#create-cluster) und das [Zuordnen von Arbeitsbereichen zu diesem Cluster](customer-managed-keys.md#link-workspace-to-cluster). Preisinformationen zur Kapazitätsreservierung finden Sie auf der Seite [Azure Monitor – Preise]( https://azure.microsoft.com/pricing/details/monitor/).  

Die Reservierungsebene für die Clusterkapazität wird programmgesteuert mit Azure Resource Manager mithilfe des `Capacity`-Parameters unter `Sku` konfiguriert. `Capacity` wird in Einheiten von GB und in Schritten von 100 GB pro Tag in Werten von 1000 GB/Tag oder mehr angegeben. Dies wird ausführlich unter [Kundenseitig verwaltete Schlüssel in Azure Monitor](customer-managed-keys.md#create-cluster) behandelt. Wenn Ihr Cluster eine Reservierung über 2000 GB/Tag erfordert, kontaktieren Sie uns unter [LAIngestionRate@microsoft.com](mailto:LAIngestionRate@microsoft.com).

Für die Abrechnung des Verbrauchs in einem Cluster stehen zwei Modi zur Verfügung. Diese können beim [Konfigurieren Ihres Clusters](customer-managed-keys.md#customer-managed-key-operations) mithilfe des Parameters `billingType` angegeben werden. Die beiden Modi sind: 

1. **Cluster** (Standardeinstellung): In diesem Fall werden erfasste Daten auf der Clusterebene abgerechnet. Die erfassten Datenmengen aus den einzelnen Arbeitsbereichen, die einem Cluster zugeordnet sind, werden aggregiert, um die tägliche Abrechnung für den Cluster zu berechnen. Beachten Sie, dass die Zuordnungen pro Knoten von [Azure Defender (Security Center)](../../security-center/index.yml) vor dieser Aggregation von Daten in allen Arbeitsbereichen im Cluster auf Arbeitsbereichsebene angewendet werden. 

2. **Arbeitsbereiche**: Die Kapazitätsreservierungskosten für Ihren Cluster werden proportional den Arbeitsbereichen im Cluster zugeordnet (nach Berücksichtigung der knotenspezifischen Zuordnungen von [Azure Defender (Security Center)](../../security-center/index.yml) für den jeweiligen Arbeitsbereich). Wenn das gesamte Datenvolumen, das an einem Tag in einem Arbeitsbereich erfasst wird, unter der Kapazitätsreservierung liegt, werden für die einzelnen Arbeitsbereiche jeweils die erfassten Daten mit dem effektiven GB-basierten Kapazitätsreservierungssatz abgerechnet. Hierzu wird ein Teil der Kapazitätsreservierung abgerechnet, und der ungenutzte Teil der Kapazitätsreservierung wird für die Clusterressource in Rechnung gestellt. Wenn das gesamte Datenvolumen, das an einem Tag in einem Arbeitsbereich erfasst wird, über der Kapazitätsreservierung liegt, wird für die einzelnen Arbeitsbereiche jeweils ein Teil der Kapazitätsreservierung (basierend auf dem Anteil der an diesem Tag erfassten Daten) abgerechnet, und für jeden Arbeitsbereich wird jeweils ein Teil der erfassten Daten in Rechnung gestellt, die über die Kapazitätsreservierung hinausgehen. Wenn das gesamte Datenvolumen, das an einem Tag in einem Arbeitsbereich erfasst wird, über der Kapazitätsreservierung liegt, wird nichts für die Clusterressource abgerechnet.

In Clusterabrechnungsoptionen wird die Datenaufbewahrung pro Arbeitsbereich abgerechnet. Beachten Sie, dass die Clusterabrechnung mit der Clustererstellung beginnt, unabhängig davon, ob dem Cluster Arbeitsbereiche zugeordnet wurden. Beachten Sie außerdem, dass Arbeitsbereiche, die einem Cluster zugeordnet sind, nicht mehr über einen Tarif verfügen.

## <a name="estimating-the-costs-to-manage-your-environment"></a>Schätzen der Kosten für die Verwaltung Ihrer Umgebung 

Wenn Sie noch keine Azure Monitor-Protokolle verwenden, können Sie mit dem [Azure Monitor-Preisrechner](https://azure.microsoft.com/pricing/calculator/?service=monitor) die Kosten für die Nutzung von Log Analytics schätzen. Geben Sie „Azure Monitor“ im Suchfeld ein, und klicken Sie auf die ausgegebene Azure Monitor-Kachel. Scrollen Sie auf der Seite nach unten bis zu „Azure Monitor“, und wählen Sie in der Dropdownliste „Typ“ den Eintrag „Log Analytics“ aus.  Hier können Sie die Anzahl der VMs sowie das Datenvolumen (in GB) eingeben, dessen Erfassung durch die einzelnen VMs erwartet wird. Von einer typischen Azure-VM werden in der Regel 1 bis 3 GB Daten pro Monat erfasst. Wenn Sie bereits Azure Monitor-Protokolle auswerten, können Sie die Datenstatistiken aus Ihrer eigenen Umgebung verwenden. Unten wird erläutert, wie die [Anzahl der überwachten VMs](#understanding-nodes-sending-data) und das [erfasste Datenvolumen im Arbeitsbereich](#understanding-ingested-data-volume) bestimmt werden. 

## <a name="understand-your-usage-and-estimate-costs"></a>Verstehen Ihrer Nutzung und Schätzen von Kosten

Wenn Sie jetzt Azure Monitor-Protokolle verwenden, können Sie auf der Grundlage aktueller Nutzungsmuster problemlos die zu erwartenden Kosten ermitteln. Verwenden Sie **Analysieren der Datennutzung in Log Analytics**, um die Datennutzung zu überprüfen und zu analysieren. Hier wird angezeigt, wie viele Daten von jeder Lösung gesammelt werden, wie viele Daten aufbewahrt werden, und es wird eine Kostenschätzung angezeigt. Diese basiert auf der Menge an erfassten Daten und berücksichtigt eine eventuelle zusätzliche Aufbewahrung über die enthaltenen Menge hinaus.

:::image type="content" source="media/manage-cost-storage/usage-estimated-cost-dashboard-01.png" alt-text="Nutzung und geschätzte Kosten":::

Um Ihre Dateien ausführlicher zu untersuchen, klicken Sie auf der Seite **Nutzung und geschätzte Kosten** auf das Symbol oben rechts neben den Diagrammen. Sie können mit dieser Abfrage weitere Details zu Ihrer Nutzung abrufen.  

:::image type="content" source="media/manage-cost-storage/logs.png" alt-text="Ansicht „Protokolle“":::

Auf der Seite **Nutzung und geschätzte Kosten** können Sie Ihr Datenvolumen für den Monat überprüfen. Dieses beinhaltet alle abrechenbaren Daten, die in Ihrem Log Analytics-Arbeitsbereich empfangen und aufbewahrt wurden.  
 
Die Gebühren für Log Analytics fließen in Ihre Azure-Rechnung ein. Die Details Ihrer Azure-Rechnung finden Sie im Bereich „Abrechnung“ des Azure-Portals oder im [Azure-Abrechnungsportal](https://account.windowsazure.com/Subscriptions).  

## <a name="viewing-log-analytics-usage-on-your-azure-bill"></a>Anzeigen des Log Analytics-Verbrauchs auf Ihrer Azure-Rechnung 

Azure bietet im Hub [Azure Cost Management und Abrechnung](../../cost-management-billing/costs/quick-acm-cost-analysis.md?toc=%2fazure%2fbilling%2fTOC.json) eine Vielzahl nützlicher Funktionen. Mithilfe der Funktion „Kostenanalyse“ können Sie beispielsweise Ihre Ausgaben für Azure-Ressourcen überprüfen. Fügen Sie zunächst einen Filter nach „Ressourcentyp“ (zu „microsoft.operationalinsights/workspace“ für Log Analytics und „microsoft.operationalinsights/cluster“ für Log Analytics-Cluster) hinzu, damit Sie Ihre Log Analytics-Ausgaben verfolgen können. Wählen Sie anschließend unter „Gruppieren nach“ die Option „Kategorie der Verbrauchseinheit“ oder „Verbrauchseinheit“ aus.  Beachten Sie, dass andere Dienste wie Azure Defender (Security Center) und Azure Sentinel die jeweilige Nutzung auch für Log Analytics-Arbeitsbereichsressourcen berechnen. Um die Zuordnung zu Dienstnamen zu sehen, können Sie die Tabellenansicht anstelle eines Diagramms auswählen. 

Ein umfassenderes Verständnis Ihres Verbrauchs erlangen Sie, indem Sie [Ihre Verbrauchsdaten aus dem Azure-Portal herunterladen](../../cost-management-billing/manage/download-azure-invoice-daily-usage-date.md#download-usage-in-azure-portal). In der heruntergeladenen Tabelle wird der Verbrauch pro Azure-Ressource (z. B. Log Analytics-Arbeitsbereich) pro Tag aufgeführt. In dieser Excel-Tabelle können Sie den Verbrauch für Ihre Log Analytics-Arbeitsbereiche ermitteln, indem Sie zuerst nach der Spalte „Kategorie der Verbrauchseinheit“ filtern, um „Log Analytics“, „Insight & Analytics“ (von einigen der Legacy-Tarife verwendet) und „Azure Monitor“ (von Tarifen für Kapazitätsreservierung verwendet) anzuzeigen und dann in der Spalte „Instanz-ID“ einen Filter namens „contains workspace“ (enthält Arbeitsbereich) oder „contains cluster“ (enthält Cluster) hinzufügen (Letzterer dient zum Einbeziehen der Log Analytics-Clusternutzung). Der Verbrauch wird in der Spalte „Verbrauchte Menge“ aufgeführt, und die Einheit für jeden Eintrag wird in der Spalte „Maßeinheit“ angegeben.  Es sind weitere Einzelheiten verfügbar, anhand derer Sie [Ihre Microsoft Azure-Rechnung verstehen](../../cost-management-billing/understand/review-individual-bill.md). 

## <a name="changing-pricing-tier"></a>Ändern des Tarifs

Gehen Sie folgendermaßen vor, um den Log Analytics-Tarif Ihres Arbeitsbereichs zu ändern: 

1. Öffnen Sie im Azure-Portal aus Ihrem Arbeitsbereich **Nutzung und geschätzte Kosten**. Hier wird eine Liste aller Tarife angezeigt, die für diesen Arbeitsbereich verfügbar sind.

2. Überprüfen Sie die geschätzten Kosten für die einzelnen Tarife. Diese Schätzungen basierend auf der Nutzung der letzten 31 Tage, d. h. bei dieser Kostenschätzung wird davon ausgegangen, dass die letzten 31 Tage repräsentativ für Ihre typische Nutzung sind. Im Beispiel unten können Sie sehen, dass die Kosten für diesen Arbeitsbereich basierend auf den Datenmustern der letzten 31 Tage im Modell der nutzungsbasierten Zahlung (Nr. 1) geringer wären als im Tarif mit einer Kapazitätsreservierung von 100 GB/Tag (Nr. 2).  

:::image type="content" source="media/manage-cost-storage/pricing-tier-estimated-costs.png" alt-text="Tarife":::
    
3. Klicken Sie nach Überprüfung der geschätzten Kosten basierend auf der Nutzung der letzten 31 Tage auf **Auswählen**, wenn Sie sich für einen Wechsel des Tarifs entscheiden.  

Es ist auch möglich, den [Tarif über Azure Resource Manager festzulegen](./resource-manager-workspace.md). Verwenden Sie hierzu den Parameter `sku` (`pricingTier` in der Azure Resource Manager-Vorlage). 

## <a name="legacy-pricing-tiers"></a>Legacytarife

Abonnements, die vor dem 2. April 2018 einen Log Analytics-Arbeitsbereich oder eine Application Insights-Ressource enthielten oder mit einem Enterprise Agreement verknüpft sind, das vor dem 1. Februar 2019 abgeschlossen wurde, haben weiterhin Zugriff auf die Legacytarife: **Free**, **Eigenständig (Pro GB)** und **Pro Knoten (OMS)** .  Für Arbeitsbereiche im Tarif „Free“ ist die tägliche Datenerfassung auf 500 MB beschränkt (mit Ausnahme von Sicherheitsdaten, die von [Azure Defender (Security Center)](../../security-center/index.yml) gesammelt werden). Darüber hinaus ist die Datenaufbewahrung auf sieben Tage beschränkt. Der Tarif „Free“ dient nur zu Evaluierungszwecken. Für Arbeitsbereiche im Tarif „Eigenständig“ oder „Pro Knoten“ gilt eine vom Benutzer festlegbare Datenaufbewahrungsfrist von 30 bis 730 Tagen.

Die Nutzung des Tarifs „Eigenständig“ wird anhand des erfassten Datenvolumens abgerechnet. Dieses wird im **Log Analytics**-Dienst gemeldet, und die Verbrauchseinheit heißt „Analysierte Daten“. 

Der Tarif „Pro Knoten“ wird pro überwachter VM (Knoten) mit einer Granularität von einer Stunde berechnet. Für jeden überwachten Knoten werden dem Arbeitsbereich 500 MB Daten pro Tag zugeteilt, die nicht berechnet werden. Diese Zuteilung wird auf Stundenbasis berechnet und täglich auf Arbeitsbereichsebene aggregiert. Daten, die über die aggregierte tägliche Datenzuteilung hinaus erfasst werden, werden pro GB als Datenüberschreitung berechnet. Beachten Sie, dass der Dienst auf Ihrer Rechnung als **Insight und Analytics** für die Log Analytics-Nutzung aufgeführt ist, wenn für den Arbeitsbereich der Tarif „Pro Knoten“ festgelegt ist. Die Nutzung wird mit drei Verbrauchseinheiten gemeldet:

1. Knoten: der Verbrauch für die Anzahl der überwachten Knoten (VMs) in Einheiten von Knoten × Monate.
2. Datenüberschreitung pro Knoten: Anzahl von GB an Daten, die über die aggregierte Datenzuordnung hinaus erfasst wurden.
3. Pro Knoten enthaltene Daten: Menge der erfassten Daten, die durch die aggregierte Datenzuordnung abgedeckt wurden. Diese Verbrauchseinheit wird auch verwendet, wenn der Arbeitsbereich in allen Tarifen verfügbar ist, um die Menge der von Azure Defender (Security Center) abgedeckten Daten anzuzeigen.

> [!TIP]
> Falls für Ihren Arbeitsbereich Zugriff auf den Tarif **Pro Knoten** besteht und Sie sich die Frage stellen, ob die Kosten beim Tarif mit nutzungsbasierter Zahlung niedriger wären, können Sie mit der [unten angegebenen Abfrage](#evaluating-the-legacy-per-node-pricing-tier) leicht eine Empfehlung abrufen. 

Vor April 2016 erstellte Arbeitsbereiche haben ebenfalls Zugriff auf die ursprünglichen Tarife **Standard** und **Premium**, für die eine feste Datenaufbewahrung von 30 bzw. 365 Tagen gilt. Neue Arbeitsbereiche können nicht im Tarif **Standard** oder **Premium** erstellt werden, und wenn ein Arbeitsbereich aus diesen Tarifen verschoben wird, kann er nicht zurück verschoben werden. Die Verbrauchseinheiten für die Datenerfassung für diese veralteten Tarife werden mit „Analysierte Daten“ bezeichnet.

Es gibt auch einige Besonderheiten bei der Verwendung von Log Analytics-Legacytarifen und der Berechnung der Nutzung für [Azure Defender (Security Center)](../../security-center/index.yml). 

1. Wenn für den Arbeitsbereich der Legacytarif „Standard“ oder „Premium“ gilt, wird Azure Defender nur für die Log Analytics-Datenerfassung und nicht pro Knoten berechnet.
2. Wenn für den Arbeitsbereich der Legacytarif „Pro Knoten“ gilt, wird Azure Defender gemäß dem aktuellen [knotenbasierten Preismodell für Azure Defender](https://azure.microsoft.com/pricing/details/security-center/) berechnet. 
3. In anderen Tarifen (einschließlich Kapazitätsreservierungen) wird Azure Defender bei Aktivierung vor dem 19. Juni 2017 nur für die Log Analytics-Datenerfassung berechnet. Andernfalls wird Azure Defender gemäß dem aktuellen knotenbasierten Preismodell für Azure Defender berechnet.

Weitere Details zu den Einschränkungen der Tarife finden Sie unter [Azure-Abonnement- und -Dienstgrenzen, Kontingente und Einschränkungen](../../azure-resource-manager/management/azure-subscription-service-limits.md#log-analytics-workspaces).

Keiner der Legacytarife bietet regionsbezogene Preise.  

> [!NOTE]
> Wenn Sie die Berechtigungen nutzen möchten, die Sie durch den Kauf der OMS E1-Suite, OMS E2-Suite oder des OMS-Add-Ons für System Center erwerben, wählen Sie den Tarif *Pro Knoten* für Log Analytics aus.

## <a name="log-analytics-and-azure-defender-security-center"></a>Log Analytics und Azure Defender (Security Center)

Die Abrechnung für [Azure Defender (Security Center)](../../security-center/index.yml) ist eng mit der Log Analytics-Abrechnung verbunden. Azure Defender bietet eine Zuordnung von 500 MB/Knoten/Tag für die folgende Teilmenge von [Sicherheitsdatentypen](/azure/azure-monitor/reference/tables/tables-category#security) (WindowsEvent, SecurityAlert, SecurityBaseline, SecurityBaselineSummary, SecurityDetection, SecurityEvent, WindowsFirewall, MaliciousIPCommunication, LinuxAuditLog, SysmonEvent, ProtectionStatus) und die Datentypen „Update“ und „UpdateSummary“, wenn die Lösung für die Updateverwaltung nicht im Arbeitsbereich ausgeführt wird oder die Zielgruppenadressierung aktiviert ist. Weitere Informationen finden Sie [hier](../../security-center/security-center-pricing.md#what-data-types-are-included-in-the-500-mb-data-daily-allowance). Wenn für den Arbeitsbereich der Legacytarif „Pro Knoten“ gilt, werden die Azure Defender- und Log Analytics-Zuordnungen kombiniert und gemeinsam auf alle abrechenbaren erfassten Daten angewendet.  

## <a name="change-the-data-retention-period"></a>Ändern des Datenaufbewahrungszeitraums

Die folgenden Schritte zeigen, wie Sie die Aufbewahrungsdauer von Protokolldaten in Ihrem Arbeitsbereich konfigurieren. Die Datenaufbewahrung auf Arbeitsbereichsebene kann für alle Arbeitsbereiche von 30 bis 730 Tagen (2 Jahre) konfiguriert werden, es sei denn, sie verwenden den Legacytarif „Free“. Die Aufbewahrung einzelner Datentypen kann bis zu 4 Tage kurz festgelegt werden. Informieren Sie sich über die [Preise](https://azure.microsoft.com/pricing/details/monitor/) für eine längere Datenaufbewahrung.  Um Daten länger als 730 Tage aufzubewahren, können Sie den [Export von Daten aus dem Log Analytics-Arbeitsbereich](logs-data-export.md) in Erwägung ziehen.

### <a name="workspace-level-default-retention"></a>Standardaufbewahrung auf Arbeitsbereichsebene

So legen Sie die Standardaufbewahrungsdauer für Ihren Arbeitsbereich fest: 
 
1. Wählen Sie im linken Bereich des Azure-Portals über Ihren Arbeitsbereich **Nutzung und geschätzte Kosten** aus.
2. Klicken Sie im oberen Bereich der Seite **Nutzungs- und geschätzte Kosten** auf **Datenaufbewahrung**.
3. Passen Sie mithilfe des Schiebereglers die Anzahl von Tagen an, und klicken Sie anschließend auf **OK**.  Wenn Sie sich im Tarif *Free* befinden, können Sie den Datenaufbewahrungszeitraum nicht ändern. Sie müssen in einen kostenpflichtigen Tarif wechseln, um diese Einstellung zu steuern.

:::image type="content" source="media/manage-cost-storage/manage-cost-change-retention-01.png" alt-text="Ändern des Datenaufbewahrungszeitraums für den Arbeitsbereich":::

Wenn die Aufbewahrungsdauer gesenkt wird, besteht ein Toleranzzeitraum von mehreren Tagen, bevor Daten, die älter als die Datenaufbewahrungseinstellung sind, entfernt werden. 

Auf der Seite **Datenaufbewahrung** können Sie die Einstellung für die Datenaufbewahrung auf 30, 31, 60, 90, 120, 180, 270, 365, 550 und 730 Tage festlegen. Wenn eine andere Einstellung erforderlich ist, kann diese mit [Azure Resource Manager](./resource-manager-workspace.md) und dem Parameter `retentionInDays` konfiguriert werden. Wenn Sie die Datenaufbewahrung auf 30 Tage festlegen, können Sie mit dem Parameter `immediatePurgeDataOn30Days` eine sofortige Bereinigung älterer Daten auslösen (und damit den Toleranzzeitraum von mehreren Tagen außer Kraft setzen). Dies kann in Szenarien nützlich sein, in denen für die Compliance das direkte Entfernen von Daten erforderlich ist. Diese sofortige Bereinigung wird ausschließlich über Azure Resource Manager verfügbar gemacht. 

Arbeitsbereiche mit einer Datenaufbewahrung von 30 Tagen behalten Daten möglicherweise tatsächlich für 31 Tage bei. Wenn es zwingend erforderlich ist, dass Daten nur 30 Tage lang aufbewahrt werden, legen Sie mit Azure Resource Manager und dem Parameter `immediatePurgeDataOn30Days` die Datenaufbewahrung auf 30 Tage fest.  

Zwei Datentypen – `Usage` und `AzureActivity` – werden standardmäßig mindestens 90 Tage lang aufbewahrt, und diese 90-tägige Aufbewahrung wird nicht in Rechnung gestellt. Wenn die Aufbewahrung des Arbeitsbereichs auf über 90 Tage verlängert wird, wird auch die Aufbewahrung dieser Datentypen verlängert.  Für diese Datentypen werden auch keine Gebühren für die Datenerfassung erhoben. 

Datentypen von arbeitsbereichsbasierten Application Insights-Ressourcen (`AppAvailabilityResults`, `AppBrowserTimings`, `AppDependencies`, `AppExceptions`, `AppEvents`, `AppMetrics`, `AppPageViews`, `AppPerformanceCounters`, `AppRequests`, `AppSystemEvents` und `AppTraces`) werden auch standardmäßig 90 Tage lang aufbewahrt, und diese 90-tägige Aufbewahrung wird nicht in Rechnung gestellt. Die jeweilige Aufbewahrungsdauer kann mithilfe der Funktion für die Aufbewahrung nach Datentyp angepasst werden. 

Beachten Sie, dass die [Bereinigungs-API](/rest/api/loganalytics/workspacepurge/purge) von Log Analytics keine Auswirkung auf die Aufbewahrungsabrechnung hat und zur Verwendung in einer sehr begrenzten Zahl von Fällen bestimmt ist. Um Ihre Aufbewahrungsrechnung zu verringern, muss die Aufbewahrungsdauer entweder für den Arbeitsbereich oder für bestimmte Datentypen gesenkt werden. 

### <a name="retention-by-data-type"></a>Aufbewahrung nach Datentyp

Es ist auch möglich, unterschiedliche Aufbewahrungseinstellungen für einzelne Datentypen zwischen 4 und 730 Tagen anzugeben (mit Ausnahme von Arbeitsbereichen im Legacytarif „Free“), mit denen die Standardaufbewahrungseinstellungen auf Arbeitsbereichsebene außer Kraft gesetzt werden. Jeder Datentyp ist eine Unterressource des Arbeitsbereichs. Die SecurityEvent-Tabelle kann in [Azure Resource Manager](../../azure-resource-manager/management/overview.md) beispielsweise wie folgt adressiert werden:

```
/subscriptions/00000000-0000-0000-0000-00000000000/resourceGroups/MyResourceGroupName/providers/Microsoft.OperationalInsights/workspaces/MyWorkspaceName/Tables/SecurityEvent
```

Beachten Sie, dass beim Datentyp (Tabelle) die Groß-und Kleinschreibung beachtet wird.  Geben Sie Folgendes an, um die aktuellen datentypspezifischen Aufbewahrungseinstellungen für einen bestimmten Datentyp (in diesem Beispiel SecurityEvent) abzurufen:

```JSON
    GET /subscriptions/00000000-0000-0000-0000-00000000000/resourceGroups/MyResourceGroupName/providers/Microsoft.OperationalInsights/workspaces/MyWorkspaceName/Tables/SecurityEvent?api-version=2017-04-26-preview
```

> [!NOTE]
> Die Aufbewahrungseinstellung wird nur für einen Datentyp zurückgegeben, wenn sie explizit für diesen festgelegt wurde.  Bei Datentypen, für die keine Aufbewahrungseinstellung explizit festgelegt wurde (und die somit die Aufbewahrungseinstellung des Arbeitsbereichs erben), wird durch diesen Befehl nichts zurückgegeben. 

Um die aktuellen datentypspezifischen Aufbewahrungseinstellungen für alle Datentypen in Ihrem Arbeitsbereich abzurufen, für die eine datentypspezifische Aufbewahrung festgelegt wurde, lassen Sie einfach den spezifischen Datentyp aus. Beispiel:

```JSON
    GET /subscriptions/00000000-0000-0000-0000-00000000000/resourceGroups/MyResourceGroupName/providers/Microsoft.OperationalInsights/workspaces/MyWorkspaceName/Tables?api-version=2017-04-26-preview
```

Wenn Sie die Aufbewahrung für einen bestimmten Datentyp (in diesem Beispiel SecurityEvent) auf 730 Tage festlegen möchten, geben Sie Folgendes an:

```JSON
    PUT /subscriptions/00000000-0000-0000-0000-00000000000/resourceGroups/MyResourceGroupName/providers/Microsoft.OperationalInsights/workspaces/MyWorkspaceName/Tables/SecurityEvent?api-version=2017-04-26-preview
    {
        "properties": 
        {
            "retentionInDays": 730
        }
    }
```

Die gültigen Werte für `retentionInDays` sind 30 bis 730.

Für die Datentypen `Usage` und `AzureActivity` kann keine benutzerdefinierte Aufbewahrung festgelegt werden. Für sie gilt die maximale Standardaufbewahrungsdauer des Arbeitsbereichs bzw. eine Dauer von 90 Tagen. 

Ein großartiges Tool für das Herstellen einer direkten Verbindung mit Azure Resource Manager zum Festlegen der Aufbewahrung nach Datentyp ist das OSS-Tool [ARMclient](https://github.com/projectkudu/ARMClient).  Erfahren Sie mehr über ARMclient in den Artikeln von [David Ebbo](http://blog.davidebbo.com/2015/01/azure-resource-manager-client.html) und [Daniel Bowbyes](https://blog.bowbyes.co.nz/2016/11/02/using-armclient-to-directly-access-azure-arm-rest-apis-and-list-arm-policy-details/).  Dies ist ein Beispiel für die Verwendung von ARMClient. Darin wird eine Aufbewahrungsdauer von 730 Tagen für SecurityEvent-Daten festgelegt:

```
armclient PUT /subscriptions/00000000-0000-0000-0000-00000000000/resourceGroups/MyResourceGroupName/providers/Microsoft.OperationalInsights/workspaces/MyWorkspaceName/Tables/SecurityEvent?api-version=2017-04-26-preview "{properties: {retentionInDays: 730}}"
```

> [!TIP]
> Durch das Festlegen der Aufbewahrung für einzelne Datentypen können Sie Ihre Kosten für die Datenaufbewahrung senken.  Bei ab Oktober 2019 (der Monat, in dem diese Funktion eingeführt wurde) gesammelten Daten kann die Reduzierung der Aufbewahrungszeit einiger Datentypen die Kosten für die Aufbewahrung mit der Zeit senken.  Wenn die Daten zuvor gesammelt wurden, wird das Festlegen einer kürzeren Aufbewahrungszeit für einen einzelnen Typ Ihre Kosten für die Aufbewahrung nicht beeinträchtigen.  

## <a name="manage-your-maximum-daily-data-volume"></a>Verwalten Ihres maximalen täglichen Datenvolumens

Sie können eine tägliche Obergrenze konfigurieren und die tägliche Erfassung für Ihren Arbeitsbereich einschränken. Dabei ist jedoch Vorsicht geboten, da dieses Limit möglichst nicht erreicht werden sollte.  Andernfalls verlieren Sie die Daten des restlichen Tages. Dies kann sich auf Azure-Dienste und -Lösungen auswirken, deren Funktionalität unter Umständen von der Verfügbarkeit aktueller Daten im Arbeitsbereich abhängt.  Infolgedessen kann auch die Integrität von Ressourcen, die IT-Diensten zugrunde liegen, nicht mehr zuverlässig überwacht werden, und es können keine Warnungen empfangen werden.  Die tägliche Obergrenze ist dazu gedacht, einen **unerwarteten Anstieg** des Datenvolumens aus Ihren verwalteten Ressourcen zu verhindern und den Grenzwert einzuhalten – oder ungeplante Gebühren für Ihren Arbeitsbereich zu vermeiden. Es ist nicht vorgesehen, eine tägliche Obergrenze festzulegen, sodass diese jeden Tag in einem Arbeitsbereich erreicht wird.

Die tägliche Obergrenze wird für jeden Arbeitsbereich zu einer anderen Stunde des Tages angewendet. Die Zurücksetzungsstunde wird auf der Seite **Daily Cap** (Tägliche Obergrenze) angezeigt (siehe unten). Diese Zurücksetzungsstunde kann nicht konfiguriert werden. 

Kurz nach Erreichen des Tageslimits werden für den Rest des Tages keine kostenpflichtigen Datentypen mehr gesammelt. Die Standardlatenz beim Anwenden des Tageslimits führt dazu, dass das Limit nicht genau beim festgelegten Tageslimit angewandt wird. Im oberen Seitenbereich erscheint ein Warnbanner für den ausgewählten Log Analytics-Arbeitsbereich, und an die Tabelle *Operation* wird unter der Kategorie **LogManagement** ein Vorgangsereignis gesendet. Die Datensammlung wird nach der unter *Daily limit will be set at* (Tageslimit wird festgelegt um) definierten Zurücksetzungszeit fortgesetzt. Es empfiehlt sich, eine Warnungsregel auf der Grundlage dieses Vorgangsereignisses zu definieren und so zu konfigurieren, dass bei Erreichen des Tageslimits für Daten eine Benachrichtigung erfolgt (siehe [unten](#alert-when-daily-cap-reached)). 

> [!NOTE]
> Die tägliche Obergrenze kann die Datensammlung nicht so genau wie die angegebene Obergrenze beenden, sodass einige überschüssige Daten zu erwarten sind. Dies gilt insbesondere dann, wenn der Arbeitsbereich große Datenmengen empfängt. Eine Abfrage, die bei der Untersuchung des Verhaltens der täglichen Obergrenze hilfreich ist, finden Sie [weiter unten](#view-the-effect-of-the-daily-cap). 

> [!WARNING]
> Die Sammlung folgender Datentypen wird durch die tägliche Obergrenze nicht beendet: WindowsEvent, SecurityAlert, SecurityBaseline, SecurityBaselineSummary, SecurityDetection, SecurityEvent, WindowsFirewall, MaliciousIPCommunication, LinuxAuditLog, SysmonEvent, ProtectionStatus, Update, UpdateSummary. Dies gilt allerdings nicht für Arbeitsbereiche, in denen Azure Defender (Security Center) vor dem 19. Juni 2017 installiert wurde. 

### <a name="identify-what-daily-data-limit-to-define"></a>Identifizieren des zu definierenden Tageslimits für Daten

Informieren Sie sich unter [Analysieren der Datennutzung in Log Analytics](../usage-estimated-costs.md) über den Datenerfassungstrend sowie über die zu definierende tägliche Volumenobergrenze. Wählen Sie die Obergrenze mit Bedacht, da Sie Ihre Ressourcen nach dem Erreichen des Limits nicht mehr überwachen können. 

### <a name="set-the-daily-cap"></a>Festlegen der täglichen Obergrenze

In den folgenden Schritten erfahren Sie, wie Sie ein Tageslimit für die vom Log Analytics-Arbeitsbereich erfasste Datenmenge konfigurieren.  

1. Klicken Sie links in Ihrem Arbeitsbereich auf **Nutzung und geschätzte Kosten**.
2. Klicken Sie im oberen Bereich der Seite **Nutzung und geschätzte Kosten** für den ausgewählten Arbeitsbereich auf **Datenobergrenze**. 
3. Die tägliche Obergrenze ist standardmäßig **AUS**. Klicken Sie auf **EIN**, um sie zu aktivieren, und legen Sie das Limit für das Datenvolumen in GB/Tag fest.

:::image type="content" source="media/manage-cost-storage/set-daily-volume-cap-01.png" alt-text="Konfigurieren des Log Analytics-Datenlimits":::
    
Die tägliche Obergrenze kann über ARM konfiguriert werden, indem Sie den Parameter `dailyQuotaGb` unter `WorkspaceCapping` wie unter [Arbeitsbereiche – Erstellen oder Aktualisieren](/rest/api/loganalytics/workspaces/createorupdate#workspacecapping) beschrieben festlegen. 

Änderungen an der täglichen Obergrenze können mithilfe der folgenden Abfrage nachverfolgt werden:

```kusto
_LogOperation | where Operation == "Workspace Configuration" | where Detail contains "Daily quota"
```

Weitere Informationen zur Funktion „_LogOperation“ finden Sie [hier](./monitor-workspace.md). 

### <a name="view-the-effect-of-the-daily-cap"></a>Anzeigen der Auswirkung der täglichen Obergrenze

Um die Auswirkung der täglichen Obergrenze zu sehen, ist es wichtig, die nicht in der täglichen Obergrenze enthaltenen Sicherheitsdatentypen sowie die Zurücksetzungsstunde für Ihren Arbeitsbereich zu berücksichtigen. Die Zurücksetzungsstunde für die tägliche Obergrenze wird auf der Seite **Tägliche Obergrenze** angezeigt.  Mithilfe der folgenden Abfrage können Sie die Datenmengen, die der täglichen Obergrenze unterliegen, zwischen den Zurücksetzungen nachverfolgen. In diesem Beispiel ist die Zurücksetzungsstunde für den Arbeitsbereich 14:00.  Sie müssen dies für Ihren Arbeitsbereich aktualisieren.

```kusto
let DailyCapResetHour=14;
Usage
| where Type !in ("SecurityAlert", "SecurityBaseline", "SecurityBaselineSummary", "SecurityDetection", "SecurityEvent", "WindowsFirewall", "MaliciousIPCommunication", "LinuxAuditLog", "SysmonEvent", "ProtectionStatus", "WindowsEvent")
| extend TimeGenerated=datetime_add("hour",-1*DailyCapResetHour,TimeGenerated)
| where TimeGenerated > startofday(ago(31d))
| where IsBillable
| summarize IngestedGbBetweenDailyCapResets=sum(Quantity)/1000. by day=bin(TimeGenerated, 1d) | render areachart  
```

(Beim Datentyp „Nutzung“ sind die Einheiten von `Quantity` in MB.)

### <a name="alert-when-daily-cap-reached"></a>Warnung bei Erreichen der täglichen Obergrenze

Im Azure-Portal wird bei Erreichen des Schwellenwerts für das Datenlimit zwar ein visueller Hinweis angezeigt, dieses Verhalten steht jedoch möglicherweise nicht im Einklang mit der gewünschten Behandlung von Betriebsproblemen, die eine umgehende Reaktion erfordern.  Wenn Sie eine Warnbenachrichtigung erhalten möchten, können Sie in Azure Monitor eine neue Warnregel erstellen.  Weitere Informationen finden Sie unter [Erstellen, Anzeigen und Verwalten von Warnungen](../alerts/alerts-metric.md).

Im Anschluss finden Sie die empfohlenen Einstellungen für die Warnung zur Abfrage der Tabelle `Operation` mithilfe der Funktion `_LogOperation`. (Weitere Informationen finden Sie [hier](./monitor-workspace.md).) 

- Ziel: Wählen Sie Ihre Log Analytics-Ressource aus.
- Kriterien: 
   - Signalname: Benutzerdefinierte Protokollsuche
   - Suchabfrage: `_LogOperation | where Operation == "Data collection Status" | where Detail contains "OverQuota"`
   - Basierend auf: Anzahl der Ergebnisse
   - Bedingung: Größer als
   - Schwellenwert: 0
   - Zeitraum: 5 (Minuten)
   - Häufigkeit: 5 (Minuten)
- Name der Warnungsregel: Daily data limit reached
- Schweregrad: Warnung (Schweregrad 1)

Nachdem die Warnung definiert wurde, wird bei Erreichen des Limits eine Warnung ausgelöst und die in der Aktionsgruppe definierte Reaktion ausgeführt. Dadurch kann Ihr Team per E-Mail und SMS benachrichtigt werden, und es können Aktionen mithilfe von Webhooks oder Automation-Runbooks oder mittels [Integration in eine externe ITSM-Lösung](../alerts/itsmc-definition.md#create-itsm-work-items-from-azure-alerts) automatisiert werden. 

## <a name="troubleshooting-why-usage-is-higher-than-expected"></a>Ermittlung per Problembehandlung, warum die Nutzung höher als erwartet ist

Eine höhere Nutzung wird durch eine bzw. beide der folgenden Bedingungen verursacht:
- Mehr Knoten als erwartet senden Daten an den Log Analytics-Arbeitsbereich
- Mehr Daten als erwartet werden an den Log Analytics-Arbeitsbereich gesendet (vielleicht aufgrund des Beginns der Verwendung einer neuen Lösung oder einer Konfigurationsänderung an einer bestehenden Lösung)

## <a name="understanding-nodes-sending-data"></a>Ermitteln der Knoten, die Daten senden

Um die Anzahl von Knoten zu ermitteln, die im letzten Monat täglich Heartbeats vom Agent gemeldet haben, verwenden Sie die folgende Abfrage:

```kusto
Heartbeat 
| where TimeGenerated > startofday(ago(31d))
| summarize nodes = dcount(Computer) by bin(TimeGenerated, 1d)    
| render timechart
```
Zum Abrufen der Anzahl von Knoten, die in den letzten 24 Stunden Daten gesendet haben, verwenden Sie die folgende Abfrage: 

```kusto
find where TimeGenerated > ago(24h) project Computer
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| where computerName != ""
| summarize nodes = dcount(computerName)
```

Eine Liste der Knoten, die Daten senden (und die Menge der von ihnen gesendeten Daten) kann mit der folgenden Abfrage abgerufen werden:

```kusto
find where TimeGenerated > ago(24h) project _BilledSize, Computer
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| where computerName != ""
| summarize TotalVolumeBytes=sum(_BilledSize) by computerName
```

### <a name="nodes-billed-by-the-legacy-per-node-pricing-tier"></a>Nach Legacytarif „Pro Knoten“ abgerechnete Knoten

Beim [Legacytarif „Pro Knoten“](#legacy-pricing-tiers) werden Knoten auf Stundenbasis abgerechnet, und es werden zudem Knoten nicht gezählt, die nur eine Reihe von Sicherheitsdatentypen senden. Die tägliche Anzahl von Knoten würde der folgenden Abfrage ähneln:

```kusto
find where TimeGenerated >= startofday(ago(7d)) and TimeGenerated < startofday(now()) project Computer, _IsBillable, Type, TimeGenerated
| where Type !in ("SecurityAlert", "SecurityBaseline", "SecurityBaselineSummary", "SecurityDetection", "SecurityEvent", "WindowsFirewall", "MaliciousIPCommunication", "LinuxAuditLog", "SysmonEvent", "ProtectionStatus", "WindowsEvent")
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| where computerName != ""
| where _IsBillable == true
| summarize billableNodesPerHour=dcount(computerName) by bin(TimeGenerated, 1h)
| summarize billableNodesPerDay = sum(billableNodesPerHour)/24., billableNodeMonthsPerDay = sum(billableNodesPerHour)/24./31.  by day=bin(TimeGenerated, 1d)
| sort by day asc
```

Die Anzahl der Einheiten auf der Rechnung ist in Einheiten von Knoten × Monate angegeben, die durch `billableNodeMonthsPerDay` in der Abfrage dargestellt ist. Wenn im Arbeitsbereich die Lösung für die Updateverwaltung installiert ist, fügen Sie die Datentypen Update und UpdateSummary der Liste in der WHERE-Klausel in der obigen Abfrage hinzu. Der tatsächliche Abrechnungsalgorithmus weist zusätzliche Komplexität auf, wenn eine Zielgruppenadressierung verwendet wird, die in der obigen Abfrage nicht dargestellt ist. 


> [!TIP]
> Verwenden Sie diese `find`-Abfragen mit Bedacht, da umfassende Scans verschiedener Datentypen [ressourcenintensiv](./query-optimization.md#query-performance-pane) sind. Wenn Sie keine Ergebnisse **pro Computer** benötigen, dann führen Sie Abfragen nach dem Datentyp „Usage“ aus (siehe unten).

## <a name="understanding-ingested-data-volume"></a>Grundlegendes zur erfassten Datenmenge

Auf der Seite **Nutzung und geschätzte Kosten** zeigt das Diagramm *Datenerfassung pro Lösung* die Gesamtmenge an gesendeten Daten sowie die von jeder Lösung gesendete Datenmenge an. Auf diese Weise können Sie Trends ermitteln, z.B. ob die Gesamtdatennutzung (oder die Nutzung durch eine bestimmte Lösung) ansteigt, konstant bleibt oder abnimmt. 

### <a name="data-volume-for-specific-events"></a>Datenmenge für bestimmte Ereignisse

Um die Größe der erfassten Daten für eine bestimmte Gruppe von Ereignissen zu überprüfen, können Sie die jeweilige Tabelle (in diesem Beispiel `Event`) abfragen und dann die Abfrage auf die relevanten Ereignisse (in diesem Beispiel Ereignis-ID 5145 oder 5156) beschränken:

```kusto
Event
| where TimeGenerated > startofday(ago(31d)) and TimeGenerated < startofday(now()) 
| where EventID == 5145 or EventID == 5156
| where _IsBillable == true
| summarize count(), Bytes=sum(_BilledSize) by EventID, bin(TimeGenerated, 1d)
``` 

Beachten Sie, dass durch die Klausel `where _IsBillable = true` Datentypen bestimmter Lösungen herausgefiltert werden, für die keine Erfassungsgebühren anfallen. Weitere Informationen zu `_IsBillable` finden Sie [hier](./log-standard-columns.md#_isbillable).

### <a name="data-volume-by-solution"></a>Datenvolumen nach Lösung

Die Abfrage, mit der die abrechenbare Datenmenge nach Lösung während des letzten Monats (ohne den letzten angebrochenen Tag) angezeigt wird, kann unter Verwendung des Datentyps [Usage](/azure/azure-monitor/reference/tables/usage) (Nutzung) wie folgt erstellt werden:

```kusto
Usage 
| where TimeGenerated > ago(32d)
| where StartTime >= startofday(ago(31d)) and EndTime < startofday(now())
| where IsBillable == true
| summarize BillableDataGB = sum(Quantity) / 1000. by bin(StartTime, 1d), Solution 
| render columnchart
```

Mit der Klausel mit `TimeGenerated` wird nur sichergestellt, dass die Abfrage im Azure-Portal über den Standardwert von 24 Stunden hinausgeht. Bei Verwendung des Datentyps „Usage“ stellen `StartTime` und `EndTime` die Zeitrahmen dar, für die Ergebnisse angezeigt werden. 

### <a name="data-volume-by-type"></a>Datenmenge nach Typ

Sie können die Daten genauer untersuchen, um Trends für die einzelnen Datentypen zu erkennen:

```kusto
Usage 
| where TimeGenerated > ago(32d)
| where StartTime >= startofday(ago(31d)) and EndTime < startofday(now())
| where IsBillable == true
| summarize BillableDataGB = sum(Quantity) / 1000. by bin(StartTime, 1d), DataType 
| render columnchart
```

Oder verwenden Sie Folgendes, um eine Tabelle nach Lösung und Typ für den letzten Monat anzuzeigen:

```kusto
Usage 
| where TimeGenerated > ago(32d)
| where StartTime >= startofday(ago(31d)) and EndTime < startofday(now())
| where IsBillable == true
| summarize BillableDataGB = sum(Quantity) / 1000 by Solution, DataType
| sort by Solution asc, DataType asc
```

### <a name="data-volume-by-computer"></a>Datenmenge nach Computer

Der Datentyp `Usage` enthält keine Informationen auf Computerebene. Um die **Größe** der pro Computer erfassten abrechenbaren Daten anzuzeigen, verwenden Sie die [Eigenschaft](./log-standard-columns.md#_billedsize) `_BilledSize`, die die Größe in Bytes angibt:

```kusto
find where TimeGenerated > ago(24h) project _BilledSize, _IsBillable, Computer, Type
| where _IsBillable == true and Type != "Usage"
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| summarize BillableDataBytes = sum(_BilledSize) by  computerName 
| sort by BillableDataBytes desc nulls last
```

Die `_IsBillable`-[Eigenschaft](./log-standard-columns.md#_isbillable) gibt an, ob für die erfassten Daten Gebühren anfallen. Der Typ „Usage“ (Nutzung) wird weggelassen, da es nur um die Analyse von Datentrends geht. 

Um die Anzahl von Ereignissen pro Computer anzuzeigen, für die **Gebühren** anfallen, verwenden Sie diese Abfrage: 

```kusto
find where TimeGenerated > ago(24h) project _IsBillable, Computer
| where _IsBillable == true and Type != "Usage"
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| summarize eventCount = count() by computerName  
| sort by eventCount desc nulls last
```

> [!TIP]
> Verwenden Sie diese `find`-Abfragen mit Bedacht, da umfassende Scans verschiedener Datentypen [ressourcenintensiv](./query-optimization.md#query-performance-pane) sind. Wenn Sie keine Ergebnisse **pro Computer** benötigen, dann führen Sie Abfragen nach dem Datentyp „Usage“ aus.

### <a name="data-volume-by-azure-resource-resource-group-or-subscription"></a>Datenmenge nach Azure-Ressource, Ressourcengruppe oder Abonnement

Für Daten von in Azure gehosteten Knoten können Sie die **Größe** der erfassten Daten __pro Computer__ mit der [Eigenschaft](./log-standard-columns.md#_resourceid) „_ResourceId“ abrufen, die den vollständigen Pfad zur Ressource enthält:

```kusto
find where TimeGenerated > ago(24h) project _ResourceId, _BilledSize, _IsBillable
| where _IsBillable == true 
| summarize BillableDataBytes = sum(_BilledSize) by _ResourceId | sort by BillableDataBytes nulls last
```

Für Daten von in Azure gehosteten Knoten können Sie die **Größe** der erfassten Daten __pro Azure-Abonnement__ mithilfe der Eigenschaft `_SubscriptionId` wie folgt abrufen:

```kusto
find where TimeGenerated > ago(24h) project _BilledSize, _IsBillable, _SubscriptionId
| where _IsBillable == true 
| summarize BillableDataBytes = sum(_BilledSize) by _SubscriptionId | sort by BillableDataBytes nulls last
```

Zum Abrufen des Datenvolumens nach Ressourcengruppe können Sie `_ResourceId` analysieren:

```kusto
find where TimeGenerated > ago(24h) project _ResourceId, _BilledSize, _IsBillable
| where _IsBillable == true 
| summarize BillableDataBytes = sum(_BilledSize) by _ResourceId
| extend resourceGroup = tostring(split(_ResourceId, "/")[4] )
| summarize BillableDataBytes = sum(BillableDataBytes) by resourceGroup | sort by BillableDataBytes nulls last
```

Bei Bedarf können Sie auch `_ResourceId` auf folgende Weise noch vollständiger analysieren:

```Kusto
| parse tolower(_ResourceId) with "/subscriptions/" subscriptionId "/resourcegroups/" 
    resourceGroup "/providers/" provider "/" resourceType "/" resourceName   
```

> [!TIP]
> Verwenden Sie diese `find`-Abfragen mit Bedacht, da umfassende Scans verschiedener Datentypen [ressourcenintensiv](./query-optimization.md#query-performance-pane) sind. Wenn Sie keine Ergebnisse pro Abonnement, Ressourcengruppe oder Ressourcenname benötigen, dann führen Sie Abfragen nach dem Datentyp „Usage“ aus.

> [!WARNING]
> Einige der Felder vom Typ „Nutzungsdaten“ sind zwar weiterhin im Schema enthalten, jedoch veraltet, und ihre Werte werden nicht mehr aufgefüllt. Dies sind neben **Computer** Felder in Bezug auf die Erfassung: **TotalBatches**, **BatchesWithinSla**, **BatchesOutsideSla**, **BatchesCapped** und **AverageProcessingTimeMs**.


### <a name="querying-for-common-data-types"></a>Abfragen von häufig verwendeten Datentypen

Wenn Sie die Datenquelle für einen bestimmten Datentyp näher untersuchen möchten, finden Sie hier einige nützliche Beispielabfragen:

+ **Arbeitsbereichsbasierte Application Insights**-Ressourcen
  - Weitere Informationen finden Sie unter [Verwalten der Nutzung und der Kosten für Application Insights](../app/pricing.md#data-volume-for-workspace-based-application-insights-resources).
+ **Sicherheitslösung**
  - `SecurityEvent | summarize AggregatedValue = count() by EventID`
+ **Protokollverwaltungslösung**
  - `Usage | where Solution == "LogManagement" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true | summarize AggregatedValue = count() by DataType`
+ Datentyp **Perf**
  - `Perf | summarize AggregatedValue = count() by CounterPath`
  - `Perf | summarize AggregatedValue = count() by CounterName`
+ Datentyp **Event**
  - `Event | summarize AggregatedValue = count() by EventID`
  - `Event | summarize AggregatedValue = count() by EventLog, EventLevelName`
+ Datentyp **Syslog**
  - `Syslog | summarize AggregatedValue = count() by Facility, SeverityLevel`
  - `Syslog | summarize AggregatedValue = count() by ProcessName`
+ Datentyp **AzureDiagnostics**
  - `AzureDiagnostics | summarize AggregatedValue = count() by ResourceProvider, ResourceId`

## <a name="tips-for-reducing-data-volume"></a>Tipps zum Reduzieren der Datenmenge

Hier finden Sie einige Vorschläge zum Verringern der erfassten Protokolle:

| Quelle mit hohem Datenvolumen | Reduzieren des Datenvolumens |
| -------------------------- | ------------------------- |
| Datensammlungsregeln      | Der [Azure Monitor-Agent](../agents/azure-monitor-agent-overview.md) verwendet Datensammlungsregeln, um das Sammeln von Daten zu verwalten. Sie können die [Datensammlung einschränken](../agents/data-collection-rule-azure-monitor-agent.md#limit-data-collection-with-custom-xpath-queries), indem Sie benutzerdefinierte XPath-Abfragen verwenden. | 
| Container Insights         | [Konfigurieren Sie Container Insights](../containers/container-insights-cost.md#controlling-ingestion-to-reduce-cost), um nur die Daten zu erfassen, die Sie benötigen. |
| Sicherheitsereignisse            | Wählen Sie [Sicherheitsereignisse vom Typ „Allgemein“ oder „Minimal“](../../security-center/security-center-enable-data-collection.md#data-collection-tier) aus. <br> Ändern der Sicherheitsüberwachungsrichtlinie, sodass nur benötigte Ereignisse erfasst werden. Überprüfen Sie insbesondere die Notwendigkeit zum Erfassen von Ereignissen für die <br> - [Überwachung der Filterplattform](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd772749(v=ws.10)) <br> - [Überwachung der Registrierung](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd941614(v%3dws.10))<br> - [Überwachung des Dateisystems](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd772661(v%3dws.10))<br> - [Überwachung des Kernelobjekts](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd941615(v%3dws.10))<br> - [Überwachung der Handleänderung](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd772626(v%3dws.10))<br> - Überwachung von Wechselmedien |
| Leistungsindikatoren       | Ändern Sie [Leistungsindikatoren-Konfiguration](../agents/data-sources-performance-counters.md) in: <br> - Reduzieren der Sammlungshäufigkeit <br> - Reduzieren der Anzahl von Leistungsindikatoren |
| Ereignisprotokolle                 | Ändern Sie die [Ereignisprotokollkonfiguration](../agents/data-sources-windows-events.md) in: <br> - Reduzieren der Anzahl von erfassten Ereignisprotokollen <br> - Ausschließliches Erfassen von erforderlichen Ereignisebenen. Erfassen Sie beispielsweise keine Ereignisse der Ebene *Informationen*. |
| syslog                     | Ändern Sie die [syslog-Konfiguration](../agents/data-sources-syslog.md) in: <br> - Reduzieren der Anzahl von erfassten Einrichtungen <br> - Ausschließliches Erfassen von erforderlichen Ereignisebenen. Erfassen Sie beispielsweise keine Ereignisse der Ebenen *Informationen* und *Debuggen*. |
| AzureDiagnostics           | Ändern Sie die [Ressourcenprotokollsammlung](../essentials/diagnostic-settings.md#create-in-azure-portal), um Folgendes zu erreichen: <br> - Verringern der Anzahl von Ressourcen, die Protokolle an Log Analytics senden <br> - Ausschließliches Erfassen von erforderlichen Protokollen |
| Lösungsdaten von Computern, für die die Lösung nicht erforderlich ist | Verwenden Sie die [Zielgruppenadressierung für Lösungen](../insights/solution-targeting.md), um Daten nur für erforderliche Gruppen mit Computern zu erfassen. |
| Application Insights | Optionen zum Verwalten des Application Insights-Datenvolumens finden Sie [hier](../app/pricing.md#managing-your-data-volume). |
| [Azure SQL-Analyse](../insights/azure-sql.md) | Passen Sie die Überwachungseinstellungen mit [Set-AzSqlServerAudit](/powershell/module/az.sql/set-azsqlserveraudit) an. |
| Azure Sentinel | Überprüfen Sie alle [Sentinel-Datenquellen](../../sentinel/connect-data-sources.md), die kürzlich als Quellen für zusätzliches Datenvolumen aktiviert wurden. |

### <a name="getting-nodes-as-billed-in-the-per-node-pricing-tier"></a>Abrufen von Knoten gemäß der Abrechnung im Tarif „Pro Knoten“

Um eine Liste von Computern abzurufen, die als Knoten berechnet werden, wenn für den Arbeitsbereich der Legacytarif „Pro Knoten“ gilt, suchen Sie Knoten, die **kostenpflichtige Datentypen** senden (einige Datentypen sind kostenlos). Verwenden Sie dazu die `_IsBillable`-[Eigenschaft](./log-standard-columns.md#_isbillable) und das linke Feld des vollständig qualifizierten Domänennamens. Dies gibt die Anzahl der Computer mit abgerechneten Daten pro Stunde zurück (was der Granularität entspricht, mit der Knoten gezählt und abgerechnet werden):

```kusto
find where TimeGenerated > ago(24h) project Computer, TimeGenerated
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| where computerName != ""
| summarize billableNodes=dcount(computerName) by bin(TimeGenerated, 1h) | sort by TimeGenerated asc
```

### <a name="getting-security-and-automation-node-counts"></a>Abrufen der Knotenanzahl für Sicherheit und Automatisierung

Um die Anzahl der verschiedenen Sicherheitsknoten anzuzeigen, können Sie diese Abfrage verwenden:

```kusto
union
(
    Heartbeat
    | where (Solutions has 'security' or Solutions has 'antimalware' or Solutions has 'securitycenter')
    | project Computer
),
(
    ProtectionStatus
    | where Computer !in~
    (
        (
            Heartbeat
            | project Computer
        )
    )
    | project Computer
)
| distinct Computer
| project lowComputer = tolower(Computer)
| distinct lowComputer
| count
```

Die Anzahl der verschiedenen Automation-Knoten können Sie mit dieser Abfrage anzeigen:

```kusto
 ConfigurationData 
 | where (ConfigDataType == "WindowsServices" or ConfigDataType == "Software" or ConfigDataType =="Daemons") 
 | extend lowComputer = tolower(Computer) | summarize by lowComputer 
 | join (
     Heartbeat 
       | where SCAgentChannel == "Direct"
       | extend lowComputer = tolower(Computer) | summarize by lowComputer, ComputerEnvironment
 ) on lowComputer
 | summarize count() by ComputerEnvironment | sort by ComputerEnvironment asc
```

## <a name="evaluating-the-legacy-per-node-pricing-tier"></a>Evaluieren des Legacy-Tarifs „Pro Knoten“

Das Treffen der Entscheidung, ob für Arbeitsbereiche mit Zugriff auf den Legacy-Tarif **Pro Knoten** anstelle des derzeitigen Tarifs ein aktueller Tarif wie **Nutzungsbasierte Zahlung** oder **Kapazitätsreservierung** besser geeignet ist, ist für Kunden häufig nicht leicht.  Hierfür muss der Kunde einerseits mit den Fixkosten pro überwachtem Knoten im Tarif „Pro Knoten“ und der enthaltenen Datenzuordnung von 500 MB pro Knoten und Tag vertraut sein und andererseits auch wissen, welche Kosten für die erfassten Daten im Tarif mit nutzungsbasierter Zahlung (pro GB) anfallen. 

Zur Vereinfachung dieser Bewertung können Sie die folgende Abfrage verwenden, um eine Empfehlung zum optimalen Tarif zu erhalten, die auf den Nutzungsmustern des Arbeitsbereichs basiert.  Bei dieser Abfrage werden die überwachten Knoten überprüft und die Daten ermittelt, die in den letzten sieben Tagen in einem Arbeitsbereich erfasst wurden. Für jeden Tag wird dann ausgewertet, welcher Tarif optimal gewesen wäre. Zum Verwenden der Abfrage müssen Sie angeben,

1. ob der Arbeitsbereich Azure Defender (Security Center) verwendet, indem Sie `workspaceHasSecurityCenter` auf `true` oder `false` festlegen, 
2. die Preise aktualisieren, wenn Sie über bestimmte Rabatte verfügen, und
3. die Anzahl der Tage angeben, die rückwirkend ermittelt und analysiert werden sollen, indem Sie `daysToEvaluate` festlegen. Dies ist hilfreich, wenn bei Berücksichtigung der Daten von sieben Tagen die Abfrage zu lange dauert. 

Hier sehen Sie die Abfrage für Tarifempfehlungen:

```kusto
// Set these parameters before running query
// Pricing details available at https://azure.microsoft.com/pricing/details/monitor/
let daysToEvaluate = 7; // Enter number of previous days to analyze (reduce if the query is taking too long)
let workspaceHasSecurityCenter = false;  // Specify if the workspace has Azure Security Center
let PerNodePrice = 15.; // Enter your montly price per monitored nodes
let PerNodeOveragePrice = 2.30; // Enter your price per GB for data overage in the Per Node pricing tier
let PerGBPrice = 2.30; // Enter your price per GB in the Pay-as-you-go pricing tier
let CarRes100Price = 196.; // Enter your price for the 100 GB/day Capacity Reservation
let CarRes200Price = 368.; // Enter your price for the 200 GB/day Capacity Reservation
let CarRes300Price = 540.; // Enter your price for the 300 GB/day Capacity Reservation
let CarRes400Price = 704.; // Enter your price for the 400 GB/day Capacity Reservation
let CarRes500Price = 865.; // Enter your price for the 500 GB/day Capacity Reservation
// ---------------------------------------
let SecurityDataTypes=dynamic(["SecurityAlert", "SecurityBaseline", "SecurityBaselineSummary", "SecurityDetection", "SecurityEvent", "WindowsFirewall", "MaliciousIPCommunication", "LinuxAuditLog", "SysmonEvent", "ProtectionStatus", "WindowsEvent", "Update", "UpdateSummary"]);
let StartDate = startofday(datetime_add("Day",-1*daysToEvaluate,now()));
let EndDate = startofday(now());
union * 
| where TimeGenerated >= StartDate and TimeGenerated < EndDate
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| where computerName != ""
| summarize nodesPerHour = dcount(computerName) by bin(TimeGenerated, 1h)  
| summarize nodesPerDay = sum(nodesPerHour)/24.  by day=bin(TimeGenerated, 1d)  
| join kind=leftouter (
    Heartbeat 
    | where TimeGenerated >= StartDate and TimeGenerated < EndDate
    | where Computer != ""
    | summarize ASCnodesPerHour = dcount(Computer) by bin(TimeGenerated, 1h) 
    | extend ASCnodesPerHour = iff(workspaceHasSecurityCenter, ASCnodesPerHour, 0)
    | summarize ASCnodesPerDay = sum(ASCnodesPerHour)/24.  by day=bin(TimeGenerated, 1d)   
) on day
| join (
    Usage 
    | where TimeGenerated >= StartDate and TimeGenerated < EndDate
    | where IsBillable == true
    | extend NonSecurityData = iff(DataType !in (SecurityDataTypes), Quantity, 0.)
    | extend SecurityData = iff(DataType in (SecurityDataTypes), Quantity, 0.)
    | summarize DataGB=sum(Quantity)/1000., NonSecurityDataGB=sum(NonSecurityData)/1000., SecurityDataGB=sum(SecurityData)/1000. by day=bin(StartTime, 1d)  
) on day
| extend AvgGbPerNode =  NonSecurityDataGB / nodesPerDay
| extend OverageGB = iff(workspaceHasSecurityCenter, 
             max_of(DataGB - 0.5*nodesPerDay - 0.5*ASCnodesPerDay, 0.), 
             max_of(DataGB - 0.5*nodesPerDay, 0.))
| extend PerNodeDailyCost = nodesPerDay * PerNodePrice / 31. + OverageGB * PerNodeOveragePrice
| extend billableGB = iff(workspaceHasSecurityCenter,
             (NonSecurityDataGB + max_of(SecurityDataGB - 0.5*ASCnodesPerDay, 0.)), DataGB )
| extend PerGBDailyCost = billableGB * PerGBPrice
| extend CapRes100DailyCost = CarRes100Price + max_of(billableGB - 100, 0.)* PerGBPrice
| extend CapRes200DailyCost = CarRes200Price + max_of(billableGB - 200, 0.)* PerGBPrice
| extend CapRes300DailyCost = CarRes300Price + max_of(billableGB - 300, 0.)* PerGBPrice
| extend CapRes400DailyCost = CarRes400Price + max_of(billableGB - 400, 0.)* PerGBPrice
| extend CapResLevel500AndAbove = max_of(floor(billableGB, 100),500)
| extend CapRes500AndAboveDailyCost = CarRes500Price*CapResLevel500AndAbove/500 + max_of(billableGB - CapResLevel500AndAbove, 0.)* PerGBPrice
| extend MinCost = min_of(
    PerNodeDailyCost,PerGBDailyCost,CapRes100DailyCost,CapRes200DailyCost,
    CapRes300DailyCost, CapRes400DailyCost, CapRes500AndAboveDailyCost)
| extend Recommendation = case(
    MinCost == PerNodeDailyCost, "Per node tier",
    MinCost == PerGBDailyCost, "Pay-as-you-go tier",
    MinCost == CapRes100DailyCost, "Capacity Reservation (100 GB/day)",
    MinCost == CapRes200DailyCost, "Capacity Reservation (200 GB/day)",
    MinCost == CapRes300DailyCost, "Capacity Reservation (300 GB/day)",
    MinCost == CapRes400DailyCost, "Capacity Reservation (400 GB/day)",
    MinCost == CapRes500AndAboveDailyCost, strcat("Capacity Reservation (",CapResLevel500AndAbove," GB/day)"),
    "Error"
)
| project day, nodesPerDay, ASCnodesPerDay, NonSecurityDataGB, SecurityDataGB, OverageGB, AvgGbPerNode, PerGBDailyCost, PerNodeDailyCost, 
    CapRes100DailyCost, CapRes200DailyCost, CapRes300DailyCost, CapRes400DailyCost, CapRes500AndAboveDailyCost, Recommendation 
| sort by day asc
//| project day, Recommendation // Comment this line to see details
| sort by day asc
```

Diese Abfrage spiegelt nicht exakt wider, wie der Verbrauch berechnet wird, ist aber in den meisten Fällen ausreichend, um Empfehlungen für einen Tarif bereitzustellen.  

> [!NOTE]
> Wenn Sie die Berechtigungen nutzen möchten, die Sie durch den Kauf der OMS E1-Suite, OMS E2-Suite oder des OMS-Add-Ons für System Center erwerben, wählen Sie den Tarif *Pro Knoten* für Log Analytics aus.

## <a name="create-an-alert-when-data-collection-is-high"></a>Erstellen einer Warnung für den Fall, dass die Datensammlung hoch ist

In diesem Abschnitt wird beschrieben, wie Sie über Azure Monitor-[Protokollwarnungen](../alerts/alerts-unified-log.md) eine Warnung erstellen, wenn das Datenvolumen in den letzten 24 Stunden einen angegebenen Wert überschritten hat. 

Führen Sie die folgenden Schritte aus, um eine Warnung auszugeben, wenn das in den letzten 24 Stunden erfasste abrechenbare Datenvolumen größer als 50 GB ist: 

- **Definieren der Warnungsbedingung**: Festlegen Ihres Log Analytics-Arbeitsbereichs als Ressourcenziel
- **Warnungskriterien**:
   - **Signalname**: **Benutzerdefinierte Protokollsuche**
   - **Suchabfrage**: `Usage | where IsBillable | summarize DataGB = sum(Quantity / 1000.) | where DataGB > 50`. Wenn Sie eine andere 
   - **Warnungslogik**: **Basiert auf** *Anzahl von Ergebnissen* und **Bedingung** ist *Größer als* ein **Schwellenwert** von *0*
   - **Zeitraum** von *1440* Minuten und **Warnungshäufigkeit** alle *1440* Minuten zur Ausführung einmal am Tag.
- **Definieren der Warnungsdetails**:
   - **Name**: *Abrechenbares Datenvolumen größer als 50 GB in 24 Stunden*
   - **Schweregrad**: *Warnung*

Geben Sie eine vorhandene [Aktionsgruppe](../alerts/action-groups.md) an, oder erstellen Sie eine neue, damit Sie benachrichtigt werden, wenn die Protokollwarnung Kriterien erfüllt.

Wenn Sie eine Warnung erhalten, verwenden Sie die Schritte in den Abschnitten oben, um per Problembehandlung zu ermitteln, warum die Nutzung höher als erwartet ist.

## <a name="data-transfer-charges-using-log-analytics"></a>Gebühren für die Datenübertragung mit Log Analytics

Beim Senden von Daten an Log Analytics fallen möglicherweise Gebühren für die Datenbandbreite an. Dies ist jedoch auf virtuelle Computer beschränkt, auf denen ein Log Analytics-Agent installiert ist, und gilt nicht bei Verwendung von Diagnoseeinstellungen oder anderen Connectors, die in Azure Sentinel integriert sind. Wie [auf der Seite Azure-Bandbreitenpreise](https://azure.microsoft.com/pricing/details/bandwidth/) beschrieben, wird die Datenübertragung zwischen Azure-Diensten in zwei Regionen mit dem normalen Preis als ausgehende Datenübertragung abgerechnet. Eingehende Datenübertragungen sind kostenlos. Diese Gebühr ist jedoch sehr niedrig (wenige %) im Vergleich zu den Kosten für die Log Analytics-Datenerfassung. Folglich muss sich die Kontrolle von Kosten für Log Analytics auf Ihr [erfasstes Datenvolumen](#understanding-ingested-data-volume) konzentrieren. 


## <a name="troubleshooting-why-log-analytics-is-no-longer-collecting-data"></a>Beheben des Problems, dass Log Analytics keine Daten mehr erfasst

Wenn Sie den kostenlosen Legacytarif nutzen und an einem Tag mehr als 500MB Daten gesendet haben, wird die Datensammlung für den Rest des Tages beendet. Das Erreichen des Tageslimits ist häufig die Ursache dafür, dass Log Analytics die Datensammlung beendet oder Daten scheinbar fehlen.  Log Analytics erstellt ein Ereignis vom Typ „Operation“, wenn die Datensammlung beginnt und endet. Führen Sie die folgende Abfrage in der Suche aus, um zu überprüfen, ob Sie das Tageslimit erreichen und Daten fehlen: 

```kusto
Operation | where OperationCategory == 'Data Collection Status'
```

Wenn die Datensammlung beendet wird, hat „OperationStatus“ den Wert **Warning** (Warnung). Wenn die Datensammlung beginnt, hat „OperationStatus“ den Wert **Succeeded** (Erfolgreich). Die folgende Tabelle beschreibt die Gründe, warum die Datensammlung endet, und eine empfohlene Aktion zum Fortsetzen der Datensammlung:  

|Grund für die Beendigung der Datensammlung| Lösung| 
|-----------------------|---------|
|Tägliche Obergrenze des Arbeitsbereichs wurde erreicht|Warten Sie, bis die Datensammlung am Folgetag automatisch neu gestartet wird, oder erhöhen Sie das Tageslimit für das Datenvolumen, wie unter „Verwalten des maximalen täglichen Datenvolumens“ beschrieben. Der Zeitpunkt für das Zurücksetzen der täglichen Obergrenze wird auf der Seite **Tägliche Obergrenze** angezeigt. |
| Ihr Arbeitsbereich hat die [Rate für das Datenerfassungsvolumen](../service-limits.md#log-analytics-workspaces) erreicht | Das Standardratenlimit für das Erfassungsvolumen für Daten, die von Azure-Ressourcen mit Diagnoseeinstellungen gesendet werden, beträgt ungefähr 6 GB/Minute pro Arbeitsbereich. Dies ist ein ungefährer Wert, da die tatsächliche Größe je nach Protokolllänge und Komprimierungsverhältnis zwischen den Datentypen variieren kann. Dieses Limit gilt nicht für Daten, die von Agents oder der Data Collector-API gesendet werden. Wenn Sie Daten mit einer höheren Rate an einen einzelnen Arbeitsbereich senden, werden einige Daten gelöscht, und es wird alle sechs Stunden ein Ereignis an die Tabelle Vorgang im Arbeitsbereich gesendet, während der Schwellenwert weiterhin überschritten wird. Wenn das Datenerfassungsvolumen weiterhin das Ratenlimit überschreitet oder Sie es wahrscheinlich in Kürze erreichen werden, können Sie eine Erhöhung für Ihren Arbeitsbereich anfordern, indem Sie eine E-Mail an LAIngestionRate@microsoft.com senden oder eine Supportanfrage öffnen. Mit der Abfrage `Operation | where OperationCategory == "Ingestion" | where Detail startswith "The rate of data crossed the threshold"` können Sie nach dem Ereignis suchen, das auf eine Überschreitung des Ratenlimits der Datenerfassung hinweist. |
|Tageslimit oder kostenloser Legacytarif erreicht |Warten Sie, bis die Datensammlung am Folgetag automatisch neu gestartet wird, oder wechseln Sie zu einem kostenpflichtigen Tarif.|
|Das Azure-Abonnement befindet sich aus folgendem Grund in einem angehaltenen Zustand:<br> Kostenlose Testversion endete<br> Azure Pass ist abgelaufen<br> Monatliches Ausgabenlimit ist erreicht (z.B. in einem MSDN- oder Visual Studio-Abonnement)|Konvertieren in ein kostenpflichtiges Abonnement<br> Limit entfernen oder warten, bis das Limit zurückgesetzt wird|

Führen Sie die Schritte unter *Erstellen einer täglichen Datenobergrenze* aus, um eine Benachrichtigung zu erhalten, wenn die Datensammlung beendet wird. Führen Sie die Schritte aus, die unter [Erstellen einer Aktionsgruppe](../alerts/action-groups.md) beschrieben sind, um eine E-Mail-, Webhook- oder Runbookaktion für die Warnungsregel zu konfigurieren. 

## <a name="late-arriving-data"></a>Spät eintreffende Daten   

Es kann vorkommen, dass Daten mit sehr alten Zeitstempeln erfasst werden – etwa, wenn ein Agent aufgrund eines Konnektivitätsproblems nicht mit Log Analytics kommunizieren kann. Auch kann es vorkommen, dass das Datum oder die Uhrzeit eines Hosts nicht korrekt ist. Verwenden Sie zum Diagnostizieren dieser Probleme die Spalte `_TimeReceived` ([weitere Informationen](./log-standard-columns.md#_timereceived)) sowie die Spalte `TimeGenerated`. `TimeReceived` ist die Zeit, zu der der Datensatz vom Azure Monitor-Erfassungspunkt in der Azure-Cloud empfangen wurde.  

## <a name="limits-summary"></a>Zusammenfassung der Grenzwerte

Es gibt einige zusätzliche Log Analytics-Grenzwerte, von denen einige vom Log Analytics-Tarif abhängen. Sie sind unter [Azure-Abonnement- und -Dienstgrenzen, Kontingente und Einschränkungen](../../azure-resource-manager/management/azure-subscription-service-limits.md#log-analytics-workspaces) dokumentiert.


## <a name="next-steps"></a>Nächste Schritte

- Informationen dazu, wie Sie die Suchsprache verwenden, finden Sie unter [Protokollsuchen in Azure Monitor-Protokollen](../logs/log-query-overview.md). Sie können Suchabfragen verwenden, um für die Nutzungsdaten eine zusätzliche Analyse durchzuführen.
- Führen Sie die unter [Erstellen, Anzeigen und Verwalten von Warnungen mithilfe von Azure Monitor](../alerts/alerts-metric.md) beschriebenen Schritte aus, um benachrichtigt zu werden, wenn ein Suchkriterium erfüllt ist.
- Verwenden Sie die [Zielgruppenadressierung für Lösungen](../insights/solution-targeting.md), um Daten nur für erforderliche Gruppen mit Computern zu erfassen.
- Machen Sie sich unter [Konfigurieren der automatischen Bereitstellung für Agents und Erweiterungen aus Azure Security Center](../../security-center/security-center-enable-data-collection.md) mit der Filterrichtlinie für Azure Defender (Security Center) vertraut, um eine effektive Richtlinie zur Erfassung von Ereignissen zu konfigurieren.
- Ändern Sie die [Leistungsindikatorenkonfiguration](../agents/data-sources-performance-counters.md).
- Informationen zum Ändern der Einstellungen für die Ereigniserfassung finden Sie unter [Datenquellen für Windows-Ereignisprotokolle in Log Analytics](../agents/data-sources-windows-events.md).
- Informationen zum Ändern der Einstellungen für die Syslog-Sammlung finden Sie unter [Syslog-Datenquellen in Log Analytics](../agents/data-sources-syslog.md).
- Informationen zum Ändern der Einstellungen für die Syslog-Sammlung finden Sie unter [Syslog-Datenquellen in Log Analytics](../agents/data-sources-syslog.md).