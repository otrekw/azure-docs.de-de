---
title: Erstellen einer PostgreSQL Hyperscale-Servergruppe mit Azure Arc-Unterstützung über das Azure-Portal
description: Erstellen einer PostgreSQL Hyperscale-Servergruppe mit Azure Arc-Unterstützung über das Azure-Portal
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: TheJY
ms.author: jeanyd
ms.reviewer: mikeray
ms.date: 07/30/2021
ms.topic: how-to
ms.openlocfilehash: 382e4e855b0a4925cfaae2f7746587a7ab2fd951
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122355979"
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
1. [Bereitstellen des Datencontrollers: Direkter Verbindungsmodus (Voraussetzungen)](create-data-controller-direct-prerequisites.md)
1. [Bereitstellen eines Azure Arc-Datencontrollers im direkten Konnektivitätsmodus über das Azure-Portal](create-data-controller-direct-azure-portal.md)


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

Nachdem Sie einen zum direkten Konnektivitätsmodus fähigen Arc-Datencontroller bereitgestellt haben, können Sie eine der folgenden drei Optionen auswählen, um eine Azure Arc-fähige Postgres Hyperscale-Servergruppe bereitzustellen:

### <a name="option-1-deploy-from-the-azure-marketplace"></a>Option 1: Bereitstellen über den Azure Marketplace
1. Öffnen Sie folgende URL in einem Browser: [https://portal.azure.com](https://portal.azure.com)
2. Suchen Sie im Suchfenster oben auf der Seite im Azure Marketplace nach „*azure arc postgres*“, und wählen Sie **Azure Database for PostgreSQL-Servergruppen – Azure Arc** aus.
3. Klicken Sie auf der Seite, die geöffnet wird, oben links auf **+ Erstellen**. 
4. Füllen Sie das Formular wie beim Bereitstellen einer anderen Azure-Ressource aus.

### <a name="option-2-deploy-from-the-azure-database-for-postgresql-deployment-option-page"></a>Option 2: Bereitstellen über die Azure Database for PostgreSQL-Bereitstellungsoptionenseite
1. Öffnen Sie folgende URL in einem Browser: https://ms.portal.azure.com/#create/Microsoft.PostgreSQLServer
2. Klicken Sie unten rechts auf die Kachel. Ihr Titel lautet: PostgreSQL Hyperscale mit Azure Arc-Unterstützung (Vorschauversion).
3. Füllen Sie das Formular wie beim Bereitstellen einer anderen Azure-Ressource aus.

### <a name="option-3-deploy-from-the-azure-arc-center"></a>Option 3: Bereitstellen über das Azure Arc-Center
1. Öffnen Sie folgende URL in einem Browser: https://ms.portal.azure.com/#blade/Microsoft_Azure_HybridCompute/AzureArcCenterBlade/overview
1. Klicken Sie in der Mitte der Seite auf [Bereitstellen] unter der Kachel *Azure-Dienste bereitstellen* und klicken Sie dann in der Kachel „PostgreSQL Hyperscale (Vorschau)“ auf [Bereitstellen].
2. Oder klicken Sie im Navigationsbereich links auf der Seite im Abschnitt „Dienste“ auf [PostgreSQL Hyperscale (Vorschau)] und dann im Bereich oben links auf [+ Erstellen].


#### <a name="important-parameters-you-should-consider"></a>Wichtige Parameter, die Sie berücksichtigen sollten:

- **Anzahl von Workerknoten**, die Sie zum Aufskalieren und ggf. zum Verbessern der Leistung bereitstellen möchten. Lesen Sie die [Konzepte zu Postgres Hyperscale](concepts-distributed-postgres-hyperscale.md), bevor Sie fortfahren. Die folgende Tabelle gibt den Bereich der unterstützten Werte und die Form der Postgres-Bereitstellung an, die Sie damit jeweils erhalten. Wenn Sie z. B. eine Servergruppe mit zwei Workerknoten bereitstellen möchten, geben Sie „2“ an. Dadurch werden drei Pods erstellt: einer für den Koordinatorknoten/die Koordinatorinstanz und zwei für die Workerknoten/Workerinstanzen (einer für jeden Worker).



|Sie benötigen Folgendes:   |Form der bereitzustellenden Servergruppe   |Anzahl anzugebender Workerknoten   |Hinweis   |
|---|---|---|---|
|Eine horizontal aufskalierte Form von Postgres, um die Skalierbarkeitsanforderungen Ihrer Anwendungen zu erfüllen.   |3 oder mehr Postgres-Instanzen, 1 ist Koordinator, n sind Worker mit n >= 2.   |n, bei n >= 2.   |Die Citus-Erweiterung, die die Hyperscale-Funktion bereitstellt, wurde geladen.   |
|Eine grundlegende Form von Postgres Hyperscale, mit der Sie Ihre Anwendung zu minimalen Kosten funktional überprüfen können. Es ist keine gültige Leistungs- und Skalierbarkeitsvalidierung damit möglich. Hierfür müssen Sie den oben beschriebenen Bereitstellungstyp verwenden.   |1 Postgres-Instanz, die sowohl Koordinator als auch Worker ist.   |0, und Citus zur Liste der zu ladenden Erweiterungen hinzufügen.   |Die Citus-Erweiterung, die die Hyperscale-Funktion bereitstellt, wurde geladen.   |
|Eine einfache Instanz von Postgres, die bei Bedarf horizontal aufskaliert werden kann.   |1 Postgres-Instanz. Die Semantik für Koordinator und Worker ist ihr noch nicht bekannt. Um sie nach der Bereitstellung aufzuskalieren, bearbeiten Sie die Konfiguration, erhöhen Sie die Anzahl der Workerknoten und verteilen Sie die Daten.   |0   |Die Citus-Erweiterung, die die Hyperscale-Funktion bereitstellt, ist in Ihrer Bereitstellung vorhanden, aber noch nicht geladen.   |
|   |   |   |   |

1 Worker anzugeben funktioniert zwar, es wird jedoch nicht empfohlen, ihn zu verwenden. Diese Bereitstellung bietet Ihnen nicht viel Nutzen. Sie umfasst 2 Instanzen von Postgres: 1 Koordinator und 1 Worker. In dieser Konfiguration werden die Daten tatsächlich nicht aufskaliert, da Sie einen einzelnen Worker bereitstellen. Daher erhalten Sie kein höheres Maß an Leistung und Skalierbarkeit. Diese Unterstützung für solche Bereitstellungen wird in einer zukünftigen Version entfernt.

- **Speicherklassen**, die von der Servergruppe verwendet werden sollen. Es ist wichtig, dass Sie die Speicherklasse direkt beim Bereitstellen einer Servergruppe festlegen, da diese nach der Bereitstellung nicht mehr geändert werden kann. Um die Speicherklasse nach der Bereitstellung zu ändern, müssten Sie die Daten extrahieren, Ihre Servergruppe löschen, eine neue Servergruppe erstellen und die Daten importieren. Sie können die Speicherklassen angeben, die für die Daten, Protokolle und Sicherungen verwendet werden sollen. Wenn Sie keine Speicherklassen angeben, werden standardmäßig die Speicherklassen des Datencontrollers verwendet.
    - Um die Speicherklasse für die Daten festzulegen, geben Sie den Parameter `--storage-class-data` oder `-scd` gefolgt vom Namen der Speicherklasse an.
    - Um die Speicherklasse für die Protokolle festzulegen, geben Sie den Parameter `--storage-class-logs` oder `-scl` gefolgt vom Namen der Speicherklasse an.
    - Um die Speicherklasse für die Sicherungen festzulegen: In dieser Vorschauversion von PostgreSQL Hyperscale mit Azure Arc-Unterstützung gibt es je nach Sicherungs-/Wiederherstellungsvorgängen, die Sie ausführen möchten, zwei Möglichkeiten zum Festlegen von Speicherklassen. Wir arbeiten daran, diese Funktion zu vereinfachen. Sie geben entweder eine Speicherklasse oder eine Volumeanspruchseinbindung an. Bei einer Volumeanspruchseinbindung handelt es sich um ein durch einen Doppelpunkt getrenntes Paar, das aus einem Anspruch auf ein vorhandenes persistentes Volume (Persistent Volume Claim, PVC) im selben Namespace und dem Volumetyp (sowie je nach Volumetyp optionalen Metadaten) besteht. Das persistente Volume wird in jedem Pod für die PostgreSQL-Servergruppe eingebunden.
        - Wenn Sie nur vollständige Datenbankwiederherstellungen ausführen möchten, geben Sie den Parameter `--storage-class-backups` oder `-scb` gefolgt vom Namen der Speicherklasse an.
        - Wenn Sie sowohl vollständige Datenbankwiederherstellungen als auch Zeitpunktwiederherstellungen ausführen möchten, geben Sie den Parameter `--volume-claim-mounts` oder `--volume-claim-mounts` gefolgt vom Namen eines Volumeanspruchs und eines Volumetyps an.


## <a name="next-steps"></a>Nächste Schritte

- Verbinden mit Ihrem PostgreSQL Hyperscale mit Azure Arc-Unterstützung: Lesen Sie hierzu [Verbindungsendpunkte und Verbindungsstrings abrufen](get-connection-endpoints-and-connection-strings-postgres-hyperscale.md)
- Lesen Sie die Konzepte und Schrittanleitungen zu Azure Database for PostgreSQL Hyperscale, um Ihre Daten auf mehrere PostgreSQL Hyperscale-Knoten zu verteilen und so von einer besseren Leistung zu profitieren:
    * [Knoten und Tabellen](../../postgresql/concepts-hyperscale-nodes.md)
    * [Festlegen des Anwendungstyps](../../postgresql/concepts-hyperscale-app-type.md)
    * [Auswählen einer Verteilungsspalte](../../postgresql/concepts-hyperscale-choose-distribution-column.md)
    * [Tabellenzusammenstellung](../../postgresql/concepts-hyperscale-colocation.md)
    * [Verteilen und Ändern von Tabellen](../../postgresql/howto-hyperscale-modify-distributed-tables.md)
    * [Entwerfen einer Datenbank mit mehreren Mandanten](../../postgresql/tutorial-design-database-hyperscale-multi-tenant.md)*
    * [Entwerfen eines Dashboards für Echtzeitanalysen](../../postgresql/tutorial-design-database-hyperscale-realtime.md)*

    > \* Überspringen Sie die Abschnitte **Anmelden am Azure-Portal** und **Erstellen einer Azure Database for PostgreSQL-Instanz für Hyperscale (Citus)** in den oben aufgeführten Dokumenten. Implementieren Sie die restlichen Schritte in Ihrer Azure Arc-Bereitstellung. Diese Abschnitte sind speziell für den PaaS-Dienst „Azure Database for PostgreSQL Hyperscale (Citus)“ in der Azure-Cloud vorgesehen. Die anderen Abschnitte der Dokumente sind jedoch direkt auf Ihre Instanz von PostgreSQL Hyperscale mit Azure Arc-Unterstützung übertragbar.

- [Aufskalieren der PostgreSQL Hyperscale-Servergruppe mit Azure Arc-Unterstützung](scale-out-in-postgresql-hyperscale-server-group.md)
- [Speicherkonfiguration und Kubernetes-Speicherkonzepte](storage-configuration.md)
- [Erweitern von Ansprüchen persistenter Volumes](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#expanding-persistent-volumes-claims)
- [Kubernetes-Ressourcenmodell](https://github.com/kubernetes/community/blob/master/contributors/design-proposals/scheduling/resources.md#resource-quantities)


