---
title: 'Verwalten von App-Gruppen per Windows Virtual Desktop-Portal: Azure'
description: Es wird beschrieben, wie Sie Windows Virtual Desktop-App-Gruppen mit dem Azure-Portal verwalten.
author: Heidilohr
ms.topic: tutorial
ms.date: 10/09/2020
ms.author: helohr
manager: femila
ms.openlocfilehash: 65ca13cba07230a7bf606a398e28761aced14857
ms.sourcegitcommit: 56b0c7923d67f96da21653b4bb37d943c36a81d6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2021
ms.locfileid: "106446008"
---
# <a name="tutorial-manage-app-groups-with-the-azure-portal"></a>Tutorial: Verwalten von App-Gruppen mit dem Azure-Portal

>[!IMPORTANT]
>Dieser Inhalt gilt für Windows Virtual Desktop mit Windows Virtual Desktop-Objekten für Azure Resource Manager. Wenn Sie Windows Virtual Desktop (klassisch) ohne Azure Resource Manager-Objekte verwenden, finden Sie weitere Informationen in [diesem Artikel](./virtual-desktop-fall-2019/manage-app-groups-2019.md).

Die Standard-App-Gruppe, die für einen neuen Windows Virtual Desktop-Hostpool erstellt wird, veröffentlicht auch den vollständigen Desktop. Zusätzlich können Sie RemoteApp-Anwendungsgruppen für den Hostpool erstellen. In diesem Tutorial erfahren Sie, wie Sie eine RemoteApp-Gruppe erstellen und individuelle Startmenü-Apps veröffentlichen.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer RemoteApp-Gruppe
> * Gewähren von Zugriff auf RemoteApp-Programme

## <a name="create-a-remoteapp-group"></a>Erstellen einer RemoteApp-Gruppe

Falls Sie bereits einen Hostpool und Sitzungshost-VMs erstellt haben, indem Sie das Azure-Portal oder PowerShell verwendet haben, können Sie mit dem folgenden Prozess über das Azure-Portal Anwendungsgruppen hinzufügen:

