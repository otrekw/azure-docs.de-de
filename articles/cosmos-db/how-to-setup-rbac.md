---
title: Konfigurieren der rollenbasierten Zugriffssteuerung für Ihr Azure Cosmos DB-Konto mit Azure AD
description: Hier erfahren Sie, wie Sie die rollenbasierte Zugriffssteuerung mit Azure Active Directory für Ihr Azure Cosmos DB-Konto konfigurieren.
author: ThomasWeiss
ms.service: cosmos-db
ms.topic: how-to
ms.date: 04/19/2021
ms.author: thweiss
ms.openlocfilehash: 209d18dfbadea89f14fd90da9a1bc57b3ccf0dfe
ms.sourcegitcommit: 6f1aa680588f5db41ed7fc78c934452d468ddb84
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/19/2021
ms.locfileid: "107728071"
---
# <a name="configure-role-based-access-control-with-azure-active-directory-for-your-azure-cosmos-db-account-preview"></a>Konfigurieren der rollenbasierten Zugriffssteuerung mit Azure Active Directory für Ihr Azure Cosmos DB-Konto (Vorschau)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

> [!IMPORTANT]
> Die rollenbasierte Zugriffssteuerung für Azure Cosmos DB befindet sich derzeit in der Vorschauphase. Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

> [!NOTE]
> In diesem Artikel geht es um die rollenbasierte Zugriffssteuerung für Vorgänge auf Datenebene in Azure Cosmos DB. Falls Sie Vorgänge auf Verwaltungsebene verwenden, finden Sie weitere Informationen in dem Artikel zur [rollenbasierten Zugriffssteuerung](role-based-access-control.md), die für Vorgänge auf Verwaltungsebene angewendet wird.

Azure Cosmos DB bietet ein integriertes System für die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC), das folgende Möglichkeiten umfasst:

- Authentifizieren Ihrer Datenanforderungen mit einer Azure AD-Identität (Azure Active Directory)
- Autorisieren Ihrer Datenanforderungen mit einem differenzierten, rollenbasierten Berechtigungsmodell

## <a name="concepts"></a>Konzepte

RBAC auf Datenebene in Azure Cosmos DB basiert auf Konzepten, die in anderen RBAC-Systemen wie [Azure RBAC](../role-based-access-control/overview.md) üblich sind:

- Das [Berechtigungsmodell](#permission-model) besteht aus einer Reihe von **Aktionen**, die jeweils einem oder mehreren Datenbankvorgängen zugeordnet sind. Einige Beispiele für Aktionen sind das Lesen eines Elements, das Schreiben eines Elements oder das Ausführen einer Abfrage.
- Azure Cosmos DB-Benutzer erstellen **[Rollendefinitionen](#role-definitions)** , die eine Liste zulässiger Aktionen enthalten.
- Rollendefinitionen werden mithilfe von **[Rollenzuweisungen](#role-assignments)** bestimmten Azure AD-Identitäten zugewiesen. Eine Rollenzuweisung definiert auch den Bereich, auf den die Rollendefinition angewendet wird. Derzeit gibt es drei Bereiche:
    - Azure Cosmos DB-Konto
    - Azure Cosmos DB-Datenbank
    - Azure Cosmos DB-Container

  :::image type="content" source="./media/how-to-setup-rbac/concepts.png" alt-text="RBAC-Konzepte":::

> [!NOTE]
> Azure Cosmos DB-RBAC bietet derzeit keine integrierten Rollendefinitionen.

## <a name="permission-model"></a><a id="permission-model"></a> Berechtigungsmodell

> [!IMPORTANT]
> Dieses Berechtigungsmodell deckt nur Datenbankvorgänge ab, mit denen Daten gelesen und geschrieben werden können. Es deckt **keine** Verwaltungsvorgänge ab, wie das Erstellen von Containern oder das Ändern ihres Durchsatzes. Dies bedeutet, dass Sie **kein Azure Cosmos DB-Datenebenen-SDK verwenden können**, um Verwaltungsvorgänge mit einer AAD-Identität zu authentifizieren. Stattdessen müssen Sie [Azure RBAC](role-based-access-control.md) verwenden über:
> - [ARM-Vorlagen](manage-with-templates.md)
> - [Azure PowerShell-Skripts](manage-with-powershell.md),
> - [Azure CLI-Skripts](manage-with-cli.md),
> - Azure-Verwaltungsbibliotheken, die verfügbar sind in
>   - [.NET](https://www.nuget.org/packages/Azure.ResourceManager.CosmosDB)
>   - [Java](https://search.maven.org/artifact/com.azure.resourcemanager/azure-resourcemanager-cosmos)
>   - [Python](https://pypi.org/project/azure-mgmt-cosmosdb/)

In der folgenden Tabelle sind alle Aktionen aufgeführt, die vom Berechtigungsmodell bereitgestellt werden.

| Name | Entsprechende Datenbankvorgänge |
|---|---|
| `Microsoft.DocumentDB/databaseAccounts/readMetadata` | Lesen von Kontometadaten. Ausführliche Informationen finden Sie unter [Metadatenanforderungen](#metadata-requests). |
| `Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers/items/create` | Erstellen eines neuen Elements. |
| `Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers/items/read` | Lesen eines einzelnen Elements anhand seiner ID und seines Partitionsschlüssels (Punktlesevorgang). |
| `Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers/items/replace` | Ersetzen eines vorhandenen Elements. |
| `Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers/items/upsert` | „Upsert“ eines Elements, d. h. Erstellen des Elements, wenn es nicht vorhanden ist, oder Ersetzen des Elements, wenn es vorhanden ist. |
| `Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers/items/delete` | Löschen eines Elements. |
| `Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers/executeQuery` | Ausführen einer [SQL-Abfrage](sql-query-getting-started.md). |
| `Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers/readChangeFeed` | Lesen aus dem [Änderungsfeed](read-change-feed.md) des Containers. |
| `Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers/executeStoredProcedure` | Ausführen einer [gespeicherten Prozedur](stored-procedures-triggers-udfs.md). |
| `Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers/manageConflicts` | Verwalten von [Konflikten](conflict-resolution-policies.md) für Konten mit mehreren Schreibregionen (d. h. Auflisten und Löschen von Elementen aus dem Konfliktfeed). |

Platzhalter werden sowohl auf *Containerebene* als auch auf *Elementebene* unterstützt:

- `Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers/*`
- `Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers/items/*`

### <a name="metadata-requests"></a><a id="metadata-requests"></a> Metadatenanforderungen

Bei Verwendung von Azure Cosmos DB-SDKs geben diese SDKs schreibgeschützte Metadatenanforderungen während der Initialisierung und zur Verarbeitung bestimmter Datenanforderungen aus. Diese Metadatenanforderungen rufen verschiedene Konfigurationsdetails ab. Dazu gehören beispielsweise: 

- Die globale Konfiguration Ihres Kontos, einschließlich der Azure-Regionen, in denen das Konto verfügbar ist
- Der Partitionsschlüssel Ihrer Container oder deren Indizierungsrichtlinie
- Die Liste der physischen Partitionen, die einen Container und seine Adressen bilden

Es werden *keine* Daten abgerufen, die Sie in Ihrem Konto gespeichert haben.

Um bestmögliche Transparenz des Berechtigungsmodells sicherzustellen, werden diese Metadatenanforderungen explizit durch die Aktion `Microsoft.DocumentDB/databaseAccounts/readMetadata` abgedeckt. Diese Aktion sollte in jeder Situation zulässig sein, in der über eines der Azure Cosmos DB-SDKs auf Ihr Azure Cosmos DB-Konto zugegriffen wird. Sie kann auf jeder Ebene in der Azure Cosmos DB-Hierarchie (d. h. Konto, Datenbank oder Container) zugewiesen werden (über eine Rollenzuweisung).

Die tatsächlich von der Aktion `Microsoft.DocumentDB/databaseAccounts/readMetadata` zugelassenen Metadatenanforderungen richten sich nach dem Bereich, dem die Aktion zugewiesen ist:

| Bereich | Von der Aktion zugelassene Anforderungen |
|---|---|
| Konto | – Auflisten der Datenbanken unter dem Konto<br>– Für jede Datenbank unter dem Konto: die zulässigen Aktionen im Datenbankbereich |
| Datenbank | – Lesen von Datenbankmetadaten<br>– Auflisten der Container unter der Datenbank<br>– Für jeden Container unter der Datenbank: die zulässigen Aktionen im Containerbereich |
| Container | – Lesen von Containermetadaten<br>– Auflisten physischer Partitionen unter dem Container<br>– Auflösen der Adresse der einzelnen physischen Partitionen |

## <a name="create-role-definitions"></a><a id="role-definitions"></a> Erstellen von Rollendefinitionen

Beim Erstellen einer Rollendefinition müssen Sie Folgendes angeben:

- Den Namen Ihres Azure Cosmos DB-Kontos.
- Die Ressourcengruppe, die Ihr Konto enthält.
- Den Typ der Rollendefinition. Derzeit wird nur `CustomRole` unterstützt.
- Den Namen der Rollendefinition.
- Eine Liste von [Aktionen](#permission-model), die von der Rolle zugelassen werden sollen.
- Mindestens einen Bereich, in dem die Rollendefinition zugewiesen werden kann. Die folgende Bereiche werden unterstützt:
    - `/` (Kontoebene)
    - `/dbs/<database-name>` (Datenbankebene)
    - `/dbs/<database-name>/colls/<container-name>` (Containerebene)

> [!NOTE]
> Die unten beschriebenen Vorgänge sind zurzeit verfügbar in:
> - Azure PowerShell: [Az.CosmosDB version 2.0.1-preview](https://www.powershellgallery.com/packages/Az.CosmosDB/2.0.1-preview)
> - Azure CLI: ['cosmosdb-preview' extension version 0.4.0](https://github.com/Azure/azure-cli-extensions/tree/master/src/cosmosdb-preview)

### <a name="using-azure-powershell"></a>Verwenden von Azure PowerShell

Erstellen Sie eine Rolle mit dem Namen *MyReadOnlyRole*, die nur Leseaktionen enthält:

```powershell
$resourceGroupName = "<myResourceGroup>"
$accountName = "<myCosmosAccount>"
New-AzCosmosDBSqlRoleDefinition -AccountName $accountName `
    -ResourceGroupName $resourceGroupName `
    -Type CustomRole -RoleName MyReadOnlyRole `
    -DataAction @( `
        'Microsoft.DocumentDB/databaseAccounts/readMetadata',
        'Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers/items/read', `
        'Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers/executeQuery', `
        'Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers/readChangeFeed') `
    -AssignableScope "/"
```

Erstellen Sie eine Rolle mit dem Namen *MyReadWriteRole*, die alle Aktionen enthält:

```powershell
New-AzCosmosDBSqlRoleDefinition -AccountName $accountName `
    -ResourceGroupName $resourceGroupName `
    -Type CustomRole -RoleName MyReadWriteRole `
    -DataAction @( `
        'Microsoft.DocumentDB/databaseAccounts/readMetadata',
        'Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers/items/*', `
        'Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers/*') `
    -AssignableScope "/"
```

Listen Sie die Rollendefinitionen auf, die Sie zum Abrufen der jeweiligen IDs erstellt haben:

```powershell
Get-AzCosmosDBSqlRoleDefinition -AccountName $accountName `
    -ResourceGroupName $resourceGroupName
```

```
RoleName         : MyReadWriteRole
Id               : /subscriptions/<mySubscriptionId>/resourceGroups/<myResourceGroup>/providers/Microsoft.DocumentDB/databaseAcc
                   ounts/<myCosmosAccount>/sqlRoleDefinitions/<roleDefinitionId>
Type             : CustomRole
Permissions      : {Microsoft.Azure.Management.CosmosDB.Models.Permission}
AssignableScopes : {/subscriptions/<mySubscriptionId>/resourceGroups/<myResourceGroup>/providers/Microsoft.DocumentDB/databaseAc
                   counts/<myCosmosAccount>}

RoleName         : MyReadOnlyRole
Id               : /subscriptions/<mySubscriptionId>/resourceGroups/<myResourceGroup>/providers/Microsoft.DocumentDB/databaseAcc
                   ounts/<myCosmosAccount>/sqlRoleDefinitions/<roleDefinitionId>
Type             : CustomRole
Permissions      : {Microsoft.Azure.Management.CosmosDB.Models.Permission}
AssignableScopes : {/subscriptions/<mySubscriptionId>/resourceGroups/<myResourceGroup>/providers/Microsoft.DocumentDB/databaseAc
                   counts/<myCosmosAccount>}
```

### <a name="using-the-azure-cli"></a>Verwenden der Azure CLI

Erstellen Sie eine Rolle mit dem Namen *MyReadOnlyRole*, die nur Leseaktionen enthält:

```json
// role-definition-ro.json
{
    "RoleName": "MyReadOnlyRole",
    "Type": "CustomRole",
    "AssignableScopes": ["/"],
    "Permissions": [{
        "DataActions": [
            "Microsoft.DocumentDB/databaseAccounts/readMetadata",
            "Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers/items/read",
            "Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers/executeQuery",
            "Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers/readChangeFeed"
        ]
    }]
}
```

```azurecli
resourceGroupName='<myResourceGroup>'
accountName='<myCosmosAccount>'
az cosmosdb sql role definition create --account-name $accountName --resource-group $resourceGroupName --body @role-definition-ro.json
```

Erstellen Sie eine Rolle mit dem Namen *MyReadWriteRole*, die alle Aktionen enthält:

```json
// role-definition-rw.json
{
    "RoleName": "MyReadWriteRole",
    "Type": "CustomRole",
    "AssignableScopes": ["/"],
    "Permissions": [{
        "DataActions": [
            "Microsoft.DocumentDB/databaseAccounts/readMetadata",
            "Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers/items/*",
            "Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers/*"
        ]
    }]
}
```

```azurecli
az cosmosdb sql role definition create --account-name $accountName --resource-group $resourceGroupName --body @role-definition-rw.json
```

Listen Sie die Rollendefinitionen auf, die Sie zum Abrufen der jeweiligen IDs erstellt haben:

```azurecli
az cosmosdb sql role definition list --account-name $accountName --resource-group $resourceGroupName
```

```
[
  {
    "assignableScopes": [
      "/subscriptions/<mySubscriptionId>/resourceGroups/<myResourceGroup>/providers/Microsoft.DocumentDB/databaseAccounts/<myCosmosAccount>"
    ],
    "id": "/subscriptions/<mySubscriptionId>/resourceGroups/<myResourceGroup>/providers/Microsoft.DocumentDB/databaseAccounts/<myCosmosAccount>/sqlRoleDefinitions/<roleDefinitionId>",
    "name": "<roleDefinitionId>",
    "permissions": [
      {
        "dataActions": [
          "Microsoft.DocumentDB/databaseAccounts/readMetadata",
          "Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers/items/*",
          "Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers/*"
        ],
        "notDataActions": []
      }
    ],
    "resourceGroup": "<myResourceGroup>",
    "roleName": "MyReadWriteRole",
    "sqlRoleDefinitionGetResultsType": "CustomRole",
    "type": "Microsoft.DocumentDB/databaseAccounts/sqlRoleDefinitions"
  },
  {
    "assignableScopes": [
      "/subscriptions/<mySubscriptionId>/resourceGroups/<myResourceGroup>/providers/Microsoft.DocumentDB/databaseAccounts/<myCosmosAccount>"
    ],
    "id": "/subscriptions/<mySubscriptionId>/resourceGroups/<myResourceGroup>/providers/Microsoft.DocumentDB/databaseAccounts/<myCosmosAccount>/sqlRoleDefinitions/<roleDefinitionId>",
    "name": "<roleDefinitionId>",
    "permissions": [
      {
        "dataActions": [
          "Microsoft.DocumentDB/databaseAccounts/readMetadata",
          "Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers/items/read",
          "Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers/executeQuery",
          "Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers/readChangeFeed"
        ],
        "notDataActions": []
      }
    ],
    "resourceGroup": "<myResourceGroup>",
    "roleName": "MyReadOnlyRole",
    "sqlRoleDefinitionGetResultsType": "CustomRole",
    "type": "Microsoft.DocumentDB/databaseAccounts/sqlRoleDefinitions"
  }
]
```

## <a name="create-role-assignments"></a><a id="role-assignments"></a> Erstellen von Rollenzuweisungen

Nachdem Sie die Rollendefinitionen erstellt haben, können Sie diese Ihren AAD-Identitäten zuordnen. Beim Erstellen einer Rollenzuweisung müssen Sie Folgendes angeben:

- Den Namen Ihres Azure Cosmos DB-Kontos.
- Die Ressourcengruppe, die Ihr Konto enthält.
- Die ID der zuzuweisenden Rollendefinition.
- Die Prinzipal-ID der Identität, der die Rollendefinition zugewiesen werden soll.
- Den Bereich der Rollenzuweisung. Folgende Bereiche werden unterstützt:
    - `/` (Kontoebene)
    - `/dbs/<database-name>` (Datenbankebene)
    - `/dbs/<database-name>/colls/<container-name>` (Containerebene)

  Der Bereich muss einem der Bereiche entsprechen, die in der Rollendefinition zugewiesen werden können, oder ein Unterbereich davon sein.

> [!NOTE]
> Wenn Sie eine Rollenzuweisung für einen Dienstprinzipal erstellen möchten, stellen Sie sicher, dass die zugehörige **Objekt-ID** wie im Abschnitt **Unternehmensanwendungen** auf dem Blatt **Azure Active Directory** des Portals verwendet wird.

> [!NOTE]
> Die unten beschriebenen Vorgänge sind zurzeit verfügbar in:
> - Azure PowerShell: [Az.CosmosDB version 2.0.1-preview](https://www.powershellgallery.com/packages/Az.CosmosDB/2.0.1-preview)
> - Azure CLI: ['cosmosdb-preview' extension version 0.4.0](https://github.com/Azure/azure-cli-extensions/tree/master/src/cosmosdb-preview)

### <a name="using-azure-powershell"></a>Verwenden von Azure PowerShell

Weisen Sie einer Identität eine Rolle zu:

```powershell
$resourceGroupName = "<myResourceGroup>"
$accountName = "<myCosmosAccount>"
$readOnlyRoleDefinitionId = "<roleDefinitionId>" // as fetched above
$principalId = "<aadPrincipalId>"
New-AzCosmosDBSqlRoleAssignment -AccountName $accountName `
    -ResourceGroupName $resourceGroupName `
    -RoleDefinitionId $readOnlyRoleDefinitionId `
    -Scope $accountName `
    -PrincipalId $principalId
```

### <a name="using-the-azure-cli"></a>Verwenden der Azure-Befehlszeilenschnittstelle

Weisen Sie einer Identität eine Rolle zu:

```azurecli
resourceGroupName='<myResourceGroup>'
accountName='<myCosmosAccount>'
readOnlyRoleDefinitionId = '<roleDefinitionId>' // as fetched above
principalId = '<aadPrincipalId>'
az cosmosdb sql role assignment create --account-name $accountName --resource-group $resourceGroupName --scope "/" --principal-id $principalId --role-definition-id $readOnlyRoleDefinitionId
```

## <a name="initialize-the-sdk-with-azure-ad"></a>Initialisieren des SDK mit Azure AD

Wenn Sie Azure Cosmos DB-RBAC in Ihrer Anwendung verwenden möchten, müssen Sie die Methode zur Initialisierung des Azure Cosmos DB-SDK aktualisieren. Anstatt den Primärschlüssel Ihres Kontos zu übergeben, müssen Sie eine Instanz einer `TokenCredential`-Klasse übergeben. Diese Instanz stellt dem Azure Cosmos DB-SDK den Kontext bereit, der zum Abrufen eines AAD-Tokens im Namen der zu verwendenden Identität erforderlich ist.

Die Methode zur Erstellung einer `TokenCredential`-Instanz wird in diesem Artikel nicht behandelt. Es gibt viele Möglichkeiten, eine solche Instanz zu erstellen, die vom Typ der zu verwendenden AAD-Identität abhängen (Benutzerprinzipal, Dienstprinzipal, Gruppe usw.). Am wichtigsten ist, dass Ihre `TokenCredential`-Instanz in die Identität (Prinzipal-ID) aufgelöst werden muss, der Sie die Rollen zugewiesen haben. Beispiele für das Erstellen einer `TokenCredential`-Klasse finden Sie hier:

- [in .NET](/dotnet/api/overview/azure/identity-readme#credential-classes)
- [in Java](/java/api/overview/azure/identity-readme#credential-classes)
- [in JavaScript](/javascript/api/overview/azure/identity-readme#credential-classes)

In den folgenden Beispielen wird ein Dienstprinzipal mit einer `ClientSecretCredential`-Instanz verwendet.

### <a name="in-net"></a>In .NET

Azure Cosmos DB RBAC wird derzeit in der `preview`-Version des [.NET SDK V3](sql-api-sdk-dotnet-standard.md) unterstützt.

```csharp
TokenCredential servicePrincipal = new ClientSecretCredential(
    "<azure-ad-tenant-id>",
    "<client-application-id>",
    "<client-application-secret>");
CosmosClient client = new CosmosClient("<account-endpoint>", servicePrincipal);
```

### <a name="in-java"></a>In Java

Azure Cosmos DB RBAC wird derzeit vom [Java SDK V4](sql-api-sdk-java-v4.md) unterstützt.

```java
TokenCredential ServicePrincipal = new ClientSecretCredentialBuilder()
    .authorityHost("https://login.microsoftonline.com")
    .tenantId("<azure-ad-tenant-id>")
    .clientId("<client-application-id>")
    .clientSecret("<client-application-secret>")
    .build();
CosmosAsyncClient Client = new CosmosClientBuilder()
    .endpoint("<account-endpoint>")
    .credential(ServicePrincipal)
    .build();
```

### <a name="in-javascript"></a>In JavaScript

Azure Cosmos DB RBAC wird derzeit vom [JavaScript SDK V3](sql-api-sdk-node.md) unterstützt.

```javascript
const servicePrincipal = new ClientSecretCredential(
    "<azure-ad-tenant-id>",
    "<client-application-id>",
    "<client-application-secret>");
const client = new CosmosClient({
    "<account-endpoint>",
    aadCredentials: servicePrincipal
});
```

## <a name="auditing-data-requests"></a>Überwachen von Datenanforderungen

Bei Verwendung von Azure Cosmos DB-RBAC werden [Diagnoseprotokolle](cosmosdb-monitor-resource-logs.md) durch Identitäts- und Autorisierungsinformationen für jeden Datenvorgang erweitert. Dies ermöglicht eine detaillierte Überwachung und das Abrufen der AAD-Identität, die für jede an Ihr Azure Cosmos DB-Konto gesendete Datenanforderung verwendet wurde.

Diese zusätzlichen Informationen fließen in die Protokollkategorie **DataPlaneRequests** ein und umfassen zwei zusätzliche Spalten:

- `aadPrincipalId_g` gibt die Prinzipal-ID der AAD-Identität an, die zum Authentifizieren der Anforderung verwendet wurde.
- `aadAppliedRoleAssignmentId_g` gibt die [Rollenzuweisung](#role-assignments) an, die beim Autorisieren der Anforderung berücksichtigt wurde.

## <a name="limits"></a>Einschränkungen

- Sie können bis zu 100 Rollendefinitionen und 2.000 Rollenzuweisungen pro Azure Cosmos DB-Konto erstellen.
- Sie können Rollendefinitionen nur Azure AD-Identitäten zuweisen, die demselben Azure AD-Anker wie Ihr Azure Cosmos DB-Konto angehören.
- Die Azure AD-Gruppenauflösung wird derzeit für Identitäten, die zu mehr als 200 Gruppen gehören, nicht unterstützt.
- Das Azure AD-Token wird zurzeit mit jeder einzelnen an den Azure Cosmos DB-Dienst gesendeten Anforderung als Header übergeben und erhöht so die Gesamtnutzlastgröße.
- Der Zugriff auf Ihre Daten mit Azure AD über den [Azure Cosmos DB-Explorer](data-explorer.md) wird noch nicht unterstützt. Wenn Sie den Azure Cosmos DB-Explorer verwenden, muss der Benutzer vorerst noch Zugriff auf den Primärschlüssel des Kontos haben.

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

### <a name="which-azure-cosmos-db-apis-are-supported-by-rbac"></a>Welche Azure Cosmos DB-APIs werden von RBAC unterstützt?

Derzeit wird nur die SQL-API unterstützt.

### <a name="is-it-possible-to-manage-role-definitions-and-role-assignments-from-the-azure-portal"></a>Können Rollendefinitionen und Rollenzuweisungen über das Azure-Portal verwaltet werden?

Eine Unterstützung für die Rollenverwaltung ist im Azure-Portal noch nicht verfügbar.

### <a name="which-sdks-in-azure-cosmos-db-sql-api-support-rbac"></a>Welche SDKs in der SQL-API von Azure Cosmos DB unterstützen RBAC?

Derzeit werden die SDKs [.NET V3](sql-api-sdk-dotnet-standard.md) und [Java V4](sql-api-sdk-java-v4.md) unterstützt.

### <a name="is-the-azure-ad-token-automatically-refreshed-by-the-azure-cosmos-db-sdks-when-it-expires"></a>Wird das Azure AD-Token bei Ablauf automatisch von den Azure Cosmos DB-SDKs aktualisiert?

Ja.

### <a name="is-it-possible-to-disable-the-usage-of-the-account-primary-key-when-using-rbac"></a>Kann die Verwendung des Primärschlüssels für das Konto bei Nutzung von RBAC deaktiviert werden?

Das Deaktivieren des Primärschlüssels für das Konto ist derzeit nicht möglich.

## <a name="next-steps"></a>Nächste Schritte

- Überblick über [sicheren Zugriff auf Daten in Cosmos DB](secure-access-to-data.md).
- Erfahren Sie mehr über [RBAC für die Azure Cosmos DB-Verwaltung](role-based-access-control.md).