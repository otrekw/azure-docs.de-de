---
title: Was ist VM Insights?
description: Hier finden Sie eine Übersicht über VM Insights, ein Tool, mit dem die Integrität und Leistung der Azure-VMs überwacht wird sowie Anwendungskomponenten und ihre Abhängigkeiten automatisch ermittelt und zugeordnet werden.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 07/22/2020
ms.openlocfilehash: 967ff9672f55af3e9c5d2e7c3c3dbc7d2ff6cb3c
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101707369"
---
# <a name="overview-of-vm-insights"></a>Übersicht über VM Insights

VM Insights überwacht die Leistung und Integrität Ihrer virtuellen Computer und Virtual Machine Scale Sets-Instanzen, einschließlich ihrer ausgeführten Prozesse und Abhängigkeiten von anderen Ressourcen. Dieser Dienst kann die Bereitstellung von Vorhersagen zu Leistung und Verfügbarkeit wichtiger Anwendungen unterstützen, indem Leistungsengpässe und Netzwerkprobleme erkannt werden, und auch Informationen dazu liefern, ob ein Problem im Zusammenhang mit anderen Abhängigkeiten steht.

VM Insights unterstützt Windows- und Linux-Betriebssysteme auf folgenden Computern:

- Virtuelle Azure-Computer
- Azure-VM-Skalierungsgruppen
- Hybrid-VMs, die mit Azure Arc verbunden sind
- Lokale virtuelle Computer
- In einer anderen Cloudumgebung gehostete virtuelle Computer
  

VM Insights speichert die Daten in Azure Monitor-Protokollen und ermöglicht dadurch leistungsstarke Aggregation und Filterung sowie die Analyse von Datentrends im zeitlichen Verlauf. Sie können diese Daten direkt in einer einzelnen VM anzeigen, oder Sie können eine aggregierte Ansicht mehrerer VMs mit Azure Monitor bereitstellen.

![VM-Insights-Perspektive im Azure-Portal](media/vminsights-overview/vminsights-azmon-directvm.png)


## <a name="pricing"></a>Preise
Es fallen keine direkten Kosten für VM Insights an, doch werden Ihnen die Aktivitäten im Log Analytics-Arbeitsbereich in Rechnung gestellt. Basierend auf den Preisen, die auf der [Seite mit der Azure Monitor-Preisübersicht](https://azure.microsoft.com/pricing/details/monitor/) veröffentlicht sind, fallen VM Insights-Gebühren für Folgendes an:

- Von Agents erfasste und im Arbeitsbereich gespeicherte Daten
- Von der Gastintegrität gesammelte Integritätsstatusdaten (Vorschau)
- Warnungsregeln basierend auf Protokoll- und Integritätsdaten
- Von Warnungsregeln gesendete Benachrichtigungen

Die Protokollgröße unterscheidet sich je nach Zeichenfolgenlänge der Leistungsindikatoren und kann mit der Anzahl der logischen Datenträger und Netzwerkadapter anwachsen, die der VM zugeordnet sind. Wenn Sie bereits die Dienstzuordnung verwenden, besteht die einzige für Sie sichtbare Veränderung in den zusätzlichen Leistungsdaten, die an den Azure Monitor-Datentyp `InsightsMetrics` gesendet werden.


## <a name="configuring-vm-insights"></a>Konfigurieren von VM Insights
Die Schritte zum Konfigurieren von VM Insights lauten wie folgt. Unter jedem Link finden Sie ausführliche Anleitungen zum jeweiligen Schritt:

- [Erstellen eines Log Analytics-Arbeitsbereichs](./vminsights-configure-workspace.md#create-log-analytics-workspace)
- [Hinzufügen der VMInsights-Lösung zum Arbeitsbereich](./vminsights-configure-workspace.md#add-vminsights-solution-to-workspace)
- [Installieren von Agents auf zu überwachender VM und VM-Skalierungsgruppe](./vminsights-enable-overview.md)



## <a name="next-steps"></a>Nächste Schritte

- Informationen zu den Anforderungen und Methoden für die Aktivierung der Überwachung für Ihre virtuellen Computer finden Sie unter [Bereitstellen von VM Insights](./vminsights-enable-overview.md).
