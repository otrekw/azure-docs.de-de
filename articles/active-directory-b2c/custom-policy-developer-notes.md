---
title: Entwicklerhinweise für Benutzerabläufe und benutzerdefinierte Richtlinien
titleSuffix: Azure AD B2C
description: Hinweise für Entwickler zur Konfiguration und Wartung von Azure AD B2C mit Benutzerströmen und benutzerdefinierten Richtlinien.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 04/30/2021
ms.custom: project-no-code
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: e6261699166e0157750fc691bc0c1726d8cefd50
ms.sourcegitcommit: f6b76df4c22f1c605682418f3f2385131512508d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2021
ms.locfileid: "108324057"
---
# <a name="developer-notes-for-azure-active-directory-b2c"></a>Entwicklerhinweise für Azure Active Directory B2C

Azure Active Directory B2C-[Benutzerströme und benutzerdefinierte Richtlinien](user-flow-overview.md) sind allgemein verfügbar. Die Azure AD B2C-Funktionen befinden sich in ständiger Entwicklung. Obwohl die meisten Funktionen allgemein verfügbar sind, befinden sich einige Funktionen in unterschiedlichen Phasen des Softwarefreigabezyklus. Dieser Artikel beschreibt die kumulativen Verbesserungen in Azure AD B2C und zeigt die Verfügbarkeit der Funktionen auf.

## <a name="terms-for-features-in-public-preview"></a>Nutzungsbedingungen für Features in der öffentlichen Vorschauversion

- Die Nutzung der neuen Funktionen der öffentlichen Vorschauversion sollte ausschließlich zu Bewertungszwecken erfolgen.
- Vereinbarungen zum Servicelevel (Service Level Agreements, SLAs) gelten nicht für die Funktionen der öffentlichen Vorschauversion.
- Supportanfragen für Funktionen der öffentlichen Vorschauversion können über die üblichen Unterstützungskanäle gesendet werden.

## <a name="user-flows"></a>Benutzerflows

|Funktion  |Benutzerflow  |Benutzerdefinierte Richtlinie  |Notizen  |
|---------|:---------:|:---------:|---------|
| [Registrieren und melden Sie sich mit E-Mail und Kennwort an](add-sign-up-and-sign-in-policy.md). | Allgemein verfügbar | Allgemein verfügbar| |
| [Registrieren und melden Sie sich mit Benutzernamen und Kennwort](add-sign-up-and-sign-in-policy.md).| Allgemein verfügbar | Allgemein verfügbar | |
| [Profilbearbeitungsflow](add-profile-editing-policy.md) | Allgemein verfügbar | Allgemein verfügbar | |
| [Self-Service-Kennwortzurücksetzung](add-password-reset-policy.md) | Allgemein verfügbar| Allgemein verfügbar| |
| [Kennwortzurücksetzung erzwingen](force-password-reset.md) | Vorschau | Nicht verfügbar | |
| [Registrieren und melden Sie sich per Telefon an](phone-authentication-user-flows.md) | Allgemein verfügbar | Allgemein verfügbar | |

## <a name="user-experience-customization"></a>Anpassung der Benutzererfahrung

|Funktion  |Benutzerflow  |Benutzerdefinierte Richtlinie  |Notizen  |
|---------|:---------:|:---------:|---------|
| [Unterstützung für mehrere Sprachen](localization.md)| Allgemein verfügbar | Allgemein verfügbar | |
| [Benutzerdefinierte E-Mail-Überprüfung](custom-email-mailjet.md) | Nicht verfügbar | Allgemein verfügbar| |
| [Anpassen der Benutzeroberfläche mit integrierten Vorlagen](customize-ui.md) | Allgemein verfügbar| Allgemein verfügbar| |
| [Anpassen der Benutzeroberfläche mit benutzerdefinierten Vorlagen](customize-ui-with-html.md) | Allgemein verfügbar| Allgemein verfügbar| Anhand von HTML-Vorlagen. |
| [JavaScript](javascript-and-page-layout.md) | Allgemein verfügbar | Allgemein verfügbar | |
| [Eingebettetes Anmeldeverfahren](embedded-login.md) | Nicht verfügbar |  Vorschau| Anhand des Inlineframe-Elements`<iframe>`. |
| [Kennwortkomplexität](password-complexity.md) | Allgemein verfügbar | Allgemein verfügbar | |
| [Deaktivieren der E-Mail-Überprüfung](disable-email-verification.md) | Allgemein verfügbar|  Allgemein verfügbar| Nicht für Produktionsumgebungen empfohlen. Die Deaktivierung der E-Mail-Überprüfung während des Registrierungsvorgangs kann zu Spam führen. |


