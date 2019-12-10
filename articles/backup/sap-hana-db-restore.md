---
title: Wiederherstellen von SAP HANA-Datenbanken auf virtuellen Azure-Computern
description: In diesem Artikel erfahren Sie, wie Sie SAP HANA-Datenbanken wiederherstellen, die in Azure Virtual Machines ausgeführt werden.
ms.topic: conceptual
ms.date: 11/7/2019
ms.openlocfilehash: 999edba61177758ad9039e81e789efcef99ca1de
ms.sourcegitcommit: e50a39eb97a0b52ce35fd7b1cf16c7a9091d5a2a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/21/2019
ms.locfileid: "74287388"
---
# <a name="restore-sap-hana-databases-on-azure-vms"></a>Wiederherstellen von SAP HANA-Datenbanken auf virtuellen Azure-Computern

In diesem Artikel wird die Wiederherstellung von SAP HANA-Datenbanken beschrieben, die auf einem virtuellen Azure-Computer (VM) ausgeführt werden und mithilfe des Diensts Azure Backup in einem Azure Backup Recovery Services-Tresor gesichert wurden. Mit Wiederherstellungen können Kopien der Daten für Entwicklungs-/Testszenarien erstellt oder Rücksetzungen auf einen früheren Zustand ausgeführt werden.

