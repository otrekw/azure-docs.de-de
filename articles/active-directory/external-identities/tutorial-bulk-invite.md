---
title: 'Tutorial für das Masseneinladen von Benutzern für die B2B-Zusammenarbeit: Azure AD'
description: In diesem Tutorial erfahren Sie, wie Sie mit PowerShell und einer CSV-Datei Masseneinladungen an externe Benutzer für die Azure AD B2B-Zusammenarbeit senden.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: tutorial
ms.date: 05/07/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.collection: M365-identity-device-management
ms.openlocfilehash: f0f88b310bc00881e66ee8e8b5f2d40616d60315
ms.sourcegitcommit: 4e5560887b8f10539d7564eedaff4316adb27e2c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2020
ms.locfileid: "87906914"
---
# <a name="tutorial-bulk-invite-azure-ad-b2b-collaboration-users"></a>Tutorial: Masseneinladen von Benutzern für die Azure AD B2B-Zusammenarbeit

Wenn Sie Azure Active Directory (Azure AD) für die B2B-Zusammenarbeit mit externen Partnern verwenden, können Sie mehrere Gastbenutzer gleichzeitig in Ihre Organisation einladen. In diesem Tutorial erfahren Sie, wie Sie mit dem Azure-Portal Masseneinladungen an externe Benutzer senden. Das Tutorial umfasst vor allem die folgenden Aufgaben:

> [!div class="checklist"]
> * Vorbereiten einer CSV-Datei mit den Benutzerinformationen und Einladungseinstellungen mithilfe von **Massenbenutzereinladung**
> * Hochladen der CSV-Datei in Azure AD
> * Überprüfen, ob Benutzer dem Verzeichnis hinzugefügt wurden

Sollten Sie keine Azure Active Directory-Instanz haben, erstellen Sie zunächst ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="understand-the-csv-template"></a>Grundlegendes zur CSV-Vorlage

Laden Sie die CSV-Vorlage für den Massenupload herunter, und füllen Sie sie aus, um mehrere Azure AD-Gastbenutzer gleichzeitig einzuladen. Die heruntergeladene CSV-Vorlage ähnelt etwa dem folgenden Beispiel:

![Hochzuladendes Arbeitsblatt mit Anmerkungen zum Zweck und den Werten für die Zeilen und Spalten](media/tutorial-bulk-invite/understand-template.png)

### <a name="csv-template-structure"></a>CSV-Vorlagenstruktur

Die Zeilen der heruntergeladenen CSV-Vorlage lauten wie folgt:

- **Versionsnummer**: Die erste Zeile, die die Versionsnummer enthält, muss in der hochzuladenden CSV-Datei enthalten sein.
- **Spaltenüberschriften:** Das Format der Spaltenüberschriften lautet &lt;*Elementname*&gt; [Eigenschaftenname] &lt;*Required (erforderlich) oder leer*&gt;. Beispiel: `Email address to invite [inviteeEmail] Required`. Einige ältere Versionen der Vorlage können geringfügige Abweichungen aufweisen.
- **Beispielzeile:** In der Vorlage ist eine Zeile mit Beispielen für zulässige Werte für alle Spalten enthalten. Sie müssen die Beispielzeile entfernen und durch Ihre eigenen Einträge ersetzen.

### <a name="additional-guidance"></a>Zusätzliche Anleitungen

- Die ersten zwei Zeilen der hochzuladenden Vorlage dürfen nicht entfernt oder geändert werden, da der Upload sonst nicht verarbeitet werden kann.
- Die erforderlichen Spalten werden zuerst aufgelistet.
- Es wird davon abgeraten, der Vorlage neue Spalten hinzuzufügen. Jegliche Spalten, die Sie zusätzlich hinzufügen, werden ignoriert und nicht verarbeitet.
- Es wird empfohlen, möglichst häufig die neueste Version der CSV-Vorlage herunterzuladen.

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen mindestens zwei Test-E-Mail-Konten, an die Sie die Einladungen senden können. Die Konten müssen sich außerhalb Ihrer Organisation befinden. Sie können jede Art von Konto verwenden, einschließlich Konten bei sozialen Netzwerken wie „gmail.com“- oder „outlook.com“-Adressen.

## <a name="invite-guest-users-in-bulk"></a>Masseneinladung von Gastbenutzern

1. Melden Sie sich beim Azure-Portal mit einem Konto an, das über Benutzeradministratorberechtigungen in der Organisation verfügt.
2. Klicken Sie im Navigationsbereich auf **Azure Active Directory**.
3. Wählen Sie unter **Verwalten** die Optionen **Benutzer** > **Masseneinladung** aus.
4. Wählen Sie auf der Seite **Massenbenutzereinladung** die Option **Herunterladen** aus, um eine gültige CSV-Vorlage mit Einladungseigenschaften zu erhalten.

    ![Downloadschaltfläche für Masseneinladungen](media/tutorial-bulk-invite/bulk-invite-button.png)

