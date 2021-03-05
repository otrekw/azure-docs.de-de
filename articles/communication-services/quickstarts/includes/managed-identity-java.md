---
ms.openlocfilehash: c5d83cc709c334e15a1c13e64ba17ef24835fed0
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2021
ms.locfileid: "101657654"
---
## <a name="add-managed-identity-to-your-communication-services-solution-java"></a>Hinzufügen einer verwalteten Identität zu Ihrer Communication Services-Lösung (Java)

### <a name="install-the-client-library-packages"></a>Installieren der Pakete der Clientbibliothek
Fügen Sie der Gruppe der Abhängigkeiten in der Datei „pom.xml“ das folgende Abhängigkeitselement hinzu:

```xml
<dependency>
    <groupId>com.azure</groupId>
    <artifactId>azure-communication-identity</artifactId>
    <version>1.0.0</version>
</dependency>
<dependency>
    <groupId>com.azure</groupId>
    <artifactId>azure-communication-sms</artifactId>
    <version>1.0.0</version>
</dependency>
<dependency>
    <groupId>com.azure</groupId>
    <artifactId>azure-identity</artifactId>
    <version>1.2.3</version>
</dependency>
```

### <a name="use-the-client-library-packages"></a>Verwenden der Pakete der Clientbibliothek

Fügen Sie dem Code die folgenden `import`-Anweisungen hinzu, um die Azure Identity- und Azure Communication-Clientbibliotheken zu verwenden.

```java
import com.azure.identity.*;
import com.azure.communication.sms.*;
import com.azure.communication.identity.*;
import com.azure.communication.common.*;
```

In den nachstehenden Beispielen wird [DefaultAzureCredential](/java/api/azure.identity.defaultazurecredential) verwendet. Diese Anmeldeinformationen eignen sich für Produktions- und Entwicklungsumgebungen.

Die Umgebungsvariablen `AZURE_CLIENT_SECRET`, `AZURE_CLIENT_ID` und `AZURE_TENANT_ID` sind für die Erstellung eines `DefaultAzureCredential`-Objekts erforderlich. Informationen zum Erstellen einer registrierten Anwendung in der Entwicklungsumgebung und zum Einrichten von Umgebungsvariablen finden Sie unter [Autorisieren des Zugriffs mit der verwalteten Identität](../managed-identity-from-cli.md).

### <a name="create-an-identity-and-issue-a-token-with-managed-identity"></a>Erstellen einer Identität und Ausstellen eines Tokens mit einer verwalteten Identität

Im folgenden Codebeispiel wird gezeigt, wie ein Dienstclientobjekt mit einer verwalteten Identität erstellt wird.
Verwenden Sie anschließend den Client, um ein Token für einen neuen Benutzer auszustellen:

```java
     public AccessToken createIdentityAndGetTokenAsync() {
          // You can find your endpoint and access key from your resource in the Azure portal
          String endpoint = "https://<RESOURCE_NAME>.communication.azure.com";

          HttpClient httpClient = new NettyAsyncHttpClientBuilder().build();
          TokenCredential credential = new DefaultAzureCredentialBuilder().build();

          CommunicationIdentityClient communicationIdentityClient = new CommunicationIdentityClientBuilder()
               .endpoint(endpoint)
               .credential(credential)
               .httpClient(httpClient)
               .buildClient();

          CommunicationUserIdentifier user = communicationIdentityClient.createUser();
          AccessToken userToken = communicationIdentityClient.getToken(user, new ArrayList<>(Arrays.asList(CommunicationTokenScope.CHAT)));
          return userToken;
     }
```

### <a name="send-an-sms-with-managed-identity"></a>Senden einer SMS mit einer verwalteten Identität

Im folgenden Codebeispiel wird veranschaulicht, wie ein Dienstclientobjekt mit einer verwalteten Identität erstellt und der Client anschließend verwendet wird, um eine SMS zu senden:

```java
     public SendSmsResponse sendSms() {
          // You can find your endpoint and access key from your resource in the Azure portal
          String endpoint = "https://<RESOURCE_NAME>.communication.azure.com";

          HttpClient httpClient = new NettyAsyncHttpClientBuilder().build();
          TokenCredential credential = new DefaultAzureCredentialBuilder().build();

          SmsClient smsClient = new SmsClientBuilder()
               .endpoint(endpoint)
               .credential(credential)
               .httpClient(httpClient)
               .buildClient();

          // Send the message and check the response for a message id
          SendSmsResponse response = smsClient.sendMessage(
               new PhoneNumberIdentifier("<leased-phone-number>"),
               to,
               "your message",
               options /* Optional */
          );
          return response;
    }
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Informationen zur Authentifizierung](../concepts/authentication.md)

Das könnte Sie auch interessieren:

- [Weitere Informationen zur rollenbasierten Zugriffssteuerung in Azure](../../../../articles/role-based-access-control/index.yml)
- [Weitere Informationen zur Azure-Identitätsbibliothek für Java](/java/api/overview/azure/identity-readme)
- [Erstellen von Benutzerzugriffstoken](../../quickstarts/access-tokens.md)
- [Senden einer SMS](../../quickstarts/telephony-sms/send.md)
- [Weitere Informationen zu SMS](../../concepts/telephony-sms/concepts.md)
