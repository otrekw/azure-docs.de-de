---
title: Microsoft Enterprise SSO-Plug-In für Apple-Geräte
titleSuffix: Microsoft identity platform | Azure
description: Enthält eine Beschreibung des Azure Active Directory-SSO-Plug-Ins von Microsoft für iOS- und macOS-Geräte.
services: active-directory
author: brandwe
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 03/31/2020
ms.author: brandwe
ms.reviewer: brandwe
ms.custom: aaddev
ms.openlocfilehash: a6af1a78582ebf62aed82bad8fa06cac37f511ea
ms.sourcegitcommit: 7d8158fcdcc25107dfda98a355bf4ee6343c0f5c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2020
ms.locfileid: "80982581"
---
# <a name="microsoft-enterprise-sso-plug-in-for-apple-devices-preview"></a>Microsoft Enterprise SSO-Plug-In für Apple-Geräte (Vorschau)

> [!NOTE]
> Dieses Feature befindet sich in der Phase der öffentlichen Vorschau.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Das *Microsoft Enterprise SSO-Plug-In für Apple-Geräte* ermöglicht einmaliges Anmelden (Single Sign-On, SSO) für Azure AD-Konten (Azure Active Directory) über alle Anwendungen hinweg, die das Feature [Enterprise Single Sign-On](https://developer.apple.com/documentation/authenticationservices) von Apple unterstützen. Microsoft hat bei der Entwicklung dieses Plug-Ins eng mit Apple zusammengearbeitet, um die Nutzbarkeit Ihrer Anwendung zu erhöhen, während gleichzeitig der bestmögliche Schutz durch Apple und Microsoft gewährt wird.

In diesem Public Preview-Release ist das Enterprise SSO-Plug-In nur für iOS-Geräte verfügbar und in bestimmten Microsoft-Anwendungen enthalten.

Die erste Nutzung des Enterprise SSO-Plug-Ins erfolgt mit unserem neuen Feature [Modus für gemeinsam genutzte Geräte](msal-ios-shared-devices.md).

## <a name="features"></a>Features

Mit dem Microsoft Enterprise SSO-Plug-In für Apple-Geräte ergeben sich für Sie die folgenden Vorteile:

- SSO für Azure AD-Konten über alle Anwendungen hinweg, die das Feature „Enterprise Single Sign-On“ von Apple unterstützen.
- Automatische Bereitstellung in Microsoft Authenticator und Aktivierung mit jeder Lösung für die mobile Geräteverwaltung (Mobile Device Management, MDM).

## <a name="requirements"></a>Anforderungen

Für die Nutzung des Microsoft Enterprise SSO-Plug-Ins für Apple-Geräte gelten die folgenden Anforderungen:

- iOS 13.0 oder höher muss auf dem Gerät installiert sein.
- Eine Microsoft-Anwendung, über die das Microsoft Enterprise SSO-Plug-In für Apple-Geräte bereitgestellt wird, muss auf dem Gerät installiert sein. In der Public Preview-Phase enthalten diese Anwendungen die [Microsoft Authenticator-App](../user-help/user-help-auth-app-overview.md).
- Das Gerät muss über die mobile Geräteverwaltung registriert werden (z. B. mit Microsoft Intune).
- Die Konfiguration muss per Pushvorgang auf das Gerät übertragen werden, um darauf das Microsoft Enterprise SSO-Plug-In für Apple-Geräte zu aktivieren. Diese Sicherheitseinschränkung wird von Apple gefordert.

## <a name="enable-the-sso-extension-with-mobile-device-management-mdm"></a>Aktivieren der SSO-Erweiterung per mobiler Geräteverwaltung (Mobile Device Management, MDM)

An Ihre Geräte muss über einen MDM-Dienst ein Signal gesendet werden, um das Microsoft Enterprise SSO-Plug-In für Apple-Geräte zu aktivieren. Da das Microsoft Enterprise SSO-Plug-In in die [Microsoft Authenticator-App](..//user-help/user-help-auth-app-overview.md) eingebunden wurde, können Sie Ihre MDM-Lösung zum Konfigurieren der App nutzen, um das Microsoft Enterprise SSO-Plug-In zu aktivieren.

Verwenden Sie die folgenden Parameter, um das Microsoft Enterprise SSO-Plug-In für Apple-Geräte zu konfigurieren:

- **Typ:** Umleiten
- **Erweiterungs-ID**: `com.microsoft.azureauthenticator.ssoextension`
- **Team-ID**: `SGGM6D27TK`
- **URLs**:
  - `https://login.microsoftonline.com`
  - `https://login.windows.net`
  - `https://login.microsoft.com`
  - `https://sts.windows.net`
  - `https://login.partner.microsoftonline.cn`
  - `https://login.chinacloudapi.cn`
  - `https://login.microsoftonline.de`
  - `https://login.microsoftonline.us`
  - `https://login.usgovcloudapi.net`
  - `https://login-us.microsoftonline.com`

Sie können Microsoft Intune als Ihren MDM-Dienst verwenden, um die Konfiguration des Microsoft Enterprise SSO-Plug-Ins zu vereinfachen. Weitere Informationen finden Sie in der [Dokumentation zur Intune-Konfiguration](https://docs.microsoft.com/intune/configuration/ios-device-features-settings).

## <a name="using-the-sso-extension-in-your-application"></a>Verwenden der SSO-Erweiterung in Ihrer Anwendung

Version 1.1.0 (und höher) der [Microsoft Authentication Library (MSAL) für Apple-Geräte](https://github.com/AzureAD/microsoft-authentication-library-for-objc) unterstützt das Microsoft Enterprise SSO-Plug-In für Apple-Geräte.

Stellen Sie sicher, dass für Ihre Anwendungen die angegebene Mindestversion der MSAL verwendet wird, wenn Sie den Modus für gemeinsam genutzte Geräte des Microsoft Enterprise SSO-Plug-Ins für Apple-Geräte unterstützen möchten.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Modus für gemeinsam genutzte Geräte unter iOS finden Sie im Artikel [Modus für gemeinsam genutzte Geräte für iOS-Geräte](msal-ios-shared-devices.md).
