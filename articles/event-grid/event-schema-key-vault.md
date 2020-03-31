---
title: Azure Event Grid-Ereignisschema für Azure Key Vault
description: Beschreibt die Eigenschaften und das Schema, die für Azure Key Vault-Ereignisse im Azure Event Grid verfügbar sind
services: event-grid
author: msmbaldwin
ms.service: event-grid
ms.topic: reference
ms.date: 10/25/2019
ms.author: mbaldwin
ms.openlocfilehash: 17404388b2b6c3fee1c6ab666f7233a66817f642
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "74082873"
---
# <a name="azure-event-grid-event-schema-for-azure-key-vault-preview"></a>Azure Event Grid-Ereignisschema für Azure Key Vault (Vorschau)

Dieser Artikel enthält die Eigenschaften und das Schema für Ereignisse in [Azure Key Vault](../key-vault/index.yml), derzeit als Vorschauversion verfügbar. Eine Einführung in Ereignisschemas finden Sie unter [Azure Event Grid-Ereignisschema](event-schema.md).

## <a name="available-event-types"></a>Verfügbare Ereignistypen

Ein Azure Key Vault Konto generiert die folgenden Ereignistypen:

| Vollständiger Ereignisname | Anzeigename des Ereignisses | BESCHREIBUNG |
| ---------- | ----------- |---|
| Microsoft.KeyVault.CertificateNewVersionCreated | Certificate New Version Created (Neue Zertifikatversion erstellt) | Wird ausgelöst, wenn ein neues Zertifikat oder eine neue Zertifikatversion erstellt wird. |
| Microsoft.KeyVault.CertificateNearExpiry | Certificate Near Expiry (Zertifikat läuft bald ab) | Wird ausgelöst, wenn die aktuelle Version des Zertifikats abläuft. (Der Standardwert ist 30 Tage vor dem Ablaufdatum.) |
| Microsoft.KeyVault.CertificateExpired | Zertifikat abgelaufen | Wird ausgelöst, wenn das Zertifikat abgelaufen ist. |
| Microsoft.KeyVault.KeyNewVersionCreated | Key New Version Created (Neue Schlüsselversion erstellt) | Wird ausgelöst, wenn ein neuer Schlüssel oder eine neue Schlüsselversion erstellt wird. |
| Microsoft.KeyVault.KeyNearExpiry | Key Near Expiry (Schlüssel läuft bald ab) | Wird ausgelöst, wenn die aktuelle Version eines Schlüssels demnächst abläuft. (Der Standardwert ist 30 Tage vor dem Ablaufdatum.) |
| Microsoft.KeyVault.KeyExpired | Schlüssel abgelaufen | Wird ausgelöst, wenn ein Schlüssel abgelaufen ist. |
| Microsoft.KeyVault.SecretNewVersionCreated | Secret New Version Created (Neue Version des Geheimnisses erstellt) | Wird ausgelöst, wenn ein neues Geheimnis oder eine neue Geheimnisversion erstellt wird. |
| Microsoft.KeyVault.SecretNearExpiry | Secret Near Expiry (Geheimnis läuft demnächst ab) | Wird ausgelöst, wenn die aktuelle Version eines Geheimnisses demnächst abläuft. (Der Standardwert ist 30 Tage vor dem Ablaufdatum.) |
| Microsoft.KeyVault.SecretExpired | Secret Expired (Geheimnis abgelaufen) | Wird ausgelöst, wenn ein Geheimnis abgelaufen ist. |

## <a name="event-examples"></a>Ereignisbeispiele

Das folgende Beispiel zeigt das Schema für **Microsoft.KeyVault.SecretNewVersionCreated**:

```JSON
[
   {
      "id":"00eccf70-95a7-4e7c-8299-2eb17ee9ad64",
      "topic":"/subscriptions/{subscription-id}/resourceGroups/sample-rg/providers/Microsoft.KeyVault/vaults/sample-kv",
      "subject":"newsecret",
      "eventType":"Microsoft.KeyVault.SecretNewVersionCreated",
      "eventTime":"2019-07-25T01:08:33.1036736Z",
      "data":{
         "Id":"https://sample-kv.vault.azure.net/secrets/newsecret/ee059b2bb5bc48398a53b168c6cdcb10",
         "vaultName":"sample-kv",
         "objectType":"Secret",
         "objectName ":"newsecret",
         "version":" ee059b2bb5bc48398a53b168c6cdcb10",
         "nbf":"1559081980",
         "exp":"1559082102"
      },
      "dataVersion":"1",
      "metadataVersion":"1"
   }
]
```

## <a name="event-properties"></a>Ereigniseigenschaften

Ein Ereignis weist die folgenden Daten auf oberster Ebene aus:

| Eigenschaft | type | BESCHREIBUNG |
| ---------- | ----------- |---|
| id | string | Die ID des Objekts, das dieses Ereignis ausgelöst hat. |
| vaultName | string | Der Schlüsseltresorname des Objekts, das dieses Ereignis ausgelöst hat. |
| objectType | string | Der Typ des Objekts, das dieses Ereignis ausgelöst hat. |
| objectName | string | Der Name des Objekts, das dieses Ereignis ausgelöst hat. |
| version | string | Die Version des Objekts, das dieses Ereignis ausgelöst hat. |
| nbf | number | Das Anfangsdatum in Sekunden seit 1970-01-01t00:00:00Z für das Objekt, das dieses Ereignis ausgelöst hat. |
| exp | number | Das Ablaufdatum in Sekunden seit 1970-01-01t00:00:00Z des Objekts, das dieses Ereignis ausgelöst hat. |


## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung zu Azure Event Grid finden Sie unter [Einführung in Azure Event Grid](overview.md).
* Weitere Informationen zum Erstellen eines Azure Event Grid-Abonnements finden Sie unter [Event Grid-Abonnementschema](subscription-creation-schema.md).
* Weitere Informationen über die Integration von Key Vault in Event Grid finden Sie unter [Überwachen von Key Vault mit Azure Event Grid (Vorschau)](../key-vault/event-grid-overview.md).
* Ein Tutorial zur Key Vault-Integration in Event Grid finden Sie unter [Empfangen von und Reagieren auf Key Vault-Benachrichtigungen mit Azure Event Grid (Vorschau)](../key-vault/event-grid-tutorial.md).
* Weitere Anleitungen für Key Vault und Azure Automation finden Sie unter:
    - [Was ist der Azure-Schlüsseltresor?](../key-vault/key-vault-overview.md)
    - [Überwachen von Key Vault mit Azure Event Grid (Vorschau)](../key-vault/event-grid-overview.md)
    - [Empfangen von und Reagieren auf Key Vault-Benachrichtigungen mit Azure Event Grid (Vorschau)](../key-vault/event-grid-tutorial.md)
    - [Azure Automation – Übersicht](../automation/index.yml)
