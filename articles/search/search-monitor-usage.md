---
title: Überwachen von Vorgängen und Aktivitäten
titleSuffix: Azure Cognitive Search
description: Aktivieren der Protokollierung, Abrufen von Abfrageaktivitätsmetriken, der Ressourcennutzung und anderer Daten von einem Dienst der kognitiven Azure-Suche.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 02/15/2020
ms.openlocfilehash: 353e00f902a7314e5e5b7c8ee03e8b925a510b26
ms.sourcegitcommit: 6ee876c800da7a14464d276cd726a49b504c45c5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2020
ms.locfileid: "77462325"
---
# <a name="monitor-operations-and-activity-of-azure-cognitive-search"></a>Überwachen von Vorgängen und Aktivitäten von Azure Cognitive Search

In diesem Artikel wird die Überwachung auf Dienstebene (Ressource) und auf Workloadebene (Abfragen und Indizierung) vorgestellt, und es wird ein Framework zum Überwachen des Benutzerzugriffs vorgeschlagen.

Dabei verwenden Sie stets eine Kombination aus integrierter Infrastruktur und grundlegenden Diensten wie Azure Monitor sowie Dienst-APIs, die Statistiken, die Anzahl und den Status zurückgeben. Kenntnisse dieses Funktionsbereichs können Ihnen helfen, eine Feedbackschleife einzurichten, mit der Sie Probleme beheben können, sobald sie auftreten.

## <a name="use-azure-monitor"></a>Verwenden von Azure Monitor

Viele Dienste, einschließlich Azure Cognitive Search, nutzen [Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/) für Warnungen, Metriken und die Protokollierung von Diagnosedaten. Bei Azure Cognitive Search wird die integrierte Überwachungsinfrastruktur hauptsächlich für die Überwachung auf Ressourcenebene (Dienstintegrität) und die [Abfrageüberwachung](search-monitor-queries.md) genutzt.

Der folgende Screenshot hilft Ihnen, Azure Monitor-Funktionen im Portal zu finden.

+ Auf der Registerkarte **Überwachung** der Hauptübersicht werden wichtige Metriken auf einen Blick angezeigt.
+ Das **Aktivitätsprotokoll** direkt unter der Übersicht enthält Informationen zu Aktionen auf Ressourcenebene: Dienstintegrität und wichtige Benachrichtigungen zu API-Anforderungen.
+ Unter **Überwachung** weiter unten in der Liste finden Sie konfigurierbare Warnungen, Metriken und Diagnoseprotokolle. Erstellen Sie diese, wenn Sie sie benötigen. Nachdem die Daten gesammelt und gespeichert wurden, können Sie sie abfragen oder visualisieren, um Erkenntnisse zu erhalten.

![Azure Monitor-Integration mit einem Suchdienst](./media/search-monitor-usage/azure-monitor-search.png
 "Azure Monitor-Integration mit einem Suchdienst")

### <a name="precision-of-reported-numbers"></a>Genauigkeit der gemeldeten Werte

Portalseiten werden alle paar Minuten aktualisiert. Daher sind die im Portal angegebenen Zahlen ungefähre Werte, die Ihnen eine allgemeine Übersicht darüber geben sollen, wie gut Anforderungen in Ihrem System verarbeitet werden. Die tatsächlichen Metriken, z. B. die Abfragen pro Sekunde (QPS), können höher oder niedriger als die auf dieser Seite angezeigte Zahl sein.

## <a name="activity-logs-and-service-health"></a>Aktivitätsprotokolle und Dienstintegrität

Im [**Aktivitätsprotokoll**](https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view) werden Informationen von Azure Resource Manager und Änderungen an der Dienstintegrität erfasst. Sie können das Aktivitätsprotokoll auf kritische, Fehler- und Warnungsbedingungen im Zusammenhang mit der Dienstintegrität überwachen.

Für dienstinterne Aufgaben, wie z. B. Abfragen, Indizierung oder Objekterstellung, werden generische Informationsbenachrichtigungen wie *Get Admin Key* (Administratorschlüssel abrufen) oder *Get Query keys* (Abfrageschlüssel abrufen) für jede Anforderung angezeigt – jedoch nicht die jeweilige Aktion selbst. Um weitere Informationen auf dieser Ebene zu erhalten, müssen Sie die Diagnoseprotokollierung konfigurieren.

