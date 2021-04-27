---
title: Konfigurieren Sie verwaltete Identitäten mit Azure AD für Ihr Azure Cosmos DB-Konto
description: Erfahren Sie, wie Sie verwaltete Identitäten mit Azure Active Directory für Ihr Azure Cosmos DB-Konto konfigurieren
author: ThomasWeiss
ms.service: cosmos-db
ms.topic: how-to
ms.date: 04/02/2021
ms.author: thweiss
ms.openlocfilehash: 30efaed09a400611861bdd3adeae1f650054b405
ms.sourcegitcommit: 3f684a803cd0ccd6f0fb1b87744644a45ace750d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/02/2021
ms.locfileid: "106230854"
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

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu verwalteten Identitäten für Azure-Ressourcen finden Sie [hier](../active-directory/managed-identities-azure-resources/overview.md).
- Erfahren Sie mehr über [kundenverwaltete Schlüssel auf Azure Cosmos DB](how-to-setup-cmk.md)
