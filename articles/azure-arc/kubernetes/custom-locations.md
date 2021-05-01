---
title: Benutzerdefinierte Speicherorte in Kubernetes-Clustern mit Azure Arc-Unterstützung
services: azure-arc
ms.service: azure-arc
ms.date: 04/05/2021
ms.topic: article
author: shashankbarsin
ms.author: shasb
ms.custom: references_regions
description: Verwenden benutzerdefinierter Speicherorte zum Bereitstellen von Azure-PaaS-Diensten in Kubernetes-Clustern mit Azure Arc-Unterstützung
ms.openlocfilehash: b3a0d89f0c352b8344aea68a613653eae43a41e4
ms.sourcegitcommit: 4a54c268400b4158b78bb1d37235b79409cb5816
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108147767"
---
# <a name="custom-locations-on-azure-arc-enabled-kubernetes"></a>Benutzerdefinierte Speicherorte in Kubernetes-Clustern mit Azure Arc-Unterstützung

Als Azure-Speicherorterweiterung bietet *Benutzerdefinierte Speicherorte* Mandantenadministratoren eine Möglichkeit, ihre Kubernetes-Cluster mit Azure Arc-Unterstützung als Zielspeicherorte für die Bereitstellung von Instanzen von Azure-Diensten zu verwenden. Beispiele für Azure-Ressourcen sind u. a. SQL Managed Instance mit Azure Arc-Unterstützung und PostgreSQL Hyperscale mit Azure Arc-Unterstützung.

Ähnlich wie bei Azure-Speicherorten können Endbenutzer im Mandanten mit Zugriff auf „Benutzerdefinierte Speicherorte“ dort Ressourcen bereitstellen, indem sie das private Compute Ihres Unternehmens verwenden.

Eine konzeptionelle Übersicht zu diesem Feature finden Sie im Artikel [„Benutzerdefinierte Speicherorte“ auf Basis von Kubernetes mit Azure Arc-Unterstützung](conceptual-custom-locations.md).

[!INCLUDE [preview features note](./includes/preview/preview-callout.md)]

## <a name="prerequisites"></a>Voraussetzungen

- [Installieren oder aktualisieren Sie Azure CLI](/cli/azure/install-azure-cli) auf eine Version >= 2.16.0.

- Azure CLI-Erweiterungen `connectedk8s` (Version >= 1.1.0), `k8s-extension` (Version >= 0.2.0) und `customlocation` (Version >= 0.1.0). Installieren Sie diese Azure CLI-Erweiterungen, indem Sie die folgenden Befehle ausführen:
  
    ```azurecli
    az extension add --name connectedk8s
    az extension add --name k8s-extension
    az extension add --name customlocation
    ```
    
    Wenn die Erweiterungen `connectedk8s`, `k8s-extension` und `customlocation` bereits installiert sind, können Sie sie mit dem folgenden Befehl auf die neueste Version aktualisieren:

    ```azurecli
    az extension update --name connectedk8s
    az extension update --name k8s-extension
    az extension update --name customlocation
    ```

- Die Anbieterregistrierung ist für `Microsoft.ExtendedLocation` abgeschlossen.
    1. Geben Sie die folgenden Befehle ein:
    
    ```azurecli
    az provider register --namespace Microsoft.ExtendedLocation
    ```

    2. Überwachen Sie den Registrierungsprozess. Die Registrierung kann bis zu 10 Minuten dauern.
    
    ```azurecli
    az provider show -n Microsoft.ExtendedLocation -o table
    ```

>[!NOTE]
>**Unterstützte Regionen für benutzerdefinierte Speicherorte:**
>* East US
>* Europa, Westen

## <a name="enable-custom-locations-on-cluster"></a>Aktivieren benutzerdefinierter Speicherorte im Cluster

Führen Sie den folgenden Befehl aus, um dieses Feature in Ihrem Cluster zu aktivieren:

```console
az connectedk8s enable-features -n <clusterName> -g <resourceGroupName> --features cluster-connect custom-locations
```

> [!NOTE]
> 1. Das Feature „Benutzerdefinierte Speicherorte“ ist vom Cluster Connect-Feature abhängig. Daher müssen beide Features aktiviert werden, damit „Benutzerdefinierte Speicherorte“ funktioniert.
> 2. `az connectedk8s enable-features` muss auf einem Computer ausgeführt werden, auf dem die `kubeconfig`-Datei auf den Cluster verweist, auf dem die Features aktiviert werden sollen.

## <a name="create-custom-location"></a>Erstellen eines benutzerdefinierten Speicherorts

1. Erstellen eines Kubernetes-Clusters mit Azure Arc-Unterstützung.
    - Wenn Sie noch keine Verbindung mit einem Cluster hergestellt haben, verwenden Sie unseren [Schnellstart](quickstart-connect-cluster.md).
    - [Aktualisieren Sie Ihre Agents](agent-upgrade.md#manually-upgrade-agents) auf Version >= 1.1.0.

1. Stellen Sie die Clustererweiterung des Azure-Dienstanbieters bereit, dessen Instanz Sie schließlich zusätzlich zum benutzerdefinierten Speicherort benötigen:

    ```azurecli
    az k8s-extension create --name <extensionInstanceName> --extension-type microsoft.arcdataservices --cluster-type connectedClusters -c <clusterName> -g <resourceGroupName> --scope cluster --release-namespace arc --config Microsoft.CustomLocation.ServiceAccount=sa-bootstrapper
    ```

    > [!NOTE]
    > Der Proxy für ausgehenden Datenverkehr ohne Authentifizierung und der Proxy für ausgehenden Datenverkehr mit Standardauthentifizierung werden von der Data Services-Clustererweiterung mit Arc-Unterstützung aktiviert. Proxys für ausgehenden Datenverkehr, von denen Zertifikate erwartet werden, werden aktuell nicht unterstützt.

1. Rufen Sie den Azure Resource Manager-Bezeichner des Kubernetes-Clusters mit Azure Arc-Unterstützung ab, auf den in späteren Schritten als `connectedClusterId` verwiesen wird:

    ```azurecli
    az connectedk8s show -n <clusterName> -g <resourceGroupName>  --query id -o tsv
    ```

1. Rufen Sie den Azure Resource Manager-Bezeichner der auf dem Kubernetes-Clusters mit Azure Arc-Unterstützung bereitgestellten Clustererweiterung ab, auf den in späteren Schritten als `extensionId` verwiesen wird:

    ```azurecli
    az k8s-extension show --name <extensionInstanceName> --cluster-type connectedClusters -c <clusterName> -g <resourceGroupName>  --query id -o tsv
    ```

1. Erstellen Sie einen benutzerdefinierten Speicherort, indem Sie auf den Kubernetes-Clusters mit Azure Arc-Unterstützung und die Erweiterung verweisen:

    ```azurecli
    az customlocation create -n <customLocationName> -g <resourceGroupName> --namespace arc --host-resource-id <connectedClusterId> --cluster-extension-ids <extensionId>
    ```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> Sicheres Herstellen einer Verbindung mit dem Cluster über [Cluster Connect](cluster-connect.md)