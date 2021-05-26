---
title: Verwenden der clientseitigen Verschlüsselung mit Always Encrypted für Azure Cosmos DB
description: Erfahren Sie, wie Sie die clientseitigen Verschlüsselung mit Always Encrypted für Azure Cosmos DB verwenden
author: ThomasWeiss
ms.service: cosmos-db
ms.topic: how-to
ms.date: 05/25/2021
ms.author: thweiss
ms.openlocfilehash: d8aaa34fe2e81515866c047c6a171b1bdb58ff3f
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110385698"
---
# <a name="use-client-side-encryption-with-always-encrypted-for-azure-cosmos-db-preview"></a>Verwenden der clientseitigen Verschlüsselung mit Always Encrypted für Azure Cosmos DB (Vorschau)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

Always Encrypted ist eine Funktion zum Schutz sensibler Daten, wie Kreditkartennummern oder Personalausweisnummern (z.B. US-Sozialversicherungsnummern), die in Azure Cosmos DB gespeichert sind. Mit Always Encrypted können Clients, vertrauliche Daten innerhalb von Clientanwendungen verschlüsseln wobei die Verschlüsselungsschlüssel niemals an den Server weitergegeben werden.

Always Encrypted bietet clientseitige Verschlüsselungsfunktionen für Azure Cosmos DB. Die clientseitige Verschlüsselung Ihrer Daten kann in den folgenden Szenarien erforderlich sein:

- **Schützen vertraulicher Daten mit bestimmten Vertraulichkeitsmerkmalen**: Always Encrypted ermöglicht Clients das Verschlüsseln vertraulicher Daten in ihren Anwendungen und gibt niemals die Nur-Text-Daten oder Verschlüsselungsschlüssel für den Azure Cosmos DB Dienst preis.
- **Implementieren der Zugriffssteuerung pro Eigenschaft**: Da die Verschlüsselung mit Schlüsseln gesteuert wird, die Sie von Azure Key Vault besitzen und verwalten, können Sie Zugriffsrichtlinien anwenden, um zu steuern, auf welche sensiblen Eigenschaften jeder Client Zugriff hat.

> [!IMPORTANT]
> Always Encrypted für Azure Cosmos DB liegt aktuell in einer Vorschauversion vor. Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Voraussetzungen

Um die Vorschauversion von Always Encrypted für Azure Cosmos DB verwenden zu können, haben Sie die folgenden Möglichkeiten:

