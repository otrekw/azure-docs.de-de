---
title: Klassifizieren der Azure SQL-Daten mithilfe von Azure Purview-Bezeichnungen
description: Importieren der Klassifizierung aus Azure Purview in Azure SQL-Datenbank und Azure Synapse Analytics
services: sql-database
ms.service: sql-database
ms.custom: ''
ms.devlang: azurepowershell
ms.topic: sample
author: davidtrigano
ms.author: datrigan
ms.reviewer: vanto
ms.date: 02/17/2021
ms.openlocfilehash: 2eab7c535ff0c68da772e8a45ead12420734279c
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "101714900"
---
# <a name="classify-your-azure-sql-data-using-azure-purview-labels"></a>Klassifizieren der Azure SQL-Daten mithilfe von Azure Purview-Bezeichnungen
[!INCLUDE[appliesto-sqldb-asa](../../azure-sql/includes/appliesto-sqldb-asa.md)]

In diesem Dokument wird beschrieben, wie Sie Azure Purview-Bezeichnungen in Azure SQL-Datenbank und Azure Synapse Analytics (vormals SQL DW) hinzufügen.

## <a name="create-an-application"></a>Erstellen einer Anwendung

1. Öffnen Sie im Azure-Portal Ihre Instanz von **Azure Active Directory**.
2. Wählen Sie unter **Verwalten** die Option **App-Registrierung** aus.
3. Erstellen Sie eine neue Azure Active Directory-App, indem Sie **Neue Anwendung** auswählen.
4. Geben Sie einen Namen für Ihre Anwendung ein, und wählen Sie **Registrieren** aus.
5. Öffnen Sie nach dem Erstellen der Anwendung unter **Manager** die Option **Zertifikate und Geheimnisse**.
6. Erstellen Sie einen neuen geheimen Clientschlüssel, indem Sie unter **Geheime Clientschlüssel** die Option **Neuer geheimer Clientschlüssel** auswählen.
7. Fügen Sie dem geheimen Clientschlüssel eine Beschreibung hinzu, und wählen Sie einen Ablaufzeitraum und anschließend **Hinzufügen** aus.
8. Notieren Sie sich den **Wert** für später.

   > [!NOTE]
   > Wenn Sie die Seite schließen, wird der Wert maskiert. Sie können den Wert des geheimen Clientschlüssels nicht abrufen, wenn Sie zu der Seite zurückkehren. Sie müssen einen neuen geheimen Clientschlüssel generieren.

9. Navigieren Sie zurück zur Übersicht Ihrer neu erstellten Anwendung, und kopieren Sie die folgenden Werte zur späteren Verwendung:
    1. Anwendungs-ID (Client)
    1. Verzeichnis-ID (Mandant)

## <a name="provide-permissions-to-the-application"></a>Erteilen von Berechtigungen für die Anwendung

1. Suchen Sie im Azure-Portal nach **Purview-Konten**.
2. Wählen Sie das Purview-Konto aus, in dem die SQL-Datenbank- und Synapse-Instanzen klassifiziert sind.
3. Öffnen Sie **Zugriffssteuerung (IAM)** > **Hinzufügen**.

4. Wählen Sie **Rollenzuweisung hinzufügen** aus.
5. Suchen Sie im Abschnitt **Rolle** nach **Datenleseberechtigter für Purview**, und wählen Sie die Rolle aus.
6. Suchen Sie im Abschnitt **Auswählen** nach der Anwendung, die Sie zuvor erstellt haben, wählen Sie sie aus, und klicken Sie auf **Speichern**.

## <a name="extract-the-classification-from-azure-purview"></a>Extrahieren der Klassifizierung aus Azure Purview

1. Öffnen Sie Ihr Purview-Konto, und suchen Sie auf der Startseite nach Ihrer Azure SQL-Datenbank- oder Azure Synapse Analytics-Instanz, in der Sie die Bezeichnungen kopieren möchten.
2. Kopieren Sie unter **Eigenschaften** den Wert für „qualifiedName“, und notieren Sie ihn zur späteren Verwendung.
3. Öffnen Sie die PowerShell-Shell.

