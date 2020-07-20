---
title: Konfigurieren von Nutzungseinstellungen in Classroom-Labs von Azure Lab Services
description: Erfahren Sie, wie Sie die Anzahl der Kursteilnehmer für ein Lab konfigurieren, sie beim Lab registrieren, die Anzahl der Stunden steuern, in denen sie den virtuellen Computer verwenden können, und vieles mehr.
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: 229147891d1456d7b7b11fd5297755842d85e600
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85445949"
---
# <a name="add-and-manage-lab-users"></a>Hinzufügen und Verwalten von Labbenutzern

In diesem Artikel wird beschrieben, wie Sie einem Lab Kursteilnehmer hinzufügen, diese beim Lab registrieren, die Anzahl der zusätzlichen Stunden steuern, für die sie den virtuellen Computer (VM) verwenden können, und vieles mehr. 

## <a name="add-users-to-a-lab"></a>Benutzer zu einem Lab hinzufügen

In diesem Abschnitt fügen Sie einem Lab Kursteilnehmer manuell oder durch Hochladen einer CSV-Datei hinzu. Gehen Sie folgendermaßen vor:

1. Wählen Sie im linken Bereich die Option **Benutzer**. 

    Standardmäßig ist die Option **Zugriff einschränken** aktiviert. Wenn sie nicht in der Benutzerliste enthalten sind, können sich Kursteilnehmer daher selbst dann nicht beim Labor registrieren, wenn sie über einen Registrierungslink verfügen. Nur in der Liste enthaltene Benutzer können sich über den von Ihnen gesendeten Registrierungslink beim Lab registrieren. In diesem Verfahren fügen Sie der Liste Benutzer hinzu. Alternativ können Sie **Zugriff beschränken** deaktivieren. In diesem Fall können sich Kursteilnehmer beim Lab registrieren, wenn sie über den Registrierungslink verfügen. 

1. Wählen Sie oben im Bereich **Benutzer** die Option **Benutzer hinzufügen** und anschließend **Per E-Mail-Adresse hinzufügen** aus. 

    ![Schaltfläche „Benutzer hinzufügen“](./media/how-to-configure-student-usage/add-users-button.png)

1. Geben Sie auf der Seite **Benutzer hinzufügen** die E-Mail-Adressen der Kursteilnehmer in separaten Zeilen oder durch Semikolons getrennt in einer einzelnen Zeile ein. 

    ![Hinzufügen der E-Mail-Adressen von Benutzern](./media/how-to-configure-student-usage/add-users-email-addresses.png)

1. Wählen Sie **Speichern** aus. 

    In der Liste werden die E-Mail-Adressen und Status der aktuellen Benutzer angezeigt, und zwar unabhängig davon, ob Sie beim Lab registriert sind oder nicht. 

    ![Benutzerliste](./media/how-to-configure-student-usage/list-of-added-users.png)

    > [!NOTE]
    > Nachdem die Kursteilnehmer beim Lab registriert wurden, werden ihre Namen in der Liste angezeigt. Der in der Liste angezeigte Name wird aus Vor- und Nachnamen der Kursteilnehmer in Azure Active Directory zusammengesetzt. 

### <a name="add-users-by-uploading-a-csv-file"></a>Hinzufügen von Benutzern durch Hochladen einer CSV-Datei

Sie können auch eine CSV-Datei mit E-Mail-Adressen von Benutzern hochladen, um Benutzer hinzuzufügen.

1. Erstellen Sie in Microsoft Excel eine CSV-Datei, die die E-Mail-Adressen der Kursteilnehmer in einer Spalte auflistet.

    ![Liste der Benutzer in einer CSV-Datei](./media/how-to-configure-student-usage/csv-file-with-users.png)

1. Wählen Sie oben im Bereich **Benutzer** die Option **Benutzer hinzufügen** und anschließend **CSV hochladen** aus.

    ![Schaltfläche „CSV hochladen“](./media/how-to-configure-student-usage/upload-csv-button.png)

1. Wählen Sie die CSV-Datei aus, die die E-Mail-Adressen der Kursteilnehmer enthält, und wählen Sie dann **Öffnen** aus.

    Im Fenster **Benutzer hinzufügen** wird die Liste der E-Mail-Adressen aus der CSV-Datei angezeigt. 

    ![Fenster „Benutzer hinzufügen“ mit E-Mail-Adressen aus einer CSV-Datei](./media/how-to-configure-student-usage/add-users-window.png)

1. Wählen Sie **Speichern** aus. 

1. Zeigen Sie im Bereich **Benutzer** die Liste der hinzugefügten Kursteilnehmer an. 

    ![Liste der hinzugefügten Benutzer im Bereich „Benutzer“](./media/how-to-configure-student-usage/list-of-added-users.png)

