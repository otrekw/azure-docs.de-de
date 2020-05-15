---
title: Überwachen von Daten – Referenz | Microsoft-Dokumentation
titleSuffix: Azure Machine Learning
description: Erfahren Sie mehr über die Daten und Ressourcen, die für Azure Machine Learning gesammelt werden und in Azure Monitor verfügbar sind. Azure Monitor sammelt Daten über Ihren Azure Machine Learning-Arbeitsbereich, stellt diese Daten dar und ermöglicht es Ihnen, Metriken anzuzeigen, Benachrichtigungen festzulegen und protokollierte Daten zu analysieren.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
ms.reviewer: larryfr
ms.author: aashishb
author: aashishb
ms.date: 04/27/2020
ms.openlocfilehash: e9a43f4a7da39869e002e2da9fb9638381e57cb8
ms.sourcegitcommit: c535228f0b77eb7592697556b23c4e436ec29f96
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2020
ms.locfileid: "82856036"
---
# <a name="azure-machine-learning-monitoring-data-reference"></a>Überwachen von Azure Machine Learning-Daten – Referenz

Erfahren Sie mehr über die Daten und Ressourcen, die von Azure Monitor aus Ihrem Azure Machine Learning-Arbeitsbereich gesammelt werden. Ausführliche Informationen über das Sammeln und Analysieren von Überwachungsdaten finden Sie unter [Überwachen von Azure Machine Learning](monitor-azure-machine-learning.md).

## <a name="resource-logs"></a>Ressourcenprotokolle

In der folgenden Tabelle sind die Eigenschaften für Azure Machine Learning-Ressourcenprotokolle aufgelistet, wenn diese in Azure Monitor-Protokollen oder Azure Storage gesammelt werden.

### <a name="amlcomputejobevents-table"></a>Tabelle „AmlComputeJobEvents“

| Eigenschaft | BESCHREIBUNG |
|:--- |:---|
| TimeGenerated | Zeitpunkt, zu dem der Protokolleintrag generiert wurde |
| Vorgangsname | Name des Vorgangs, der dem Protokollereignis zugeordnet ist |
| Category | Name des Protokollereignisses, AmlComputeClusterNodeEvent |
| JobId | ID des übermittelten Auftrags |
| ExperimentId | ID des Experiments |
| ExperimentName | Name des Experiments |
| CustomerSubscriptionId | Abonnement-ID, unter der das Experiment und der Auftrag übermittelt wurden |
| WorkspaceName | Name des Machine Learning-Arbeitsbereichs |
| ClusterName | Name des Clusters |
| ProvisioningState | Status der Auftragsübermittlung |
| ResourceGroupName | Name der Ressourcengruppe |
| JobName | Name des Auftrags |
| ClusterId | ID des Clusters |
| EventType | Typ des Auftragsereignisses, z. B. JobSubmitted, JobRunning, JobFailed, JobSucceeded usw. |
| ExecutionState | Status des Auftrags (der Ausführung), z. B. Queued, Running, Succeeded, Failed |
| ErrorDetails | Details zum Auftragsfehler |
| CreationApiVersion | API-Version, mit der der Auftrag erstellt wurde |
| ClusterResourceGroupName | Ressourcengruppenname des Clusters |
| TFWorkerCount | Anzahl der TF-Worker |
| TFParameterServerCount | Anzahl der TF-Parameterserver |
| ToolType | Typ des verwendeten Tools |
| RunInContainer | Flag, das beschreibt, ob der Auftrag in einem Container ausgeführt werden soll |
| JobErrorMessage | Ausführliche Meldung für den Auftragsfehler |
| NodeId | ID des erstellten Knotens, auf dem der Auftrag ausgeführt wird |

### <a name="amlcomputeclusterevents-table"></a>Tabelle „AmlComputeClusterEvents“

