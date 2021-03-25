---
title: Problembehandlung für automatische Skalierung in Azure
description: Hier wird das Behandeln von Problemen mit der automatischen Skalierung in Azure erläutert, die in Service Fabric, Virtual Machines, Web-Apps und Clouddiensten verwendet wird.
ms.topic: conceptual
ms.date: 11/4/2019
ms.subservice: autoscale
ms.openlocfilehash: a0fed6c2d06edcb2c9eb8d715feb0ef6c6ade46f
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "101711398"
---
# <a name="troubleshooting-azure-autoscale"></a>Problembehandlung für automatische Skalierung in Azure
 
Mit der automatischen Skalierung von Azure Monitor können Sie jeweils die richtige Menge an Ressourcen ausführen, um die Lasten für Ihre Anwendung zu bewältigen. Sie können Ressourcen hinzufügen, um auf einen Anstieg der Last zu reagieren, und Kosten sparen, indem Sie ungenutzte Ressourcen entfernen. Sie können eine Skalierung basierend auf einem Zeitplan, nach festem Datum/Uhrzeit oder anhand einer ausgewählten Ressourcenmetrik ausführen. Weitere Informationen finden Sie unter [Übersicht über die automatische Skalierung](autoscale-overview.md).

Der Dienst für die automatische Skalierung bietet Metriken und Protokolle, anhand derer Sie ein Verständnis der Skalierungsaktionen und der ausgewerteten ursächlichen Bedingungen für diese Aktionen gewinnen können. Es werden u. a. folgende Fragen beantwortet:

- Warum wurde der Dienst horizontal hoch- oder herunterskaliert?
- Warum wurde mein Dienst nicht skaliert?
- Warum ist eine Aktion der automatischen Skalierung fehlgeschlagen?
- Warum dauert eine Aktion der automatischen Skalierung eine Weile?
  
## <a name="autoscale-metrics"></a>Metriken der automatischen Skalierung

