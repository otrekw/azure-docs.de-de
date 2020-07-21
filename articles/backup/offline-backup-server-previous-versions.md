---
title: 'Offlinesicherung für Data Protection Manager (DPM) und Microsoft Azure Backup Server (MABS): Vorherige Versionen'
description: Mit Azure Backup können Sie Daten unter Verwendung des Azure Import/Export-Diensts aus dem Netzwerk senden. In diesem Artikel wird der Offlinesicherungsworkflow für DPM und Azure Backup Server erläutert.
ms.topic: conceptual
ms.date: 06/08/2020
ms.openlocfilehash: 128051210984a55620be60a5965a7067e74de7c7
ms.sourcegitcommit: ec682dcc0a67eabe4bfe242fce4a7019f0a8c405
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2020
ms.locfileid: "86186944"
---
# <a name="offline-backup-workflow-for-dpm-and-azure-backup-server-previous-versions"></a>Offlinesicherungsworkflow für DPM und Azure Backup Server (vorherige Versionen)

>[!IMPORTANT]
>Diese Schritte gelten für DPM 2019 RTM/MABS v3 RTM und für ältere Versionen.

Azure Backup verfügt über mehrere integrierte effizienzsteigernde Funktionen, die die Netzwerk- und Speicherkosten bei den ersten vollständigen Datensicherungen in Azure reduzieren. Bei den ersten vollständigen Sicherungen werden meist große Datenmengen übertragen, sodass eine höhere Netzwerkbandbreite als bei den nachfolgenden Sicherungen erforderlich ist, bei denen nur die Deltamengen bzw. Inkremente übertragen werden. Azure Backup komprimiert die Erstsicherungen. Durch den Prozess des Offlineseedings kann Azure Backup Datenträger verwenden, um die komprimierten Daten der Erstsicherungen offline in Azure hochzuladen.

Der Offlineseedingvorgang von Azure Backup ist eng in den [Azure Import/Export-Dienst](../storage/common/storage-import-export-service.md) integriert. Sie können diesen Dienst verwenden, um die Daten mithilfe von Datenträgern an Azure zu übertragen. Wenn Sie bei der Erstsicherung mehrere Terabytes an Daten über ein Netzwerk mit hoher Wartezeit und geringer Bandbreite übertragen müssen, können Sie die erste Sicherungskopie mithilfe des Offlineseeding-Workflows auf einer oder mehreren Festplatten speichern und an ein Azure-Datencenter senden. Dieser Artikel bietet eine Übersicht und weitere Schritte zum Abschließen dieses Workflows für System Center Data Protection Manager (DPM) und Microsoft Azure Backup Server (MABS).

> [!NOTE]
> Der Prozess der Offlinesicherung für den MARS-Agent (Microsoft Azure Recovery Services) unterscheidet sich vom Prozess für DPM und MABS. Informationen zur Verwendung der Offlinesicherung mit dem MARS-Agent finden Sie unter [Offlinesicherungsworkflow in Azure Backup](backup-azure-backup-import-export.md). Die Offlinesicherung wird für Sicherungen des Systemstatus mithilfe des Azure Backup-Agents nicht unterstützt.
>

## <a name="overview"></a>Übersicht

Mit der Offlineseeding-Funktion von Azure Backup und dem Azure Import/Export-Dienst können die Daten einfach mithilfe von Datenträgern offline in Azure hochgeladen werden. Der Offlinesicherungsprozess umfasst folgende Schritte:

> [!div class="checklist"]
>
> * Die Sicherungsdaten werden nicht über das Netzwerk gesendet, sondern an einen Stagingspeicherort gespeichert.
> * Die Daten am Stagingspeicherort werden dann mithilfe des Hilfsprogramms *AzureOfflineBackupDiskPrep* auf SATA-Datenträger geschrieben.
> * Ein Azure-Importauftrag wird automatisch vom Hilfsprogramm erstellt.
> * Die SATA-Datenträger werden dann an das nächste Azure-Rechenzentrum gesendet.
> * Nachdem die Sicherungsdaten in Azure hochgeladen wurden, kopiert Azure Backup die Sicherungsdaten in den Sicherungstresor, und die inkrementellen Sicherungen werden geplant.

