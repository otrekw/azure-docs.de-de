---
title: Azure Key Vault-Protokollierung | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie den Zugriff auf Ihre Schlüsseltresore überwachen, indem Sie die Protokollierung für Azure Key Vault aktivieren, bei der Informationen im von Ihnen bereitgestellten Azure-Speicherkonto gespeichert werden.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: how-to
ms.date: 08/12/2019
ms.author: mbaldwin
ms.openlocfilehash: a51e9a628f67269357d42bd1d3af10c1d86f301a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91739781"
---
# <a name="azure-key-vault-logging"></a>Azure Key Vault-Protokollierung

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Nachdem Sie einen oder mehrere Schlüsseltresore erstellt haben, möchten Sie vermutlich überwachen, wie, wann und von wem auf die Schlüsseltresore zugegriffen wird. Hierfür können Sie die Protokollierung für Azure Key Vault aktivieren, bei der Informationen im von Ihnen bereitgestellten Azure-Speicherkonto gespeichert werden. Für Ihr angegebenes Speicherkonto wird automatisch ein neuer Container namens **insights-logs-logs-auditevent** erstellt. Sie können dasselbe Speicherkonto verwenden, um Protokolle für mehrere Schlüsseltresore zu sammeln.

Sie können auf Ihre Protokollinformationen (spätestens) zehn Minuten nach dem Schlüsseltresorvorgang zugreifen. In den meisten Fällen geht es aber schneller.  Die Verwaltung der Protokolle im Speicherkonto ist Ihre Aufgabe:

* Verwenden Sie zum Schützen der Protokolle standardmäßige Azure-Zugriffssteuerungsmethoden, indem Sie einschränken, wer darauf zugreifen kann.
* Löschen Sie Protokolle, die im Speicherkonto nicht mehr aufbewahrt werden sollen.

