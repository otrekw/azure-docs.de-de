---
title: Verwalten von gesicherten SAP HANA-Datenbanken für Azure-VMs
description: In diesem Artikel erfahren Sie mehr über allgemeine Aufgaben zum Verwalten und Überwachen von SAP HANA-Datenbanken, die auf virtuellen Azure-Computern ausgeführt werden.
ms.topic: conceptual
ms.date: 11/12/2019
ms.openlocfilehash: f76054c7c78c55a9754975267ee4fa3caab968a3
ms.sourcegitcommit: e50a39eb97a0b52ce35fd7b1cf16c7a9091d5a2a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/21/2019
ms.locfileid: "74287520"
---
# <a name="manage-and-monitor-backed-up-sap-hana-databases"></a>Verwalten und Überwachen gesicherter SAP HANA-Datenbanken

In diesem Artikel werden allgemeine Aufgaben zur Verwaltung und Überwachung von SAP HANA-Datenbanken beschrieben, die auf einem virtuellen Azure-Computer (Virtual Machine, VM) ausgeführt und mit dem [Azure Backup](https://docs.microsoft.com/azure/backup/backup-overview)-Dienst in einem Azure Backup Recovery Services-Tresor gesichert werden. Sie erfahren, wie Sie Aufträge und Warnungen überwachen, eine bedarfsgesteuerte Sicherung auslösen, Richtlinien bearbeiten, Datenbankschutz beenden und fortsetzen sowie die Registrierung einer VM für Sicherungen aufheben.

Wenn Sie noch keine Sicherungen für Ihre SAP HANA-Datenbanken konfiguriert haben, finden Sie Näheres dazu unter [Informationen zur SAP HANA-Sicherung auf virtuellen Azure-Computern](https://docs.microsoft.com/azure/backup/backup-azure-sap-hana-database).

## <a name="monitor-manual-backup-jobs-in-the-portal"></a>Überwachen manueller Sicherungsaufträge im Portal

Azure Backup zeigt alle manuell ausgelösten Aufträge im Azure-Portal im Abschnitt **Sicherungsaufträge** an.

![Abschnitt „Sicherungsaufträge“](./media/sap-hana-db-manage/backup-jobs.png)

Zu den Aufträgen, die in diesem Portal angezeigt werden, gehören Datenbankermittlung und -registrierung sowie Sicherungs- und Wiederherstellungsvorgänge. Geplante Aufträge (einschließlich Protokollsicherungen) werden in diesem Abschnitt nicht angezeigt. Manuell ausgelöste Sicherungen aus den nativen SAP HANA-Clients (Studio/Cockpit/DBA Cockpit) werden hier ebenfalls nicht angezeigt.

![Liste „Sicherungsaufträge“](./media/sap-hana-db-manage/backup-jobs-list.png)

Weitere Informationen zur Überwachung finden Sie unter [Überwachung im Azure-Portal](https://docs.microsoft.com/azure/backup/backup-azure-monitoring-built-in-monitor) und [Überwachen im richtigen Maßstab mithilfe von Azure Monitor](https://docs.microsoft.com/azure/backup/backup-azure-monitoring-use-azuremonitor).

## <a name="view-backup-alerts"></a>Anzeigen von Sicherungswarnungen

Warnungen sind eine einfache Möglichkeit zum Überwachen von Sicherungen von SAP HANA-Datenbanken. Warnungen unterstützen Sie dabei, sich auf die Ereignisse zu konzentrieren, die Sie am meisten interessieren, ohne den Überblick über die unzähligen von einer Sicherung generierten Ereignisse zu verlieren. Azure Backup ermöglicht es Ihnen, Warnungen festzulegen, und Sie können diese wie folgt überwachen:

* Melden Sie sich am [Azure-Portal](https://portal.azure.com/) an.
* Wählen Sie auf dem Tresordashboard die Option **Sicherungswarnungen** aus.

  ![Sicherungswarnungen auf dem Tresordashboard](./media/sap-hana-db-manage/backup-alerts-dashboard.png)

* Die Warnungen können angezeigt werden:

  ![Liste der Sicherungswarnungen](./media/sap-hana-db-manage/backup-alerts-list.png)

* Klicken Sie auf die Warnungen, um weitere Details anzuzeigen:

  ![Warnungsdetails](./media/sap-hana-db-manage/alert-details.png)

Heute ermöglicht Azure Backup das Senden von Warnungen per E-Mail. Es handelt sich um die folgenden Warnungen:

* Für alle Sicherungsfehler ausgelöst.
* Auf Datenbankebene nach Fehlercode konsolidiert.
* Nur beim ersten Sicherungsfehler einer Datenbank gesendet.

Weitere Informationen zur Überwachung finden Sie unter [Überwachung im Azure-Portal](https://docs.microsoft.com/azure/backup/backup-azure-monitoring-built-in-monitor) und [Überwachen im richtigen Maßstab mithilfe von Azure Monitor](https://docs.microsoft.com/azure/backup/backup-azure-monitoring-use-azuremonitor).

## <a name="management-operations"></a>Verwaltungsvorgänge

Mit Azure Backup wird die Verwaltung einer gesicherten SAP HANA-Datenbank mit einer Vielzahl von Verwaltungsvorgängen vereinfacht, die unterstützt werden. Diese Vorgänge werden in den folgenden Abschnitten ausführlicher erläutert.

### <a name="run-an-ad-hoc-backup"></a>Ausführen einer Ad-hoc-Sicherung

Sicherungen werden gemäß dem Richtlinienzeitplan ausgeführt. Eine bedarfsgesteuerte Sicherung können Sie wie folgt ausführen:

1. Klicken Sie im Tresormenü auf **Sicherungselemente**.
2. Wählen Sie unter **Sicherungselemente** den virtuellen Computer aus, auf dem die SAP HANA-Datenbank ausgeführt wird, und klicken Sie dann auf **Jetzt sichern**.
3. Verwenden Sie unter **Jetzt sichern** den Kalender, um den letzten Tag zur Beibehaltung des Wiederherstellungspunkts auszuwählen. Klicken Sie dann auf **OK**.
4. Überwachen Sie die Portalbenachrichtigungen. Sie können den Auftragsstatus im Dashboard des Tresors unter **Sicherungsaufträge** > **In Bearbeitung** überwachen. Je nach Größe Ihrer Datenbank kann das Erstellen der ersten Sicherung einige Zeit dauern.

### <a name="run-sap-hana-native-client-backup-on-a-database-with-azure-backup-enabled"></a>Ausführen einer Sicherung des nativen SAP HANA-Clients für eine Datenbank, für die Azure Backup aktiviert ist

Führen Sie die folgenden Schritte aus, wenn Sie (mit HANA Studio/Cockpit) eine lokale Sicherung einer Datenbank ausführen möchten, die mit Azure Backup gesichert wird:

1. Warten Sie, bis alle vollständigen Sicherungen oder Protokollsicherungen für die Datenbank abgeschlossen sind. Überprüfen Sie den Status in SAP HANA Studio/Cockpit.
2. Deaktivieren Sie Protokollsicherungen, und legen Sie den Sicherungskatalog auf das Dateisystem für die entsprechende Datenbank fest.
3. Doppelklicken Sie hierzu auf **systemdb** > **Konfiguration** > **Datenbank auswählen** > **Filter (Protokoll)** .
4. Legen Sie **enable_auto_log_backup** auf **No** fest.
5. Legen Sie **log_backup_using_backint** auf **False** fest.
6. Erstellen Sie eine vollständige Ad-hoc-Sicherung der Datenbank.
7. Warten Sie, bis die vollständige Sicherung und die Katalogsicherung abgeschlossen sind.
8. Stellen Sie die vorherigen Einstellungen für Azure wieder her:
   * Legen Sie **enable_auto_log_backup** auf **Yes** fest.
   * Legen Sie **log_backup_using_backint** auf **True** fest.

### <a name="edit-underlying-policy"></a>Bearbeiten der zugrunde liegenden Richtlinie

Bearbeiten Sie die Richtlinie, um die Sicherungshäufigkeit oder die Aufbewahrungsdauer zu ändern:

* Navigieren Sie im Tresordashboard zu **Verwalten** > **Sicherungsrichtlinien**.

  ![Sicherungsrichtlinien im Tresordashboard](./media/sap-hana-db-manage/backup-policies-dashboard.png)

* Wählen Sie die Richtlinie aus, die Sie bearbeiten möchten:

  ![Liste der Sicherungsrichtlinien](./media/sap-hana-db-manage/backup-policies-list.png)

  ![Sicherungsrichtliniendetails](./media/sap-hana-db-manage/backup-policy-details.png)

>[!NOTE]
> Jede Änderung der Aufbewahrungsdauer wird nicht nur auf die neuen Wiederherstellungspunkte angewendet, sondern auch rückwirkend auf alle älteren.
>
> Inkrementelle Sicherungsrichtlinien können nicht für SAP HANA-Datenbanken verwendet werden. Inkrementelle Sicherungen werden für diese Datenbanken derzeit nicht unterstützt.

### <a name="stop-protection-for-an-sap-hana-database"></a>Beenden des Schutzes für eine SAP HANA-Datenbank

Sie können den Schutz einer SAP HANA-Datenbank auf verschiedene Weise beenden:

* Beenden aller zukünftigen Sicherungsaufträge und Löschen aller Wiederherstellungspunkte
* Beenden aller zukünftigen Sicherungsaufträge und Beibehalten der Wiederherstellungspunkte.

Wenn Sie die Wiederherstellungspunkte beibehalten, sollten Sie Folgendes beachten:

* Alle Wiederherstellungspunkte werden unbegrenzt beibehalten, und die Bereinigung endet mit der Beendung des Schutzes unter Beibehaltung der Daten.
* Ihnen werden die geschützte Instanz und der verbrauchte Speicher in Rechnung gestellt. Weitere Informationen finden Sie unter [Azure Backup – Preise](https://azure.microsoft.com/pricing/details/backup/).
* Wenn Sie eine Datenquelle löschen, ohne die Sicherungen zu beenden, treten bei neuen Sicherungen Fehler auf.

Gehen Sie wie folgt vor, um den Schutz für eine Datenbank zu beenden:

* Wählen Sie auf dem Tresordashboard die Option **Sicherungselemente** aus.
* Wählen Sie unter **Sicherungsverwaltungstyp** die Option **SAP HANA in Azure-VM** aus.

  ![Auswählen von „SAP HANA in Azure-VM“](./media/sap-hana-db-manage/sap-hana-azure-vm.png)

* Wählen Sie die Datenbank aus, für die Sie den Schutz beenden möchten:

  ![Auswählen der Datenbank zum Beenden des Schutzes](./media/sap-hana-db-manage/select-database.png)

* Wählen Sie im Datenbankmenü **Sicherung beenden** aus.

  ![Auswählen von „Sicherung beenden“](./media/sap-hana-db-manage/stop-backup.png)

* Wählen Sie im Menü **Sicherung beenden** aus, ob Daten beibehalten oder gelöscht werden sollen. Geben Sie bei Bedarf einen Grund und einen Kommentar ein.

  ![Auswählen der Beibehaltung oder Löschung von Daten](./media/sap-hana-db-manage/retain-backup-data.png)

* Wählen Sie **Sicherung beenden** aus.

### <a name="resume-protection-for-an-sap-hana-database"></a>Fortsetzen des Schutzes für eine SAP HANA-Datenbank

Wenn Sie beim Beenden des Schutzes für die SAP HANA-Datenbank die Option **Sicherungsdaten beibehalten** auswählen, können Sie den Schutz später fortsetzen. Wenn Sie die gesicherten Daten nicht beibehalten, können Sie den Schutz nicht fortsetzen.

So setzen Sie den Schutz für eine SAP HANA-Datenbank fort:

* Öffnen Sie das Sicherungselement, und wählen Sie **Sicherung fortsetzen** aus.

   ![Auswählen der Fortsetzung einer Sicherung](./media/sap-hana-db-manage/resume-backup.png)

* Wählen Sie im Menü **Sicherungsrichtlinie** eine Richtlinie aus, und klicken Sie dann auf **Speichern**.

### <a name="upgrading-from-sap-hana-10-to-20"></a>Upgrade von SAP HANA 1.0 auf 2.0

Erfahren Sie, wie Sie nach einem [Upgrade von SAP Hana 1.0 auf 2.0](backup-azure-sap-hana-database-troubleshoot.md#upgrading-from-sap-hana-10-to-20) die Sicherung für eine SAP HANA-Datenbank fortsetzen.

### <a name="upgrading-without-a-sid-change"></a>Upgrade ohne SID-Änderung

Erfahren Sie, wie Sie die Sicherung einer SAP HANA-Datenbank fortsetzen, deren [SID sich nach dem Upgrade nicht geändert hat](backup-azure-sap-hana-database-troubleshoot.md#upgrading-without-an-sid-change).

### <a name="unregister-an-sap-hana-database"></a>Aufheben der Registrierung einer SAP HANA-Datenbank

Heben Sie die Registrierung einer SAP HANA-Instanz auf, nachdem Sie den Schutz deaktiviert haben, aber bevor Sie den Tresor löschen:

* Wählen Sie auf dem Tresordashboard unter **Verwalten** die Option **Sicherungsinfrastruktur** aus.

   ![Auswählen von „Sicherungsinfrastruktur“](./media/sap-hana-db-manage/backup-infrastructure.png)

* Wählen Sie den **Sicherungsverwaltungstyp** als **Workload in Azure-VM** aus.

   ![Auswählen des Sicherungsverwaltungstyp als „Workload in Azure-VM“](./media/sap-hana-db-manage/backup-management-type.png)

* Wählen Sie unter **Geschützte Server** die Instanz aus, deren Registrierung Sie aufheben möchten. Um den Tresor zu löschen, müssen Sie die Registrierung aller Server/Instanzen aufheben.

* Klicken Sie mit der rechten Maustaste auf die geschützten Instanz, und wählen Sie dann **Registrierung aufheben** aus.

   ![Auswählen der Aufhebung der Registrierung](./media/sap-hana-db-manage/unregister.png)

## <a name="next-steps"></a>Nächste Schritte

* Informieren Sie sich über das [Behandeln allgemeiner Probleme beim Sichern von SAP HANA-Datenbanken](https://docs.microsoft.com/azure/backup/backup-azure-sap-hana-database-troubleshoot).

