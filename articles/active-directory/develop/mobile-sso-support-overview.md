---
title: Unterstützung von einmaligem Anmelden und App-Schutzrichtlinien in von Ihnen entwickelten mobilen Apps | Azure
titleSuffix: Microsoft identity platform
description: Hier erhalten Sie eine Erläuterung und Übersicht über die Entwickelung mobiler Anwendungen, die einmaliges Anmelden und App-Schutzrichtlinien unterstützen
services: active-directory
author: knicholasa
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 10/14/2020
ms.author: nichola
ms.openlocfilehash: 27a58fc66de31fef272ab2f2c2bfaa9896a07d4b
ms.sourcegitcommit: 957c916118f87ea3d67a60e1d72a30f48bad0db6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2020
ms.locfileid: "92209205"
---
# <a name="support-single-sign-on-and-app-protection-policies-in-mobile-apps-you-develop"></a>Unterstützung von einmaligem Anmelden und App-Schutzrichtlinien in von Ihnen entwickelten mobilen Apps

Einmaliges Anmelden (Single Sign-On, SSO) ermöglicht Benutzern Ihrer App einfache und sichere Anmeldungen. App-Schutzrichtlinien (App Protection Policies, APP) erlauben die Unterstützung der wesentlichen Sicherheitsrichtlinien zum Schutz der Daten Ihrer Benutzer. Diese beiden Features ermöglichen sichere Benutzeranmeldungen und die Verwaltung der Daten Ihrer App.

In diesem Artikel wird erläutert, was SSO und APP so wichtig macht. Außerdem finden Sie hier die allgemeinen Schritte zum Erstellen mobiler Anwendungen, die diese Features unterstützen. Diese Informationen gelten für Smartphone- und Tablet-Apps gleichermaßen. Wenn Sie IT-Administrator sind, der SSO für den Azure Active Directory-Mandanten Ihrer Organisation bereitstellen möchte, lesen Sie unseren [Leitfaden zum Planen einer Bereitstellung von einmaligem Anmelden](../manage-apps/plan-sso-deployment.md).

## <a name="about-single-sign-on-and-app-protection-policies"></a>Informationen zu einmaligem Anmelden und App-Schutzrichtlinien

[Einmaliges Anmelden (Single Sign-On, SSO)](../manage-apps/plan-sso-deployment.md) ermöglicht es einem Benutzer, sich einmal anzumelden und so Zugriff auf andere Anwendungen zu erhalten, ohne die Anmeldeinformationen erneut einzugeben. Dadurch wird der Zugriff auf Apps vereinfacht, und es entfällt die Notwendigkeit, dass Benutzer sich lange Listen von Benutzernamen und Kennwörtern merken müssen. Durch die Implementierung in Ihrer App werden der Zugriff auf Ihre App und deren Verwendung vereinfacht.

