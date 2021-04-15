---
title: Konfigurieren der PV-Überwachung mit Container Insights | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie die Überwachung von Kubernetes-Clustern mit persistenten Volumes mit Container Insights konfigurieren können.
ms.topic: conceptual
ms.date: 03/03/2021
ms.openlocfilehash: 7c6ddd62bf06d313987289e444962378cea43fc8
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105627896"
---
# <a name="configure-pv-monitoring-with-container-insights"></a>Konfigurieren der PV-Überwachung mit Container Insights

Ab der Agent-Version *ciprod10052020* unterstützt der integrierte Agent von Azure Monitor für Container die Überwachung der Nutzung persistenter Volumes (PVs). Mit der Agent-Version *ciprod01112021* unterstützt der Agent die Überwachung des PV-Bestands – einschließlich Informationen zu Status, Speicherklasse, Typ, Zugriffsmodi und weiteren Details.
## <a name="pv-metrics"></a>PV-Metriken

Container Insights beginnt automatisch mit der Überwachung der PV-Nutzung. Hierzu werden die folgenden Metriken in 60-Sekunden-Intervallen erfasst und in der Tabelle **InsightMetrics** gespeichert:

| Metrikname | Metrikdimension (Tags) | Beschreibung der Metrik |
|-----|-----------|----------|
| `pvUsedBytes`| podUID, podName, pvcName, pvcNamespace, capacityBytes, clusterId, clusterName| Verwendeter Speicherplatz in Bytes für ein bestimmtes persistentes Volume mit einem von einem bestimmten Pod verwendeten Anspruch. `capacityBytes` wird als Dimension in das Feld „Tags“ integriert, um die Datenerfassungskosten zu verringern und Abfragen zu vereinfachen.|

Weitere Informationen zum Konfigurieren erfasster PV-Metriken finden Sie [hier](./container-insights-agent-config.md).

## <a name="pv-inventory"></a>PV-Bestand

Azure Monitor für Container beginnt automatisch mit der Überwachung von PVs. Hierzu werden die folgenden Informationen in 60-Sekunden-Intervallen erfasst und in der Tabelle **KubePVInventory** gespeichert:

|Daten |Datenquelle| Datentyp| Felder|
|-----|-----------|----------|-------|
|Bestand der persistenten Volumes in einem Kubernetes-Cluster |Kube-API |`KubePVInventory` |    PVName, PVCapacityBytes, PVCName, PVCNamespace, PVStatus, PVAccessModes, PVType, PVTypeInfo, PVStorageClassName, PVCreationTimestamp, TimeGenerated, ClusterId, ClusterName, _ResourceId |

## <a name="monitor-persistent-volumes"></a>Überwachen persistenter Volumes

Azure Monitor für Container schließt vorkonfigurierte Diagramme für diese Nutzungsmetrik und Bestandsinformationen in Arbeitsmappenvorlagen für jeden Cluster ein. Sie können auch eine empfohlene Warnung für die PV-Nutzung aktivieren und diese Metriken in Log Analytics abfragen.  

### <a name="workload-details-workbook"></a>Arbeitsmappe für Workloaddetails

Nutzungsdiagramme für bestimmte Workloads finden Sie auf der Registerkarte „Persistentes Volume“ der Arbeitsmappe **Workloaddetails** direkt aus einem AKS-Cluster. Wählen Sie hierzu die Option „Arbeitsmappen“ aus. Diese befindet sich im linken Bereich sowie in der Dropdownliste **Arbeitsmappen anzeigen** und auf der Registerkarte **Berichte (Vorschau)** im Bereich „Erkenntnisse“.


:::image type="content" source="./media/container-insights-persistent-volumes/pv-workload-example.PNG" alt-text="Azure Monitor: Beispielworkbook für PV-Workload":::

### <a name="persistent-volume-details-workbook"></a>Arbeitsmappe mit Details zum persistenten Volume

Eine Übersicht über den PV-Bestand finden Sie in der Arbeitsmappe **Details zum persistenten Volume** direkt aus einem AKS-Cluster. Wählen Sie hierzu die Option „Arbeitsmappen“ aus. Diese befindet sich im linken Bereich sowie in der Dropdownliste **Arbeitsmappen anzeigen** und auf der Registerkarte **Berichte (Vorschau)** im Bereich „Erkenntnisse“.


:::image type="content" source="./media/container-insights-persistent-volumes/pv-details-workbook-example.PNG" alt-text="Azure Monitor: Beispielarbeitsmappe für PV-Details":::

### <a name="persistent-volume-usage-recommended-alert"></a>Empfohlene Warnung für die Nutzung des persistenten Volumes
Sie können eine empfohlene Warnung aktivieren, um benachrichtigt zu werden, wenn die durchschnittliche PV-Auslastung für einen Pod 80 Prozent übersteigt. Weitere Informationen zu Warnungen finden Sie [hier](./container-insights-metric-alerts.md). Informationen zum Überschreiben des Standardschwellenwerts finden Sie [hier](./container-insights-metric-alerts.md#configure-alertable-metrics-in-configmaps).
## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu erfassten PV-Metriken finden Sie [hier](./container-insights-agent-config.md).