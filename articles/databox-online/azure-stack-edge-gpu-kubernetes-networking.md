---
title: Grundlegendes zum Kubernetes-Netzwerk auf einem Azure Stack Edge Pro-Gerät | Microsoft-Dokumentation
description: Hier wird die Funktionsweise von Kubernetes-Netzwerken auf Azure Stack Edge Pro-Geräten beschrieben.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: conceptual
ms.date: 08/21/2020
ms.author: alkohli
ms.openlocfilehash: 001304ad6eda27db2285aaa9ad8b28929e2a04f8
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "90899311"
---
# <a name="kubernetes-networking-in-your-azure-stack-edge-pro-gpu-device"></a>Kubernetes-Netzwerke auf Ihrem Azure Stack Edge Pro-GPU-Gerät

Wenn Sie die Computerolle konfigurieren, wird auf Ihrem Azure Stack Edge Pro-Gerät ein Kubernetes-Cluster erstellt. Sobald der Kubernetes-Cluster erstellt wurde, können innerhalb des Kubernetes-Clusters Containeranwendungen in Pods bereitgestellt werden. Für die Pods in Ihrem Kubernetes-Cluster werden unterschiedliche Netzwerkfunktionen genutzt. 

In diesem Artikel werden die Netzwerkfunktionen in einem Kubernetes-Cluster sowohl allgemein als auch im Kontext Ihres Azure Stack Edge Pro-Geräts beschrieben. 

## <a name="networking-requirements"></a>Netzwerkanforderungen

Nachfolgend sehen Sie ein Beispiel einer typischen App mit 2 Ebenen, die im Kubernetes-Cluster bereitgestellt wird.

- Die App verfügt über ein Webserver-Front-End und eine Datenbankanwendung im Back-End. 
- Jedem Pod ist eine IP-Adresse zugewiesen. Diese IP-Adressen können sich beim Neustart und Failover des Pods jedoch ändern. 
- Jede App umfasst mehrere Pods, und für den Datenverkehr der Podreplikate sollte ein Lastenausgleich implementiert sein. 

![Kubernetes-Netzwerkanforderungen](./media/azure-stack-edge-gpu-kubernetes-networking/kubernetes-networking-1.png)

Für das oben gezeigte Szenario gelten die folgenden Netzwerkanforderungen:

 - Auf die Anwendung mit externem Zugriff muss über einen Namen oder eine IP-Adresse von einem Anwendungsbenutzer außerhalb des Kubernetes-Clusters zugegriffen werden können. 
 - Die Anwendungen innerhalb des Kubernetes-Clusters (z. B. die Front-End- und Back-End-Pods) müssen miteinander kommunizieren können.

Um beide Anforderungen zu erfüllen, wird ein Kubernetes-Dienst eingeführt. 


## <a name="networking-services"></a>Netzwerkdienste

Es gibt zwei Arten von Kubernetes-Diensten: 

- **Cluster-IP-Dienst**: Sie können sich dies als einen konstanten Endpunkt für die Anwendungspods vorstellen. Auf Pods, die mit diesen Diensten verknüpft sind, kann nicht von außerhalb des Kubernetes-Clusters zugegriffen werden. Die für diese Dienste verwendete IP-Adresse stammt aus dem Adressraum im privaten Netzwerk. 
- **Lastenausgleichs-IP**: Dieser Dienst ist mit dem Cluster-IP-Dienst vergleichbar, die verknüpfte IP-Adresse stammt jedoch aus dem externen Netzwerk, und es kann von außerhalb des Kubernetes-Clusters darauf zugegriffen werden.


<!--## Networking example for an app


![Kubernetes networking example](./media/azure-stack-edge-gpu-kubernetes-networking/kubernetes-networking-2.png)

Each of these applications pods has a label associated with it. For example, the web server application pods have a label `app = WS` and the service has a label selector which the same as `app = WS`. Whenever a service of type load balancer or cluster IP is created, there is a control loop that runs in the master and publishes an endpoint corresponding to this service. This service uses a combination of labels and label selectors to discover the pods associated with this service. As a pod gets created, the new endpoint for the pod is added to the endpoint mapping. Whenever a pod is deleted, it gets deleted from the endpoint mapping. Using this endpoint controller, the service has a most up-to-date view of the pods that make up this application.

