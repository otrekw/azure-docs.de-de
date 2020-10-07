---
title: Sichern eines Exchange-Servers mit Azure Backup Server
description: Erfahren Sie, wie Sie einen Exchange-Server mit Azure Backup Server in Azure Backup sichern.
ms.reviewer: kasinh
ms.topic: conceptual
ms.date: 03/24/2017
ms.openlocfilehash: 86389ef89ced74905db0fc8bfeaabcea6140762a
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91262993"
---
# <a name="back-up-an-exchange-server-to-azure-with-azure-backup-server"></a>Sichern eines Exchange-Servers in Azure mit Azure Backup Server

In diesem Artikel wird beschrieben, wie Sie einen Microsoft Azure Backup Server (MABS) konfigurieren, um einen Microsoft Exchange-Server in Azure zu sichern.  

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie fortfahren, stellen Sie sicher, dass Azure Backup Server [installiert und vorbereitet](backup-azure-microsoft-azure-backup.md) ist.

## <a name="mabs-protection-agent"></a>MABS-Schutz-Agent

Führen Sie die folgenden Schritte aus, um den MABS-Schutz-Agent auf dem Exchange-Server zu installieren:

1. Stellen Sie sicher, dass die Firewalls richtig konfiguriert sind. Weitere Informationen finden Sie unter [Konfigurieren von Firewallausnahmen für den Agent](/system-center/dpm/configure-firewall-settings-for-dpm).
2. Installieren Sie den Agent auf dem Exchange-Server, indem Sie in der MABS-Verwaltungskonsole **Verwaltung > Agents > Installieren** auswählen. Ausführliche Schritte finden Sie unter [Installieren des MABS-Schutz-Agents](/system-center/dpm/deploy-dpm-protection-agent).

## <a name="create-a-protection-group-for-the-exchange-server"></a>Erstellen einer Schutzgruppe für den Exchange-Server

1. Wählen Sie in der MABS-Verwaltungskonsole **Schutz** und dann im Menüband des Tools **Neu** aus, um den Assistenten **Neue Schutzgruppe erstellen** zu öffnen.
2. Wählen Sie im Assistenten auf dem Bildschirm **Willkommen** die Option **Weiter** aus.
3. Wählen Sie auf dem Bildschirm **Schutzgruppentyp auswählen** die Option **Server** und dann **Weiter** aus.
4. Wählen Sie die Datenbank des Exchange Servers aus, die Sie schützen möchten, und wählen Sie dann **Weiter** aus.

   > [!NOTE]
   > Wenn Sie Exchange 2013 schützen, aktivieren Sie die [Exchange 2013-Voraussetzungen](/system-center/dpm/back-up-exchange).
   >
   >

    Im folgenden Beispiel wird die Exchange 2010-Datenbank ausgewählt.

    ![Gruppenmitglieder auswählen](./media/backup-azure-backup-exchange-server/select-group-members.png)
5. Wählen Sie die Methoden zum Schützen der Daten aus.

    Geben Sie der Schutzgruppe einen Namen, und wählen Sie dann die beiden folgenden Optionen aus:

   * Kurzfristiger Schutz unter Verwendung eines Datenträgers
   * Onlineschutz
6. Wählen Sie **Weiter** aus.
7. Wählen Sie die Option **„Eseutil“ zum Prüfen der Datenintegrität ausführen** aus, wenn Sie die Integrität der Exchange-Server-Datenbanken überprüfen möchten.

    Nach dem Auswählen dieser Option wird auf dem MABS die Sicherungskonsistenz überprüft, um den E/A-Datenverkehr zu vermeiden, der durch das Ausführen des Befehls **eseutil** auf dem Exchange-Server generiert wird.

   > [!NOTE]
   > Zum Verwenden dieser Option müssen Sie die Dateien „Ese.dll“ und „Eseutil.exe“ auf dem MABS-Server in das Verzeichnis „C:\Programme\Microsoft Azure Backup\DPM\DPM\bin“ kopieren. Andernfalls wird der folgende Fehler ausgelöst:  
   > ![eseutil-Fehler](./media/backup-azure-backup-exchange-server/eseutil-error.png)
   >
   >
