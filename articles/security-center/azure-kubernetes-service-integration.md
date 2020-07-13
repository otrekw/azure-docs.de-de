---
title: Azure Security Center und Azure Kubernetes Service
description: Erfahren Sie mehr über die Integration von Azure Security Center in Azure Kubernetes Service.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/30/2020
ms.author: memildin
ms.openlocfilehash: 94d1bccc9a7f45d24d8c5b92aecba54d9f7f630a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85800175"
---
# <a name="azure-kubernetes-services-integration-with-security-center"></a>Integration von Security Center in Azure Kubernetes Service

Azure Kubernetes Service (AKS) ist der verwaltete Dienst von Microsoft für die Entwicklung, Bereitstellung und Verwaltung von Anwendungen in Containern. 

In Verbindung mit dem Standardtarif von Azure Security Center (siehe [Preise](security-center-pricing.md)) können Sie das AKS-Paket hinzufügen, um einen tieferen Einblick in Ihre AKS-Knoten, den Clouddatenverkehr und die Sicherheitssteuerungen zu erhalten.

Security Center und AKS bilden gemeinsam das beste cloudnative Kubernetes-Sicherheitsangebot.

## <a name="what-are-the-components-of-security-centers-kubernetes-protection"></a>Was sind die Komponenten des Kubernetes-Schutzes von Security Center?

Der Schutz von Security Center für Kubernetes erfolgt durch eine Kombination aus zwei Elementen:

- **Bedrohungsschutz für virtuelle Computer von Azure Security Center**: Security Center kann unter Verwendung desselben Log Analytics-Agents, der von Security Center für andere VMs verwendet wird, Sicherheitsprobleme aufzeigen, die auf Ihren AKS-Knoten auftreten. Der Agent überwacht außerdem containerspezifische Analysen.

- **Optionales Kubernetes-Paket von Azure Security Center**: Das Kubernetes-Paket empfängt Protokolle und Informationen aus dem Kubernetes-Subsystem über den AKS-Dienst. Diese Protokolle sind bereits in Azure über den AKS-Dienst verfügbar. Wenn Sie das Kubernetes-Paket von Security Center aktivieren, erteilen Sie Security Center Zugriff auf die Protokolle. Security Center bringt daher mithilfe von Daten, die bereits vom AKS-Masterknoten erfasst wurden, Sicherheitsvorteile für Ihre AKS-Cluster mit sich. Einige der Daten, die von Azure Security Center in Ihrer Kubernetes-Umgebung gescannt werden, können vertrauliche Informationen enthalten.

    ![Allgemeine Übersicht zu Azure Security Center und Azure Kubernetes Service (AKS)](./media/azure-kubernetes-service-integration/aks-asc-integration-overview.png)

## <a name="what-protections-are-provided"></a>Welche Schutzmaßnahmen werden bereitgestellt?

Die gemeinsame Verwendung der beiden Dienste bietet Folgendes:

* **Sicherheitsempfehlungen** – Security Center identifiziert und kategorisiert Ihre AKS-Ressourcen: von Clustern zu einzelnen virtuellen Computern. Anschließend können Sie die Sicherheitsempfehlungen pro Ressource anzeigen. Weitere Informationen finden Sie in den Containerempfehlungen in der [Referenzliste der Empfehlungen](recommendations-reference.md#recs-containers). 

* **Umgebungshärtung:** Security Center überwacht ständig die Konfiguration Ihrer Kubernetes-Cluster und Docker-Konfigurationen. Anschließend werden den Branchenstandards entsprechende Sicherheitsempfehlungen generiert.

* **Laufzeitschutz**: Durch fortlaufende Analyse der folgenden AKS-Quellen warnt Security Center Sie bei auf Host- *und* AKS-Clusterebene erkannten Bedrohungen und schädlichen Aktivitäten. [Weitere Informationen zum Bedrohungsschutz für Container](threat-protection.md#azure-containers).


     

![Weitere Details zu Azure Security Center und Azure Kubernetes Service (AKS)](./media/azure-kubernetes-service-integration/aks-asc-integration-detailed.png)



## <a name="aks-with-security-center-faq"></a>Häufig gestellte Fragen zu AKS mit Security Center

### <a name="can-i-still-get-aks-protections-without-the-log-analytics-agent"></a>Kann ich AKS-Schutzmaßnahmen ohne den Log Analytics-Agent erhalten?

Wie bereits erwähnt, bietet das optionale Kubernetes-Paket Schutzmaßnahmen auf Clusterebene, der Log Analytics-Agent des Azure Security Center-Standardtarifs schützt Ihre Knoten. 

Es wird empfohlen, beide Komponenten bereitzustellen, um einen möglichst umfassenden Schutz zu gewährleisten.

Wenn Sie den Agent nicht auf Ihren Hosts installieren, profitieren Sie nicht von allen Vorteilen und Sicherheitswarnungen des Bedrohungsschutzes. Sie erhalten weiterhin Warnungen im Zusammenhang mit der Netzwerkanalyse und der Kommunikation mit schädlichen Servern.



## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu den Containersicherheitsfeatures von Security Center finden Sie unter:

* [Containersicherheit in Security Center](container-security.md)

* [Integration mit Azure Container Registry](azure-container-registry-integration.md)

* [Datenverwaltung bei Microsoft:](https://www.microsoft.com/trust-center/privacy/data-management) Beschreibt die Datenrichtlinien von Microsoft-Diensten (z. B. Azure, Intune und Microsoft 365), Details zur Microsoft-Datenverwaltung und die Aufbewahrungsrichtlinien, die sich auf Ihre Daten auswirken.
