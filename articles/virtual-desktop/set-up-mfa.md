---
title: 'Einrichten von Azure Multi-Factor Authentication für Windows Virtual Desktop: Azure'
description: Erfahren Sie, wie Sie Azure Multi-Factor Authentication einrichten, um die Sicherheit in Windows Virtual Desktop zu erhöhen.
author: Heidilohr
ms.topic: how-to
ms.date: 07/15/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 5e42ca0a0d0ff9d9df3dc42f1e165d1035d56d6a
ms.sourcegitcommit: 98854e3bd1ab04ce42816cae1892ed0caeedf461
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/07/2020
ms.locfileid: "88009459"
---
# <a name="enable-azure-multi-factor-authentication-for-windows-virtual-desktop"></a>Aktivieren von Azure Multi-Factor Authentication für Windows Virtual Desktop

>[!IMPORTANT]
> Wenn Sie diese Seite von der Dokumentation zu Windows Virtual Desktop (klassisch) aus besuchen, stellen Sie sicher, dass Sie [zur Dokumentation zu Windows Virtual Desktop (klassisch) zurückkehren](./virtual-desktop-fall-2019/tenant-setup-azure-active-directory.md), wenn Sie fertig sind.

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

Hier erfahren Sie, wie Sie eine Richtlinie für bedingten Zugriff erstellen, die beim Herstellen einer Verbindung mit Windows Virtual Desktop Multi-Factor Authentication erfordert:

1. Melden Sie sich beim **Azure-Portal** als globaler Administrator, Sicherheitsadministrator oder Administrator für bedingten Zugriff an.
2. Navigieren Sie zu **Azure Active Directory** > **Sicherheit** > **Bedingter Zugriff**.
3. Wählen Sie **Neue Richtlinie**.
4. Benennen Sie Ihre Richtlinie. Es wird empfohlen, dass Unternehmen einen aussagekräftigen Standard für die Namen ihrer Richtlinien erstellen.
5. Klicken Sie unter **Zuweisungen** auf **Benutzer und Gruppen**.
6. Wählen Sie unter **Einschließen** **Benutzer und Gruppen auswählen** > **Benutzer und Gruppen** aus, und wählen Sie die Gruppe aus, die Sie in der [Voraussetzungsphase](#prerequisites) erstellt haben.
7. Wählen Sie **Fertig**aus.
8. Wählen Sie unter **Cloud-Apps oder -Aktionen** > **Einschließen** die Option **Apps auswählen** aus.
9. Wählen Sie eine der folgenden Gruppen von Apps aus, je nachdem, welche Version von Windows Virtual Desktop Sie verwenden.
   - Wenn Sie Windows Virtual Desktop (klassisch) verwenden, wählen Sie diese beiden Apps aus:
       - **Windows Virtual Desktop** (App-ID 5a0aa725-4958-4b0c-80a9-34562e23f3b7)
       - **Windows Virtual Desktop Client** (App-ID fa4345a4-a730-4230-84a8-7d9651b86739)
   - Wenn Sie Windows Virtual Desktop verwenden, wählen Sie stattdessen diese beiden Apps aus:
       -  **Windows Virtual Desktop** (App-ID 9cdead84-a844-4324-93f2-b2e6bb768d07)
       -  **Windows Virtual Desktop-Client** (App-ID a85cf173-4192-42f8-81fa-777a763e6e2c)

   >[!IMPORTANT]
   > Die Windows Virtual Desktop-Client-Apps werden für den Webclient verwendet. Wählen Sie jedoch nicht die App namens Windows Virtual Desktop Azure Resource Manager-Anbieter (50e95039-b200-4007-bc97-8d5790743a63) aus. Diese App wird nur zum Abrufen des Benutzerfeeds verwendet und sollte keine MFA aufweisen.

1. Nachdem Sie Ihre App ausgewählt haben, wählen Sie **Auswählen** und dann **Fertig** aus.

   > [!div class="mx-imgBorder"]
   > ![Screenshot der Seite „Cloud-Apps oder -aktionen“. Die Windows Virtual Desktop- und Windows Virtual Desktop-Client-Apps werden rot hervorgehoben.](media/cloud-apps-enterprise.png)

   >[!NOTE]
   >Um die App-ID der auszuwählenden App zu finden, wechseln Sie zu **Unternehmensanwendungen**, und wählen Sie **Microsoft-Anwendungen** aus dem Dropdownmenü für den Anwendungstyp aus.

10. Wählen Sie unter **Zugriffssteuerung** > **Erteilen** die Option **Zugriff erteilen**, dann **Mehrstufige Authentifizierung erforderlich** und anschließend **Auswählen** aus.
11. Wählen Sie unter **Zugriffssteuerung** > **Sitzung** die Option **Anmeldehäufigkeit** aus, legen Sie den Wert auf **1** und die Einheit auf **Stunden** fest, und wählen Sie dann **Auswählen** aus.
12. Bestätigen Sie die Einstellungen und legen Sie **Richtlinie aktivieren** auf **Ein** fest.
13. Wählen Sie **Erstellen** aus, um die Richtlinie zu aktivieren.

## <a name="next-steps"></a>Nächste Schritte

- [Weitere Informationen zu Richtlinien für bedingten Zugriff](../active-directory/conditional-access/concept-conditional-access-policies.md)

- [Weitere Informationen zur Benutzeranmeldehäufigkeit](../active-directory/conditional-access/howto-conditional-access-session-lifetime.md#user-sign-in-frequency)
