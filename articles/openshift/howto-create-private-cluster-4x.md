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
ms.openlocfilehash: 581587382c3bfd03ed329672e5c6ca065554d1c7
ms.sourcegitcommit: 6fd8dbeee587fd7633571dfea46424f3c7e65169
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/21/2020
ms.locfileid: "83727438"
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

### <a name="get-a-red-hat-pull-secret-optional"></a>Abrufen eines Red Hat-Pullgeheimnisses (optional)

Das Red Hat-Pullgeheimnis ermöglicht dem Cluster den Zugriff auf Red Hat-Containerregistrierungen zusammen mit zusätzlichem Inhalt. Dieser Schritt ist optional, wird aber empfohlen.

1. **[Navigieren Sie zum Manager-Portal für Ihren Red Hat OpenShift-Cluster](https://cloud.redhat.com/openshift/install/azure/aro-provisioned), und melden Sie sich an.**

   Sie müssen sich bei Ihrem Red Hat-Konto anmelden oder ein neues Red Hat-Konto mit Ihrer Geschäfts-E-Mail-Adresse erstellen und den Geschäftsbedingungen zustimmen.

2. **Klicken Sie auf „Download pull secret“ (Pullgeheimnis herunterladen).**

Speichern Sie die Datei `pull-secret.txt` an einem sicheren Ort. Sie wird bei jeder Clustererstellung verwendet.

Wenn Sie den Befehl `az aro create` ausführen, können Sie mit dem Parameter `--pull-secret @pull-secret.txt` auf Ihr Pullgeheimnis verweisen. Führen Sie `az aro create` von dem Verzeichnis aus, in dem Sie Ihre `pull-secret.txt`-Datei gespeichert haben. Ersetzen Sie andernfalls `@pull-secret.txt` durch `@<path-to-my-pull-secret-file`.

Wenn Sie Ihr Pullgeheimnis kopieren oder es in anderen Skripts referenzieren, sollte Ihr Pullgeheimnis als gültige JSON-Zeichenfolge formatiert werden.

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

Führen Sie den folgenden Befehl aus, um einen Cluster zu erstellen. Optional können Sie Ihr [Red Hat-Pullgeheimnis übergeben](#get-a-red-hat-pull-secret-optional), das dem Cluster den Zugriff auf Red Hat-Containerregistrierungen und zusätzliche Inhalte ermöglicht.

>[!NOTE]
> Wenn Sie Befehle kopieren/einfügen und einen der optionalen Parameter verwenden, stellen Sie sicher, dass Sie die anfänglichen Hashtags und den abschließenden Kommentartext löschen. Schließen Sie auch das Argument auf der vorhergehenden Zeile des Befehls mit einem abschließenden umgekehrten Schrägstrich ab.

```azurecli-interactive
az aro create \
  --resource-group $RESOURCEGROUP \
  --name $CLUSTER \
  --vnet aro-vnet \
  --master-subnet master-subnet \
  --worker-subnet worker-subnet \
  --apiserver-visibility Private \
  --ingress-visibility Private
  # --domain foo.example.com # [OPTIONAL] custom domain
  # --pull-secret @pull-secret.txt # [OPTIONAL]
```

Nach der Ausführung des Befehls `az aro create` dauert es normalerweise etwa 35 Minuten, um einen Cluster zu erstellen.

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

Wenn Sie bei der OpenShift-Webkonsole angemeldet sind, klicken Sie auf das Fragezeichen ( **?** ) im oberen rechten Bereich und dann auf **Befehlszeilentools**. Laden Sie die für Ihren Computer geeignete Version herunter.

![Azure Red Hat OpenShift-Anmeldebildschirm](media/aro4-download-cli.png)

Sie können auch unter <https://mirror.openshift.com/pub/openshift-v4/clients/ocp/latest/> die aktuelle Version der CLI herunterladen, die für Ihren Computer geeignet ist.

## <a name="connect-using-the-openshift-cli"></a>Herstellen einer Verbindung mithilfe der OpenShift-CLI

Rufen Sie die Adresse des API-Servers ab.

```azurecli-interactive
apiServer=$(az aro show -g $RESOURCEGROUP -n $CLUSTER --query apiserverProfile.url -o tsv)
```

>[!IMPORTANT]
> Zum Herstellen einer Verbindung mit einem Azure Red Hat OpenShift-Cluster müssen Sie den folgenden Schritt über einen Host ausführen, der sich entweder in dem von Ihnen erstellten virtuellen Netzwerk oder in einem virtuellen Netzwerk befindet, das mit dem virtuellen Netzwerk, für das der Cluster bereitgestellt wurde, per [Peering](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) verknüpft ist.

Melden Sie sich mit dem folgenden Befehl beim API-Server des OpenShift-Clusters an. Ersetzen Sie **\<kubeadmin password>** durch das Kennwort, das Sie gerade abgerufen haben:

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
