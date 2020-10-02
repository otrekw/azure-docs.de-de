---
title: Protokolle in Azure Monitor | Microsoft-Dokumentation
description: Beschreibt Protokolle in Azure Monitor, die für eine erweiterte Analyse von Überwachungsdaten verwendet werden.
documentationcenter: ''
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.date: 09/09/2020
ms.author: bwren
ms.openlocfilehash: e08c649b9a1d7e8b909a413ee435fce30a8d7e48
ms.sourcegitcommit: 3fc3457b5a6d5773323237f6a06ccfb6955bfb2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2020
ms.locfileid: "90032769"
---
# <a name="azure-monitor-logs-overview"></a>Übersicht über Azure Monitor-Protokolle
Azure Monitor-Protokolle sind ein Feature von Azure Monitor, mit dem Protokolldaten aus einer Vielzahl von Quellen gesammelt, organisiert und mithilfe einer ausgereiften Abfragesprache für die Analyse verfügbar gemacht werden. Daten aus unterschiedlichen Quellen können in einem einzigen Arbeitsbereich konsolidiert und gemeinsam analysiert werden, um solche Aufgaben wie Trendanalysen, Warnungen und Visualisierungen durchzuführen.

## <a name="relationship-to-azure-monitor-metrics"></a>Beziehung zu Azure Monitor-Metriken
Bei Azure Monitor-Metriken werden numerische Daten in einer Zeitreihendatenbank gespeichert, wodurch diese Daten kompakter sind als Azure Monitor-Protokolle und Szenarien nahezu in Echtzeit unterstützen können. Dies macht sie besonders nützlich für Warnungen und eine schnelle Erkennung von Problemen. In Metriken können jedoch nur numerische Daten in einer bestimmten Struktur gespeichert werden, während dies in Protokollen für eine Vielzahl verschiedener Datentypen mit jeweils eigener Struktur möglich ist. Sie können auch komplexe Analysen von Protokolldaten mithilfe von Protokollabfragen durchführen, die für die Analyse von Metrikdaten nicht verwendet werden können.

Zusätzlich zu Metriken werden häufig numerische Daten aus Datenquellen an Protokolle gesendet. Während für das Sammeln und Aufbewahren dieser Daten in Protokollen eine zusätzliche Gebühr anfällt, können Sie Metrikdaten in Protokollabfragen einbeziehen und gemeinsam mit Ihren anderen Überwachungsdaten analysieren.

## <a name="relationship-to-azure-data-explorer"></a>Beziehung zu Azure Data Explorer
Azure Monitor-Protokolle basieren auf Azure Data Explorer. Ein Log Analytics-Arbeitsbereich entspricht ungefähr einer Datenbank in Azure Data Explorer, Tabellen sind gleich strukturiert und beide verwenden dieselbe Kusto-Abfragesprache (KQL). Die Verwendung von Log Analytics zum Arbeiten mit Azure Monitor-Abfragen im Azure-Portal ähnelt der Verwendung der Azure Data Explorer-Webbenutzeroberfläche. Sie können sogar [Daten aus einem Log Analytics-Arbeitsbereich in eine Azure Data Explorer-Abfrage einschließen](/azure/data-explorer/query-monitor-data). 


## <a name="structure-of-data"></a>Struktur von Daten
Die von Azure Monitor-Protokollen gesammelten Daten werden in einem [Log Analytics-Arbeitsbereich](./design-logs-deployment.md) mit mehreren Tabellen gespeichert, die jeweils Daten aus einer bestimmten Quelle enthalten. Der Arbeitsbereich definiert den geografischen Standort der Daten, Zugriffsrechte zur Festlegung, welche Benutzer auf Daten zugreifen können, und Konfigurationseinstellungen wie Tarif und Datenaufbewahrung. Je nach Anforderungen können Sie einen einzigen Arbeitsbereich für alle Überwachungsdaten verwenden oder mehrere Arbeitsbereiche erstellen. Überlegungen zum Erstellen mehrerer Arbeitsbereiche finden Sie unter [Entwerfen Ihrer Azure Monitor-Protokollbereitstellung](design-logs-deployment.md).

Jeder Arbeitsbereich enthält mehrere Tabellen, die in separaten Spalten mit mehreren Datenzeilen angeordnet sind. Jede Tabelle ist durch einen eindeutigen Satz von Spalten definiert, die von den von der Datenquelle bereitgestellten Datenzeilen gemeinsam genutzt werden. 

