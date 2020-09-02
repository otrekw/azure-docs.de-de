---
title: Erstellen von Updatebereitstellungen für die Azure Automation-Updateverwaltung
description: In diesem Artikel wird beschrieben, wie Sie Updatebereitstellungen planen und deren Status anzeigen.
services: automation
ms.subservice: update-management
ms.date: 08/20/2020
ms.topic: conceptual
ms.openlocfilehash: 4336ba272dd83ad2a35060c1c7524a564b928484
ms.sourcegitcommit: 6fc156ceedd0fbbb2eec1e9f5e3c6d0915f65b8e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/21/2020
ms.locfileid: "88717692"
---
# <a name="how-to-deploy-updates-and-review-results"></a>Bereitstellen von Updates und Überprüfen von Ergebnissen

In diesem Artikel wird beschrieben, wie Sie eine Updatebereitstellung planen und nach Abschluss der Bereitstellung den Prozess überprüfen.

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich beim [Azure-Portal](https://portal.azure.com)

## <a name="schedule-an-update-deployment"></a>Planen einer Updatebereitstellung

Bei der Planung einer Updatebereitstellung wird eine [Zeitplanressource](../shared-resources/schedules.md) erstellt. Diese wird mit dem Runbook **Patch-MicrosoftOMSComputers** verknüpft, das die Updatebereitstellung auf den Zielcomputern verarbeitet. Sie müssen eine Bereitstellung planen, die Ihrem Releasezeitplan und Wartungsfenster entspricht, um Updates zu installieren. Sie können auswählen, welche Updatetypen in die Bereitstellung eingeschlossen werden sollen. Beispielsweise können Sie kritische oder Sicherheitsupdates einschließen und Updaterollups ausschließen.

>[!NOTE]
>Wenn Sie die Zeitplanressource nach der Bereitstellungserstellung über das Azure-Portal oder mithilfe von PowerShell löschen, wird die geplante Updatebereitstellung unterbrochen, und es wird ein Fehler angezeigt, wenn Sie versuchen, die Konfiguration der Zeitplanressource über das Portal zu ändern. Sie können die Zeitplanressource nur löschen, indem Sie den entsprechenden Bereitstellungszeitplan löschen.  

So planen Sie eine neue Updatebereitstellung

1. Navigieren Sie in Ihrem Automation-Konto unter **Updateverwaltung** zu **Updateverwaltung**, und wählen Sie dann **Updatebereitstellung planen** aus.

2. Geben Sie unter **Neue Updatebereitstellung**  im Feld **Name** einen eindeutigen Namen für Ihre Bereitstellung ein.

3. Wählen Sie das Betriebssystem aus, das als Ziel für die Updatebereitstellung dienen soll.

4. Definieren Sie im Bereich **Zu aktualisierende Gruppen (Vorschau)** eine Abfrage mit einer Kombination aus Abonnement, Ressourcengruppen, Standorten und Tags, um eine dynamische Gruppe von Azure-VMs zu erstellen, die in Ihre Bereitstellung eingeschlossen werden sollen. Weitere Informationen finden Sie unter [Verwenden dynamischer Gruppen mit der Updateverwaltung](update-mgmt-groups.md).

5. Wählen Sie im Bereich **Zu aktualisierende Computer** eine gespeicherte Suche oder eine importierte Gruppe aus, oder wählen Sie im Dropdownmenü die Option **Computer** und anschließend einzelne Computer aus. Mit dieser Option können Sie die Bereitschaft des Log Analytics-Agents für jeden Computer erkennen. Weitere Informationen zu den verschiedenen Methoden zum Erstellen von Computergruppen in Azure Monitor-Protokollen finden Sie unter [Computergruppen in Azure Monitor-Protokollen](../../azure-monitor/platform/computer-groups.md).

6. Verwenden Sie den Bereich **Updateklassifizierungen**, um [Updateklassifizierungen](update-mgmt-view-update-assessments.md#work-with-update-classifications) für Produkte anzugeben. Deaktivieren Sie für jedes Produkt alle unterstützten Updateklassifizierungen, die nicht in Ihre Updatebereitstellung eingeschlossen werden sollen.

    Wenn Ihre Bereitstellung nur für ausgewählte Updates gelten soll, ist es erforderlich, alle vorab ausgewählten Updateklassifizierungen zu deaktivieren, wenn die Option  **Updates einschließen/ausschließen** wie im nächsten Schritt beschrieben konfiguriert wird. Dadurch wird sichergestellt, dass nur die Updates, die Sie in diese Bereitstellung *einschließen* möchten, auf den Zielcomputern installiert werden.

7. Über den Bereich **Updates ein-/ausschließen** können Sie ausgewählte Updates in die Bereitstellung einschließen oder von ihr ausschließen. Auf der Seite **Einschließen/Ausschließen** geben Sie KB-Artikel-ID-Nummern ein, die ein- oder ausgeschlossen werden sollen.

   > [!IMPORTANT]
   > Denken Sie daran, dass Ausschlüsse eine höhere Priorität haben als Einschlüsse. Wenn Sie also beispielsweise die Ausschlussregel `*` definieren, schließt die Updateverwaltung alle Patches oder Pakete aus der Installation aus. Ausgeschlossene Patches werden weiterhin als auf dem Computer nicht vorhanden angezeigt. Wenn Sie für Linux-Computer ein Paket mit einem ausgeschlossenen abhängigen Paket einschließen, wird das Hauptpaket von der Updateverwaltung nicht installiert.

   > [!NOTE]
   > Sie können keine Updates angeben, die für die Aufnahme in die Updatebereitstellung ersetzt wurden.

8. Wählen Sie **Zeitplaneinstellungen** aus. Der Standard-Startzeitpunkt liegt 30 Minuten nach der aktuellen Uhrzeit. Als Startzeit können Sie einen beliebigen Wert festlegen, er muss jedoch mindestens 10 Minuten in der Zukunft liegen.

9. Verwenden Sie das Feld **Wiederholung**, um anzugeben, ob die Bereitstellung einmal oder nach einem wiederkehrenden Zeitplan erfolgt, und wählen Sie dann **OK** aus.

10. Wählen Sie im Bereich **Vor und nach dem Vorgang auszuführende Skripts** die Skripts aus, die vor und nach Ihrer Bereitstellung ausgeführt werden sollen. Weitere Informationen finden Sie unter [Verwalten von Pre- und Post-Skripts](update-mgmt-pre-post-scripts.md).
    
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

13. Wählen Sie nach Abschluss der Konfiguration des Bereitstellungszeitplans **Erstellen** aus.

    ![Bereich für Updatezeitplan-Einstellungen](./media/update-mgmt-deploy-updates/manageupdates-schedule-win.png)

14. Das Statusdashboard wird wieder angezeigt. Wählen Sie **Geplante Updatebereitstellungen** aus, um den erstellten Bereitstellungszeitplan anzuzeigen.

## <a name="schedule-an-update-deployment-programmatically"></a>Programmgesteuertes Planen einer Updatebereitstellung

Weitere Informationen zum Erstellen einer Updatebereitstellung mit der REST-API finden Sie unter [Softwareupdatekonfigurationen – Erstellen](/rest/api/automation/softwareupdateconfigurations/create).

Sie können auch ein Beispielrunbook zum Erstellen einer wöchentlichen Updatebereitstellung verwenden. Weitere Informationen zu diesem Runbook finden Sie unter [Erstellen einer wöchentlichen Updatebereitstellung für einen oder mehrere virtuelle Computer in einer Ressourcengruppe](https://gallery.technet.microsoft.com/scriptcenter/Create-a-weekly-update-2ad359a1).

## <a name="check-deployment-status"></a>Überprüfen des Bereitstellungsstatus

Nach dem Start der geplanten Bereitstellung wird ihr Status auf der Registerkarte **Updatebereitstellungen** unter **Updateverwaltung** angezeigt. Der Status lautet **In Bearbeitung**, wenn die Bereitstellung derzeit ausgeführt wird. Nach erfolgreichem Abschluss der Bereitstellung ändert sich der Status in **Erfolgreich**. Wenn bei einzelnen oder mehreren Updates in der Bereitstellung Fehler auftreten, wird der Status **Der Vorgang ist teilweise fehlgeschlagen.** angezeigt.

## <a name="view-results-of-a-completed-update-deployment"></a>Anzeigen der Ergebnisse einer abgeschlossenen Updatebereitstellung

Wenn die Bereitstellung abgeschlossen ist, können Sie sie auswählen, um ihre Ergebnisse anzuzeigen.

[ ![Updatebereitstellungsstatus-Dashboard für eine bestimmte Bereitstellung](./media/update-mgmt-deploy-updates/manageupdates-view-results.png)](./media/update-mgmt-deploy-updates/manageupdates-view-results-expanded.png#lightbox)

Unter **Updateergebnisse** wird eine Zusammenfassung der Gesamtanzahl von Updates und der Bereitstellungsergebnisse auf den Ziel-VMs angegeben. Die Tabelle rechts enthält eine detaillierte Aufschlüsselung der einzelnen Updates und der jeweiligen Installationsergebnisse.

Die verfügbaren Werte sind:

* **Nicht versucht:** Das Update wurde nicht installiert, da aufgrund des definierten Wartungsfensters nicht genügend Zeit zur Verfügung stand.
* **Nicht ausgewählt:** Das Update wurde nicht zur Bereitstellung ausgewählt.
* **Erfolgreich:** Das Update wurde erfolgreich ausgeführt.
* **Fehler:** Beim Update ist ein Fehler aufgetreten.

Wählen Sie **Alle Protokolle** aus, um alle von der Bereitstellung erstellten Protokolleinträge anzuzeigen.

Wählen Sie **Ausgabe** aus, um den Auftragsdatenstrom des Runbooks anzuzeigen, das für die Verwaltung der Updatebereitstellung auf den Ziel-VMs verantwortlich ist.

Klicken Sie auf **Fehler**, um ausführliche Informationen zu Fehlern bei der Bereitstellung anzuzeigen.

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Erstellen von Warnungen, die Sie über die Ergebnisse der Updatebereitstellung informieren, finden Sie unter [Erstellen von Warnungen für die Updateverwaltung](update-mgmt-configure-alerts.md).