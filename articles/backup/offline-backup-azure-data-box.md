---
title: Offlinesicherung mit Azure Data Box
description: In diesem Artikel erfahren Sie, wie Sie Azure Data Box verwenden können, um ein Offlineseeding für eine große Menge Erstsicherungsdaten vom MARS-Agent in einen Recovery Services-Tresor von Azure auszuführen.
ms.topic: conceptual
ms.date: 1/27/2020
ms.openlocfilehash: 553939df8aa7a1b097b2cd6c7f29365302101324
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2020
ms.locfileid: "77026494"
---
# <a name="azure-backup-offline-backup-using-azure-data-box"></a>Offlinesicherung in Azure Backup mit Azure Data Box

Sie können den Dienst [Azure Data Box](https://docs.microsoft.com/azure/databox/data-box-overview) verwenden, um ein Offlineseeding (ohne Verwendung eines Netzwerks) für eine große Menge an MARS-Erstsicherungsdateien in einen Recovery Services-Tresor von Azure auszuführen.  So können Sie sowohl Zeit als auch Netzwerkbandbreite einsparen, die andernfalls erforderlich wären, um die große Menge an Sicherungsdaten über ein Netzwerk mit hoher Latenz online zu stellen. Diese Optimierung befindet sich zurzeit in der Vorschauphase. Mit einer Offlinesicherung mit Azure Data Box entstehen Ihnen zwei entscheidende Vorteile im Vergleich zur [Offlinesicherung mit dem Azure Import/Export-Dienst in Azure Backup](https://docs.microsoft.com/azure/backup/backup-azure-backup-import-export):

1. Sie benötigen keine eigenen mit Azure kompatiblen Datenträger und Connectors. Die der ausgewählten [Data Box-SKU](https://azure.microsoft.com/services/databox/data/) zugeordneten Datenträger sind im Azure Data Box-Dienst enthalten.

2. Azure Backup (der MARS-Agent) kann die Sicherungsdaten direkt in die unterstützten SKUs des Azure Data Box-Diensts schreiben. Dadurch müssen Sie keinen Stagingspeicherort für Ihre Erstsicherungsdaten mehr bereitstellen und benötigen auch keine Hilfsprogramme mehr, um diese Daten zu formatieren und auf die Datenträger zu kopieren.

## <a name="azure-data-box-with-mars-agent"></a>Azure Data Box mit MARS-Agent

In diesem Artikel erfahren Sie, wie Sie Azure Data Box verwenden können, um ein Offlineseeding für eine große Menge Erstsicherungsdaten vom MARS-Agent in einen Recovery Services-Tresor von Azure auszuführen. Der Abschnitt ist folgendermaßen gegliedert:

* Unterstützte Plattformen
* Unterstützte Data Box-SKUs
* Voraussetzungen
* Einrichten des MARS-Agents
* Einrichten von Azure Data Box
* Übertragung der Sicherungsdaten zu Azure Data Box
* Schritte nach der Sicherung

## <a name="supported-platforms"></a>Unterstützte Plattformen

Der Seedingprozess für Daten vom MARS-Agent mithilfe von Azure Data Box wird für folgende Windows-SKUs unterstützt:

| **Betriebssystem**                                 | **SKU**                                                      |
| -------------------------------------- | ------------------------------------------------------------ |
| **Arbeitsstation**                        |                                                              |
| Windows 10, 64 Bit                     | Enterprise, Pro, Home                                       |
| Windows 8.1, 64 Bit                    | Enterprise, Pro                                             |
| Windows 8, 64 Bit                      | Enterprise, Pro                                             |
| Windows 7, 64 Bit                      | Ultimate, Enterprise, Professional, Home Premium, Home Basic, Starter |
| **Server**                             |                                                              |
| Windows Server 2019, 64 Bit            | Standard, Datacenter, Essentials                            |
| Windows Server 2016, 64 Bit            | Standard, Datacenter, Essentials                            |
| Windows Server 2012 R2, 64 Bit         | Standard, Datacenter, Foundation                            |
| Windows Server 2012, 64 Bit            | Datacenter, Foundation, Standard                            |
| Windows Storage Server 2016, 64 Bit    | Standard, Workgroup                                         |
| Windows Storage Server 2012 R2, 64 Bit | Standard, Workgroup, Essential                              |
| Windows Storage Server 2012, 64 Bit    | Standard, Workgroup                                         |
| Windows Server 2008 R2 SP1, 64 Bit     | Standard, Enterprise, Datacenter, Foundation                |
| Windows Server 2008 SP2, 64 Bit        | Standard, Enterprise, Datacenter                            |

## <a name="backup-data-size-and-supported-data-box-skus"></a>Größe von Sicherungsdaten und unterstützte Data Box-SKUs

| Größe der Sicherungsdaten (nach MARS-Komprimierung)* pro Server | Unterstützte Azure Data Box-SKU                                      |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| <= 7,2 TB                                                    | [Azure Data Box Disk](https://docs.microsoft.com/azure/databox/data-box-disk-overview) |
| > 7,2 TB und <= 80 TB**                                      | [Azure Data Box (100 TB)](https://docs.microsoft.com/azure/databox/data-box-overview) |

* Typische Komprimierungsraten variieren zwischen 10 bis 20 Prozent. <br>
** Wenden Sie sich an [AskAzureBackupTeam@microsoft.com](mailto:AskAzureBackupTeam@microsoft.com), wenn Sie vermuten, dass Sie über mehr als 80 TB an Erstsicherungsdaten für einen einzelnen MARS-Server verfügen.

>[!IMPORTANT]
>Erstsicherungsdaten von einem einzelnen Server müssen in einer einzelnen Azure Data Box- oder Azure Data Box Disk-SKU enthalten sein und können nicht für mehrere Geräte derselben oder verschiedener SKUs freigegeben werden. Ein Azure Data Box-Gerät kann jedoch Erstsicherungsdaten von mehreren Servern enthalten.

## <a name="pre-requisites"></a>Voraussetzungen

### <a name="azure-subscription-and-required-permissions"></a>Azure-Abonnement und erforderliche Berechtigungen

* Für den Prozess ist ein Azure-Abonnement erforderlich.
* Für den Prozess ist erforderlich, dass der Benutzer, der die Offlinesicherungsrichtlinie ausführen soll, „Besitzer“ des Azure-Abonnements ist.
* Der Data Box-Auftrag und der Recovery Services-Tresor (das Ziel des Datenseedings) müssen sich im selben Abonnement befinden.
* Das Zielspeicherkonto für den Azure Data Box-Auftrag und der Recovery Services-Tresor sollten sich in derselben Region befinden. Dies ist jedoch nicht notwendig.

### <a name="get-azure-powershell-370"></a>Abrufen von Azure PowerShell 3.7.0

**Hierbei handelt es sich um die wichtigste Voraussetzung für den Prozess.** Bevor Sie Azure PowerShell installieren (Version 3.7.0), müssen Sie die folgenden Schritte ausführen:**

#### <a name="step-1-check-powershell-version"></a>Schritt 1: Überprüfen der PowerShell-Version

* Öffnen Sie Windows PowerShell, und führen Sie den folgenden Befehl aus:

    ```powershell
    Get-Module -ListAvailable AzureRM*
    ```

* Führen Sie den unten aufgeführten Schritt 2 aus, wenn in der Ausgabe eine Version höher als **Version 3.7.0** angezeigt wird. Fahren Sie andernfalls mit Schritt 3 fort.

#### <a name="step-2-uninstall-the-powershell-version"></a>Schritt 2: Deinstallieren der PowerShell-Version

Deinstallieren Sie die aktuelle Version von PowerShell, indem Sie die folgenden Aktionen durchführen:

* Entfernen Sie die abhängigen Module, indem Sie den folgenden Befehl in PowerShell ausführen:

    ```powershell
    foreach ($module in (Get-Module -ListAvailable AzureRM*).Name |Get-Unique)  { write-host "Removing Module $module" Uninstall-module $module }
    ```

* Führen Sie den folgenden Befehl aus, um sicherzustellen, dass alle abhängigen Module erfolgreich entfernt wurden:

    ```powershell
    Get-Module -ListAvailable AzureRM*
    ```

#### <a name="step-3-install-powershell-version-370"></a>Schritt 3: Installieren der PowerShell-Version 3.7.0

Sobald Sie bestätigen konnten, dass keine AzureRM-Module mehr vorhanden sind, installieren Sie die Version 3.7.0 mithilfe einer der folgenden Methoden:

* Klicken Sie auf [diesen Link](https://github.com/Azure/azure-powershell/releases/tag/v3.7.0-March2017), um die Version von GitHub zu installieren.

oder

* Führen Sie den folgenden Befehl im PowerShell-Fenster aus:

    ```powershell
    Install-Module -Name AzureRM -RequiredVersion 3.7.0
    ```

Azure PowerShell könnte auch mithilfe einer MSI-Datei installiert werden. Wenn Sie Azure PowerShell wieder entfernen möchten, deinstallieren Sie die Umgebung mithilfe der Option „Programme deinstallieren“ in der Systemsteuerung.

### <a name="order-and-receive-the-data-box-device"></a>Bestellen und Empfangen des Data Box-Geräts

Für den Offlinesicherungsprozess mithilfe von MARS und Azure Data Box ist es erforderlich, dass die erforderlichen Data Box-Geräte den Zustand „Übermittelt“ aufweisen, bevor die Offlinesicherung mithilfe des MARS-Agents ausgelöst wird. Im Abschnitt [Größe von Sicherungsdaten und unterstützte Data Box-SKUs](#backup-data-size-and-supported-data-box-skus) finden Sie die nötigen Informationen, mit denen Sie die für Ihre Anforderung am besten geeignete SKU bestellen können. Führen Sie die Schritte in [diesem Artikel](https://docs.microsoft.com/azure/databox/data-box-disk-deploy-ordered) aus, um Ihre Data Box-Geräte zu bestellen und zu empfangen.

>[!IMPORTANT]
>Wählen Sie als Kontoart nicht BlobStorage aus. Für den MARS-Agent ist ein Konto erforderlich, das Seitenblobs unterstützt, was nicht der Fall ist, wenn BlobStorage ausgewählt wird. Bei der Erstellung des Zielspeicherkontos für Ihren Azure Data Box-Auftrag sollten Sie als Kontoart die Option *StorageV2* (*allgemein, Version 2*) auswählen.

![Auswählen der Kontoart in den Instanzdetails](./media/offline-backup-azure-data-box/instance-details.png)

## <a name="install-and-setup-the-mars-agent"></a>Installieren und Einrichten des MARS-Agents

* Vergewissern Sie sich, dass Sie alle vorherigen Installationen des MARS-Agents deinstalliert haben.

* Laden Sie die aktuelle Version des MARS-Agents [hier](https://aka.ms/azurebackup_agent) herunter.

* Führen Sie *MARSAgentInstaller.exe* aus, und führen Sie ***nur** die Schritte zum [Installieren und Registrieren des Agents](https://docs.microsoft.com/azure/backup/backup-configure-vault#install-and-register-the-agent) aus. Wählen Sie dabei den Recovery Services-Tresor aus, in dem Ihre Sicherungen gespeichert werden sollen.

  >[!NOTE]
  > Der Recovery Services-Tresor muss sich im selben Abonnement wie der Azure Data Box-Auftrag befinden.

* Führen Sie die unten aufgeführten Schritte aus, sobald der Agent für den Recovery Services-Tresor registriert ist.

## <a name="setup-azure-data-box-devices"></a>Einrichten von Azure Data Box-Geräten

Führen Sie je nach der von Ihnen bestellten Azure Data Box-SKU die unten in den jeweiligen Abschnitten erläuterten Schritte aus, um die Data Box-Geräte für den MARS-Agent einzurichten und vorzubereiten und dann die Erstsicherungsdaten identifizieren und übertragen zu können.

### <a name="setup-azure-data-box-disk"></a>Einrichten von Azure Data Box Disk

Führen Sie die hier genannten Schritte zum [Entpacken, Verbinden und Entsperren von Azure Data Box Disk](https://docs.microsoft.com/azure/databox/data-box-disk-deploy-set-up) aus, wenn Sie mindestens eine Azure Data Box Disk-Bestellung getätigt haben (jeweils bis zu 8 TB).

>[!NOTE]
>Möglicherweise hat der Server für den MARS-Agent keinen USB-Anschluss. In diesem Fall können Sie Azure Data Box Disk mit einem anderen Server/Client verbinden und den Gerätepfad als Netzwerkfreigabe angeben.

### <a name="setup-azure-data-box"></a>Einrichten von Azure Data Box

Führen Sie die hier genannten Schritte zum [Verkabeln und Herstellen einer Verbindung mit Azure Data Box](https://docs.microsoft.com/azure/databox/data-box-deploy-set-up) aus, wenn Sie eine Azure Data Box-Bestellung getätigt haben (bis zu 100 TB).

#### <a name="mount-your-azure-data-box-as-local-system"></a>Einbinden von Azure Data Box als lokales System

Der MARS-Agent wird im Kontext des lokalen Systems ausgeführt. Deshalb ist es erforderlich, dass für den Einbindungspfad dieselben Berechtigungen gewährt werden, über den Azure Data Box verbunden ist. Führen Sie die unten aufgeführten Schritte aus, damit Sie Ihr Data Box-Gerät mithilfe des NFS-Protokolls als lokales System einbinden können:

* Aktivieren Sie das Feature „Client für NFS“ auf dem Windows-Server, auf dem der MARS-Agent installiert ist.<br>
  Geben Sie als alternative Quelle Folgendes an: *WIM:D:\Sources\Install.wim:4*

* Laden Sie PSExec von <https://download.sysinternals.com/files/PSTools.zip> auf den Server herunter, auf dem der MARS-Agent installiert ist.

* Öffnen Sie eine Eingabeaufforderung mit erhöhten Rechten, und führen Sie den folgenden Befehl aus. Das Verzeichnis muss dabei PSExec.exe als aktuelles Verzeichnis enthalten:

    ```cmd
    psexec.exe  -s  -i  cmd.exe
    ```

* Das Befehlsfenster, das durch den obigen Befehl geöffnet wird, befindet sich im lokalen Systemkontext. Führen Sie die Schritte zum Einbinden der Seitenblobfreigabe von Azure als Netzwerklaufwerk auf Ihrem Windows-Server in diesem Befehlsfenster aus.

* Führen Sie die [hier](https://docs.microsoft.com/azure/databox/data-box-deploy-copy-data-via-nfs#connect-to-data-box) aufgeführten Schritte aus, um Ihren Server und den MARS-Agent über NFS mit dem Data Box-Gerät zu verbinden. Führen Sie dann den folgenden Befehl in der Eingabeaufforderung des lokalen Systems aus, um die Seitenblobfreigabe von Azure einzubinden:

    ```cmd
    mount -o nolock \\<DeviceIPAddress>\<StorageAccountName_PageBlob X:  
    ```

* Überprüfen Sie, ob Sie über Ihren Server auf X: zugreifen können, sobald die Einbindung abgeschlossen ist. Wenn dies der Fall ist, fahren Sie mit dem nächsten Abschnitt dieses Artikels fort.

## <a name="transfer-initial-backup-data-to-azure-data-box-devices"></a>Übertragen von Erstsicherungsdaten auf Azure Data Box-Geräte

* Öffnen Sie die **Microsoft Azure Backup**-Anwendung auf Ihrem Server.

* Klicken Sie im Bereich **Aktionen** auf die Option **Sicherung planen**.

    ![Klicken auf die Option „Sicherung planen“](./media/offline-backup-azure-data-box/schedule-backup.png)

* Führen Sie die Schritte des **Zeitplansicherungs-Assistenten** aus.

* **Fügen Sie Elemente hinzu**, ohne dass die Gesamtgröße der Elemente dabei die zulässige [Größe von Sicherungsdaten für die von Ihnen bestellte und empfangene Data Box-SKU](#backup-data-size-and-supported-data-box-skus) überschreitet.

    ![Hinzufügen von Elementen zur Sicherung](./media/offline-backup-azure-data-box/add-items.png)

* Wählen Sie den geeigneten Sicherungsplan und die geeignete Aufbewahrungsrichtlinie für Dateien und Ordner und den Systemstatus aus. Der Systemstatus ist nur für Windows-Server und nicht für Windows-Clients verfügbar.

* Klicken Sie auf dem Bildschirm **Typ für die erste Sicherung wählen (Dateien und Ordner)** des Assistenten zunächst auf die Option **Transfer using Microsoft Azure Data Box disks** (Übertragung mithilfe von Microsoft Azure Data Box-Datenträgern) und dann auf **Weiter**.

    ![Erstsicherungstyp wählen](./media/offline-backup-azure-data-box/initial-backup-type.png)

* Melden Sie sich bei einer entsprechenden Aufforderung bei Azure an, und verwenden Sie dazu die Benutzeranmeldeinformationen, mit denen Sie als „Besitzer“ Zugriff auf das Azure-Abonnement haben. Nachdem Sie das getan haben, sollte ein Bildschirm ähnlich dem folgenden angezeigt werden:

    ![Erstellen von Ressourcen und Anwenden von erforderlichen Berechtigungen](./media/offline-backup-azure-data-box/creating-resources.png)

* Der MARS-Agent ruft dann die Data Box-Aufträge im Abonnement ab, die den Zustand „Übermittelt“ aufweisen.

    ![Abrufen von Data Box-Aufträgen nach Abonnement-ID](./media/offline-backup-azure-data-box/fetching-databox-jobs.png)

* Wählen Sie den Data Box-Auftrag aus, für den Sie Ihren Data Box-Datenträger entpackt, verbunden und entsperrt haben. Klicken Sie auf **Weiter**.

    ![Data Box-Aufträge auswählen](./media/offline-backup-azure-data-box/select-databox-order.png)

* Klicken Sie auf dem Bildschirm **Data Box-Geräteerkennung** auf die Option **Gerät erkennen**. Der MARS-Agent beginnt dann, lokal angefügte Azure Data Box-Datenträger zu überprüfen und sie wie unten gezeigt zu erkennen:

    ![Data Box-Geräteerkennung](./media/offline-backup-azure-data-box/databox-device-detection.png)

    Wenn Sie Azure Data Box als Netzwerkfreigabe verbunden haben, da keine USB-Anschlüsse verfügbar sind oder weil Sie das Data Box-Gerät mit 100 TB bestellt und eingebunden haben, schlägt die Geräteerkennung zunächst fehl. Sie erhalten aber die Option, den Netzwerkpfad zum Data Box-Gerät wie unten gezeigt einzugeben:

    ![Eingeben des Netzwerkpfads](./media/offline-backup-azure-data-box/enter-network-path.png)

    >[!IMPORTANT]
    > Geben Sie den Netzwerkpfad zum Stammverzeichnis des Azure Data Box-Datenträgers an. Wie Sie unten sehen, muss dieses Verzeichnis ein Verzeichnis namens *PageBlob* beinhalten:
    >
    >![Stammverzeichnis des Azure Data Box-Datenträgers](./media/offline-backup-azure-data-box/root-directory.png)
    >
    >Wenn der Pfad auf dem Datenträger beispielsweise `\\mydomain\myserver\disk1\` lautet und *disk1* ein Verzeichnis namens *PageBlob* enthält, muss auf dem MARS-Agent-Assistenten `\\mydomain\myserver\disk1\` als Pfad angegeben werden.
    >
    >Beim [Einrichten eines Azure Data Box-Geräts mit 100 TB](#setup-azure-data-box) müssen Sie folgenden Netzwerkpfad als Pfad zum Gerät angeben: `\\<DeviceIPAddress>\<StorageAccountName>_PageBlob`.

* Klicken Sie zunächst auf **Weiter** und dann auf dem nächsten Bildschirm auf **Fertig stellen**, um die Sicherungs- und Aufbewahrungsrichtlinie mit der Konfiguration der Offlinesicherung mithilfe von Azure Data Box zu speichern.

* Im folgenden Bildschirm wird bestätigt, dass die Richtlinie erfolgreich gespeichert wurde:

    ![Bestätigung, dass die Richtlinie erfolgreich gespeichert wurde](./media/offline-backup-azure-data-box/policy-saved.png)

* Klicken Sie im Bildschirm oben auf **Schließen**.

* Klicken Sie im Bereich **Aktionen** der MARS-Agent-Konsole zunächst auf ***Jetzt sichern** und dann im Assistentenbildschirm wie unten gezeigt auf die Option **Sichern**:

    ![Assistent für die sofortige Sicherung](./media/offline-backup-azure-data-box/backup-now.png)

* Der MARS-Agent beginnt dann mit der Sicherung der von Ihnen ausgewählten Daten auf dem Azure Data Box-Gerät. Das kann je nach Anzahl der Dateien und der Verbindungsgeschwindigkeit zwischen dem Server mit dem MARS-Agent und Azure Data Box Disk ein paar Stunden bis hin zu einigen Tagen dauern.

* Sobald die Sicherung der Daten abgeschlossen ist, wird auf dem MARS-Agent ein Bildschirm ähnlich dem folgenden angezeigt:

    ![Angezeigter Sicherungsstatus](./media/offline-backup-azure-data-box/backup-progress.png)

## <a name="post-backup-steps"></a>Schritte nach der Sicherung

In diesem Abschnitt werden die Schritte erläutert, die ausgeführt werden müssen, sobald die Datensicherung auf dem Azure Data Box-Datenträger erfolgreich war.

* Führen Sie die Schritte in diesem Artikel zum [Zurücksenden des Azure Data Box-Datenträgers](https://docs.microsoft.com/azure/databox/data-box-disk-deploy-picked-up) aus. Führen Sie diese Schritte zum [Zurücksenden des Azure Data Box-Geräts und Überprüfen des Datenuploads in Azure](https://docs.microsoft.com/azure/databox/data-box-deploy-picked-up) aus, wenn Sie ein Azure Data Box-Gerät mit 100 TB verwendet haben.

* [Überprüfen Sie den Datenupload aus Azure Data Box Disk](https://docs.microsoft.com/azure/databox/data-box-disk-deploy-upload-verify) im Azure-Portal. Sobald der Azure Data Box-Auftrag „Abgeschlossen“ ist, verschiebt der MARS-Agent die Daten zum Zeitpunkt der nächsten geplanten Sicherung automatisch vom Speicherkonto in den Recovery Services-Tresor. Der Sicherungsauftrag wird dann als „Auftrag abgeschlossen“ markiert, sobald erfolgreich ein Wiederherstellungspunkt erstellt wurde.

    >[!NOTE]
    >Der MARS-Agent löst zu den Zeitpunkten, die während der Erstellung der Richtlinie festgelegt wurden, Sicherungen aus. Diese Aufträge werden jedoch als „Waiting for Azure Data Box to be completed“ (Auf Abschluss von Azure Data Box warten) gekennzeichnet, bis der Auftrag abgeschlossen ist.

* Nachdem der MARS-Agent erfolgreich einen der Erstsicherung entsprechenden Wiederherstellungspunkt erstellt hat, können Sie das mit dem Azure Data Box-Auftrag verbundene Speicherkonto oder darin enthaltene spezifische Inhalte löschen.

## <a name="troubleshooting"></a>Problembehandlung

Der Microsoft Azure Backup-Agent (MAB) erstellt für Sie eine Azure AD-Anwendung in Ihrem Mandanten. Für diese Anwendung ist zu Authentifizierungszwecken ein Zertifikat erforderlich, das bei der Konfiguration der Offlineseedingrichtlinie erstellt und hochgeladen wird. Für das Erstellen und Hochladen des Zertifikats für die Azure AD-Anwendung wird Azure PowerShell verwendet.

### <a name="issue"></a>Problem

Beim Konfigurieren der Offlinesicherung tritt möglicherweise ein Problem auf, bei dem Sie aufgrund eines Fehlers im Azure PowerShell-Cmdlet nicht mehrere Zertifikate für dieselbe vom MAB-Agent erstellte Azure AD-Anwendung hinzufügen können. Dies kann Sie beeinträchtigen, wenn Sie eine Offlineseedingrichtlinie für denselben oder einen anderen Server konfiguriert haben.

### <a name="how-to-verify-if-the-issue-is-caused-by-this-specific-root-cause"></a>Überprüfen, ob das Problem durch diese spezifische Grundursache verursacht wird

Führen Sie einen der folgenden Schritte aus, um zu überprüfen, ob der Fehler dem obigen Problem geschuldet ist:

#### <a name="step-1"></a>Schritt 1

Überprüfen Sie, ob die folgende Fehlermeldung in der MAB-Konsole angezeigt wird, wenn Sie die Offlinesicherung konfigurieren:

![Fehlermeldung „Die Richtlinie zur Offlinesicherung für das aktuelle Azure-Konto konnte nicht erstellt werden […]“](./media/offline-backup-azure-data-box/unable-to-create-policy.png)

#### <a name="step-2"></a>Schritt 2

* Öffnen Sie den **Temp**-Ordner im Installationspfad. Der Standardpfad zum Temp-Ordner lautet *C:\Programme\Microsoft Azure Recovery Services Agent\Temp*). Suchen Sie nach der **CBUICurr**-Datei, und öffnen Sie sie.

* Scrollen Sie in der **CBUICurr**-Datei zur letzten Zeile, und überprüfen Sie, ob der Fehler durch `Unable to create an Azure AD application credential in customer's account. Exception: Update to existing credential with KeyId <some guid> is not allowed` begründet werden kann.

### <a name="workaround"></a>Problemumgehung

Als Umgehung dieses Problems können Sie die folgenden Schritte ausführen und noch mal versuchen, die Richtlinie zu konfigurieren.

#### <a name="first-step"></a>Erster Schritt

Melden Sie sich bei PowerShell an (wird auf der MAB-Benutzeroberfläche angezeigt). Verwenden Sie dazu ein anderes Konto mit Administratorenzugriff auf das Abonnement, für das der Import-/Exportauftrag erstellt werden soll.

#### <a name="second-step"></a>Zweiter Schritt

Wenn das Offlineseeding für keinen anderen Server konfiguriert wurde und kein anderer Server von der `AzureOfflineBackup_<Azure User Id>`-Anwendung abhängig ist, löschen Sie diese Anwendung über **Azure-Portal** > **Azure Active Directory** > **App-Registrierungen**.

>[!NOTE]
> Überprüfen Sie, ob für die Anwendung `AzureOfflineBackup_<Azure User Id>` kein weiteres Offlineseeding konfiguriert wurde, und überprüfen Sie außerdem, ob kein weiterer Server von dieser Anwendung abhängig ist. Navigieren Sie im Bereich **Öffentliche Schlüssel** zu **Einstellungen** > **Schlüssel**. Dort sollten keine weiteren öffentlichen Schlüssel hinzugefügt worden sein. Als Referenz können Sie sich den folgenden Screenshot ansehen:
>
>![Öffentliche Schlüssel](./media/offline-backup-azure-data-box/public-keys.png)

#### <a name="third-step"></a>Dritter Schritt

Führen Sie für den Server, für den Sie die Offlinesicherung konfigurieren möchten, die folgenden Aktionen aus:

1. Öffnen Sie die Registerkarte **Manage computer certificate application** > **Personal** (Computerzertifikatanwendung verwalten > Persönlich), und suchen Sie nach dem Zertifikat mit dem Namen `CB_AzureADCertforOfflineSeeding_<ResourceId>`.

2. Wählen Sie das oben angegebene Zertifikat aus, klicken Sie mit der rechten Maustaste auf **Alle Aufgaben**, und führen Sie dann einen **Export** ohne privaten Schlüssel im CER-Format aus.

3. Navigieren Sie zur in **Schritt 2** genannten Anwendung, die mit Azure Backup offline gesichert werden soll. Laden Sie über **Einstellungen** > **Schlüssel** > **Öffentlichen Schlüssel hochladen** das im obigen Schritt exportierte Zertifikat hoch.

    ![Öffentlichen Schlüssel hochladen](./media/offline-backup-azure-data-box/upload-public-key.png)

4. Öffnen Sie auf dem Server die Registrierung, indem Sie im Fenster „Ausführen“ **regedit** eingeben.

5. Wechseln Sie zur Registrierung *Computer\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider*. Klicken Sie mit der rechten Maustaste auf **CloudBackupProvider**, und fügen Sie einen neuen Zeichenfolgenwert mit dem Namen `AzureADAppCertThumbprint_<Azure User Id>` hinzu.

    >[!NOTE]
    > Führen Sie einen der folgenden Aktionen aus, um die Azure-Benutzer-ID zu ermitteln:
    >
    >1. Führen Sie in PowerShell mit Azure-Verbindung den Befehl `Get-AzureRmADUser -UserPrincipalName “Account Holder’s email as defined in the portal”` aus.
    > 2. Navigieren Sie zum Registrierungspfad: *Computer\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\DbgSettings\OnlineBackup*; Name: *CurrentUserId*

6. Klicken Sie zunächst mit der rechten Maustaste auf die im vorherigen Schritt hinzugefügte Zeichenfolge und dann auf **Ändern**. Geben Sie als Wert den Fingerabdruck des Zertifikats an, das Sie in **Schritt 2** exportiert haben, und klicken Sie auf **OK**.

7. Doppelklicken Sie auf das Zertifikat, klicken Sie auf die Registerkarte **Details**, und scrollen Sie nach unten, bis das Feld „Fingerabdruck“ angezeigt wird, um den Wert des Fingerabdrucks zu erhalten. Klicken Sie auf **Fingerabdruck**, und kopieren Sie den Wert.

    ![Feld „Fingerabdruck“ des Zertifikats](./media/offline-backup-azure-data-box/thumbprint-field.png)

## <a name="questions"></a>Fragen

Wenn Sie weitere Informationen benötigen oder offene Fragen zu den hier behandelten Problemen haben, wenden Sie sich an [AskAzureBackupTeam@microsoft.com](mailto:AskAzureBackupTeam@microsoft.com).
