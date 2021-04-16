---
ms.openlocfilehash: c642b0e5f459b2412bca6588c8ae625142f0f59f
ms.sourcegitcommit: 56b0c7923d67f96da21653b4bb37d943c36a81d6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2021
ms.locfileid: "106450276"
---
## <a name="add-managed-identity-to-your-communication-services-solution"></a>Hinzufügen einer verwalteten Identität zu Ihrer Communication Services-Lösung

### <a name="install-the-sdk-packages"></a>Installieren der SDK-Pakete

```console
pip install azure-identity
pip install azure-communication-identity
pip install azure-communication-sms
```

### <a name="use-the-sdk-packages"></a>Verwenden der SDK-Pakete

Fügen Sie dem Code das folgende `import`-Element hinzu, um die Azure-Identität zu verwenden.

```python
from azure.identity import DefaultAzureCredential
```

In den nachstehenden Beispielen wird [DefaultAzureCredential](/python/api/azure-identity/azure.identity.defaultazurecredential) verwendet. Diese Anmeldeinformationen eignen sich für Produktions- und Entwicklungsumgebungen.

Eine einfache Möglichkeit, direkt mit der Authentifizierung mithilfe verwalteter Identitäten zu beginnen, wird unter [Autorisieren des Zugriffs mit der verwalteten Identität](../managed-identity-from-cli.md) erläutert.

Ausführlichere Informationen zur Funktionsweise des DefaultAzureCredential-Objekts und zu seinen Einsatzmöglichkeiten, die in dieser Schnellstartanleitung nicht behandelt werden, finden Sie unter [Azure-Identitätsclientbibliothek für Python](https://docs.microsoft.com/python/api/overview/azure/identity-readme).

### <a name="create-an-identity-and-issue-a-token"></a>Erstellen einer Identität und Ausstellen eines Tokens

Im folgenden Codebeispiel wird veranschaulicht, wie ein Dienstclientobjekt mit einer verwalteten Identität erstellt und der Client anschließend verwendet wird, um ein Token für einen neuen Benutzer auszustellen:

```python
from azure.communication.identity import CommunicationIdentityClient

def create_identity_and_get_token(resource_endpoint):
     credential = DefaultAzureCredential()
     client = CommunicationIdentityClient(resource_endpoint, credential)

     user = client.create_user()
     token_response = client.get_token(user, scopes=["voip"])

     return token_response
```

### <a name="send-an-sms-with-azure-managed-identity"></a>Senden einer SMS mit einer verwalteten Azure-Identität
Im folgenden Codebeispiel wird veranschaulicht, wie ein Dienstclientobjekt mit einer verwalteten Azure-Identität erstellt und der Client anschließend verwendet wird, um eine SMS zu senden:

```python
from azure.communication.sms import SmsClient

def send_sms(resource_endpoint, from_phone_number, to_phone_number, message_content):
     credential = DefaultAzureCredential()
     sms_client = SmsClient(resource_endpoint, credential)

     sms_client.send(
          from_=from_phone_number,
          to_=[to_phone_number],
          message=message_content,
          enable_delivery_report=True  # optional property
     )
```

### <a name="list-all-your-purchased-phone-numbers"></a>Auflisten aller erworbenen Telefonnummern

Im folgenden Codebeispiel wird veranschaulicht, wie ein Dienstclientobjekt mit einer verwalteten Azure-Identität erstellt wird. Anschließend wird der Client zum Anzeigen aller erworbenen Telefonnummern der Ressource verwendet:

```python
from azure.communication.phonenumbers import PhoneNumbersClient

def list_purchased_phone_numbers(resource_endpoint):
     credential = DefaultAzureCredential()
     phone_numbers_client = PhoneNumbersClient(resource_endpoint, credential)

     return phone_numbers_client.list_purchased_phone_numbers()
```