---
ms.openlocfilehash: 956c92c5c020f892b8148e9d43d403b1099fbdba
ms.sourcegitcommit: 5fd1f72a96f4f343543072eadd7cdec52e86511e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/01/2021
ms.locfileid: "106113237"
---
## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- [Node.js](https://nodejs.org/): Active LTS- und Maintenance LTS-Versionen (8.11.1 und 10.14.1 empfohlen).
- Eine aktive Communication Services-Ressource und eine Verbindungszeichenfolge. [Erstellen Sie eine Communication Services-Ressource.](../../create-communication-resource.md)

### <a name="prerequisite-check"></a>Prüfen der Voraussetzungen

- Führen Sie in einem Terminal- oder Befehlsfenster `node --version` aus, um sich zu vergewissern, dass Node.js installiert ist.

## <a name="setting-up"></a>Einrichten

### <a name="create-a-new-nodejs-application"></a>Erstellen einer neuen Node.js-Anwendung

Öffnen Sie zunächst Ihr Terminal- oder Befehlsfenster, erstellen Sie ein neues Verzeichnis für Ihre App, und navigieren Sie zu diesem Verzeichnis.

```console
mkdir phone-numbers-quickstart && cd phone-numbers-quickstart
```

Führen Sie `npm init -y` aus, um die Datei **package.json** mit den Standardeinstellungen zu erstellen.

```console
npm init -y
```

Erstellen Sie im Stammverzeichnis des soeben erstellten Verzeichnisses eine Datei mit dem Namen **phone-numbers-quickstart.js**. Führen Sie den folgenden Codeausschnitt hinzu:

```javascript
async function main() {
    // quickstart code will here
}

main();
```

### <a name="install-the-package"></a>Installieren des Pakets

Verwenden Sie den Befehl `npm install`, um die JavaScript-Clientbibliothek für Telefonnummern von Azure Communication Services zu installieren.

```console
npm install @azure/communication-phone-numbers --save
```

Mit der Option `--save` wird die Bibliothek als Abhängigkeit zu Ihrer Datei **package.json** hinzugefügt.

## <a name="authenticate-the-client"></a>Authentifizieren des Clients

