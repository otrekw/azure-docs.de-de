---
title: Anzeigen von Protokollen und Metriken mithilfe von Kibana und Grafana
description: Anzeigen von Protokollen und Metriken mithilfe von Kibana und Grafana
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: 3693c30a34601512770f5d9071f5d786410fb00e
ms.sourcegitcommit: 28c5fdc3828316f45f7c20fc4de4b2c05a1c5548
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2020
ms.locfileid: "92360376"
---
# <a name="view-logs-and-metrics-using-kibana-and-grafana"></a>Anzeigen von Protokollen und Metriken mithilfe von Kibana und Grafana

Kibana- und Grafana-Webdashboards werden bereitgestellt, um Erkenntnisse und Klarheit in Bezug auf die Kubernetes-Namespaces zu vermitteln, die von Azure Arc-fähigen Datendiensten verwendet werden.

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="retrieve-the-ip-address-of-your-cluster"></a>Abrufen der IP-Adresse Ihres Clusters

Für den Zugriff auf die Dashboards müssen Sie die IP-Adresse Ihres Clusters abrufen. Die zum Abrufen der richtigen IP-Adresse verwendete Methode hängt davon ab, wie Sie Kubernetes bereitstellen. Sehen Sie sich die folgenden Optionen an, um die richtige für Sie zu finden.

### <a name="azure-virtual-machine"></a>Virtueller Azure-Computer

Verwenden Sie den folgenden Befehl, um die öffentliche IP-Adresse abzurufen:

```azurecli
az network public-ip list -g azurearcvm-rg --query "[].{PublicIP:ipAddress}" -o table
```

### <a name="kubeadm-cluster"></a>kubeadm-Cluster

Verwenden Sie den folgenden Befehl, um die Cluster-IP-Adresse abzurufen:

```console
kubectl cluster-info
```


### <a name="aks-or-other-load-balanced-cluster"></a>AKS oder anderer Cluster mit Lastenausgleich

Zum Überwachen Ihrer Umgebung in AKS oder einem anderen Cluster mit Lastenausgleich müssen Sie die IP-Adresse des Verwaltungsproxydiensts abrufen. Verwenden Sie den folgenden Befehl, um die **externe IP-Adresse** abzurufen:

```console
kubectl get svc mgmtproxy-svc-external -n <namespace>

#Example:
#kubectl get svc mgmtproxy-svc-external -n arc
NAME                     TYPE           CLUSTER-IP    EXTERNAL-IP     PORT(S)           AGE
mgmtproxy-svc-external   LoadBalancer   10.0.186.28   52.152.148.25   30777:30849/TCP   19h
```

## <a name="additional-firewall-configuration"></a>Zusätzliche Firewallkonfiguration

Möglicherweise stellen Sie fest, dass Sie Ports in Ihrer Firewall öffnen müssen, um auf die Kibana- und Grafana-Endpunkte zugreifen zu können.

Im Folgenden sehen Sie ein Beispiel für diesen Vorgang für eine Azure-VM. Diese Aktion ist erforderlich, wenn Sie Kubernetes mithilfe des Skripts bereitgestellt haben.

In den folgenden Schritten wird gezeigt, wie Sie eine NSG-Regel für die Kibana- und Grafana-Endpunkte erstellen:

### <a name="find-the-name-of-the-nsg"></a>Ermitteln Sie den Namen der NSG.

```azurecli
az network nsg list -g azurearcvm-rg --query "[].{NSGName:name}" -o table
```

### <a name="add-the-nsg-rule"></a>Fügen Sie die NSG-Regel hinzu.

Sobald Sie den Namen der NSG kennen, können Sie mithilfe des folgenden Befehls eine Regel hinzufügen:

```azurecli
az network nsg rule create -n ports_30777 --nsg-name azurearcvmNSG --priority 600 -g azurearcvm-rg --access Allow --description 'Allow Kibana and Grafana ports' --destination-address-prefixes '*' --destination-port-ranges 30777 --direction Inbound --protocol Tcp --source-address-prefixes '*' --source-port-ranges '*'
```

## <a name="monitor-azure-sql-managed-instances-on-azure-arc"></a>Überwachen verwalteter Azure SQL-Instanzen in Azure Arc

Nun, da Sie über die IP-Adresse verfügen und die Ports geöffnet haben, sollten Sie auf Grafana und Kibana zugreifen können.

> [!NOTE]
>  Wenn Sie zur Eingabe eines Benutzernamens und Kennworts aufgefordert werden, geben Sie denselben Benutzernamen und dasselbe Kennwort ein wie bei der Erstellung des Azure Arc-Datencontrollers.

> [!NOTE]
>  Sie werden in einer Zertifikatwarnung hierzu aufgefordert, da die in der Vorschauversion verwendeten Zertifikate selbstsignierte Zertifikate sind.

Verwenden Sie das folgende URL-Muster für den Zugriff auf die Protokollierungs- und Überwachungsdashboards für verwaltete Azure SQL-Instanzen:

```html
https://<external-ip-from-above>:30777/grafana
https://<external-ip-from-above>:30777/kibana
```

Die relevanten Dashboards sind:

* „Azure SQL managed instance Metrics“ (Metriken für verwaltete Azure SQL-Instanzen)
* „Host Node Metrics“ (Hostknotenmetriken)
* „Host Pods Metrics“ (Hostpodmetriken)

## <a name="monitor-azure-database-for-postgresql-hyperscale---azure-arc"></a>Überwachen von Azure Database for PostgreSQL Hyperscale – Azure Arc

Verwenden Sie das folgende URL-Muster für den Zugriff auf die Protokollierungs- und Überwachungsdashboards für PostgreSQL Hyperscale:

```html
https://<external-ip-from-above>:30777/grafana
https://<external-ip-from-above>:30777/kibana
```

Die relevanten Dashboards sind:

* „Postgres Metrics“ (Postgres-Metriken)
* „Postgres Table Metrics“ (Postgres-Tabellenmetriken)
* „Host Node Metrics“ (Hostknotenmetriken)
* „Host Pods Metrics“ (Hostpodmetriken)

## <a name="next-steps"></a>Nächste Schritte
- Versuchen Sie, [Metriken und Protokolle in Azure Monitor hochzuladen](upload-metrics-and-logs-to-azure-monitor.md).
- Informieren Sie sich über Grafana:
   - [Erste Schritte](https://grafana.com/docs/grafana/latest/getting-started/getting-started)
   - [Grafana-Grundlagen](https://grafana.com/tutorials/grafana-fundamentals/#1)
   - [Grafana-Tutorials](https://grafana.com/tutorials/grafana-fundamentals/#1)
- Informieren Sie sich über Kibana:
   - [Introduction (Einführung)](https://www.elastic.co/webinars/getting-started-kibana?baymax=default&elektra=docs&storm=top-video)
   - [Kibana-Leitfaden](https://www.elastic.co/guide/en/kibana/current/index.html)
   - [Einführung in Dashboarddrilldowns mit Datenvisualisierungen in Kibana](https://www.elastic.co/webinars/dashboard-drilldowns-with-data-visualizations-in-kibana/)
   - [Erstellen von Kibana-Dashboards](https://www.elastic.co/webinars/how-to-build-kibana-dashboards/)

