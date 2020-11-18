---
title: Erste Schritte mit Azure Lab Services
description: Dieser Artikel enthält eine Einführung in die Verwendung von Azure Lab Services.
ms.topic: article
ms.date: 10/02/2020
ms.openlocfilehash: 33e052931b0c3bd1bb1434b7eeefeed7a2a7ceab
ms.sourcegitcommit: 0d171fe7fc0893dcc5f6202e73038a91be58da03
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2020
ms.locfileid: "93380058"
---
# <a name="get-started-with-lab-services"></a>Erste Schritte mit Lab Services 

Als Kursteilnehmer können Sie über Azure Lab Services auf branchenübliche Software zugreifen, die für Kurse auf VMs benötigt wird. 

Kursleiter müssen wissen, wie sie Kursteilnehmern in ihren Kursen die Verwendung von Azure Lab Services mit dedizierter Hardware pro Kursteilnehmer vermitteln können.

Dieser Artikel enthält Informationen für Kursleiter dazu, wie sie Zugriff auf Azure Lab Services erhalten, den Dienst verwalten und Kursteilnehmern dessen Verwendung vermitteln können.

## <a name="overview"></a>Übersicht

Was sind VMs, und wie werden sie eingesetzt?

Eine VM (virtual Machine, virtueller Computer) ist eine virtuelle Umgebung, die wie ein Computer eingesetzt wird. VMs verfügen über einen eigenen Prozessor, Arbeitsspeicher und Speicher. VMs stellen einen Ersatz für einen physischen Computer dar und können Benutzern Zugriff auf Betriebssysteme und Software ermöglichen, ohne dass sie diese auf ihrem eigenen Gerät installieren müssen. Azure Lab Services bietet ein Tool für Kursteilnehmer, mit dem sie auf VMs zugreifen und darin navigieren können, und für Kursleiter, um ihre VM-Labs zu verwalten. 

Weitere Informationen finden Sie unter [Erstellen und Verwalten von Classroom-Labs](how-to-manage-classroom-labs.md).

## <a name="lab-dashboards"></a>Lab-Dashboards

Dashboards für Classroom-Labs in Azure Lab Services bieten eine Momentaufnahme der verschiedenen Aspekte eines bestimmten Labs, einschließlich der Informationen zu den VMs, der Anzahl zugewiesener und nicht zugewiesener VMs, der Anzahl registrierter und nicht registrierter Benutzer und der Informationen zu Lab-Zeitplänen. 

