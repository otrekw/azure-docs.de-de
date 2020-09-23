---
title: Problembehandlung von PostgreSQL Hyperscale-Servergruppen
description: Problembehandlung von PostgreSQL Hyperscale-Servergruppen mit einem Jupyter Notebook
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: TheJY
ms.author: jeanyd
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: 9c0d3d9c74be8dabaec20ff5d4c7e7cfc74d8eef
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90931301"
---
# <a name="troubleshooting-postgresql-hyperscale-server-groups"></a>Problembehandlung von PostgreSQL Hyperscale-Servergruppen

Notebooks können Prozeduren dokumentieren, indem sie Markdowninhalte einschließen, die beschreiben, welche Aktionen wie durchzuführen sind. Notebooks können auch ausführbaren Code bereitstellen, um eine Prozedur zu automatisieren.  Dieses Muster eignet sich für zahlreiche Szenarien: von Standardbetriebsprozeduren bis hin zu Problembehandlungsanleitungen.

Beispielsweise können Sie Problembehandlung für eine PostgreSQL Hyperscale-Servergruppe ausführen, bei der möglicherweise Probleme bei der Verwendung von Azure Data Studio auftreten.

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="install-tools"></a>Installieren von Tools

Installieren Sie Azure Data Studio, `kubectl` und `azdata` auf dem Clientcomputer, den Sie zum Ausführen des Notebooks in Azure Data Studio verwenden. Befolgen Sie hierzu die Anweisungen unter [Installieren von Clienttools](install-client-tools.md).

## <a name="update-the-path-environment-variable"></a>Aktualisieren der PATH-Umgebungsvariablen

Stellen Sie sicher, dass diese Tools überall auf diesem Clientcomputer aufgerufen werden können. Aktualisieren Sie z. B. auf einem Windows-Clientcomputer die PATH-Systemumgebungsvariable, und fügen Sie den Ordner hinzu, in dem Sie kubectl installiert haben.

## <a name="sign-in-with-azdata"></a>Melden Sie sich mit `azdata` an.

Melden Sie Ihren Arc-Datencontroller von diesem Clientcomputer aus und vor dem Starten von Azure Data Studio an. Führen Sie zu diesem Zweck beispielsweise den folgenden Befehl aus:

```console
azdata login --endpoint https://<IP address>:<port>
```

Ersetzen Sie `<IP address>` durch die IP-Adresse Ihres Kubernetes-Clusters und `<port>` durch den Port, an dem Kubernetes lauscht. Sie werden zur Eingabe des Benutzernamens und Kennworts aufgefordert. Um weitere Informationen anzuzeigen, führen Sie Folgendes aus: _

```console
azdata login --help
```

## <a name="log-into-your-kubernetes-cluster-with-kubectl"></a>Anmelden bei Ihrem Kubernetes-Cluster mit kubectl

Zu diesem Zweck können Sie die Beispielbefehle verwenden, die in [diesem](https://blog.christianposta.com/kubernetes/logging-into-a-kubernetes-cluster-with-kubectl/) Blogbeitrag bereitgestellt werden.
Sie führen Befehle wie die folgenden aus:

```console
kubectl config view
kubectl config set-credentials kubeuser/my_kubeuser --username=<your Arc Data Controller Admin user name> --password=<password>
kubectl config set-cluster my_kubeuser --server=https://<IP address>:<port>
kubectl config set-context default/my_kubeuser/ArcDataControllerAdmin --user=ArcDataControllerAdmin/my_kubeuser --namespace=arc --cluster=my_kubeuser
kubectl config use-context default/my_kubeuser/ArcDataControllerAdmin
```

### <a name="the-troubleshooting-notebook"></a>Das Notebook zur Problembehandlung

Starten Sie Azure Data Studio, und öffnen Sie das Notebook zur Problembehandlung. 

Implementieren Sie die Schritte, die in [033-manage-Postgres-with-AzureDataStudio.md](manage-postgresql-hyperscale-server-group-with-azure-data-studio.md) beschrieben werden, um die folgenden Aufgaben auszuführen:

1. Herstellen einer Verbindung mit dem Arc-Datencontroller
2. Auswählen Ihrer Postgres-Instanz mit der rechten Maustaste und Auswählen von **[Manage]** (Verwalten)
3. Auswählen des **Dashboards [Diagnose and solve problems]** (Probleme diagnostizieren und beheben).
4. Auswählen des **Links [Troubleshoot]** (Problembehandlung)

:::image type="content" source="media/postgres-hyperscale/ads-controller-postgres-troubleshooting-notebook.jpg" alt-text="Azure Data Studio: Öffnen des PostgreSQL-Notebooks zur Problembehandlung":::

Das Notebook **TSG100: The Azure Arc enabled PostgreSQL Hyperscale troubleshooter** (TSG100: Problembehandlung für Azure Arc-aktiviertes PostgreSQL Hyperscale): :::image type="content" source="media/postgres-hyperscale/ads-controller-postgres-troubleshooting-notebook2.jpg" alt-text="Azure Data Studio: Verwenden des PostgreSQL-Notebooks zur Problembehandlung":::

### <a name="run-the-scripts"></a>Ausführen der Skripts
Klicken Sie oben auf die Schaltfläche „Run all“ (Alle ausführen), um das Notebook sofort auszuführen, oder durchlaufen Sie das Notebook schrittweise, und führen Sie die einzelnen Codezellen nacheinander aus.

Zeigen Sie die Ausgabe der Ausführung der Codezellen an, um potenzielle Probleme zu ermitteln.

Wir werden dem Notebook im Laufe der Zeit weitere Details hinzufügen, um häufige Probleme zu erkennen und zu beheben.

## <a name="next-step"></a>Nächster Schritt
- Weitere Informationen zum [Abrufen von Protokollen für Azure Arc-fähige Datendienste](troubleshooting-get-logs.md)
- Weitere Informationen zum [Suchen nach Protokollen mit Kibana](monitor-grafana-kibana.md)
- Weitere Informationen zur [Überwachung mit Grafana](monitor-grafana-kibana.md)
- Erstellen eigener Notebooks
