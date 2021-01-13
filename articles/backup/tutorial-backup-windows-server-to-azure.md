---
title: 'Tutorial: Sichern von Windows Server in Azure'
description: Dieses Tutorial enthält Informationen zum Sichern lokaler Windows Server-Instanzen in einem Recovery Services-Tresor.
ms.topic: tutorial
ms.date: 08/22/2018
ms.custom: mvc
ms.openlocfilehash: d2990b5950cf8812367c3a59c6cace39e4085e2a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "88261904"
---
# <a name="back-up-windows-server-to-azure"></a>Sichern von Windows Server in Azure

Mit Azure Backup können Sie Ihre Windows Server-Instanzen vor Beschädigungen, Angriffen und Notfällen schützen. Azure Backup stellt dazu ein einfaches Tool – den Microsoft Azure Recovery Services-Agent (MARS) – bereit. Der MARS-Agent wird unter Windows Server installiert, um Dateien und Ordner sowie Serverkonfigurationsinformationen über den Windows Server-Systemstatus zu schützen. In diesem Tutorial erfahren Sie, wie Sie den MARS-Agent zum Sichern Ihrer Windows Server-Instanzen in Azure verwenden. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
>
> * Herunterladen und Einrichten des MARS-Agents
> * Konfigurieren der Sicherungszeitpunkte und des Zeitplans für die Aufbewahrung von Sicherungen des Servers
> * Durchführen einer bedarfsgesteuerten Sicherung

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter <https://portal.azure.com> beim Azure-Portal an.

## <a name="create-a-recovery-services-vault"></a>Erstellen eines Recovery Services-Tresors

Bevor Sie Windows Server sichern können, müssen Sie einen Speicherort für die Sicherungen oder Wiederherstellungspunkte erstellen. Ein [Recovery Services-Tresor](backup-azure-recovery-services-vault-overview.md) ist ein Container in Azure, in dem die Sicherungen Ihrer Windows Server-Instanzen gespeichert werden. Gehen Sie folgendermaßen vor, um einen Recovery Services-Tresor im Azure-Portal zu erstellen.

1. Wählen Sie im linken Menü **Alle Dienste** aus, und geben Sie in der Dienstliste **Recovery Services** ein. Wählen Sie **Recovery Services-Tresore**.

   ![Öffnen des Recovery Services-Tresors](./media/tutorial-backup-windows-server-to-azure/full-browser-open-rs-vault_2.png)

2. Wählen Sie im Menü **Recovery Services-Tresore** die Option **Hinzufügen**.

   ![Angeben von Informationen zum Tresor](./media/tutorial-backup-windows-server-to-azure/provide-vault-detail-2.png)

3. Führen Sie im Menü **Recovery Services-Tresor** folgende Aktionen durch:

    * Geben Sie *myRecoveryServicesVault* in **Name** ein.
    * Die aktuelle Abonnement-ID wird unter **Abonnement** angezeigt.
    * Wählen Sie für **Ressourcengruppe** die Option **Vorhandene verwenden** und dann *myResourceGroup* aus. Wenn *myResourceGroup* nicht vorhanden ist, wählen Sie **Neu erstellen** aus und geben *myResourceGroup* ein.
    * Wählen Sie im Dropdownmenü **Standort** die Region *Europa, Westen* aus.
    * Wählen Sie **Erstellen** aus, um den Recovery Services-Tresor zu erstellen.

Nach Abschluss des Erstellungsvorgangs wird der Tresor in der Liste mit den Recovery Services-Tresoren angezeigt.

## <a name="download-recovery-services-agent"></a>Herunterladen des Recovery Services-Agents

Der Microsoft Azure Recovery Services-Agents (MARS) erstellt eine Zuordnung zwischen Windows Server und dem Recovery Services-Tresor. Das folgende Verfahren erläutert, wie der Agent auf Ihren Server heruntergeladen wird.

