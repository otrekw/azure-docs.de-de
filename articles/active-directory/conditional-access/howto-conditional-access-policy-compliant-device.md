---
title: 'Bedingter Zugriff: Anfordern von kompatiblen Geräten – Azure Active Directory'
description: Erstellen einer benutzerdefinierten Richtlinie für bedingten Zugriff, um konforme Geräte zu anzufordern.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 12/12/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8a18f0b1f83ae02b06344c332cfdd1cc093f37fc
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75424890"
---
# <a name="conditional-access-require-compliant-devices"></a>Bedingter Zugriff: Anfordern von kompatiblen Geräten

Unternehmen, die Microsoft Intune bereitgestellt haben, können die von ihren Geräten zurückgegebenen Informationen verwenden, um Geräte zu identifizieren, die den Compliancevorgaben entsprechen, z. B.:

* Erfordern einer PIN zum Entsperren
* Erfordern der Geräteverschlüsselung
* Erfordern einer minimalen oder maximalen Betriebssystemversion
* Erfordern, dass ein Gerät keinen Jailbreak oder Rootzugriff verwendet

Diese Informationen zur Richtliniencompliance werden an Azure AD weitergeleitet, wo Entscheidungen zum bedingten Zugriff zur Gewährung oder Sperrung des Zugriffs auf Ressourcen treffen kann.

## <a name="create-a-conditional-access-policy"></a>Erstellen der Richtlinie für bedingten Zugriff

Die folgenden Schritte helfen bei der Erstellung einer Richtlinie für bedingten Zugriff, bei der Geräte, die auf Ressourcen zugreifen, als mit den Compliancerichtlinien von Intune Ihres Unternehmens konform gekennzeichnet werden.

1. Melden Sie sich beim **Azure-Portal** als globaler Administrator, Sicherheitsadministrator oder Administrator für bedingten Zugriff an.
1. Navigieren Sie zu **Azure Active Directory** > **Sicherheit** > **Bedingter Zugriff**.
1. Wählen Sie **Neue Richtlinie**.
1. Benennen Sie Ihre Richtlinie. Es wird empfohlen, dass Unternehmen einen aussagekräftigen Standard für die Namen ihrer Richtlinien erstellen.
1. Wählen Sie unter **Zuweisungen** die Option **Benutzer und Gruppen** aus.
   1. Wählen Sie unter **Einschließen** die Option **Alle Benutzer** aus.
   1. Wählen Sie unter **Ausschließen** die Option **Benutzer und Gruppen** und dann die Konten für den Notfallzugriff Ihres Unternehmens aus. 
   1. Wählen Sie **Fertig**aus.
1. Wählen Sie unter **Cloud-Apps oder -aktionen** > **Einschließen** die Option **Alle Cloud-Apps** aus.
   1. Wenn Sie bestimmte Anwendungen von Ihrer Richtlinie ausschließen müssen, können Sie sie auf der Registerkarte **Ausschließen** unter **Ausgeschlossene Cloudanwendungen auswählen** die Option **Auswählen** auswählen.
   1. Wählen Sie **Fertig**aus.
1. Wählen Sie unter **Zugriffssteuerung** > **Erteilen** die Option **Markieren des Geräts als kompatibel erforderlich** aus.
   1. Wählen Sie **Auswählen**.
1. Bestätigen Sie die Einstellungen und legen Sie **Richtlinie aktivieren** auf **Ein** fest.
1. Wählen Sie **Erstellen** aus, um die Richtlinie zu erstellen und zu aktivieren.

### <a name="known-behavior"></a>Bekanntes Verhalten

Unter Windows 7, iOS, Android, macOS und einigen Webbrowsern von Drittanbietern identifiziert Azure AD das Gerät anhand eines Clientzertifikats, das beim Registrieren des Geräts bei Azure AD bereitgestellt wird. Wenn sich ein Benutzer zum ersten Mal über den Browser anmeldet, wird er zum Auswählen des Zertifikats aufgefordert. Der Endbenutzer muss dieses Zertifikat auswählen, bevor der Browser verwendet werden kann.

## <a name="next-steps"></a>Nächste Schritte

[Allgemeine Richtlinien für bedingten Zugriff](concept-conditional-access-policy-common.md)

[Bestimmen der Auswirkung durch Verwendung des reinen Berichtsmodus des bedingten Zugriffs](howto-conditional-access-report-only.md)

[Simulieren des Anmeldeverhaltens mit dem Was-wäre-wenn-Tool für den bedingten Zugriff](troubleshoot-conditional-access-what-if.md)

[Richtlinien für Gerätekonformität funktionieren mit Azure AD](https://docs.microsoft.com/intune/device-compliance-get-started#device-compliance-policies-work-with-azure-ad)
