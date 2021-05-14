---
title: 'Benutzerdefinierte Speicherorte: Kubernetes mit Azure Arc-Unterstützung'
services: azure-arc
ms.service: azure-arc
ms.date: 04/05/2021
ms.topic: conceptual
author: shashankbarsin
ms.author: shasb
description: Dieser Artikel bietet eine konzeptionelle Übersicht über die Funktionalität „Benutzerdefinierte Speicherorte“ von Kubernetes mit Azure Arc-Unterstützung.
ms.openlocfilehash: 1235c6adfe97a943ef77584a6a0c8683d691be91
ms.sourcegitcommit: 56b0c7923d67f96da21653b4bb37d943c36a81d6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2021
ms.locfileid: "106450818"
---
# <a name="custom-locations-on-top-of-azure-arc-enabled-kubernetes"></a>„Benutzerdefinierte Speicherorte“ auf Basis von Kubernetes mit Azure Arc-Unterstützung

Als Erweiterung des Azure-Speicherortkonstrukts bietet *Benutzerdefinierte Speicherorte* Mandantenadministratoren eine Möglichkeit, ihre Kubernetes-Cluster mit Azure Arc-Unterstützung als Zielspeicherorte für die Bereitstellung von Instanzen von Azure-Diensten zu verwenden. Beispiele für Azure-Ressourcen sind u. a. SQL Managed Instance mit Azure Arc-Unterstützung und PostgreSQL Hyperscale mit Azure Arc-Unterstützung.

Ähnlich wie bei Azure-Speicherorten können Endbenutzer im Mandanten mit Zugriff auf „Benutzerdefinierte Speicherorte“ dort Ressourcen bereitstellen, indem sie das private Compute Ihres Unternehmens verwenden.

[ ![Arc-Plattformebenen](./media/conceptual-arc-platform-layers.png) ](./media/conceptual-arc-platform-layers.png#lightbox)

Sie können „Benutzerdefinierte Speicherorte“ als Abstraktionsschicht auf der Grundlage von Kubernetes-Clustern mit Azure Arc-Unterstützung, Cluster Connect und Clustererweiterungen visualisieren. „Benutzerdefinierte Speicherorte“ erstellt die präzisen [RoleBindings und ClusterRoleBindings](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#rolebinding-and-clusterrolebinding), die erforderlich sind, damit andere Azure-Dienste auf den Cluster zugreifen können. Diese anderen Azure-Dienste erfordern Clusterzugriff zum Verwalten von Ressourcen, die der Kunde in seinen Clustern bereitstellen möchte.

[!INCLUDE [preview features note](./includes/preview/preview-callout.md)]

## <a name="architecture"></a>Aufbau

Wenn der Administrator das Feature „Benutzerdefinierte Speicherorte“ auf dem Cluster aktiviert, wird ein ClusterRoleBinding auf dem Cluster erstellt, das die Azure AD-Anwendung autorisiert, die vom Ressourcenanbieter (Resource Provider, RP) für „Benutzerdefinierte Speicherorte“ verwendet wird. Nach der Autorisierung kann der RP für „Benutzerdefinierte Speicherorte“ ClusterRoleBindings oder RoleBindings erstellen, die von anderen Azure-RPs benötigt werden, um benutzerdefinierte Ressourcen in diesem Cluster zu erstellen. Die Liste der zu autorisierenden RPs wird von den Clustererweiterungen bestimmt, die auf dem Cluster installiert sind.

[ ![Verwenden benutzerdefinierter Speicherorte](./media/conceptual-custom-locations-usage.png) ](./media/conceptual-custom-locations-usage.png#lightbox)

Wenn der Benutzer eine Datendienstinstanz im Cluster erstellt: 
1. Die PUT-Anforderung wird an Azure Resource Manager gesendet.
1. Die PUT-Anforderung wird an den Data Services-RP mit Azure Arc-Unterstützung weitergeleitet. 
1. Der RP ruft die `kubeconfig`-Datei ab, die dem Kubernetes-Cluster mit Azure Arc-Unterstützung zugeordnet ist, auf dem der benutzerdefinierte Speicherort vorhanden ist. 
   * Auf den benutzerdefinierten Speicherort wird in der ursprünglichen PUT-Anforderung als `extendedLocation` verwiesen. 
1. Der Data Services-RP mit Azure Arc-Unterstützung verwendet die `kubeconfig`-Datei für die Kommunikation mit dem Cluster, um eine benutzerdefinierte Ressource des Data Services-Typs mit Azure Arc-Unterstützung für den Namespace zu erstellen, der dem benutzerdefinierten Speicherort zugeordnet ist. 
   * Der Data Services-Operator mit Azure Arc-Unterstützung wurde über die Erstellung der Clustererweiterung bereitgestellt, bevor der benutzerdefinierte Speicherort existierte. 
1. Der Data Services-Operator mit Azure Arc-Unterstützung liest die auf dem Cluster erstellte neue benutzerdefinierte Ressource, erstellt den Datencontroller und realisiert dabei den gewünschten Zustand im Cluster. 

Die Abfolge der Schritte zum Erstellen der verwalteten SQL-Instanz und der PostgreSQL-Instanz ist identisch mit der oben beschriebenen Abfolge von Schritten.

## <a name="next-steps"></a>Nächste Schritte

* Nutzen Sie den Schnellstart zum [Verbinden eines Kubernetes-Clusters mit Azure Arc](./quickstart-connect-cluster.md).
* [Erstellen Sie einen benutzerdefinierten Speicherort](./custom-locations.md) in Ihrem Kubernetes-Cluster mit Azure Arc-Unterstützung.