---
title: Verwalten von Updates und Patches für Ihre Azure-VMs in Azure Automation
description: In diesem Artikel erfahren Sie, wie Sie die Updateverwaltung verwenden können, um Updates und Patches für Ihre Azure-VMs zu verwalten.
services: automation
ms.subservice: update-management
ms.topic: conceptual
ms.date: 04/06/2020
ms.custom: mvc
ms.openlocfilehash: 5b5172df6ed6993742a08d5ac08cf700681dfc6a
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2020
ms.locfileid: "83829153"
---
# <a name="manage-updates-and-patches-for-your-azure-vms"></a>Verwalten von Updates und Patches für Ihre virtuellen Azure-Computer

In diesem Artikel wird beschrieben, wie Sie das Azure Automation-Feature [Updateverwaltung](automation-update-management.md) verwenden können, um Updates und Patches für Ihre Azure-VMs zu verwalten. 

Informationen zu den Preisen finden Sie unter [Automation – Preise](https://azure.microsoft.com/pricing/details/automation/) unter „Updateverwaltung“.

## <a name="prerequisites"></a>Voraussetzungen

* Aktiviertes Feature [Updateverwaltung](automation-update-management.md) für eine oder mehrere Ihrer VMs. 
* Ein für die Updateverwaltung aktivierter [virtuellen Computer](../virtual-machines/windows/quick-create-portal.md).

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter https://portal.azure.com beim Azure-Portal an.

## <a name="view-update-assessment"></a>Anzeigen der Updatebewertung

Nach dem Aktivieren der Updateverwaltung wird die Seite „Updateverwaltung“ geöffnet. Falls Updates fehlen, wird auf der Registerkarte **Fehlende Updates** eine Liste mit den entsprechenden Updates angezeigt.

Wählen Sie unter **Informationslink** den Updatelink aus, um den Supportartikel für das Update zu öffnen. Dort finden Sie wichtige Informationen zum Update.

![Anzeigen des Updatestatus](./media/automation-tutorial-update-management/manageupdates-view-status-win.png)

Klicken Sie im Update auf eine andere Stelle, um den Bereich „Protokollsuche“ für das ausgewählte Update zu öffnen. Die Abfrage für die Protokollsuche ist für das jeweilige Update vordefiniert. Sie können diese Abfrage ändern oder eine eigene Abfrage erstellen, um ausführliche Informationen zu Updates anzuzeigen, die in Ihrer Umgebung bereitgestellt wurden oder fehlen.

![Anzeigen des Updatestatus](./media/automation-tutorial-update-management/logsearch.png)

## <a name="configure-alerts"></a>Konfigurieren von Warnungen

In diesem Schritt erfahren Sie, wie Sie eine Warnung einrichten, um über den Status einer Updatebereitstellung informiert zu werden.

### <a name="alert-conditions"></a>Warnungsbedingungen

Navigieren Sie in Ihrem Automation-Konto unter **Überwachung** zu **Warnungen**, und klicken Sie auf **Neue Warnungsregel**.

Ihr Automation-Konto ist bereits als Ressource ausgewählt. Falls Sie sie ändern möchten, klicken Sie auf **Auswählen**. Wählen Sie auf der Seite „Ressource auswählen“ im Dropdownmenü **Nach Ressourcentyp filtern** die Option **Automation-Konten** aus. Wählen Sie Ihr Automation-Konto aus, und klicken Sie anschließend auf **Fertig**.

Klicken Sie auf **Bedingung hinzufügen**, um das geeignete Signal für Ihre Updatebereitstellung auszuwählen. Die folgende Tabelle enthält die Details der beiden verfügbaren Signale:

|Signalname|Dimensionen|BESCHREIBUNG|
|---|---|---|
|`Total Update Deployment Runs`|- Name der Updatebereitstellung<br>- Status|Dieses Signal wird für Warnungen im Zusammenhang mit dem allgemeinen Status einer Updatebereitstellung verwendet.|
|`Total Update Deployment Machine Runs`|- Name der Updatebereitstellung</br>- Status</br>- Zielcomputer</br>- ID der Updatebereitstellungsausführung|Dieses Signal wird für Warnungen im Zusammenhang mit dem Status einer Updatebereitstellung auf bestimmten Computern verwendet.|

Wählen Sie als Dimension einen gültigen Wert aus der Liste aus. Sollte der gewünschte Wert nicht in der Liste enthalten sein, klicken Sie neben der Dimension auf das Plussymbol ( **\+** ), und geben Sie den benutzerdefinierten Namen ein. Wählen Sie anschließend den gewünschten Suchwert aus. Wenn Sie alle Werte für eine Dimension auswählen möchten, klicken Sie auf die Schaltfläche **Auswählen \*** . Wenn Sie für eine Dimension keinen Wert auswählen, wird diese Dimension von der Updateverwaltung ignoriert.

![Konfigurieren der Signallogik](./media/automation-tutorial-update-management/signal-logic.png)

Geben Sie unter **Warnungslogik** für **Schwellenwert** den Wert **1** ein. Klicken Sie auf **Fertig**, wenn Sie fertig sind.

### <a name="alert-details"></a>Warnungsdetails

Wählen Sie unter **2. Warnungsdetails definieren** einen Namen und eine Beschreibung für die Warnung ein. Legen Sie **Schweregrad** für eine erfolgreiche Ausführung auf **Information (Schweregrad 2)** bzw. für eine Ausführung mit Fehler auf **Information (Schweregrad 1)** fest.

![Konfigurieren der Signallogik](./media/automation-tutorial-update-management/define-alert-details.png)

Wählen Sie unter **Aktionsgruppen** die Option **Neu erstellen** aus. Eine Aktionsgruppe ist eine Gruppe mit Aktionen, die Sie übergreifend für mehrere Warnungen verwenden können. Dabei kann es sich beispielsweise um E-Mail-Benachrichtigungen, Runbooks oder Webhooks handeln. Weitere Informationen zu Aktionsgruppen finden Sie unter [Erstellen und Verwalten von Aktionsgruppen im Azure-Portal](../azure-monitor/platform/action-groups.md).

Geben Sie im Feld **Name der Aktionsgruppe** einen Namen für die Warnung sowie einen Kurznamen ein. Der Kurzname wird von der Updateverwaltung anstelle eines vollständigen Aktionsgruppennamens verwendet, wenn Benachrichtigungen unter Verwendung der angegebenen Gruppe gesendet werden.

Geben Sie unter **Aktionen** einen Namen für die Aktion ein (beispielsweise **Email Notification**). Wählen Sie unter **Aktionstyp** die Option **E-Mail/SMS/Push/Sprachanruf** aus. Wählen Sie unter **Details** die Option **Details bearbeiten** aus.

Geben Sie auf der Seite „E-Mail/SMS/Push/Sprachanruf“ einen Namen ein. Aktivieren Sie das Kontrollkästchen **E-Mail**, und geben Sie eine gültige E-Mail-Adresse ein.

![Konfigurieren der E-Mail-Aktionsgruppe](./media/automation-tutorial-update-management/configure-email-action-group.png)

Klicken Sie im Bereich „E-Mail/SMS/Push/Sprachanruf“ auf **OK**. Klicken Sie im Bereich „Aktionsgruppe hinzufügen“ auf **OK**.

Wenn Sie den Betreff der Warnungs-E-Mail anpassen möchten, wählen Sie unter **Regel erstellen** > **Aktionen anpassen** die Option **E-Mail-Betreff** aus. Klicken Sie abschließend auf **Warnungsregel erstellen**. Die Warnung informiert Sie, wann die Bereitstellung eines Updates erfolgreich war. Außerdem wird angegeben, welche Computer Teil der Updatebereitstellung waren.

## <a name="schedule-an-update-deployment"></a>Planen einer Updatebereitstellung

Planen Sie eine Bereitstellung, die Ihrem Releasezeitplan und Wartungsfenster entspricht, um Updates zu installieren. Sie können auswählen, welche Updatetypen in die Bereitstellung eingeschlossen werden sollen. Beispielsweise können Sie kritische oder Sicherheitsupdates einschließen und Updaterollups ausschließen.

>[!NOTE]
>Bei der Planung einer Updatebereitstellung wird eine [Zeitplanressource](shared-resources/schedules.md) erstellt. Diese wird mit dem Runbook **Patch-MicrosoftOMSComputers** verknüpft, das die Updatebereitstellung auf den Zielcomputern verarbeitet. Wenn Sie die Zeitplanressource nach der Bereitstellungserstellung über das Azure-Portal oder mithilfe von PowerShell löschen, wird die geplante Updatebereitstellung unterbrochen, und es wird ein Fehler angezeigt, wenn Sie versuchen, die Konfiguration der Zeitplanressource über das Portal zu ändern. Sie können die Zeitplanressource nur löschen, indem Sie den entsprechenden Bereitstellungszeitplan löschen.  

Um eine neue Updatebereitstellung für den virtuellen Computer zu planen, klicken Sie auf **Updateverwaltung** und dann auf **Updatebereitstellung planen**.

Geben Sie unter **Neue Updatebereitstellung** die folgenden Informationen ein:

* **Name**: Geben Sie einen eindeutigen Namen für die Updatebereitstellung ein.

* **Betriebssystem**: Wählen Sie das Betriebssystem aus, für das die Updatebereitstellung ausgeführt werden soll.

* **Zu aktualisierende Gruppen (Vorschau)** : Definieren Sie eine Abfrage mit einer Kombination aus Abonnement, Ressourcengruppen, Standorten und Tags, um eine dynamische Gruppe virtueller Azure-Computer zu erstellen, die in Ihre Bereitstellung eingeschlossen werden sollen. Weitere Informationen finden Sie unter [Dynamische Gruppen](automation-update-management-groups.md).

* **Zu aktualisierende Computer**: Wählen Sie eine gespeicherte Suche oder eine importierte Gruppe aus, oder wählen Sie im Dropdownmenü die Option **Computer** und anschließend einzelne Computer aus. Wenn Sie **Computer** auswählen, wird die Bereitschaft des jeweiligen Computers in der Spalte **Update-Agent-Bereitschaft** angezeigt. Weitere Informationen zu den verschiedenen Methoden zum Erstellen von Computergruppen in Azure Monitor-Protokollen finden Sie unter [Computergruppen in Azure Monitor-Protokollen](../azure-monitor/platform/computer-groups.md).

* **Updateklassifizierung**: Deaktivieren Sie für jedes Produkt alle unterstützten Updateklassifizierungen, die nicht in Ihre Updatebereitstellung eingeschlossen werden sollen. Beschreibungen der Klassifizierungstypen finden Sie unter [Updateklassifizierungen](automation-view-update-assessments.md#work-with-update-classifications).

* **Einzuschließende/auszuschließende Updates**: Öffnet die Seite „Einschließen/ausschließen“. Ein- bzw. auszuschließende Updates werden auf separaten Registerkarten anhand der jeweiligen KB-Artikel-ID angegeben. Wenn Sie eine oder mehrere ID-Nummern angeben, müssen Sie alle Klassifizierungen mit der Updatebereitstellung entfernen oder deaktivieren. Dadurch wird sichergestellt, dass bei der Angabe von Update-IDs keine anderen Updates in Ihrem Updatepaket enthalten sind.

> [!NOTE]
> Ausschlüsse haben eine höhere Priorität als Einschlüsse. Wenn Sie also beispielsweise die Ausschlussregel `*` definieren, werden von der Updateverwaltung keine Patches oder Pakete installiert, da sie alle ausgeschlossen wurden. Ausgeschlossene Patches werden weiterhin als auf dem Computer nicht vorhanden angezeigt. Wenn Sie für Linux-Computer ein Paket mit einem ausgeschlossenen abhängigen Paket einschließen, wird das Hauptpaket von der Updateverwaltung nicht installiert.

> [!NOTE]
> Sie können keine Updates angeben, die für die Aufnahme in die Updatebereitstellung ersetzt wurden.
>
* **Zeitplaneinstellungen**: Der Bereich „Zeitplaneinstellungen“ wird geöffnet. Der Standard-Startzeitpunkt liegt 30 Minuten nach der aktuellen Uhrzeit. Als Startzeit können Sie einen beliebigen Wert festlegen, er muss jedoch mindestens 10 Minuten in der Zukunft liegen.

   Sie können auch angeben, ob die Bereitstellung einmalig erfolgt, oder einen sich wiederholenden Zeitplan einrichten. Wählen Sie unter **Wiederholung** die Option **Einmal**. Übernehmen Sie den Standardwert „1 Tag“, und klicken Sie auf **OK**. Durch diese Einträge wird eine Zeitplanserie eingerichtet.

* **Vor und nach dem Vorgang auszuführende Skripts**: Wählen Sie die Skripts aus, die vor und nach Ihrer Bereitstellung ausgeführt werden sollen. Weitere Informationen finden Sie unter [Verwalten von Pre- und Post-Skripts](pre-post-scripts.md).

* **Wartungsfenster (Minuten)** : Behalten Sie den Standardwert bei. Mithilfe von Wartungsfenstern wird der zulässige Zeitraum für die Installation von Updates gesteuert. Bei der Angabe eines Wartungsfensters sind folgende Aspekte zu beachten:

  * Wartungsfenster steuern, wie viele Updates installiert werden.
  * Die Installation neuer Updates wird von der Updateverwaltung nicht beendet, wenn das Ende eines Wartungsfensters fast erreicht ist.
  * Bereits aktive Updates werden von der Updateverwaltung bei Überschreiten des Wartungsfensters nicht beendet.
  * Unter Windows ist die Überschreitung des Wartungsfensters häufig auf eine lange dauernde Installation eines Service Pack-Updates zurückzuführen.

  > [!NOTE]
  > Damit unter Ubuntu keine Updates außerhalb der Wartungsfenster angewendet werden, konfigurieren Sie das „Unattended-Upgrade“-Paket erneut, um automatische Updates zu deaktivieren. Informationen zur Konfiguration dieses Pakets finden Sie im [Thema zu automatischen Updates im Ubuntu-Serverhandbuch](https://help.ubuntu.com/lts/serverguide/automatic-updates.html).

* **Neustartoptionen**: Hiermit können Optionen für den Umgang mit Neustarts angegeben werden. Die folgenden Optionen sind verfügbar:
  * Neustart, falls erforderlich (Standard)
  * Immer neu starten
  * Nie neu starten
  * Nur Neustart – keine Installation von Updates

> [!NOTE]
> Die unter [Registrierungsschlüssel zum Verwalten des Neustarts](/windows/deployment/update/waas-restart#registry-keys-used-to-manage-restart) aufgeführten Registrierungsschlüssel können zu einem Neustartereignis führen, wenn **Neustartoptionen** auf **Nie neu starten** festgelegt ist.

Klicken Sie nach Abschluss der Zeitplankonfiguration auf **Erstellen**.

![Bereich für Updatezeitplan-Einstellungen](./media/automation-tutorial-update-management/manageupdates-schedule-win.png)

Das Statusdashboard wird wieder angezeigt. Wählen Sie **Geplante Updatebereitstellungen** aus, um den soeben erstellten Bereitstellungszeitplan anzuzeigen.

> [!NOTE]
> Die Updateverwaltung unterstützt die Bereitstellung von Erstanbieterupdates sowie Vorabdownloads von Patches. Hierzu sind Änderungen an den Systemen erforderlich, die gepatcht werden. Informationen zum Konfigurieren der entsprechenden Einstellungen für Ihre Systeme finden Sie im [Abschnitt zu Erstanbieterpatches und Vorabdownloads](automation-configure-windows-update.md).

Sie können Updatebereitstellungen auch programmgesteuert erstellen. Weitere Informationen zum Erstellen einer Updatebereitstellung mit der REST-API finden Sie unter [Softwareupdatekonfigurationen – Erstellen](/rest/api/automation/softwareupdateconfigurations/create). Es ist auch ein Beispielrunbook vorhanden, das Sie zum Erstellen einer wöchentlichen Updatebereitstellung verwenden können. Weitere Informationen zu diesem Runbook finden Sie unter [Erstellen einer wöchentlichen Updatebereitstellung für einen oder mehrere virtuelle Computer in einer Ressourcengruppe](https://gallery.technet.microsoft.com/scriptcenter/Create-a-weekly-update-2ad359a1).

## <a name="view-results-of-an-update-deployment"></a>Anzeigen der Ergebnisse einer Updatebereitstellung

Nach dem Start der geplanten Bereitstellung sehen Sie den Status der Bereitstellung auf der Registerkarte **Updatebereitstellungen** unter **Updateverwaltung**. Der Status lautet **In Bearbeitung**, wenn die Bereitstellung derzeit ausgeführt wird. Nach erfolgreichem Abschluss der Bereitstellung ändert sich der Status in **Erfolgreich**. Wenn bei einzelnen oder mehreren Updates in der Bereitstellung Fehler auftreten, wird der Status **Der Vorgang ist teilweise fehlgeschlagen.** angezeigt.

Wählen Sie die abgeschlossene Updatebereitstellung aus, um das zugehörige Dashboard anzuzeigen.

![Updatebereitstellungs-Statusdashboard für eine bestimmte Bereitstellung](./media/automation-tutorial-update-management/manageupdates-view-results.png)

Unter **Updateergebnisse** werden eine Zusammenfassung der Gesamtzahl von Updates und der Ergebnisse der Bereitstellung auf dem virtuellen Computer angegeben. Die Tabelle rechts enthält eine detaillierte Aufschlüsselung der einzelnen Updates und die Installationsergebnisse.

Die verfügbaren Werte sind:

* **Kein Versuch erfolgt**: Das Update wurde nicht installiert, da aufgrund des definierten Wartungsfensters nicht genügend Zeit zur Verfügung stand.
* **Erfolg:** Das Update war erfolgreich.
* **Fehler:** Beim Update ist ein Fehler aufgetreten.

Wählen Sie **Alle Protokolle** aus, um alle von der Bereitstellung erstellten Protokolleinträge anzuzeigen.

Klicken Sie auf **Ausgabe**, um den Auftragsdatenstrom des Runbooks anzuzeigen, das für die Verwaltung der Updatebereitstellung auf dem virtuellen Zielcomputer verantwortlich ist.

Klicken Sie auf **Fehler**, um ausführliche Informationen zu Fehlern bei der Bereitstellung anzuzeigen.

War Ihre Updatebereitstellung erfolgreich, erhalten Sie eine Bestätigungs-E-Mail wie die folgende:

![Konfigurieren der E-Mail-Aktionsgruppe](./media/automation-tutorial-update-management/email-notification.png)

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zur Updateverwaltung finden Sie unter [Übersicht über die Updateverwaltung](automation-update-management.md).