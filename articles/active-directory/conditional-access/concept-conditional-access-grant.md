---
title: Gewährungssteuerelemente in Richtlinien für bedingten Zugriff – Azure Active Directory
description: Was sind Gewährungssteuerelemente in einer Richtlinie für bedingten Zugriff in Azure AD?
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 02/11/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: 89063cc8131c28f20153c6fe9b4c71b58794e609
ms.sourcegitcommit: b07964632879a077b10f988aa33fa3907cbaaf0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2020
ms.locfileid: "77192043"
---
# <a name="conditional-access-grant"></a>Bedingter Zugriff: Erteilen

Innerhalb einer Richtlinie für bedingten Zugriff kann ein Administrator Zugriffssteuerungen verwenden, um den Zugriff auf Ressourcen zu gewähren oder zu blockieren.

![Richtlinie für bedingten Zugriff mit einem Gewährungssteuerelement, durch das eine mehrstufige Authentifizierung (Multi-Factor Authentication, MFA) erforderlich ist](./media/concept-conditional-access-grant/conditional-access-grant.png)

## <a name="block-access"></a>Zugriff blockieren

Beim Blockieren des Zugriffs werden alle Zuweisungen berücksichtigt, und der Zugriff wird basierend auf der Konfiguration der Richtlinie für bedingten Zugriff verhindert.

Das Blockieren des Zugriffs ist ein leistungsstarkes Steuerelement, das nur mit entsprechenden Kenntnissen eingesetzt werden sollte. Administratoren sollten den [Modus „Nur Bericht“](concept-conditional-access-report-only.md) verwenden, um die Option vor dem Aktivieren zu testen.

## <a name="grant-access"></a>Gewähren von Zugriff

Administratoren können auswählen, ob beim Gewähren des Zugriffs ein oder mehrere Steuerelemente erzwungen werden sollen. Diese Steuerelemente umfassen die folgenden Optionen: 

- [Mehrstufige Authentifizierung erforderlich (Azure Multi-Factor Authentication)](../authentication/concept-mfa-howitworks.md)
- [Markieren des Geräts als kompatibel erforderlich (Microsoft Intune)](https://docs.microsoft.com/intune/protect/device-compliance-get-started)
- [Gerät mit Hybrid-Azure AD-Einbindung erforderlich](../devices/concept-azure-ad-join-hybrid.md)
- [Genehmigte Client-App erforderlich](app-based-conditional-access.md)
- [App-Schutzrichtlinie erforderlich](app-protection-based-conditional-access.md)

Wenn sich Administratoren dafür entscheiden, diese Optionen zu kombinieren, können sie die folgenden Methoden auswählen:

- Alle ausgewählten Steuerungen anfordern (Steuerelement **UND** Steuerelement)
- Eine der ausgewählten Steuerungen anfordern (Steuerelement **ODER** Steuerelement)

Für den bedingten Zugriff sind standardmäßig alle ausgewählten Steuerelemente erforderlich.

### <a name="require-multi-factor-authentication"></a>Mehrstufige Authentifizierung erforderlich

Wenn Sie dieses Kontrollkästchen aktivieren, müssen Benutzer Azure Multi-Factor Authentication ausführen. Weitere Informationen zum Bereitstellen von Azure Multi-Factor Authentication finden Sie im Artikel [Planen einer cloudbasierten Azure Multi-Factor Authentication-Bereitstellung](../authentication/howto-mfa-getstarted.md).

### <a name="require-device-to-be-marked-as-compliant"></a>Markieren des Geräts als kompatibel erforderlich

Organisationen, die Microsoft Intune bereitstellen, können die von ihren Geräten zurückgegebenen Informationen verwenden, um Geräte zu identifizieren, die bestimmte Kompatibilitätsanforderungen erfüllen. Diese Informationen zur Richtliniencompliance werden von Intune an Azure AD weitergeleitet, sodass der bedingte Zugriff Entscheidungen zum Gewähren oder Blockieren des Zugriffs auf Ressourcen treffen kann. Weitere Informationen zu Konformitätsrichtlinien finden Sie im Artikel [Legen Sie mit Intune Regeln auf Geräten fest, um Zugriff auf Ressourcen in Ihrer Organisation zu gewähren](https://docs.microsoft.com/intune/protect/device-compliance-get-started).

### <a name="require-hybrid-azure-ad-joined-device"></a>Gerät mit Hybrid-Azure AD-Einbindung erforderlich

Organisationen können die Geräteidentität als Teil ihrer Richtlinie für bedingten Zugriff verwenden. Mit diesem Kontrollkästchen können Organisationen festlegen, dass Geräte in Hybrid-Azure AD eingebunden sein müssen. Im Artikel [Was ist eine Geräteidentität?](../devices/overview.md) finden Sie weitere Informationen zu Geräteidentitäten.

### <a name="require-approved-client-app"></a>Genehmigte Client-App erforderlich

Organisationen können festlegen, dass ein versuchter Zugriff auf die ausgewählten Cloud-Apps über eine genehmigte Client-App erfolgen muss.

Diese Einstellung gilt für die folgenden Client-Apps:

- Microsoft Azure Information Protection
- Microsoft Bookings
- Microsoft Cortana
- Microsoft Dynamics 365
- Microsoft Edge
- Microsoft Excel
- Microsoft Flow
- Microsoft Intune Managed Browser
- Microsoft Invoicing
- Microsoft Kaizala
- Microsoft Launcher
- Microsoft OneDrive
- Microsoft OneNote
- Microsoft Outlook
- Microsoft Planner
- Microsoft PowerApps
- Microsoft Power BI
- Microsoft PowerPoint
- Microsoft SharePoint
- Microsoft Skype for Business
- Microsoft StaffHub
- Microsoft Stream
- Microsoft Teams
- Microsoft To-Do
- Microsoft Visio
- Microsoft Word
- Microsoft Yammer

**Anmerkungen**

- Die genehmigten Client-Apps unterstützen das Intune-Feature für die mobile Anwendungsverwaltung.
- Anforderung **Genehmigte Client-App erforderlich**:
   - Unterstützt als Geräteplattformbedingung nur iOS und Android.
- Der bedingte Zugriff kann Microsoft Edge im InPrivate-Modus einer genehmigten Client-App nicht berücksichtigen.

### <a name="require-app-protection-policy"></a>App-Schutzrichtlinie erforderlich

In Ihrer Richtlinie für bedingten Zugriff können Sie verlangen, dass eine App-Schutzrichtlinie für die Client-App vorhanden sein muss, bevor der Zugriff auf die ausgewählten Cloud-Apps verfügbar ist. 

![Steuern des Zugriffs mit einer App-Schutzrichtlinie](./media/technical-reference/22.png)

Diese Einstellung gilt für die folgenden Client-Apps:

- Microsoft Cortana
- Microsoft OneDrive
- Microsoft Outlook
- Microsoft Planner

**Anmerkungen**

- Apps für die App-Schutzrichtlinie unterstützen die mobile Anwendungsverwaltung mit Intune mit Richtlinienschutz.
- Die Voraussetzung **App-Schutzrichtlinie erforderlich**:
    - Unterstützt als Geräteplattformbedingung nur iOS und Android.

## <a name="next-steps"></a>Nächste Schritte

- [Bedingter Zugriff: Sitzungssteuerelemente](concept-conditional-access-session.md)

- [Allgemeine Richtlinien für bedingten Zugriff](concept-conditional-access-policy-common.md)

- [Modus „Nur Bericht“](concept-conditional-access-report-only.md)
