---
title: Verwenden eines kundenseitig verwalteten Schlüssels zum Verschlüsseln von Azure-Datenträgern in Azure Kubernetes Service (AKS)
description: Bring Your Own Key(BYOK) zum Verschlüsseln von AKS-Datenträgern für Betriebssystem und Daten.
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 01/12/2020
ms.author: mlearned
ms.openlocfilehash: 054113da7fe06436fab0cdfff6c4257f1263de97
ms.sourcegitcommit: 7221918fbe5385ceccf39dff9dd5a3817a0bd807
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/21/2020
ms.locfileid: "76290358"
---
# <a name="bring-your-own-keys-byok-with-azure-disks-in-azure-kubernetes-service-aks"></a>Bring Your Own Key (BYOK) mit Azure-Datenträgern in Azure Kubernetes Service (AKS)

Azure Storage verschlüsselt alle Daten in einem ruhenden Speicherkonto. Standardmäßig werden Daten mit von Microsoft verwalteten Schlüsseln verschlüsselt. Für zusätzliche Kontrolle über die Verschlüsselungsschlüssel können Sie [kundenseitig verwaltete Schlüssel][customer-managed-keys] bereitstellen, die für die Verschlüsselung der Datenträger für Betriebssystem und Daten für Ihre AKS-Cluster verwendet werden sollen.

> [!NOTE]
> Linux- und Windows-basierte AKS-Cluster mit BYOK sind in [Azure-Regionen][supported-regions] verfügbar, die die serverseitige Verschlüsselung von verwalteten Azure-Datenträgern unterstützen.

## <a name="before-you-begin"></a>Voraussetzungen

* In diesem Artikel wird vorausgesetzt, dass Sie einen *neuen AKS-Cluster* erstellen.

* Sie müssen vorläufiges Löschen und den Schutz vor endgültigem Löschen für *Azure Key Vault* aktivieren, wenn Sie Key Vault zum Verschlüsseln von verwalteten Datenträgern verwenden.

* Sie benötigen Version 2.0.79 oder höher der Azure-Befehlszeilenschnittstelle sowie die Erweiterung „aks-preview 0.4.26“.

> [!IMPORTANT]
> AKS-Previewfunktionen stehen gemäß dem Self-Service- und Aktivierungsprinzip zur Verfügung. Vorschauversionen werden „wie besehen“ und „wie verfügbar“ bereitgestellt und sind von den Vereinbarungen zum Service Level und der eingeschränkten Garantie ausgeschlossen. AKS-Vorschauen werden teilweise vom Kundensupport auf der Grundlage der bestmöglichen Leistung abgedeckt. Daher sind diese Funktionen nicht für die Verwendung in der Produktion vorgesehen. Weitere Informationen finden Sie in den folgenden Supportartikeln:
>
> * [Unterstützungsrichtlinien für Azure Kubernetes Service](support-policies.md)
> * [Häufig gestellte Fragen zum Azure-Support](faq.md)

## <a name="install-latest-aks-cli-preview-extension"></a>Installieren der neuesten AKS-CLI-Vorschauerweiterung

Um kundenseitig verwaltete Schlüssel zu verwenden, benötigen Sie mindestens Version 0.4.26 der CLI-Erweiterung *aks-preview*. Installieren Sie die Azure CLI-Erweiterung *aks-preview* mit dem Befehl [az extension add][az-extension-add], und suchen Sie dann mit dem Befehl [az extension update][az-extension-update] nach verfügbaren Updates:

```azurecli-interactive
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```

## <a name="create-an-azure-key-vault-instance"></a>Erstellen einer Azure Key Vault-Instanz

Verwenden Sie eine Azure Key Vault-Instanz zum Speichern Ihrer Schlüssel.  Sie können optional das Azure-Portal verwenden, um [kundenseitig verwaltete Schlüsseln mit Azure Key Vault zu konfigurieren][byok-azure-portal].

Erstellen Sie eine neue *Ressourcengruppe*, erstellen Sie dann eine neue *Key Vault*-Instanz, und aktivieren Sie vorläufiges Löschen und den Schutz vor endgültigem Löschen.  Stellen Sie sicher, dass Sie dieselben Regions- und Ressourcengruppennamen für jeden Befehl verwenden.

