---
title: Offlinesicherung für DPM und Azure Backup Server
description: Mit Azure Backup können Sie Daten unter Verwendung des Azure Import/Export-Diensts aus dem Netzwerk senden. In diesem Artikel wird der Offlinesicherungsworkflow für DPM und Azure Backup Server erläutert.
ms.topic: conceptual
ms.date: 05/24/2020
ms.openlocfilehash: 0ff6198eed4e3e365b443a51e5c63534c2cf0973
ms.sourcegitcommit: dee7b84104741ddf74b660c3c0a291adf11ed349
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85921255"
---
# <a name="offline-backup-workflow-for-dpm-and-azure-backup-server-mabs"></a>Offlinesicherungsworkflow für DPM und Azure Backup Server (MABS)

>[!IMPORTANT]
> Diese Schritte gelten für DPM 2019 UR1 (oder höher) und MABS V3 UR1 (oder höher).

System Center Data Protection Manager und Azure Backup Server (MABS) können in Azure Backup integriert werden und nutzen verschiedene integrierte effizienzsteigernde Funktionen, mit denen sich bei den ersten vollständigen Sicherungen von Daten in Azure Netzwerk- und Speicherkosten einsparen lassen. Bei den ersten vollständigen Sicherungen werden meist große Datenmengen übertragen, sodass eine höhere Netzwerkbandbreite als bei den nachfolgenden Sicherungen erforderlich ist, bei denen nur die Deltamengen bzw. Inkremente übertragen werden. Azure Backup komprimiert die Erstsicherungen. Durch den Prozess des Offlineseedings kann Azure Backup Datenträger verwenden, um die komprimierten Daten der Erstsicherungen offline in Azure hochzuladen.

Der Offlineseedingvorgang von Azure Backup ist eng in den [Azure Import/Export-Dienst](../storage/common/storage-import-export-service.md) integriert. Sie können diesen Dienst verwenden, um die Daten mithilfe von Datenträgern an Azure zu übertragen. Wenn Sie bei der Erstsicherung mehrere Terabytes an Daten über ein Netzwerk mit hoher Wartezeit und geringer Bandbreite übertragen müssen, können Sie die erste Sicherungskopie mithilfe des Offlineseeding-Workflows auf einer oder mehreren Festplatten speichern und an ein Azure-Datencenter senden. Dieser Artikel bietet eine Übersicht und weitere Schritte zum Abschließen dieses Workflows für System Center Data Protection Manager (DPM) und Microsoft Azure Backup Server (MABS).

> [!NOTE]
> Der Prozess der Offlinesicherung für den MARS-Agent (Microsoft Azure Recovery Services) unterscheidet sich vom Prozess für DPM und MABS. Informationen zur Verwendung der Offlinesicherung mit dem MARS-Agent finden Sie unter [Offlinesicherungsworkflow in Azure Backup](backup-azure-backup-import-export.md). Die Offlinesicherung wird für Sicherungen des Systemstatus mithilfe des Azure Backup-Agents nicht unterstützt.
>
> Mit dem MABS UR1-Update wird unter Verwendung von Azure Data Box in MABS eine Vorschauversion von Offlinesicherung eingeführt. Wenden Sie sich an [SystemCenterFeedback@microsoft.com](mailto:SystemCenterFeedback@microsoft.com), um mehr darüber zu erfahren.

## <a name="overview"></a>Übersicht

Mit der Offlineseeding-Funktion von Azure Backup und dem Azure Import/Export-Dienst können die Daten einfach mithilfe von Datenträgern offline in Azure hochgeladen werden. Der Offlinesicherungsprozess umfasst folgende Schritte:

> [!div class="checklist"]
>
> * Die Sicherungsdaten werden nicht über das Netzwerk gesendet, sondern an einen Stagingspeicherort gespeichert.
> * Die Daten am Stagingspeicherort werden dann mithilfe des Hilfsprogramms *AzureOfflineBackupDiskPrep* auf SATA-Datenträger geschrieben.
> * Ein Azure-Importauftrag wird automatisch vom Hilfsprogramm erstellt.
> * Die SATA-Datenträger werden dann an das nächste Azure-Rechenzentrum gesendet.
> * Nachdem die Sicherungsdaten in Azure hochgeladen wurden, kopiert Azure Backup die Sicherungsdaten in den Sicherungstresor, und die inkrementellen Sicherungen werden geplant.

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind, bevor Sie den Offlinesicherungs-Workflow starten:

