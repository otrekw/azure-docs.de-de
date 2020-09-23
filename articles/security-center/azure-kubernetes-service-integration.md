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
ms.openlocfilehash: 7f43f50fc35b5ecbc9720224036ea13e2ba753f5
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90894933"
---
# <a name="azure-kubernetes-services-integration-with-security-center"></a>Integration von Security Center in Azure Kubernetes Service

Azure Kubernetes Service (AKS) ist der verwaltete Dienst von Microsoft für die Entwicklung, Bereitstellung und Verwaltung von Anwendungen in Containern. 

Aktivieren Sie **Azure Defender für Kubernetes**, um einen tieferen Einblick in Ihre AKS-Knoten, den Clouddatenverkehr und die Sicherheitssteuerungen zu erhalten.

Security Center und AKS bilden gemeinsam das beste cloudnative Kubernetes-Sicherheitsangebot.

## <a name="what-are-the-components-of-security-centers-kubernetes-protection"></a>Was sind die Komponenten des Kubernetes-Schutzes von Security Center?

Der Schutz von Security Center für Kubernetes erfolgt durch eine Kombination aus zwei Elementen:

- **Bedrohungsschutz für virtuelle Computer von Azure Security Center**: Security Center kann unter Verwendung desselben Log Analytics-Agents, der von Security Center für andere VMs verwendet wird, Sicherheitsprobleme aufzeigen, die auf Ihren AKS-Knoten auftreten. Der Agent überwacht außerdem containerspezifische Analysen.

- **Optionaler Azure Security Center-Plan für Azure Defender für Kubernetes:** Der Kubernetes-Plan empfängt Protokolle und Informationen aus dem Kubernetes-Subsystem über den AKS-Dienst. Diese Protokolle sind bereits in Azure über den AKS-Dienst verfügbar. Wenn Sie Azure Defender für Kubernetes aktivieren, erteilen Sie Security Center Zugriff auf die Protokolle. Security Center bringt daher mithilfe von Daten, die bereits vom AKS-Masterknoten erfasst wurden, Sicherheitsvorteile für Ihre AKS-Cluster mit sich. Einige der Daten, die von Azure Security Center in Ihrer Kubernetes-Umgebung gescannt werden, können vertrauliche Informationen enthalten.


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu den Containersicherheitsfeatures von Security Center finden Sie unter:

* [Containersicherheit in Security Center](container-security.md)

* [Integration mit Azure Container Registry](azure-container-registry-integration.md)

* [Datenverwaltung bei Microsoft:](https://www.microsoft.com/trust-center/privacy/data-management) Beschreibt die Datenrichtlinien von Microsoft-Diensten (z. B. Azure, Intune und Microsoft 365), Details zur Microsoft-Datenverwaltung und die Aufbewahrungsrichtlinien, die sich auf Ihre Daten auswirken.