## <a name="send-invitations-to-users"></a>Senden von Einladungen an Benutzer

Um einen Registrierungslink an neue Benutzer zu senden, verwenden Sie eine der folgenden Methoden. 

Wenn die Option **Zugriff einschränken** für das Lab aktiviert ist, können sich nur in der Liste enthaltene Benutzer über den Registrierungslink beim Lab registrieren. Diese Option ist standardmäßig aktiviert. 

### <a name="invite-all-users"></a>Einladen aller Benutzer

Diese Methode zeigt, wie Sie E-Mails mit dem Registrierungslink und einer optionalen Nachricht an alle aufgelisteten Kursteilnehmer senden können.

1. Wählen Sie im Bereich **Benutzer** die Option **Alle einladen** aus. 

    ![Schaltfläche „Alle einladen“](./media/tutorial-setup-classroom-lab/invite-all-button.png)

1. Geben Sie im Fenster **Einladung per E-Mail senden** eine optionale Nachricht ein, und wählen Sie anschließend die Option **Senden** aus. 

    Die E-Mail enthält automatisch den Registrierungslink. Zum separaten Aktivieren und Speichern des Registrierungslinks wählen Sie die Auslassungspunkte ( **...** ) am oberen Rand des Bereichs **Benutzer** aus, und wählen Sie dann **Registrierungslink** aus. 

    ![Fenster „Registrierungslink per E-Mail senden“](./media/tutorial-setup-classroom-lab/send-email.png)

    In der Spalte **Einladung** der Liste **Benutzer** wird der Einladungsstatus für jeden hinzugefügten Benutzer angezeigt. Der Status sollte sich in **Wird gesendet** und dann in **Gesendet am \<date>** ändern. 

### <a name="invite-selected-users"></a>Einladen ausgewählter Benutzer

Diese Methode zeigt Ihnen, wie Sie nur bestimmte Kursteilnehmer einladen und einen Registrierungslink abrufen, den Sie für andere Personen freigeben können.

1. Wählen Sie im Bereich **Benutzer** einen Kursteilnehmer oder mehrere Kursteilnehmer in der Liste aus. 

1. Wählen Sie in der Zeile für den ausgewählten Kursteilnehmer das **Briefumschlagsymbol** aus, oder wählen Sie auf der Symbolleiste **Einladen** aus. 

    ![Einladen ausgewählter Benutzer](./media/how-to-configure-student-usage/invite-selected-users.png)

1. Geben Sie im Fenster **Send invitation by email** (Einladung per E-Mail senden) eine optionale **Nachricht** ein, und wählen Sie anschließend die Option **Senden** aus. 

    ![Senden einer E-Mail an ausgewählte Benutzer](./media/how-to-configure-student-usage/send-invitation-to-selected-users.png)

    Im Bereich **Benutzer** wird der Status dieses Vorgangs in der Spalte **Einladung** der Tabelle angezeigt. Die Einladungs-E-Mail enthält den Registrierungslink, über den sich Kursteilnehmer beim Lab registrieren können.

## <a name="get-the-registration-link"></a>Abrufen des Registrierungslinks

In diesem Abschnitt können Sie den Registrierungslink über das Portal abrufen und mithilfe Ihrer eigenen E-Mail-Anwendung senden. 

1. Wählen Sie im Bereich **Benutzer** die Option **Registrierungslink** aus.

    ![Registrierungslink für Teilnehmer](./media/how-to-configure-student-usage/registration-link-button.png)

1. Wählen Sie im Fenster **Benutzerregistrierung** die Option **Kopieren** aus, und klicken Sie dann auf **Fertig**. 

    ![Fenster „Benutzerregistrierung“](./media/how-to-configure-student-usage/registration-link.png)

    Der Link wird in die Zwischenablage kopiert. 
    
1. Fügen Sie in Ihrer E-Mail-Anwendung den Registrierungslink ein, und senden Sie die E-Mail an einen Kursteilnehmer, damit sich der Kursteilnehmer für den Kurs registrieren kann. 

## <a name="view-registered-users"></a>Anzeigen registrierter Benutzer