| Eigenschaft | BESCHREIBUNG |
|:--- |:--- |
| TimeGenerated | Zeitpunkt, zu dem der Protokolleintrag generiert wurde |
| Vorgangsname | Name des Vorgangs, der dem Protokollereignis zugeordnet ist |
| Category | Name des Protokollereignisses, AmlComputeClusterNodeEvent |
| ProvisioningState | Bereitstellungsstatus des Clusters |
| ClusterName | Name des Clusters |
| ClusterType | Typ des Clusters |
| CreatedBy | Benutzer, der den Cluster erstellt hat |
| CoreCount | Anzahl der Kerne im Cluster |
| VmSize | VM-Größe des Clusters |
| VmPriority | Priorität der Knoten, die in einem Cluster erstellt wurden (Dedicated/LowPriority) |
| ScalingType | Typ der Clusterskalierung (manual/auto) |
| InitialNodeCount | Anfängliche Knotenanzahl des Clusters |
| MinimumNodeCount | Kleinste Knotenanzahl des Clusters |
| MaximumNodeCount | Größte Knotenanzahl des Clusters |
| NodeDeallocationOption | Art und Weise, wie die Zuordnung des Knotens aufgehoben werden soll |
| Herausgeber | Herausgeber des Clustertyps |
| Angebot | Angebot, mit dem der Cluster erstellt wird |
| Sku | SKU des im Cluster erstellten Knotens/virtuellen Computers |
| Version | Version des Images, das beim Erstellen des Knotens/virtuellen Computers verwendet wird |
| SubnetId | Subnetz-ID des Clusters |
| AllocationState | Zuweisungsstatus des Clusters |
| CurrentNodeCount | Aktuelle Knotenanzahl des Clusters |
| TargetNodeCount | Zielknotenanzahl des Clusters beim zentralen Hoch-/Herunterskalieren |
| EventType | Typ des Ereignisses während der Clustererstellung |
| NodeIdleTimeSecondsBeforeScaleDown | Leerlaufzeit in Sekunden vor horizontalem Herunterskalieren des Clusters |
| PreemptedNodeCount | Anzahl der vorzeitig entfernten Knoten des Clusters |
| IsResizeGrow | Flag, das angibt, dass der Cluster zentral hochskaliert wird |
| VmFamilyName | Name der VM-Familie der Knoten, die im Cluster erstellt werden können |
| LeavingNodeCount | Anzahl der verbliebenen Knoten des Clusters |
| UnusableNodeCount | Anzahl der nicht verwendbaren Knoten des Clusters |
| IdleNodeCount | Anzahl der nicht verwendbaren Knoten des Clusters |
| RunningNodeCount | Anzahl der ausführenden Knoten des Clusters |
| PreparingNodeCount | Anzahl der vorzubereitenden Knoten des Clusters |
| QuotaAllocated | Zugeordnetes Kontingent für den Cluster |
| QuotaUtilized | Genutztes Kontingent für den Cluster |
| AllocationStateTransitionTime | Übergangszeit von einem Zustand in einen anderen |
| ClusterErrorCodes | Fehlercode, der während der Clustererstellung oder Skalierung empfangen wurde |
| CreationApiVersion | API-Version, die beim Erstellen des Clusters verwendet wurde |

### <a name="amlcomputeclusternodeevents-table"></a>Tabelle „AmlComputeClusterNodeEvents“

