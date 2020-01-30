---
title: Leitfaden für die Migration von ADAL zu MSAL für Python | Azure
description: Erfahren Sie, wie Sie Ihre Python-App mit ADAL (Azure Active Directory Authentication Library) zu MSAL (Microsoft Authentication Library) für Python migrieren.
services: active-directory
titleSuffix: Microsoft identity platform
author: rayluo
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.tgt_pltfrm: Python
ms.workload: identity
ms.date: 11/11/2019
ms.author: rayluo
ms.reviewer: rayluo, nacanuma, twhitney
ms.custom: aaddev
ms.openlocfilehash: fe9dc6c04fe033fd518218d1b5ea971e573405fc
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2020
ms.locfileid: "76696556"
---
# <a name="adal-to-msal-migration-guide-for-python"></a>Leitfaden für die Migration von ADAL zu MSAL für Python

In diesem Artikel werden die Änderungen beschrieben, die Sie durchführen müssen, um eine App, die ADAL (Azure Active Directory Authentification Library) verwendet, zu MSAL (Microsoft Authentication Library) zu migrieren.

## <a name="difference-highlights"></a>Wichtige Unterschiede

ADAL arbeitet mit dem Azure Active Directory v1.0-Endpunkt (Azure AD). MSAL (Microsoft Authentication Library) arbeitet mit Microsoft Identity Platform (ehemals bekannt als Azure Active Directory v2.0-Endpunkt). Microsoft Identity Platform weist einige Unterschiede im Vergleich zu Azure AD v1.0 auf:

Unterstützt:
  - Geschäfts-, Schul- oder Unikonten (von Azure AD bereitgestellte Konten)
  - Persönliche Konten (z. B. Outlook.com oder Hotmail.com)
  - Kunden mit eigenen E-Mail-Adressen oder Social Media-Konten (z. B. LinkedIn, Facebook, Google) über das Azure AD B2C-Angebot

- Kompatible Standards:
  - OAuth v2.0
  - OpenID Connect (OIDC)