Außerdem werden durch Aktivieren von einmaligem Anmelden in Ihrer App neue Authentifizierungsmechanismen unterstützt, die zu einer zeitgemäßen Authentifizierung gehören, z. B. [kennwortlose Anmeldungen](../authentication/concept-authentication-passwordless.md). Benutzernamen und Kennwörter gehören zu den Vektoren, die bei Anwendungen am häufigsten angegriffen werden. Durch Aktivieren von SSO können Sie dieses Risiko verringern, indem Sie den bedingten Zugriff oder kennwortlose Anmeldungen erzwingen, die zusätzliche Sicherheit bieten oder auf sicherere Authentifizierungsmechanismen zurückgreifen. Schließlich ermöglicht das Aktivieren von einmaligem Anmelden auch [einmaliges Abmelden](v2-protocols-oidc.md#single-sign-out). Dies ist z. B. bei Unternehmensanwendungen hilfreich, die auf gemeinsam genutzten Geräten verwendet werden.

[App-Schutzrichtlinien (App Protection Policies, APP)](/mem/intune/apps/app-protection-policy) stellen sicher, dass die Daten einer Organisation geschützt und abgekapselt sind. Sie ermöglichen es Unternehmen, ihre Daten in einer App zu verwalten und zu schützen sowie zu steuern, wer auf die App und die zugehörigen Daten zugreifen kann. Durch Implementieren von App-Schutzrichtlinien können Benutzer mit Ressourcen, die durch Richtlinien für bedingten Zugriff geschützt sind, eine Verbindung herstellen und Daten sicher mit anderen geschützten Apps auszutauschen. Zu den Szenarien, die von App-Schutzrichtlinien unterstützt werden, gehört das Abfragen einer obligatorischen PIN zum Öffnen einer App, das Steuern der Freigabe von Daten für andere Apps und das Verhindern des Speicherns von Daten aus einer Unternehmens-App an einem privaten Speicherort auf dem Gerät.

## <a name="implementing-single-sign-on"></a>Implementieren von einmaligem Anmelden

Damit Ihre App einmaliges Anmelden nutzen kann, wird Folgendes empfohlen:

### <a name="use-microsoft-authentication-library-msal"></a>Verwenden der Microsoft Authentication Library (MSAL)

Am besten implementieren Sie einmaliges Anmelden in Ihrer Anwendung, indem Sie die [Microsoft Authentication Library (MSAL)](msal-overview.md) verwenden. Mithilfe von MSAL können Sie Ihrer App mit wenig Code und API-Aufrufen eine Authentifizierung hinzufügen, bei der Sie von allen Features der [Microsoft Identity Platform](/azure/active-directory/develop/) profitieren. Die Wartung einer sicheren Authentifizierungslösung überlassen Sie dabei Microsoft. Die MSAL fügt standardmäßig Unterstützung von einmaligem Anmelden für Ihre Anwendung hinzu. Außerdem ist die Verwendung von MSAL eine Voraussetzung, um auch App-Schutzrichtlinien implementieren zu können.

> [!NOTE]
> MSAL kann so konfiguriert werden, dass eine eingebettete Webansicht verwendet wird. Dies verhindert einmaliges Anmelden. Verwenden Sie das Standardverhalten (d. h. den Systemwebbrowser), um sicherzustellen, dass einmaliges Anmelden möglich ist.

Wenn Sie derzeit die [ADAL-Bibliothek](../azuread-dev/active-directory-authentication-libraries.md) in Ihrer Anwendung verwenden, wird eine [Migration zu MSAL](msal-migration.md) dringend empfohlen, da [ADAL veraltet ist](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/update-your-applications-to-use-microsoft-authentication-library/ba-p/1257363).

Für iOS-Anwendungen lesen Sie den [Schnellstart](quickstart-v2-ios.md) zur Einrichtung von Anmeldungen mithilfe von MSAL sowie die [Anleitungen zum Konfigurieren der MSAL für verschiedene SSO-Szenarien](single-sign-on-macos-ios.md).

Für Android-Anwendungen lesen Sie den [Schnellstart](quickstart-v2-android.md) zum Einrichten von Anmeldungen mit MSAL sowie Anleitungen zu [Brokerauthentifizierung](brokered-auth.md) und [Autorisierungs-Agents](authorization-agents.md).

### <a name="use-the-system-web-browser"></a>Verwenden des Systemwebbrowsers

Ein Webbrowser ist Voraussetzung für die interaktive Authentifizierung. Für mobile Apps, die andere moderne Authentifizierungsbibliotheken als MSAL verwenden (d. h. andere OpenID Connect- oder SAML-Bibliotheken), oder wenn Sie Ihren eigenen Authentifizierungscode implementieren, sollten Sie den Systembrowser als Authentifizierungsoberfläche verwenden, um einmaliges Anmelden zu unterstützen.

Google bietet hierzu Anleitungen für Android-Anwendungen: [Chrome Custom Tabs – Google Chrome](https://developer.chrome.com/multidevice/android/customtabs) (Benutzerdefinierte Chrome-Registerkarten).

Apple bietet hierzu Anleitungen für iOS-Anwendungen: [Authenticating a User Through a Web Service | Apple Developer Documentation](https://developer.apple.com/documentation/authenticationservices/authenticating_a_user_through_a_web_service) (Authentifizieren eines Benutzers über einen Webdienst).

> [!TIP]
> Das [SSO-Plug-In für Apple-Geräte](apple-sso-plugin.md) ermöglicht SSO für iOS-Apps, die eingebettete Webansichten auf verwalteten Geräten mithilfe von Intune verwenden. Wir empfehlen die Verwendung von MSAL und des Systembrowsers als beste Option zum Entwickeln von Apps, die einmaliges Anmelden für alle Benutzer ermöglichen. Dies ermöglicht einmaliges Anmelden in einigen Szenarien, in denen es auf andere Weise nicht möglich ist.

## <a name="enable-app-protection-policies"></a>Aktivieren von App-Schutzrichtlinien

Zum Aktivieren von App-Schutzrichtlinien verwenden Sie die [Microsoft Authentication Library (MSAL)](msal-overview.md). MSAL ist die Authentifizierungs- und Autorisierungsbibliothek der Microsoft Identity Platform, und das Intune SDK wurde für die Verwendung mit der Microsoft Identity Platform entwickelt.

Außerdem müssen Sie eine Broker-App für die Authentifizierung verwenden. Der Broker erfordert, dass die App Anwendungs- und Geräteinformationen bereitstellt, um die Konformität der App sicherzustellen. iOS-Benutzer verwenden die [Microsoft Authenticator-App](../user-help/user-help-auth-app-sign-in.md) und Android-Benutzer verwenden entweder die Microsoft Authenticator-App oder die [Unternehmensportal-App](https://play.google.com/store/apps/details?id=com.microsoft.windowsintune.companyportal) für die [Brokerauthentifizierung](brokered-auth.md). Standardmäßig greift MSAL zum Erfüllen einer Authentifizierungsanforderung zuerst auf einen Broker zurück. Daher wird die Verwendung des Brokers zum Authentifizieren automatisch für Ihre App aktiviert, wenn die MSAL in der Standardkonfiguration verwendet wird.

Schließlich [fügen Sie Ihrer App das Intune SDK hinzu](/mem/intune/developer/app-sdk-get-started), um App-Schutzrichtlinien zu aktivieren. Das SDK folgt zum größten Teil einem Abfangmodell und wendet automatisch App-Schutzrichtlinien an, um zu bestimmen, ob von der App ausgeführte Aktionen zulässig sind. Es gibt auch APIs, die Sie manuell aufrufen können, um der App Einschränkungen bei bestimmten Aktionen mitzuteilen.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Planen einer Azure Active Directory-Bereitstellung für einmaliges Anmelden](../manage-apps/plan-sso-deployment.md)
- [Vorgehensweise: Konfigurieren von SSO unter macOS und iOS](single-sign-on-macos-ios.md)
- [Microsoft Enterprise SSO-Plug-In für Apple-Geräte (Vorschau)](apple-sso-plugin.md)
- [Brokerauthentifizierung in Android](brokered-auth.md)
- [Autorisierungs-Agents und deren Aktivierung](authorization-agents.md)
- [Erste Schritte mit dem Microsoft Intune App SDK](/mem/intune/developer/app-sdk-get-started)
- [Konfigurieren der Einstellungen für Intune App SDK](/mem/intune/developer/app-sdk-ios#configure-settings-for-the-intune-app-sdk)
- [Durch Microsoft Intune geschützte Apps](/mem/intune/apps/apps-supported-intune-apps)
