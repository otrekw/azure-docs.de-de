---
title: Installieren des Microsoft Azure Recovery Services-Agents (MARS)
description: Hier erfahren Sie, wie Sie den MARS-Agent (Microsoft Azure Recovery Services) zum Sichern von Windows-Computern installieren.
ms.topic: conceptual
ms.date: 03/03/2020
ms.openlocfilehash: fb59c245c469791233ce973b00426a127b116535
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90975302"
---
# <a name="install-the-azure-backup-mars-agent"></a>Installieren des Azure Backup-MARS-Agents

In diesem Artikel wird erläutert, wie Sie den Microsoft Azure Recovery Services-Agent (MARS) installieren. MARS wird auch als Azure Backup-Agent bezeichnet.

## <a name="about-the-mars-agent"></a>Informationen zum MARS-Agent

Azure Backup verwendet den MARS-Agent, um Dateien, Ordner und den Systemstatus von lokalen Computern und Azure-VMs zu sichern. Diese Sicherungen werden in einem Recovery Services-Tresor in Azure gespeichert. Sie können den Agent wie folgt ausführen:

* Direkt auf lokalen Windows-Computern. Diese Computer können direkt in einem Recovery Services-Tresor in Azure gesichert werden.
* Auf Azure-VMs, auf denen Windows parallel mit der Azure-VM-Backup-Erweiterung ausgeführt wird. Der Agent sichert bestimmte Dateien und Ordner auf der VM.
* Auf einer Microsoft Azure Backup Server-Instanz (MABS) oder einem System Center Data Protection Manager-Server (DPM). In diesem Szenario werden Computer und Workloads in MABS oder Data Protection Manager gesichert. Anschließend verwendet MABS oder Data Protection Manager den MARS-Agent zum Sichern in einem Tresor in Azure.

Welche Daten für die Sicherung verfügbar sind, hängt vom Installationsort des Agents ab.