- Verwenden Sie die Version 2.11.13.0 oder höher des [Azure Cosmos DB-Emulators für lokale Umgebungen](local-emulator.md).
- Anforderung der Aktivierung für die Vorschau für Ihr Azure Cosmos DB-Konto durch Ausfüllen [dieses Formulars](https://ncv.microsoft.com/poTcF52I6N).

> [!TIP]
> Möchten Sie uns Feedback zur Vorschau von Always Encrypted für Azure Cosmos DB geben? Wenden Sie sich an: [azurecosmosdbcse@service.microsoft.com](mailto:azurecosmosdbcse@service.microsoft.com).

## <a name="concepts"></a>Konzepte

Always Encrypted für Azure Cosmos DB führt einige neue Konzepte, die an der Konfiguration Ihrer clientseitigen Verschlüsselung beteiligt sind, ein.

### <a name="encryption-keys"></a>Verschlüsselungsschlüssel

#### <a name="data-encryption-keys"></a>Datenverschlüsselungsschlüssel

Wenn Sie Always Encrypted einsetzen, werden Daten mit Datenverschlüsselungsschlüsseln ( Data Encryption Key, DEK) verschlüsselt, die im Voraus erstellt werden sollten. Diese DEKs werden im Azure Cosmos DB gespeichert und auf Datenbankebene definiert, sodass ein DEK über mehrere Container hinweg freigegeben werden kann. Die Erstellung der DEKs erfolgt clientseitig mithilfe des Azure Cosmos DB Software Development Kit.

Sie haben folgende Möglichkeiten:

- Erstellen Sie einen DEK pro zu verschlüsselnden Eigenschaft, oder
- Verwenden Sie denselben DEK, um mehrere Eigenschaften zu verschlüsseln.

#### <a name="customer-managed-keys"></a>Vom Kunden verwaltete Schlüssel

Bevor DEKs in einem Azure Cosmos DB gespeichert werden, werden sie von einem vom Kunden verwalteten Schlüssel (Customer-Managed Key, CMK) gepackt. Durch Steuern des Packens und Entpackens von DEKs steuern CMKs effektiv den Zugriff auf die Daten, die mit ihren entsprechenden DEKs verschlüsselt sind. Der CMK-Speicher ist als erweiterbares/Plug-In-Modell mit einer Standardimplementierung konzipiert, die erwartet, dass sie in Azure Key Vault gespeichert werden.

:::image type="content" source="./media/how-to-always-encrypted/encryption-keys.png" alt-text="Verschlüsselungsschlüssel" border="true":::

### <a name="encryption-policy"></a>Verschlüsselungsrichtlinie

Ähnlich wie bei einer [Indizierungsrichtlinie](index-policy.md) ist eine Verschlüsselungsrichtlinie eine Spezifikation auf Containerebene, die beschreibt, wie JSON-Eigenschaften verschlüsselt werden sollen. Diese Richtlinie muss bereitgestellt werden, wenn der Container erstellt wird und unveränderlich ist. In der aktuellen Version können Sie die Verschlüsselungsrichtlinie nicht aktualisieren.

Für jede Eigenschaft, die Sie verschlüsseln möchten, definiert die Verschlüsselungsrichtlinie Folgendes:

- Der Pfad der Eigenschaft in Form von `/property`. Derzeit werden nur Pfade der obersten Ebene unterstützt, geschachtelte Pfade wie `/path/to/property` werden nicht unterstützt.
- Die ID des [DEK](#data-encryption-keys), der beim Verschlüsseln und Entschlüsseln der Eigenschaft verwendet wird.
- Ein Verschlüsselungstyp. Er kann entweder zufällig oder deterministisch sein.
- Der Verschlüsselungsalgorithmus, der beim Verschlüsseln der Eigenschaft verwendet wird. Der angegebene Algorithmus kann den beim Erstellen des Schlüssels definierten Algorithmus überschreiben, wenn sie kompatibel sind.

> [!NOTE]
> Die folgenden Eigenschaften können nicht verschlüsselt werden:
> - ID
> - Der Container-Partitionsschlüssel

#### <a name="randomized-vs-deterministic-encryption"></a>Zufällige im Vergleich zu deterministischer Verschlüsselung

Der Azure Cosmos DB Dienst erkennt nie den Nur-Text von Eigenschaften, die mit Always Encrypted verschlüsselt sind. Er unterstützt jedoch weiterhin einige Abfragefunktionen für die verschlüsselten Daten, abhängig vom Verschlüsselungstyp, der für eine Eigenschaft verwendet wird. Always Encrypted unterstützt die folgenden zwei Verschlüsselungstypen:

- Bei der **deterministischen Verschlüsselung** wird immer derselbe verschlüsselte Wert für einen gegebenen Nur-Textwert und eine gegebene Verschlüsselungskonfiguration generiert. Mithilfe der deterministischen Verschlüsselung können Abfragen Gleichheitsfilter für verschlüsselte Eigenschaften ausgeführt werden. Es kann Angreifern jedoch ermöglichen, Informationen zu verschlüsselten Werten zu erraten, indem sie die Muster in der verschlüsselten Eigenschaft untersuchen. Dies gilt besonders, wenn es sich um einen kleinen Satz möglicher verschlüsselter Werte handelt, beispielsweise WAHR/FALSCH, oder die Regionen „Nord“/ „Süd“/ „Ost“/ „West“.

- **Zufälligen Verschlüsselung**: Es wird eine Methode verwendet, die Daten in einer weniger vorhersagbaren Weise verschlüsselt. Die zufällige Verschlüsselung ist sicherer, verhindert jedoch, dass Abfragen auf verschlüsselte Eigenschaften gefiltert werden können.

## <a name="setup-azure-key-vault"></a>Einrichten von Azure Key Vault

Der erste Schritt um mit Always Encrypted zu beginnen besteht im Erstellen Ihrer CMKs in Azure Key Vault:

1. Erstellen einer neue Azure Key Vault-Instanz, oder navigieren zu einer vorhandenen Instanz.
1. Erstellen Sie im Abschnitt **Schlüssel** einen neuen Schlüssel.
1. Navigieren Sie nach dem Erstellen des Schlüssels zu seiner aktuellen Version, und kopieren Sie den vollständigen Schlüsselbezeichner:<br>`https://<my-key-vault>.vault.azure.net/keys/<key>/<version>`. Wenn Sie die Schlüsselversion am Ende des Schlüsselbezeichners weglassen, wird die neueste Version des Schlüssels verwendet.

Als Nächstes müssen Sie konfigurieren, wie das Azure Cosmos DB SDK auf Ihre Azure Key Vault zugreifen soll. Diese Authentifizierung erfolgt über eine Azure Active Directory (AD) Identität. Höchstwahrscheinlich verwenden Sie die Identität einer Azure AD-Anwendung oder einer [verwalteten Identität](../active-directory/managed-identities-azure-resources/overview.md) als Proxy zwischen Ihrem Clientcode und Ihrer Azure Key Vault-Instanz, obwohl jede Art von Identität verwendet werden kann. Führen Sie die folgenden Schritte aus, um eine Azure AD-Anwendung als Proxy zu verwenden:

1. Erstellen Sie eine neue Anwendung, und fügen Sie einen geheimen Clientgeheimnis hinzu, wie in dieser [Schnellstartanleitung](../active-directory/develop/quickstart-register-app.md) beschrieben.

1. Wechseln Sie zurück zu Ihrer Azure Key Vault-Instanz, navigieren Sie zum Abschnitt **Zugriffsrichtlinien** und fügen Sie eine neue Richtlinie hinzu:

   1. Wählen Sie unter **Schlüsselberechtigungen** die Optionen **Abrufen**, **Liste**, **Schlüssel entpacken**, **Schlüssel verpacken**, **Bestätigen** und **Abmelden** aus.
   1. Suchen Sie unter **Prinzipal auswählen** nach der AAD-Anwendung, die Sie zuvor erstellt haben.

### <a name="protect-your-cmk-from-accidental-deletion"></a>Schützen Ihres CMK vor versehentlichem Löschen

Um sicherzustellen, dass Sie nach dem versehentlichen Löschen Ihres CMK nicht den Zugriff auf Ihre verschlüsselten Daten verlieren, wird empfohlen, zwei Eigenschaften für Ihre Azure Key Vault-Instanz zu setzen: **vorläufiges Löschen** und **Löschschutz**.

Wenn Sie eine neue Azure Key Vault-Instanz erstellen, aktivieren Sie diese Eigenschaften während der Erstellung:

:::image type="content" source="./media/how-to-setup-cmk/portal-akv-prop.png" alt-text="Aktivieren Sie vorläufiges Löschen und Löschschutz für eine neue Azure Key Vault-Instanz":::

Bei Verwendung einer vorhandenen Azure Key Vault-Instanz können Sie überprüfen, ob diese Eigenschaften aktiviert sind, indem Sie sich im Azure-Portal den Abschnitt **Eigenschaften** ansehen. Ist eine dieser Eigenschaften nicht aktiviert, finden Sie in den Abschnitten „Aktivieren des vorläufigen Löschens“ und „Aktivieren des Bereinigungsschutzes“ in einem der folgenden Artikel weitere Informationen:

- [Verwenden des vorläufigen Löschens mit PowerShell](../key-vault/general/key-vault-recovery.md)
- [Verwenden des vorläufigen Löschens mit Azure CLI](../key-vault/general/key-vault-recovery.md)

## <a name="initialize-the-sdk"></a>Initialisieren des SDK

> [!NOTE]
> Always Encrypted für Azure Cosmos DB wird aktuell unterstützt:
> - In **.NET** mit dem [Microsoft.Azure.Cosmos.Encryption-Paket](https://www.nuget.org/packages/Microsoft.Azure.Cosmos.Encryption).
> - In **Java** mit dem [Azure.Cosmos.Encryption-Paket](https://mvnrepository.com/artifact/com.azure/azure-cosmos-encryption).

Für die Verwendung von Always Encrypted, muss eine Instanz von `EncryptionKeyStoreProvider` an Ihre Azure Cosmos DB SDK-Instanz angefügt werden. Dieses Objekt wird verwendet, um mit dem Schlüsselspeicher zu interagieren, der Ihre CMKs hostet. Der Standardschlüsselspeicheranbieter für Azure Key Vault heißt `AzureKeyVaultKeyStoreProvider`.

Die folgenden Ausschnitte zeigen, wie die Identität einer Azure AD Anwendung mit einem geheimen Clientschlüssel verwendet wird. Hier finden Sie Beispiele für das Erstellen verschiedener Arten von `TokenCredential`-Klassen:

- [In .NET](/dotnet/api/overview/azure/identity-readme#credential-classes)
- [In Java](/java/api/overview/azure/identity-readme#credential-classes)

# <a name="net"></a>[.NET](#tab/dotnet)

> [!NOTE]
> In .NET benötigen Sie das zusätzliche [Microsoft.Data.Encryption.AzureKeyVaultProvider-Paket](https://www.nuget.org/packages/Microsoft.Data.Encryption.AzureKeyVaultProvider), um auf die `AzureKeyVaultKeyStoreProvider`-Klasse zuzugreifen.

```csharp
var tokenCredential = new ClientSecretCredential(
    "<aad-app-tenant-id>", "<aad-app-client-id>", "<aad-app-secret>");
var keyStoreProvider = new AzureKeyVaultKeyStoreProvider(tokenCredential);
var client = new CosmosClient("<connection-string>")
    .WithEncryption(keyStoreProvider);
```

# <a name="java"></a>[Java](#tab/java)

```java
TokenCredential tokenCredential = new ClientSecretCredentialBuilder()
    .authorityHost("https://login.microsoftonline.com")
    .tenantId("<aad-app-tenant-id>")
    .clientId("<aad-app-client-id>")
    .clientSecret("<aad-app-secret>")
    .build();
AzureKeyVaultKeyStoreProvider encryptionKeyStoreProvider =
    new AzureKeyVaultKeyStoreProvider(tokenCredential);
CosmosAsyncClient client = new CosmosClientBuilder()
    .endpoint("<endpoint>")
    .key("<primary-key>")
    .buildAsyncClient();
EncryptionAsyncCosmosClient encryptionClient =
    EncryptionAsyncCosmosClient.buildEncryptionAsyncClient(client, encryptionKeyStoreProvider);
```
---

## <a name="create-a-data-encryption-key"></a>Erstellen eines Datenverschlüsselungsschlüssel

Bevor Daten in einem Container verschlüsselt werden können, muss ein [Datenverschlüsselungsschlüssel](#data-encryption-keys) in der übergeordneten Datenbank erstellt werden. Dieser Vorgang wird durch Aufrufen der `CreateClientEncryptionKeyAsync`-Methode und Übergabe durchgeführt:

- Eine Zeichenfolgenerkennung, die den Schlüssel in der Datenbank eindeutig identifiziert.
- Der Verschlüsselungsalgorithmus, der mit dem Schlüssel verwendet werden soll. Derzeit wird nur ein Algorithmus unterstützt.
- Der Schlüsselbezeichner des in Azure Key Vault gespeicherten [CMK](#customer-managed-keys). Dieser Parameter wird in ein generisches `EncryptionKeyWrapMetadata`-Objekt übergeben, wobei der `name` ein beliebiger Anzeigename sein kann und `value` der Schlüsselbezeichner sein muss.

# <a name="net"></a>[.NET](#tab/dotnet)

```csharp
var database = client.GetDatabase("my-database");
await database.CreateClientEncryptionKeyAsync(
    "my-key",
    DataEncryptionKeyAlgorithm.AEAD_AES_256_CBC_HMAC_SHA256,
    new EncryptionKeyWrapMetadata(
        keyStoreProvider.ProviderName,
        "akvKey",
        "https://<my-key-vault>.vault.azure.net/keys/<key>/<version>"));
```

# <a name="java"></a>[Java](#tab/java)

```java
EncryptionCosmosAsyncDatabase database =
    client.getEncryptedCosmosAsyncDatabase("my-database");
database.createClientEncryptionKey(
    "my-key",
    CosmosEncryptionAlgorithm.AEAES_256_CBC_HMAC_SHA_256,
    new EncryptionKeyWrapMetadata(
        "akvKey",
        "https://<my-key-vault>.vault.azure.net/keys/<key>/<version>"));
```
---

## <a name="create-a-container-with-encryption-policy"></a>Erstellen eines Containers mit einer Verschlüsselungsrichtlinie

Geben Sie beim Erstellen des Containers die Verschlüsselungsrichtlinie auf der Containerebene an.

# <a name="net"></a>[.NET](#tab/dotnet)

```csharp
var path1 = new ClientEncryptionIncludedPath
{
    Path = "/property1",
    ClientEncryptionKeyId = "my-key",
    EncryptionType = EncryptionType.Deterministic.ToString(),
    EncryptionAlgorithm = DataEncryptionKeyAlgorithm.AEAD_AES_256_CBC_HMAC_SHA256.ToString()
};
var path2 = new ClientEncryptionIncludedPath
{
    Path = "/property2",
    ClientEncryptionKeyId = "my-key",
    EncryptionType = EncryptionType.Randomized.ToString(),
    EncryptionAlgorithm = DataEncryptionKeyAlgorithm.AEAD_AES_256_CBC_HMAC_SHA256.ToString()
};
await database.DefineContainer("my-container", "/partition-key")
    .WithClientEncryptionPolicy()
    .WithIncludedPath(path1)
    .WithIncludedPath(path2)
    .Attach()
    .CreateAsync();
```

# <a name="java"></a>[Java](#tab/java)

```java
ClientEncryptionIncludedPath path1 = new ClientEncryptionIncludedPath();
path1.clientEncryptionKeyId = "my-key":
path1.path = "/property1";
path1.encryptionType = CosmosEncryptionType.DETERMINISTIC;
path1.encryptionAlgorithm = CosmosEncryptionAlgorithm.AEAES_256_CBC_HMAC_SHA_256;

ClientEncryptionIncludedPath path2 = new ClientEncryptionIncludedPath();
path2.clientEncryptionKeyId = "my-key":
path2.path = "/property2";
path2.encryptionType = CosmosEncryptionType.RANDOMIZED;
path2.encryptionAlgorithm = CosmosEncryptionAlgorithm.AEAES_256_CBC_HMAC_SHA_256;

List<ClientEncryptionIncludedPath> paths = new ArrayList<>();
paths.add(path1);
paths.add(path2);

CosmosContainerProperties containerProperties =
    new CosmosContainerProperties("my-container", "/id");
containerProperties.setClientEncryptionPolicy(new ClientEncryptionPolicy(paths));
database.createEncryptionContainerAsync(containerProperties);
```
---

## <a name="read-and-write-encrypted-data"></a>Lesen und Schreiben verschlüsselter Daten

### <a name="how-data-gets-encrypted"></a>So werden Daten verschlüsselt

Jedes Mal, wenn ein Dokument in Azure Cosmos DB geschrieben wird, sucht das SDK nach der Verschlüsselungsrichtlinie, um herauszufinden, welche Eigenschaften verschlüsselt werden müssen und wie. Das Ergebnis der Verschlüsselung ist eine Base64-Zeichenfolge.

**Verschlüsseln komplexer Typen**:

- Wenn die zu verschlüsselnde Eigenschaft ein JSON-Array ist, wird jeder Eintrag des Arrays verschlüsselt.

- Wenn die zu verschlüsselnde Eigenschaft ein JSON-Objekt ist, werden nur die Blattwerte des Objekts verschlüsselt. Die dazwischenliegenden Untereigenschaftsnamen bleiben in Nur-Text-Form erhalten.

### <a name="read-encrypted-items"></a>Lesen verschlüsselter Elemente

Es ist keine explizite Aktion erforderlich, um verschlüsselte Eigenschaften zu entschlüsseln, wenn Sie Punktlesevorgänge (Abrufen eines einzelnen Elements anhand seiner ID und seines Partitionsschlüssels), Abfragen oder das Lesen des Änderungsfeed durchführen. Der Grund ist wie folgt:

- Das SDK sucht die Verschlüsselungsrichtlinie, um herauszufinden, welche Eigenschaften entschlüsselt werden müssen.
- Das Ergebnis der Verschlüsselung bettet den ursprünglichen JSON-Typ des Werts ein.

Bitte beachten Sie, dass die Auflösung verschlüsselter Eigenschaften und deren nachfolgende Entschlüsselung nur auf den Ergebnissen basiert, die von Ihren Anforderungen zurückgegeben werden. Wenn beispielsweise `property1` verschlüsselt ist, aber in `property2` (`SELECT property1 AS property2 FROM c`) projiziert wird, wird es beim Empfangen durch das SDK nicht als verschlüsselte Eigenschaft identifiziert.

### <a name="filter-queries-on-encrypted-properties"></a>Filtern von Abfragen nach verschlüsselten Eigenschaften

Beim Schreiben von Abfragen, die nach verschlüsselten Eigenschaften filtern, muss die `AddParameterAsync`-Methode verwendet werden, um den Wert des Abfrageparameters zu übergeben. Diese Methode verwendet die folgenden Argumente:

- Der Name des Abfrageparameters.
- Der Wert, der in der Abfrage verwendet werden soll.
- Der Pfad der verschlüsselten Eigenschaft (wie in der Verschlüsselungsrichtlinie definiert).

> [!IMPORTANT]
> Verschlüsselte Eigenschaften können nur in Gleichheitsfiltern (`WHERE c.property = @Value`) verwendet werden. Jede andere Verwendung gibt unvorhersehbare und falsche Abfrageergebnisse zurück. Diese Einschränkung wird in den nächsten Versionen des SDK besser durchgesetzt.

# <a name="net"></a>[.NET](#tab/dotnet)

```csharp
var queryDefinition = container.CreateQueryDefinition(
    "SELECT * FROM c where c.property1 = @Property1");
await queryDefinition.AddParameterAsync(
    "@Property1",
    1234,
    "/property1");
```

# <a name="java"></a>[Java](#tab/java)

```java
EncryptionSqlQuerySpec encryptionSqlQuerySpec = new EncryptionSqlQuerySpec(
    new SqlQuerySpec("SELECT * FROM c where c.property1 = @Property1"), container);
encryptionSqlQuerySpec.addEncryptionParameterAsync(
    new SqlParameter("@Property1", 1234), "/property1")
```
---

### <a name="reading-documents-when-only-a-subset-of-properties-can-be-decrypted"></a>Lesen von Dokumenten, wenn nur eine Teilmenge der Eigenschaften entschlüsselt werden kann

In Situationen, in denen der Client nicht auf alle CMK-Elemente zugreifen kann, die zum Verschlüsseln von Eigenschaften verwendet werden, kann nur eine Teilmenge der Eigenschaften entschlüsselt werden, wenn die Daten zurückgelesen werden. Wenn beispielsweise `property1` mit Schlüssel1 und `property2` mit Schlüssel2 verschlüsselt wurde, kann eine Clientanwendung, die nur Zugriff auf Schlüssel1 hat, weiterhin Daten lesen, nicht aber `property2`. In diesem Fall müssen Sie Ihre Daten über SQL-Abfragen lesen und die Eigenschaften entfernen, die der Client nicht entschlüsseln kann: `SELECT c.property1, c.property3 FROM c`.

## <a name="cmk-rotation"></a>CMK-Drehung

Möglicherweise möchten Sie Ihren CMK „rotieren“ (d. h. einen neuen CMK anstelle des aktuellen CMK verwenden), wenn Sie vermuten, dass der aktuelle CMK kompromittiert wurde. Es ist auch eine gängige Sicherheitspraxis, den CMK regelmäßig zu rotieren. Um diese Rotation durchzuführen, müssen Sie nur den Schlüsselbezeichner des neuen CMK angeben, der zum Packen eines bestimmten DEK verwendet werden soll. Bitte beachten Sie, dass sich dieser Vorgang nicht auf die Verschlüsselung Ihrer Daten auswirkt, sondern auf den Schutz des DEK. Der Zugriff auf den vorherigen CMK sollte erst widerrufen werden, wenn die Rotation abgeschlossen ist.

# <a name="net"></a>[.NET](#tab/dotnet)

```csharp
await database.RewrapClientEncryptionKeyAsync(
    "my-key",
    new EncryptionKeyWrapMetadata(
        keyStoreProvider.ProviderName,
        "akvKey",
        " https://<my-key-vault>.vault.azure.net/keys/<new-key>/<version>"));
```

# <a name="java"></a>[Java](#tab/java)

```java
database. rewrapClientEncryptionKey(
    "my-key",
    new EncryptionKeyWrapMetadata(
        "akvKey", " https://<my-key-vault>.vault.azure.net/keys/<new-key>/<version>"));
```
---

## <a name="next-steps"></a>Nächste Schritte

- Überblick über [sicheren Zugriff auf Daten in Cosmos DB](secure-access-to-data.md).
- Erfahren Sie mehr über [vom Kunden verwaltete Schlüssel](how-to-setup-cmk.md).