8. Wählen Sie **Weiter** aus.
9. Wählen Sie die Datenbank für **Kopiesicherung** und dann **Weiter**aus.

   > [!NOTE]
   > Wenn Sie nicht für mindestens eine DAG-Kopie einer Datenbank die Option „Vollständige Sicherung“ auswählen, werden die Protokolle nicht abgeschnitten.
   >
   >
10. Konfigurieren Sie die Ziele für **Kurzfristige Sicherung**, und wählen Sie **Weiter** aus.
11. Überprüfen Sie den verfügbaren Speicherplatz, und wählen Sie **Weiter** aus.
12. Wählen Sie den Zeitpunkt aus, an dem der MABS-Server die erste Replikation erstellt, und wählen Sie dann **Weiter** aus.
13. Wählen Sie die Optionen für die Konsistenzüberprüfung und dann **Weiter** aus.
14. Wählen Sie die Datenbank aus, die Sie unter Azure sichern möchten, und wählen Sie dann **Weiter** aus. Beispiel:

    ![Onlineschutzdaten angeben](./media/backup-azure-backup-exchange-server/specify-online-protection-data.png)
15. Definieren Sie den Zeitplan für **Azure Backup**, und wählen Sie **Weiter** aus. Beispiel:

    ![Zeitplan für Onlinesicherung angeben](./media/backup-azure-backup-exchange-server/specify-online-backup-schedule.png)

    > [!NOTE]
    > Beachten Sie, dass die Online-Wiederherstellungspunkte auf den Wiederherstellungspunkten für die schnelle vollständige Wiederherstellung basieren. Daher müssen Sie den Onlinewiederherstellungspunkt nach dem Zeitpunkt einplanen, der für den Wiederherstellungspunkt für die schnelle vollständige Wiederherstellung angegeben wird.
    >
    >
16. Konfigurieren Sie die Aufbewahrungsrichtlinie für **Azure Backup**, und wählen Sie **Weiter** aus.
17. Wählen Sie eine Option für die Onlinereplikation und dann **Weiter** aus.

    Wenn Sie über eine große Datenbank verfügen, kann es lange dauern, bis das erste Backup über das Netzwerk erstellt wird. Sie können ein Offlinebackup erstellen, um dieses Problem zu vermeiden.  

    ![Online-Aufbewahrungsrichtlinie angeben](./media/backup-azure-backup-exchange-server/specify-online-retention-policy.png)
18. Bestätigen Sie die Einstellungen, und wählen Sie **Gruppe erstellen** aus.
19. Klicken Sie auf **Schließen**.

## <a name="recover-the-exchange-database"></a>Wiederherstellen der Exchange-Datenbank

1. Wählen Sie zum Wiederherstellen einer Exchange-Datenbank in der MABS-Administratorkonsole **Wiederherstellung** aus.
2. Suchen Sie nach der Exchange-Datenbank, die Sie wiederherstellen möchten.
3. Wählen Sie in der Dropdownliste *Wiederherstellungszeit* einen Online-Wiederherstellungspunkt aus.
4. Wählen Sie **Wiederherstellen** aus, um den **Wiederherstellungs-Assistenten** zu starten.

Für Online-Wiederherstellungspunkte gibt es fünf Wiederherstellungstypen:

* **Im ursprünglichen Exchange Server-Speicherort wiederherstellen:** Die Daten werden auf dem ursprünglichen Exchange Server wiederhergestellt.
* **In einer anderen Datenbank auf einem Exchange Server wiederherstellen:** Die Daten werden in einer anderen Datenbank auf einem anderen Exchange Server wiederhergestellt.
* **In einer Wiederherstellungsdatenbank wiederherstellen:** Die Daten werden in einer Exchange-Wiederherstellungsdatenbank (RDB) wiederhergestellt.
* **In einen Netzwerkordner kopieren:** Die Daten werden in einem Netzwerkordner wiederhergestellt.
* **Auf Band kopieren:** Wenn Sie eine Bandbibliothek oder ein eigenständiges Bandlaufwerk angeschlossen und auf dem MABS konfiguriert haben, wird der Wiederherstellungspunkt auf ein freies Band kopiert.

    ![Onlinereplikation auswählen](./media/backup-azure-backup-exchange-server/choose-online-replication.png)

## <a name="next-steps"></a>Nächste Schritte

* [Azure Backup – Häufig gestellte Fragen](backup-azure-backup-faq.md)
