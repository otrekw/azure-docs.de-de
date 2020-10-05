---
title: 'Bedingter Zugriff: Blockieren der Legacyauthentifizierung – Azure Active Directory'
description: Erstellen einer Richtlinien für den bedingten Zugriff, um Legacyauthentifizierungsprotokolle zu blockieren
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: how-to
ms.date: 08/07/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7b4a0a411f51eed3a262ee59eb8e93b983755af3
ms.sourcegitcommit: 80b9c8ef63cc75b226db5513ad81368b8ab28a28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/16/2020
ms.locfileid: "90601553"
---
# <a name="conditional-access-block-legacy-authentication"></a>Bedingter Zugriff: Blockieren älterer Authentifizierungsmethoden

Aufgrund des erhöhten Risikos, das mit Legacyauthentifizierungsprotokollen verbunden ist, empfiehlt Microsoft, dass Unternehmen entsprechende Authentifizierungsanforderungen, die diese Protokolle verwenden, blockieren und eine moderne Authentifizierung erfordern.

## <a name="create-a-conditional-access-policy"></a>Erstellen der Richtlinie für bedingten Zugriff

Durch Ausführen der folgenden Schritte können Sie eine Richtlinie für bedingten Zugriff erstellen, um Legacyauthentifizierungsanforderungen zu blockieren. Für diese Richtlinie wird zu Beginn der Modus [Nur Bericht](howto-conditional-access-insights-reporting.md) festgelegt, damit Administratoren die Auswirkungen auf vorhandene Benutzer ermitteln können. Wenn Sie als Administrator der Ansicht sind, dass die Richtlinie den beabsichtigten Zweck erfüllt, können Sie sie auf **EIN** einstellen oder die nächste Phase der Bereitstellung beginnen, indem Sie bestimmte Gruppen hinzufügen und andere ausschließen.

1. Melden Sie sich als globaler Administrator beim **Azure-Portal** an.
1. Navigieren Sie zu **Azure Active Directory** > **Sicherheit** > **Bedingter Zugriff**.
1. Wählen Sie **Neue Richtlinie**.
1. Benennen Sie Ihre Richtlinie. Es wird empfohlen, dass Unternehmen einen aussagekräftigen Standard für die Namen ihrer Richtlinien erstellen.
1. Wählen Sie unter **Zuweisungen** die Option **Benutzer und Gruppen** aus.
   1. Wählen Sie unter **Einschließen** die Option **Alle Benutzer** aus.
   1. Wählen Sie unter **Ausschließen** die Option **Benutzer und Gruppen** und dann alle Konten aus, die die Möglichkeit aufweisen müssen, die Legacyauthentifizierung zu verwenden. Schließen Sie mindestens ein Konto aus, um zu verhindern, dass Sie selbst ausgesperrt werden. Wenn Sie kein Konto ausschließen, können Sie diese Richtlinie nicht erstellen.
   1. Wählen Sie **Fertig**aus.
1. Wählen Sie unter **Cloud-Apps oder -Aktionen** die Option **Alle Cloud-Apps** aus.
   1. Wählen Sie **Fertig**aus.
1. Legen Sie unter **Bedingungen** > **Client-Apps** die Option **Konfigurieren** auf **Ja** fest.
   1. Aktivieren Sie nur die Kontrollkästchen **Exchange ActiveSync-Clients** und **Andere Clients**.
   1. Wählen Sie **Fertig**aus.
1. Wählen Sie unter **Zugriffssteuerung** > **Erteilen** die Option **Zugriff blockieren** aus.
   1. Wählen Sie **Auswählen**.
1. Bestätigen Sie die Einstellungen, und legen Sie **Richtlinie aktivieren** auf **Nur Bericht** fest.
1. Wählen Sie **Erstellen** aus, um die Richtlinie zu erstellen und zu aktivieren.

## <a name="next-steps"></a>Nächste Schritte

[Allgemeine Richtlinien für bedingten Zugriff](concept-conditional-access-policy-common.md)

[Bestimmen der Auswirkung durch Verwendung des reinen Berichtsmodus des bedingten Zugriffs](howto-conditional-access-insights-reporting.md)

[Simulieren des Anmeldeverhaltens mit dem Was-wäre-wenn-Tool für den bedingten Zugriff](troubleshoot-conditional-access-what-if.md)

[Einrichten eines Multifunktionsgeräts oder einer -anwendung zum Senden von E-Mails mit Microsoft 365](/exchange/mail-flow-best-practices/how-to-set-up-a-multifunction-device-or-application-to-send-email-using-office-3)
