---
author: mikben
ms.service: azure-communication-services
ms.topic: include
ms.date: 06/30/2021
ms.author: mikben
ms.openlocfilehash: 4f1552f911e62c6722c224927e05245bc3f9fed5
ms.sourcegitcommit: 98308c4b775a049a4a035ccf60c8b163f86f04ca
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/30/2021
ms.locfileid: "113113167"
---
## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/dotnet/).
- Die aktuelle Version des [.NET Core SDK](https://dotnet.microsoft.com/download/dotnet-core) für Ihr Betriebssystem.
- Rufen Sie die neueste Version des [.NET-Identitäts-SDK](/dotnet/api/azure.identity) ab.
- Rufen Sie die neueste Version des [.NET-VerwaltungsSDK](../../concepts/sdk-options.md) ab.

## <a name="installing-the-sdk"></a>Installieren des SDK

Fügen Sie zunächst die Communication Services-Verwaltungs-SDK in das C#-Projekt ein:

```csharp
using Azure.ResourceManager.Communication;
```

## <a name="subscription-id"></a>Abonnement-ID

Sie müssen die ID Ihres Azure-Abonnements kennen. Diese kann über das Portal abgerufen werden:

1.  Melden Sie sich bei Ihrem Azure-Konto an.
2.  Wählen Sie auf der linken Seitenleiste die Option „Abonnements“ aus.
3.  Wählen Sie das erforderliche Abonnement aus.
4.  Klicken Sie auf „Übersicht“.
5.  Wählen Sie die ID für Ihr Abonnement aus.

In dieser Schnellstartanleitung wird davon ausgegangen, dass Sie die Abonnement-ID in einer Umgebungsvariablen mit dem Namen `AZURE_SUBSCRIPTION_ID` gespeichert haben.

## <a name="authentication"></a>Authentifizierung

Für die Kommunikation mit Azure Communication Services müssen Sie sich zunächst bei Azure authentifizieren. In der Regel verwenden Sie dazu eine Dienstprinzipalidentität.

### <a name="option-1-managed-identity"></a>Option 1: Verwaltete Identität

Wenn Ihr Code als Dienst in Azure ausgeführt wird, können Sie sich am einfachsten authentifizieren, indem Sie eine verwaltete Identität von Azure abrufen. Informieren Sie sich ausführlicher über [verwaltete Identitäten](../../../active-directory/managed-identities-azure-resources/overview.md).

[Azure-Dienste mit Unterstützung verwalteter Identitäten](../../../active-directory/managed-identities-azure-resources/services-support-managed-identities.md)

[Verwenden verwalteter Identitäten für App Service und Azure Functions](../../../app-service/overview-managed-identity.md?tabs=dotnet)

#### <a name="system-assigned-managed-identity"></a>[Systemseitig zugewiesene verwaltete Identität](../../../app-service/overview-managed-identity.md?tabs=dotnet#add-a-system-assigned-identity)

```csharp
using Azure.Identity;
using Azure.ResourceManager.Communication;
using Azure.ResourceManager.Communication.Models;
using System;
...
var subscriptionId = "AZURE_SUBSCRIPTION_ID";
var acsClient = new CommunicationManagementClient(subscriptionId, new ManagedIdentityCredential());
```

#### <a name="user-assigned-managed-identity"></a>[Benutzerseitig zugewiesene verwaltete Identität](../../../app-service/overview-managed-identity.md?tabs=dotnet#add-a-user-assigned-identity)

Die Client-ID der von Ihnen erstellten verwalteten Identität muss explizit an `ManagedIdentityCredential` übergeben werden.

```csharp
using Azure.Identity;
using Azure.ResourceManager.Communication;
using Azure.ResourceManager.Communication.Models;
using System;
...
var subscriptionId = "AZURE_SUBSCRIPTION_ID";
var managedIdentityCredential = new ManagedIdentityCredential("AZURE_CLIENT_ID");
var acsClient = new CommunicationManagementClient(subscriptionId, managedIdentityCredential);
```

### <a name="option-2-service-principal"></a>Option 2: Dienstprinzipal

Anstatt eine verwaltete Identität zu verwenden, können Sie sich mithilfe eines Dienstprinzipals, den Sie selbst verwalten, bei Azure authentifizieren. Weitere Informationen finden Sie in der Dokumentation zum [Erstellen und Verwalten eines Dienstprinzipals in Azure Active Directory](../../../active-directory/develop/howto-create-service-principal-portal.md).

Nach dem Erstellen des Dienstprinzipals müssen Sie die folgenden Informationen zu ihm über das Azure-Portal erfassen:

- **Client-ID**
- **Geheimer Clientschlüssel**
- **Tenant ID**

Speichern Sie diese Werte in Umgebungsvariablen namens `AZURE_CLIENT_ID`, `AZURE_CLIENT_SECRET` und `AZURE_TENANT_ID`. Anschließend können Sie einen Communication Services-Verwaltungsclient wie den folgenden erstellen:

```csharp
using Azure.Identity;
using Azure.ResourceManager.Communication;
using Azure.ResourceManager.Communication.Models;
using System;
...
var subscriptionId = Environment.GetEnvironmentVariable("AZURE_SUBSCRIPTION_ID");
var acsClient = new CommunicationManagementClient(subscriptionId, new EnvironmentCredential());
```

### <a name="option-3-user-identity"></a>Option 3: Benutzeridentität

Wenn Sie Azure im Namen eines interaktiven Benutzers und nicht mithilfe einer Dienstidentität aufrufen möchten, können Sie den folgenden Code verwenden, um einen Azure Communication Services-Verwaltungsclient zu erstellen. Dadurch wird ein Browserfenster geöffnet, in dem der Benutzer zur Eingabe seiner MSA- oder Azure AD-Anmeldeinformationen aufgefordert wird.

```csharp
using Azure.Identity;
using Azure.ResourceManager.Communication;
using Azure.ResourceManager.Communication.Models;
using System;
...
var subscriptionId = Environment.GetEnvironmentVariable("AZURE_SUBSCRIPTION_ID");
var communicationServiceClient = new CommunicationManagementClient(subscriptionId, new InteractiveBrowserCredential());
```

## <a name="managing-communication-services-resources"></a>Verwalten von Communication Services-Ressourcen

### <a name="interacting-with-azure-resources"></a>Interaktion mit Azure-Ressourcen

Sie haben sich authentifiziert und können nun den Verwaltungsclient für API-Aufrufe verwenden.

Für jedes der folgenden Beispiele werden die Communication Services-Ressourcen einer vorhandenen Ressourcengruppe zugewiesen.

Wenn Sie eine Ressourcengruppe erstellen müssen, können Sie dazu das [Azure-Portal](../../../azure-resource-manager/management/manage-resource-groups-portal.md) oder das [Azure Resource Manager-SDK](https://github.com/Azure/azure-sdk-for-net/blob/master/doc/mgmt_preview_quickstart.md) verwenden.

### <a name="create-and-manage-a-communication-services-resource"></a>Erstellen und Verwalten einer Communication Services-Ressource

Unsere Instanz des Clients für das Communication Services-Verwaltungs-SDK (``Azure.ResourceManager.Communication.CommunicationManagementClient``) kann dazu verwendet werden, Vorgänge für Communication Services-Ressourcen auszuführen.

#### <a name="create-a-communication-services-resource"></a>Erstellen einer Communication Services-Ressource

Beim Erstellen einer Communication Services-Ressource geben Sie den Ressourcengruppennamen und den Ressourcennamen an. Beachten Sie, dass die Eigenschaft `Location` immer `global` ist. Während der öffentlichen Vorschauphase muss `UnitedStates` für den Wert `DataLocation` angegeben werden.

```csharp
var resourceGroupName = "myResourceGroupName";
var resourceName = "myResource";
var resource = new CommunicationServiceResource { Location = "Global", DataLocation = "UnitedStates"  };
var operation = await acsClient.CommunicationService.StartCreateOrUpdateAsync(resourceGroupName, resourceName, resource);
await operation.WaitForCompletionAsync();
```

#### <a name="update-a-communication-services-resource"></a>Aktualisieren einer Communication Services-Ressource

```csharp
...
var resourceGroupName = "myResourceGroupName";
var resourceName = "myResource";
var resource = new CommunicationServiceResource { Location = "Global", DataLocation = "UnitedStates" };
resource.Tags.Add("environment","test");
resource.Tags.Add("department","tech");
// Use existing resource name and new resource object
var operation = await acsClient.CommunicationService.StartCreateOrUpdateAsync(resourceGroupName, resourceName, resource);
await operation.WaitForCompletionAsync();
```

#### <a name="list-all-communication-services-resources"></a>Auflisten aller Communication Services-Ressourcen

```csharp
var resources = acsClient.CommunicationService.ListBySubscription();
foreach (var resource in resources)
{
    Console.WriteLine(resource.Name);
}
```

#### <a name="delete-a-communication-services-resource"></a>Löschen einer Communication Services-Ressource

```csharp
var resourceGroupName = "myResourceGroupName";
var resourceName = "myResource";
await acsClient.CommunicationService.StartDeleteAsync(resourceGroupName, resourceName);
```

## <a name="managing-keys-and-connection-strings"></a>Verwalten von Schlüsseln und Verbindungszeichenfolgen

Jede Communication Services-Ressource verfügt über ein Zugriffsschlüsselpaar und zugehörige Verbindungszeichenfolgen. Auf diese Schlüssel kann mit dem Verwaltungs-SDK zugegriffen werden. Anschließend können sie dann von anderen Communication Services-SDKs verwendet werden, um sich bei Azure Communication Services zu authentifizieren.

#### <a name="get-access-keys-for-a-communication-services-resource"></a>Abrufen von Zugriffsschlüsseln für eine Communication Services-Ressource

```csharp
var resourceGroupName = "myResourceGroupName";
var resourceName = "myResource";
var keys = await acsClient.CommunicationService.ListKeysAsync(resourceGroupName, resourceName);

Console.WriteLine(keys.Value.PrimaryConnectionString);
Console.WriteLine(keys.Value.SecondaryConnectionString);
```

#### <a name="regenerate-an-access-key-for-a-communication-services-resource"></a>Erneutes Generieren eines Zugriffsschlüssels für eine Communication Services-Ressource

```csharp
var resourceGroupName = "myResourceGroupName";
var resourceName = "myResource";
var keyParams = new RegenerateKeyParameters { KeyType = KeyType.Primary };
var keys = await acsClient.CommunicationService.RegenerateKeyAsync(resourceGroupName, resourceName, keyParams);

Console.WriteLine(keys.Value.PrimaryKey);
```