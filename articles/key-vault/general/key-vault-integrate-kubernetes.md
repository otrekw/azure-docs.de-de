---
title: Integrieren von Azure Key Vault mit Kubernetes
description: In diesem Tutorial greifen Sie auf Geheimnisse in Azure Key Vault zu und rufen sie daraus ab, indem Sie den Secrets Store CSI-Treiber (Container Storage Interface) für die Einbindung in Kubernetes-Pods verwenden.
author: msmbaldwin
ms.author: mbaldwin
ms.service: key-vault
ms.subservice: general
ms.topic: tutorial
ms.date: 09/25/2020
ms.openlocfilehash: defe546c008f741040c78e639d5bc4b9c6e02fb8
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101741445"
---
# <a name="tutorial-configure-and-run-the-azure-key-vault-provider-for-the-secrets-store-csi-driver-on-kubernetes"></a>Tutorial: Konfigurieren und Ausführen des Azure Key Vault-Anbieters für den Secrets Store CSI-Treiber auf Kubernetes

> [!IMPORTANT]
> Der Secrets Store CSI-Treiber ist ein Open-Source-Projekt, das vom technischen Support von Azure nicht unterstützt wird. Senden Sie Feedback und Probleme im Zusammenhang mit der Key Vault-Integration des CSI-Treibers über den GitHub-Link am unteren Rand der Seite. Dieses Tool wird den Benutzern zur Verfügung gestellt, damit sie es selbst in Clustern installieren und Feedback von unserer Community erhalten können.

In diesem Tutorial greifen Sie auf Geheimnisse in Azure Key Vault zu und rufen sie daraus ab, indem Sie den Secrets Store CSI-Treiber (Container Storage Interface) für die Einbindung der Geheimnisse in Kubernetes-Pods verwenden.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Verwenden verwalteter Identitäten
> * Bereitstellen eines AKS-Clusters (Azure Kubernetes Service) über die Azure CLI.
> * Installieren Sie Helm, den Secrets Store CSI-Treiber und den Azure Key Vault-Anbieter für den CSI-Treiber.
> * Erstellen einer Azure Key Vault-Instanz und Festlegen Ihrer Geheimnisse.
> * Erstellen Ihres eigenen „SecretProviderClass“-Objekts.
> * Bereitstellen Ihres Pods mit eingebundenen Geheimnissen aus Ihrem Schlüsseltresor.

## <a name="prerequisites"></a>Voraussetzungen

* Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

* Installieren Sie die [Azure CLI](/cli/azure/install-azure-cli-windows?view=azure-cli-latest), bevor Sie mit diesem Tutorial beginnen.

In diesem Tutorial wird davon ausgegangen, dass Sie Azure Kubernetes Service auf Linux-Knoten ausführen.

## <a name="use-managed-identities"></a>Verwenden verwalteter Identitäten

Im folgenden Diagramm wird der Flow für eine verwaltete Identität bei der AKS-Key Vault-Integration veranschaulicht:

![Diagramm, das den Flow für eine verwaltete Identität bei der AKS-Key Vault-Integration veranschaulicht](../media/aks-key-vault-integration-flow.png)

## <a name="deploy-an-azure-kubernetes-service-aks-cluster-by-using-the-azure-cli"></a>Bereitstellen eines AKS-Clusters (Azure Kubernetes Service) über die Azure CLI

Sie müssen Azure Cloud Shell nicht verwenden. Ihre Eingabeaufforderung (Terminal) mit einer installierten Azure CLI reicht aus. 

Führen Sie die Abschnitte „Erstellen einer Ressourcengruppe“, „Erstellen eines AKS-Clusters“ und „Herstellen einer Verbindung mit dem Cluster“ in [Bereitstellen eines Azure Kubernetes Service-Clusters mithilfe der Azure CLI](../../aks/kubernetes-walkthrough.md) aus. 