Eine Übersicht über Key Vault finden Sie unter [Was ist Azure Key Vault?](overview.md). Informationen dazu, wo Key Vault verfügbar ist, finden Sie auf der [Seite mit der Preisübersicht](https://azure.microsoft.com/pricing/details/key-vault/). Informationen zur Verwendung von Azure Monitor für Key Vault finden Sie [hier](https://docs.microsoft.com/azure/azure-monitor/insights/key-vault-insights-overview).

## <a name="interpret-your-key-vault-logs"></a>Interpretieren der Key Vault-Protokolle

Einzelne Blobs werden als Text und formatiert als JSON-Blob gespeichert. Schauen wir uns einen Beispielprotokolleintrag an. 

```json
    {
        "records":
        [
            {
                "time": "2016-01-05T01:32:01.2691226Z",
                "resourceId": "/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSOGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT",
                "operationName": "VaultGet",
                "operationVersion": "2015-06-01",
                "category": "AuditEvent",
                "resultType": "Success",
                "resultSignature": "OK",
                "resultDescription": "",
                "durationMs": "78",
                "callerIpAddress": "104.40.82.76",
                "correlationId": "",
                "identity": {"claim":{"http://schemas.microsoft.com/identity/claims/objectidentifier":"d9da5048-2737-4770-bd64-XXXXXXXXXXXX","http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn":"live.com#username@outlook.com","appid":"1950a258-227b-4e31-a9cf-XXXXXXXXXXXX"}},
                "properties": {"clientInfo":"azure-resource-manager/2.0","requestUri":"https://control-prod-wus.vaultcore.azure.net/subscriptions/361da5d4-a47a-4c79-afdd-XXXXXXXXXXXX/resourcegroups/contosoresourcegroup/providers/Microsoft.KeyVault/vaults/contosokeyvault?api-version=2015-06-01","id":"https://contosokeyvault.vault.azure.net/","httpStatusCode":200}
            }
        ]
    }
```

In der folgenden Tabelle sind die Feldnamen und Beschreibungen aufgeführt:

| Feldname | BESCHREIBUNG |
| --- | --- |
| **time** |Datum und Uhrzeit (UTC). |
| **Ressourcen-ID** |Azure Resource Manager-Ressourcen-ID Für Schlüsseltresor-Protokolle ist dies immer die Schlüsseltresor-Ressourcen-ID. |
| **operationName** |Name des Vorgangs, wie in der folgenden Tabelle beschrieben |
| **operationVersion** |Die vom Client angeforderte REST-API-Version. |
| **category** |Der Typ des Ergebnisses. Für Key Vault-Protokolle ist `AuditEvent` der einzige verfügbare Wert. |
| **resultType** |Das Ergebnis der REST-API-Anforderung. |
| **resultSignature** |HTTP-Status |
| **resultDescription** |Zusätzliche Beschreibung zum Ergebnis, falls verfügbar |
| **durationMs** |Verarbeitungsdauer der REST-API-Anforderung in Millisekunden. Die Netzwerklatenz ist hierin nicht enthalten, sodass die auf der Clientseite gemessene Zeit unter Umständen nicht mit diesem Zeitraum übereinstimmt. |
| **callerIpAddress** |Die IP-Adresse des Clients, der die Anforderung gestellt hat. |
| **correlationId** |Optionale GUID, die vom Client zum Korrelieren von clientseitigen Protokollen mit dienstseitigen Protokollen (Schlüsseltresor) übergeben werden kann |
| **Identität** |Identität des Tokens, das in der REST-API-Anforderung angegeben wurde. Dies ist normalerweise ein „Benutzer“, ein „Dienstprinzipal“ oder die Kombination „Benutzer + App-ID“, wie bei einer Anforderung, die auf einem Azure PowerShell-Cmdlet basiert. |
| **properties** |Informationen, die je nach Vorgang (**operationName**) variieren. In den meisten Fällen enthält dieses Feld Clientinformationen (vom Client übergebene Zeichenfolge „useragent“), den genauen REST-API-Anforderungs-URI und den HTTP-Statuscode. Wenn ein Objekt als Ergebnis einer Anforderung (z. B. **KeyCreate** oder **VaultGet**) zurückgegeben wird, enthält es außerdem den Schlüssel-URI (als `id`), den Tresor-URI oder den URI des Geheimnisses. |

Die Feldwerte unter **operationName** liegen im *ObjectVerb*-Format vor. Beispiel:

* Alle Schlüsseltresorvorgänge verfügen über das Format `Vault<action>`, z. B. `VaultGet` und `VaultCreate`.
* Alle Schlüsselvorgänge verfügen über das Format `Key<action>`, z. B. `KeySign` und `KeyList`.
* Alle Geheimnisvorgänge verfügen über das Format `Secret<action>`, z. B. `SecretGet` und `SecretListVersions`.

Die folgende Tabelle enthält die **operationName**-Werte und die entsprechenden REST-API-Befehle:

### <a name="operation-names-table"></a>Tabelle mit Vorgangsnamen

| operationName | REST-API-Befehl |
| --- | --- |
| **Authentifizierung** |Authentifizieren über Azure Active Directory-Endpunkt |
| **VaultGet** |[Abrufen von Informationen über einen Schlüsseltresor](https://msdn.microsoft.com/library/azure/mt620026.aspx) |
| **VaultPut** |[Erstellen oder Aktualisieren eines Schlüsseltresors](https://msdn.microsoft.com/library/azure/mt620025.aspx) |
| **VaultDelete** |[Löschen eines Schlüsseltresors](https://msdn.microsoft.com/library/azure/mt620022.aspx) |
| **VaultPatch** |[Erstellen oder Aktualisieren eines Schlüsseltresors](https://msdn.microsoft.com/library/azure/mt620025.aspx) |
| **VaultList** |[Auflisten aller Schlüsseltresore in einer Ressourcengruppe](https://msdn.microsoft.com/library/azure/mt620027.aspx) |
| **KeyCreate** |[Erstellen eines Schlüssels](https://msdn.microsoft.com/library/azure/dn903634.aspx) |
| **KeyGet** |[Abrufen von Informationen zu einem Schlüssel](https://msdn.microsoft.com/library/azure/dn878080.aspx) |
| **KeyImport** |[Importieren eines Schlüssels in einen Tresor](https://msdn.microsoft.com/library/azure/dn903626.aspx) |
| **KeyBackup** |[Sichern eines Schlüssels](https://msdn.microsoft.com/library/azure/dn878058.aspx) |
| **KeyDelete** |[Löschen eines Schlüssels](https://msdn.microsoft.com/library/azure/dn903611.aspx) |
| **KeyRestore** |[Wiederherstellen eines Schlüssels](https://msdn.microsoft.com/library/azure/dn878106.aspx) |
| **KeySign** |[Signieren mit einem Schlüssel](https://msdn.microsoft.com/library/azure/dn878096.aspx) |
| **KeyVerify** |[Überprüfen mit einem Schlüssel](https://msdn.microsoft.com/library/azure/dn878082.aspx) |
| **KeyWrap** |[Umschließen eines Schlüssels](https://msdn.microsoft.com/library/azure/dn878066.aspx) |
| **KeyUnwrap** |[Aufheben der Umschließung eines Schlüssels](https://msdn.microsoft.com/library/azure/dn878079.aspx) |
| **KeyEncrypt** |[Verschlüsseln mit einem Schlüssel](https://msdn.microsoft.com/library/azure/dn878060.aspx) |
| **KeyDecrypt** |[Mit einem Schlüssel entschlüsseln](https://msdn.microsoft.com/library/azure/dn878097.aspx) |
| **KeyUpdate** |[Aktualisieren eines Schlüssels](https://msdn.microsoft.com/library/azure/dn903616.aspx) |
| **KeyList** |[Auflisten der Schlüssel in einem Tresor](https://msdn.microsoft.com/library/azure/dn903629.aspx) |
| **KeyListVersions** |[Auflisten der Versionen eines Schlüssels](https://msdn.microsoft.com/library/azure/dn986822.aspx) |
| **SecretSet** |[Erstellen eines geheimen Schlüssels](https://msdn.microsoft.com/library/azure/dn903618.aspx) |
| **SecretGet** |[Abrufen eines Geheimnisses](https://msdn.microsoft.com/library/azure/dn903633.aspx) |
| **SecretUpdate** |[Aktualisieren eines Geheimnisses](https://msdn.microsoft.com/library/azure/dn986818.aspx) |
| **SecretDelete** |[Löschen eines Geheimnisses](https://msdn.microsoft.com/library/azure/dn903613.aspx) |
| **SecretList** |[Auflisten von Geheimnissen in einem Tresor](https://msdn.microsoft.com/library/azure/dn903614.aspx) |
| **SecretListVersions** |[Auflisten von Versionen eines Geheimnisses](https://msdn.microsoft.com/library/azure/dn986824.aspx) |
| **VaultAccessPolicyChangedEventGridNotification** | Ereignis „Tresorzugriffsrichtlinie geändert“ veröffentlicht |
| **SecretNearExpiryEventGridNotification** |Ereignis „Geheimnis läuft demnächst ab“ veröffentlicht |
| **SecretExpiredEventGridNotification** |Ereignis „Geheimnis abgelaufen“ veröffentlicht |
| **KeyNearExpiryEventGridNotification** |Ereignis „Schlüssel läuft demnächst ab“ veröffentlicht |
| **KeyExpiredEventGridNotification** |Ereignis „Schlüssel abgelaufen“ veröffentlicht |
| **CertificateNearExpiryEventGridNotification** |Ereignis „Zertifikat läuft demnächst ab“ veröffentlicht |
| **CertificateExpiredEventGridNotification** |Ereignis „Zertifikat abgelaufen“ veröffentlicht |

## <a name="use-azure-monitor-logs"></a><a id="loganalytics"></a>Verwenden von Azure Monitor-Protokollen

Sie können die Key Vault-Lösung in Azure Monitor verwenden, um `AuditEvent`-Protokolle von Key Vault zu überprüfen. In Azure Monitor-Protokollen verwenden Sie Protokollabfragen, um Daten zu analysieren und die benötigten Informationen zu erhalten. 

Weitere Informationen, z. B. zur Einrichtung, finden Sie im Artikel zu [Azure Key Vault in Azure Monitor](../../azure-monitor/insights/key-vault-insights-overview.md).

## <a name="next-steps"></a><a id="next"></a>Nächste Schritte

Ein Tutorial zur Verwendung von Azure Key Vault in einer .NET-Webanwendung finden Sie unter [Verwenden von Azure Key Vault aus einer Webanwendung](tutorial-net-create-vault-azure-web-app.md).

Eine Referenz zur Programmierung finden Sie im [Entwicklerhandbuch für den Azure-Schlüsseltresor](developers-guide.md).

Eine Liste der Azure PowerShell 1.0-Cmdlets für Azure Key Vault finden Sie unter [Azure Key Vault-Cmdlets](/powershell/module/az.keyvault/?view=azps-1.2.0#key_vault).
