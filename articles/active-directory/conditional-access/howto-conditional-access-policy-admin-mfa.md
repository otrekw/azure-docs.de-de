---
title: 'Bedingter Zugriff: Anfordern der mehrstufigen Authentifizierung (Multi-Factor Authentication, MFA) für Administratoren – Azure Active Directory'
description: Erstellen einer benutzerdefinierten Richtlinie für bedingten Zugriff zum Anfordern der mehrstufigen Authentifizierung für Administratoren
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: how-to
ms.date: 08/03/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5917de03468b86b67520c0b4f04dfd732377a021
ms.sourcegitcommit: 28c5fdc3828316f45f7c20fc4de4b2c05a1c5548
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2020
ms.locfileid: "92366291"
---
# <a name="conditional-access-require-mfa-for-administrators"></a>Bedingter Zugriff: Vorschreiben der MFA für Administratoren

Konten, denen Administratorrechte zugewiesen sind, werden von Angreifern ins Visier genommen. Die Anforderung einer mehrstufigen Authentifizierung (MFA) für diese Konten ist ein einfacher Weg, um das Risiko zu verringern, dass diese Konten gefährdet werden.

Microsoft empfiehlt, dass Sie MFA mindestens für die folgenden Rollen erfordern:

* Authentifizierungsadministrator
* Rechnungsadministrator
* Administrator für den bedingten Zugriff
* Exchange-Administrator
* Globaler Administrator
* Helpdesk-Administrator
* Kennwortadministrator
* Sicherheitsadministrator
* SharePoint-Administrator
* Benutzeradministrator

Unternehmen können Rollen nach eigenem Ermessen ein- oder ausschließen.

## <a name="user-exclusions"></a>Ausschluss von Benutzern

Richtlinien für bedingten Zugriff sind leistungsstarke Tools, daher wird empfohlen, die folgenden Konten von Ihrer Richtlinie auszuschließen:

* **Notfallzugriffs** - oder **Break-Glass** -Konten, um eine mandantenweite Kontosperrung zu vermeiden. In dem unwahrscheinlichen Fall, dass alle Administratoren aus dem Mandanten ausgeschlossen sind, können Sie sich mit Ihrem Administratorkonto für den Notfallzugriff beim Mandanten anmelden und Maßnahmen ergreifen, um den Zugriff wiederherzustellen.
   * Weitere Informationen finden Sie im Artikel [Verwalten von Konten für den Notfallzugriff in Azure AD](../roles/security-emergency-access.md).
* **Dienstkonten** und **Dienstprinzipale** , z. B. das Konto für die Azure AD Connect-Synchronisierung. Dienstkonten sind nicht interaktive Konten, die an keinen bestimmten Benutzer gebunden sind. Sie werden normalerweise von Back-End-Diensten verwendet, die den programmgesteuerten Zugriff auf Anwendungen ermöglichen, können aber auch zu Verwaltungszwecken für die Anmeldung bei Systemen verwendet werden. Derartige Dienstkonten sollten ausgeschlossen werden, weil die MFA nicht programmgesteuert abgeschlossen werden kann. Aufrufe von Dienstprinzipalen werden durch den bedingten Zugriff nicht blockiert.
   * Wenn Ihre Organisation diese Konten in Skripts oder Code verwendet, sollten Sie in Betracht ziehen, diese durch [verwaltete Identitäten](../managed-identities-azure-resources/overview.md) zu ersetzen. Als vorübergehende Problemumgehung können Sie diese spezifischen Konten aus der Basisrichtlinie ausschließen.

## <a name="create-a-conditional-access-policy"></a>Erstellen der Richtlinie für bedingten Zugriff

Die folgenden Schritte helfen bei der Erstellung einer Richtlinie für bedingten Zugriff, nach der die zugewiesenen Administratorrollen eine mehrstufige Authentifizierung durchführen müssen.

1. Melden Sie sich beim **Azure-Portal** als globaler Administrator, Sicherheitsadministrator oder Administrator für bedingten Zugriff an.
1. Navigieren Sie zu **Azure Active Directory** > **Sicherheit** > **Bedingter Zugriff** .
1. Wählen Sie **Neue Richtlinie** .
1. Benennen Sie Ihre Richtlinie. Es wird empfohlen, dass Unternehmen einen aussagekräftigen Standard für die Namen ihrer Richtlinien erstellen.
1. Wählen Sie unter **Zuweisungen** die Option **Benutzer und Gruppen** aus.
   1. Wählen Sie unter **Einschließen** die Option **Verzeichnisrollen (Vorschau)** , und wählen Sie mindestens die folgenden Rollen aus:
      * Authentifizierungsadministrator
      * Rechnungsadministrator
      * Administrator für den bedingten Zugriff
      * Exchange-Administrator
      * Globaler Administrator
      * Helpdesk-Administrator
      * Kennwortadministrator
      * Sicherheitsadministrator
      * SharePoint-Administrator
      * Benutzeradministrator
   
      > [!WARNING]
      > Richtlinien für bedingten Zugriff unterstützen keine Benutzer, die einer [auf eine Verwaltungseinheit beschränkten](../roles/admin-units-assign-roles.md) Verzeichnisrolle zugewiesen sind. Das gleiche gilt für Benutzer, die Verzeichnisrollen zugewiesen sind, die sich direkt auf ein Objekt beziehen (z.B. über [benutzerdefinierte Rollen](../roles/custom-create.md)).

   1. Wählen Sie unter **Ausschließen** die Option **Benutzer und Gruppen** und dann die Konten für den Notfallzugriff Ihres Unternehmens aus. 
   1. Wählen Sie **Fertig** aus.
1. Wählen Sie unter **Cloud-Apps oder -aktionen** > **Einschließen** die Option **Alle Cloud-Apps** und dann **Fertig** aus.
1. Ändern Sie unter **Bedingungen** > **Client-Apps** den Wert von **Konfigurieren** in **Ja** , übernehmen Sie unter **Wählen Sie die Client-Apps aus, auf die diese Richtlinie angewendet wird** alle ausgewählten Standardeinstellungen, und wählen Sie **Fertig** aus.
1. Wählen Sie unter **Zugriffssteuerung** > **Erteilen** die Option **Zugriff erteilen** , dann **Mehrstufige Authentifizierung erforderlich** und anschließend **Auswählen** aus.
1. Bestätigen Sie die Einstellungen und legen Sie **Richtlinie aktivieren** auf **Ein** fest.
1. Wählen Sie **Erstellen** aus, um die Richtlinie zu erstellen und zu aktivieren.

## <a name="next-steps"></a>Nächste Schritte

[Allgemeine Richtlinien für bedingten Zugriff](concept-conditional-access-policy-common.md)

[Bestimmen der Auswirkung durch Verwendung des reinen Berichtsmodus des bedingten Zugriffs](howto-conditional-access-insights-reporting.md)

[Simulieren des Anmeldeverhaltens mit dem Was-wäre-wenn-Tool für den bedingten Zugriff](troubleshoot-conditional-access-what-if.md)
