---
title: Replizieren von Daten über ExpressRoute mithilfe des Azure Migrate-Servermigrationstools
description: Hier erfahren Sie, wie Sie Azure ExpressRoute für die Replikation mit dem Azure Migrate-Servermigrationstool verwenden.
author: DeSeelam
ms.author: deseelam
ms.manager: bsiva
ms.topic: how-to
ms.date: 02/22/2021
ms.openlocfilehash: b52d61c2828ddf5c04ab943d73964d236c9017c1
ms.sourcegitcommit: 4b7a53cca4197db8166874831b9f93f716e38e30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "102098841"
---
# <a name="replicate-data-over-expressroute-with-azure-migrate-server-migration"></a>Replizieren von Daten über ExpressRoute mithilfe des Azure Migrate-Servermigrationstools

In diesem Artikel wird erläutert, wie Sie das [Azure Migrate-Servermigrationstool](https://docs.microsoft.com/azure/migrate/migrate-services-overview#azure-migrate-server-migration-tool) zum Replizieren von Daten über eine ExpressRoute-Verbindung konfigurieren, während Sie Server zu Azure migrieren.

## <a name="understand-azure-expressroute-circuits"></a>Informationen zu Azure ExpressRoute-Verbindungen
Über eine ExpressRoute-Verbindung (ER) wird Ihre lokale Infrastruktur über einen Konnektivitätsanbieter mit Microsoft verbunden. ExpressRoute-Verbindungen können konfiguriert werden, um privates Peering, Microsoft-Peering oder beides zu verwenden. Lesen Sie den Artikel zu [ExpressRoute-Verbindungen und Peering](https://docs.microsoft.com/azure/expressroute/expressroute-circuit-peerings#peeringcompare), um mehr über die verschiedenen Peeringoptionen zu erfahren, die mit ExpressRoute verfügbar sind.

Das Azure Migrate-Servermigrationstool unterstützt Sie beim Migrieren von lokalen Servern und Servern von anderen Clouds zu virtuellen Azure-Computern. Das Tool richtet einen kontinuierlichen Replikationsstream zum Replizieren von Daten von den zu migrierenden Servern zu den verwalteten Datenträgern in Ihrem Azure-Abonnement ein. Wenn Sie bereit sind, die Server zu migrieren, werden die replizierten Daten in Azure zum Migrieren der Server verwendet.

Daten, die von Ihren lokalen Servern repliziert werden, können so konfiguriert werden, dass sie über das Internet (über eine sichere verschlüsselte Verbindung) oder über eine ExpressRoute-Verbindung an Ihr Azure-Abonnement gesendet werden. Wenn Sie über eine große Anzahl von Servern verfügen, die Sie migrieren möchten, können Sie ExpressRoute für die Replikation verwenden, um Server unter Verwendung der bereitgestellten Bandbreite, die über Ihre ExpressRoute-Verbindung verfügbar ist, effizienter zu migrieren.

In diesem Artikel lernen Sie Folgendes:
> [!div class="checklist"]
>
> * Replizieren von Daten mithilfe einer ExpressRoute-Verbindung mit privatem Peering
> * Replizieren von Daten mithilfe einer ExpressRoute-Verbindung mit Microsoft-Peering

## <a name="replicate-data-using-an-expressroute-circuit-with-private-peering"></a>Replizieren von Daten mithilfe einer ExpressRoute-Verbindung mit privatem Peering

> [!NOTE]
> Die Replikation über eine private Peeringverbindung wird derzeit nur für die [Migration von virtuellen VMware-Computern zu Azure ohne Agent](./tutorial-migrate-vmware.md) unterstützt. Die Unterstützung für private Endpunkte für andere [Replikationsmethoden](./migrate-services-overview.md#azure-migrate-server-migration-tool) ist in Kürze verfügbar.

Bei der Methode zum Migrieren von virtuellen VMware-Computern zu Azure ohne Agent lädt die Azure Migrate-Appliance zunächst Replikationsdaten in ein Speicherkonto (Cachespeicherkonto) in Ihrem Abonnement hoch. Replizierte Daten aus dem Cachespeicherkonto werden dann vom Azure Migrate-Dienst auf vom Replikat verwaltete Datenträger in Ihrem Abonnement verschoben. Wenn Sie eine private Peeringverbindung für die Replikation verwenden möchten, erstellen Sie einen privaten Endpunkt, und fügen diesen an das zu verwendende Cachespeicherkonto an. Private Endpunkte verwenden eine oder mehrere private IP-Adressen Ihres virtuellen Netzwerks (VNet), sodass das Speicherkonto effektiv in Ihr virtuelles Azure-Netzwerk integriert wird. Der private Endpunkt ermöglicht der Azure Migrate-Appliance, mithilfe des privaten ExpressRoute-Peerings eine Verbindung mit dem Cachespeicherkonto herzustellen und Daten direkt über die private IP-Adresse zu übertragen. <br/>  

![Replikationsprozess](./media/replicate-using-expressroute/replication-process.png)

> [!Important]
>
> - Die Azure Migrate-Appliance kommuniziert mit dem Azure Migrate-Dienst, um zusätzlich zu den Replikationsdaten Informationen über die Steuerungsebenenaktivitäten einschließlich Orchestrierungsreplikationen zu erhalten. Die Steuerungsebenenkommunikation zwischen der Azure Migrate-Appliance und dem Azure Migrate-Dienst erfolgt weiterhin über das Internet und den öffentlichen Endpunkt des Azure Migrate-Diensts.
> - Der private Endpunkt des Speicherkontos sollte über das Netzwerk zugänglich sein, in dem die Azure Migrate-Appliance bereitgestellt wird.
> - Das DNS muss so konfiguriert sein, dass DNS-Abfragen von der Azure Migrate-Appliance für den Blobdienstendpunkt des Cachespeicherkontos in die private IP-Adresse des privaten Endpunkts aufgelöst werden, der mit dem Cachespeicherkonto verknüpft ist.
> - Der Zugriff auf das Cachespeicherkonto muss über dessen öffentlichen Endpunkt möglich sein. (Der Azure Migrate-Dienst verwendet den öffentlichen Endpunkt des Cachespeicherkontos zum Verschieben von Daten aus dem Speicherkonto auf vom Replikat verwaltete Datenträger.) 


### <a name="1-pre-requisites"></a>1. Voraussetzungen

Der Azure-Benutzer, der den privaten Endpunkt erstellt, muss über die folgenden Berechtigungen für die Ressourcengruppe und das virtuelle Netzwerk verfügen, in denen der private Endpunkt erstellt wird.

**Anwendungsfall** | **Berechtigungen** 
--- | --- 
 Erstellen und Verwalten privater Endpunkte | Microsoft.Network/privateEndpoint/write/action<br/>Microsoft.Network/privateEndpoint/read/action  
|Anfügen eines privaten Endpunkts an ein virtuelles Netzwerk oder Subnetz:<br/>Dies ist für das virtuelle Netzwerk erforderlich, in dem der private Endpunkt erstellt wird.| Microsoft.Network/virtualNetworks/subnet/join/action  Microsoft.Network/virtualNetworks/join/action
|Verknüpfen des privaten Endpunkts mit einem Speicherkonto <br/>| Microsoft.Microsoft.Storage/storageAccounts/privateEndpointConnectionApproval/action <br/> Microsoft.Microsoft.Storage/storageAccounts/privateEndpointConnections/read
|Erstellen einer Netzwerkschnittstelle und Verknüpfen mit einer Netzwerksicherheitsgruppe | Microsoft.Network/networkInterfaces/read <br/> Microsoft.Network/networkInterfaces/subnets/write <br/> Microsoft.Network/networkInterfaces/subnets/read<br/> Microsoft.Network/networkSecurityGroups/join/action (optional)
Erstellen und Verwalten privater DNS-Zonen| Rolle „Mitwirkender für private DNS-Zone“ <br/> _Oder_ <br/> Microsoft.Network/privateDnsZones/A/* <br/>  Microsoft.Network/privateDnsZones/write Microsoft.Network/privateDnsZones/read <br/> Microsoft.Network/privateEndpoints/privateDnsZoneGroups/write <br/> Microsoft.Network/privateEndpoints/privateDnsZoneGroups/read <br/> Microsoft.Network/privateDnsZones/virtualNetworkLinks/write <br/>  Microsoft.Network/privateDnsZones/virtualNetworkLinks/read <br/> Microsoft.Network/virtualNetworks/join/action 

### <a name="2-identify-the-cache-storage-account"></a>2. Identifizieren des Cachespeicherkontos 
 
Azure Migrate erstellt automatisch ein Cachespeicherkonto, wenn Sie die Replikation (über die Benutzeroberfläche des Azure-Portals) für einen virtuellen Computer zum ersten Mal in einem Azure Migrate-Projekt konfigurieren. Das Speicherkonto wird in demselben Abonnement und in derselben Ressourcengruppe erstellt, in dem bzw. der Sie das Azure Migrate-Projekt erstellt haben.

So erstellen und suchen Sie das Speicherkonto:

1. Verwenden Sie die Benutzeroberfläche des Azure-Portals für Azure Migrate, um einen oder mehrere virtuellen Computer im Projekt zu replizieren.
2. Navigieren Sie zur Ressourcengruppe des Azure Migrate-Projekts.
3. Suchen Sie das Cachespeicherkonto, indem Sie das Präfix **„Isa“** im Speicherkontonamen ermitteln.

![Ressourcengruppenansicht](./media/replicate-using-expressroute/storage-account-name.png)

> [!Tip]
> Wenn Sie in der Ressourcengruppe über mehrere Speicherkonten mit dem Präfix **„Isa“** verfügen, können Sie das Speicherkonto überprüfen, indem Sie zu den Replikationseinstellungen und dem Zielkonfigurationsmenü für jede der replizierenden VMs im Projekt navigieren. <br/> 
> ![Übersicht über Replikationseinstellungen](./media/replicate-using-expressroute/storage-account.png)

### <a name="3-upgrade--cache-storage-account-to-general-purpose-v2"></a>3. Durchführen eines Upgrades des Cachspeicherkontos auf „Universell v2“ 

Sie können private Endpunkte nur für ein Speicherkonto des Typs „Universell v2 (GPv2)“ erstellen. Wenn das Cachespeicherkonto kein Speicherkonto vom Typ „GPv2“ ist, führen Sie mithilfe der folgenden Schritte ein Upgrade auf GPv2 durch:

1. Navigieren Sie zum Speicherkonto.
2. Wählen Sie **Konfiguration** aus.
3. Klicken Sie unter **Kontoart** auf **Upgrade durchführen**.
4. Geben Sie unter **Confirm upgrade** (Upgrade bestätigen) den Namen Ihres Kontos ein.
5. Klicken Sie unten auf der Seite auf **Upgrade durchführen**.

![Upgrade für Speicherkonto](./media/replicate-using-expressroute/upgrade-storage-account.png)

### <a name="4-create-a-private-endpoint-for-the-storage-account"></a>4. Erstellen eines privaten Endpunkts für das Speicherkonto

1. Navigieren Sie zu Ihrem Speicherkonto, klicken Sie im Menü auf der linken Seite auf **Netzwerk**, und klicken Sie dann auf die Registerkarte **Private Endpunktverbindungen**.  
2. Wählen Sie **+ Privaten Endpunkt hinzufügen** aus.

    a. Wählen Sie im Fenster **Privaten Endpunkt erstellen** das **Abonnement** und die **Ressourcengruppe** aus. Geben Sie einen Namen für Ihren privaten Endpunkt an, und wählen Sie die Speicherkontoregion aus.  
    ![Fenster für die Konfiguration des privaten Endpunkts](./media/replicate-using-expressroute/storage-account-private-endpoint-creation.png)

    b. Geben Sie auf der Registerkarte **Ressource** für **Abonnementname** den Namen des Abonnements an, in dem sich das Speicherkonto befindet. Wählen Sie **Microsoft.Storage/storageAccounts** als **Ressourcentyp** aus. Geben Sie unter **Ressource** den Namen des Replikationsspeicherkontos vom Typ „GPv2“ an. Wählen Sie für **Untergeordnete Zielressource** **Blob** aus.  
    ![Einstellungen zum Erstellen eines privaten Endpunkts für ein Speicherkonto](./media/replicate-using-expressroute/storage-account-private-endpoint-settings.png)

    c. Wählen Sie auf der Registerkarte **Konfiguration** die Optionen für **Virtuelles Netzwerk** und **Subnetz** für den privaten Endpunkt des Speicherkontos aus.  

    > [!Note]
    > Das virtuelle Netzwerk muss den ExpressRoute-Gatewayendpunkt enthalten oder eine Verbindung mit dem virtuellen Netzwerk mit dem ExpressRoute-Gateway aufweisen. 

    Klicken Sie im Abschnitt **Private DNS-Integration** auf **Ja**, und integrieren Sie mit einer privaten DNS-Zone. Wenn Sie auf **Ja** klicken, wird die DNS-Zone automatisch mit dem ausgewählten virtuellen Netzwerk verknüpft, und es werden die DNS-Einträge für die DNS-Auflösung der neuen IP-Adressen und vollqualifizierten Domänennamen hinzugefügt, die für den privaten Endpunkt erstellt wurden. Informieren Sie sich weiter über [private DNS-Zonen](https://docs.microsoft.com/azure/dns/private-dns-overview).

    ![Private DNS-Zone](./media/replicate-using-expressroute/private-dns-zone.png)

    d. Sie können auch **Tags** für Ihren privaten Endpunkt hinzufügen.  

    e. Fahren Sie mit **Überprüfen + erstellen** fort, nachdem Sie die Details eingegeben haben. Klicken Sie nach Abschluss der Validierung auf **Erstellen**, um den privaten Endpunkt zu erstellen.

    > [!Note]
    > Wenn der Benutzer, der den privaten Endpunkt erstellt, auch der Besitzer des Speicherkontos ist, wird der private Endpunkt automatisch genehmigt. Andernfalls muss der Besitzer den privaten Endpunkt für die Verwendung genehmigen. 

#### <a name="create-private-dns-zones-and-add-dns-records-manually-optional"></a>Erstellen privater DNS-Zonen und manuelles Hinzufügen von DNS-Einträgen (optional)

Wenn Sie zum Zeitpunkt der Erstellung des privaten Endpunkts nicht die Option zur Integration mit einer privaten DNS-Zone ausgewählt haben, führen Sie die Schritte in diesem Abschnitt aus, um eine private DNS-Zone manuell zu erstellen. 

> [!Note]
> Wenn Sie für die Integration mit einer privaten DNS-Zone **Ja** ausgewählt haben, können Sie diesen Abschnitt überspringen. 

1. Erstellen Sie eine private DNS-Zone. 

    ![Erstellen privater DNS-Zonen](./media/replicate-using-expressroute/create-private-dns.png)

    a.  Klicken Sie auf der Seite **Private DNS-Zonen** auf die Schaltfläche **+ Hinzufügen**, um mit dem Erstellen einer neuen Zone zu beginnen.  
    b.  Geben Sie auf der Seite **Private DNS-Zone erstellen** die erforderlichen Details ein. Geben Sie _privatelink_.blob.core.windows.net als Name für die private DNS-Zone ein. c. Überprüfen und erstellen Sie die DNS-Zone anschließend über die Registerkarte **Überprüfen + erstellen**.

2. Verknüpfen Sie die private DNS-Zone mit Ihrem virtuellen Netzwerk.  

    Die zuvor erstellte private DNS-Zone muss mit dem virtuellen Netzwerk verknüpft werden, an das der private Endpunkt angefügt ist.

    a. Wechseln Sie zu der im vorherigen Schritt erstellten privaten DNS-Zone, und navigieren Sie dann auf der linken Seite zu „VNET-Verknüpfungen“. Klicken Sie auf die Schaltfläche **+ Hinzufügen**.   
    b. Geben Sie die erforderlichen Details ein. Die Felder **Abonnement** und **Virtuelles Netzwerk** müssen mit entsprechenden Angaben zum virtuellen Netzwerk ausgefüllt werden, an das Ihr privater Endpunkt angefügt ist. Die anderen Felder können Sie unverändert lassen.

3. Der nächste Schritt besteht darin, der DNS-Zone DNS-Einträge hinzuzufügen. Fügen Sie einen Eintrag für den vollqualifizierten Domänennamen des Speicherkontos in Ihre private DNS-Zone ein.

    a. Wechseln Sie zu Ihrer privaten DNS-Zone, und navigieren Sie auf der linken Seite zum Abschnitt **Übersicht**. Klicken Sie dort auf **+ Datensatzgruppe**, um mit dem Hinzufügen von Einträgen zu beginnen.  

    b. Fügen Sie auf der Seite **Datensatzgruppe hinzufügen** einen Eintrag für den vollqualifizierten Domänennamen und die private IP-Adresse als A-Eintrag hinzu.

> [!Important]
> Möglicherweise benötigen Sie zusätzliche DNS-Einstellungen, um die private IP-Adresse des privaten Endpunkts des Speicherkontos aus der Quellumgebung aufzulösen. [Lesen Sie diesen Artikel](https://docs.microsoft.com/azure/private-link/private-endpoint-dns#on-premises-workloads-using-a-dns-forwarder), um zu verstehen, welche DNS-Konfiguration erforderlich ist.

## <a name="replicate-data-using-an-expressroute-circuit-with-microsoft-peering"></a>Replizieren von Daten mithilfe einer ExpressRoute-Verbindung mit Microsoft-Peering

Sie können das Microsoft-Peering oder eine vorhandene öffentliche Peeringdomäne verwenden (für neue ExpressRoute-Verbindungen veraltet), um den Replikationsdatenverkehr wie auf der folgenden Abbildung dargestellt über eine ExpressRoute-Verbindung weiterzuleiten.
![Replikation mit Microsoft-Peering](./media/replicate-using-expressroute/replication-with-microsoft-peering.png)

Auch wenn Replikationsdaten über die Microsoft-Peeringverbindung weitergeleitet werden, benötigen Sie für die Kommunikation (Steuerungsebene) mit dem Azure Migrate-Dienst weiterhin eine Internetverbindung vom lokalen Standort aus. Es gibt einige zusätzliche URLs, die über ExpressRoute nicht erreichbar sind, auf die die Replikationsappliance bzw. der Hyper-V-Host für die Orchestrierung des Replikationsprozesses Zugriff benötigt. Sie können die auf den Migrationsszenarios [URLs für die öffentliche Cloud](https://docs.microsoft.com/azure/migrate/migrate-appliance#public-cloud-urls) und [Replikationsappliance](https://docs.microsoft.com/azure/migrate/migrate-replication-appliance) basierenden URL-Anforderungen überprüfen.  

Wenn Sie an Ihrem lokalen Standort einen Proxyserver nutzen und ExpressRoute für den Replikationsdatenverkehr verwenden möchten, müssen Sie eine Proxyumgehung für relevante URLs auf der lokalen Appliance konfigurieren. 

### <a name="configure-proxy-bypass-rules-on-the-azure-migrate-appliance-for-vmware-agentless-migrations"></a>Konfigurieren von Proxyumgehungsregeln auf der Azure Migrate-Appliance (für VMware-Migrationen ohne Agent)

1. Melden Sie sich bei der Azure Migrate-Appliance an (Remotedesktopverbindung).   
2. Öffnen Sie mithilfe von Editor die Datei „C:/ProgramData/MicrosoftAzure/Config/appliance.json“.
3. Ändern Sie in der Datei die Zeile "EnableProxyBypassList": "false" in "EnableProxyBypassList": "true" Speichern Sie die Änderungen, und starten Sie die Appliance neu.
4. Wenn Sie nach dem Neustart den Konfigurations-Manager der Appliance öffnen, werden auf der Benutzeroberfläche der Web-App die Proxyumgehungsoptionen angezeigt. Fügen Sie der Proxyumgehungsliste die folgenden URLs hinzu.   
    - .*.vault.azure.net
    - .*.servicebus.windows.net
    - .*.discoverysrv.windowsazure.com
    - .*.migration.windowsazure.com
    - .*.hypervrecoverymanager.windowsazure.com
    - .*.blob.core.windows.net

### <a name="configure-proxy-bypass-rules-on-the-replication-appliance-for-agent-based-migrations"></a>Konfigurieren von Proxyumgehungsregeln auf der Replikationsappliance (für Migrationen mit Agent)

Führen Sie die folgenden Schritte aus, um die Proxyumgehungsliste auf dem Konfigurationsserver und den Prozessservern zu konfigurieren:

1. [Laden Sie das PsExec-Tool herunter](https://docs.microsoft.com/sysinternals/downloads/psexec), um auf Systembenutzerkontext zuzugreifen.
2. Öffnen Sie Internet Explorer im Systembenutzerkontext, indem Sie die folgende Befehlszeile ausführen: psexec -s -i "%programfiles%\Internet Explorer\iexplore.exe".
3. Fügen Sie in Internet Explorer Proxyeinstellungen hinzu.
4. Fügen Sie in der Umgehungsliste die Azure-Speicher-URL „.*.blob.core.windows.net“ hinzu.  

Mit den oben genannten Umgehungsregeln wird sichergestellt, dass der Replikationsdatenverkehr über ExpressRoute weitergeleitet werden kann, während die Verwaltungskommunikation über den Proxyserver für das Internet erfolgt.  

Außerdem müssen Sie Routen im Routenfilter für die folgenden BGP-Communitys ankündigen, damit der Azure Migrate-Replikationsdatenverkehr eine ExpressRoute-Verbindung und nicht das Internet durchläuft.  

- Regionale BGP-Community für die Azure-Quellregion (Azure Migrate-Projektbereich)
- Regionale BGP-Community für die Azure-Zielregion (Region für die Migration)
- BGP-Community für Azure Active Directory (12076:5060)

Informieren Sie sich weiter über [Routenfilter](https://docs.microsoft.com/azure/expressroute/how-to-routefilter-portal), und sehen Sie sich die Liste der [BGP-Communitys für ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-routing#bgp) an. 

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu [ExpressRoute-Verbindungen](https://docs.microsoft.com/azure/expressroute/expressroute-circuit-peerings)
- Weitere Informationen zu [ExpressRoute-Routingdomänen](https://docs.microsoft.com/azure/expressroute/expressroute-circuit-peerings#peeringcompare)
- Weitere Informationen zu [privaten Endpunkten](https://docs.microsoft.com/azure/private-link/private-endpoint-overview)