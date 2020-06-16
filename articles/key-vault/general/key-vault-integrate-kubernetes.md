---
title: Integrieren von Azure Key Vault mit Kubernetes
description: In diesem Tutorial greifen Sie auf Geheimnisse in Azure Key Vault zu und rufen sie ab und verwenden dann den Secrets Store CSI-Treiber (Container Storage Interface) für die Einbindung in Kubernetes-Pods.
author: taytran0
ms.author: t-trtr
ms.service: key-vault
ms.topic: tutorial
ms.date: 06/04/2020
ms.openlocfilehash: e945a30ca1fcd62fdfccd16d4e853540dbf73d8a
ms.sourcegitcommit: ce44069e729fce0cf67c8f3c0c932342c350d890
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2020
ms.locfileid: "84636934"
---
# <a name="tutorial-configure-and-run-the-azure-key-vault-provider-for-secret-store-csi-driver-on-kubernetes"></a>Tutorial: Konfigurieren und Ausführen des Azure Key Vault-Anbieters für den Secrets Store CSI-Treiber auf Kubernetes

In diesem Tutorial greifen Sie auf Geheimnisse in Azure Key Vault zu und rufen sie ab und verwenden dann den Secrets Store CSI-Treiber (Container Storage Interface) für die Einbindung in Kubernetes-Pods.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines Dienstprinzipals
> * Bereitstellen eines Azure Kubernetes Service-Clusters
> * Installieren von Helm und des Secrets Store CSI-Treibers
> * Erstellen einer Azure Key Vault-Instanz und Festlegen von Geheimnissen
> * Erstellen eines eigenen „SecretProviderClass“-Objekts
> * Bereitstellen Ihres Pods mit eingebundenen Geheimnissen aus Key Vault

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

Installieren Sie die [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli-windows?view=azure-cli-latest), bevor Sie mit diesem Tutorial beginnen.

## <a name="create-a-service-principal-or-use-managed-identities"></a>Erstellen eines Dienstprinzipals oder Verwenden von verwalteten Identitäten

Wenn Sie verwaltete Identitäten verwenden möchten, können Sie mit dem nächsten Abschnitt fortfahren.

Erstellen Sie einen Dienstprinzipal, um zu steuern, auf welche Ressourcen von Ihrer Azure Key Vault-Instanz aus zugegriffen werden kann. Die Zugriffsberechtigungen dieses Dienstprinzipals sind durch die Rollen eingeschränkt, die ihm zugewiesen sind. Mit diesem Feature haben Sie die Kontrolle darüber, wie der Dienstprinzipal Ihre Geheimnisse verwalten kann. Im Beispiel unten lautet der Name des Dienstprinzipals **contosoServicePrincipal**.

```azurecli
az ad sp create-for-rbac --name contosoServicePrincipal --skip-assignment
```
Bei diesem Vorgang wird eine Reihe von Schlüssel-Wert-Paaren zurückgegeben:

![Image](../media/kubernetes-key-vault-1.png)

Kopieren Sie die App-ID (**appID**) und das **Kennwort** (password). Sie benötigen diese Anmeldeinformationen später noch.



## <a name="deploy-an-azure-kubernetes-service-cluster-using-azure-cli"></a>Bereitstellen eines Azure Kubernetes Service-Clusters über die Azure CLI

Sie müssen Azure Cloud Shell nicht verwenden. Ihre Eingabeaufforderung (Terminal) mit einer Azure CLI-Installation reicht aus. 

