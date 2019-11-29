---
title: Konfiguration der Integritätsmonitore in Azure Monitor für Container | Microsoft-Dokumentation
description: In diesem Artikel wird die Konfiguration der Integritätsmonitore in Azure Monitor für Container ausführlich beschrieben.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: conceptual
ms.workload: infrastructure-services
ms.date: 11/12/2019
ms.author: magoedte
ms.openlocfilehash: b782581318751830ec47b9fecb056fecefb353eb
ms.sourcegitcommit: 2d3740e2670ff193f3e031c1e22dcd9e072d3ad9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2019
ms.locfileid: "74134386"
---
# <a name="azure-monitor-for-containers-health-monitor-configuration-guide"></a>Konfigurationsleitfaden für Integritätsmonitore in Azure Monitor für Container

Monitore sind das Hauptelement zum Messen der Integrität und Erkennen von Fehlern in Azure Monitor für Container. Dieser Artikel erläutert die Konzepte zum Messen der Integrität sowie die Elemente, die das Integritätsmodell zum Überwachen und Melden der Integrität Ihres Kubernetes-Clusters mit dem [Integritätsfeature](container-insights-health.md) umfasst.

## <a name="monitors"></a>Monitore

Mit einem Monitor wird die Integrität eines bestimmten Aspekts eines verwalteten Objekts gemessen. Monitore verfügen jeweils über zwei oder drei Integritätsstatusarten. Ein Monitor kann zu einem bestimmten Zeitpunkt immer nur jeweils einen der möglichen Status aufweisen. Wenn ein Monitor vom Container-Agent geladen wird, wird er mit einem fehlerfreien Status initialisiert. Der Status ändert sich nur, wenn die angegebenen Bedingungen für einen anderen Status erkannt werden.

Die Gesamtintegrität eines bestimmten Objekts wird durch die Integrität der einzelnen Monitore bestimmt. Diese Hierarchie ist im Bereich „Integritätshierarchie“ in Azure Monitor für Container dargestellt. Die Richtlinie für den Rollup der Integrität ist Bestandteil der Konfiguration der Aggregatmonitore.

## <a name="types-of-monitors"></a>Monitortypen

|Monitor | BESCHREIBUNG | 
|--------|-------------|
| Einheitenmonitor |Ein Einheitenmonitor misst einen bestimmten Aspekt einer Ressource oder Anwendung. Dabei kann es sich um die Überprüfung eines Leistungszählers zum Ermitteln der Leistung der Ressource oder ihrer Verfügbarkeit handeln. |
|Aggregatmonitor | Mit Aggregatmonitoren werden mehrere Monitore in einer Gruppe zusammengefasst, um einen einzelnen aggregierten Integritätsstatus bereitzustellen. Einheitenmonitore werden in der Regel unter einem bestimmten Aggregatmonitor konfiguriert. Ein Aggregatmonitor vom Typ „Knoten“ führt z.B. einen Rollup des Status der Knoten-CPU-Auslastung, der Arbeitsspeicherauslastung und des Knotenstatus durch.
 |

### <a name="aggregate-monitor-health-rollup-policy"></a>Richtlinie für Integritätsstatus-Rollups des Aggregatmonitors

Für jeden Aggregatmonitor ist eine Richtlinie für Integritätsstatus-Rollups definiert. Hierbei handelt es sich um die Logik, mit der die Integrität des Aggregatmonitors basierend auf der Integrität der untergeordneten Monitore ermittelt wird. Folgende Richtlinien für Integritätsstatus-Rollups sind für einen Aggregatmonitor möglich:

#### <a name="worst-state-policy"></a>Schlechtester Status-Richtlinie

Der Status des Aggregatmonitors entspricht dem Status des untergeordneten Monitors mit dem schlechtesten Integritätsstatus. Dies ist die von Aggregatmonitoren am häufigsten verwendete Richtlinie.

![Beispiel für Rollup des Aggregatmonitors nach schlechtestem Status](./media/container-insights-health-monitoring-cfg/aggregate-monitor-rollup-worstof.png)

### <a name="percentage-policy"></a>Prozentsatz-Richtlinie

Das Quellobjekt entspricht dem schlechtesten Status eines einzelnen Elements eines angegebenen Prozentsatzes von Zielobjekten im besten Status. Diese Richtlinie wird verwendet, wenn ein bestimmter Prozentsatz von Zielobjekten fehlerfrei sein muss, damit das Zielobjekt als fehlerfrei eingestuft wird. Bei der Prozentsatz-Richtlinie werden die Monitore in absteigender Reihenfolge nach Schweregrad des Status sortiert, und der Status des Aggregatmonitors wird als der schlechteste Status von N % berechnet (N wird durch den Konfigurationsparameter *StateThresholdPercentage* vorgegeben).