## <a name="supported-configurations"></a>Unterstützte Konfigurationen

Die Offlinesicherung wird für alle Bereitstellungsmodelle von Azure Backup unterstützt, die lokale Sicherungsdaten in die Microsoft Cloud sichern. Zu diesen Modellen gehören folgende:

> [!div class="checklist"]
>
> * Sichern von Dateien und Ordnern mit dem MARS-Agent oder dem Azure Backup-Agent.
> * Sichern aller Workloads und Dateien mit DPM.
> * Sichern aller Workloads und Dateien mit MABS.

>[!NOTE]
>Azure CSP-Abonnements werden für die Verwendung mit Offlineseeding für DPM 2019 RTM und MABS v3 RTM sowie für ältere Versionen nicht unterstützt. Onlinesicherungen über das Netzwerk werden allerdings weiterhin unterstützt.

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind, bevor Sie den Offlinesicherungs-Workflow starten:

* Es wurde ein [Recovery Services-Tresor](backup-azure-recovery-services-vault-overview.md) erstellt. Führen Sie zum Erstellen eines Tresors die folgenden Schritte in [Erstellen eines Recovery Services-Tresor](tutorial-backup-windows-server-to-azure.md#create-a-recovery-services-vault) aus.
* Ein Azure Backup-Agent bzw. MABS oder DPM wurde entweder unter Windows Server oder auf einem Windows-Client installiert, und der Computer ist beim Recovery Services-Tresor registriert. Vergewissern Sie sich, dass [die neueste Version von Azure Backup](https://go.microsoft.com/fwlink/?linkid=229525) verwendet wird.
* [Laden Sie die Datei mit den Azure-Veröffentlichungseinstellungen](https://portal.azure.com/#blade/Microsoft_Azure_ClassicResources/PublishingProfileBlade) auf den Computer herunter, von dem aus Sie Ihre Daten sichern möchten. Das Abonnement, aus dem Sie die Datei mit den Veröffentlichungseinstellungen herunterladen, kann ein anderes als das mit dem Recovery Services-Tresor sein. Wenn sich Ihr Abonnement in Sovereign Clouds von Azure befindet, verwenden Sie die folgenden Links zum Herunterladen der Datei mit den Veröffentlichungseinstellungen für Azure.

    | Region der Sovereign Cloud | Link zur Datei mit den Azure-Veröffentlichungseinstellungen |
    | --- | --- |
    | USA | [Link](https://portal.azure.us#blade/Microsoft_Azure_ClassicResources/PublishingProfileBlade) |
    | China | [Link](https://portal.azure.cn/#blade/Microsoft_Azure_ClassicResources/PublishingProfileBlade) |

* Es wurde ein Azure Storage-Konto mit dem Resource Manager-Bereitstellungsmodell in dem Abonnement erstellt, aus dem Sie die Datei mit den Veröffentlichungseinstellungen heruntergeladen haben. Erstellen Sie im Speicherkonto einen neuen Blobcontainer als Ziel.

  ![Erstellen eines Speicherkontos mit Resource Manager-Entwicklung](./media/offline-backup-dpm-mabs-previous-versions/storage-account-resource-manager.png)

* Es wurde ein Stagingspeicherort, bei dem es sich um eine Netzwerkfreigabe oder ein zusätzliches Laufwerk auf dem Computer (intern oder extern) handeln kann, mit genügend Speicherplatz zum Speichern der Erstkopie erstellt. Wenn Sie beispielsweise einen 500-GB-Dateiserver sichern möchten, muss der Stagingbereich mindestens 500 GB groß sein. (Aufgrund der Komprimierung wird weniger Speicherplatz genutzt.)
* Bei Datenträgern, die an Azure gesendet werden, stellen Sie sicher, dass nur interne 2,5-Zoll-SSD- oder 2,5-Zoll- bzw. 3,5-Zoll-SATA II/III-Festplatten verwendet werden. Sie können Festplatten mit bis zu 10 TB verwenden. Schlagen Sie in der [Dokumentation zum Azure Import/Export-Dienst](../storage/common/storage-import-export-requirements.md#supported-hardware) die aktuell vom Dienst unterstützten Laufwerke nach.
* Die SATA-Laufwerke müssen mit einem Computer verbunden sein (als *Kopiercomputer* bezeichnet), auf dem die Sicherungsdaten vom Stagingspeicherort auf die SATA Laufwerke kopiert werden. Stellen Sie sicher, dass BitLocker auf dem Kopiercomputer aktiviert ist.

## <a name="prepare-the-server-for-the-offline-backup-process"></a>Vorbereiten des Servers für die Offlinesicherung

>[!NOTE]
> Wenn Sie die aufgelisteten Hilfsprogramme (z. B. *AzureOfflineBackupCertGen.exe*) in Ihrer Installation des MARS-Agents nicht finden können, schreiben Sie an AskAzureBackupTeam@microsoft.com, um Zugriff darauf zu erhalten.

* Öffnen Sie auf dem Server eine Eingabeaufforderung mit erhöhten Rechten, und führen Sie den folgenden Befehl aus:

    ```cmd
    AzureOfflineBackupCertGen.exe CreateNewApplication SubscriptionId:<Subs ID>
    ```

    Das Tool erstellt eine Azure-Offlinesicherungs-Active Directory-Anwendung, wenn noch keine vorhanden ist.

    Wenn bereits eine Anwendung vorhanden ist, werden Sie durch die ausführbare Datei aufgefordert, das Zertifikat manuell in die Anwendung im Mandanten hochzuladen. Führen Sie die Schritte in [diesem Abschnitt](#manually-upload-an-offline-backup-certificate) aus, um das Zertifikat manuell in die Anwendung hochzuladen.

* Das Tool *AzureOfflineBackupCertGen.exe* generiert eine Datei namens *OfflineApplicationParams.xml*. Kopieren Sie diese Datei mit dem MABS oder DPM auf den Server.
* Installieren Sie den [aktuellen MARS-Agent](https://aka.ms/azurebackup_agent) auf der DPM-Instanz oder dem Azure Backup-Server.
* Registrieren Sie den Server bei Azure.
* Führen Sie den folgenden Befehl aus:

    ```cmd
    AzureOfflineBackupCertGen.exe AddRegistryEntries SubscriptionId:<subscriptionid> xmlfilepath:<path of the OfflineApplicationParams.xml file>  storageaccountname:<storageaccountname to be used for offline backup>
    ```

* Mit dem zuvor genannten Befehl wird die Datei `C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch\MicrosoftBackupProvider\OfflineApplicationParams_<Storageaccountname>.xml` erstellt.

## <a name="manually-upload-an-offline-backup-certificate"></a>Manuelles Hochladen eines Offlinesicherungszertifikats

Führen Sie die folgenden Schritte aus, um das Offlinesicherungszertifikat manuell in eine zuvor erstellte Azure Active Directory-Anwendung hochzuladen, die für die Offlinesicherung vorgesehen ist.

1. Melden Sie sich beim Azure-Portal an.
1. Wechseln Sie zu **Azure Active Directory** > **App-Registrierungen**.
1. Suchen Sie auf der Registerkarte **Anwendungen mit Besitzer** nach einer Anwendung mit dem Anzeigenamensformat `AzureOfflineBackup _<Azure User Id`.

    ![Suchen der Anwendung auf der Registerkarte „Anwendungen mit Besitzer“](./media/offline-backup-dpm-mabs-previous-versions/owned-applications.png)

1. Wählen Sie die Anwendung aus. Wechseln Sie unter **Verwalten** im linken Bereich zu **Zertifikate & Geheimnisse**.
1. Überprüfen Sie, ob bereits Zertifikate oder öffentliche Schlüssel vorhanden sind. Wenn noch keine vorhanden sind, können Sie die Anwendung auf sichere Weise löschen, indem Sie auf der Seite **Übersicht** der Anwendung die Schaltfläche **Löschen** auswählen. Anschließend können Sie die Schritte zum [Vorbereiten des Servers für die Offlinesicherung](#prepare-the-server-for-the-offline-backup-process) wiederholen und die folgenden Schritte überspringen. Führen Sie andernfalls die folgenden Schritte aus der DPM-Instanz oder dem Azure Backup-Server aus, auf dem Sie die Offlinesicherung konfigurieren möchten.
1. Geben Sie unter **Start** > **Ausführen** die Zeichenfolge *Certlm.msc* ein. Wählen Sie im Fenster **Zertifikate – Lokaler Computer** die Registerkarte **Zertifikate – Lokaler Computer** > **Persönlich** aus. Suchen Sie nach dem Zertifikat mit dem Namen `CB_AzureADCertforOfflineSeeding_<ResourceId>`.
1. Wählen Sie das Zertifikat aus, klicken Sie mit der rechten Maustaste auf **Alle Aufgaben**, und wählen Sie dann **Exportieren** ohne privaten Schlüssel im CER-Format aus.
1. Wechseln Sie im Azure-Portal zur Anwendung für die Azure-Offlinesicherung.
1. Wählen Sie **Verwalten** > **Zertifikate & Geheimnisse** > **Zertifikat hochladen** aus. Laden Sie das im vorherigen Schritt exportierte Zertifikat hoch.

    ![Hochladen des Zertifikats](./media/offline-backup-dpm-mabs-previous-versions/upload-certificate.png)

1. Öffnen Sie auf dem Server die Registrierung, indem Sie im Fenster „Ausführen“ **regedit** eingeben.
1. Wechseln Sie zum Registrierungseintrag *Computer\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider*.
1. Klicken Sie mit der rechten Maustaste auf **CloudBackupProvider**, und fügen Sie einen neuen Zeichenfolgenwert mit dem Namen `AzureADAppCertThumbprint_<Azure User Id>` hinzu.

    >[!NOTE]
    > Führen Sie einen der folgenden Schritte aus, um die Azure-Benutzer-ID zu ermitteln:
    >
    >* Führen Sie in PowerShell mit Azure-Verbindung den Befehl `Get-AzureRmADUser -UserPrincipalName "Account Holder's email as appears in the portal"` aus.
    >* Navigieren Sie zum Registrierungspfad `Computer\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\DbgSettings\OnlineBackup; Name: CurrentUserId;`.

1. Klicken Sie mit der rechten Maustaste auf die im vorherigen Schritt hinzugefügte Zeichenfolge, und wählen Sie **Ändern** aus. Geben Sie als Wert den Fingerabdruck des Zertifikats an, das Sie in Schritt 7 exportiert haben. Klicken Sie anschließend auf **OK**.
1. Um den Wert des Fingerabdrucks zu erhalten, doppelklicken Sie auf das Zertifikat. Wählen Sie die Registerkarte **Details** aus, und scrollen Sie nach unten, bis das Fingerabdruckfeld angezeigt wird. Wählen Sie **Fingerabdruck** aus, und kopieren Sie den Wert.

    ![Kopieren des Werts aus dem Feld „Fingerabdruck“](./media/offline-backup-dpm-mabs-previous-versions/thumbprint-field.png)

1. Fahren Sie mit dem Abschnitt [Workflow](#workflow) fort, um den Offlinesicherungsprozess fortzusetzen.

## <a name="workflow"></a>Workflow

In diesem Abschnitt wird erläutert, wie Sie den Workflow zur Offlinesicherung abschließen, sodass Ihre Daten an ein Azure-Datencenter gesendet und in Azure Storage hochgeladen werden können. Falls Sie Fragen zum Import-Dienst oder zu einem anderen Aspekt des Prozesses haben, finden Sie in der oben erwähnten Dokumentation eine [Übersicht über den Import-Dienst](../storage/common/storage-import-export-service.md).

### <a name="initiate-offline-backup"></a>Initiieren der Offlinesicherung

1. Wenn Sie eine Sicherung planen, wird die folgende Seite in Windows Server, einem Windows-Client oder DPM angezeigt.

    ![Seite „Importieren“](./media/offline-backup-dpm-mabs-previous-versions/offline-backup-screen-inputs.png)

    Hier ist die entsprechende Seite in DPM. <br/>

    ![DPM und Azure Backup Server – Seite „Importieren“](./media/offline-backup-dpm-mabs-previous-versions/dpm-offline.png)

    Folgende Felder sind auszufüllen:

   * **Stagingspeicherort**: Der temporäre Speicherort, an den die erste Sicherungskopie geschrieben wird. Beim Stagingspeicherort kann es sich um eine Netzwerkfreigabe oder einen lokalen Computer handeln. Wenn der Kopiercomputer und der Quellcomputer nicht identisch sind, geben Sie den vollständigen Netzwerkpfad des Stagingspeicherorts an.
   * **Azure-Veröffentlichungseinstellungen**: Der lokale Pfad zur Datei mit den Veröffentlichungseinstellungen.
   * **Name des Azure Importauftrags**: Der eindeutige Name, anhand dessen der Azure Import/Export-Dienst und Azure Backup Datenübertragungen nachverfolgen, die auf Datenträgern an Azure gesendet werden.
   * **Azure-Abonnement-ID**: Azure-Abonnement-ID für das Abonnement, aus dem Sie die Datei mit den Azure-Veröffentlichungseinstellungen heruntergeladen haben.
   * **Azure Storage-Konto**: Name des Speicherkontos im Azure-Abonnement, das der Datei mit den Azure-Veröffentlichungseinstellungen zugeordnet ist.
   * **Azure Storage-Container**: Name des Zielspeicherblobs im Azure Storage-Konto, in das die Sicherungsdaten importiert werden.

   Speichern Sie den **Stagingspeicherort** und den angegebenen **Namen des Azure-Importauftrags**. Diese Informationen werden zur Vorbereitung der Datenträger benötigt.

1. Schließen Sie den Workflow ab. Wählen Sie in der Verwaltungskonsole des Azure Backup-Agents **Jetzt sichern** aus, um die Offlinesicherungskopie zu initiieren. Die erste Sicherung wird bei diesem Schritt in den Stagingbereich geschrieben.

    ![Jetzt sichern](./media/offline-backup-dpm-mabs-previous-versions/backup-now.png)

    Um den entsprechenden Workflow in DPM oder Azure Backup Server abzuschließen, klicken Sie mit der rechten Maustaste auf die **Schutzgruppe**. Wählen Sie die Option **Wiederherstellungspunkt erstellen** aus. Wählen Sie anschließend die Option **Onlineschutz** aus.

    ![DPM und MABS – Jetzt sichern](./media/offline-backup-dpm-mabs-previous-versions/dpm-backup-now.png)

    Nach Abschluss des Vorgangs kann der Stagingspeicherort für die Datenträgervorbereitung verwendet werden.

    ![Sicherungsstatus](./media/offline-backup-dpm-mabs-previous-versions/op-backup-now.png)

### <a name="prepare-sata-drives-and-ship-to-azure"></a>Vorbereiten der SATA-Laufwerke und Senden an Azure

Das Hilfsprogramm *AzureOfflineBackupDiskPrep* wird zum Vorbereiten der SATA-Laufwerke verwendet, die an das nächste Azure-Rechenzentrum gesendet werden. Dieses Hilfsprogramm steht im Installationsverzeichnis des Recovery Services-Agents unter folgendem Pfad zur Verfügung:

`*\\Microsoft Azure Recovery Services Agent\Utils\*`

1. Navigieren Sie zu diesem Verzeichnis, und kopieren Sie das Verzeichnis *AzureOfflineBackupDiskPrep* auf einen Kopiercomputer, mit dem die vorzubereitenden SATA-Laufwerke verbunden sind. Stellen Sie Folgendes sicher:

   * Der Kopiercomputer kann über denselben Netzwerkpfad, der während des Workflows im Abschnitt „Initiieren der Offlinesicherung“ angegeben wurde, auf den Stagingspeicherort für den Offlineseeding-Workflow zugreifen.
   * BitLocker ist auf dem Kopiercomputer aktiviert.
   * Der Kopiercomputer kann auf das Azure-Portal zugreifen. Sofern erforderlich, kann der Kopiercomputer mit dem Quellcomputer identisch sein.

     > [!IMPORTANT]
     > Wenn der Quellcomputer ein virtueller Computer ist, muss ein anderer physischer Server- oder Clientcomputer als Kopiercomputer verwendet werden.

1. Öffnen Sie auf dem Kopiercomputer eine Eingabeaufforderung mit erhöhten Rechten und dem Verzeichnis des Hilfsprogramms *AzureOfflineBackupDiskPrep* als aktuelles Verzeichnis. Führen Sie den folgenden Befehl aus:

    `*.\AzureOfflineBackupDiskPrep.exe*   s:<*Staging Location Path*>   [p:<*Path to AzurePublishSettingsFile*>]`

    | Parameter | Beschreibung |
    | --- | --- |
    | s:&lt;*Pfad zum Stagingspeicherort*&gt; |Mit dieser obligatorischen Eingabe wird der Pfad zum Stagingspeicherort angegeben, den Sie im Workflow im Abschnitt „Initiieren der Offlinesicherung“ eingegeben haben. |
    | p:&lt;*Pfad zu PublishSettingsFile*&gt; |Mit dieser optionalen Eingabe wird der Pfad zur Datei mit den Azure-Veröffentlichungseinstellungen angegeben, den Sie im Workflow im Abschnitt „Initiieren der Offlinesicherung“ eingegeben haben. |

    > [!NOTE]
    > Der Wert &lt;Pfad zu PublishSettingFile&gt; ist obligatorisch, wenn der Kopiercomputer und der Quellcomputer nicht identisch sind.
    >
    >

    Beim Ausführen des Befehls fordert das Hilfsprogramm die Auswahl des Azure-Importauftrags an, der den vorzubereitenden Datenträgern entspricht. Falls dem angegebenen Stagingspeicherort nur ein einzelner Importauftrag zugeordnet ist, wird eine Seite wie diese angezeigt.

    ![Azure-Tool für die Datenträgervorbereitung – Eingabe](./media/offline-backup-dpm-mabs-previous-versions/azure-disk-preparation-tool-drive-input.png) <br/>

1. Geben Sie den Laufwerkbuchstaben ohne nachgestellten Doppelpunkt für den bereitgestellten Datenträger an, den Sie für die Übertragung an Azure vorbereiten möchten. Bestätigen Sie die Formatierung des Laufwerks, wenn Sie dazu aufgefordert werden.

    Das Tool beginnt dann, den Datenträger vorzubereiten und die Sicherungsdaten zu kopieren. Falls auf dem bereitgestellten Datenträger nicht genügend Speicherplatz für die Sicherungsdaten verfügbar ist, müssen Sie unter Umständen weitere Datenträger anfügen, wenn Sie vom Tool dazu aufgefordert werden. <br/>

    Nach der erfolgreichen Ausführung des Tools sind ein oder mehrere Datenträger, die Sie angegeben haben, für den Versand an Azure vorbereitet. In Azure wird ein Importauftrag mit dem Namen erstellt, den Sie während des Workflows im Abschnitt „Initiieren der Offlinesicherung“ angegeben haben. Zum Schluss zeigt das Hilfsprogramm die Lieferadresse des Azure-Rechenzentrums an, an das die Datenträger gesendet werden müssen.

    ![Azure-Tool für die Datenträgervorbereitung – Abgeschlossen](./media/offline-backup-dpm-mabs-previous-versions/azure-disk-preparation-tool-success.png)<br/>

1. Am Ende der Befehlsausführung wird auch die Option zum Aktualisieren der Versandinformationen angezeigt.

    ![Option zum Aktualisieren der Versandinformationen](./media/offline-backup-dpm-mabs-previous-versions/update-shipping-utility.png)<br/>

1. Sie können die Details sofort eingeben. Das Tool führt Sie durch die verschiedenen Eingaben. Wenn Ihnen Informationen wie die Nachverfolgungsnummer oder andere Daten zum Versand nicht bekannt sind, können Sie die Sitzung auch beenden. Sie finden in diesem Artikel die Schritte zur späteren Aktualisierung der Versandinformationen.

1. Senden Sie die Datenträger an die im Tool angegebene Adresse, und bewahren Sie die Nummer für die Nachverfolgung zur späteren Referenz auf.

   > [!IMPORTANT]
   > Zwei Azure-Importaufträge dürfen nicht dieselbe Nachverfolgungsnummer aufweisen. Stellen Sie sicher, dass die vom Hilfsprogramm unter einem gemeinsamen Azure-Importauftrag vorbereiteten Laufwerke zusammen in einem einzelnen Paket versendet werden und dass das Paket eine einzelne eindeutige Nachverfolgungsnummer besitzt. Fassen Sie Laufwerke, die im Rahmen unterschiedlicher Azure Importaufträge vorbereitet wurden, nicht in einem Paket zusammen.

1. Wenn Sie über die Nachverfolgungsnummer verfügen, wechseln Sie zum Quellcomputer, der auf den Abschluss des Importauftrags wartet. Führen Sie den folgenden Befehl an einer Eingabeaufforderung mit erhöhten Rechten und dem Verzeichnis des Hilfsprogramms *AzureOfflineBackupDiskPrep* als aktuelles Verzeichnis aus.

   `*.\AzureOfflineBackupDiskPrep.exe*  u:`

   Sie können alternativ auch den folgenden Befehl auf einem anderen Computer als dem Kopiercomputer mit dem Verzeichnis des Hilfsprogramms *AzureOfflineBackupDiskPrep* als aktuelles Verzeichnis ausführen.

   `*.\AzureOfflineBackupDiskPrep.exe*  u:  s:<*Staging Location Path*>   p:<*Path to AzurePublishSettingsFile*>`

    | Parameter | Beschreibung |
    | --- | --- |
    | u: | Diese erforderliche Eingabe wird zum Aktualisieren der Versanddetails für einen Azure-Importauftrag verwendet. |
    | s:&lt;*Pfad zum Stagingspeicherort*&gt; | Diese erforderliche Eingabe wird verwendet, wenn der Befehl nicht auf dem Quellcomputer ausgeführt wird. Mit dieser Eingabe wird der Pfad zum Stagingspeicherort angegeben, den Sie im Workflow im Abschnitt „Initiieren der Offlinesicherung“ eingegeben haben. |
    | p:&lt;*Pfad zu PublishSettingsFile*&gt; | Diese erforderliche Eingabe wird verwendet, wenn der Befehl nicht auf dem Quellcomputer ausgeführt wird. Hiermit wird der Pfad zur Datei mit den Azure-Veröffentlichungseinstellungen angegeben, den Sie während des Workflows im Abschnitt „Initiieren der Offlinesicherung“ eingegeben haben. |

    Das Hilfsprogramm erkennt automatisch den Importauftrag, auf den der Quellcomputer wartet, oder die Importaufträge mit dem Stagingspeicherort, sofern der Befehl auf einem anderen Computer ausgeführt wird. Es zeigt dann die Option zum Aktualisieren der Versandinformationen über eine Reihe von Eingaben an.

    ![Versandinformationen eingeben](./media/offline-backup-dpm-mabs-previous-versions/shipping-inputs.png)<br/>

1. Nachdem alle Eingaben bereitgestellt wurden, überprüfen Sie die Details sorgfältig, und bestätigen Sie die von Ihnen bereitgestellten Versandinformationen durch Eingabe von **Ja**.

    ![Versandinformationen überprüfen](./media/offline-backup-dpm-mabs-previous-versions/review-shipping-information.png)<br/>

1. Nachdem die Versandinformationen erfolgreich aktualisiert wurden, stellt das Hilfsprogramm einen lokalen Speicherort bereit, an dem die von Ihnen eingegebenen Versandinformationen gespeichert werden.

    ![Versandinformationen speichern](./media/offline-backup-dpm-mabs-previous-versions/storing-shipping-information.png)<br/>

   > [!IMPORTANT]
   > Stellen Sie sicher, dass die Laufwerke das Azure-Rechenzentrum innerhalb von zwei Wochen nach der Bereitstellung der Versandinformationen mithilfe des Hilfsprogramms *AzureOfflineBackupDiskPrep* erreichen. Andernfalls werden die Laufwerke nicht bearbeitet.

Nachdem Sie die vorstehenden Schritte abgeschlossen haben, kann das Azure-Rechenzentrum die Laufwerke annehmen und weiter verarbeiten, um die zu sichernden Daten von den Laufwerken in das klassische Azure Storage-Konto zu übertragen, das Sie erstellt haben.

### <a name="time-to-process-the-drives"></a>Verarbeitungsdauer der Laufwerke

Der Zeitaufwand zum Verarbeiten eines Azure Import/Export-Auftrags variiert. Die Verarbeitungsdauer hängt von Faktoren wie Versanddauer, Auftragstyp, Typ und Größe der zu kopierenden Daten und Größe der bereitgestellten Datenträger ab. Der Azure Import/Export-Dienst verfügt über keine Vereinbarung zum Servicelevel (SLA). Nach Erhalt der Datenträger versucht der Dienst jedoch, das Kopieren der Sicherungsdaten in Ihr Azure Storage-Konto innerhalb von sieben bis zehn Tagen fertigzustellen. Im nächsten Abschnitt wird beschrieben, wie Sie den Status des Azure-Importauftrags überwachen können.

### <a name="monitor-azure-import-job-status"></a>Überwachen des Status von Azure-Importaufträgen

Während Ihre Laufwerke gesendet werden oder sich im Azure-Rechenzentrum befinden, um in das Speicherkonto kopiert zu werden, zeigt der Azure Backup-Agent oder SC DPM oder die MABS-Konsole auf dem Quellcomputer den folgenden Auftragsstatus für geplante Sicherungen an:

  `Waiting for Azure Import Job to complete. Please check on Azure Management portal for more information on job status`

So überprüfen Sie den Status des Importauftrags

1. Öffnen Sie auf dem Quellcomputer eine Eingabeaufforderung mit erhöhten Rechten, und führen Sie den folgenden Befehl aus:

     `AzureOfflineBackupDiskPrep.exe u:`

1. Die Ausgabe zeigt den aktuellen Status des Importauftrags an.

    ![Importauftragsstatus überprüfen](./media/offline-backup-dpm-mabs-previous-versions/import-job-status-reporting.png)<br/>

Weitere Informationen zu den verschiedenen Status von Azure-Importaufträgen finden Sie unter [Anzeigen des Status von Azure Import/Export-Aufträgen](../storage/common/storage-import-export-view-drive-status.md).

### <a name="finish-the-workflow"></a>Workflow abschließen

Nach Abschluss des Importauftrags sind die Daten der Erstsicherung in Ihrem Speicherkonto verfügbar. Bei der nächsten geplanten Sicherung kopiert Azure Backup den Inhalt der Daten aus dem Speicherkonto in den Recovery Services-Tresor.

   ![Daten in den Recovery Services-Tresor kopieren](./media/offline-backup-dpm-mabs-previous-versions/copying-from-storage-account-to-azure-backup.png)<br/>

Zum nächsten geplanten Sicherungszeitpunkt führt Azure Backup eine inkrementelle Sicherung für die erste Sicherungskopie durch.

## <a name="next-steps"></a>Nächste Schritte

* Falls Sie Fragen zum Workflow des Azure Import/Export-Diensts haben, finden Sie unter [Verwenden des Microsoft Azure Import/Export-Diensts zum Übertragen von Daten in den Blobspeicher](../storage/common/storage-import-export-service.md) weitere Informationen.
