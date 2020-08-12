---
title: Benutzerspezifisches Aktivieren von Multi-Factor Authentication – Azure Active Directory
description: Erfahren Sie, wie Azure Multi-Factor Authentication benutzerspezifisch aktiviert wird, indem der Benutzerstatus geändert wird.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 07/20/2020
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2bd688a9e488c1206b0c8531698b061f650e2afe
ms.sourcegitcommit: e71da24cc108efc2c194007f976f74dd596ab013
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2020
ms.locfileid: "87417929"
---
# <a name="enable-per-user-azure-multi-factor-authentication-to-secure-sign-in-events"></a>Benutzerspezifisches Aktivieren von Azure Multi-Factor Authentication zum Schutz von Anmeldeereignissen

Es gibt zwei Möglichkeiten, Benutzeranmeldeereignisse zu schützen, indem in Azure AD eine mehrstufige Authentifizierung angefordert wird. Die erste und bevorzugte Option ist die Einrichtung einer Richtlinie für bedingten Zugriff, die unter bestimmten Bedingungen eine mehrstufige Authentifizierung verlangt. Die zweite Option besteht darin, jeden Benutzer für Azure Multi-Factor Authentication zu aktivieren. Wenn Benutzer individuell aktiviert werden, führen sie die mehrstufige Authentifizierung bei jeder Anmeldung durch (bis auf einige Ausnahmen, beispielsweise wenn sie sich von vertrauenswürdigen IP-Adressen aus anmelden oder wenn die Funktion _Gespeicherte Geräte_ aktiviert ist).

> [!NOTE]
> Der empfohlene Ansatz ist das Aktivieren von Azure Multi-Factor Authentication mit Richtlinien für bedingten Zugriff. Das Ändern von Benutzerstatus wird nicht mehr empfohlen, es sei denn, Ihre Lizenzen decken den bedingten Zugriff nicht ab, da Benutzer in dem Fall bei jeder Anmeldung eine mehrstufige Authentifizierung durchführen müssen. Informationen zu den ersten Schritten mit bedingtem Zugriff finden Sie im [Tutorial: Schützen von Benutzeranmeldeereignissen mit Azure Multi-Factor Authentication](tutorial-enable-azure-mfa.md).
>
> Für kostenlose Azure AD-Mandanten ohne bedingten Zugriff können Sie [Sicherheitsstandards zum Schützen von Benutzern verwenden](../fundamentals/concept-fundamentals-security-defaults.md).

## <a name="azure-multi-factor-authentication-user-states"></a>Benutzerstatus in Microsoft Azure Multi-Factor Authentication

Benutzerkonten in Azure Multi-Factor Authentication können die folgenden drei Zustände aufweisen:

> [!IMPORTANT]
> Wenn Sie Azure Multi-Factor Authentication über eine Richtlinie für bedingten Zugriff aktivieren, ändert sich der Status des Benutzers nicht. Keine Sorge, falls Benutzer als deaktiviert angezeigt werden. Der Status wird durch bedingten Zugriff nicht geändert.
>
> **Bei Verwendung von Richtlinien für bedingten Zugriff sollten Sie Benutzer nicht für MFA aktivieren oder diese erzwingen.**

| Status | BESCHREIBUNG | Nicht-Browser-Apps betroffen | Browser-Apps betroffen | Moderne Authentifizierung betroffen |
|:---:| --- |:---:|:--:|:--:|
| Disabled | Der Standardstatus eines neuen Benutzers, der nicht in Azure Multi-Factor Authentication registriert ist. | Nein | Nein | Nein |
| Aktiviert | Der Benutzer wurde zwar in Multi-Factor Authentication registriert, hat aber keine Authentifizierungsmethoden registriert. Der Benutzer wird aufgefordert, sich bei der nächsten Anmeldung zu registrieren. | Nein.  Sie werden weiterhin ausgeführt, bis die Registrierung abgeschlossen ist. | Ja. Nach Ablauf der Sitzung ist eine Registrierung bei Azure Multi-Factor Authentication erforderlich.| Ja. Nach Ablauf des Zugriffstokens ist eine Registrierung bei Azure Multi-Factor Authentication erforderlich. |
| Erzwungen | Der Benutzer wurde registriert und hat den Registrierungsprozess für Azure Multi-Factor Authentication abgeschlossen. | Ja. Für Apps sind App-Kennwörter erforderlich. | Ja. Azure Multi-Factor Authentication ist bei der Anmeldung erforderlich. | Ja. Azure Multi-Factor Authentication ist bei der Anmeldung erforderlich. |

Der Status eines Benutzers gibt an, ob ein Administrator den Benutzer bei Azure Multi-Factor Authentication registriert hat und ob der Registrierungsprozess abgeschlossen ist.

Der Anfangsstatus aller Benutzer lautet *Deaktiviert*. Wenn Sie Benutzer für Azure Multi-Factor Authentication registrieren, ändert sich der Status in *Aktiviert*. Wenn aktivierte Benutzer sich anmelden und den Registrierungsprozess abschließen, ändert sich ihr Status in *Erzwungen*.