```azurecli-interactive
# Optionally retrieve Azure region short names for use on upcoming commands
az account list-locations
```

```azurecli-interactive
# Create new resource group in a supported Azure region
az group create -l myAzureRegionName -n myResourceGroup

# Create an Azure Key Vault resource in a supported Azure region
az keyvault create -n myKeyVaultName -g myResourceGroup -l myAzureRegionName  --enable-purge-protection true --enable-soft-delete true
```

## <a name="create-an-instance-of-a-diskencryptionset"></a>Erstellen einer DiskEncryptionSet-Instanz

Ersetzen Sie *myKeyVaultName* durch den Namen Ihres Schlüsseltresors.  Sie benötigen außerdem einen in Azure Key Vault gespeicherten *Schlüssel*, um die folgenden Schritte auszuführen.  Speichern Sie entweder Ihren vorhandenen Schlüssel in dem Key Vault, den Sie in den vorherigen Schritten erstellt haben, oder [generieren Sie einen neuen Schlüssel][key-vault-generate], und ersetzen Sie unten *myKeyName* durch den Namen Ihres Schlüssels.
    
```azurecli-interactive
# Retrieve the Key Vault Id and store it in a variable
keyVaultId=$(az keyvault show --name myKeyVaultName --query [id] -o tsv)

# Retrieve the Key Vault key URL and store it in a variable
keyVaultKeyUrl=$(az keyvault key show --vault-name myKeyVaultName  --name myKeyName  --query [key.kid] -o tsv)

# Create a DiskEncryptionSet
az disk-encryption-set create -n myDiskEncryptionSetName  -l myAzureRegionName  -g myResourceGroup --source-vault $keyVaultId --key-url $keyVaultKeyUrl 
```

## <a name="grant-the-diskencryptionset-access-to-key-vault"></a>Gewähren des DiskEncryptionSet-Zugriffs auf den Schlüsseltresor

Verwenden Sie DiskEncryptionSet und Ressourcengruppen, die Sie in den vorherigen Schritten erstellt haben, und gewähren Sie der DiskEncryptionSet-Ressource Zugriff auf den Azure Key Vault.

```azurecli-interactive
# Retrieve the DiskEncryptionSet value and set a variable
desIdentity=$(az disk-encryption-set show -n myDiskEncryptionSetName  -g myResourceGroup --query [identity.principalId] -o tsv)

# Update security policy settings
az keyvault set-policy -n myKeyVaultName -g myResourceGroup --object-id $desIdentity --key-permissions wrapkey unwrapkey get

# Assign the reader role
az role assignment create --assignee $desIdentity --role Reader --scope $keyVaultId
```

## <a name="create-a-new-aks-cluster-and-encrypt-the-os-disk"></a>Erstellen eines neuen AKS-Clusters und Verschlüsseln des Betriebssystemdatenträgers

Erstellen Sie eine **neue Ressourcengruppe** und einen neuen AKS-Cluster, und verwenden Sie dann Ihren Schlüssel zum Verschlüsseln des Betriebssystemdatenträgers. Kundenseitig verwaltete Schlüssel werden nur in Kubernetes-Versionen höher als 1.17 unterstützt. 

> [!IMPORTANT]
> Stellen Sie sicher, dass Sie eine neue Ressourcengruppe für Ihren AKS-Cluster erstellen.

```azurecli-interactive
# Retrieve the DiskEncryptionSet value and set a variable
diskEncryptionSetId=$(az resource show -n diskEncryptionSetName -g myResourceGroup --resource-type "Microsoft.Compute/diskEncryptionSets" --query [id] -o tsv)

# Create a resource group for the AKS cluster
az group create -n myResourceGroup -l myAzureRegionName

# Create the AKS cluster
az aks create -n myAKSCluster -g myResourceGroup --node-osdisk-diskencryptionset-id $diskEncryptionSetId --kubernetes-version 1.17.0 --generate-ssh-keys
```

Wenn dem oben erstellten Cluster neue Knotenpools hinzugefügt werden, wird der während der Erstellung bereitgestellte kundenseitig verwaltete Schlüssel zum Verschlüsseln des Betriebssystemdatenträgers verwendet.