Befolgen Sie [diese Anleitung](https://docs.microsoft.com/azure/aks/kubernetes-walkthrough), und arbeiten Sie darin die folgenden Abschnitte durch: **Erstellen einer Ressourcengruppe**, **Erstellen eines AKS-Clusters** und **Herstellen einer Verbindung mit dem Cluster**.

**Hinweis:** Beachten Sie die hier angegebenen Informationen, falls Sie planen, anstelle eines Dienstprinzipals eine Podidentität zu verwenden. Aktivieren Sie sie wie unten gezeigt beim Erstellen des Kubernetes-Clusters:

```azurecli
az aks create -n contosoAKSCluster -g contosoResourceGroup --kubernetes-version 1.16.9 --node-count 1 --enable-managed-identity
```

1. Führen Sie das [Festlegen Ihrer PATH-Umgebungsvariable](https://www.java.com/en/download/help/path.xml) auf die heruntergeladene Datei „kubectl.exe“ durch.
1. Überprüfen Sie Ihre Kubernetes-Version mit dem unten angegebenen Befehl. Mit diesem Befehl wird die Client- und die Serverversion ausgegeben. Die Clientversion ist die Datei „kubectl.exe“, die Sie installiert haben, und die Serverversion ist die Azure Kubernetes Services-Instanz, auf der Ihr Cluster ausgeführt wird.
    ```azurecli
    kubectl version
    ```
1. Stellen Sie sicher, dass Sie die Kubernetes-Version **v1.16.0** oder höher verwenden. Mit diesem Befehl wird ein Upgrade sowohl für den Kubernetes-Cluster als auch für den Knotenpool durchgeführt. Die Ausführung kann einige Minuten dauern. In diesem Beispiel werden die Ressourcengruppe **contosoResourceGroup** und der Kubernetes-Cluster **contosoAKSCluster** verwendet.
    ```azurecli
    az aks upgrade --kubernetes-version 1.16.9 --name contosoAKSCluster --resource-group contosoResourceGroup
    ```
1. Zeigen Sie die Metadaten des von Ihnen erstellten AKS-Clusters mit dem unten angegebenen Befehl an. Kopieren Sie **principalId**, **clientId**, **subscriptionId** und **nodeResourceGroup**.
    ```azurecli
    az aks show --name contosoAKSCluster --resource-group contosoResourceGroup
    ```

    Hier ist die Ausgabe mit den beiden hervorgehobenen Parametern angegeben.
    
    ![Abbildung](../media/kubernetes-key-vault-5.png) ![Abbildung](../media/kubernetes-key-vault-6.png)
    
## <a name="install-helm-and-secrets-store-csi-driver"></a>Installieren von Helm und des Secrets Store CSI-Treibers

Für die Installation des Secrets Store CSI-Treibers müssen Sie [Helm](https://helm.sh/docs/intro/install/) installieren.

Mit der [Secrets Store CSI](https://github.com/Azure/secrets-store-csi-driver-provider-azure/blob/master/charts/csi-secrets-store-provider-azure/README.md)-Treiberschnittstelle können Sie Geheimnisinhalte abrufen, die auf einer Azure Key Vault-Instanz gespeichert sind. Anschließend können Sie die Treiberschnittstelle dann nutzen, um diese Geheimnisinhalte in Kubernetes-Pods einzubinden.

1. Überprüfen Sie die Helm-Version, und stellen Sie sicher, dass Version 3 oder höher verwendet wird:
    ```azurecli
    helm version
    ```
1. Installieren Sie den Secrets Store CSI-Treiber und den Azure Key Vault-Anbieter für den Treiber:
    ```azurecli
    helm repo add csi-secrets-store-provider-azure https://raw.githubusercontent.com/Azure/secrets-store-csi-driver-provider-azure/master/charts

    helm install csi-secrets-store-provider-azure/csi-secrets-store-provider-azure --generate-name
    ```

## <a name="create-an-azure-key-vault-and-set-secrets"></a>Erstellen einer Azure Key Vault-Instanz und Festlegen von Geheimnissen

Befolgen Sie [diese Anleitung](https://docs.microsoft.com/azure/key-vault/secrets/quick-create-cli), um Ihre eigene Key Vault-Instanz zu erstellen und Ihre Geheimnisse festzulegen.

**Hinweis:** Es ist nicht erforderlich, Azure Cloud Shell zu verwenden oder eine neue Ressourcengruppe zu erstellen. Es reicht aus, wenn Sie die Ressourcengruppe nutzen, die Sie zuvor für den Kubernetes-Cluster erstellt haben.

## <a name="create-your-own-secretproviderclass-object"></a>Erstellen eines eigenen „SecretProviderClass“-Objekts

Verwenden Sie [diese Vorlage](https://github.com/Azure/secrets-store-csi-driver-provider-azure/blob/master/examples/v1alpha1_secretproviderclass.yaml), um Ihr eigenes benutzerdefiniertes „SecretProviderClass“-Objekt zu erstellen. Hiermit können Sie anbieterspezifische Parameter für den Secrets Store CSI-Treiber bereitstellen. Dieses Objekt ermöglicht den Identitätszugriff auf Ihre Key Vault-Instanz.

Verwenden Sie die bereitgestellte YAML-Beispieldatei für „SecretProviderClass“. Sie fügen die fehlenden Parameter ein. Die folgenden Parameter sind erforderlich:

1.  **userAssignedIdentityID:** Client-ID des Dienstprinzipals
1.  **keyvaultName:** Name der Key Vault-Instanz
1.  **objects:** Dieses Objekt enthält den gesamten Geheimnisinhalt, den Sie einbinden möchten.
    1.  **objectName:** Name des Geheimnisinhalts
    1.  **objectType:** Objekttyp (Geheimnis, Schlüssel, Zertifikat)
1.  **resourceGroup:** Name der Ressourcengruppe
1.  **subscriptionId:** Abonnement-ID der Key Vault-Instanz
1.  **tenantID:** Mandanten-ID (Verzeichnis-ID) der Key Vault-Instanz

Unten ist die aktualisierte Vorlage angegeben. Laden Sie sie als YAML-Datei herunter, und fügen Sie die Informationen in die entsprechenden erforderlichen Felder ein. In diesem Beispiel wird die Key Vault-Instanz **contosoKeyVault5** mit den beiden Geheimnissen **secret1** und **secret2** verwendet.

**Hinweis:** Bei Verwendung von verwalteten Identitäten muss das Feld**usePodIdentity** auf **true** festgelegt werden. Behalten Sie im Feld **userAssignedIdentityID** die Anführungszeichen ( **""** ) bei, und fügen Sie keinen weiteren Text hinzu. 

```yaml
apiVersion: secrets-store.csi.x-k8s.io/v1alpha1
kind: SecretProviderClass
metadata:
  name: azure-kvname
spec:
  provider: azure
  parameters:
    usePodIdentity: "false"                   # [REQUIRED] Set to "true" if using managed identities
    useVMManagedIdentity: "false"             # [OPTIONAL] if not provided, will default to "false"
    userAssignedIdentityID: "servicePrincipalClientID"       # [REQUIRED] If using a Service Principal, use the client id to specify which user assigned managed identity to use. If using a user assigned identity as the VM's managed identity, then specify the identity's client id. If empty, then defaults to use the system assigned identity on the VM
                                                             #     az ad sp show --id http://contosoServicePrincipal --query appId -o tsv
                                                             #     the above command will return the Client ID of your service principal
    keyvaultName: "contosoKeyVault5"          # [REQUIRED] the name of the Key Vault
                                              #     az keyvault show --name contosoKeyVault5
                                              #     the above command will displays the Key Vault metadata, which includes the subscription ID, resource group name, Key Vault 
    cloudName: ""                             # [OPTIONAL for Azure] if not provided, azure environment will default to AzurePublicCloud
    objects:  |
      array:
        - |
          objectName: secret1                 # [REQUIRED] object name
                                              #     az keyvault secret list --vault-name “contosoKeyVault5”
                                              #     the above command will display a list of secret names from your Key Vault
          objectType: secret                  # [REQUIRED] object types: secret, key or cert
          objectVersion: ""                   # [OPTIONAL] object versions, default to latest if empty
        - |
          objectName: secret2
          objectType: secret
          objectVersion: ""
    resourceGroup: "contosoResourceGroup"     # [REQUIRED] the resource group name of the Key Vault
    subscriptionId: "subscriptionID"          # [REQUIRED] the subscription ID of the Key Vault
    tenantId: "tenantID"                      # [REQUIRED] the tenant ID of the Key Vault
```
Unten ist die Konsolenausgabe für „az keyvault show --name contosoKeyVault5“ mit den relevanten hervorgehobenen Metadaten angegeben:

![Image](../media/kubernetes-key-vault-2.png)

## <a name="assign-your-service-principal-or-use-managed-identities"></a>Zuweisen Ihres Dienstprinzipals oder Verwenden von verwalteten Identitäten

### <a name="using-service-principal"></a>Verwenden eines Dienstprinzipals

Gehen Sie bei Verwendung eines Dienstprinzipals wie folgt vor: Sie müssen Ihrem Dienstprinzipal die Berechtigung zum Zugreifen auf Ihre Key Vault-Instanz und zum Abrufen von Geheimnissen erteilen. Weisen Sie die Rolle **„Leser“** zu, und erteilen Sie dem Dienstprinzipal die Berechtigung zum Abrufen ( **„get“** ) von Geheimnissen von Ihrer Key Vault-Instanz, indem Sie die hier angegebenen Schritte ausführen:

1. Weisen Sie den Dienstprinzipal der vorhandenen Key Vault-Instanz zu. Der Parameter **$AZURE_CLIENT_ID** ist die **appId**, die Sie nach dem Erstellen Ihres Dienstprinzipals kopiert haben.
    ```azurecli
    az role assignment create --role Reader --assignee $AZURE_CLIENT_ID --scope /subscriptions/$SUBID/resourcegroups/$KEYVAULT_RESOURCE_GROUP/providers/Microsoft.KeyVault/vaults/$KEYVAULT_NAME
    ```

    Hier ist die Ausgabe des Befehls angegeben: 

    ![Image](../media/kubernetes-key-vault-3.png)

1. Erteilen Sie dem Dienstprinzipal die Berechtigung zum Abrufen von Geheimnissen:
    ```azurecli
    az keyvault set-policy -n $KEYVAULT_NAME --secret-permissions get --spn $AZURE_CLIENT_ID
    ```

1. Sie haben Ihren Dienstprinzipal nun so konfiguriert, dass er über die Berechtigung zum Lesen von Geheimnissen von Ihrer Key Vault-Instanz verfügt. **$AZURE_CLIENT_SECRET** ist das **Kennwort** für Ihren Dienstprinzipal. Fügen Sie die Anmeldeinformationen des Dienstprinzipals als Kubernetes-Geheimnis hinzu, auf das vom Secrets Store CSI-Treiber zugegriffen werden kann:
    ```azurecli
    kubectl create secret generic secrets-store-creds --from-literal clientid=$AZURE_CLIENT_ID --from-literal clientsecret=$AZURE_CLIENT_SECRET
    ```

**Hinweis:** Es kann sein, dass Sie später beim Bereitstellen des Kubernetes-Pods einen Fehler zu einer ungültigen ID für einen geheimen Clientschlüssel erhalten. Unter Umständen verfügen Sie über eine ältere ID für einen geheimen Clientschlüssel, die abgelaufen ist oder zurückgesetzt wurde. Löschen Sie zum Beheben dieses Problems „secrets-store-creds“ für Ihr Geheimnis, und erstellen Sie diese Informationen mit der aktuellen ID für den geheimen Clientschlüssel neu. Führen Sie den folgenden Befehl aus, um „secrets-store-creds“ zu löschen:
```azurecli
kubectl delete secrets secrets-store-creds
```

Falls Sie die ID des geheimen Clientschlüssels für Ihren Dienstprinzipal vergessen haben, können Sie sie mit dem folgenden Befehl zurücksetzen:

```azurecli
az ad sp credential reset --name contosoServicePrincipal --credential-description "APClientSecret" --query password -o tsv
```

### <a name="using-managed-identities"></a>Verwenden von verwalteten Identitäten

Weisen Sie dem AKS-Cluster, den Sie erstellt haben, bei Verwendung von verwalteten Identitäten bestimmte Rollen zu. 
1. Ihrem AKS-Cluster muss die Rolle [Mitwirkender für verwaltete Identität](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#managed-identity-contributor) zugewiesen sein, um eine vom Benutzer zugewiesene verwaltete Identität erstellen, auflisten oder lesen zu können. Stellen Sie sicher, dass die **$clientId** die ID des Kubernetes-Clusters ist.

    ```azurecli
    az role assignment create --role "Managed Identity Contributor" --assignee $clientId --scope /subscriptions/$SUBID/resourcegroups/$NODE_RESOURCE_GROUP
    
    az role assignment create --role "Virtual Machine Contributor" --assignee $clientId --scope /subscriptions/$SUBID/resourcegroups/$NODE_RESOURCE_GROUP
    ```

1. Installieren Sie die Azure AD-Identität (Azure Active Directory) in AKS.
    ```azurecli
    helm repo add aad-pod-identity https://raw.githubusercontent.com/Azure/aad-pod-identity/master/charts

    helm install pod-identity aad-pod-identity/aad-pod-identity
    ```

1. Erstellen Sie eine Azure AD-Identität. Kopieren Sie die **clientId** und die **principalId**.
    ```azurecli
    az identity create -g $resourceGroupName -n $identityName
    ```

1. Weisen Sie die Rolle „Leser“ der Azure AD-Identität zu, die Sie gerade für Ihre Key Vault-Instanz erstellt haben. Erteilen Sie für die Identität anschließend die Berechtigung zum Abrufen von Geheimnissen von Ihrer Key Vault-Instanz. Sie verwenden die **clientId** und **principalId** der eben erstellten Azure-Identität.
    ```azurecli
    az role assignment create --role "Reader" --assignee $principalId --scope /subscriptions/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/resourceGroups/contosoResourceGroup/providers/Microsoft.KeyVault/vaults/contosoKeyVault5

    az keyvault set-policy -n contosoKeyVault5 --secret-permissions get --spn $clientId
    ```

## <a name="deploy-your-pod-with-mounted-secrets-from-key-vault"></a>Bereitstellen Ihres Pods mit eingebundenen Geheimnissen aus Key Vault

Mit dem folgenden Befehl wird Ihr „SecretProviderClass“-Objekt konfiguriert:
```azurecli
kubectl apply -f secretProviderClass.yaml
```

### <a name="using-service-principal"></a>Verwenden eines Dienstprinzipals

Gehen Sie bei Verwendung eines Dienstprinzipals wie folgt vor: Mit dem folgenden Befehl werden Ihre Kubernetes-Pods mit „SecretProviderClass“ und den von Ihnen konfigurierten Informationen für „secrets-store-creds“ bereitgestellt. Verwenden Sie die Vorlage für die Bereitstellung unter [Linux](https://github.com/Azure/secrets-store-csi-driver-provider-azure/blob/master/examples/nginx-pod-secrets-store-inline-volume-secretproviderclass.yaml) oder unter [Windows](https://github.com/Azure/secrets-store-csi-driver-provider-azure/blob/master/examples/windows-pod-secrets-store-inline-volume-secret-providerclass.yaml).
```azurecli
kubectl apply -f updateDeployment.yaml
```

### <a name="using-managed-identities"></a>Verwenden von verwalteten Identitäten

Gehen Sie bei Verwendung von verwalteten Identitäten wie folgt vor: Erstellen Sie ein **AzureIdentity**-Element in Ihrem Cluster, für das auf die zuvor erstellte Identität verwiesen wird. Erstellen Sie anschließend ein **AzureIdentityBinding**-Element, für das auf das erstellte **AzureIdentity**-Element verwiesen wird. Verwenden Sie die unten angegebene Vorlage. Fügen Sie die entsprechenden Parameterwerte ein, und speichern Sie sie als **podIdentityAndBinding.yaml**.  
```yml
apiVersion: aadpodidentity.k8s.io/v1
kind: AzureIdentity
metadata:
    name: "azureIdentityName"               # The name of your Azure Idenity
spec:
    type: 0                                 # Set type: 0 for Managed Service Identity
    resourceID: /subscriptions/<SUBID>/resourcegroups/<RESOURCEGROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<AZUREIDENTITYNAME>
    clientID: "managedIdentityClientId"     # The clientId of your Azure Identity that you created earlier
---
apiVersion: aadpodidentity.k8s.io/v1
kind: AzureIdentityBinding
metadata:
    name: azure-pod-identity-binding
spec:
    azureIdentity: "azureIdentityName"      # The name of your Azure Idenity
    selector: azure-pod-identity-binding-selector
```
    
Führen Sie den folgenden Befehl aus, um die Bindung auszuführen:

```azurecli
kubectl apply -f podIdentityAndBinding.yaml
```

Als Nächstes folgt die eigentliche Bereitstellung des Pods. Unten ist die YAML-Datei für die Bereitstellung angegeben, für die die Bindung der Podidentität aus dem letzten Schritt genutzt wird. Speichern Sie diese Datei als **podBindingDeployment.yaml**.

```yml
kind: Pod
apiVersion: v1
metadata:
    name: nginx-secrets-store-inline
    labels:
    aadpodidbinding: azure-pod-identity-binding-selector
spec:
    containers:
    - name: nginx
        image: nginx
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
            secretProviderClass: azure-kvname
```

Führen Sie den folgenden Befehl aus, um Ihren Pod bereitzustellen:

```azurecli
kubectl apply -f podBindingDeployment.yaml
```
### <a name="check-status-and-secret-content"></a>Überprüfen des Status und Geheimnisinhalts 
Zeigen Sie die bereitgestellten Pods wie folgt an:
```azurecli
kubectl get pods
```

Verwenden Sie den folgenden Befehl, um den Status Ihres Pods zu überprüfen:
```azurecli
kubectl describe pod/nginx-secrets-store-inline
```

![Image](../media/kubernetes-key-vault-4.png)

Der bereitgestellte Pod sollte den Status „Wird ausgeführt“ aufweisen. Unten im Abschnitt „Ereignisse“ sind alle links angezeigten Ereignisse als „Normal“ klassifiziert.
Nachdem Sie sich vergewissert haben, dass der Pod ausgeführt wird, können Sie überprüfen, ob Ihr Pod die Geheimnisse aus Ihrer Key Vault-Instanz enthält.

Zeigen Sie wie folgt alle Geheimnisse des Pods an:
```azurecli
kubectl exec -it nginx-secrets-store-inline -- ls /mnt/secrets-store/
```

Rufen Sie den Inhalt eines bestimmten Geheimnisses wie folgt ab:
```azurecli
kubectl exec -it nginx-secrets-store-inline -- cat /mnt/secrets-store/secret1
```

Überprüfen Sie den Inhalt des angezeigten Geheimnisses.

## <a name="next-steps"></a>Nächste Schritte

Stellen Sie sicher, dass Ihre Key Vault-Instanz wiederherstellbar ist:
> [!div class="nextstepaction"]
> [Aktivieren des vorläufigen Löschens](https://docs.microsoft.com/azure/key-vault/general/soft-delete-clid)
