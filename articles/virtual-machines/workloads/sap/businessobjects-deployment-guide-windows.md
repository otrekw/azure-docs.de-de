---
title: Bereitstellung der SAP BusinessObjects BI-Plattform in Azure für Linux | Microsoft-Dokumentation
description: Bereitstellen und Konfigurieren der SAP BusinessObjects BI-Plattform in Azure für Windows
services: virtual-machines-windows,virtual-network,storage,azure-netapp-files,azure-files,azure-sql
documentationcenter: saponazure
author: dennispadia
manager: juergent
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-windows
ms.subservice: workloads
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 04/08/2021
ms.author: depadia
ms.openlocfilehash: a13b62da91e3ce6f18ba4dcda2b3b0f22c815026
ms.sourcegitcommit: 4a54c268400b4158b78bb1d37235b79409cb5816
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108144383"
---
# <a name="sap-businessobjects-bi-platform-deployment-guide-for-windows-on-azure"></a>Bereitstellungsleitfaden für die SAP BusinessObjects BI-Plattform für Windows in Azure

In diesem Artikel wird die Strategie zum Bereitstellen der SAP BusinessObjects BI-Plattform in Azure für Windows beschrieben. In diesem Beispiel werden zwei virtuelle Computer mit SSD Premium Managed Disks als Installationsverzeichnis konfiguriert. Azure SQL Datenbank (PaaS-Angebot) wird für den zentralen Verwaltungsdienst (Central Management Service, CMS) und die Überwachungsdatenbank sowie Azure Premium Files (SMB-Protokoll) als Dateispeicher verwendet, der von beiden virtuellen Computern gemeinsam genutzt wird. Die standardmäßige Tomcat Java-Webanwendung und die BI-Plattformanwendung werden zusammen auf beiden virtuellen Computern installiert. Für den Lastenausgleich der Benutzeranforderungen wird das Azure Application Gateway verwendet, das native TLS/SSL-Auslagerungsfunktionen besitzt.

Diese Art von Architektur eignet sich für kleine Bereitstellungen oder Nicht-Produktionsumgebungen. Für die Produktion oder Bereitstellung in großem Stil sollten Sie getrennte Hosts für die Web Application sowie mehrere BOBI-Anwendungshosts verwenden, sodass der Server mehr Informationen verarbeiten kann.

![SAP BOBI-Bereitstellung in Azure für Windows](media/businessobjects-deployment-guide/businessobjects-deployment-windows.png)

In diesem Beispiel werden eine Version und ein Dateisystemlayout verwendet, die unter der Produktversion angeordnet sind.

- SAP BusinessObjects Plattform 4.3 SP01 Patch 1
- Windows Server 2019
- Azure SQL-Datenbank (Version: 12.0.2000.8)
- Microsoft ODBC-Treiber – msodbcsql.msi (Version: 13.1)

| Dateisystem        | BESCHREIBUNG                                                                                                               | Größe (GB)             | Erforderlicher Zugriff  | Storage                    |
|--------------------|---------------------------------------------------------------------------------------------------------------------------|-----------------------|---------------|----------------------------|
| F:          | Das Dateisystem für die Installation der SAP BOBI-Instanz, der standardmäßigen Tomcat-Webanwendung und der Datenbanktreiber (falls erforderlich). | SAP-Größenrichtlinien | Lokale Administratorrechte | Managed Disk – SSD Premium |
| \\\azusbobi.file.core.windows.net\frsinput  | Das Einbindungsverzeichnis ist für die freigegebenen Dateien aller BOBI-Hosts vorgesehen, die als Speicherverzeichnis der Eingabedatei verwendet werden. | Geschäftsanforderung         | Lokale Administratorrechte | Azure NetApp Files         |
| \\\azusbobi.file.core.windows.net\frsoutput | Das Einbindungsverzeichnis ist für die freigegebenen Dateien aller BOBI-Hosts vorgesehen, die als Speicherverzeichnis der Ausgabedatei verwendet werden. | Geschäftsanforderung         | Lokale Administratorrechte | Azure NetApp Files         |

## <a name="deploy-windows-virtual-machine-via-azure-portal"></a>Bereitstellen von virtuellen Windows-Computern über das Azure-Portal

In diesem Abschnitt erstellen wir zwei virtuelle Computer (VMs) mit Windows-Betriebssystemimage für die SAP BOBI-Plattform. Die allgemeinen Schritte zur Erstellung virtueller Computer sind folgende:

