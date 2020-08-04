---
title: 'Verwalten von App-Gruppen per Windows Virtual Desktop-Portal: Azure'
description: Es wird beschrieben, wie Sie Windows Virtual Desktop-App-Gruppen mit dem Azure-Portal verwalten.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: tutorial
ms.date: 04/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 5b0269b80cb879923f9f00861494915b6abe103e
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/28/2020
ms.locfileid: "87283400"
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

2.  Suchen Sie nach **Windows Virtual Desktop**, und wählen Sie diese Option aus.

3.  Wählen Sie im Menü links auf der Seite die Option **Anwendungsgruppen** und dann **+ Hinzufügen** aus.

4. Wählen Sie auf der Registerkarte **Grundlagen** die Abonnementgruppe und Ressourcengruppe aus, für die Sie die App-Gruppe erstellen möchten. Sie können auch eine neue Ressourcengruppe erstellen, anstatt eine vorhandene auszuwählen.

5. Wählen Sie den Hostpool, der der Anwendungsgruppe zugeordnet ist, im Dropdownmenü neben **Hostpool** aus.

    >[!NOTE]
    >Sie müssen den Hostpool auswählen, der der Anwendungsgruppe zugeordnet ist. App-Gruppen verfügen über Apps oder Desktops, die über einen Sitzungshost bereitgestellt werden, und Sitzungshosts sind Teil von Hostpools. Die App-Gruppe muss bei der Erstellung einem Hostpool zugeordnet werden.

    > [!div class="mx-imgBorder"]
    > ![Screenshot: Registerkarte „Grundlagen“ im Azure-Portal](media/basics-tab.png)

6. Wählen Sie im Menü links auf der Seite die Option **Hostpools** aus, wenn Sie Ihrem Hostpool Anwendungsgruppen hinzufügen möchten.
   
    Wählen Sie als Nächstes den Namen des Hostpools aus, dem Sie Anwendungsgruppen hinzufügen möchten.
   
    Wählen Sie anschließend im Menü links auf der Seite die Option **Anwendungsgruppen** und dann **+ Hinzufügen** aus.

    Wählen Sie abschließend die Abonnementgruppe und Ressourcengruppe aus, in der Sie die App-Gruppe erstellen möchten. Sie können entweder im Dropdownmenü den Namen einer vorhandenen Ressourcengruppe oder die Option **Neu erstellen** auswählen, um eine neue zu erstellen.

      >[!NOTE]
      >Wenn Sie Ihrem Hostpool Anwendungsgruppen hinzufügen, ist der Hostpool, der zu der Anwendungsgruppe gehört, bereits ausgewählt, weil Sie den Navigationsvorgang von dort aus gestartet haben.
      > 
      > [!div class="mx-imgBorder"]
      >![Screenshot: Registerkarte „Grundlagen“ mit vorausgewähltem Hostpool](media/host-pool-selected.png)

7. Wählen Sie als Anwendungsgruppentyp die Option **RemoteApp** aus, und geben Sie dann einen Namen für Ihre RemoteApp ein.

      > [!div class="mx-imgBorder"]
      > ![Screenshot: Felder für Anwendungsgruppentyp mit Hervorhebung von „RemoteApp“](media/remoteapp-button.png)

8.  Wählen Sie die Registerkarte **Zuweisungen** aus.

9.  Wählen Sie **+ Add Azure AD users or user groups** (+ Azure AD-Benutzer oder -Benutzergruppen hinzufügen) aus, um einzelne Benutzer oder Benutzergruppen für die App-Gruppe zu veröffentlichen.

10.  Wählen Sie die Anzahl von Benutzern aus, denen Sie die Apps hinzufügen möchten. Sie können einzelne oder mehrere Benutzer und Benutzergruppen auswählen.

     > [!div class="mx-imgBorder"]
     > ![Screenshot: Menü für Benutzerauswahl](media/select-users.png)

11.  Wählen Sie **Auswählen**.

12.  Wählen Sie die Registerkarte **Anwendungen** und dann **+ Anwendungen hinzufügen** aus.