| Eigenschaft | BESCHREIBUNG |
|:--- |:--- |
| TimeGenerated | Zeitpunkt, zu dem der Protokolleintrag generiert wurde |
| Vorgangsname | Name des Vorgangs, der dem Protokollereignis zugeordnet ist |
| Category | Name des Protokollereignisses, AmlComputeClusterNodeEvent |
| ClusterName | Name des Clusters |
| NodeId | ID des erstellten Clusterknotens |
| VmSize | Größe des virtuellen Computers des Knotens |
| VmFamilyName | VM-Familie, zu der der Knoten gehört |
| VmPriority | Priorität des erstellten Knotens (Dedicated/LowPriority) |
| Herausgeber | Herausgeber des VM-Images, z. B. „microsoft-dsvm“ |
| Angebot | Angebot, das mit der VM-Erstellung verknüpft ist |
| Sku | SKU des erstellten Knotens/virtuellen Computers |
| Version | Version des Images, das beim Erstellen des Knotens/virtuellen Computers verwendet wird |
| ClusterCreationTime | Zeitpunkt, zu dem der Cluster erstellt wurde |
| ResizeStartTime | Zeitpunkt, zu dem das Hoch-/Herunterskalieren des Clusters gestartet wurde |
| ResizeEndTime | Zeitpunkt, zu dem das Hoch-/Herunterskalieren des Clusters beendet wurde |
| NodeAllocationTime | Zeitpunkt, zu dem der Knoten zugeordnet wurde |
| NodeBootTime | Zeitpunkt, zu dem der Knoten hochgefahren wurde |
| StartTaskStartTime | Zeitpunkt, zu dem die Aufgabe einem Knoten zugewiesen und gestartet wurde |
| StartTaskEndTime | Zeitpunkt, zu dem die einem Knoten zugewiesene Aufgabe beendet wurde |
| TotalE2ETimeInSeconds | Gesamtzeit, die der Knoten aktiv war |

### <a name="metrics"></a>Metriken

In den folgenden Tabellen sind die für Azure Machine Learning gesammelten Metriken aufgeführt. Alle Metriken werden im Namespace **Azure Machine Learning Workspace** gespeichert.

**Modell**

| Metrik | Einheit | BESCHREIBUNG |
| ----- | ----- | ----- |
| Model deploy failed (Fehler bei der Modellimplementierung) | Anzahl | Die Anzahl der fehlerhaften Modellimplementierungen |
| Model deploy started (Gestartete Modellimplementierungen) | Anzahl | Die Anzahl der gestarteten Modellimplementierungen |
| Model deploy succeeded (Erfolgreiche Modellimplementierungen) | Anzahl | Die Anzahl der erfolgreichen Modellimplementierungen |
| Model register failed (Fehler bei der Modellregistrierung) | Anzahl | Die Anzahl der fehlerhaften Modellregistrierungen |
| Model register succeeded (Erfolgreiche Modellregistrierungen) | Anzahl | Die Anzahl der erfolgreichen Modellregistrierungen |

**Kontingent**

Kontingentinformationen gelten nur für Azure Machine Learning Compute.

| Metrik | Einheit | BESCHREIBUNG |
| ----- | ----- | ----- |
| Active cores (Aktive Kerne) | Anzahl | Die Anzahl der aktiven Compute-Kerne |
| Active nodes (Aktive Knoten) | Anzahl | Die Anzahl der aktiven Knoten |
| Idle cores (Kerne im Leerlauf) | Anzahl | Die Anzahl der Compute-Kerne im Leerlauf |
| Idle nodes (Knoten im Leerlauf) | Anzahl | Die Anzahl der Serverknoten (Compute-Knoten) im Leerlauf |
| Leaving cores (Verbleibende Kerne) | Anzahl | Die Anzahl der verbleibenden Kerne |
| Leaving nodes (Verbleibende Knoten) | Anzahl | Die Anzahl der verbleibenden Knoten |
| Preempted cores (Kerne mit präemptivem Multitasking) | Anzahl | Die Anzahl der Kerne mit präemptivem Multitasking |
| Vorzeitig entfernte Knoten | Anzahl | Die Anzahl der Knoten mit präemptivem Multitasking |
| Quota utilization percentage (Prozentsatz der Kontingentnutzung) | Percent | Der Prozentsatz, mit dem das Kontingent genutzt wird |
| Total cores (Kerne gesamt) | Anzahl | Die Gesamtzahl der Kerne |
| Total nodes (Knoten gesamt) | Anzahl | Die Gesamtzahl der Knoten |
| Unusable cores (Nicht verwendbare Kerne) | Anzahl | Die Anzahl der nicht verwendbaren Kerne |
| Unusable nodes (Nicht verwendbare Knoten) | Anzahl | Die Anzahl der nicht verwendbaren Knoten |