5. Öffnen Sie die CSV-Vorlage, und fügen Sie eine Zeile für jeden Gastbenutzer hinzu. Erforderliche Werte:

   * **E-Mail-Adresse für Einladung**: Der Benutzer, der eine Einladung erhält

   * **Umleitungs-URL**: Die URL, an die der eingeladene Benutzer nach dem Akzeptieren der Einladung weitergeleitet wird

    ![Beispiel für eine CSV-Datei mit eingegebenen Gastbenutzern](media/tutorial-bulk-invite/bulk-invite-csv.png)

   > [!NOTE]
   > Verwenden Sie unter **Benutzerdefinierte Einladungsnachricht** keine Kommas, da die Nachricht sonst nicht erfolgreich analysiert werden kann.

6. Speichern Sie die Datei .
7. Navigieren Sie auf der Seite **Massenbenutzereinladung** unter **CSV-Datei hochladen** zur entsprechenden Datei. Wenn Sie die Datei auswählen, wird mit der Überprüfung der CSV-Datei begonnen. 
8. Nach der Überprüfung des Dateiinhalts wird die Meldung **Datei erfolgreich hochgeladen** angezeigt. Wenn Fehler vorliegen, müssen Sie diese beheben, bevor Sie den Auftrag übermitteln können.
9. Wenn Ihre Datei die Überprüfung bestanden hat, wählen Sie **Senden** aus, um den Azure-Massenvorgang zum Hinzufügen der Einladungen zu starten. 
10. Wählen Sie zum Anzeigen des Auftragsstatus **Klicken Sie hier, um den Status für jeden Vorgang anzuzeigen.** aus. Alternativ können Sie im Abschnitt **Aktivität** die Option **Ergebnisse von Massenvorgängen** auswählen. Wenn Sie ausführliche Informationen zu jedem Zeilenelement des Massenvorgangs erhalten möchten, wählen Sie die Werte in der Spalte **# Erfolg**, **# Fehler** oder **Anforderungen insgesamt** aus. Wenn Fehler aufgetreten sind, werden die Fehlerursachen aufgeführt.

    ![Beispiel für Ergebnisse von Massenvorgängen](media/tutorial-bulk-invite/bulk-operation-results.png)

11. Nach Abschluss des Vorgangs wird eine Benachrichtigung angezeigt, dass der Massenvorgang erfolgreich abgeschlossen wurde.

## <a name="verify-guest-users-in-the-directory"></a>Überprüfen von Gastbenutzern im Verzeichnis

Überprüfen Sie entweder im Azure-Portal oder mithilfe von PowerShell, ob die hinzugefügten Gastbenutzer im Verzeichnis vorhanden sind.

### <a name="view-guest-users-in-the-azure-portal"></a>Anzeigen von Gastbenutzern im Azure-Portal

1. Melden Sie sich beim Azure-Portal mit einem Konto an, das über Benutzeradministratorberechtigungen in der Organisation verfügt.
2. Klicken Sie im Navigationsbereich auf **Azure Active Directory**.
3. Wählen Sie unter **Verwalten** die Option **Benutzer** aus.
4. Wählen Sie unter **Anzeigen** die Option **Nur Gastbenutzer** aus, und überprüfen Sie, ob die hinzugefügten Benutzer aufgelistet werden.

### <a name="view-guest-users-with-powershell"></a>Anzeigen von Gastbenutzern mit PowerShell

Führen Sie den folgenden Befehl aus:

```powershell
 Get-AzureADUser -Filter "UserType eq 'Guest'"
```

Die von Ihnen eingeladenen Benutzer sollten mit einem Benutzerprinzipalnamen (UPN) im Format *E-Mail-Adresse*#EXT#\@*Domäne* angezeigt werden. Beispiel: *lstokes_fabrikam.com#EXT#\@contoso.onmicrosoft.com*, wobei „contoso.onmicrosoft.com“ der Organisation entspricht, für die Sie die Einladungen gesendet haben.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Testbenutzerkonten nicht mehr benötigen, können Sie sie im Azure-Portal auf der Seite „Benutzer“ aus dem Verzeichnis löschen. Aktivieren Sie dazu das Kontrollkästchen neben dem Gastbenutzer, und wählen Sie dann **Löschen** aus. 

Sie können aber auch den folgenden PowerShell-Befehl ausführen, um ein Benutzerkonto zu löschen:

```powershell
 Remove-AzureADUser -ObjectId "<UPN>"
```

Beispiel: `Remove-AzureADUser -ObjectId "lstokes_fabrikam.com#EXT#@contoso.onmicrosoft.com"`

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Masseneinladungen an Gastbenutzer außerhalb Ihrer Organisation gesendet. Als Nächstes erfahren Sie, wie Einladungen eingelöst werden.

> [!div class="nextstepaction"]
> [Azure Active Directory B2B-Zusammenarbeit: Einlösen von Einladungen](redemption-experience.md)
