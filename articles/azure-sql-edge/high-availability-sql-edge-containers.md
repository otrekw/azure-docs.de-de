---
title: Hochverfügbarkeit für Azure SQL Edge-Container – Azure SQL Edge
description: Hier erfahren Sie mehr über Hochverfügbarkeit für Azure SQL Edge-Container.
keywords: SQL Edge, Container, Hochverfügbarkeit
services: sql-edge
ms.service: sql-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 09/22/2020
ms.openlocfilehash: 56b9f06547f737bc05d573f98ce1dbac2ba48758
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90930417"
---
# <a name="high-availability-for-azure-sql-edge-containers"></a>Hochverfügbarkeit für Azure SQL Edge-Container

Erstellen und verwalten Sie Ihre Azure SQL Edge-Instanzen nativ in Kubernetes. Stellen Sie Azure SQL Edge in Docker-Containern bereit, die von [Kubernetes](https://kubernetes.io/) verwaltet werden. In Kubernetes kann ein Container mit einer Azure SQL Edge-Instanz automatisch wiederhergestellt werden, wenn ein Clusterknoten ausfällt. Sie können das SQL Edge-Containerimage mit einem PersistentVolumeClaim (PVC) in Kubernetes konfigurieren. Kubernetes überwacht den Azure SQL Edge-Prozess im Container. Wenn Prozess, Pod, Container oder Knoten ausfallen, startet Kubernetes automatisch eine andere Instanz und stellt die Verbindung zum Speicher wieder her.

## <a name="azure-sql-edge-containers-on-kubernetes"></a>Azure SQL Edge-Container in Kubernetes

Kubernetes 1.6 und höher unterstützt [*Speicherklassen*](https://kubernetes.io/docs/concepts/storage/storage-classes/) und [*PersistentVolumeClaims*](https://kubernetes.io/docs/concepts/storage/storage-classes/#persistentvolumeclaims).

In dieser Konfiguration spielt Kubernetes die Rolle des Containerorchestrators. 

![Diagramm mit Azure SQL Edge in einem Kubernetes-Cluster](media/deploy-kubernetes/kubernetes-sql-edge.png)

Im obigen Diagramm ist `azure-sql-edge` ein Container in einem [Pod](https://kubernetes.io/docs/concepts/workloads/pods/pod/). Kubernetes orchestriert die Ressourcen im Cluster. Eine [Replikatgruppe](https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/) stellt sicher, dass der Pod nach dem Ausfall eines Knotens automatisch wiederhergestellt wird. Anwendungen stellen eine Verbindung zum Dienst her. In diesem Fall stellt der Dienst einen Lastenausgleich dar, der eine IP-Adresse hostet, die sich nach dem Ausfall von `azure-sql-edge` nicht ändert.

Im folgenden Diagramm ist der `azure-sql-edge`-Container fehlgeschlagen. Als Orchestrator garantiert Kubernetes die richtige Anzahl fehlerfreier Instanzen in der Replikatgruppe und startet gemäß der Konfiguration einen neuen Container. Der Orchestrator startet einen neuen Pod auf demselben Knoten, und `azure-sql-edge` stellt erneut eine Verbindung mit demselben persistenten Speicher her. Der Dienst stellt eine Verbindung zur neu erstellen `azure-sql-edge`-Instanz her.

![Azure SQL Edge in einem Kubernetes-Cluster nach einem Podausfall](media/deploy-kubernetes/kubernetes-sql-edge-after-pod-fail.png)

Im folgenden Diagramm ist der Knoten fehlgeschlagen, der den `azure-sql-edge`-Container hostet. Der Orchestrator startet den neuen Pod auf einem anderen Knoten, und `azure-sql-edge` stellt erneut eine Verbindung mit demselben persistenten Speicher her. Der Dienst stellt eine Verbindung zur neu erstellen `azure-sql-edge`-Instanz her.

![Azure SQL Edge in einem Kubernetes-Cluster nach einem Knotenausfall](media/deploy-kubernetes/kubernetes-sql-edge-after-node-fail.png).

Informationen zur Erstellung eines Containers in Kubernetes finden Sie unter [Bereitstellen eines Azure SQL Edge-Containers in Kubernetes](deploy-Kubernetes.md).

## <a name="next-steps"></a>Nächste Schritte

Informationen zur Bereitstellung von Azure SQL Edge-Containern in Azure Kubernetes Service (AKS) finden Sie in den folgenden Artikeln:
- [Bereitstellen eines Azure SQL Edge-Containers in Kubernetes](deploy-Kubernetes.md)
- [Machine Learning und künstliche Intelligenz mit ONNX in SQL Edge](onnx-overview.md)
- [Entwickeln einer End-to-End-IoT-Lösung mit SQL Edge unter Verwendung von IoT Edge](tutorial-deploy-azure-resources.md)
- [Datenstreaming in Azure SQL Edge](stream-data.md)