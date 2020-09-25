---
title: Erstellen von Azure Arc-fähigen PostgreSQL Hyperscale-Servergruppen mit Azure Data Studio
description: Erstellen von Azure Arc-fähigen PostgreSQL Hyperscale-Servergruppen mit Azure Data Studio
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: TheJY
ms.author: jeanyd
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: af0cdb814433b739b15d79bec9cb399cf0a2fef7
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90931596"
---
# <a name="create-azure-arc-enabled-postgresql-hyperscale-using-azure-data-studio"></a>Erstellen von Azure Arc-fähigen PostgreSQL Hyperscale-Servergruppen mit Azure Data Studio

In diesem Dokument werden die Schritte zum Verwenden von Azure Data Studio zum Bereitstellen von Azure Arc-fähigen PostgreSQL Hyperscale-Servergruppen beschrieben.

[!INCLUDE [azure-arc-common-prerequisites](../../../includes/azure-arc-common-prerequisites.md)]

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="connect-to-the-azure-arc-data-controller"></a>Herstellen einer Verbindung mit dem Azure Arc-Datencontroller

Bevor Sie eine Instanz erstellen können, müssen Sie sich beim Azure Arc-Datencontroller anmelden, wenn dies noch nicht geschehen ist.

```console
azdata login
```

Sie werden dann dazu aufgefordert, den Namespace anzugeben, in dem der Datencontroller erstellt wurde, sowie den Benutzernamen und das Kennwort, um sich beim Controller anzumelden.

> Wenn Sie den Namespace validieren müssen, können Sie ```kubectl get pods -A``` ausführen, um eine Liste aller Namespaces im Cluster abzurufen.

```console
Username: arcadmin
Password:
Namespace: arc
Logged in successfully to `https://10.0.0.4:30080` in namespace `arc`. Setting active context to `arc`
```

## <a name="create-an-azure-arc-enabled-postgresql-hyperscale-server-group"></a>Erstellen einer Azure Arc-fähigen PostgreSQL Hyperscale-Servergruppe

1. Starten von Azure Data Studio
1. Klicken Sie auf der Registerkarte „Verbindungen“ auf die drei Punkte oben links, und wählen Sie dann „Neue Bereitstellung“ aus.
1. Wählen Sie in den Bereitstellungsoptionen **PostgreSQL Hyperscale-Servergruppe - Azure Arc** aus.
    >[!NOTE]
    > Möglicherweise werden Sie hier dazu aufgefordert, die `azdata`-CLI zu installieren, wenn dies noch nicht geschehen ist.
1. Akzeptieren Sie die Datenschutz- und Lizenzbedingungen, und klicken Sie unten auf **Auswählen**.
1. Geben Sie auf dem Blatt „PostgreSQL Hyperscale-Servergruppe - Azure Arc“ die folgenden Informationen ein:
   - Geben Sie einen Namen für die Servergruppe ein.
   - Geben Sie ein Kennwort für den _Postgres_-Administratorbenutzer der Servergruppe ein, und bestätigen Sie es.
   - Wählen Sie die Speicherklasse aus, die für die Daten geeignet ist.
   - Wählen Sie die Speicherklasse aus, die für Protokolle geeignet ist.
   - Wählen Sie die Speicherklasse aus, die für Sicherungen geeignet ist.
   - Wählen Sie die Anzahl der bereitzustellenden Workerknoten aus.
1. Klicken Sie auf die Schaltfläche **Bereitstellen**.

Dadurch wird die Erstellung der Azure Arc-fähigen PostgreSQL Hyperscale-Servergruppe für den Datencontroller gestartet.

Nach einigen Minuten sollte die Erstellung erfolgreich abgeschlossen werden.

## <a name="next-steps"></a>Nächste Schritte
- [Verwalten der Servergruppe mit Azure Data Studio](manage-postgresql-hyperscale-server-group-with-azure-data-studio.md)
- [Überwachen der Servergruppe](monitor-grafana-kibana.md)
- Lesen Sie die Konzepte und Schrittanleitungen zu Azure Database for PostgreSQL Hyperscale, um Ihre Daten auf mehrere PostgreSQL Hyperscale-Knoten zu verteilen und von der gesamten Leistung von Azure Database for Postgres Hyperscale zu profitieren. :
    * [Knoten und Tabellen](../../postgresql/concepts-hyperscale-nodes.md)
    * [Festlegen des Anwendungstyps](../../postgresql/concepts-hyperscale-app-type.md)
    * [Auswählen einer Verteilungsspalte](../../postgresql/concepts-hyperscale-choose-distribution-column.md)
    * [Tabellenzusammenstellung](../../postgresql/concepts-hyperscale-colocation.md)
    * [Verteilen und Ändern von Tabellen](../../postgresql/howto-hyperscale-modify-distributed-tables.md)
    * [Entwerfen einer Datenbank mit mehreren Mandanten](../../postgresql/tutorial-design-database-hyperscale-multi-tenant.md)*
    * [Entwerfen eines Dashboards für Echtzeitanalysen](../../postgresql/tutorial-design-database-hyperscale-realtime.md)*

    > \* Überspringen Sie die Abschnitte **Anmelden am Azure-Portal** und **Erstellen einer Azure Database for PostgreSQL-Instanz für Hyperscale (Citus)** in den oben aufgeführten Dokumenten. Implementieren Sie die restlichen Schritte in Ihrer Azure Arc-Bereitstellung. Diese Abschnitte sind speziell für den PaaS-Dienst „Azure Database for PostgreSQL Hyperscale (Citus)“ in der Azure-Cloud vorgesehen. Die anderen Abschnitte der Dokumente sind jedoch direkt auf Ihre Azure Arc-fähige PostgreSQL Hyperscale-Instanz übertragbar.

- [Skalieren der Azure Database for PostgreSQL Hyperscale-Servergruppe](scale-out-postgresql-hyperscale-server-group.md)
- [Speicherkonfiguration und Kubernetes-Speicherkonzepte](storage-configuration.md)
- [Erweitern von Ansprüchen persistenter Volumes](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#expanding-persistent-volumes-claims)
- [Kubernetes-Ressourcenmodell](https://github.com/kubernetes/community/blob/master/contributors/design-proposals/scheduling/resources.md#resource-quantities)

