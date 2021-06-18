---
ms.openlocfilehash: c312b33a63c179d9af8347e268d400c5e48b3921
ms.sourcegitcommit: c385af80989f6555ef3dadc17117a78764f83963
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2021
ms.locfileid: "111430132"
---
> [!NOTE]
> Den fertigen Code für diesen Schnellstart finden Sie auf [GitHub](https://github.com/Azure-Samples/communication-services-java-quickstarts/tree/main/PhoneNumbers).

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- [Java Development Kit (JDK)](/java/azure/jdk/?preserve-view=true&view=azure-java-stable), Version 8 oder höher.
- [Apache Maven](https://maven.apache.org/download.cgi): Eine bereitgestellte Communication Services-Ressource und eine Verbindungszeichenfolge. [Erstellen Sie eine Communication Services-Ressource.](../../create-communication-resource.md)

## <a name="setting-up"></a>Einrichten

### <a name="create-a-new-java-application"></a>Erstellen einer neuen Java-Anwendung

Öffnen Sie Ihr Terminal oder Befehlsfenster. Navigieren Sie zu dem Verzeichnis, in dem Sie Ihre Java-Anwendung erstellen möchten. Führen Sie den unten angegebenen Befehl aus, um das Java-Projekt auf der Grundlage der Vorlage „maven-archetype-quickstart“ zu generieren.

```console
mvn archetype:generate -DgroupId=com.communication.quickstart -DartifactId=communication-quickstart -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeVersion=1.4 -DinteractiveMode=false
```

Wie Sie sehen, wurde durch die Aufgabe „generate“ ein Verzeichnis erstellt, das den gleichen Namen besitzt wie die Artefakt-ID (`artifactId`). In diesem Verzeichnis enthält das Verzeichnis „src/main/java“ den Quellcode des Projekts. Das Verzeichnis `src/test/java directory` enthält die Testquelle, und die Datei `pom.xml` ist das Projektobjektmodell (POM) des Projekts.

### <a name="install-the-package"></a>Installieren des Pakets

Öffnen Sie die Datei **pom.xml** in Ihrem Text-Editor. Fügen Sie der Gruppe der Abhängigkeiten das folgende Abhängigkeitselement hinzu:

```xml
<dependency>
    <groupId>com.azure</groupId>
    <artifactId>azure-communication-phonenumbers</artifactId>
    <version>1.0.0</version>
</dependency>

<dependency>
    <groupId>com.azure</groupId>
    <artifactId>azure-identity</artifactId>
    <version>1.2.3</version>
</dependency>
```

### <a name="set-up-the-app-framework"></a>Einrichten des App-Frameworks

Über das Projektverzeichnis:

1. Navigieren Sie zum Verzeichnis */src/main/java/com/communication/quickstart*.
1. Öffnen Sie die Datei *App.java* im Editor.
1. Ersetzen Sie die Anweisung `System.out.println("Hello world!");`.
1. Fügen Sie Anweisungen vom Typ `import` hinzu.

Verwenden Sie zum Einstieg den folgenden Code:

```java
import com.azure.communication.phonenumbers.*;
import com.azure.communication.phonenumbers.models.*;
import com.azure.core.http.rest.*;
import com.azure.core.util.Context;
import com.azure.core.util.polling.LongRunningOperationStatus;
import com.azure.core.util.polling.PollResponse;
import com.azure.core.util.polling.SyncPoller;
import com.azure.identity.*;
import java.io.*;

public class App
{
    public static void main( String[] args ) throws IOException
    {
        System.out.println("Azure Communication Services - Phone Numbers Quickstart");
        // Quickstart code goes here
    }
}
```

## <a name="authenticate-the-phone-numbers-client"></a>Authentifizieren des Phone Numbers-Clients

Der PhoneNumberClientBuilder ist für die Verwendung Azure Active Directory-Authentifizierung aktiviert
<!-- embedme ./src/samples/java/com/azure/communication/phonenumbers/ReadmeSamples.java#L52-L62 -->
```java
// You can find your endpoint and access key from your resource in the Azure portal
String endpoint = "https://<RESOURCE_NAME>.communication.azure.com";

PhoneNumbersClient phoneNumberClient = new PhoneNumbersClientBuilder()
    .endpoint(endpoint)
    .credential(new DefaultAzureCredentialBuilder().build())
    .buildClient();
```

Alternativ können Sie auch den Endpunkt und den Zugriffsschlüssel aus der Kommunikationsressource zur Authentifizierung verwenden.
<!-- embedme ./src/samples/java/com/azure/communication/phonenumbers/ReadmeSamples.java#L30-L41 -->
```java
// You can find your connection string from your resource in the Azure portal
String connectionString = "endpoint=https://<RESOURCE_NAME>.communication.azure.com/;accesskey=<ACCESS_KEY>";

PhoneNumbersClient phoneNumberClient = new PhoneNumbersClientBuilder()
    .connectionString(connectionString)
    .buildClient();
```

## <a name="manage-phone-numbers"></a>Verwalten von Telefonnummern

### <a name="search-for-available-phone-numbers"></a>Suchen nach verfügbaren Telefonnummern

Um Telefonnummern zu kaufen, müssen Sie zuerst nach verfügbaren Telefonnummern suchen. Geben Sie für die Suche nach Telefonnummern die Vorwahl, den Zuweisungstyp, die [Funktionen der Telefonnummer](../../../concepts/telephony-sms/plan-solution.md#phone-number-capabilities-in-azure-communication-services), den [Telefonnummerntyp](../../../concepts/telephony-sms/plan-solution.md#phone-number-types-in-azure-communication-services) und die Menge an. Beachten Sie, dass die Angabe der Vorwahl für gebührenfreie Telefonnummern nicht zwingend ist.

```java
 PhoneNumberCapabilities capabilities = new PhoneNumberCapabilities()
    .setCalling(PhoneNumberCapabilityType.INBOUND)
    .setSms(PhoneNumberCapabilityType.INBOUND_OUTBOUND);
PhoneNumberSearchOptions searchOptions = new PhoneNumberSearchOptions().setAreaCode("833").setQuantity(1);

SyncPoller<PhoneNumberOperation, PhoneNumberSearchResult> poller = phoneNumberClient
    .beginSearchAvailablePhoneNumbers("US", PhoneNumberType.TOLL_FREE, PhoneNumberAssignmentType.APPLICATION, capabilities, searchOptions, Context.NONE);
PollResponse<PhoneNumberOperation> response = poller.waitForCompletion();
String searchId = "";

if (LongRunningOperationStatus.SUCCESSFULLY_COMPLETED == response.getStatus()) {
    PhoneNumberSearchResult searchResult = poller.getFinalResult();
    searchId = searchResult.getSearchId();
    System.out.println("Searched phone numbers: " + searchResult.getPhoneNumbers());
    System.out.println("Search expires by: " + searchResult.getSearchExpiresBy());
    System.out.println("Phone number costs:" + searchResult.getCost().getAmount());
}
```

### <a name="purchase-phone-numbers"></a>Kaufen von Telefonnummern

Das Ergebnis der Suche nach Telefonnummern ist ein PhoneNumberSearchResult. Hierin ist ein Wert `searchId` enthalten, der an die API zum Kaufen von Nummern übermittelt werden kann, um die mit der Suche ermittelten Nummern zu erwerben. Beachten Sie, dass der Aufruf der API zum Kaufen von Telefonnummern für Ihr Azure-Konto zu einer Abrechnung führt.

```java
PollResponse<PhoneNumberOperation> purchaseResponse = phoneNumberClient.beginPurchasePhoneNumbers(searchId, Context.NONE).waitForCompletion();
System.out.println("Purchase phone numbers operation is: " + purchaseResponse.getStatus());
```

### <a name="get-phone-numbers"></a>Telefonnummern erhalten

Nach dem Kauf einer Nummer können Sie diese vom Client abrufen.
```java
PurchasedPhoneNumber phoneNumber = phoneNumberClient.getPurchasedPhoneNumber("+14255550123");
System.out.println("Phone Number Country Code: " + phoneNumber.getCountryCode());
```

Sie können auch alle erworbenen Telefonnummern abrufen.
``` java
PagedIterable<PurchasedPhoneNumber> phoneNumbers = phoneNumberClient.listPurchasedPhoneNumbers(Context.NONE);
PurchasedPhoneNumber phoneNumber = phoneNumbers.iterator().next();
System.out.println("Phone Number Country Code: " + phoneNumber.getCountryCode());
```

### <a name="update-phone-number-capabilities"></a>Funktionen für Telefonnummern

Mit einer erworbenen Nummer können Sie die Funktionen aktualisieren.
```java
PhoneNumberCapabilities capabilities = new PhoneNumberCapabilities();
capabilities
    .setCalling(PhoneNumberCapabilityType.INBOUND)
    .setSms(PhoneNumberCapabilityType.INBOUND_OUTBOUND);

SyncPoller<PhoneNumberOperation, PurchasedPhoneNumber> poller = phoneNumberClient.beginUpdatePhoneNumberCapabilities("+18001234567", capabilities, Context.NONE);
PollResponse<PhoneNumberOperation> response = poller.waitForCompletion();
if (LongRunningOperationStatus.SUCCESSFULLY_COMPLETED == response.getStatus()) {
    PurchasedPhoneNumber phoneNumber = poller.getFinalResult();
    System.out.println("Phone Number Calling capabilities: " + phoneNumber.getCapabilities().getCalling()); //Phone Number Calling capabilities: inbound
    System.out.println("Phone Number SMS capabilities: " + phoneNumber.getCapabilities().getSms()); //Phone Number SMS capabilities: inbound+outbound
}
```

### <a name="release-phone-number"></a>Freigeben einer Telefonnummer

Sie können eine erworbene Telefonnummer freigeben.
```java
PollResponse<PhoneNumberOperation> releaseResponse =
    phoneNumberClient.beginReleasePhoneNumber("+14255550123", Context.NONE).waitForCompletion();
System.out.println("Release phone number operation is: " + releaseResponse.getStatus());
```

## <a name="run-the-code"></a>Ausführen des Codes

Navigieren Sie zum Verzeichnis, das die Datei *pom.xml* enthält, und kompilieren Sie das Projekt mit dem folgenden `mvn`-Befehl.

```console
mvn compile
```

Erstellen Sie dann das Paket.

```console
mvn package
```

Führen Sie die App mit dem folgenden `mvn`-Befehl aus.

```console
mvn exec:java -Dexec.mainClass="com.communication.quickstart.App" -Dexec.cleanupDaemonThreads=false
```

In der Ausgabe der App wird die jeweils abgeschlossene Aktion beschrieben:
<!---cSpell:disable --->
```console
Azure Communication Services - Phone Numbers Quickstart

Searched phone numbers: [+18001234567]

Purchase phone numbers operation is: SUCCESSFULLY_COMPLETED

Phone Number Country Code: US

Phone Number Calling capabilities: inbound

Phone Number SMS capabilities: inbound

Release phone number operation is: SUCCESSFULLY_COMPLETED

```
<!---cSpell:enable --->