* Es wurde ein [Recovery Services-Tresor](backup-azure-recovery-services-vault-overview.md) erstellt. Um einen Tresor zu erstellen, führen Sie die Schritte unter [Erstellen eines Recovery Services-Tresors](tutorial-backup-windows-server-to-azure.md#create-a-recovery-services-vault) aus.
* Stellen Sie sicher, dass nur die [neueste Version des Microsoft Azure Recovery Services-Agent](https://aka.ms/azurebackup_agent) auf dem SC DPM- oder MABS-Server installiert und beim Recovery Services-Tresor registriert ist.
* Updaterollup 1 ist in SC DPM 2019 oder MABS V3 installiert.

  > [!NOTE]
  > Mit DPM 2019 UR1 und MABS V3 UR1 wird das Offlineseeding mithilfe von Azure Active Directory authentifiziert.

* Auf dem DPM- oder MABS-Server muss Microsoft Edge oder Internet Explorer 11 installiert und JavaScript aktiviert sein.
* Erstellen Sie ein Azure Storage-Konto in dem Abonnement, in dem sich auch der Recovery Services-Tresor befindet.
* Vergewissern Sie sich, dass Sie über die [erforderlichen Berechtigungen](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal) für die Erstellung der Azure Active Directory-Anwendung verfügen. Der Workflow zur Offlinesicherung erstellt eine Azure Active Directory-Anwendung in dem Abonnement, das dem Azure Storage-Konto zugeordnet ist. Die Anwendung dient dazu, Azure Backup sicheren und bereichsbezogenen Zugriff auf den Azure-Importdienst zu ermöglichen, der für den Workflow zur Offlinesicherung erforderlich ist.
* Registrieren Sie den Ressourcenanbieter „Microsoft.ImportExport“ bei dem Abonnement mit dem Azure Storage-Konto. So registrieren Sie den Ressourcenanbieter:
    1. Klicken Sie im Hauptmenü auf **Abonnements**.
    2. Wählen Sie bei Verwendung mehrerer Abonnements das Abonnement aus, das Sie für die Offlinesicherung verwenden. Falls Sie nur ein einzelnes Abonnement verwenden, wird Ihr Abonnement angezeigt.
    3. Klicken Sie im Abonnementmenü auf **Ressourcenanbieter**, um die Anbieterliste anzuzeigen.
    4. Scrollen Sie in der Anbieterliste nach unten zu „Microsoft.ImportExport“. Falls der Status „NotRegistered“ lautet, klicken Sie auf **Registrieren**.

       ![Registrieren des Ressourcenanbieters](./media/backup-azure-backup-server-import-export/register-import-export.png)

* Es wurde ein Stagingspeicherort, bei dem es sich um eine Netzwerkfreigabe oder ein zusätzliches Laufwerk auf dem Computer (intern oder extern) handeln kann, mit genügend Speicherplatz zum Speichern der Erstkopie erstellt. Wenn Sie beispielsweise einen 500-GB-Dateiserver sichern möchten, muss der Stagingbereich mindestens 500 GB groß sein. (Aufgrund der Komprimierung wird weniger Speicherplatz genutzt.)
* Bei Datenträgern, die an Azure gesendet werden, stellen Sie sicher, dass nur interne 2,5-Zoll-SSD- oder 2,5-Zoll- bzw. 3,5-Zoll-SATA II/III-Festplatten verwendet werden. Sie können Festplatten mit bis zu 10 TB verwenden. Schlagen Sie in der [Dokumentation zum Azure Import/Export-Dienst](../storage/common/storage-import-export-requirements.md#supported-hardware) die aktuell vom Dienst unterstützten Laufwerke nach.
* Die SATA-Laufwerke müssen mit einem Computer verbunden sein (als *Kopiercomputer* bezeichnet), auf dem die Sicherungsdaten vom Stagingspeicherort auf die SATA Laufwerke kopiert werden. Stellen Sie sicher, dass BitLocker auf dem Kopiercomputer aktiviert ist.

## <a name="workflow"></a>Workflow

In diesem Abschnitt wird erläutert, wie Sie den Workflow zur Offlinesicherung durchführen, sodass Ihre Daten an ein Azure-Datencenter gesendet und in Azure Storage hochgeladen werden können. Falls Sie Fragen zum Import-Dienst oder zu einem anderen Aspekt des Prozesses haben, finden Sie in der oben erwähnten Dokumentation eine [Übersicht über den Import-Dienst](https://docs.microsoft.com/azure/storage/common/storage-import-export-service) .

## <a name="initiate-offline-backup"></a>Initiieren der Offlinesicherung

1. Wenn Sie eine neue Schutzgruppe mit Onlineschutz erstellen oder einer vorhandenen Schutzgruppe Onlineschutz hinzufügen, wird der folgende Bildschirm angezeigt. Um die anfängliche Onlinereplikationsmethode auszuwählen, wählen Sie **Übertragung über eigenen Datenträger** aus, und klicken Sie dann auf **Weiter**.

    ![Importbildschirm](./media/backup-azure-backup-server-import-export/create-new-protection-group.png)

2. Die Azure-Anmeldeseite wird geöffnet. Melden Sie sich mit Ihrem Azure-Benutzerkonto an, das über die Rollenberechtigung *Besitzer* für das Azure-Abonnement verfügt.

    ![Azure-Anmeldeseite](./media/backup-azure-backup-server-import-export/choose-initial-online-replication.png)

3. Nehmen Sie die Eingaben auf der Seite **Eigenen Datenträger verwenden** vor.

   ![Eingaben für „Eigenen Datenträger verwenden“](./media/backup-azure-backup-server-import-export/use-your-own-disk.png)

   Die Beschreibung der Eingaben lautet wie folgt:

   * **Stagingspeicherort**: Der temporäre Speicherort, an den die erste Sicherungskopie geschrieben wird. Beim Stagingspeicherort kann es sich um eine Netzwerkfreigabe oder einen lokalen Computer handeln. Wenn der Kopiercomputer und der Quellcomputer nicht identisch sind, geben Sie den vollständigen Netzwerkpfad des Stagingspeicherorts an.
   * **Azure Resource Manager-Speicherkonto**: Der Name des Resource Manager-Speicherkontos (Universell v1 oder Universell v2) in allen Azure-Abonnements.
   * **Azure Storage-Container**: Der Name des Blob Storage-Zielcontainers im Azure Storage-Konto, in das die Sicherungsdaten importiert werden.
   * **Azure-Abonnement-ID**: Die ID für das Azure-Abonnement, in dem das Azure-Speicherkonto erstellt wird.
   * **Name des Azure Importauftrags**: Der eindeutige Name, anhand dessen der Azure Import-Dienst und Azure Backup Datenübertragungen nachverfolgen, die auf Datenträgern an Azure gesendet werden.

    Speichern Sie den **Stagingspeicherort** und den angegebenen **Namen des Azure-Importauftrags**. Diese Informationen werden zur Vorbereitung der Datenträger benötigt.

4. Schließen Sie den Workflow ab, um den Schutz zu erstellen oder zu aktualisieren. Um die Offlinesicherungskopie zu initiieren, klicken Sie mit der rechten Maustaste auf die **Schutzgruppe**, und wählen Sie dann die Option **Wiederherstellungspunkt erstellen** aus. Wählen Sie anschließend die Option **Onlineschutz** aus.

   ![Wiederherstellungspunkt erstellen](./media/backup-azure-backup-server-import-export/create-recovery-point.png)

5. Überwachen Sie den Auftrag für die Onlinereplikaterstellung im Bereich „Überwachung“. Der Auftrag sollte mit der Warnung *Warten auf Fertigstellung des Azure-Importauftrags* erfolgreich abgeschlossen werden.

   ![Fertigstellen des Wiederherstellungspunkts](./media/backup-azure-backup-server-import-export/complete-recovery-point.png)

6. Nach Abschluss des Vorgangs kann der Stagingspeicherort für die Datenträgervorbereitung verwendet werden.

## <a name="prepare-sata-drives-and-ship-to-azure"></a>Vorbereiten der SATA-Laufwerke und Senden an Azure

Das Hilfsprogramm *AzureOfflineBackupDiskPrep* bereitet die SATA-Laufwerke vor, die an das nächstgelegene Azure-Datencenter gesendet werden. Dieses Hilfsprogramm steht im Installationsverzeichnis des Azure Backup-Agent unter folgendem Pfad zur Verfügung: `*\Microsoft Azure Recovery Services Agent\Utils\\*`

1. Navigieren Sie zu diesem Verzeichnis, und kopieren Sie das Verzeichnis **AzureOfflineBackupDiskPrep** auf einen anderen Computer, mit dem die SATA-Laufwerke verbunden sind. Vergewissern Sie sich, dass auf dem Computer mit den verbundenen SATA-Laufwerken Folgendes erfüllt ist:

   * Der Kopiercomputer kann über denselben Netzwerkpfad, der während des Workflows im Abschnitt „Initiieren der Offlinesicherung“ angegeben wurde, auf den Stagingspeicherort für den Offlineseeding-Workflow zugreifen.
   * BitLocker ist auf dem Kopiercomputer aktiviert.
   * Azure PowerShell 3.7.0 wird auf dem Kopiercomputer installiert (nicht erforderlich, wenn Sie das Hilfsprogramm AzureOfflineBackupDiskPrep auf dem DPM- oder MABS-Server ausführen).
   * Die neuesten kompatiblen Browser (Microsoft Edge oder Internet Explorer 11) sind installiert, und JavaScript ist aktiviert.
   * Der Kopiercomputer kann auf das Azure-Portal zugreifen. Sofern erforderlich, kann der Kopiercomputer mit dem Quellcomputer identisch sein.

     > [!IMPORTANT]
     > Wenn der Quellcomputer ein virtueller Computer ist, muss ein anderer physischer Server- oder Clientcomputer als Kopiercomputer verwendet werden.

1. Öffnen Sie auf dem Kopiercomputer eine Eingabeaufforderung mit erhöhten Rechten und dem Verzeichnis des Hilfsprogramms *AzureOfflineBackupDiskPrep* als aktuelles Verzeichnis. Führen Sie den folgenden Befehl aus:

    ```console
    .\AzureOfflineBackupDiskPrep.exe s:<Staging Location Path>
    ```

    | Parameter | BESCHREIBUNG |
    | --- | --- |
    | s:&lt;*Pfad zum Stagingspeicherort*&gt; |Mit dieser obligatorischen Eingabe wird der Pfad zum Stagingspeicherort angegeben, den Sie im Workflow im Abschnitt „Initiieren der Offlinesicherung“ eingegeben haben. |
    | p:&lt;*Pfad zu PublishSettingsFile*&gt; |Mit dieser optionalen Eingabe wird der Pfad zur Datei mit den Azure-Veröffentlichungseinstellungen angegeben, den Sie im Workflow im Abschnitt „Initiieren der Offlinesicherung“ eingegeben haben. |

    Beim Ausführen des Befehls fordert das Hilfsprogramm die Auswahl des Azure-Importauftrags an, der den vorzubereitenden Datenträgern entspricht. Falls dem angegebenen Stagingspeicherort nur ein einzelner Importauftrag zugeordnet ist, wird ein Bildschirm wie in der folgenden Abbildung angezeigt.

      ![Konsole zur Datenträgervorbereitung](./media/backup-azure-backup-server-import-export/disk-prep-console.png)

1. Geben Sie den Laufwerkbuchstaben ohne nachgestellten Doppelpunkt für den bereitgestellten Datenträger an, den Sie für die Übertragung an Azure vorbereiten möchten.
1. Bestätigen Sie die Formatierung des Laufwerks, wenn Sie dazu aufgefordert werden.
1. Sie werden aufgefordert, sich bei Ihrem Azure-Abonnement anzumelden. Geben Sie Ihre Anmeldeinformationen ein.

    ![Azure-Anmeldebildschirm](./media/backup-azure-backup-server-import-export/signin-disk-prep.png)

    Das Tool beginnt dann, den Datenträger vorzubereiten und die Sicherungsdaten zu kopieren. Falls auf dem bereitgestellten Datenträger nicht genügend Speicherplatz für die Sicherungsdaten verfügbar ist, müssen Sie unter Umständen weitere Datenträger anfügen, wenn Sie vom Tool dazu aufgefordert werden. <br/>

    Nach erfolgreicher Ausführung des Tools gibt die Befehlszeile drei Informationen zurück:
    * Die von Ihnen bereitgestellten Datenträger werden für den Versand an Azure vorbereitet.
    * Sie erhalten eine Erstellungsbestätigung für Ihren Importauftrag. Der Importauftrag hat den von Ihnen angegebenen Namen.
    * Das Tool zeigt die Lieferadresse für das Azure-Datencenter an.

     ![Azure-Tool für die Datenträgervorbereitung – Abgeschlossen](./media/backup-azure-backup-server-import-export/console.png)

1. Am Ende der Befehlsausführung wird auch die Option zum Aktualisieren der Versandinformationen angezeigt.

1. Senden Sie die Datenträger an die im Tool angegebene Adresse, und bewahren Sie die Nachverfolgungsnummer zur späteren Nutzung auf.

   > [!IMPORTANT]
   > Zwei Azure-Importaufträge dürfen nicht dieselbe Nachverfolgungsnummer aufweisen. Stellen Sie sicher, dass die vom Hilfsprogramm unter einem gemeinsamen Azure-Importauftrag vorbereiteten Laufwerke zusammen in einem einzelnen Paket versendet werden und dass das Paket eine einzelne eindeutige Nachverfolgungsnummer besitzt. Fassen Sie Laufwerke, die im Rahmen unterschiedlicher Azure Importaufträge vorbereitet wurden, nicht in einem Paket zusammen.

## <a name="update-shipping-details-on-the-azure-import-job"></a>Aktualisieren der Versanddetails für den Azure-Importauftrag

Das folgende Verfahren dient zum Aktualisieren der Versanddetails für den Azure-Importauftrag. Dazu zählen folgende Informationen:

* Der Name des Transportunternehmens, das die Datenträger an Azure liefert.
* Die Details für den Rückversand Ihrer Datenträger.

   1. Melden Sie sich bei Ihrem Azure-Abonnement an.
   2. Klicken Sie im Hauptmenü auf **Alle Dienste**, und geben Sie im Dialogfeld „Alle Dienste“ den Text „Import“ ein. Wenn die Option **Import-/Exportaufträge** erscheint, klicken Sie darauf.
       ![Versandinformationen eingeben](./media/backup-azure-backup-server-import-export/search-import-job.png)

       Das Menü **Import-/Exportaufträge** wird geöffnet, und eine Liste mit allen Import-/Exportaufträgen im ausgewählten Abonnement wird angezeigt.

   3. Wenn Sie über mehrere Abonnements verfügen, wählen Sie das Abonnement aus, das zum Importieren der Sicherungsdaten verwendet wird. Wählen Sie anschließend den neu erstellten Importauftrag aus, um dessen Details anzuzeigen.

       ![Versandinformationen überprüfen](./media/backup-azure-backup-server-import-export/import-job-found.png)

   4. Klicken Sie im Einstellungsmenü für den Importauftrag auf **Versandinformationen verwalten**, und geben Sie die Details für den Rückversand an.

       ![Versandinformationen speichern](./media/backup-azure-backup-server-import-export/shipping-info.png)

   5. Wenn Sie die Nachverfolgungsnummer von Ihrem Transportunternehmen erhalten haben, klicken Sie auf der Übersichtsseite des Azure-Importauftrags auf das Banner, und geben Sie folgende Details ein:

      > [!IMPORTANT]
      > Die Angaben zum Transportunternehmen und die Nachverfolgungsnummer müssen innerhalb von zwei Wochen nach Erstellung des Azure-Importauftrags aktualisiert werden. Andernfalls wird der Auftrag unter Umständen gelöscht, und die Datenträger werden nicht verarbeitet.

      ![Auftragsübersicht](./media/backup-azure-backup-server-import-export/job-overview.png)

      ![Nachverfolgungsinformationen](./media/backup-azure-backup-server-import-export/tracking-info.png)

### <a name="time-to-process-the-drives"></a>Verarbeitungsdauer der Laufwerke

Der Zeitaufwand zum Verarbeiten eines Azure Import/Export-Auftrags variiert. Die Verarbeitungsdauer hängt von Faktoren wie Versanddauer, Auftragstyp, Typ und Größe der zu kopierenden Daten und Größe der bereitgestellten Datenträger ab. Der Azure Import/Export-Dienst verfügt über keine Vereinbarung zum Servicelevel (SLA). Nach Erhalt der Datenträger versucht der Dienst jedoch, das Kopieren der Sicherungsdaten in Ihr Azure Storage-Konto innerhalb von sieben bis zehn Tagen fertigzustellen. Im nächsten Abschnitt wird beschrieben, wie Sie den Status des Azure-Importauftrags überwachen können.

### <a name="monitor-azure-import-job-status"></a>Überwachen des Status von Azure-Importaufträgen

Sie können den Status des Importauftrags über das Azure-Portal überwachen. Navigieren Sie hierzu zur Seite **Import-/Exportaufträge**, und wählen Sie Ihren Auftrag aus. Weitere Informationen zum Status von Importaufträgen finden Sie im Artikel [Verwenden des Microsoft Azure Import/Export-Diensts zum Übertragen von Daten in Azure Store](https://docs.microsoft.com/azure/storage/common/storage-import-export-service).

### <a name="complete-the-workflow"></a>Ausführen des Workflows

Nach Abschluss des Importauftrags sind die Daten der Erstsicherung in Ihrem Speicherkonto verfügbar. Bei der nächsten geplanten Sicherung kopiert Azure Backup den Inhalt der Daten aus dem Speicherkonto in den Recovery Services-Tresor.

Zum Zeitpunkt des nächsten geplanten Auftrags für die Onlinereplikaterstellung führt Data Protection Manager eine inkrementelle Sicherung für die erste Sicherungskopie durch.

## <a name="next-steps"></a>Nächste Schritte

* Falls Sie Fragen zum Workflow des Azure Import/Export-Diensts haben, finden Sie unter [Verwenden des Microsoft Azure Import/Export-Diensts zum Übertragen von Daten in den Blobspeicher](../storage/common/storage-import-export-service.md) weitere Informationen.
