---
title: Verschlüsselung sicherer Ressourcen in Azure Automation
description: Dieser Artikel enthält Konzepte zum Konfigurieren des Automation-Kontos für die Verwendung der Verschlüsselung.
services: automation
ms.service: automation
ms.subservice: process-automation
author: snehithm
ms.author: snmuvva
ms.date: 01/11/2020
ms.topic: conceptual
manager: kmadnani
ms.openlocfilehash: b46cf3a742158a3347b43a1e9bc6d62d0b2160d4
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104773690"
---
# <a name="encryption-of-secure-assets-in-azure-automation"></a>Verschlüsselung sicherer Ressourcen in Azure Automation

Zu den sicheren Objekten in Azure Automation gehören Anmeldeinformationen, Zertifikate, Verbindungen und verschlüsselte Variablen. Diese Ressourcen werden in Azure Automation mithilfe mehrerer Verschlüsselungsstufen geschützt. Basierend auf dem Schlüssel der obersten Ebene, der für die Verschlüsselung verwendet wird, gibt es zwei Modelle für die Verschlüsselung:

- Verwendung der von Microsoft verwalteten Schlüssel
- Verwenden von Schlüsseln, die Sie verwalten

## <a name="microsoft-managed-keys"></a>Von Microsoft verwaltete Schlüssel

Standardmäßig verwendet Ihr Azure Automation-Konto von Microsoft verwaltete Schlüssel.

Jede sichere Ressource wird in Azure Automation mit einem eindeutigen Schlüssel (Datenverschlüsselungsschlüssel), der für jedes Automation-Konto generiert wird, verschlüsselt und gespeichert. Diese Schlüssel selbst werden mithilfe eines anderen eindeutigen Schlüssels, der für jedes Konto generiert wird, verschlüsselt und in Azure Automation gespeichert. Dieser Schlüssel wird als Kontoverschlüsselungsschlüssel (Account Encryption Key, AEK) bezeichnet. Diese Kontoverschlüsselungsschlüssel werden mithilfe von von Microsoft verwalteten Schlüsseln in Azure Automation verschlüsselt und gespeichert. 

## <a name="keys-that-you-manage-with-key-vault-preview"></a>Schlüssel, die Sie mit Key Vault (Vorschau) verwalten

Sie können die Verschlüsselung sicherer Ressourcen für Ihr Automation-Konto mit Ihren eigenen Schlüsseln verwalten. Wenn Sie einen kundenseitig verwalteten Schlüssel auf der Ebene des Automation-Kontos angeben, wird dieser Schlüssel zum Schutz und zur Kontrolle des Zugriffs auf den Kontoverschlüsselungsschlüssel für das Automation-Konto verwendet. Dieser wiederum wird zum Ver- und Entschlüsseln aller sicheren Ressourcen verwendet. Vom Kunden verwaltete Schlüssel bieten größere Flexibilität beim Erstellen, Rotieren, Deaktivieren und Widerrufen von Zugriffssteuerungen. Sie können auch die zum Schutz Ihrer sicheren Ressourcen verwendeten Verschlüsselungsschlüssel überwachen.

Verwenden Sie Azure Key Vault, um kundenseitig verwaltete Schlüssel zu speichern. Sie können entweder Ihre eigenen Schlüssel erstellen und in einem Schlüsseltresor speichern oder mit den Azure Key Vault-APIs Schlüssel generieren.  Weitere Informationen zum Azure-Schlüsseltresor finden Sie unter [Was ist der Azure-Schlüsseltresor?](../key-vault/general/overview.md)

## <a name="use-of-customer-managed-keys-for-an-automation-account"></a>Verwenden von kundenseitig verwalteten Schlüsseln für ein Automation-Konto

Wenn Sie die Verschlüsselung mit kundenseitig verwalteten Schlüsseln für ein Automation-Konto verwenden, wird der Kontoverschlüsselungsschlüssel von Azure Automation mit dem kundenseitig verwalteten Schlüssel im zugeordneten Schlüsseltresor umschlossen. Das Aktivieren von kundenseitig verwalteten Schlüsseln wirkt sich nicht auf die Leistung aus. Das Konto wird ohne Verzögerung sofort mit dem neuen Schlüssel verschlüsselt.

Ein neues Automation-Konto wird immer mit von Microsoft verwalteten Schlüsseln verschlüsselt. Es ist nicht möglich, zum Zeitpunkt der Kontoerstellung vom Kunden verwaltete Schlüssel zu aktivieren. Kundenseitig verwaltete Schlüssel werden in Azure Key Vault gespeichert. Für den Schlüsseltresor müssen Zugriffsrichtlinien bereitgestellt werden, mit denen Schlüsselberechtigungen für die verwaltete Identität erteilt werden, die dem Automation-Konto zugeordnet ist. Die verwaltete Identität ist erst verfügbar, nachdem das Speicherkonto erstellt wurde.

