---
ms.openlocfilehash: f2e4bf603fa4cfb93c7ca51f64029ccaedcff727
ms.sourcegitcommit: 225e4b45844e845bc41d5c043587a61e6b6ce5ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/11/2021
ms.locfileid: "103021384"
---
## <a name="add-managed-identity-to-your-communication-services-solution-java"></a>Hinzufügen einer verwalteten Identität zu Ihrer Communication Services-Lösung (Java)

### <a name="install-the-client-library-packages"></a>Installieren der Pakete der Clientbibliothek
Fügen Sie der Gruppe der Abhängigkeiten in der Datei „pom.xml“ das folgende Abhängigkeitselement hinzu:

```xml
<dependency>
    <groupId>com.azure</groupId>
    <artifactId>azure-communication-identity</artifactId>
    <version>1.0.0-beta.6</version>
</dependency>
<dependency>
    <groupId>com.azure</groupId>
    <artifactId>azure-communication-sms</artifactId>
    <version>1.0.0-beta.4</version>
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
import com.azure.communication.common.*;
import com.azure.communication.identity.*;
import com.azure.communication.identity.models.*;
import com.azure.communication.sms.*;
import com.azure.core.credential.*;
import com.azure.core.http.*;
import com.azure.core.http.netty.*;
import com.azure.identity.*;

import java.io.IOException;
import java.util.*;
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
          SmsSendResult response = smsClient.send(
               "<from-phone-number>",
               "<to-phone-number>",
               "your message"
          );

          return response;
    }
```

