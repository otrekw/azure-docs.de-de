---
title: Überprüfen dedizierter SQL-Pools
description: In dieser ausführlichen Anleitung erfahren Sie, wie Sie dedizierte SQL-Pools überprüfen.
author: viseshag
ms.author: viseshag
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 10/22/2020
ms.openlocfilehash: 7d6a0b04306c2ed6ae4887c79962cbb5528643fc
ms.sourcegitcommit: 5f482220a6d994c33c7920f4e4d67d2a450f7f08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2021
ms.locfileid: "107106971"
---
# <a name="register-and-scan-dedicated-sql-pools-formerly-sql-dw"></a>Registrieren und Überprüfen dedizierter SQL-Pools (ehemals SQL DW)

> [!NOTE]
> Wenn Sie eine dedizierte SQL-Datenbank in einem Synapse-Arbeitsbereich registrieren und scannen möchten, gehen Sie wie [hier](register-scan-synapse-workspace.md) beschrieben vor.

In diesem Artikel wird beschrieben, wie Sie einen dedizierten SQL-Pool (ehemals SQL DW) in Purview registrieren und überprüfen.

## <a name="supported-capabilities"></a>Unterstützte Funktionen

Für Azure Synapse Analytics (vormals SQL DW) werden vollständige und inkrementelle Überprüfungen zur Erfassung der Metadaten und Schemas unterstützt. Darüber hinaus werden die Daten bei Überprüfungen basierend auf den System- und benutzerdefinierten Klassifizierungsregeln automatisch klassifiziert.

### <a name="known-limitations"></a>Bekannte Einschränkungen

> * Die Überprüfung von [Sichten](/sql/relational-databases/views/views?view=azure-sqldw-latest&preserve-view=true) in Azure Synapse Analytics wird von Azure Purview nicht unterstützt.
> * Von Azure Purview werden auf der Registerkarte „Schema“ maximal 300 Spalten unterstützt, und es wird „Additional-Columns-Truncated“ angezeigt. 

## <a name="prerequisites"></a>Voraussetzungen

- Erstellen Sie vor dem Registrieren der Datenquellen zunächst ein Azure Purview-Konto. Weitere Informationen zum Erstellen eines Purview-Kontos finden Sie unter [Schnellstart: Erstellen eines Azure Purview-Kontos im Azure-Portal](create-catalog-portal.md).
- Sie müssen ein Azure Purview-Datenquellenadministrator sein.
- Es muss Netzwerkzugriff zwischen dem Purview-Konto und Azure Synapse Analytics bestehen.
 
## <a name="setting-up-authentication-for-a-scan"></a>Einrichten der Authentifizierung für eine Überprüfung

Es gibt drei Möglichkeiten, die Authentifizierung für Azure Synapse Analytics einzurichten:

- Verwaltete Identität
- SQL-Authentifizierung
- Dienstprinzipal

    > [!Note]
    > Neue Anmeldenamen können nur mit den Anmeldenamen des Serverebenenprinzipals (im Bereitstellungsprozess erstellt) oder von Mitgliedern der `loginmanager`-Datenbankrolle in der Masterdatenbank erstellt werden. Nach dem Gewähren der Berechtigung dauert es ca. 15 Minuten, bis im Purview-Konto die entsprechenden Berechtigungen zum Überprüfen der Ressourcen verfügbar sind.

### <a name="managed-identity-recommended"></a>Verwaltete Identität (empfohlen) 
   
Ihr Purview-Konto verfügt über eine eigene verwaltete Identität, bei der es sich im Grunde genommen um den Purview-Namen handelt, den Sie bei der Erstellung eingegeben haben. Sie müssen einen Azure AD-Benutzer in Azure Synapse Analytics (vormals SQL DW) erstellen, indem Sie genau den Namen der verwalteten Identität von Purview angeben. Erfüllen Sie hierfür die Voraussetzungen unter [Tutorial: Erstellen von Azure AD-Benutzern mithilfe von Azure AD-Anwendungen](../azure-sql/database/authentication-aad-service-principal-tutorial.md), und arbeiten Sie das Tutorial durch.

