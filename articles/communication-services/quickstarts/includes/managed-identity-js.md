---
ms.openlocfilehash: 0fd97fe0eebb23130513409698b75d2ee7b98a6f
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2021
ms.locfileid: "101657632"
---
## <a name="add-managed-identity-to-your-communication-services-solution-js"></a>Hinzufügen einer verwalteten Identität zu Ihrer Communication Services-Lösung (JS)

### <a name="install-the-client-library-packages"></a>Installieren der Pakete der Clientbibliothek

```console
npm install @azure/communication-identity
npm install @azure/communication-common
npm install @azure/communication-sms
npm install @azure/identity
```

### <a name="use-the-client-library-packages"></a>Verwenden der Pakete der Clientbibliothek

Fügen Sie dem Code die folgenden `import`-Anweisungen hinzu, um die Azure Identity- und Azure Storage-Clientbibliotheken zu verwenden.

```typescript
import { DefaultAzureCredential } from "@azure/identity";
import { CommunicationIdentityClient } from "@azure/communication-identity";
import { SmsClient } from "@azure/communication-sms";
```

In den nachstehenden Beispielen wird [DefaultAzureCredential](/javascript/api/azure.identity.defaultazurecredential) verwendet. Diese Anmeldeinformationen eignen sich für Produktions- und Entwicklungsumgebungen.

Informationen zum Registrieren einer Anwendung in der Entwicklungsumgebung und zum Einrichten von Umgebungsvariablen finden Sie unter [Autorisieren des Zugriffs mit der verwalteten Identität](../managed-identity-from-cli.md).  

### <a name="create-an-identity-and-issue-a-token-with-managed-identity"></a>Erstellen einer Identität und Ausstellen eines Tokens mit einer verwalteten Identität

Im folgenden Codebeispiel wird veranschaulicht, wie ein Dienstclientobjekt mit einer verwalteten Identität erstellt und der Client anschließend verwendet wird, um ein Token für einen neuen Benutzer auszustellen:

```JavaScript
export async function createIdentityAndIssueToken(resourceEndpoint: string): Promise<CommunicationUserToken> {
     let credential = new DefaultAzureCredential();
     const client = new CommunicationIdentityClient(resourceEndpoint, credential);
     return await client.createUserWithToken(["chat"]);
}
```

### <a name="send-an-sms-with-managed-identity"></a>Senden einer SMS mit einer verwalteten Identität

Im folgenden Codebeispiel wird veranschaulicht, wie ein Dienstclientobjekt mit einer verwalteten Identität erstellt und der Client anschließend verwendet wird, um eine SMS zu senden:

```JavaScript
export async function sendSms(resourceEndpoint: string, fromNumber: any, toNumber: any, message: string) {
     let credential = new DefaultAzureCredential();
     const smsClient = new SmsClient(resourceEndpoint, credential);
     const sendRequest: SendRequest = { 
          from: fromNumber, 
          to: [toNumber], 
          message: message 
     };

     const response = await smsClient.send(
          sendRequest, 
          {} //Optional SendOptions
          );
}
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Informationen zur Authentifizierung](../concepts/authentication.md)

Das könnte Sie auch interessieren:

- [Weitere Informationen zur rollenbasierten Zugriffssteuerung in Azure](../../../../articles/role-based-access-control/index.yml)
- [Weitere Informationen zur Azure-Identitätsbibliothek für JS](/javascript/api/overview/azure/identity-readme)
- [Erstellen von Benutzerzugriffstoken](../../quickstarts/access-tokens.md)
- [Senden einer SMS](../../quickstarts/telephony-sms/send.md)
- [Weitere Informationen zu SMS](../../concepts/telephony-sms/concepts.md)