Auf das **Aktivitätsprotokoll** können Sie im linken Navigationsbereich, über „Benachrichtigungen“ auf der Befehlsleiste oben im Fenster oder über die Seite **Probleme diagnostizieren und beheben** zugreifen.

## <a name="monitor-storage"></a>Überwachen des Speichers

Auf den Registerkartenseiten der Übersicht werden Informationen zur Ressourcenverwendung angezeigt. Diese Informationen stehen zur Verfügung, sobald Sie den Dienst verwenden, ohne dass eine Konfiguration erforderlich ist. Die Seite wird alle paar Minuten aktualisiert. 

Wenn Sie Entscheidungen dazu treffen möchten, [welcher Tarif für Produktionsworkloads verwendet werden soll](search-sku-tier.md) oder ob [die Anzahl der aktiven Replikate und Partitionen angepasst wird](search-capacity-planning.md), können Sie diese Metriken heranziehen. Anhand der Metriken können Sie sehen, wie schnell Ressourcen verbraucht werden und wie die vorhandene Last in der aktuellen Konfiguration verarbeitet wird.

Warnungen im Zusammenhang mit dem Speicher sind derzeit nicht verfügbar. Der Speicherverbrauch wird nicht aggregiert oder in der Tabelle **AzureMetrics** in Azure Monitor protokolliert. Sie müssen dazu [eine benutzerdefinierte Lösung erstellen](https://docs.microsoft.com/azure/azure-monitor/insights/solutions-creating), die ressourcenbezogene Benachrichtigungen ausgibt und in der Ihr Code die Speichergröße überprüft und die Antwort verarbeitet. Weitere Informationen zu Speichermetriken finden Sie unter [Abrufen von Dienststatistiken](https://docs.microsoft.com/rest/api/searchservice/get-service-statistics#response).

Für die visuelle Überwachung im Portal wird auf der Registerkarte **Verwendung** die Ressourcenverfügbarkeit relativ zu den aktuellen [Limits](search-limits-quotas-capacity.md) angezeigt, die durch die Dienstebene vorgegeben sind. 

Die folgende Abbildung bezieht sich auf den kostenlosen Dienst, der auf 3 Objekte pro Typ und auf 50 MB Speicher begrenzt ist. Für einen Dienst mit dem Tarif „Basic“ oder „Standard“ gelten höhere Grenzwerte. Wenn Sie die Anzahl der Partitionen erhöhen, steigt der maximale Speicher proportional an.

![Verwendungsstatus relativ zu den Tarifgrenzwerten](./media/search-monitor-usage/usage-tab.png
 "Verwendungsstatus relativ zu den Tarifgrenzwerten")

## <a name="monitor-workloads"></a>Überwachen von Workloads

Es werden auch Ereignisse im Zusammenhang mit Indizierung und Abfragen protokolliert. In der Tabelle **AzureDiagnostics** in Log Analytics werden operative Daten im Zusammenhang mit Abfragen und Indizierung gesammelt.

Die meisten der protokollierten Daten sind für schreibgeschützte Vorgänge vorgesehen. Bei anderen Erstellungs-/Aktualisierungs-/Löschvorgängen, die nicht im Protokoll erfasst werden, können Sie den Suchdienst nach Systeminformationen abfragen.

| Vorgangsname | BESCHREIBUNG |
|---------------|-------------|
| ServiceStats | Bei diesem Vorgang handelt es sich um einen Routineaufruf zum [Abrufen von Dienststatistiken](https://docs.microsoft.com/rest/api/searchservice/get-service-statistics), der entweder direkt oder implizit erfolgt, um eine Übersicht im Portal aufzufüllen, wenn diese geladen oder aktualisiert wird. |
| Query.Search |  Abfrageanforderungen für einen Index. Weitere Informationen zu protokollierten Abfragen finden Sie unter [Überwachen von Abfragen](search-monitor-queries.md).|
| Indexing.Index  | Dieser Vorgang dient dem [Hinzufügen, Aktualisieren oder Löschen von Dokumenten](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents). |
| indexes.Prototype | Dies ist ein Index, der vom Datenimport-Assistenten erstellt wird. |
| Indexers.Create | Erstellt explizit oder implizit einen Indexer mithilfe des Datenimport-Assistenten. |
| Indexers.Get | Gibt jeweils den Namen eines Indexers zurück, wenn der Indexer ausgeführt wird. |
| Indexers.Status | Gibt jeweils den Status eines Indexers zurück, wenn der Indexer ausgeführt wird. |
| DataSources.Get | Gibt den Namen der Datenquelle zurück, wenn ein Indexer ausgeführt wird.|
| Indexes.Get | Gibt den Namen eines Indexes zurück, wenn ein Indexer ausgeführt wird. |

### <a name="kusto-queries-about-workloads"></a>Kusto-Abfragen zu Workloads

Wenn Sie die Protokollierung aktiviert haben, können Sie **AzureDiagnostics** abfragen, um eine Liste der für Ihren Dienst ausgeführten Vorgänge einschließlich der Ausführungszeit zu erhalten. Sie können Aktivitäten auch korrelieren, um Änderungen an der Leistung zu untersuchen.

#### <a name="example-list-operations"></a>Beispiel: Auflisten von Vorgängen 

So geben Sie eine Liste der Vorgänge und die Anzahl dieser Vorgänge zurück

```
AzureDiagnostics
| summarize count() by OperationName
```

#### <a name="example-correlate-operations"></a>Beispiel: Korrelieren von Vorgängen

So korrelieren Sie die Abfrageanforderung mit Indizierungsvorgängen und rendern die Datenpunkte in einem Zeitdiagramm, um die Vorgänge abzugleichen

```
AzureDiagnostics
| summarize OperationName, Count=count()
| where OperationName in ('Query.Search', 'Indexing.Index')
| summarize Count=count(), AvgLatency=avg(DurationMs) by bin(TimeGenerated, 1h), OperationName
| render timechart
```

### <a name="use-search-apis"></a>Verwenden der Such-APIs

Sowohl die REST-API der kognitiven Azure-Suche als auch das .NET SDK bieten programmgesteuerten Zugriff auf Dienstmetriken, Index- und Indexerinformationen und die Anzahl von Dokumenten.

+ [Abrufen von Dienststatistiken](/rest/api/searchservice/get-service-statistics)
+ [Abrufen von Indexstatistiken](/rest/api/searchservice/get-index-statistics)
+ [Abrufen der Dokumentanzahl](/rest/api/searchservice/count-documents)
+ [Abrufen des Indexerstatus](/rest/api/searchservice/get-indexer-status)

## <a name="monitor-user-access"></a>Überwachen des Benutzerzugriffs

Da Suchindizes eine Komponente einer größeren Clientanwendung sind, gibt es keine integrierte Methode, um den Zugriff einzelner Benutzer auf einen Index zu steuern oder zu überwachen. Es wird davon ausgegangen, dass Anforderungen von einer Clientanwendung von Administrator- oder von Abfrageanforderungen stammen. Lese-/Schreibvorgänge von Administratoren umfassen das Erstellen, Aktualisieren und Löschen von Objekten für den gesamten Dienst. Schreibgeschützte Vorgänge sind Abfragen der Dokumentensammlung, die auf einen einzelnen Index bezogen sind. 

Daher werden in den Aktivitätsprotokollen Verweise auf Aufrufe mithilfe von Administrator- oder Abfrageschlüsseln angezeigt. Der zugehörige Schlüssel ist in Anforderungen enthalten, die aus Clientcode stammen. Der Dienst ist nicht für die Verarbeitung von Identitätstoken oder Identitätswechseln ausgelegt.

Wenn die Geschäftsanforderungen eine benutzerspezifische Autorisierung erfordern, wird die Integration mit Azure Active Directory empfohlen. Sie können $filter und Benutzeridentitäten verwenden, um Suchergebnisse von Dokumenten, die einem Benutzer nicht angezeigt werden sollen, zu [kürzen](search-security-trimming-for-azure-search-with-aad.md). 

Diese Informationen können nicht getrennt von der Abfragezeichenfolge protokolliert werden, die den $filter-Parameter enthält. Ausführliche Informationen zur Berichterstellung für Abfragezeichenfolgen finden Sie unter [Überwachen von Abfragen](search-monitor-queries.md).

## <a name="next-steps"></a>Nächste Schritte

Kenntnisse von Azure Monitor sind für die Überwachung aller Azure-Dienste, einschließlich Ressourcen wie Azure Cognitive Search, von entscheidender Bedeutung. Wenn Sie mit Azure Monitor nicht vertraut sind, nehmen Sie sich die Zeit, die Artikel zu den Ressourcen zu lesen. Zusätzlich zu den Tutorials stellt der folgende Artikel einen guten Ausgangspunkt dar.

> [!div class="nextstepaction"]
> [Überwachen von Azure-Ressourcen mit Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/insights/monitor-azure-resource)
