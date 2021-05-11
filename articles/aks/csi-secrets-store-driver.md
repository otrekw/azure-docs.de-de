---
title: Verwenden des Secrets Store CSI-Treibers für Azure Kubernetes Service-Geheimnisse
description: Erfahren Sie, wie Sie mithilfe des Secrets Store CSI-Treibers Geheimnisspeicher in Azure Kubernetes Service (AKS) integrieren.
author: nickomang
ms.author: nickoman
ms.service: container-service
ms.topic: how-to
ms.date: 03/30/2021
ms.custom: template-how-to
ms.openlocfilehash: 535c79dba122fd22cc09b4a74b04b846d55538f9
ms.sourcegitcommit: dd425ae91675b7db264288f899cff6add31e9f69
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/01/2021
ms.locfileid: "108331266"
---
# <a name="use-the-secrets-store-csi-driver-for-kubernetes-in-an-azure-kubernetes-service-aks-cluster-preview"></a>Verwenden des Secrets Store CSI-Treibers für Kubernetes in einem AKS-Cluster (Azure Kubernetes Service) (Vorschau)

Der Secrets Store CSI-Treiber für Kubernetes ermöglicht, Azure Key Vault über ein [CSI-Volume][kube-csi] als Geheimnisspeicher in einen Kubernetes-Cluster zu integrieren.

## <a name="prerequisites"></a>Voraussetzungen

- Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

- Bevor Sie beginnen, installieren Sie die neueste Version der [Azure-Befehlszeilenschnittstelle](/cli/azure/install-azure-cli-windows) und die Erweiterung *aks-preview*.

## <a name="features"></a>Features

- Einbinden von Geheimnissen, Schlüsseln und Zertifikaten in einen Pod mithilfe eines CSI-Volumes
- Unterstützung von CSI-Inlinevolumes (ab Kubernetes-Version 1.15)
- Unterstützung für das Einbinden mehrerer Geheimnisspeicherobjekte als einzelnes Volume
- Unterstützung der Podportabilität mit der SecretProviderClass-CRD
- Unterstützung für Windows-Container (ab Kubernetes-Version 1.18)
- Synchronisieren mit Kubernetes-Geheimnissen (ab Secrets Store CSI-Treiber v0.0.10)
- Unterstützung für die automatische Rotation eingebundener Inhalte und synchronisierter Kubernetes-Geheimnisse (ab Secrets Store CSI-Treiber v0.0.15)

## <a name="register-the-aks-azurekeyvaultsecretsprovider-preview-feature"></a>Registrieren der Previewfunktion `AKS-AzureKeyVaultSecretsProvider`

[!INCLUDE [preview features callout](./includes/preview/preview-callout.md)]

Um einen AKS-Cluster zu erstellen, der den Secrets Store CSI-Treiber verwenden kann, müssen Sie das Featureflag `AKS-AzureKeyVaultSecretsProvider` in Ihrem Abonnement aktivieren.

Registrieren Sie das Featureflag `AKS-AzureKeyVaultSecretsProvider` mithilfe des Befehls [az feature register][az-feature-register], wie im folgenden Beispiel gezeigt:

```azurecli-interactive
az feature register --namespace "Microsoft.ContainerService" --name "AKS-AzureKeyVaultSecretsProvider"
```

