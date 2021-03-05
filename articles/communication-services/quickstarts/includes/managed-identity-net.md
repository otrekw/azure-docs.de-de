---
ms.openlocfilehash: 2c816f005dea452c3ffa2889aa7d2742a5762759
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2021
ms.locfileid: "101657649"
---
## <a name="add-managed-identity-to-your-communication-services-solution-net"></a>Hinzufügen einer verwalteten Identität zu Ihrer Communication Services-Lösung (.NET)

### <a name="install-the-client-library-packages"></a>Installieren der Pakete der Clientbibliothek

```console
dotnet add package Azure.Communication.Identity
dotnet add package Azure.Communication.Sms
dotnet add package Azure.Identity
```

### <a name="use-the-client-library-packages"></a>Verwenden der Pakete der Clientbibliothek

Fügen Sie dem Code die folgenden `using`-Anweisungen hinzu, um die Azure Identity- und Azure Storage-Clientbibliotheken zu verwenden.

```csharp
using Azure.Identity;
using Azure.Communication.Identity;
using Azure.Communication.Sms;
```

In den nachstehenden Beispielen wird [DefaultAzureCredential](/dotnet/api/azure.identity.defaultazurecredential) verwendet. Diese Anmeldeinformationen eignen sich für Produktions- und Entwicklungsumgebungen.

Die Umgebungsvariablen `AZURE_CLIENT_SECRET`, `AZURE_CLIENT_ID` und `AZURE_TENANT_ID` sind für die Erstellung eines `DefaultAzureCredential`-Objekts erforderlich. Informationen zum Erstellen einer registrierten Anwendung in der Entwicklungsumgebung und zum Einrichten von Umgebungsvariablen finden Sie unter [Autorisieren des Zugriffs mit der verwalteten Identität](../managed-identity-from-cli.md).

### <a name="create-an-identity-and-issue-a-token-with-managed-identity"></a>Erstellen einer Identität und Ausstellen eines Tokens mit einer verwalteten Identität

Im folgenden Codebeispiel wird veranschaulicht, wie ein Dienstclientobjekt mit Azure Active Directory-Token erstellt wird.

Verwenden Sie anschließend den Client, um ein Token für einen neuen Benutzer auszustellen:

```csharp
     public async Task<Response<CommunicationUserToken>> CreateIdentityAndGetTokenAsync(Uri resourceEdnpoint)
     {
          TokenCredential credential = new DefaultAzureCredential();
          // You can find your endpoint and access key from your resource in the Azure portal
          String resourceEndpoint = "https://<RESOURCE_NAME>.communication.azure.com";

          var client = new CommunicationIdentityClient(resourceEndpoint, credential);
          var identityResponse = await client.CreateUserAsync();

          var tokenResponse = await client.GetTokenAsync(identity, scopes: new [] { CommunicationTokenScope.VoIP });

          return tokenResponse;
     }
```

### <a name="send-an-sms-with-managed-identity"></a>Senden einer SMS mit einer verwalteten Identität

Im folgenden Codebeispiel wird veranschaulicht, wie ein SMS-Dienstclientobjekt mit einer verwalteten Identität erstellt und der Client anschließend verwendet wird, um eine SMS zu senden:

```csharp
     public async Task SendSms(Uri resourceEndpoint, PhoneNumber from, PhoneNumber to, string message)
     {
          TokenCredential credential = new DefaultAzureCredential();
          // You can find your endpoint and access key from your resource in the Azure portal
          String resourceEndpoint = "https://<RESOURCE_NAME>.communication.azure.com";

          SmsClient smsClient = new SmsClient(resourceEndpoint, credential);
          smsClient.Send(
               from: from,
               to: to,
               message: message,
               new SendSmsOptions { EnableDeliveryReport = true } // optional
          );
     }
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Informationen zur Authentifizierung](../concepts/authentication.md)

Das könnte Sie auch interessieren:

- [Weitere Informationen zur rollenbasierten Zugriffssteuerung in Azure](../../../../articles/role-based-access-control/index.yml)
- [Weitere Informationen zur Azure-Identitätsbibliothek für .NET](/dotnet/api/overview/azure/identity-readme)
- [Erstellen von Benutzerzugriffstoken](../../quickstarts/access-tokens.md)
- [Senden einer SMS](../telephony-sms/send.md)
- [Weitere Informationen zu SMS](../../concepts/telephony-sms/concepts.md)
