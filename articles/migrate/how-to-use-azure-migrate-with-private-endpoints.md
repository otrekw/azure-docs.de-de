---
title: Verwenden von Azure Migrate mit privaten Endpunkten
description: Verwenden Sie Azure Migrate mit Private Link-Unterstützung für das Entdecken, Bewerten und Migrieren.
author: deseelam
ms.author: deseelam
ms.manager: bsiva
ms.topic: how-to
ms.date: 04/07/2020
ms.openlocfilehash: 97d4f0a387b75c9b23f64992a8ef39bc0bad17f0
ms.sourcegitcommit: 3ed0f0b1b66a741399dc59df2285546c66d1df38
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/19/2021
ms.locfileid: "107715213"
---
# <a name="using-azure-migrate-with-private-endpoints"></a>Verwenden von Azure Migrate mit privaten Endpunkten  

In diesem Artikel wird beschrieben, wie Sie Azure Migrate verwenden, um Server über ein privates Netzwerk mithilfe von [Azure Private Link](https://docs.microsoft.com/azure/private-link/private-endpoint-overview) zu entdecken, zu bewerten und zu migrieren. 

Sie können die Tools [Azure Migrate: Ermittlung und Bewertung](https://docs.microsoft.com/azure/migrate/migrate-services-overview#azure-migrate-discovery-and-assessment-tool) und [Azure Migrate: Servermigration](https://docs.microsoft.com/azure/migrate/migrate-services-overview#azure-migrate-server-migration-tool) verwenden, um eine private und sichere Verbindung mit dem Azure Migrate-Dienst über ein privates ExpressRoute-Peering oder eine Site-to-Site-VPN-Verbindung mithilfe von Azure Private Link herzustellen. 

Die Verbindungsherstellung über private Endpunkte wird empfohlen, wenn die Organisationsanforderungen den Zugriff auf den Azure Migrate-Dienst und andere Azure-Ressourcen über öffentliche Netzwerke verbieten. Sie können die Private Link-Unterstützung auch für vorhandene ExpressRoute-Leitungen zum privaten Peering nutzen, um höhere Bandbreiten- oder Latenzanforderungen zu erfüllen. 

## <a name="support-requirements"></a>Supportanforderungen 

### <a name="required-permissions"></a>Erforderliche Berechtigungen

Berechtigungen **Mitwirkender und Benutzerzugriffsadministrator** oder **Besitzer** für das Abonnement 

### <a name="supported-scenarios-and-tools"></a>Unterstützte Szenarien und Tools

**Bereitstellung** | **Details** | **Extras** 
--- | --- | ---
**Ermittlung und Bewertung** | Ausführen von Ermittlung und Bewertung Ihrer Server ohne Agent im großen Stil und auf beliebigen Plattformen – Hypervisorplattformen wie [VMware vSphere](https://docs.microsoft.com/azure/migrate/tutorial-discover-vmware) oder [Microsoft Hyper-V](https://docs.microsoft.com/azure/migrate/tutorial-discover-hyper-v), öffentliche Clouds wie [AWS](https://docs.microsoft.com/azure/migrate/tutorial-discover-aws) oder [GCP](https://docs.microsoft.com/azure/migrate/tutorial-discover-gcp) und sogar [Bare-Metal-Server](https://docs.microsoft.com/azure/migrate/tutorial-discover-physical) | Azure Migrate: Ermittlung und Bewertung  <br/> 
**Softwareinventur** | Entdecken von Apps, Rollen und Features, die auf VMware-VMs ausgeführt werden | Azure Migrate: Ermittlung und Bewertung  
**Visualisierung von Abhängigkeiten** | Funktionen zur Abhängigkeitsanalyse zum serverübergreifenden Ermitteln und Verstehen von Abhängigkeiten. <br/> Die [Visualisierung von Abhängigkeiten ohne Agent](https://docs.microsoft.com/azure/migrate/how-to-create-group-machine-dependencies-agentless) wird nativ durch die Private Link-Unterstützung von Azure Migrate unterstützt. <br/>Für die [Agent-basierte Visualisierung von Abhängigkeiten](https://docs.microsoft.com/azure/migrate/how-to-create-group-machine-dependencies) ist eine Internetverbindung erforderlich. [Erfahren Sie](https://docs.microsoft.com/azure/azure-monitor/logs/private-link-security), wie Sie private Endpunkte für die Agent-basierte Visualisierung von Abhängigkeiten verwenden. | Azure Migrate: Ermittlung und Bewertung |
**Migration** | Ausführen von [Hyper-V-Migrationsvorgängen ohne Agent](https://docs.microsoft.com/azure/migrate/tutorial-migrate-hyper-v) oder Verwenden des Agent-basierten Ansatzes zum Migrieren von [VMware-VMs](./tutorial-migrate-vmware-agent.md), [Hyper-V-VMs](./tutorial-migrate-physical-virtual-machines.md), [physischen Servern](./tutorial-migrate-physical-virtual-machines.md), [VMs bei AWS](./tutorial-migrate-aws-virtual-machines.md), [VMs bei GCP](https://docs.microsoft.com/azure/migrate/tutorial-migrate-gcp-virtual-machines) oder bei einem anderen Virtualisierungsanbieter ausgeführten VMs | Azure Migrate: Servermigration
 
>[!Note]
>
> [VMware-Migrationsvorgänge ohne Agent](https://docs.microsoft.com/azure/migrate/tutorial-migrate-vmware) erfordern Internetzugriff oder Konnektivität über ExpressRoute-Peering mit Microsoft. <br/> [Erfahren Sie](https://docs.microsoft.com/azure/migrate/replicate-using-expressroute), wie Sie mithilfe von privaten Endpunkten Replikationen über das private ExpressRoute-Peering oder eine Site-to-Site-VPN-Verbindung (S2S) durchführen.  <br/><br/> 
   
#### <a name="other-integrated-tools"></a>Weitere integrierte Tools

Einige Migrationstools können möglicherweise keine Nutzungsdaten in das Azure Migrate-Projekt hochladen, wenn der Zugriff auf das öffentliche Netzwerk deaktiviert ist. Das Azure Migrate-Projekt sollte so konfiguriert werden, dass Datenverkehr aus allen Netzwerken zugelassen wird, sodass Daten von anderen Microsoft-Angeboten oder Angeboten von externen [unabhängigen Softwareanbietern](https://docs.microsoft.com/azure/migrate/migrate-services-overview#isv-integration) (Independent Software Vendor, ISV) empfangen werden können. 


Um den Zugriff über das öffentliche Netzwerk für ein Azure Migrate-Projekt zu aktivieren, wechseln Sie im Azure-Portal zur **Eigenschaftenseite** von Azure Migrate und wählen **Nein** und dann **Speichern** aus.

![Diagramm der Änderung des Netzwerkzugriffsmodus](./media/how-to-use-azure-migrate-with-private-endpoints/migration-project-properties.png)

### <a name="other-considerations"></a>Weitere Überlegungen   

**Überlegungen** | **Details**
--- | --- 
**Preise** | Preisinformationen finden Sie unter [Azure-Seitenblobs: Preise](https://azure.microsoft.com/pricing/details/storage/page-blobs/) und [Azure Private Link – Preise](https://azure.microsoft.com/pricing/details/private-link/).  
**Anforderungen für virtuelle Netzwerke** | Der Endpunkt für ExpressRoute bzw. das VPN-Gateway sollte sich im ausgewählten virtuellen Netzwerk oder in einem mit diesem verbundenen virtuellen Netzwerk befinden. Sie benötigen bis zu ca. 15 IP-Adressen im virtuellen Netzwerk.  

## <a name="create-a-project-with-private-endpoint-connectivity"></a>Erstellen eines Projekts mit Konnektivität zu einem privaten Endpunkt

Richten Sie ein neues Azure Migrate-Projekt nach diesem [Artikel](https://docs.microsoft.com/azure/migrate/create-manage-projects#create-a-project-for-the-first-time) ein. 

> [!Note]
> Sie können die Konnektivitätsmethode für vorhandene Azure Migrate-Projekte nicht in Konnektivität über private Endpunkte ändern.

Geben Sie im Abschnitt **Erweiterte Konfiguration** die folgenden Details an, um einen privaten Endpunkt für Ihr Azure Migrate-Projekt zu erstellen.
- Wählen Sie als **Verbindungsmethode** die Option **Privater Endpunkt** aus. 
- Behalten Sie unter **Disable public endpoint access** (Zugriff auf öffentlichen Endpunkt deaktivieren) die Standardeinstellung **Nein** bei. Einige Migrationstools können möglicherweise keine Nutzungsdaten in das Azure Migrate-Projekt hochladen, wenn der Zugriff auf das öffentliche Netzwerk deaktiviert ist. [Weitere Informationen.](#other-integrated-tools)
- Wählen Sie unter **VNET-Abonnement** das Abonnement für das virtuelle Netzwerk des privaten Endpunkts aus. 
- Wählen Sie unter **Virtuelles Netzwerk** das virtuelle Netzwerk für den privaten Endpunkt aus. Die Azure Migrate-Appliance und andere Softwarekomponenten, die eine Verbindung mit dem Azure Migrate-Projekt herstellen sollen, müssen sich in diesem oder in einem verbundenen virtuellen Netzwerk befinden.
- Wählen Sie unter **Subnetz** das Subnetz für den privaten Endpunkt aus. 

Klicken Sie auf **Erstellen**. Warten Sie einige Minuten, bis das Azure Migrate-Projekt bereitgestellt wurde. Schließen Sie diese Seite nicht, während das Projekt erstellt wird.

![Projekt erstellen](./media/how-to-use-azure-migrate-with-private-endpoints/create-project.png)

    
Damit wird ein Migrationsprojekt erstellt und ein privater Endpunkt daran angefügt. 

## <a name="discover-and-assess-servers-for-migration-using-azure-private-link"></a>Ermitteln und Bewerten von Servern für die Migration mithilfe von Azure Private Link 

### <a name="set-up-the-azure-migrate-appliance"></a>Einrichten der Azure Migrate-Appliance 

1. Wählen Sie unter **Computer ermitteln** > **Sind Ihre Computer virtualisiert?** den Servertyp aus.
2. Geben Sie unter **Azure Migrate-Projektschlüssel generieren** einen Namen für die Azure Migrate-Appliance an. 
3. Wählen Sie **Schlüssel generieren** aus, um die erforderlichen Azure-Ressourcen zu erstellen. 

    > [!Important]
    > Schließen Sie die Seite „Computer ermitteln“ nicht, während die Ressourcen erstellt werden.  
    - In diesem Schritt erstellt Azure Migrate einen Schlüsseltresor, ein Speicherkonto, einen Recovery Services-Tresor (nur bei VMware-Migrationsvorgängen ohne Agent) und einige interne Ressourcen und fügt an jede Ressource einen privaten Endpunkt an. Die privaten Endpunkte werden in dem virtuellen Netzwerk erstellt, das während der Projekterstellung ausgewählt wurde.  
    - Nachdem die privaten Endpunkte erstellt wurden, werden die DNS CNAME-Ressourceneinträge für die Azure Migrate-Ressourcen mit einem Alias in einer Unterdomäne mit dem Präfix „privatelink“ aktualisiert. Standardmäßig erstellt Azure Migrate auch für jede Unterdomäne „privatelink“ der einzelnen Ressourcentypen eine private DNS-Zone, und fügt DNS-A-Datensätze für die zugeordneten privaten Endpunkte ein. Dadurch können die Azure Migrate-Appliance und andere Softwarekomponenten im Quellnetzwerk Endpunkte von Azure Migrate-Ressourcen über deren private IP-Adressen erreichen.  
    - Azure Migrate aktiviert auch eine [verwaltete Identität](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) für das Migrationsprojekt und gewährt der verwalteten Identität Berechtigungen für den sicheren Zugriff auf das Speicherkonto.  

4. Nachdem der Schlüssel erfolgreich generiert wurde, kopieren Sie die Schlüsseldetails, um die Appliance zu konfigurieren und zu registrieren.   

#### <a name="download-the-appliance-installer-file"></a>Herunterladen der Installationsdatei für die Appliance

Für das Azure Migrate-Tool zur Ermittlung und Bewertung wird eine einfache Azure Migrate-Appliance verwendet. Die Appliance führt die Serverermittlung durch und sendet Metadaten zur Serverkonfiguration und -leistung an Azure Migrate.

Zum Einrichten der Appliance laden Sie die ZIP-Datei mit dem Installationsskript aus dem Portal herunter. Kopieren Sie die gezippte Datei auf den Server, der die Appliance hostet.

Stellen Sie sicher, dass der Server die [Hardwareanforderungen](https://docs.microsoft.com/azure/migrate/migrate-appliance) für das ausgewählte Szenario erfüllt (VMware/Hyper-V/physisch usw.) und eine Verbindung mit den erforderlichen Azure-URLs für [öffentliche](./migrate-appliance.md#public-cloud-urls-for-private-link-connectivity) Clouds und [Government-Clouds](./migrate-appliance.md#government-cloud-urls-for-private-link-connectivity) herstellen kann.

Überprüfen Sie nach dem Herunterladen der ZIP-Datei die Dateisicherheit, und führen Sie das Installationsskript aus, um die Appliance bereitzustellen.

#### <a name="verify-file-security"></a>Überprüfen der Dateisicherheit

Vergewissern Sie sich vor der Bereitstellung, dass die gezippte Datei sicher ist.

1. Öffnen Sie auf dem Server, auf den Sie die Datei heruntergeladen haben, ein Administratorbefehlsfenster. 
2. Führen Sie den folgenden Befehl aus, um den Hash für die gezippte Datei zu generieren:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Beispiel: ```C:\>CertUtil -HashFile C:\Users\administrator\Desktop\AzureMigrateInstaller-VMware-PrivateEndPoint.zip SHA256```
3. Überprüfen Sie die neueste Geräteversion und das Skript für die öffentliche Azure-Cloud:

    **Algorithmus** | **Download** | **SHA256**
    --- | --- | ---
    VMware (85,8 MB) | [Aktuelle Version](https://go.microsoft.com/fwlink/?linkid=2116601) | 85b74d93dfcee43412386141808d82147916330e6669df94c7969fe1b3d0fe72
    Hyper-V (85,8 MB) | [Aktuelle Version](https://go.microsoft.com/fwlink/?linkid=2116601) | 85b74d93dfcee43412386141808d82147916330e6669df94c7969fe1b3d0fe72
    Physisch oder andere (85,8 MB) | [Aktuelle Version](https://go.microsoft.com/fwlink/?linkid=2116601) | 85b74d93dfcee43412386141808d82147916330e6669df94c7969fe1b3d0fe72

#### <a name="run-the-script"></a>Führen Sie das Skript aus.

1. Extrahieren Sie die gezippte Datei in einem Ordner auf dem Server, der die Appliance hostet. 
2. Starten Sie PowerShell auf dem Computer mit Administratorberechtigungen (erhöhten Rechten).
3. Ändern Sie das PowerShell-Verzeichnis in den Ordner mit den Inhalten, die aus der gezippten Datei extrahiert wurden, die Sie heruntergeladen haben.
4. Führen Sie das Skript **AzureMigrateInstaller.ps1** wie folgt aus:

    ``` PS C:\Users\administrator\Desktop\AzureMigrateInstaller-Server-Public> .\AzureMigrateInstaller.ps1```
   
5. Nachdem das Skript erfolgreich ausgeführt wurde, wird der Konfigurations-Manager für die Appliance gestartet, damit Sie sie einrichten können. Bei Problemen sehen Sie sich die Skriptprotokolle in diesem Pfad an: C:\ProgramData\Microsoft Azure\Logs\AzureMigrateScenarioInstaller_<em>Zeitstempel</em>.log.

### <a name="configure-the-appliance-and-start-continuous-discovery"></a>Konfigurieren der Appliance und Starten der kontinuierlichen Ermittlung

Öffnen Sie in einem Browser auf einem beliebigen Computer, der eine Verbindung mit dem Server auf der Appliance herstellen kann, und öffnen Sie die URL des Konfigurations-Managers der Appliance: `https://appliance name or IP address: 44368`. Alternativ können Sie den Konfigurations-Manager auch auf dem Desktop des Servers auf der Appliance öffnen, indem Sie die Verknüpfung für den Konfigurations-Manager auswählen.

#### <a name="set-up-prerequisites"></a>Einrichten erforderlicher Komponenten

1. Lesen Sie die Drittanbieterinformationen, und akzeptieren Sie die **Lizenzbedingungen**.    
 
2. Navigieren Sie im Konfigurations-Manager zu **Erforderliche Komponenten einrichten**, und führen Sie die folgenden Schritte aus:
   - **Konnektivität:** Die Appliance überprüft den Zugriff auf die erforderlichen URLs. Wenn der Server einen Proxy verwendet:
     - Wählen Sie **Proxy einrichten** aus, um die Proxyadresse `http://ProxyIPAddress` oder `http://ProxyFQDN` und den lauschenden Port anzugeben.
     - Geben Sie die Anmeldeinformationen an, wenn der Proxy eine Authentifizierung erfordert. Es werden nur HTTP-Proxys unterstützt.
     - Wenn Sie möchten, können Sie eine Liste von URLs/IP-Adressen hinzufügen, die den Proxyserver umgehen sollen. Wenn Sie privates ExpressRoute-Peering verwenden, stellen Sie sicher, dass [diese URLs](https://docs.microsoft.com/azure/migrate/replicate-using-expressroute#configure-proxy-bypass-rules-on-the-azure-migrate-appliance-for-vmware-agentless-migrations) umgangen werden.
     - Wenn Sie Proxyserverdetails geändert oder URLs/IP-Adressen für die Proxyumgehung hinzugefügt haben, müssen Sie **Speichern** auswählen, damit die Konfiguration registriert wird.
     
        > [!Note]
        > Wenn Sie während der Konnektivitätsprüfung einen Fehler mit dem Link aka.ms/* erhalten und nicht möchten, dass die Appliance über das Internet auf diese URL zugreift, müssen Sie den Dienst für automatische Updates auf der Appliance deaktivieren. Befolgen Sie dazu die [**hier**](https://docs.microsoft.com/azure/migrate/migrate-appliance#turn-off-auto-update) beschriebenen Schritte. Nachdem automatische Updates deaktiviert wurden, wird die URL-Konnektivitätsprüfung über aka.ms/* übersprungen. 

   - **Uhrzeitsynchronisierung**: Die Uhrzeit der Appliance muss mit der Internetzeit synchronisiert werden, damit die Ermittlung ordnungsgemäß funktioniert.
   - **Updates installieren**: Die Appliance stellt sicher, dass die neuesten Updates installiert sind. Nachdem die Überprüfung abgeschlossen ist, können Sie **Appliancedienste anzeigen** auswählen, um den Status und die Versionen der auf dem Applianceserver ausgeführten Dienste anzuzeigen.
        > [!Note]
        > Wenn Sie sich dafür entschieden haben, automatische Updates auf der Appliance zu deaktivieren, können Sie die Appliancedienste manuell aktualisieren, um die neuesten Versionen der Dienste abzurufen. Befolgen Sie dazu die [**hier**](https://docs.microsoft.com/azure/migrate/migrate-appliance#manually-update-an-older-version) beschriebenen Schritte.
   - **Install VDDK** (VDDK installieren) (_nur bei VMware-Appliances erforderlich_): Die Appliance überprüft, ob das VMware vSphere-VDDK (Virtual Disk Development Kit) installiert ist. Laden Sie bei Bedarf VDDK 6.7 von VMware herunter, und extrahieren Sie den Inhalt der heruntergeladenen ZIP-Datei am angegebenen Ort auf der Appliance, wie unter **Installationsanweisungen** beschrieben.

#### <a name="register-the-appliance-and-start-continuous-discovery"></a>Registrieren der Appliance und Starten der kontinuierlichen Ermittlung

Führen Sie nach der Überprüfung der Voraussetzungen die folgenden Schritte aus, um die Appliance zu registrieren und die kontinuierliche Ermittlung für die jeweiligen Szenarien zu starten: [VMware-VMs](https://docs.microsoft.com/azure/migrate/tutorial-discover-vmware#register-the-appliance-with-azure-migrate), [Hyper-V-VMs](https://docs.microsoft.com/azure/migrate/tutorial-discover-hyper-v#register-the-appliance-with-azure-migrate), [physische Server](https://docs.microsoft.com/azure/migrate/tutorial-discover-physical#register-the-appliance-with-azure-migrate), [AWS-VMs](https://docs.microsoft.com/azure/migrate/tutorial-discover-aws#register-the-appliance-with-azure-migrate), [GCP-VMs](https://docs.microsoft.com/azure/migrate/tutorial-discover-gcp#register-the-appliance-with-azure-migrate).


>[!Note]
> Wenn bei der Registrierung der Appliance oder beim Starten der Ermittlung Probleme mit der DNS-Auflösung auftreten, stellen Sie sicher, dass die im Schritt zum **Generieren der Schlüssel** im Portal erstellten Azure Migrate Ressourcen über den lokalen Server erreichbar sind, auf dem die Azure Migrate-Appliance gehostet wird. [Erfahren Sie mehr über das Überprüfen der Netzwerkkonnektivität.](#troubleshoot-network-connectivity)

### <a name="assess-your-servers-for-migration-to-azure"></a>Bewerten von Servern für die Migration zu Azure
Bewerten Sie im Anschluss an die Ermittlung Ihre Server ([VMware-VMs](https://docs.microsoft.com/azure/migrate/tutorial-assess-vmware-azure-vm), [Hyper-V-VMs](https://docs.microsoft.com/azure/migrate/tutorial-assess-hyper-v), [physische Server](https://docs.microsoft.com/azure/migrate/tutorial-assess-vmware-azure-vm), [AWS-VMs](https://docs.microsoft.com/azure/migrate/tutorial-assess-aws), [GCP-VMs](https://docs.microsoft.com/azure/migrate/tutorial-assess-gcp)) auf eine Migration zu Azure-VMs oder zu Azure VMware Solution (AVS). Dazu verwenden Sie das Tool Azure Migrate: Ermittlung und Bewertung. 

Sie können mit dem Tool Azure Migrate: Ermittlung und Bewertung [auch Ihre lokalen Computer bewerten](https://docs.microsoft.com/azure/migrate/tutorial-discover-import#prepare-the-csv), indem Sie eine importierte CSV-Datei (Comma-Separated Values) verwenden.   

## <a name="migrate-servers-to-azure-using-azure-private-link"></a>Migrieren von Servern zu Azure mithilfe von Azure Private Link

In den folgenden Abschnitten werden die Schritte beschrieben, die zum Verwenden von Azure Migrate mit [privaten Endpunkten](https://docs.microsoft.com/azure/private-link/private-endpoint-overview) für Migrationsvorgänge mit dem private ExpressRoute-Peering oder über VPN-Verbindungen erforderlich sind.  

Dieser Artikel zeigt einen Proof of Concept-Bereitstellungspfad für Agent-basierte Replikationen zum Migrieren Ihrer [VMware-VMs](./tutorial-migrate-vmware-agent.md), [Hyper-V-VMs](./tutorial-migrate-physical-virtual-machines.md), [physischen Server](./tutorial-migrate-physical-virtual-machines.md), [VMs bei AWS](./tutorial-migrate-aws-virtual-machines.md), [VMs bei GCP](https://docs.microsoft.com/azure/migrate/tutorial-migrate-gcp-virtual-machines) oder VMs, die bei einem anderen Virtualisierungsanbieter ausgeführt werden, mithilfe privater Azure-Endpunkte. Einen ähnlichen Ansatz können Sie auch für [Hyper-V-Migrationsvorgänge ohne Agent](https://docs.microsoft.com/azure/migrate/tutorial-migrate-hyper-v) mit Private Link verwenden.

>[!Note]
>[VMware-Migrationsvorgänge ohne Agent](https://docs.microsoft.com/azure/migrate/tutorial-assess-physical) erfordern Internetzugriff oder Konnektivität über ExpressRoute-Peering mit Microsoft. 

### <a name="set-up-a-replication-appliance-for-migration"></a>Einrichten einer Replikationsappliance für die Migration 

Das folgende Diagramm veranschaulicht den Workflow bei der Agent-basierten Replikation mit privaten Endpunkten mithilfe des Tools Azure Migrate: Servermigration.  

![Replikationsarchitektur](./media/how-to-use-azure-migrate-with-private-endpoints/replication-architecture.png)

Das Tool verwendet eine Replikationsappliance, um Ihre Server in Azure zu replizieren. In [diesem Artikel](https://docs.microsoft.com/azure/migrate/tutorial-migrate-physical-virtual-machines#prepare-a-machine-for-the-replication-appliance) wird beschrieben, wie Sie einen Computer für die Replikationsappliance vorbereiten und einrichten.

Nachdem Sie die Replikationsappliance eingerichtet haben, erstellen Sie anhand der folgenden Anweisungen die erforderlichen Ressourcen für die Migration. 

1. Klicken Sie unter **Computer ermitteln** > **Sind Ihre Computer virtualisiert?** auf **Nicht virtualisiert/Andere**.
2. Wählen Sie unter **Zielregion** die Azure-Region aus, zu der Sie die Computer migrieren möchten, und bestätigen Sie Ihre Auswahl.
3. Wählen Sie **Ressourcen generieren** aus, um die erforderlichen Azure-Ressourcen zu erstellen. Schließen Sie die Seite nicht, während die Ressourcen erstellt werden.   
    - Hierbei wird ein Recovery Services-Tresor im Hintergrund erstellt und eine verwaltete Identität für den Tresor aktiviert. Ein Recovery Services-Tresor ist eine Entität, die die Replikationsinformationen von Servern enthält und zum Initiieren von Site Recovery-Vorgängen verwendet wird.  
    - Wenn das Azure Migrate-Projekt über Konnektivität mit privaten Endpunkten verfügt, wird ein privater Endpunkt für den Recovery Services-Tresor erstellt. Dabei werden dem privaten Endpunkt fünf vollqualifizierte private Namen (FQDNs), einer für jeden Microservice, hinzugefügt, die mit dem Recovery Services-Tresor verknüpft sind.   
    - Die fünf Domänennamen werden nach folgendem Muster formatiert: <br/> _{Tresor-ID}-asr-pod01-{Typ}-.{Zielregionscode}_ .privatelink.siterecovery.windowsazure.com  
    - Standardmäßig erstellt Azure Migrate automatisch eine private DNS-Zone und fügt DNS-A-Datensätze für die Microservices des Recovery Services-Tresors hinzu. Die private DNS-Zone wird dann mit dem virtuellen Netzwerk des privaten Endpunkts verknüpft. Dadurch kann die lokale Replikationsappliance die vollqualifizierten Domänennamen in ihre privaten IP-Adressen auflösen.

4. Stellen Sie vor dem Registrieren der Replikationsappliance sicher, dass die FQDNs für private Links des Tresors über den Computer erreichbar sind, auf dem die Replikationsappliance gehostet wird. [Erfahren Sie mehr über das Überprüfen der Netzwerkkonnektivität.](#troubleshoot-network-connectivity) 

5. Nachdem Sie die Konnektivität überprüft haben, laden Sie die Setup- und Schlüsseldatei der Appliance herunter, führen Sie den Installationsvorgang aus, und registrieren Sie die Appliance bei Azure Migrate. Sehen Sie sich [hier](https://docs.microsoft.com/azure/migrate/tutorial-migrate-physical-virtual-machines#set-up-the-replication-appliance) die ausführlichen Schritte an. Nachdem Sie die Replikationsappliance eingerichtet haben, befolgen Sie [diese Anweisungen](https://docs.microsoft.com/azure/migrate/tutorial-migrate-physical-virtual-machines#install-the-mobility-service), um den Mobilitätsdienst auf den Computern zu installieren, die Sie migrieren möchten. 

### <a name="replicate-servers-to-azure-using-azure-private-link"></a>Replizieren von Servern zu Azure mithilfe von Azure Private Link 

Führen Sie nun [diese Schritte](https://docs.microsoft.com/azure/migrate/tutorial-migrate-physical-virtual-machines#replicate-machines) aus, um Server für die Replikation auszuwählen.  

Verwenden Sie das Dropdownmenü unter **Replizieren** > **Zieleinstellungen** > **Cache/Replication storage account** (Cache-/Replikationsspeicherkonto), um ein Speicherkonto auszuwählen, das über eine private Verbindung repliziert werden soll.  

Wenn Ihr Azure Migrate-Projekt über private Endpunkte verbunden ist, müssen Sie [der verwalteten Identität des Recovery Services-Tresors Berechtigungen gewähren](#grant-access-permissions-to-the-recovery-services-vault), damit sie auf das für Azure Migrate erforderliche Speicherkonto zugreifen kann.   

[Erstellen Sie außerdem einen privaten Endpunkt für das Speicherkonto](#create-a-private-endpoint-for-the-storage-account-optional), um Replikationen über eine private Verbindung zu ermöglichen.

#### <a name="grant-access-permissions-to-the-recovery-services-vault"></a>Gewähren von Zugriffsberechtigungen für den Recovery Services-Tresor

Die verwaltete Identität des Recovery Services-Tresors erfordert Berechtigungen für den authentifizierten Zugriff auf das Cache- und das Replikationsspeicherkonto. 

Ermitteln Sie anhand der Anweisungen unten den Recovery Services-Tresor, der von Azure Migrate erstellt wurde, und gewähren Sie die erforderlichen Berechtigungen. 

**_Ermitteln des Recovery Services-Tresors und der Objekt-ID der verwalteten Identität_**

Die Details zum Recovery Services-Tresor finden Sie auf der **Eigenschaftenseite** des Tools Azure Migrate: Servermigration. 

1. Navigieren Sie zum **Azure Migrate-Hub**, und wählen Sie auf der Kachel für Azure Migrate: Servermigration **Übersicht** aus.

    ![Übersichtsseite im Azure Migrate-Hub](./media/how-to-use-azure-migrate-with-private-endpoints/hub-overview.png)

2. Wählen Sie im linken Bereich **Eigenschaften** aus. Notieren Sie sich den Namen des Recovery Services-Tresors und die ID der verwalteten Identität. Für den Tresor sind _Privater Endpunkt_ als **Konnektivitätstyp** und _Andere_ als **Replikationstyp** festgelegt. Sie benötigen diese Informationen, wenn Sie Zugriff auf den Tresor gewähren.
      
    ![Eigenschaftenseite von Azure Migrate: Servermigration](./media/how-to-use-azure-migrate-with-private-endpoints/vault-info.png)

**_Gewähren der erforderlichen Berechtigungen für den Zugriff auf das Speicherkonto_**

 Der verwalteten Identität des Tresors müssen die folgenden Rollenberechtigungen für das Speicherkonto gewährt werden, das für die Replikation erforderlich ist.  In diesem Fall müssen Sie das Speicherkonto im Voraus erstellen.

>[!Note]
> Für die Migration von Hyper-V-VMs zu Azure mithilfe von Private Link müssen Sie Zugriff auf das Replikationsspeicherkonto und das Cachespeicherkonto gewähren. 

Die Rollenberechtigungen variieren je nach Typ des Speicherkontos.

- Resource Manager-basierte Speicherkonten (Standard):
  - [Mitwirkender](../role-based-access-control/built-in-roles.md#contributor) _und_
  - [Mitwirkender an Speicherblobdaten](../role-based-access-control/built-in-roles.md#storage-blob-data-contributor)
- Resource Manager-basierte Speicherkonten (Premium):
  - [Mitwirkender](../role-based-access-control/built-in-roles.md#contributor) _und_
  - [Besitzer von Speicherblobdaten](../role-based-access-control/built-in-roles.md#storage-blob-data-owner)

1. Wechseln Sie zu dem Replikations-/Cachespeicherkonto, das für die Replikation ausgewählt wurde. Klicken Sie im linken Bereich auf **Zugriffssteuerung (IAM)** . 

1. Klicken Sie im Bereich **Rollenzuweisung hinzufügen** auf **Hinzufügen**:

   ![Hinzufügen einer Rollenzuweisung](./media/how-to-use-azure-migrate-with-private-endpoints/storage-role-assignment.png)


1. Wählen Sie auf der Seite **Rollenzuweisung hinzufügen** im Feld **Rolle** die passende Rolle aus der oben genannten Berechtigungsliste aus. Geben Sie den zuvor notierten Tresornamen ein, und wählen Sie **Speichern** aus.

    ![Bereitstellen von rollenbasiertem Zugriff](./media/how-to-use-azure-migrate-with-private-endpoints/storage-role-assignment-select-role.png)

4. Zusätzlich zu diesen Berechtigungen müssen Sie auch den Zugriff auf vertrauenswürdige Microsoft-Dienste zulassen. Wenn Ihr Netzwerkzugriff auf ausgewählte Netzwerke beschränkt ist, wählen Sie auf der Registerkarte **Netzwerk** im Abschnitt **Ausnahmen** die Option **Vertrauenswürdigen Microsoft-Diensten den Zugriff auf dieses Speicherkonto erlauben** aus. 

![Zulassen vertrauenswürdiger Microsoft-Dienste für das Speicherkonto](./media/how-to-use-azure-migrate-with-private-endpoints/exceptions.png)


### <a name="create-a-private-endpoint-for-the-storage-account-optional"></a>Erstellen eines privaten Endpunkts für das Speicherkonto (optional)

[Erstellen Sie einen privaten Endpunkt](https://docs.microsoft.com/azure/private-link/tutorial-private-endpoint-storage-portal#create-storage-account-with-a-private-endpoint) für die Cache-/Replikationsspeicherkonten, um die Replikation mithilfe von ExpressRoute mit privatem Peering zu ermöglichen (Zielunterressource: **_Blob_**). 

>[!Note]
>
> - Sie können private Endpunkte nur für ein Speicherkonto des Typs „Universell v2 (GPv2)“ erstellen. Preisinformationen finden Sie unter [Azure-Seitenblobs: Preise](https://azure.microsoft.com/pricing/details/storage/page-blobs/) und [Azure Private Link – Preise](https://azure.microsoft.com/pricing/details/private-link/).

Der private Endpunkt für das Speicherkonto sollte im selben virtuellen Netzwerk wie der private Endpunkt des Azure Migrate-Projekts oder in einem anderen, mit diesem Netzwerk verbundenen virtuellen Netzwerk erstellt werden. 

Wählen Sie **Ja** aus, und führen Sie die Integration mit einer privaten DNS-Zone aus. Die private DNS-Zone hilft beim Weiterleiten der Verbindungen aus dem virtuellen Netzwerk an das Speicherkonto über eine private Verbindung. Wenn Sie **Ja** auswählen, wird die DNS-Zone automatisch mit dem virtuellen Netzwerk verknüpft, und es werden die DNS-Einträge für die Auflösung neu erstellter IP-Adressen und vollqualifizierter Domänennamen hinzugefügt. Informieren Sie sich weiter über [private DNS-Zonen](https://docs.microsoft.com/azure/dns/private-dns-overview).

Wenn der Benutzer, der den privaten Endpunkt erstellt, auch der Besitzer des Speicherkontos ist, wird der private Endpunkt automatisch genehmigt. Andernfalls muss der Besitzer des Speicherkontos den privaten Endpunkt für die Verwendung genehmigen. Navigieren Sie auf der Seite des Speicherkontos unter **Netzwerk** zu **Verbindungen mit privatem Endpunkt**, um eine angeforderte Verbindung mit einem privaten Endpunkt zu genehmigen oder abzulehnen.

Überprüfen Sie vor dem Fortfahren den Verbindungsstatus des privaten Endpunkts. 

![Genehmigungsstatus privater Endpunkte](./media/how-to-use-azure-migrate-with-private-endpoints/private-endpoint-connection-state.png)

Nachdem Sie den privaten Endpunkt erstellt haben, wählen Sie über die Dropdownliste **Replizieren** > **Zieleinstellungen** > **Cachespeicherkonto** das Speicherkonto für die Replikation über eine private Verbindung aus.  

Stellen Sie sicher, dass die lokale Replikationsappliance über Netzwerkkonnektivität mit dem Speicherkonto an dessen privaten Endpunkt verfügt. [Erfahren Sie mehr über das Überprüfen der Netzwerkkonnektivität.](#troubleshoot-network-connectivity)

>[!Note]
>
> - Wenn bei Hyper-V-VM-Migrationsvorgängen zu Azure das Replikationsspeicherkonto den Typ _Premium_ aufweist, müssen Sie ein anderes Speicherkonto vom Typ _Standard_ für das Cachespeicherkonto auswählen. In diesem Fall müssen Sie private Endpunkte sowohl für das Replikations- als auch für das Cachespeicherkonto erstellen.  

Befolgen Sie als Nächstes die Anweisungen zum [Überprüfen und Starten der Replikation](https://docs.microsoft.com/azure/migrate/tutorial-migrate-physical-virtual-machines#replicate-machines) und zum [Durchführen der Migration](https://docs.microsoft.com/azure/migrate/tutorial-migrate-physical-virtual-machines#run-a-test-migration).  

## <a name="troubleshoot-network-connectivity"></a>Problembehandlung für die lokale Netzwerkkonnektivität 

### <a name="validate-private-endpoints-configuration"></a>Überprüfen der Konfiguration privater Endpunkte 

Stellen Sie sicher, dass der private Endpunkt den Status „Genehmigt“ aufweist.  

1. Wechseln Sie zu den Eigenschaftenseiten der Azure Migrate-Tools Ermittlung und Bewertung und Servermigration.
2. Die jeweilige Eigenschaftenseite enthält die Liste der privaten Endpunkte und FQDNs der privaten Verbindungen, die automatisch von Azure Migrate erstellt wurden.  

3. Wählen Sie den zu diagnostizierenden privaten Endpunkt aus.  
    1. Vergewissern Sie sich, dass der Verbindungsstatus Genehmigt lautet.
    2. Wenn sich die Verbindung im Status „Ausstehend“ befindet, müssen Sie sie zunächst genehmigen lassen.
    3. Sie können auch zur Ressource des privaten Endpunkts navigieren und überprüfen, ob das virtuelle Netzwerk mit dem virtuellen Netzwerk des privaten Endpunkts des Migrate-Projekts übereinstimmt. 

    ![Anzeigen einer Verbindung mit dem privaten Endpunkt](./media/how-to-use-azure-migrate-with-private-endpoints/private-endpoint-connection.png)

### <a name="verify-dns-resolution"></a>Überprüfen der DNS-Auflösung 

Die lokale Appliance (oder der Replikationsanbieter) greift über die vollqualifizierten Domänennamen (FQDNs) der privaten Verbindung auf die Azure Migrate-Ressourcen zu. Möglicherweise sind zusätzliche DNS-Einstellungen erforderlich, um die private IP-Adresse der privaten Endpunkte aus der Quellumgebung aufzulösen. In [diesem Artikel](https://docs.microsoft.com/azure/private-link/private-endpoint-dns#on-premises-workloads-using-a-dns-forwarder) werden DNS-Konfigurationsszenarien erläutert, die bei der Problembehandlung mit der Netzwerkkonnektivität helfen können.  

Führen Sie zum Überprüfen der privaten Verbindung von dem lokalen Server aus, auf dem die Migrate-Appliance gehostet wird, eine DNS-Auflösung für die Azure Migrate-Ressourcenendpunkte (Ressourcen-FQDNs der privaten Verbindung) durch, und vergewissern Sie sich, dass eine Auflösung in eine private IP-Adresse erfolgt. Die Details des privaten Endpunkts und die FQDNs der Ressource für die private Verbindung sind auf den Eigenschaftenseiten der Tools Ermittlung und Bewertung und Servermigration verfügbar. Wählen Sie **DNS-Einstellungen herunterladen** aus, um die Liste anzuzeigen.   

 ![Eigenschaften von Azure Migrate: Ermittlung und Bewertung](./media/how-to-use-azure-migrate-with-private-endpoints/server-assessment-properties.png)

 ![Eigenschaften von Azure Migrate: Servermigration](./media/how-to-use-azure-migrate-with-private-endpoints/azure-migrate-server-migration-properties.png)

Ein anschauliches Beispiel für die DNS-Auflösung des FQDN für die private Verbindung des Speicherkontos.  

- Geben Sie _nslookup<Speicherkontoname>.blob.core.windows.net_ ein.  Ersetzen Sie dabei <Speicherkontoname> durch den Namen des Speicherkontos, das für Azure Migrate verwendet wird.  

    Sie erhalten eine Meldung ähnlich der folgenden:  

   ![Beispiel für die DNS-Auflösung](./media/how-to-use-azure-migrate-with-private-endpoints/dns-resolution-example.png)

- Für das Speicherkonto wird als private IP-Adresse 10.1.0.5 zurückgegeben. Diese Adresse gehört zum Subnetz des virtuellen Netzwerks des privaten Endpunkts.   

Sie können die DNS-Auflösung für andere Azure Migrate-Artefakte mithilfe eines ähnlichen Ansatzes überprüfen.   

Führen Sie bei einer nicht korrekten DNS-Auflösung die folgenden Schritte aus:  

- Wenn Sie ein benutzerdefiniertes DNS verwenden, überprüfen Sie dessen Einstellungen und vergewissern sich, dass die DNS-Konfiguration korrekt ist. Einen entsprechenden Leitfaden finden Sie unter [Was ist privater Endpunkt in Azure? – DNS-Konfiguration](https://docs.microsoft.com/azure/private-link/private-endpoint-overview#dns-configuration).
- Wenn Sie von Azure bereitgestellte DNS-Server verwenden, finden Sie im folgenden Abschnitt weitere Informationen zur Problembehandlung.  

> [!Tip]
> Sie können die DNS-Einträge Ihrer Quellumgebung manuell aktualisieren, indem Sie die DNS-Datei „hosts“ auf Ihrer lokalen Appliance bearbeiten und die FQDNs der Ressource für die private Verbindung und die zugehörigen privaten IP-Adressen einfügen. Diese Option wird nur für Testzwecke empfohlen. <br/>  


### <a name="validate-the-private-dns-zone"></a>Überprüfen der privaten DNS-Zone   
Wenn die DNS-Auflösung nicht wie im vorherigen Abschnitt beschrieben funktioniert, liegt möglicherweise ein Problem mit Ihrer privaten DNS Zone vor.  

#### <a name="confirm-that-the-required-private-dns-zone-resource-exists"></a>Vergewissern, dass die erforderliche private DNS-Zonenressource vorhanden ist  
Standardmäßig erstellt Azure Migrate auch eine private DNS-Zone, die der Unterdomäne „privatelink“ für jeden Ressourcentyp entspricht. Die private DNS-Zone wird in derselben Azure-Ressourcengruppe wie die Ressourcengruppe des privaten Endpunkts erstellt. Die Azure-Ressourcengruppe sollte die Ressourcen der privaten DNS-Zone im folgenden Format enthalten:
- privatelink.vaultcore.azure.net für den Schlüsseltresor 
- privatelink.blob.core.windows.net für das Speicherkonto
- privatelink.siterecovery.windowsazure.com für den Recovery Services-Tresor (bei Hyper-V- und Agent-basierten Replikationen)
- privatelink.prod.migration.windowsazure.com für das Migrieren von Projekt, Bewertungsprojekt und Ermittlungsstandort   

Die private DNS-Zone wird automatisch von Azure Migrate erstellt (mit Ausnahme des vom Benutzer ausgewählten Cache-/Replikationsspeicherkontos). Sie können die verknüpfte private DNS-Zone ermitteln, indem Sie zur Seite des privaten Endpunkts navigieren und „DNS-Konfiguration“ auswählen. Die private DNS-Zone sollte im Abschnitt „Integration von privatem DNS“ angezeigt werden. 

![Screenshot der DNS-Konfiguration](./media/how-to-use-azure-migrate-with-private-endpoints/dns-configuration.png)  

Wenn die DNS-Zone nicht vorhanden ist (wie unten dargestellt), [erstellen Sie eine neue private DNS-Zonenressource](https://docs.microsoft.com/azure/dns/private-dns-getstarted-portal).  

![Erstellen einer privaten DNS-Zone](./media/how-to-use-azure-migrate-with-private-endpoints/create-dns-zone.png) 

#### <a name="confirm-that-the-private-dns-zone-is-linked-to-the-virtual-network"></a>Überprüfen der Verknüpfung der privaten DNS-Zone mit dem virtuellen Netzwerk  
Die private DNS-Zone muss mit dem virtuellen Netzwerk verknüpft sein, das den privaten Endpunkt für die DNS-Abfrage enthält, damit die private IP-Adresse des Ressourcenendpunkts aufgelöst werden kann. Wenn die private DNS-Zone nicht mit dem richtigen virtuellen Netzwerk verknüpft ist, ignoriert jegliche DNS-Auflösung aus diesem virtuellen Netzwerk die private DNS-Zone.   

Navigieren Sie im Azure-Portal zur Ressource „Private DNS-Zone“, und wählen Sie im linken Menü „Verknüpfungen virtueller Netzwerke“ aus. Daraufhin sollten die verknüpften virtuellen Netzwerke angezeigt werden.

![Anzeigen von Verknüpfungen virtueller Netzwerke](./media/how-to-use-azure-migrate-with-private-endpoints/virtual-network-links.png) 

Dadurch wird eine Liste mit Links angezeigt, die jeweils den Namen eines virtuellen Netzwerks in Ihrem Abonnement enthalten. Das virtuelle Netzwerk, das die Ressource des privaten Endpunkts enthält, muss hier aufgeführt sein. Befolgen Sie andernfalls [diesen Artikel](https://docs.microsoft.com/azure/dns/private-dns-getstarted-portal#link-the-virtual-network), um die private DNS-Zone mit einem virtuellen Netzwerk zu verknüpfen.    

Nachdem die private DNS-Zone mit dem virtuellen Netzwerk verknüpft ist, suchen DNS-Anforderungen aus dem virtuellen Netzwerk nach DNS-Einträgen in der privaten DNS-Zone. Dies ist für eine korrekte Adressenauflösung in dem virtuellen Netzwerk erforderlich, in dem der private Endpunkt erstellt wurde.   

#### <a name="confirm-that-the-private-dns-zone-contains-the-right-a-records"></a>Überprüfen der A-Datensätze der privaten DNS-Zone 

Wechseln Sie zur privaten DNS-Zone, für die Sie die Problembehandlung ausführen möchten. Auf der Übersichtsseite werden alle DNS-Einträge für diese private DNS-Zone angezeigt. Überprüfen Sie, ob für die Ressource ein DNS-A-Datensatz vorhanden ist. Der Wert des A-Datensatzes (die IP-Adresse) muss die private IP-Adresse der Ressource sein. Wenn Sie den A-Datensatz finden, dieser aber die falsche IP-Adresse enthält, müssen Sie die falsche IP-Adresse entfernen und eine neue hinzufügen. Es wird empfohlen, den gesamten A-Datensatz zu entfernen, einen neuen Eintrag hinzuzufügen und dann eine DNS-Leerung auf der lokalen Quellappliance durchführen.   

Ein anschauliches Beispiel für den DNS-A-Datensatz des Speicherkontos in der privaten DNS-Zone:

![DNS-Datensätze](./media/how-to-use-azure-migrate-with-private-endpoints/dns-a-records.png)   

Ein anschauliches Beispiel für den DNS-A-Datensatz der Microservices des Recovery Services-Tresors in der privaten DNS-Zone: 

![DNS-Einträge für den Recovery Services-Tresor](./media/how-to-use-azure-migrate-with-private-endpoints/rsv-a-records.png)   

>[!Note]
> Wenn Sie einen A-Datensatz entfernen oder ändern, kann der Computer weiterhin zu der alten IP-Adresse auflösen, da der TTL-Wert (Time to Live, Gültigkeitsdauer) möglicherweise noch nicht abgelaufen ist.  

#### <a name="other-things-that-may-affect-private-link-connectivity"></a>Weitere Dinge, die sich auf die Konnektivität einer privaten Verbindung auswirken können  

Dies ist eine unvollständige Liste von Elementen, die in erweiterten oder komplexen Szenarien zu finden sind: 

- Firewalleinstellungen: entweder die mit dem virtuellen Netzwerk verbundene Azure Firewall oder eine benutzerdefinierte Firewalllösung, die auf dem Computer mit der Appliance bereitgestellt wird  
- Netzwerkpeering, das beeinträchtigen kann, welche DNS-Server verwendet werden und wie Datenverkehr weitergeleitet wird.  
- Benutzerdefinierte Gatewaylösungen (NAT) beeinträchtigen möglicherweise, wie Datenverkehr weitergeleitet wird, einschließlich Datenverkehr von DNS-Abfragen.

Weitere Informationen finden Sie im [Leitfaden zur Problembehandlung bei Konnektivitätsproblemen mit privaten Endpunkten](https://docs.microsoft.com/azure/private-link/troubleshoot-private-endpoint-connectivity).  

## <a name="next-steps"></a>Nächste Schritte 
- [Schließen Sie den Migrationsprozess ab](https://docs.microsoft.com/azure/migrate/tutorial-migrate-physical-virtual-machines#complete-the-migration), und überprüfen Sie die [bewährten Methoden nach der Migration](https://docs.microsoft.com/azure/migrate/tutorial-migrate-physical-virtual-machines#post-migration-best-practices).