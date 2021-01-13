---
title: Abrufen von Informationen zu Verschlüsselungsschlüsseln
titleSuffix: Azure Cognitive Search
description: Rufen Sie den Namen und die in einem Index oder einer Synonymzuordnung verwendete Verschlüsselungsschlüsselversion ab, um den Schlüssel in Azure Key Vault verwalten zu können.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 08/01/2020
ms.openlocfilehash: 37ff94608e9756142f70a4f3c64d0a6f7eeea685
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "88932898"
---
# <a name="get-customer-managed-key-information-from-indexes-and-synonym-maps"></a>Abrufen von Informationen zu kundenseitig verwalteten Schlüsseln aus Indizes und Synonymzuordnungen

Bei Azure Cognitive Search werden kundenseitig verwaltete Verschlüsselungsschlüssel erstellt, gespeichert und in Azure Key Vault verwaltet. Wenn Sie ermitteln müssen, ob ein Objekt verschlüsselt ist, oder welcher Schlüsselname oder welche Version verwendet wurde, verwenden Sie die REST-API oder ein SDK, um die Eigenschaft **encryptionKey** aus einer Index- oder Synonymzuordnungsdefinition abzurufen. 

Wir empfehlen die [Aktivierung der Protokollierung](../key-vault/general/logging.md) in Azure Key Vault, damit Sie die Schlüsselverwendung überwachen können.

## <a name="get-the-admin-api-key"></a>Abrufen des Admin-API-Schlüssels

Um Objektdefinitionen von einem Suchdienst zu abzurufen, müssen Sie sich mit Administratorrechten authentifizieren. Der einfachste Weg, den Admin-API-Schlüssel zu erhalten, ist über das Portal.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an, und öffnen Sie die Übersichtsseite des Suchdiensts.

1. Klicken Sie auf der linken Seite auf **Schlüssel**, und kopieren Sie eine Admin-API. Zum Abrufen von Index und Synonymzuordnungen ist ein Administratorschlüssel erforderlich.

Wechseln Sie für die restlichen Schritte zu PowerShell und der REST-API. Das Portal zeigt weder Synonymzuordnungen noch die Verschlüsselungsschlüsseleigenschaften von Indizes an.

## <a name="use-powershell-and-rest"></a>Verwenden von PowerShell und REST

Führen Sie die folgenden Befehle aus, um die Variablen einzurichten und Objektdefinitionen abzurufen.

```powershell
<# Connect to Azure #>
$Connect-AzAccount

<# Provide the admin API key used for search service authentication  #>
$headers = @{
'api-key' = '<YOUR-ADMIN-API-KEY>'
'Content-Type' = 'application/json'
'Accept' = 'application/json' }

<# List all existing synonym maps #>
$uri= 'https://<YOUR-SEARCH-SERVICE>.search.windows.net/synonyms?api-version=2020-06-30&$select=name'
Invoke-RestMethod -Uri $uri -Headers $headers | ConvertTo-Json

<# List all existing indexes #>
$uri= 'https://<YOUR-SEARCH-SERVICE>.search.windows.net/indexes?api-version=2020-06-30&$select=name'
Invoke-RestMethod -Uri $uri -Headers $headers | ConvertTo-Json

<# Return a specific synonym map definition. The encryptionKey property is at the end #>
$uri= 'https://<YOUR-SEARCH-SERVICE>.search.windows.net/synonyms/<YOUR-SYNONYM-MAP-NAME>?api-version=2020-06-30'
Invoke-RestMethod -Uri $uri -Headers $headers | ConvertTo-Json

<# Return a specific index definition. The encryptionKey property is at the end #>
$uri= 'https://<YOUR-SEARCH-SERVICE>.search.windows.net/indexes/<YOUR-INDEX-NAME>?api-version=2020-06-30'
Invoke-RestMethod -Uri $uri -Headers $headers | ConvertTo-Json
```

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun wissen, welcher Verschlüsselungsschlüssel und welche Version verwendet wird, können Sie den Schlüssel in Azure Key Vault verwalten oder andere Konfigurationseinstellungen überprüfen.

+ [Schnellstart: Festlegen eines Geheimnisses und Abrufen des Geheimnisses aus Azure Key Vault mithilfe von PowerShell](../key-vault/secrets/quick-create-powershell.md)

+ [Konfigurieren von kundenseitig verwalteten Schlüsseln für die Datenverschlüsselung in Azure Cognitive Search](search-security-manage-encryption-keys.md)