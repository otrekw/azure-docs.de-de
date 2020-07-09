---
title: Verwalten von Updates für mehrere virtuelle Computer in Azure Automation
description: In diesem Artikel erfahren Sie, wie Sie Updates für mehrere virtuelle Computer verwalten.
services: automation
ms.subservice: update-management
ms.date: 03/26/2020
ms.topic: conceptual
ms.openlocfilehash: a2d16bdca18b7fc0afab2a3deb325d1a75be3bb8
ms.sourcegitcommit: ec682dcc0a67eabe4bfe242fce4a7019f0a8c405
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2020
ms.locfileid: "86185159"
---
# <a name="manage-updates-for-multiple-vms"></a>Verwalten von Updates für mehrere virtuelle Computer

Sie können die Azure Automation-Updateverwaltung verwenden, um Updates und Patches für Ihre virtuellen Windows- und Linux-Computer zu verwalten. Über Ihr [Azure Automation](./index.yml)-Konto können Sie:

- Virtuelle Computer für die Updateverwaltung aktivieren
- Den Status verfügbarer Updates bewerten
- Die Installation erforderlicher Updates planen
- Bereitstellungsergebnisse überprüfen, um sicherzustellen, dass Updates erfolgreich auf alle virtuellen Computer angewendet wurden, für die die Updateverwaltung aktiviert ist

