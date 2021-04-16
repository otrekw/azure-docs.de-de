---
author: robinsh
ms.author: robinsh
ms.service: iot-hub
ms.topic: include
ms.date: 10/26/2018
ms.openlocfilehash: 7f7dc1483002c2bdfe3227a8aade8dbf2a8da417
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "70803005"
---
## <a name="obtain-an-azure-resource-manager-token"></a>Abrufen eines Azure Resource Manager-Tokens
Azure Active Directory muss alle Aufgaben authentifizieren, die Sie mithilfe des Azure-Ressourcen-Managers für die Ressourcen ausführen. Im hier gezeigten Beispiel wird die Kennwortauthentifizierung verwendet; andere Ansätze finden Sie unter [Authentifizieren von Azure Resource Manager-Anforderungen][lnk-authenticate-arm].

1. Fügen Sie der **Main** -Methode in „Program.cs“ folgenden Code hinzu, um mithilfe der Anwendungs-ID und des Kennworts ein Token von Azure AD abzurufen.
   
    ```csharp
    var authContext = new AuthenticationContext(string.Format  
      ("https://login.microsoftonline.com/{0}", tenantId));
    var credential = new ClientCredential(applicationId, password);
    AuthenticationResult token = authContext.AcquireTokenAsync
      ("https://management.core.windows.net/", credential).Result;
   
    if (token == null)
    {
      Console.WriteLine("Failed to obtain the token");
      return;
    }
    ```
2. Erstellen Sie ein **ResourceManagementClient**-Objekt, das das Token verwendet. Fügen Sie dazu folgenden Code am Ende der **Main**-Methode hinzu:
   
    ```csharp
    var creds = new TokenCredentials(token.AccessToken);
    var client = new ResourceManagementClient(creds);
    client.SubscriptionId = subscriptionId;
    ```
3. Erstellen Sie oder erhalten Sie einen Verweis auf die Ressourcengruppe, die Sie verwenden:
   
    ```csharp
    var rgResponse = client.ResourceGroups.CreateOrUpdate(rgName,
        new ResourceGroup("East US"));
    if (rgResponse.Properties.ProvisioningState != "Succeeded")
    {
      Console.WriteLine("Problem creating resource group");
      return;
    }
    ```

[lnk-authenticate-arm]: https://msdn.microsoft.com/library/azure/dn790557.aspx