Es dauert einige Minuten, bis der Status *Registered (Registriert)* angezeigt wird. Überprüfen Sie den Registrierungsstatus mithilfe des Befehls [az feature list][az-feature-list]:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/AKS-AzureKeyVaultSecretsProvider')].{Name:name,State:properties.state}"
```

Wenn der Vorgang abgeschlossen ist, können Sie die Registrierung des *Microsoft.ContainerService*-Ressourcenanbieters mit dem Befehl [az provider register][az-provider-register] aktualisieren:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

## <a name="install-the-aks-preview-cli-extension"></a>Installieren der CLI-Erweiterung aks-preview

Sie benötigen außerdem mindestens Version 0.5.10 der Erweiterung *aks-preview* für die Azure-Befehlszeilenschnittstelle. Installieren Sie die Erweiterung *aks-preview* der Azure-Befehlszeilenschnittstelle mithilfe des Befehls [az extension add][az-extension-add]. Wenn Sie die Erweiterung bereits installiert haben, führen Sie mit dem Befehl [az extension update][az-extension-update] ein Update auf die neueste verfügbare Version durch.

```azurecli-interactive
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```

## <a name="create-an-aks-cluster-with-secrets-store-csi-driver-support"></a>Erstellen eines AKS-Clusters mit Unterstützung für den Secrets Store CSI-Treiber

> [!NOTE]
> Wenn Sie vorhaben, den Zugriff auf den Cluster über eine benutzer- oder systemseitig zugewiesene verwaltete Identität bereitzustellen, aktivieren Sie mit dem Flag `enable-managed-identity` Azure Active Directory in Ihrem Cluster. Weitere Informationen finden Sie unter [Verwenden verwalteter Identitäten in Azure Kubernetes Service][aks-managed-identity].

Verwenden Sie den Befehl [az-aks-create][az-aks-create] mit dem Add-On `azure-keyvault-secrets-provider`, um einen AKS-Cluster mit Unterstützung für den Secrets Store CSI-Treiber zu erstellen:

```azurecli-interactive
az aks create -n myAKSCluster -g myResourceGroup --enable-addons azure-keyvault-secrets-provider
```

## <a name="upgrade-an-existing-aks-cluster-with-secrets-store-csi-driver-support"></a>Upgraden eines vorhandenen AKS-Clusters für die Unterstützung des Secrets Store CSI-Treibers

> [!NOTE]
> Wenn Sie vorhaben, den Zugriff auf den Cluster über eine benutzer- oder systemseitig zugewiesene verwaltete Identität bereitzustellen, aktivieren Sie mit dem Flag `enable-managed-identity` Azure Active Directory in Ihrem Cluster. Weitere Informationen finden Sie unter [Verwenden verwalteter Identitäten in Azure Kubernetes Service][aks-managed-identity].

Verwenden Sie den Befehl [az-aks-create][az-aks-create] mit dem Add-On `azure-keyvault-secrets-provider`, um ein Upgrade eines vorhandenen AKS-Clusters auf die Unterstützung für den Secrets Store CSI-Treiber durchzuführen:

```azurecli-interactive
az aks upgrade -n myAKSCluster -g myResourceGroup --enable-addons azure-keyvault-secrets-provider
```

Mit diesen Befehlen werden der Secrets Store CSI-Treiber und der Azure Key Vault-Anbieter auf Ihren Knoten installiert. Zur Überprüfung listen Sie alle Pods aus allen Namespaces auf und vergewissern sich, dass Ihre Ausgabe der folgenden ähnelt:

```bash
kubectl get pods -n kube-system

NAMESPACE     NAME                                     READY   STATUS    RESTARTS   AGE
kube-system   aks-secrets-store-csi-driver-4vpkj       3/3     Running   2          4m25s
kube-system   aks-secrets-store-csi-driver-ctjq6       3/3     Running   2          4m21s
kube-system   aks-secrets-store-csi-driver-tlvlq       3/3     Running   2          4m24s
kube-system   aks-secrets-store-provider-azure-5p4nb   1/1     Running   0          4m21s
kube-system   aks-secrets-store-provider-azure-6pqmv   1/1     Running   0          4m24s
kube-system   aks-secrets-store-provider-azure-f5qlm   1/1     Running   0          4m25s
```

### <a name="enabling-autorotation"></a>Aktivieren der automatischen Rotation

> [!NOTE]
> Wenn diese Option aktiviert ist, aktualisiert der Secrets Store CSI-Treiber die Podeinbindung und das Kubernetes-Geheimnis, das in secretObjects von SecretProviderClass definiert ist, indem er alle zwei Minuten sämtliche Änderungen abruft.

Um die automatische Rotation von Geheimnissen zu aktivieren, verwenden Sie beim Erstellen Ihres Clusters das Flag `enable-secret-rotation`:

```azurecli-interactive
az aks create -n myAKSCluster2 -g myResourceGroup --enable-addons azure-keyvault-secrets-provider --enable-secret-rotation --rotation-poll-interval 5m
```

## <a name="create-or-use-an-existing-azure-key-vault"></a>Erstellen einer Azure Key Vault-Instanz oder Verwenden einer vorhandenen

Zusätzlich zu einem AKS-Cluster benötigen Sie eine Azure Key Vault-Ressource, die den Inhalt des Geheimnisses enthält. Führen Sie die folgenden Schritte aus, um eine Azure Key Vault-Instanz zu erstellen:

1. [Erstellen eines Schlüsseltresors][create-key-vault]
2. [Festlegen eines Geheimnisses in einem Schlüsseltresor][set-secret-key-vault]

Notieren Sie sich die folgenden Eigenschaften für die Verwendung im nächsten Abschnitt:

- Name des Geheimnisobjekts in Key Vault
- Inhaltstyp des Geheimnisses (Geheimnis, Schlüssel, Zertifikat)
- Name der Key Vault-Ressource
- Azure-Mandanten-ID, zu der das Abonnement gehört

## <a name="create-and-apply-your-own-secretproviderclass-object"></a>Erstellen und Anwenden eines eigenen SecretProviderClass-Objekts

Um den Secrets Store CSI-Treiber für Ihren AKS-Cluster zu verwenden und zu konfigurieren, erstellen Sie eine benutzerdefinierte SecretProviderClass-Ressource.

Hier ist ein Beispiel für die Verwendung eines Dienstprinzipals für den Zugriff auf den Schlüsseltresor:

```yml
apiVersion: secrets-store.csi.x-k8s.io/v1alpha1
kind: SecretProviderClass
metadata:
  name: azure-kvname
