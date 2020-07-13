---
title: Containersicherheit in Azure Security Center | Microsoft-Dokumentation
description: Hier erfahren Sie mehr über die Containersicherheitsfeatures von Security Center.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2020
ms.author: memildin
ms.openlocfilehash: c01ed6dbbd6e1f7febfb99df11d2ee67cb1e5465
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85800601"
---
# <a name="container-security-in-security-center"></a>Containersicherheit in Security Center

Azure Security Center ist die native Azure-Lösung zum Sichern Ihrer Container. Security Center kann die folgenden Containerressourcentypen schützen:



|Resource |Name  |Details  |
|:---------:|---------|---------|
|![Containerhost](./media/security-center-virtual-machine-recommendations/icon-container-host-rec.png)|Containerhosts (virtuelle Computer, die Docker ausführen)|Security Center scannt Ihre Docker-Konfigurationen und bietet Ihnen durch die Bereitstellung einer Liste aller festgestellten Regelverstöße Einblick in die Fehlkonfigurationen. Security Center bietet Richtlinien, mit denen Sie diese Probleme schnell beheben und Zeit sparen können. Security Center bewertet fortlaufend die Docker-Konfigurationen und unterrichtet Sie über deren aktuellen Zustand.|
|![Kubernetes-Dienst](./media/security-center-virtual-machine-recommendations/icon-kubernetes-service-rec.png)|AKS-Cluster (Azure Kubernetes Service)|Gewinnen Sie mit dem [optionalen AKS-Paket von Security Center](azure-kubernetes-service-integration.md) für Benutzer des Standard-Tarifs einen tieferen Einblick in Ihre AKS-Knoten, den Clouddatenverkehr und die Sicherheitssteuerungen.|
|![Containerregistrierung](./media/security-center-virtual-machine-recommendations/icon-container-registry-rec.png)|Azure Container Registry-Registrierungen (ACR)|Verschaffen Sie sich tiefere Einblicke in die Sicherheitsrisiken der Images in ihren ARM-basierten ACR-Registrierungen, indem Sie das [optionale ACR-Paket von Security Center](azure-kubernetes-service-integration.md) für Benutzer des Standard-Tarifs verwenden.|
||||


In diesem Artikel wird beschrieben, wie Sie diese Pakete verwenden können, um die Sicherheit ihrer Container und deren Apps zu verbessern, zu überwachen und zu verwalten. Sie erfahren, wie Security Center diese Kernaspekte der Containersicherheit unterstützt:

