---
title: Sichern eines Exchange-Servers über System Center DPM
description: Erfahren Sie, wie Sie einen Exchange-Server unter Azure Backup mit System Center 2012 R2 DPM sichern.
ms.reviewer: kasinh
ms.topic: conceptual
ms.date: 01/31/2019
ms.openlocfilehash: ee89af311619922fa6ca585381d70ca66955f36a
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "91271646"
---
# <a name="back-up-an-exchange-server-to-azure-backup-with-system-center-2012-r2-dpm"></a>Sichern eines Exchange-Servers unter Azure Backup mit System Center 2012 R2 DPM

In diesem Artikel wird beschrieben, wie Sie einen System Center 2012 R2 Data Protection Manager-Server (DPM) konfigurieren, um einen Microsoft Exchange-Server unter Azure Backup zu sichern.  

## <a name="updates"></a>Aktualisierungen

Zum erfolgreichen Registrieren des DPM-Servers für Azure Backup müssen Sie das aktuelle Updaterollup für System Center 2012 R2 DPM und die aktuelle Version des Azure Backup-Agents installieren. Sie finden das aktuelle Updaterollup im [Microsoft-Katalog](https://catalog.update.microsoft.com/v7/site/Search.aspx?q=System%20Center%202012%20R2%20Data%20protection%20manager).

> [!NOTE]
> Für die Beispiele in diesem Artikel wurde Version 2.0.8719.0 des Azure Backup-Agents installiert, und das Updaterollup 6 wurde unter System Center 2012 R2 DPM installiert.
>
>

## <a name="prerequisites"></a>Voraussetzungen

Vergewissern Sie sich zunächst, dass für den Schutz von Workloads mit Microsoft Azure Backup alle [Voraussetzungen](backup-azure-dpm-introduction.md#prerequisites-and-limitations) erfüllt sind. Zu diesen Voraussetzungen gehört Folgendes:

* Auf der Azure-Website wurde ein Sicherungstresor erstellt.
* Die Anmeldeinformationen für den Agent und den Tresor wurden auf den DPM-Server heruntergeladen.
* Der Agent wurde auf dem DPM-Server installiert.
* Die Anmeldeinformationen für den Tresor wurden verwendet, um den DPM-Server zu registrieren.
* Wenn Sie Exchange 2016 schützen, führen Sie ein Upgrade auf DPM 2012 R2 UR9 oder höher aus.

## <a name="dpm-protection-agent"></a>DPM-Schutz-Agent

Führen Sie die folgenden Schritte aus, um den DPM-Schutz-Agent auf dem Exchange-Server zu installieren:

1. Stellen Sie sicher, dass die Firewalls richtig konfiguriert sind. Weitere Informationen finden Sie unter [Konfigurieren von Firewallausnahmen für den Agent](/system-center/dpm/configure-firewall-settings-for-dpm).
2. Installieren Sie den Agent auf dem Exchange-Server, indem Sie in der DPM-Verwaltungskonsole **Verwaltung > Agents > Installieren** auswählen. Ausführliche Schritte finden Sie unter [Installieren des DPM-Schutz-Agents](/system-center/dpm/deploy-dpm-protection-agent) .

## <a name="create-a-protection-group-for-the-exchange-server"></a>Erstellen einer Schutzgruppe für den Exchange-Server

1. Wählen Sie in der DPM-Verwaltungskonsole **Schutz** und dann im Menüband des Tools **Neu** aus, um den Assistenten **Neue Schutzgruppe erstellen** zu öffnen.
2. Wählen Sie im Assistenten auf dem Bildschirm **Willkommen** die Option **Weiter** aus.
3. Wählen Sie auf dem Bildschirm **Schutzgruppentyp auswählen** die Option **Server** und dann **Weiter** aus.
4. Wählen Sie die Exchange Server-Datenbank aus, die Sie schützen möchten, und wählen Sie dann **Weiter** aus.

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

    Nach dem Auswählen dieser Option wird für den DPM-Server die Backupkonsistenz überprüft, um den generierten E/A-Datenverkehr zu vermeiden, indem der Befehl **eseutil** auf dem Exchange-Server ausgeführt wird.

   > [!NOTE]
   > Zum Verwenden dieser Option müssen Sie die Dateien „Ese.dll“ und „Eseutil.exe“ auf dem DPM-Server in das Verzeichnis „C:\Programme\Microsoft System Center 2012 R2\DPM\DPM\bin“ kopieren. Andernfalls wird der folgende Fehler ausgelöst:  
   > ![eseutil-Fehler](./media/backup-azure-backup-exchange-server/eseutil-error.png)
   >
   >
8. Wählen Sie **Weiter** aus.
9. Wählen Sie die Datenbank für **Kopiesicherung** und dann **Weiter** aus.

   > [!NOTE]
   > Wenn Sie nicht für mindestens eine DAG-Kopie einer Datenbank die Option „Vollständige Sicherung“ auswählen, werden die Protokolle nicht abgeschnitten.
   >
   >
10. Konfigurieren Sie die Ziele für **Kurzfristige Sicherung**, und wählen Sie **Weiter** aus.
11. Überprüfen Sie den verfügbaren Speicherplatz, und wählen Sie **Weiter** aus.
12. Wählen Sie den Zeitpunkt aus, an dem der DPM-Server die erste Replikation erstellt, und wählen Sie dann **Weiter** aus.
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

1. Wählen Sie zum Wiederherstellen einer Exchange-Datenbank in der DPM-Administratorkonsole **Wiederherstellung** aus.
2. Suchen Sie nach der Exchange-Datenbank, die Sie wiederherstellen möchten.
3. Wählen Sie in der Dropdownliste *Wiederherstellungszeit* einen Online-Wiederherstellungspunkt aus.
4. Wählen Sie **Wiederherstellen** aus, um den **Wiederherstellungs-Assistenten** zu starten.

Für Online-Wiederherstellungspunkte gibt es fünf Wiederherstellungstypen:

* **Im ursprünglichen Exchange Server-Speicherort wiederherstellen:** Die Daten werden auf dem ursprünglichen Exchange Server wiederhergestellt.
* **In einer anderen Datenbank auf einem Exchange Server wiederherstellen:** Die Daten werden in einer anderen Datenbank auf einem anderen Exchange Server wiederhergestellt.
* **In einer Wiederherstellungsdatenbank wiederherstellen:** Die Daten werden in einer Exchange-Wiederherstellungsdatenbank (RDB) wiederhergestellt.
* **In einen Netzwerkordner kopieren:** Die Daten werden in einem Netzwerkordner wiederhergestellt.
* **Auf Band kopieren:** Wenn Sie eine Bandbibliothek oder ein eigenständiges Bandlaufwerk angeschlossen und auf dem DPM-Server konfiguriert haben, wird der Wiederherstellungspunkt auf ein freies Band kopiert.

    ![Onlinereplikation auswählen](./media/backup-azure-backup-exchange-server/choose-online-replication.png)

## <a name="next-steps"></a>Nächste Schritte

* [Azure Backup – Häufig gestellte Fragen](backup-azure-backup-faq.md)