13.  Fügen Sie wie folgt eine Anwendung über das Startmenü hinzu: 

      - Navigieren Sie zu **Anwendungsquelle**, und wählen Sie im Dropdownmenü **Startmenü** aus. Navigieren Sie als Nächstes zu **Anwendung**, und wählen Sie im Dropdownmenü die Anwendung aus.

     > [!div class="mx-imgBorder"]
     > ![Screenshot: Bildschirm zum Hinzufügen einer Anwendung mit Auswahl von „Startmenü“](media/add-app-start.png)

      - Geben Sie unter **Anzeigename** den Namen für die Anwendung ein, der dem Benutzer auf dem Client angezeigt wird.

      - Lassen Sie die anderen Optionen unverändert, und wählen Sie **Speichern** aus.

14. Fügen Sie eine Anwendung über den jeweiligen Dateipfad wie folgt hinzu:

      - Navigieren Sie zu **Anwendungsquelle**, und wählen Sie im Dropdownmenü **Dateipfad** aus.

      - Geben Sie den Pfad zur Anwendung auf dem Sitzungshost ein, der beim zugeordneten Hostpool registriert ist.

      - Geben Sie die Details der Anwendung in die Felder **Anwendungsname**, **Anzeigename**, **Symbolpfad** und **Symbolindex** ein.

      - Wählen Sie **Speichern** aus.

     > [!div class="mx-imgBorder"]
     > ![Screenshot: Seite „Anwendung hinzufügen“ mit Auswahl von „Dateipfad“](media/add-app-file.png)

     Wiederholen Sie diesen Vorgang für jede Anwendung, die Sie der Anwendungsgruppe hinzufügen möchten.

15.  Wählen Sie als Nächstes die Registerkarte **Arbeitsbereich** aus.

16.  Navigieren Sie zu **Register application group** (Anwendungsgruppe registrieren), und wählen Sie **Ja** aus, wenn Sie die App-Gruppe in einem Arbeitsbereich registrieren möchten. Wählen Sie **Nein** aus, falls Sie die App-Gruppe zu einem späteren Zeitpunkt registrieren möchten.

17.  Bei Auswahl von **Ja** können Sie einen vorhandenen Arbeitsbereich für die Registrierung Ihrer App-Gruppe auswählen.
       
       >[!NOTE]
       >Sie können die App-Gruppe nur in Arbeitsbereichen registrieren, die an demselben Standort wie der Hostpool erstellt werden. Außerdem gilt: Wenn Sie zuvor eine andere App-Gruppe aus demselben Hostpool als Ihre neue App-Gruppe für einen Arbeitsbereich registriert haben, wird sie ausgewählt, und Sie können sie nicht bearbeiten. Alle App-Gruppen aus einem Hostpool müssen unter demselben Arbeitsbereich registriert sein.

     > [!div class="mx-imgBorder"]
     > ![Screenshot: Seite für die Registrierung einer Anwendungsgruppenseite für einen bereits vorhandenen Arbeitsbereich. Der Hostpool ist bereits ausgewählt.](media/register-existing.png)

18. Optional können Sie auch die Registerkarte **Tags** auswählen und Ihre Tagnamen eingeben, falls Sie Tags erstellen möchten, um die Organisation Ihres Arbeitsbereichs zu vereinfachen.

19. Wählen Sie abschließend die Registerkarte **Überprüfen + erstellen** aus.

20. Warten Sie kurz ab, bis der Überprüfungsprozess abgeschlossen ist. Wählen Sie anschließend die Option **Erstellen** aus, um Ihre App-Gruppe bereitzustellen.

Beim Bereitstellungsprozess wird Folgendes für Sie durchgeführt:

- Erstellen der App-Gruppe „RemoteApp“
- Hinzufügen Ihrer ausgewählten Apps zur App-Gruppe
- Veröffentlichen der App-Gruppe für die von Ihnen ausgewählten Benutzer und Benutzergruppen
- Registrieren der App-Gruppe (falls gewünscht)
- Erstellen eines Links zu einer Azure Resource Manager-Vorlage basierend auf Ihrer Konfiguration, die Sie herunterladen und zur späteren Verwendung speichern können

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie erfahren, wie Sie App-Gruppen erstellen, sie mit RemoteApp-Programmen auffüllen und der App-Gruppe Benutzer hinzufügen. Informationen zum Erstellen eines Überprüfungshostpools finden Sie im folgenden Tutorial. Mit einem Überprüfungshostpool können Sie Dienstupdates überwachen, bevor Sie sie in Ihrer Produktionsumgebung bereitstellen.

> [!div class="nextstepaction"]
> [Erstellen eines Hostpools zum Überprüfen von Dienstupdates](./create-validation-host-pool.md)
