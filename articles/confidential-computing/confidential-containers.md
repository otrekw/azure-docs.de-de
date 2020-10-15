---
title: Vertrauliche Container in Azure Kubernetes Service (AKS)
description: Erfahren Sie mehr über die Unterstützung von unveränderten Containern in vertraulichen Containern.
services: container-service
author: agowdamsft
ms.topic: article
ms.date: 9/22/2020
ms.author: amgowda
ms.service: container-service
ms.openlocfilehash: 528b843e24e1d63e4822c253b3636ef490e8fe8e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "90993492"
---
# <a name="confidential-containers"></a>Vertrauliche Container

## <a name="overview"></a>Übersicht

Ermöglichen Sie es Entwicklern, eine **vorhandene Docker-Anwendung (neu oder vorhanden)** mitzubringen und dank der Unterstützung von Confidential Computing-Knoten sicher in Azure Kubernetes Service (AKS) auszuführen.

Vertrauliche Container schützen Folgendes:

- Datenintegrität 
- Datenvertraulichkeit
- Codeintegrität
- Schutz des Containercodes durch Verschlüsselung
- Hardwarebasierte Sicherheit
- Ausführung von vorhandenen Apps
- Erstellen von Vertrauensanker in Hardware

Eine hardwarebasierte vertrauenswürdigen Ausführungsumgebung (Trusted Execution Environment, TEE) ist eine wichtige Komponente, die dank hardware- und softwarebasierter Messungen über TCB-Komponenten (Trusted Computing Base) umfassende Schutzmaßnahmen bietet. Die Überprüfung dieser Messungen hilft bei der Validierung des erwarteten Berechnungsergebnisses. Zudem wird damit kontrolliert, ob die Container-Apps manipuliert wurden.

Vertrauliche Container unterstützen benutzerdefinierte Anwendungen, die mit **Python, Java, Node JS usw. oder mit gepackten Softwareanwendungen wie NGINX, Redis Cache, MemCache usw. entwickelt wurden**, um in AKS unverändert ausgeführt zu werden.

Vertrauliche Container sind der schnellste Weg zu Containervertraulichkeit. Hierzu gehören Containerschutz durch Verschlüsselung sowie die Ermöglichung von Lift & Shift ohne oder mit nur minimalen Änderungen an der Geschäftslogik.

![Der Wechsel zu vertraulichen Containern](./media/confidential-containers/conf-con-deploy-process.jpg)


## <a name="confidential-container-enablers"></a>Grundvoraussetzungen für vertrauliche Container

Damit ein vorhandener Docker-Container ausgeführt werden kann, müssen Anwendungen auf Confidential Computing-Knoten über eine Abstraktionsschicht oder SGX-Software verfügen, um den speziellen CPU-Anweisungssatz nutzen zu können. Mit der SGX-Software kann auch der Code vertraulicher Anwendungen geschützt werden. Zudem wird damit eine direkte Ausführung auf der CPU erstellt, um Gast- oder Hostbetriebssysteme oder den Hypervisor zu entfernen. Durch diesen Schutz werden die Angriffsflächen und Sicherheitsrisiken bei Betriebssystem- oder Hypervisorebenen insgesamt verringert.

Vertrauliche Container werden in AKS vollständig unterstützt und durch Azure-Partner und OSS-Projekte (Open-Source-Software) ermöglicht. Entwickler können Softwareanbieter je nach Features, je nach Diensten zur Integration in Azure und je nach Toolunterstützung auswählen.

## <a name="partner-enablers"></a>Grundvoraussetzungen für Partner
> [!NOTE]
> Die folgenden Lösungen werden über Azure-Partner angeboten. Für diese Lösungen können Lizenzierungsgebühren anfallen. Prüfen Sie daher die Bestimmungen für jede einzelne Partnersoftware. 

### <a name="fortanix"></a>Fortanix

