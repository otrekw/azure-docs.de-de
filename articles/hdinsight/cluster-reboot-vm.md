---
title: Neustarten von VMs für Azure HDInsight-Cluster
description: In diesem Artikel erfahren Sie, wie Sie nicht reagierende VMs für HDInsight-Cluster neu starten.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.service: hdinsight
ms.topic: how-to
ms.date: 06/22/2020
ms.openlocfilehash: c0f0bd9eb423b3de6a602647dff93fd9fce6e13e
ms.sourcegitcommit: 124f7f699b6a43314e63af0101cd788db995d1cb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86077013"
---
# <a name="reboot-vms-for-hdinsight-cluster"></a>Neustarten von VMs für HDInsight-Cluster

HDInsight-Cluster enthalten Gruppen von VMs als Clusterknoten. Bei zeitintensiven Clustern ist es möglich, dass diese Knoten aus verschiedenen Gründen nicht mehr reagieren. In diesem Artikel wird beschrieben, wie Sie nicht reagierende VMs in einem HDInsight-Cluster neu starten.

## <a name="when-to-reboot"></a>Situationen, in denen ein Neustart erforderlich ist

> [!WARNING]  
> Das Neustarten von VMs im Cluster bewirkt die Downtime des Knotens und einen Neustart der Dienste auf dem Knoten. 

Beim Neustarten des Knotens wird der Cluster möglicherweise fehlerhaft, und Aufträge können verlangsamt werden oder fehlschlagen. Wenn Sie versuchen, den aktiven Hauptknoten neu zu starten, werden alle ausgeführten Aufträge beendet, und Sie können erst dann wieder Aufträge an den Cluster senden, wenn die Dienste wieder aktiv sind und ausgeführt werden. Sie sollten VMs nur neu starten, wenn dies tatsächlich erforderlich ist. In den folgenden Fällen kann es erforderlich sein, dass VMs neu gestartet werden müssen.

- Sie können keine SSH-Verbindung mit dem Knoten herstellen, er reagiert jedoch auf Pings.
- Der Workerknoten ist ohne Heartbeat in der Ambari-Benutzeroberfläche ausgefallen.
- Der temporäre Datenträger auf dem Knoten ist voll.
- Die Prozesstabelle auf der VM enthält viele Einträge, in denen der Prozess abgeschlossen wurde, jedoch mit dem Status „Beendet“ aufgeführt ist.

> [!WARNING]  
> Sie sollten vorsichtiger beim Neustarten von VMs für **HBase**- und **Kafka**-Cluster sein, da dabei Datenverluste auftreten können.

## <a name="use-powershell-to-reboot-vms"></a>Verwenden von PowerShell zum Neustarten von VMs

Es sind zwei Schritte zum Ausführen des Neustartvorgangs für Knoten erforderlich: Auflisten von Knoten und Neustarten von Knoten.

1. Auflisten von Knoten. Sie können die Liste der Clusterknoten über [Get-AzHDInsightHost](https://docs.microsoft.com/powershell/module/az.hdinsight/get-azhdinsighthost) abrufen. 

  ```
  Get-AzHDInsightHost -ClusterName myclustername
  ```

2. Neustarten von Hosts. Starten Sie nach dem Abrufen der Namen der neu zu startenden Knoten die Knoten mithilfe von [Restart-AzHDInsightHost](https://docs.microsoft.com/powershell/module/az.hdinsight/restart-azhdinsighthost).

  ```
  Restart-AzHDInsightHost -ClusterName myclustername -Name wn0-myclus, wn1-myclus
  ```

## <a name="use-rest-api-to-reboot-vms"></a>Verwenden der REST-API zum Neustarten von VMs

Sie können das Feature **Ausprobieren** in der API-Dokumentation verwenden, um Anforderungen an HDInsight zu senden. Es sind zwei Schritte zum Ausführen des Neustartvorgangs für Knoten erforderlich: Auflisten von Knoten und Neustarten von Knoten.

1. Auflisten von Knoten. Sie können die Liste der Clusterknoten über die REST-API oder in Ambari abrufen. Weitere Informationen finden Sie unter [HDInsight: REST-API-Vorgang zum Auflisten von Hosts](https://docs.microsoft.com/rest/api/hdinsight/virtualmachines/listhosts).

    ```
    POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.HDInsight/clusters/{clusterName}/listHosts?api-version=2018-06-01-preview
    ```

2. Neustarten von Hosts. Verwenden Sie nach dem Abrufen der Namen der neu zu startenden Knoten die REST-API zum Neustarten der Knoten. Der Knotenname weist das Muster **„NodeType(wn/hn/zk/gw)“ + „x“ + „erste 6 Zeichen des Clusternamens“** auf. Weitere Informationen finden Sie unter [HDInsight: REST-API-Vorgang zum Neustarten von Hosts](https://docs.microsoft.com/rest/api/hdinsight/virtualmachines/restarthosts).

    ```
    POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.HDInsight/clusters/{clusterName}/restartHosts?api-version=2018-06-01-preview
    ```

Die tatsächlichen Namen der neu zu startenden Knoten werden in einem JSON-Array im Anforderungstext angegeben.

```json
[
  "wn0-abcdef",
  "zk1-abcdef"
]
```

## <a name="next-steps"></a>Nächste Schritte

* [Restart-AzHDInsightHost](https://docs.microsoft.com/powershell/module/az.hdinsight/restart-azhdinsighthost)
* [REST-API für HDInsight-VMs](https://docs.microsoft.com/rest/api/hdinsight/virtualmachines)
* [HDInsight-REST-API](https://docs.microsoft.com/rest/api/hdinsight/)
