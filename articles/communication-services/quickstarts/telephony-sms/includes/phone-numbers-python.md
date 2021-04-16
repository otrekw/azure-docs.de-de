---
ms.openlocfilehash: e62aed02a0ad5f26ec8fd0a79de5e91269386095
ms.sourcegitcommit: 56b0c7923d67f96da21653b4bb37d943c36a81d6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2021
ms.locfileid: "106450442"
---
## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- [Python](https://www.python.org/downloads/) 2.7, 3.5 oder höher.
- Eine bereitgestellte Communication Services-Ressource und eine Verbindungszeichenfolge. [Erstellen Sie eine Communication Services-Ressource](../../create-communication-resource.md).

## <a name="setting-up"></a>Einrichten

### <a name="create-a-new-python-application"></a>Erstellen einer neuen Python-Anwendung

Öffnen Sie Ihr Terminal- oder Befehlsfenster, erstellen Sie ein neues Verzeichnis für Ihre App, und navigieren Sie dann zu diesem Verzeichnis.

```console
mkdir phone-numbers-quickstart && cd phone-numbers-quickstart
```

Erstellen Sie mithilfe eines Text-Editors im Stammverzeichnis des Projekts eine Datei mit dem Namen phone_numbers_sample.py, und fügen Sie den folgenden Code hinzu. In den folgenden Abschnitten fügen wir den restlichen Schnellstart.Code hinzu.

```python
import os
from azure.communication.phonenumbers import PhoneNumbersClient

try:
   print('Azure Communication Services - Phone Numbers Quickstart')
   # Quickstart code goes here
except Exception as ex:
   print('Exception:')
   print(ex)
```

### <a name="install-the-package"></a>Installieren des Pakets

Installieren Sie im Anwendungsverzeichnis mithilfe des Befehls `pip install` das Python-Paket der Azure Communication Services-Clientbibliothek „Administration“.

```console
pip install azure-communication-phonenumbers
```

## <a name="authenticate-the-phone-numbers-client"></a>Authentifizieren des Phone Numbers-Clients

`PhoneNumbersClient` ist für die Verwendung Azure Active Directory-Authentifizierung aktiviert. Die Verwendung des `DefaultAzureCredential`-Objekts ist die einfachste Möglichkeit, um mit Azure Active Directory zu beginnen. Sie können es mit dem Befehl `pip install` installieren.

```console
pip install azure-identity
```

Zum Erstellen eines `DefaultAzureCredential`-Objekts müssen Sie `AZURE_CLIENT_ID`, `AZURE_CLIENT_SECRET` und `AZURE_TENANT_ID` bereits als Umgebungsvariablen mit den entsprechenden Werten aus der registrierten Azure AD-Anwendung festlegen.

Nachdem Sie die `azure-identity`-Bibliothek installiert haben, können Sie die Authentifizierung des Clients fortsetzen.

```python
import os
from azure.communication.phonenumbers import PhoneNumbersClient
from azure.identity import DefaultAzureCredential

# You can find your endpoint from your resource in the Azure portal
endpoint = 'https://<RESOURCE_NAME>.communication.azure.com'
try:
    print('Azure Communication Services - Phone Numbers Quickstart')
    credential = DefaultAzureCredential()
    phone_numbers_client = PhoneNumbersClient(endpoint, credential)
except Exception as ex:
    print('Exception:')
    print(ex)
```

Alternativ können Sie auch den Endpunkt und den Zugriffsschlüssel aus der Kommunikationsressource zur Authentifizierung verwenden.

```python
import os
from azure.communication.phonenumbers import PhoneNumbersClient

# You can find your connection string from your resource in the Azure portal
connection_string = 'https://<RESOURCE_NAME>.communication.azure.com/;accesskey=<YOUR_ACCESS_KEY>'
try:
    print('Azure Communication Services - Phone Numbers Quickstart')
    phone_numbers_client = PhoneNumbersClient.from_connection_string(connection_string)
except Exception as ex:
    print('Exception:')
    print(ex)
```

## <a name="functions"></a>Functions

Nachdem `PhoneNumbersClient` authentifiziert wurde, können Sie mit der Arbeit an den verschiedenen Funktionen beginnen, die davon übernommen werden können.

### <a name="search-for-available-phone-numbers"></a>Suchen nach verfügbaren Telefonnummern