1.  Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
   
    >[!NOTE]
    > Wenn Sie sich beim US Gov-Portal anmelden, wechseln Sie stattdessen zu [https://portal.azure.us/](https://portal.azure.us/).

2.  Suchen Sie nach **Windows Virtual Desktop**, und wählen Sie diese Option aus.

3. Sie können eine Anwendungsgruppe direkt oder aus einem vorhandenen Hostpool hinzufügen. Wählen Sie eine der folgenden Option aus:

    - Wählen Sie im Menü links auf der Seite die Option **Anwendungsgruppen** und dann **+ Hinzufügen** aus.

    - Wählen Sie im Menü links auf dem Bildschirm **Hostpools** und den Namen des Hostpools aus. Wählen Sie dann im Menü auf der linken Seite **Anwendungsgruppen** und anschließend **+ Hinzufügen** aus. In diesem Fall ist der Hostpool auf der Registerkarte „Grundlagen“ bereits ausgewählt.

4. Wählen Sie auf der Registerkarte **Grundlagen** das **Abonnement** und die **Ressourcengruppe** aus, für die Sie die App-Gruppe erstellen möchten. Sie können auch eine neue Ressourcengruppe erstellen, anstatt eine vorhandene auszuwählen.

5. Wählen Sie den **Hostpool**, der der Anwendungsgruppe zugeordnet ist, im Dropdownmenü aus.

    >[!NOTE]
    >Sie müssen den Hostpool auswählen, der der Anwendungsgruppe zugeordnet ist. App-Gruppen verfügen über Apps oder Desktops, die über einen Sitzungshost bereitgestellt werden, und Sitzungshosts sind Teil von Hostpools. Die App-Gruppe muss bei der Erstellung einem Hostpool zugeordnet werden.

    > [!div class="mx-imgBorder"]
    > ![Screenshot: Registerkarte „Grundlagen“ im Azure-Portal](media/basics-tab.png)

6. Wählen Sie unter **Anwendungsgruppentyp** die Option **RemoteApp** aus, und geben Sie dann einen Namen für Ihre RemoteApp ein.

      > [!div class="mx-imgBorder"]
      > ![Screenshot: Felder für Anwendungsgruppentyp mit Hervorhebung von „RemoteApp“](media/remoteapp-button.png)

7.  Klicken Sie auf **Weiter: Zuweisungen >** .

8.  Wählen Sie **+ Add Azure AD users or user groups** (+ Azure AD-Benutzer oder -Benutzergruppen hinzufügen) aus, um der App-Gruppe einzelne Benutzer oder Benutzergruppen zuzuweisen.

9.  Wählen Sie die Benutzer aus, die Zugriff auf die Apps haben sollen. Sie können einzelne oder mehrere Benutzer und Benutzergruppen auswählen.

     > [!div class="mx-imgBorder"]
     > ![Screenshot: Menü für Benutzerauswahl](media/select-users.png)

10.  Wählen Sie **Auswählen**.

11.  Klicken Sie auf **Weiter: Anwendungen >** , und wählen Sie dann **+Anwendungen hinzufügen** aus.

12.  Fügen Sie wie folgt eine Anwendung über das Startmenü hinzu:

      - Wählen Sie unter **Anwendungsquelle** im Dropdownmenü **Startmenü** aus. Wählen Sie als Nächstes unter **Anwendung** im Dropdownmenü die Anwendung aus.

     > [!div class="mx-imgBorder"]
     > ![Screenshot: Bildschirm zum Hinzufügen einer Anwendung mit Auswahl von „Startmenü“](media/add-app-start.png)

      - Geben Sie unter **Anzeigename** den Namen für die Anwendung ein, der dem Benutzer auf dem Client angezeigt wird.

      - Lassen Sie die anderen Optionen unverändert, und wählen Sie **Speichern** aus.

13.  So fügen Sie eine Anwendung über den jeweiligen Dateipfad hinzu:

      - Wählen Sie unter **Anwendungsquelle** im Dropdownmenü **Dateipfad** aus.

      - Geben Sie unter **Anwendungspfad** den Pfad zur Anwendung auf dem Sitzungshost ein, der beim zugeordneten Hostpool registriert ist.

      - Geben Sie die Details der Anwendung in die Felder **Anwendungsname**, **Anzeigename**, **Symbolpfad** und **Symbolindex** ein.

      - Wählen Sie **Speichern** aus.

     > [!div class="mx-imgBorder"]
     > ![Screenshot: Seite „Anwendung hinzufügen“ mit Auswahl von „Dateipfad“](media/add-app-file.png)

14.  Wiederholen Sie diesen Vorgang für jede Anwendung, die Sie der Anwendungsgruppe hinzufügen möchten.

15.  Wählen Sie dann **Weiter: Arbeitsbereich >** aus.

16.  Wenn Sie die App-Gruppe in einem Arbeitsbereich registrieren möchten, wählen Sie **Ja** für **Register application group** (Anwendungsgruppe registrieren) aus. Wählen Sie **Nein** aus, falls Sie die App-Gruppe zu einem späteren Zeitpunkt registrieren möchten.

17.  Bei Auswahl von **Ja** können Sie einen vorhandenen Arbeitsbereich für die Registrierung Ihrer App-Gruppe auswählen.

       >[!NOTE]
       >Sie können die App-Gruppe nur in Arbeitsbereichen registrieren, die an demselben Standort wie der Hostpool erstellt werden. Außerdem gilt: Wenn Sie zuvor eine andere App-Gruppe aus demselben Hostpool als Ihre neue App-Gruppe für einen Arbeitsbereich registriert haben, wird sie ausgewählt, und Sie können sie nicht bearbeiten. Alle App-Gruppen aus einem Hostpool müssen unter demselben Arbeitsbereich registriert sein.

     > [!div class="mx-imgBorder"]
     > ![Screenshot: Seite für die Registrierung einer Anwendungsgruppenseite für einen bereits vorhandenen Arbeitsbereich. Der Hostpool ist bereits ausgewählt.](media/register-existing.png)

18.  Falls Sie Tags erstellen möchten, um die Organisation Ihres Arbeitsbereichs zu vereinfachen, können Sie optional **Weiter: Tags >** auswählen und Ihre Tagnamen eingeben.

19.  Wählen Sie abschließend **Überprüfen + erstellen** aus.

20.  Warten Sie kurz ab, bis der Überprüfungsprozess abgeschlossen ist. Wählen Sie anschließend die Option **Erstellen** aus, um Ihre App-Gruppe bereitzustellen.

Beim Bereitstellungsprozess wird Folgendes für Sie durchgeführt:

- Erstellen der App-Gruppe „RemoteApp“
- Hinzufügen Ihrer ausgewählten Apps zur App-Gruppe
- Veröffentlichen der App-Gruppe für die von Ihnen ausgewählten Benutzer und Benutzergruppen
- Registrieren der App-Gruppe (falls gewünscht)
- Erstellen eines Links zu einer Azure Resource Manager-Vorlage basierend auf Ihrer Konfiguration, die Sie herunterladen und zur späteren Verwendung speichern können

>[!IMPORTANT]
>Für jeden Azure Active Directory-Mandanten können nur 200 Anwendungsgruppen erstellt werden. Dieser Grenzwert wurde aufgrund von Diensteinschränkungen beim Abrufen von Feeds für unsere Benutzer hinzugefügt. Er gilt nicht für App-Gruppen, die in Windows Virtual Desktop (klassisch) erstellt wurden.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie erfahren, wie Sie App-Gruppen erstellen, sie mit RemoteApp-Programmen auffüllen und der App-Gruppe Benutzer hinzufügen. Informationen zum Erstellen eines Überprüfungshostpools finden Sie im folgenden Tutorial. Mit einem Überprüfungshostpool können Sie Dienstupdates überwachen, bevor Sie sie in Ihrer Produktionsumgebung bereitstellen.

> [!div class="nextstepaction"]
> [Erstellen eines Hostpools zum Überprüfen von Dienstupdates](./create-validation-host-pool.md)