1. Erstellen einer [Ressourcengruppe](../../../azure-resource-manager/management/manage-resource-groups-portal.md#create-resource-groups)

2. Erstellen eines [virtuellen Netzwerks](../../../virtual-network/quick-create-portal.md#create-a-virtual-network)

   - Verwenden Sie bei der Bereitstellung der SAP BI-Plattform nicht ein einzelnes Subnetz für alle Azure-Dienste. Basierend auf der Architektur der SAP BI-Plattform müssen Sie ggf. mehrere Subnetze erstellen. Bei dieser Bereitstellung werden wir zwei Subnetze erstellen: BI Application-Subnetz und Application Gateway-Subnetz.
   - Befolgen Sie SAP-Hinweis [2276646](https://launchpad.support.sap.com/#/notes/2276646), um Ports für die Kommunikation zwischen SAP BusinessObjects BI-Platform und verschiedenen Komponenten zu identifizieren.
   - Azure SQL-Datenbank kommuniziert über Port 1433. Daher sollte ausgehender Datenverkehr über Port 1433 von Ihren SAP BOBI-Anwendungsservern zugelassen werden.
   - Bei Azure muss das Application Gateway auf einem separaten Subnetz bereitgestellt werden. Weitere Informationen finden Sie im Artikel [Azure Application Gateway](../../../application-gateway/configuration-overview.md).
   - Falls Sie Azure NetApp Files anstelle von Azure Files für den Dateispeicher verwenden, müssen Sie ein separates Subnetz für Azure NetApp Files erstellen. Weitere Informationen finden Sie im Artikel [Richtlinien für die Azure NetApp Files-Netzwerkplanung](../../../azure-netapp-files/azure-netapp-files-network-topologies.md).

3. Erstellen Sie eine Verfügbarkeitsgruppe.

   - Stellen Sie zur Erzielung von Redundanz für jede Ebene bei einer Bereitstellung mit mehreren Instanzen virtuelle Computer für jede Ebene in einer Verfügbarkeitsgruppe zusammen. Stellen Sie sicher, dass Sie die Verfügbarkeitsgruppen für jede Ebene auf der Grundlage Ihrer Architektur trennen.

4. Erstellen Sie den virtuellen Computer 1 **(azuswinboap1)** .

   - Sie können entweder ein benutzerdefiniertes Image verwenden oder ein Image aus Azure Marketplace auswählen. Weitere Informationen finden Sie entsprechend Ihren Anforderungen unter [Bereitstellen eines virtuellen Computers aus dem Azure Marketplace für SAP](deployment-guide.md) oder [Bereitstellen eines virtuellen Computers mit einem benutzerdefinierten Image für SAP](deployment-guide.md).

5. Erstellen Sie den virtuellen Computer 2 **(azuswinboap2)** .
6. Fügen Sie einen SSD Premium-Datenträger hinzu. Er wird als SAP BOBI-Installationsverzeichnis verwendet.

## <a name="provision-azure-premium-files"></a>Bereitstellen von Azure Premium-Dateien

Bevor Sie mit der Einrichtung von Azure Files beginnen, sollten Sie sich mit der [Azure Files-Dokumentation](../../../storage/files/storage-files-introduction.md) vertraut machen.

Azure Files verfügt über Standard-Dateifreigaben, die auf festplattenbasierter Hardware (HDD) gehostet werden, und über Premium-Dateifreigaben, die auf SSD-Datenträgerhardware (Solid State Drive) gehostet werden. Für den SAP BusinessObjects-Dateispeicher empfehlen wir die Verwendung von Azure Premium-Dateien.

Azure Premium-Dateifreigaben sind in einigen Regionen mit lokaler Redundanz und Zonenredundanz verfügbar. Um herauszufinden, ob Premium-Dateifreigaben derzeit in Ihrer Region verfügbar sind, lesen Sie die Seite [Verfügbare Produkte nach Region](https://azure.microsoft.com/global-infrastructure/services/?products=storage) für Azure. Informationen zu Regionen, die ZRS unterstützen, finden Sie unter [Azure Storage-Redundanz](../../../storage/common/storage-redundancy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

### <a name="deploy-azure-files-storage-account-and-nfs-shares"></a>Bereitstellen eines Azure Files-Speicherkontos und von NFS-Freigaben

Azure-Dateifreigaben werden in Speicherkonten bereitgestellt, bei denen es sich um Objekte der obersten Ebene handelt, die einen freigegebenen Speicherpool darstellen. Dieser Speicherpool kann verwendet werden, um mehrere Dateifreigaben bereitzustellen. Azure unterstützt mehrere Arten von Speicherkonten für verschiedene Speicherszenarien, die Kunden möglicherweise haben, aber für den SAP BusinessObjects-Dateispeicher müssen Sie ein **FileStorage-Speicherkonto** erstellen. Es ermöglicht Ihnen die Bereitstellung von Azure-Dateifreigaben auf SSD-basierter Hardware (Premium/Solid State Drive).

> [!NOTE]
> FileStorage-Konten können nur zum Speichern von Azure-Dateifreigaben verwendet werden. In einem FileStorage-Konto können keine anderen Speicherressourcen (Blobs, Container, Warteschlangen, Tabellen usw.) bereitgestellt werden.

Der Zugriff auf das Speicherkonto erfolgt über einen [privaten Endpunkt](../../../storage/files/storage-files-networking-endpoints.md), der im gleichen virtuellen Netzwerk der SAP BOBI-Plattform bereitgestellt wird. Bei diesem Setup verlässt der Datenverkehr ihres SAP-Systems niemals die Sicherheitsgrenzen des virtuellen Netzwerks. SAP-Systeme enthalten häufig sensible und unternehmenskritische Daten, und das Halten innerhalb der Grenzen des virtuellen Netzwerks ist für viele Kunden ein wichtiger Aspekt der Sicherheit.

Wenn Sie über ein anderes virtuelles Netzwerk auf das Speicherkonto zugreifen müssen, können Sie das [Azure VNET-Peering](../../../virtual-network/virtual-network-peering-overview.md) verwenden.

#### <a name="azure-files-storage-account"></a>Azure Files-Speicherkonto

1. Informationen zum Erstellen eines Speicherkontos über Azure-Portal finden Sie unter **Erstellen einer Ressource** > **Speichern** > **Speicherkonto**.

2. Füllen Sie auf der Registerkarte **Grundlagen** alle erforderlichen Felder aus, um ein Speicherkonto zu erstellen.

   a. Wählen Sie **Abonnement**, **Ressourcengruppe**, und **Region** aus.

   b. Geben Sie den **Namen des Speicherkontos** ein (z. B. **azusbobi**). Dieser Name muss global eindeutig sein, ansonsten gelten keine weiteren Einschränkungen.

   c. Wählen Sie **Premium** als Leistungsstufe und **FileStorage** als Kontoart aus.

   d. Wählen Sie für Replikationsbezeichnung die Redundanzstufe aus. Wählen Sie **Lokal redundanter Speicher (LRS)** aus.

   Für FileStorage Premium sind lokal redundanter Speicher (LRS) und zonenredundanter Speicher (ZRS) die einzigen verfügbaren Optionen. Wählen Sie daher basierend auf Ihrer Bereitstellungsstrategie (Verfügbarkeitsgruppe oder Verfügbarkeitszone) die entsprechende Redundanzstufe aus. Weitere Informationen finden Sie unter [Azure Storage-Redundanz](../../../storage/common/storage-redundancy.md).

   e. Klicken Sie auf Weiter.

3. Wählen Sie auf der Registerkarte **Netzwerk** die Option [Privater Endpunkt](../../../storage/files/storage-files-networking-endpoints.md) als Konnektivitätsmethode aus. Weitere Informationen finden Sie unter [Azure Files – Überlegungen zum Netzwerkbetrieb](../../../storage/files/storage-files-networking-overview.md).

   a. Wählen Sie im Abschnitt privater Endpunkt die Option **Hinzufügen** aus.

   b. Wählen Sie **Abonnement**, **Ressourcengruppe** und **Standort** aus.

   c. Geben Sie den **Namen** des privaten Endpunkts ein (z. B. azusbobi-pe).

   d. Wählen Sie die **Datei** in der **Speicherunterressource** aus.

   e. Wählen Sie im Abschnitt **Netzwerk** das **virtuelle Netzwerk** und das **Subnetz** aus, in dem die SAP BusinessObjects BI-Anwendung bereitgestellt wird.

   f. Übernehmen Sie die **Standardeinstellung (Ja)** für **In private DNS-Zone integrieren**.

   g. Wählen Sie in der Dropdownliste Ihre **private DNS-Zone** aus.

   h. Wählen Sie **OK** aus, um zur Registerkarte „Netzwerk“ unter „Speicherkonto erstellen“ zu wechseln.

4. Im Abschnitt „**Datenschutz**“ können Sie die Richtlinie für vorläufiges Löschen für Azure-Dateifreigaben in Ihrem Speicherkonto konfigurieren. Standardmäßig ist die Funktion für vorläufiges Löschen deaktiviert. Weitere Informationen zum vorläufigen Löschen finden Sie unter [Verhindern des versehentlichen Löschens von Azure-Dateifreigaben](../../../storage/files/storage-files-prevent-file-share-deletion.md).

5. Überprüfen Sie auf der Registerkarte **Erweitert** verschiedene Sicherheitsoptionen.

   **Sichere Übertragung erforderlich**: Dieses Feld gibt an, ob für die eingehende Kommunikation mit dem Speicherkonto eine Verschlüsselung für die Übertragung erforderlich ist. Wenn Sie Unterstützung für SMB 2.1 benötigen, müssen Sie diese Option deaktivieren. Behalten Sie für die SAP BusinessObjects BI-Plattform die **Standardeinstellung (aktiviert)** bei.

6. Fahren Sie fort und erstellen Sie das Speicherkonto.

Weitere Informationen zum Erstellen eines Speicherkontos finden Sie unter [Erstellen eines FileStorage-Speicherkontos](../../../storage/files/storage-how-to-create-file-share.md).

#### <a name="create-azure-file-shares"></a>Erstellen von Azure-Dateifreigaben

Im nächsten Schritt erstellen Sie Azure Files im Speicherkonto. Azure Files verwendet für Premium-Dateifreigaben das Modell „Bereitgestellt“. Beim diesem Geschäftsmodell geben Sie dem Azure Files-Dienst aktiv an, wie hoch Ihr Speicherbedarf ist, anstatt nach Inanspruchnahme abgerechnet zu werden. Weitere Informationen zu diesem Modell finden Sie unter [Bereitgestelltes Modell](../../../storage/files/understanding-billing.md#provisioned-model). In diesem Beispiel erstellen wir zwei Azure Files: frsinput (256 GB) und frsoutput (256 GB) für den SAP BOBI-Dateispeicher.

Navigieren Sie zum Speicherkonto **azusbobi** > **Dateifreigaben**

1. Wählen Sie **Neue Dateifreigabe** aus.
2. Geben Sie den **Namen** der Dateifreigabe ein (z. B. frsinput, frsouput).
3. Fügen Sie die erforderliche Dateifreigabegröße in **bereitgestellte Kapazität** ein (z. B. 256 GB).
4. Wählen Sie **SMB** als **Protokoll** aus.
5. Klicken Sie auf **Erstellen**.

## <a name="configure-data-disk-on-windows-virtual-machine"></a>Konfigurieren des Datenträgers auf einem virtuellen Windows-Computer

Für die Schritte in diesem Abschnitt werden die folgenden Präfixe verwendet:

**[A]** : Der Schritt gilt für alle Hosts.

### <a name="initialize-a-new-data-disk"></a>Initialisieren eines neuen Datenträgers

Die SAP BusinessObjects BI-Anwendung erfordert eine Partition, auf der ihre Binärdateien installiert werden können. Sie können die SAP BOBI-Anwendung auf der Betriebssystempartition installieren (C: ), aber stellen Sie sicher, dass genügend Speicherplatz für die Bereitstellung und das Betriebssystem vorhanden ist. Es wird empfohlen, mindestens 2 GB für temporäre Dateien und Webanwendungen verfügbar zu haben. Bei all diesen Überlegungen empfiehlt es sich, SAP BOBI-Installationsbinärdateien in separaten Partitionen zu trennen.

In diesem Beispiel wird die SAP BOBI-Anwendung auf einer separaten Partition installiert (F: ). Initialisieren Sie den SSD Premium-Datenträger, den Sie während der Bereitstellung virtueller Computer verbunden haben.

1. **[A]** Falls kein Datenträger mit dem virtuellen Computer verbunden ist (azuswinboap1 und azuswinboap2), führen Sie die Schritte unter [Hinzufügen eines Datenträgers](../../windows/attach-managed-disk-portal.md#add-a-data-disk) zum Verbinden eines neuen verwalteten Datenträgers aus.
2. **[A]** Nachdem der verwaltete Datenträger mit dem virtuellen Computer verbunden wurde, initialisieren Sie den Datenträger, indem Sie die Schritte unter [Initialisieren eines neuen Datenträgers](../../windows/attach-managed-disk-portal.md#initialize-a-new-data-disk) ausführen.

### <a name="mount-azure-premium-files"></a>Einbinden von Azure Files Premium

Um Azure-Dateien als Dateispeicher zu verwenden, müssen Sie sie einbinden. Dies bedeutet, dass Sie ihr einen Laufwerksbuchstaben oder einen Einbundungspunktpfad zuweisen.

1. **[A]** Führen Sie zum Einbinden der Azure-Dateifreigabe die Schritte aus, die im Dokument [Einbinden der Azure-Dateifreigabe](../../../storage/files/storage-how-to-use-files-windows.md#mount-the-azure-file-share) beschrieben sind.

Stellen Sie sicher, dass Port 445 geöffnet ist, um die Azure-Dateifreigabe auf einem Windows-Server einbinden zu können. Das SMB-Protokoll erfordert dass TCP-Port 445 geöffnet ist. Wenn Port 445 gesperrt ist, sind keine Verbindungen möglich. Sie können überprüfen, ob die Firewall Port 445 blockiert, indem Sie das `Test-NetConnection` Cmdlet verwenden, das im Leitfaden zur [Problembehandlung](../../../storage/files/storage-troubleshoot-windows-file-connection-problems.md#cause-1-port-445-is-blocked) erwähnt wird.

## <a name="configure-cms-database---azure-sql"></a>Konfigurieren der CMS-Datenbank: Azure SQL

Dieser Abschnitt enthält ausführliche Informationen zur Bereitstellung von Azure SQL mithilfe des Azure-Portals. Es enthält auch Anweisungen zum Erstellen von CMS- und Überwachungsdatenbanken für die SAP BOBI-Plattform und ein Benutzerkonto für den Zugriff auf die Datenbank.

Die Richtlinien gelten nur, wenn Sie Azure SQL (DBaaS-Angebot in Azure) verwenden. Informationen zu anderen Datenbanken finden Sie in der SAP- oder datenbankspezifischen Dokumentation.

### <a name="create-sql-database-server"></a>Neuen SQL-Datenbank-Server erstellen

Azure SQL-Datenbank bietet verschiedene Bereitstellungsoptionen: Einzeldatenbank, Pool für elastische Datenbanken und Datenbankserver. Für SAP BOBI benötigen wir zwei Datenbanken (CMS und Audit). Anstatt also zwei Einzeldatenbanken zu erstellen, können Sie einen SQL-Datenbank-Server erstellen, der die Gruppe von Einzeldatenbanken und Pools für elastische Datenbanken verwalten kann.  Im Folgenden finden Sie die Schritte zum Erstellen eines SQL-Datenbankservers:

1. Navigieren Sie zur Seite [SQL-Bereitstellungsoption auswählen](https://portal.azure.com/#create/Microsoft.AzureSQL).
2. Ändern Sie unter **SQL-Datenbanken** den **Ressourcentyp** in **Datenbankserver** und wählen Sie **Erstellen** aus.
3. Füllen Sie auf der Registerkarte **Grundlagen** alle erforderlichen Felder aus, um einen **SQL-Datenbank-Server** zu erstellen.

   a. Wählen Sie unter **Projektdetails** das **Abonnement** und die  **Ressourcengruppe** aus.

   b. Geben Sie **Servername** ein (z. B. azussqlbodb). Dieser Name muss global eindeutig sein, ansonsten gelten keine weiteren Einschränkungen.

   c. Wählen Sie den **Speicherort** aus.

   d. Geben Sie **Serveradministratoranmeldung** (z. B. boadmin) und **Kennwort** ein.

4. Ändern Sie auf der Registerkarte **Netzwerk** unter **Firewallregeln** den **Zugriff von Azure-Diensten und -Ressourcen auf diesen Server erlauben** in **Nein**.
5. Behalten Sie unter **Zusätzliche Einstellungen** die Standardeinstellungen bei.
6. Fahren Sie fort und erstellen Sie den **SQL-Datenbank-Server**.

Erstellen Sie im nächsten Schritt auf dem SQL-Datenbank-Server (azussqlbodb.database.windows.net) das CMS und die Überwachungsdatenbanken.

### <a name="create-the-cms-and-the-audit-database"></a>Erstellen des CMS und der Überwachungsdatenbanken

Navigieren Sie nach der Bereitstellung des SQL-Datenbankservers zur Ressource **azussqlbodb** und führen Sie dann die folgenden Schritte aus, um das CMS und die Überwachungsdatenbanken zu erstellen.

1. Wählen Sie auf der Seite **Übersicht** für azussqlbodb die Option **Datenbank erstellen** aus.
2. Füllen Sie auf der Registerkarte **Grundlagen** alle erforderlichen Felder aus:

   a. Geben Sie den **Datenbanknamen** ein (z. B. bocms oder boaudit).

   b. Wählen Sie unter **Berechnen + Speichern** die Option **Datenbank konfigurieren** aus und wählen Sie das entsprechende Modell basierend auf Ihrem Größenergebnis aus. Unter [Größenmodelle für Azure SQL-Datenbank](businessobjects-deployment-guide.md#sizing-models-for-azure-sql-database) finden Sie Informationen zu den Optionen.
3. Wählen Sie auf der Registerkarte **Netzwerk** die Option [Privater Endpunkt](../../../private-link/tutorial-private-endpoint-sql-portal.md) als Konnektivitätsmethode aus. Der private Endpunkt wird verwendet, um auf die Azure SQL-Datenbank innerhalb des konfigurierten virtuellen Netzwerks zuzugreifen.

   a. Wählen Sie **Privaten Endpunkt hinzufügen**.

   b. Wählen Sie **Abonnement**, **Ressourcengruppe** und **Standort** aus.

   c. Geben Sie den **Namen** des privaten Endpunkts ein (z. B. azusbodb-pe).

   e. Wählen Sie unter **Zielunterressource** die Option **SqlServer** aus.

   f. Wählen Sie im Abschnitt **Netzwerk** das **virtuelle Netzwerk** und das **Subnetz** aus, in dem die SAP BusinessObjects BI-Anwendung bereitgestellt wird.

   g. Übernehmen Sie die **Standardeinstellung (Ja)** für **In private DNS-Zone integrieren**.

   h. Wählen Sie in der Dropdownliste Ihre **private DNS-Zone** aus.

   i. Wählen Sie **OK** aus, um zur Registerkarte „Netzwerk“ unter „SQL-Datenbank erstellen“ zu wechseln.
4. Ändern Sie auf der Registerkarte **Zusätzliche Einstellungen** die **Sortierung** in **SQL_Latin1_General_CP850_BIN2**.
5. Fahren Sie fort und erstellen Sie die CMS-Datenbank.

Auf ähnliche Weise können Sie eine **Überwachungsdatenbank** erstellen (z. B. boaudit).

### <a name="download-and-install-odbc-driver"></a>Laden Sie den ODBC-Treiber herunter und installieren Sie ihn:

Damit der SAP BOBI-Anwendungsserver auf das CMS oder die Überwachungsdatenbank zugreifen kann, benötigt er einen Datenbankclient bzw. Datenbanktreiber. Der Microsoft ODBC-Treiber wird verwendet, um auf CMS- und Audit-Datenbanken zuzugreifen, die auf der Azure SQL Datenbank ausgeführt werden. Dieser Abschnitt enthält Anweisungen zum Herunterladen und Einrichten des ODBC-Treibers unter Windows. 

1. Informationen zu den Datenbankconnectoren, die mit Azure SQL Datenbank kompatibel sind, finden Sie im Abschnitt **CMS + Audit-Repositorysupport nach Betriebssystem** in der [Produktverfügbarkeitsmatrix (PAM) für SAP BusinessObjects BI-Platform.](https://support.sap.com/pam)
2. Laden Sie die ODBC-Treiberversion über den [Link](/sql/connect/odbc/windows/release-notes-odbc-sql-server-windows?preserve-view=true&view=sql-server-ver15) herunter. In diesem Beispiel laden wir den ODBC-Treiber [13.1](/sql/connect/odbc/windows/release-notes-odbc-sql-server-windows?preserve-view=true&view=sql-server-ver15#131) herunter.
3. Installieren Sie den ODBC-Treiber auf allen BI-Servern (azuswinboap1 und azuswinboap2).
4. Navigieren Sie nach Installieren des Treibers in **azuswinboap1** zu **Starten** für  > **Windows Administrative Tools** > **ODBC-Datenquellen (64-Bit)** .  
5. Wechseln Sie zur Registerkarte **System-DSN**. 
6. Wählen Sie **Hinzufügen** aus, um eine Verbindung mit der CMS-Datenbank herzustellen.
7. Wählen Sie **ODBC Driver 13 for SQL Server** und dann **Fertig stellen** aus.
8. Geben Sie die Informationen Ihrer CMS-Datenbank wie unten angegeben ein und wählen Sie **Weiter** aus.
   - **Name:** *Name der Datenbank, die im Abschnitt „Erstellen von CMS und Überwachungsdatenbank“ erstellt wurde* (z. B. bocms oder boaudit)
   - **Beschreibung:** *Beschreibung der Datenquelle* (z. B. CMS-Datenbank oder Überwachungsdatenbank)
   - **Server:** *Name des SQL-Servers, der im Abschnitt „Erstellen eines SQL-Datenbankservers“ erstellt wurde* (z. B. azussqlbodb.database.windows.net)
9. Wählen Sie „**Mit SQL Server-Authentifizierung unter Verwendung einer vom Benutzer eingegebenen Anmelde-ID und eines Kennworts“** aus, um die Authentizität bei SQL-Server zu überprüfen. Geben Sie die Anmeldeinformationen des Benutzers ein, die zum Zeitpunkt der Erstellung des Azure SQL Datenbankservers erstellt wurden (z. B. boadmin), und wählen Sie **Weiter** aus.
10. **Ändern Sie die Standarddatenbank** in **bocms** und behalten Sie für alles andere die Standardeinstellung bei. Wählen Sie **Weiter** aus.
11. Aktivieren Sie **Starke Verschlüsselung für Daten verwenden** und behalten Sie für alles andere die Standardeinstellung bei. Wählen Sie **Fertig stellen** aus.
12. Die Datenquelle zur CMS-Datenbank wurde erstellt. Sie können jetzt **Datenquelle testen** auswählen, um die Verbindung mit der CMS-Datenbank aus der BI-Anwendung zu überprüfen. Dies sollte erfolgreich abgeschlossen werden. Wenn ein Fehler auftritt, [beheben Sie](../../../azure-sql/database/troubleshoot-common-errors-issues.md) das Konnektivitätsproblem.

>[!Note]
>Azure SQL-Datenbank kommuniziert über Port 1433. Daher sollte ausgehender Datenverkehr über Port 1433 von Ihren SAP BOBI-Anwendungsservern zugelassen werden.

Wiederholen Sie die oben genannten Schritte, um eine Verbindung für die Überwachungsdatenbank auf dem Server azuswinboap1 herzustellen. Installieren und konfigurieren Sie auf ähnliche Weise beide ODBC-Datenquellen (bocms und boaudit) auf allen BI-Anwendungsservern (azuswinboap2). 

## <a name="server-preparation"></a>Servervorbereitung

Befolgen Sie die neuesten Anleitungen von SAP, um Server für die Installation der BI-Plattform vorzubereiten. Aktuelle Informationen finden Sie im Abschnitt „Vorbereitung“ des [Business Intelligence-Plattforminstallationshandbuchs für Windows](https://help.sap.com/viewer/df8899896b364f6c880112f52e4d06c8/4.3.1/en-US/46b0d1a26e041014910aba7db0e91070.html).

## <a name="installation"></a>Installation

Um die BI-Plattform auf einem Windows-Host zu installieren, melden Sie sich mit einem Benutzer an, der über lokale Administratorrechte verfügt.

Navigieren Sie zu den Medien der SAP BusinessObjects BI-Plattform und führen Sie `setup.exe` aus.

Befolgen Sie die [Installationsanleitung für die SAP Business Intelligence-Plattform für Windows](https://help.sap.com/viewer/df8899896b364f6c880112f52e4d06c8/4.3.1/en-US/46ae62456e041014910aba7db0e91070.html), die spezifisch für Ihre Version ist. Beim Installieren der SAP BOBI-Plattform sind einige Punkte zu beachten. 

- Geben Sie auf dem Bildschirm **Zielordner konfigurieren** den Zielordner an, in dem Sie die BI-Plattform installieren möchten. (Beispiel: F:\SAP BusinessObjects \). 

- Auf dem Bildschirm zum **Produktregistrierung konfigurieren** können Sie entweder einen temporären Lizenzschlüssel für SAP BusinessObjects Solutions aus dem SAP-Hinweis [1288121](https://launchpad.support.sap.com/#/notes/1288121) verwenden oder einen Lizenzschlüssel im SAP Service Marketplace generieren.

- Wählen Sie auf dem Bildschirm zum **Installationstyp auswählen** die Option für eine **vollständige** Installation auf dem ersten Server (azuswinboap1) aus. Für den anderen Server (azuswinboap2) wählen Sie die Option zum **Anpassen/Erweitern** aus, wodurch das bestehende BOBI-Setup erweitert wird.

- Wählen Sie auf dem Bildschirm zum **Auswählen der Standard- oder einer vorhandenen Datenbank** die Option zum **Konfigurieren einer vorhandenen Datenbank** aus, wodurch Sie aufgefordert werden, die CMS- und Überwachungsdatenbank auszuwählen. Wählen Sie **Microsoft SQL Server für ODBC** als CMS-Datenbanktyp und Überwachungsdatenbanktyp aus.

  Sie können auch die Option für „Keine Überwachungsdatenbank“ auswählen, wenn Sie die Überwachung während der Installation nicht konfigurieren möchten.

- Wählen Sie geeignete Optionen auf dem Bildschirm zum **Auswählen des Java-Webanwendungsservers** basierend auf Ihrer SAP BOBI-Architektur aus. In diesem Beispiel haben wir Option 1 ausgewählt, bei der der Tomcat-Server auf derselben SAP BOBI-Plattform installiert wird.

- Geben Sie CMS-Datenbankinformationen in **CMS-Repository-Datenbank konfigurieren: SQL-Server (ODBC)** ein. Beispieleingabe für CMS-Datenbankinformationen für eine Windows-Installation.
  
  ![CMS-Datenbankinformationen für Windows](media/businessobjects-deployment-guide/businessobjects-deployment-windows-sql-cms.png)

- (Optional) Geben Sie Überwachungsdatenbankinformationen in **Überwachungs-Repository-Datenbank: SQL Server (ODBC)** ein. Beispieleingabe für Überwachungsdatenbankinformationen für eine Windows-Installation.
  
  ![Überwachungsdatenbankinformationen für Windows](media/businessobjects-deployment-guide/businessobjects-deployment-windows-sql-audit.png)

- Befolgen Sie die Anweisungen, und geben Sie die erforderlichen Eingaben ein, um die Installation abzuschließen.

Bei Bereitstellung mehrerer Instanzen führen Sie das Installationssetup auf einem zweiten Host (azuswinboap2) aus. Wählen Sie auf dem Bildschirm zum **Auswählen des Installationstyps** die Option zum **Anpassen/Erweitern** aus, wodurch das bestehende BOBI-Setup erweitert wird. Sehen Sie sich den SAP-Blog zum [SAP BusinessObjects Business Intelligence-Plattformsetup mit Azure SQL DB](https://blogs.sap.com/2020/06/19/sap-on-azure-sap-businessobjects-business-intelligence-platform-setup-with-azure-sql-db-managed-paas-database/) an.

> [!IMPORTANT]
> Die Versionsnummern der Datenbank-Engine für SQL Server und Azure SQL-Datenbank sind nicht miteinander vergleichbar, sondern sind interne Buildnummern für diese separaten Produkte. Die Datenbank-Engine für Azure SQL-Datenbank basiert auf der gleichen Codebasis wie die SQL Server-Datenbank-Engine. Entscheidend ist dabei, dass die Datenbank-Engine in Azure SQL-Datenbank immer über die neuesten SQL-Datenbank-Engine-Bits verfügt. Die Version 12 von Azure SQL-Datenbank ist neuer als die Version 15 von SQL Server.

Um die aktuelle Azure SQL Datenbankversion zu ermitteln, können Sie entweder die Einstellungen der zentralen Verwaltungskonsole (Central Management Console, CMC) checken oder die folgende Abfrage mit [sqlcmd](/sql/tools/sqlcmd-utility?preserve-view=true&view=sql-server-ver15) oder [SQL Server Management Studio (SSMS)](/sql/ssms/sql-server-management-studio-ssms?preserve-view=true&view=sql-server-ver15) ausführen. Die Ausrichtung von SQL-Versionen an der Standardkompatibilität finden Sie im Artikel [Datenbank-Kompatibilitätsgrad.](/sql/t-sql/statements/alter-database-transact-sql-compatibility-level?preserve-view=true&view=sql-server-ver15)

![Datenbankinformationen in CMC](media/businessobjects-deployment-guide/businessobjects-deployment-windows-sql-cmc.PNG)

```sql
1> select @@version as version;
2> go
version
------------------------------------------------------------------------------------------
Microsoft SQL Azure (RTM) - 12.0.2000.8
        Feb 20 2021 17:51:58
        Copyright (C) 2019 Microsoft Corporation

(1 rows affected)

1> select name, compatibility_level from sys.databases;
2> go
name                                                                  compatibility_level
--------------------------------------------------------------------- -------------------
master                                                                                150
bocms                                                                                 150
boaudit                                                                               150

(3 rows affected)
```

## <a name="post-installation"></a>Nach der Installation

Nach Installatieren der SAP BOBI-Platform mit mehreren Instanzen müssen zusätzliche Schritte nach der Konfiguration ausgeführt werden, um die Hochverfügbarkeit von Anwendungen zu unterstützen.

### <a name="configuring-cluster-name"></a>Konfigurieren des Clusternamens

Bei der Bereitstellung der SAP BOBI-Platform mit mehreren Instanzen sollten Sie mehrere CMS-Server gemeinsam in einem Cluster ausführen. Ein Cluster besteht aus zwei oder mehr CMS-Servern, die mit einer gemeinsamen CMS-Systemdatenbank zusammenarbeiten. Wenn ein Knoten ausfällt, der auf CMS ausgeführt wird, bedient ein Knoten mit einem anderen CMS weiterhin BI-Plattformanforderungen. Standardmäßig gibt ein Clustername auf der SAP BOBI-Plattform den Hostnamen des ersten CMS an, das Sie installieren. 

Um den Clusternamen unter Windows zu konfigurieren, befolgen Sie die Anweisungen im [SAP Business Intelligence Platform Administrator Guide (Administratorhandbuch für die SAP Business Intelligence-Plattform)](https://help.sap.com/viewer/2e167338c1b24da9b2a94e68efd79c42/4.3.1/en-US). Befolgen Sie nach dem Konfigurieren des Clusternamens den SAP-Hinweis [1660440](https://launchpad.support.sap.com/#/notes/1660440), um den Standardsystemeintrag auf der CMC- oder BI Launchpad-Anmeldeseite festzulegen. 

### <a name="configure-input-and-output-filestore-location-to-azure-premium-files"></a>Konfigurieren des Speicherorts des Eingabe- und Ausgabedateispeichers in Azure Premium Files

Filestore bezieht sich auf die Datenträgerverzeichnisse, in denen sich die eigentlichen SAP BusinessObjects-Dateien befinden. Der Standardspeicherort des Dateirepositoryservers für die SAP BOBI-Plattform befindet sich im lokalen Installationsverzeichnis. Bei der Bereitstellung mit mehreren Instanzen ist es wichtig, den Dateispeicher in einem freigegebenen Speicher wie Azure Premium Files oder Azure NetApp Files einzurichten, damit er von allen Servern der Speicherebene zugegriffen werden kann.

1. Wenn sie nicht erstellt wird, befolgen Sie die Anweisungen im obigen Abschnitt **Bereitstellen von Azure Premium Files** zum Erstellen und Bereitstellen von Azure Premium Files.

   > [!Tip]
   > Wählen Sie die Speicherredundanz für Azure Premium Files (LRS oder ZRS) basierend auf der Bereitstellung Ihrer virtuellen Computer (Verfügbarkeitsgruppe oder Verfügbarkeitszone) aus.

2. Befolgen Sie den SAP-Hinweis [2512660](https://launchpad.support.sap.com/#/notes/0002512660), um den Pfad des Dateirepositorys (Eingabe und Ausgabe) zu ändern.

### <a name="tomcat-clustering---session-replication"></a>Tomcat-Clustering: Sitzungsreplikation

Tomcat unterstützt das Clustering von mindestens zwei Anwendungsservern für Sitzungsreplikation und Failover. SAP BOBI-Plattformsitzungen sind serialisiert, eine Benutzersitzung kann reibungslos ein Failover auf eine andere Instanz von Tomcat ausführen, selbst wenn ein Anwendungsserver ausfällt. Wenn ein Benutzer z. B. mit einem Webserver verbunden ist, der ausfällt, während der Benutzer in einer Ordnerhierarchie in einer SAP BI-Anwendung navigiert. Mit einer ordnungsgemäß konfigurierten Clusterkonfiguration kann der Benutzer weiterhin in der Ordnerhierarchie navigieren, ohne zur Anmeldeseite umgeleitet zu werden.

Im SAP-Hinweis [2808640](https://launchpad.support.sap.com/#/notes/2808640) werden Schritte zur Konfiguration des Tomcat-Clusterings mithilfe von Multicast bereitgestellt. In Azure wird Multicast jedoch nicht unterstützt. Damit der Tomcat-Cluster in Azure funktioniert, müssen Sie [StaticMembershipInterceptor](https://tomcat.apache.org/tomcat-8.0-doc/config/cluster-interceptor.html#Static_Membership) verwenden (SAP-Hinweis [2764907](https://launchpad.support.sap.com/#/notes/2764907)). Lesen Sie [Tomcat-Clustering mithilfe der statischen Mitgliedschaft für die SAP BusinessObjects BI-Plattform](https://blogs.sap.com/2020/09/04/sap-on-azure-tomcat-clustering-using-static-membership-for-sap-businessobjects-bi-platform/) im SAP-Blog, um einen Tomcat-Cluster in Azure einzurichten.

### <a name="load-balancing-web-tier-of-sap-bi-platform"></a>Lastenausgleich für die Webebene der SAP BI-Plattform

Bei der Bereitstellung von SAP BOBI mit mehreren Instanzen werden Java-Webanwendungsserver (Webebene) auf zwei oder mehr Hosts ausgeführt. Um die Benutzerauslastung gleichmäßig auf die Webserver zu verteilen, können Sie einen Lastenausgleich zwischen Endbenutzern und Webservern verwenden. In Azure können Sie entweder Azure Load Balancer oder Azure Application Gateway verwenden, um den Datenverkehr zu Ihren Webanwendungsservern zu verwalten. Einzelheiten zum jeweiligen Angebot werden im folgenden Abschnitt erläutert.

1. [Azure Load Balancer](../../../load-balancer/load-balancer-overview.md) ist ein Lastenausgleich mit hoher Leistung und Layer 4-Latenz (TCP, UDP), mit dem der Datenverkehr auf fehlerfreie virtuelle Computer verteilt wird. Der Integritätstest eines Lastenausgleichs überwacht einen bestimmten Port auf jedem virtuellen Computer und verteilt Datenverkehr nur an einen betriebsbereiten virtuellen Computer. Sie können entweder einen öffentlichen Lastenausgleich oder einen internen Lastenausgleich wählen, je nachdem, ob die SAP BI-Plattform über das Internet zugänglich sein soll. Es ist zonenredundant und gewährleistet Hochverfügbarkeit über mehrere Verfügbarkeitszonen hinweg.

   Weitere Informationen finden Sie im Abschnitt zum internen Lastenausgleich in der nachfolgenden Abbildung, in welcher der Webanwendungsserver an Port 8080 ausgeführt wird (Standard-HTTP-Port von Tomcat), der vom Integritätstest überwacht wird. Somit wird jede von Endbenutzern eingehende Anforderung an die Webanwendungsserver (azuswinboap1 oder azuswinboap2) im Backend-Pool umgeleitet. Der Lastenausgleich unterstützt keine TLS/SSL-Terminierung (auch bekannt als TLS/SSL-Abladung). Wenn Sie Azure Load Balancer verwenden, um Datenverkehr auf Webserver zu verteilen, empfehlen wir die Verwendung von Load Balancer Standard.

   > [!NOTE]
   > Wenn virtuelle Computer ohne öffentliche IP-Adressen im Back-End-Pool einer internen Azure Load Balancer Standard-Instanz (ohne öffentliche IP-Adresse) platziert werden, liegt keine ausgehende Internetverbindung vor, sofern nicht in einer zusätzlichen Konfiguration das Routing an öffentliche Endpunkte zugelassen wird. Ausführliche Informationen zum Erreichen ausgehender Konnektivität finden Sie unter [Public endpoint connectivity for Virtual Machines using Azure Standard Load Balancer in SAP high-availability scenarios](high-availability-guide-standard-load-balancer-outbound-connections.md) (Konnektivität mit öffentlichen Endpunkten für virtuelle Computer mithilfe von Azure Load Balancer Standard in SAP-Szenarien mit Hochverfügbarkeit).

   ![Azure Load Balancer zum Ausgleichen des Datenverkehrs über Webserver](media/businessobjects-deployment-guide/businessobjects-deployment-windows-load-balancer.png)

2. [Azure Application Gateway](../../../application-gateway/overview.md) stellt Application Delivery Controller (ADC) als Dienst zur Verfügung, mit dessen Hilfe Anwendungen den Benutzerdatenverkehr zu einem oder mehreren Webanwendungsservern leiten können. Es bietet für Ihre Anwendungen verschiedene Layer 7-Lastenausgleichsfunktionen wie TLS/SSL-Abladung, Web Application Firewall (WAF), cookiebasierte Sitzungsaffinität und andere.

   Auf der SAP BI-Plattform leitet das Anwendungsgateway den Webdatenverkehr von Anwendungen an die angegebenen Ressourcen in einem Backend-Pool (azuswinboap1 oder azuswinboap2) weiter. Sie weisen dem Port einen Listener zu, erstellen Regeln und fügen Ressourcen zu einem Back-End-Pool hinzu. In der folgenden Abbildung fungiert ein Anwendungsgateway mit privater Frontend-IP-Adresse (10.31.3.25) als Einstiegspunkt für die Benutzer, behandelt eingehende TLS/SSL-Verbindungen (HTTPS – TCP/443), entschlüsselt TLS/SSL und leitet die unverschlüsselte Anforderung (HTTP – TCP/8080) an die Server im Backend-Pool weiter. Mit dem integrierten TLS/SSL-Terminierungsfeature brauchen wir nur ein TLS/SSL-Zertifikat am Anwendungsgateway zu erstellen, was den Betrieb vereinfacht.

   ![Application Gateway zum Ausgleichen des Datenverkehrs über Webserver](media/businessobjects-deployment-guide/businessobjects-deployment-windows-application-gateway.png)

   Informationen zum Konfigurieren von Application Gateway für SAP BOBI-Webserver finden Sie unter [Lastenausgleich für SAP BOBI-Webserver mithilfe von Azure Application Gateway](https://blogs.sap.com/2020/09/17/sap-on-azure-load-balancing-web-application-servers-for-sap-bobi-using-azure-application-gateway/) im SAP-Blog.

   > [!NOTE]
   > Es wird empfohlen, Azure Application Gateway für den Lastenausgleich des Datenverkehrs zum Webserver zu verwenden, sodass es Features wie SSL-Abladung, Zentralisieren der SSL-Verwaltung zur Verringerung des Aufwands für die Ver- und Entschlüsselung auf dem Server, Roundrobin-Algorithmus zur Verteilung des Datenverkehrs, WAF-Funktionen (Web Application Firewall), Hochverfügbarkeit usw. bietet.

## <a name="sap-businessobjects-bi-platform-reliability-on-azure"></a>Zuverlässigkeit der SAP BusinessObjects BI-Plattform in Azure

Die SAP BusinessObjects BI-Plattform umfasst unterschiedliche Ebenen, die für bestimmte Aufgaben und Vorgänge optimiert sind. Wenn eine Komponente einer Ebene nicht mehr verfügbar ist, kann auf die SAP BOBI-Anwendung entweder nicht mehr zugegriffen werden oder bestimmte Funktionen der Anwendung funktionieren nicht mehr. Es muss daher sichergestellt werden, dass jede Ebene so ausgelegt ist, dass sie zuverlässig ist, um die Anwendung ohne Unterbrechung beim Geschäftsbetrieb betriebsbereit zu halten.

In diesem Leitfaden wird untersucht, wie native Funktionen in Azure kombiniert mit der SAP BOBI-Plattformkonfiguration die Verfügbarkeit der SAP-Bereitstellung verbessert. In diesem Abschnitt werden die folgenden Optionen für die SAP BOBI-Plattformzuverlässigkeit bei Azure behandelt:

- „**Sichern und Wiederherstellen**“ ist ein Prozess, bei dem regelmäßig Kopien von Daten und Anwendungen an einem separaten Speicherort erstellt werden. Somit kann bei Verlust oder Beschädigung der ursprünglichen Daten oder Anwendungen der vorherige Zustand wiederhergestellt werden.

- **Hochverfügbarkeit:** Eine Plattform mit Hochverfügbarkeit verfügt innerhalb der Azure-Region von jeder Komponente über mindestens zwei Kopien, damit die Anwendung betriebsbereit bleibt, wenn einer der Server nicht mehr verfügbar ist.
- **Notfallwiederherstellung:** Es ist ein Prozess zur Wiederherstellung der Azure-Funktionalität Ihrer Anwendung, wenn es in einer Notfallsituation zu einem Ausfall kommt, z. B. wenn die gesamte Azure-Region aufgrund einer Naturkatastrophe nicht mehr verfügbar ist.

Die Implementierung dieser Lösung variiert je nach Art des Systemsetups in Azure. Daher müssen Kunden ihre Lösung für Sichern und Wiederherstellen, Hochverfügbarkeit und Notfallwiederherstellung an ihre Geschäftsanforderungen anpassen.

## <a name="back-up-and-restore"></a>Sichern und Wiederherstellen

„Sicherung und Wiederherstellung“ ist ein Prozess, bei dem regelmäßig Kopien von Daten und Anwendungen an einem separaten Speicherort erstellt werden. Somit kann bei Verlust oder Beschädigung der ursprünglichen Daten oder Anwendungen der vorherige Zustand wiederhergestellt werden. Es ist auch ein wesentlicher Bestandteil jeder Strategie zur Notfallwiederherstellung in Unternehmen. Diese Sicherungen ermöglichen die Wiederherstellung der Anwendung und Datenbank bis zu einem bestimmten Zeitpunkt innerhalb der konfigurierten Beibehaltungsdauer.

Identifizieren Sie zur Entwicklung einer umfassenden Sicherungs- und Wiederherstellungsstrategie für die SAP BOBI-Plattform die Komponenten, die zu Systemausfällen oder Störungen der Anwendung führen. Auf der SAP BOBI-Plattform ist die Sicherung der folgenden Komponenten für den Schutz der Anwendung unerlässlich.

- SAP BOBI-Installationsverzeichnis (verwaltete Premium-Datenträger)
- Filestore (Azure Premium Files oder Azure NetApp Files für verteilte Installation)
- CMS- und Überwachungsdatenbank (Azure SQL-Datenbank, Azure Database for MySQL oder Datenbank auf einem virtuellen Azure-Computer)

Im folgenden Abschnitt wird beschrieben, wie die Sicherungs- und Wiederherstellungsstrategie für die einzelnen Komponenten auf der SAP BOBI-Plattform implementiert wird.

### <a name="backup--restore-for-sap-bobi-installation-directory"></a>Sicherung und Wiederherstellung für das SAP BOBI-Installationsverzeichnis

In Azure ist die einfachste Möglichkeit zur Sicherung von virtuellen Maschinen und allen angeschlossenen Datenträgern die Verwendung des [Azure Backup](../../../backup/backup-azure-vms-introduction.md)-Diensts. Er bietet unabhängige und isolierte Sicherungen zum Schutz vor dem versehentlichen Löschen der Daten auf Ihren VMs. Sicherungen werden in einem Recovery Services-Tresor mit integrierter Verwaltung von Wiederherstellungspunkten gespeichert. Konfiguration und Skalierung sind unkompliziert. Die Sicherungen sind optimiert und können bei Bedarf problemlos wiederhergestellt werden.

Im Rahmen des Sicherungsvorgangs wird eine Momentaufnahme erstellt und die Daten werden ohne Auswirkung auf Produktionsworkloads in den Recovery Services-Tresor übertragen. Die Momentaufnahme bietet einen anderen Grad an Konsistenz, wie im Artikel [Konsistenz von Momentaufnahmen](../../../backup/backup-azure-vms-introduction.md#snapshot-consistency) beschrieben. Azure Backup bietet parallele Unterstützung für die Sicherung verwalteter Datenträger mithilfe von [Azure Disk Backup](../../../backup/disk-backup-overview.md) zusätzlich zu [Azure VM Backup](../../../backup/backup-azure-vms-introduction.md). Dies ist hilfreich, wenn Sie tägliche anwendungskonsistente Sicherungen von VMs und häufigere absturzkonsistente Sicherungen des Betriebssystemdatenträgers oder eines bestimmten anderen Datenträgers durchführen möchten. Weitere Informationen finden Sie im Dokument [Azure VM Backup](../../../backup/backup-azure-vms-introduction.md), [Azure Disk Backup](../../../backup/disk-backup-overview.md) und unter [Häufig gestellte Fragen: Sichern von virtuellen Azure-Computern](../../../backup/backup-azure-vm-backup-faq.yml).

### <a name="backup--restore-for-filestore"></a>Sichern und Wiederherstellen für Dateispeicher

Basierend auf Ihrer Bereitstellung erfolgt der Dateispeicher der SAP BOBI-Plattform auf Azure NetApp Files oder Azure Files. Wählen Sie basierend auf dem Speicher, den Sie für den Dateispeicher verwenden, eine der folgenden Optionen zum Sichern und Wiederherstellen aus.

1. Für **Azure NetApp Files** können Sie eine On-Demand-Momentaufnahme erstellen und mithilfe von Momentaufnahmerichtlinien eine automatische Momentaufnahme planen. Momentaufnahmekopien stellen eine Zeitpunktkopie Ihres ANF-Volumes dar. Weitere Informationen finden Sie unter [Verwalten von Momentaufnahmen mithilfe von Azure NetApp Files](../../../azure-netapp-files/azure-netapp-files-manage-snapshots.md).
2. Die **Azure Files**-Sicherung ist in den nativen [Azure Backup](../../../backup/backup-overview.md)-Dienst integriert, der die Sicherungs- und Wiederherstellungsfunktion zusammen mit der Sicherung von VMs zentralisiert und die Abläufe vereinfacht. Weitere Informationen finden Sie unter [Sichern von Azure-Dateifreigaben](../../../backup/azure-file-share-backup-overview.md) und [Häufig gestellte Fragen zu Azure Files](../../../backup/backup-azure-files-faq.yml).

Wenn Sie einen separaten NFS-Server erstellt haben, stellen Sie sicher, dass Sie die Backup- und Restore-Strategie für die gleiche implementieren.

### <a name="backup--restore-for-cms-and-audit-database"></a>Sichern und Wiederherstellen für CMS-Datenbank

Für die SAP BOBI-Plattform, die auf virtuellen Windows-Computern ausgeführt wird, können CMS und Überwachungsdatenbank auf allen unterstützten Datenbanken ausgeführt werden, wie in der [Unterstützungsmatrix](businessobjects-deployment-guide.md#support-matrix) des SAP BusinessObjects BI-Plattformplanungs- und -Implementierungshandbuchs in Azure beschrieben. Daher ist es wichtig, dass Sie die Sicherungs- und Wiederherstellungsstrategie basierend auf der Datenbank übernehmen, die für CMS und den Überwachungsdatenspeicher verwendet wird.

1. **Azure SQL-Datenbank** nutzt SQL Server-Technologie, um wöchentlich [vollständige Sicherungen](/sql/relational-databases/backup-restore/full-database-backups-sql-server?preserve-view=true&view=sql-server-ver15), alle 12-24 Stunden [differenzielle Sicherungen](/sql/relational-databases/backup-restore/differential-backups-sql-server?preserve-view=true&view=sql-server-ver15) und alle 5 bis 10 Minuten [Transaktionsprotokollsicherungen](/sql/relational-databases/backup-restore/transaction-log-backups-sql-server?preserve-view=true&view=sql-server-ver15) zu erstellen. Die Häufigkeit von Transaktionsprotokollsicherungen basiert auf der Computegröße und dem Umfang der Datenbankaktivität. Benutzer können eine Option zum Konfigurieren der Sicherungsspeicherredundanz zwischen lokal redundanten, zonenredundanten oder georedundanten Speicherblobs auswählen. Mechanismen der Speicherredundanz speichern mehrere Kopien Ihrer Daten, damit sie vor geplanten und ungeplanten Ereignissen geschützt sind – von vorübergehend auftretenden Hardwarefehlern über Netzwerk- oder Stromausfälle bis hin zu schweren Naturkatastrophen. Standardmäßig speichert die Azure SQL-Datenbank in georedundanten [Speicherblobs](../../../storage/common/storage-redundancy.md), die in einem [Regionspaar](../../../best-practices-availability-paired-regions.md) repliziert werden. Sie kann basierend auf den geschäftlichen Anforderungen in lokal redundante oder zonenredundante Speicherblobs geändert werden. Weitere aktuelle Informationen zur Planung, Aufbewahrung und Speichernutzung Azure SQL Datenbanksicherung finden Sie unter [Automatisierte Sicherungen](../../../azure-sql/database/automated-backups-overview.md) – Azure SQL-Datenbank &amp; SQL Managed Instance

2. **Azure Database of MySQL** erstellt automatisch Server-Backups und speichert diese in vom Benutzer konfigurierten im lokal redundanten oder georedundanten Speicher. Azure Database for MySQL nimmt Sicherungen der Datendateien und der Transaktionsprotokolle vor. Abhängig von der unterstützten maximalen Speichergröße werden entweder vollständige oder differenzielle Sicherungen (Server mit maximal 4 TB Speicher) oder Sicherungsmomentaufnahmen (Server mit bis zu 16 TB Speicher) angelegt. Diese Sicherungen ermöglichen es Ihnen, einen Server zu jedem Zeitpunkt innerhalb des von Ihnen konfigurierten Aufbewahrungszeitraums für Sicherungen wiederherzustellen. Der standardmäßige Aufbewahrungszeitraum für Sicherungen beträgt sieben Tage, den Sie [optional auf bis zu 35 Tage festlegen können](../../../mysql/howto-restore-server-portal.md#set-backup-configuration). Zur Verschlüsselung aller Sicherungen wird die AES-Verschlüsselung mit 256 Bit verwendet. Diese Sicherungsdateien sind nicht für den Benutzer verfügbar und können nicht exportiert werden. Sie können nur für Wiederherstellungsvorgänge in Azure Database for MySQL verwendet werden. Zum Kopieren einer Datenbank können Sie [mysqldump](../../../mysql/concepts-migrate-dump-restore.md) verwenden. Weitere Informationen finden Sie unter [Sicherung und Wiederherstellung in Azure Database for MySQL](../../../mysql/concepts-backup.md).

3. Für Datenbanken, die auf einem virtuellen Azure-Computer installiert sind, können Sie Standardsicherungstools oder [Azure Backup](../../../backup/sap-hana-db-about.md) Dienst für unterstützte Datenbanken verwenden. Wenn die Azure-Dienste und -Tools Ihre Anforderungen nicht erfüllen, können Sie auch unterstützte Sicherungstools von Drittanbietern verwenden, die einen Agent für die Sicherung und Wiederherstellung aller SAP BOBI-Plattformkomponenten bereitstellen.

## <a name="high-availability"></a>Hochverfügbarkeit

Hochverfügbarkeit bezieht sich auf eine Reihe von Technologien, die IT-Störungen minimieren kann, indem sie die Geschäftskontinuität von Anwendungen/Diensten durch redundante, fehlertolerante oder durch Failover geschützte Komponenten innerhalb desselben Rechenzentrums bereitstellen. In unserem Fall befindet sich die Rechenzentren in einer Azure-Region. Der Artikel [Hochverfügbarkeits-Architektur und -Szenarien für SAP](sap-high-availability-architecture-scenarios.md) bietet einen Einblick in verschiedene Techniken zur Hochverfügbarkeit und Empfehlungen für Azure für SAP-Anwendungen, welche die Anleitungen in diesem Abschnitt ergänzen.

Basierend auf dem Dimensionierungsergebnis der SAP BOBI-Plattform müssen Sie die Umgebung entwerfen und die Verteilung der BI-Komponenten auf Azure Virtual Machines und Subnetze bestimmen. Das Maß der Redundanz in der verteilten Architektur hängt von dem geschäftlich erforderlichen RTO (Recovery Time Objective) und RPO (Recovery Point Objective) ab. Die SAP BOBI-Plattform umfasst verschiedene Ebenen, und die Komponenten auf den einzelnen Ebenen sollten so konzipiert sein, dass Redundanz erreicht wird. Wenn bei einer Komponente ein Fehler auftritt, gibt es nur wenige oder keine Unterbrechungen für Ihre SAP BOBI-Anwendung. Beispiel:

- Redundante Anwendungsserver wie BI-Anwendungsserver und -Webserver
- Einzigartige Komponenten wie CMS-Datenbank, Filestore, Load Balancer

Im folgenden Abschnitt wird beschrieben, wie Sie für jede Komponente der SAP BOBI-Plattform Hochverfügbarkeit erreichen können.

### <a name="high-availability-for-application-servers"></a>Hochverfügbarkeit für Anwendungsserver

Für BI- und Webanwendungsserver (ob getrennt oder zusammen installiert), ist keine bestimmte Lösung für Hochverfügbarkeit erforderlich. Sie können Hochverfügbarkeit durch Redundanz erreichen, d. h. durch die Konfiguration mehrerer Instanzen von BI- und Webservern in verschiedenen Azure Virtual Machines-Instanzen. Sie können diesen virtuellen Computer entweder in [Verfügbarkeitsgruppen](sap-high-availability-architecture-scenarios.md#multiple-instances-of-virtual-machines-in-the-same-availability-set) oder [Verfügbarkeitszonen](sap-high-availability-architecture-scenarios.md#azure-availability-zones) basierend auf der für das Unternehmen erforderlichen RTO bereitstellen. Stellen Sie für die Bereitstellung über Verfügbarkeitszonen sicher, dass alle anderen Komponenten der SAP BOBI-Plattform auch zonenredundant sind.

Derzeit bieten nicht alle Azure-Regionen Verfügbarkeitszonen, daher müssen Sie die Bereitstellungsstrategie basierend auf Ihrer Region übernehmen. Die Azure-Regionen, die Zonen anbieten, sind unter [Azure-Verfügbarkeitszonen](../../../availability-zones/az-overview.md) aufgelistet.

> [!Important]
> Die Konzepte von Azure-Verfügbarkeitszonen und Azure-Verfügbarkeitsgruppen schließen sich gegenseitig aus. Dies bedeutet, dass Sie entweder zwei oder mehrere VMs in einer bestimmten Verfügbarkeitszone oder einer Azure-Verfügbarkeitsgruppe bereitstellen können, jedoch nicht in beiden.

### <a name="high-availability-for-cms-database"></a>Hochverfügbarkeit für die CMS-Datenbank

Wenn Sie den Azure-DBaaS-Dienst (Database-as-a-Service) für die CMS-Datenbank und Überwachungsdatenbank verwenden, wird standardmäßig ein lokal redundantes Hochverfügbarkeitsframework bereitgestellt. Sie müssen nur die Region und den Dienst auswählen, der Funktionen für Hochverfügbarkeit, Redundanz und Resilienz bietet, ohne dass Sie zusätzliche Komponenten konfigurieren müssen. Wenn die Bereitstellungsstrategie für die SAP BOBI-Plattform per Verfügbarkeitszone erfolgt, müssen Sie sicherstellen, dass Sie Zonenredundanz für Ihre CMS- und Überwachungsdatenbanken erzielen. Weitere Informationen zum Hochverfügbarkeitsangebot für unterstützte DBaaS-Angebote in Azure finden Sie unter [Hochverfügbarkeit für Azure SQL-Datenbank](../../../azure-sql/database/high-availability-sla.md) und [Hochverfügbarkeit in Azure Database for MySQL](../../../mysql/concepts-high-availability.md). 

Weitere Informationen zur DBMS-Bereitstellung für CMS-Datenbanken finden Sie in den [DBMS-Bereitstellungshandbüchern für SAP-Workloads](dbms_guide_general.md), die einen Einblick in die verschiedenen DBMS-Bereitstellungen und ihren Ansatz zum Erzielen von Hochverfügbarkeit bietet.

### <a name="high-availability-for-filestore"></a>Hochverfügbarkeit für Filestore

Filestore bezieht sich auf die Datenträgerverzeichnisse, in denen Inhalte wie Berichte, Universen und Verbindungen gespeichert werden. Es wird von allen Anwendungsservern dieses Systems gemeinsam genutzt. Daher müssen Sie nur sicherstellen, dass sie zusammen mit anderen SAP BOBI-Plattformkomponenten hoch verfügbar ist.

Für die unter Windows ausgeführte SAP BOBI-Plattform können Sie entweder [Azure Premium Files](../../../storage/files/storage-files-introduction.md) oder [Azure NetApp Files](../../../azure-netapp-files/azure-netapp-files-introduction.md) für den Dateispeicher auswählen, die hochverfügbar und dauerhaft ausgelegt sind. Azure Premium-Dateien unterstützen zonenredundanten Speicher, der für die zonenübergreifende Bereitstellung der SAP BOBI-Plattform nützlich sein kann. Weitere Informationen finden Sie im Abschnitt [Redundanz](../../../storage/files/storage-files-planning.md#redundancy) für Azure Files.

Da dieser Dateifreigabedienst nicht in allen Regionen verfügbar ist, stellen Sie sicher, dass Sie auf der Website [Verfügbare Produkte nach Region](https://azure.microsoft.com/en-us/global-infrastructure/services/) nach aktuellen Informationen suchen. Wenn der Dienst in Ihrer Region nicht verfügbar ist, können Sie einen NFS-Server erstellen, über den Sie das Dateisystem für die SAP BOBI-Anwendung freigeben können. Sie müssen aber auch seine Hochverfügbarkeit berücksichtigen.

### <a name="high-availability-for-load-balancer"></a>Hochverfügbarkeit für den Lastenausgleich

Für die Verteilung des Datenverkehrs über Webserver können Sie entweder Azure Load Balancer oder Azure Application Gateway verwenden. Die Redundanz für einen der beiden Lastenausgleiche kann auf der Grundlage der SKU erreicht werden, die Sie für die Bereitstellung auswählen.

1. Für Azure Load Balancer kann Redundanz erreicht werden, indem das Front-End von Load Balancer Standard als zonenredundant konfiguriert wird. Weitere Informationen finden Sie unter [Load Balancer Standard und Verfügbarkeitszonen](../../../load-balancer/load-balancer-standard-availability-zones.md).
2. Für Application Gateway kann Hochverfügbarkeit basierend auf der Art der während der Bereitstellung ausgewählten Ebene erreicht werden.
   1. Die v1-SKU unterstützt Szenarien mit Hochverfügbarkeit, wenn Sie mindestens zwei Instanzen bereitgestellt haben. Azure verteilt diese Instanzen auf Update- und Fehlerdomänen, um sicherzustellen, dass nicht alle Instanzen gleichzeitig ausfallen. Mit dieser SKU kann die Redundanz in der Zone erreicht werden.
   2. Die v2-SKU stellt automatisch sicher, dass neue Instanzen über Fehlerdomänen und Updatedomänen hinweg verteilt werden. Haben Sie Zonenredundanz gewählt, werden die neuesten Instanzen darüber hinaus über Verfügbarkeitszonen hinweg verteilt, um Resilienz gegen Zonenausfälle zu erreichen. Ausführlichere Informationen finden Sie unter [Automatische Skalierung und zonenredundantes Application Gateway v2](../../../application-gateway/application-gateway-autoscaling-zone-redundant.md).

### <a name="reference-high-availability-architecture-for-sap-businessobjects-bi-platform"></a>Referenzarchitektur für Hochverfügbarkeit für die SAP BusinessObjects BI-Plattform

Die folgende Referenzarchitektur beschreibt die Einrichtung der SAP BOBI-Plattform über Verfügbarkeitszonen hinweg, die unter Windows Server ausgeführt werden. Die Architektur zeigt die Verwendung verschiedener Azure-Dienste wie Azure Application Gateway, Azure Premium Files (Filestore) und Azure SQL-Datenbank (CMS- und Überwachungsdatenbank) für die SAP BOBI-Plattform, die integrierte Zonenredundanz bieten, was die Komplexität der Verwaltung verschiedener Lösungen für Hochverfügbarkeit reduziert.

In der folgenden Abbildung wird für den eingehenden Datenverkehr (HTTPS – TCP/443) ein Lastenausgleich mithilfe der Azure Application Gateway v2-SKU durchgeführt, die sich über mehrere Verfügbarkeitszonen hinweg erstreckt. Das Anwendungsgateway verteilt die Benutzeranforderung auf Webserver, die über Verfügbarkeitszonen verteilt sind. Der Webserver gibt die Anforderung an Verwaltungs- und Verarbeitungsserverinstanzen weiter, die auf separaten virtuellen Computern über Verfügbarkeitszonen hinweg bereitgestellt werden. Azure Premium Files mit zonenredundantem Speicher wird über einen privaten Link zu virtuellen Computern der Verwaltungs- und Speicherebene angefügt, um auf Inhalte wie Berichte, Universe und Verbindungen zu zugreifen. Die Anwendung greift auf CMS- und die Überwachungsdatenbank zu, die auf einer zonenredundanten Azure SQL-Datenbank (DBaaS) ausgeführt wird, die Datenbanken an mehreren physischen Standorten innerhalb der Azure-Region replizieren.

![Hochverfügbarkeitsarchitektur für die SAP BOBI-Plattform unter Windows](media/businessobjects-deployment-guide/businessobjects-deployment-windows-high-availability-availability-zone.png)

Die obige Architektur bietet einen Einblick, wie die SAP BOBI-Bereitstellung in Azure erfolgen kann. Aber sie behandelt nicht alle möglichen Konfigurationsoptionen für die SAP BOBI-Plattform in Azure. Kunden können die Bereitstellung basierend auf Geschäftsanforderungen anpassen, durch Auswahl verschiedener Produkte bzw. Dienste für Komponenten wie z. B. Lastenausgleich, Dateirepository-Server und DBMS.

Falls Verfügbarkeitszonen nicht in Ihrer ausgewählten Region verfügbar sind, können Sie Azure Virtual Machines in Verfügbarkeitsgruppen bereitstellen. Azure stellt sicher, dass die in einer Verfügbarkeitsgruppe platzierten VMs über mehrere physische Server, Computerracks, Speichereinheiten und Netzwerkswitches hinweg betrieben werden. Sodass im Falle eines Hardware- oder Software-Ausfalles nur ein Subset der VMs betroffen ist und die Gesamtlösung betriebsbereit bleibt.

## <a name="disaster-recovery"></a>Notfallwiederherstellung

Die Anleitung in diesem Abschnitt erläutert die Strategie zur Bereitstellung von Schutz für die Notfallwiederherstellung für die SAP BOBI-Plattform. Sie ergänzt das Dokument [Notfallwiederherstellung für SAP](../../../site-recovery/site-recovery-sap.md), das die primären Ressourcen für den gesamten SAP-Ansatz zur Notfallwiederherstellung darstellt. Informationen zur SAP BusinessObjects BI-Plattform finden Sie im SAP-Hinweis [2056228](https://launchpad.support.sap.com/#/notes/2056228), in dem die folgenden Methoden zum sicheren Implementieren einer DR-Umgebung beschrieben werden.

 1. Vollständige oder selektive Verwendung der Lebenszyklusverwaltung (LCM) oder des Verbunds zum Promoten bzw. Verteilen des Inhalts aus dem primären System.
 2. Regelmäßiges Kopieren der CMS- und FRS-Inhalte.

In diesem Leitfaden wird die zweite Option zum Implementieren einer DR-Umgebung erläutert. Es wird keine vollständige Liste aller möglichen Konfigurationsoptionen für die Notfallwiederherstellung behandelt, sondern eine Lösung, die native Azure-Dienste in Kombination mit der SAP BOBI-Plattformkonfiguration bietet.

>[!Important]
>Die Verfügbarkeit jeder Komponente der SAP BusinessObjects BI-Plattform sollte in die sekundäre Region integriert werden und die gesamte Notfallwiederherstellungsstrategie muss gründlich getestet werden.

### <a name="reference-disaster-recovery-architecture-for-sap-businessobjects-bi-platform"></a>Referenzarchitektur für Notfallwiederherstellung für die SAP BusinessObjects BI-Plattform

Diese Referenzarchitektur führt eine Bereitstellung mit mehreren Instanzen der SAP BOBI-Plattform mit redundanten Anwendungsservern aus. Für die Notfallwiederherstellung sollten Sie ein Fail-Over aller Komponenten der SAP BOBI-Plattform in eine sekundäre Region unternehmen. In der folgenden Abbildung wird Azure Premium Files als Dateispeicher, Azure SQL Datenbank als CMS-/Überwachungsrepository und Azure Application Gateway zum Lastenausgleich des Datenverkehrs verwendet. Die Strategie zum Erreichen des Notfallwiederherstellungsschutzes für jede Komponente unterscheidet sich. Dies wird im folgenden Abschnitt ausführlich beschrieben.

![SAP BusinessObjects BI-Plattform: Notfallwiederherstellung für Windows](media\businessobjects-deployment-guide\businessobjects-deployment-windows-disaster-recovery.png)

### <a name="load-balancer"></a>Load Balancer

Load Balancer wird verwendet, um Datenverkehr über Webanwendungsserver der SAP BOBI-Plattform zu verteilen. In Azure können Sie entweder Azure Load Balancer oder Azure Application Gateway verwenden, um den Datenverkehr zu Ihren Webservern zu verwalten. Um DR für die Lastenausgleichsdienste zu erreichen, müssen Sie einen weiteren Azure Load Balancer oder ein Azure Application Gateway auf der sekundären Region implementieren. Um die gleiche URL nach dem DR-Fail-Over beizubehalten, müssen Sie den Eintrag im DNS ändern, damit dieser auf den Lastenausgleichsdienst auf der sekundären Region verweist.

### <a name="virtual-machines-running-web-and-bi-application-servers"></a>Virtuelle Computer mit Web- und BI-Anwendungsservern

Der [Azure Site Recovery-Dienst](../../../site-recovery/site-recovery-overview.md) kann verwendet werden, um virtuelle Computer, die Web- und BI-Anwendungsserver ausführen, in der sekundären Region zu replizieren. Es repliziert die Server sowie die verbundenen verwaltenen Datenträger auf die sekundäre Region, damit bei Notfällen und Ausfällen ein einfaches Fail-Over auf die replizierte Umgebung möglich ist und Sie weiter arbeiten können. Um mit dem Replizieren aller SAP Application Virtual Machines auf das Azure-Notfallwiederherstellungsdatenzentrum zu beginnen, folgen Sie den Anweisungen in [Replizieren einer Virtual Machine auf Azure](../../../site-recovery/azure-to-azure-tutorial-enable-replication.md).

### <a name="filestore"></a>Filestore

Filestore ist ein Datenträgerverzeichnis, auf dem die tatsächlichen Dateien wie Berichte und BI-Dokumente gespeichert werden. Es ist wichtig, dass alle Dateien bei Filestore synchron zur DR-Region sind. Basierend auf dem Typ des Dateifreigabedienstes, den Sie für die SAP BOBI-Plattform für Windows verwenden, muss die entsprechende DR-Strategie synchron zum Inhalt sein.

- **Azure Files Premium** unterstützt nur lokal redundanten Speicher (LRS) und zonenredundanten Speicher (ZRS). Für die Azure Files Premium-Strategie zur Notfallwiederherstellung können Sie [AzCopy](../../../storage/common/storage-use-azcopy-v10.md) oder [Azure PowerShell](/powershell/module/az.storage/?preserve-view=true&view=azps-5.8.0) verwenden, um die Dateien in ein anderes Speicherkonto in einer anderen Region zu kopieren. Weitere Informationen finden Sie unter [Notfallwiederherstellung und Speicherkontofailover](../../../storage/common/storage-disaster-recovery-guidance.md).

- **Azure NetApp Files** stellt NFS- und SMB-Volumes bereit, sodass jedes dateibasierte Kopiertool zur Replikation von Daten zwischen Azure-Regionen verwendet werden kann. Weitere Informationen zum Kopieren eines ANF-Volumes in eine andere Region finden Sie unter [Häufig gestellte Fragen zu Azure NetApp Files](../../../azure-netapp-files/azure-netapp-files-faqs.md#how-do-i-create-a-copy-of-an-azure-netapp-files-volume-in-another-azure-region).

  Sie können die regionsübergreifende Replikation von Azure NetApp Files verwenden, die sich derzeit in der [Vorschauversion](https://azure.microsoft.com/en-us/blog/azure-netapp-files-cross-region-replication-and-new-enhancements-in-preview/) befindet und die NetApp SnapMirror®-Technologie verwendet. Daher werden nur geänderte Blöcke in einem komprimierten, effizienten Format über das Netzwerk gesendet. Diese proprietäre Technologie minimiert die Menge an Daten, die regionsübergreifend repliziert werden muss, wodurch Datenübertragungskosten eingespart werden. Auch die Replikationszeit verkürzt sich dadurch, sodass Sie eine kürzere Recovery Point Objective (RPO) erreichen können. Weitere Informationen finden Sie unter [Regionsübergreifende Replikation: Anforderungen und Überlegungen](../../../azure-netapp-files/cross-region-replication-requirements-considerations.md).

### <a name="cms-database"></a>CMS-Datenbank

Die CMS- und Überwachungsdatenbank in der DR-Region muss eine Kopie der Datenbanken sein, die in der primären Region ausgeführt werden. Basierend auf dem Datenbanktyp muss die Datenbank in die DR-Region kopiert werden, basierend auf dem vom Unternehmen erforderlichen RTO und RPO. Dieser Abschnitt beschreibt verschiedene verfügbare Optionen für jeden DBaaS-Dienst (Database-as-a-Service) bei Azure, der von der auf Windows ausgeführten SAP BOBI-Anwendung unterstützt wird.

#### <a name="azure-sql-database"></a>Azure SQL-Datenbank

Für die [Azure SQL Database](../../../azure-sql/database/business-continuity-high-availability-disaster-recover-hadr-overview.md) DR-Strategie gibt es zwei verfügbare Optionen zum Kopieren der Datenbank auf die sekundäre Region. Beide Wiederherstellungsoptionen bieten unterschiedliche RTO- und RPO-Ebenen. Weitere Informationen zu RTO und RPO für jede Wiederherstellungsoption finden Sie unter [Wiederherstellen einer Datenbank auf einem vorhandenen Server](../../../azure-sql/database/business-continuity-high-availability-disaster-recover-hadr-overview.md#recover-a-database-to-the-existing-server).

1. [Georedundante Datenbankwiederherstellung](../../../azure-sql/database/recovery-using-backups.md#geo-restore)

   Standardmäßig speichert die Azure SQL-Datenbank in georedundanten [Speicherblobs](../../../storage/common/storage-redundancy.md), die in einem [Regionspaar](../../../best-practices-availability-paired-regions.md) repliziert werden. Die Sicherungsspeicherredundanz für eine SQL-Datenbank-Instanz kann entweder beim Erstellen der CMS- und Überprüfungsdatenbank konfiguriert oder für vorhandene Datenbanken aktualisiert werden. Die an einer vorhandenen Datenbank vorgenommenen Änderungen gelten jedoch nur für zukünftige Sicherungen. Sie können eine Datenbank auf einer beliebigen Azure SQL-Datenbank in einer beliebigen Azure-Region aus den letzten georeplizierten Sicherungen wiederherstellen. Geowiederherstellung verwendet eine georeplizierte Sicherung als Quelle. Es gibt jedoch eine Verzögerung zwischen dem Erstellen einer Sicherung und der Georeplikation in einem Azure-Blob in einer anderen Region. Daher kann die wiederhergestellte Datenbank bis zu einer Stunde hinter der ursprünglichen Datenbank zurückliegen.

   >[!Important]
   >Geowiederherstellung ist für Azure SQL-Datenbanken verfügbar, die mit einem georedundanten [Sicherungsspeicher](../../../azure-sql/database/automated-backups-overview.md#backup-storage-redundancy) konfiguriert wurden.

2. [Georeplikation](../../../azure-sql/database/active-geo-replication-overview.md) oder [Autofailover-Gruppe](../../../azure-sql/database/auto-failover-group-overview.md)

   Die aktive Georeplikation ist ein Feature der Azure SQL-Datenbank, mit der Sie lesbare sekundäre Datenbanken für einzelne Datenbanken auf einem Server in der gleichen oder einer anderen Region erstellen können. Wenn Georeplikation für CMS- und Überwachungsdatenbanken aktiviert ist, kann die Anwendung ein Failover auf eine sekundäre Datenbank in einer anderen Azure-Region initiieren. Die Georeplikation ist für einzelne Datenbanken aktiviert, aber um ein transparentes und koordiniertes Failover mehrerer Datenbanken (CMS und Überwachung) für die SAP BOBI-Anwendung zu ermöglichen, ist es ratsam, die Automatische Failover-Gruppe zu verwenden. Sie stellt die Gruppensemantik auf Grundlage der aktiven Georeplikation zur Verfügung. Dies bedeutet, dass der gesamte Azure SQL-Server (alle Datenbanken) in eine andere Region und nicht in einzelne Datenbanken repliziert wird. Überprüfen Sie die Funktionstabelle in [Vergleich der Georeplikation mit Failover-Gruppen](../../../azure-sql/database/business-continuity-high-availability-disaster-recover-hadr-overview.md#compare-geo-replication-with-failover-groups).

   Autofailover-Gruppen bieten lese- bzw. schreibgeschützte Listener-Endpunkte, die während eines Failovers unverändert bleiben. Der Lese-/Schreib-Endpunkt kann als Listener im ODBC-Verbindungseintrag für CMS- und Überwachungsdatenbank verwaltet werden. Sowohl bei manueller als auch automatischer Failover-Aktivierung schaltet das Failover alle sekundären Datenbanken in der Gruppe zur primären um. Nach Abschluss des Datenbankfailovers wird der DNS-Eintrag automatisch aktualisiert, um die Endpunkte in die neue Region umzuleiten. Daher wird die Anwendung automatisch mit der CMS-Datenbank verbunden, da Lese-/Schreib-Endpunkte als Listener in der ODBC-Verbindung verwaltet werden.

   In der folgenden Abbildung wird die Autofailover-Gruppe für den Azure SQL-Server (azussqlbodb), der in der Region „USA, Osten 2“ ausgeführt wird, in die sekundäre Region „USA, Osten“ (Standort der DR) repliziert. Der Lese-/Schreib-Listener-Endpunkt wird als Listener in der ODBC-Verbindung für den BI-Anwendungsserver verwaltet, der unter Windows ausgeführt wird. Nach dem Failover bleibt der Endpunkt unverändert und es ist kein manueller Eingriff erforderlich, um die BI-Anwendung mit Azure SQL-Datenbank in der sekundären Region zu verbinden.

   ![Azure SQL Datenbank Autofailover-Gruppen](media\businessobjects-deployment-guide\businessobjects-deployment-windows-sql-failover-group.png)

   Diese Option bietet eine niedrigere RTO und RPO als Option 1. Weitere Informationen zu dieser Option finden Sie unter [Verwenden von Autofailover-Gruppen für transparentes und koordiniertes Failover mehrerer Datenbanken](../../../azure-sql/database/auto-failover-group-overview.md)

#### <a name="azure-database-for-mysql"></a>Azure Database for MySQL

Azure Database for MySQL bietet mehrere Optionen zum Wiederherstellen einer Datenbank im Notfall. Wählen Sie die entsprechende Option aus, die für Ihr Unternehmen geeignet ist.

1. Aktivieren Sie regionsübergreifende Lesereplikate, um Ihre BCDR-Planung (Business Continuity & Disaster Recovery) zu verbessern. Sie können vom Quellserver auf bis zu fünf Replikate replizieren. Lesereplikate werden mithilfe der binären Protokollreplikation von MySQL asynchron aktualisiert. Replikate sind neue Server, die Sie ähnlich wie normale Azure Database for MySQL-Server verwalten. Weitere Informationen zu Lesereplikaten, zu verfügbaren Regionen, zu Einschränkungen und zum Failover finden Sie im [Konzeptartikel zu Lesereplikaten](../../../mysql/concepts-read-replicas.md).

2. Verwenden Sie das Geowiederherstellungsfeature von Azure Database for MySQL, das den Server mithilfe von georedundanten Sicherungen wiederherstellt. Auf diese Sicherungen kann selbst dann zugegriffen werden, wenn die Region, in der Ihr Server gehostet wird, offline ist. Sie können eine Wiederherstellung aus diesen Sicherungen in eine andere Region ausführen und den Server wieder online schalten.

  > [!Important]
  > Die Geowiederherstellung ist nur möglich, wenn Sie den Server mit einem georedundanten Sicherungsspeicher bereitgestellt haben. Das Ändern der **Optionen für Sicherungsredundanz** nach der Erstellung des Servers wird nicht unterstützt. Weitere Informationen finden Sie im Artikel [Sicherungsredundanz](../../../mysql/concepts-backup.md#backup-redundancy-options).

Nachfolgend ist die Empfehlung für eine Notfallwiederherstellung der einzelnen Ebenen angegeben, die in diesem Beispiel verwendet werden.

| Ebenen der SAP BOBI-Plattform                          | Empfehlung                                               |
| ------------------------------------------------ | ------------------------------------------------------------ |
| Azure Application Gateway oder Azure Load Balancer | Paralleles Einrichten von Application Gateway in der sekundären Region    |
| Webanwendungsserver                          | Replizieren über Site Recovery                             |
| BI-Anwendungsserver                           | Replizieren über Site Recovery                             |
| Azure Premium-Dateien                              | AzCopy **oder** Azure PowerShell                               |
| Azure NetApp Files                               | Dateibasiertes Kopiertool zum Replizieren von Daten in die sekundäre Region **oder** für die regionsübergreifende ANF-Replikation (Vorschau) |
| Azure SQL-Datenbank                               | Georeplikation bzw. Autofailover-Gruppen **oder** Geowiederherstellung.     |
| Azure Database for MySQL                         | Regionsübergreifende Lesereplikate **oder** Wiederherstellung einer Sicherung aus georedundanten Sicherungen. |
## <a name="next-steps"></a>Nächste Schritte

- [Einrichten der Notfallwiederherstellung für die Bereitstellung einer SAP-App mit mehreren Ebenen](../../../site-recovery/site-recovery-sap.md)
- [Azure Virtual Machines – Planung und Implementierung für SAP](planning-guide.md)
- [Azure Virtual Machines – Bereitstellung für SAP](deployment-guide.md)
- [Azure Virtual Machines – DBMS-Bereitstellung für SAP](./dbms_guide_general.md)