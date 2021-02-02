---
title: Bedingter Zugriff für Benutzer von B2B Collaboration – Azure AD
description: Hier erfahren Sie, wie Sie Richtlinien der mehrstufigen Authentifizierung für Azure Active Directory B2B-Benutzer erzwingen.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 01/21/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: elisolMS
ms.collection: M365-identity-device-management
ms.openlocfilehash: 42b3c3d4d474c61cbe472b4122ac2f80f218bf8d
ms.sourcegitcommit: 95c2cbdd2582fa81d0bfe55edd32778ed31e0fe8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/26/2021
ms.locfileid: "98797261"
---
# <a name="conditional-access-for-b2b-collaboration-users"></a>Bedingter Zugriff für Benutzer von B2B-Zusammenarbeit

In diesem Artikel wird beschrieben, wie Organisationen Richtlinien für bedingten Zugriff für B2B-Gastbenutzer festlegen können, damit diese auf die Ressourcen der Organisation zugreifen können.
>[!NOTE]
>Dieser Authentifizierungs- bzw. Autorisierungsvorgang läuft für Gastbenutzer etwas anders als für die vorhandenen Benutzer dieses Identitätsanbieters (IdP) ab.

## <a name="authentication-flow-for-b2b-guest-users-from-an-external-directory"></a>Authentifizierungsablauf für B2B-Gastbenutzer aus einem externen Verzeichnis

Im folgenden Diagramm ist der Ablauf dargestellt: ![Abbildung: Authentifizierungsablauf für B2B-Gastbenutzer aus einem externen Verzeichnis](./media/conditional-access-b2b/authentication-flow-b2b-guests.png)

| Schritt | Beschreibung |
|--------------|-----------------------|
| 1. | Der B2B-Gastbenutzer fordert Zugriff auf eine Ressource an. Die Ressource leitet den Benutzer an ihren Ressourcenmandanten (vertrauenswürdiger IdP) weiter.|
| 2. | Der Ressourcenmandant identifiziert den Benutzer als „Extern“ und leitet ihn an den IdP des B2B-Gastbenutzers um. Der Benutzer führt die primäre Authentifizierung über den IdP durch.
| 3. | Vom IdP des B2B-Gastbenutzers wird ein Token für den Benutzer ausgestellt. Der Benutzer wird mit dem Token zurück an den Ressourcenmandanten geleitet. Der Ressourcenmandant überprüft das Token und führt dann die Bewertung des Benutzers anhand seiner ZS-Richtlinien durch. Beispielsweise kann der Ressourcenmandant erzwingen, dass der Benutzer die mehrstufige Authentifizierung über Azure Active Directory (AD) durchführen muss.
| 4. | Nachdem alle ZS-Richtlinien des Ressourcenmandanten erfüllt wurden, erstellt dieser ein eigenes Token und leitet den Benutzer an seine Ressource um.

## <a name="authentication-flow-for-b2b-guest-users-with-one-time-passcode"></a>Authentifizierungsablauf für B2B-Gastbenutzer mit Einmalkennung (One-Time Passcode, OTP)

Im folgenden Diagramm ist der Ablauf dargestellt: ![Abbildung: Authentifizierungsablauf für B2B-Gastbenutzer mit Einmalkennung](./media/conditional-access-b2b/authentication-flow-b2b-guests-otp.png)

