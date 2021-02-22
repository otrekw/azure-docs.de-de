---
title: Unterstützen der kennwortlosen Authentifizierung mit FIDO2-Schlüsseln in von Ihnen entwickelten Apps | Azure
titleSuffix: Microsoft identity platform
description: In diesem Bereitstellungsleitfaden wird erläutert, wie Sie die kennwortlose Authentifizierung mit FIDO2-Sicherheitsschlüsseln in den von Ihnen entwickelten Anwendungen unterstützen.
services: active-directory
author: knicholasa
ms.service: active-directory
ms.subservice: develop
ms.topic: reference
ms.workload: identity
ms.date: 1/29/2021
ms.author: nichola
ms.custom: aaddev
ms.openlocfilehash: 5abece0e272d4b72ba6f787ad44b091df5d45226
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2021
ms.locfileid: "100416635"
---
# <a name="support-passwordless-authentication-with-fido2-keys-in-apps-you-develop"></a>Unterstützen der kennwortlosen Authentifizierung mit FIDO2-Schlüsseln in von Ihnen entwickelten Apps

Diese Konfigurationen und bewährten Methoden helfen Ihnen dabei, häufige Szenarien zu vermeiden, in denen verhindert wird, dass die [kennwortlose FIDO2-Authentifizierung](../../active-directory/authentication/concept-authentication-passwordless.md) den Benutzern Ihrer Anwendungen zur Verfügung steht.

## <a name="general-best-practices"></a>Allgemeine bewährte Methoden

### <a name="domain-hints"></a>Domänenhinweise

Verwenden Sie keinen Domänenhinweis, um die [Startbereichsermittlung](../../active-directory/manage-apps/configure-authentication-for-federated-users-portal.md) zu umgehen. Diese Funktion dient dazu, Anmeldungen zu optimieren, aber der Verbundidentitätsanbieter unterstützt möglicherweise keine kennwortlose Authentifizierung.

### <a name="requiring-specific-credentials"></a>Anfordern spezifischer Anmeldeinformationen

Wenn Sie SAML verwenden, geben Sie nicht [mit dem „RequestedAuthnContext“-Element](single-sign-on-saml-protocol.md#requestauthncontext) an, dass ein Kennwort erforderlich ist.

Das „RequestedAuthnContext“-Element ist optional. Daher können Sie es aus Ihren SAML-Authentifizierungsanforderungen entfernen, um dieses Problem zu lösen. Dies ist eine allgemeine bewährte Methode, da die Verwendung dieses Elements auch verhindern kann, dass andere Authentifizierungsoptionen wie die mehrstufige Authentifizierung ordnungsgemäß funktionieren.

### <a name="using-the-most-recently-used-authentication-method"></a>Verwenden der zuletzt verwendeten Authentifizierungsmethode

Die zuletzt von einem Benutzer verwendete Anmeldemethode wird dem Benutzer zuerst angezeigt. Dies kann zu Verwirrung führen, wenn Benutzer glauben, dass sie die zuerst angezeigte Option verwenden müssen. Sie können jedoch eine andere Option auswählen, indem sie die Option „Weitere Anmeldemethoden“ auswählen, wie unten gezeigt.

:::image type="content" source="./media/support-fido2-authentication/most-recently-used-method.png" alt-text="Abbildung der Benutzerauthentifizierungsumgebung, in der die Schaltfläche hervorgehoben ist, über die der Benutzer die Authentifizierungsmethode ändern kann.":::

## <a name="platform-specific-best-practices"></a>Plattformspezifische bewährte Methoden

### <a name="desktop"></a>Desktop

Die empfohlenen Optionen zum Implementieren der Authentifizierung lauten (in der folgenden Reihenfolge) wie folgt:

- .NET-Desktopanwendungen, die die Microsoft Authentication Library (MSAL) verwenden, sollten den Windows-Authentifizierungs-Manager (Windows Authentication Manager, WAM) verwenden. Diese Integration und ihre Vorteile sind [auf GitHub dokumentiert](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/wam).
- Verwenden Sie [WebView2](https://docs.microsoft.com/microsoft-edge/webview2/), um FIDO2 in einem eingebetteten Browser zu unterstützen.
- Verwenden Sie den Systembrowser. Die MSAL-Bibliotheken für Desktopplattformen verwenden diese Methode standardmäßig. Auf unserer Seite der FIDO2-Browserkompatibilität können Sie überprüfen, ob der von Ihnen verwendete Browser die FIDO2-Authentifizierung unterstützt.

### <a name="mobile"></a>Mobile

Ab Februar 2020 wird FIDO2 derzeit nicht für native iOS- oder Android-Apps unterstützt, befindet sich aber in der Entwicklung.

Zur Vorbereitung von Anwendungen auf die Verfügbarkeit und als allgemeine bewährte Methode sollten iOS- und Android-Anwendungen MSAL mit der Standardkonfiguration der Verwendung des Systemwebbrowsers verwenden.

Wenn Sie MSAL nicht nutzen, sollten Sie den Systemwebbrowser trotzdem für die Authentifizierung verwenden. Funktionen wie das einmalige Anmelden und der bedingte Zugriff basieren auf einer vom Systemwebbrowser bereitgestellten gemeinsamen Weboberfläche. Dies bedeutet die Verwendung von [benutzerdefinierten Chrome-Registerkarten](https://developer.chrome.com/docs/multidevice/android/customtabs/) (Android) oder das [Authentifizieren eines Benutzers über einen Webdienst | Apple-Entwicklerdokumentation](https://developer.apple.com/documentation/authenticationservices/authenticating_a_user_through_a_web_service) (iOS).

### <a name="web-and-single-page-apps"></a>Web-Apps und Single-Page-Webanwendungen

Die Verfügbarkeit der kennwortlosen FIDO2-Authentifizierung für Anwendungen, die in einem Webbrowser ausgeführt werden, hängt von der Kombination aus Browser und Plattform ab. Anhand unserer [Matrix der FIDO2-Kompatibilität](../authentication/fido2-compatibility.md) können Sie überprüfen, ob die jeweilige Kombination Ihrer Benutzer unterstützt wird.

## <a name="next-steps"></a>Nächste Schritte

[Optionen für die kennwortlose Authentifizierung für Azure Active Directory](../../active-directory/authentication/concept-authentication-passwordless.md)
