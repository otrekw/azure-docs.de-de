---
title: Anzeigen von Protokollen und Metriken mithilfe von Kibana und Grafana
description: Anzeigen von Protokollen und Metriken mithilfe von Kibana und Grafana
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
ms.date: 12/08/2020
ms.topic: how-to
ms.openlocfilehash: cb53aba300b933c78d9ac2f5fc5cf8054f3413e3
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "104670000"
---
# <a name="view-logs-and-metrics-using-kibana-and-grafana"></a>Anzeigen von Protokollen und Metriken mithilfe von Kibana und Grafana

Kibana- und Grafana-Webdashboards werden bereitgestellt, um Erkenntnisse und Klarheit in Bezug auf die Kubernetes-Namespaces zu vermitteln, die von Azure Arc-fähigen Datendiensten verwendet werden.

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]


## <a name="monitor-azure-sql-managed-instances-on-azure-arc"></a>Überwachen verwalteter Azure SQL-Instanzen in Azure Arc

Führen Sie den folgenden `azdata`-CLI-Befehl aus, um auf die Protokolle und Überwachungsdashboards für Arc-fähige SQL Managed Instance-Instanzen zuzugreifen.

```bash

azdata arc sql endpoint list -n <name of SQL instance>

```
Diese Grafana-Dashboards sind relevant:

* „Azure SQL managed instance Metrics“ (Metriken für verwaltete Azure SQL-Instanzen)
* „Host Node Metrics“ (Hostknotenmetriken)
* „Host Pods Metrics“ (Hostpodmetriken)


> [!NOTE]
>  Wenn Sie zur Eingabe eines Benutzernamens und Kennworts aufgefordert werden, geben Sie denselben Benutzernamen und dasselbe Kennwort ein wie bei der Erstellung des Azure Arc-Datencontrollers.

> [!NOTE]
>  Sie werden in einer Zertifikatwarnung hierzu aufgefordert, da die in der Vorschauversion verwendeten Zertifikate selbstsignierte Zertifikate sind.


## <a name="monitor-azure-database-for-postgresql-hyperscale-on-azure-arc"></a>Überwachen von Azure Database for PostgreSQL Hyperscale in Azure Arc

Führen Sie den folgenden `azdata`-CLI-Befehl aus, um auf die Protokolle und Überwachungsdashboards für PostgreSQL Hyperscale zuzugreifen.

```bash

azdata arc postgres endpoint list -n <name of postgreSQL instance>

```

Diese PostgreSQL-Dashboards sind relevant:

* „Postgres Metrics“ (Postgres-Metriken)
* „Postgres Table Metrics“ (Postgres-Tabellenmetriken)
* „Host Node Metrics“ (Hostknotenmetriken)
* „Host Pods Metrics“ (Hostpodmetriken)


## <a name="additional-firewall-configuration"></a>Zusätzliche Firewallkonfiguration

Je nachdem, wo der Datencontroller bereitgestellt wurde, müssen Sie Ports in Ihrer Firewall öffnen, um auf die Kibana- und Grafana-Endpunkte zugreifen zu können.

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

