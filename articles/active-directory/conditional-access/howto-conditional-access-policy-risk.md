---
title: Bedingter Zugriff anhand des Anmelderisikos – Azure Active Directory
description: Erstellen von Richtlinien für bedingten Zugriff mit der Azure AD Identity Protection-Anmelderisikoerkennung
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: how-to
ms.date: 07/02/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0c6e3316afc09992d16e17d9d2e2afe6b92dcd22
ms.sourcegitcommit: 8a7b82de18d8cba5c2cec078bc921da783a4710e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2020
ms.locfileid: "89049042"
---
# <a name="conditional-access-sign-in-risk-based-conditional-access"></a>Bedingter Zugriff: Risikobasierter bedingter Zugriff beim Anmelden

Die meisten Benutzer weisen ein normales Verhalten auf, das nachverfolgt werden kann. Wenn sie sich aber außerhalb dieser Norm bewegen, ist es ggf. riskant, ihnen das Anmelden ohne Weiteres zu erlauben. Es kann ratsam sein, den entsprechenden Benutzer zu blockieren oder ggf. einfach um die Durchführung einer mehrstufigen Authentifizierung zu bitten. So kann bewiesen werden, ob es sich auch wirklich um die Person handelt, die vorgegeben wird. 

Ein Anmelderisiko stellt die Wahrscheinlichkeit dar, dass eine bestimmte Authentifizierungsanforderung vom Identitätsbesitzer nicht autorisiert wurde. Organisationen mit Azure AD Premium P2-Lizenzen können Richtlinien für bedingten Zugriff erstellen, die [Azure AD Identity Protection-Risikoerkennungen](../identity-protection/concept-identity-protection-risks.md#sign-in-risk) enthalten.

Es gibt zwei Möglichkeiten, diese Richtlinie zuzuweisen. Organisationen sollten eine der folgenden Optionen auswählen, um eine Richtlinie für bedingten Zugriff anhand des Anmelderisikos zu aktivieren, die eine sichere Kennwortänderung erfordert.

## <a name="enable-with-conditional-access-policy"></a>Aktivieren mit einer Richtlinie für bedingten Zugriff

1. Melden Sie sich beim **Azure-Portal** als globaler Administrator, Sicherheitsadministrator oder Administrator für bedingten Zugriff an.
1. Navigieren Sie zu **Azure Active Directory** > **Sicherheit** > **Bedingter Zugriff**.
1. Wählen Sie **Neue Richtlinie**.
1. Benennen Sie Ihre Richtlinie. Es wird empfohlen, dass Unternehmen einen aussagekräftigen Standard für die Namen ihrer Richtlinien erstellen.
1. Klicken Sie unter **Zuweisungen** auf **Benutzer und Gruppen**.
   1. Wählen Sie unter **Einschließen** die Option **Alle Benutzer** aus.
   1. Wählen Sie unter **Ausschließen** die Option **Benutzer und Gruppen** und dann die Konten für den Notfallzugriff Ihres Unternehmens aus. 
   1. Wählen Sie **Fertig**aus.
1. Wählen Sie unter **Cloud-Apps oder -aktionen** > **Einschließen** die Option **Alle Cloud-Apps** aus.
1. Legen Sie unter **Bedingungen** > **Benutzerrisiko** die Option **Konfigurieren** auf **Ja**fest. Wählen Sie unter **Anmelderisikostufe auswählen, auf die diese Richtlinie angewendet werden soll** 
   1. entweder **Hoch** oder **Mittel** aus.
   1. Wählen Sie **Fertig**aus.
1. Wählen Sie unter **Zugriffssteuerung** > **Erteilen** die Option **Zugriff erteilen**, dann **Mehrstufige Authentifizierung erforderlich** und anschließend **Auswählen** aus.
1. Bestätigen Sie die Einstellungen und legen Sie **Richtlinie aktivieren** auf **Ein** fest.
1. Wählen Sie **Erstellen** aus, um die Richtlinie zu erstellen und zu aktivieren.

## <a name="enable-through-identity-protection"></a>Aktivieren über Identity Protection

1. Melden Sie sich beim **Azure-Portal** an.
1. Wählen Sie **Alle Dienste** aus, und navigieren Sie zu **Azure AD Identity Protection**.
1. Wählen Sie **Anmelderisiko-Richtlinie** aus.
1. Wählen Sie unter **Zuweisungen** die Option **Benutzer** aus.
   1. Wählen Sie unter **Einschließen** die Option **Alle Benutzer** aus.
   1. Wählen Sie unter **Ausschließen** die Option **Ausgeschlossene Benutzer auswählen** und dann die Konten für den Notfallzugriff Ihres Unternehmens aus. Abschließend wählen Sie **Auswählen** aus.
   1. Wählen Sie **Fertig**aus.
1. Wählen Sie unter **Bedingungen** die Option **Anmelderisiko** und dann **Mittel und darüber**.
   1. Wählen Sie **Auswählen** und anschließend **Fertig** aus.
1. Wählen Sie unter **Kontrollen** > **Zugriff** die Option **Zugriff zulassen** und dann **Multi-Factor Authentication erforderlich** aus.
   1. Wählen Sie **Auswählen**.
1. Legen Sie **Richtlinie erzwingen** auf **Ein** fest.
1. Wählen Sie **Speichern** aus.

## <a name="next-steps"></a>Nächste Schritte

[Allgemeine Richtlinien für bedingten Zugriff](concept-conditional-access-policy-common.md)

[Bedingter Zugriff anhand des Benutzerrisikos](howto-conditional-access-policy-risk-user.md)

[Bestimmen der Auswirkung durch Verwendung des reinen Berichtsmodus des bedingten Zugriffs](howto-conditional-access-insights-reporting.md)

[Simulieren des Anmeldeverhaltens mit dem Was-wäre-wenn-Tool für den bedingten Zugriff](troubleshoot-conditional-access-what-if.md)

[Was ist Azure Active Directory Identity Protection?](../identity-protection/overview-identity-protection.md)