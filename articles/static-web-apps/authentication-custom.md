---
title: Benutzerdefinierte Authentifizierung in Azure Static Web Apps
description: Erfahren Sie, wie Sie benutzerdefinierte Authentifizierung für Azure Static Web Apps konfigurieren.
services: static-web-apps
author: aaronpowell
ms.author: aapowell
ms.service: static-web-apps
ms.topic: conceptual
ms.date: 05/07/2021
ms.openlocfilehash: e4583c6474872cc1de909d86d812aa9ac9630536
ms.sourcegitcommit: 67cdbe905eb67e969d7d0e211d87bc174b9b8dc0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2021
ms.locfileid: "111854574"
---
# <a name="custom-authentication-in-azure-static-web-apps"></a>Benutzerdefinierte Authentifizierung in Azure Static Web Apps

Azure Static Web Apps bietet [verwaltete Authentifizierung](authentication-authorization.md), die von Azure verwaltete Anbieterregistrierungen verwendet. Um mehr Flexibilität bei der Registrierung zu ermöglichen, können Sie die Standardwerte mit einer benutzerdefinierten Registrierung überschreiben.

- Mit der benutzerdefinierten Authentifizierung können Sie auch [benutzerdefinierte Anbieter konfigurieren](#configure-a-custom-openid-connect-provider), die [OpenID Connect](https://openid.net/connect/) unterstützen. Diese Konfiguration ermöglicht die Registrierung mehrerer externer Anbieter.

- Wenn Sie benutzerdefinierte Registrierungen verwenden, werden alle vorkonfigurierten Anbieter deaktiviert.

- Insbesondere bei AAD-Registrierungen (Azure Active Directory) haben Sie die Möglichkeit, einen Mandanten bereitzustellen, mit dem Sie den [Einladungsflow](./authentication-authorization.md#role-management) für die Gruppenverwaltung umgehen können.

> [!NOTE]
> Die benutzerdefinierte Authentifizierung ist nur im Azure Static Web Apps-Standardplan verfügbar.

## <a name="override-pre-configured-provider"></a>Überschreiben vorkonfigurierter Anbieter

Die Einstellungen, die zum Überschreiben eines Anbieters verwendet werden, werden im Abschnitt `auth` der [Konfigurationsdatei](configuration.md) konfiguriert.

Um zu vermeiden, dass Geheimnisse in die Quellcodeverwaltung aufgenommen werden, sucht die Konfiguration in den [Anwendungseinstellungen](application-settings.md) nach einem übereinstimmenden Namen in der Konfigurationsdatei. Sie können ihre Geheimnisse auch in [Azure Key Vault](./key-vault-secrets.md) speichern.

### <a name="configuration"></a>Konfiguration

Die folgenden Tabellen enthalten die verschiedenen Konfigurationsoptionen für jeden Anbieter.

# <a name="azure-active-directory"></a>[Azure Active Directory](#tab/aad)

| Feldpfad                             | BESCHREIBUNG                                                                                                               |
| -------------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| `registration.openIdIssuer`            | Der Endpunkt für die OpenID-Konfiguration des AAD-Mandanten.                                                  |
| `registration.clientIdSettingName`     | Der Name der Anwendungseinstellung, die die Anwendungs-ID (Client) für die Azure AD-App-Registrierung enthält. |
| `registration.clientSecretSettingName` | Der Name der Anwendungseinstellung, die den geheimen Clientschlüssel für die Azure AD-App-Registrierung enthält.           |

```json
{
  "auth": {
    "identityProviders": {
      "azureActiveDirectory": {
        "registration": {
          "openIdIssuer": "https://login.microsoftonline.com/<TENANT_ID>",
          "clientIdSettingName": "<AAD_CLIENT_ID>",
          "clientSecretSettingName": "<AAD_CLIENT_SECRET>"
        }
      }
    }
  }
}
```

Azure Active Directory verfügt über versionierte Endpunkte, die sich darauf auswirken, wie Ihre Registrierung konfiguriert wird. Wenn Sie AAD v1 verwenden (der Ausstellerendpunkt endet nicht auf „/v2.0“), müssen Sie Ihrer Konfiguration im `"azureActiveDirectory"`-Objekt den folgenden `userDetailsClaim`-Eintrag hinzufügen.

```json
"azureActiveDirectory": {
  "registration": { ... },
  "userDetailsClaim": "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name" 
}
```

Weitere Informationen zum Konfigurieren von Azure Active Directory finden Sie in der [Dokumentation zur App Service-Authentifizierung/-Autorisierung](../app-service/configure-authentication-provider-aad.md).

# <a name="apple"></a>[Apple](#tab/apple)

| Feldpfad                             | BESCHREIBUNG                                                                                  |
| -------------------------------------- | -------------------------------------------------------------------------------------------- |
| `registration.clientIdSettingName`     | Der Name der Anwendungseinstellung, die die Client-ID enthält.                                       |
| `registration.clientSecretSettingName` | Der Name der Anwendungseinstellung, die den geheimen Clientschlüssel enthält.                                   |

```json
{
  "auth": {
    "identityProviders": {
      "apple": {
        "registration": {
          "clientIdSettingName": "<APPLE_CLIENT_ID>",
          "clientSecretSettingName": "<APPLE_CLIENT_SECRET>"
        }
      }
    }
  }
}
```

Weitere Informationen zum Konfigurieren von Apple als Authentifizierungsanbieter finden Sie in der [Dokumentation zur App Service-Authentifizierung/-Autorisierung](../app-service/configure-authentication-provider-apple.md).

# <a name="facebook"></a>[Facebook](#tab/facebook)

| Feldpfad                          | BESCHREIBUNG                                                                            |
| ----------------------------------- | -------------------------------------------------------------------------------------- |
| `registration.appIdSettingName`     | Der Name der Anwendungseinstellung, die die App-ID enthält.                             |
| `registration.appSecretSettingName` | Der Name der Anwendungseinstellung, die das App-Geheimnis enthält.                         |

```json
{
  "auth": {
    "identityProviders": {
      "facebook": {
        "registration": {
          "appIdSettingName": "<FACEBOOK_APP_ID>",
          "appSecretSettingName": "<FACEBOOK_APP_SECRET>"
        }
      }
    }
  }
}
```

Weitere Informationen zum Konfigurieren von Facebook als Authentifizierungsanbieter finden Sie in der [Dokumentation zur App Service-Authentifizierung/-Autorisierung.](../app-service/configure-authentication-provider-facebook.md)

# <a name="github"></a>[GitHub](#tab/github)

| Feldpfad                             | BESCHREIBUNG                                                                                  |
| -------------------------------------- | -------------------------------------------------------------------------------------------- |
| `registration.clientIdSettingName`     | Der Name der Anwendungseinstellung, die die Client-ID enthält.                                |
| `registration.clientSecretSettingName` | Der Name der Anwendungseinstellung, die den geheimen Clientschlüssel enthält.                            |

```json
{
  "auth": {
    "identityProviders": {
      "github": {
        "registration": {
          "clientIdSettingName": "<GITHUB_CLIENT_ID>",
          "clientSecretSettingName": "<GITHUB_CLIENT_SECRET>"
        }
      }
    }
  }
}
```

# <a name="google"></a>[Google](#tab/google)

| Feldpfad                             | BESCHREIBUNG                                                                                  |
| -------------------------------------- | -------------------------------------------------------------------------------------------- |
| `registration.clientIdSettingName`     | Der Name der Anwendungseinstellung, die die Client-ID enthält.                                |
| `registration.clientSecretSettingName` | Der Name der Anwendungseinstellung, die den geheimen Clientschlüssel enthält.                            |

```json
{
  "auth": {
    "identityProviders": {
      "google": {
        "registration": {
          "clientIdSettingName": "<GOOGLE_CLIENT_ID>",
          "clientSecretSettingName": "<GOOGLE_CLIENT_SECRET>"
        }
      }
    }
  }
}
```

Weitere Informationen zum Konfigurieren von Google als Authentifizierungsanbieter finden Sie in der [Dokumentation zur App Service-Authentifizierung/-Autorisierung](../app-service/configure-authentication-provider-google.md).

# <a name="twitter"></a>[Twitter](#tab/twitter)

| Feldpfad                               | BESCHREIBUNG                                                                                        |
| ---------------------------------------- | -------------------------------------------------------------------------------------------------- |
| `registration.consumerKeySettingName`    | Der Name der Anwendungseinstellung, die den Consumerschlüssel enthält.                                   |
| `registration.consumerSecretSettingName` | Der Name der Anwendungseinstellung, die das Consumergeheimnis enthält.                                |

```json
{
  "auth": {
    "identityProviders": {
      "twitter": {
        "registration": {
          "consumerKeySettingName": "<TWITTER_CONSUMER_KEY>",
          "consumerSecretSettingName": "<TWITTER_CONSUMER_SECRET>"
        }
      }
    }
  }
}
```

Weitere Informationen zum Konfigurieren von Twitter als Authentifizierungsanbieter finden Sie in der [Dokumentation zur App Service-Authentifizierung/-Autorisierung](../app-service/configure-authentication-provider-twitter.md).

---

## <a name="configure-a-custom-openid-connect-provider"></a>Konfigurieren eines benutzerdefinierten OpenID Connect-Anbieters

In diesem Abschnitt erfahren Sie, wie Sie Azure Static Web Apps für die Verwendung eines benutzerdefinierten Authentifizierungsanbieters konfigurieren, der die [OpenID Connect-Spezifikation (OIDC)](https://openid.net/connect/) einhält. Die folgenden Schritte sind erforderlich, um einen benutzerdefinierten OIDC-Anbieter zu verwenden.

- Ein oder mehrere OIDC-Anbieter sind zulässig.
- Jeder Anbieter muss über einen eindeutigen Namen in der Konfiguration verfügen.
- Nur ein Anbieter kann als Standardumleitungsziel dienen.

### <a name="register-your-application-with-the-identity-provider"></a>Registrieren Ihrer Anwendung beim Identitätsanbieter

Sie müssen die Details Ihrer Anwendung bei einem Identitätsanbieter registrieren. Fragen Sie den Anbieter nach den Schritten, die zum Generieren einer **Client-ID** und eines **geheimen Clientschlüssels** für Ihre Anwendung erforderlich sind.

> [!IMPORTANT]
> Anwendungsgeheimnisse sind vertrauliche Sicherheitsanmeldeinformationen. Teilen Sie dieses Geheimnis mit niemandem, verteilen Sie es in keiner Clientanwendung, und checken Sie es nicht in die Quellcodeverwaltung ein.

Sobald Sie über die Anmeldeinformationen für die Registrierung verfügen, erstellen Sie mithilfe der folgenden Schritte eine benutzerdefinierte Registrierung.

1. Fügen Sie die Client-ID und den geheimen Clientschlüssel als [Anwendungseinstellungen](application-settings.md) für die App hinzu, indem Sie Einstellungsnamen Ihrer Wahl verwenden. Notieren Sie sich diese Namen für die spätere Nutzung. Alternativ kann die Client-ID in der Konfigurationsdatei enthalten sein.

1. Sie benötigen die OpenID Connect-Metadaten für den Anbieter. Diese Informationen werden häufig über ein [Konfigurationsmetadatendokument](https://openid.net/specs/openid-connect-discovery-1_0.html#ProviderConfig) zur Verfügung gestellt, bei dem es sich um die _Aussteller-URL_ des Anbieters mit dem Suffix `/.well-known/openid-configuration` handelt. Erfassen Sie diese Konfigurations-URL.

1. Fügen Sie der [Konfigurationsdatei](configuration.md) einen `auth`-Abschnitt mit einem Konfigurationsblock für die OIDC-Anbieter und Ihre Anbieterdefinition hinzu.

   ```json
   {
     "auth": {
       "identityProviders": {
         "customOpenIdConnectProviders": {
           "myProvider": {
             "registration": {
               "clientIdSettingName": "<MY_PROVIDER_CLIENT_ID_SETTING_NAME>",
               "clientCredential": {
                 "clientSecretSettingName": "<MY_PROVIDER_CLIENT_SECRET_SETTING_NAME>"
               },
               "openIdConnectConfiguration": {
                 "wellKnownOpenIdConfiguration": "https://<PROVIDER_ISSUER_URL>/.well-known/openid-configuration"
               }
             },
             "login": {
               "nameClaimType": "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name",
               "scopes": [],
               "loginParameterNames": []
             }
           }
         }
       }
     }
   }
   ```

  Ändern Sie die folgenden Ersetzungstoken im Code mit Ihren Werten.

  | Ersetzen: | durch: |
  | --- | --- |
  | `<MY_PROVIDER_CLIENT_ID_SETTING_NAME>` | Der Name der Anwendungseinstellung, die der Client-ID zugeordnet ist, die aus Ihrer benutzerdefinierten Registrierung generiert wurde. |
  | `<MY_PROVIDER_CLIENT_SECRET_SETTING_NAME>` | Der Name der Anwendungseinstellung, die dem geheimen Clientschlüssel zugeordnet ist, der aus Ihrer benutzerdefinierten Registrierung generiert wurde. |
  | `<PROVIDER_ISSUER_URL>` | Der Pfad zur _Aussteller-URL_ des Anbieters. |

- Der Anbietername, in diesem Beispiel `myProvider`, ist der eindeutige Bezeichner, der von Azure Static Web Apps verwendet wird.
- Mit dem `login`-Objekt können Sie Werte für benutzerdefinierte Bereiche, Anmeldeparameter oder benutzerdefinierte Ansprüche angeben.

### <a name="login-logout-and-purging-user-details"></a>Anmelden, Abmelden und Bereinigen von Benutzerdetails

Um einen benutzerdefinierten OIDC-Anbieter zu verwenden, verwenden Sie die folgenden URL-Muster.

| Aktion             | Muster                                  |
| ------------------ | ---------------------------------------- |
| Anmelden              | `/.auth/login/<PROVIDER_NAME_IN_CONFIG>` |
| Logout             | `/.auth/logout`                          |
| Bereinigen von Benutzerdetails | `/.auth/purge/<PROVIDER_NAME_IN_CONFIG>` |

### <a name="authentication-callbacks"></a>Authentifizierungsrückrufe

Authentifizierungsanbieter benötigen eine Umleitungs-URL, um die Anmelde- oder Abmeldeanforderung abzuschließen. Die folgenden Endpunkte sind als Umleitungsziele verfügbar.

| Typ   | URL-Muster                                                 |
| ------ | ----------------------------------------------------------- |
| Anmelden  | `https://<YOUR_SITE>/.auth/login/<PROVIDER_NAME_IN_CONFIG>/callback`  |
| Logout | `https://<YOUR_SITE>/.auth/logout/<PROVIDER_NAME_IN_CONFIG>/callback` |

> [!Note]
> Diese URLs werden von Azure Static Web Apps bereitgestellt, um die Antwort vom Authentifizierungsanbieter zu empfangen. Sie müssen keine Seiten auf diesen Routen erstellen.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Schützen von Authentifizierungsgeheimnissen in Azure Key Vault](./key-vault-secrets.md)
