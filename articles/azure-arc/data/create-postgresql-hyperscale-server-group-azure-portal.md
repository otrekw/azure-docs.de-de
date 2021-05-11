---
title: Erstellen einer PostgreSQL Hyperscale-Servergruppe mit Azure Arc-Unterstützung über das Azure-Portal
description: Erstellen einer PostgreSQL Hyperscale-Servergruppe mit Azure Arc-Unterstützung über das Azure-Portal
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: TheJY
ms.author: jeanyd
ms.reviewer: mikeray
ms.date: 04/28/2021
ms.topic: how-to
ms.openlocfilehash: 5cc99acf303b99e138edd0418d2d49824bbf27fe
ms.sourcegitcommit: fc9fd6e72297de6e87c9cf0d58edd632a8fb2552
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2021
ms.locfileid: "108294050"
---
# <a name="create-an-azure-arc-enabled-postgresql-hyperscale-server-group-from-the-azure-portal"></a>Erstellen einer PostgreSQL Hyperscale-Servergruppe mit Azure Arc-Unterstützung über das Azure-Portal

In diesem Dokument werden die Schritte zur Erstellung einer PostgreSQL Hyperscale-Servergruppe in Azure Arc über das Azure-Portal beschrieben.

[!INCLUDE [azure-arc-common-prerequisites](../../../includes/azure-arc-common-prerequisites.md)]

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="getting-started"></a>Erste Schritte
Wenn Sie bereits mit den folgenden Themen vertraut sind, können Sie diesen Absatz überspringen.
Dabei handelt es sich um wichtige Themen, die Sie lesen sollten, bevor Sie mit dem Erstellen beginnen:
- [Übersicht über Azure Arc-fähige Datendienste](overview.md)
- [Konnektivitätsmodi und -anforderungen](connectivity.md)
- [Speicherkonfiguration und Kubernetes-Speicherkonzepte](storage-configuration.md)
- [Kubernetes-Ressourcenmodell](https://github.com/kubernetes/community/blob/master/contributors/design-proposals/scheduling/resources.md#resource-quantities)

Wenn Sie es vorziehen, dies auszuprobieren, ohne selbst eine vollständige Umgebung bereitzustellen, können Sie mit dem [Azure Arc-Schnelleinstieg](https://azurearcjumpstart.io/azure_arc_jumpstart/azure_arc_data/) in Azure Kubernetes Service (AKS), AWS Elastic Kubernetes Service (EKS), Google Cloud Kubernetes Engine (GKE) oder auf einer Azure-VM einsteigen.


## <a name="deploy-an-arc-data-controller-configured-to-use-the-direct-connectivity-mode"></a>Bereitstellen eines Arc-Datencontrollers, der für die Verwendung des „direkten“ Konnektivitätsmodus konfiguriert ist

Anforderung: Bevor Sie eine PostgreSQL Hyperscale-Servergruppe mit Azure Arc-Unterstützung bereitstellen, die Sie über das Azure-Portal betreiben, müssen Sie zunächst einen Azure Arc-Datencontroller bereitstellen, der für die Verwendung des Konnektivitätsmodus *Direkt* konfiguriert ist.
Führen Sie zum Bereitstellen eines Arc-Datencontrollers die Anweisungen in den folgenden Artikeln aus:
1. [Bereitstellen des Datencontrollers: Direkter Verbindungsmodus (Voraussetzungen)](deploy-data-controller-direct-mode-prerequisites.md)
1. [Bereitstellen eines Azure Arc-Datencontrollers | Direkter Verbindungsmodus](deploy-data-controller-direct-mode.md)


## <a name="preliminary-and-temporary-step-for-openshift-users-only"></a>Vorläufiger und temporärer Schritt nur für OpenShift-Benutzer
Implementieren Sie diesen Schritt, bevor Sie mit dem nächsten fortfahren. Sie müssen die folgenden Befehle zum Updaten der Sicherheitseinschränkungen für Ihren Cluster ausführen, um eine PostgreSQL Hyperscale-Servergruppe in Red Hat OpenShift in einem Projekt bereitzustellen, das nicht dem Standardprojekt entspricht. Dieser Befehl gewährt den Dienstkonten, unter denen Ihre PostgreSQL Hyperscale-Servergruppe ausgeführt wird, die erforderlichen Berechtigungen. Die Sicherheitskontexteinschränkung arc-data-scc haben Sie hinzugefügt, als Sie den Azure Arc-Datencontroller bereitgestellt haben.

```Console
oc adm policy add-scc-to-user arc-data-scc -z <server-group-name> -n <namespace name>
```

**Server-group-name entspricht dem Namen der Servergruppe, die Sie im nächsten Schritt erstellen.**

Weitere Informationen zu Sicherheitskontexteinschränkungen in OpenShift finden Sie in der [OpenShift-Dokumentation](https://docs.openshift.com/container-platform/4.2/authentication/managing-security-context-constraints.html). 

Fahren Sie mit dem nächsten Schritt fort.

## <a name="deploy-an-azure-arc-enabled-postgresql-hyperscale-server-group-from-the-azure-portal"></a>Bereitstellen einer PostgreSQL Hyperscale-Servergruppe mit Azure Arc-Unterstützung über das Azure-Portal

Zum Bereitstellen und Betreiben einer PostgreSQL Hyperscale-Servergruppe mit Azure Arc-Unterstützung über das Azure-Portal müssen Sie sie auf einem Arc-Datencontroller bereitstellen, der für die Verwendung des Konnektivitätsmodus *Direkt* konfiguriert ist. 

> [!IMPORTANT]
> Sie können eine PostgreSQL Hyperscale-Servergruppe mit Azure Arc-Unterstützung nicht über das Azure-Portal betreiben, wenn Sie sie auf einem Azure Arc-Datencontroller bereitgestellt haben, der für die Verwendung des Konnektivitätsmodus *Indirekt* konfiguriert ist. 

Nachdem Sie einen Arc-Datencontroller bereitgestellt haben, der für den Konnektivitätsmodus „Direkt“ aktiviert ist:
1. Öffnen Sie in einem Browser die folgende URL: [https://portal.azure.com](https://portal.azure.com).
2. Suchen Sie im Suchfenster oben auf der Seite im Azure Marketplace nach „*azure arc postgres*“, und wählen Sie **Azure Database for PostgreSQL-Servergruppen – Azure Arc** aus.
3. Klicken Sie auf der Seite, die geöffnet wird, oben links auf **+ Erstellen**. 
4. Füllen Sie das Formular wie beim Bereitstellen einer anderen Azure-Ressource aus.


### <a name="important-parameters-you-should-consider-are"></a>Wichtige Parameter, die Sie berücksichtigen sollten, sind:

- **Die Anzahl von Workerknoten**, die Sie zum Aufskalieren und ggf. zum Verbessern der Leistung bereitstellen möchten. Lesen Sie die [Konzepte zu Postgres Hyperscale](concepts-distributed-postgres-hyperscale.md), bevor Sie fortfahren. Wenn Sie beispielsweise eine Servergruppe mit zwei Workerknoten bereitstellen, werden bei der Bereitstellung drei Pods erstellt: einer für den Koordinatorknoten/die Koordinatorinstanz und zwei für die Workerknoten/-instanzen (jeweils einer für jeden Worker).

## <a name="next-steps"></a>Nächste Schritte

- Verbinden mit Ihrem PostgreSQL Hyperscale mit Azure Arc-Unterstützung: Lesen Sie hierzu [Verbindungsendpunkte und Verbindungsstrings abrufen](get-connection-endpoints-and-connection-strings-postgres-hyperscale.md).
- Lesen Sie die Konzepte und Schrittanleitungen zu Azure Database for PostgreSQL Hyperscale, um Ihre Daten auf mehrere PostgreSQL Hyperscale-Knoten zu verteilen und so von einer besseren Leistung zu profitieren:
    * [Knoten und Tabellen](../../postgresql/concepts-hyperscale-nodes.md)
    * [Festlegen des Anwendungstyps](../../postgresql/concepts-hyperscale-app-type.md)
    * [Auswählen einer Verteilungsspalte](../../postgresql/concepts-hyperscale-choose-distribution-column.md)
    * [Tabellenzusammenstellung](../../postgresql/concepts-hyperscale-colocation.md)
    * [Verteilen und Ändern von Tabellen](../../postgresql/howto-hyperscale-modify-distributed-tables.md)
    * [Entwerfen einer Datenbank mit mehreren Mandanten](../../postgresql/tutorial-design-database-hyperscale-multi-tenant.md)*
    * [Entwerfen eines Dashboards für Echtzeitanalysen](../../postgresql/tutorial-design-database-hyperscale-realtime.md)*

    > \* Überspringen Sie die Abschnitte **Anmelden am Azure-Portal** und **Erstellen einer Azure Database for PostgreSQL-Instanz für Hyperscale (Citus)** in den oben aufgeführten Dokumenten. Implementieren Sie die restlichen Schritte in Ihrer Azure Arc-Bereitstellung. Diese Abschnitte sind speziell für den PaaS-Dienst „Azure Database for PostgreSQL Hyperscale (Citus)“ in der Azure-Cloud vorgesehen. Die anderen Abschnitte der Dokumente sind jedoch direkt auf Ihre Azure Arc-fähige PostgreSQL Hyperscale-Instanz übertragbar.

- [Aufskalieren der PostgreSQL Hyperscale-Servergruppe mit Azure Arc-Unterstützung](scale-out-postgresql-hyperscale-server-group.md)
- [Speicherkonfiguration und Kubernetes-Speicherkonzepte](storage-configuration.md)
- [Erweitern von Ansprüchen persistenter Volumes](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#expanding-persistent-volumes-claims)
- [Kubernetes-Ressourcenmodell](https://github.com/kubernetes/community/blob/master/contributors/design-proposals/scheduling/resources.md#resource-quantities)