Importieren Sie den **PhoneNumbersClient** aus der Clientbibliothek, und instanziieren Sie ihn mit Ihrer Verbindungszeichenfolge. Im folgenden Code wird die Verbindungszeichenfolge für die Ressource aus einer Umgebungsvariablen namens `COMMUNICATION_SERVICES_CONNECTION_STRING` abgerufen. Informationen zur [Verwaltung der Verbindungszeichenfolge Ihrer Ressource](../../create-communication-resource.md#store-your-connection-string).

Fügen Sie den folgenden Code am Anfang von **phone-numbers-quickstart.js** hinzu:

```javascript
const { PhoneNumbersClient } = require('@azure/communication-phone-numbers');

// This code demonstrates how to fetch your connection string
// from an environment variable.
const connectionString = process.env['COMMUNICATION_SERVICES_CONNECTION_STRING'];

// Instantiate the phone numbers client
const phoneNumbersClient = new PhoneNumbersClient(connectionString);
```

## <a name="manage-phone-numbers"></a>Verwalten von Telefonnummern

### <a name="search-for-available-phone-numbers"></a>Suchen nach verfügbaren Telefonnummern

Um Telefonnummern zu kaufen, müssen Sie zuerst nach verfügbaren Telefonnummern suchen. Geben Sie für die Suche nach Telefonnummern die Vorwahl, den Zuweisungstyp, die [Funktionen der Telefonnummer](../../../concepts/telephony-sms/plan-solution.md#phone-number-capabilities-in-azure-communication-services), den [Telefonnummerntyp](../../../concepts/telephony-sms/plan-solution.md#phone-number-types-in-azure-communication-services) und die Menge an. Beachten Sie, dass die Angabe der Vorwahl für gebührenfreie Telefonnummern nicht zwingend ist.

Fügen Sie Ihrer `main`-Funktion den unten angegebenen Codeausschnitt hinzu:

```javascript
/**
 * Search for Available Phone Number
 */

// Create search request
const searchRequest = {
    countryCode: "US",
    phoneNumberType: "tollFree",
    assignmentType: "application",
    capabilities: {
      sms: "outbound",
      calling: "none"
    },
    areaCode: "833",
    quantity: 1
  };

const searchPoller = await phoneNumbersClient.beginSearchAvailablePhoneNumbers(searchRequest);

// The search is underway. Wait to receive searchId.
const { searchId, phoneNumbers } = searchPoller.pollUntilDone();
const phoneNumber = phoneNumbers[0];

console.log(`Found phone number: ${phoneNumber}`);
console.log(`searchId: ${searchId}`);
```

### <a name="purchase-phone-number"></a>Kaufen von Telefonnummern

Das Ergebnis der Suche nach Telefonnummern ist ein `PhoneNumberSearchResult`. Hierin ist ein Wert `searchId` enthalten, der an die API zum Kaufen von Nummern übermittelt werden kann, um die mit der Suche ermittelten Nummern zu erwerben. Beachten Sie, dass der Aufruf der API zum Kaufen von Telefonnummern für Ihr Azure-Konto zu einer Abrechnung führt.

Fügen Sie Ihrer `main`-Funktion den unten angegebenen Codeausschnitt hinzu:

```javascript
/**
 * Purchase Phone Number
 */

const purchasePoller = await phoneNumbersClient.beginPurchasePhoneNumbers(searchId);

// Purchase is underway.
await purchasePoller.pollUntilDone();
console.log(`Successfully purchased ${phoneNumber}`);
```

### <a name="update-phone-number-capabilities"></a>Aktualisieren der Funktionen für Telefonnummern

Fügen Sie, nachdem Sie nun eine Telefonnummer erworben haben, den folgenden Code hinzu, um ihre Funktionen zu aktualisieren:

```javascript
/**
 * Update Phone Number Capabilities
 */

// Create update request.
// This will update phone number to send and receive sms, but only send calls.
const updateRequest = {
  sms: "inbound+outbound",
  calling: "outbound"
};

const updatePoller = await phoneNumbersClient.beginUpdatePhoneNumberCapabilities(
  phoneNumber,
  updateRequest
);

// Update is underway.
await updatePoller.pollUntilDone();
console.log("Phone number updated successfully.");
```

### <a name="get-purchased-phone-numbers"></a>Erhalten von erworbenen Telefonnummern

Nach dem Kauf einer Nummer können Sie diese vom Client abrufen. Fügen Sie zu ihrer `main`-Funktion den folgenden Code hinzu, um die soeben erworbene Telefonnummer zu erhalten:

```javascript
/**
 * Get Purchased Phone Number
 */

const { capabilities } = await phoneNumbersClient.getPurchasedPhoneNumber(phoneNumber);
console.log(`These capabilities: ${capabilities}, should be the same as these: ${updateRequest}.`);
```

Sie können auch alle erworbenen Telefonnummern abrufen.

```javascript
const phoneNumbers = await phoneNumbersClient.listPurchasedPhoneNumbers();

for await (const purchasedPhoneNumber of phoneNumbers) {
  console.log(`Phone number: ${purchasedPhoneNumber.phoneNumber}, country code: ${purchasedPhoneNumber.countryCode}.`);
}
```

### <a name="release-phone-number"></a>Freigeben einer Telefonnummer

Nun können Sie die erworbene Telefonnummer freigeben. Fügen Sie zur `main`-Funktion den folgenden Codeausschnitt hinzu:

```javascript
/**
 * Release Purchased Phone Number
 */

const releasePoller = await client.beginReleasePhoneNumber(phoneNumber);

// Release is underway.
await releasePoller.pollUntilDone();
console.log("Successfully release phone number.");
```

## <a name="run-the-code"></a>Ausführen des Codes

Verwenden Sie den Befehl `node`, um den Code auszuführen, den Sie zur Datei **phone-numbers-quickstart.js** hinzugefügt haben.

```console
node phone-numbers-quickstart.js
```