Wenn Sie den für die Verschlüsselung sicherer Azure Automation-Ressourcen verwendeten Schlüssel ändern, indem Sie kundenseitig verwaltete Schlüssel aktivieren oder deaktivieren, die Schlüsselversion aktualisieren oder einen anderen Schlüssel angeben, wird die Verschlüsselung des Kontoverschlüsselungsschlüssels zwar geändert, aber die sicheren Ressourcen in Ihrem Azure Automation-Konto müssen nicht erneut verschlüsselt werden.

> [!NOTE] 
> Um kundenseitig verwaltete Schlüssel zu aktivieren, müssen Sie Azure Automation REST-API-Aufrufe mithilfe der API-Version 2020-01-13-preview durchführen.

### <a name="prerequisites-for-using-customer-managed-keys-in-azure-automation"></a>Voraussetzungen für die Verwendung kundenseitig verwalteter Schlüssel in Azure Automation

Vor dem Aktivieren kundenseitig verwalteter Schlüssel für ein Automation-Konto müssen Sie sicherstellen, dass die folgenden Voraussetzungen erfüllt sind:

 - Der kundenseitig verwaltete Schlüssel muss in einem Azure Key Vault gespeichert sein. 
 - Aktivieren Sie die beiden Eigenschaften **Vorläufiges Löschen** und **Nicht bereinigen** im Schlüsseltresor. Diese Funktionen sind erforderlich, um die Wiederherstellung von Schlüsseln im Falle einer versehentlichen Löschung zu ermöglichen.
 - Für die Azure Automation-Verschlüsselung werden nur RSA-Schlüssel unterstützt. Weitere Informationen zu Schlüsseln finden Sie unter [Informationen zu Schlüsseln, Geheimnissen und Zertifikaten in Azure Key Vault](../key-vault/general/about-keys-secrets-certificates.md).
- Das Automation-Konto und der Schlüsseltresor können sich in verschiedenen Abonnements befinden, müssen aber im selben Azure Active Directory-Mandanten sein.

### <a name="assignment-of-an-identity-to-the-automation-account"></a>Zuweisung einer Identität zum Automation-Konto

Wenn Sie kundenseitig verwaltete Schlüssel mit einem Automation-Konto verwenden möchten, muss sich Ihr Automation-Konto bei dem Schlüsseltresor authentifizieren, der kundenseitig verwaltete Schlüssel speichert. Azure Automation verwendet systemseitig zugewiesene verwaltete Identitäten, um das Konto bei Azure Key Vault zu authentifizieren. Weitere Informationen zu verwalteten Identitäten finden Sie unter [Was sind verwaltete Identitäten für Azure-Ressourcen?](../active-directory/managed-identities-azure-resources/overview.md).

Konfigurieren einer systemseitig zugewiesenen verwalteten Identität mit dem folgenden REST-API-Aufruf für das Automation-Konto:

```http
PATCH https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resource-group-name/providers/Microsoft.Automation/automationAccounts/automation-account-name?api-version=2020-01-13-preview
```

Anforderungstext:

```json
{ 
 "identity": 
 { 
  "type": "SystemAssigned" 
  } 
}
```

Die systemseitig zugewiesene Identität für das Automation-Konto wird in einer Antwort ähnlich der folgenden zurückgegeben:

```json
{
 "name": "automation-account-name",
 "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resource-group-name/providers/Microsoft.Automation/automationAccounts/automation-account-name",
 ..
 "identity": {
    "type": "SystemAssigned",
    "principalId": "aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa",
    "tenantId": "bbbbbbbb-bbbb-bbbb-bbbb-bbbbbbbbbbbb"
 },
..
}
```

### <a name="configuration-of-the-key-vault-access-policy"></a>Konfiguration der Key Vault-Zugriffsrichtlinie

Nachdem dem Automation-Konto eine verwaltete Identität zugewiesen wurde, konfigurieren Sie den Zugriff auf den Schlüsseltresor, in dem kundenseitig verwaltete Schlüssel gespeichert sind. Azure Automation erfordert **Get**, **recover**, **wrapKey**, **UnwrapKey** für die kundenseitig verwalteten Schlüssel.

Eine solche Zugriffsrichtlinie kann mit dem folgenden REST-API-Aufruf festgelegt werden:

