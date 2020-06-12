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
ms.date: 05/28/2020
ms.author: bwren
ms.subservice: ''
ms.openlocfilehash: cded8fef70e22ffebc412ea37898100cda4bb3df
ms.sourcegitcommit: 12f23307f8fedc02cd6f736121a2a9cea72e9454
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/30/2020
ms.locfileid: "84219019"
---
# <a name="manage-usage-and-costs-with-azure-monitor-logs"></a>Verwalten von Nutzung und Kosten mit Azure Monitor-Protokollen

> [!NOTE]
> In diesem Artikel wird beschrieben, wie Sie die Kosten für Azure Monitor-Protokolle ermitteln und steuern. In einem verwandten Artikel, [Überwachen der Nutzung und der geschätzten Kosten](https://docs.microsoft.com/azure/azure-monitor/platform/usage-estimated-costs), wird erläutert, wie die Nutzung und geschätzten Kosten über mehrere Azure-Überwachungsfeatures hinweg für unterschiedliche Preismodelle angezeigt werden. Alle in diesem Artikel gezeigten Preise und Kosten dienen nur zu Beispielzwecken. 

Azure Monitor-Protokolle sind für die Skalierung und Unterstützung der täglichen Sammlung, Indizierung und Speicherung enormer Datenmengen aus beliebigen Quellen in Ihrem Unternehmen oder aus in Azure bereitgestellten Quellen konzipiert.  Dies ist zwar ggf. die primäre Motivation für die Verwendung in Ihrem Unternehmen, letztendlich geht es jedoch um Kosteneffizienz. In diesem Zusammenhang ist es wichtig zu wissen, dass die Kosten eines Log Analytics-Arbeitsbereichs nicht nur auf dem Umfang der gesammelten Daten basieren, sondern auch davon abhängen, welcher Tarif gewählt wurde und wie lange die von den verbundenen Quellen generierten Daten gespeichert werden sollen.  

In diesem Artikel erfahren Sie, wie Sie das erfasste Datenvolumen und Speicherwachstum proaktiv überwachen und Grenzwerte festlegen, um die damit verbundenen Kosten zu steuern. 

## <a name="pricing-model"></a>Preismodell

Die Standardpreise für Log Analytics werden gemäß der **nutzungsbasierten Zahlung** berechnet und basieren auf dem verbrauchten Datenvolumen, und optional wird die längere Datenaufbewahrung berücksichtigt. Das Datenvolumen wird als Größe der gespeicherten Daten gemessen. Jeder Log Analytics-Arbeitsbereich wird als separater Dienst abgerechnet und auf der Rechnung für Ihr Azure-Abonnement aufgeführt. Die bei der Erfassung anfallende Datenmenge kann erheblich sein und hängt von den folgenden Faktoren ab: 

  - Anzahl der aktivierten Verwaltungslösungen und deren Konfiguration
  - Anzahl der überwachten VMs
  - Typ der Daten, die von jeder überwachten VM gesammelt werden 
  
Zusätzlich zum Modell der nutzungsbasierten Bezahlung bietet Log Analytics Tarife für die **Kapazitätsreservierung**. Mit diesen Tarifen können Sie Einsparungen von 25 % gegenüber der nutzungsbasierten Zahlung erzielen. Die Preise der Kapazitätsreservierung ermöglichen Ihnen den Kauf einer Reservierung ab 100 GB/Tag. Jeder über die Reservierung hinausgehende Verbrauch wird entsprechend der nutzungsbasierten Bezahlung berechnet. Die Tarife für die Kapazitätsreservierung umfassen einen Verpflichtungszeitraum von 31 Tagen. Während dieses Verpflichtungszeitraums können Sie in einen höheren Tarif für die Kapazitätsreservierung wechseln (hierbei wird der 31-tägige Verpflichtungszeitraum neu gestartet), aber es ist erst nach Ablauf des Verpflichtungszeitraums möglich, zum nutzungsbasierten Modell oder zu einem niedrigeren Tarif für die Kapazitätsreservierung zurückzukehren. Bei den Tarifen für die Kapazitätsreservierung erfolgt die Abrechnung täglich. [Erfahren Sie mehr](https://azure.microsoft.com/pricing/details/monitor/) über Preise der nutzungsbasierten Bezahlung und Kapazitätsreservierung für Log Analytics. 

In allen Tarifen wird das Datenvolume anhand einer Zeichenfolgendarstellung der Daten berechnet, während ihre Speicherung vorbereitet wird. Mehrere [Eigenschaften, die alle Datentypen gemein haben](https://docs.microsoft.com/azure/azure-monitor/platform/log-standard-properties), werden bei der Berechnung der Ereignisgröße nicht berücksichtigt. Dazu gehören `_ResourceId`, `_ItemId`, `_IsBillable` und `_BilledSize`.

Beachten Sie außerdem, dass für einige Lösungen, z. B. [Azure Security Center](https://azure.microsoft.com/pricing/details/security-center/), [Azure Sentinel](https://azure.microsoft.com/pricing/details/azure-sentinel/) und [Konfigurationsverwaltung](https://azure.microsoft.com/pricing/details/automation/), ein eigenes Preismodell gilt. 

### <a name="log-analytics-dedicated-clusters"></a>Dedizierte Log Analytics-Cluster

Dedizierte Log Analytics-Cluster sind Sammlungen von Arbeitsbereichen in einem einzelnen verwalteten Azure Data Explorer-Cluster, die erweiterte Szenarien unterstützen, beispielsweise [vom Kunden verwaltete Schlüssel](https://docs.microsoft.com/azure/azure-monitor/platform/customer-managed-keys).  Dedizierte Log Analytics-Cluster unterstützen nur ein Preismodell für die Kapazitätsreservierung ab 1000 GB/Tag mit einem Rabatt von 25 % im Vergleich zu nutzungsbasierten Preisen. Jeder über die Reservierung hinausgehende Verbrauch wird entsprechend der nutzungsbasierten Bezahlung berechnet. Die Clusterkapazitätsreservierung hat einen Verpflichtungszeitraum von 31 Tagen, nachdem die Reservierungsebene erhöht wurde. Während des Verpflichtungszeitraums kann die Kapazitätsreservierungsebene nicht herabgesetzt, aber jederzeit erhöht werden. Erfahren Sie mehr über das [Erstellen eines Log Analytics-Clusters](https://docs.microsoft.com/azure/azure-monitor/platform/customer-managed-keys#create-cluster-resource) und das [Zuordnen von Arbeitsbereichen zu diesem Cluster](https://docs.microsoft.com/azure/azure-monitor/platform/customer-managed-keys#workspace-association-to-cluster-resource).  

Die Reservierungsebene für die Clusterkapazität wird programmgesteuert mit Azure Resource Manager mithilfe des `Capacity`-Parameters unter `Sku` konfiguriert. `Capacity` wird in Einheiten von GB und in Schritten von 100 GB pro Tag in Werten von 1000 GB/Tag oder mehr angegeben. Weitere Informationen finden Sie [hier](https://docs.microsoft.com/azure/azure-monitor/platform/customer-managed-keys#create-cluster-resource). Wenn Ihr Cluster eine Reservierung über 2000 GB/Tag erfordert, kontaktieren Sie uns unter [LAIngestionRate@microsoft.com](mailto:LAIngestionRate@microsoft.com).

Für die Abrechnung des Verbrauchs in einem Cluster stehen zwei Modi zur Verfügung. Diese können beim [Konfigurieren Ihres Clusters](https://docs.microsoft.com/azure/azure-monitor/platform/customer-managed-keys#cmk-manage) mithilfe des Parameters `billingType` angegeben werden. Die beiden Modi sind: 

1. **Cluster** (Standardeinstellung): In diesem Fall werden erfasste Daten auf der Clusterebene abgerechnet. Die erfassten Datenmengen aus den einzelnen Arbeitsbereichen, die einem Cluster zugeordnet sind, werden aggregiert, um die tägliche Abrechnung für den Cluster zu berechnen. Beachten Sie, dass die Zuordnungen pro Knoten von [Azure Security Center](https://docs.microsoft.com/azure/security-center/) vor dieser Aggregation von Daten in allen Arbeitsbereichen im Cluster auf Arbeitsbereichsebene angewendet werden. 

2. **Arbeitsbereiche**: Die Kapazitätsreservierungskosten für Ihren Cluster werden proportional den Arbeitsbereichen im Cluster zugeordnet (nach Berücksichtigung der knotenspezifischen Zuordnungen von [Azure Security Center](https://docs.microsoft.com/azure/security-center/) für den jeweiligen Arbeitsbereich). Wenn das gesamte Datenvolumen, das an einem Tag in einem Arbeitsbereich erfasst wird, unter der Kapazitätsreservierung liegt, werden für die einzelnen Arbeitsbereiche jeweils die erfassten Daten mit dem effektiven GB-basierten Kapazitätsreservierungssatz abgerechnet. Hierzu wird ein Teil der Kapazitätsreservierung abgerechnet, und der ungenutzte Teil der Kapazitätsreservierung wird für die Clusterressource in Rechnung gestellt. Wenn das gesamte Datenvolumen, das an einem Tag in einem Arbeitsbereich erfasst wird, über der Kapazitätsreservierung liegt, wird für die einzelnen Arbeitsbereiche jeweils ein Teil der Kapazitätsreservierung (basierend auf dem Anteil der an diesem Tag erfassten Daten) abgerechnet, und für jeden Arbeitsbereich wird jeweils ein Teil der erfassten Daten in Rechnung gestellt, die über die Kapazitätsreservierung hinausgehen. Wenn das gesamte Datenvolumen, das an einem Tag in einem Arbeitsbereich erfasst wird, über der Kapazitätsreservierung liegt, wird nichts für die Clusterressource abgerechnet.


In Clusterabrechnungsoptionen wird die Datenaufbewahrung auf der Arbeitsbereichsebene abgerechnet. Beachten Sie, dass die Clusterabrechnung mit der Clustererstellung beginnt, unabhängig davon, ob dem Cluster Arbeitsbereiche zugeordnet wurden. Beachten Sie außerdem, dass Arbeitsbereiche, die einem Cluster zugeordnet sind, nicht mehr über einen Tarif verfügen.

## <a name="estimating-the-costs-to-manage-your-environment"></a>Schätzen der Kosten für die Verwaltung Ihrer Umgebung 

Wenn Sie noch keine Azure Monitor-Protokolle verwenden, können Sie mit dem [Azure Monitor-Preisrechner](https://azure.microsoft.com/pricing/calculator/?service=monitor) die Kosten für die Nutzung von Log Analytics schätzen. Geben Sie „Azure Monitor“ im Suchfeld ein, und klicken Sie auf die ausgegebene Azure Monitor-Kachel. Scrollen Sie auf der Seite nach unten bis zu „Azure Monitor“, und wählen Sie in der Dropdownliste „Typ“ den Eintrag „Log Analytics“ aus.  Hier können Sie die Anzahl der VMs sowie das Datenvolumen (in GB) eingeben, dessen Erfassung durch die einzelnen VMs erwartet wird. Von einer typischen Azure-VM werden in der Regel 1 bis 3 GB Daten pro Monat erfasst. Wenn Sie bereits Azure Monitor-Protokolle auswerten, können Sie die Datenstatistiken aus Ihrer eigenen Umgebung verwenden. Unten wird erläutert, wie die [Anzahl der überwachten VMs](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#understanding-nodes-sending-data) und das [erfasste Datenvolumen im Arbeitsbereich](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#understanding-ingested-data-volume) bestimmt werden. 

## <a name="understand-your-usage-and-estimate-costs"></a>Verstehen Ihrer Nutzung und Schätzen von Kosten

Wenn Sie jetzt Azure Monitor-Protokolle verwenden, können Sie auf der Grundlage aktueller Nutzungsmuster problemlos die zu erwartenden Kosten ermitteln. Verwenden Sie **Analysieren der Datennutzung in Log Analytics**, um die Datennutzung zu überprüfen und zu analysieren. Hier wird angezeigt, wie viele Daten von jeder Lösung gesammelt werden, wie viele Daten aufbewahrt werden, und es wird eine Kostenschätzung angezeigt. Diese basiert auf der Menge an erfassten Daten und berücksichtigt eine eventuelle zusätzliche Aufbewahrung über die enthaltenen Menge hinaus.

![Nutzung und geschätzte Kosten](media/manage-cost-storage/usage-estimated-cost-dashboard-01.png)

Um Ihre Dateien ausführlicher zu untersuchen, klicken Sie auf der Seite **Nutzung und geschätzte Kosten** auf das Symbol oben rechts neben den Diagrammen. Sie können mit dieser Abfrage weitere Details zu Ihrer Nutzung abrufen.  

![Ansicht „Protokolle“](media/manage-cost-storage/logs.png)

Auf der Seite **Nutzung und geschätzte Kosten** können Sie Ihr Datenvolumen für den Monat überprüfen. Dieses beinhaltet alle Daten, die in Ihrem Log Analytics-Arbeitsbereich empfangen und aufbewahrt wurden.  Klicken Sie im oberen Bereich der Seite auf **Nutzungsdetails**, um das Dashboard „Nutzung“ anzuzeigen. Dort finden Sie Informationen zu Datenvolumentrends nach Quelle, Computern und Angebot. Klicken Sie auf **Datenmengenverwaltung**, um die tägliche Obergrenze anzuzeigen oder festzulegen oder um die Aufbewahrungsdauer zu ändern.
 
Die Gebühren für Log Analytics fließen in Ihre Azure-Rechnung ein. Die Details Ihrer Azure-Rechnung finden Sie im Bereich „Abrechnung“ des Azure-Portals oder im [Azure-Abrechnungsportal](https://account.windowsazure.com/Subscriptions).  

## <a name="viewing-log-analytics-usage-on-your-azure-bill"></a>Anzeigen des Log Analytics-Verbrauchs auf Ihrer Azure-Rechnung 

Azure bietet im Hub [Azure Cost Management und Abrechnung](https://docs.microsoft.com/azure/cost-management/quick-acm-cost-analysis?toc=/azure/billing/TOC.json) eine Vielzahl nützlicher Funktionen. Mithilfe der Funktion „Kostenanalyse“ können Sie beispielsweise Ihre Ausgaben für Azure-Ressourcen überprüfen. Fügen Sie zunächst einen Filter nach „Ressourcentyp“ (zu „microsoft.operationalinsights/workspace“ für Log Analytics und „microsoft.operationalinsights/workspace“ für Log Analytics-Cluster) hinzu, damit Sie Ihre Log Analytics-Ausgaben verfolgen können. Wählen Sie anschließend unter „Gruppieren nach“ die Option „Kategorie der Verbrauchseinheit“ oder „Verbrauchseinheit“ aus.  Beachten Sie, dass andere Dienste wie Azure Security Center und Azure Sentinel die jeweilige Nutzung ebenfalls für Log Analytics-Arbeitsbereichsressourcen berechnen. Um die Zuordnung zu Dienstnamen zu sehen, können Sie die Tabellenansicht anstelle eines Diagramms auswählen. 

Ein umfassenderes Verständnis Ihres Verbrauchs erlangen Sie, indem Sie [Ihre Verbrauchsdaten aus dem Azure-Portal herunterladen](https://docs.microsoft.com/azure/billing/billing-download-azure-invoice-daily-usage-date#download-usage-in-azure-portal). In der heruntergeladenen Tabelle wird der Verbrauch pro Azure-Ressource (z. B. Log Analytics-Arbeitsbereich) pro Tag aufgeführt. In dieser Excel-Tabelle können Sie den Verbrauch für Ihre Log Analytics-Arbeitsbereiche ermitteln, indem Sie zuerst nach der Spalte „Kategorie der Verbrauchseinheit“ filtern, um „Log Analytics“, „Insights & Analytics“ (von einigen der Legacy-Tarife verwendet) und „Azure Monitor“ (von Tarifen für Kapazitätsreservierung verwendet) anzuzeigen und dann in der Spalte „Instanz-ID“ einen Filter namens „contains workspace“ (enthält Arbeitsbereich) oder „contains cluster“ (enthält Cluster) hinzufügen (Letzterer dient zum Einbeziehen der Log Analytics-Clusternutzung). Der Verbrauch wird in der Spalte „Verbrauchte Menge“ aufgeführt, und die Einheit für jeden Eintrag wird in der Spalte „Maßeinheit“ angegeben.  Es sind weitere Einzelheiten verfügbar, anhand derer Sie [Ihre Microsoft Azure-Rechnung verstehen](https://docs.microsoft.com/azure/billing/billing-understand-your-bill). 

## <a name="changing-pricing-tier"></a>Ändern des Tarifs

Gehen Sie folgendermaßen vor, um den Log Analytics-Tarif Ihres Arbeitsbereichs zu ändern: 

1. Öffnen Sie im Azure-Portal aus Ihrem Arbeitsbereich **Nutzung und geschätzte Kosten**. Hier wird eine Liste aller Tarife angezeigt, die für diesen Arbeitsbereich verfügbar sind.

2. Überprüfen Sie die geschätzten Kosten für die einzelnen Tarife. Diese Schätzungen basierend auf der Nutzung der letzten 31 Tage, d. h. bei dieser Kostenschätzung wird davon ausgegangen, dass die letzten 31 Tage repräsentativ für Ihre typische Nutzung sind. Im Beispiel unten können Sie sehen, dass die Kosten für diesen Arbeitsbereich basierend auf den Datenmustern der letzten 31 Tage im Modell der nutzungsbasierten Zahlung (Nr. 1) geringer wären als im Tarif mit einer Kapazitätsreservierung von 100 GB/Tag (Nr. 2).  

    ![Tarife](media/manage-cost-storage/pricing-tier-estimated-costs.png)

3. Klicken Sie nach Überprüfung der geschätzten Kosten basierend auf der Nutzung der letzten 31 Tage auf **Auswählen**, wenn Sie sich für einen Wechsel des Tarifs entscheiden.  

Es ist auch möglich, den [Tarif über Azure Resource Manager festzulegen](https://docs.microsoft.com/azure/azure-monitor/platform/template-workspace-configuration#configure-a-log-analytics-workspace). Verwenden Sie hierzu den Parameter `sku` (`pricingTier` in der Azure Resource Manager-Vorlage). 

## <a name="legacy-pricing-tiers"></a>Legacytarife

Abonnements, die vor dem 2. April 2018 einen Log Analytics-Arbeitsbereich oder eine Application Insights-Ressource enthielten oder mit einem Enterprise Agreement verknüpft sind, das vor dem 1. Februar 2019 abgeschlossen wurde, haben weiterhin Zugriff auf die Legacytarife: **Free**, **Eigenständig (Pro GB)** und **Pro Knoten (OMS)** .  Für Arbeitsbereiche im Tarif „Free“ ist die tägliche Datenerfassung auf 500 MB beschränkt (mit Ausnahme von Sicherheitsdaten, die von [Azure Security Center](https://docs.microsoft.com/azure/security-center/) gesammelt werden). Darüber hinaus ist die Datenaufbewahrung auf sieben Tage beschränkt. Der Tarif „Free“ dient nur zu Evaluierungszwecken. Für Arbeitsbereiche im Tarif „Eigenständig“ oder „Pro Knoten“ gilt eine vom Benutzer festlegbare Datenaufbewahrungsfrist von 30 bis 730 Tagen.

Der Tarif „Pro Knoten“ wird pro überwachter VM (Knoten) mit einer Granularität von einer Stunde berechnet. Für jeden überwachten Knoten werden dem Arbeitsbereich 500 MB Daten pro Tag zugeteilt, die nicht berechnet werden. Diese Zuteilung wird auf Arbeitsbereichsebene aggregiert. Daten, die über die aggregierte tägliche Datenzuteilung hinaus erfasst werden, werden pro GB als Datenüberschreitung berechnet. Beachten Sie, dass der Dienst auf Ihrer Rechnung als **Insight und Analytics** für die Log Analytics-Nutzung aufgeführt ist, wenn für den Arbeitsbereich der Tarif „Pro Knoten“ festgelegt ist. 

> [!TIP]
> Falls für Ihren Arbeitsbereich Zugriff auf den Tarif **Pro Knoten** besteht und Sie sich die Frage stellen, ob die Kosten beim Tarif mit nutzungsbasierter Zahlung niedriger wären, können Sie mit der [unten angegebenen Abfrage](#evaluating-the-legacy-per-node-pricing-tier) leicht eine Empfehlung abrufen. 

Vor April 2016 erstellte Arbeitsbereiche haben ebenfalls Zugriff auf die ursprünglichen Tarife **Standard** und **Premium**, für die eine feste Datenaufbewahrung von 30 bzw. 365 Tagen gilt. Neue Arbeitsbereiche können nicht im Tarif **Standard** oder **Premium** erstellt werden, und wenn ein Arbeitsbereich aus diesen Tarifen verschoben wird, kann er nicht zurück verschoben werden.

Es gibt auch einige Besonderheiten bei der Verwendung von Log Analytics-Legacytarifen und der Berechnung der Nutzung für [Azure Security Center](https://docs.microsoft.com/azure/security-center/). 

1. Wenn für den Arbeitsbereich der Legacytarif „Standard“ oder „Premium“ zutrifft, wird Azure Security Center nur für die Log Analytics-Datenerfassung und nicht pro Knoten berechnet.
2. Wenn für den Arbeitsbereich der Legacytarif „Pro Knoten“ zutrifft, wird Azure Security Center gemäß dem aktuellen [knotenbasierten Preismodell für Azure Security Center](https://azure.microsoft.com/pricing/details/security-center/) berechnet. 
3. In anderen Tarifen (einschließlich Kapazitätsreservierungen) wird Azure Security Center bei Aktivierung vor dem 19. Juni 2017 nur für die Log Analytics-Datenerfassung berechnet. Andernfalls wird Azure Security Center gemäß dem aktuellen knotenbasierten Preismodell für Azure Security Center berechnet.

Weitere Informationen zu Einschränkungen von Tarifen finden Sie [hier](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#log-analytics-workspaces).

> [!NOTE]
> Wenn Sie die Berechtigungen nutzen möchten, die Sie durch den Kauf der OMS E1-Suite, OMS E2-Suite oder des OMS-Add-Ons für System Center erwerben, wählen Sie den Tarif *Pro Knoten* für Log Analytics aus.

## <a name="change-the-data-retention-period"></a>Ändern des Datenaufbewahrungszeitraums

Die folgenden Schritte zeigen, wie Sie die Aufbewahrungsdauer von Protokolldaten in Ihrem Arbeitsbereich konfigurieren. Die Datenaufbewahrung kann für alle Arbeitsbereiche zwischen 30 und 730 Tagen (2 Jahre) konfiguriert werden, es sei denn, sie verwenden den Legacytarif „Free“. Informieren Sie sich über die [Preise](https://azure.microsoft.com/pricing/details/monitor/) für eine längere Datenaufbewahrung. 

### <a name="default-retention"></a>Standardaufbewahrungsdauer

So legen Sie die Standardaufbewahrungsdauer für Ihren Arbeitsbereich fest: 
 
1. Wählen Sie im linken Bereich des Azure-Portals über Ihren Arbeitsbereich **Nutzung und geschätzte Kosten** aus.
2. Klicken Sie im oberen Bereich der Seite **Nutzung und geschätzte Kosten** auf **Datenmengenverwaltung**.
3. Passen Sie mithilfe des Schiebereglers die Anzahl von Tagen an, und klicken Sie anschließend auf **OK**.  Wenn Sie sich im Tarif *Free* befinden, können Sie den Datenaufbewahrungszeitraum nicht ändern. Sie müssen in einen kostenpflichtigen Tarif wechseln, um diese Einstellung zu steuern.

    ![Ändern des Datenaufbewahrungszeitraums für den Arbeitsbereich](media/manage-cost-storage/manage-cost-change-retention-01.png)

Wenn die Aufbewahrungsdauer gesenkt wird, besteht ein Toleranzzeitraum von mehreren Tagen, bevor die ältesten Daten entfernt werden. 
    
Die Aufbewahrungsdauer kann auch [über Azure Resource Manager festgelegt werden](https://docs.microsoft.com/azure/azure-monitor/platform/template-workspace-configuration#configure-a-log-analytics-workspace) (mithilfe des Parameters `retentionInDays`). Wenn Sie die Datenaufbewahrung auf 30 Tage festlegen, können Sie außerdem mit dem Parameter `immediatePurgeDataOn30Days` eine sofortige Bereinigung älterer Daten auslösen, was für konformitätsrelevante Szenarien nützlich sein kann. Diese Funktion wird ausschließlich über Azure Resource Manager verfügbar gemacht. 


Zwei Datentypen – `Usage` und `AzureActivity` – werden standardmäßig mindestens 90 Tage lang aufbewahrt, und diese 90-tägige Aufbewahrung wird nicht in Rechnung gestellt. Wenn die Aufbewahrung des Arbeitsbereichs auf über 90 Tage verlängert wird, wird auch die Aufbewahrung dieser Datentypen verlängert.  Für diese Datentypen werden auch keine Gebühren für die Datenerfassung erhoben. 

Datentypen von arbeitsbereichsbasierten Application Insights-Ressourcen (`AppAvailabilityResults`, `AppBrowserTimings`, `AppDependencies`, `AppExceptions`, `AppEvents`, `AppMetrics`, `AppPageViews`, `AppPerformanceCounters`, `AppRequests`, `AppSystemEvents` und `AppTraces`) werden auch standardmäßig 90 Tage lang aufbewahrt, und diese 90-tägige Aufbewahrung wird nicht in Rechnung gestellt. Die jeweilige Aufbewahrungsdauer kann mithilfe der Funktion für die Aufbewahrung nach Datentyp angepasst werden. 


### <a name="retention-by-data-type"></a>Aufbewahrung nach Datentyp

Es ist auch möglich, unterschiedliche Aufbewahrungseinstellungen für einzelne Datentypen zwischen 30 und 730 Tagen anzugeben (mit Ausnahme von Arbeitsbereichen im Legacytarif „Free“). Jeder Datentyp ist eine Unterressource des Arbeitsbereichs. Die SecurityEvent-Tabelle kann in [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) beispielsweise wie folgt adressiert werden:

```
/subscriptions/00000000-0000-0000-0000-00000000000/resourceGroups/MyResourceGroupName/providers/Microsoft.OperationalInsights/workspaces/MyWorkspaceName/Tables/SecurityEvent
```

Beachten Sie, dass beim Datentyp (Tabelle) die Groß-und Kleinschreibung beachtet wird.  Geben Sie Folgendes an, um die aktuellen datentypspezifischen Aufbewahrungseinstellungen für einen bestimmten Datentyp (in diesem Beispiel SecurityEvent) abzurufen:

```JSON
    GET /subscriptions/00000000-0000-0000-0000-00000000000/resourceGroups/MyResourceGroupName/providers/Microsoft.OperationalInsights/workspaces/MyWorkspaceName/Tables/SecurityEvent?api-version=2017-04-26-preview
```

Um die aktuellen datentypspezifischen Aufbewahrungseinstellungen für alle Datentypen in Ihrem Arbeitsbereich abzurufen, lassen Sie einfach den spezifischen Datentyp aus. Beispiel:

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

Sie können eine tägliche Obergrenze konfigurieren und die tägliche Erfassung für Ihren Arbeitsbereich einschränken. Dabei ist jedoch Vorsicht geboten, da dieses Limit möglichst nicht erreicht werden sollte.  Andernfalls verlieren Sie die Daten des restlichen Tages. Dies kann sich auf Azure-Dienste und -Lösungen auswirken, deren Funktionalität unter Umständen von der Verfügbarkeit aktueller Daten im Arbeitsbereich abhängt.  Infolgedessen kann auch die Integrität von Ressourcen, die IT-Diensten zugrunde liegen, nicht mehr zuverlässig überwacht werden, und es können keine Warnungen empfangen werden.  Die tägliche Obergrenze ist dazu gedacht, einen unerwarteten Anstieg des Datenvolumens aus Ihren verwalteten Ressourcen zu verhindern und den Grenzwert einzuhalten – oder ungeplante Gebühren für Ihren Arbeitsbereich zu vermeiden.  

Kurz nach Erreichen des Tageslimits werden für den Rest des Tages keine kostenpflichtigen Datentypen mehr gesammelt. (Die Standardlatenz beim Anwenden des Tageslimits kann dazu führen, dass das Limit nicht so genau wie das festgelegte Tageslimit angewendet wird.) Im oberen Seitenbereich erscheint ein Warnbanner für den ausgewählten Log Analytics-Arbeitsbereich, und an die Tabelle *Operation* wird unter der Kategorie **LogManagement** ein Vorgangsereignis gesendet. Die Datensammlung wird nach der unter *Daily limit will be set at* (Tageslimit wird festgelegt um) definierten Zurücksetzungszeit fortgesetzt. Es empfiehlt sich, eine Warnungsregel auf der Grundlage dieses Vorgangsereignisses zu definieren und so zu konfigurieren, dass bei Erreichen des Tageslimits für Daten eine Benachrichtigung erfolgt. 

> [!WARNING]
> Die Sammlung von Daten im Azure Security Center wird durch die tägliche Obergrenze nicht beendet, mit Ausnahme von Arbeitsbereichen, in denen Azure Security Center vor dem 19. Juni 2017 installiert wurde. 

### <a name="identify-what-daily-data-limit-to-define"></a>Identifizieren des zu definierenden Tageslimits für Daten

Informieren Sie sich unter [Analysieren der Datennutzung in Log Analytics](usage-estimated-costs.md) über den Datenerfassungstrend sowie über die zu definierende tägliche Volumenobergrenze. Wählen Sie die Obergrenze mit Bedacht, da Sie Ihre Ressourcen nach dem Erreichen des Limits nicht mehr überwachen können. 

### <a name="set-the-daily-cap"></a>Festlegen der täglichen Obergrenze

In den folgenden Schritten erfahren Sie, wie Sie ein Tageslimit für die vom Log Analytics-Arbeitsbereich erfasste Datenmenge konfigurieren.  

1. Klicken Sie links in Ihrem Arbeitsbereich auf **Nutzung und geschätzte Kosten**.
2. Klicken Sie im oberen Bereich der Seite **Nutzung und geschätzte Kosten** für den ausgewählten Arbeitsbereich auf **Datenmengenverwaltung**. 
3. Die tägliche Obergrenze ist standardmäßig **AUS**. Klicken Sie auf **EIN**, um sie zu aktivieren, und legen Sie das Limit für das Datenvolumen in GB/Tag fest.

    ![Konfigurieren des Log Analytics-Datenlimits](media/manage-cost-storage/set-daily-volume-cap-01.png)

### <a name="alert-when-daily-cap-reached"></a>Warnung bei Erreichen der täglichen Obergrenze

Im Azure-Portal wird bei Erreichen des Schwellenwerts für das Datenlimit zwar ein visueller Hinweis angezeigt, dieses Verhalten steht jedoch möglicherweise nicht im Einklang mit der gewünschten Behandlung von Betriebsproblemen, die eine umgehende Reaktion erfordern.  Wenn Sie eine Warnbenachrichtigung erhalten möchten, können Sie in Azure Monitor eine neue Warnregel erstellen.  Weitere Informationen finden Sie unter [Erstellen, Anzeigen und Verwalten von Warnungen](alerts-metric.md).

Im Anschluss finden Sie die empfohlenen Einstellungen für die Warnung:

- Ziel: Wählen Sie Ihre Log Analytics-Ressource aus.
- Kriterien: 
   - Signalname: Benutzerdefinierte Protokollsuche
   - Suchabfrage: Operation | where Detail has 'OverQuota'
   - Basierend auf: Anzahl der Ergebnisse
   - Bedingung: Größer als
   - Schwellenwert: 0
   - Zeitraum: 5 (Minuten)
   - Häufigkeit: 5 (Minuten)
- Name der Warnungsregel: Daily data limit reached
- Schweregrad: Warnung (Schweregrad 1)

Nachdem die Warnung definiert wurde, wird bei Erreichen des Limits eine Warnung ausgelöst und die in der Aktionsgruppe definierte Reaktion ausgeführt. Dadurch kann Ihr Team per E-Mail und SMS benachrichtigt werden, und es können Aktionen mithilfe von Webhooks oder Automation-Runbooks oder mittels [Integration in eine externe ITSM-Lösung](itsmc-overview.md#create-itsm-work-items-from-azure-alerts) automatisiert werden. 

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
union withsource = tt * 
| where TimeGenerated > ago(24h)
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| where computerName != ""
| summarize nodes = dcount(computerName)
```

Eine Liste der Knoten, die Daten senden (und die Menge der von ihnen gesendeten Daten) kann mit der folgenden Abfrage abgerufen werden:

```kusto
union withsource = tt * 
| where TimeGenerated > ago(24h)
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| where computerName != ""
| summarize TotalVolumeBytes=sum(_BilledSize) by computerName
```

> [!TIP]
> Verwenden Sie diese `union *`-Abfragen mit Bedacht, da umfassende Scans verschiedener Datentypen [ressourcenintensiv](https://docs.microsoft.com/azure/azure-monitor/log-query/query-optimization#query-performance-pane) sind. Wenn Sie keine Ergebnisse **pro Computer** benötigen, dann führen Sie Abfragen nach dem Datentyp „Usage“ aus (siehe unten).

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

Beachten Sie, dass durch die Klausel `where IsBillable = true` Datentypen bestimmter Lösungen herausgefiltert werden, für die keine Erfassungsgebühren anfallen. 

### <a name="data-volume-by-solution"></a>Datenvolumen nach Lösung

Die Abfrage, mit der die abrechenbare Datenmenge nach Lösung während des letzten Monats (ohne den letzten angebrochenen Tag) angezeigt wird, lautet wie folgt:

```kusto
Usage 
| where TimeGenerated > ago(32d)
| where StartTime >= startofday(ago(31d)) and EndTime < startofday(now())
| where IsBillable == true
| summarize BillableDataGB = sum(Quantity) / 1000. by bin(StartTime, 1d), Solution | render barchart
```

Mit der Klausel mit `TimeGenerated` wird nur sichergestellt, dass die Abfrage im Azure-Portal über den Standardwert von 24 Stunden hinausgeht. Bei Verwendung des Datentyps „Usage“ stellen `StartTime` und `EndTime` die Zeitrahmen dar, für die Ergebnisse angezeigt werden. 

### <a name="data-volume-by-type"></a>Datenmenge nach Typ

Sie können die Daten genauer untersuchen, um Trends für die einzelnen Datentypen zu erkennen:

```kusto
Usage 
| where TimeGenerated > ago(32d)
| where StartTime >= startofday(ago(31d)) and EndTime < startofday(now())
| where IsBillable == true
| summarize BillableDataGB = sum(Quantity) / 1000. by bin(StartTime, 1d), DataType | render barchart
```

Oder verwenden Sie Folgendes, um eine Tabelle nach Lösung und Typ für den letzten Monat anzuzeigen:

```kusto
Usage 
| where TimeGenerated > ago(32d)
| where StartTime >= startofday(ago(31d)) and EndTime < startofday(now())
| where IsBillable == true
| summarize BillableDataGB = sum(Quantity) by Solution, DataType
| sort by Solution asc, DataType asc
```

### <a name="data-volume-by-computer"></a>Datenmenge nach Computer

Der Datentyp `Usage` enthält keine Informationen auf Computerebene. Um die **Größe** der pro Computer erfassten Daten anzuzeigen, verwenden Sie die `_BilledSize`-[Eigenschaft](log-standard-properties.md#_billedsize), die die Größe in Bytes bereitstellt:

```kusto
union withsource = tt * 
| where TimeGenerated > ago(24h)
| where _IsBillable == true 
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| summarize BillableDataBytes = sum(_BilledSize) by  computerName | sort by Bytes nulls last
```

Die `_IsBillable`-[Eigenschaft](log-standard-properties.md#_isbillable) gibt an, ob für die erfassten Daten Gebühren anfallen. 

Um die Anzahl von Ereignissen pro Computer anzuzeigen, für die **Gebühren** anfallen, verwenden Sie diese Abfrage: 

```kusto
union withsource = tt * 
| where TimeGenerated > ago(24h)
| where _IsBillable == true 
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| summarize eventCount = count() by computerName  | sort by eventCount nulls last
```

> [!TIP]
> Verwenden Sie diese `union  *`-Abfragen mit Bedacht, da umfassende Scans verschiedener Datentypen [ressourcenintensiv](https://docs.microsoft.com/azure/azure-monitor/log-query/query-optimization#query-performance-pane) sind. Wenn Sie keine Ergebnisse **pro Computer** benötigen, dann führen Sie Abfragen nach dem Datentyp „Usage“ aus.

### <a name="data-volume-by-azure-resource-resource-group-or-subscription"></a>Datenmenge nach Azure-Ressource, Ressourcengruppe oder Abonnement

Für Daten von in Azure gehosteten Knoten können Sie die **Größe** der erfassten Daten __pro Computer__ mit der [Eigenschaft](log-standard-properties.md#_resourceid) „_ResourceId“ abrufen, die den vollständigen Pfad zur Ressource enthält:

```kusto
union withsource = tt * 
| where TimeGenerated > ago(24h)
| where _IsBillable == true 
| summarize BillableDataBytes = sum(_BilledSize) by _ResourceId | sort by Bytes nulls last
```

Für Daten von in Azure gehosteten Knoten können Sie die **Größe** der erfassten Daten __pro Azure-Abonnement__ durch Analysieren der Eigenschaft `_ResourceId` wie folgt abrufen:

```kusto
union withsource = tt * 
| where TimeGenerated > ago(24h)
| where _IsBillable == true 
| parse tolower(_ResourceId) with "/subscriptions/" subscriptionId "/resourcegroups/" 
    resourceGroup "/providers/" provider "/" resourceType "/" resourceName   
| summarize BillableDataBytes = sum(_BilledSize) by subscriptionId | sort by Bytes nulls last
```

Durch Ändern von `subscriptionId` in `resourceGroup` wird die abrechenbare erfasste Datenmenge pro Azure-Ressourcengruppe angezeigt. 

> [!TIP]
> Verwenden Sie diese `union  *`-Abfragen mit Bedacht, da umfassende Scans verschiedener Datentypen [ressourcenintensiv](https://docs.microsoft.com/azure/azure-monitor/log-query/query-optimization#query-performance-pane) sind. Wenn Sie keine Ergebnisse pro Abonnement, Ressourcengruppe oder Ressourcenname benötigen, dann führen Sie Abfragen nach dem Datentyp „Usage“ aus.

> [!WARNING]
> Einige der Felder vom Typ „Nutzungsdaten“ sind zwar weiterhin im Schema enthalten, jedoch veraltet, und ihre Werte werden nicht mehr aufgefüllt. Dies sind neben **Computer** Felder in Bezug auf die Erfassung: **TotalBatches**, **BatchesWithinSla**, **BatchesOutsideSla**, **BatchesCapped** und **AverageProcessingTimeMs**.


### <a name="querying-for-common-data-types"></a>Abfragen von häufig verwendeten Datentypen

Wenn Sie die Datenquelle für einen bestimmten Datentyp näher untersuchen möchten, finden Sie hier einige nützliche Beispielabfragen:

+ **Arbeitsbereichsbasierte Application Insights**-Ressourcen
  - Weitere Informationen finden Sie [hier](https://docs.microsoft.com/azure/azure-monitor/app/pricing#data-volume-for-workspace-based-application-insights-resources).
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
| Container Insights         | [Konfigurieren Sie Container Insights](https://docs.microsoft.com/azure/azure-monitor/insights/container-insights-cost#controlling-ingestion-to-reduce-cost), um nur die Daten zu erfassen, die Sie benötigen. |
| Sicherheitsereignisse            | Wählen Sie [Sicherheitsereignisse vom Typ „Allgemein“ oder „Minimal“](https://docs.microsoft.com/azure/security-center/security-center-enable-data-collection#data-collection-tier) aus. <br> Ändern der Sicherheitsüberwachungsrichtlinie, sodass nur benötigte Ereignisse erfasst werden. Überprüfen Sie insbesondere die Notwendigkeit zum Erfassen von Ereignissen für die <br> - [Überwachung der Filterplattform](https://technet.microsoft.com/library/dd772749(WS.10).aspx) <br> - [Überwachung der Registrierung](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd941614(v%3dws.10))<br> - [Überwachung des Dateisystems](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd772661(v%3dws.10))<br> - [Überwachung des Kernelobjekts](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd941615(v%3dws.10))<br> - [Überwachung der Handleänderung](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd772626(v%3dws.10))<br> - Überwachung von Wechselmedien |
| Leistungsindikatoren       | Ändern Sie [Leistungsindikatoren-Konfiguration](data-sources-performance-counters.md) in: <br> - Reduzieren der Sammlungshäufigkeit <br> - Reduzieren der Anzahl von Leistungsindikatoren |
| Ereignisprotokolle                 | Ändern Sie die [Ereignisprotokollkonfiguration](data-sources-windows-events.md) in: <br> - Reduzieren der Anzahl von erfassten Ereignisprotokollen <br> - Ausschließliches Erfassen von erforderlichen Ereignisebenen. Erfassen Sie beispielsweise keine Ereignisse der Ebene *Informationen*. |
| syslog                     | Ändern Sie die [syslog-Konfiguration](data-sources-syslog.md) in: <br> - Reduzieren der Anzahl von erfassten Einrichtungen <br> - Ausschließliches Erfassen von erforderlichen Ereignisebenen. Erfassen Sie beispielsweise keine Ereignisse der Ebenen *Informationen* und *Debuggen*. |
| AzureDiagnostics           | Ändern Sie die Ressourcenprotokollsammlung, um Folgendes zu erreichen: <br> - Verringern der Anzahl von Ressourcen, die Protokolle an Log Analytics senden <br> - Ausschließliches Erfassen von erforderlichen Protokollen |
| Lösungsdaten von Computern, für die die Lösung nicht erforderlich ist | Verwenden Sie die [Zielgruppenadressierung für Lösungen](../insights/solution-targeting.md), um Daten nur für erforderliche Gruppen mit Computern zu erfassen. |

### <a name="getting-nodes-as-billed-in-the-per-node-pricing-tier"></a>Abrufen von Knoten gemäß der Abrechnung im Tarif „Pro Knoten“

Um eine Liste von Computern abzurufen, die als Knoten berechnet werden, wenn für den Arbeitsbereich der Legacytarif „Pro Knoten“ gilt, suchen Sie Knoten, die **kostenpflichtige Datentypen** senden (einige Datentypen sind kostenlos). Verwenden Sie dazu die `_IsBillable`-[Eigenschaft](log-standard-properties.md#_isbillable) und das linke Feld des vollständig qualifizierten Domänennamens. Dies gibt die Anzahl der Computer mit abgerechneten Daten pro Stunde zurück (was der Granularität entspricht, mit der Knoten gezählt und abgerechnet werden):

```kusto
union withsource = tt * 
| where _IsBillable == true 
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| where computerName != ""
| summarize billableNodes=dcount(computerName) by bin(TimeGenerated, 1h) | sort by TimeGenerated asc
```

### <a name="getting-security-and-automation-node-counts"></a>Abrufen der Knotenanzahl für Sicherheit und Automatisierung

Wenn Sie den Tarif „Pro Knoten (OMS)“ nutzen, erfolgt die Abrechnung basierend auf der Anzahl von verwendeten Knoten und Lösungen. Die Anzahl von Insights- und Analytics-Knoten, für die Sie Gebühren entrichten, wird in der Tabelle auf der Seite **Nutzung und geschätzte Kosten** angezeigt.  

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

1. ob der Arbeitsbereich Azure Security Center verwendet, indem Sie `workspaceHasSecurityCenter` auf `true` oder `false` festlegen, 
2. die Preise aktualisieren, wenn Sie über bestimmte Rabatte verfügen, und
3. die Anzahl der Tage angeben, die rückwirkend ermittelt und analysiert werden sollen, indem Sie `daysToEvaluate` festlegen. Dies ist hilfreich, wenn bei Berücksichtigung der Daten von sieben Tagen die Abfrage zu lange dauert. 

Hier sehen Sie die Abfrage für Tarifempfehlungen:

```kusto
// Set these parameters before running query
let workspaceHasSecurityCenter = true;  // Specify if the workspace has Azure Security Center
let PerNodePrice = 15.; // Enter your montly price per monitored nodes
let PerNodeOveragePrice = 2.30; // Enter your price per GB for data overage in the Per Node pricing tier
let PerGBPrice = 2.30; // Enter your price per GB in the Pay-as-you-go pricing tier
let daysToEvaluate = 7; // Enter number of previous days look at (reduce if the query is taking too long)
// ---------------------------------------
let SecurityDataTypes=dynamic(["SecurityAlert", "SecurityBaseline", "SecurityBaselineSummary", "SecurityDetection", "SecurityEvent", "WindowsFirewall", "MaliciousIPCommunication", "LinuxAuditLog", "SysmonEvent", "ProtectionStatus", "WindowsEvent", "Update", "UpdateSummary"]);
let StartDate = startofday(datetime_add("Day",-1*daysToEvaluate,now()));
let EndDate = startofday(now());
union withsource = tt * 
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
| extend PerGBDailyCost = iff(workspaceHasSecurityCenter,
             (NonSecurityDataGB + max_of(SecurityDataGB - 0.5*ASCnodesPerDay, 0.)) * PerGBPrice,
             DataGB * PerGBPrice)
| extend OverageGB = iff(workspaceHasSecurityCenter, 
             max_of(DataGB - 0.5*nodesPerDay - 0.5*ASCnodesPerDay, 0.), 
             max_of(DataGB - 0.5*nodesPerDay, 0.))
| extend PerNodeDailyCost = nodesPerDay * PerNodePrice / 31. + OverageGB * PerNodeOveragePrice
| extend Recommendation = iff(PerNodeDailyCost < PerGBDailyCost, "Per Node tier", 
             iff(NonSecurityDataGB > 85., "Capacity Reservation tier", "Pay-as-you-go (Per GB) tier"))
| project day, nodesPerDay, ASCnodesPerDay, NonSecurityDataGB, SecurityDataGB, OverageGB, AvgGbPerNode, PerGBDailyCost, PerNodeDailyCost, Recommendation | sort by day asc
//| project day, Recommendation // Comment this line to see details
| sort by day asc
```

Diese Abfrage spiegelt nicht exakt wider, wie der Verbrauch berechnet wird, ist aber in den meisten Fällen ausreichend, um Empfehlungen für einen Tarif bereitzustellen.  

> [!NOTE]
> Wenn Sie die Berechtigungen nutzen möchten, die Sie durch den Kauf der OMS E1-Suite, OMS E2-Suite oder des OMS-Add-Ons für System Center erwerben, wählen Sie den Tarif *Pro Knoten* für Log Analytics aus.

## <a name="create-an-alert-when-data-collection-is-high"></a>Erstellen einer Warnung für den Fall, dass die Datensammlung hoch ist

In diesem Abschnitt wird beschrieben, wie Sie eine Warnung erstellen, wenn Folgendes gilt:
- Das Datenvolumen übersteigt eine angegebene Menge.
- Für das Datenvolumen besteht die Vorhersage, dass eine bestimmte Menge überschritten wird.

Azure-Warnungen unterstützen [Protokollwarnungen](alerts-unified-log.md), die Suchabfragen nutzen. 

Für die folgende Abfrage wird ein Ergebnis erzielt, wenn innerhalb der letzten 24 Stunden mehr als 100 GB an Daten gesammelt wurden:

```kusto
union withsource = $table Usage 
| where QuantityUnit == "MBytes" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true 
| extend Type = $table | summarize DataGB = sum((Quantity / 1000.)) by Type 
| where DataGB > 100
```

Für die folgende Abfrage wird eine einfache Formel verwendet, um vorherzusagen, wenn an einem Tag mehr als 100 GB an Daten gesendet werden: 

```kusto
union withsource = $table Usage 
| where QuantityUnit == "MBytes" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true 
| extend Type = $table 
| summarize EstimatedGB = sum(((Quantity * 8) / 1000.)) by Type 
| where EstimatedGB > 100
```

Wenn die Warnung für ein anderes Datenvolumen gelten soll, ändern Sie den Wert 100 in den Abfragen einfach in den gewünschten GB-Wert.

Führen Sie die Schritte unter [Erstellen, Anzeigen und Verwalten von Warnungen mithilfe von Azure Monitor](alerts-metric.md) aus, um eine Benachrichtigung zu erhalten, wenn die Datensammlung höher als erwartet ausfällt.

Legen Sie beim Erstellen der Warnung für die erste Abfrage Folgendes fest, wenn mehr als 100 GB an Daten innerhalb von 24 Stunden anfallen:  

- **Definieren der Warnungsbedingung**: Festlegen Ihres Log Analytics-Arbeitsbereichs als Ressourcenziel
- **Warnungskriterien**:
   - **Signalname**: **Benutzerdefinierte Protokollsuche**
   - **Suchabfrage** auf `union withsource = $table Usage | where QuantityUnit == "MBytes" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true | extend Type = $table | summarize DataGB = sum((Quantity / 1000.)) by Type | where DataGB > 100`
   - **Warnungslogik**: **Basiert auf** *Anzahl von Ergebnissen* und **Bedingung** ist *Größer als* ein **Schwellenwert** von *0*
   - **Zeitraum**: *1440* Minuten, **Warnungshäufigkeit**: alle *60* Minuten, da die Nutzungsdaten nur einmal pro Stunde aktualisiert werden
- **Definieren der Warnungsdetails**:
   - **Name** auf *Datenvolumen größer als 100 GB in 24 Stunden*
   - **Schweregrad** auf *Warnung*

Geben Sie eine vorhandene [Aktionsgruppe](action-groups.md) an, oder erstellen Sie eine neue, damit Sie benachrichtigt werden, wenn die Protokollwarnung Kriterien erfüllt.

Legen Sie beim Erstellen der Warnung für die zweite Abfrage Folgendes fest, wenn die Vorhersage besteht, dass innerhalb von 24 Stunden mehr als 100 GB an Daten anfallen:

- **Definieren der Warnungsbedingung**: Festlegen Ihres Log Analytics-Arbeitsbereichs als Ressourcenziel
- **Warnungskriterien**:
   - **Signalname**: **Benutzerdefinierte Protokollsuche**
   - **Suchabfrage** auf `union withsource = $table Usage | where QuantityUnit == "MBytes" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true | extend Type = $table | summarize EstimatedGB = sum(((Quantity * 8) / 1000.)) by Type | where EstimatedGB > 100`
   - **Warnungslogik**: **Basiert auf** *Anzahl von Ergebnissen* und **Bedingung** ist *Größer als* ein **Schwellenwert** von *0*
   - **Zeitraum**: *180* Minuten, **Warnungshäufigkeit**: alle *60* Minuten, da die Nutzungsdaten nur einmal pro Stunde aktualisiert werden
- **Definieren der Warnungsdetails**:
   - **Name** auf *Erwartetes Datenvolumen von mehr als 100 GB in 24 Stunden*
   - **Schweregrad** auf *Warnung*

Geben Sie eine vorhandene [Aktionsgruppe](action-groups.md) an, oder erstellen Sie eine neue, damit Sie benachrichtigt werden, wenn die Protokollwarnung Kriterien erfüllt.

Wenn Sie eine Warnung erhalten, können Sie die Schritte im folgenden Abschnitt verwenden, um per Problembehandlung zu ermitteln, warum die Nutzung höher als erwartet ist.

## <a name="data-transfer-charges-using-log-analytics"></a>Gebühren für die Datenübertragung mit Log Analytics

Das Senden von Daten an Log Analytics kann Gebühren für die Bandbreitennutzung nach sich ziehen. Wie [auf der Seite Azure-Bandbreitenpreise](https://azure.microsoft.com/pricing/details/bandwidth/) beschrieben, wird die Datenübertragung zwischen Azure-Diensten in zwei Regionen mit dem normalen Preis als ausgehende Datenübertragung abgerechnet. Eingehende Datenübertragungen sind kostenlos. Diese Gebühr ist jedoch sehr niedrig (wenige %) im Vergleich zu den Kosten für die Log Analytics-Datenerfassung. Folglich muss sich die Kontrolle von Kosten für Log Analytics auf Ihr erfasstes Datenvolumen konzentrieren. Anleitungen zu einem besseren Verständnis davon, finden Sie [hier](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#understanding-ingested-data-volume).   


## <a name="troubleshooting-why-log-analytics-is-no-longer-collecting-data"></a>Beheben des Problems, dass Log Analytics keine Daten mehr erfasst

Wenn Sie den kostenlosen Legacytarif nutzen und an einem Tag mehr als 500MB Daten gesendet haben, wird die Datensammlung für den Rest des Tages beendet. Das Erreichen des Tageslimits ist häufig die Ursache dafür, dass Log Analytics die Datensammlung beendet oder Daten scheinbar fehlen.  Log Analytics erstellt ein Ereignis vom Typ „Operation“, wenn die Datensammlung beginnt und endet. Führen Sie die folgende Abfrage in der Suche aus, um zu überprüfen, ob Sie das Tageslimit erreichen und Daten fehlen: 

```kusto
Operation | where OperationCategory == 'Data Collection Status'
```

Wenn die Datensammlung beendet wird, hat „OperationStatus“ den Wert **Warning** (Warnung). Wenn die Datensammlung beginnt, hat „OperationStatus“ den Wert **Succeeded** (Erfolgreich). Die folgende Tabelle beschreibt die Gründe, warum die Datensammlung endet, und eine empfohlene Aktion zum Fortsetzen der Datensammlung:  

|Grund für die Beendigung der Datensammlung| Lösung| 
|-----------------------|---------|
|Tageslimit oder kostenloser Legacytarif erreicht |Warten Sie, bis die Datensammlung am Folgetag automatisch neu gestartet wird, oder wechseln Sie zu einem kostenpflichtigen Tarif.|
|Tägliche Obergrenze des Arbeitsbereichs wurde erreicht|Warten Sie, bis die Datensammlung am Folgetag automatisch neu gestartet wird, oder erhöhen Sie das Tageslimit für das Datenvolumen, wie unter „Verwalten des maximalen täglichen Datenvolumens“ beschrieben. Der Zeitpunkt für das Zurücksetzen der täglichen Obergrenze wird auf der Seite **Datenmengenverwaltung** angezeigt. |
|Das Azure-Abonnement befindet sich aus folgendem Grund in einem angehaltenen Zustand:<br> Kostenlose Testversion endete<br> Azure Pass ist abgelaufen<br> Monatliches Ausgabenlimit ist erreicht (z.B. in einem MSDN- oder Visual Studio-Abonnement)|Konvertieren in ein kostenpflichtiges Abonnement<br> Limit entfernen oder warten, bis das Limit zurückgesetzt wird|

Führen Sie die Schritte unter *Erstellen einer täglichen Datenobergrenze* aus, um eine Benachrichtigung zu erhalten, wenn die Datensammlung beendet wird. Führen Sie die Schritte aus, die unter [Erstellen einer Aktionsgruppe](action-groups.md) beschrieben sind, um eine E-Mail-, Webhook- oder Runbookaktion für die Warnungsregel zu konfigurieren. 

## <a name="limits-summary"></a>Zusammenfassung der Grenzwerte

Es gibt einige zusätzliche Log Analytics-Grenzwerte, von denen einige vom Log Analytics-Tarif abhängen. Diese sind [hier](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#log-analytics-workspaces) dokumentiert.


## <a name="next-steps"></a>Nächste Schritte

- Informationen dazu, wie Sie die Suchsprache verwenden, finden Sie unter [Protokollsuchen in Azure Monitor-Protokollen](../log-query/log-query-overview.md). Sie können Suchabfragen verwenden, um für die Nutzungsdaten eine zusätzliche Analyse durchzuführen.
- Führen Sie die unter [Erstellen, Anzeigen und Verwalten von Warnungen mithilfe von Azure Monitor](alerts-metric.md) beschriebenen Schritte aus, um benachrichtigt zu werden, wenn ein Suchkriterium erfüllt ist.
- Verwenden Sie die [Zielgruppenadressierung für Lösungen](../insights/solution-targeting.md), um Daten nur für erforderliche Gruppen mit Computern zu erfassen.
- Lesen Sie zum Konfigurieren einer effektiven Richtlinie zur Erfassung von Ereignissen die Informationen unter [Datensammlung in Azure Security Center](../../security-center/security-center-enable-data-collection.md).
- Ändern Sie die [Leistungsindikatorenkonfiguration](data-sources-performance-counters.md).
- Informationen zum Ändern der Einstellungen für die Ereigniserfassung finden Sie unter [Datenquellen für Windows-Ereignisprotokolle in Log Analytics](data-sources-windows-events.md).
- Informationen zum Ändern der Einstellungen für die Syslog-Sammlung finden Sie unter [Syslog-Datenquellen in Log Analytics](data-sources-syslog.md).