## <a name="protocols-and-authorization-flows"></a>Protokolle und Autorisierungsabläufe

|Funktion  |Benutzerflow  |Benutzerdefinierte Richtlinie  |Notizen  |
|---------|:---------:|:---------:|---------|
|[OAuth2-Autorisierungscode](authorization-code-flow.md) | Allgemein verfügbar | Allgemein verfügbar |
|[OAuth2-Autorisierungscode mit PKCE](authorization-code-flow.md)| Allgemein verfügbar | Allgemein verfügbar | Öffentliche Clients und Einzelblatt-Webanwendungen. |
|[Impliziter OAuth2-Fluss](implicit-flow-single-page-application.md) | Allgemein verfügbar | Allgemein verfügbar | |
|[Kennwortanmeldeinformationen von Ressourcenbesitzern mittels OAuth2](add-ropc-policy.md) | Vorschau | Vorschau | |
|OAuth1 | Nicht verfügbar | Nicht verfügbar | Wird nicht unterstützt. |
|[OpenID Connect](openid-connect.md) | Allgemein verfügbar | Allgemein verfügbar | |
|[SAML2](saml-service-provider.md) | Nicht verfügbar | Allgemein verfügbar | POST- und Umleitungsbindungen |
| WSFED | Nicht verfügbar | Nicht verfügbar | Wird nicht unterstützt. |

## <a name="identity-providers"></a>Identitätsanbieter

|Funktion  |Benutzerflow  |Benutzerdefinierte Richtlinie  |Notizen  |
|---------|:---------:|:---------:|---------|
|[AD FS](identity-provider-adfs.md) | Nicht verfügbar | Allgemein verfügbar | |
|[Amazon](identity-provider-amazon.md) | Allgemein verfügbar | Allgemein verfügbar | |
|[Apple](identity-provider-apple-id.md) | Vorschau | Vorschau | |
|[Azure AD (ein Mandant)](identity-provider-azure-ad-single-tenant.md) | Allgemein verfügbar | Allgemein verfügbar | |
|[Azure AD (mehrere Mandanten)](identity-provider-azure-ad-multi-tenant.md) | Nicht verfügbar  | Allgemein verfügbar | |
|[Azure AD B2C](identity-provider-azure-ad-b2c.md) | Allgemein verfügbar | Allgemein verfügbar | |
|[eBay](identity-provider-ebay.md) | Nicht verfügbar | Vorschau | |
|[Facebook](identity-provider-facebook.md) | Allgemein verfügbar | Allgemein verfügbar | |
|[GitHub](identity-provider-github.md) | Allgemein verfügbar | Allgemein verfügbar | |
|[Google](identity-provider-google.md) | Allgemein verfügbar | Allgemein verfügbar | |
|[ID.me](identity-provider-id-me.md) | Allgemein verfügbar | Allgemein verfügbar | |
|[LinkedIn](identity-provider-linkedin.md) | Allgemein verfügbar | Allgemein verfügbar | |
|[Microsoft-Konto](identity-provider-microsoft-account.md) | Allgemein verfügbar | Allgemein verfügbar | |
|[QQ](identity-provider-qq.md) | Vorschau | Allgemein verfügbar | |
|[Salesforce](identity-provider-salesforce.md) | Allgemein verfügbar | Allgemein verfügbar | |
|[Salesforce (SAML-Protokoll)](identity-provider-salesforce-saml.md) | Nicht verfügbar | Allgemein verfügbar | |
|[Twitter](identity-provider-twitter.md) | Allgemein verfügbar | Allgemein verfügbar | |
|[WeChat](identity-provider-wechat.md) | Vorschau | Allgemein verfügbar | |
|[Weibo](identity-provider-weibo.md) | Vorschau | Allgemein verfügbar | |

## <a name="generic-identity-providers"></a>Generischer Identitätsanbieter