spec:
  provider: azure
  parameters:
    usePodIdentity: "false"         # [OPTIONAL] if not provided, will default to "false"
    keyvaultName: "kvname"          # the name of the KeyVault
    cloudName: ""                   # [OPTIONAL for Azure] if not provided, azure environment will default to AzurePublicCloud 
    objects:  |
      array:
        - |
          objectName: secret1
          objectType: secret        # object types: secret, key or cert
          objectVersion: ""         # [OPTIONAL] object versions, default to latest if empty
        - |
          objectName: key1
          objectType: key
          objectVersion: ""
    tenantId: "<tenant-id>"                 # the tenant ID of the KeyVault
```

Weitere Informationen finden Sie unter [Erstellen Ihres eigenen SecretProviderClass-Objekts][sample-secret-provider-class]. Achten Sie darauf, die oben notierten Werte zu verwenden.

## <a name="provide-identity-to-access-azure-key-vault"></a>Bereitstellen einer Identität für den Zugriff auf Azure Key Vault

Im Beispiel in diesem Artikel wird ein Dienstprinzipal verwendet, der Azure Key Vault-Anbieter bietet aber vier Zugriffsmethoden. Sehen Sie sich alle vier an, und wählen Sie die Option aus, die für Ihren Anwendungsfall am besten geeignet ist. Beachten Sie, dass je nach gewählter Methode möglicherweise zusätzliche Schritte erforderlich sind, z. B. das Erteilen von Berechtigungen für den Dienstprinzipal zum Abrufen von Geheimnissen aus dem Schlüsseltresor.

- [Dienstprinzipal][service-principal-access]
- [Podidentität][pod-identity-access]
- [Benutzerseitig zugewiesene verwaltete Identität][ua-mi-access]
- [Systemseitig zugewiesene verwaltete Identität][sa-mi-access]

### <a name="apply-the-secretproviderclass-to-your-cluster"></a>Anwenden der SecretProviderClass auf Ihren Cluster

Als Nächstes stellen Sie die erstellte SecretProviderClass bereit. Beispiel:

```bash
kubectl apply -f ./new-secretproviderclass.yaml
```

## <a name="update-and-apply-your-clusters-deployment-yaml"></a>Aktualisieren und Anwenden der Bereitstellungs-YAML Ihres Clusters

Um sicherzustellen, dass Ihr Cluster die neue benutzerdefinierte Ressource verwendet, aktualisieren Sie die Bereitstellungs-YAML. Ein umfassenderes Beispiel finden Sie in der [Beispielbereitstellung][sample-deployment], bei der ein Dienstprinzipal für den Zugriff auf Azure Key Vault verwendet wird. Befolgen Sie auch alle zusätzlichen Schritte, die für die ausgewählte Methode für den Zugriff auf den Schlüsseltresor erforderlich sind.

```yml
kind: Pod
apiVersion: v1
metadata:
  name: busybox-secrets-store-inline
