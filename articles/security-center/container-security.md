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
ms.date: 02/11/2020
ms.author: memildin
ms.openlocfilehash: fac9cba28f90f3642de660ed7d070b165c06bb2e
ms.sourcegitcommit: f915d8b43a3cefe532062ca7d7dbbf569d2583d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2020
ms.locfileid: "78303255"
---
# <a name="container-security-in-security-center"></a>Containersicherheit in Security Center

Azure Security Center ist die native Azure-Lösung für die Containersicherheit. Security Center ist auch die optimale zentralisierte Benutzeroberfläche für die Sicherheit Ihrer Cloudworkloads, VMs, Server und Container.

In diesem Artikel wird beschrieben, wie Security Center dabei hilft, die Sicherheit Ihrer Container und deren Apps zu verbessern, zu überwachen und zu verwalten. Sie erfahren, wie Security Center diese Kernaspekte der Containersicherheit unterstützt:

* Verwaltung von Sicherheitsrisiken
* Härtung der Containerumgebung
* Laufzeitschutz

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

### <a name="continuous-monitoring-of-your-kubernetes-clusters-preview"></a>Fortlaufende Überwachung ihrer Kubernetes-Cluster (Vorschau)
Security Center kann zusammen mit Azure Kubernetes Service (AKS) verwendet werden, dem verwalteten Containerorchestrierungsdienst von Microsoft für die Entwicklung, Bereitstellung und Verwaltung von Anwendungen in Containern.

AKS bietet Sicherheitssteuerungen und Einblicke in den Sicherheitsstatus ihrer Cluster. Security Center verwendet diese Features für Folgendes:
* Konstante Überwachung der Konfiguration Ihrer AKS-Cluster
* Generieren von an Branchenstandards ausgerichteten Sicherheitsempfehlungen

Ausführliche Informationen zu den möglichen relevanten Security Center-Empfehlungen für dieses Feature finden Sie im Abschnitt [Container](recommendations-reference.md#recs-containers) der Referenztabelle zu Empfehlungen.

## <a name="run-time-protection---real-time-threat-detection"></a>Laufzeitschutz: Bedrohungserkennung in Echtzeit

Security Center bietet eine Echtzeit-Bedrohungserkennung für Ihre Containerumgebungen und generiert Warnungen für verdächtige Aktivitäten. Mit diesen Informationen können Sie schnell Sicherheitsprobleme lösen und die Sicherheit Ihrer Container verbessern.

Bedrohungen werden auf Host- und AKS-Clusterebene erkannt. Ausführliche Informationen finden Sie unter [Bedrohungserkennung für Azure-Container](https://docs.microsoft.com/azure/security-center/security-center-alerts-compute#azure-containers-).


## <a name="container-security-faq"></a>Containersicherheit – häufig gestellte Fragen (FAQ)

### <a name="what-types-of-images-can-azure-security-center-scan"></a>Welche Imagetypen kann Azure Security Center scannen?
Security Center scannt auf Linux-Betriebssystemen basierende Images, die Shellzugriff bereitstellen. 

Der Qualys-Scanner unterstützt keine extrem minimalistischen Images wie [Docker-Scratch](https://hub.docker.com/_/scratch/)-Images oder Images ohne Distribution, die nur Ihre Anwendung und deren Laufzeitabhängigkeiten ohne Paket-Manager, Shell oder Betriebssystem enthalten.

### <a name="how-does-azure-security-center-scan-an-image"></a>Wie scannt Azure Security Center ein Image?
Das Image wird aus der Registrierung gepullt. Anschließend wird es in einer isolierten Sandbox mit dem Qualys-Scanner ausgeführt, der eine Liste bekannter Sicherheitsrisiken extrahiert.

Security Center filtert und klassifiziert die Ergebnisse des Scanners. Wenn ein Image fehlerfrei ist, markiert Security Center es entsprechend. Security Center generiert Sicherheitsempfehlungen nur für Images, bei denen Probleme behoben werden müssen. Indem Sie nur benachrichtigt werden, wenn Probleme auftreten, reduziert Security Center das Potenzial von unerwünschten Informationswarnungen.

### <a name="how-often-does-azure-security-center-scan-my-images"></a>Wie häufig scannt Azure Security Center meine Images?
Imagescans werden bei jedem Push ausgelöst.

### <a name="can-i-get-the-scan-results-via-rest-api"></a>Kann ich die Scanergebnisse über die REST-API abrufen?
Ja. Die Ergebnisse befinden sich unter [Sub-Assessments Rest API](/rest/api/securitycenter/subassessments/list/) (Unterbewertungen-REST-API). Außerdem können Sie Azure Resource Graph (ARG) verwenden, die Kusto-ähnliche API für alle Ihre Ressourcen: Mit einer Abfrage kann ein bestimmter Scan abgerufen werden.
 

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Containersicherheit in Azure Security Center finden Sie in diesen verwandten Artikeln:

* Informationen zum Sicherheitsstatus Ihrer containerbezogenen Ressourcen finden Sie im Abschnitt „Container“ unter [Schützen Ihrer Computer und Anwendungen](security-center-virtual-machine-protection.md#containers).

* Ausführliche Informationen zur [Integration mit Azure Kubernetes Service](azure-kubernetes-service-integration.md)

* Ausführliche Informationen zur [Integration mit Azure Container Registry](azure-container-registry-integration.md)