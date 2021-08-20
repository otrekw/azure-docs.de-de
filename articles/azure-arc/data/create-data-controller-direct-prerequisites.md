---
title: Voraussetzungen | Direkter Verbindungsmodus
description: Hier werden die Voraussetzungen zum Bereitstellen des Datencontrollers im direkten Verbindungsmodus erläutert.
author: dnethi
ms.author: dinethi
ms.reviewer: mikeray
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
ms.date: 03/31/2021
ms.topic: overview
ms.openlocfilehash: 143a601f89301d3ed4302efa6e978cba53539e2c
ms.sourcegitcommit: ee8ce2c752d45968a822acc0866ff8111d0d4c7f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/14/2021
ms.locfileid: "113733918"
---
# <a name="prerequisites-to-deploy-the-data-controller-in-direct-connectivity-mode"></a>Hier werden die Voraussetzungen zum Bereitstellen des Datencontrollers im direkten Konnektivitätsmodus erläutert

In diesem Artikel wird beschrieben, wie Sie die Bereitstellung eines Datencontrollers für Azure Arc-fähige Datendienste im direkten Verbindungsmodus vorbereiten. Die Bereitstellung des Azure Arc-Datencontrollers erfordert zusätzliche Kenntnisse und Konzepte, wie unter [Planen der Bereitstellung von Azure Arc-fähigen Datendiensten](plan-azure-arc-data-services.md) beschrieben.

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

Zu den wichtigsten Voraussetzungen für die Erstellung von Azure Arc Data Controllern im **direkten** Konnektivitätsmodus gehören:

1. Das Verbinden eines Kubernetes-Clusters mit Azure unter der Verwendung von Azure Arc-fähigem Kubernetes
2. Erstellen des Dienstprinzipals und Konfigurieren von Rollen für Metriken
3. Das Erstellen von Azure Arc-fähigen Datendienstdatencontrollern. Dieser Schritt umfasst das Erstellen von
    - einer Azure Arc-Datendiensterweiterung
    - einem benutzerdefinierten Speicherort
    - einem Azure Arc-Datencontroller

## <a name="1-connect-kubernetes-cluster-to-azure-using-azure-arc-enabled-kubernetes"></a>1. Das Verbinden eines Kubernetes-Clusters mit Azure unter der Verwendung von Azure Arc-fähigem Kubernetes

Sie können Ihren Kubernetes-Cluster mit Azure verbinden, indem Sie die CLI ```az``` mit den folgenden Erweiterungen und Helm verwenden.

#### <a name="install-tools"></a>Installieren von Tools

- Helm-Version 3.3 oder höher ([Installieren](https://helm.sh/docs/intro/install/))
- Informationen zur Installation und auf die neueste Version der Azure CLI finden Sie unter ([Installieren](/sql/azdata/install/deploy-install-azdata))

#### <a name="add-extensions-for-azure-cli"></a>Hinzufügen von Erweiterungen für die Azure CLI

Installieren Sie die neuesten Versionen der folgenden Azure CLI-Erweiterungen:
- ```k8s-extension```
- ```connectedk8s```
- ```k8s-configuration```
- `customlocation`

Führen Sie den folgenden Befehl aus, um die Azure CLI-Erweiterungen zu installieren:

```azurecli
az extension add --name k8s-extension
az extension add --name connectedk8s
az extension add --name k8s-configuration
az extension add --name customlocation
```

Wenn Sie zuvor die ```k8s-extension```, ```connectedk8s```, ```k8s-configuration```, `customlocation` Erweiterungen installiert haben, können Sie über den folgenden Befehl auf die neueste Version aktualisieren:

```azurecli
az extension update --name k8s-extension
az extension update --name connectedk8s
az extension update --name k8s-configuration
az extension update --name customlocation
```
#### <a name="connect-your-cluster-to-azure"></a>Verbinden Sie Ihren Cluster mit Azure

Führen Sie zum Abschließen dieser Aufgabe die Schritte unter [Schnellstart: Herstellen einer Verbindung eines vorhandenen Kubernetes-Clusters mit Azure Arc](../kubernetes/quickstart-connect-cluster.md) aus.

Nachdem Sie Ihren Cluster mit Azure verbunden haben, können Sie mit dem Erstellen eines Dienstprinzipals fortfahren. 

## <a name="2-create-service-principal-and-configure-roles-for-metrics"></a>2. Das Erstellen des Dienstprinzipals und das Konfigurieren von Rollen für Metriken

Führen Sie die im Artikel [Hochladen von Nutzungsdaten, Metriken und Protokollen in Azure Monitor](upload-metrics-and-logs-to-azure-monitor.md) beschriebenen Schritte aus, erstellen Sie einen Dienstprinzipal, und gewähren Sie die Rollen wie in diesem Artikel beschrieben. 

Beim [Bereitstellen des Azure Arc-Datencontrollers](create-data-controller-direct-azure-portal.md) sind die Informationen für die SPN-Client-ID, die Mandanten-ID und den geheimen Clientschlüssel erforderlich. 

## <a name="3-create-azure-arc-data-services"></a>3. Das Erstellen eines Azure Arc-Datendienstes

Wenn diese Voraussetzungen erfüllt sind, können Sie die Schritte unter [Bereitstellen eines Azure Arc-Datencontrollers | Direkter Verbindungsmodus](create-data-controller-direct-azure-portal.md) ausführen.