SQL-Beispielsyntax für das Erstellen eines Benutzers und das Gewähren der Berechtigung:

```sql
CREATE USER [PurviewManagedIdentity] FROM EXTERNAL PROVIDER
GO

EXEC sp_addrolemember 'db_owner', [PurviewManagedIdentity]
GO
```

Die Authentifizierung muss über die Berechtigung zum Abrufen von Metadaten für die Datenbank, die Schemas und die Tabellen verfügen. Darüber hinaus muss das Abfragen der Tabellen möglich sein, damit Stichproben für die Klassifizierung genommen werden können. Wir empfehlen Ihnen, der Identität die Berechtigung `db_owner` zuzuweisen.

### <a name="service-principal"></a>Dienstprinzipal

Sie können für Überprüfungen eine vorhandene Dienstprinzipalauthentifizierung verwenden oder eine neue erstellen. 

> [!Note]
> Führen Sie die folgenden Schritte aus, falls Sie einen neuen Dienstprinzipal erstellen müssen:
> 1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com).
> 1. Wählen Sie im Menü auf der linken Seite die Option **Azure Active Directory** aus.
> 1. Wählen Sie **App-Registrierungen** aus.
> 1. Wählen Sie **+ Registrierung einer neuen Anwendung** aus.
> 1. Geben Sie einen Namen für die **Anwendung** ein (Dienstprinzipalname).
> 1. Wählen Sie **Nur Konten in diesem Organisationsverzeichnis** aus.
> 1. Wählen Sie als Umleitungs-URI die Option **Web** aus, und geben Sie die gewünschte URL ein. Hierbei muss es sich nicht um eine reale oder geschäftliche URL handeln.
> 1. Klicken Sie anschließend auf **Registrieren**.