For discovery of applications within the cluster, Kubernetes cluster has an inbuilt DNS server pod. This is a cluster DNS that resolves service names to cluster IP. Anytime a cluster IP service is created, a DNS record is added to the DNS server that maps the name of the service to the cluster internal IP. That is how the applications within the cluster can discover each other. For load balancing, there is also the `kube-proxy`. This runs on every node and captures the traffic that comes in through the cluster IP and then distributes the traffic across the pods. 

When an application or the end user would first use the IP address associated with the service of type load balancer to discover the service. Then it would use the label select `app = WS` to discover the pods associated with the application. The `kube-proxy` component would then distribute the traffic and ensure that it hits one of the web server application pods. If the web server app wanted to talk to the database app, then it would simply use the name of the service and using the name and the DNS server pod, resolve the name to an IP address. Again using labels and label selector, it would discover the pods associated with the database application. The `kube-proxy` would then distribute the traffic across each of the database app nodes.-->


## <a name="kubernetes-networking-on-azure-stack-edge-pro"></a>Kubernetes-Netzwerke auf Azure Stack Edge Pro-Geräten

Zur Bereitstellung der Netzwerkfunktionen auf Ihrem Azure Stack Edge Pro-Gerät werden die Komponenten Calico, Metallb und Core DNS installiert. 

- **Calico** weist jedem Pod eine IP-Adresse aus einem privaten IP-Adressbereich zu und konfiguriert Netzwerkfunktionen für diese Pods, um die Kommunikation zwischen Pods auf verschiedenen Knoten zu ermöglichen. 
- **Metallb** wird in einem Pod innerhalb des Clusters ausgeführt und weist Diensten vom Typ Lastenausgleich eine IP-Adresse zu. Die IP-Adressen für den Lastenausgleich werden aus dem Dienst-IP-Adressbereich ausgewählt, der über die lokale Benutzeroberfläche angegeben wird. 
- **Core DNS**: Mit diesem Add-On wird ein Dienst zur Zuordnung von DNS-Einträgen für Cluster-IP-Adressen konfiguriert.

Die IP-Adressen, die für Kubernetes-Knoten und die externen Dienste verwendet werden, werden auf der Seite **Computenetzwerk** auf der lokalen Benutzeroberfläche des Geräts angegeben.

![Kubernetes-IP-Adresszuweisung auf der lokalen Benutzeroberfläche](./media/azure-stack-edge-gpu-kubernetes-networking/kubernetes-ip-assignment-local-ui-1.png)

Die IP-Adresszuweisung gilt für:

- **IP-Adressen für Kubernetes-Knoten**: Dieser IP-Adressbereich wird für Kubernetes-Master- und -Workerknoten verwendet. Diese IP-Adressen werden für die Kommunikation zwischen Kubernetes-Knoten verwendet.

- **IP-Adressen externer Dienste für Kubernetes**: Dieser IP-Adressbereich wird für externe Dienste (auch als Lastenausgleichsdienste bezeichnet) verwendet, auf die von außerhalb des Kubernetes-Clusters zugegriffen werden kann.  


## <a name="next-steps"></a>Nächste Schritte

Lesen Sie die folgenden Artikel, um Informationen zum Konfigurieren der Kubernetes-Netzwerkfunktionen auf Ihrem Azure Stack Edge Pro-Gerät zu erhalten:

- [Externes Bereitstellen einer zustandslosen Anwendung auf Ihrem Azure Stack Edge Pro-Gerät über IoT Edge](azure-stack-edge-gpu-deploy-stateless-application-iot-edge-module.md).

- [Externes Bereitstellen einer zustandslosen Anwendung auf Ihrem Azure Stack Edge Pro-Gerät über kuebctl](azure-stack-edge-j-series-deploy-stateless-application-kubernetes.md).