spec:
  containers:
  - name: busybox
    image: k8s.gcr.io/e2e-test-images/busybox:1.29
    command:
      - "/bin/sh"
      - "10000"
    volumeMounts:
    - name: secrets-store-inline
      mountPath: "/mnt/secrets-store"
      readOnly: true
  volumes:
    - name: secrets-store-inline
      csi:
        driver: secrets-store.csi.k8s.io
        readOnly: true
        volumeAttributes:
          secretProviderClass: "azure-kvname"
        nodePublishSecretRef:                       # Only required when using service principal mode
          name: secrets-store-creds                 # Only required when using service principal mode. The name of the Kubernetes secret that contains the service principal credentials to access keyvault.
```

Wenden Sie die aktualisierte Bereitstellung auf den Cluster an:

```bash
kubectl apply -f ./my-deployment.yaml
```

## <a name="validate-the-secrets"></a>Überprüfen der Geheimnisse

Nachdem der Pod gestartet wurde, ist der eingebundene Inhalt unter dem in Ihrer Bereitstellungs-YAML angegebenen Volumepfad verfügbar.

```Bash
## show secrets held in secrets-store
kubectl exec busybox-secrets-store-inline -- ls /mnt/secrets-store/

## print a test secret 'secret1' held in secrets-store
kubectl exec busybox-secrets-store-inline -- cat /mnt/secrets-store/secret1
```

## <a name="disable-secrets-store-csi-driver"></a>Deaktivieren des Secrets Store CSI-Treibers

Verwenden Sie den Befehl „az aks“ mit dem Deaktivierungs-Add-On `azure-keyvault-secrets-provider`, um die Funktionen des Secrets Store CSI-Treibers in einem vorhandenen Cluster zu deaktivieren:

```azurecli-interactive
az aks disable-addons -n myAKSCluster -g myResourceGroup --addons azure-keyvault-secrets-provider
```

## <a name="next-steps"></a>Nächste Schritte
<!-- Add a context sentence for the following links -->
Nachdem Sie erfahren haben, wie Sie den Secrets Store CSI-Treiber mit einem AKS-Cluster verwenden, sehen Sie sich die folgenden Ressourcen an:

- [Ausführen des Azure Key Vault-Anbieters für den Secrets Store CSI-Treiber][key-vault-provider]
- [Aktivieren von CSI-Treibern für Azure-Datenträger und Azure Files in AKS][csi-storage-drivers]

<!-- Links -->
<!-- Internal -->
[az-feature-register]: /cli/azure/feature#az_feature_register
[az-feature-list]: /cli/azure/feature#az_feature_list
[az-provider-register]: /cli/azure/provider#az_provider_register
[az-extension-add]: /cli/azure/extension#az_extension_add
[az-extension-update]: /cli/azure/extension#az_extension_update
[az-aks-create]: /cli/azure/aks#az_aks_create
[key-vault-provider]: ../key-vault/general/key-vault-integrate-kubernetes.md
[csi-storage-drivers]: ./csi-storage-drivers.md
[create-key-vault]: ../key-vault/general/quick-create-cli.md
[set-secret-key-vault]: ../key-vault/secrets/quick-create-portal.md
[aks-managed-identity]: ./use-managed-identity.md

<!-- External -->
[kube-csi]: https://kubernetes-csi.github.io/docs/
[key-vault-provider-install]: https://azure.github.io/secrets-store-csi-driver-provider-azure/getting-started/installation
[sample-secret-provider-class]: https://azure.github.io/secrets-store-csi-driver-provider-azure/getting-started/usage/#create-your-own-secretproviderclass-object
[service-principal-access]: https://azure.github.io/secrets-store-csi-driver-provider-azure/configurations/identity-access-modes/service-principal-mode/
[pod-identity-access]: https://azure.github.io/secrets-store-csi-driver-provider-azure/configurations/identity-access-modes/pod-identity-mode/
[ua-mi-access]: https://azure.github.io/secrets-store-csi-driver-provider-azure/configurations/identity-access-modes/user-assigned-msi-mode/
[sa-mi-access]: https://azure.github.io/secrets-store-csi-driver-provider-azure/configurations/identity-access-modes/system-assigned-msi-mode/
[sample-deployment]: https://raw.githubusercontent.com/Azure/secrets-store-csi-driver-provider-azure/master/examples/service-principal/pod-inline-volume-service-principal.yaml
