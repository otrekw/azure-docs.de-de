---
title: 'Einrichten von Azure Multi-Factor Authentication für Windows Virtual Desktop: Azure'
description: Erfahren Sie, wie Sie Azure Multi-Factor Authentication einrichten, um die Sicherheit in Windows Virtual Desktop zu erhöhen.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 04/01/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: b470f9278bdca94d1fe98c64b11b070fb36cb075
ms.sourcegitcommit: 25490467e43cbc3139a0df60125687e2b1c73c09
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2020
ms.locfileid: "80998470"
---
# <a name="set-up-azure-multi-factor-authentication"></a>Einrichten von Azure Multi-Factor Authentication

Der Windows-Client für Windows Virtual Desktop ist eine hervorragende Option für die Integration von Windows Virtual Desktop in Ihren lokalen Computer. Wenn Sie jedoch Ihr Windows Virtual Desktop-Konto für den Windows-Client konfigurieren, müssen Sie bestimmte Maßnahmen ergreifen, um sich selbst und Ihre Benutzer zu schützen.

Wenn Sie sich zum ersten Mal anmelden, fragt der Client nach Benutzername, Kennwort und Azure MFA. Wenn Sie sich das nächste Mal anmelden, speichert der Client Ihr Token von ihrer Azure Active Directory-Unternehmensanwendung. Wenn Sie **Speichern** auswählen, können sich Ihre Benutzer nach dem Neustart des Clients anmelden, ohne ihre Anmeldeinformationen erneut eingeben zu müssen.

Das Speichern von Anmeldeinformationen ist zwar praktisch, kann aber auch die Sicherheit von Bereitstellungen in Unternehmensszenarien oder auf persönlichen Geräten beeinträchtigen. Um Ihre Benutzer zu schützen, müssen Sie sicherstellen, dass der Client immer wieder zur Eingabe von MFA-Anmeldeinformationen (Azure Multi-Factor Authentication) auffordert. In diesem Artikel erfahren Sie, wie Sie die Richtlinie für bedingten Zugriff für Windows Virtual Desktop konfigurieren, um diese Einstellung zu aktivieren.

## <a name="prerequisites"></a>Voraussetzungen

Voraussetzungen für die ersten Schritte:

- Weisen Sie allen Benutzern eine der folgenden Lizenzen zu:
  - Microsoft 365 E3 oder E5
  - Azure Active Directory Premium P1 oder P2
  - Enterprise Mobility + Security E3 oder E5
