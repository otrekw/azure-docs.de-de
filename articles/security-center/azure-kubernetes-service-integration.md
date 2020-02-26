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
ms.date: 11/04/2019
ms.author: memildin
ms.openlocfilehash: 9d8c5f98cfd8b4b3831bcbd7e65285f93e6c323f
ms.sourcegitcommit: 6e87ddc3cc961945c2269b4c0c6edd39ea6a5414
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/18/2020
ms.locfileid: "77441951"
---
# <a name="azure-kubernetes-services-integration-with-security-center-preview"></a>Integration von Security Center in Azure Kubernetes Service (Vorschau)
Azure Kubernetes Service (AKS) ist der verwaltete Dienst von Microsoft für Entwicklung, Bereitstellung und Verwaltung von Anwendungen in Containern. 

Verwenden Sie AKS in Verbindung mit dem Standard-Tarif von Azure Security Center (siehe [Preise](security-center-pricing.md)), um einen tieferen Einblick in Ihre AKS-Knoten, den Clouddatenverkehr und die Sicherheitssteuerungen zu erhalten.

Security Center bringt mithilfe von Daten, die bereits vom AKS-Masterknoten erfasst wurden, Sicherheitsvorteile für Ihre AKS-Cluster. 

![Allgemeine Übersicht zu Azure Security Center und Azure Kubernetes Service (AKS)](./media/azure-kubernetes-service-integration/aks-asc-integration-overview.png)

Diese beiden Tools gemeinsam bilden das beste cloudnative Kubernetes-Sicherheitsangebot. 

## <a name="benefits-of-integration"></a>Vorteile der Integration

Die gemeinsame Verwendung der beiden Dienste bietet Folgendes:

* **Sicherheitsempfehlungen** – Security Center identifiziert und kategorisiert Ihre AKS-Ressourcen: von Clustern zu einzelnen virtuellen Computern. Anschließend können Sie die Sicherheitsempfehlungen pro Ressource anzeigen. Weitere Informationen finden Sie in den Containerempfehlungen in der [Referenzliste der Empfehlungen](recommendations-reference.md#recs-computeapp). 

    > [!NOTE]
    > Wenn der Name einer Security Center-Empfehlung mit dem Tag „(Vorschau)“ endet, bezieht sich dies auf die Empfehlung und bedeutet nicht, dass sich das Feature in der Vorschauphase befindet.

* **Umgebungshärtung:** Security Center überwacht ständig die Konfiguration Ihrer Kubernetes-Cluster und Docker-Konfigurationen. Anschließend werden den Branchenstandards entsprechende Sicherheitsempfehlungen generiert.

* **Laufzeitschutz:** Durch fortlaufende Analyse der folgenden AKS-Quellen warnt Security Center Sie über auf Host- *und* AKS-Clusterebene erkannte Bedrohungen und schädliche Aktivitäten:
    * Sicherheitsrohereignisse, z.B. Erstellung von Netzwerkdaten und -prozessen
    * Das Kubernetes-Überwachungsprotokoll

    Weitere Informationen finden Sie unter [Bedrohungserkennung für Azure-Container](security-center-alerts-compute.md#azure-containers-).

    Die Liste der möglichen Warnungen finden Sie in den folgenden Abschnitten in der Referenztabelle zu Warnungen: [Warnungen auf AKS-Cluster Ebene](alerts-reference.md#alerts-akscluster) und [Warnungen auf Containerhostebene](alerts-reference.md#alerts-containerhost).  

![Weitere Details zu Azure Security Center und Azure Kubernetes Service (AKS)](./media/azure-kubernetes-service-integration/aks-asc-integration-detailed.png)

> [!NOTE]
> Einige der Daten, die von Azure Security Center in Ihrer Kubernetes-Umgebung gescannt werden, können vertrauliche Informationen enthalten.


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu den Containersicherheitsfeatures von Security Center finden Sie unter:

* [Containersicherheit in Security Center](container-security.md)

* [Integration mit Azure Container Registry](azure-container-registry-integration.md)

* [Datenverwaltung bei Microsoft](https://www.microsoft.com/trust-center/privacy/data-management) – Beschreibt die Datenrichtlinien von Microsoft-Diensten (z.B. Azure, Intune und Office 365), Details zur Microsoft-Datenverwaltung und Aufbewahrungsrichtlinien, die sich auf Ihre Daten auswirken.