1. Navigieren Sie zur Website [Azure Lab Services](https://labs.azure.com). 
1. Wählen Sie **Anmelden** aus, und geben Sie dann Ihre Anmeldeinformationen ein. Azure Lab Services unterstützt Geschäfts-, Schul- oder Unikonten und Microsoft-Konten.
1. Wählen Sie auf der Seite **Meine Labs** das Lab aus, dessen Nutzung Sie nachverfolgen möchten. 
1. Wählen Sie im linken Bereich **Benutzer** aus, oder wählen Sie die Kachel **Benutzer** aus. 

    Im Bereich **Benutzer** wird eine Liste von Kursteilnehmern angezeigt, die sich bei Ihrem Lab registriert haben.  

    ![Liste der registrierten Benutzer](./media/tutorial-track-usage/registered-users.png)

## <a name="set-quotas-for-users"></a>Festlegen von Kontingenten für Benutzer

Sie können ein Stundenkontingent für jeden Kursteilnehmer festlegen, indem Sie folgendermaßen vorgehen: 

1. Wählen Sie im Bereich **Benutzer** auf der Symbolleiste die Option **Kontingent pro Benutzer: \<number> Stunden** aus. 
1. Geben Sie im Fenster **Kontingent pro Benutzer** die Anzahl der Stunden an, die Sie jedem Kursteilnehmer außerhalb der planmäßigen Kursstunden zuteilen möchten, und wählen Sie dann **Speichern** aus.

    ![Fenster „Kontingent pro Benutzer“](./media/how-to-configure-student-usage/quota-per-user.png)    

    Die geänderten Werte werden jetzt auf der Schaltfläche **Kontingent pro Benutzer: \<number of hours>** auf der Symbolleiste und in der Benutzerliste angezeigt, wie hier gezeigt:

    ![Kontingent Stunden pro Benutzer](./media/how-to-configure-student-usage/quot-per-user-after.png)

    > [!IMPORTANT]
    > Die [geplante Ausführungszeit von VMs](how-to-create-schedules.md) wird nicht mit dem Kontingent eines Kursteilnehmers verrechnet. Das Kontingent gilt für die Zeiten außerhalb der geplanten Stunden, die ein Kursteilnehmer mit VMs verbringt. 

## <a name="set-additional-quotas-for-specific-users"></a>Festlegen eines zusätzlichen Kontingents für bestimmte Benutzer

Sie können Kontingente für bestimmte Kursteilnehmer angeben, die über die allgemeinen Kontingente hinausgehen, die für alle Benutzer im vorherigen Abschnitt festgelegt wurden. Wenn Sie beispielsweise als Dozent das Kontingent für alle Kursteilnehmer auf 10 Stunden und ein zusätzliches Kontingent von 5 Stunden für einen bestimmten Kursteilnehmer festlegen, erhält dieser Kursteilnehmer ein Kontingent von 15 (10 + 5) Stunden. Wenn Sie das allgemeine Kontingent später z. B. in den Wert 15 ändern, erhält der Kursteilnehmer ein Kontingent von 20 (15 + 5) Stunden. Beachten Sie, dass dieses Gesamtkontingent außerhalb der geplanten Zeit liegt. Die Zeit, die ein Kursteilnehmer während der planmäßigen Kursstunden an einer Lab-VM verbringt, wird nicht auf dieses Kontingent angerechnet. 

Gehen Sie folgendermaßen vor, um zusätzliche Kontingente festzulegen:

1. Wählen Sie im Bereich **Benutzer** einen Kursteilnehmer aus der Liste aus, und wählen Sie dann auf der Symbolleiste **Kontingent anpassen** aus. 

    ![Schaltfläche „Kontingent anpassen“](./media/how-to-configure-student-usage/adjust-quota-button.png)

1. Geben Sie unter **Kontingent anpassen für \<selected user or users email address>** die Anzahl zusätzlicher Lab-Stunden ein, die Sie den ausgewählten Kursteilnehmern gewähren möchten, und wählen Sie dann **Anwenden** aus. 

    ![Fenster „Kontingent anpassen“](./media/how-to-configure-student-usage/additional-quota.png)

    In der Spalte **Nutzung** wird das aktualisierte Kontingent für die ausgewählten Kursteilnehmer angezeigt. 

    ![Neue Nutzung für den Benutzer](./media/how-to-configure-student-usage/new-usage-hours.png)

## <a name="student-accounts"></a>Konten für Kursteilnehmer

Zum Hinzufügen von Kursteilnehmern zu einem Classroom-Lab verwenden Sie deren E-Mail-Konten. Kursteilnehmer können die folgenden Arten von E-Mail-Konten verwenden:

- Ein E-Mail-Konto für Kursteilnehmer, das von der Azure Active Directory-Instanz Ihrer Universität für Office 365 bereitgestellt wird. 
- Ein Microsoft-Domänen-E-Mail-Konto, z. B. *outlook.com*, *hotmail.com*, *msn.com* oder *live.com*.
- Ein Microsoft-fremdes E-Mail-Konto, z. B. ein von Yahoo! oder Google bereitgestelltes Konto. Diese Kontotypen müssen jedoch mit einem Microsoft-Konto verknüpft werden.
- Ein GitHub-Konto. Dieses Konto muss mit einem Microsoft-Konto verknüpft werden.

### <a name="use-a-non-microsoft-email-account"></a>Verwenden eines Microsoft-fremden E-Mail-Kontos
Kursteilnehmer können Microsoft-fremde E-Mail-Konten verwenden, um sich für ein Classroom-Lab zu registrieren und sich bei diesem anzumelden.  Die Registrierung erfordert jedoch, dass sie zunächst ein Microsoft-Konto erstellen, das mit ihrer Microsoft-fremden E-Mail-Adresse verknüpft ist.

Viele Kursteilnehmer verfügen möglicherweise bereits über ein Microsoft-Konto, das mit ihrer Microsoft-fremden E-Mail-Adresse verknüpft ist. Beispielsweise verfügen Kursteilnehmer bereits über ein Microsoft-Konto, wenn sie ihre E-Mail-Adresse mit anderen Microsoft-Produkten oder -Diensten wie Office, Skype, OneDrive oder Windows verwendet haben.  

Wenn Kursteilnehmer den Registrierungslink verwenden, um sich bei einem Classroom anzumelden, werden sie aufgefordert, ihre E-Mail-Adresse und ihr Kennwort einzugeben. Kursteilnehmer, die versuchen, sich mit einem Microsoft-fremden Konto anzumelden, das nicht mit einem Microsoft-Konto verknüpft ist, erhalten die folgende Fehlermeldung: 

![Fehlermeldung bei der Anmeldung](./media/how-to-configure-student-usage/cant-find-account.png)

Hier ist ein Link für Kursteilnehmer, um sich [für ein Microsoft-Konto zu registrieren](http://signup.live.com).  

> [!IMPORTANT]
> Wenn sich Kursteilnehmer bei einem Classroom-Lab anmelden, ist keine Option zum Erstellen eines Microsoft-Kontos verfügbar. Daher wird empfohlen, diesen Registrierungslink (http://signup.live.com ) in die Registrierungs-E-Mail für das Classroom-Lab einzufügen, die Sie an Kursteilnehmer senden, die Microsoft-fremde Konten verwenden.

### <a name="use-a-github-account"></a>Verwenden eines GitHub-Kontos
Kursteilnehmer können auch ein vorhandenes GitHub-Konto verwenden, um sich für ein Classroom-Lab zu registrieren und bei diesem anzumelden. Wenn Kursteilnehmer bereits über ein Microsoft-Konto verfügen, das mit ihrem GitHub-Konto verknüpft ist, können sie sich anmelden und ihr Kennwort angeben, wie im vorherigen Abschnitt gezeigt. 

Wenn sie ihr GitHub-Konto noch nicht mit einem Microsoft-Konto verknüpft haben, können sie folgendermaßen vorgehen:

1. Wählen Sie den Link **Anmeldeoptionen** aus, wie hier gezeigt:

    ![Link „Anmeldeoptionen“](./media/how-to-configure-student-usage/signin-options.png)

1. Wählen Sie im Fenster **Anmeldeoptionen** die Option **Mit GitHub anmelden** aus.

    ![Link „Mit GitHub anmelden“](./media/how-to-configure-student-usage/signin-github.png)

    An der Eingabeaufforderung erstellen die Kursteilnehmer dann ein Microsoft-Konto, das mit ihrem GitHub-Konto verknüpft ist. Die Verknüpfung erfolgt automatisch, wenn sie **Weiter** auswählen. Anschließend werden sie sofort angemeldet und mit dem Classroom-Lab verbunden.

## <a name="export-a-list-of-users-to-a-csv-file"></a>Exportieren einer Liste von Benutzern in eine CSV-Datei

1. Navigieren Sie zum Bereich **Benutzer**.
1. Wählen Sie auf der Symbolleiste die Auslassungspunkte ( **...** ) und dann **CSV exportieren** aus. 

    ![Schaltfläche „CSV exportieren“](./media/how-to-export-users-virtual-machines-csv/users-export-csv.png)


## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie in folgenden Artikeln:

- Für Administratoren: [Erstellen und Verwalten von Lab-Konten](how-to-manage-lab-accounts.md)
- Für Lab-Besitzer: [Erstellen und Verwalten von Labs](how-to-manage-classroom-labs.md) und [Einrichten und Veröffentlichen von Vorlagen](how-to-create-manage-template.md)
- Für Lab-Benutzer: [Zugreifen auf Classroom-Labs](how-to-use-classroom-lab.md)