[Fortanix](https://www.fortanix.com/) bietet Entwicklern die Möglichkeit, ihre containerisierten Anwendungen in einem Portal oder in einer CLI-basierten Benutzeroberfläche zu nutzen und in SGX-fähige vertrauliche Container zu konvertieren, ohne sie ändern oder neu kompilieren zu müssen. Dadurch ist es möglich, mit Fortanix eine große Vielzahl unterschiedlichster Anwendungen flexibel auszuführen und zu verwalten. Das schließt auch bereits vorhandene Anwendungen, neue Enclave-native Anwendungen sowie vorab gepackte Anwendungen ein. Benutzer können über die Benutzeroberfläche von [Enclave Manager](https://em.fortanix.com/) oder mit [REST-APIs](https://www.fortanix.com/api/em/) mithilfe der [Kurzanleitung](https://support.fortanix.com/hc/en-us/articles/360049658291-Fortanix-Confidential-Container-on-Azure-Kubernetes-Service) für Azure Kubernetes Service vertrauliche Container erstellen.

![Bereitstellungsprozess mit Fortanix](./media/confidential-containers/fortanix-confidential-containers-flow.png)

### <a name="scone-scontain"></a>Scone (Scontain)

[SCONE](https://scontain.com/index.html?lang=en) unterstützt Sicherheitsrichtlinien, die Zertifikate, Schlüssel und Geheimnisse generieren können, und stellt sicher, dass diese nur für bestätigte Dienste einer Anwendung sichtbar sind. So bestätigen sich die Dienste einer Anwendung automatisch gegenseitig über TLS, ohne dass die Anwendung oder TLS geändert werden muss. Dies wird anhand einer einfachen Flask-Anwendung hier erläutert: https://sconedocs.github.io/flask_demo/  

SCONE kann die meisten vorhandenen Binärdateien in Anwendungen konvertieren, die innerhalb von Enclaves ausgeführt werden, ohne dass die Anwendung geändert oder neu kompiliert werden muss. Darüber hinaus schützt SCONE interpretierte Sprachen wie Python, da sowohl Datendateien als auch Python-Codedateien verschlüsselt werden. Mithilfe einer SCONE-Sicherheitsrichtlinie können die verschlüsselten Datei vor unbefugten Zugriffen, Änderungen und Rollbacks geschützt werden. Wie eine vorhandene Python-Anwendung „sconifiziert“ wird, wird [hier](https://sconedocs.github.io/sconify_image/) erläutert.

![Scontain-Workflow](./media/confidential-containers/scone-workflow.png)

Scone-Bereitstellungen auf Confidential Computing-Knoten mit AKS werden umfassend unterstützt und sind vollständig integriert. Beginnen Sie mit einer Beispielanwendung hier https://sconedocs.github.io/aks/.

### <a name="anjuna"></a>Anjuna

[Anjuna](https://www.anjuna.io/) stellt SGX-Plattformsoftware bereit, mit der Container unverändert in AKS ausgeführt werden können. Weitere Informationen zu dieser **neue** Funktion und den Benutzerflow finden Sie [hier](https://www.anjuna.io/microsoft-azure-confidential-computing-aks-lp).

Beginnen Sie mit einer Redis Cache-Beispielanwendung und einer benutzerdefinierten Python-Anwendung [hier](https://www.anjuna.io/microsoft-azure-confidential-computing-aks-lp).

![Anjuna-Prozess](./media/confidential-containers/anjuna-process-flow.png)

## <a name="oss-enablers"></a>Grundvoraussetzungen für OSS 
> [!NOTE]
> Die folgenden Lösungen werden über Open-Source-Projekte bereitgestellt und sind nicht direkt mit Azure Confidential Computing (ACC) oder Microsoft verbunden.  

### <a name="graphene"></a>Graphene

[Graphene](https://grapheneproject.io/) ist ein einfaches Gastbetriebssystem zur Ausführung einer einzelnen Linux-Anwendung mit minimalen Hostanforderungen. Mit Graphene können Anwendungen in einer isolierten Umgebung mit Vorteilen ausgeführt werden, die mit denen einer Ausführung in einem vollständigen Betriebssystem vergleichbar sind. Zudem bietet diese Lösung eine umfassende Toolunterstützung für die Konvertierung vorhandener Docker-Containeranwendungen in Graphene Shielded Containers (GSC).

Beginnen Sie [hier](https://graphene.readthedocs.io/en/latest/cloud-deployment.html#azure-kubernetes-service-aks) mit einer Beispielanwendung und -bereitstellung in AKS.

### <a name="occlum"></a>Occlum
[Occlum](https://occlum.io/) ist ein arbeitsspeichersicheres Multiprozess-Bibliotheksbetriebssystem (LibOS) für Intel SGX. Damit können ältere Anwendungen mit nur geringfügigen oder ganz ohne Änderungen am Quellcode unter SGX ausgeführt werden. Occlum schützt die Vertraulichkeit von Benutzerworkloads auf transparente Weise und erlaubt gleichzeitig eine einfache Migration per Lift & Shift zu vorhandenen Dockeranwendungen.

Occlum unterstützt AKS-Bereitstellungen. Befolgen Sie [hier](https://github.com/occlum/occlum/blob/master/docs/azure_aks_deployment_guide.md) die Anweisungen zur Bereitstellung anhand verschiedener Beispielanwendungen.


## <a name="confidential-containers-demo"></a>Demo zu vertraulichen Containern
Sehen Sie sich die Demo zu vertraulichen Gesundheitsdaten in vertraulichen Containern an. Das Beispiel ist [hier](https://github.com/Azure-Samples/confidential-container-samples/blob/main/confidential-healthcare-scone-confinf-onnx/README.md) verfügbar. 

> [!VIDEO https://www.youtube.com/embed/PiYCQmOh0EI]


## <a name="get-in-touch"></a>Setzen Sie sich mit uns in Verbindung

Haben Sie Fragen zu ihrer Implementierung, oder möchten Sie Enabler werden? Eine E-Mail an acconaks@microsoft.com senden

## <a name="reference-links"></a>Referenzlinks

[Microsoft Azure Attestation](../attestation/overview.md)

[Virtuelle DCsv2-Computer](virtual-machine-solutions.md)

[Azure Kubernetes Service (AKS)](../aks/intro-kubernetes.md)
