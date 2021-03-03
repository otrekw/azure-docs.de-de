---
title: Erstellen eines privaten Clusters mit Azure Red Hat OpenShift 3.11 | Microsoft-Dokumentation
description: Erstellen eines privaten Clusters mit Azure Red Hat OpenShift 3.11
author: sakthi-vetrivel
ms.author: suvetriv
ms.service: azure-redhat-openshift
ms.topic: conceptual
ms.date: 03/02/2020
keywords: aro, openshift, privater cluster, red hat
ms.openlocfilehash: 28a21635da770a990a21e0da2a8034c754f4f7aa
ms.sourcegitcommit: 58ff80474cd8b3b30b0e29be78b8bf559ab0caa1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/17/2021
ms.locfileid: "100633239"
---
# <a name="create-a-private-cluster-with-azure-red-hat-openshift-311"></a>Erstellen eines privaten Clusters mit Azure Red Hat OpenShift 3.11

> [!IMPORTANT]
> Azure Red Hat OpenShift 3.11 wird zum 30. Juni 2022 eingestellt. Unterstützung für die Erstellung neuer Azure Red Hat OpenShift 3.11-Cluster wird bis zum 30. November 2020 bereitgestellt. Nach der Einstellung werden die verbleibenden Azure Red Hat OpenShift 3.11-Cluster abgeschaltet, um Sicherheitsrisiken zu vermeiden.
> 
> Führen Sie die Schritte in diesem Leitfaden aus, um [einen Azure Red Hat OpenShift 4-Cluster zu erstellen](tutorial-create-cluster.md).
> Wenn Sie spezielle Fragen haben, [kontaktieren Sie uns](mailto:arofeedback@microsoft.com).

Private Cluster bieten folgende Vorteile:

* Private Cluster machen Komponenten der Clustersteuerungsebene (z. B. die API-Server) nicht für eine öffentliche IP-Adresse verfügbar.
* Das virtuelle Netzwerk eines privaten Clusters kann von Kunden konfiguriert werden, sodass der Netzwerkbetrieb für ein Peering mit anderen virtuellen Netzwerken, einschließlich ExpressRoute-Umgebungen, eingerichtet werden kann. Sie können auch benutzerdefiniertes DNS im virtuellen Netzwerk konfigurieren, um es in interne Dienste zu integrieren.

## <a name="before-you-begin"></a>Voraussetzungen

Die Felder im folgenden Codeausschnitt für die Konfiguration sind neu und müssen in Ihrer Clusterkonfiguration enthalten sein. `managementSubnetCidr` muss sich im virtuellen Netzwerk des Clusters befinden und wird von Azure zum Verwalten des Clusters verwendet.

```json
properties:
 networkProfile:
   managementSubnetCidr: 10.0.1.0/24
 masterPoolProfile:
   apiProperties:
     privateApiServer: true
```

Ein privater Cluster kann mithilfe der unten aufgeführten Beispielskripts bereitgestellt werden. Nachdem der Cluster bereitgestellt wurde, führen Sie den `cluster get`-Befehl aus, und zeigen Sie die Eigenschaft `properties.FQDN` an, um die private IP-Adresse des OpenShift-API-Servers zu ermitteln.

Das virtuelle Netzwerk des Clusters wird mit entsprechenden Berechtigungen zum Ändern erstellt. Anschließend können Sie den Netzwerkbetrieb für den Zugriff auf das virtuelle Netzwerk (ExpressRoute, VPN, Peering virtueller Netzwerke) entsprechend Ihren Anforderungen einrichten.

Wenn Sie die DNS-Namenserver im virtuellen Netzwerk des Clusters ändern, müssen Sie ein Update für den Cluster ausgeben, bei dem die Eigenschaft `properties.RefreshCluster` auf `true` festgelegt ist, damit ein Reimaging der VMs durchgeführt werden kann. Mit diesem Update können die neuen Namenserver übernommen werden.

## <a name="sample-configuration-scripts"></a>Konfigurationsbeispielskripts

Verwenden Sie die Beispielskripts in diesem Abschnitt zum Einrichten und Bereitstellen des privaten Clusters.

### <a name="environment"></a>Environment

Füllen Sie die Umgebungsvariablen unten mit Ihren eigenen Werten aus.

> [!NOTE]
> Der Standort muss auf `eastus2` festgelegt werden, da dies derzeit der einzige unterstützte Standort für private Cluster ist.

``` bash
export CLUSTER_NAME=
export LOCATION=eastus2
export TOKEN=$(az account get-access-token --query 'accessToken' -o tsv)
export SUBID=
export TENANT_ID=
export ADMIN_GROUP=
export CLIENT_ID=
export SECRET=
```

### <a name="private-clusterjson"></a>private-cluster.json

Hier sehen Sie ein Beispiel für eine Clusterkonfiguration mit aktiviertem privatem Cluster unter Verwendung der oben definierten Umgebungsvariablen.

```json
{
   "location": "$LOCATION",
   "name": "$CLUSTER_NAME",
   "properties": {
       "openShiftVersion": "v3.11",
       "networkProfile": {
           "vnetCIDR": "10.0.0.0/8",
           "managementSubnetCIDR" : "10.0.1.0/24"
       },
       "authProfile": {
           "identityProviders": [
               {
                   "name": "Azure AD",
                   "provider": {
                       "kind": "AADIdentityProvider",
                       "clientId": "$CLIENT_ID",
                       "secret": "$SECRET",
                       "tenantId": "$TENANT_ID",
                       "customerAdminGroupID": "$ADMIN_GROUP"
                   }
               }
           ]
       },
       "masterPoolProfile": {
           "name": "master",
           "count": 3,
           "vmSize": "Standard_D4s_v3",
           "osType": "Linux",
           "subnetCIDR": "10.0.0.0/24",
           "apiProperties": {
                "privateApiServer": true
            }
       },
       "agentPoolProfiles": [
           {
               "role": "compute",
               "name": "compute",
               "count": 1,
               "vmSize": "Standard_D4s_v3",
               "osType": "Linux",
               "subnetCIDR": "10.0.0.0/24"
           },
           {
               "role": "infra",
               "name": "infra",
               "count": 3,
               "vmSize": "Standard_D4s_v3",
               "osType": "Linux",
               "subnetCIDR": "10.0.0.0/24"
           }
       ],
       "routerProfiles": [
           {
               "name": "default"
           }
       ]
   }
}
```

## <a name="deploy-a-private-cluster"></a>Bereitstellen eines privaten Clusters

Nachdem Sie den privaten Cluster mit den oben aufgeführten Beispielskripts konfiguriert haben, führen Sie den folgenden Befehl aus, um den privaten Cluster bereitzustellen.

``` bash
az group create --name $CLUSTER_NAME --location $LOCATION
cat private-cluster.json | envsubst | curl -v -X PUT \
-H 'Content-Type: application/json; charset=utf-8' \
-H 'Authorization: Bearer '$TOKEN'' -d @- \
 https://management.azure.com/subscriptions/$SUBID/resourceGroups/$CLUSTER_NAME/providers/Microsoft.ContainerService/openShiftManagedClusters/$CLUSTER_NAME?api-version=2019-10-27-preview
```

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Zugriff auf die OpenShift-Konsole finden Sie in der [exemplarischen Vorgehensweise für die Webkonsole](https://docs.openshift.com/container-platform/3.11/getting_started/developers_console.html).