|Funktion  |Benutzerflow  |Benutzerdefinierte Richtlinie  |Notizen  |
|---------|:---------:|:---------:|---------|
|[OAuth2](oauth2-technical-profile.md) | Nicht verfügbar | Allgemein verfügbar | Beispiel: [Google](identity-provider-google.md), [GitHub](identity-provider-github.md), und [Facebook.](identity-provider-facebook.md)|
|[OAuth1](oauth1-technical-profile.md) | Nicht verfügbar | Allgemein verfügbar | Beispiel: [Twitter](identity-provider-twitter.md). |
|[OpenID Connect](openid-connect-technical-profile.md) | Allgemein verfügbar | Allgemein verfügbar | Zum Beispiel, [Azure AD](identity-provider-azure-ad-single-tenant.md).  |
|[SAML2](identity-provider-generic-saml.md) | Nicht verfügbar | Allgemein verfügbar | Zum Beispiel, [Salesforce](identity-provider-salesforce-saml.md) und[AD-FS].(identity-provider-adfs.md) |
| WSFED | Nicht verfügbar | Nicht verfügbar | |

### <a name="api-connectors"></a>API-Connectors

|Funktion  |Benutzerflow  |Benutzerdefinierte Richtlinie  |Notizen  |
|---------|:---------:|:---------:|---------|
|[API-Connectors](api-connectors-overview.md) | Vorschau | Allgemein verfügbar | |
|[Sicher mit der Basisauthentifizierung](secure-rest-api.md#http-basic-authentication) | Vorschau | Allgemein verfügbar | |
|[Sicherheit mit der Client-Zertifikat-Authentifizierung](secure-rest-api.md#https-client-certificate-authentication) | Vorschau | Allgemein verfügbar | |
|[Mit der OAuth2-Trägerauthentifizierung schützen](secure-rest-api.md#oauth2-bearer-authentication) | Nicht verfügbar | Allgemein verfügbar | |
|[Sichere API-Schlüssel-Authentifizierung](secure-rest-api.md#api-key-authentication) | Nicht verfügbar | Allgemein verfügbar | |

### <a name="application-and-azure-ad-b2c-integration"></a>Anwendungs- und Azure AD B2C-Integration

|Funktion  |Benutzerflow  |Benutzerdefinierte Richtlinie  |Notizen  |
|---------|:---------:|:---------:|---------|
| [Umleiten einer Anmeldung zu einem Anbieter sozialer Netzwerke](direct-signin.md#redirect-sign-in-to-a-social-provider) | Allgemein verfügbar | Allgemein verfügbar | Abfrage-Zeichenfolgenparameter`domain_hint`. |
| [Auffüllen des Anmeldenamens](direct-signin.md#prepopulate-the-sign-in-name) | Allgemein verfügbar | Allgemein verfügbar | Abfrage-Zeichenfolgenparameter`login_hint`. |
| Einfügen von JSON in User Journey über `client_assertion`| Nicht verfügbar| Als veraltet markiert |  |
| Einfügen von JSON in UserJourney als [id_token_hint](id-token-hint.md) | Nicht verfügbar | Allgemein verfügbar | |
| [Übergeben des Tokens eines Identitätsanbieters an die Anwendung](idp-pass-through-user-flow.md)| Vorschau| Vorschau| z.B. von Facebook in eine App |


## <a name="custom-policy-features"></a>Benutzerdefinierte Richtlinienfunktionen

### <a name="session-management"></a>Sitzungsverwaltung

| Funktion |  Benutzerdefinierte Richtlinie | Notizen |
| ------- | :--: | ----- |
| [SSO-Standardsitzungsanbieter](custom-policy-reference-sso.md#defaultssosessionprovider) | Allgemein verfügbar |  |
| [Sitzungsanbieter mit externer Anmeldung](custom-policy-reference-sso.md#externalloginssosessionprovider) | Allgemein verfügbar |  |
| [SAML SSO-Sitzungsanbieter](custom-policy-reference-sso.md#samlssosessionprovider) | Allgemein verfügbar |  |
| [OAuth SSO Session-Anbieter](custom-policy-reference-sso.md#oauthssosessionprovider)  | Allgemein verfügbar|  |
| [Einmaliges Abmelden](session-behavior.md#sign-out)  |  Vorschau |  |

### <a name="components"></a>Komponenten

| Funktion | Benutzerdefinierte Richtlinie | Notizen |
| ------- | :--: | ----- |
| [PhoneFactor-Authentifizierung](phone-factor-technical-profile.md) | Allgemein verfügbar |  |
| [Azure AD MFA-Authentifizierung](multi-factor-auth-technical-profile.md) | Vorschau |  |
| [Einmalkennwort](one-time-password-technical-profile.md) | Allgemein verfügbar |  |
| [Azure Active Directory](active-directory-technical-profile.md) als lokales Verzeichnis | Allgemein verfügbar |  |
| [Prädikatüberprüfungen](predicates.md) | Allgemein verfügbar | z.B. Kennwortkomplexität |
| [Anzeigesteuerelemente](display-controls.md) | Allgemein verfügbar |  |

### <a name="developer-interface"></a>Entwicklerschnittstelle

| Funktion | Benutzerdefinierte Richtlinie | Notizen |
| ------- | :--: | ----- |
| Azure-Portal | Allgemein verfügbar |   |
| [Application Insights, User Journey-Protokolle](troubleshoot-with-application-insights.md) | Vorschau | für die Problembehandlung bei der Entwicklung  |
| [Application Insights, Ereignisprotokolle](analytics-with-application-insights.md) | Vorschau | für die Überwachung von Benutzerabläufen in der Produktion |

## <a name="responsibilities-of-custom-policy-feature-set-developers"></a>Zuständigkeiten für Entwickler von Featuregruppen für benutzerdefinierte Richtlinien

Die manuelle Richtlinienkonfiguration gewährt eingehenderen Zugriff auf die zugrunde liegende Plattform von Azure AD B2C und führt zur Erstellung eines eindeutigen Vertrauensframeworks. Für die möglichen Permutationen von benutzerdefinierten Identitätsanbietern, Vertrauensstellungen, Integrationen in externe Dienste und ausführlichen Workflows ist ein methodischer Ansatz beim Entwurf und bei der Konfiguration nötig.

Entwickler, die das Feature für benutzerdefinierte Richtlinien verwenden, sollten folgende Leitlinien einhalten:

- Machen Sie sich mit der Konfigurationssprache der benutzerdefinierten Richtlinien und der Verwaltung von Schlüsseln und Geheimnissen vertraut. Weitere Informationen finden Sie unter [TrustFrameworkPolicy](trustframeworkpolicy.md).
- Übernehmen Sie den Besitz für Szenarien und benutzerdefinierte Integrationen. Dokumentieren Sie Ihre Arbeit, und halten Sie das Organisationsteam für Ihre Livewebsite auf dem Laufenden.
- Führen Sie methodische Szenariotests durch.
- Berücksichtigen Sie bewährte Methoden für die Softwareentwicklung und das Staging. Die Verwendung von mindestens einer Entwicklungs- und Testumgebung wird empfohlen.
- Informieren Sie sich regelmäßig über neue Entwicklungen bei den von Ihnen integrierten Identitätsanbietern und -diensten. Achten Sie beispielsweise auf Änderungen an Geheimnissen sowie auf geplante und ungeplante Änderungen am Dienst.
- Richten Sie eine aktive Überwachung ein, und überwachen Sie die Reaktionsfähigkeit von Produktionsumgebungen. Weitere Informationen zur Integration mit Application Insights finden Sie unter [Nachverfolgen des Benutzerverhaltens in Azure Active Directory B2C mithilfe von Application Insights](analytics-with-application-insights.md).
- Halten Sie die E-Mail-Adressen der Kontaktpersonen im Azure-Abonnement aktuell, und sorgen Sie dafür, dass auf die E-Mails des Microsoft-Livewebsite-Teams schnell reagiert wird.
- Ergreifen Sie ohne Verzögerung Maßnahmen, wenn Sie vom Microsoft-Livewebsite-Team dazu aufgefordert werden.

## <a name="next-steps"></a>Nächste Schritte

- Sehen Sie sich die [Microsoft Graph-Vorgänge an, die für Azure AD B2C verfügbar sind](microsoft-graph-operations.md).
- Weitere Informationen finden Sie unter [Benutzerdefinierte Richtlinien in Azure Active Directory B2C](custom-policy-overview.md).
