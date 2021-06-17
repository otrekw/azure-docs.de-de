---
author: memildin
ms.author: memildin
manager: rkarlin
ms.date: 04/07/2021
ms.topic: include
ms.openlocfilehash: d14274c43c3241a4c091ee004747c1ad8e12968b
ms.sourcegitcommit: 67cdbe905eb67e969d7d0e211d87bc174b9b8dc0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2021
ms.locfileid: "111896443"
---
Azure Defender bietet einen Echtzeit-Bedrohungsschutz für Ihre Containerumgebungen und generiert Warnungen für verdächtige Aktivitäten. Mit diesen Informationen können Sie schnell Sicherheitsprobleme lösen und die Sicherheit Ihrer Container verbessern.

Azure Defender bietet Bedrohungsschutz auf unterschiedlichen Ebenen: 

* **Hostebene (bereitgestellt von Azure Defender für Server):** Mit dem gleichen Log Analytics-Agent, den Security Center auch auf anderen VMs verwendet, überwacht Azure Defender Ihre Linux-Kubernetes-Knoten auf verdächtige Aktivitäten wie Webshellerkennung und Verbindungen mit bekannten verdächtigen IP-Adressen. Der Agent führt bei der Überwachung auch containerspezifische Analysen durch, z. B. für die Erstellung privilegierter Container, den verdächtigen Zugriff auf API-Server und SSH-Server (Secure Shell), die in einem Docker-Container ausgeführt werden.

    Wenn Sie die Agents nicht auf Ihren Hosts installieren, profitieren Sie nicht von allen Vorteilen und Sicherheitswarnungen des Bedrohungsschutzes. Sie erhalten weiterhin Warnungen im Zusammenhang mit der Netzwerkanalyse und der Kommunikation mit schädlichen Servern.

    >[!IMPORTANT]
    > Derzeit wird die Installation des Log Analytics-Agents in Azure Kubernetes Service-Clustern, die in VM-Skalierungsgruppen ausgeführt werden, nicht unterstützt.

    Eine Liste der Warnungen auf Clusterebene finden Sie in der [Referenztabelle der Warnungen](../articles/security-center/alerts-reference.md#alerts-containerhost).


* **Clusterebene (bereitgestellt von Azure Defender für Kubernetes):** Auf Clusterebene basiert der Bedrohungsschutz auf der Analyse der Überwachungsprotokolle von Kubernetes. Aktivieren Sie Azure Defender, um diese Überwachung **ohne Agents** zu ermöglichen. Wenn Ihr Cluster lokal oder von einem anderen Cloudanbieter gehostet wird, aktivieren Sie [Kubernetes mit Arc-Unterstützung und die Azure Defender-Erweiterung](../articles/security-center/defender-for-kubernetes-azure-arc.md).

    Azure Defender überwacht die Protokolle Ihres Clusters, um Warnungen auf dieser Ebene zu generieren. Beispiele für Ereignisse auf dieser Ebene sind verfügbar gemachte Kubernetes-Dashboards und die Erstellung von Rollen mit hohen Berechtigungen und von sensiblen Einbindungen.

    >[!NOTE]
    > Azure Defender generiert Sicherheitswarnungen für Aktionen und Bereitstellungen, die nach der Aktivierung des Plans „Defender für Kubernetes“ in Ihrem Abonnement ausgeführt werden. 

    Eine Liste der Warnungen auf Clusterebene finden Sie in der [Referenztabelle der Warnungen](../articles/security-center/alerts-reference.md#alerts-k8scluster).

Außerdem wird die Bedrohungslandschaft von unserem globalen Team von Sicherheitsforschern ständig überwacht. Sie fügen containerspezifische Warnungen und Sicherheitsrisiken hinzu, sobald sie erkannt werden.

> [!TIP]
> Sie können Containerwarnungen simulieren, indem Sie die Anweisungen in [diesem Blogbeitrag](https://techcommunity.microsoft.com/t5/azure-security-center/how-to-demonstrate-the-new-containers-features-in-azure-security/ba-p/1011270) befolgen.