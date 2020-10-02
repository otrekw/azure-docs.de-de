---
title: Einrichten eines Classroom-Labs in Azure Lab Services | Microsoft-Dokumentation
description: In diesem Tutorial richten Sie unter Verwendung von Azure Lab Services ein Classroom-Lab mit virtuellen Computern für Kursteilnehmer ein.
ms.topic: tutorial
ms.date: 06/26/2020
ms.openlocfilehash: 1155646d31ddb8a0a3abce025acde5c4cb645f54
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91336733"
---
# <a name="tutorial-set-up-a-classroom-lab"></a>Tutorial: Einrichten eines Classroom-Labs 
In diesem Tutorial richten Sie ein Classroom-Lab mit virtuellen Computern ein, die von den Teilnehmern im Classroom verwendet werden.  

In diesem Tutorial führen Sie die folgenden Aktionen aus:

> [!div class="checklist"]
> * Erstellen eines Classroom-Labs
> * Hinzufügen von Benutzern zum Lab
> * Festlegen eines Zeitplans für das Lab
> * Senden einer Einladungs-E-Mail an Schüler/Studenten

## <a name="prerequisites"></a>Voraussetzungen
In diesem Tutorial richten Sie ein Lab mit virtuellen Computern für Ihre Klasse ein. Zum Einrichten eines Classroom-Labs in einem Labkonto müssen Sie Mitglied einer der folgenden Rollen für das Labkonto sein: „Besitzer“, „Ersteller des Labs“ oder „Mitwirkender“. Das zum Erstellen eines Labkontos verwendete Konto wird automatisch der Rolle „Besitzer“ hinzugefügt. Daher können Sie mit dem Benutzerkonto, das Sie zum Erstellen eines Labkontos verwendet haben, ein Classroom-Lab erstellen. 

Nachfolgend ist der typische Workflow für die Verwendung von Azure Lab Services beschrieben:

1. Der Ersteller eines Labkontos kann andere Benutzer zur Rolle **Ersteller des Labs** hinzufügen. Der Ersteller/Administrator des Labkontos fügt der Rolle **Ersteller des Labs** beispielsweise Lehrkräfte hinzu, damit diese Labs für ihre Kurse erstellen können. 
2. Anschließend erstellen die Lehrkräfte Labs mit virtuellen Computern für ihre Kurse und senden Registrierungslinks an die Kursteilnehmer. 
3. Kursteilnehmer verwenden zum Registrieren für das Lab den Registrierungslink, den sie von Lehrkräften erhalten. Nach der Registrierung können sie virtuelle Computer in den Labs für Unterrichtsarbeit und Hausaufgaben nutzen. 

## <a name="create-a-classroom-lab"></a>Erstellen eines Classroom-Labs
In diesem Schritt erstellen Sie ein Lab für Ihre Klasse in Azure. 

