---
title: Konfigurieren Sie verwaltete Identitäten mit Azure AD für Ihr Azure Cosmos DB-Konto
description: Erfahren Sie, wie Sie verwaltete Identitäten mit Azure Active Directory für Ihr Azure Cosmos DB-Konto konfigurieren
author: ThomasWeiss
ms.service: cosmos-db
ms.topic: how-to
ms.date: 04/23/2021
ms.author: thweiss
ms.openlocfilehash: 3f33cc08fcb9f3c43d9da312ce9ff12d9b20d722
ms.sourcegitcommit: 5f785599310d77a4edcf653d7d3d22466f7e05e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/27/2021
ms.locfileid: "108065293"
---
# <a name="configure-managed-identities-with-azure-active-directory-for-your-azure-cosmos-db-account"></a>Konfigurieren Sie verwaltete Identitäten mit Azure Active Directory für Ihr Azure Cosmos DB-Konto
[!INCLUDE[appliesto-all-apis](includes/appliesto-all-apis.md)]

Verwaltete Identitäten für Azure-Ressourcen stellen für Azure-Dienste eine automatisch verwaltete Identität in Azure Active Directory bereit. Dieser Artikel zeigt, wie Sie eine verwaltete Identität für Azure Cosmos DB-Konten erstellen.

> [!NOTE]
> Nur vom System zugewiesene verwaltete Identitäten werden derzeit von Azure Cosmos DB unterstützt.

## <a name="prerequisites"></a>Voraussetzungen

- Wenn Sie mit verwalteten Identitäten für Azure-Ressourcen noch nicht vertraut sind, lesen Sie [Was sind verwaltete Identitäten für Azure-Ressourcen?](../active-directory/managed-identities-azure-resources/overview.md). Informationen über verwaltete Identitätstypen finden Sie unter [Verwaltete Identitätstypen](../active-directory/managed-identities-azure-resources/overview.md#managed-identity-types).
- Um verwaltete Identitäten einzurichten, muss Ihr Konto über die [Rolle DocumentDB Account Contributor](../role-based-access-control/built-in-roles.md#documentdb-account-contributor) verfügen.

## <a name="add-a-system-assigned-identity"></a>Hinzufügen einer systemseitig zugewiesenen Identität

### <a name="using-an-azure-resource-manager-arm-template"></a>Verwenden einer Azure-Ressourcenmanager (ARM) Vorlage

> [!IMPORTANT]
> Stellen Sie sicher, dass Sie ein `apiVersion` oder `2021-03-15` höher verwenden, wenn Sie mit verwalteten Identitäten arbeiten.

Wenn Sie eine vom System zugewiesene Identität für ein neues oder vorhandenes Azure Cosmos DB Konto aktivieren möchten, fügen Sie die folgende Eigenschaft in die Ressourcendefinition ein:

```json
"identity": {
    "type": "SystemAssigned"
}
```

Der `resources` Abschnitt Ihrer ARM-Vorlage sollte dann wie folgt aussehen:

```json
"resources": [
    {
        "type": " Microsoft.DocumentDB/databaseAccounts",
        "identity": {
            "type": "SystemAssigned"
        },
        // ...
    },
    // ...
 ]
```

Nachdem Ihr Azure Cosmos DB Konto erstellt oder aktualisiert wurde, wird die folgende Eigenschaft angezeigt:

```json
"identity": {
    "type": "SystemAssigned",
    "tenantId": "<azure-ad-tenant-id>",
    "principalId": "<azure-ad-principal-id>"
}
```

### <a name="using-the-azure-cli"></a>Verwenden der Azure-Befehlszeilenschnittstelle

Um beim Erstellen eines neuen Azure Cosmos DB-Kontos eine systemseitig zugewiesene Identität zu aktivieren, fügen Sie die Option `--assign-identity` hinzu:

```azurecli
resourceGroupName='myResourceGroup'
accountName='mycosmosaccount'

az cosmosdb create \
    -n $accountName \
    -g $resourceGroupName \
    --locations regionName='West US 2' failoverPriority=0 isZoneRedundant=False \
    --assign-identity
```

Sie können über den Befehl `az cosmosdb identity assign` auch einem vorhandenen Konto eine systemseitig zugewiesene Identität hinzufügen:

```azurecli
resourceGroupName='myResourceGroup'
accountName='mycosmosaccount'

az cosmosdb identity assign \
    -n $accountName \
    -g $resourceGroupName
```

Nachdem Ihr Azure Cosmos DB-Konto erstellt oder aktualisiert wurde, können Sie die zugewiesene Identität mit dem Befehl `az cosmosdb identity show` abrufen:

```azurecli
resourceGroupName='myResourceGroup'
accountName='mycosmosaccount'

az cosmosdb identity show \
    -n $accountName \
    -g $resourceGroupName
```

```json
{
    "type": "SystemAssigned",
    "tenantId": "<azure-ad-tenant-id>",
    "principalId": "<azure-ad-principal-id>"
}
```

## <a name="remove-a-system-assigned-identity"></a>Entfernen einer systemseitig zugewiesenen Identität

### <a name="using-an-azure-resource-manager-arm-template"></a>Verwenden einer Azure-Ressourcenmanager (ARM) Vorlage

> [!IMPORTANT]
> Stellen Sie sicher, dass Sie ein `apiVersion` oder `2021-03-15` höher verwenden, wenn Sie mit verwalteten Identitäten arbeiten.

Legen Sie den `type` der Eigenschaft `identity` auf `None` fest, um eine systemseitig zugewiesene Identität aus Ihrem Azure Cosmos DB-Konto zu entfernen:

```json
"identity": {
    "type": "None"
}
```

### <a name="using-the-azure-cli"></a>Verwenden der Azure-Befehlszeilenschnittstelle

Verwenden Sie den Befehl `az cosmosdb identity remove`, um eine systemseitig zugewiesene Identität aus Ihrem Azure Cosmos DB-Konto zu entfernen:

```azurecli
resourceGroupName='myResourceGroup'
accountName='mycosmosaccount'

az cosmosdb identity remove \
    -n $accountName \
    -g $resourceGroupName
```

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu verwalteten Identitäten für Azure-Ressourcen finden Sie [hier](../active-directory/managed-identities-azure-resources/overview.md).
- Erfahren Sie mehr über [kundenverwaltete Schlüssel auf Azure Cosmos DB](how-to-setup-cmk.md)
