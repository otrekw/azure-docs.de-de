---
ms.openlocfilehash: 9da0cbc3777359994fac3778dcc126c844754861
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2021
ms.locfileid: "101657637"
---
## <a name="add-managed-identity-to-your-communication-services-solution"></a>Hinzufügen einer verwalteten Identität zu Ihrer Communication Services-Lösung

### <a name="install-the-client-library-packages"></a>Installieren der Pakete der Clientbibliothek

```console
pip install azure-identity
pip install azure-communication-identity
pip install azure-communication-sms
```

### <a name="use-the-client-library-packages"></a>Verwenden der Pakete der Clientbibliothek

Fügen Sie dem Code das folgende `import`-Element hinzu, um die Azure-Identität zu verwenden.

```python
from azure.identity import DefaultAzureCredential
```

In den nachstehenden Beispielen wird [DefaultAzureCredential](/python/api/azure.identity.defaultazurecredential) verwendet. Diese Anmeldeinformationen eignen sich für Produktions- und Entwicklungsumgebungen.

Informationen zum Registrieren einer Anwendung in der Entwicklungsumgebung und zum Einrichten von Umgebungsvariablen finden Sie unter [Autorisieren des Zugriffs mit der verwalteten Identität](../managed-identity-from-cli.md).

### <a name="create-an-identity-and-issue-a-token"></a>Erstellen einer Identität und Ausstellen eines Tokens

Im folgenden Codebeispiel wird veranschaulicht, wie ein Dienstclientobjekt mit einer verwalteten Identität erstellt und der Client anschließend verwendet wird, um ein Token für einen neuen Benutzer auszustellen:

```python
import azure.communication.identity 

def create_identity_and_get_token(resource_endpoint):
     credential = DefaultAzureCredential()
     client = CommunicationIdentityClient(endpoint, credential)

     user = client.create_user()
     token_response = client.get_token(user, scopes=["voip"])
     
     return token_response
```

### <a name="send-an-sms-with-azure-managed-identity"></a>Senden einer SMS mit einer verwalteten Azure-Identität

Im folgenden Codebeispiel wird veranschaulicht, wie ein Dienstclientobjekt mit einer verwalteten Azure-Identität erstellt und der Client anschließend verwendet wird, um eine SMS zu senden:

```python
from azure.communication.sms import (
    PhoneNumberIdentifier,
    SendSmsOptions,
    SmsClient
)

def send_sms(resource_endpoint, from_phone_number, to_phone_number, message_content):
     credential = DefaultAzureCredential()
     sms_client = SmsClient(resource_endpoint, credential)

     sms_client.send(
          from_phone_number=PhoneNumberIdentitifier(from_phone_number),
          to_phone_numbers=[PhoneNumberIdentifier(to_phone_number)],
          message=message_content,
          send_sms_options=SendSmsOptions(enable_delivery_report=True))  # optional property
     )
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Informationen zur Authentifizierung](../../concepts/authentication.md)

Das könnte Sie auch interessieren:

- [Weitere Informationen zur rollenbasierten Zugriffssteuerung in Azure](../../../../articles/role-based-access-control/index.yml)
- [Weitere Informationen zur Azure-Identitätsbibliothek für Python](/net/api/overview/azure/identity-readme)
- [Erstellen von Benutzerzugriffstoken](../../quickstarts/access-tokens.md)
- [Senden einer SMS](../../quickstarts/telephony-sms/send.md)
- [Weitere Informationen zu SMS](../../concepts/telephony-sms/concepts.md)