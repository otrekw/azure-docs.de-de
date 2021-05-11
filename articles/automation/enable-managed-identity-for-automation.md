---
title: Aktivieren einer verwalteten Identität für Ihr Azure Automation-Konto (Vorschau)
description: In diesem Artikel wird beschrieben, wie Sie eine verwaltete Identität für Azure Automation-Konten einrichten.
services: automation
ms.subservice: process-automation
ms.date: 04/28/2021
ms.topic: conceptual
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 1bdba095c18943be5b367bd605d1a22d6eb6499f
ms.sourcegitcommit: f6b76df4c22f1c605682418f3f2385131512508d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2021
ms.locfileid: "108323668"
---
# <a name="enable-a-managed-identity-for-your-azure-automation-account-preview"></a>Aktivieren einer verwalteten Identität für Ihr Azure Automation-Konto (Vorschau)

In diesem Thema erfahren Sie, wie eine verwaltete Identität Azure Automations-Konten erstellt und für den Zugriff auf andere Ressourcen verwendet wird. Weitere Informationen zur Funktionsweise der verwalteten Identität mit Azure Automation finden Sie unter [Verwaltete Identitäten](automation-security-overview.md#managed-identities-preview).

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto und ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen. Sowohl die verwaltete Identität als auch die Azure-Zielressourcen, die Ihr Runbook mit dieser Identität verwaltet, müssen sich im selben Azure-Abonnement befinden.

- Die neueste Version der Azure-Kontomodule. Derzeit ist dies 2.2.8. (Ausführliche Informationen zu dieser Version finden Sie unter [Az.Accounts](https://www.powershellgallery.com/packages/Az.Accounts/).)

- Eine Azure-Ressource, auf die Sie über Ihr Automation-Runbook zugreifen möchten. Für diese Ressource muss eine Rolle für die verwaltete Identität definiert sein, mit der das Automation-Runbook den Zugriff auf die Ressource authentifizieren kann. Zum Hinzufügen von Rollen müssen Sie Besitzer der Ressource im entsprechenden Azure AD sein.

- Wenn Sie Hybridaufträge mit einer verwalteten Identität ausführen möchten, aktualisieren Sie die Hybrid Runbook Worker auf die neueste Version. Mindestens erforderliche Version:

   - Windows Hybrid Runbook Worker: Version 7.3.1125.0
   - Linux Hybrid Runbook Worker: Version 1.7.4.0

## <a name="enable-system-assigned-identity"></a>Aktivieren systemseitig zugewiesener Identitäten

>[!IMPORTANT]
>Die neue Identität auf Automation-Kontoebene überschreibt alle vorherigen vom System zugewiesenen Identitäten auf VM-Ebene, die unter Verwendung der [Runbook-Authentifizierung](./automation-hrw-run-runbooks.md#runbook-auth-managed-identities) mit verwalteten Identitäten beschrieben sind. Wenn Sie Hybridaufträge auf virtuellen Azure-Computern ausführen, die die vom System zugewiesene Identität eines virtuellen Computers für den Zugriff auf Runbookressourcen verwenden, wird die Identität des Automation-Kontos für die Hybridaufträge verwendet. Dies bedeutet, dass ihre vorhandene Auftragsausführung beeinträchtigt werden kann, wenn Sie das CmK-Feature (Customer Managed Keys) Ihres Automation-Kontos verwendet haben.<br/><br/>Wenn Sie die verwaltete Identität des virtuellen Computers weiterhin verwenden möchten, sollten Sie die Identität auf Automation-Kontoebene nicht aktivieren. Wenn Sie dies bereits aktiviert haben, können Sie die verwaltete Identität des Automation-Kontos deaktivieren. Weitere Informationen finden Sie unter [verwaltete Identität ihres Azure Automation-Kontos deaktivieren](./disable-managed-identity-for-automation.md).

Das Einrichten der vom System zugewiesenen Identitäten für Azure Automation kann auf zwei Arten erfolgen. Sie können das Azure-Portal oder die CLI Azure REST-API verwenden, um die Gruppe zu erstellen.

>[!NOTE]
>Vom Benutzer zugewiesene Identitäten werden noch nicht unterstützt.

### <a name="enable-system-assigned-identity-in-azure-portal"></a>Aktivieren der systemseitig zugewiesenen Identität im Azure-Portal

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Navigieren Sie zu Ihrem Automation-Konto, und wählen Sie **Identität** unter **Kontoeinstellungen** aus.

1. Legen Sie die Option **System zugewiesen** auf **Ein** fest und klicken Sie auf **Speichern**. Wenn Sie aufgefordert werden, die Auswahl zu bestätigen, klicken Sie auf **Ja**.

:::image type="content" source="media/managed-identity/managed-identity-on.png" alt-text="Aktivieren der systemseitig zugewiesenen Identität im Azure-Portal.":::

In Ihrem Automation-Konto kann nun die systemseitig zugewiesene Identität verwendet werden, die bei Azure Active Directory (Azure ID) registriert ist und durch eine Objekt-ID angegeben wird.

:::image type="content" source="media/managed-identity/managed-identity-object-id.png" alt-text="Objekt-ID der verwalteten Identität.":::

### <a name="enable-system-assigned-identity-through-the-rest-api"></a>Aktivieren der vom System zugezuweisenen Identität über die REST-API

Sie können eine systemseitig zugewiesene verwaltete Identität mit dem folgenden REST-API-Aufruf für das Automation-Konto konfigurieren.

```http
PATCH https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resource-group-name/providers/Microsoft.Automation/automationAccounts/automation-account-name?api-version=2020-01-13-preview
```

Anforderungstext
```json
{ 
 "identity": 
 { 
  "type": "SystemAssigned" 
  } 
}
```

```json
{
 "name": "automation-account-name",
 "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resource-group-name/providers/Microsoft.Automation/automationAccounts/automation-account-name",
 .
 .
 "identity": {
    "type": "SystemAssigned",
    "principalId": "aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa",
    "tenantId": "bbbbbbbb-bbbb-bbbb-bbbb-bbbbbbbbbbbb"
 },
.
.
}
```

|Eigenschaft (JSON) | Wert | BESCHREIBUNG|
|----------|-----------|------------|
| principalid | \<principal-ID\> | Die GUID (Globally Unique Identifier) des Dienstprinzipalobjekts für die verwaltete Identität, die das Automation-Konto im Azure AD-Mandanten angibt. Diese GUID wird manchmal als „Objekt-ID“ oder „objectID“ angezeigt. |
| tenantid | \<Azure-AD-tenant-ID\> | Die GUID (Globally Unique Identifier), die den Azure AD-Mandanten angibt, in dem das Automation-Konto nun Mitglied ist. Unter dem Azure AD-Mandanten hat der Dienstprinzipal den gleichen Namen wie das Automation-Konto. |

## <a name="give-identity-access-to-azure-resources-by-obtaining-a-token"></a>Gewähren des Identitätszugriffs auf Azure-Ressourcen durch Abrufen eines Tokens

Ein Automation-Konto kann mithilfe ihrer verwalteten Identität Token für den Zugriff auf andere durch Azure AD geschützte Ressourcen wie z. B. Azure Key Vault abrufen. Diese Token repräsentieren keinen bestimmten Benutzer der Anwendung. Stattdessen stellen sie die Anwendung dar, die auf die Ressource zugreift. In diesem Fall stellt das Token beispielsweise ein Automation-Konto dar.

Bevor Sie die systemseitig verwaltete Identität für die Authentifizierung verwenden können, richten Sie den Zugriff dieser Identität auf die Azure-Ressource ein, in der Sie die Identität verwenden möchten. Für diese Aufgabe muss der Identität in der Azure-Zielressource die entsprechende Rolle zugewiesen werden.

In diesem Beispiel wird Azure PowerShell verwendet, um zu zeigen, wie die Rolle Mitwirkender im Abonnement der Azure-Zielressource zugewiesen wird. Die Rolle Mitwirkender wird als Beispiel verwendet und ist in Ihrem Fall möglicherweise erforderlich.

```powershell
New-AzRoleAssignment -ObjectId <automation-Identity-object-id> -Scope "/subscriptions/<subscription-id>" -RoleDefinitionName "Contributor"
```

## <a name="authenticate-access-with-managed-identity"></a>Authentifizieren des Zugriffs mit der verwalteten Identität

Nachdem Sie die verwaltete Identität für Ihr Automation-Konto aktiviert und einer Identität Zugriff auf die Zielressource gegeben haben, können Sie diese Identität in Runbooks für Ressourcen angeben, die die verwaltete Identität unterstützen. Verwenden Sie für die Identitätsunterstützung das Az cmdlet `Connect-AzAccount` cmdlet. Weitere Informationen finden Sie in der PowerShell-Referenz unter [Connect-AzAccount](/powershell/module/az.accounts/Connect-AzAccount).

```powershell
Connect-AzAccount -Identity
```

>[!NOTE]
>Wenn Ihre Organisation weiterhin die veralteten AzureRM-Cmdlets verwendet, können Sie `Connect-AzureRMAccount -Identity` verwenden.

## <a name="generate-an-access-token-without-using-azure-cmdlets"></a>Generieren eines Zugriffstokens ohne Verwendung von Azure-Cmdlets

Stellen Sie für HTTP-Endpunkte Folgendes sicher.
- Der Metadatenheader muss vorhanden sein und auf „true“ festgelegt werden.
- Eine Ressource muss zusammen mit der Anforderung als Abfrageparameter für eine GET-Anforderung und als Formulardaten für eine POST-Anforderung übergeben werden.
- Der X-IDENTITY-HEADER sollte auf den Wert der Umgebungsvariablen festgelegt werden, IDENTITY_HEADER für Hybrid Runbook Workers. 
- Der Inhaltstyp für die Post-Anforderung muss „application/x-www-form-urlencoded“ sein. 

### <a name="sample-get-request"></a>Abrufbeipsielanforderung

```powershell
$resource= "?resource=https://management.azure.com/" 
$url = $env:IDENTITY_ENDPOINT + $resource 
$Headers = New-Object "System.Collections.Generic.Dictionary[[String],[String]]" 
$Headers.Add("X-IDENTITY-HEADER", $env:IDENTITY_HEADER) 
$Headers.Add("Metadata", "True") 
$accessToken = Invoke-RestMethod -Uri $url -Method 'GET' -Headers $Headers
Write-Output $accessToken.access_token
```

### <a name="sample-post-request"></a>POST-Beispielanforderung
```powershell
$url = $env:IDENTITY_ENDPOINT  
$headers = New-Object "System.Collections.Generic.Dictionary[[String],[String]]" 
$headers.Add("X-IDENTITY-HEADER", $env:IDENTITY_HEADER) 
$headers.Add("Metadata", "True") 
$body = @{resource='https://management.azure.com/' } 
$accessToken = Invoke-RestMethod $url -Method 'POST' -Headers $headers -ContentType 'application/x-www-form-urlencoded' -Body $body 
Write-Output $accessToken.access_token
```

## <a name="sample-runbooks-using-managed-identity"></a>Beispiel-Runbooks mit verwalteter Identität

### <a name="sample-runbook-to-access-a-sql-database-without-using-azure-cmdlets"></a>Beispiel-Runbook für den Zugriff auf eine SQL-Datenbank ohne Verwendung von Azure-Cmdlets

Stellen Sie sicher, dass Sie eine Identität aktiviert haben, bevor Sie dieses Skript ausprobieren. Siehe [Aktivieren systemseitig zugewiesener Identitäten](#enable-system-assigned-identity).

Ausführliche Informationen zum Bereitstellen des Zugriffs auf eine Azure SQL-Datenbank finden Sie unter [Bereitstellen eines Azure AD-Administrators (SQL-Datenbank)](../azure-sql/database/authentication-aad-configure.md#provision-azure-ad-admin-sql-database).

```powershell
$queryParameter = "?resource=https://database.windows.net/" 
$url = $env:IDENTITY_ENDPOINT + $queryParameter
$Headers = New-Object "System.Collections.Generic.Dictionary[[String],[String]]" 
$Headers.Add("X-IDENTITY-HEADER", $env:IDENTITY_HEADER) 
$Headers.Add("Metadata", "True") 
$content =[System.Text.Encoding]::Default.GetString((Invoke-WebRequest -UseBasicParsing -Uri $url -Method 'GET' -Headers $Headers).RawContentStream.ToArray()) | ConvertFrom-Json 
$Token = $content.access_token 
echo "The managed identities for Azure resources access token is $Token" 
$SQLServerName = "<ServerName>"    # Azure SQL logical server name  
$DatabaseName = "<DBname>"     # Azure SQL database name 
Write-Host "Create SQL connection string" 
$conn = New-Object System.Data.SqlClient.SQLConnection  
$conn.ConnectionString = "Data Source=$SQLServerName.database.windows.net;Initial Catalog=$DatabaseName;Connect Timeout=30" 
$conn.AccessToken = $Token 
Write-host "Connect to database and execute SQL script" 
$conn.Open()  
$ddlstmt = "CREATE TABLE Person( PersonId INT IDENTITY PRIMARY KEY, FirstName NVARCHAR(128) NOT NULL)" 
Write-host " " 
Write-host "SQL DDL command" 
$ddlstmt 
$command = New-Object -TypeName System.Data.SqlClient.SqlCommand($ddlstmt, $conn) 
Write-host "results" 
$command.ExecuteNonQuery() 
$conn.Close()
```

### <a name="sample-runbook-to-access-a-key-vault-using-azure-cmdlets"></a>Beispiel-Runbook für den Zugriff auf einen Schlüsseltresor mithilfe von Azure-Cmdlets

Stellen Sie sicher, dass Sie eine Identität aktiviert haben, bevor Sie dieses Skript ausprobieren. Siehe [Aktivieren systemseitig zugewiesener Identitäten](#enable-system-assigned-identity).

Weitere Informationen finden Sie unter [Get-AzKeyVaultSecret](/powershell/module/az.keyvault/get-azkeyvaultsecret).

```powershell
Write-Output "Connecting to azure via  Connect-AzAccount -Identity" 
Connect-AzAccount -Identity 
Write-Output "Successfully connected with Automation account's Managed Identity" 
Write-Output "Trying to fetch value from key vault using MI. Make sure you have given correct access to Managed Identity" 
$secret = Get-AzKeyVaultSecret -VaultName '<KVname>' -Name '<KeyName>' 

$ssPtr = [System.Runtime.InteropServices.Marshal]::SecureStringToBSTR($secret.SecretValue) 
try { 
  $secretValueText = [System.Runtime.InteropServices.Marshal]::PtrToStringBSTR($ssPtr) 
    Write-Output $secretValueText 
} finally { 
    [System.Runtime.InteropServices.Marshal]::ZeroFreeBSTR($ssPtr) 
}
```

### <a name="sample-python-runbook-to-get-a-token"></a>Python-Beispiel-Runbook zum Abrufen eines Tokens

Stellen Sie sicher, dass Sie eine Identität aktiviert haben, bevor Sie dieses Runbook ausprobieren. Siehe [Aktivieren systemseitig zugewiesener Identitäten](#enable-system-assigned-identity).

```python
#!/usr/bin/env python3 
import os 
import requests  
# printing environment variables 
endPoint = os.getenv('IDENTITY_ENDPOINT')+"?resource=https://management.azure.com/" 
identityHeader = os.getenv('IDENTITY_HEADER') 
payload={} 
headers = { 
  'X-IDENTITY-HEADER': identityHeader,
  'Metadata': 'True' 
} 
response = requests.request("GET", endPoint, headers=headers, data=payload) 
print(response.text) 
```

## <a name="next-steps"></a>Nächste Schritte

- Wenn Ihre Runbooks nicht erfolgreich abgeschlossen werden, lesen Sie [Behandlung von Problemen mit der verwalteten Azure Automation-Identität (Vorschau)](troubleshoot/managed-identity.md).

- Wenn Sie eine verwaltete Identität deaktivieren müssen, finden Sie weitere Informationen unter [ Deaktivieren der verwalteten Identität Ihres Azure Automation-Kontos (Vorschauversion)](disable-managed-identity-for-automation.md).

- Eine Übersicht über die Azure Automation-Kontosicherheit finden Sie unter [Übersicht über die Automation-Kontoauthentifizierung.](automation-security-overview.md)