Angenommen, es gibt fünf Containerinstanzen eines Containerimages, und der jeweilige Status ist **Kritische**, **Warnung**, **Fehlerfrei**, **Fehlerfrei** und **Fehlerfrei**.  Der Status des Monitors für die Container-CPU-Auslastung lautet **Kritisch**, da der schlechteste Status von 90 % der Container bei Sortierung in absteigender Reihenfolge nach Schweregrad **Kritisch** lautet.

## <a name="understand-the-monitoring-configuration"></a>Verstehen der Überwachungskonfiguration

Azure Monitor für Container umfasst eine Reihe wichtiger Überwachungsszenarien, die wie folgt konfiguriert sind.

### <a name="unit-monitors"></a>Einheitenmonitore

|**Monitorname** | Monitortyp | **Beschreibung** | **Parameter** | **Wert** |
|-----------------|--------------|-----------------|---------------|-----------|
|Knotenspeicherauslastung |Einheitenmonitor |Mit diesem Monitor wird die Arbeitsspeicherauslastung eines Knotens anhand der von cAdvisor gemeldeten Daten minütlich ausgewertet. |ConsecutiveSamplesForStateTransition<br> FailIfGreaterThanPercentage<br> WarnIfGreaterThanPercentage | 3<br> 90<br> 80  ||
|Knoten-CPU-Auslastung |Einheitenmonitor |Mit diesem Monitor wird die CPU-Auslastung des Knotens anhand der von cAdvisor gemeldeten Daten minütlich ausgewertet. | ConsecutiveSamplesForStateTransition<br> FailIfGreaterThanPercentage<br> WarnIfGreaterThanPercentage | 3<br> 90<br> 80  ||
|Knotenstatus |Einheitenmonitor |Dieser Monitor überprüft die von Kubernetes gemeldeten Knotenbedingungen.<br> Derzeit werden die folgenden Knotenbedingungen überprüft: Datenträgerauslastung, Arbeitsspeicherauslastung, PID-Auslastung, Nicht genügend Speicherplatz, Netzwerk nicht verfügbar, Status „Bereit“ für den Knoten.<br> Wenn von den oben genannten Bedingungen entweder *Nicht genügend Arbeitsspeicher* oder *Netzwerk nicht verfügbar* als zutreffend (**true**) ausgewertet werden, wechselt der Monitor in den Status **Kritisch**.<br> Wenn eine der anderen Bedingungen als zutreffend (**true**) ausgewertet wird, mit Ausnahme des Status **Bereit**, wechselt der Monitor in den Status **Warnung**. | NodeConditionTypeForFailedState | outofdisk,networkunavailable ||
|Containerspeicherauslastung |Einheitenmonitor |Dieser Monitor meldet den kombinierten Integritätsstatus der Arbeitsspeicherauslastung (RSS) der Containerinstanzen.<br> Dabei wird ein einfacher Vergleich durchgeführt, bei dem jede Stichprobe mit einem einzelnen Schwellenwert verglichen und durch den Konfigurationsparameter **ConsecutiveSamplesForStateTransition** angegeben wird.<br> Der Status wird als der schlechteste Status von 90 % der Containerinstanzen (StateThresholdPercentage), sortiert in absteigender Reihenfolge nach Schweregrad des Containerintegritätsstatus (Kritisch, Warnung, Fehlerfrei), berechnet.<br> Wenn von einer Containerinstanz kein Datensatz empfangen wird, wird der Integritätsstatus der Containerinstanz als **Unbekannt** gemeldet und hat in der Sortierreihenfolge Vorrang vor dem Status **Kritisch**.<br> Der Status der einzelnen Containerinstanzen wird anhand der in der Konfiguration angegebenen Schwellenwerte berechnet. Wenn die Auslastung den kritischen Schwellenwert (90 %) übersteigt, weist die Instanz den Status **Kritisch** auf. Liegt die Auslastung unterhalb des kritischen Schwellenwerts (90 %), aber über dem Warnungsschwellenwert (80 %), weist die Instanz den Status **Warnung** auf. Andernfalls liegt der Status **Fehlerfrei** vor. |ConsecutiveSamplesForStateTransition<br> FailIfLessThanPercentage<br> StateThresholdPercentage<br> WarnIfGreaterThanPercentage| 3<br> 90<br> 90<br> 80 ||
|Container-CPU-Auslastung |Einheitenmonitor |Dieser Monitor meldet den kombinierten Integritätsstatus der CPU-Auslastung der Containerinstanzen.<br> Dabei wird ein einfacher Vergleich durchgeführt, bei dem jede Stichprobe mit einem einzelnen Schwellenwert verglichen und durch den Konfigurationsparameter **ConsecutiveSamplesForStateTransition** angegeben wird.<br> Der Status wird als der schlechteste Status von 90 % der Containerinstanzen (StateThresholdPercentage), sortiert in absteigender Reihenfolge nach Schweregrad des Containerintegritätsstatus (Kritisch, Warnung, Fehlerfrei), berechnet.<br> Wenn von einer Containerinstanz kein Datensatz empfangen wird, wird der Integritätsstatus der Containerinstanz als **Unbekannt** gemeldet und hat in der Sortierreihenfolge Vorrang vor dem Status **Kritisch**.<br> Der Status der einzelnen Containerinstanzen wird anhand der in der Konfiguration angegebenen Schwellenwerte berechnet. Wenn die Auslastung den kritischen Schwellenwert (90 %) übersteigt, weist die Instanz den Status **Kritisch** auf. Liegt die Auslastung unterhalb des kritischen Schwellenwerts (90 %), aber über dem Warnungsschwellenwert (80 %), weist die Instanz den Status **Warnung** auf. Andernfalls liegt der Status **Fehlerfrei** vor. |ConsecutiveSamplesForStateTransition<br> FailIfLessThanPercentage<br> StateThresholdPercentage<br> WarnIfGreaterThanPercentage| 3<br> 90<br> 90<br> 80 ||
|Systemworkloadpods bereit |Einheitenmonitor |Dieser Monitor meldet den Status basierend auf dem Prozentsatz von Pods im Zustand „Bereit“ in einer bestimmten Workload. Der Status wird auf **Kritisch** gesetzt, wenn weniger als 100 % der Pods den Status **Fehlerfrei** aufweisen. |ConsecutiveSamplesForStateTransition<br> FailIfLessThanPercentage |2<br> 100 ||
|Kube-API-Status |Einheitenmonitor |Dieser Monitor meldet den Status des Kube-API-Diensts. Der Monitor befindet sich im kritischen Status, wenn der Kube-API-Endpunkt nicht verfügbar ist. Bei diesem speziellen Monitor wird der Status durch eine Abfrage an den Endpunkt „nodes“ für den Kube-API-Server bestimmt. Bei allen anderen Antwortcodes als „OK“ wird der Monitor in den Status **Kritisch** gesetzt. | Keine Konfigurationseigenschaften |||