## <a name="encrypt-your-aks-cluster-data-disk"></a>Verschlüsseln Ihres AKS-Clusterdatenträgers für Daten

Sie können auch die AKS-Datenträger für Daten mit ihren eigenen Schlüsseln verschlüsseln.

> [!IMPORTANT]
> Stellen Sie sicher, dass Sie über die richtigen AKS-Anmeldeinformationen verfügen. Der Dienstprinzipal benötigt „Mitwirkender“-Zugriff auf die Ressourcengruppe, in der Ihr DiskEncryptionSet bereitgestellt ist. Andernfalls erhalten Sie eine Fehlermeldung, die darauf hinweist, dass der Dienstprinzipal keine Berechtigungen besitzt.

```azurecli-interactive
# Retrieve your Azure Subscription Id from id property as shown below
az account list
```

```
someuser@Azure:~$ az account list
[
  {
    "cloudName": "AzureCloud",
    "id": "666e66d8-1e43-4136-be25-f25bb5de5893",
    "isDefault": true,
    "name": "MyAzureSubscription",
    "state": "Enabled",
    "tenantId": "3ebbdf90-2069-4529-a1ab-7bdcb24df7cd",
    "user": {
      "cloudShellID": true,
      "name": "someuser@azure.com",
      "type": "user"
    }
  }
]
```

Erstellen Sie eine Datei namens **byok-azure-disk.yaml**, die folgende Informationen enthält.  Ersetzen Sie „myAzureSubscriptionId“, „myResourceGroup“ und „myDiskEncrptionSetName“ durch Ihre Werte, und wenden Sie die yaml-Datei an.  Stellen Sie sicher, dass Sie die Ressourcengruppe verwenden, in der Ihr DiskEncryptionSet bereitgestellt ist.  Bei Verwendung von Azure Cloud Shell kann diese Datei mit vi oder nano erstellt werden – genau wie bei der Verwendung eines virtuellen oder physischen Systems:

```
kind: StorageClass
apiVersion: storage.k8s.io/v1  
metadata:
  name: hdd
provisioner: kubernetes.io/azure-disk
parameters:
  skuname: Standard_LRS
  kind: managed
  diskEncryptionSetID: "/subscriptions/{myAzureSubscriptionId}/resourceGroups/{myResourceGroup}/providers/Microsoft.Compute/diskEncryptionSets/{myDiskEncryptionSetName}"
```
Führen Sie als Nächstes diese Bereitstellung in Ihrem AKS-Cluster aus:
```azurecli-interactive
# Get credentials
az aks get-credentials --name myAksCluster --resource-group myResourceGroup --output table

# Update cluster
kubectl apply -f byok-azure-disk.yaml
```

## <a name="limitations"></a>Einschränkungen

* BYOK ist derzeit allgemein und als Preview nur in bestimmten [Azure-Regionen][supported-regions] verfügbar.
* Verschlüsselung von Betriebssystemdatenträger mit Kubernetes, Version 1.17 und höher.   
* Nur in Regionen verfügbar, in denen BYOK unterstützt wird.
* Verschlüsselung mit kundenseitig verwalteten Schlüsseln ist derzeit nur für neue AKS-Cluster vorgesehen, ein Upgrade vorhandener Cluster ist nicht möglich.
* AKS-Cluster, die Virtual Machine Scale Sets verwenden, sind erforderlich; keine Unterstützung für VM-Verfügbarkeitsgruppen.


## <a name="next-steps"></a>Nächste Schritte

Lesen Sie [Bewährte Methoden für AKS-Clustersicherheit][best-practices-security].

<!-- LINKS - external -->

<!-- LINKS - internal -->
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
[best-practices-security]: /azure/aks/operator-best-practices-cluster-security
[byok-azure-portal]: /azure/storage/common/storage-encryption-keys-portal
[customer-managed-keys]: /azure/virtual-machines/windows/disk-encryption#customer-managed-keys
[key-vault-generate]: /azure/key-vault/key-vault-manage-with-cli2
[supported-regions]: /azure/virtual-machines/windows/disk-encryption#supported-regions