4. Kopieren Sie abhängig von Ihrem SQL-Ressourcentyp (Azure SQL-Datenbank oder Azure Synapse) eines der unten aufgeführten Skripts.
5. Füllen Sie die Parameter mit den oben kopierten Werten aus:

   a. $TenantID: Abschnitt 1, Schritt 9
   
   b. $ClientID: Abschnitt 1, Schritt 9
   
   c. $SecretID: Abschnitt 1, Schritt 8
   
   d. $purviewAccountName: Abschnitt 2, Schritt 2
   
   e. $sqlDatabaseName: Abschnitt 3, Schritt 2

6. Kopieren Sie die Ausgabe des Skripts zur späteren Verwendung.

### <a name="for-azure-sql-database"></a>Für Azure SQL-Datenbank

```powershell
# Replace the values below with the relevant values for your environment
$TenantID = 'your_tenant_id'
$ClientID = 'your_client_id'
$SecretID = 'your_secret_id'
$purviewAccountName='purview_account_name'
$sqlDatabaseName="mssql://sql_server_name.database.windows.net/db_name"

###############################################################################
# Get an access accessToken, and build REST Request Header.
###############################################################################
$cmdletParameters = @{
  Method  = "POST"
  URI     = "https://login.microsoftonline.com/$TenantID/oauth2/token"
  Headers = @{
    "Content-Type" = 'application/x-www-form-urlencoded'
  }
  Body    = "client_id=$ClientID&client_secret=$SecretID&resource=https://purview.azure.net&grant_type=client_credentials"
}

$invokeResult = Invoke-RestMethod @cmdletParameters;
$accessToken = $invokeResult.access_token; 

$restRequestHeader = @{
  "authorization" = "Bearer $accessToken"
};

###############################################################################
# Get database entity.
###############################################################################
$getDatabaseEntityEntryEndpoint = "https://" + $purviewAccountName + ".catalog.purview.azure.com/api/atlas/v2/entity/uniqueAttribute/type/azure_sql_db?attr:qualifiedName=" + $sqlDatabaseName;
$cmdletParameters = @{
  Method      = "Get"
  Uri         = $getDatabaseEntityEntryEndpoint
  Headers     = $restRequestHeader
  ContentType = "application/json"        
};

$invokeResult =  Invoke-RestMethod @cmdletParameters;
$referredEntities = $invokeResult.referredEntities;
if ($null -eq $referredEntities) {
  Write-Output "No referred entities found under database entity!";
  exit;
}

###############################################################################
# Iterate database referred entities, find classified columns, and build T-SQL.
###############################################################################
foreach ($referredEntity in $referredEntities.psobject.Properties.GetEnumerator()) {
  $typeName = $referredEntity.Value.typeName;
  if ($null -eq $typeName -or $typeName -ne 'azure_sql_column') {
    continue;
  }
  
  $classifications = $referredEntity.Value.classifications;
  if ($null -eq $classifications) {
    continue;
  }
  
  foreach ($classification in $classifications.GetEnumerator())
  {
    if ($classification.typeName -notmatch 'Microsoft\.Label\.(?<labelId>.+)') { 
        continue;
    }

      $labelId = $Matches.labelId -replace "_", "-";

      $attributes = $referredEntity.Value.attributes;
      if ($null -eq $attributes) {
        continue;
      }
    
      $qualifiedName = $attributes.qualifiedName;
      if ($qualifiedName -notmatch '.+\.database\.windows\.net\/.+\/(?<schemaName>.+)\/(?<tableName>.+)#(?<columnName>.+)') {
        continue;
      }

      $schemaName = $Matches.schemaName;
      $tableName = $Matches.tableName;
      $columnName = $Matches.columnName;
      
      Write-Output "ADD SENSITIVITY CLASSIFICATION TO ${schemaName}.${tableName}.${columnName} WITH (LABEL='Azure Purview Label', LABEL_ID='${labelId}');";
  }
}
```

