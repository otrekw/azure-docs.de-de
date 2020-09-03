---
title: 'Bedingter Zugriff: Anfordern der mehrstufigen Authentifizierung (Multi-Factor Authentication, MFA) für die Azure-Verwaltung – Azure Active Directory'
description: Erstellen einer benutzerdefinierten Richtlinie für bedingten Zugriff zum Anfordern der mehrstufigen Authentifizierung für Azure-Verwaltungsaufgaben
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: how-to
ms.date: 05/26/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: 717e81a1385b04d3152beac39105c56754c55c40
ms.sourcegitcommit: 8a7b82de18d8cba5c2cec078bc921da783a4710e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2020
ms.locfileid: "89049280"
---
# <a name="conditional-access-require-mfa-for-azure-management"></a>Bedingter Zugriff: Vorschreiben der MFA für die Azure-Verwaltung

Organisationen verwenden verschiedenste Azure-Dienste und verwalten diese mit Azure Resource Manager-basierten Tools wie:

* Azure-Portal
* Azure PowerShell
* Azure CLI

Diese Tools bieten Zugriff mit umfassenden Berechtigungen auf Ressourcen, die die abonnementweiten Konfigurationen, Diensteinstellungen und die Abonnementabrechnung ändern können. Um diese Ressourcen mit umfassenden Berechtigungen zu schützen, empfiehlt Microsoft, die mehrstufige Authentifizierung für jeden Benutzer zu erfordern, der auf diese Ressourcen zugreift.

## <a name="user-exclusions"></a>Ausschluss von Benutzern

Richtlinien für bedingten Zugriff sind leistungsstarke Tools, daher wird empfohlen, die folgenden Konten von Ihrer Richtlinie auszuschließen:

* **Notfallzugriffs**- oder **Break-Glass**-Konten, um eine mandantenweite Kontosperrung zu vermeiden. In dem unwahrscheinlichen Fall, dass alle Administratoren aus dem Mandanten ausgeschlossen sind, können Sie sich mit Ihrem Administratorkonto für den Notfallzugriff beim Mandanten anmelden und Maßnahmen ergreifen, um den Zugriff wiederherzustellen.
   * Weitere Informationen finden Sie im Artikel [Verwalten von Konten für den Notfallzugriff in Azure AD](../users-groups-roles/directory-emergency-access.md).
* **Dienstkonten** und **Dienstprinzipale**, z. B. das Konto für die Azure AD Connect-Synchronisierung. Dienstkonten sind nicht interaktive Konten, die an keinen bestimmten Benutzer gebunden sind. Sie werden normalerweise von Back-End-Diensten verwendet, die den programmgesteuerten Zugriff auf Anwendungen ermöglichen, können aber auch zu Verwaltungszwecken für die Anmeldung bei Systemen genutzt werden. Derartige Dienstkonten sollten ausgeschlossen werden, weil die MFA nicht programmgesteuert abgeschlossen werden kann. Aufrufe von Dienstprinzipalen werden durch den bedingten Zugriff nicht blockiert.
   * Wenn Ihre Organisation diese Konten in Skripts oder Code verwendet, sollten Sie in Betracht ziehen, diese durch [verwaltete Identitäten](../managed-identities-azure-resources/overview.md) zu ersetzen. Als vorübergehende Problemumgehung können Sie diese spezifischen Konten aus der Basisrichtlinie ausschließen.

## <a name="create-a-conditional-access-policy"></a>Erstellen der Richtlinie für bedingten Zugriff

Die folgenden Schritte helfen bei der Erstellung einer Richtlinie für bedingten Zugriff, nach der Benutzer mit Zugriff auf die [Microsoft Azure Management](concept-conditional-access-cloud-apps.md#microsoft-azure-management)-App mehrstufige Authentifizierung durchführen müssen.

1. Melden Sie sich beim **Azure-Portal** als globaler Administrator, Sicherheitsadministrator oder Administrator für bedingten Zugriff an.
1. Navigieren Sie zu **Azure Active Directory** > **Sicherheit** > **Bedingter Zugriff**.
1. Wählen Sie **Neue Richtlinie**.
1. Benennen Sie Ihre Richtlinie. Es wird empfohlen, dass Unternehmen einen aussagekräftigen Standard für die Namen ihrer Richtlinien erstellen.
1. Wählen Sie unter **Zuweisungen** die Option **Benutzer und Gruppen** aus.
   1. Wählen Sie unter **Einschließen** die Option **Alle Benutzer** aus.
   1. Wählen Sie unter **Ausschließen** die Option **Benutzer und Gruppen** und dann die Konten für den Notfallzugriff Ihres Unternehmens aus. 
   1. Wählen Sie **Fertig**aus.
1. Wählen Sie unter **Cloud-Apps oder -aktionen** > **Einschließen** die Option **Apps auswählen** aus. Wählen Sie dann **Microsoft Azure-Verwaltung** und anschließend **Auswählen** sowie **Fertig** aus.
1. Lassen Sie unter **Bedingungen** > **Client-Apps (Vorschau)** unter der Option **Wählen Sie die Client-Apps aus, auf die diese Richtlinie angewendet wird.** alle Standardwerte aktiviert, und klicken Sie auf **Fertig**.
1. Wählen Sie unter **Zugriffssteuerung** > **Erteilen** die Option **Zugriff erteilen**, dann **Mehrstufige Authentifizierung erforderlich** und anschließend **Auswählen** aus.
1. Bestätigen Sie die Einstellungen und legen Sie **Richtlinie aktivieren** auf **Ein** fest.
1. Wählen Sie **Erstellen** aus, um die Richtlinie zu erstellen und zu aktivieren.

## <a name="next-steps"></a>Nächste Schritte

[Allgemeine Richtlinien für bedingten Zugriff](concept-conditional-access-policy-common.md)

[Bestimmen der Auswirkung durch Verwendung des reinen Berichtsmodus des bedingten Zugriffs](howto-conditional-access-insights-reporting.md)

[Simulieren des Anmeldeverhaltens mit dem Was-wäre-wenn-Tool für den bedingten Zugriff](troubleshoot-conditional-access-what-if.md)
