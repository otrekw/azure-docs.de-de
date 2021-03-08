---
title: Konfigurieren der PV-Überwachung mit Container Insights | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie die Überwachung von Kubernetes-Clustern mit persistenten Volumes mit Container Insights konfigurieren können.
ms.topic: conceptual
ms.date: 10/20/2020
ms.openlocfilehash: 0afbeab49a6909a0011cd75a0419f7325ca68132
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101713727"
---
# <a name="configure-pv-monitoring-with-container-insights"></a>Konfigurieren der PV-Überwachung mit Container Insights

Ab der Agent-Version *ciprod10052020* unterstützt der integrierte Agent von Container Insights die Überwachung der Nutzung persistenter Volumes (PVs).

## <a name="pv-metrics"></a>PV-Metriken

Container Insights beginnt automatisch mit der Überwachung von PVs. Hierzu werden die folgenden Metriken im 60-Sekunden-Takt gesammelt und in der Tabelle **InsightMetrics** gespeichert:

|Metrikname |Metrikdimension (Tags) |Beschreibung |
|------------|------------------------|------------|
| `pvUsedBytes`|`container.azm.ms/pv`|Verwendeter Speicherplatz in Bytes für ein bestimmtes persistentes Volume mit einem von einem bestimmten Pod verwendeten Anspruch. `pvCapacityBytes` wird als Dimension in das Feld „Tags“ integriert, um die Datenerfassungskosten zu verringern und Abfragen zu vereinfachen.|

## <a name="monitor-persistent-volumes"></a>Überwachen persistenter Volumes

Container Insights enthält vorkonfigurierte Diagramme für diese Metrik in einer Arbeitsmappe für jeden Cluster. Die Diagramme befinden sich in der Arbeitsmappe **Workloaddetails** auf der Registerkarte „Persistentes Volume“. Dorthin können Sie direkt von einem AKS-Cluster aus navigieren, indem Sie im linken Bereich die Option „Arbeitsmappen“ auswählen. Alternativ können Sie in der Erkenntnis die Dropdownliste **Arbeitsmappen anzeigen** verwenden. Sie können auch eine empfohlene Warnung für die PV-Nutzung aktivieren sowie diese Metriken in Log Analytics abfragen.  

![Azure Monitor: Beispielworkbook für PV-Workload](./media/container-insights-persistent-volumes/pv-workload-example.PNG)

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu erfassten PV-Metriken finden Sie [hier](./container-insights-agent-config.md).