Um Telefonnummern zu kaufen, müssen Sie zuerst nach verfügbaren Telefonnummern suchen. Geben Sie für die Suche nach Telefonnummern die Vorwahl, den Zuweisungstyp, die [Funktionen der Telefonnummer](../../../concepts/telephony-sms/plan-solution.md#phone-number-capabilities-in-azure-communication-services), den [Telefonnummerntyp](../../../concepts/telephony-sms/plan-solution.md#phone-number-types-in-azure-communication-services) und die Menge an (Standardmenge ist 1). Beachten Sie, dass die Angabe der Vorwahl für gebührenfreie Telefonnummern nicht zwingend ist.

```python
import os
from azure.communication.phonenumbers import PhoneNumbersClient, PhoneNumberCapabilityType, PhoneNumberAssignmentType, PhoneNumberType, PhoneNumberCapabilities
from azure.identity import DefaultAzureCredential

# You can find your endpoint from your resource in the Azure portal
endpoint = 'https://<RESOURCE_NAME>.communication.azure.com'
try:
    print('Azure Communication Services - Phone Numbers Quickstart')
    credential = DefaultAzureCredential()
    phone_numbers_client = PhoneNumbersClient(endpoint, credential)
    capabilities = PhoneNumberCapabilities(
        calling = PhoneNumberCapabilityType.INBOUND,
        sms = PhoneNumberCapabilityType.INBOUND_OUTBOUND
    )
    search_poller = self.phone_number_client.begin_search_available_phone_numbers(
        "US",
        PhoneNumberType.TOLL_FREE,
        PhoneNumberAssignmentType.APPLICATION,
        capabilities,
        polling = True
    )
    search_result = search_poller.result()
    print ('Search id: ' + search_result.search_id)
    phone_number_list = search_result.phone_numbers
    print('Reserved phone numbers:')
    for phone_number in phone_number_list:
        print(phone_number)

except Exception as ex:
    print('Exception:')
    print(ex)
```

### <a name="purchase-phone-numbers"></a>Kaufen von Telefonnummern

Das Ergebnis der Suche nach Telefonnummern ist ein `PhoneNumberSearchResult`. Hierin ist ein Wert `searchId` enthalten, der an die API zum Kaufen von Nummern übermittelt werden kann, um die mit der Suche ermittelten Nummern zu erwerben. Beachten Sie, dass der Aufruf der API zum Kaufen von Telefonnummern für Ihr Azure-Konto zu einer Abrechnung führt.

```python
import os
from azure.communication.phonenumbers import (
    PhoneNumbersClient,
    PhoneNumberCapabilityType,
    PhoneNumberAssignmentType,
    PhoneNumberType,
    PhoneNumberCapabilities
)
from azure.identity import DefaultAzureCredential

# You can find your endpoint from your resource in the Azure portal
endpoint = 'https://<RESOURCE_NAME>.communication.azure.com'
try:
    print('Azure Communication Services - Phone Numbers Quickstart')
    credential = DefaultAzureCredential()
    phone_numbers_client = PhoneNumbersClient(endpoint, credential)
    capabilities = PhoneNumberCapabilities(
        calling = PhoneNumberCapabilityType.INBOUND,
        sms = PhoneNumberCapabilityType.INBOUND_OUTBOUND
    )
    search_poller = phone_numbers_client.begin_search_available_phone_numbers(
        "US",
        PhoneNumberType.TOLL_FREE,
        PhoneNumberAssignmentType.APPLICATION,
        capabilities,
        area_code="833",
        polling = True
    )
    search_result = poller.result()
    print ('Search id: ' + search_result.search_id)
    phone_number_list = search_result.phone_numbers
    print('Reserved phone numbers:')
    for phone_number in phone_number_list:
        print(phone_number)

    purchase_poller = phone_numbers_client.begin_purchase_phone_numbers(search_result.search_id, polling = True)
    purchase_poller.result()
    print("The status of the purchase operation was: " + purchase_poller.status())
except Exception as ex:
    print('Exception:')
    print(ex)
```

### <a name="get-purchased-phone-numbers"></a>Erhalten von erworbenen Telefonnummern

Nach dem Kauf einer Nummer können Sie diese vom Client abrufen.

```python
purchased_phone_number_information = phone_numbers_client.get_purchased_phone_number("+18001234567")
print('Phone number: ' + purchased_phone_number_information.phone_number)
print('Country code: ' + purchased_phone_number_information.country_code)
```

Sie können auch alle erworbenen Telefonnummern abrufen.

```python
purchased_phone_numbers = phone_numbers_client.list_purchased_phone_numbers()
print('Purchased phone numbers:')
for purchased_phone_number in purchased_phone_numbers:
    print(purchased_phone_number.phone_number)
```

### <a name="update-phone-number-capabilities"></a>Aktualisieren der Funktionen für Telefonnummern

Sie können die Funktionen einer zuvor erworbenen Telefonnummer aktualisieren.
```python
update_poller = phone_numbers_client.begin_update_phone_number_capabilities(
    "+18001234567",
    PhoneNumberCapabilityType.OUTBOUND,
    PhoneNumberCapabilityType.OUTBOUND,
    polling = True
)
update_poller.result()
print('Status of the operation: ' + update_poller.status())
```

### <a name="release-phone-number"></a>Freigeben einer Telefonnummer

Sie können eine erworbene Telefonnummer freigeben.

```python
release_poller = phone_numbers_client.begin_release_phone_number("+18001234567")
release_poller.result()
print('Status of the operation: ' + release_poller.status())
```

## <a name="run-the-code"></a>Ausführen des Codes

Navigieren Sie in einer Konsoleneingabeaufforderung zum Verzeichnis mit der Datei phone_numbers_sample.py, und führen Sie anschließend den folgenden Phyton-Befehl aus, um die App auszuführen.

```console
./phone_numbers_sample.py
```