### <a name="aggregate-monitors"></a>Aggregatmonitore

|**Monitorname** | **Beschreibung** | **Algorithmus** |
|-----------------|-----------------|---------------|
|Knoten |Dieser Monitor ist ein Aggregat aller Knotenmonitore. Sein Status entspricht dem Status des untergeordneten Monitors mit dem schlechtesten Integritätsstatus:<br> Knoten-CPU-Auslastung<br> Knotenspeicherauslastung<br> Knotenstatus | Schlechtester|
|Knotenpool |Dieser Monitor meldet den kombinierten Integritätsstatus aller Knoten im Knotenpool *agentpool*. Hierbei handelt es sich um einen Monitor mit drei möglichen Statuswerten, dessen jeweiliger Status auf dem schlechtesten Status von 80 % der Knoten im Knotenpool, sortiert in absteigender Reihenfolge nach Schweregrad des Knotenstatus (Kritisch, Warnung, Fehlerfrei), basiert.|Prozentsatz |
|Knoten (dem Knotenpool übergeordnet) |Hierbei handelt es sich um einen Aggregatmonitor aller Knotenpools. Sein Status basiert auf dem schlechtesten Status der untergeordneten Monitore (d.h. den im Cluster vorhandenen Knotenpools). |Schlechtester |
|Cluster (den Knoten übergeordnet/<br> Kubernetes-Infrastruktur) |Dies ist der übergeordnete Monitor, der mit dem Status des untergeordneten Monitors mit dem schlechtesten Integritätsstatus (Kubernetes-Infrastruktur und Knoten) übereinstimmt. |Schlechtester |
|Kubernetes-Infrastruktur |Dieser Monitor meldet den kombinierten Integritätsstatus der verwalteten Infrastrukturkomponenten des Clusters. Sein Status wird als „schlechtester“ der untergeordneten Monitorstatus (Kube-Systemworkloads und API-Serverstatus) berechnet. |Schlechtester|
|Systemworkload |Dieser Monitor meldet den Integritätsstatus einer Kube-Systemworkload. Dieser Monitor stimmt mit dem Status des untergeordneten Monitors mit dem schlechtesten Integritätsstatus überein, d.h. dem Monitor für **Pods im Zustand „Bereit“** und den Containern in der Workload. |Schlechtester |
|Container |Dieser Monitor meldet den Gesamtintegritätsstatus eines Containers in einer bestimmten Workload. Dieser Monitor stimmt mit dem Status des untergeordneten Monitors mit dem schlechtesten Integritätsstatus überein, d.h. den Monitoren für **CPU-Auslastung** und **Arbeitsspeicherauslastung**. |Schlechtester |

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Anzeigen des Integritätsstatus Ihres Kubernetes-Clusters finden Sie unter [Überwachen der Clusterintegrität](container-insights-health.md).