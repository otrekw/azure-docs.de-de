---
title: Schützen Ihrer Computer und Anwendungen
description: In diesem Dokument werden Empfehlungen in Security Center erläutert, die zum Schutz Ihrer virtuellen und physischen Computer sowie Ihrer Web-Apps und App Service-Umgebungen beitragen.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 47fa1f76-683d-4230-b4ed-d123fef9a3e8
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/11/2020
ms.author: memildin
ms.openlocfilehash: bcf92838483fbb6b54802cc0d44cc44ea086d705
ms.sourcegitcommit: dfa543fad47cb2df5a574931ba57d40d6a47daef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/18/2020
ms.locfileid: "77430631"
---
# <a name="protect-your-machines-and-applications"></a>Schützen Ihrer Computer und Anwendungen
Werden potenzielle Sicherheitslücken erkannt, erstellt Azure Security Center Empfehlungen, die Sie beim Konfigurieren der erforderlichen Steuerelemente zum Härten und Schützen Ihrer Ressourcen unterstützen.

In diesem Artikel wird die Seite **Compute und Apps** des Abschnitts „Ressourcensicherheit“ von Azure Security Center erläutert.

Eine vollständige Liste der Empfehlungen, die auf dieser Seite angezeigt werden können, finden Sie unter [Compute- und App-Empfehlungen](recommendations-reference.md#recs-computeapp).


## <a name="view-the-security-of-your-compute-and-apps-resources"></a>Anzeigen der Sicherheit Ihrer Compute- und App-Ressourcen

[![Security Center-Dashboard](./media/security-center-virtual-machine-recommendations/compute-and-apps-recs-overview.png)](./media/security-center-virtual-machine-recommendations/compute-and-apps-recs-overview.png#lightbox)

Um den Status Ihrer Compute- und App-Ressourcen anzuzeigen, wählen Sie im linken Bereich von Security Center **Compute und Apps** aus. Die folgenden Registerkarten sind verfügbar:

* **Übersicht**: Listet die Empfehlungen für alle Compute- und App-Ressourcen sowie deren aktuellen Sicherheitsstatus auf. 

* [**VMs und Server**](#vms-and-computers): Listet die Empfehlungen für Ihre VMs und Computer sowie deren aktuellen Sicherheitsstatus auf.

* [**VM-Skalierungsgruppen**](#vmscale-sets): Listet die Empfehlungen für Ihre Skalierungsgruppen auf. 

* [**Clouddienste**](#cloud-services): Listet die Empfehlungen für die von Security Center überwachten Web- und Workerrollen auf.

* [**App Services**](#app-services): Listet die Empfehlungen für Ihre App Service-Umgebungen und deren jeweiligen Sicherheitsstatus auf.

* [**Container**](#containers): Listet die Empfehlungen für Ihre Container und die Sicherheitsbewertung ihrer Konfigurationen auf.

* **Computeressourcen**: Listet die Empfehlungen für Ihre Computeressourcen wie Service Fabric-Cluster und Event Hubs auf.

### <a name="whats-in-each-tab"></a>Was befindet sich auf jeder Registerkarte?

Jede Registerkarte enthält mehrere Abschnitte, und in jedem Abschnitt können Sie einen Drilldown ausführen, um weitere Details zum angezeigten Element anzuzeigen.

Auf jeder Registerkarte finden Sie außerdem Empfehlungen für die relevanten Ressourcen in Ihrer überwachten Umgebung. In der ersten Spalte wird die Empfehlung aufgelistet, die zweite zeigt die Gesamtzahl der betroffenen Ressourcen an, und die dritte zeigt den Schweregrad des Problems an.

Jeder Empfehlung ist eine Reihe von Aktionen zugeordnet, die ausgeführt werden können, nachdem Sie sie ausgewählt haben. Wenn Sie beispielsweise **Fehlende Systemupdates** auswählen, werden die Anzahl der virtuellen und physischen Computer, auf denen Patches fehlen, und der Schweregrad des fehlenden Updates angezeigt.

> [!NOTE]
> Die Sicherheitsempfehlungen sind identisch mit denen auf der Seite **Empfehlungen**, aber hier werden sie nach dem von Ihnen ausgewählten Ressourcentyp gefiltert. Weitere Informationen zur Anwendung von Empfehlungen finden Sie unter [Implementieren von Sicherheitsempfehlungen in Azure Security Center](security-center-recommendations.md).
>




### <a name="vms-and-computers"></a>VMs und Server
Der Abschnitt „VMs und Computer“ enthält eine Übersicht über alle Sicherheitsempfehlungen für Ihre VMs und Computer. Es sind vier Arten von Computern enthalten:

![Nicht-Azure-Computer](./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon1.png) Azure-fremder Computer

![Virtueller Azure Resource Manager-Computer](./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon2.png) Virtueller Azure Resource Manager-Computer

![Klassischer virtueller Azure-Computer](./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon3.png) Klassischer virtueller Azure-Computer

![Virtuelle Computer, die anhand des Arbeitsbereichs identifiziert werden](./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon4.png) Virtuelle Computer, die nur anhand des Arbeitsbereichs identifiziert werden, der dem angezeigten Abonnement angehört. Dazu zählen virtuelle Computer aus anderen Abonnements, die dem Arbeitsbereich in diesem Abonnement unterstellt sind, sowie virtuelle Computer, die mit dem direkten Operations Manager-Agent installiert wurden und über keine Ressourcen-ID verfügen.

Anhand des Symbols unter der jeweiligen Empfehlung sehen Sie sofort, bei welchem virtuellen oder physischen Computer eine Aktion erforderlich ist und um welche Art von Empfehlung es sich handelt. Sie können auch die Filter verwenden, um die Liste nach **Ressourcentyp** und **Schweregrad** zu durchsuchen.

Um einen Drilldown in die Sicherheitsempfehlungen für jeden virtuellen Computer durchzuführen, klicken Sie auf den virtuellen Computer.
Hier finden Sie die Sicherheitsdetails für den virtuellen oder physischen Computer. Im unteren Bereich des Blatts werden die empfohlene Aktion und der Schweregrad des jeweiligen Problems angezeigt.

[![Clouddienste](./media/security-center-virtual-machine-recommendations/recommendation-list.png)](./media/security-center-virtual-machine-recommendations/recommendation-list.png#lightbox)




### <a name="vmscale-sets"></a>VM-Skalierungsgruppen
Security Center erkennt automatisch, ob Sie über Skalierungsgruppen verfügen und empfiehlt, Microsoft Monitoring Agent auf diesen installieren.

So installieren Sie den Microsoft Monitoring Agent: 

1. Wählen Sie die Empfehlung **Überwachungs-Agent für VM-Skalierungsgruppen installieren** aus. Sie erhalten eine Liste der nicht überwachten Skalierungsgruppen.

1. Wählen Sie eine Skalierungsgruppe in fehlerhaftem Zustand aus. Führen Sie die Anweisungen zum Installieren des Überwachungs-Agents mit einem vorhandenen aufgefüllten Arbeitsbereich aus, oder erstellen Sie einen neuen. Stellen Sie unbedingt den [Tarif](security-center-pricing.md) des Arbeitsbereichs ein, wenn er nicht festgelegt ist.

   ![Installieren von MMS](./media/security-center-virtual-machine-recommendations/install-mms.png)

Festlegen einer neuen Skalierungsgruppen, um automatisch Microsoft Monitoring Agent zu installieren:
1. Wechseln Sie zu Azure Policy, und klicken Sie auf **Definitionen**.

1. Suchen Sie nach der Richtlinie **Bereitstellen von Log Analytics-Agent für Windows-VM-Skalierungsgruppen**, und klicken Sie darauf.

1. Klicken Sie auf **Zuweisen**.

1. Legen Sie **Bereich** und **Log Analytics-Arbeitsbereich** fest, und klicken Sie auf **Zuweisen**.

Wenn Sie die Installation des Microsoft Monitoring Agent in Azure Policy für alle vorhandenen Skalierungsgruppen festlegen möchten, wenden Sie unter **Wiederherstellung** die vorhandene Richtlinie auf die vorhandenen Skalierungsgruppen an.





### <a name="cloud-services"></a>Clouddienste
Für Clouddienste wird eine Empfehlung erstellt, wenn die Betriebssystemversion nicht mehr aktuell ist.

![Clouddienste](./media/security-center-virtual-machine-recommendations/security-center-monitoring-fig1-new006-2017.png)

In einem Szenario, in dem eine Empfehlung vorliegt, befolgen Sie die Schritte in der Empfehlung zum Aktualisieren des Betriebssystems. Ist ein Update verfügbar, erhalten Sie eine Warnung (rot oder orange, je nach Schweregrad des Problems). Eine vollständige Erläuterung der Empfehlung erhalten Sie, wenn Sie in der Spalte **BESCHREIBUNG** auf **Betriebssystemversion aktualisieren** klicken.






### <a name="app-services"></a>App Services
Um die App Service-Informationen anzuzeigen, müssen Sie den Tarif „Standard“ von Security Center verwenden und App Service in Ihrem Abonnement aktivieren. Anweisungen zum Aktivieren dieses Features finden Sie unter [Schützen von App Services in Azure Security Center](security-center-app-services.md).

Unter **App Services** wird eine Liste Ihrer App Service-Umgebungen und die Integritätszusammenfassung basierend auf der von Security Center ausgeführten Bewertung angezeigt.

![App Services](./media/security-center-virtual-machine-recommendations/app-services.png)

Es werden drei Arten von Anwendungsdiensten angezeigt:

![App Service-Umgebung](./media/security-center-virtual-machine-recommendations/ase.png) App Service-Umgebung

![Webanwendung](./media/security-center-virtual-machine-recommendations/web-app.png) Webanwendung

![Funktionsanwendung](./media/security-center-virtual-machine-recommendations/function-app.png) Funktionsanwendung

Wenn Sie eine Webanwendung auswählen, wird eine Zusammenfassungsansicht mit drei Registerkarten geöffnet:

   - **Empfehlungen**: Basiert auf von Security Center ausgeführten Bewertungen, bei denen ein Fehler aufgetreten ist.
   - **Bestandene Bewertungen**: Liste der vom Security Center ausgeführten Bewertungen, die bestanden wurden.
   - **Nicht verfügbare Bewertungen:** Liste der Bewertungen, die aufgrund eines Fehlers nicht ausgeführt wurden, oder bei denen die Empfehlung für den jeweiligen Service nicht relevant ist.

   Unter **Empfehlungen** finden Sie eine Liste der Empfehlungen für die ausgewählte Webanwendung und den Schweregrad der einzelnen Empfehlungen.

   ![Empfehlungen für App Services](./media/security-center-virtual-machine-recommendations/app-services-rec.png)

Wählen Sie eine Empfehlung aus, um eine Beschreibung der Empfehlung und eine Liste von fehlerhaften und fehlerfreien sowie nicht überprüften Ressourcen anzuzeigen.

   - In der Spalte **Bestandene Bewertungen** wird eine Liste der bestandenen Bewertungen angezeigt. Der Schweregrad dieser Bewertungen wird immer in Grün angezeigt.

   - Wählen Sie eine bestandene Bewertung in der Liste aus, um eine Beschreibung der Bewertung, eine Liste von fehlerhaften und fehlerfreien Ressourcen und eine Liste von nicht überprüften Ressourcen anzuzeigen. Zwar ist eine Registerkarte für fehlerhafte Ressourcen vorhanden, diese Liste ist aber immer leer, da die Bewertung bestanden wurde.





### <a name="containers"></a>Container

Wenn Sie die Registerkarte **Container** öffnen, wird je nach Ihrer Umgebung möglicherweise einer von drei Ressourcentypen angezeigt:

![Containerhost](./media/security-center-virtual-machine-recommendations/icon-container-host-rec.png) Containerhosts – VMs auf denen Docker ausgeführt wird 

![Kubernetes-Dienst](./media/security-center-virtual-machine-recommendations/icon-kubernetes-service-rec.png) AKS-Cluster (Azure Kubernetes Service)

![Containerregistrierung](./media/security-center-virtual-machine-recommendations/icon-container-registry-rec.png) ACR-Registrierungen (Azure Container Registry) – Wird nur in der Standardpreisstufe angezeigt und wenn das Azure Container Registry-Paket aktiviert ist.

Anweisungen zur Verwendung der Containersicherheitsfeatures finden Sie unter [Überwachung der Sicherheit Ihrer Container](monitor-container-security.md).

Die Vorteile des Azure Container Registry-Pakets werden [hier](azure-container-registry-integration.md) erklärt.

Die Vorteile des Kubernetes Services-Pakets werden [hier](azure-kubernetes-service-integration.md) erklärt.

[![Container-Registerkarte](./media/security-center-virtual-machine-recommendations/container-recommendations-all-types.png)](./media/security-center-virtual-machine-recommendations/container-recommendations-all-types.png#lightbox)

Um die Empfehlungen für eine bestimmte Ressource in der Liste anzuzeigen, klicken Sie auf diese Ressource.

#### <a name="visibility-into-container-registries"></a>Einblick in Containerregistrierungen

Wenn Sie beispielsweise in der Liste, die in der obigen Grafik dargestellt ist, auf das ACR-Register „asc-demo“ klicken, gelangen Sie zu dieser Detailseite:

[![Empfehlungen für eine bestimmte ACR-Registrierung](./media/security-center-virtual-machine-recommendations/acr-registry-recs-list.png)](./media/security-center-virtual-machine-recommendations/acr-registry-recs-list.png#lightbox)


#### <a name="visibility-into-containers-hosted-on-iaas-linux-machines"></a>Einblick in die Container, die auf IaaS-Linux-Computern gehostet werden

Wenn Sie auf einen der virtuellen Computer klicken, auf denen Docker ausgeführt wird, wird die Detailseite mit Informationen zu den Containern auf dem Computer angezeigt, wie z. B. die Dockerversion und die Anzahl der auf dem Host ausgeführten Images.

![Empfehlungen für einen virtuellen Computer, auf dem Docker ausgeführt wird](./media/security-center-virtual-machine-recommendations/docker-recommendation.png)


#### <a name="security-recommendations-based-on-cis-benchmark-for-docker"></a>Empfehlungen zur Sicherheit basierend auf der CIS-Benchmark für Docker

Security Center scannt Ihre Docker-Konfigurationen und bietet Ihnen durch die Bereitstellung einer Liste aller festgestellten Regelverstöße Einblick in die Fehlkonfigurationen. Security Center bietet Richtlinien, mit denen Sie diese Probleme schnell beheben und Zeit sparen können. Security Center bewertet fortlaufend die Docker-Konfigurationen und unterrichtet Sie über deren aktuellen Zustand.

![Registerkarte „Container“](./media/security-center-container-recommendations/container-cis-benchmark.png)


## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Empfehlungen für andere Arten von Azure-Ressourcen finden Sie in den folgenden Artikeln:

* [Vollständige Referenzliste der Sicherheitsempfehlungen von Azure Security Center](recommendations-reference.md)
* [Überwachen von Identität und Zugriff in Azure Security Center](security-center-identity-access.md)
* [Schützen Ihres Netzwerks in Azure Security Center](security-center-network-recommendations.md)
* [Schützen Ihres Azure SQL-Diensts in Azure Security Center](security-center-sql-service-recommendations.md)