Die folgenden Dimensionen sind Dimensionen, mit denen Kontingentmetriken gefiltert werden können:

| Dimension | Metrik(en) verfügbar mit | BESCHREIBUNG |
| ---- | ---- | ---- |
| Clustername | All quota metrics (Alle Kontingentmetriken) | Der Name der Compute-Instanz |
| Vm Family Name (Name der VM-Familie) | Quota utilization percentage (Prozentsatz der Kontingentnutzung) | Der Name der vom Cluster verwendeten VM-Familie |
| Vm Priority (VM-Priorität) | Quota utilization percentage (Prozentsatz der Kontingentnutzung) | Die Priorität des virtuellen Computers (VM)

**Ressource**

| Metrik | Einheit | BESCHREIBUNG |
| ----- | ----- | ----- |
| CpuUtilization | Percent | Wie viel Prozent der CPU-Auslastung für einen bestimmten Knoten während eines Laufs/eines Auftrags verwendet wurden. Diese Metrik wird nur veröffentlicht, wenn ein Auftrag auf einem Knoten ausgeführt wird. Ein Auftrag kann einen oder mehrere Knoten verwenden. Diese Metrik wird pro Knoten veröffentlicht. |
| GpuUtilization | Percent | Gibt an, welcher Prozentsatz der GPU-Auslastung für einen bestimmten Knoten während eines Laufs/eines Auftrags verwendet wurde. Ein Knoten kann eine oder mehrere GPUs aufweisen. Diese Metrik wird pro GPU pro Knoten veröffentlicht. |

Die folgenden Dimensionen sind Dimensionen, mit denen Ressourcenmetriken gefiltert werden können:

| Dimension | BESCHREIBUNG |
| ----- | ----- |
| CreatedTime | |
| deviceId | ID des Geräts (GPU). Nur für GpuUtilization verfügbar. |
| NodeId | ID des erstellten Knotens, auf dem der Auftrag ausgeführt wird. |
| RunId | ID des Laufs/Auftrags. |

**Ausführen**

Informationen zu Trainingsausführungen

| Metrik | Einheit | BESCHREIBUNG |
| ----- | ----- | ----- |
| Completed runs (Abgeschlossene Ausführungen) | Anzahl | Die Anzahl der abgeschlossenen Ausführungen |
| Failed runs (Ausführungen mit Fehlern) | Anzahl | Die Anzahl der Ausführungen mit Fehlern |
| Started runs (Gestartete Ausführungen) | Anzahl | Die Anzahl der gestarteten Ausführungen |

Die folgenden Dimensionen sind Dimensionen, mit denen Ausführungsmetriken gefiltert werden können:

| Dimension | BESCHREIBUNG |
| ---- | ---- |
| ComputeType | Der für die Ausführung verwendete Computetyp |
| PipelineStepType | Der Typ des [PipelineStep](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipelinestep?view=azure-ml-py)-Objekts, das in der Ausführung verwendet wurde |
| PublishedPipelineId | Die ID der veröffentlichten Pipeline, die in der Ausführung verwendet wurde |
| RunType | Der Typ der Ausführung. |

Gültige Werte für die RunType-Dimension sind:

| Wert | BESCHREIBUNG |
| ----- | ----- |
| Experiment | Nicht-Pipelineausführungen |
| PipelineRun | Eine Pipelineausführung, die das übergeordnete Element einer Schrittausführung (StepRun) ist |
| StepRun | Eine Ausführung für einen Pipelineschritt |
| ReusedStepRun | Eine Ausführung für einen Pipelineschritt, in dem eine vorherige Ausführung erneut verwendet wird |

## <a name="see-also"></a>Weitere Informationen

- Eine Beschreibung der Überwachung von Azure Machine Learning finden Sie unter [Überwachen von Azure Machine Learning](monitor-azure-machine-learning.md).
- Ausführliche Informationen zur Überwachung von Azure-Ressourcen finden Sie unter [Überwachen von Azure-Ressourcen mit Azure Monitor](/azure/azure-monitor/insights/monitor-azure-resource).
