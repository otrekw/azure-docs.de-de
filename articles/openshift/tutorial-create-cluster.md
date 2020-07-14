---
title: 'Tutorial: Erstellen eines Azure Red Hat OpenShift 4-Clusters'
description: Erfahren Sie, wie Sie einen Microsoft Azure Red Hat OpenShift-Cluster mithilfe der Azure-Befehlszeilenschnittstelle erstellen
author: sakthi-vetrivel
ms.author: suvetriv
ms.topic: tutorial
ms.service: container-service
ms.date: 04/24/2020
ms.openlocfilehash: 61b6ad0bedb4817c262b4269a6e9f6930a6caa6c
ms.sourcegitcommit: 93462ccb4dd178ec81115f50455fbad2fa1d79ce
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/06/2020
ms.locfileid: "85985687"
---
# <a name="tutorial-create-an-azure-red-hat-openshift-4-cluster"></a>Tutorial: Erstellen eines Azure Red Hat OpenShift 4-Clusters

In diesem Tutorial (Teil 1 von 3) bereiten Sie Ihre Umgebung auf die Erstellung eines Azure Red Hat OpenShift-Clusters mit OpenShift 4 vor und erstellen einen Cluster. Sie lernen Folgendes:
> [!div class="checklist"]
> * Richten Sie die erforderlichen Komponenten ein, und erstellen Sie das erforderliche virtuelle Netzwerk und die Subnetze.
> * Bereitstellen eines Clusters

## <a name="before-you-begin"></a>Voraussetzungen

Wenn Sie die Befehlszeilenschnittstelle lokal installieren und verwenden möchten, müssen Sie für dieses Tutorial mindestens die Azure CLI-Version 2.0.75 ausführen. Führen Sie `az --version` aus, um die Version zu ermitteln. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sie bei Bedarf unter [Installieren der Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

### <a name="verify-your-permissions"></a>Überprüfen von Berechtigungen

Überprüfen Sie für die Erstellung eines Azure Red Hat OpenShift-Clusters die folgenden Berechtigungen für Ihr Azure-Abonnement, den Azure Active Directory-Benutzer oder den Dienstprinzipal:

|Berechtigungen|Ressourcengruppe, die das VNet enthält|Benutzer führt `az aro create` aus|Dienstprinzipal wurde als `–client-id` übergeben|
|----|:----:|:----:|:----:|
|**Benutzerzugriffsadministrator**|X|X| |
|**Mitwirkender**|X|X|X|

### <a name="install-the-az-aro-extension"></a>Installieren Sie die `az aro`-Erweiterung.
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

1. **[Navigieren Sie zu Ihrem Manager-Portal des Red Hat OpenShift Clusters](https://cloud.redhat.com/openshift/install/azure/aro-provisioned), und melden Sie sich an.**

   Sie müssen sich bei Ihrem Red Hat-Konto anmelden oder ein neues Red Hat-Konto mit Ihrer Geschäfts-E-Mail-Adresse erstellen und den Geschäftsbedingungen zustimmen.

2. **Klicken Sie auf „Download pull secret“ (Pullgeheimnis herunterladen).**

Speichern Sie die Datei `pull-secret.txt` an einem sicheren Ort. Sie wird bei jeder Clustererstellung verwendet.

Wenn Sie den Befehl `az aro create` ausführen, können Sie mit dem Parameter `--pull-secret @pull-secret.txt` auf Ihr Pullgeheimnis verweisen. Führen Sie `az aro create` von dem Verzeichnis aus, in dem Sie Ihre `pull-secret.txt`-Datei gespeichert haben. Ersetzen Sie andernfalls `@pull-secret.txt` durch `@<path-to-my-pull-secret-file>`.

Wenn Sie Ihr Pullgeheimnis kopieren oder es in anderen Skripts referenzieren, sollte Ihr Pullgeheimnis als gültige JSON-Zeichenfolge formatiert werden.

### <a name="create-a-virtual-network-containing-two-empty-subnets"></a>Erstellen eines virtuellen Netzwerks mit zwei leeren Subnetzen

Als Nächstes erfahren Sie, wie Sie ein virtuelles Netzwerk mit zwei leeren Subnetzen erstellen.

1. **Legen Sie die folgenden Variablen fest:**

   ```console
   LOCATION=eastus                 # the location of your cluster
   RESOURCEGROUP=aro-rg            # the name of the resource group where you want to create your cluster
   CLUSTER=cluster                 # the name of your cluster
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
  --worker-subnet worker-subnet
  # --domain foo.example.com # [OPTIONAL] custom domain
  # --pull-secret @pull-secret.txt # [OPTIONAL]
```

Nach der Ausführung des Befehls `az aro create` dauert es normalerweise etwa 35 Minuten, um einen Cluster zu erstellen.

>[!IMPORTANT]
> Wenn Sie eine benutzerdefinierte Domäne angeben möchten (etwa **foo.example.com**), steht die OpenShift-Konsole unter einer URL wie `https://console-openshift-console.apps.foo.example.com` anstatt über die integrierte Domäne `https://console-openshift-console.apps.<random>.<location>.aroapp.io` zur Verfügung.
>
> OpenShift verwendet standardmäßig selbstsignierte Zertifikate für alle für `*.apps.<random>.<location>.aroapp.io` erstellten Routen.  Wenn Sie nach dem Herstellen einer Verbindung mit dem Cluster benutzerdefiniertes DNS verwenden möchten, müssen Sie anhand der OpenShift-Dokumentation [eine benutzerdefinierte Zertifizierungsstelle für Ihren Eingangscontroller](https://docs.openshift.com/container-platform/4.3/authentication/certificates/replacing-default-ingress-certificate.html) und eine [benutzerdefinierte Zertifizierungsstelle für Ihren API-Server](https://docs.openshift.com/container-platform/4.3/authentication/certificates/api-server.html) konfigurieren.
>

## <a name="next-steps"></a>Nächste Schritte

In diesem Teil des Tutorials haben Sie Folgendes gelernt:
> [!div class="checklist"]
> * Richten Sie die erforderlichen Komponenten ein, und erstellen Sie das erforderliche virtuelle Netzwerk und die Subnetze.
> * Bereitstellen eines Clusters

Fahren Sie mit dem nächsten Tutorial fort:
> [!div class="nextstepaction"]
> [Herstellen einer Verbindung mit einem Azure Red Hat OpenShift 4-Cluster](tutorial-connect-cluster.md)