- Eine Azure Active Directory-Gruppe, bei der Ihre Benutzer als Gruppenmitglieder zugewiesen sind
- Aktivieren Sie Azure MFA für alle Benutzer. Weitere Informationen dazu finden Sie unter [Vorgehensweise zum Erzwingen einer zweistufigen Überprüfung für einen Benutzer](../active-directory/authentication/howto-mfa-userstates.md#view-the-status-for-a-user).

>[!NOTE]
>Die folgende Einstellung gilt auch für den [Windows Virtual Desktop-Webclient](https://rdweb.wvd.microsoft.com/webclient/index.html).

## <a name="opt-in-to-the-conditional-access-policy"></a>Aktivieren der Richtlinie für bedingten Zugriff

1. Öffnen Sie **Azure Active Directory**.

2. Wechseln Sie zur Registerkarte **Alle Anwendungen**. Wählen Sie im Dropdownmenü „Anwendungstyp“ die Option **Unternehmensanwendungen** aus, und suchen Sie dann nach **Windows Virtual Desktop-Client**.

    ![Screenshot der Registerkarte „Alle Anwendungen“. Der Benutzer hat „Windows Virtual Desktop-Client“ in die Suchleiste eingegeben, und die App wurde in den Suchergebnissen angezeigt.](media/all-applications-search.png)

3. Wählen Sie **Bedingter Zugriff** aus.

    ![Ein Screenshot, der anzeigt, dass der Benutzer den Mauszeiger über die Registerkarte für den bedingten Zugriff bewegt.](media/conditional-access-location.png)

4. Wählen Sie **+ Neue Richtlinie** aus.

   ![Screenshot der Seite „Bedingter Zugriff“. Der Benutzer bewegt den Mauszeiger über die Schaltfläche „Neue Richtlinie“.](media/new-policy-button.png)

5. Geben Sie einen **Namen** für die **Regel** ein, dann **wählen** Sie den *Namen der **Gruppe** aus, die Sie in den Voraussetzungen erstellt haben.

6. Wählen Sie **Auswählen** und dann **Fertig** aus.

7. Öffnen Sie als nächstes **Cloud-Apps oder -aktionen**.

8. Wählen Sie im Bereich **Auswählen** die Unternehmensanwendung **Windows Virtual Desktop** aus.

    ![Screenshot der Seite „Cloud-Apps oder -aktionen“. Der Benutzer hat die Windows Virtual Desktop-App aktiviert, indem er das Häkchen daneben gesetzt hat. Die ausgewählte App wird rot hervorgehoben.](media/cloud-apps-select.png)
    
    >[!NOTE]
    >Sie sollten auch die ausgewählte Windows Virtual Desktop Client-App auf der linken Seite des Bildschirms sehen, wie in der folgenden Abbildung gezeigt. Sie benötigen sowohl die Windows Virtual Desktop- als auch die Windows Virtual Desktop-Clientunternehmensanwendungen, damit die Richtlinie funktioniert.
    >
    > ![Screenshot der Seite „Cloud-Apps oder -aktionen“. Die Windows Virtual Desktop- und Windows Virtual Desktop-Client-Apps sind rot hervorgehoben.](media/cloud-apps-enterprise-selected.png)

9. Wählen Sie **Auswählen** aus.

10. Als nächstes öffnen Sie **Erteilen**. 

11. Wählen Sie **Mehrstufige Authentifizierung anfordern** und dann **Eine der ausgewählten Steuerungen anfordern** aus.
   
    ![Screenshot der Seite „Erteilen“. Die Option „Mehrstufige Authentifizierung anfordern“ ist aktiviert.](media/grant-page.png)

    >[!NOTE]
    >Wenn Sie in Ihrem Unternehmen über MDM-registrierte Geräte verfügen und nicht möchten, dass diese die MFA-Eingabeaufforderung anzeigen, können Sie auch **Markieren des Geräts als kompatibel erforderlich** auswählen.

12. Wählen Sie **Sitzung** aus.

13. Legen Sie die **Anmeldehäufigkeit** auf **Aktiv** fest, und ändern Sie dann ihren Wert in **1 Stunde(n)** .

    ![Screenshot der Seite „Sitzung“. Das Sitzungsmenü zeigt, dass die Dropdownmenüs für die Anmeldehäufigkeit in „1“ und „Stunden“ geändert wurden.](media/sign-in-frequency.png)
   
    >[!NOTE]
    >Aktive Sitzungen in Ihrer Windows Virtual Desktop-Umgebung funktionieren weiterhin, wenn Sie die Richtlinie ändern. Wenn Sie jedoch die Verbindung trennen oder sich abmelden, müssen Sie die Anmeldeinformationen nach 60 Minuten erneut bereitstellen. Wenn Sie die Einstellungen ändern, können Sie das Zeitlimit beliebig verlängern (solange es der Sicherheitsrichtlinie Ihrer Organisation entspricht).
    >
    >Die Standardeinstellung ist ein rollierendes Fenster von 90 Tagen. Das bedeutet, dass der Client die Benutzer auffordert, sich erneut anzumelden, wenn sie versuchen, auf eine Ressource zuzugreifen, nachdem sie 90 Tage oder länger auf ihrem Computer inaktiv waren.

14. Aktivieren Sie die Richtlinie.

15. Wählen Sie **Erstellen** aus, um die Richtlinie zu bestätigen.

Alles erledigt! Sie können die Richtlinie testen, um sicherzustellen, dass Ihre Zulassungsliste ordnungsgemäß funktioniert.
