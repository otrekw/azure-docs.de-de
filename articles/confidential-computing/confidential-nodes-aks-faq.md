---
title: Häufig gestellte Fragen zur Unterstützung von vertraulichen Knoten in Azure Kubernetes Service (AKS)
description: Hier finden Sie Antworten auf einige häufig gestellte Fragen zur Unterstützung von Azure Kubernetes Service (AKS)- und Azure Confidential Computing (ACC)-Knoten.
author: agowdamsft
ms.service: container-service
ms.subservice: confidential-computing
ms.topic: conceptual
ms.date: 02/09/2020
ms.author: amgowda
ms.openlocfilehash: 94b585078ce5e78a658fd8f110f09963200010b6
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105933558"
---
# <a name="frequently-asked-questions-about-confidential-computing-nodes-on-azure-kubernetes-service-aks"></a>Häufig gestellte Fragen zu Confidential Computing-Knoten in Azure Kubernetes Service (AKS)

In diesem Artikel werden häufig gestellte Fragen zu Intel SGX-basierten Confidential Computing-Knoten in Azure Kubernetes Service (AKS) behandelt. Falls Sie weitere Fragen haben, senden Sie eine E-Mail an **acconaks@microsoft.com** .

<a name="1"></a>
### <a name="are-the-confidential-computing-nodes-on-aks-in-ga"></a>Sind die Confidential Computing-Knoten in AKS allgemein verfügbar? ###
Ja

<a name="2"></a>
### <a name="what-is-attestation-and-how-can-we-do-attestation-of-apps-running-in-enclaves"></a>Was ist ein Nachweis, und wie können wir einen Nachweis von Apps in Enklaven führen? ###
Nachweis ist der Prozess zur Veranschaulichung und Überprüfung, dass ein Softwareelement auf der jeweiligen Hardwareplattform ordnungsgemäß instanziiert wurde. Außerdem stellt er sicher, dass sein Beweis überprüft werden kann, um zu gewährleisten, dass er auf einer sicheren Plattform ausgeführt wird und nicht manipuliert wurde. [Informieren Sie sich genauer](attestation.md) dazu, wie ein Nachweis bei Enklaven-Apps geführt wird.

<a name="3"></a>
### <a name="can-i-enable-accelerated-networking-with-azure-confidential-computing-aks-clusters"></a>Kann ich den beschleunigten Netzwerkbetrieb mit AKS-Clustern für Azure Confidential Computing aktivieren? ###
Nein. Beschleunigter Netzwerkbetrieb wird auf virtuellen DCSv2-Computern, die Confidential Computing-Knoten in AKS bilden, nicht unterstützt. 

<a name="4"></a>
### <a name="can-i-bring-my-existing-containerized-applications-and-run-it-on-aks-with-azure-confidential-computing"></a>Kann ich meine vorhandenen Containeranwendungen in AKS mit Azure Confidential Computing ausführen? ###
Ja. Weitere Informationen zu den Grundvoraussetzungen für Plattformen finden Sie auf der [Seite „Vertrauliche Container“](confidential-containers.md).

<a name="5"></a>
### <a name="what-version-of-intel-sgx-driver-version-is-on-the-aks-image-for-confidential-nodes"></a>Welche Intel SGX-Treiberversion ist im AKS-Image für vertrauliche Knoten verfügbar? ### 
Zurzeit sind Azure Confidential Computing DCSv2-VMs bei Intel SGX DCAP 1.33 installiert. 