1. Navigieren Sie zur Website [Azure Lab Services](https://labs.azure.com). Beachten Sie den Hinweis, dass Internet Explorer 11 noch nicht unterstützt wird. 
2. Wählen Sie **Anmelden**, und geben Sie Ihre Anmeldeinformationen ein. Azure Lab Services unterstützt Geschäfts-, Schul- oder Unikonten und Microsoft-Konten. 
3. Wählen Sie **Neues Lab** aus. 
    
    ![Screenshot, der „Azure Lab Services“ mit ausgewählter Schaltfläche „Neues Lab“ zeigt.](./media/tutorial-setup-classroom-lab/new-lab-button.png)
4. Führen Sie im Fenster **Neues Lab** die folgenden Aktionen aus: 
    1. Geben Sie unter **Name** einen Namen für Ihr Lab ein, und wählen Sie **Weiter** aus.  

        ![Erstellen eines Classroom-Labs](./media/tutorial-setup-classroom-lab/new-lab-window.png)
    2. Geben Sie auf der Seite mit den **VM-Anmeldeinformationen** die Standardanmeldeinformationen für alle VMs im Lab an. Geben Sie **Name** und **Kennwort** für den Benutzer an, und wählen Sie anschließend **Weiter** aus.  

        ![Fenster „Neues Lab“](./media/tutorial-setup-classroom-lab/virtual-machine-credentials.png)

        > [!IMPORTANT]
        > Notieren Sie sich den Benutzernamen und das Kennwort. Diese Angaben werden nicht noch einmal angezeigt.
    3. Wählen Sie auf der Seite **Labrichtlinien** die Option **Fertig stellen** aus. 

        ![Kontingent pro Benutzer](./media/tutorial-setup-classroom-lab/quota-for-each-user.png)
5. Der folgende Bildschirm sollte angezeigt werden, auf dem der Status der VM-Vorlagenerstellung angegeben ist. Dieser Vorgang dauert bis zu 20 Minuten. 

    ![Status der VM-Vorlagenerstellung](./media/tutorial-setup-classroom-lab/create-template-vm-progress.png)
8. Führen Sie auf der Seite **Vorlage** die folgenden Schritte aus: Diese Schritte sind für das Tutorial **optional**.

    1. Stellen Sie eine Verbindung mit der Vorlage für virtuelle Computer her, indem Sie **Verbinden** wählen. Handelt es sich um einen virtuellen Computer mit Linux-Vorlage, legen Sie fest, ob eine Verbindung per SSH oder RDP (sofern RDP aktiviert ist) hergestellt werden soll.
    3. Installieren und konfigurieren Sie die für Ihre Klasse erforderliche Software auf dem virtuellen Vorlagencomputer. 
    4. **Beenden** Sie den virtuellen Vorlagencomputer.  

    > [!NOTE]
    > Bei Vorlagen-VMs entstehen **Kosten**, wenn sie ausgeführt werden. Stellen Sie daher sicher, dass die Vorlagen-VM heruntergefahren wird, wenn sie nicht ausgeführt werden muss. 

## <a name="publish-the-template-vm"></a>Veröffentlichen der Vorlage für virtuelle Computer
In diesem Schritt veröffentlichen Sie den virtuellen Vorlagencomputer. Wenn Sie den virtuellen Vorlagencomputer veröffentlichen, erstellt Azure Lab Services im Lab mithilfe der Vorlage virtuelle Computer. Alle virtuellen Computer haben die gleiche Konfiguration wie die Vorlage.

1. Wählen Sie auf der Seite **Vorlage** auf der Symbolleiste die Option **Veröffentlichen** aus. 

    ![Schaltfläche „Vorlage veröffentlichen“](./media/tutorial-setup-classroom-lab/template-page-publish-button.png)

    > [!WARNING]
    > Nachdem die Veröffentlichung erfolgt ist, kann sie nicht mehr rückgängig gemacht werden. 
2. Geben Sie auf der Seite **Vorlage veröffentlichen** die Anzahl von virtuellen Computern ein, die Sie im Lab erstellen möchten, und wählen Sie anschließend die Option **Veröffentlichen** aus. 

    ![Vorlage veröffentlichen: Anzahl von VMs](./media/tutorial-setup-classroom-lab/publish-template-number-vms.png)
3. Auf der Seite wird der **Veröffentlichungsstatus** der Vorlage angezeigt. Dieser Vorgang kann bis zu einer Stunde dauern. 

    ![Veröffentlichen der Vorlage – Status](./media/tutorial-setup-classroom-lab/publish-template-progress.png)
4. Warten Sie, bis die Veröffentlichung abgeschlossen ist, und navigieren Sie zur Seite **VM-Pool**, indem Sie die Option **Virtuelle Computer** im linken Menü bzw. die Kachel **Virtuelle Computer** auswählen. Vergewissern Sie sich, dass virtuelle Computer mit dem Status **Nicht zugewiesen** angezeigt werden. Diese virtuellen Computer sind noch keinen Teilnehmern zugewiesen. Sie sollten den Status **Beendet** aufweisen. Auf dieser Seite können Sie einen virtuellen Computer für einen Teilnehmer starten, eine Verbindung damit herstellen und ihn beenden und löschen. Sie können virtuelle Computer auf dieser Seite starten oder sie von Ihren Teilnehmern starten lassen. 

    ![Virtuelle Computer im Status „Beendet“](./media/tutorial-setup-classroom-lab/virtual-machines-stopped.png)   

    > [!NOTE]
    > Wenn ein Lehrer/Dozent eine Kursteilnehmer-VM aktiviert, hat dies keine Auswirkung auf das Kontingent für den Kursteilnehmer. Das Kontingent für einen Benutzer gibt die Anzahl von Labstunden an, die für den Benutzer außerhalb der geplanten Kurszeit verfügbar sind. Weitere Informationen zu Kontingenten finden Sie unter [Festlegen von Kontingenten für Benutzer](how-to-configure-student-usage.md?#set-quotas-for-users).

## <a name="set-a-schedule-for-the-lab"></a>Festlegen eines Zeitplans für das Lab
Erstellen Sie ein geplantes Ereignis für das Lab, damit VMs im Lab zu bestimmten Zeiten automatisch gestartet und beendet werden. Das von Ihnen zuvor angegebene Benutzerkontingent (Standard: zehn Stunden) ist die zusätzliche Zeit, die jedem Benutzer außerhalb dieser geplanten Zeit zugewiesen ist. 

1. Wechseln Sie zur Seite **Zeitpläne**, und wählen Sie in der Symbolleiste die Option **Add scheduled event** (Geplantes Ereignis hinzufügen) aus. 

    ![Screenshot, der die Schaltfläche „Geplantes Ereignis hinzufügen“ auf der Seite „Zeitpläne“ zeigt.](./media/how-to-create-schedules/add-schedule-button.png)
2. Führen Sie auf der Seite **Add scheduled event** (Geplantes Ereignis hinzufügen) die folgenden Schritte aus:
    1. Vergewissern Sie sich, dass unter **Ereignistyp** die Option **Standard** ausgewählt ist.  
    2. Wählen Sie das **Startdatum** für die Klasse aus. 
    4. Wählen Sie die **Startzeit** aus, zu der die virtuellen Computer gestartet werden sollen.
    5. Wählen Sie die **Endzeit** aus, zu der die virtuellen Computer heruntergefahren werden sollen. 
    6. Wählen Sie die **Zeitzone** für die von Ihnen angegebene Start- und Endzeit aus. 
3. Wählen Sie auf der gleichen Seite (**Add scheduled event** (Geplantes Ereignis hinzufügen)) im Abschnitt **Wiederholen** den aktuellen Zeitplan aus.  

    ![Schaltfläche „Zeitplan hinzufügen“ auf der Seite „Zeitpläne“](./media/how-to-create-schedules/select-current-schedule.png)
5. Führen Sie im Dialogfeld **Wiederholen** die folgenden Schritte aus:
    1. Vergewissern Sie sich, dass für das Feld **Wiederholen** die Option **Wöchentlich** festgelegt ist. 
    2. Wählen Sie die Tage aus, an denen der Zeitplan gelten soll. Im folgenden Beispiel sind Montag bis Freitag ausgewählt. 
    3. Wählen Sie ein **Enddatum** für den Zeitplan aus.
    8. Wählen Sie **Speichern** aus. 

        ![Festlegen des Zeitplans für die Wiederholung](./media/how-to-create-schedules/set-repeat-schedule.png)

3. Geben Sie nun auf der Seite **Add scheduled event** (Geplantes Ereignis hinzufügen) unter **Hinweise (optional)** ein beliebige Beschreibung bzw. Hinweise zum Zeitplan ein. 
4. Wählen Sie auf der Seite **Add scheduled event** (Geplantes Ereignis hinzufügen) die Option **Speichern** aus. 

    ![Wöchentlicher Zeitplan](./media/how-to-create-schedules/add-schedule-page-weekly.png)
5. Navigieren Sie im Kalender zum Startdatum, um sich zu vergewissern, dass der Zeitplan festgelegt wurde.
    
    ![Zeitplans im Kalender](./media/how-to-create-schedules/schedule-calendar.png)

    Weitere Informationen zum Erstellen und Verwalten von Zeitplänen für eine Klasse finden Sie unter [Erstellen und Verwalten von Zeitplänen für Classroom-Labs in Azure Lab Services](how-to-create-schedules.md).


## <a name="add-users-to-the-lab"></a>Hinzufügen von Benutzern zum Lab

1. Klicken Sie im linken Menü auf **Benutzer**. Die Option **Zugriff beschränken** ist standardmäßig aktiviert. Wenn diese Einstellung aktiviert ist, kann sich ein Benutzer auch dann nicht beim Lab registrieren, wenn er über den Registrierungslink verfügt. Um sich zu registrieren, muss er in der Liste der Benutzer enthalten sein. Nur Benutzer in der Liste können sich über den von Ihnen gesendeten Registrierungslink beim Lab registrieren. In diesem Verfahren fügen Sie der Liste Benutzer hinzu. Alternativ können Sie **Zugriff beschränken** deaktivieren. In diesem Fall können sich Benutzer beim Lab registrieren, wenn sie über den Registrierungslink verfügen. 
2. Wählen Sie auf der Symbolleiste die Option **Benutzer hinzufügen** und anschließend **Per E-Mail-Adresse hinzufügen** aus. 

    ![Schaltfläche „Benutzer hinzufügen“](./media/how-to-configure-student-usage/add-users-button.png)
1. Geben Sie auf der Seite **Benutzer hinzufügen** die E-Mail-Adressen von Benutzern in separaten Zeilen oder durch Semikolons getrennt in einer einzelnen Zeile ein. 

    ![Hinzufügen der E-Mail-Adressen von Benutzern](./media/how-to-configure-student-usage/add-users-email-addresses.png)
4. Wählen Sie **Speichern** aus. In der Liste werden die E-Mail-Adressen und der Status (registriert oder nicht registriert) von Benutzern angezeigt. 

    ![Benutzerliste](./media/how-to-configure-student-usage/users-list-new.png)

    Nach der Registrierung für das Lab werden die Namen von Benutzern in der Liste angezeigt. 
    

## <a name="send-invitation-emails-to-users"></a>Senden von Einladungs-E-Mails an Benutzer

1. Wechseln Sie zur Ansicht **Benutzer**, falls Sie sich nicht bereits auf der Seite befinden, und wählen Sie in der Symbolleiste die Option **Invite all** (Alle einladen) aus. 

    ![Auswählen von Kursteilnehmern](./media/tutorial-setup-classroom-lab/invite-all-button.png)
1. Geben Sie auf der Seite **Send invitation by email** (Einladung per E-Mail senden) eine optionale Nachricht ein, und wählen Sie anschließend die Option **Senden** aus. Die E-Mail enthält automatisch den Registrierungslink. Sie erhalten diesen Registrierungslink, indem Sie in der Symbolleiste die Option **...** (Auslassungszeichen) und dann **Registrierungslink** auswählen. 

    ![Senden eines Registrierungslinks per E-Mail](./media/tutorial-setup-classroom-lab/send-email.png)
4. Der Status der **Einladung** wird in der Liste **Benutzer** angezeigt. Der Status sollte sich in **Wird gesendet** und dann in **Gesendet am &lt;Datum&gt;** ändern. 

    Weitere Informationen zum Hinzufügen von Kursteilnehmern zu einer Klasse sowie zum Verwalten der Verwendung des Labs finden Sie unter [Hinzufügen und Verwalten von Labbenutzern](how-to-configure-student-usage.md).

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie ein Lab für Ihre Klasse in Azure erstellt. Um zu erfahren, wie ein Teilnehmer auf einen virtuellen Computer im Labor über den Registrierungslink zugreifen kann, fahren Sie mit nächsten Tutorial fort:

> [!div class="nextstepaction"]
> [Herstellen einer Verbindung mit einem virtuellen Computer im Classroom-Lab](tutorial-connect-virtual-machine-classroom-lab.md)