Weitere Informationen finden Sie unter [Neuerungen des Microsoft Identity Platform (v 2.0)-Endpunkts](https://docs.microsoft.com/azure/active-directory/develop/azure-ad-endpoint-comparison).

### <a name="scopes-not-resources"></a>Geltungsbereiche im Gegensatz zu Ressourcen

ADAL Python ruft Token für Ressourcen ab, während MSAL Python Token für Geltungsbereiche abruft. Die API-Oberfläche in MSAL Python weist keinen Ressourcenparameter mehr auf. Sie müssten Geltungsbereiche in Form einer Liste von Zeichenfolgen bereitstellen, welche die gewünschten Berechtigungen und Ressourcen deklarieren, die angefordert werden. Einige Beispiele für Geltungsbereiche finden Sie unter [Geltungsbereiche von Microsoft Graph](https://docs.microsoft.com/graph/permissions-reference).

### <a name="error-handling"></a>Fehlerbehandlung

ADAL (Azure Active Directory Authentication Library) für Python verwendet die Ausnahme `AdalError`, um anzugeben, dass ein Problem aufgetreten ist. MSAL für Python verwendet dafür in der Regel Fehlercodes. Weitere Informationen finden Sie unter [MSAL für Python – Fehlerbehandlung](https://docs.microsoft.com/azure/active-directory/develop/msal-handling-exceptions?tabs=python).

### <a name="api-changes"></a>API-Änderungen

In der folgenden Tabelle finden Sie eine API in ADAL für Python und eine, die stattdessen in MSAL für Python verwendet wird:

| API – ADAL für Python  | API – MSAL für Python |
| ------------------- | ---------------------------------- |
| [AuthenticationContext](https://adal-python.readthedocs.io/en/latest/#adal.AuthenticationContext)  | [PublicClientApplication or ConfidentialClientApplication](https://msal-python.readthedocs.io/en/latest/#msal.ClientApplication.__init__)  |
| –  | [get_authorization_request_url()](https://msal-python.readthedocs.io/en/latest/#msal.ClientApplication.get_authorization_request_url)  |
| [acquire_token_with_authorization_code()](https://adal-python.readthedocs.io/en/latest/#adal.AuthenticationContext.acquire_token_with_authorization_code) | [acquire_token_by_authorization_code()](https://msal-python.readthedocs.io/en/latest/#msal.ClientApplication.acquire_token_by_authorization_code) |
| [acquire_token()](https://adal-python.readthedocs.io/en/latest/#adal.AuthenticationContext.acquire_token) | [acquire_token_silent()](https://msal-python.readthedocs.io/en/latest/#msal.ClientApplication.acquire_token_silent) |
| [acquire_token_with_refresh_token()](https://adal-python.readthedocs.io/en/latest/#adal.AuthenticationContext.acquire_token_with_refresh_token) | – |
| [acquire_user_code()](https://adal-python.readthedocs.io/en/latest/#adal.AuthenticationContext.acquire_user_code) | [initiate_device_flow()](https://msal-python.readthedocs.io/en/latest/#msal.PublicClientApplication.initiate_device_flow) |
| [acquire_token_with_device_code()](https://adal-python.readthedocs.io/en/latest/#adal.AuthenticationContext.acquire_token_with_device_code) und [cancel_request_to_get_token_with_device_code()](https://adal-python.readthedocs.io/en/latest/#adal.AuthenticationContext.cancel_request_to_get_token_with_device_code) | [acquire_token_by_device_flow()](https://msal-python.readthedocs.io/en/latest/#msal.PublicClientApplication.acquire_token_by_device_flow) |
| [acquire_token_with_username_password()](https://adal-python.readthedocs.io/en/latest/#adal.AuthenticationContext.acquire_token_with_username_password) | [acquire_token_by_username_password()](https://msal-python.readthedocs.io/en/latest/#msal.PublicClientApplication.acquire_token_by_username_password) |
| [acquire_token_with_client_credentials()](https://adal-python.readthedocs.io/en/latest/#adal.AuthenticationContext.acquire_token_with_client_credentials) und [acquire_token_with_client_certificate()](https://adal-python.readthedocs.io/en/latest/#adal.AuthenticationContext.acquire_token_with_client_certificate) | [acquire_token_for_client()](https://msal-python.readthedocs.io/en/latest/#msal.ConfidentialClientApplication.acquire_token_for_client) |
| – | [acquire_token_on_behalf_of()](https://msal-python.readthedocs.io/en/latest/#msal.ConfidentialClientApplication.acquire_token_on_behalf_of) |
| [TokenCache()](https://adal-python.readthedocs.io/en/latest/#adal.TokenCache) | [SerializableTokenCache()](https://msal-python.readthedocs.io/en/latest/#msal.SerializableTokenCache) |
| – | Cache mit Persistenz, verfügbar über [MSAL-Erweiterungen](https://github.com/marstr/original-microsoft-authentication-extensions-for-python) |

## <a name="migrate-existing-refresh-tokens-for-msal-python"></a>Migrieren vorhandener Aktualisierungstoken für MSAL Python

In MSAL (Microsoft Authentication Library) wird das Konzept der Aktualisierungstoken abstrahiert. MSAL Python stellt standardmäßig einen Tokencache im Arbeitsspeicher bereit, sodass Sie keine Aktualisierungstoken speichern, suchen oder aktualisieren müssen. Benutzern werden auch weniger Anmeldeaufforderungen angezeigt, da Aktualisierungstoken in der Regel ohne Benutzereingriff aktualisiert werden können. Weitere Informationen zum Tokencache finden Sie unter [Benutzerdefinierte Tokencacheserialisierung in MSAL für Python](msal-python-token-cache-serialization.md).

Der folgende Code hilft Ihnen beim Migrieren Ihrer von einer anderen OAuth2-Bibliothek (einschließlich, aber nicht beschränkt auf ADAL Python) verwalteten Aktualisierungstoken, damit diese von MSAL für Python verwaltet werden. Ein Grund für die Migration dieser Aktualisierungstoken besteht darin, zu verhindern, dass sich vorhandene Benutzer beim Migrieren Ihrer App zu MSAL für Python erneut anmelden müssen.

Als Methode zum Migrieren eines Aktualisierungstokens wird MSAL für Python verwendet, um mithilfe des vorherigen Aktualisierungstokens ein neues Zugriffstoken abzurufen. Wenn das neue Aktualisierungstoken zurückgegeben wird, speichert MSAL für Python es im Cache. Hier finden Sie ein Beispiel für die Vorgehensweise:

```python
from msal import PublicClientApplication

def get_preexisting_rt_and_their_scopes_from_elsewhere(...):
    raise NotImplementedError("You will need to implement this by yourself")

app = PublicClientApplication(..., token_cache=...)

for old_rt, old_scope in get_preexisting_rt_and_their_scopes_from_elsewhere(...):
    # Assuming the old scope could be a space-delimited string.
    # MSAL expects a list, like ["scope1", "scope2"].
    scopes = old_scope.split()
        # If your old refresh token came from ADAL for Python, which uses a resource rather than a scope,
        # you need to convert your v1 resource into v2 scopes
        # See https://docs.microsoft.com/azure/active-directory/develop/azure-ad-endpoint-comparison#scopes-not-resources
        # You may be able to append "/.default" to your v1 resource to form a scope
        # See https://docs.microsoft.com/azure/active-directory/develop/v2-permissions-and-consent#the-default-scope

    result = app.client.obtain_token_by_refresh_token(old_rt, scope=scopes)
    # When this call returns the new token(s), a new refresh token is issued by the Microsoft identity platform and MSAL for Python
    # stores it in the token cache.
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter [Vergleich von v1.0 und v2.0](active-directory-v2-compare.md).