Weitere Informationen zum Sichern von SAP HANA-Datenbanken finden Sie unter [Sichern von SAP HANA-Datenbanken auf virtuellen Azure-Computern](https://docs.microsoft.com/azure/backup/backup-azure-sap-hana-database).

## <a name="restore-to-a-point-in-time-or-to-a-recovery-point"></a>Wiederherstellen eines Zeitpunkts oder eines Wiederherstellungspunkts

Azure Backup kann auf virtuellen Azure-Computern ausgeführte SAP HANA-Datenbanken wie folgt wiederherstellen:

* Wiederherstellung eines bestimmten Datums oder einer bestimmten Uhrzeit (sekundengenau) mithilfe von Protokollsicherungen. Azure Backup ermittelt automatisch die geeigneten vollständige differenziellen Sicherungen und die Kette von Protokollsicherungen, die für die Wiederherstellung Ihrer Daten basierend auf dem ausgewählten Zeitpunkt benötigt werden.

* Wiederherstellung einer bestimmten vollständigen oder differenziellen Sicherung, um die Daten eines bestimmten Wiederherstellungspunkts wiederherzustellen.

## <a name="prerequisites"></a>Voraussetzungen

Beachten Sie vor dem Wiederherstellen einer Datenbank Folgendes:

* Sie können die Datenbank nur in einer SAP HANA-Instanz wiederherstellen, die sich in derselben Region befindet.

* Die Zielinstanz muss beim gleichen Tresor wie die Quelle registriert sein.

* Azure Backup kann keine zwei verschiedenen SAP HANA-Instanzen auf derselben VM identifizieren. Daher ist das Wiederherstellen von Daten aus einer Instanz in einer anderen auf demselben virtuellen Computer nicht möglich.

* Um sicherzustellen, dass die SAP HANA-Zielinstanz bereit für eine Wiederherstellung ist, überprüfen Sie ihren Status **Sicherungsbereitschaft**:

  * Öffnen Sie den Tresor, in dem die SAP HANA-Zielinstanz registriert ist.

  * Wählen Sie im Dashboard des Tresors unter **Erste Schritte** die Option **Sicherung** aus.

![„Sicherung“ im Dashboard des Tresors](media/sap-hana-db-restore/getting-started-backup.png)

* Wählen Sie in **Sicherung** unter **Was möchten Sie sichern?** die Option **SAP HANA in Azure-VM** aus.

![Auswählen von „SAP HANA in Azure-VM“](media/sap-hana-db-restore/sap-hana-backup.png)

* Klicken Sie unter **DBs in VMs ermitteln** auf **Details anzeigen**

![Anzeigen der Details](media/sap-hana-db-restore/view-details.png)

* Überprüfen Sie die **Sicherungsbereitschaft** der Ziel-VM.

![Geschützte Server](media/sap-hana-db-restore/protected-servers.png)

* Weitere Informationen zu den von SAP HANA unterstützten Wiederherstellungstypen finden Sie in SAP HANA-Hinweis [1642148](https://launchpad.support.sap.com/#/notes/1642148)

## <a name="restore-a-database"></a>Wiederherstellen einer Datenbank

* Öffnen Sie den Tresor, in dem die wiederherzustellende SAP HANA-Datenbank registriert ist.

* Wählen Sie im Tresor-Dashboard unter **Geschützte Elemente** die Option **Sicherungselemente** aus.

![Sicherungselemente](media/sap-hana-db-restore/backup-items.png)

* Wählen Sie im Menü **Sicherungselemente** unter **Sicherungsverwaltungstyp** die Option **SAP HANA in Azure-VM** aus.

![Sicherungsverwaltungstyp](media/sap-hana-db-restore/backup-management-type.png)

* Auswählen der wiederherzustellenden Datenbank

 ![Wiederherzustellende Datenbank](media/sap-hana-db-restore/database-to-restore.png)

* Überprüfen Sie das Datenbankmenü. Es enthält folgende Informationen über die Datenbanksicherung:

  * Die ältesten und neuesten Wiederherstellungspunkte

  * Den Protokollsicherungsstatus für die letzten 24 und 72 Stunden für die Datenbank

![Menü „Datenbank“](media/sap-hana-db-restore/database-menu.png)

* Wählen Sie **Datenbank wiederherstellen** aus.

* Geben Sie unter **Wiederherstellungskonfiguration** an, wo (oder wie) die Daten wiederhergestellt werden sollen:

  * **Alternativer Standort:** Die Datenbank wird an einem alternativen Speicherort wiederhergestellt, und die ursprüngliche Quelldatenbank bleibt erhalten.

  * **Datenbank überschreiben:** Die Daten werden auf derselben SAP HANA-Instanz wiederhergestellt, auf der sich auch die ursprüngliche Quelle befunden hat. Bei dieser Option wird die ursprüngliche Datenbank überschrieben.

![Wiederherstellungskonfiguration](media/sap-hana-db-restore/restore-configuration.png)

### <a name="restore-to-alternate-location"></a>Wiederherstellen an einem alternativen Speicherort

* Wählen Sie im Menü **Wiederherstellungskonfiguration** unter **Zielort der Wiederherstellung** die Option **Alternativer Speicherort** aus.

![Wiederherstellen an einem alternativen Speicherort](media/sap-hana-db-restore/restore-alternate-location.png)

* Wählen Sie den SAP HANA-Hostnamen und die Instanz aus, in der Sie die Datenbank wiederherstellen möchten.
* Überprüfen Sie, ob die SAP HANA-Zielinstanz bereit für die Wiederherstellung ist, indem Sie ihre **Sicherungsbereitschaft** sicherstellen. Weitere Informationen finden Sie im Abschnitt zu [Voraussetzungen](#prerequisites).
* Geben Sie im Feld **Name der wiederhergestellten Datenbank** den Namen der Zieldatenbank ein.

> [!NOTE]
> SDC-Wiederherstellungen (Single Database Container, Einzeldatenbank-Container) müssen diesen [Überprüfungen](backup-azure-sap-hana-database-troubleshoot.md#single-container-database-sdc-restore) folgen.

* Aktivieren Sie ggf. **Überschreiben, wenn die gleichnamige Datenbank bereits in der ausgewählten HANA-Instanz vorhanden ist**.
* Klicken Sie auf **OK**.

![Wiederherstellungskonfiguration – abschließender Bildschirm](media/sap-hana-db-restore/restore-configuration-last.png)

* Wählen Sie unter **Wiederherstellungspunkt auswählen** die Option **Protokolle (Zeitpunkt)** aus, um [einen bestimmten Zeitpunkt wiederherzustellen](#restore-to-a-specific-point-in-time). Oder wählen Sie **Vollständig und differenziell** aus, um einen [bestimmten Wiederherstellungspunkt](#restore-to-a-specific-recovery-point) wiederherzustellen.

### <a name="restore-and-overwrite"></a>Wiederherstellen und Überschreiben

* Wählen Sie im Menü **Wiederherstellungskonfiguration** unter **Zielort der Wiederherstellung** die Option **Datenbank überschreiben** > **OK** aus.

![Datenbank überschreiben](media/sap-hana-db-restore/overwrite-db.png)

* Wählen Sie unter **Wiederherstellungspunkt auswählen** die Option **Protokolle (Zeitpunkt)** aus, um [einen bestimmten Zeitpunkt wiederherzustellen](#restore-to-a-specific-point-in-time). Oder wählen Sie **Vollständig und differenziell** aus, um einen [bestimmten Wiederherstellungspunkt](#restore-to-a-specific-recovery-point) wiederherzustellen.

### <a name="restore-to-a-specific-point-in-time"></a>Wiederherstellen eines bestimmten Zeitpunkts

Wenn Sie **Protokolle (Zeitpunkt)** als Wiederherstellungstyp ausgewählt haben, gehen Sie folgendermaßen vor:

* Wählen Sie einen Wiederherstellungspunkt aus dem Protokolldiagramm aus, und klicken Sie auf **OK**, um den Wiederherstellungspunkt auszuwählen.

![Wiederherstellungspunkt](media/sap-hana-db-restore/restore-point.png)

* Wählen Sie im Menü **Wiederherstellen** **Wiederherstellen** aus, um den Wiederherstellungsauftrag zu starten.

![Auswählen von „Wiederherstellen“](media/sap-hana-db-restore/restore-restore.png)

* Verfolgen Sie den Fortschritt der Wiederherstellung im Bereich **Benachrichtigungen**, oder wählen Sie im Datenbankmenü die Option **Wiederherstellungsaufträge** aus.

![Wiederherstellung wurde erfolgreich ausgelöst](media/sap-hana-db-restore/restore-triggered.png)

### <a name="restore-to-a-specific-recovery-point"></a>Wiederherstellen eines bestimmten Wiederherstellungspunkts

Wenn Sie **Vollständig und differenziell** als Wiederherstellungstyp ausgewählt haben, gehen Sie folgendermaßen vor:

* Wählen Sie einen Wiederherstellungspunkt aus der Liste aus, und klicken Sie auf **OK**, um den Wiederherstellungspunkt auszuwählen.

![Wiederherstellen eines bestimmten Wiederherstellungspunkts](media/sap-hana-db-restore/specific-recovery-point.png)

* Wählen Sie im Menü **Wiederherstellen** **Wiederherstellen** aus, um den Wiederherstellungsauftrag zu starten.

![Starten des Wiederherstellungsauftrags](media/sap-hana-db-restore/restore-specific.png)

* Verfolgen Sie den Fortschritt der Wiederherstellung im Bereich **Benachrichtigungen**, oder wählen Sie im Datenbankmenü die Option **Wiederherstellungsaufträge** aus.

![Status der Wiederherstellung](media/sap-hana-db-restore/restore-progress.png)

> [!NOTE]
> Bei MDC-Wiederherstellungen (Multiple Database Container, Container mit mehreren Datenbanken) muss das Vorregistrierungsskript erneut ausgeführt werden, nachdem die Systemdatenbank auf einer Zielinstanz wiederhergestellt wurde. Nur dann ist die nachfolgende Wiederherstellung der Mandantendatenbanken erfolgreich. Weitere Informationen finden Sie unter [Problembehandlung – MDC-Wiederherstellungen](backup-azure-sap-hana-database-troubleshoot.md#multiple-container-database-mdc-restore).

## <a name="next-steps"></a>Nächste Schritte

* [Erfahren Sie, wie Sie ](sap-hana-db-manage.md)mit Azure Backup gesicherte SAP HANA-Datenbanken verwalten.
