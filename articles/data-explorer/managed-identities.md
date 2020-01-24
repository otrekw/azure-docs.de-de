---
title: Konfigurieren verwalteter Identitäten für Azure Data Explorer-Cluster
description: Hier erfahren Sie, wie Sie verwaltete Identitäten für Azure Data Explorer-Cluster konfigurieren.
author: saguiitay
ms.author: itsagui
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 01/06/2020
ms.openlocfilehash: 3b0bb62de4a96a45d607e05b32a87feec692e4d4
ms.sourcegitcommit: 02160a2c64a5b8cb2fb661a087db5c2b4815ec04
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/07/2020
ms.locfileid: "75722203"
---
# <a name="configure-managed-identities-for-your-azure-data-explorer-cluster"></a>Konfigurieren verwalteter Identitäten für Ihren Azure Data Explorer-Cluster

Durch eine [verwaltete Entität aus Azure Active Directory](/azure/active-directory/managed-identities-azure-resources/overview) kann Ihr Cluster problemlos auf andere durch AAD geschützte Ressourcen wie Azure Key Vault zugreifen. Da die Identität von der Azure-Plattform verwaltet wird, müssen Sie keine Geheimnisse bereitstellen oder rotieren. In diesem Artikel erfahren Sie, wie Sie eine verwaltete Identität für Azure Data Explorer-Cluster erstellen. 

