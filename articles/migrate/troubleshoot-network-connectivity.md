---
title: Beheben von Problemen mit der Netzwerkkonnektivität | Microsoft-Dokumentation
description: Enthält Tipps zur Problembehandlung für häufige Fehler bei der Verwendung von Azure Migrate mit privaten Endpunkten.
author: SGSneha
ms.author: v-ssudhir
ms.manager: deseelam
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 05/21/2021
ms.openlocfilehash: 9e987b4db88379e0dfe40b8839b073b893811fb4
ms.sourcegitcommit: 9ad20581c9fe2c35339acc34d74d0d9cb38eb9aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2021
ms.locfileid: "110547606"
---
# <a name="troubleshoot-network-connectivity"></a>Problembehandlung für die lokale Netzwerkkonnektivität
Dieser Artikel hilft Ihnen bei der Behandlung von Problemen mit der Netzwerkkonnektivität bei der Verwendung von Azure Migrate mit privaten Endpunkten.

## <a name="validate-private-endpoints-configuration"></a>Überprüfen der Konfiguration privater Endpunkte

Stellen Sie sicher, dass der private Endpunkt den Status „Genehmigt“ aufweist.  

1. Wechseln Sie zu den Eigenschaftenseiten der **Azure Migrate**-Tools **Ermittlung und Bewertung** und **Servermigration**.

2. Die jeweilige Eigenschaftenseite enthält die Liste der privaten Endpunkte und FQDNs der privaten Verbindungen, die automatisch von Azure Migrate erstellt wurden.  

3. Wählen Sie den zu diagnostizierenden privaten Endpunkt aus.  
   a. Vergewissern Sie sich, dass der Verbindungsstatus Genehmigt lautet.           
   b. Wenn sich die Verbindung im Status „Ausstehend“ befindet, müssen Sie sie zunächst genehmigen lassen.                         
   c. Sie können auch zur Ressource des privaten Endpunkts navigieren und überprüfen, ob das virtuelle Netzwerk mit dem virtuellen Netzwerk des privaten Endpunkts des Migrate-Projekts übereinstimmt.                                                        

     ![Anzeigen einer Verbindung mit dem privaten Endpunkt](./media/how-to-use-azure-migrate-with-private-endpoints/private-endpoint-connection.png)


## <a name="validate-the-data-flow-through-the-private-endpoints"></a>Überprüfen des Datenflusses über die privaten Endpunkte
Sehen Sie sich die Datenflussmetriken an, um den Datenfluss über private Endpunkte zu überprüfen. Wählen Sie auf der Eigenschaftenseite der Azure Migrate-Tools „Serverbewertung“ und „Servermigration“ den privaten Endpunkt aus. Damit gelangen Sie zum Abschnitt „Übersicht“ für den privaten Endpunkt in Azure Private Link Center. Wählen Sie im linken Menü **Metriken** aus, um die Werte für _Data Bytes In_ (Eingehende Datenbytes) und _Data Bytes Out_ (Ausgehende Datenbytes) und damit den Datenfluss anzuzeigen.

## <a name="verify-dns-resolution"></a>Überprüfen der DNS-Auflösung