```http
PUT https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/sample-group/providers/Microsoft.KeyVault/vaults/sample-vault/accessPolicies/add?api-version=2018-02-14
```

Anforderungstext:

```json
{
  "properties": {
    "accessPolicies": [
      {
        "tenantId": "bbbbbbbb-bbbb-bbbb-bbbb-bbbbbbbbbbbb",
        "objectId": "aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa",
        "permissions": {
          "keys": [
            "get",
            "recover",
            "wrapKey",
            "unwrapKey"
          ],
          "secrets": [],
          "certificates": []
        }
      }
    ]
  }
}
```

> [!NOTE]
> Für die Felder **tenantId** und **objectId** müssen die Werte von **identity.tenantId** und **identity.principalId** aus der Antwort der verwalteten Identität für das Automation-Konto bereitgestellt werden.

### <a name="change-the-configuration-of-automation-account-to-use-customer-managed-key"></a>Ändern der Konfiguration des Automation-Kontos zur Verwendung von kundenseitig verwalteten Schlüsseln

Schließlich können Sie mit dem folgenden REST-API-Aufruf für Ihr Automation-Konto zwischen von Microsoft verwalteten Schlüsseln und kundenseitig verwalteten Schlüsseln wechseln:

```http
PATCH https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resource-group-name/providers/Microsoft.Automation/automationAccounts/automation-account-name?api-version=2020-01-13-preview
```

Anforderungstext:

```json
{
    "identity": {
    "type": "SystemAssigned"
    },
    "properties": {
        "encryption": {
            "keySource": "Microsoft.Keyvault",
            "keyvaultProperties": {
                "keyName": "sample-vault-key",
                "keyvaultUri": "https://sample-vault-key12.vault.azure.net",
                "keyVersion": "7c73556c521340209371eaf623cc099d"
            }
        }
    }
}
```

Beispiel für eine Antwort

```json
{
  "name": "automation-account-name",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resource-group-name/providers/Microsoft.Automation/automationAccounts/automation-account-name",
  ..
  "properties": {
    ..
    "encryption": {
      "keyvaultProperties": {
         "keyName": "sample-vault-key",
          "keyvaultUri": "https://sample-vault-key12.vault.azure.net",
          "keyVersion": "7c73556c521340209371eaf623cc099d"
      },
      "keySource": "Microsoft.Keyvault"
    },
    ..
  }
}
```

## <a name="rotation-of-a-customer-managed-key"></a>Rotation eines kundenseitig verwalteten Schlüssels

Sie können einen vom Kunden verwalteten Schlüssel in Azure Key Vault entsprechend Ihren Konformitätsrichtlinien rotieren. Wenn der Schlüssel rotiert wurde, müssen Sie das Automation-Konto aktualisieren, damit der neue Schlüssel-URI verwendet wird.

Durch Rotieren des Schlüssels wird keine Neuverschlüsselung sicherer Ressourcen im Automation-Konto ausgelöst. Es ist keine weitere Aktion erforderlich.

## <a name="revocation-of-access-to-a-customer-managed-key"></a>Widerrufen des Zugriffs auf einen kundenseitig verwalteten Schlüssel

Zum Widerrufen des Zugriffs auf von Kunden verwaltete Schlüssel können Sie PowerShell oder die Azure-Befehlszeilenschnittstelle verwenden. Weitere Informationen finden Sie unter [Azure Key Vault – PowerShell](/powershell/module/az.keyvault/) oder [Azure Key Vault – CLI](/cli/azure/keyvault). Durch das Widerrufen des Zugriffs wird der Zugriff auf alle sicheren Ressourcen im Automation-Konto wirksam blockiert, da Azure Automation keinen Zugriff mehr auf den Verschlüsselungsschlüssel hat.

## <a name="next-steps"></a>Nächste Schritte

- Informationen zu Azure Key Vault finden Sie unter [Was ist Azure Key Vault?](../key-vault/general/overview.md).
- Informationen zum Arbeiten mit Zertifikaten finden Sie unter [Verwalten von Zertifikaten in Azure Automation](shared-resources/certificates.md).
- Informationen zum Behandeln von Anmeldeinformationen finden Sie unter [Verwalten von Anmeldeinformationen in Azure Automation](shared-resources/credentials.md).
- Informationen zum Verwalten von Automation-Variablen finden Sie unter [Verwalten von Variablen in Azure Automation](shared-resources/variables.md).
- Hilfe beim Arbeiten mit Verbindungen finden Sie unter [Verwalten von Verbindungen in Azure Automation](automation-connections.md).