> [!NOTE]
> Wenn MFA für ein Benutzerobjekt reaktiviert wird, das bereits über Registrierungsdetails wie Telefonnummer oder E-Mail-Adresse verfügt, müssen die Administratoren den betreffenden Benutzer zur erneuten MFA-Registrierung über das Azure-Portal oder über PowerShell auffordern. Wenn sich der Benutzer nicht erneut registriert, ändert sich der MFA-Zustand auf der MFA-Verwaltungsoberfläche nicht von *Aktiviert* in *Erzwungen*.

## <a name="view-the-status-for-a-user"></a>Anzeigen des Status eines Benutzers

Führen Sie die folgenden Schritte aus, um auf das Azure-Portal zuzugreifen, in dem Sie Benutzerstatus anzeigen und verwalten können:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) als Administrator an.
1. Suchen und wählen Sie *Azure Active Directory* und dann **Benutzer** > **Alle Benutzer** aus.
1. Wählen Sie **Multi-Factor Authentication** aus. Scrollen Sie bei Bedarf nach rechts, um diese Menüoption anzuzeigen. Wählen Sie den unten gezeigten Beispielscreenshot aus, um das gesamte Fenster des Azure-Portals sowie die Menüposition anzuzeigen: [![Auswählen von „Multi-Factor Authentication“ im Fenster „Benutzer“ in Azure AD](media/howto-mfa-userstates/selectmfa-cropped.png)](media/howto-mfa-userstates/selectmfa.png#lightbox)
1. Eine neue Seite wird geöffnet, auf der, wie im folgenden Beispiel gezeigt, der Benutzerstatus angezeigt wird.
   ![Screenshot mit Beispielinformationen zum Benutzerstatus für Azure Multi-Factor Authentication](./media/howto-mfa-userstates/userstate1.png)

## <a name="change-the-status-for-a-user"></a>Ändern des Status eines Benutzers

Um den Azure Multi-Factor Authentication-Status eines Benutzers zu ändern, führen Sie die folgenden Schritte aus:

1. Führen Sie die vorstehenden Schritte aus, um zur Seite mit den Azure Multi-Factor Authentication-**Benutzern** zu gelangen.
1. Suchen Sie den Benutzer, für den Sie Azure Multi-Factor Authentication aktivieren möchten. Sie müssen möglicherweise oben die Ansicht in **Benutzer** ändern.
   ![Benutzer, für den der Status geändert werden soll, auf der Registerkarte „Benutzer“ auswählen](./media/howto-mfa-userstates/enable1.png)
1. Aktivieren Sie das Kontrollkästchen neben den Namen der Benutzer, deren Status geändert werden soll.
1. Wählen Sie auf der rechten Seite unter **QuickSteps** die Option **Aktivieren** oder **Deaktivieren** aus. Im folgenden Beispiel hat der Benutzer *John Smith* ein Häkchen neben seinem Namen und ist für die Verwendung aktiviert: ![Ausgewählten Benutzer durch Klicken auf „Aktivieren“ (im Menü „QuickSteps“) aktivieren](./media/howto-mfa-userstates/user1.png)

   > [!TIP]
   > *Aktivierte* Benutzer werden automatisch in *Erzwungen* geändert, wenn sie sich für Azure Multi-Factor Authentication registrieren. Ändern Sie den Benutzerstatus keinesfalls manuell in *Erzwungen*.

1. Bestätigen Sie Ihre Auswahl im Popupfenster, das geöffnet wird.

Benachrichtigen Sie die Benutzer per E-Mail, nachdem Sie die Benutzer aktiviert haben. Teilen Sie den Benutzern mit, dass eine Aufforderung angezeigt wird, sich bei der nächsten Anmeldung zu registrieren. Und wenn Ihre Organisation auch nicht auf Browsern basierende Apps verwendet, die die moderne Authentifizierung nicht unterstützen, müssen die Benutzer App-Kennwörter erstellen. Weitere Informationen finden Sie in der [Anleitung zu Azure Multi-Factor Authentication für Endbenutzer](../user-help/multi-factor-authentication-end-user.md).

## <a name="change-state-using-powershell"></a>Ändern des Status mithilfe von PowerShell

Zum Ändern des Benutzerstatus mit [Azure AD PowerShell](/powershell/azure/) ändern Sie den Parameter`$st.State` für ein Benutzerkonto. Für ein Benutzerkonto gibt es drei mögliche Status:

* *Aktiviert*
* *Erzwungen*
* *Disabled*  

Ändern Sie nicht direkt den Status *Erzwungen* für Benutzer. Wenn Sie dies tun, funktionieren nicht browserbasierte Apps nicht mehr, weil der Benutzer die Azure Multi-Factor Authentication-Registrierung nicht durchlaufen und kein [App-Kennwort](howto-mfa-app-passwords.md) erhalten hat.

Installieren Sie zunächst das Modul *MSOnline* mithilfe von [Install-Module](/powershell/module/powershellget/install-module) wie folgt:

```PowerShell
Install-Module MSOnline
```

Stellen Sie als Nächstes mithilfe von [Connect-MsolService](/powershell/module/msonline/connect-msolservice) eine Verbindung her:

```PowerShell
Connect-MsolService
```

Das folgende PowerShell-Beispielskript aktiviert MFA für einen einzelnen Benutzer namens *bsimon@contoso.com* :

```PowerShell
$st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
$st.RelyingParty = "*"
$st.State = "Enabled"
$sta = @($st)

# Change the following UserPrincipalName to the user you wish to change state
Set-MsolUser -UserPrincipalName bsimon@contoso.com -StrongAuthenticationRequirements $sta
```

PowerShell ist eine gute Wahl für die Massenaktivierung von Benutzern. Das folgende Skript durchläuft eine Liste mit Benutzern und aktiviert MFA für ihre Konten. Legen Sie die Benutzerkonten in der ersten Zeile für `$users` wie folgt fest:

   ```PowerShell
   # Define your list of users to update state in bulk
   $users = "bsimon@contoso.com","jsmith@contoso.com","ljacobson@contoso.com"

   foreach ($user in $users)
   {
       $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
       $st.RelyingParty = "*"
       $st.State = "Enabled"
       $sta = @($st)
       Set-MsolUser -UserPrincipalName $user -StrongAuthenticationRequirements $sta
   }
   ```

Um MFA zu deaktivieren, ruft das folgende Beispiel einen Benutzer mit [Get-MsolUser](/powershell/module/msonline/get-msoluser) ab. Anschließend werden alle *StrongAuthenticationRequirements* entfernt, die für den definierten Benutzer mit [Set-MsolUser](/powershell/module/msonline/set-msoluser) festgelegt wurden:

```PowerShell
Get-MsolUser -UserPrincipalName bsimon@contoso.com | Set-MsolUser -StrongAuthenticationRequirements @()
```

Sie können MFA für einen Benutzer auch direkt deaktivieren, indem Sie [Set-MsolUser](/powershell/module/msonline/set-msoluser) wie folgt verwenden:

```PowerShell
Set-MsolUser -UserPrincipalName bsimon@contoso.com -StrongAuthenticationRequirements @()
```

## <a name="convert-users-from-per-user-mfa-to-conditional-access-based-mfa"></a>Konvertieren von Benutzern von „MFA pro Benutzer“ zu „MFA mit bedingtem Zugriff“

Das folgende PowerShell-Skript kann Ihnen bei der Konvertierung in Azure Multi-Factor Authentication mit bedingtem Zugriff helfen.

```PowerShell
# Sets the MFA requirement state
function Set-MfaState {

    [CmdletBinding()]
    param(
        [Parameter(ValueFromPipelineByPropertyName=$True)]
        $ObjectId,
        [Parameter(ValueFromPipelineByPropertyName=$True)]
        $UserPrincipalName,
        [ValidateSet("Disabled","Enabled","Enforced")]
        $State
    )

    Process {
        Write-Verbose ("Setting MFA state for user '{0}' to '{1}'." -f $ObjectId, $State)
        $Requirements = @()
        if ($State -ne "Disabled") {
            $Requirement =
                [Microsoft.Online.Administration.StrongAuthenticationRequirement]::new()
            $Requirement.RelyingParty = "*"
            $Requirement.State = $State
            $Requirements += $Requirement
        }

        Set-MsolUser -ObjectId $ObjectId -UserPrincipalName $UserPrincipalName `
                     -StrongAuthenticationRequirements $Requirements
    }
}

