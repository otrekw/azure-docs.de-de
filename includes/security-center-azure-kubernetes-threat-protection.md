---
author: memildin
ms.author: memildin
manager: rkarlin
ms.date: 06/30/2020
ms.topic: include
ms.openlocfilehash: ba695337a1da1e440895f6b9f6fe2eb34f5041e3
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85800602"
---
Security Center bietet einen Echtzeit-Bedrohungsschutz für Ihre Containerumgebungen und generiert Warnungen für verdächtige Aktivitäten. Mit diesen Informationen können Sie schnell Sicherheitsprobleme lösen und die Sicherheit Ihrer Container verbessern.

Security Center bietet Bedrohungsschutz auf unterschiedlichen Ebenen: 

* **Hostebene**: Der Log Analytics-Agent überwacht Linux auf verdächtige Aktivitäten. Der Agent löst Warnungen für verdächtige Aktivitäten aus, die aus dem Knoten oder einem darauf ausgeführten Container stammen. Beispiele für derartige Aktivitäten sind Webshell-Erkennung und Verbindungen mit bekannten verdächtigen IP-Adressen.

    Um einen tieferen Einblick in die Sicherheit Ihrer Containerumgebung zu erhalten, überwacht der Agent containerspezifische Analysen. Er löst Warnungen für Ereignisse aus, z.B. die Erstellung privilegierter Container, den verdächtigen Zugriff auf API-Server und Secure Shell (SSH)-Server, die in einem Docker-Container ausgeführt werden.

    >[!IMPORTANT]
    > Wenn Sie die Agents nicht auf Ihren Hosts installieren, profitieren Sie nicht von allen Vorteilen und Sicherheitswarnungen des Bedrohungsschutzes. Sie erhalten weiterhin Warnungen im Zusammenhang mit der Netzwerkanalyse und der Kommunikation mit schädlichen Servern.

    Eine Liste der Warnungen auf AKS-Hostebene finden Sie in der [Referenztabelle der Warnungen](https://docs.microsoft.com/azure/security-center/alerts-reference#alerts-containerhost).


* Auf **AKS-Cluster Ebene** basiert der Bedrohungsschutz auf der Analyse von Kubernetes-Überwachungsprotokollen. Um diese Überwachung **ohne Agents** zu aktivieren, fügen Sie die Kubernetes-Option über die Seite **Preise und Einstellungen** (siehe [Preise](https://docs.microsoft.com/azure/security-center/security-center-pricing)) zu Ihrem Abonnement hinzu. Zum Generieren von Warnungen auf dieser Ebene überwacht Security Center Ihre von AKS verwalteten Dienste mithilfe der von AKS abgerufenen Protokolle. Beispiele für Ereignisse auf dieser Ebene sind verfügbar gemachte Kubernetes-Dashboards und die Erstellung von Rollen mit hohen Berechtigungen und von sensiblen Einbindungen.

    >[!NOTE]
    > Security Center generiert Sicherheitswarnungen für Azure Kubernetes Service-Aktionen und -Bereitstellungen, die nach der Aktivierung der Kubernetes-Option in den Abonnementeinstellungen erfolgen. 

    Eine Liste der Warnungen auf AKS-Clusterebene finden Sie in der [Referenztabelle der Warnungen](https://docs.microsoft.com/azure/security-center/alerts-reference#alerts-akscluster).

Außerdem wird die Bedrohungslandschaft von unserem globalen Team von Sicherheitsforschern ständig überwacht. Sie fügen containerspezifische Warnungen und Sicherheitsrisiken hinzu, sobald sie erkannt werden.

> [!TIP]
> Sie können Containerwarnungen simulieren, indem Sie die Anweisungen in [diesem Blogbeitrag](https://techcommunity.microsoft.com/t5/azure-security-center/how-to-demonstrate-the-new-containers-features-in-azure-security/ba-p/1011270) befolgen.