Die automatische Skalierung bietet Ihnen [vier Metriken](../essentials/metrics-supported.md#microsoftinsightsautoscalesettings), anhand derer Sie den Vorgang verstehen können. 

- **Beobachteter Metrikwert**: Der Wert der Metrik, für welche die Skalierungsaktion ausgeführt werden soll, entsprechend der Anzeige oder berechnet durch das Autoskalierungsmodul. Da eine einzelne Autoskalierungseinstellung über mehrere Regeln und damit mehrere Metrikquellen verfügen kann, können Sie nach der Dimension „Metrikquelle“ filtern.
- **Metrikschwellenwert**: Der Schwellenwert, den Sie für die Ausführung der Skalierungsaktion festlegen. Da eine einzelne Autoskalierungseinstellung über mehrere Regeln und damit mehrere Metrikquellen verfügen kann, können Sie nach der Dimension „Metrikregel“ filtern.
- **Beobachtete Kapazität**: Die aktive Anzahl von Instanzen der Zielressource, die vom Autoskalierungsmodul festgestellt wurde.
- **Initiierte Skalierungsaktionen**: Die Anzahl der vom Autoskalierungsmodul initiierten Aktionen zum horizontalen Hoch- und Herunterskalieren. Sie können nach Aktionen zum Aufskalieren und Abskalieren filtern.

Mit dem [Metrik-Explorer](../essentials/metrics-getting-started.md) können Sie die obigen Metriken an einem Ort grafisch darstellen. Das Diagramm sollte Folgendes angezeigt werden:

  - tatsächliche Metrik
  - Metrik entsprechend Anzeige/Berechnung durch das Autoskalierungsmodul
  - Schwellenwert für eine Skalierungsaktion
  - Änderung der Kapazität 

## <a name="example-1---analyzing-a-simple-autoscale-rule"></a>Beispiel 1: Analysieren einer einfachen Regel für die automatische Skalierung 

Es gibt eine einfache Autoskalierungseinstellung für eine VM-Skalierungsgruppe, die Folgendes steuert:

- Horizontales Hochskalieren, wenn der durchschnittliche CPU-Prozentsatz einer Gruppe 10 Minuten lang größer als 70 % ist 
- Horizontales Herunterskalieren, wenn der CPU-Prozentsatz der Gruppe länger als 10 Minuten kleiner als 5 % ist. 

Betrachten wir nun die Metriken aus dem Autoskalierungsdienst.
 
![Screenshot eines Beispiels für den CPU-Prozentsatz einer VM-Skalierungsgruppe](media/autoscale-troubleshoot/autoscale-vmss-CPU-ex-full-1.png)

![Beispiel für CPU-Prozentsatz – VM-Skalierungsgruppe](media/autoscale-troubleshoot/autoscale-vmss-CPU-ex-full-2.png)

***Abbildung 1a: Metrik „CPU in Prozent“ für VM-Skalierungsgruppe und Metrik „Beobachteter Metrikwert“ für Autoskalierungseinstellung***

![„Schwellenwert der Metrik“ und „Beobachtete Kapazität“](media/autoscale-troubleshoot/autoscale-metric-threshold-capacity-ex-full.png)

***Abbildung 1b: „Schwellenwert der Metrik“ und „Beobachtete Kapazität“***

In Abbildung 1b ist der **Schwellenwert der Metrik** (hellblaue Linie) für die Regel für die horizontale Skalierung gleich 70.  Die **Beobachtete Kapazität** (dunkelblaue Linie) zeigt die Anzahl der aktiven Instanzen; derzeit sind drei Instanzen aktiv. 

> [!NOTE]
> Sie müssen den **Schwellenwert der Metrik** anhand der Regel für das Aufskalieren (Vergrößern) der Metriktriggerregel-Dimension filtern, um den Schwellenwert für Aufskalierung anzuzeigen, sowie anhand der Regel für das Abskalieren (Verringern). 

## <a name="example-2---advanced-autoscaling-for-a-virtual-machine-scale-set"></a>Beispiel 2: Erweiterte automatische Skalierung für eine VM-Skalierungsgruppe

Es gibt eine Einstellung, die das Aufskalieren der Ressource einer VM-Skalierungsgruppe anhand ihrer eigenen Metrik **Ausgehende Datenflüsse** ermöglicht. Sie stellen fest, dass die Option **divide metric by instance count** (Metrik durch Anzahl der Instanzen dividieren) für den Metrikschwellenwert aktiviert ist. 

Regel für die Skalierungsaktion: 

Wenn der Wert von **Outbound Flow per instance** (Ausgehender Datenfluss pro Instanz) größer als 10 ist, führt der Autoskalierungsdienst eine Aufskalierung um 1 Instanz durch. 

In diesem Fall wird der beobachtete Metrikwert des Autoskalierungsmoduls berechnet, indem der tatsächliche Metrikwert durch die Anzahl der Instanzen dividiert wird. Wenn der beobachtete Metrikwert kleiner als der Schwellenwert ist, wird keine Aktion zum horizontalen Hochskalieren initiiert. 
 
![Screenshot der Seite mit dem Durchschnittswert für ausgehende Datenflüsse mit einem Beispiel für Diagramme mit Metriken der automatischen Skalierung für die VM-Skalierungsgruppe](media/autoscale-troubleshoot/autoscale-vmss-metric-chart-ex-1.png)

![Beispiel für Diagramme mit Metriken der automatischen Skalierung für VM-Skalierungsgruppe](media/autoscale-troubleshoot/autoscale-vmss-metric-chart-ex-2.png)

***Abbildung 2: Beispiel für Diagramme mit Metriken der automatischen Skalierung für VM-Skalierungsgruppe***

In Abbildung 2 sehen Sie zwei Metrikdiagramme. 

Das Diagramm oben zeigt den tatsächlichen Wert der Metrik **Ausgehende Datenflüsse** an. Der tatsächliche Wert ist 6. 

Im Diagramm unten werden einige Werte dargestellt. 
 - **Beobachteter Metrikwert** (hellblau) ist 3, da zwei aktive Instanzen vorhanden sind und 6 dividiert durch 2 ist gleich 3. 
 - Die **Beobachtete Kapazität** (lila) zeigt die Anzahl der Instanzen, die vom Autoskalierungsmodul festgestellt wurde. 
 - Der **Metrikschwellenwert** (hellgrün) ist auf 10 festgelegt. 

Wenn mehrere Regeln für Skalierungsaktionen vorhanden sind, können Sie die Teilungsoption oder die Option **Filter hinzufügen** im Diagramm des Metrik-Explorers verwenden, um Metriken nach einer bestimmten Quelle oder Regel anzeigen zu lassen. Weitere Informationen zum Teilen eines Metrikdiagramms finden Sie unter [Erweiterte Funktionen von Azure Metrik-Explorer – Teilen](../essentials/metrics-charts.md#apply-splitting).

## <a name="example-3---understanding-autoscale-events"></a>Beispiel 3: Verstehen von Autoskalierungsereignissen

Wechseln Sie im Bildschirm für Autoskalierungseinstellungen zur Registerkarte **Ausführungsverlauf**, um die aktuellen Skalierungsaktionen zu überprüfen. Auf der Registerkarte wird auch die Änderung des Werts von **Beobachtete Kapazität** im Lauf der Zeit angezeigt. Weitere Einzelheiten zu allen Autoskalierungsaktionen, einschließlich von Vorgängen wie Aktualisieren/Löschen von Autoskalierungseinstellungen finden Sie im Aktivitätsprotokoll; filtern Sie dort nach Autoskalierungsvorgängen.

![Ausführungsverlauf für Autoskalierungseinstellungen](media/autoscale-troubleshoot/autoscale-setting-run-history-smaller.png)

## <a name="autoscale-resource-logs"></a>Ressourcenprotokolle für die Autoskalierung

Wie jede andere Azure-Ressource stellt auch der Autoskalierungsdienst [Ressourcenprotokolle](../essentials/platform-logs-overview.md) bereit. Es gibt zwei Kategorien von Protokollen.

- **Bewertungen der Autoskalierung**: Das Autoskalierungsmodul zeichnet bei jeder Überprüfung Protokolleinträge für jede einzelne Bedingungsauswertung auf.  Der Eintrag enthält Details zu den beobachteten Werten der Metriken, zu den ausgewerteten Regeln sowie Angaben dazu, ob die Bewertung zu einer Skalierungsaktion geführt hat.

- **Skalierungsaktionen der Autoskalierung**: Das Modul zeichnet vom Autoskalierungsdienst initiierte Ereignisse von Skalierungsaktionen sowie die Ergebnisse dieser Skalierungsaktionen (Erfolg, Fehler und vom Autoskalierungsdienst beobachteter Umfang der Skalierung) auf.

Wie bei jedem von Azure Monitor unterstützten Dienst können Sie diese Protokolle mit [Diagnoseeinstellungen](../essentials/diagnostic-settings.md) weiterleiten:

- an Ihren Log Analytics-Arbeitsbereich für die ausführliche Analyse
- an Event Hubs und anschließend an Nicht-Azure-Tools
- an Ihr Azure-Speicherkonto zu Archivierungszwecken  

![Diagnoseeinstellungen für Autoskalierung](media/autoscale-troubleshoot/diagnostic-settings.png)

In der vorherigen Abbildung wurden die Diagnoseeinstellungen für die Autoskalierung im Azure-Portal veranschaulicht. Dort können Sie die Registerkarte „Diagnose-/Ressourcenprotokolle“ auswählen und die Protokollaufzeichnung und -weiterleitung aktivieren. Sie können dieselbe Aktion für Diagnoseeinstellungen auch mit der REST-API, CLI, PowerShell und Resource Manager-Vorlagen ausführen, indem Sie als Ressourcentyp *Microsoft.Insights/AutoscaleSettings* auswählen. 

## <a name="troubleshooting-using-autoscale-logs"></a>Problembehandlung mithilfe von Autoskalierungsprotokollen 

Für eine optimale Problembehandlung empfiehlt es sich, die Protokolle über einen Arbeitsbereich an Azure Monitor-Protokolle (Log Analytics) weiterzuleiten, wenn Sie die Autoskalierungseinstellung festlegen. Die Vorgehensweise wird in der Abbildung im vorherigen Abschnitt veranschaulicht. Sie können die Auswertungen und Skalierungsaktionen besser mit Log Analytics überprüfen.

Wenn Sie das Senden Ihrer Autoskalierungsprotokolle an den Log Analytics-Arbeitsbereich konfiguriert haben, können Sie die Protokolle durch Ausführen der folgenden Abfragen überprüfen. 

Führen Sie zuerst diese Abfrage aus, um die aktuellen Protokolle zur Autoskalierungsauswertung aufzurufen:

```Kusto
AutoscaleEvaluationsLog
| limit 50
```

Sie können auch die folgende Abfrage ausführen, um die Protokolle für aktuelle Skalierungsaktionen aufzurufen:

```Kusto
AutoscaleScaleActionsLog
| limit 50
```

In den folgenden Abschnitten finden Sie Antworten auf diese Fragen. 

## <a name="a-scale-action-occurred-that-i-didnt-expect"></a>Es ist eine unerwartete Skalierungsaktion aufgetreten.

Führen Sie zuerst die Abfrage für die Skalierungsaktion aus, um die gewünschte Skalierungsaktion zu ermitteln. Wenn es sich um die letzte Skalierungsaktion handelt, verwenden Sie die folgende Abfrage:

```Kusto
AutoscaleScaleActionsLog
| take 1
```

Wählen Sie im Protokoll der Skalierungsaktionen das Feld „CorrelationId“ aus. Suchen Sie anhand der CorrelationId nach dem betreffenden Auswertungsprotokoll. Durch Ausführen der unten stehenden Abfrage werden sämtliche Regeln und ausgewerteten Bedingungen angezeigt, die zu dieser Skalierungsaktion geführt haben.

```Kusto
AutoscaleEvaluationsLog
| where CorrelationId = "<correliationId>"
```

## <a name="what-profile-caused-a-scale-action"></a>Welches Profil hat eine Skalierungsaktion verursacht?

Es ist eine Skalierungsaktion aufgetreten. Sie verfügen jedoch über einander überschneidende Regeln und Profile und müssen feststellen, welche die Aktion verursacht haben. 

Suchen Sie nach der CorrelationId der Skalierungsaktion (wie in Beispiel 1 erläutert), und führen Sie die Abfrage für Auswertungsprotokolle aus, um mehr über das Profil zu erfahren.

```Kusto
AutoscaleEvaluationsLog
| where CorrelationId = "<correliationId_Guid>"
| where ProfileSelected == true
| project ProfileEvaluationTime, Profile, ProfileSelected, EvaluationResult
```

Mithilfe der folgenden Abfrage können Sie ein besseres Verständnis der gesamten Profilauswertung erlangen.

```Kusto
AutoscaleEvaluationsLog
| where TimeGenerated > ago(2h)
| where OperationName contains == "profileEvaluation"
| project OperationName, Profile, ProfileEvaluationTime, ProfileSelected, EvaluationResult
```

## <a name="a-scale-action-did-not-occur"></a>Es ist keine Skalierungsaktion aufgetreten.

Ich habe eine Skalierungsaktion erwartet, die jedoch nicht stattgefunden hat. Möglicherweise sind keine Ereignisse oder Protokolle für Skalierungsaktionen vorhanden.

Überprüfen Sie die Metriken der automatischen Skalierung, wenn Sie eine metrikbasierte Skalierungsregel verwenden. Möglicherweise entsprechen **Beobachteter Metrikwert** oder **Beobachtete Kapazität** nicht Ihren Erwartungen, und daher wurde die Skalierungsregel nicht ausgelöst. Sie sehen weiterhin Auswertungen, jedoch keine Regel für die horizontale Skalierung. Möglicherweise wurde die Skalierungsaktion auch durch die Abkühldauer verhindert. 

 Überprüfen Sie die Autoskalierungsprotokolle für den Zeitraum, in dem Sie die Skalierungsaktion erwartet haben. Überprüfen Sie alle vorgenommenen Auswertungen, und untersuchen Sie, warum keine Skalierungsaktion ausgelöst wurde.


```Kusto
AutoscaleEvaluationsLog
| where TimeGenerated > ago(2h)
| where OperationName == "MetricEvaluation" or OperationName == "ScaleRuleEvaluation"
| project OperationName, MetricData, ObservedValue, Threshold, EstimateScaleResult
```

## <a name="scale-action-failed"></a>Die Skalierungsaktion ist fehlgeschlagen.

In einigen Fällen hat der Autoskalierungsdienst die Skalierungsaktion möglicherweise initiiert, das System hat jedoch entschieden, die Skalierung nicht auszuführen, oder es konnte die Skalierungsaktion nicht abschließen. Führen diese Abfrage aus, um die fehlgeschlagenen Skalierungsaktionen aufzufinden.

```Kusto
AutoscaleScaleActionsLog
| where ResultType == "Failed"
| project ResultDescription
```

Erstellen Sie Warnungsregeln, um über Autoskalierungsaktionen und auftretende Fehler benachrichtigt zu werden. Sie können auch Warnungsregeln erstellen, um Benachrichtigungen über Autoskalierungsereignisse zu erhalten.

## <a name="schema-of-autoscale-resource-logs"></a>Schema der Ressourcenprotokolle für die Autoskalierung

Weitere Informationen finden Sie unter [Ressourcenprotokolle für die Autoskalierung](autoscale-resource-log-schema.md).

## <a name="next-steps"></a>Nächste Schritte
Informieren Sie sich über [bewährte Methoden der Autoskalierung](autoscale-best-practices.md).