---
title: Azure Defender für Kubernetes – Vorteile und Features
description: Erfahren Sie mehr über die Vorteile und Features von Azure Defender für Kubernetes.
author: memildin
ms.author: memildin
ms.date: 04/07/2021
ms.topic: overview
ms.service: security-center
manager: rkarlin
ms.openlocfilehash: 6eec2fed58d9c4fa3b0a05dc6ed03d9c5bf9d840
ms.sourcegitcommit: a038863c0a99dfda16133bcb08b172b6b4c86db8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2021
ms.locfileid: "113002538"
---
# <a name="introduction-to-azure-defender-for-kubernetes"></a>Einführung in Azure Defender für Kubernetes

Azure Defender für Kubernetes ist der Azure Defender-Plan, der Schutz für Ihre Kubernetes-Cluster unabhängig von ihrem Ausführungsort bietet. 

Cluster können an folgenden Orten geschützt werden:

- **Azure Kubernetes Service (AKS):** Der verwaltete Dienst von Microsoft für die Entwicklung, Bereitstellung und Verwaltung von containerisierten Anwendungen

- **Lokale Umgebungen und Multicloudumgebungen:** Verwenden einer [Erweiterung für Kubernetes mit Arc-Unterstützung](defender-for-kubernetes-azure-arc.md)

Azure Security Center und AKS bilden ein cloudnatives Kubernetes-Sicherheitsangebot mit Umgebungshärtung, Workloadschutz und Laufzeitschutz, wie unter [Containersicherheit in Security Center](container-security.md) beschrieben.

Die Bedrohungserkennung auf Hostebene für Ihre Linux-AKS-Knoten ist verfügbar, wenn Sie [Azure Defender für Server](defender-for-servers-introduction.md) und den zugehörigen Log Analytics-Agent aktivieren. Wird Ihr Cluster jedoch in einer VM-Skalierungsgruppe bereitgestellt, wird der Log Analytics-Agent derzeit nicht unterstützt.



## <a name="availability"></a>Verfügbarkeit

|Aspekt|Details|
|----|:----|
|Status des Release:|Allgemeine Verfügbarkeit (General Availability, GA)|
|Preise:|**Azure Defender für Kubernetes** wird gemäß [Security Center – Preise](https://azure.microsoft.com/pricing/details/security-center/) abgerechnet.|
|Erforderliche Rollen und Berechtigungen:|**Sicherheitsadministrator** kann Warnungen verwerfen.<br>Der **Sicherheitsleseberechtigte** kann Ergebnisse anzeigen.|
|Clouds:|![Ja](./media/icons/yes-icon.png) Kommerzielle Clouds<br>![Ja](./media/icons/yes-icon.png) National/Sovereign (US Gov, Azure China)|
|||

## <a name="what-are-the-benefits-of-azure-defender-for-kubernetes"></a>Welche Vorteile bietet Azure Defender für Kubernetes?

Azure Defender für Kubernetes bietet **Bedrohungsschutz auf Clusterebene** durch Überwachung der Clusterprotokolle.

Beispiele für sicherheitsrelevante Ereignisse, die von Azure Defender für Kubernetes überwacht werden, sind verfügbar gemachte Kubernetes-Dashboards und die Erstellung von Rollen mit hohen Berechtigungen und von sensiblen Einbindungen. Eine vollständige Liste der Warnungen auf Clusterebene finden Sie in der [Referenztabelle der Warnungen](alerts-reference.md#alerts-k8scluster).

> [!TIP]
> Sie können Containerwarnungen simulieren, indem Sie die Anweisungen in [diesem Blogbeitrag](https://techcommunity.microsoft.com/t5/azure-security-center/how-to-demonstrate-the-new-containers-features-in-azure-security/ba-p/1011270) befolgen.

Außerdem wird die Bedrohungslandschaft von unserem globalen Team von Sicherheitsforschern ständig überwacht. Sie fügen containerspezifische Warnungen und Sicherheitsrisiken hinzu, sobald sie erkannt werden.

>[!NOTE]
> Azure Defender generiert Sicherheitswarnungen für Aktionen und Bereitstellungen, die nach der Aktivierung des Plans „Defender für Kubernetes“ in Ihrem Abonnement ausgeführt werden.




## <a name="faq---azure-defender-for-kubernetes"></a>Häufig gestellte Fragen zu Azure Defender für Kubernetes

### <a name="can-i-still-get-cluster-protections-without-the-log-analytics-agent"></a>Kann ich Schutzmaßnahmen ohne den Log Analytics-Agent erhalten?

Der Plan **Azure Defender für Kubernetes** bietet Schutz auf Clusterebene. Wenn Sie außerdem den Log Analytics-Agent von **Azure Defender für Server** bereitstellen, erhalten Sie den Bedrohungsschutz für Ihre Knoten, der mit diesem Plan bereitgestellt wird. Weitere Informationen finden Sie unter [Einführung in Azure Defender für Server](defender-for-servers-introduction.md).

Es wird empfohlen, beide Komponenten bereitzustellen, um einen möglichst umfassenden Schutz zu gewährleisten.

Wenn Sie den Agent nicht auf Ihren Hosts installieren, profitieren Sie nicht von allen Vorteilen und Sicherheitswarnungen des Bedrohungsschutzes. Sie erhalten weiterhin Warnungen im Zusammenhang mit der Netzwerkanalyse und der Kommunikation mit schädlichen Servern.

### <a name="does-aks-allow-me-to-install-custom-vm-extensions-on-my-aks-nodes"></a>Erlaubt AKS mir, benutzerdefinierte VM-Erweiterungen auf meinen AKS-Knoten zu installieren?
Damit Azure Defender Ihre AKS-Knoten überwachen kann, muss dort der Log Analytics-Agent ausgeführt werden. 

AKS ist ein verwalteter Dienst, und da der Log Analytics-Agent eine von Microsoft verwaltete Erweiterung ist, wird er auch auf AKS-Clustern unterstützt.

### <a name="if-my-cluster-is-already-running-an-azure-monitor-for-containers-agent-do-i-need-the-log-analytics-agent-too"></a>Wenn auf meinem Cluster bereits ein Agent für Azure Monitor für Containers ausgeführt wird, brauche ich dann auch den Log Analytics-Agent?
Damit Azure Defender Ihre Knoten überwachen kann, muss dort der Log Analytics-Agent ausgeführt werden.

Wenn auf Ihren Clustern bereits der Azure Monitor für Container-Agent ausgeführt wird, können Sie auch den Log Analytics-Agent installieren, und die beiden Agents können problemlos nebeneinander verwendet werden.

[Erfahren Sie mehr über den Azure Monitor für Container-Agent](../azure-monitor/containers/container-insights-manage-agent.md).


## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie etwas über den Kubernetes-Schutz von Security Center erfahren, einschließlich Azure Defender für Kubernetes. 

> [!div class="nextstepaction"]
> [Aktivieren von Azure Defender](enable-azure-defender.md)

Weitere Informationen finden Sie in den folgenden Artikeln: 

- [Streamen von Warnungen in eine SIEM-, SOAR- oder IT Service Management-Lösung](export-to-siem.md)
- [Verweistabelle zu Warnungen](alerts-reference.md)
