---
title: Abrufen von Dienstzugriffstoken
description: Informationen zum Erstellen von Token für den Zugriff auf REST-APIs von ARR
author: florianborn71
ms.author: flborn
ms.date: 02/11/2020
ms.topic: how-to
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 9721685fc3ccd2c1c80b55e9118d6d347cc97a9c
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "97830699"
---
# <a name="get-service-access-tokens"></a>Abrufen von Dienstzugriffstoken

Der Zugriff auf die REST-APIs von ARR wird nur autorisierten Benutzern gewährt. Um Ihre Autorisierung nachzuweisen, müssen Sie zusammen mit den REST-Anforderungen ein *Zugriffstoken* senden. Diese Token werden vom *Sicherheitstokendienst (Secure Token Service, STS)* im Austausch gegen einen Kontoschlüssel ausgestellt. Token haben eine **Gültigkeitsdauer von 24 Stunden** und können daher an Benutzer ausgegeben werden, ohne ihnen Vollzugriff auf den Dienst zu gewähren.

In diesem Artikel wird das Erstellen solcher Zugriffstoken beschrieben.

## <a name="prerequisites"></a>Voraussetzungen

[Erstellen Sie ein ARR-Konto](create-an-account.md), wenn Sie noch keines haben.

## <a name="token-service-rest-api"></a>REST-API des Tokendiensts

Zum Erstellen von Zugriffstoken stellt der *Sicherheitstokendienst (STS)* eine einzelne Rest-API bereit. Die URL für den STS-Dienst hängt von der Kontodomäne des Kontos für das Rendering per Remotezugriff ab. Sie hat die Form https://sts.[Kontodomäne], z. B. `https://sts.southcentralus.mixedreality.azure.com`

### <a name="get-token-request"></a>Anforderung „Token abrufen“

| URI | Methode |
|-----------|:-----------|
| /accounts/**accountId**/token | GET |

| Header | Wert |
|--------|:------|
| Authorization | „Bearer **accountId**:**accountKey**“ |

Ersetzen Sie *accountId* und *accountKey* durch die entsprechenden Daten.

### <a name="get-token-response"></a>Antwort auf „Token abrufen“

| Statuscode | JSON-Nutzlast | Kommentare |
|-----------|:-----------|:-----------|
| 200 | AccessToken: Zeichenfolge | Erfolg |

| Header | Zweck |
|--------|:------|
| MS-CV | Dieser Wert kann verwendet werden, um den Aufruf innerhalb des Diensts nachzuverfolgen. |

## <a name="getting-a-token-using-powershell"></a>Abrufen eines Tokens mit PowerShell

Der nachstehende PowerShell-Code veranschaulicht, wie die erforderliche REST-Anforderung an den STS gesendet wird. Anschließend wird das Token an der PowerShell-Eingabeaufforderung ausgegeben.

```PowerShell
$accountId = "<account_id_from_portal>"
$accountKey = "<account_key_from_portal>"
$accountDomain = "<account_domain_from_portal>

[System.Net.ServicePointManager]::SecurityProtocol = [System.Net.SecurityProtocolType]::Tls12;
$webResponse = Invoke-WebRequest -Uri "https://sts.$accountDomain/accounts/$accountId/token" -Method Get -Headers @{ Authorization = "Bearer ${accountId}:$accountKey" }
$response = ConvertFrom-Json -InputObject $webResponse.Content

Write-Output "Token: $($response.AccessToken)"
```

Das Skript gibt lediglich das Token an die Ausgabe aus, von wo aus Sie es kopieren und einfügen können. Bei einem echten Projekt müssen Sie diesen Prozess automatisieren.

## <a name="next-steps"></a>Nächste Schritte

* [PowerShell-Beispielskripts](../samples/powershell-example-scripts.md)
* [Azure-Front-End-APIs](../how-tos/frontend-apis.md)
* [REST-API für die Sitzungsverwaltung](../how-tos/session-rest-api.md)