1. Wählen Sie in der Liste mit den Recovery Services-Tresoren **myRecoveryServicesVault** aus, um das zugehörige Dashboard zu öffnen.

   ![Auswählen des Tresors zum Öffnen des Dashboards](./media/tutorial-backup-windows-server-to-azure/open-vault-from-list.png)

2. Wählen Sie im Menü des Tresordashboards die Option **Sicherung**.

3. Führen Sie im Menü **Sicherungsziel** folgende Aktionen durch:

   * Wählen Sie für **Wo wird Ihre Workload ausgeführt?** die Option **Lokal** aus.
   * Wählen Sie für **Was möchten Sie sichern?** die Option **Dateien und Ordner** und **Systemstatus** aus.

   ![Menü „Sicherungsziel“](./media/tutorial-backup-windows-server-to-azure/backup-goal.png)

4. Wählen Sie **Infrastruktur vorbereiten** aus, um das Menü **Infrastruktur vorbereiten** zu öffnen.

5. Wählen Sie im Menü **Infrastruktur vorbereiten** die Option **Agent für Windows Server oder Windows-Client herunterladen** aus, um die Datei *MARSAgentInstaller.exe* herunterzuladen.

    ![Agent für Windows Server oder Windows-Client herunterladen](./media/tutorial-backup-windows-server-to-azure/prepare-infrastructure.png)

    Das Installationsprogramm öffnet ein separates Browserfenster und lädt die Datei **MARSAgentInstaller.exe** herunter.

6. Bevor Sie die heruntergeladene Datei ausführen, wählen Sie im Menü „Infrastruktur vorbereiten“ die Option **Herunterladen** aus, und speichern Sie die Datei mit den **Tresoranmeldeinformationen**. Die Tresoranmeldeinformationen sind für das Herstellen einer Verbindung zwischen MARS-Agent und Recovery Services-Tresor erforderlich.

    ![Herunterladen der Tresoranmeldedaten](./media/tutorial-backup-windows-server-to-azure/download-vault-credentials.png)

## <a name="install-and-register-the-agent"></a>Installieren und Registrieren des Agents

1. Suchen Sie die heruntergeladene Datei **MARSagentinstaller.exe**, und doppelklicken Sie darauf.
2. Der **Setup-Assistent für den Microsoft Azure Recovery Services-Agent** wird angezeigt. Geben Sie im Assistenten die folgenden Informationen an, wenn Sie dazu aufgefordert werden, und wählen Sie **Registrieren** aus.
   * Speicherort für den Installations- und Cacheordner
   * Proxyserverinformationen (wenn Sie einen Proxyserver verwenden, um eine Internetverbindung herzustellen)
   * Informationen zum Benutzernamen und zum Kennwort (wenn Sie einen authentifizierten Proxy verwenden)

     ![Setup-Assistent für Microsoft Azure Recovery Services-Agent](./media/tutorial-backup-windows-server-to-azure/mars-installer.png)

3. Wählen Sie am Ende des Assistenten **Mit Registrierung fortfahren** aus, und geben Sie die Datei mit den **Tresoranmeldeinformationen** an, die Sie im vorherigen Verfahren heruntergeladen haben.

4. Geben Sie, wenn Sie dazu aufgefordert werden, eine Verschlüsselungspassphrase zum Verschlüsseln von Sicherungen von Windows Server an. Bewahren Sie die Passphrase an einem sicheren Ort auf, da Microsoft die Passphrase nicht wiederherstellen kann, wenn sie verloren geht.

5. Wählen Sie **Fertig stellen** aus.

## <a name="configure-backup-and-retention"></a>Konfigurieren von Sicherung und Aufbewahrung

Verwenden Sie den Microsoft Azure Recovery Services-Agent, um die Zeitpunkte für Sicherungen in Azure für Windows Server zu planen. Führen Sie die folgenden Schritte auf dem Server, auf dem Sie den Agent heruntergeladen haben, aus.

1. Öffnen Sie den Microsoft Azure Recovery Services-Agent. Den Agent finden Sie, indem Sie auf Ihrem Computer nach **Microsoft Azure Backup**suchen.