> [!Note]
> Verwaltete Identitäten für Azure Data Explorer verhalten sich nicht wie erwartet, wenn Ihre App abonnement- oder mandantenübergreifend migriert wird. Die App benötigt in diesem Fall eine neue Identität. Zu diesem Zweck kann das Feature wie unter [Entfernen einer Identität](#remove-an-identity) beschrieben deaktiviert und anschließend erneut aktiviert werden. Darüber hinaus müssen auch Zugriffsrichtlinien nachgeschalteter Ressourcen aktualisiert werden, um die neue Identität zu verwenden.

## <a name="add-a-system-assigned-identity"></a>Hinzufügen einer systemseitig zugewiesenen Identität

Ihrem Cluster kann eine **systemseitig zugewiesene Identität** zugewiesen werden. Diese ist an Ihren Cluster gebunden und wird gelöscht, wenn der Cluster gelöscht wird. Ein Cluster kann nur über eine einzelne systemseitig zugewiesene Identität verfügen. Für die Erstellung eines Clusters mit einer systemseitig zugewiesenen Identität muss eine zusätzliche Eigenschaft für den Cluster festgelegt werden.

### <a name="add-a-system-assigned-identity-using-c"></a>Hinzufügen einer systemseitig zugewiesenen Identität unter Verwendung von C#

Gehen Sie wie folgt vor, um eine verwaltete Identität mithilfe des Azure Data Explorer-C#-Clients einzurichten:

* Installieren Sie das [Azure Data Explorer-NuGet-Paket (Kusto)](https://www.nuget.org/packages/Microsoft.Azure.Management.Kusto/).
* Installieren Sie das [NuGet-Paket Microsoft.IdentityModel.Clients.ActiveDirectory](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) für die Authentifizierung.
* [Erstellen Sie eine Azure AD-Anwendung](/azure/active-directory/develop/howto-create-service-principal-portal) und einen Dienstprinzipal, der auf Ressourcen zugreifen kann, um das folgende Beispiel ausführen zu können. Sie können eine Rollenzuweisung im Abonnementbereich hinzufügen und die erforderlichen Werte für `Directory (tenant) ID`, `Application ID` und `Client Secret` abrufen.

#### <a name="create-or-update-your-cluster"></a>Erstellen oder Aktualisieren Ihres Clusters

1. Erstellen oder aktualisieren Sie Ihren Cluster mithilfe der Eigenschaft `Identity`:

    ```csharp
    var tenantId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";//Directory (tenant) ID
    var clientId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";//Application ID
    var clientSecret = "xxxxxxxxxxxxxx";//Client Secret
    var subscriptionId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";
    var authenticationContext = new AuthenticationContext($"https://login.windows.net/{tenantId}");
    var credential = new ClientCredential(clientId, clientSecret);
    var result = await authenticationContext.AcquireTokenAsync(resource: "https://management.core.windows.net/", clientCredential: credential);
    
    var credentials = new TokenCredentials(result.AccessToken, result.AccessTokenType);
    
    var kustoManagementClient = new KustoManagementClient(credentials)
    {
        SubscriptionId = subscriptionId
    };
    
    var resourceGroupName = "testrg";
    var clusterName = "mykustocluster";
    var location = "Central US";
    var skuName = "Standard_D13_v2";
    var tier = "Standard";
    var capacity = 5;
    var sku = new AzureSku(skuName, tier, capacity);
    var identity = new Identity(IdentityType.SystemAssigned);
    var cluster = new Cluster(location, sku, identity: identity);
    await kustoManagementClient.Clusters.CreateOrUpdateAsync(resourceGroupName, clusterName, cluster);
    ```
    
2. Führen Sie den folgenden Befehl aus, um zu überprüfen, ob Ihr Cluster erfolgreich mit einer Identität erstellt oder aktualisiert wurde:

    ```csharp
    kustoManagementClient.Clusters.Get(resourceGroupName, clusterName);
    ```

    Wenn das Ergebnis `ProvisioningState` mit dem Wert `Succeeded` enthält, wurde der Cluster erstellt oder aktualisiert und sollte über folgende Eigenschaften verfügen:
   
    ```csharp
    var principalId = cluster.Identity.PrincipalId;
    var tenantId = cluster.Identity.TenantId;
    ```

    `PrincipalId` und `TenantId` werden durch GUIDs ersetzt. Die Eigenschaft `TenantId` gibt den AAD-Mandanten an, zu dem die Identität gehört. `PrincipalId` ist ein eindeutiger Bezeichner für die neue Identität des Clusters. In AAD weist der Dienstprinzipal denselben Namen auf, den Sie für Ihre App Service- oder Azure Functions-Instanz vergeben haben.

### <a name="add-a-system-assigned-identity-using-an-azure-resource-manager-template"></a>Hinzufügen einer systemseitig zugewiesenen Identität unter Verwendung einer Azure Resource Manager-Vorlage

Mithilfe einer Azure Resource Manager-Vorlage kann die Bereitstellung Ihrer Azure-Ressourcen automatisiert werden. Weitere Informationen zum Bereitstellen in Azure Data Explorer finden Sie unter [Erstellen eines Azure Data Explorer-Clusters und einer Datenbank mithilfe einer Azure Resource Manager-Vorlage](create-cluster-database-resource-manager.md).

Durch Hinzufügen des systemseitig zugewiesenen Typs wird Azure angewiesen, die Identität für Ihrem Cluster zu erstellen und zu verwalten. Ressourcen vom Typ `Microsoft.Kusto/clusters` können mit einer Identität erstellt werden, indem die folgende Eigenschaft in der Ressourcendefinition eingeschlossen wird: 

```json
"identity": {
    "type": "SystemAssigned"
}    
```

Beispiel:

```json
{
    "apiVersion": "2019-09-07",
    "type": "Microsoft.Kusto/clusters",
    "name": "[variables('clusterName')]",
    "location": "[resourceGroup().location]",
    "identity": {
        "type": "SystemAssigned"
    },
    "properties": {
        "trustedExternalTenants": [],
        "virtualNetworkConfiguration": null,
        "optimizedAutoscale": null,
        "enableDiskEncryption": false,
        "enableStreamingIngest": false,
    }
}
```

Wenn der Cluster erstellt wird, verfügt er über folgende zusätzliche Eigenschaften:

```json
"identity": {
    "type": "SystemAssigned",
    "tenantId": "<TENANTID>",
    "principalId": "<PRINCIPALID>"
}
```

`<TENANTID>` und `<PRINCIPALID>` werden durch GUIDs ersetzt. Die Eigenschaft `TenantId` gibt den AAD-Mandanten an, zu dem die Identität gehört. `PrincipalId` ist ein eindeutiger Bezeichner für die neue Identität des Clusters. In AAD weist der Dienstprinzipal denselben Namen auf, den Sie für Ihre App Service- oder Azure Functions-Instanz vergeben haben.

## <a name="remove-an-identity"></a>Entfernen einer Identität

Wenn Sie eine systemseitig zugewiesene Identität entfernen, wird sie auch aus AAD gelöscht. Systemseitig zugewiesene Identitäten werden außerdem automatisch aus AAD entfernt, wenn die Clusterressource gelöscht wird. Eine systemseitig zugewiesene Identität kann durch Deaktivieren des Features entfernt werden:

```json
"identity": {
    "type": "None"
}
```

## <a name="next-steps"></a>Nächste Schritte

* [Schützen von Azure Data Explorer-Clustern in Azure](security.md)
* [Sichern Ihres Clusters in Azure Data Explorer – Azure-Portal](manage-cluster-security.md) durch Aktivieren der Verschlüsselung ruhender Daten
 * [Konfigurieren von kundenseitig verwalteten Schlüsseln mithilfe von C#](customer-managed-keys-csharp.md)
 * [Konfigurieren von kundenseitig verwalteten Schlüsseln mithilfe der Azure Resource Manager-Vorlage](customer-managed-keys-resource-manager.md)