Informationen zu den Systemanforderungen für die Updateverwaltung finden Sie unter [Clientanforderungen für die Updateverwaltung](automation-update-management.md#client-requirements).

## <a name="prerequisites"></a>Voraussetzungen

* Ein virtueller Computer bzw. ein Computer, auf dem eines der unterstützten Betriebssysteme installiert ist
* Zugriff auf ein Updaterepository für virtuelle Linux-Computer, für die die Updateverwaltung aktiviert ist

## <a name="enable-update-management-for-azure-vms"></a>Aktivieren der Updateverwaltung für virtuelle Azure-Computer

1. Öffnen Sie im Azure-Portal Ihr Automation-Konto, und wählen Sie dann **Updateverwaltung** aus.

2. Wählen Sie **Azure-VMs hinzufügen** aus.

    ![Registerkarte „Azure-VM hinzufügen“](./media/manage-update-multi/update-onboard-vm.png)

3. Wählen Sie einen virtuellen Computer für die Aktivierung und anschließend unter **Updateverwaltung aktivieren** die Option **Aktivieren** aus.

    ![Dialogfeld „Updateverwaltung aktivieren“](./media/manage-update-multi/update-enable.png)

    Wenn der Vorgang abgeschlossen ist, ist die Updateverwaltung auf Ihrem virtuellen Computer aktiviert.

## <a name="enable-update-management-for-non-azure-vms-and-computers"></a>Aktivieren der Updateverwaltung für virtuelle Computer und Computer, die nicht Azure unterstehen

Der Log Analytics-Agent für Windows und Linux muss auf den VMS installiert werden, die in Ihrem Unternehmensnetzwerk oder in einer anderen Cloudumgebung ausgeführt werden, um Sie für die Updateverwaltung zu aktivieren. Informationen zu den Systemanforderungen und unterstützten Methoden für die Bereitstellung des Agent auf außerhalb von Azure gehosteten Computern finden Sie unter [Übersicht zum Log Analytics-Agent](../azure-monitor/platform/log-analytics-agent.md).

## <a name="view-computers-attached-to-your-automation-account"></a>Anzeigen von Computern, die an Ihr Automation-Konto angefügt sind

Nachdem Sie die Updateverwaltung für Ihre Computer aktiviert haben, können Sie Computerinformationen anzeigen, indem Sie **Computer** auswählen. Folgende Informationen werden für Ihre Computer angezeigt: Computername, Compliancestatus, Umgebung, Betriebssystemtyp, installierte kritische und Sicherheitsupdates, andere installierte Updates sowie Bereitschaft des Update-Agents.

  ![Registerkarte „Computer“](./media/manage-update-multi/update-computers-tab.png)

Für Computer, für die die Updateverwaltung erst vor Kurzem aktiviert wurde, ist ggf. noch keine Bewertung vorhanden. Der Compliancestatus dieser Computer lautet `Not assessed`. Es folgt eine Liste mit möglichen Werten für den Konformitätsstatus:

- `Compliant`: Computer, für die keine kritischen Updates oder Sicherheitsupdates fehlen.
- `Non-compliant`: Computer, für die mindestens ein kritisches Update oder Sicherheitsupdate fehlt.
- `Not assessed`: Die Daten für die Updatebewertung wurden vom Computer nicht innerhalb des erwarteten Zeitrahmens empfangen. Bei Linux-Computern liegt der erwartete Zeitrahmen in der letzten Stunde. Bei Windows-Computer liegt der erwartete Zeitrahmen in den letzten 12 Stunden.

Klicken Sie zum Anzeigen des Agent-Status auf den Link in der Spalte **Bereitschaft des Update-Agents**. Bei Auswahl dieser Option wird der Bereich „Hybrid Worker“ geöffnet und der Status des Hybrid Workers angezeigt. In der folgenden Abbildung wird ein Beispiel für einen Agent gezeigt, der über einen längeren Zeitraum nicht mit der Updateverwaltung verbunden war:

![Registerkarte „Computer“](./media/manage-update-multi/update-agent-broken.png)

## <a name="view-an-update-assessment"></a>Anzeigen einer Updatebewertung

Sobald die Updateverwaltung aktiviert ist, wird der Bereich „Updateverwaltung“ angezeigt. Auf der Registerkarte **Fehlende Updates** wird eine Liste der fehlenden Updates angezeigt.

## <a name="collect-data"></a>Sammeln von Daten

Mit Agents, die auf virtuellen Computern und Computern installiert sind, werden Daten zu Updates gesammelt. Die Agents senden die Daten an die Azure-Updateverwaltung.

### <a name="supported-agents"></a>Unterstützte Agents

In der folgenden Tabelle sind die verbundenen Quellen beschrieben, die von der Updateverwaltung unterstützt werden:

| Verbundene Quelle | Unterstützt | BESCHREIBUNG |
| --- | --- | --- |
| Windows-Agents |Ja |Die Updateverwaltung sammelt Informationen zu Systemupdates von Windows-Agents und initiiert dann die Installation von erforderlichen Updates. |
| Linux-Agents |Ja |Die Updateverwaltung sammelt Informationen zu Systemupdates von Linux-Agents und initiiert dann die Installation von erforderlichen Updates für unterstützte Distributionen. |
| Operations Manager-Verwaltungsgruppe |Ja |Die Updateverwaltung sammelt Informationen zu Systemupdates von Agents in einer verbundenen Verwaltungsgruppe. |
| Azure-Speicherkonto |Nein |Azure Storage enthält keine Informationen zu Systemupdates. |

### <a name="collection-frequency"></a>Sammlungshäufigkeit

Nachdem ein Computer einen Scanvorgang abgeschlossen hat, um die Konformität für das Update zu überprüfen, leitet der Agent die Informationen gesammelt an Azure Monitor-Protokolle weiter. Auf einem Windows-Computer wird der Konformitätsscan standardmäßig alle 12 Stunden ausgeführt.

Darüber hinaus wird der Update-Konformitätsscan innerhalb von 15 Minuten nach dem MMA-Neustart sowie vor und nach der Updateinstallation initiiert.

Für einen Linux-Computer wird der Konformitätsscan standardmäßig jede Stunde durchgeführt. Wenn der MMA-Agent neu gestartet wird, wird ein Konformitätsscan innerhalb von 15 Minuten eingeleitet.

Es kann zwischen 30 Minuten und sechs Stunden dauern, bis im Dashboard aktualisierte Daten von verwalteten Computern angezeigt werden.

## <a name="schedule-an-update-deployment"></a>Planen einer Updatebereitstellung

Planen Sie zum Installieren von Updates eine Bereitstellung, die Ihrem Releasezeitplan und Wartungsfenster entspricht. Sie können auswählen, welche Updatetypen in die Bereitstellung eingeschlossen werden sollen. Beispielsweise können Sie kritische oder Sicherheitsupdates einschließen und Updaterollups ausschließen.

>[!NOTE]
>Bei der Planung einer Updatebereitstellung wird eine [Zeitplanressource](shared-resources/schedules.md) erstellt, die mit dem Runbook **Patch-MicrosoftOMSComputers** verknüpft ist, das die Updatebereitstellung auf den Zielcomputern verarbeitet. Wenn Sie die Zeitplanressource nach der Bereitstellungserstellung über das Azure-Portal oder mithilfe von PowerShell löschen, wird die geplante Updatebereitstellung unterbrochen, und es wird ein Fehler angezeigt, wenn Sie sie nochmal über das Portal konfigurieren möchten. Sie können die Zeitplanressource nur löschen, indem Sie den entsprechenden Bereitstellungszeitplan löschen.
>

Um eine neue Updatebereitstellung für einen oder mehrere virtuelle Computer zu planen, wählen Sie unter **Updateverwaltung** die Option **Updatebereitstellung planen** aus.

Geben Sie im Bereich **Neue Updatebereitstellung** die folgenden Informationen ein:

- **Name**: Geben Sie einen eindeutigen Namen zur Identifizierung der Updatebereitstellung ein.
- **Betriebssystem**: Wählen Sie **Windows** oder **Linux** aus.
- **Zu aktualisierende Gruppen**: Definieren Sie eine Abfrage basierend auf einer Kombination aus Abonnement, Ressourcengruppen, Standorten und Tags, um eine dynamische Gruppe von Azure-VMs zu erstellen, die in Ihre Bereitstellung eingeschlossen werden sollen. Bei Nicht-Azure-VMs werden gespeicherte Suchvorgänge verwendet, um eine dynamische Gruppe zu erstellen, die in Ihre Bereitstellung eingeschlossen werden soll. Weitere Informationen finden Sie unter [Dynamische Gruppen](automation-update-management-groups.md).
- **Zu aktualisierende Computer**: Wählen Sie eine gespeicherte Suche, eine importierte Gruppe oder Computer aus, um die Computer auszuwählen, die Sie aktualisieren möchten.

   >[!NOTE]
   >Wenn Sie die Option „Gespeicherte Suche“ auswählen, werden keine Computeridentitäten, sondern nur ihre Namen zurückgegeben. Wenn Sie über mehrere virtuelle Computer mit demselben Namen in mehreren Ressourcengruppen verfügen, werden diese in den Ergebnissen zurückgegeben. Die Verwendung der Option **Zu aktualisierende Gruppen** wird empfohlen, um sicherzustellen, dass Sie eindeutige VMS einschließen, die Ihren Kriterien entsprechen.

   Wenn Sie **Computer** auswählen, wird die Bereitschaft des Computers in der Spalte **Update-Agent-Bereitschaft** angezeigt. Sie können den Integritätsstatus des Computers sehen, bevor Sie die Updatebereitstellung planen. Weitere Informationen zu den verschiedenen Methoden zum Erstellen von Computergruppen in Azure Monitor-Protokollen finden Sie unter [Computergruppen in Azure Monitor-Protokollen](../azure-monitor/platform/computer-groups.md).

  ![Bereich „Neue Updatebereitstellung“](./media/manage-update-multi/update-select-computers.png)

- **Updateklassifizierung**: Wählen Sie die Softwaretypen aus, die in die Updatebereitstellung eingeschlossen werden sollen. Eine Beschreibung der Klassifizierungstypen finden Sie unter [Updateklassifizierungen](automation-view-update-assessments.md#work-with-update-classifications). Es gibt die folgenden Klassifizierungstypen:
  - Kritische Updates
  - Sicherheitsupdates
  - Updaterollups
  - Feature Packs
  - Service Packs
  - Definitionsupdates
  - Tools
  - Aktualisierungen

- **Einzuschließende/auszuschließende Updates**: Öffnet die Seite „Einschließen/ausschließen“. Updates, die eingeschlossen oder ausgeschlossen werden sollen, befinden sich auf verschiedenen Registerkarten. Weitere Informationen zur Vorgehensweise beim Einschließen finden Sie unter [Planen einer Updatebereitstellung](automation-tutorial-update-management.md#schedule-an-update-deployment).

> [!NOTE]
> Ausschlüsse haben eine höhere Priorität als Einschlüsse. Wenn Sie beispielsweise die Ausschlussregel `*` definieren, werden keine Patches oder Pakete installiert, da sie alle ausgeschlossen wurden. Ausgeschlossene Patches werden weiterhin als auf dem Computer nicht vorhanden angezeigt. Wenn auf Linux-Computern ein Paket eingeschlossen wird, das jedoch eine Abhängigkeit zu einem ausgeschlossenen Paket aufweist, wird das Paket nicht installiert.

> [!NOTE]
> Sie können keine Updates angeben, die für die Aufnahme in die Updatebereitstellung ersetzt wurden.

- **Zeitplaneinstellungen**: Sie können das Standarddatum und die Standarduhrzeit (30 Minuten nach der aktuellen Zeit) übernehmen. Sie können auch eine andere Zeit angeben.

   Sie können auch angeben, ob die Bereitstellung einmalig erfolgt, oder einen sich wiederholenden Zeitplan einrichten. Wählen Sie zum Einrichten eines sich wiederholenden Zeitplans unter **Wiederholung** die Option **Serie** aus.

   ![Dialogfeld „Zeitplaneinstellungen“](./media/manage-update-multi/update-set-schedule.png)

- **Vor und nach dem Vorgang auszuführende Skripts**: Wählen Sie die Skripts aus, die vor und nach Ihrer Bereitstellung ausgeführt werden sollen. Weitere Informationen finden Sie unter [Verwalten von Pre- und Post-Skripts](pre-post-scripts.md).
- **Wartungsfenster (Minuten)** : Geben Sie den Zeitraum an, in dem die Updatebereitstellung stattfinden soll. Mit dieser Einstellung können Sie sicherstellen, dass Änderungen in den von Ihnen festgelegten Wartungsfenstern ausgeführt werden.

- **Neustartsteuerung**: Diese Einstellung bestimmt, wie Neustarts für die Updatebereitstellung behandelt werden.

   |Option|BESCHREIBUNG|
   |---|---|
   |Neustart bei Bedarf| **(Standard)** : Bei Bedarf wird ein Neustart eingeleitet, wenn das Wartungsfenster dies zulässt.|
   |Immer neu starten|Ein Neustart wird unabhängig davon eingeleitet, ob er erforderlich ist. |
   |Nie neu starten|Unabhängig davon, ob ein Neustart erforderlich ist, werden Neustarts unterdrückt.|
   |Nur neu starten – Updates werden nicht installiert|Diese Option ignoriert die Installation von Updates und leitet nur einen Neustart ein.|

Nachdem Sie die Konfiguration des Zeitplans abgeschlossen haben, klicken Sie auf die Schaltfläche **Erstellen**, um zum Statusdashboard zurückzukehren. Die Tabelle **Geplant** zeigt den von Ihnen erstellten Bereitstellungszeitplan.

> [!NOTE]
> Die Updateverwaltung unterstützt die Bereitstellung von Erstanbieterupdates sowie Vorabdownloads von Patches. Hierzu sind Änderungen an den Systemen erforderlich, die gepatcht werden. Informationen zum Konfigurieren der entsprechenden Einstellungen für Ihre Systeme finden Sie im [Abschnitt zu Erstanbieterpatches und Vorabdownloads](automation-configure-windows-update.md#pre-download-updates).

## <a name="view-results-of-an-update-deployment"></a>Anzeigen der Ergebnisse einer Updatebereitstellung

Nach dem Start der geplanten Bereitstellung sehen Sie den Status der Bereitstellung auf der Registerkarte **Updatebereitstellungen** unter **Updateverwaltung**.

Wenn die Bereitstellung derzeit ausgeführt wird, lautet der Status **In Bearbeitung**. Nachdem die Bereitstellung erfolgreich abgeschlossen wurde, ändert sich der Status in **Erfolgreich**.

Wenn bei einem oder mehreren Updates in der Bereitstellung ein Fehler auftritt, wird der Status **Der Vorgang ist teilweise fehlgeschlagen** angezeigt.

![Status der Updatebereitstellung](./media/manage-update-multi/update-view-results.png)

Klicken Sie auf die abgeschlossene Bereitstellung, um das Dashboard für eine Updatebereitstellung anzuzeigen.

Im Bereich „Updateergebnisse“ werden die Gesamtzahl von Updates und die Ergebnisse der Bereitstellung für den virtuellen Computer angezeigt. Die Tabelle rechts enthält eine detaillierte Aufschlüsselung der einzelnen Updates und die Installationsergebnisse. Bei den Installationsergebnissen kann es sich um einen der folgenden Werte handeln:

- `Not attempted`: Das Update wurde nicht installiert, da aufgrund des definierten Wartungsfensters nicht genügend Zeit zur Verfügung stand.
- `Succeeded`: Das Update war erfolgreich.
- `Failed`: Beim Update ist ein Fehler aufgetreten.

Klicken Sie auf **Alle Protokolle**, um alle von der Bereitstellung erstellten Protokolleinträge anzuzeigen.

Wählen Sie die Ausgabekachel aus, um den Auftragsdatenstrom des Runbooks anzuzeigen, das die Updatebereitstellung auf dem virtuellen Zielcomputer verwaltet.

Klicken Sie auf **Fehler**, um ausführliche Informationen zu Fehlern bei der Bereitstellung anzuzeigen.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zum Durchsuchen von Updateprotokollen finden Sie unter [Abfragen von Updatedatensätzen für die Updateverwaltung in Azure Monitor-Protokollen](automation-update-management-query-logs.md).
