---
title: Azure RBAC für Kubernetes-Cluster mit Azure Arc-Unterstützung
services: azure-arc
ms.service: azure-arc
ms.date: 04/05/2021
ms.topic: article
author: shashankbarsin
ms.author: shasb
description: Verwenden von Azure RBAC für Autorisierungsüberprüfungen in Kubernetes-Clustern mit Azure Arc-Unterstützung
ms.openlocfilehash: f0275e1516e8487b5a00fb08c885b09b6df1684c
ms.sourcegitcommit: 4a54c268400b4158b78bb1d37235b79409cb5816
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108145697"
---
# <a name="integrate-azure-active-directory-with-azure-arc-enabled-kubernetes-clusters"></a>Integrieren von Azure Active Directory in Azure Arc-fähige Kubernetes-Cluster

Die Kubernetes-Objekttypen [„ClusterRoleBinding“ und „RoleBinding“](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#rolebinding-and-clusterrolebinding) bieten Unterstützung bei der nativen Definition der Autorisierung in Kubernetes. Mithilfe dieses Features können Sie unter Verwendung von Azure Active Directory und Rollenzuweisungen in Azure Autorisierungsüberprüfungen für den Cluster steuern. Dies impliziert, dass Sie ab sofort mithilfe von Azure-Rollenzuweisungen differenziert steuern können, wer Ihre Kubernetes-Objekte – z. B. Bereitstellungen, Pods und Dienste – lesen, schreiben und löschen kann.

Eine konzeptionelle Übersicht zu diesem Feature finden Sie im Artikel [Azure RBAC: Kubernetes mit Azure Arc-Unterstützung](conceptual-azure-rbac.md).

[!INCLUDE [preview features note](./includes/preview/preview-callout.md)]

## <a name="prerequisites"></a>Voraussetzungen

- [Installieren oder aktualisieren Sie Azure CLI](/cli/azure/install-azure-cli) auf eine Version >= 2.16.0

- Installieren Sie die Azure CLI-Erweiterung `connectedk8s` in Version >= 1.1.0:

    ```azurecli
    az extension add --name connectedk8s
    ```
    
    Wenn die Erweiterung `connectedk8s` bereits installiert ist, können Sie sie mit dem folgenden Befehl auf die neueste Version aktualisieren: 

    ```azurecli
    az extension update --name connectedk8s
    ```

- Ein vorhandener Cluster, der mit Kubernetes mit Azure Arc-Aktivierung verbunden ist.
    - Wenn Sie noch keine Verbindung mit einem Cluster hergestellt haben, verwenden Sie unseren [Schnellstart](quickstart-connect-cluster.md).
    - [Aktualisieren Sie Ihre Agents](agent-upgrade.md#manually-upgrade-agents) auf Version >= 1.1.0.

> [!NOTE]
> Dieses Feature kann für Managed Kubernetes-Angebote von Cloudanbietern wie Elastic Kubernetes Service oder Google Kubernetes Engine eingerichtet werden, bei denen der Benutzer keinen Zugriff auf den `apiserver` des Clusters hat. Für Azure Kubernetes Service-Cluster (AKS) ist dieses [Feature nativ verfügbar](../../aks/manage-azure-rbac.md) und erfordert nicht, dass der AKS-Cluster mit Azure Arc verbunden ist.

## <a name="set-up-azure-ad-applications"></a>Einrichten von Azure AD-Anwendungen

### <a name="create-server-application"></a>Erstellen einer Serveranwendung

1. Erstellen Sie eine neue Azure AD-Anwendung, und rufen Sie den zugehörigen `appId`-Wert ab, der in den späteren Schritten als `serverApplicationId` verwendet wird:

    ```azurecli
    az ad app create --display-name "<clusterName>Server" --identifier-uris "https://<clusterName>Server" --query appId -o tsv
    ```

1. Aktualisieren Sie die Anwendungsgruppen-Mitgliedschaftsansprüche:

    ```azurecli
    az ad app update --id <serverApplicationId> --set groupMembershipClaims=All
    ```

1. Erstellen Sie einen Dienstprinzipal, und rufen Sie den zugehörigen `password`-Feldwert ab, der später als `serverApplicationSecret` benötigt wird, wenn dieses Feature für den Cluster aktiviert wird:

    ```azurecli
    az ad sp create --id <serverApplicationId>
    az ad sp credential reset --name <serverApplicationId> --credential-description "ArcSecret" --query password -o tsv
    ```

1. Gewähren Sie der Anwendung API-Berechtigungen:

    ```azurecli
    az ad app permission add --id <serverApplicationId> --api 00000003-0000-0000-c000-000000000000 --api-permissions e1fe6dd8-ba31-4d61-89e7-88639da4683d=Scope
    az ad app permission grant --id <serverApplicationId> --api 00000003-0000-0000-c000-000000000000
    ```

    > [!NOTE]
    > * Dieser Schritt muss von einem Azure-Mandantenadministrator ausgeführt werden.
    > * Zur Verwendung dieses Features in der Produktionsumgebung wird empfohlen, für jeden Cluster eine andere Serveranwendung zu erstellen.

### <a name="create-client-application"></a>Erstellen einer Clientanwendung

1. Erstellen Sie eine neue Azure AD-Anwendung, und rufen Sie den zugehörigen appId-Wert ab, der in den späteren Schritten als `clientApplicationId` verwendet wird:

    ```azurecli
    az ad app create --display-name "<clusterName>Client" --native-app --reply-urls "https://<clusterName>Client" --query appId -o tsv
    ```

2. Erstellen Sie einen Dienstprinzipal für diese Clientanwendung:

    ```azurecli
    az ad sp create --id <clientApplicationId>
    ```

3. Rufen Sie die `oAuthPermissionId` für die Serveranwendung ab:

    ```azurecli
    az ad app show --id <serverApplicationId> --query "oauth2Permissions[0].id" -o tsv
    ```

4. Gewähren Sie der Clientanwendung die benötigten Berechtigungen:

    ```azurecli
    az ad app permission add --id <clientApplicationId> --api <serverApplicationId> --api-permissions <oAuthPermissionId>=Scope
    az ad app permission grant --id <clientApplicationId> --api <serverApplicationId>
    ```

## <a name="create-a-role-assignment-for-the-server-application"></a>Erstellen einer Rollenzuweisung für die Serveranwendung

Die Serveranwendung benötigt die Berechtigungen `Microsoft.Authorization/*/read`, um zu überprüfen, ob der Benutzer, der die Anforderung sendet, für die in der Anforderung enthaltenen Kubernetes-Objekte autorisiert ist.

1. Erstellen Sie eine Datei namens „accessCheck.json“ mit den folgenden Inhalten:

    ```json
    {
      "Name": "Read authorization",
      "IsCustom": true,
      "Description": "Read authorization",
      "Actions": ["Microsoft.Authorization/*/read"],
      "NotActions": [],
      "DataActions": [],
      "NotDataActions": [],
      "AssignableScopes": [
        "/subscriptions/<subscription-id>"
      ]
    }
    ```

    Ersetzen Sie `<subscription-id>` durch die tatsächliche Abonnement-ID.

2. Führen Sie den folgenden Befehl aus, um die neue benutzerdefinierte Rolle zu erstellen:

    ```azurecli
    az role definition create --role-definition ./accessCheck.json
    ```

3. Speichern Sie den Wert des Felds `id` aus der Ausgabe des obigen Befehls. Dieser Wert wird in den späteren Schritten als `roleId` verwendet.

4. Erstellen Sie unter Verwendung der oben erstellten Rolle eine Rollenzuweisung mit der Serveranwendung als zugewiesener Person:

    ```azurecli
    az role assignment create --role <roleId> --assignee <serverApplicationId> --scope /subscriptions/<subscription-id>
    ```

## <a name="enable-azure-rbac-on-cluster"></a>Aktivieren von Azure RBAC für den Cluster

1. Aktivieren Sie Azure RBAC für Ihren Kubernetes-Cluster mit Arc-Unterstützung, indem Sie den folgenden Befehl ausführen:

    ```console
    az connectedk8s enable-features -n <clusterName> -g <resourceGroupName> --features azure-rbac --app-id <serverApplicationId> --app-secret <serverApplicationSecret>
    ```
    
    > [!NOTE]
    > 1. Stellen Sie vor der Ausführung des obigen Befehls sicher, dass die Datei `kubeconfig` auf dem Computer auf den Cluster zeigt, für den Sie Azure RBAC aktivieren möchten.
    > 2. Verwenden Sie `--skip-azure-rbac-list` mit dem vorstehenden Befehl, um eine durch Komma getrennte Liste der Benutzernamen/E-Mail-Adressen/OIDs anzugeben, für die anstelle einer Verwendung von Azure RBAC Autorisierungsprüfungen unter Verwendung der nativen Kubernetes-Objekte „ClusterRoleBinding“ und „RoleBinding“ durchgeführt werden.

### <a name="for-a-generic-cluster-where-there-is-no-reconciler-running-on-apiserver-specification"></a>Für einen generischen Cluster ohne Abstimmung gemäß apiserver-Spezifikation

1. Stellen Sie über SSH eine Verbindung mit jedem Masterknoten des Clusters her, und führen Sie die folgenden Schritte aus:

    1. Öffnen Sie das Manifest `apiserver` im Bearbeitungsmodus:
        
        ```console
        sudo vi /etc/kubernetes/manifests/kube-apiserver.yaml
        ```

    1. Fügen Sie unter `volumes` die folgende Spezifikation hinzu:
        
        ```yml
        - name: azure-rbac
          secret:
            secretName: azure-arc-guard-manifests
        ```

    1. Fügen Sie unter `volumeMounts` die folgende Spezifikation hinzu:

        ```yml
        - mountPath: /etc/guard
          name: azure-rbac
          readOnly: true
        ```

    1. Fügen Sie die folgenden `apiserver`-Argumente hinzu:

        ```yml
        - --authentication-token-webhook-config-file=/etc/guard/guard-authn-webhook.yaml
        - --authentication-token-webhook-cache-ttl=5m0s
        - --authorization-webhook-cache-authorized-ttl=5m0s
        - --authorization-webhook-config-file=/etc/guard/guard-authz-webhook.yaml
        - --authorization-webhook-version=v1
        - --authorization-mode=Node,Webhook,RBAC
        ```
    
        Wenn der Kubernetes-Cluster Version 1.19.0 oder höher verwendet, muss auch das folgende `apiserver argument` festgelegt werden:

        ```yml
        - --authentication-token-webhook-version=v1
        ```

    1. Speichern Sie Ihre Änderungen, und beenden Sie den Editor, um den `apiserver`-Pod zu aktualisieren.


### <a name="for-a-cluster-created-using-cluster-api"></a>Für einen mithilfe der Cluster-API erstellten Cluster

1. Kopieren Sie das guard-Geheimnis mit den Konfigurationsdateien für Authentifizierungs- und Autorisierungswebhooks `from the workload cluster` auf Ihren Computer:

    ```console
    kubectl get secret azure-arc-guard-manifests -n kube-system -o yaml > azure-arc-guard-manifests.yaml
    ```

1. Ändern Sie das Feld `namespace` in der Datei `azure-arc-guard-manifests.yaml` in den Namespace innerhalb des Verwaltungsclusters, in dem Sie die benutzerdefinierten Ressourcen zum Erstellen von Workloadclustern anwenden.

1. Wenden Sie dieses Manifest an:

    ```console
    kubectl apply -f azure-arc-guard-manifests.yaml
    ```

1. Bearbeiten Sie das Objekt `KubeadmControlPlane` durch Ausführung von `kubectl edit kcp <clustername>-control-plane`:
    
    1. Fügen Sie unter `files:` den folgenden Codeausschnitt hinzu:
    
        ```console
        - contentFrom:
            secret:
              key: guard-authn-webhook.yaml
              name: azure-arc-guard-manifests
          owner: root:root
          path: /etc/kubernetes/guard-authn-webhook.yaml
          permissions: "0644"
        - contentFrom:
            secret:
              key: guard-authz-webhook.yaml
              name: azure-arc-guard-manifests
          owner: root:root
          path: /etc/kubernetes/guard-authz-webhook.yaml
          permissions: "0644"
        ```

    1. Fügen Sie unter `apiServer:` -> `extraVolumes:` den folgenden Codeausschnitt hinzu:
    
        ```console
        - hostPath: /etc/kubernetes/guard-authn-webhook.yaml
            mountPath: /etc/guard/guard-authn-webhook.yaml
            name: guard-authn
            readOnly: true
        - hostPath: /etc/kubernetes/guard-authz-webhook.yaml
            mountPath: /etc/guard/guard-authz-webhook.yaml
            name: guard-authz
            readOnly: true
        ```

    1. Fügen Sie unter `apiServer:` -> `extraArgs:` den folgenden Codeausschnitt hinzu:
    
        ```console
        authentication-token-webhook-cache-ttl: 5m0s
        authentication-token-webhook-config-file: /etc/guard/guard-authn-webhook.yaml
        authentication-token-webhook-version: v1
        authorization-mode: Node,Webhook,RBAC
        authorization-webhook-cache-authorized-ttl: 5m0s
        authorization-webhook-config-file: /etc/guard/guard-authz-webhook.yaml
        authorization-webhook-version: v1
        ```

    1. Speichern und beenden Sie, um das Objekt `KubeadmControlPlane` zu aktualisieren. Warten Sie, bis diese Änderungen für den Workloadcluster übernommen wurden.


## <a name="create-role-assignments-for-users-to-access-the-cluster"></a>Erstellen von Rollenzuweisungen für Benutzer zum Zugriff auf den Cluster

Benutzer der Kubernetes-Ressource mit Azure Arc-Unterstützung können entweder integrierte Rollen oder benutzerdefinierte Rollen verwenden, um Benutzern Zugriff auf den Kubernetes-Cluster zu gewähren.

### <a name="built-in-roles"></a>Integrierte Rollen

| Role | BESCHREIBUNG |
|---|---|
| [Anzeigeberechtigter für Azure Arc Kubernetes](../../role-based-access-control/built-in-roles.md#azure-arc-kubernetes-viewer) | Ermöglicht schreibgeschützten Zugriff, um die meisten Objekte in einem Namespace anzuzeigen. Mit dieser Rolle können keine Geheimnisse angezeigt werden. Dies liegt daran, dass durch eine `read`-Berechtigung für Geheimnisse Zugriff auf `ServiceAccount`-Anmeldeinformationen im Namespace gewährt würde, wodurch wiederum API-Zugriff über dieses `ServiceAccount` erteilt würde (eine Form der Rechteausweitung). |
| [Schreibberechtigter für Azure Arc Kubernetes](../../role-based-access-control/built-in-roles.md#azure-arc-kubernetes-writer) | Ermöglicht Lese-/Schreibzugriff auf die meisten Objekte in einem Namespace. Diese Rolle lässt das Anzeigen oder Ändern von Rollen oder Rollenbindungen nicht zu. Diese Rolle ermöglicht jedoch den Zugriff auf Geheimnisse und das Ausführen von Pods als beliebiges `ServiceAccount` im Namespace, sodass sie verwendet werden kann, um die API-Zugriffsebenen für ein beliebiges `ServiceAccount` im Namespace zu erhalten. |
| [Azure Arc Kubernetes-Administrator](../../role-based-access-control/built-in-roles.md#azure-arc-kubernetes-admin) | Mit dieser Rolle wird Administratorzugriff gewährt. Die Zuweisung ist unter Verwendung von „RoleBinding“ innerhalb eines Namespace vorgesehen. Bei Verwendung in „RoleBinding“ wird Lese-/Schreibzugriff auf die meisten Ressourcen in einem Namespace gewährt, einschließlich der Möglichkeit zum Erstellen von Rollen und Rollenbindungen innerhalb des Namespace. Diese Rolle lässt keinen Schreibzugriff auf das Ressourcenkontingent oder den Namespace selbst zu. |
| [Azure Arc Kubernetes-Clusteradministrator](../../role-based-access-control/built-in-roles.md#azure-arc-kubernetes-cluster-admin) | Ermöglicht Superuserzugriff, um beliebige Aktionen für beliebige Ressourcen auszuführen. Bei Verwendung in „ClusterRoleBinding“ wird über diese Rolle Vollzugriff auf jede Ressource im Cluster und in allen Namespaces gewährt. Bei Verwendung in „RoleBinding“ wird über diese Rolle Vollzugriff auf jede Ressource im Namespace der Rollenbindung gewährt, einschließlich des Namespace selbst.|

Sie können auf den Kubernetes-Cluster mit Arc-Unterstützung beschränkte Rollenzuweisungen im Azure-Portal auf dem Blatt `Access Control (IAM)` der Clusterressource erstellen. Sie können auch Azure CLI-Befehle verwenden, wie nachstehend gezeigt:

```azurecli
az role assignment create --role "Azure Arc Kubernetes Cluster Admin" --assignee <AZURE-AD-ENTITY-ID> --scope $ARM_ID
```

Dabei kann `AZURE-AD-ENTITY-ID` ein Benutzername (z. B. testuser@mytenant.onmicrosoft.com) oder sogar die `appId` eines Dienstprinzipals sein.

Hier sehen Sie ein weiteres Beispiel für die Erstellung einer Rollenzuweisung, die auf einen bestimmten Namespace innerhalb des Clusters beschränkt ist:

```azurecli
az role assignment create --role "Azure Arc Kubernetes Viewer" --assignee <AZURE-AD-ENTITY-ID> --scope $ARM_ID/namespaces/<namespace-name>
```

> [!NOTE]
> Rollenzuweisungen, die auf den Cluster beschränkt sind, können entweder mithilfe des Azure-Portals oder unter Verwendung der CLI erstellt werden. Rollenzuweisungen, die auf Namespaces beschränkt sind, können nur über die CLI erstellt werden.

### <a name="custom-roles"></a>Benutzerdefinierte Rollen

Sie können eine eigene Rollendefinition für die Verwendung in Rollenzuweisungen erstellen.

Die nachstehende exemplarische Vorgehensweise zeigt eine Rollendefinition, durch die ein Benutzer Bereitstellungen nur lesen kann. Weitere Informationen finden Sie in der [vollständigen Liste der Datenaktionen, die zum Erstellen einer Rollendefinition verwendet werden können](../../role-based-access-control/resource-provider-operations.md#microsoftkubernetes).

Kopieren Sie das nachstehende JSON-Objekt in eine Datei mit dem Namen „custom-role.json“. Ersetzen Sie den Platzhalter `<subscription-id>` durch die tatsächliche Abonnement-ID. Die nachstehende benutzerdefinierte Rolle verwendet eine der Datenaktionen und ermöglicht Ihnen die Anzeige aller Bereitstellungen in dem Bereich (Cluster/Namespace), in dem die Rollenzuweisung erstellt wird.

```json
{
    "Name": "Arc Deployment Viewer",
    "Description": "Lets you view all deployments in cluster/namespace.",
    "Actions": [],
    "NotActions": [],
    "DataActions": [
        "Microsoft.Kubernetes/connectedClusters/apps/deployments/read"
    ],
    "NotDataActions": [],
    "assignableScopes": [
        "/subscriptions/<subscription-id>"
    ]
}
```

1. Erstellen Sie die Rollendefinition, indem Sie den folgenden Befehl aus dem Ordner ausführen, in dem Sie `custom-role.json` gespeichert haben:

    ```bash
    az role definition create --role-definition @custom-role.json
    ```

1. Erstellen Sie eine Rollenzuweisung unter Verwendung dieser benutzerdefinierten Rollendefinition:

    ```bash
    az role assignment create --role "Arc Deployment Viewer" --assignee <AZURE-AD-ENTITY-ID> --scope $ARM_ID/namespaces/<namespace-name>
    ```

## <a name="configure-kubectl-with-user-credentials"></a>Konfigurieren von kubectl mit Benutzeranmeldeinformationen

Es gibt zwei Möglichkeiten zum Abruf der Datei `kubeconfig`, die zum Zugriff auf den Cluster benötigt wird:
1. Verwenden Sie das Feature [Cluster Connect](cluster-connect.md) (`az connectedk8s proxy`) des Kubernetes-Clusters mit Azure Arc-Unterstützung.
1. Der Clusteradministrator gibt die Datei `kubeconfig` für jeden anderen Benutzer frei.

### <a name="if-you-are-accessing-cluster-using-cluster-connect-feature"></a>Bei Clusterzugriff über das Feature für die Clusterverbindung

Führen Sie den folgenden Befehl aus, um den Proxyprozess zu starten:

```console
az connectedk8s proxy -n <clusterName> -g <resourceGroupName>
```

Sobald der Proxyprozess ausgeführt wird, können Sie eine weitere Registerkarte in Ihrer Konsole öffnen, um [Anforderungen an den Cluster zu senden](#sending-requests-to-cluster).

### <a name="if-cluster-admin-shared-the-kubeconfig-file-with-you"></a>Bei Freigabe der Datei `kubeconfig` durch den Clusteradministrator 

1. Führen Sie den folgenden Befehl aus, um Anmeldeinformationen für den Benutzer festzulegen:

    ```console
    kubectl config set-credentials <testuser>@<mytenant.onmicrosoft.com> \
    --auth-provider=azure \
    --auth-provider-arg=environment=AzurePublicCloud \
    --auth-provider-arg=client-id=<clientApplicationId> \
    --auth-provider-arg=tenant-id=<tenantId> \
    --auth-provider-arg=apiserver-id=<serverApplicationId>
    ```

1. Öffnen Sie die zuvor erstellte Datei `kubeconfig`. Vergewissern Sie sich, dass unter `contexts` der dem Cluster zugeordnete Kontext auf die im vorherigen Schritt erstellten Benutzeranmeldeinformationen zeigt.

1. Fügen Sie unterhalb der Benutzerkonfiguration die Einstellung **config-mode** hinzu:
  
    ```console
    name: testuser@mytenant.onmicrosoft.com
    user:
        auth-provider:
        config:
            apiserver-id: $SERVER_APP_ID
            client-id: $CLIENT_APP_ID
            environment: AzurePublicCloud
            tenant-id: $TENANT_ID
            config-mode: "1"
        name: azure
    ```

## <a name="sending-requests-to-cluster"></a>Senden von Anforderungen an den Cluster

1. Führen Sie einen beliebigen `kubectl`-Befehl aus. Beispiel:
  * `kubectl get nodes` 
  * `kubectl get pods`

1. Wenn Sie zur Durchführung einer browserbasierten Authentifizierung aufgefordert werden, kopieren Sie die URL `https://microsoft.com/devicelogin` für die Geräteanmeldung, und öffnen Sie diese in Ihrem Webbrowser.

1. Geben Sie den in der Konsole angezeigten Code ein. Kopieren Sie den Code, und fügen Sie ihn im Terminal an der Eingabeaufforderung für die Geräteauthentifizierung ein.

1. Geben Sie den Benutzernamen (testuser@mytenant.onmicrosoft.com) und das zugehörige Kennwort ein.

1. Falls eine Fehlermeldung wie die folgende angezeigt wird, sind Sie nicht zum Zugriff auf die angeforderte Ressource autorisiert:

    ```console
    Error from server (Forbidden): nodes is forbidden: User "testuser@mytenant.onmicrosoft.com" cannot list resource "nodes" in API group "" at the cluster scope: User doesn't have access to the resource in Azure. Update role assignment to allow access.
    ```

    Ein Administrator muss eine neue Rollenzuweisung erstellen, die diesen Benutzer zum Zugriff auf die Ressource berechtigt.


## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> Sicheres Herstellen einer Verbindung mit dem Cluster über [Cluster Connect](cluster-connect.md)