### <a name="for-azure-synapse-analytics"></a>Für Azure Synapse Analytics

```powershell
# Replace the values below with the relevant values for your environment
$TenantID = 'your_tenant_id'
$ClientID = 'your_client_id'
$SecretID = 'your_secret_id'
$purviewAccountName='purview_account_name'
$dwDatabaseName="mssql://dw_server_name.database.windows.net/dw_name"

###############################################################################
# Get an access accessToken, and build REST Request Header.
###############################################################################
$cmdletParameters = @{
  Method  = "POST"
  URI     = "https://login.microsoftonline.com/$TenantID/oauth2/token"
  Headers = @{
    "Content-Type" = 'application/x-www-form-urlencoded'
  }
  Body    = "client_id=$ClientID&client_secret=$SecretID&resource=https://purview.azure.net&grant_type=client_credentials"
}

$invokeResult = Invoke-RestMethod @cmdletParameters;
$accessToken = $invokeResult.access_token; 

$restRequestHeader = @{
  "authorization" = "Bearer $accessToken"
};

###############################################################################
# Get database entity.
###############################################################################
$getDatabaseEntityEntryEndpoint = "https://" + $purviewAccountName + ".catalog.purview.azure.com/api/atlas/v2/entity/uniqueAttribute/type/azure_sql_dw?attr:qualifiedName=" + $dwDatabaseName;
$cmdletParameters = @{
  Method      = "Get"
  Uri         = $getDatabaseEntityEntryEndpoint
  Headers     = $restRequestHeader
  ContentType = "application/json"        
};

$invokeResult =  Invoke-RestMethod @cmdletParameters;
$referredEntities = $invokeResult.referredEntities;
if ($null -eq $referredEntities) {
  Write-Output "No referred entities found under database entity!";
  exit;
}

###############################################################################
# Iterate database referred entities, find classified columns, and build T-SQL.
###############################################################################
foreach ($referredEntity in $referredEntities.psobject.Properties.GetEnumerator()) {
  $typeName = $referredEntity.Value.typeName;
  if ($null -eq $typeName -or $typeName -ne 'azure_sql_dw_column') {
    continue;
  }
  
  $classifications = $referredEntity.Value.classifications;
  if ($null -eq $classifications) {
    continue;
  }
  
  foreach ($classification in $classifications.GetEnumerator())
  {
    if ($classification.typeName -notmatch 'Microsoft\.Label\.(?<labelId>.+)') { 
        continue;
    }

      $labelId = $Matches.labelId -replace "_", "-";

      $attributes = $referredEntity.Value.attributes;
      if ($null -eq $attributes) {
        continue;
      }
    
      $qualifiedName = $attributes.qualifiedName;
      if ($qualifiedName -notmatch '.+\.database\.windows\.net\/.+\/(?<schemaName>.+)\/(?<tableName>.+)#(?<columnName>.+)') {
        continue;
      }

      $schemaName = $Matches.schemaName;
      $tableName = $Matches.tableName;
      $columnName = $Matches.columnName;
      
      Write-Output "ADD SENSITIVITY CLASSIFICATION TO ${schemaName}.${tableName}.${columnName} WITH (LABEL='Azure Purview Label', LABEL_ID='${labelId}');";
  }
}
```

## <a name="run-the-t-sql-command-on-your-sql-asset"></a>Ausführen des T-SQL-Befehls für die SQL-Ressource

1. Stellen Sie mithilfe eines Tools Ihrer Wahl eine Verbindung mit Azure SQL-Datenbank oder Azure Synapse her.
2. Führen Sie den T-SQL-Befehl aus, den Sie im vorherigen Abschnitt kopiert haben.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure PowerShell finden Sie in der [Azure PowerShell-Dokumentation](/powershell/).

Weitere Informationen zu Azure Purview finden Sie in der [Dokumentation zu Azure Purview](../../purview/index.yml).