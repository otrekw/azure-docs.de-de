---
ms.openlocfilehash: 0bfb23977f6553568da24df614621bdf1eb9d06d
ms.sourcegitcommit: 5fd1f72a96f4f343543072eadd7cdec52e86511e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/01/2021
ms.locfileid: "106112808"
---
## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Die aktuelle Version der [.NET Core-Clientbibliothek](https://dotnet.microsoft.com/download/dotnet-core) für Ihr Betriebssystem.
- Eine aktive Communication Services-Ressource und eine Verbindungszeichenfolge. [Erstellen Sie eine Communication Services-Ressource.](../../create-communication-resource.md)

### <a name="prerequisite-check"></a>Prüfen der Voraussetzungen

- Führen Sie in einem Terminal- oder Befehlsfenster den Befehl `dotnet` aus, um sich zu vergewissern, dass die .NET-Clientbibliothek installiert ist.

## <a name="setting-up"></a>Einrichten

### <a name="create-a-new-c-application"></a>Erstellen einer neuen C#-Anwendung

Verwenden Sie in einem Konsolenfenster (z. B. cmd, PowerShell oder Bash) den Befehl `dotnet new` zum Erstellen einer neuen Konsolen-App mit dem Namen `PhoneNumbersQuickstart`. Dieser Befehl erstellt ein einfaches „Hallo Welt“-C#-Projekt mit einer einzigen Quelldatei: **Program.cs**.

```console
dotnet new console -o PhoneNumbersQuickstart
```

Wechseln Sie zum neu erstellten App-Ordner, und verwenden Sie den Befehl `dotnet build`, um Ihre Anwendung zu kompilieren.

```console
cd PhoneNumbersQuickstart
dotnet build
```

### <a name="install-the-package"></a>Installieren des Pakets

Installieren Sie im Anwendungsverzeichnis mithilfe des Befehls `dotnet add package` die Azure Communication PhoneNumbers-Clientbibliothek für das .NET-Paket.

```console
dotnet add package Azure.Communication.PhoneNumbers --version 1.0.0-beta.6
```

Fügen Sie am Anfang von **Program.cs** eine Anweisung vom Typ `using` hinzu, um den Namespace einzuschließen.

```csharp
using System;
using System.Linq;
using System.Threading.Tasks;
using Azure.Communication.PhoneNumbers;
```

Ändern Sie die `Main`-Funktionssignatur in asynchron.

```csharp
static async Task Main(string[] args)
{
  ...
}
```

## <a name="authenticate-the-client"></a>Authentifizieren des Clients

Telefonnummer-Clients können mithilfe von Verbindungszeichenfolgen authentifiziert werden, die von Azure-Kommunikationsressourcen im [Azure-Portal] [azure_portal] abgerufen werden.

```csharp
// Get a connection string to our Azure Communication resource.
var connectionString = "<connection_string>";
var client = new PhoneNumbersClient(connectionString);
```

Telefonnummern-Clients haben zudem die Möglichkeit, sich mittels Azure Active Directory-Authentifizierung zu authentifizieren. Mit dieser Option müssen die Umgebungsvariablen `AZURE_CLIENT_SECRET`, `AZURE_CLIENT_ID` und `AZURE_TENANT_ID` für die Authentifizierung eingerichtet werden.

```csharp
// Get an endpoint to our Azure Communication resource.
var endpoint = new Uri("<endpoint_url>");
TokenCredential tokenCredential = new DefaultAzureCredential();
client = new PhoneNumbersClient(endpoint, tokenCredential);
```

## <a name="manage-phone-numbers"></a>Verwalten von Telefonnummern

### <a name="search-for-available-phone-numbers"></a>Suchen nach verfügbaren Telefonnummern

Um Telefonnummern zu kaufen, müssen Sie zuerst nach verfügbaren Telefonnummern suchen. Geben Sie für die Suche nach Telefonnummern die Vorwahl, den Zuweisungstyp, die [Funktionen der Telefonnummer](../../../concepts/telephony-sms/plan-solution.md#phone-number-capabilities-in-azure-communication-services), den [Telefonnummerntyp](../../../concepts/telephony-sms/plan-solution.md#phone-number-types-in-azure-communication-services) und die Menge an. Beachten Sie, dass die Angabe der Vorwahl für gebührenfreie Telefonnummern nicht zwingend ist.

```csharp
var capabilities = new PhoneNumberCapabilities(calling:PhoneNumberCapabilityType.None, sms:PhoneNumberCapabilityType.Outbound);
var searchOptions = new PhoneNumberSearchOptions { AreaCode = "833", Quantity = 1 };

var searchOperation = await client.StartSearchAvailablePhoneNumbersAsync("US", PhoneNumberType.TollFree, PhoneNumberAssignmentType.Application, capabilities, searchOptions);
await searchOperation.WaitForCompletionAsync();
```

### <a name="purchase-phone-numbers"></a>Kaufen von Telefonnummern

Das Ergebnis der Suche nach Telefonnummern ist ein `PhoneNumberSearchResult`. Hierin ist ein Wert `SearchId` enthalten, der an die API zum Kaufen von Nummern übermittelt werden kann, um die mit der Suche ermittelten Nummern zu erwerben. Beachten Sie, dass der Aufruf der API zum Kaufen von Telefonnummern für Ihr Azure-Konto zu einer Abrechnung führt.

```csharp
var purchaseOperation = await client.StartPurchasePhoneNumbersAsync(searchOperation.Value.SearchId);
await purchaseOperation.WaitForCompletionResponseAsync();
```

### <a name="get-phone-numbers"></a>Telefonnummern erhalten

Nach dem Kauf einer Nummer können Sie diese vom Client abrufen.
```csharp
var getPhoneNumberResponse = await client.GetPurchasedPhoneNumberAsync("+14255550123");
Console.WriteLine($"Phone number: {getPhoneNumberResponse.Value.PhoneNumber}, country code: {getPhoneNumberResponse.Value.CountryCode}");
```

Sie können auch alle erworbenen Telefonnummern abrufen.
``` csharp
var purchasedPhoneNumbers = client.GetPurchasedPhoneNumbersAsync();
await foreach (var purchasedPhoneNumber in purchasedPhoneNumbers)
{
    Console.WriteLine($"Phone number: {purchasedPhoneNumber.PhoneNumber}, country code: {purchasedPhoneNumber.CountryCode}");
}
```

### <a name="update-phone-number-capabilities"></a>Funktionen für Telefonnummern

Mit einer erworbenen Nummer können Sie die Funktionen aktualisieren.

````csharp
var updateCapabilitiesOperation = await client.StartUpdateCapabilitiesAsync("+14255550123", calling: PhoneNumberCapabilityType.Outbound, sms: PhoneNumberCapabilityType.InboundOutbound);
await updateCapabilitiesOperation.WaitForCompletionAsync();
````


### <a name="release-phone-number"></a>Freigeben einer Telefonnummer

Sie können eine erworbene Telefonnummer freigeben.

````csharp
var releaseOperation = await client.StartReleasePhoneNumberAsync("+14255550123");
await releaseOperation.WaitForCompletionResponseAsync();
````

## <a name="run-the-code"></a>Ausführen des Codes

Führen Sie die Anwendung mit dem Befehl `dotnet run` aus dem Anwendungsverzeichnis aus.

```console
dotnet run
```

## <a name="sample-code"></a>Beispielcode

Sie können die Beispiel-App von [GitHub](https://github.com/Azure-Samples/communication-services-dotnet-quickstarts/tree/main/PhoneNumbers) herunterladen.
