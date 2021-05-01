---
title: 'Cluster Connect: Kubernetes mit Azure Arc-Unterstützung'
services: azure-arc
ms.service: azure-arc
ms.date: 04/05/2021
ms.topic: conceptual
author: shashankbarsin
ms.author: shasb
description: Dieser Artikel bietet eine konzeptionelle Übersicht über die Cluster Connect-Funktionalität von Kubernetes mit Azure Arc-Unterstützung.
ms.openlocfilehash: 716ffe0c1f123c2a6abe8f407f6435e157ba5b6a
ms.sourcegitcommit: 56b0c7923d67f96da21653b4bb37d943c36a81d6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2021
ms.locfileid: "106450714"
---
# <a name="cluster-connect-on-azure-arc-enabled-kubernetes"></a>Cluster Connect auf Kubernetes mit Azure Arc-Unterstützung

Das Feature *Cluster Connect* von Kubernetes mit Azure Arc-Unterstützung bietet Konnektivität mit dem `apiserver` des Clusters, ohne dass ein Eingangsport in der Firewall aktiviert werden muss. Ein auf dem Cluster ausgeführter Reverseproxy-Agent kann eine ausgehende Sitzung auf sichere Weise mit dem Azure Arc-Dienst starten. 

Mit Cluster Connect können Entwickler von überall zum interaktiven Entwickeln und Debuggen auf ihre Cluster zugreifen. Außerdem können Clusterbenutzer und -administratoren von überall auf ihre Cluster zugreifen oder diese verwalten. Sie können auch gehostete Agents/Runner von Azure Pipelines, GitHub-Aktionen oder einen anderen gehosteten CI/CD-Dienst verwenden, um Anwendungen lokalen Clustern bereitzustellen, ohne dass selbstgehostete Agents erforderlich sind.

[!INCLUDE [preview features note](./includes/preview/preview-callout.md)]

## <a name="architecture"></a>Aufbau

[ ![Cluster Connect-Architektur](./media/conceptual-cluster-connect.png) ](./media/conceptual-cluster-connect.png#lightbox)

Auf der Clusterseite führt ein Reverseproxy-Agent namens`clusterconnect-agent`, der als Teil des Agent-Helm-Diagramms bereitgestellt wird, ausgehende Aufrufe an den Azure Arc-Dienst aus, um die Sitzung einzurichten.

Wenn der Benutzer `az connectedk8s proxy` aufruft, geschieht Folgendes:
1. Die Binärdatei des Azure Arc-Proxys wird heruntergeladen und als Prozess auf dem Clientcomputer hochgefahren. 
1. Der Azure Arc-Proxy ruft eine `kubeconfig`-Datei ab, die dem Kubernetes-Cluster mit Azure Arc-Unterstützung zugeordnet ist, in dem der `az connectedk8s proxy` aufgerufen wird.
    * Der Azure Arc-Proxy verwendet das Azure-Zugriffstoken des Aufrufers und den Azure Resource Manager-ID-Namen. 
1. Die vom Azure Arc-Proxy auf dem Computer gespeicherte `kubeconfig` Datei verweist die Server-URL auf einen Endpunkt im Azure Arc-Proxyprozess.

Wenn ein Benutzer eine Anforderung mit dieser `kubeconfig`-Datei sendet, geschieht Folgendes:
1. Der Azure Arc-Proxy ordnet den Endpunkt, der die Anforderung empfängt, dem Azure Arc-Dienst zu. 
1. Der Azure Arc-Dienst leitet die Anforderung dann an den im Cluster ausgeführten `clusterconnect-agent` weiter. 
1. Der `clusterconnect-agent` übergibt die Anforderung an die `kube-aad-proxy`-Komponente, die die Azure AD-Authentifizierung für die aufrufende Entität ausführt. 
1. Nach der Azure AD-Authentifizierung verwendet `kube-aad-proxy` das Kubernetes-Feature [User impersonation](https://kubernetes.io/docs/reference/access-authn-authz/authentication/#user-impersonation) (Benutzeridentitätswechsel), um die Anforderung an den `apiserver` des Clusters weiterzuleiten.

## <a name="next-steps"></a>Nächste Schritte

* Nutzen Sie den Schnellstart zum [Verbinden eines Kubernetes-Clusters mit Azure Arc](./quickstart-connect-cluster.md).
* Sicherer [Zugriff auf Ihren Cluster](./cluster-connect.md) von überall mit Cluster Connect.