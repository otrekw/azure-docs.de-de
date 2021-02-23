---
title: Erstellen und Verwalten von Anmeldeinformationen für Überprüfungen
description: In diesem Artikel werden die Schritte zum Erstellen und Verwalten von Anmeldeinformationen in Azure Purview beschrieben.
author: viseshag
ms.author: viseshag
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 02/11/2021
ms.openlocfilehash: 9ffc450294f186b77cc7a6c44c10eecf266161d3
ms.sourcegitcommit: e3151d9b352d4b69c4438c12b3b55413b4565e2f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/15/2021
ms.locfileid: "100526692"
---
# <a name="credentials-for-source-authentication-in-azure-purview"></a>Anmeldeinformationen für die Quellenauthentifizierung in Azure Purview

In diesem Artikel wird beschrieben, wie Sie in Azure Purview Anmeldeinformationen erstellen. Mithilfe dieser gespeicherten Anmeldeinformationen können Sie gespeicherte Authentifizierungsinformationen schnell wiederverwenden und auf Ihre Datenquellenüberprüfungen anwenden.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Schlüsseltresor. Informationen zur Erstellung finden Sie unter [Schnellstart: Erstellen eines Schlüsseltresors über das Azure-Portal](../key-vault/general/quick-create-portal.md).

## <a name="introduction"></a>Einführung

Bei Anmeldeinformationen handelt es sich um Informationen für die Authentifizierung, die von Azure Purview für die Authentifizierung bei Ihren registrierten Datenquellen genutzt werden können. Ein Objekt mit Anmeldeinformationen kann für verschiedene Arten von Authentifizierungsszenarien erstellt werden, z. B. die Standardauthentifizierung mit Benutzername und Kennwort. In den Anmeldeinformationen werden spezifische Informationen erfasst, die je nach der ausgewählten Authentifizierungsmethode für die Authentifizierung erforderlich sind. Für die Anmeldeinformationen werden Ihre vorhandenen Azure Key Vault-Geheimnisse verwendet, um bei der Erstellung der Anmeldeinformationen vertrauliche Authentifizierungsinformationen abzurufen.

## <a name="use-purview-managed-identity-to-set-up-scans"></a>Verwenden einer verwalteten Purview-Identität zum Einrichten von Überprüfungen

Wenn Sie die verwaltete Purview-Identität zum Einrichten von Überprüfungen verwenden, müssen Sie nicht explizit Anmeldeinformationen erstellen und Ihren Schlüsseltresor für die Speicherung nicht mit Purview verknüpfen. Eine ausführliche Anleitung zum Hinzufügen der verwalteten Purview-Identität, um den Zugriff auf die Überprüfung Ihrer Datenquellen zu ermöglichen, finden Sie unten in den Abschnitten zur Authentifizierung von Datenquellen:

- [Azure Blob Storage](register-scan-azure-blob-storage-source.md#setting-up-authentication-for-a-scan)
- [Azure Data Lake Storage Gen1](register-scan-adls-gen1.md#setting-up-authentication-for-a-scan)
- [Azure Data Lake Storage Gen2](register-scan-adls-gen2.md#setting-up-authentication-for-a-scan)
- [Azure SQL-Datenbank](register-scan-azure-sql-database.md)
- [Verwaltete Azure SQL-Datenbank-Instanz](register-scan-azure-sql-database-managed-instance.md#setting-up-authentication-for-a-scan)
- [Azure Synapse Analytics](register-scan-azure-synapse-analytics.md#setting-up-authentication-for-a-scan)

## <a name="create-azure-key-vaults-connections-in-your-azure-purview-account"></a>Erstellen von Azure Key Vault-Verbindungen in Ihrem Azure Purview-Konto

Bevor Sie Anmeldeinformationen erstellen können, müssen Sie Ihrem Azure Purview-Konto mindestens eine Ihrer vorhandenen Azure Key Vault-Instanzen zuordnen.

1. Wählen Sie Ihr Azure Purview-Konto im [Azure-Portal](https://portal.azure.com) aus. Navigieren Sie zum **Verwaltungscenter** und dann zu den **Anmeldeinformationen**.

2. Wählen Sie auf der Seite **Anmeldeinformationen** die Option **Manage Key Vault Connections** (Key Vault-Verbindungen verwalten) aus.

   :::image type="content" source="media/manage-credentials/manage-kv-connections.png" alt-text="Verwalten von Azure Key Vault-Verbindungen":::

3. Wählen Sie auf der Seite zum Verwalten Ihrer Key Vault-Verbindungen die Option **+ Neu** aus.

4. Geben Sie die erforderlichen Informationen ein, und wählen Sie anschließend **Erstellen** aus.

5. Vergewissern Sie sich, dass die Zuordnung Ihrer Key Vault-Instanz zu Ihrem Azure Purview-Konto wie im folgenden Beispiel erfolgreich war:

   :::image type="content" source="media/manage-credentials/view-kv-connections.png" alt-text="Anzeigen der Azure Key Vault-Verbindungen zur Bestätigung":::

## <a name="grant-the-purview-managed-identity-access-to-your-azure-key-vault"></a>Gewähren des Zugriffs auf Ihre Azure Key Vault-Instanz für die verwaltete Purview-Identität

1. Navigieren Sie zu Ihrer Azure Key Vault-Instanz.

2. Wählen Sie die Seite **Zugriffsrichtlinien** aus.

3. Wählen Sie **Zugriffsrichtlinie hinzufügen** aus.

   :::image type="content" source="media/manage-credentials/add-msi-to-akv.png" alt-text="Hinzufügen des MSI für Purview zu AKV":::

4. Wählen Sie in der Dropdownliste **Secrets permissions** (Geheimnisberechtigungen) die Berechtigungen **Get** und **List** aus.

5. Wählen Sie für **Prinzipal auswählen** die verwaltete Purview-Identität aus. Sie können die Purview-MSI entweder mithilfe des Purview-Instanznamens **oder** der Anwendungs-ID der verwalteten Identität suchen. Verbundidentitäten (Name der verwalteten Identität + Anwendungs-ID) werden zurzeit nicht unterstützt.

   :::image type="content" source="media/manage-credentials/add-access-policy.png" alt-text="Zugriffsrichtlinie hinzufügen":::

6. Wählen Sie **Hinzufügen**.

7. Wählen Sie **Speichern** aus, um die Zugriffsrichtlinie zu speichern.

   :::image type="content" source="media/manage-credentials/save-access-policy.png" alt-text="Speichern der Zugriffsrichtlinie":::

## <a name="create-a-new-credential"></a>Erstellen von neuen Anmeldeinformationen

Die folgenden Typen von Anmeldeinformationen werden in Purview unterstützt:

- Standardauthentifizierung: Sie fügen das **Kennwort** dem Schlüsseltresor als Geheimnis hinzu.
- Dienstprinzipal: Sie fügen den **Dienstprinzipalschlüssel** dem Schlüsseltresor als Geheimnis hinzu.
- SQL-Authentifizierung: Sie fügen das **Kennwort** dem Schlüsseltresor als Geheimnis hinzu.
- Kontoschlüssel: Sie fügen den **Kontoschlüssel** dem Schlüsseltresor als Geheimnis hinzu.

Weitere Informationen finden Sie unter [Hinzufügen eines Geheimnisses in Key Vault](../key-vault/secrets/quick-create-portal.md#add-a-secret-to-key-vault).

Nach dem Speichern Ihrer Geheimnisse in Key Vault:

1. Wechseln Sie in Azure Purview zur Seite „Anmeldeinformationen“.

2. Erstellen Sie Ihre neuen Anmeldeinformationen, indem Sie **+ Neu** auswählen.

3. Geben Sie die erforderlichen Informationen an. Wählen Sie die **Authentifizierungsmethode** und eine **Key Vault-Verbindung** aus, von der Sie ein Geheimnis auswählen möchten.

4. Wählen Sie **Erstellen** aus, nachdem Sie alle Details eingegeben haben.

   :::image type="content" source="media/manage-credentials/new-credential.png" alt-text="Neue Anmeldeinformationen":::

5. Vergewissern Sie sich, dass Ihre neuen Anmeldeinformationen in der Liste angezeigt werden und einsatzbereit sind.

   :::image type="content" source="media/manage-credentials/view-credentials.png" alt-text="Anzeigen von Anmeldeinformationen":::

## <a name="manage-your-key-vault-connections"></a>Verwalten Ihrer Key Vault-Verbindungen

1. Suchen/Finden von Key Vault-Verbindungen anhand des Namens

   :::image type="content" source="media/manage-credentials/search-kv.png" alt-text="Schlüsseltresor suchen":::

2. Löschen von Key Vault-Verbindungen

   :::image type="content" source="media/manage-credentials/delete-kv.png" alt-text="Löschen von Schlüsseltresoren":::

## <a name="manage-your-credentials"></a>Verwalten Ihrer Anmeldeinformationen

1. Suchen/Finden von Anmeldeinformationen anhand des Namens
  
2. Auswählen und Aktualisieren vorhandener Anmeldeinformationen

3. Löschen von Anmeldeinformationen

## <a name="next-steps"></a>Nächste Schritte

[Erstellen eines Überprüfungsregelsatzes](create-a-scan-rule-set.md)