Die lokale Appliance (oder der Replikationsanbieter) greift über die vollqualifizierten Domänennamen (FQDNs) der privaten Verbindung auf die Azure Migrate-Ressourcen zu. Möglicherweise sind zusätzliche DNS-Einstellungen erforderlich, um die private IP-Adresse der privaten Endpunkte aus der Quellumgebung aufzulösen. In [diesem Artikel](../private-link/private-endpoint-dns.md#on-premises-workloads-using-a-dns-forwarder) werden DNS-Konfigurationsszenarien erläutert, die bei der Problembehandlung mit der Netzwerkkonnektivität helfen können.  

Führen Sie zum Überprüfen der privaten Verbindung von dem lokalen Server aus, auf dem die Migrate-Appliance gehostet wird, eine DNS-Auflösung für die Azure Migrate-Ressourcenendpunkte (Ressourcen-FQDNs der privaten Verbindung) durch, und vergewissern Sie sich, dass eine Auflösung in eine private IP-Adresse erfolgt.
Die Details des privaten Endpunkts und die FQDNs der Ressource für die private Verbindung sind auf den Eigenschaftenseiten der Tools Ermittlung und Bewertung und Servermigration verfügbar. Wählen Sie **DNS-Einstellungen herunterladen** aus, um die Liste anzuzeigen.   

 ![Eigenschaften von Azure Migrate: Ermittlung und Bewertung](./media/how-to-use-azure-migrate-with-private-endpoints/server-assessment-properties.png)

 [![Eigenschaften von Azure Migrate: Servermigration](./media/how-to-use-azure-migrate-with-private-endpoints/azure-migrate-server-migration-properties-inline.png)](./media/how-to-use-azure-migrate-with-private-endpoints/azure-migrate-server-migration-properties-expanded.png#lightbox)

Ein anschauliches Beispiel für die DNS-Auflösung des FQDN für die private Verbindung des Speicherkontos.  

- Geben Sie _nslookup<Speicherkontoname>.blob.core.windows.net_ ein.  Ersetzen Sie dabei <Speicherkontoname> durch den Namen des Speicherkontos, das für Azure Migrate verwendet wird.  

    Sie erhalten eine Meldung ähnlich der folgenden:  

   ![Beispiel für die DNS-Auflösung](./media/how-to-use-azure-migrate-with-private-endpoints/dns-resolution-example.png)

- Für das Speicherkonto wird als private IP-Adresse 10.1.0.5 zurückgegeben. Diese Adresse gehört zum Subnetz des virtuellen Netzwerks des privaten Endpunkts.   

Sie können die DNS-Auflösung für andere Azure Migrate-Artefakte mithilfe eines ähnlichen Ansatzes überprüfen.   

Führen Sie bei einer nicht korrekten DNS-Auflösung die folgenden Schritte aus:  

- Wenn Sie ein benutzerdefiniertes DNS verwenden, überprüfen Sie dessen Einstellungen und vergewissern sich, dass die DNS-Konfiguration korrekt ist. Einen entsprechenden Leitfaden finden Sie unter [Was ist privater Endpunkt in Azure? – DNS-Konfiguration](../private-link/private-endpoint-overview.md#dns-configuration).
- Wenn Sie von Azure bereitgestellte DNS-Server verwenden, finden Sie im folgenden Abschnitt weitere Informationen zur Problembehandlung.  

> [!Tip]
> Sie können die DNS-Einträge Ihrer Quellumgebung manuell aktualisieren, indem Sie die DNS-Datei „hosts“ auf Ihrer lokalen Appliance bearbeiten und die FQDNs der Ressource für die private Verbindung und die zugehörigen privaten IP-Adressen einfügen. Diese Option wird nur für Testzwecke empfohlen. <br/>  


## <a name="validate-the-private-dns-zone"></a>Überprüfen der privaten DNS-Zone   
Wenn die DNS-Auflösung nicht wie im vorherigen Abschnitt beschrieben funktioniert, liegt möglicherweise ein Problem mit Ihrer privaten DNS Zone vor.  

### <a name="confirm-that-the-required-private-dns-zone-resource-exists"></a>Vergewissern, dass die erforderliche private DNS-Zonenressource vorhanden ist  
Standardmäßig erstellt Azure Migrate auch eine private DNS-Zone, die der Unterdomäne *privatelink* für jeden Ressourcentyp entspricht. Die private DNS-Zone wird in derselben Azure-Ressourcengruppe wie die Ressourcengruppe des privaten Endpunkts erstellt. Die Azure-Ressourcengruppe sollte die Ressourcen der privaten DNS-Zone im folgenden Format enthalten:
- privatelink.vaultcore.azure.net für den Schlüsseltresor
- privatelink.blob.core.windows.net für das Speicherkonto
- privatelink.siterecovery.windowsazure.com für den Recovery Services-Tresor (bei Hyper-V- und Agent-basierten Replikationen)
- privatelink.prod.migration.windowsazure.com für das Migrieren von Projekt, Bewertungsprojekt und Ermittlungsstandort   

Die private DNS-Zone wird automatisch von Azure Migrate erstellt (mit Ausnahme des vom Benutzer ausgewählten Cache-/Replikationsspeicherkontos). Sie können die verknüpfte private DNS-Zone ermitteln, indem Sie zur Seite des privaten Endpunkts navigieren und „DNS-Konfiguration“ auswählen. Hier sollte die private DNS-Zone im Abschnitt „Integration von privatem DNS“ angezeigt werden.

[![Screenshot der DNS-Konfiguration](./media/how-to-use-azure-migrate-with-private-endpoints/dns-configuration-inline.png)](./media/how-to-use-azure-migrate-with-private-endpoints/dns-configuration-expanded.png#lightbox)

Wenn die DNS-Zone nicht vorhanden ist (wie unten dargestellt), [erstellen Sie eine neue private DNS-Zonenressource](../dns/private-dns-getstarted-portal.md).  

[![Erstellen einer privaten DNS-Zone](./media/how-to-use-azure-migrate-with-private-endpoints/create-dns-zone-inline.png)](./media/how-to-use-azure-migrate-with-private-endpoints/create-dns-zone-expanded.png#lightbox)

### <a name="confirm-that-the-private-dns-zone-is-linked-to-the-virtual-network"></a>Überprüfen der Verknüpfung der privaten DNS-Zone mit dem virtuellen Netzwerk  
Die private DNS-Zone muss mit dem virtuellen Netzwerk verknüpft sein, das den privaten Endpunkt für die DNS-Abfrage enthält, damit die private IP-Adresse des Ressourcenendpunkts aufgelöst werden kann. Wenn die private DNS-Zone nicht mit dem richtigen virtuellen Netzwerk verknüpft ist, ignoriert jegliche DNS-Auflösung aus diesem virtuellen Netzwerk die private DNS-Zone.   

Navigieren Sie im Azure-Portal zur Ressource „Private DNS-Zone“, und wählen Sie im linken Menü „Verknüpfungen virtueller Netzwerke“ aus. Daraufhin sollten die verknüpften virtuellen Netzwerke angezeigt werden.

[![Anzeigen von Verknüpfungen virtueller Netzwerke](./media/how-to-use-azure-migrate-with-private-endpoints/virtual-network-links-inline.png)](./media/how-to-use-azure-migrate-with-private-endpoints/virtual-network-links-expanded.png#lightbox)

Dadurch wird eine Liste mit Links angezeigt, die jeweils den Namen eines virtuellen Netzwerks in Ihrem Abonnement enthalten. Das virtuelle Netzwerk, das die Ressource des privaten Endpunkts enthält, muss hier aufgeführt sein. Befolgen Sie andernfalls [diesen Artikel](../dns/private-dns-getstarted-portal.md#link-the-virtual-network), um die private DNS-Zone mit einem virtuellen Netzwerk zu verknüpfen.    

Nachdem die private DNS-Zone mit dem virtuellen Netzwerk verknüpft ist, suchen DNS-Anforderungen aus dem virtuellen Netzwerk nach DNS-Einträgen in der privaten DNS-Zone. Dies ist für eine korrekte Adressenauflösung in dem virtuellen Netzwerk erforderlich, in dem der private Endpunkt erstellt wurde.   

### <a name="confirm-that-the-private-dns-zone-contains-the-right-a-records"></a>Überprüfen der A-Datensätze der privaten DNS-Zone

Wechseln Sie zur privaten DNS-Zone, für die Sie die Problembehandlung ausführen möchten. Auf der Übersichtsseite werden alle DNS-Einträge für diese private DNS-Zone angezeigt. Überprüfen Sie, ob für die Ressource ein DNS-A-Datensatz vorhanden ist. Der Wert des A-Datensatzes (die IP-Adresse) muss die private IP-Adresse der Ressource sein. Wenn Sie den A-Datensatz finden, dieser aber die falsche IP-Adresse enthält, müssen Sie die falsche IP-Adresse entfernen und eine neue hinzufügen. Es wird empfohlen, den gesamten A-Datensatz zu entfernen, einen neuen Eintrag hinzuzufügen und dann eine DNS-Leerung auf der lokalen Quellappliance durchführen.   

Ein anschauliches Beispiel für den DNS-A-Datensatz des Speicherkontos in der privaten DNS-Zone:

![DNS-Datensätze](./media/how-to-use-azure-migrate-with-private-endpoints/dns-a-records.png)   

Ein anschauliches Beispiel für den DNS-A-Datensatz der Microservices des Recovery Services-Tresors in der privaten DNS-Zone:

[![DNS-Einträge für den Recovery Services-Tresor](./media/how-to-use-azure-migrate-with-private-endpoints/rsv-a-records-inline.png)](./media/how-to-use-azure-migrate-with-private-endpoints/rsv-a-records-expanded.png#lightbox)  

>[!Note]
> Wenn Sie einen A-Datensatz entfernen oder ändern, kann der Computer weiterhin zu der alten IP-Adresse auflösen, da der TTL-Wert (Time to Live, Gültigkeitsdauer) möglicherweise noch nicht abgelaufen ist.  

### <a name="items-that-may-affect-private-link-connectivity"></a>Aspekte, die sich auf die Konnektivität einer privaten Verbindung auswirken können  

Dies ist eine unvollständige Liste von Elementen, die in erweiterten oder komplexen Szenarien zu finden sind:

- Firewalleinstellungen: entweder die mit dem virtuellen Netzwerk verbundene Azure Firewall oder eine benutzerdefinierte Firewalllösung, die auf dem Computer mit der Appliance bereitgestellt wird  
- Netzwerkpeering, das beeinträchtigen kann, welche DNS-Server verwendet werden und wie Datenverkehr weitergeleitet wird.  
- Benutzerdefinierte Gatewaylösungen (NAT) beeinträchtigen möglicherweise, wie Datenverkehr weitergeleitet wird, einschließlich Datenverkehr von DNS-Abfragen.

Weitere Informationen finden Sie im [Leitfaden zur Problembehandlung bei Konnektivitätsproblemen mit privaten Endpunkten](../private-link/troubleshoot-private-endpoint-connectivity.md).  