- [Verwaltung von Sicherheitsrisiken: Scannen von Containerimages](#vulnerability-management---scanning-container-images)
- [Umgebungshärtung: kontinuierliche Überwachung ihrer Docker-Konfiguration und Kubernetes-Cluster](#environment-hardening)
- [Laufzeitschutz: Bedrohungserkennung in Echtzeit](#run-time-protection---real-time-threat-detection)

[![Registerkarte „Containersicherheit“ in Azure Security Center](media/container-security/container-security-tab.png)](media/container-security/container-security-tab.png#lightbox)

Anweisungen zur Verwendung dieser Features finden Sie unter [Überwachen der Sicherheit Ihrer Container](monitor-container-security.md).

## <a name="vulnerability-management---scanning-container-images"></a>Verwaltung von Sicherheitsrisiken: Scannen von Containerimages
Stellen Sie zur Überwachung Ihrer ARM-basierten Azure Container Registry-Instanz sicher, dass Sie den Standard-Tarif von Security Center verwenden (siehe [Preise](/azure/security-center/security-center-pricing)). Aktivieren Sie anschließend das optionale Paket „Containerregistrierungen“. Wenn ein neues Image per Push bereitgestellt wird, überprüft Security Center das Image mithilfe eines Scanners von Qualys, dem branchenführenden Hersteller von Anwendungen zur Sicherheitsrisikoüberprüfung.

Werden Probleme gefunden – von Qualys oder Security Center –, werden Sie auf dem Security Center-Dashboard benachrichtigt. Für jedes Sicherheitsrisiko bietet Security Center Handlungsempfehlungen sowie eine Klassifizierung des Schweregrads und Anleitungen für die Behebung des Problems. Ausführliche Informationen zu Security Center-Empfehlungen für Container finden Sie in der [Referenzliste der Empfehlungen](recommendations-reference.md#recs-containers).

Security Center filtert und klassifiziert die Ergebnisse des Scanners. Wenn ein Image fehlerfrei ist, markiert Security Center es entsprechend. Security Center generiert Sicherheitsempfehlungen nur für Images, bei denen Probleme behoben werden müssen. Indem Sie nur benachrichtigt werden, wenn Probleme auftreten, reduziert Security Center das Potenzial von unerwünschten Informationswarnungen.

## <a name="environment-hardening"></a>Umgebungshärtung

### <a name="continuous-monitoring-of-your-docker-configuration"></a>Kontinuierliche Überwachung ihrer Docker-Konfiguration
Azure Security Center identifiziert nicht verwaltete Container, die auf IaaS-Linux-VMs oder anderen Linux-Computern gehostet werden, auf denen Docker-Container ausgeführt werden. Security Center bewertet kontinuierlich die Konfigurationen dieser Container. Anschließend werden sie mit dem [Docker-Benchmark von Center for Internet Security (CIS)](https://www.cisecurity.org/benchmark/docker/) verglichen.

Security Center umfasst den gesamten Regelsatz des CIS-Docker-Benchmark und benachrichtigt Sie, sobald Ihre Container eine der Steuerungen nicht erfüllen. Werden Fehlkonfigurationen gefunden, generiert Security Center Sicherheitsempfehlungen. Verwenden Sie die Seite **Empfehlungen**, um Empfehlungen anzuzeigen und Probleme zu beheben. Die Empfehlungen werden auch auf der Registerkarte **Container** angezeigt, auf der alle mit Docker bereitgestellten virtuellen Computer aufgeführt sind. 

Ausführliche Informationen zu den möglichen relevanten Security Center-Empfehlungen für dieses Feature finden Sie im Abschnitt [Container](recommendations-reference.md#recs-containers) der Referenztabelle zu Empfehlungen.

Wenn Sie die Sicherheitsprobleme auf einem virtuellen Computer untersuchen, stellt Security Center zusätzliche Informationen zu den Containern auf dem Computer bereit. Zu diesen Informationen gehören die Docker-Version und Anzahl der auf dem Host ausgeführten Images. 

>[!NOTE]
> Diese CIS-Benchmarkprüfungen können nicht auf von AKS oder Databricks verwalteten virtuellen Computern ausgeführt werden.

### <a name="continuous-monitoring-of-your-kubernetes-clusters"></a>Fortlaufende Überwachung ihrer Kubernetes-Cluster
Security Center kann zusammen mit Azure Kubernetes Service (AKS) verwendet werden, dem verwalteten Containerorchestrierungsdienst von Microsoft für die Entwicklung, Bereitstellung und Verwaltung von Anwendungen in Containern.

AKS bietet Sicherheitssteuerungen und Einblicke in den Sicherheitsstatus ihrer Cluster. Security Center verwendet diese Features für Folgendes:
* Konstante Überwachung der Konfiguration Ihrer AKS-Cluster
* Generieren von an Branchenstandards ausgerichteten Sicherheitsempfehlungen

Ausführliche Informationen zu den möglichen relevanten Security Center-Empfehlungen für dieses Feature finden Sie im Abschnitt [Container](recommendations-reference.md#recs-containers) der Referenztabelle zu Empfehlungen.

## <a name="run-time-protection---real-time-threat-detection"></a>Laufzeitschutz: Bedrohungserkennung in Echtzeit

[!INCLUDE [AKS in ASC threat protection](../../includes/security-center-azure-kubernetes-threat-protection.md)]




## <a name="next-steps"></a>Nächste Schritte

In dieser Übersicht haben Sie die Kernelemente der Containersicherheit in Azure Security Center kennengelernt. Fahren Sie mit [Überwachen der Sicherheit Ihrer Container](monitor-container-security.md) fort.
> [!div class="nextstepaction"]
> [Überwachen der Sicherheit Ihrer Container](monitor-container-security.md)