---
title: 'Einrichten von Azure Multi-Factor Authentication für Windows Virtual Desktop: Azure'
description: Erfahren Sie, wie Sie Azure Multi-Factor Authentication einrichten, um die Sicherheit in Windows Virtual Desktop zu erhöhen.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 04/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: a769b5584abbd6da89ccb6032e5f0c5ac8ea1cb1
ms.sourcegitcommit: a6d477eb3cb9faebb15ed1bf7334ed0611c72053
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2020
ms.locfileid: "82930521"
---
# <a name="enable-azure-multi-factor-authentication-for-windows-virtual-desktop"></a>Aktivieren von Azure Multi-Factor Authentication für Windows Virtual Desktop

Der Windows-Client für Windows Virtual Desktop ist eine hervorragende Option für die Integration von Windows Virtual Desktop in Ihren lokalen Computer. Wenn Sie jedoch Ihr Windows Virtual Desktop-Konto für den Windows-Client konfigurieren, müssen Sie bestimmte Maßnahmen ergreifen, um sich selbst und Ihre Benutzer zu schützen.

Wenn Sie sich zum ersten Mal anmelden, fragt der Client nach Benutzername, Kennwort und Azure MFA. Wenn Sie sich das nächste Mal anmelden, speichert der Client Ihr Token von ihrer Azure Active Directory-Unternehmensanwendung. Wenn Sie **Speichern** auswählen, können sich Ihre Benutzer nach dem Neustart des Clients anmelden, ohne ihre Anmeldeinformationen erneut eingeben zu müssen.

Das Speichern von Anmeldeinformationen ist zwar praktisch, kann aber auch die Sicherheit von Bereitstellungen in Unternehmensszenarien oder auf persönlichen Geräten beeinträchtigen. Um Ihre Benutzer zu schützen, müssen Sie sicherstellen, dass der Client immer wieder zur Eingabe von MFA-Anmeldeinformationen (Azure Multi-Factor Authentication) auffordert. In diesem Artikel erfahren Sie, wie Sie die Richtlinie für bedingten Zugriff für Windows Virtual Desktop konfigurieren, um diese Einstellung zu aktivieren.

## <a name="prerequisites"></a>Voraussetzungen

Voraussetzungen für die ersten Schritte:

- Weisen Sie Benutzern eine Lizenz zu, die Azure Active Directory Premium P1 oder P2 umfasst.
- Eine Azure Active Directory-Gruppe, bei der Ihre Benutzer als Gruppenmitglieder zugewiesen sind
- Aktivieren Sie Azure MFA für alle Benutzer. Weitere Informationen dazu finden Sie unter [Vorgehensweise zum Erzwingen einer zweistufigen Überprüfung für einen Benutzer](../active-directory/authentication/howto-mfa-userstates.md#view-the-status-for-a-user).

> [!NOTE]
> Die folgende Einstellung gilt auch für den [Windows Virtual Desktop-Webclient](https://rdweb.wvd.microsoft.com/webclient/index.html).

## <a name="create-a-conditional-access-policy"></a>Erstellen der Richtlinie für bedingten Zugriff

In diesem Abschnitt erfahren Sie, wie Sie eine Richtlinie für bedingten Zugriff erstellen, die beim Herstellen einer Verbindung mit Windows Virtual Desktop Multi-Factor Authentication erfordert.

1. Melden Sie sich beim **Azure-Portal** als globaler Administrator, Sicherheitsadministrator oder Administrator für bedingten Zugriff an.
2. Navigieren Sie zu **Azure Active Directory** > **Sicherheit** > **Bedingter Zugriff**.
3. Wählen Sie **Neue Richtlinie**.
4. Benennen Sie Ihre Richtlinie. Es wird empfohlen, dass Unternehmen einen aussagekräftigen Standard für die Namen ihrer Richtlinien erstellen.
5. Klicken Sie unter **Zuweisungen** auf **Benutzer und Gruppen**.
   - Wählen Sie unter **Einschließen** **Benutzer und Gruppen auswählen** > **Benutzer und Gruppen** aus, und wählen Sie die Gruppe aus, die Sie in der Voraussetzungsphase erstellt haben.
   - Wählen Sie **Fertig**aus.
6. Wählen Sie unter **Cloud-Apps oder -Aktionen** > **Einschließen** die Option **Apps auswählen** aus.
   - Wählen Sie **Windows Virtual Desktop** (App-ID 9cdead84-a844-4324-93f2-b2e6bb768d07), dann **Auswählen** und anschließend **Fertig** aus.
   
     ![Screenshot der Seite „Cloud-Apps oder -aktionen“. Die Windows Virtual Desktop- und Windows Virtual Desktop-Client-Apps sind rot hervorgehoben.](media/cloud-apps-enterprise.png)

     >[!NOTE]
     >Um die App-ID der auszuwählenden App zu finden, wechseln Sie zu **Unternehmensanwendungen**, und wählen Sie **Microsoft-Anwendungen** aus dem Dropdownmenü für den Anwendungstyp aus.

7. Wählen Sie unter **Zugriffssteuerung** > **Erteilen** die Option **Zugriff erteilen**, dann **Mehrstufige Authentifizierung erforderlich** und anschließend **Auswählen** aus.
8. Wählen Sie unter **Zugriffssteuerung** > **Sitzung** die Option **Anmeldehäufigkeit** aus, legen Sie den Wert auf **1** und die Einheit auf **Stunden** fest, und wählen Sie dann **Auswählen** aus.
9. Bestätigen Sie die Einstellungen und legen Sie **Richtlinie aktivieren** auf **Ein** fest.
10. Wählen Sie **Erstellen** aus, um die Richtlinie zu aktivieren.

## <a name="next-steps"></a>Nächste Schritte

- [Weitere Informationen zu Richtlinien für bedingten Zugriff](../active-directory/conditional-access/concept-conditional-access-policies.md)

- [Weitere Informationen zur Benutzeranmeldehäufigkeit](../active-directory/conditional-access/howto-conditional-access-session-lifetime.md#user-sign-in-frequency)
