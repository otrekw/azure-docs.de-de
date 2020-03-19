---
title: Erstellen eines Azure Red Hat OpenShift 4.3-Clusters | Microsoft-Dokumentation
description: Erstellen eines Clusters mit Azure Red Hat OpenShift 3.11
author: lamek
ms.author: suvetriv
ms.service: container-service
ms.topic: conceptual
ms.date: 03/06/2020
keywords: aro, openshift, az aro, red hat, cli
ms.openlocfilehash: 3c336a1fbfb9f991ff824e8deafe84f3d899771d
ms.sourcegitcommit: 72c2da0def8aa7ebe0691612a89bb70cd0c5a436
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "79082828"
---
# <a name="create-access-and-manage-an-azure-red-hat-openshift-43-cluster"></a>Erstellen, Zugreifen auf und Verwalten eines Azure Red Hat OpenShift 4.3-Clusters

> [!IMPORTANT]
> Beachten Sie, dass Azure Red Hat OpenShift 4.3 derzeit nur in der privaten Vorschau in der Region „USA, Osten“ verfügbar ist. Die Annahme der privaten Vorschau ist nur auf Einladung möglich. Stellen Sie sicher, dass Sie Ihr Abonnement registrieren, bevor Sie versuchen, dieses Feature zu aktivieren: [Registrierung für die private Vorschau von Azure Red Hat OpenShift](https://aka.ms/aro-preview-register)

> [!NOTE]
> Vorschaufunktionen stehen gemäß dem Self-Service-Prinzip zur Verfügung, werden wie besehen und wie verfügbar bereitgestellt und sind von den Vereinbarungen zum Service Level (SLA) und der eingeschränkten Garantie ausgeschlossen. Daher sind die Funktionen nicht für den Einsatz in der Produktion vorgesehen.

## <a name="prerequisites"></a>Voraussetzungen

Zum Erstellen eines Azure Red Hat OpenShift 4.3-Clusters benötigen Sie Folgendes:

- Azure CLI, Version 2.0.72 oder höher
  
- Erweiterung „az aro“

- Ein virtuelles Netzwerk mit zwei leeren Subnetzen, denen jeweils keine Netzwerksicherheitsgruppe angefügt ist.  Ihr Cluster wird in diesen Subnetzen bereitgestellt.

- Eine Cluster-AAD-Anwendung (Client-ID und Geheimnis) und einen Dienstprinzipal oder ausreichende AAD-Berechtigungen für `az aro create`, um eine AAD-Anwendung und einen Dienstprinzipal automatisch zu erstellen.

- Der RP-Dienstprinzipal und der Clusterdienstprinzipal müssen jeweils über die Rolle „Mitwirkender“ im virtuellen Netzwerk des Clusters verfügen.  Wenn Sie über die Rolle „Benutzerzugriffsadministrator“ im virtuellen Netzwerk verfügen, werden die Rollenzuweisungen von `az aro create` automatisch eingerichtet.

### <a name="install-the-az-aro-extension"></a>Installieren der Erweiterung „az aro“
Mit der `az aro`-Erweiterung können Sie Azure Red Hat OpenShift-Cluster mithilfe der Azure CLI direkt über die Befehlszeile erstellen, darauf zugreifen und löschen.

> [!Note] 
> Die `az aro`-Erweiterung befindet sich derzeit in der Vorschauphase. Sie kann in einem zukünftigen Release geändert oder entfernt werden.
> Um die Vorschauversion der `az aro`-Erweiterung zu abonnieren, müssen Sie den Ressourcenanbieter `Microsoft.RedHatOpenShift` registrieren.
> 
>    ```console
>    az provider register -n Microsoft.RedHatOpenShift --wait
>    ```

1. Melden Sie sich bei Azure an.

   ```console
   az login
   ```

2. Führen Sie zum Installieren der `az aro`-Erweiterung den folgenden Befehl aus:

   ```console
   az extension add --source https://arosvc.blob.core.windows.net/az-preview/aro-0.1.0-py2.py3-none-any.whl
   ```

3. Vergewissern Sie sich, dass die ARO-Erweiterung registriert ist.

   ```console
   az -v
   ...
   Extensions:
   aro                                0.1.0
   ...
   ```
  
### <a name="create-a-virtual-network-containing-two-empty-subnets"></a>Erstellen eines virtuellen Netzwerks mit zwei leeren Subnetzen

Führen Sie die folgenden Schritte aus, um ein virtuelles Netzwerk mit zwei leeren Subnetzen zu erstellen.

1. Legen Sie die folgenden Variablen fest.

   ```console
   LOCATION=eastus        #the location of your cluster
   RESOURCEGROUP="v4-$LOCATION"    #the name of the resource group where you want to create your cluster
   CLUSTER=cluster        #the name of your cluster
   ```

2. Erstellen Sie eine Ressourcengruppe für Ihren Cluster.

   ```console
   az group create -g "$RESOURCEGROUP" -l $LOCATION
   ```

3. Erstellen Sie das virtuelle Netzwerk.

   ```console
   az network vnet create \
     -g "$RESOURCEGROUP" \
     -n vnet \
     --address-prefixes 10.0.0.0/9 \
     >/dev/null
   ```

4. Fügen Sie dem virtuellen Netzwerk zwei leere Subnetze hinzu.

   ```console
    for subnet in "$CLUSTER-master" "$CLUSTER-worker"; do
     az network vnet subnet create \
       -g "$RESOURCEGROUP" \
       --vnet-name vnet \
       -n "$subnet" \
       --address-prefixes 10.$((RANDOM & 127)).$((RANDOM & 255)).0/24 \
       --service-endpoints Microsoft.ContainerRegistry \
       >/dev/null
   done
   ```

5. Deaktivieren Sie Netzwerkrichtlinien für den Private Link-Dienst in Ihrem virtuellen Netzwerk und in den Subnetzen. Dies ist erforderlich, damit der ARO-Dienst auf den Cluster zugreifen und ihn verwalten kann.

   ```console
   az network vnet subnet update \
     -g "$RESOURCEGROUP" \
     --vnet-name vnet \
     -n "$CLUSTER-master" \
     --disable-private-link-service-network-policies true \
     >/dev/null
   ```

## <a name="create-a-cluster"></a>Erstellen eines Clusters

Führen Sie den folgenden Befehl aus, um einen Cluster zu erstellen.

```console
az aro create \
  -g "$RESOURCEGROUP" \
  -n "$CLUSTER" \
  --vnet vnet \
  --master-subnet "$CLUSTER-master" \
  --worker-subnet "$CLUSTER-worker"
```

>[!NOTE]
> Das Erstellen eines Clusters dauert in der Regel ca. 35 Minuten.

## <a name="access-the-cluster-console"></a>Zugreifen auf die Clusterkonsole

Die URL der Clusterkonsole (in der Form `https://console-openshift-console.apps.<random>.<location>.aroapp.io/`) finden Sie unter der Azure Red Hat OpenShift 4.3-Clusterressource. Führen Sie zum Anzeigen der Ressource den folgenden Befehl aus:

```console
az aro list -o table
```

Sie können sich mit dem Benutzer `kubeadmin` beim Cluster anmelden.  Führen Sie den folgenden Befehl aus, um das Kennwort für den Benutzer `kubeadmin` zu suchen:

```dotnetcli
az aro list-credentials -g "$RESOURCEGROUP" -n "$CLUSTER"
```

## <a name="delete-a-cluster"></a>Löschen eines Clusters

Führen Sie zum Löschen eines Clusters den folgenden Befehl aus.

```console
az aro delete -g "$RESOURCEGROUP" -n "$CLUSTER"

# (optional)
for subnet in "$CLUSTER-master" "$CLUSTER-worker"; do
  az network vnet subnet delete -g "$RESOURCEGROUP" --vnet-name vnet -n "$subnet"
done
```