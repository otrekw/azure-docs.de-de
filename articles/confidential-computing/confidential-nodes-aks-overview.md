---
title: 'Confidential Computing-Knoten in Azure Kubernetes Service (AKS): Public Preview'
description: Confidential Computing-Knoten in AKS
services: virtual-machines
author: agowdamsft
ms.service: container-service
ms.topic: overview
ms.date: 9/22/2020
ms.author: amgowda
ms.openlocfilehash: 2aa30f86b32005b9c85664b5bb2d0772a6e5f443
ms.sourcegitcommit: 541bb46e38ce21829a056da880c1619954678586
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2020
ms.locfileid: "91940768"
---
# <a name="confidential-computing-nodes-on-azure-kubernetes-service-public-preview"></a>Confidential Computing-Knoten in Azure Kubernetes Service (Public Preview)

Mit [Azure Confidential Computing](overview.md) können Sie Ihre vertraulichen Daten während der Nutzung schützen. Mit den zugrunde liegenden Infrastrukturen werden diese Daten vor anderen Anwendungen, Administratoren und Cloudanbietern geschützt. 

## <a name="overview"></a>Übersicht

Azure Kubernetes Service (AKS) unterstützt das Hinzufügen von [DCsv2 Confidential Computing-Knoten](confidential-computing-enclaves.md) für Intel SGX. Auf diesen Knoten werden vertrauliche Workloads in einer hardwarebasierten vertrauenswürdigen Ausführungsumgebung (Trusted Execution Environment, TEE) ausgeführt, indem für Code auf Benutzerebene das Zuordnen von privaten Arbeitsspeicherregionen zugelassen wird. Diese Regionen des privaten Arbeitsspeichers werden als Enklaven bezeichnet. Enklaven sind dafür ausgelegt, Code und Daten vor Prozessen zu schützen, die mit einer höheren Berechtigungsstufe ausgeführt werden. Mit dem SGX-Ausführungsmodell werden die Zwischenschichten des Gastbetriebssystems und von Hypervisor entfernt. Dies ermöglicht Ihnen die Ausführung von Containeranwendungen direkt über die CPU, während der spezielle Arbeitsspeicherblock verschlüsselt bleibt. 


![Übersicht über SGX-Knoten](./media/confidential-nodes-aks-overview/sgxaksnode.jpg)

## <a name="aks-confidential-nodes-features"></a>Features von vertraulichen AKS-Knoten

- Hardwarebasierte Containerisolation auf Prozessebene über eine vertrauenswürdige SGX-Ausführungsumgebung (Trusted Execution Environment, TEE) 
- Heterogene Knotenpoolcluster (Mischung aus vertraulichen und nicht vertraulichen Knotenpools)
- Arbeitsspeicherbasierte Podplanung mit verschlüsseltem Seitencache (Encrypted Page Cache, EPC)
- SGX DCAP-Treiber vorinstalliert
- Intel FSGS-Patch vorinstalliert
- Unterstützung der auf der CPU-Auslastung basierenden automatischen horizontalen Podskalierung und automatischen Clusterskalierung
- Hilfsprogramm für Out-of-Proc-Nachweis über AKS-DaemonSet
- Unterstützung für Linux-Container über Ubuntu 18.04 Gen2-VM-Workerknoten

## <a name="aks-provided-daemon-sets"></a>Von AKS bereitgestellte DaemonSets

#### <a name="sgx-device-plugin"></a>SGX-Geräte-Plug-In <a id="sgx-plugin"></a>

Mit dem SGX-Geräte-Plug-In wird die Plug-In-Schnittstelle des Kubernetes-Geräts für den EPC-Arbeitsspeicher implementiert. Mit diesem Plug-In wird der EPC-Arbeitsspeicher quasi zu einem zusätzlichen Ressourcentyp in Kubernetes. Benutzer können Grenzwerte für diese Ressource genauso wie für andere Ressourcen angeben. Neben der Planungsfunktion ermöglicht das Geräte-Plug-In auch die Zuweisung von SGX-Gerätetreiberberechtigungen für vertrauliche Workloadcontainer. Eine Beispielimplementierung der arbeitsspeicherbasierten EPC-Bereitstellung (`kubernetes.azure.com/sgx_epc_mem_in_MiB`) finden Sie [hier](https://github.com/Azure-Samples/confidential-computing/blob/main/containersamples/helloworld/helm/templates/helloworld.yaml).

#### <a name="sgx-quote-helper-service"></a>SGX-Angebotshilfsdienst <a id="sgx-quote"></a>

Für Enklavenanwendungen, bei denen ein Remotenachweis durchgeführt wird, muss ein ANGEBOT erstellt werden. Das ANGEBOT liefert den kryptografischen Nachweis der Identität und des Status der Anwendung sowie für die Umgebung, in der die Enklave ausgeführt wird. Die Erstellung des ANGEBOTS basiert auf bestimmten vertrauenswürdigen Softwarekomponenten von Intel, die Teil der SGX-Plattformsoftwarekomponenten (PSW/DCAP) sind. Diese Plattformsoftwarekomponenten werden als DaemonSet verpackt, das pro Knoten ausgeführt wird. Es kann beim Anfordern des ANGEBOTS von Enklaven-Apps zu Nachweiszwecken genutzt werden. Durch die Nutzung des von AKS bereitgestellten Diensts kann die Kompatibilität zwischen den Plattformsoftwarekomponenten und anderen Softwarekomponenten auf dem Host besser verwaltet werden. Lesen Sie die [weiteren Informationen](confidential-nodes-out-of-proc-attestation.md) zur Nutzung und zu den Featuredetails.

## <a name="programming--application-models"></a>Programmier- und Anwendungsmodelle

### <a name="confidential-containers"></a>Vertrauliche Container

In [vertraulichen Containern](confidential-containers.md) werden vorhandene Programme und die meisten Runtimes **gängiger Programmiersprachen** (Python, Node, Java usw.) sowie die jeweils vorhandenen Bibliotheksabhängigkeiten ausgeführt, ohne dass Quellcode geändert oder eine erneute Kompilierung durchgeführt werden muss. Dieses Modell ist das schnellste Modell mit aktivierter Vertraulichkeit über Open-Source-Projekte und Azure-Partner. Die Containerimages, die für die Ausführung in den sicheren Enklaven erstellt werden, werden als vertrauliche Container bezeichnet.

### <a name="enclave-aware-containers"></a>Für Enklaven geeignete Container

AKS unterstützt Anwendungen, die für die Ausführung auf vertraulichen Knoten programmiert sind und für die **spezielle Anweisungssätze** über die SDKs und Frameworks verfügbar gemacht werden. Dieses Anwendungsmodell ermöglicht die beste Kontrolle über Ihre Anwendungen mit der kleinsten vertrauenswürdigen Computerbasis (Trusted Computing Base, TCB). [Erfahren Sie mehr](enclave-aware-containers.md) zu Containern, die für Enklaven geeignet sind.

## <a name="next-steps"></a>Nächste Schritte

[Bereitstellen eines AKS-Clusters mit Confidential Computing-Knoten](./confidential-nodes-aks-get-started.md)

[Schnellstart: Beispiele für vertrauliche Container](https://github.com/Azure-Samples/confidential-container-samples)

[DCsv2: SKU-Liste](https://docs.microsoft.com/azure/virtual-machines/dcv2-series)

<!-- LINKS - external -->
[Azure Attestation]: https://docs.microsoft.com/en-us/azure/attestation/


<!-- LINKS - internal -->
[DC Virtual Machine]: /confidential-computing/virtual-machine-solutions