> [!NOTE]
> Im Allgemeinen sichern Sie eine Azure-VM mithilfe einer Azure Backup-Erweiterung auf der VM. Bei dieser Methode wird die gesamte VM gesichert. Wenn Sie bestimmte Dateien und Ordner auf der VM sichern möchten, installieren und verwenden Sie den MARS-Agent parallel zur Erweiterung. Weitere Informationen finden Sie unter der [Architektur einer integrierten Azure-VM-Sicherung](backup-architecture.md#architecture-built-in-azure-vm-backup).

![Sicherungsprozessschritte](./media/backup-configure-vault/initial-backup-process.png)

## <a name="before-you-start"></a>Vorbereitung

* Informieren Sie sich darüber, wie [Azure Backup den MARS-Agent zum Sichern von Windows-Computern verwendet](backup-architecture.md#architecture-direct-backup-of-on-premises-windows-server-machines-or-azure-vm-files-or-folders).
* Informieren Sie sich über die [Sicherungsarchitektur](backup-architecture.md#architecture-back-up-to-dpmmabs), die den MARS-Agent auf einem sekundären MABS- oder Data Protection Manager-Server ausführt.
* Überprüfen Sie, [was unterstützt wird und was mit dem MARS-Agent gesichert werden kann](backup-support-matrix-mars-agent.md).
* Stellen Sie sicher, dass Sie über ein Azure-Konto verfügen, wenn Sie einen Server oder Client in Azure sichern müssen. Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen.
* Überprüfen Sie den Internetzugriff auf den Computern, die Sie sichern möchten.
* Stellen Sie sicher, dass der Benutzer, der den MARS-Agent installiert und konfiguriert, über lokale Administratorrechte auf dem zu schützenden Server verfügt.

[!INCLUDE [How to create a Recovery Services vault](../../includes/backup-create-rs-vault.md)]

## <a name="modify-storage-replication"></a>Ändern der Speicherreplikation

Tresore verwenden standardmäßig den [georedundanten Speicher (GRS)](../storage/common/storage-redundancy.md#geo-redundant-storage).

* Wenn der Tresor Ihr Hauptmechanismus für Sicherungen ist, empfehlen wir die Verwendung von GRS.
* Sie können [lokal redundanten Speicher (LRS)](../storage/common/storage-redundancy.md#locally-redundant-storage) verwenden, um die Azure-Speicherkosten zu reduzieren.

So ändern Sie den Speicherreplikationstyp

1. Wählen Sie im neuen Tresor im Abschnitt **Einstellungen** auf **Eigenschaften**.

1. Wählen Sie auf dem Blatt **Eigenschaften** unter **Sicherungskonfiguration** die Option **Aktualisieren** aus.

1. Wählen Sie den Speicherreplikationstyp und dann **Speichern** aus.

    ![Aktualisieren der Sicherungskonfiguration](./media/backup-afs/backup-configuration.png)

> [!NOTE]
> Sie können den Speichertyp für die Replikation nicht mehr ändern, nachdem der Tresor eingerichtet wurde und Sicherungselemente enthält. Dazu müssen Sie den Tresor neu erstellen.
>

### <a name="verify-internet-access"></a>Überprüfen des Internetzugriffs

Wenn Ihr Computer über einen eingeschränkten Internetzugriff verfügt, stellen Sie sicher, dass die Firewalleinstellungen auf dem Computer oder Proxy die folgenden URLs und IP-Adressen zulassen:

* URLs
  * `www.msftncsi.com`
  * `*.Microsoft.com`
  * `*.WindowsAzure.com`
  * `*.microsoftonline.com`
  * `*.windows.net`
  * `www.msftconnecttest.com`
* IP-Adressen
  * 20.190.128.0/18
  * 40.126.0.0/18

### <a name="use-azure-expressroute"></a>Verwenden von Azure ExpressRoute

Sie können Ihre Daten über Azure ExpressRoute mithilfe von öffentlichem Peering (verfügbar für alte Leitungen) und Microsoft-Peering sichern. Die Sicherung über privates Peering wird nicht unterstützt.

Um öffentliches Peering zu verwenden, stellen Sie zunächst sicher, dass Sie auf die folgenden Domänen und Adressen zugreifen können:

* `http://www.msftncsi.com/ncsi.txt`
* `http://www.msftconnecttest.com/connecttest.txt`
* `microsoft.com`
* `.WindowsAzure.com`
* `.microsoftonline.com`
* `.windows.net`

Um Microsoft-Peering zu verwenden, wählen Sie die folgenden Dienste, Regionen und relevanten Communitywerte aus:

* Azure Active Directory (12076:5060)
* Azure-Region, entsprechend dem Standort Ihres Recovery Services-Tresors
* Azure Storage, entsprechend dem Standort Ihres Recovery Services-Tresors

Weitere Informationen finden Sie unter [ExpressRoute-Routinganforderungen](../expressroute/expressroute-routing.md).

> [!NOTE]
> Öffentliches Peering gilt für neue Leitungen als veraltet.

Alle vorhergehenden URLs und IP-Adressen verwenden das HTTPS-Protokoll an Port 443.

### <a name="private-endpoints"></a>Private Endpunkte

[!INCLUDE [Private Endpoints](../../includes/backup-private-endpoints.md)]

## <a name="download-the-mars-agent"></a>Herunterladen des MARS-Agents

Laden Sie den MARS-Agent herunter, damit Sie ihn auf den zu sichernden Computern installieren können.

Wenn Sie den Agent bereits auf einem Computer installiert haben, stellen Sie sicher, dass die neueste Version des Agents verwendet wird. Suchen Sie die neueste Version im Portal, oder wechseln Sie direkt zum [Download](https://aka.ms/azurebackup_agent).

1. Wählen Sie im Tresor unter **Erste Schritte** die Option **Sicherung** aus.

    ![Öffnen des Sicherungsziels](./media/backup-try-azure-backup-in-10-mins/open-backup-settings.png)

1. Wählen Sie unter **Wo wird Ihre Workload ausgeführt?** die Option **Lokal** aus. Wählen Sie diese Option auch dann aus, wenn Sie den MARS-Agent auf einer Azure-VM installieren möchten.
1. Wählen Sie unter **Was möchten Sie sichern?** die Option **Dateien und Ordner** aus. Sie können auch **Systemstatus** auswählen. Es stehen viele weitere Optionen zur Verfügung, doch werden diese Optionen nur unterstützt, wenn Sie einen sekundären Sicherungsserver ausführen. Wählen Sie **Infrastruktur vorbereiten** aus.

    ![Konfigurieren von Dateien und Ordnern](./media/backup-try-azure-backup-in-10-mins/set-file-folder.png)

1. Laden Sie für **Infrastruktur vorbereiten** den MARS-Agent unter **Recovery Services-Agent installieren** herunter.

    ![Vorbereiten der Infrastruktur](./media/backup-try-azure-backup-in-10-mins/choose-agent-for-server-client.png)

1. Wählen Sie im Downloadmenü die Option **Speichern** aus. Die Datei *MARSagentinstaller.exe* wird standardmäßig in Ihrem Downloadordner gespeichert.

1. Wählen Sie **Bereits heruntergeladen oder Verwendung der neuesten Installation von Recovery Services-Agent** aus, und laden Sie dann die Anmeldeinformationen für den Tresor herunter.

    ![Herunterladen der Tresoranmeldedaten](./media/backup-try-azure-backup-in-10-mins/download-vault-credentials.png)

1. Wählen Sie **Speichern** aus. Die Datei wird in Ihren Ordner „Downloads“ heruntergeladen. Sie können die Datei mit den Anmeldeinformationen für den Tresor nicht öffnen.

## <a name="install-and-register-the-agent"></a>Installieren und Registrieren des Agents

1. Führen Sie die Datei *MARSagentinstaller.exe* auf den Computern aus, die Sie sichern möchten.
1. Wählen Sie im Setup-Assistenten des MARS-Agents die Option **Installationseinstellungen** aus. Wählen Sie dort den Installationsort für den Agent und einen Speicherort für den Cache aus. Wählen Sie **Weiter**aus.
   * Azure Backup verwendet den Cache, um Datenmomentaufnahmen zu speichern, bevor sie an Azure gesendet werden.
   * Der verfügbare Speicherplatz im Cachespeicherort sollte mindestens fünf Prozent der Größe der zu sichernden Daten entsprechen.

    ![Auswählen der Installationseinstellungen im Setup-Assistenten des MARS-Agents](./media/backup-configure-vault/mars1.png)

1. Geben Sie für **Proxykonfiguration** an, wie der Agent, der auf dem Windows-Computer ausgeführt wird, eine Verbindung mit dem Internet herstellen soll. Wählen Sie **Weiter**aus.

   * Wenn Sie einen benutzerdefinierten Proxy verwenden, geben Sie alle erforderlichen Proxyeinstellungen und Anmeldeinformationen an.
   * Denken Sie daran dass der MARS-Agent Zugriff auf [bestimmte URLs](#before-you-start) benötigt.

    ![Einrichten des Internetzugriffs im MARS-Assistenten](./media/backup-configure-vault/mars2.png)

1. Überprüfen Sie die Voraussetzungen für die **Installation**, und wählen Sie **Installieren** aus.
1. Wählen Sie nach der Installation des Agents die Option **Mit Registrierung fortfahren** aus.
1. Navigieren Sie unter **Assistent zum Registrieren von Servern** > **Tresoridentifikation** zu der Datei mit den Anmeldeinformationen, die Sie heruntergeladen haben, und wählen Sie die Datei aus. Wählen Sie **Weiter**aus.

    ![Hinzufügen von Tresoranmeldeinformationen mithilfe des Assistenten zum Registrieren von Servern](./media/backup-configure-vault/register1.png)

1. Geben Sie auf der Seite **Verschlüsselungseinstellung** eine Passphrase an, die zum Ver- und Entschlüsseln von Sicherungen für den Computer verwendet werden soll. Weitere Informationen zu zulässigen Zeichen in Passphrasen finden Sie [hier](backup-azure-file-folder-backup-faq.md#what-characters-are-allowed-for-the-passphrase).

    * Speichern Sie die Passphrase an einem sicheren Ort. Sie benötigen sie zum Wiederherstellen einer Sicherung.
    * Wenn Sie die Passphrase verlieren oder vergessen, kann Microsoft Ihnen bei der Wiederherstellung der Sicherungsdaten nicht behilflich sein.

1. Wählen Sie **Fertig stellen** aus. Der Agent ist jetzt installiert, und Ihr Computer ist im Tresor registriert. Sie können die Sicherung jetzt konfigurieren und planen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum [Sichern von Windows-Computern mit dem Azure Backup-MARS-Agent](backup-windows-with-mars-agent.md)
