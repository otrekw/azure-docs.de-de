---
title: Verwenden von Azure Migrate mit privaten Endpunkten
description: Verwenden Sie Azure Migrate mit Private Link-Unterstützung für das Entdecken, Bewerten und Migrieren.
author: deseelam
ms.author: deseelam
ms.manager: bsiva
ms.topic: how-to
ms.date: 05/10/2020
ms.openlocfilehash: 85202474e8fd3c4340357b99ae91ee7d25d79ebf
ms.sourcegitcommit: 8bca2d622fdce67b07746a2fb5a40c0c644100c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2021
ms.locfileid: "111747183"
---
# <a name="using-azure-migrate-with-private-endpoints"></a>Verwenden von Azure Migrate mit privaten Endpunkten  

In diesem Artikel wird beschrieben, wie Sie Azure Migrate verwenden, um Server über ein privates Netzwerk mithilfe von [Azure Private Link](../private-link/private-endpoint-overview.md) zu entdecken, zu bewerten und zu migrieren.

Sie können die Tools [Azure Migrate: Ermittlung und Bewertung](./migrate-services-overview.md#azure-migrate-discovery-and-assessment-tool) und [Azure Migrate: Servermigration](./migrate-services-overview.md#azure-migrate-server-migration-tool) verwenden, um eine private und sichere Verbindung mit dem Azure Migrate-Dienst über ein privates ExpressRoute-Peering oder eine Site-to-Site-VPN-Verbindung mithilfe von Azure Private Link herzustellen.

Die Verbindungsherstellung über private Endpunkte wird empfohlen, wenn die Organisationsanforderungen den Zugriff auf den Azure Migrate-Dienst und andere Azure-Ressourcen über öffentliche Netzwerke verbieten. Mit Private Link können Sie auch vorhandene ExpressRoute-Leitungen zum privaten Peering nutzen, um höhere Bandbreiten- oder Latenzanforderungen zu erfüllen.

## <a name="support-requirements"></a>Supportanforderungen

### <a name="required-permissions"></a>Erforderliche Berechtigungen

Berechtigungen **Mitwirkender und Benutzerzugriffsadministrator** oder **Besitzer** für das Abonnement

### <a name="supported-scenarios-and-tools"></a>Unterstützte Szenarien und Tools

**Bereitstellung** | **Details** | **Extras**
--- | --- | ---
**Ermittlung und Bewertung** | Ausführen von Ermittlung und Bewertung Ihrer Server ohne Agent im großen Stil und auf beliebigen Plattformen – Hypervisorplattformen wie [VMware vSphere](./tutorial-discover-vmware.md) oder [Microsoft Hyper-V](./tutorial-discover-hyper-v.md), öffentliche Clouds wie [AWS](./tutorial-discover-aws.md) oder [GCP](./tutorial-discover-gcp.md) und sogar [Bare-Metal-Server](./tutorial-discover-physical.md) | Azure Migrate: Ermittlung und Bewertung  <br/>
**Softwareinventur** | Entdecken von Apps, Rollen und Features, die auf VMware-VMs ausgeführt werden | Azure Migrate: Ermittlung und Bewertung  
**Visualisierung von Abhängigkeiten** | Funktionen zur Abhängigkeitsanalyse zum serverübergreifenden Ermitteln und Verstehen von Abhängigkeiten. <br/> Die [Visualisierung von Abhängigkeiten ohne Agent](./how-to-create-group-machine-dependencies-agentless.md) wird nativ durch die Private Link-Unterstützung von Azure Migrate unterstützt. <br/>Für die [Agent-basierte Visualisierung von Abhängigkeiten](./how-to-create-group-machine-dependencies.md) ist eine Internetverbindung erforderlich. [Erfahren Sie](../azure-monitor/logs/private-link-security.md), wie Sie private Endpunkte für die Agent-basierte Visualisierung von Abhängigkeiten verwenden. | Azure Migrate: Ermittlung und Bewertung |
**Migration** | Ausführen von [Hyper-V-Migrationsvorgängen ohne Agent](./tutorial-migrate-hyper-v.md) oder Verwenden des Agent-basierten Ansatzes zum Migrieren von [VMware-VMs](./tutorial-migrate-vmware-agent.md), [Hyper-V-VMs](./tutorial-migrate-physical-virtual-machines.md), [physischen Servern](./tutorial-migrate-physical-virtual-machines.md), [VMs bei AWS](./tutorial-migrate-aws-virtual-machines.md), [VMs bei GCP](./tutorial-migrate-gcp-virtual-machines.md) oder bei einem anderen Virtualisierungsanbieter ausgeführten VMs | Azure Migrate: Servermigration

>[!Note]
>
> [VMware-Migrationsvorgänge ohne Agent](./tutorial-migrate-vmware.md) erfordern Internetzugriff oder Konnektivität über ExpressRoute-Peering mit Microsoft. <br/> [Erfahren Sie](./replicate-using-expressroute.md), wie Sie mithilfe von privaten Endpunkten Replikationen über das private ExpressRoute-Peering oder eine Site-to-Site-VPN-Verbindung (S2S) durchführen.  <br/><br/>

#### <a name="other-integrated-tools"></a>Weitere integrierte Tools

Andere Migrationstools können möglicherweise keine Nutzungsdaten in das Azure Migrate-Projekt hochladen, wenn der Zugriff auf das öffentliche Netzwerk deaktiviert ist. Das Azure Migrate-Projekt sollte so konfiguriert werden, dass Datenverkehr aus allen Netzwerken zugelassen wird, sodass Daten von anderen Microsoft-Angeboten oder Angeboten von externen [unabhängigen Softwareanbietern](./migrate-services-overview.md#isv-integration) (Independent Software Vendor, ISV) empfangen werden können.


Wechseln Sie im Azure-Portal zur Seite **Eigenschaften von Azure Migrate**, und wählen Sie **Nein** > **Speichern** aus, um den Zugriff über das öffentliche Netzwerk für ein Azure Migrate-Projekt zu aktivieren.

![Diagramm der Änderung des Netzwerkzugriffsmodus](./media/how-to-use-azure-migrate-with-private-endpoints/migration-project-properties.png)

### <a name="other-considerations"></a>Weitere Überlegungen   

**Überlegungen** | **Details**
--- | ---
**Preise** | Preisinformationen finden Sie unter [Azure-Seitenblobs: Preise](https://azure.microsoft.com/pricing/details/storage/page-blobs/) und [Azure Private Link – Preise](https://azure.microsoft.com/pricing/details/private-link/).  
**Anforderungen für virtuelle Netzwerke** | Der Endpunkt für ExpressRoute bzw. das VPN-Gateway sollte sich im ausgewählten virtuellen Netzwerk oder in einem mit diesem verbundenen virtuellen Netzwerk befinden. Sie benötigen bis zu ca. 15 IP-Adressen im virtuellen Netzwerk.  

## <a name="create-a-project-with-private-endpoint-connectivity"></a>Erstellen eines Projekts mit Konnektivität zu einem privaten Endpunkt

Richten Sie ein neues Azure Migrate-Projekt nach diesem [Artikel](./create-manage-projects.md#create-a-project-for-the-first-time) ein.

> [!Note]
> Sie können die Konnektivitätsmethode für vorhandene Azure Migrate-Projekte nicht in Konnektivität über private Endpunkte ändern.

Geben Sie im Abschnitt **Erweiterte Konfiguration** die folgenden Details an, um einen privaten Endpunkt für Ihr Azure Migrate-Projekt zu erstellen.
1. Wählen Sie als **Verbindungsmethode** die Option **Privater Endpunkt** aus.
2. Behalten Sie unter **Disable public endpoint access** (Zugriff auf öffentlichen Endpunkt deaktivieren) die Standardeinstellung **Nein** bei. Einige Migrationstools können möglicherweise keine Nutzungsdaten in das Azure Migrate-Projekt hochladen, wenn der Zugriff auf das öffentliche Netzwerk deaktiviert ist. [Weitere Informationen.](#other-integrated-tools)
3. Wählen Sie unter **VNET-Abonnement** das Abonnement für das virtuelle Netzwerk des privaten Endpunkts aus.
4. Wählen Sie unter **Virtuelles Netzwerk** das virtuelle Netzwerk für den privaten Endpunkt aus. Die Azure Migrate-Appliance und andere Softwarekomponenten, die eine Verbindung mit dem Azure Migrate-Projekt herstellen sollen, müssen sich in diesem oder in einem verbundenen virtuellen Netzwerk befinden.
5. Wählen Sie unter **Subnetz** das Subnetz für den privaten Endpunkt aus.

   ![Projekt erstellen](./media/how-to-use-azure-migrate-with-private-endpoints/create-project.png)

6. Klicken Sie auf **Erstellen**. Damit wird ein Migrationsprojekt erstellt und ein privater Endpunkt daran angefügt. Warten Sie einige Minuten, bis das Azure Migrate-Projekt bereitgestellt wurde. Schließen Sie diese Seite nicht, während das Projekt erstellt wird.

## <a name="discover-and-assess-servers-for-migration-using-azure-private-link"></a>Ermitteln und Bewerten von Servern für die Migration mithilfe von Azure Private Link

### <a name="set-up-the-azure-migrate-appliance"></a>Einrichten der Azure Migrate-Appliance

1. Wählen Sie unter **Computer ermitteln** > **Sind Ihre Computer virtualisiert?** den Servertyp aus.
2. Geben Sie unter **Azure Migrate-Projektschlüssel generieren** einen Namen für die Azure Migrate-Appliance an.
3. Wählen Sie **Schlüssel generieren** aus, um die erforderlichen Azure-Ressourcen zu erstellen.

    > [!Important]
    > Schließen Sie die Seite „Computer ermitteln“ nicht, während die Ressourcen erstellt werden.  
    - In diesem Schritt erstellt Azure Migrate einen Schlüsseltresor, ein Speicherkonto, einen Recovery Services-Tresor (nur bei VMware-Migrationsvorgängen ohne Agent) und einige interne Ressourcen und fügt an jede Ressource einen privaten Endpunkt an. Die privaten Endpunkte werden in dem virtuellen Netzwerk erstellt, das während der Projekterstellung ausgewählt wurde.  
    - Nachdem die privaten Endpunkte erstellt wurden, werden die DNS CNAME-Ressourceneinträge für die Azure Migrate-Ressourcen mit einem Alias in einer Unterdomäne mit dem Präfix *privatelink* aktualisiert. Standardmäßig erstellt Azure Migrate auch für jede Unterdomäne *privatelink* der einzelnen Ressourcentypen eine private DNS-Zone, und fügt DNS-A-Datensätze für die zugeordneten privaten Endpunkte ein. Dadurch können die Azure Migrate-Appliance und andere Softwarekomponenten im Quellnetzwerk Endpunkte von Azure Migrate-Ressourcen über deren private IP-Adressen erreichen.  
    - Azure Migrate aktiviert auch eine [verwaltete Identität](../active-directory/managed-identities-azure-resources/overview.md) für das Migrationsprojekt und gewährt der verwalteten Identität Berechtigungen für den sicheren Zugriff auf das Speicherkonto.  

4. Nachdem der Schlüssel erfolgreich generiert wurde, kopieren Sie die Schlüsseldetails, um die Appliance zu konfigurieren und zu registrieren.   

#### <a name="download-the-appliance-installer-file"></a>Herunterladen der Installationsdatei für die Appliance  

Für das Azure Migrate-Tool zur Ermittlung und Bewertung wird eine einfache Azure Migrate-Appliance verwendet. Die Appliance führt die Serverermittlung durch und sendet Metadaten zur Serverkonfiguration und -leistung an Azure Migrate.  

> [!Note]
> Die Option zum Bereitstellen einer Appliance mithilfe einer Vorlage (OVA für Server in der VMware-Umgebung und VHD-Hyper-V-Umgebung) wird für Azure Migrate Projekte mit privater Endpunktkonnektivität nicht unterstützt.

Richten Sie die Appliance wie folgt ein:
  1. Laden Sie die ZIP-Datei mit dem Installationsskript aus dem Portal herunter.
  2. Kopieren Sie die gezippte Datei auf den Server, der die Appliance hostet.   
  3. Überprüfen Sie nach dem Herunterladen der ZIP-Datei die Dateisicherheit.
  4. Führen Sie das Installationsskript aus, um die Appliance bereitzustellen.

Hier sehen Sie die Downloadlinks für jedes Szenario:

Szenario | Downloadlink | Hashwert
--- | --- | ---
Hyper-V | [AzureMigrateInstaller-HyperV-Public-PrivateLink.zip](https://go.microsoft.com/fwlink/?linkid=2160557) | CBF8927AF137A106E2A34AC4F77CFFCB1CD96873C592E1DF37BC5606254989EC
Physisch | [AzureMigrateInstaller-Physical-Public-PrivateLink.zip](https://go.microsoft.com/fwlink/?linkid=2160558) | 1CB967D92096EB48E4C3C809097F52C8341FC7CA7607CF840C529E7A21B1A21D
VMware | [AzureMigrateInstaller-VMware-public-PrivateLink.zip](https://go.microsoft.com/fwlink/?linkid=2160648) | 0A4FCC4D1500442C5EB35E4095EF781CB17E8ECFE8E4F8C859E65231E00BB154
Horizontale VMware-Skalierung | [AzureMigrateInstaller-VMware-Public-Scaleout-PrivateLink.zip](https://go.microsoft.com/fwlink/?linkid=2160811) | 2F035D34E982EE507EAEC59148FDA8327A45D2A845B4A95475EC6D2469D72D28

#### <a name="verify-security"></a>Überprüfen der Sicherheit

Vergewissern Sie sich vor der Bereitstellung, dass die gezippte Datei sicher ist.

1. Öffnen Sie auf dem Server, auf den Sie die Datei heruntergeladen haben, ein Administratorbefehlsfenster.
2. Führen Sie den folgenden Befehl aus, um den Hash für die gezippte Datei zu generieren:

    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Beispielverwendung für die öffentliche Cloud: ```C:\>CertUtil -HashFile C:\Users\administrator\Desktop\AzureMigrateInstaller-VMware-public-PrivateLink.zip SHA256 ```

3.  Überprüfen der aktuellen Version der Appliance, indem Sie die Hashwerte aus der obigen Tabelle vergleichen.

Stellen Sie sicher, dass der Server die [Hardwareanforderungen](./migrate-appliance.md) für das ausgewählte Szenario erfüllt (VMware/Hyper-V/physisch usw.) und eine Verbindung mit den [erforderlichen URLs](./migrate-appliance.md#public-cloud-urls-for-private-link-connectivity) herstellen kann.


#### <a name="run-the-script"></a>Führen Sie das Skript aus.

1. Extrahieren Sie die gezippte Datei in einem Ordner auf dem Server, der die Appliance hostet.
2. Starten Sie PowerShell auf dem Computer mit Administratorberechtigungen (erhöhten Rechten).
3. Ändern Sie das PowerShell-Verzeichnis in den Ordner mit den Inhalten, die aus der gezippten Datei extrahiert wurden, die Sie heruntergeladen haben.
4. Führen Sie das Skript **AzureMigrateInstaller.ps1** wie folgt aus:

    ```
    PS C:\Users\administrator\Desktop\AzureMigrateInstaller-VMware-public-PrivateLink> .\AzureMigrateInstaller.ps1
    ```

5. Nachdem das Skript erfolgreich ausgeführt wurde, wird der Konfigurations-Manager für die Appliance gestartet, damit Sie sie einrichten können. Bei Problemen sehen Sie sich die Skriptprotokolle in diesem Pfad an: C:\ProgramData\Microsoft Azure\Logs\AzureMigrateScenarioInstaller_<em>Zeitstempel</em>.log.

### <a name="configure-the-appliance-and-start-continuous-discovery"></a>Konfigurieren der Appliance und Starten der kontinuierlichen Ermittlung

Öffnen Sie in einem Browser auf einem beliebigen Computer, der eine Verbindung mit dem Server auf der Appliance herstellen kann, und öffnen Sie die URL des Konfigurations-Managers der Appliance: `https://appliance name or IP address: 44368`. Alternativ können Sie den Konfigurations-Manager auch auf dem Desktop des Servers auf der Appliance öffnen, indem Sie die Verknüpfung für den Konfigurations-Manager auswählen.

#### <a name="set-up-prerequisites"></a>Einrichten erforderlicher Komponenten

1. Lesen Sie die Drittanbieterinformationen, und akzeptieren Sie die **Lizenzbedingungen**.    

2. Navigieren Sie im Konfigurations-Manager zu **Erforderliche Komponenten einrichten**, und führen Sie die folgenden Schritte aus:
   - **Konnektivität:** Die Appliance überprüft den Zugriff auf die erforderlichen URLs. Wenn der Server einen Proxy verwendet:
     - Wählen Sie **Proxy einrichten** aus, um die Proxyadresse `http://ProxyIPAddress` oder `http://ProxyFQDN` und den lauschenden Port anzugeben.
     - Geben Sie die Anmeldeinformationen an, wenn der Proxy eine Authentifizierung erfordert. Es werden nur HTTP-Proxys unterstützt.
     - Sie können eine Liste von URLs/IP-Adressen hinzufügen, die den Proxyserver umgehen sollen.
     - Wenn Sie Proxyserverdetails geändert oder URLs/IP-Adressen für die Proxyumgehung hinzugefügt haben, wählen Sie **Speichern** aus, damit die Konfiguration registriert wird.

        > [!Note]
        > Wenn Sie während der Konnektivitätsprüfung einen Fehler mit dem Link aka.ms/* erhalten und nicht möchten, dass die Appliance über das Internet auf diese URL zugreift, müssen Sie den Dienst für automatische Updates auf der Appliance deaktivieren. Befolgen Sie dazu die [**hier**](./migrate-appliance.md#turn-off-auto-update) beschriebenen Schritte. Nachdem automatische Updates deaktiviert wurden, wird die URL-Konnektivitätsprüfung über aka.ms/* übersprungen.

   - **Uhrzeitsynchronisierung**: Die Uhrzeit der Appliance muss mit der Internetzeit synchronisiert werden, damit die Ermittlung ordnungsgemäß funktioniert.
   - **Updates installieren**: Die Appliance stellt sicher, dass die neuesten Updates installiert sind. Nachdem die Überprüfung abgeschlossen ist, wählen Sie **Appliancedienste anzeigen** aus, um den Status und die Versionen der auf dem Applianceserver ausgeführten Dienste anzuzeigen.
        > [!Note]
        > Wenn Sie sich dafür entschieden haben, automatische Updates auf der Appliance zu deaktivieren, können Sie die Appliancedienste manuell aktualisieren, um die neuesten Versionen der Dienste abzurufen. Befolgen Sie dazu die [**hier**](./migrate-appliance.md#manually-update-an-older-version) beschriebenen Schritte.
   - **Install VDDK** (VDDK installieren) (_nur bei VMware-Appliances erforderlich_): Die Appliance überprüft, ob das VMware vSphere-VDDK (Virtual Disk Development Kit) installiert ist. Laden Sie bei Bedarf VDDK 6.7 von VMware herunter, und extrahieren Sie den Inhalt der heruntergeladenen ZIP-Datei am angegebenen Ort auf der Appliance, wie unter **Installationsanweisungen** beschrieben.

#### <a name="register-the-appliance-and-start-continuous-discovery"></a>Registrieren der Appliance und Starten der kontinuierlichen Ermittlung

Führen Sie nach der Überprüfung der Voraussetzungen die folgenden Schritte aus, um die Appliance zu registrieren und die kontinuierliche Ermittlung für die jeweiligen Szenarios zu starten:
- [Virtuelle VMware-Computer](./tutorial-discover-vmware.md#register-the-appliance-with-azure-migrate)
- [Virtuelle Hyper-V-Computer](./tutorial-discover-hyper-v.md#register-the-appliance-with-azure-migrate)
- [Physische Server](./tutorial-discover-physical.md#register-the-appliance-with-azure-migrate)
- [AWS-VMs](./tutorial-discover-aws.md#register-the-appliance-with-azure-migrate)
- [GCP-VMs](./tutorial-discover-gcp.md#register-the-appliance-with-azure-migrate)


>[!Note]
> Wenn bei der Registrierung der Appliance oder beim Starten der Ermittlung Probleme mit der DNS-Auflösung auftreten, stellen Sie sicher, dass die im Schritt zum **Generieren der Schlüssel** im Portal erstellten Azure Migrate Ressourcen über den lokalen Server erreichbar sind, auf dem die Azure Migrate-Appliance gehostet wird. [Erfahren Sie mehr über das Überprüfen der Netzwerkkonnektivität.](./troubleshoot-network-connectivity.md)

### <a name="assess-your-servers-for-migration-to-azure"></a>Bewerten von Servern für die Migration zu Azure
Bewerten Sie im Anschluss an die Ermittlung Ihre Server ([VMware-VMs](./tutorial-assess-vmware-azure-vm.md), [Hyper-V-VMs](./tutorial-assess-hyper-v.md), [physische Server](./tutorial-assess-vmware-azure-vm.md), [AWS-VMs](./tutorial-assess-aws.md), [GCP-VMs](./tutorial-assess-gcp.md)) auf eine Migration zu Azure-VMs oder zu Azure VMware Solution (AVS). Dazu verwenden Sie das Tool Azure Migrate: Ermittlung und Bewertung.

Sie können mit dem Tool Azure Migrate: Ermittlung und Bewertung [auch Ihre lokalen Computer bewerten](./tutorial-discover-import.md#prepare-the-csv), indem Sie eine importierte CSV-Datei (Comma-Separated Values) verwenden.   

## <a name="migrate-servers-to-azure-using-azure-private-link"></a>Migrieren von Servern zu Azure mithilfe von Azure Private Link

In den folgenden Abschnitten werden die Schritte beschrieben, die zum Verwenden von Azure Migrate mit [privaten Endpunkten](../private-link/private-endpoint-overview.md) für Migrationsvorgänge mit dem private ExpressRoute-Peering oder über VPN-Verbindungen erforderlich sind.  

Dieser Artikel zeigt einen Proof of Concept-Bereitstellungspfad für Agent-basierte Replikationen zum Migrieren Ihrer [VMware-VMs](./tutorial-migrate-vmware-agent.md), [Hyper-V-VMs](./tutorial-migrate-physical-virtual-machines.md), [physischen Server](./tutorial-migrate-physical-virtual-machines.md), [VMs bei AWS](./tutorial-migrate-aws-virtual-machines.md), [VMs bei GCP](./tutorial-migrate-gcp-virtual-machines.md) oder VMs, die bei einem anderen Virtualisierungsanbieter ausgeführt werden, mithilfe privater Azure-Endpunkte. Einen ähnlichen Ansatz können Sie auch für [Hyper-V-Migrationsvorgänge ohne Agent](./tutorial-migrate-hyper-v.md) mit Private Link verwenden.

>[!Note]
>[VMware-Migrationsvorgänge ohne Agent](./tutorial-assess-physical.md) erfordern Internetzugriff oder Konnektivität über ExpressRoute-Peering mit Microsoft.

### <a name="set-up-a-replication-appliance-for-migration"></a>Einrichten einer Replikationsappliance für die Migration

Das folgende Diagramm veranschaulicht den Workflow bei der Agent-basierten Replikation mit privaten Endpunkten mithilfe des Tools Azure Migrate: Servermigration.  

![Replikationsarchitektur](./media/how-to-use-azure-migrate-with-private-endpoints/replication-architecture.png)

Das Tool verwendet eine Replikationsappliance, um Ihre Server in Azure zu replizieren. In diesem Artikel wird beschrieben, wie Sie [einen Computer für die Replikationsappliance vorbereiten und einrichten](./tutorial-migrate-physical-virtual-machines.md#prepare-a-machine-for-the-replication-appliance).

Nachdem Sie die Replikationsappliance eingerichtet haben, erstellen Sie anhand der folgenden Anweisungen die erforderlichen Ressourcen für die Migration.

1. Klicken Sie unter **Computer ermitteln** > **Sind Ihre Computer virtualisiert?** auf **Nicht virtualisiert/Andere**.
2. Wählen Sie unter **Zielregion** die Azure-Region aus, zu der Sie die Computer migrieren möchten, und bestätigen Sie Ihre Auswahl.
3. Wählen Sie **Ressourcen generieren** aus, um die erforderlichen Azure-Ressourcen zu erstellen. Schließen Sie die Seite nicht, während die Ressourcen erstellt werden.   
    - Hierbei wird ein Recovery Services-Tresor im Hintergrund erstellt und eine verwaltete Identität für den Tresor aktiviert. Ein Recovery Services-Tresor ist eine Entität, die die Replikationsinformationen von Servern enthält und zum Initiieren von Site Recovery-Vorgängen verwendet wird.  
    - Wenn das Azure Migrate-Projekt über Konnektivität mit privaten Endpunkten verfügt, wird ein privater Endpunkt für den Recovery Services-Tresor erstellt. Dabei werden dem privaten Endpunkt fünf vollqualifizierte private Namen (FQDNs), einer für jeden Microservice, hinzugefügt, die mit dem Recovery Services-Tresor verknüpft sind.   
    - Die fünf Domänennamen werden nach folgendem Muster formatiert: <br/> _{Tresor-ID}-asr-pod01-{Typ}-.{Zielregionscode}_ .privatelink.siterecovery.windowsazure.com  
    - Standardmäßig erstellt Azure Migrate automatisch eine private DNS-Zone und fügt DNS-A-Datensätze für die Microservices des Recovery Services-Tresors hinzu. Die private DNS-Zone ist mit dem virtuellen Netzwerk des privaten Endpunkts verknüpft und ermöglicht es der lokalen Replikationsappliance, die vollqualifizierten Domänennamen in ihre privaten IP-Adressen aufzulösen.

4. Stellen Sie vor dem Registrieren der Replikationsappliance sicher, dass die FQDNs für private Links des Tresors über den Computer erreichbar sind, auf dem die Replikationsappliance gehostet wird. [Erfahren Sie mehr über das Überprüfen der Netzwerkkonnektivität.](./troubleshoot-network-connectivity.md)

5. Nachdem Sie die Konnektivität überprüft haben, laden Sie die Setup- und Schlüsseldatei der Appliance herunter, führen Sie den Installationsvorgang aus, und registrieren Sie die Appliance bei Azure Migrate. Sehen Sie sich [hier](./tutorial-migrate-physical-virtual-machines.md#set-up-the-replication-appliance) die ausführlichen Schritte an. Nachdem Sie die Replikationsappliance eingerichtet haben, befolgen Sie [diese Anweisungen](./tutorial-migrate-physical-virtual-machines.md#install-the-mobility-service), um den Mobilitätsdienst auf den Computern zu installieren, die Sie migrieren möchten.

### <a name="replicate-servers-to-azure-using-azure-private-link"></a>Replizieren von Servern zu Azure mithilfe von Azure Private Link

Führen Sie [diese Schritte](./tutorial-migrate-physical-virtual-machines.md#replicate-machines) aus, um Server für die Replikation auszuwählen.  

Verwenden Sie das Dropdownmenü unter **Replizieren** > **Zieleinstellungen** > **Cache/Replication storage account** (Cache-/Replikationsspeicherkonto), um ein Speicherkonto auszuwählen, das über eine private Verbindung repliziert werden soll.  

Wenn Ihr Azure Migrate-Projekt über private Endpunkte verbunden ist, müssen Sie [der verwalteten Identität des Recovery Services-Tresors Berechtigungen gewähren](#grant-access-permissions-to-the-recovery-services-vault), damit sie auf das für Azure Migrate erforderliche Speicherkonto zugreifen kann.   

[Erstellen Sie außerdem einen privaten Endpunkt für das Speicherkonto](#create-a-private-endpoint-for-the-storage-account-optional), um Replikationen über eine private Verbindung zu ermöglichen.

#### <a name="grant-access-permissions-to-the-recovery-services-vault"></a>Gewähren von Zugriffsberechtigungen für den Recovery Services-Tresor

Sie müssen dem Recovery Services-Tresor die Berechtigungen für den authentifizierten Zugriff auf das Cache-/Replikationsspeicherkonto gewähren.

Ermitteln Sie anhand der folgenden Schritte den Recovery Services-Tresor, der von Azure Migrate erstellt wurde, und gewähren Sie die erforderlichen Berechtigungen.

**_Ermitteln des Recovery Services-Tresors und der Objekt-ID der verwalteten Identität_**

Die Details zum Recovery Services-Tresor finden Sie auf der **Eigenschaftenseite** des Tools Azure Migrate: Servermigration.

1. Navigieren Sie zum **Azure Migrate-Hub**, und wählen Sie auf der Kachel für Azure Migrate: Servermigration **Übersicht** aus.

    ![Übersichtsseite im Azure Migrate-Hub](./media/how-to-use-azure-migrate-with-private-endpoints/hub-overview.png)

2. Wählen Sie im linken Bereich **Eigenschaften** aus. Notieren Sie sich den Namen des Recovery Services-Tresors und die ID der verwalteten Identität. Für den Tresor sind _Privater Endpunkt_ als **Konnektivitätstyp** und _Andere_ als **Replikationstyp** festgelegt. Sie benötigen diese Informationen, wenn Sie Zugriff auf den Tresor gewähren.

    ![Eigenschaftenseite von Azure Migrate: Servermigration](./media/how-to-use-azure-migrate-with-private-endpoints/vault-info.png)

**_Berechtigungen für den Zugriff auf das Speicherkonto_**

 Der verwalteten Identität des Tresors müssen die folgenden Rollenberechtigungen für das Speicherkonto gewährt werden, das für die Replikation erforderlich ist.  In diesem Fall müssen Sie das Speicherkonto im Voraus erstellen.

>[!Note]
> Für die Migration von Hyper-V-VMs zu Azure mithilfe von Private Link müssen Sie Zugriff auf das Replikationsspeicherkonto und das Cachespeicherkonto gewähren.

Die Rollenberechtigungen für Resource Manager variieren je nach Typ des Speicherkontos.

|**Speicherkontotyp** | **Rollen/Berechtigungen**|
|--- | ---|
|Standardtyp | [Mitwirkender](../role-based-access-control/built-in-roles.md#contributor)<br>[Mitwirkender an Speicherblobdaten](../role-based-access-control/built-in-roles.md#storage-blob-data-contributor)|
|Premium-Typ | [Mitwirkender](../role-based-access-control/built-in-roles.md#contributor)<br>[Besitzer von Speicherblobdaten](../role-based-access-control/built-in-roles.md#storage-blob-data-owner)

1. Wechseln Sie zu dem Replikations-/Cachespeicherkonto, das für die Replikation ausgewählt wurde. Klicken Sie im linken Bereich auf **Zugriffssteuerung (IAM)** .

1. Klicken Sie im Bereich **Rollenzuweisung hinzufügen** auf **Hinzufügen**:

   ![Hinzufügen einer Rollenzuweisung](./media/how-to-use-azure-migrate-with-private-endpoints/storage-role-assignment.png)


1. Wählen Sie auf der Seite **Rollenzuweisung hinzufügen** im Feld **Rolle** die passende Rolle aus der oben genannten Berechtigungsliste aus. Geben Sie den zuvor notierten Tresornamen ein, und wählen Sie **Speichern** aus.

    ![Bereitstellen von rollenbasiertem Zugriff](./media/how-to-use-azure-migrate-with-private-endpoints/storage-role-assignment-select-role.png)

4. Zusätzlich zu diesen Berechtigungen müssen Sie auch den Zugriff auf vertrauenswürdige Microsoft-Dienste zulassen. Wenn Ihr Netzwerkzugriff auf ausgewählte Netzwerke beschränkt ist, wählen Sie auf der Registerkarte **Netzwerk** im Abschnitt **Ausnahmen** die Option **Vertrauenswürdigen Microsoft-Diensten den Zugriff auf dieses Speicherkonto erlauben** aus.

![Zulassen vertrauenswürdiger Microsoft-Dienste für das Speicherkonto](./media/how-to-use-azure-migrate-with-private-endpoints/exceptions.png)


### <a name="create-a-private-endpoint-for-the-storage-account-optional"></a>Erstellen eines privaten Endpunkts für das Speicherkonto (optional)

[Erstellen Sie einen privaten Endpunkt](../private-link/tutorial-private-endpoint-storage-portal.md#create-storage-account-with-a-private-endpoint) für die Cache-/Replikationsspeicherkonten, um die Replikation mithilfe von ExpressRoute mit privatem Peering zu ermöglichen (Zielunterressource: **_Blob_**).

>[!Note]
>
> - Sie können private Endpunkte nur für ein Speicherkonto des Typs „Universell v2 (GPv2)“ erstellen. Preisinformationen finden Sie unter [Azure-Seitenblobs: Preise](https://azure.microsoft.com/pricing/details/storage/page-blobs/) und [Azure Private Link – Preise](https://azure.microsoft.com/pricing/details/private-link/).

Erstellen Sie den privaten Endpunkt für das Speicherkonto im selben virtuellen Netzwerk wie der private Endpunkt des Azure Migrate-Projekts oder in einem anderen, mit diesem Netzwerk verbundenen virtuellen Netzwerk.

Wählen Sie **Ja** aus, und führen Sie die Integration mit einer privaten DNS-Zone aus. Die private DNS-Zone hilft beim Weiterleiten der Verbindungen aus dem virtuellen Netzwerk an das Speicherkonto über eine private Verbindung. Wenn Sie **Ja** auswählen, wird die DNS-Zone automatisch mit dem virtuellen Netzwerk verknüpft, und es werden die DNS-Einträge für die Auflösung neu erstellter IP-Adressen und vollqualifizierter Domänennamen hinzugefügt. Informieren Sie sich weiter über [private DNS-Zonen](../dns/private-dns-overview.md).

Wenn der Benutzer, der den privaten Endpunkt erstellt, auch der Besitzer des Speicherkontos ist, wird das Erstellen des privaten Endpunkts automatisch genehmigt. Andernfalls muss der Besitzer des Speicherkontos den privaten Endpunkt für die Verwendung genehmigen. Navigieren Sie auf der Seite des Speicherkontos unter **Netzwerk** zu **Verbindungen mit privatem Endpunkt**, um eine angeforderte Verbindung mit einem privaten Endpunkt zu genehmigen oder abzulehnen.

Überprüfen Sie vor dem Fortfahren den Verbindungsstatus des privaten Endpunkts.

![Genehmigungsstatus privater Endpunkte](./media/how-to-use-azure-migrate-with-private-endpoints/private-endpoint-connection-state.png)

Nachdem Sie den privaten Endpunkt erstellt haben, wählen Sie über die Dropdownliste **Replizieren** > **Zieleinstellungen** > **Cachespeicherkonto** das Speicherkonto für die Replikation über eine private Verbindung aus.  

Stellen Sie sicher, dass die lokale Replikationsappliance über Netzwerkkonnektivität mit dem Speicherkonto an dessen privaten Endpunkt verfügt. [Erfahren Sie mehr über das Überprüfen der Netzwerkkonnektivität.](./troubleshoot-network-connectivity.md)

>[!Note]
>
> - Wenn bei Hyper-V-VM-Migrationsvorgängen zu Azure das Replikationsspeicherkonto den Typ _Premium_ aufweist, müssen Sie ein anderes Speicherkonto vom Typ _Standard_ für das Cachespeicherkonto auswählen. In diesem Fall müssen Sie private Endpunkte sowohl für das Replikations- als auch für das Cachespeicherkonto erstellen.  

Befolgen Sie als Nächstes die Anweisungen zum [Überprüfen und Starten der Replikation](./tutorial-migrate-physical-virtual-machines.md#replicate-machines) und zum [Durchführen der Migration](./tutorial-migrate-physical-virtual-machines.md#run-a-test-migration).


## <a name="next-steps"></a>Nächste Schritte
- [Schließen Sie den Migrationsprozess ab](./tutorial-migrate-physical-virtual-machines.md#complete-the-migration), und überprüfen Sie die [bewährten Methoden nach der Migration](./tutorial-migrate-physical-virtual-machines.md#post-migration-best-practices).
