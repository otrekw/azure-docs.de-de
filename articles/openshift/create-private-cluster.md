---
title: Erstellen eines privaten Azure Red Hat OpenShift 4-Clusters
description: Informationen zum Erstellen eines privaten Azure Red Hat OpenShift Clusters mit OpenShift 4
ms.service: container-service
ms.topic: article
ms.date: 03/12/2020
author: ms-jasondel
ms.author: jasondel
keywords: aro, openshift, az aro, red hat, cli
ms.custom: mvc
ms.openlocfilehash: a0f726d32f2f63cf85101254fded005fc0b5a1db
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "82233549"
---
# <a name="create-an-azure-red-hat-openshift-4-private-cluster"></a>Erstellen eines privaten Azure Red Hat OpenShift 4-Clusters

In diesem Artikel erfahren Sie, wie Sie Ihre Umgebung für das Erstellen von privaten Azure Red Hat OpenShift-Clustern mit OpenShift 4 vorbereiten. Sie lernen Folgendes:

> [!div class="checklist"]
> * Einrichten der erforderlichen Komponenten sowie Erstellen des erforderlichen virtuellen Netzwerks und der Subnetze
> * Bereitstellen eines Clusters mit einem privaten Endpunkt für den API-Server und einem privaten Eingangscontroller

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für dieses Tutorial die Version 2.0.75 oder höher der Azure CLI ausführen. Führen Sie `az --version` aus, um die Version zu ermitteln. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sie bei Bedarf unter [Installieren der Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="before-you-begin"></a>Voraussetzungen

### <a name="install-the-az-aro-extension"></a>Installieren der Erweiterung „az aro“
Mit der `az aro`-Erweiterung können Sie Azure Red Hat OpenShift-Cluster mithilfe der Azure CLI direkt über die Befehlszeile erstellen, darauf zugreifen und löschen.

Führen Sie zum Installieren der Erweiterung `az aro` den folgenden Befehl aus:

```azurecli-interactive
az extension add -n aro --index https://az.aroapp.io/stable
```

Wenn Sie die Erweiterung bereits installiert haben, können Sie zum Aktualisieren den folgenden Befehl ausführen:

```azurecli-interactive
az extension update -n aro --index https://az.aroapp.io/stable
```

### <a name="register-the-resource-provider"></a>Registrieren des Ressourcenanbieters

Als Nächstes müssen Sie den Ressourcenanbieter `Microsoft.RedHatOpenShift` in Ihrem Abonnement registrieren.

```azurecli-interactive
az provider register -n Microsoft.RedHatOpenShift --wait
```

Vergewissern Sie sich, dass die Erweiterung registriert ist.

```azurecli-interactive
az -v
```

  Die Ausgabe sollte in etwa wie folgt aussehen:

```output
...
Extensions:
aro                                1.0.0
...
```

### <a name="obtain-a-red-hat-pull-secret-optional"></a>Abrufen eines Red Hat-Pullgeheimnisses (optional)

Ein Red Hat-Pullgeheimnis ermöglicht dem Cluster den Zugriff auf Red Hat-Containerregistrierungen und zusätzliche Inhalten. Dieser Schritt ist optional, wird aber empfohlen.

Sie können auf Ihr Pullgeheimnis zugreifen, indem Sie zu https://cloud.redhat.com/openshift/install/azure/aro-provisioned navigieren und auf *Download pull secret* (Pullgeheimnis herunterladen) klicken.

Sie müssen sich bei Ihrem Red Hat-Konto anmelden oder ein neues Red Hat-Konto mit Ihrer Geschäfts-E-Mail-Adresse erstellen und den Geschäftsbedingungen zustimmen.

Speichern Sie die Datei `pull-secret.txt` an einem sicheren Ort. Sie wird bei jeder Clustererstellung verwendet.

### <a name="create-a-virtual-network-containing-two-empty-subnets"></a>Erstellen eines virtuellen Netzwerks mit zwei leeren Subnetzen

Als Nächstes erfahren Sie, wie Sie ein virtuelles Netzwerk mit zwei leeren Subnetzen erstellen.

1. **Legen Sie die folgenden Variablen fest:**

   ```console
   LOCATION=eastus                 # the location of your cluster
   RESOURCEGROUP="v4-$LOCATION"    # the name of the resource group where you want to create your cluster
   CLUSTER=aro-cluster             # the name of your cluster
   ```

1. **Erstellen einer Ressourcengruppe**

    Eine Azure-Ressourcengruppe ist eine logische Gruppe, in der Azure-Ressourcen bereitgestellt und verwaltet werden. Wenn Sie eine Ressourcengruppe erstellen, müssen Sie einen Speicherort angeben. An diesem Speicherort werden die Metadaten der Ressourcengruppe gespeichert. Darüber hinaus werden dort die Ressourcen in Azure ausgeführt, wenn Sie während der Ressourcenerstellung keine andere Region angeben. Erstellen Sie mit dem Befehl [az group create][az-group-create] eine Ressourcengruppe.

    ```azurecli-interactive
    az group create --name $RESOURCEGROUP --location $LOCATION
    ```

    Die folgende Beispielausgabe zeigt, dass die Ressourcengruppe erfolgreich erstellt wurde:

    ```json
    {
    "id": "/subscriptions/<guid>/resourceGroups/aro-rg",
    "location": "eastus",
    "managedBy": null,
    "name": "aro-rg",
    "properties": {
        "provisioningState": "Succeeded"
    },
    "tags": null
    }
    ```

2. **Erstellen Sie ein virtuelles Netzwerk.**

    Für Azure Red Hat OpenShift-Cluster mit OpenShift 4 ist ein virtuelles Netzwerk mit zwei leeren Subnetzen für die Master- und Workerknoten erforderlich.

    Erstellen Sie ein neues virtuelles Netzwerk in derselben Ressourcengruppe, die Sie zuvor erstellt haben.

    ```azurecli-interactive
    az network vnet create \
    --resource-group $RESOURCEGROUP \
    --name aro-vnet \
    --address-prefixes 10.0.0.0/22
    ```

    Die folgende Beispielausgabe zeigt, dass das virtuelle Netzwerk erfolgreich erstellt wurde:

    ```json
    {
    "newVNet": {
        "addressSpace": {
        "addressPrefixes": [
            "10.0.0.0/22"
        ]
        },
        "id": "/subscriptions/<guid>/resourceGroups/aro-rg/providers/Microsoft.Network/virtualNetworks/aro-vnet",
        "location": "eastus",
        "name": "aro-vnet",
        "provisioningState": "Succeeded",
        "resourceGroup": "aro-rg",
        "type": "Microsoft.Network/virtualNetworks"
    }
    }
    ```

3. **Fügen Sie ein leeres Subnetz für die Masterknoten hinzu.**

    ```azurecli-interactive
    az network vnet subnet create \
    --resource-group $RESOURCEGROUP \
    --vnet-name aro-vnet \
    --name master-subnet \
    --address-prefixes 10.0.0.0/23 \
    --service-endpoints Microsoft.ContainerRegistry
    ```

4. **Fügen Sie ein leeres Subnetz für die Workerknoten hinzu.**

    ```azurecli-interactive
    az network vnet subnet create \
    --resource-group $RESOURCEGROUP \
    --vnet-name aro-vnet \
    --name worker-subnet \
    --address-prefixes 10.0.2.0/23 \
    --service-endpoints Microsoft.ContainerRegistry
    ```

5. **[Deaktivieren Sie Richtlinien für private Endpunkte](https://docs.microsoft.com/azure/private-link/disable-private-link-service-network-policy) im Mastersubnetz.** Dies ist erforderlich, um eine Verbindung mit dem Cluster herzustellen und um diesen zu verwalten.

    ```azurecli-interactive
    az network vnet subnet update \
    --name master-subnet \
    --resource-group $RESOURCEGROUP \
    --vnet-name aro-vnet \
    --disable-private-link-service-network-policies true
    ```

## <a name="create-the-cluster"></a>Erstellen Sie den Cluster.

Führen Sie den folgenden Befehl aus, um einen Cluster zu erstellen. Beachten Sie die Parameter`apiserver-visibility` und `ingress-visibility`. Optional können Sie ein Pullgeheimnis übergeben, das dem Cluster den Zugriff auf Red Hat-Containerregistrierungen und zusätzliche Inhalte ermöglicht. Sie können Sie auf Ihr Pullgeheimnis zugreifen, indem Sie zum [Cluster-Manager für Red Hat OpenShift](https://cloud.redhat.com/openshift/install/azure/installer-provisioned) navigieren und auf „Copy Pull Secret“ (Pullgeheimnis kopieren) klicken.

```azurecli-interactive
az aro create \
  --resource-group $RESOURCEGROUP \
  --name $CLUSTER \
  --vnet aro-vnet \
  --master-subnet master-subnet \
  --worker-subnet worker-subnet \
  --apiserver-visibility Private \
  --ingress-visibility Private
  # --domain aro.example.com # [OPTIONAL] custom domain
  # --pull-secret 'Pull secret from https://cloud.redhat.com/openshift/install/azure/installer-provisioned/' # [OPTIONAL]
```

>[!NOTE]
> Das Erstellen eines Clusters dauert in der Regel ca. 35 Minuten.

>[!IMPORTANT]
> Wenn Sie eine benutzerdefinierte Domäne angeben möchten (etwa **foo.example.com**), steht die OpenShift-Konsole unter einer URL wie `https://console-openshift-console.apps.foo.example.com` anstatt über die integrierte Domäne `https://console-openshift-console.apps.<random>.<location>.aroapp.io` zur Verfügung.
>
> OpenShift verwendet standardmäßig selbstsignierte Zertifikate für alle für `*.apps.<random>.<location>.aroapp.io` erstellten Routen.  Wenn Sie nach dem Herstellen einer Verbindung mit dem Cluster ein benutzerdefiniertes DNS verwenden möchten, müssen Sie anhand der OpenShift-Dokumentation [eine benutzerdefinierte Zertifizierungsstelle für Ihren Eingangscontroller](https://docs.openshift.com/container-platform/4.3/authentication/certificates/replacing-default-ingress-certificate.html) und eine [benutzerdefinierte Zertifizierungsstelle für Ihren API-Server](https://docs.openshift.com/container-platform/4.3/authentication/certificates/api-server.html) konfigurieren.

## <a name="connect-to-the-private-cluster"></a>Herstellen einer Verbindung mit dem privaten Cluster

Sie können sich mit dem Benutzer `kubeadmin` beim Cluster anmelden.  Führen Sie den folgenden Befehl aus, um das Kennwort für den Benutzer `kubeadmin` zu suchen:

```azurecli-interactive
az aro list-credentials \
  --name $CLUSTER \
  --resource-group $RESOURCEGROUP
```

Die folgende Beispielausgabe zeigt, dass sich das Kennwort in `kubeadminPassword` befindet.

```json
{
  "kubeadminPassword": "<generated password>",
  "kubeadminUsername": "kubeadmin"
}
```

Sie können die URL der Clusterkonsole über den folgenden Befehl abrufen. Diese sieht in etwa wie folgt aus: `https://console-openshift-console.apps.<random>.<region>.aroapp.io/`.

```azurecli-interactive
 az aro show \
    --name $CLUSTER \
    --resource-group $RESOURCEGROUP \
    --query "consoleProfile.url" -o tsv
```

>[!IMPORTANT]
> Zum Herstellen einer Verbindung mit einem Azure Red Hat OpenShift-Cluster müssen Sie den folgenden Schritt über einen Host ausführen, der sich entweder in dem von Ihnen erstellten virtuellen Netzwerk oder in einem virtuellen Netzwerk befindet, das mit dem virtuellen Netzwerk, für das der Cluster bereitgestellt wurde, per [Peering](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) verknüpft ist.

Starten Sie die Konsolen-URL in einem Browser, und melden Sie sich mit den `kubeadmin`-Anmeldeinformationen an.

![Azure Red Hat OpenShift-Anmeldebildschirm](media/aro4-login.png)

## <a name="install-the-openshift-cli"></a>Installieren der OpenShift-CLI

Wenn Sie bei der OpenShift-Webkonsole angemeldet sind, klicken Sie erst auf das Fragezeichen ( **?** ) im oberen rechten Bereich und dann auf **Befehlszeilentools**. Laden Sie die für Ihren Computer geeignete Version herunter.

![Azure Red Hat OpenShift-Anmeldebildschirm](media/aro4-download-cli.png)

Sie können auch unter <https://mirror.openshift.com/pub/openshift-v4/clients/ocp/latest/> die aktuelle Version der CLI herunterladen, die für Ihren Computer geeignet ist.

## <a name="connect-using-the-openshift-cli"></a>Herstellen einer Verbindung mithilfe der OpenShift-CLI

Rufen Sie die Adresse des API-Servers ab.

```azurecli-interactive
apiServer=$(az aro show -g $RESOURCEGROUP -n $CLUSTER --query apiserverProfile.url -o tsv)
```

>[!IMPORTANT]
> Zum Herstellen einer Verbindung mit einem Azure Red Hat OpenShift-Cluster müssen Sie den folgenden Schritt über einen Host ausführen, der sich entweder in dem von Ihnen erstellten virtuellen Netzwerk oder in einem virtuellen Netzwerk befindet, das mit dem virtuellen Netzwerk, für das der Cluster bereitgestellt wurde, per [Peering](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) verknüpft ist.

Melden Sie sich mit dem folgenden Befehl beim API-Server des OpenShift-Clusters an. Ersetzen Sie **\<kubeadmin password>** durch das Kennwort, das Sie gerade abgerufen haben.

```azurecli-interactive
oc login $apiServer -u kubeadmin -p <kubeadmin password>
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel wurde erläutert, wie Sie ein Azure Red Hat OpenShift-Cluster mit OpenShift 4 bereitstellen können. Sie haben Folgendes gelernt:

> [!div class="checklist"]
> * Einrichten der erforderlichen Komponenten sowie Erstellen des erforderlichen virtuellen Netzwerks und der Subnetze
> * Bereitstellen eines Clusters
> * Herstellen einer Verbindung mit dem Cluster mithilfe des `kubeadmin`-Benutzers

Im nächsten Artikel erfahren Sie, wie Sie den Cluster für die Authentifizierung mithilfe von Azure Active Directory konfigurieren können.


* [Konfigurieren der Authentifizierung mit Azure Active Directory über die Befehlszeile](configure-azure-ad-cli.md)


* [Konfigurieren der Authentifizierung mit Azure Active Directory über das Azure-Portal und die OpenShift-Webkonsole](configure-azure-ad-cli.md)