| Schritt | Beschreibung |
|--------------|-----------------------|
| 1. |Der Benutzer fordert den Zugriff auf eine Ressource auf einem anderen Mandanten an. Die Ressource leitet den Benutzer an ihren Ressourcenmandanten (vertrauenswürdiger IdP) weiter.|
| 2. | Der Ressourcenmandant identifiziert den Benutzer als [externen Benutzer mit Einmalkennung](https://docs.microsoft.com/azure/active-directory/external-identities/one-time-passcode) und sendet eine E-Mail mit der Einmalkennung an den Benutzer.|
| 3. | Der Benutzer ruft die Einmalkennung ab und übermittelt den Code. Der Ressourcenmandant führt die Bewertung des Benutzers anhand seiner ZS-Richtlinien durch.
| 4. | Nachdem alle ZS-Richtlinien erfüllt wurden, erstellt der Ressourcenmandant ein Token und leitet den Benutzer an seine Ressource um. |

>[!NOTE]
>Falls der Benutzer von einem externen Ressourcenmandanten stammt, ist es nicht möglich, dass die ZS-Richtlinien des IdP für den B2B-Gastbenutzer ebenfalls ausgewertet werden. Nach dem aktuellen Stand gelten nur die ZS-Richtlinien des Ressourcenmandanten für seine Gäste.

## <a name="azure-ad-multi-factor-authentication-for-b2b-users"></a>Mehrstufige Azure AD-Authentifizierung für B2B-Benutzer

Organisationen können für ihre B2B-Gastbenutzer mehrere Azure AD-Richtlinien für die mehrstufige Authentifizierung erzwingen. Diese Richtlinien können auf Mandantenebene, App-Ebene oder der Ebene einzelner Benutzer auf die gleiche Weise erzwungen werden, wie sie für Vollzeitmitarbeiter und Mitglieder der Organisation aktiviert werden können.
Der Ressourcenmandant ist immer für die mehrstufige Azure AD-Authentifizierung für Benutzer zuständig. Dies gilt auch, wenn die Organisation des Gastbenutzers über Funktionen für die mehrstufige Authentifizierung verfügt. Beispiel:

1. Ein Administrator oder Information-Worker des Unternehmens „Fabrikam“ lädt Benutzer aus einem anderen Unternehmen mit dem Namen „Contoso“ zur Nutzung der Anwendung „Woodgrove“ ein.

2. Die Woodgrove-App von Fabrikam ist so konfiguriert, dass beim Zugreifen die mehrstufige Azure AD-Authentifizierung erzwungen wird.

3. Wenn der B2B-Gastbenutzer von Contoso aus versucht, auf dem Fabrikam-Mandanten auf Woodgrove zuzugreifen, wird er aufgefordert, die mehrstufige Azure AD-Authentifizierung durchzuführen.

4. Der Gastbenutzer kann seine mehrstufige Azure AD-Authentifizierung für Fabrikam dann einrichten und die Optionen auswählen.

5. Dieses Szenario funktioniert für alle Identitäten: Azure AD oder persönliches Microsoft-Konto (MSA). Ein Beispiel hierfür ist ein Fall, in dem ein Benutzer die Authentifizierung für Contoso mit einer ID aus einem sozialen Netzwerk durchführt.

6. Fabrikam muss über eine ausreichende Zahl von Azure AD-Premium-Lizenzen verfügen, für die die mehrstufige Azure AD-Authentifizierung unterstützt wird. Der Contoso-Benutzer nutzt dann diese Lizenz von Fabrikam. Informationen zur B2B-Lizenzierung finden Sie unter [Abrechnungsmodell für externe Identitäten in Azure AD](https://docs.microsoft.com/azure/active-directory/external-identities/external-identities-pricing).

>[!NOTE]
>Die mehrstufige Azure AD-Authentifizierung wird auf dem Ressourcenmandanten durchgeführt, um die Vorhersagbarkeit sicherzustellen.

### <a name="set-up-azure-ad-multi-factor-authentication-for-b2b-users"></a>Einrichten der mehrstufigen Azure AD-Authentifizierung für B2B-Benutzer

Sehen Sie sich das folgende Video an, um die mehrstufige Azure AD-Authentifizierung für Benutzer von B2B Collaboration zu erhalten:

>[!VIDEO https://channel9.msdn.com/Blogs/Azure/b2b-conditional-access-setup/Player]

### <a name="b2b-users-azure-ad-multi-factor-authentication-for-offer-redemption"></a>B2B-Benutzer: Mehrstufige Azure AD-Authentifizierung für Angebotseinlösung

Weitere Informationen zur mehrstufigen Azure AD-Authentifizierung für die Einlösung erhalten Sie im folgenden Video:

>[!VIDEO https://channel9.msdn.com/Blogs/Azure/MFA-redemption/Player]

### <a name="azure-ad-multi-factor-authentication-reset-for-b2b-users"></a>Mehrstufige Azure AD-Authentifizierung: Zurücksetzung für B2B-Benutzer

Die folgenden PowerShell-Cmdlets für die Nachweiserbringung für B2B-Gastbenutzer sind jetzt verfügbar:

1. Stellen Sie eine Verbindung mit Azure AD her.

   ```
   $cred = Get-Credential
   Connect-MsolService -Credential $cred
   ```
2. Rufen Sie alle Benutzer mit Authentifizierungsmethoden ab.

   ```
   Get-MsolUser | where { $_.StrongAuthenticationMethods} | select UserPrincipalName, @{n="Methods";e={($_.StrongAuthenticationMethods).MethodType}}
   ```
   Beispiel:

   ```
   Get-MsolUser | where { $_.StrongAuthenticationMethods} | select UserPrincipalName, @{n="Methods";e={($_.StrongAuthenticationMethods).MethodType}}
   ```

3. Setzen Sie die Methode für die mehrstufige Azure AD-Authentifizierung für einen bestimmten Benutzer zurück, damit der B2B Collaboration-Benutzer die Nachweismethoden erneut festlegen muss. 
   Beispiel:

   ```
   Reset-MsolStrongAuthenticationMethodByUpn -UserPrincipalName gsamoogle_gmail.com#EXT#@ WoodGroveAzureAD.onmicrosoft.com
   ```

## <a name="conditional-access-for-b2b-users"></a>Bedingter Zugriff für B2B-Benutzer

Es gibt verschiedene Faktoren, die ZS-Richtlinien für B2B-Gastbenutzer beeinflussen.

### <a name="device-based-conditional-access"></a>Gerätebasierter bedingter Zugriff

Für die ZS gibt es eine Option, mit der erzwungen werden kann, dass das [Gerät des Benutzers als konform markiert oder in Azure AD Hybrid eingebunden ist](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-conditions#device-state-preview). B2B-Gastbenutzer können nur dann Konformität erzielen, wenn ihr Gerät vom Ressourcenmandanten verwaltet werden kann. Geräte können nicht von mehr als einer Organisation gleichzeitig verwaltet werden. B2B-Gastbenutzer können die Anforderungen für die Einbindung in Azure AD Hybrid nicht erfüllen, weil sie nicht über ein lokales AD-Konto verfügen. Nur wenn es sich beim Gerät des Gastbenutzers um ein nicht verwaltetes Gerät handelt, kann dieser es beim Ressourcenmandanten registrieren und dann die Konformität dafür herstellen. Anschließend kann der Benutzer die Anforderungen des Gewährungssteuerelements erfüllen.

>[!Note]
>Wir empfehlen Ihnen, für externe Benutzer die Verwendung eines verwalteten Geräts zu erzwingen.

### <a name="mobile-application-management-policies"></a>Verwaltungsrichtlinien für mobile Anwendungen

Für die Nutzung der ZS-Gewährungssteuerelemente, z. B. **Vorschreiben der Verwendung genehmigter Client-Apps** und **App-Schutzrichtlinie erforderlich**, muss das Gerät beim Mandanten registriert sein. Diese Steuerelemente können nur auf [iOS- und Android-Geräte](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-conditions#device-platforms) angewendet werden. Das Anwenden dieser Steuerelemente auf B2B-Gastbenutzer ist aber nicht möglich, wenn das Gerät des Benutzers bereits von einer anderen Organisation verwaltet wird. Ein mobiles Gerät kann nicht gleichzeitig bei mehr als einem Mandanten registriert sein. Wenn das mobile Gerät von einer anderen Organisation verwaltet wird, wird der Benutzer blockiert. Nur wenn das Gerät des Gastbenutzers nicht verwaltet wird, kann dieser das Gerät beim Ressourcenmandanten registrieren. Anschließend kann der Benutzer die Anforderungen des Gewährungssteuerelements erfüllen.  

>[!NOTE]
>Es ist nicht zu empfehlen, für externe Benutzer eine App-Schutzrichtlinie zu erzwingen.

### <a name="location-based-conditional-access"></a>Standortbasierter bedingter Zugriff

Die [standortbasierte Richtlinie](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-conditions#locations) auf Grundlage von IP-Adressbereichen kann erzwungen werden, wenn die einladende Organisation einen vertrauenswürdigen IP-Adressbereich erstellen kann, mit dem ihre Partnerorganisationen definiert werden.

Richtlinien können auch anhand von **geografischen Standorten** erzwungen werden.

### <a name="risk-based-conditional-access"></a>Risikobasierter bedingter Zugriff

Die [Anmelderisiko-Richtlinie](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-conditions#sign-in-risk) wird erzwungen, wenn der B2B-Gastbenutzer die Anforderungen des Gewährungssteuerelements erfüllt. Beispielsweise kann eine Organisation bei einem mittleren oder hohen Anmelderisiko die mehrstufige Azure AD-Authentifizierung erzwingen. Falls sich ein Benutzer aber nicht bereits auf dem Ressourcenmandanten für die mehrstufige Azure AD-Authentifizierung registriert hat, wird er blockiert. Hierdurch soll verhindert werden, dass böswillige Benutzer ihre eigenen Anmeldeinformationen für die mehrstufige Azure AD-Authentifizierung registrieren können, wenn sie in den Besitz des Kennworts eines rechtmäßigen Benutzers gelangen.

Die [Benutzerrisiko-Richtlinie](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-conditions#user-risk) kann auf dem Ressourcenmandanten aber nicht aufgelöst werden. Falls beispielsweise eine Kennwortänderung für Gastbenutzer mit hohem Risiko erzwungen wird, werden diese blockiert, weil Kennwörter im Ressourcenverzeichnis nicht zurückgesetzt werden können.

### <a name="conditional-access-client-apps-condition"></a>Bedingter Zugriff: Client-Apps-Bedingung

[Client-Apps-Bedingungen](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-conditions#client-apps) verhalten sich für B2B-Gastbenutzer genauso wie für andere Arten von Benutzern. Sie können beispielsweise verhindern, dass Gastbenutzer Legacy-Authentifizierungsprotokolle verwenden.

### <a name="conditional-access-session-controls"></a>Bedingter Zugriff: Sitzungssteuerelemente

[Sitzungssteuerelemente](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-session) verhalten sich für B2B-Gastbenutzer genauso wie für andere Arten von Benutzern.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in den folgenden Artikeln zur Azure AD B2B-Zusammenarbeit:

- [Was ist die Azure AD B2B-Zusammenarbeit?](https://docs.microsoft.com/azure/active-directory/external-identities/what-is-b2b)
- [Identity Protection und B2B-Benutzer](https://docs.microsoft.com/azure/active-directory/identity-protection/concept-identity-protection-b2b)
- [Preise für externe Identitäten](https://azure.microsoft.com/pricing/details/active-directory/)
- [Häufig gestellte Fragen (FAQs)](https://docs.microsoft.com/azure/active-directory/external-identities/faq)

