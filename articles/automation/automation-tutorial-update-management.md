---
title: Verwalten von Updates und Patches für Ihre Azure-VMs in Azure Automation
description: In diesem Artikel erfahren Sie, wie Sie die Updateverwaltung verwenden können, um Updates und Patches für Ihre Azure-VMs zu verwalten.
services: automation
ms.subservice: update-management
ms.topic: conceptual
ms.date: 04/06/2020
ms.custom: mvc
ms.openlocfilehash: 79cffa7aedd0fc04dd4a747ef28bc67cacf37905
ms.sourcegitcommit: 0fa52a34a6274dc872832560cd690be58ae3d0ca
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2020
ms.locfileid: "84204887"
---
# <a name="manage-updates-and-patches-for-your-azure-vms"></a>Verwalten von Updates und Patches für Ihre virtuellen Azure-Computer

In diesem Artikel wird beschrieben, wie Sie das Azure Automation-Feature [Updateverwaltung](automation-update-management.md) verwenden können, um Updates und Patches für Ihre Azure-VMs zu verwalten. Informationen zu den Preisen finden Sie unter [Automation – Preise](https://azure.microsoft.com/pricing/details/automation/) unter „Updateverwaltung“.

> [!NOTE]
> Die Updateverwaltung unterstützt die Bereitstellung von Erstanbieterupdates sowie Vorabdownloads von Patches. Hierzu sind Änderungen an den Systemen erforderlich, die gepatcht werden. Informationen zum Konfigurieren dieser Einstellungen auf Ihren Systemen finden Sie unter [Konfigurieren von Windows Update-Einstellungen für die Azure Automation-Updateverwaltung](automation-configure-windows-update.md).

Bevor Sie die Verfahren in diesem Artikel verwenden, stellen Sie sicher, dass Sie die Updateverwaltung über eines der folgenden Verfahren auf Ihren VMs aktiviert haben:

* [Aktivieren der Updateverwaltung über ein Automation-Konto](automation-onboard-solutions-from-automation-account.md)
* [Aktivieren der Updateverwaltung mittels Durchsuchen im Azure-Portal](automation-onboard-solutions-from-browse.md)
* [Aktivieren der Updateverwaltung über ein Runbook](automation-onboard-solutions.md)
* [Aktivieren der Updateverwaltung über einen virtuellen Azure-Computer](automation-onboard-solutions-from-vm.md)

## <a name="limit-the-scope-for-the-deployment"></a><a name="scope-configuration"></a>Beschränken des Bereichs für die Bereitstellung

Die Updateverwaltung verwendet eine Bereichskonfiguration innerhalb des Arbeitsbereichs, um die Computer, die Updates erhalten sollen, zu erreichen. Weitere Informationen finden Sie unter [Einschränken des Bereitstellungsbereichs für die Updateverwaltung](automation-scope-configurations-update-management.md).

## <a name="view-update-assessment"></a>Anzeigen der Updatebewertung

So zeigen Sie eine Updatebewertung an

1. Wählen Sie in Ihrem Automation-Konto unter **Updateverwaltung** die Option **Updateverwaltung** aus. 

2. Die Updates für Ihre Umgebung werden auf der Seite „Updateverwaltung“ aufgelistet. Falls Updates fehlen, wird auf der Registerkarte **Fehlende Updates** eine Liste mit den entsprechenden Updates angezeigt.

3. Wählen Sie unter **Informationslink** den Link für ein Update aus, um den Supportartikel mit wichtigen Informationen zum Update zu öffnen.

    ![Anzeigen des Updatestatus](./media/automation-tutorial-update-management/manageupdates-view-status-win.png)

4. Klicken Sie im Update auf eine andere Stelle, um den Bereich „Protokollsuche“ zu öffnen. Die Abfrage für die Protokollsuche ist für das jeweilige Update vordefiniert. Sie können diese Abfrage ändern oder eine eigene erstellen, um ausführliche Informationen anzuzeigen.

    ![Anzeigen des Updatestatus](./media/automation-tutorial-update-management/logsearch.png)

## <a name="configure-alerts"></a>Konfigurieren von Warnungen

Gehen Sie wie folgt vor, um Warnungen einrichten, die Sie über den Status einer Updatebereitstellung informieren:

1. Navigieren Sie in Ihrem Automation-Konto unter **Überwachung** zu **Warnungen**, und klicken Sie auf **Neue Warnungsregel**.

2. Auf der Seite „Warnungsregel erstellen“ ist Ihr Automation-Konto bereits als Ressource ausgewählt. Falls Sie es ändern möchten, klicken Sie auf **Ressource bearbeiten**. 

3. Wählen Sie auf der Seite „Ressource auswählen“ im Dropdownmenü **Nach Ressourcentyp filtern** die Option **Automation-Konten** aus. 

4. Wählen Sie das Automation-Konto aus, das Sie verwenden möchten, und klicken Sie dann auf **Fertig**.

5. Klicken Sie auf **Bedingung hinzufügen**, um das geeignete Signal für Ihre Updatebereitstellung auszuwählen. Die folgende Tabelle enthält die Details der beiden verfügbaren Signale:

    |Signalname|Dimensionen|BESCHREIBUNG
    |---|---|---|
    |`Total Update Deployment Runs`|- Name der Updatebereitstellung<br>- Status    |Warnungen zum allgemeinen Status einer Updatebereitstellung|
    |`Total Update Deployment Machine Runs`|- Name der Updatebereitstellung</br>- Status</br>- Zielcomputer</br>- ID der Updatebereitstellungsausführung    |Warnungen zum Status einer Updatebereitstellung für bestimmte Computer.|

6. Wählen Sie als Dimension einen gültigen Wert aus der Liste aus. Falls der gewünschte Wert nicht in der Liste enthalten ist, klicken Sie neben der Dimension auf **\+** , und geben Sie den benutzerdefinierten Namen ein. Wählen Sie anschließend den gewünschten Suchwert aus. Wenn Sie alle Werte für eine Dimension auswählen möchten, klicken Sie auf die Schaltfläche **Auswählen \*** . Wenn Sie für eine Dimension keinen Wert auswählen, wird diese Dimension von der Updateverwaltung ignoriert.

    ![Konfigurieren der Signallogik](./media/automation-tutorial-update-management/signal-logic.png)

7. Geben Sie unter **Warnungslogik** Werte in den Feldern **Zeitaggregation** und **Schwellenwert** ein, und klicken Sie dann auf **Fertig**.

8. Geben Sie im nächsten Bereich einen Namen und eine Beschreibung für die Warnung ein.

9. Legen Sie das Feld **Schweregrad** für eine erfolgreiche Ausführung auf **Information (Schweregrad 2)** bzw. für eine fehlerhafte Ausführung auf **Information (Schweregrad 1)** fest.

    ![Konfigurieren der Signallogik](./media/automation-tutorial-update-management/define-alert-details.png)

10. Klicken Sie je nachdem, wie Sie die Warnungsregel aktivieren möchten, auf **Ja** oder **Nein**.

11. Wenn keine Warnungen für diese Regel ausgegeben werden sollen, wählen Sie **Warnungen unterdrücken** aus.

## <a name="configure-action-groups-for-your-alerts"></a>Konfigurieren von Aktionsgruppen für Ihre Warnungen

Nachdem Sie Ihre Warnungen konfiguriert haben, können Sie eine Aktionsgruppe einrichten. Dabei handelt es sich um eine Gruppe von Aktionen, die für mehrere Warnungen verwendet werden sollen. Dabei kann es sich beispielsweise um E-Mail-Benachrichtigungen, Runbooks oder Webhooks handeln. Weitere Informationen zu Aktionsgruppen finden Sie unter [Erstellen und Verwalten von Aktionsgruppen im Azure-Portal](../azure-monitor/platform/action-groups.md).

1. Wählen Sie eine Warnung und dann unter **Aktionsgruppen** die Option **Neu erstellen** aus. 

2. Geben Sie einen vollständigen und einen kurzen Namen für die Aktionsgruppe ein. Der Kurzname wird von der Updateverwaltung verwendet, wenn Benachrichtigungen unter Verwendung der angegebenen Gruppe gesendet werden.

3. Geben Sie unter **Aktionen** einen Namen ein, der die Aktion angibt, z. B. **E-Mail-Benachrichtigung**. 

4. Wählen Sie für **Aktionstyp** den entsprechenden Typ aus, z. B. **Email/SMS/Push/Voice** (E-Mail/SMS/Push/Sprache). 

5. Klicken Sie auf **Details bearbeiten**.

6. Füllen Sie den Bereich für den Aktionstyp aus. Wenn Sie z. B. **Email/SMS/Push/Voice** (E-Mail/SMS/Push/Sprache) verwenden, geben Sie einen Aktionsnamen ein, aktivieren Sie das Kontrollkästchen **E-Mail**, geben Sie eine gültige E-Mail-Adresse ein, und klicken Sie dann auf **OK**.

    ![Konfigurieren der E-Mail-Aktionsgruppe](./media/automation-tutorial-update-management/configure-email-action-group.png)

7. Klicken Sie im Bereich „Aktionsgruppe hinzufügen“ auf **OK**.

8. Für eine Warn-E-Mail können Sie den E-Mail-Betreff anpassen. Wählen Sie unter **Regel erstellen** die Option **Aktionen anpassen** und dann **E-Mail-Betreff** aus. 

9. Klicken Sie abschließend auf **Warnungsregel erstellen**. 

## <a name="schedule-an-update-deployment"></a>Planen einer Updatebereitstellung

Bei der Planung einer Updatebereitstellung wird eine [Zeitplanressource](shared-resources/schedules.md) erstellt. Diese wird mit dem Runbook **Patch-MicrosoftOMSComputers** verknüpft, das die Updatebereitstellung auf den Zielcomputern verarbeitet. Sie müssen eine Bereitstellung planen, die Ihrem Releasezeitplan und Wartungsfenster entspricht, um Updates zu installieren. Sie können auswählen, welche Updatetypen in die Bereitstellung eingeschlossen werden sollen. Beispielsweise können Sie kritische oder Sicherheitsupdates einschließen und Updaterollups ausschließen.

>[!NOTE]
>Wenn Sie die Zeitplanressource nach der Bereitstellungserstellung über das Azure-Portal oder mithilfe von PowerShell löschen, wird die geplante Updatebereitstellung unterbrochen, und es wird ein Fehler angezeigt, wenn Sie versuchen, die Konfiguration der Zeitplanressource über das Portal zu ändern. Sie können die Zeitplanressource nur löschen, indem Sie den entsprechenden Bereitstellungszeitplan löschen.  

So planen Sie eine neue Updatebereitstellung

1. Navigieren Sie in Ihrem Automation-Konto unter **Updateverwaltung** zu **Updateverwaltung**, und wählen Sie dann **Updatebereitstellung planen** aus.

2. Geben Sie unter **Neue Updatebereitstellung**  im Feld **Name** einen eindeutigen Namen für die Bereitstellung ein.

3. Wählen Sie das Betriebssystem aus, das als Ziel für die Updatebereitstellung dienen soll.

4. Definieren Sie im Bereich **Zu aktualisierende Gruppen (Vorschau)** eine Abfrage mit einer Kombination aus Abonnement, Ressourcengruppen, Standorten und Tags, um eine dynamische Gruppe von Azure-VMs zu erstellen, die in Ihre Bereitstellung eingeschlossen werden sollen. Weitere Informationen finden Sie unter [Verwenden dynamischer Gruppen mit der Updateverwaltung](automation-update-management-groups.md).

5. Wählen Sie im Bereich **Zu aktualisierende Computer** eine gespeicherte Suche oder eine importierte Gruppe aus, oder wählen Sie im Dropdownmenü die Option **Computer** und anschließend einzelne Computer aus. Mit dieser Option können Sie die Bereitschaft des Log Analytics-Agents für jeden Computer erkennen. Weitere Informationen zu den verschiedenen Methoden zum Erstellen von Computergruppen in Azure Monitor-Protokollen finden Sie unter [Computergruppen in Azure Monitor-Protokollen](../azure-monitor/platform/computer-groups.md).

6. Verwenden Sie den Bereich **Updateklassifizierungen**, um [Updateklassifizierungen](automation-view-update-assessments.md#work-with-update-classifications) für Produkte anzugeben. Deaktivieren Sie für jedes Produkt alle unterstützten Updateklassifizierungen, die nicht in Ihre Updatebereitstellung eingeschlossen werden sollen.

7. Verwenden Sie den Bereich **Updates einschließen/ausschließen**, um bestimmte Updates für die Bereitstellung auszuwählen. Auf der Seite „Einschließen/ausschließen“ werden die Updates nach KB-Artikelnummern angezeigt, die ein- oder ausgeschlossen werden sollen. 
    
   > [!IMPORTANT]
   > Denken Sie daran, dass Ausschlüsse eine höhere Priorität haben als Einschlüsse. Wenn Sie also beispielsweise die Ausschlussregel `*` definieren, schließt die Updateverwaltung alle Patches oder Pakete aus der Installation aus. Ausgeschlossene Patches werden weiterhin als auf dem Computer nicht vorhanden angezeigt. Wenn Sie für Linux-Computer ein Paket mit einem ausgeschlossenen abhängigen Paket einschließen, wird das Hauptpaket von der Updateverwaltung nicht installiert.

   > [!NOTE]
   > Sie können keine Updates angeben, die für die Aufnahme in die Updatebereitstellung ersetzt wurden.

8. Wählen Sie **Zeitplaneinstellungen** aus. Der Standard-Startzeitpunkt liegt 30 Minuten nach der aktuellen Uhrzeit. Als Startzeit können Sie einen beliebigen Wert festlegen, er muss jedoch mindestens 10 Minuten in der Zukunft liegen.

9. Verwenden Sie das Feld **Wiederholung**, um anzugeben, ob die Bereitstellung einmal oder nach einem wiederkehrenden Zeitplan erfolgt, und klicken Sie dann auf **OK**.

10. Wählen Sie im Bereich **Vor und nach dem Vorgang auszuführende Skripts** die Skripts aus, die vor und nach Ihrer Bereitstellung ausgeführt werden sollen. Weitere Informationen finden Sie unter [Verwalten von Pre- und Post-Skripts](pre-post-scripts.md).
    
11. Geben Sie im Feld **Wartungsfenster (Minuten)** die zulässige Zeit für die Installation von Updates an. Bei der Angabe eines Wartungsfensters sind folgende Aspekte zu beachten:

    * Wartungsfenster steuern, wie viele Updates installiert werden.
    * Die Installation neuer Updates wird von der Updateverwaltung nicht beendet, wenn das Ende eines Wartungsfensters fast erreicht ist.
    * Bereits aktive Updates werden von der Updateverwaltung bei Überschreiten des Wartungsfensters nicht beendet.
    * Unter Windows ist die Überschreitung des Wartungsfensters häufig auf eine lange dauernde Installation eines Service Pack-Updates zurückzuführen.

    > [!NOTE]
    > Damit unter Ubuntu keine Updates außerhalb der Wartungsfenster angewandt werden, konfigurieren Sie das Paket `Unattended-Upgrade` erneut, um automatische Updates zu deaktivieren. Weitere Informationen zur Konfiguration dieses Pakets finden Sie im [Thema zu automatischen Updates im Ubuntu-Serverhandbuch](https://help.ubuntu.com/lts/serverguide/automatic-updates.html).

12. Verwenden Sie das Feld **Neustartoptionen**, um die Methode zum Behandeln von Neustarts während der Bereitstellung anzugeben. Die folgenden Optionen sind verfügbar: 
    * Neustart, falls erforderlich (Standard)
    * Immer neu starten
    * Nie neu starten
    * Nur Neustart: Mit dieser Option werden keine Updates installiert.

    > [!NOTE]
    > Die unter [Registrierungsschlüssel zum Verwalten des Neustarts](/windows/deployment/update/waas-restart#registry-keys-used-to-manage-restart) aufgeführten Registrierungsschlüssel können zu einem Neustartereignis führen, wenn **Neustartoptionen** auf **Nie neu starten** festgelegt ist.

13. Klicken Sie nach Abschluss der Konfiguration des Bereitstellungszeitplans auf **Erstellen**.

    ![Bereich für Updatezeitplan-Einstellungen](./media/automation-tutorial-update-management/manageupdates-schedule-win.png)

14. Das Statusdashboard wird wieder angezeigt. Wählen Sie **Geplante Updatebereitstellungen** aus, um den erstellten Bereitstellungszeitplan anzuzeigen.

## <a name="schedule-an-update-deployment-programmatically"></a>Programmgesteuertes Planen einer Updatebereitstellung

Weitere Informationen zum Erstellen einer Updatebereitstellung mit der REST-API finden Sie unter [Softwareupdatekonfigurationen – Erstellen](/rest/api/automation/softwareupdateconfigurations/create). 

Sie können auch ein Beispielrunbook zum Erstellen einer wöchentlichen Updatebereitstellung verwenden. Weitere Informationen zu diesem Runbook finden Sie unter [Erstellen einer wöchentlichen Updatebereitstellung für einen oder mehrere virtuelle Computer in einer Ressourcengruppe](https://gallery.technet.microsoft.com/scriptcenter/Create-a-weekly-update-2ad359a1).

## <a name="check-deployment-status"></a>Überprüfen des Bereitstellungsstatus

Nach dem Start der geplanten Bereitstellung wird ihr Status auf der Registerkarte **Updatebereitstellungen** unter **Updateverwaltung** angezeigt. Der Status lautet **In Bearbeitung**, wenn die Bereitstellung derzeit ausgeführt wird. Nach erfolgreichem Abschluss der Bereitstellung ändert sich der Status in **Erfolgreich**. Wenn bei einzelnen oder mehreren Updates in der Bereitstellung Fehler auftreten, wird der Status **Der Vorgang ist teilweise fehlgeschlagen.** angezeigt.

## <a name="view-results-of-a-completed-update-deployment"></a>Anzeigen der Ergebnisse einer abgeschlossenen Updatebereitstellung

Wenn die Bereitstellung abgeschlossen ist, können Sie sie auswählen, um das zugehörige Dashboard anzuzeigen.

![Updatebereitstellungs-Statusdashboard für eine bestimmte Bereitstellung](./media/automation-tutorial-update-management/manageupdates-view-results.png)

Unter **Updateergebnisse** wird eine Zusammenfassung der Gesamtanzahl von Updates und der Bereitstellungsergebnisse auf den Ziel-VMs angegeben. Die Tabelle rechts enthält eine detaillierte Aufschlüsselung der einzelnen Updates und der jeweiligen Installationsergebnisse.

Die verfügbaren Werte sind:

* **Nicht versucht:** Das Update wurde nicht installiert, da aufgrund des definierten Wartungsfensters nicht genügend Zeit zur Verfügung stand.
* **Nicht ausgewählt:** Das Update wurde nicht zur Bereitstellung ausgewählt.
* **Erfolgreich:** Das Update wurde erfolgreich ausgeführt.
* **Fehler:** Beim Update ist ein Fehler aufgetreten.

Wählen Sie **Alle Protokolle** aus, um alle von der Bereitstellung erstellten Protokolleinträge anzuzeigen.

Wählen Sie **Ausgabe** aus, um den Auftragsdatenstrom des Runbooks anzuzeigen, das für die Verwaltung der Updatebereitstellung auf den Ziel-VMs verantwortlich ist.

Klicken Sie auf **Fehler**, um ausführliche Informationen zu Fehlern bei der Bereitstellung anzuzeigen.

## <a name="view-the-deployment-alert"></a>Anzeigen der Bereitstellungswarnung

Wenn die Updatebereitstellung abgeschlossen ist, erhalten Sie die Warnung, die Sie beim Setup für die Bereitstellung angegeben haben. Hier ist beispielsweise eine E-Mail, die einen Patch bestätigt.

![Konfigurieren der E-Mail-Aktionsgruppe](./media/automation-tutorial-update-management/email-notification.png)

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu Bereichskonfigurationen finden Sie unter [Einschränken des Bereitstellungsbereichs für die Updateverwaltung](automation-scope-configurations-update-management.md).
* Wenn Sie Protokolle durchsuchen müssen, die in Ihrem Log Analytics Arbeitsbereich gespeichert sind, finden Sie unter [Protokollsuchvorgänge in Azure Monitor-Protokollen](../log-analytics/log-analytics-log-searches.md) weitere Informationen.
* Wenn Sie mit den Bereitstellungen fertig sind, fahren Sie mit [Aufheben der Verknüpfung eines Arbeitsbereichs mit einem Automation-Konto für die Updateverwaltung](automation-unlink-workspace-update-management.md) fort.
* Informationen zum Löschen Ihrer VMs aus der Updateverwaltung finden Sie unter [Entfernen von VMs aus der Updateverwaltung](automation-remove-vms-from-update-management.md).
* Informationen zum Behandeln von Fehlern bei der Updateverwaltung finden Sie unter [Behandeln von Problemen mit der Updateverwaltung](troubleshoot/update-management.md).
* Informationen zum Behandeln von Problemen mit dem Windows Update-Agent finden Sie unter [Beheben von Problemen mit dem Windows Update-Agent](troubleshoot/update-agent-issues.md).
* Informationen zum Behandeln von Problemen mit dem Linux-Update-Agent finden Sie unter [Beheben von Problemen mit dem Linux-Update-Agent](troubleshoot/update-agent-issues-linux.md).