2. Wählen Sie in der Konsole des Recovery Services-Agents im Bereich **Aktionen** die Option **Sicherung planen** aus.

    ![Sicherung planen](./media/tutorial-backup-windows-server-to-azure/mars-schedule-backup.png)

3. Wählen Sie **Weiter** aus, um zur Seite **Zu sichernde Elemente auswählen** zu navigieren.

4. Wählen Sie **Elemente hinzufügen** und im angezeigten Dialogfeld die Option **Systemstatus** sowie die Dateien oder Ordner aus, die Sie sichern möchten. Klicken Sie anschließend auf **OK**.

5. Wählen Sie **Weiter** aus.

6. Geben Sie auf der Seite **Sicherungszeitplan angeben (Systemstatus)** an, zu welchen Tageszeiten oder wann in der Woche Sicherungen für den Systemstatus ausgelöst werden sollen. Wählen Sie anschließend **Weiter** aus.

7. Wählen Sie auf der Seite **Aufbewahrungsrichtlinie auswählen (Systemstatus)** die Aufbewahrungsrichtlinie für die Sicherungskopien des Systemstatus und dann **Weiter** aus.

8. Wählen Sie analog den Sicherungszeitplan und die Aufbewahrungsrichtlinie für Dateien und Ordner aus.

9. Wählen Sie auf der Seite **Erstsicherungstyp wählen** die Option **Automatisch über das Netzwerk** und dann **Weiter** aus.

10. Lesen Sie sich die Informationen auf der Seite **Bestätigung** durch, und wählen Sie **Fertig stellen** aus.

11. Wählen Sie **Schließen** aus, nachdem der Assistent die Erstellung des Sicherungszeitplans abgeschlossen hat.

## <a name="perform-an-on-demand-backup"></a>Durchführen eines bedarfsgesteuerten Sicherungsauftrags

Sie haben den Zeitplan für die Ausführung von Sicherungsaufträgen eingerichtet. Sie haben allerdings den Server noch nicht gesichert. Es wird für die Notfallwiederherstellung empfohlen, eine bedarfsgesteuerte Sicherung durchzuführen, um Datenresilienz auf dem Server sicherzustellen.

1. Wählen Sie in der Konsole des Microsoft Azure Recovery Services-Agents die Option **Jetzt sichern** aus.

    ![Jetzt sichern](./media/tutorial-backup-windows-server-to-azure/backup-now.png)

2. Wählen Sie im Assistenten **Jetzt sichern** entweder **Dateien und Ordner** oder **Systemstatus** aus (je nachdem, was Sie sichern möchten). Wählen Sie anschließend **Weiter** aus.
3. Überprüfen Sie auf der Seite **Bestätigung** die Einstellungen, die vom Assistenten für **Jetzt sichern** zum Sichern des Servers verwendet werden. Wählen Sie anschließend **Registrieren** aus.
4. Wählen Sie **Schließen** aus, um den Assistenten zu schließen. Wenn Sie den Assistenten schließen, bevor der Sicherungsvorgang abgeschlossen ist, wird der Assistent im Hintergrund weiter ausgeführt.
5. Nach Abschluss der ersten Sicherung wird in der Konsole des MARS-Agents im Bereich **Aufträge** als Status **Auftrag abgeschlossen** angezeigt.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie das Azure-Portal zu folgenden Zwecken verwendet:

> [!div class="checklist"]
>
> * Erstellen eines Recovery Services-Tresors
> * Herunterladen des Microsoft Azure Recovery Services-Agents
> * Installieren des Agents
> * Konfigurieren der Sicherung für Windows Server
> * Durchführen eines bedarfsgesteuerten Sicherungsauftrags

Fahren Sie mit dem nächsten Tutorial zum Wiederherstellen von Dateien aus Azure für Windows Server fort.

> [!div class="nextstepaction"]
> [Wiederherstellen von Dateien aus Azure für Windows Server](./tutorial-backup-restore-files-windows-server.md)