# Disable MFA for all users
Get-MsolUser -All | Set-MfaState -State Disabled
```

> [!NOTE]
> Wenn MFA für ein Benutzerobjekt reaktiviert wird, das bereits über Registrierungsdetails wie Telefonnummer oder E-Mail-Adresse verfügt, müssen die Administratoren den betreffenden Benutzer zur erneuten MFA-Registrierung über das Azure-Portal oder über PowerShell auffordern. Wenn sich der Benutzer nicht erneut registriert, ändert sich der MFA-Zustand auf der MFA-Verwaltungsoberfläche nicht von *Aktiviert* in *Erzwungen*.

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Konfigurieren der Azure Multi-Factor Authentication-Einstellungen finden Sie unter [Konfigurieren von Azure Multi-Factor Authentication-Einstellungen](howto-mfa-mfasettings.md).

Informationen zum Verwalten von Benutzereinstellungen für die Azure Multi-Factor Authentication finden Sie unter [Verwalten von Benutzereinstellungen mit Azure Multi-Factor Authentication](howto-mfa-userdevicesettings.md).

Um zu verstehen, warum ein Benutzer zur Ausführung von MFA aufgefordert bzw. nicht aufgefordert wurde, finden Sie unter [Azure Multi-Factor Authentication-Berichte](howto-mfa-reporting.md).