Es ist erforderlich, die Anwendungs-ID und das Geheimnis des Dienstprinzipals abzurufen:

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrem Dienstprinzipal.
1. Kopieren Sie die Werte von **Anwendungs-ID (Client)** unter **Übersicht** und **Geheimer Clientschlüssel** unter **Zertifikate und Geheimnisse**.
1. Navigieren zum Schlüsseltresor
1. Wählen Sie **Einstellungen > Geheimnisse** aus.
1. Wählen Sie **+ Generieren/Importieren** aus, und geben Sie unter **Name** einen gewünschten Namen und den **Wert** als **Geheimen Clientschlüssel** Ihres Dienstprinzipals ein.
1. Wählen Sie **Erstellen** aus, um den Vorgang abzuschließen.
1. Falls für Ihren Schlüsseltresor noch keine Verbindung mit Purview hergestellt wurde, müssen Sie eine [neue Schlüsseltresorverbindung erstellen](manage-credentials.md#create-azure-key-vaults-connections-in-your-azure-purview-account).
1. [Erstellen Sie abschließend neue Anmeldeinformationen](manage-credentials.md#create-a-new-credential), indem Sie den Dienstprinzipal zum Einrichten Ihrer Überprüfung verwenden. 

#### <a name="granting-the-service-principal-access-to-your-azure-synapse-analytics-formerly-sql-dw"></a>Gewähren des Zugriffs auf Ihre Azure Synapse Analytics-Instanz (vormals SQL DW) für den Dienstprinzipal

Darüber hinaus müssen Sie auch in Azure Synapse Analytics einen Azure AD-Benutzer erstellen, indem Sie die Voraussetzungen unter [Tutorial: Erstellen von Azure AD-Benutzern mithilfe von Azure AD-Anwendungen](../azure-sql/database/authentication-aad-service-principal-tutorial.md) erfüllen und das Tutorial durcharbeiten. SQL-Beispielsyntax für das Erstellen eines Benutzers und das Gewähren der Berechtigung:

```sql
CREATE USER [ServicePrincipalName] FROM EXTERNAL PROVIDER
GO

ALTER ROLE db_owner ADD MEMBER [ServicePrincipalName]
GO
```

> [!Note]
> Für Purview werden für die Überprüfung die **Anwendungs-ID (Client)** und der **geheime Clientschlüssel** benötigt.

### <a name="sql-authentication"></a>SQL-Authentifizierung

Sie können die Anleitung unter [CREATE LOGIN](/sql/t-sql/statements/create-login-transact-sql?view=azure-sqldw-latest&preserve-view=true#examples-1) befolgen, um einen Anmeldenamen für Azure Synapse Analytics (vormals SQL DW) zu erstellen, falls Sie noch keinen besitzen.

Wenn **SQL-Authentifizierung** als Authentifizierungsmethode ausgewählt wird, müssen Sie Ihr Kennwort eingeben und im Schlüsseltresor speichern:

1. Beschaffen Sie das Kennwort für Ihren SQL-Anmeldenamen.
1. Navigieren zum Schlüsseltresor
1. Wählen Sie **Einstellungen > Geheimnisse** aus.
1. Wählen Sie **+ Generieren/Importieren** aus, und geben Sie den **Namen** und **Wert** als *Kennwort* für Ihren SQL-Anmeldenamen ein.
1. Wählen Sie **Erstellen** aus, um den Vorgang abzuschließen.
1. Falls für Ihren Schlüsseltresor noch keine Verbindung mit Purview hergestellt wurde, müssen Sie eine [neue Schlüsseltresorverbindung erstellen](manage-credentials.md#create-azure-key-vaults-connections-in-your-azure-purview-account).
1. [Erstellen Sie abschließend neue Anmeldeinformationen](manage-credentials.md#create-a-new-credential), indem Sie den Schlüssel zum Einrichten Ihrer Überprüfung verwenden.

## <a name="register-a-sql-dedicated-pool-formerly-sql-dw"></a>Registrieren eines dedizierten SQL-Pools (ehemals SQL DW)

Gehen Sie wie folgt vor, um einen neuen Azure Synapse Analytics-Server für Ihren Datenkatalog zu registrieren:

1. Navigieren Sie zu Ihrem Purview-Konto.
1. Wählen Sie im linken Navigationsbereich die Option **Quellen** aus.
1. Wählen Sie **Registrieren** aus.
1. Wählen Sie unter **Quellen registrieren** die Option **SQL dedicated pool (formerly SQL DW)** (Dedizierter SQL-Pool (ehemals SQL DW)) aus.
1. Wählen Sie **Weiter** aus.

Gehen Sie unter **Register sources (Azure Synapse Analytics)** (Quellen registrieren (Azure Synapse Analytics)) wie folgt vor:

1. Geben Sie unter **Name** einen Namen ein, unter dem die Datenquelle im Katalog aufgeführt werden soll.
1. Wählen Sie aus, auf welche Weise Sie auf die gewünschte logische SQL Server-Instanz verweisen möchten:
   1. Wählen Sie die Option **Aus Azure-Abonnement** und dann im Dropdownfeld **Azure-Abonnement** das entsprechende Abonnement und im Dropdownfeld **Servername** den entsprechenden Server aus.
   1. Alternativ können Sie auch die Option **Manuell eingeben** auswählen und einen **Servernamen** eingeben.
1. Wählen Sie **Fertig stellen** aus, um die Datenquelle zu registrieren.

:::image type="content" source="media/register-scan-azure-synapse-analytics/register-sources.png" alt-text="Optionen für die Quellenregistrierung" border="true":::

[!INCLUDE [create and manage scans](includes/manage-scans.md)]

## <a name="next-steps"></a>Nächste Schritte

- [Browsen im Azure Purview-Datenkatalog](how-to-browse-catalog.md)
- [Suchen im Azure Purview-Datenkatalog](how-to-search-catalog.md)