[![Struktur von Azure Monitor-Protokollen](media/data-platform-logs/logs-structure.png)](media/data-platform-logs/logs-structure.png#lightbox)


Protokolldaten aus Application Insights werden ebenfalls in Azure Monitor-Protokollen gespeichert, doch erfolgt das Speichern je nach Konfiguration Ihrer Anwendung auf unterschiedliche Weise. Bei einer arbeitsbereichsbasierten Anwendung werden Daten in einem Log Analytics-Arbeitsbereich in einem Standardsatz von Tabellen gespeichert, um Daten wie Anwendungsanforderungen, Ausnahmen und Seitenaufrufe aufzubewahren. Mehrere Anwendungen können denselben Arbeitsbereich verwenden. Bei einer klassischen Anwendung werden die Daten nicht in einem Log Analytics-Arbeitsbereich gespeichert. Es wird die gleiche Abfragesprache verwendet, und Sie können Abfragen mit dem gleichen Log Analytics-Tool im Azure-Portal erstellen und ausführen. Daten für klassische Anwendungen werden jedoch getrennt voneinander gespeichert. Die allgemeine Struktur entspricht arbeitsbereichsbasierten Anwendungen, doch sind die Tabellen- und Spaltennamen unterschiedlich. Einen ausführlichen Vergleich der beiden finden Sie unter [Änderungen bei arbeitsbereichsbasierten Ressourcen](../app/apm-tables.md).


> [!NOTE]
> Wir stellen auf der Application Insights-Benutzeroberfläche weiterhin vollständige Abwärtskompatibilität für Ihre klassischen Ressourcenabfragen, Arbeitsmappen und protokollbasierten Warnungen von Application Insights bereit. Um die [neue arbeitsbereichsbasierte Tabellenstruktur und das neue Tabellenschema](../app/apm-tables.md) anzuzeigen und abzufragen, müssen Sie zuerst zu Ihrem Log Analytics-Arbeitsbereich navigieren. Während der Vorschau erhalten Sie durch Auswahl der Option **Protokolle** in den Application Insights-Bereichen Zugriff auf die klassische Application Insights-Benutzeroberfläche für Abfragen. Weitere Informationen finden Sie unter [Abfragebereich](../log-query/scope.md).


[![Struktur von Azure Monitor-Protokollen für Application Insights](media/data-platform-logs/logs-structure-ai.png)](media/data-platform-logs/logs-structure-ai.png#lightbox)

## <a name="log-queries"></a>Protokollabfragen
Daten werden mithilfe einer [Protokollabfrage](../log-query/log-query-overview.md) aus einem Log Analytics-Arbeitsbereich abgerufen. Hierbei handelt es sich um eine schreibgeschützte Anforderung zum Verarbeiten von Daten und Zurückgeben von Ergebnissen. Protokollabfragen werden in der [Kusto-Abfragesprache (KQL)](/azure/data-explorer/kusto/query/) geschrieben – der Abfragesprache, die von Azure Data Explorer verwendet wird. Verwenden Sie Log Analytics. Dies ist ein Tool im Azure-Portal, das zum Bearbeiten und Ausführen von Protokollabfragen und interaktiven Analysieren von deren Ergebnissen verwendet wird. Anschließend können Sie die von Ihnen erstellten Abfragen zur Unterstützung anderer Funktionen in Azure Monitor wie Protokollabfragewarnungen und Arbeitsmappen verwenden.


## <a name="sources-of-data-for-azure-monitor-logs"></a>Datenquellen für Azure Monitor-Protokolle
Azure Monitor sammelt Protokolldaten aus einer Vielzahl von Quellen, einschließlich Ressourcen in Azure Monitor und Agents, die auf virtuellen Computern ausgeführt werden. Eine vollständige Liste der Datenquellen, die Daten an einen Log Analytics-Arbeitsbereich senden, finden Sie unter [Was wird von Azure Monitor überwacht?](../monitor-reference.md)



## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [Protokollabfragen](../log-query/log-query-overview.md) zum Abrufen und Analysieren von Daten aus einem Log Analytics-Arbeitsbereich.
- Informieren Sie sich über [Metriken in Azure Monitor](data-platform-metrics.md).
- Erfahren Sie mehr über die für verschiedene Ressourcen in Azure [verfügbaren Überwachungsdaten](data-sources.md).