> [!NOTE]
> Auch wenn die meisten verwaltungsbezogenen Aspekte des Dashboards und der [Azure Lab Services-Website](https://labs.azure.com/) für Kursleiter sichtbar sind, können sich die für Ihre Rolle spezifischen Berechtigungen auf Ihre Möglichkeiten auswirken, bestimmte Kriterien im Dashboard zu ändern. Wenn Sie ein Problem bei Ihrer speziellen Lab-Einrichtung feststellen, wenden Sie sich an Ihren CTE-Administrator.

:::image type="content" source="./media/use-dashboard/dashboard.png" alt-text="Azure Lab Services-Portal":::

1. Navigieren Sie zur [Azure Lab Services-Website](https://labs.azure.com/), und melden Sie sich dort an.
1. Wählen Sie Ihr Lab aus.
1. Im linken Bereich des Fensters wird ein **Dashboard** angezeigt. Wenn Sie auf **Dashboard** klicken, wird eine Reihe von Kacheln in Ihrem Dashboard angezeigt.
1. Unter der Kachel **Kosten und Abrechnung** werden weitere Kacheln für Vorlagen, VM-Pools, Benutzer und Zeitpläne angezeigt, in denen Sie Aspekte ändern und weitere Details zu dem Classroom-Lab anzeigen können.

    1. Kachel „Vorlage“: Datum, an dem die Vorlage erstellt und an dem sie zuletzt veröffentlicht wurde 
    1. Kachel „VM-Pool“: Anzahl zugewiesener und nicht zugewiesener VMs
    1. Kachel „Benutzer“: Anzahl registrierter Benutzer und der Benutzer, die dem Lab hinzugefügt, aber nicht registriert wurden
    1. Kachel „Zeitpläne“: anstehende geplante Ereignisse für das Lab und Link zum Anzeigen von weiteren Ereignissen

Weitere Informationen finden Sie unter [Verwenden des Dashboards](use-dashboard.md).

## <a name="quota-hours"></a>Kontingentstunden

Kursteilnehmer können während der geplanten Kurszeiten jederzeit auf die VMs zugreifen, ohne dass sich dies auf ihre Kontingentstunden auswirkt. Die Kontingentstunden sind für das gesamte Semester festgelegt und geben die Anzahl der Stunden an, die ein Kursteilnehmer seine VM außerhalb der regulären geplanten Kurszeiten verwenden kann.

8 Stunden pro Woche, Rücksetzung jeweils sonntags – nicht anrechenbar.

Weitere Informationen finden Sie unter [Festlegen von Kontingenten](how-to-configure-student-usage.md#set-quotas-for-users).

### <a name="automatic-shut-down"></a>Automatisches Herunterfahren

Um die Kosten gering zu halten und Kontingentstunden von Kursteilnehmern einzusparen, ist in unseren Labs das automatische Herunterfahren aktiviert. Durch automatisches Herunterfahren werden VMs nach einem bestimmten Zeitraum der Inaktivität (keine Maus- oder Tastatureingaben) abgeschaltet. Das automatische Herunterfahren erfolgt in zwei Phasen. Zunächst wird die Verbindung mit der VM eines Kursteilnehmers nach einem Zeitraum der Inaktivität getrennt. Zu diesem Zeitpunkt wird die VM weiterhin **ausgeführt**, und der Kursteilnehmer kann die Verbindung wiederherstellen. Nach einem weiteren Zeitraum der Inaktivität nach der Unterbrechung der Verbindung schaltet sich die VM selbst ab.

Das automatische Herunterfahren ist ein wichtiges Tool zur Kosteneinsparung, stellt jedoch eine gewisse Beeinträchtigung für Kursteilnehmer beim Speichern ihrer Arbeit und Rendern großer Projektdateien dar. Wenn die Verbindung der Kursteilnehmer häufig getrennt wird oder die VMs zu schnell heruntergefahren werden, wenden Sie sich an Ihren CTE-Administrator. 

Weitere Informationen finden Sie unter [Konfigurieren des automatischen Herunterfahrens von VMs für ein Labkonto](how-to-configure-lab-accounts.md).

## <a name="managing-virtual-machines"></a>Verwalten von virtuellen Maschinen

Das Verwalten des Labs ermöglicht Kursleitern, beispielsweise die Lab-Kapazität (Anzahl der für Kursteilnehmer verfügbaren VMs) zu steuern und VMs manuell zu starten, zu beenden oder zurückzusetzen. Kursleiter können außerdem eine Verbindung mit VMs herstellen, um die Benutzeroberfläche von Kursteilnehmern zu testen, auf Dateien zuzugreifen und Probleme mit der Software oder der VM zu beheben.

Der wichtigste Aspekt bei der Verwaltung unserer VMs ist, dass für eine **ausgeführte** VM immer Kosten anfallen, auch wenn keine Verbindung mit der VM besteht.

### <a name="manually-starting-vms"></a>Manuelles Starten von VMs

1. Auf der Seite **VM-Pool** können Sie alle VMs in einem Lab starten. Dazu klicken Sie oben auf der Seite auf die Schaltfläche **Alle starten**.

    :::image type="content" source="./media/how-to-set-virtual-machine-passwords/start-all-vms-button.png" alt-text="Starten der VMs":::
1. Einzelne VMs können durch Klicken auf die Umschaltfläche „Status“ gestartet werden. 

    Die Umschaltfläche gibt **Wird gestartet** beim Start der VM an und dann **Wird ausgeführt**, nachdem die VM gestartet wurde.
1. Durch Aktivieren der Kontrollkästchen links neben der Spalte **Name** können Sie mehrere VMs auswählen. 

    Klicken Sie nach dem Auswählen der gewünschten VMs oben auf dem Bildschirm auf die Schaltfläche **Starten**.
1. Nach dem Starten können Sie auf die Schaltfläche **Alle beenden** klicken, um alle VMs zu beenden.

    :::image type="content" source="./media/how-to-set-virtual-machine-passwords/stop-all-vms-button.png" alt-text="Beenden der VMs":::

### <a name="stopping-and-resetting-vms"></a>Beenden und Zurücksetzen von VMs

* Sie können einzelne VMs durch Klicken auf die Umschaltfläche „Status“ beenden.
* Außerdem können Sie mehrere VMs durch Aktivieren der entsprechenden Kontrollkästchen und Klicken auf die Schaltfläche „Beenden“ oben auf dem Bildschirm beenden.

Wenn bei einem Kursteilnehmer Probleme beim Herstellen einer Verbindung mit der VM auftreten oder die VM aus einem anderen Grund zurückgesetzt werden muss, können Sie die Funktion „Zurücksetzen“ verwenden.
1. Um einen oder mehrere VMs zurückzusetzen, aktivieren Sie die entsprechenden Kontrollkästchen, und klicken Sie dann oben auf der Seite auf die Schaltfläche **Zurücksetzen**.
1. Klicken Sie im Popupfenster auf **Zurücksetzen**.

    :::image type="content" source="./media/how-to-set-virtual-machine-passwords/reset-vms-dialog.png" alt-text="Zurücksetzen der VMs":::

    > [!NOTE]
    > Das Aktivieren der VM eines Kursteilnehmers wirkt sich nicht auf das Kontingent für den Kursteilnehmer aus. Kontingente für Benutzer geben die Anzahl von Lab-Stunden an, die pro Benutzer außerhalb der geplanten Kurszeiten verfügbar sind.

### <a name="connect-to-virtual-machines"></a>Herstellen einer Verbindung mit VMs

Kursleiter können eine Verbindung mit der VM eines Kursteilnehmers herstellen, wenn die VM eingeschaltet ist und der Kursteilnehmer KEINE Verbindung mit der VM hergestellt hat. Durch Herstellen einer Verbindung mit der VM können Sie auf die lokalen Dateien auf der VM zugreifen und den Kursteilnehmern helfen, Probleme zu beheben.

1. Um eine Verbindung mit der VM eines Kursteilnehmers herzustellen, zeigen Sie mit der Maus in der Liste auf die VM, und klicken Sie dann die Schaltfläche **Verbinden**. 
1. Befolgen Sie dann die Anleitung für die ersten Schritte für Kursteilnehmer für Chromebooks, Macs oder PCs.

:::image type="content" source="./media/how-to-set-virtual-machine-passwords/connect-student-vm.png" alt-text="Schaltfläche „Herstellen einer Verbindung mit Kursteilnehmer-VM“":::

## <a name="add-and-manage-lab-users"></a>Hinzufügen und Verwalten von Labbenutzern

Kursleiter können einem Lab Kursteilnehmer hinzufügen und ihre Stundenkontingente überwachen. 

### <a name="add-users-by-email-address"></a>Hinzufügen von Benutzern per E-Mail-Adresse

1. Klicken Sie auf der [Azure Lab Services-Website](https://labs.azure.com/) im linken Bereich des Fensters auf **Benutzer**.
1. Klicken Sie oben im Fenster auf **Benutzer hinzufügen**, und wählen Sie **Per E-Mail-Adresse hinzufügen** aus. 

    :::image type="content" source="./media/how-to-configure-student-usage/add-users-button.png" alt-text="Schaltfläche „Benutzer hinzufügen“":::
1. Geben Sie im rechts angezeigten Bereich **Benutzer hinzufügen** die E-Mail-Adressen der Kursteilnehmer in separaten Zeilen oder durch Semikolons getrennt in einer einzelnen Zeile ein.
1. Klicken Sie auf **Speichern**.

    :::image type="content" source="./media/get-started-manage-labs/add-students.png" alt-text="Hinzufügen von Kursteilnehmern zum Lab":::
1. Die Liste der Benutzer wird nun mit E-Mail-Adresse, Status, Einladung und Kontingentstunden aktualisiert.

    Nachdem Kursteilnehmer für ein Lab registriert wurden, werden ihre Namen mit den Vor- und Nachnamen aus dem MPS-Verzeichnis aktualisiert.

    > [!NOTE]
    > Behalten Sie für die Umschaltfläche „Zugriff einschränken“ für die Benutzer die Aktivierung bei. Das bedeutet, dass nur aufgeführte Benutzer sich über den von Ihnen gesendeten Registrierungslink beim Lab registrieren können.

### <a name="add-users-using-a-spreadsheet"></a>Hinzufügen von Benutzern mittels einer Tabelle 

Sie können auch eine CSV-Datei mit E-Mail-Adressen von Benutzern hochladen, um Benutzer hinzuzufügen.

1. Erstellen Sie in Microsoft Excel eine CSV-Datei, die die E-Mail-Adressen der Kursteilnehmer in einer Spalte auflistet.
1. Klicken Sie auf der Seite [Azure Lab Services-Website](https://labs.azure.com/) oben auf der Seite **Benutzer** auf die Schaltfläche **Benutzer hinzufügen**.
1. Wählen Sie die Option **CSV hochladen** aus.
1. Wählen Sie die CSV-Datei aus, die die E-Mail-Adressen der Kursteilnehmer enthält, und klicken Sie dann auf **Öffnen**.

    :::image type="content" source="./media/get-started-manage-labs/add-users-spreadsheet.png" alt-text="Hinzufügen von Benutzern mittels einer Tabelle":::
1. Die E-Mail-Adressen werden nun im Fenster im rechten Bereich gezeigt. Klicken Sie auf **Speichern**.

    :::image type="content" source="./media/get-started-manage-labs/register-users.png" alt-text="Registrieren von Benutzern":::

### <a name="register-users"></a>Registrieren von Benutzern

Nachdem dem Lab Benutzer hinzugefügt wurden, müssen diese sich registrieren, um auf die VMs zugreifen zu können. Hierzu können Sie entweder Benutzer über das Azure Web Services-Portal einladen, über das dann eine E-Mail mit dem Registrierungslink für das Lab gesendet wird. Oder Sie können den Registrierungslink kopieren und in eine E-Mail oder ein anderes Kommunikationsmedium mit den Kursteilnehmern einfügen.

1. Wählen Sie auf der Seite **Benutzer** einen oder mehrere Kursteilnehmer in der Liste aus.

    Wählen Sie in der Zeile für den ausgewählten Kursteilnehmer das Briefumschlagsymbol in der Liste aus, oder klicken Sie oben auf dem Bildschirm auf **Einladen**.

    :::image type="content" source="./media/get-started-manage-labs/send-invitation.png" alt-text="Senden einer Einladung":::
    
    Geben Sie im Fenster **Einladung per E-Mail senden** eine optionale Nachricht an die Kursteilnehmer ein (z. B. Benutzername und Kennwort), und klicken Sie dann auf **Senden**. 
    
    :::image type="content" source="./media/get-started-manage-labs/send-invitation-mail.png" alt-text="Senden einer Einladung per E-Mail":::

    Alternativ können Sie auf der Seite **Benutzer** oben auf dem Bildschirm auf die Schaltfläche **Registrierungslink** klicken. 

    :::image type="content" source="./media/get-started-manage-labs/registration-link.png" alt-text="Registrierungslink für Benutzer":::
    
    Kopieren Sie den Registrierungslink aus dem Textfeld, und fügen Sie ihn in einer E-Mail oder Ihrer bevorzugten sicheren Nachrichten-App ein.  
    
    :::image type="content" source="./media/get-started-manage-labs/user-registration.png" alt-text="Senden der Benutzerregistrierung":::

Nachdem Sie die Benutzer über das Azure-Portal eingeladen oder den Link freigegeben haben, können Sie auf der Seite **Benutzer** in der Spalte **Status** sehen, welche Benutzer erfolgreich registriert wurden. 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie Ressourcen, die Sie in diesem Schnellstart erstellt haben, nicht weiterverwenden möchten, sollten Sie sie löschen.

## <a name="next-steps"></a>Nächste Schritte

[Einrichten eines Lab-Kontos](tutorial-setup-lab-account.md)