> [!NOTE] 
> Wenn Sie planen, die Podidentität zu nutzen, dann ist `azure` das empfohlene Netzwerk-Plug-In. Weitere Informationen finden Sie in [dieser Dokumentation](https://azure.github.io/aad-pod-identity/docs/configure/aad_pod_identity_on_kubenet/). Erstellen Sie den Kubernetes-Cluster, wie im folgenden Befehl gezeigt:
>
> ```azurecli
> az aks create -n contosoAKSCluster -g contosoResourceGroup --kubernetes-version 1.16.9 --node-count 1 --enable-managed-identity
> ```

1. [Legen Sie Ihre PATH-Umgebungsvariable](https://www.java.com/en/download/help/path.xml) auf die heruntergeladene Datei *kubectl.exe* fest.
2. Überprüfen Sie Ihre Kubernetes-Version, indem Sie den folgenden Befehl verwenden, der die Client- und Serverversion ausgibt. Die Clientversion ist die Datei *kubectl.exe*, die Sie installiert haben, und die Serverversion ist die Azure Kubernetes Services-Instanz (AKS), auf der Ihr Cluster ausgeführt wird.
    ```azurecli
    kubectl version
    ```
3. Stellen Sie sicher, dass Ihre Kubernetes-Version 1.1.16.0 oder höher ist. Stellen Sie bei Windows-Clustern sicher, dass Sie mindestens Kubernetes-Version 1.1.18.0 verwenden. Mit dem folgenden Befehl wird ein Upgrade sowohl für den Kubernetes-Cluster als auch für den Knotenpool durchgeführt. Die Ausführung des Befehls kann einige Minuten dauern. In diesem Beispiel ist die Ressourcengruppe *contosoResourceGroup*, und der Kubernetes-Cluster ist *contosoAKSCluster*.
    ```azurecli
    az aks upgrade --kubernetes-version 1.16.9 --name contosoAKSCluster --resource-group contosoResourceGroup
    ```
4. Um die Metadaten des von Ihnen erstellten AKS-Clusters anzuzeigen, verwenden Sie den folgenden Befehl. Kopieren Sie **principalId**, **clientId**, **subscriptionId** und **nodeResourceGroup** zur späteren Verwendung. Wenn bei der Erstellung des AKS-Clusters keine verwalteten Identitäten aktiviert wurden, sind **principalId** und **clientId** NULL. 

    ```azurecli
    az aks show --name contosoAKSCluster --resource-group contosoResourceGroup
    ```

    In der Ausgabe sind beide Parameter hervorgehoben:
    
    ![Screenshot der Azure CLI mit hervorgehobenen Werten von „principalId“ und „clientId“](../media/kubernetes-key-vault-2.png) ![Screenshot der Azure CLI mit hervorgehobenen Werten von „subscriptionId“ und „nodeResourceGroup“](../media/kubernetes-key-vault-3.png)
    
## <a name="install-helm-and-the-secrets-store-csi-driver"></a>Installieren von Helm und des Secrets Store CSI-Treibers
> [!NOTE]
> Die folgende Installation funktioniert nur in AKS unter Linux. Weitere Informationen zur Installation des Secrets Store CSI-Treibers finden Sie unter [Azure Key Vault-Anbieter für den Secrets Store CSI-Treiber](https://azure.github.io/secrets-store-csi-driver-provider-azure/getting-started/installation/). 

Sie müssen zunächst [Helm](https://helm.sh/docs/intro/install/) installieren, um den Secrets Store CSI-Treiber und den Azure Key Vault-Anbieter zu installieren.

Mit der [Secrets Store CSI](https://azure.github.io/secrets-store-csi-driver-provider-azure/)-Treiberschnittstelle können Sie die Geheimnisse abrufen, die in Ihrer Azure Key Vault-Instanz gespeichert sind. Anschließend können Sie die Treiberschnittstelle nutzen, um die Geheimnisinhalte in Kubernetes-Pods einzubinden.

1. Überprüfen Sie die Helm-Version, und stellen Sie sicher, dass es v3 oder höher ist:
    ```azurecli
    helm version
    ```
1. Installieren Sie den Secrets Store CSI-Treiber und den Azure Key Vault-Anbieter für den Treiber:
    ```azurecli
    helm repo add csi-secrets-store-provider-azure https://raw.githubusercontent.com/Azure/secrets-store-csi-driver-provider-azure/master/charts

    helm install csi-secrets-store-provider-azure/csi-secrets-store-provider-azure --generate-name
    ```
> [!NOTE] 
> Wenn Sie den Secrets Store CSI-Treiber und den Azure Key Vault-Anbieter auf Windows-Knoten verwenden möchten, aktivieren Sie den Treiber und den Anbieter auf mithilfe der [Helm-Konfigurationswerte](https://github.com/Azure/secrets-store-csi-driver-provider-azure/tree/master/charts/csi-secrets-store-provider-azure#configuration) auf den Windows-Knoten.

## <a name="create-an-azure-key-vault-and-set-your-secrets"></a>Erstellen einer Azure Key Vault-Instanz und Festlegen Ihrer Geheimnisse

Befolgen Sie zum Erstellen Ihres eigenen Schlüsseltresors und zum Festlegen Ihrer Geheimnisse die Anleitungen unter [Festlegen eines Geheimnisses und Abrufen des Geheimnisses aus Azure Key Vault mithilfe der Azure CLI](../secrets/quick-create-cli.md).

> [!NOTE] 
> Sie müssen weder Azure Cloud Shell verwenden noch eine neue Ressourcengruppe erstellen. Sie können die Ressourcengruppe verwenden, die Sie zuvor für den Kubernetes-Cluster erstellt haben.

## <a name="create-your-own-secretproviderclass-object"></a>Erstellen Ihres eigenen „SecretProviderClass“-Objekts

Um Ihr eigenes „SecretProviderClass“-Objekt mit anbieterspezifischen Parametern für den Secrets Store CSI-Treiber zu erstellen, [verwenden Sie diese Vorlage](https://raw.githubusercontent.com/Azure/secrets-store-csi-driver-provider-azure/master/examples/pod-identity/v1alpha1_secretproviderclass_pod_identity.yaml). Dieses Objekt ermöglicht den Identitätszugriff auf Ihren Schlüsseltresor.

Tragen Sie in der YAML-Beispieldatei „SecretProviderClass“ die fehlenden Parameter ein. Die folgenden Parameter sind erforderlich:

* **keyvaultName**: Der Name Ihres Schlüsseltresors.
* **objects:** Liste aller Geheimnisinhalte, die Sie einbinden möchten
    * **objectName**: Der Name des Geheimnisinhalts.
    * **objectType**: Der Objekttyp (Geheimnis, Schlüssel, Zertifikat).
* **tenantID**: Die Mandanten-ID oder Verzeichnis-ID Ihres Schlüsseltresors.

Die Dokumentation für alle erforderlichen Felder und unterstützten Konfigurationen finden Sie [hier](https://azure.github.io/secrets-store-csi-driver-provider-azure/getting-started/usage/#create-your-own-secretproviderclass-object).

Die aktualisierte Vorlage ist im folgenden Code dargestellt. Laden Sie sie als YAML-Datei herunter, und füllen Sie die erforderlichen Felder aus. In diesem Beispiel ist der Schlüsseltresor **contosoKeyVault5**. Er verfügt über zwei Geheimnisse, **secret1** und **secret2**.

> [!NOTE] 
> Wenn Sie Podidentitäten verwenden, legen Sie den Wert von **usePodIdentity** auf *true* fest, und legen Sie den Wert von **userAssignedIdentityID** auf ein Paar Anführungszeichen ( **""** ) fest. Wenn Sie verwaltete Identitäten verwenden, legen Sie den Wert **useVMManagedIdentity** als *true* und den Wert **userAssignedIdentityID** als clientID der benutzerseitig zugewiesenen Identität fest.

```yaml
apiVersion: secrets-store.csi.x-k8s.io/v1alpha1
kind: SecretProviderClass
metadata:
  name: azure-kvname
spec:
  provider: azure
  parameters:
    usePodIdentity: "false"                                                 # [OPTIONAL] if not provided, will default to "false". Set to "true" if using pod identities.
    useVMManagedIdentity: "false"                                           # [OPTIONAL] if not provided, will default to "false". Set to "true" if using managed identities.
    userAssignedIdentityID: "<clientID of user-assigned managed identity"   # [OPTIONAL] If you're using managed identities, use the client id to specify which user-assigned managed identity to use. If the value is empty, it defaults to use the system-assigned identity on the VM
    keyvaultName: "contosoKeyVault5"                                        # [REQUIRED] the name of the key vault
                                                                            #     az keyvault show --name contosoKeyVault5
                                                                            #     the preceding command will display the key vault metadata, which includes the subscription ID, resource group name, key vault 
    cloudName: ""                                                           # [OPTIONAL] if not provided, Azure environment will default to AzurePublicCloud
    objects:  |
      array:
        - |
          objectName: secret1                                               # [REQUIRED] object name
                                                                            #     az keyvault secret list --vault-name "contosoKeyVault5"
                                                                            #     the above command will display a list of secret names from your key vault
          objectType: secret                                                # [REQUIRED] object types: secret, key, or cert
          objectVersion: ""                                                 # [OPTIONAL] object versions, default to latest if empty
        - |
          objectName: secret2
          objectType: secret
          objectVersion: ""
    tenantId: "tenantID"                                                    # [REQUIRED] the tenant ID of the key vault
```
Die folgende Abbildung zeigt die Konsolenausgabe für **az keyvault show --name contosoKeyVault5** mit den relevanten hervorgehobenen Metadaten:

![Screenshot der Konsolenausgabe für „az keyvault show --name contosoKeyVault5“](../media/kubernetes-key-vault-4.png)

## <a name="install-azure-active-directory-azure-ad-pod-identity"></a>Installieren der Podidentität für Azure Active Directory (Azure AD)

1. Weisen Sie dem AKS-Cluster, den Sie erstellt haben, bestimmte Rollen zu. 

    Die Dokumentation für alle erforderlichen Rollenzuweisungen mit der Podidentität für Azure Active Directory (Azure AD) ist [hier](https://azure.github.io/aad-pod-identity/docs/getting-started/role-assignment/) verfügbar.

    ```azurecli
    RESOURCE_GROUP=contosoResourceGroup
    
    az role assignment create --role "Managed Identity Operator" --assignee $clientId --scope /subscriptions/<SUBID>/resourcegroups/$RESOURCE_GROUP
    
    az role assignment create --role "Virtual Machine Contributor" --assignee $clientId --scope /subscriptions/<SUBID>/resourcegroups/$RESOURCE_GROUP
    ```

2. Installieren Sie die Azure AD-Identität (Azure Active Directory) in AKS.

    > [!NOTE] 
    > Wenn Sie einen AKS-Cluster mit dem Netzwerk-Plug-In von kubenet verwenden, lesen Sie diese [Dokumentation](https://azure.github.io/aad-pod-identity/docs/configure/aad_pod_identity_on_kubenet/), und erfahren Sie, wie eine Podidentität im Cluster bereitgestellt wird.

    ```azurecli
    helm repo add aad-pod-identity https://raw.githubusercontent.com/Azure/aad-pod-identity/master/charts

    helm install pod-identity aad-pod-identity/aad-pod-identity
    ```

3. Erstellen Sie eine benutzerseitig zugewiesene verwaltete Identität. Kopieren Sie in der Ausgabe **clientid** für die spätere Verwendung.
    ```azurecli
    az identity create -g $resourceGroupName -n $identityName
    ```

4. Erteilen Sie für die Identität anschließend die Berechtigung zum Abrufen von Geheimnissen von Ihrer Key Vault-Instanz. Verwenden Sie die **clientid** aus der benutzerseitig zugewiesenen verwalteten Identität.
    ```azurecli
    az keyvault set-policy -n contosoKeyVault5 --secret-permissions get --spn $clientId
    az keyvault set-policy -n contosoKeyVault5 --key-permissions get --spn $clientId
    az keyvault set-policy -n contosoKeyVault5 --certificate-permissions get --spn $clientId
    ```

## <a name="deploy-your-pod-with-mounted-secrets-from-your-key-vault"></a>Bereitstellen Ihres Pods mit eingebundenen Geheimnissen aus Ihrem Schlüsseltresor

Um Ihr „SecretProviderClass“-Objekt zu konfigurieren, führen Sie den folgenden Befehl aus:
```azurecli
kubectl apply -f secretProviderClass.yaml
```

### <a name="use-managed-identities"></a>Verwenden verwalteter Identitäten

Wenn Sie Podidentitäten verwenden, erstellen Sie eine *AzureIdentity* in Ihrem Cluster, die auf die zuvor von Ihnen erstellte Identität verweist. Erstellen Sie dann eine *AzureIdentityBinding*, die auf die von Ihnen erstellte AzureIdentity verweist. Tragen Sie die Parameter in der folgenden Vorlage ein, und speichern Sie diese dann als *podIdentityAndBinding.yaml*.  

```yml
apiVersion: aadpodidentity.k8s.io/v1
kind: AzureIdentity
metadata:
    name: azureIdentityName                 # The name of your Azure identity
spec:
    type: 0                                 # Set type: 0 for managed identity
    resourceID: /subscriptions/<SUBID>/resourcegroups/<RESOURCEGROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<AZUREIDENTITYNAME>
    clientID: "<managed identity clientID>"   # The clientId of the User-assigned managed identity that you created earlier
---
apiVersion: aadpodidentity.k8s.io/v1
kind: AzureIdentityBinding
metadata:
    name: azure-pod-identity-binding
spec:
    azureIdentity: "azureIdentityName"      # The name of your Azure identity
    selector: azure-pod-identity-binding-selector
```
    
Führen Sie den folgenden Befehl aus, um die Bindung zu konfigurieren:

```azurecli
kubectl apply -f podIdentityAndBinding.yaml
```

Als Nächstes stellen Sie den Pod bereit. Der folgende Code ist die YAML-Datei für die Bereitstellung, die die Bindung der Pod-Identität aus dem vorangegangenen Schritt nutzt. Speichern Sie diese Datei als *podBindingDeployment.yaml*.

```yml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-secrets-store-inline
  labels:
    aadpodidbinding: azure-pod-identity-binding-selector # The selector defined in AzureIdentityBinding in the previous step
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

### <a name="check-the-pod-status-and-secret-content"></a>Überprüfen des Pod-Status und des Geheimnisinhalts 

Um die von Ihnen bereitgestellten Pods anzuzeigen, führen Sie folgenden Befehl aus:
```azurecli
kubectl get pods
```

Um den Status ihres Pods zu überprüfen, führen Sie den folgenden Befehl aus:
```azurecli
kubectl describe pod/nginx-secrets-store-inline
```

![Screenshot der Azure CLI-Ausgabe, in dem der Zustand des Pods als „Wird ausgeführt“ und alle Ereignisse als „Normal“ angezeigt werden. ](../media/kubernetes-key-vault-6.png)

Im Ausgabefenster sollte sich der bereitgestellte Pod im Zustand *Wird ausgeführt* befinden. Unten im Abschnitt **Ereignisse** sind alle Ereignistypen als *Normal* angezeigt.

Nachdem Sie sich vergewissert haben, dass der Pod ausgeführt wird, können Sie überprüfen, ob der Pod die Geheimnisse aus Ihrem Schlüsseltresor enthält.

Um alle im Pod enthaltenen Geheimnisse anzuzeigen, führen Sie den folgenden Befehl aus:
```azurecli
kubectl exec nginx-secrets-store-inline -- ls /mnt/secrets-store/
```

Um den Inhalt eines bestimmten Geheimnisses anzuzeigen, führen Sie den folgenden Befehl aus:
```azurecli
kubectl exec nginx-secrets-store-inline -- cat /mnt/secrets-store/secret1
```

Überprüfen Sie, ob der Inhalt des Geheimnisses angezeigt wird.

## <a name="next-steps"></a>Nächste Schritte

Um sicherzustellen, dass Ihr Schlüsseltresor wiederherstellbar ist:
> [!div class="nextstepaction"]
> [Aktivieren des vorläufigen Löschens](./key-vault-recovery.md)