<a name="6"></a>
### <a name="can-i-inject-post-install-scriptscustomize-drivers-to-the-nodes-provisioned-by-aks"></a>Kann ich Skripts nach der Installation einfügen/Treiber an die von AKS bereitgestellten Knoten anpassen? ###
Nein [Auf der AKS-Engine basierende Confidential Computing-Knoten](https://github.com/Azure/aks-engine/blob/master/docs/topics/sgx.md) unterstützen Confidential Computing-Knoten, die benutzerdefinierte Installationen zulassen und die vollständige Kontrolle über Ihre Kubernetes-Steuerungsebene haben.
<a name="7"></a>

### <a name="should-i-be-using-a-docker-base-image-to-get-started-on-enclave-applications"></a>Sollte ich für den Einstieg in Enklavenanwendungen ein Docker-Basisimage verwenden? ###
Verschiedene Grundvoraussetzungen (ISVs und OSS-Projekte) bieten Möglichkeiten zum Aktivieren von vertraulichen Containern. Weitere Details und individuelle Verweise auf Implementierungen finden Sie auf der [Seite „Vertrauliche Container“](confidential-containers.md).

<a name="8"></a>
### <a name="can-i-run-acc-nodes-with-other-standard-aks-skus-build-a-heterogenous-node-pool-cluster"></a>Kann ich ACC-Knoten bei anderen Standard-AKS-SKUs ausführen (einen heterogenen Knotenpoolcluster erstellen)? ###

Ja. Sie können unterschiedliche Knotenpools innerhalb desselben AKS-Clusters ausführen, einschließlich ACC-Knoten. Wenn Sie Ihre Enklavenanwendungen in einem bestimmten Knotenpool als Ziel verwenden möchten, ziehen Sie das Hinzufügen von Knotenselektoren oder Anwenden von EPC-Limits in Betracht. Weitere Informationen finden Sie im Schnellstart für vertrauliche Knoten [hier](confidential-nodes-aks-get-started.md).

<a name="9"></a>
### <a name="can-i-run-windows-nodes-and-windows-containers-with-acc"></a>Kann ich Windows-Knoten und Windows-Container bei ACC ausführen? ###
Derzeit leider nicht. Wenden Sie sich unter *acconaks@microsoft.com* an das Produktteam, wenn Sie Unterstützung für Windows-Knoten oder -Container benötigen. 

<a name="10"></a>
### <a name="what-if-my-container-size-is-more-than-available-epc-memory"></a>Was geschieht, wenn mein Container größer als der verfügbare EPC-Speicher ist? ###
Der EPC-Speicher bezieht sich auf den Teil Ihrer Anwendung, der zur Ausführung in der Enklave programmiert wurde. Die Gesamtgröße Ihres Containers ist nicht die richtige Methode für einen Vergleich mit dem maximal verfügbaren EPC-Speicher. Tatsächlich ermöglichen DCSv2-Computer mit SGX einen maximalen VM-Arbeitsspeicher von 32 GB, in dem Ihr nicht vertrauenswürdiger Teil der Anwendung genutzt würde. Wenn Ihr Container aber mehr als den verfügbaren EPC-Speicher verbraucht, könnte die Leistung des Programmteils, der in der Enklave ausgeführt wird, dadurch beeinträchtigt werden.

Zur besseren Verwaltung des EPC-Speichers in den Workerknoten sollten Sie die EPC-speicherbasierte Verwaltung von Grenzwerten über Kubernetes in Erwägung ziehen. Nutzen Sie das folgende Beispiel als Referenz.

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: sgx-test
  labels:
    app: sgx-test
spec:
  template:
    metadata:
      labels:
        app: sgx-test
    spec:
      containers:
      - name: sgxtest
        image: oeciteam/sgx-test:1.0
        resources:
          limits:
            kubernetes.azure.com/sgx_epc_mem_in_MiB: 10 # This limit will automatically place the job into confidential computing node. Alternatively, you can target deployment to nodepools
      restartPolicy: Never
  backoffLimit: 0
```
<a name="11"></a>
### <a name="what-happens-if-my-enclave-consumes-more-than-maximum-available-epc-memory"></a>Was geschieht, wenn meine Enklave mehr als den maximal verfügbaren EPC-Speicher verbraucht? ###

Der gesamte verfügbare EPC-Speicher wird von den Enklavenanwendungen auf denselben VMs oder Workerknoten gemeinsam genutzt. Wenn Ihre Anwendung mehr als den verfügbaren EPC-Speicher nutzt, wirkt sich dies möglicherweise auf ihre Leistung aus. Deshalb empfehlen wir, die Toleranz pro Anwendung in Ihrer YAML-Datei für die Bereitstellung festzulegen, um den verfügbaren EPC-Speicher pro Workerknoten besser verwalten zu können, wie in den vorstehenden Beispielen gezeigt wird. Alternativ können Sie jederzeit wahlweise auf den VM-Größen des Workerknotenpools nach oben navigieren oder weitere Knoten hinzufügen. 

<a name="12"></a>
### <a name="why-cant-i-do-forks--and-exec-to-run-multiple-processes-in-my-enclave-application"></a>Warum kann ich keine Forks () und „exec“ zur Ausführung mehrerer Prozesse in meiner Enklavenanwendung ausführen? ###

Azure Confidential Computing DCsv2 SKU-VMs unterstützen zurzeit einen einzigen Adressraum für das Programm, das in einer Enklave ausgeführt wird. Ein einzelner Prozess ist eine aktuelle Einschränkung, die mit dem Fokus auf hohe Sicherheit konzipiert wurde. Bei den Grundvoraussetzungen für vertrauliche Container gibt es jedoch möglicherweise alternative Implementierungen, um diese Einschränkung zu umgehen.
<a name="13"></a>
### <a name="do-you-automatically-install-any-additional-daemonset-to-expose-the-sgx-drivers"></a>Installieren Sie automatisch zusätzliche DaemonSets, um die SGX-Treiber verfügbar zu machen? ###

Ja. Der Name des DaemonSets lautet „sgx-device-plugin“. Weitere Informationen zu den jeweiligen Zwecken finden Sie [hier](confidential-nodes-aks-overview.md).  

<a name="14"></a>
### <a name="what-is-the-vm-sku-i-should-be-choosing-for-confidential-computing-nodes"></a>Welche VM-SKU sollte ich für Confidential Computing-Knoten wählen? ###

DCSv2-SKUs. Die [DCSv2-SKUs](../virtual-machines/dcv2-series.md) stehen in den [unterstützten Regionen](https://azure.microsoft.com/global-infrastructure/services/?products=virtual-machines&regions=all) zur Verfügung.

<a name="15"></a>
### <a name="can-i-still-schedule-and-run-non-enclave-containers-on-confidential-computing-nodes"></a>Kann ich Nicht-Enklavencontainer auf Confidential Computing-Knoten weiterhin planen und ausführen? ###

Ja. Die VMs haben auch einen regulären Arbeitsspeicher, der Standardcontainer-Workloads ausführen kann. Berücksichtigen Sie das Sicherheits- und Bedrohungsmodell Ihrer Anwendungen, bevor Sie über die Bereitstellungsmodelle entscheiden.
<a name="16"></a>

### <a name="can-i-provision-aks-with-dcsv2-node-pools-through-azure-portal"></a>Kann ich AKS mit DCSv2-Knotenpools über das Azure-Portal bereitstellen? ###

Ja. Die Azure CLI könnte auch als Alternative verwendet werden, wie [hier](confidential-nodes-aks-get-started.md) beschrieben wird.

<a name="17"></a>
### <a name="what-ubuntu-version-and-vm-generation-is-supported"></a>Welche Ubuntu-Version und VM-Generation wird unterstützt? ###
18.04 für Gen 2. 

<a name="18"></a>
### <a name="can-we-change-the-current-intel-sgx-dcap-diver-version-on-aks"></a>Können wir die aktuelle Intel SGX DCAP-Treiberversion für AKS ändern? ###

Nein Wir empfehlen Ihnen zur Ausführung von benutzerdefinierten Installationen, Bereitstellungen von [AKS-Engine Confidential Computing-Workerknoten ](https://github.com/Azure/aks-engine/blob/master/docs/topics/sgx.md) zu wählen. 

<a name="19"></a>

### <a name="what-version-of-kubernetes-do-you-support-and-recommend"></a>Welche Kubernetes-Version wird unterstützt und empfohlen? ###

Wir unterstützen und empfehlen die Kubernetes-Version 1.16 und höher. 

<a name="20"></a>
### <a name="what-are-the-known-current-limitations-of-the-product"></a>Was sind die aktuellen Einschränkungen des Produkts? ###

- Unterstützt nur VM-Knoten für Ubuntu 18.04 Gen 2 
- Keine Unterstützung für Windows-Knoten oder Windows-Container
- EPC-speicherbasierte horizontale automatische Podskalierung wird nicht unterstützt. CPU und reguläre speicherbasierte Skalierung wird unterstützt.
- Dev Spaces in AKS für vertrauliche Apps wird zurzeit nicht unterstützt.

<a name="21"></a>
### <a name="will-only-signed-and-trusted-images-be-loaded-in-the-enclave-for-confidential-computing"></a>Werden nur signierte und vertrauenswürdige Images in die Enklave für Confidential Computing geladen? ###
Nicht nativ während der Enklaveninitialisierung, die Signatur kann jedoch per Nachweis überprüft werden. Weitere Informationen finden Sie [hier](../attestation/basic-concepts.md#benefits-of-policy-signing). 

### <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu vertraulichen Containern finden Sie auf der [Seite „Vertrauliche Container“](confidential-containers.md).
