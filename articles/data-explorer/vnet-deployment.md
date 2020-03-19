---
title: Bereitstellen von Azure Data Explorer in Ihrem virtuellen Netzwerk
description: Erfahren Sie, wie Sie Azure Data Explorer in Ihrem virtuellen Netzwerk bereitstellen.
author: basaba
ms.author: basaba
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 10/31/2019
ms.openlocfilehash: 5a2731e26ba4f371177cf2ae649f0695f27e6304
ms.sourcegitcommit: be53e74cd24bbabfd34597d0dcb5b31d5e7659de
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/11/2020
ms.locfileid: "79096766"
---
# <a name="deploy-azure-data-explorer-into-your-virtual-network"></a>Bereitstellen von Azure Data Explorer in Ihrem virtuellen Netzwerk

In diesem Artikel werden die Ressourcen erläutert, die beim Bereitstellen eines Azure Data Explorer-Clusters in einem benutzerdefinierten virtuellen Azure-Netzwerk vorhanden sind. Diese Informationen helfen Ihnen bei der Bereitstellung eines Clusters in einem Subnetz in Ihrem virtuellen Netzwerk (VNET). Weitere Informationen zu virtuellen Azure-Netzwerken finden Sie unter [Was ist Azure Virtual Network?](/azure/virtual-network/virtual-networks-overview)

   ![VNET-Diagramm](media/vnet-deployment/vnet-diagram.png)

Azure Data Explorer unterstützt die Bereitstellung eines Clusters in einem Subnetz in Ihrem virtuellen Netzwerk (VNET). Diese Funktion ermöglicht Ihnen Folgendes:

* Erzwingen von [NSG-Regeln (Network Security Group)](/azure/virtual-network/security-overview) für Ihren Azure Data Explorer-Clusterdatenverkehr
* Verbinden Ihres lokalen Netzwerks mit dem Subnetz des Azure Data Explorer-Clusters
* Sichern Ihrer Datenverbindungsquellen ([Event Hub](/azure/event-hubs/event-hubs-about) und [Event Grid](/azure/event-grid/overview)) mit [Dienstendpunkten](/azure/virtual-network/virtual-network-service-endpoints-overview)

## <a name="access-your-azure-data-explorer-cluster-in-your-vnet"></a>Zugreifen auf den Azure Data Explorer-Cluster in Ihrem VNET

Sie können auf Ihren Azure Data Explorer-Cluster mit den folgenden IP-Adressen für die einzelnen Dienste (Engine- und Datenverwaltungsdienste) zugreifen:

* **Private IP**: Wird für den Zugriff auf den Cluster innerhalb des VNET verwendet.
* **Öffentliche IP**: Wird für den Zugriff auf den Cluster von außerhalb des VNET zur Verwaltung und Überwachung sowie als Quelladresse für ausgehende Verbindungen, die aus dem Cluster gestartet werden, verwendet.

Die folgenden DNS-Einträge werden für den Zugriff auf den Dienst erstellt: 

* `[clustername].[geo-region].kusto.windows.net` (Engine) und `ingest-[clustername].[geo-region].kusto.windows.net` (Datenverwaltung) werden der öffentlichen IP-Adresse für jeden Dienst zugeordnet. 

* `private-[clustername].[geo-region].kusto.windows.net` (Engine) und `private-ingest-[clustername].[geo-region].kusto.windows.net` (Datenverwaltung) werden der privaten IP-Adresse für jeden Dienst zugeordnet.

## <a name="plan-subnet-size-in-your-vnet"></a>Planen der Subnetzgröße in Ihrem VNET

Die Größe des Subnetzes, das zum Hosten eines Azure Data Explorer-Clusters verwendet wird, kann nach der Bereitstellung des Subnetzes nicht geändert werden. In Ihrem VNET verwendet Azure Data Explorer eine private IP-Adresse für jede VM und zwei private IP-Adressen für die internen Lastenausgleiche (Engine und Datenverwaltung). Das Azure-Netzwerk verwendet außerdem fünf IP-Adressen für jedes Subnetz. Azure Data Explorer stellt zwei VMs für den Datenverwaltungsdienst bereit. Enginedienst-VMs werden nach Skalierungskapazität der Benutzerkonfiguration bereitgestellt.

Gesamtanzahl von IP-Adressen:

| Zweck | Anzahl der Adressen |
| --- | --- |
| Enginedienst | 1 pro Instanz |
| Datenverwaltungsdienst | 2 |
| Interne Lastenausgleiche | 2 |
| Für Azure reservierte Adressen | 5 |
| **Gesamt** | **Anzahl der Engine-Instanzen + 9** |

> [!IMPORTANT]
> Die Subnetzgröße muss im Voraus geplant werden, da sie nach der Bereitstellung von Azure Data Explorer nicht mehr geändert werden kann. Reservieren Sie deshalb die erforderliche Subnetzgröße entsprechend.

## <a name="service-endpoints-for-connecting-to-azure-data-explorer"></a>Dienstendpunkte zum Herstellen einer Verbindung mit Azure Data Explorer

Mit [Azure-Dienstendpunkten](/azure/virtual-network/virtual-network-service-endpoints-overview) können Sie Ihre mehrinstanzenfähigen Azure-Ressourcen im virtuellen Netzwerk sichern.
Wenn Sie Azure Data Explorer-Cluster in Ihrem Subnetz bereitstellen, können Sie Datenverbindungen mit [Event Hub](/azure/event-hubs/event-hubs-about) oder [Event Grid](/azure/event-grid/overview) einrichten, während Sie die zugrunde liegenden Ressourcen für das Azure Data Explorer-Subnetz einschränken.

> [!NOTE]
> Wenn Sie die EventGrid-Einrichtung mit [Storage](/azure/storage/common/storage-introduction) und [Event Hub] verwenden, kann das im Abonnement verwendete Speicherkonto mit Dienstendpunkten im Subnetz von Azure Data Explorer gesperrt werden, während vertrauenswürdige Azure-Plattformdienste in der [Firewallkonfiguration](/azure/storage/common/storage-network-security) zugelassen werden. Der Event Hub kann jedoch Dienstendpunkte nicht aktivieren, da er keine vertrauenswürdigen [Azure-Plattformdienste](/azure/event-hubs/event-hubs-service-endpoints) unterstützt.

## <a name="dependencies-for-vnet-deployment"></a>Abhängigkeiten für die VNET-Bereitstellung

### <a name="network-security-groups-configuration"></a>Konfiguration von Netzwerksicherheitsgruppen

[Netzwerksicherheitsgruppen (NSG)](/azure/virtual-network/security-overview) bieten die Möglichkeit, den Netzwerkzugriff innerhalb eines VNET zu steuern. Auf Azure Data Explorer kann über zwei Endpunkte zugegriffen werden: HTTPs (443) und TDS (1433). Die folgenden NSG-Regeln müssen konfiguriert werden, um den Zugriff auf diese Endpunkte für die Verwaltung, Überwachung und den ordnungsgemäßen Betrieb Ihres Clusters zuzulassen.

#### <a name="inbound-nsg-configuration"></a>Konfiguration eingehender Netzwerksicherheitsgruppen

| **Verwenden Sie**   | **From**   | **An**   | **Protokoll**   |
| --- | --- | --- | --- |
| Verwaltung  |[ADX-Verwaltungsadressen](#azure-data-explorer-management-ip-addresses)/AzureDataExplorerManagement(ServiceTag) | ADX-Subnetz: 443  | TCP  |
| Systemüberwachung  | [ADX-Systemüberwachungsadressen](#health-monitoring-addresses)  | ADX-Subnetz: 443  | TCP  |
| Interne ADX-Kommunikation  | ADX-Subnetz: Alle Ports  | ADX-Subnetz: Alle Ports  | All  |
| Azure Load Balancer eingehend zulassen (Integritätstest)  | AzureLoadBalancer  | ADX-Subnetz: 80, 443  | TCP  |

#### <a name="outbound-nsg-configuration"></a>Konfiguration ausgehender Netzwerksicherheitsgruppen

| **Verwenden Sie**   | **From**   | **An**   | **Protokoll**   |
| --- | --- | --- | --- |
| Abhängigkeit von Azure Storage  | ADX-Subnetz  | Storage: 443  | TCP  |
| Abhängigkeit von Azure Data Lake  | ADX-Subnetz  | AzureDataLake: 443  | TCP  |
| EventHub-Erfassung und Dienstüberwachung  | ADX-Subnetz  | EventHub: 443, 5671  | TCP  |
| Metriken veröffentlichen  | ADX-Subnetz  | AzureMonitor: 443 | TCP  |
| Azure Monitor-Konfigurationsdownload  | ADX-Subnetz  | [Azure Monitor-Konfigurationsendpunktadressen](#azure-monitor-configuration-endpoint-addresses): 443 | TCP  |
| Active Directory (falls zutreffend) | ADX-Subnetz | AzureActiveDirectory: 443 | TCP |
| Zertifizierungsstelle | ADX-Subnetz | Internet: 80 | TCP |
| Interne Kommunikation  | ADX-Subnetz  | ADX-Subnetz: Alle Ports  | All  |
| Ports, die für die Plug-Ins `sql\_request` und `http\_request` verwendet werden  | ADX-Subnetz  | Internet: Benutzerdefiniert  | TCP  |

### <a name="relevant-ip-addresses"></a>Relevante IP-Adressen

#### <a name="azure-data-explorer-management-ip-addresses"></a>IP-Adressen der Azure Data Explorer-Verwaltung

| Region | Adressen |
| --- | --- |
| Australien, Mitte | 20.37.26.134 |
| Australien, Mitte 2 | 20.39.99.177 |
| Australien (Osten) | 40.82.217.84 |
| Australien, Südosten | 20.40.161.39 |
| Brasilien, Süden | 191.233.25.183 |
| Kanada, Mitte | 40.82.188.208 |
| Kanada, Osten | 40.80.255.12 |
| Indien, Mitte | 40.81.249.251 |
| USA (Mitte) | 40.67.188.68 |
| USA, Mitte (EUAP) | 40.89.56.69 |
| Asien, Osten | 20.189.74.103 |
| East US | 52.224.146.56 |
| USA (Ost 2) | 52.232.230.201 |
| USA, Osten 2 (EUAP) | 52.253.226.110 |
| Frankreich, Mitte | 40.66.57.91 |
| Frankreich, Süden | 40.82.236.24 |
| Japan, Osten | 20.43.89.90 |
| Japan, Westen | 40.81.184.86 |
| Korea, Mitte | 40.82.156.149 |
| Korea, Süden | 40.80.234.9 |
| USA Nord Mitte | 40.81.45.254 |
| Nordeuropa | 52.142.91.221 |
| Südafrika, Norden | 102.133.129.138 |
| Südafrika, Westen | 102.133.0.97 |
| USA Süd Mitte | 20.45.3.60 |
| Asien, Südosten | 40.119.203.252 |
| Indien (Süden) | 40.81.72.110 |
| UK, Süden | 40.81.154.254 |
| UK, Westen | 40.81.122.39 |
| USA, Westen-Mitte | 52.159.55.120 |
| Europa, Westen | 51.145.176.215 |
| Indien, Westen | 40.81.88.112 |
| USA (Westen) | 13.64.38.225 |
| USA, Westen 2 | 40.90.219.23 |

#### <a name="health-monitoring-addresses"></a>Systemüberwachungsadressen

| Region | Adressen |
| --- | --- |
| Australien, Mitte | 191.239.64.128 |
| Australien, Mitte 2 | 191.239.64.128 |
| Australien (Osten) | 191.239.64.128 |
| Australien, Südosten | 191.239.160.47 |
| Brasilien Süd | 23.98.145.105 |
| Kanada, Mitte | 168.61.212.201 |
| Kanada, Osten | 168.61.212.201 |
| Indien, Mitte | 23.99.5.162 |
| USA (Mitte) | 168.61.212.201 |
| USA, Mitte (EUAP) | 168.61.212.201 |
| Asien, Osten | 168.63.212.33 |
| East US | 137.116.81.189 |
| USA (Ost) 2 | 137.116.81.189 |
| USA, Osten 2 (EUAP) | 137.116.81.189 |
| Frankreich, Mitte | 23.97.212.5 |
| Frankreich, Süden | 23.97.212.5 |
| Japan, Osten | 138.91.19.129 |
| Japan, Westen | 138.91.19.129 |
| Korea, Mitte | 138.91.19.129 |
| Korea, Süden | 138.91.19.129 |
| USA Nord Mitte | 23.96.212.108 |
| Nordeuropa | 191.235.212.69 
| Südafrika, Norden | 104.211.224.189 |
| Südafrika, Westen | 104.211.224.189 |
| USA Süd Mitte | 23.98.145.105 |
| Indien (Süden) | 23.99.5.162 |
| Asien, Südosten | 168.63.173.234 |
| UK, Süden | 23.97.212.5 |
| UK, Westen | 23.97.212.5 |
| USA, Westen-Mitte | 168.61.212.201 |
| Europa, Westen | 23.97.212.5 |
| Indien, Westen | 23.99.5.162 |
| USA (Westen) | 23.99.5.162 |
| USA, Westen 2 | 23.99.5.162 |    

#### <a name="azure-monitor-configuration-endpoint-addresses"></a>Azure Monitor-Konfigurationsendpunktadressen

| Region | Adressen |
| --- | --- |
| Australien, Mitte | 52.148.86.165 |
| Australien, Mitte 2 | 52.148.86.165 |
| Australien, Osten | 52.148.86.165 |
| Australien, Südosten | 52.148.86.165 |
| Brasilien, Süden | 13.68.89.19 |
| Kanada, Mitte | 13.90.43.231 |
| Kanada, Osten | 13.90.43.231 |
| Indien, Mitte | 13.71.25.187 |
| USA, Mitte | 52.173.95.68 |
| USA, Mitte (EUAP) | 13.90.43.231 |
| Asien, Osten | 13.75.117.221 |
| USA, Osten | 13.90.43.231 |
| USA, Osten 2 | 13.68.89.19 |    
| USA, Osten 2 (EUAP) | 13.68.89.19 |
| Frankreich, Mitte | 52.174.4.112 |
| Frankreich, Süden | 52.174.4.112 |
| Japan, Osten | 13.75.117.221 |
| Japan, Westen | 13.75.117.221 |
| Korea, Mitte | 13.75.117.221 |
| Korea, Süden | 13.75.117.221 |
| USA, Norden-Mitte | 52.162.240.236 |
| Europa, Norden | 52.169.237.246 |
| Südafrika, Norden | 13.71.25.187 |
| Südafrika, Westen | 13.71.25.187 |
| USA, Süden-Mitte | 13.84.173.99 |
| Indien, Süden | 13.71.25.187 |
| Asien, Südosten | 52.148.86.165 |
| UK, Süden | 52.174.4.112 |
| UK, Westen | 52.169.237.246 |
| USA, Westen-Mitte | 52.161.31.69 |
| Europa, Westen | 52.174.4.112 |
| Indien, Westen | 13.71.25.187 |
| USA, Westen | 40.78.70.148 |
| USA, Westen 2 | 52.151.20.103 |

## <a name="expressroute-setup"></a>ExpressRoute-Setup

Mithilfe von ExpressRoute können Sie ein lokales Netzwerk mit dem virtuellen Azure-Netzwerk verbinden. Ein gängiges Setup ist das Ankündigen der Standardroute (0.0.0.0/0) über die BGP-Sitzung (Border Gateway Protocol). Dadurch wird der Datenverkehr aus dem virtuellen Netzwerk zwangsläufig an das lokale Netzwerk des Kunden weitergeleitet, das den Datenverkehr möglicherweise abbricht und so ausgehende Datenflüsse unterbrochen werden. Um dieses Standardverhalten außer Kraft zu setzen, kann eine [benutzerdefinierte Route (User Defined Route, UDR)](/azure/virtual-network/virtual-networks-udr-overview#user-defined) (0.0.0.0/0) konfiguriert werden, und der nächste Hop ist dann *Internet*. Da die UDR Vorrang vor BGP hat, wird der Datenverkehr an das Internet geleitet.

## <a name="securing-outbound-traffic-with-firewall"></a>Sichern von ausgehendem Datenverkehr mit Firewall

Wenn Sie ausgehenden Datenverkehr mithilfe von [Azure Firewall](/azure/firewall/overview) oder einem virtuellen Gerät zum Einschränken von Domänennamen sichern möchten, müssen die folgenden vollqualifizierten Domänennamen (FQDN) in der Firewall zulässig sein.

```
prod.warmpath.msftcloudes.com:443
production.diagnostics.monitoring.core.windows.net:443
graph.windows.net:443
*.update.microsoft.com:443
shavamanifestcdnprod1.azureedge.net:443
login.live.com:443
wdcp.microsoft.com:443
login.microsoftonline.com:443
azureprofilerfrontdoor.cloudapp.net:443
*.core.windows.net:443
*.servicebus.windows.net:443
shoebox2.metrics.nsatc.net:443
prod-dsts.dsts.core.windows.net:443
ocsp.msocsp.com:80
*.windowsupdate.com:80
ocsp.digicert.com:80
go.microsoft.com:80
dmd.metaservices.microsoft.com:80
www.msftconnecttest.com:80
crl.microsoft.com:80
www.microsoft.com:80
adl.windows.com:80
crl3.digicert.com:80
```

Sie müssen außerdem die [Routingtabelle](/azure/virtual-network/virtual-networks-udr-overview) für das Subnetz mit den [Verwaltungsadressen](#azure-data-explorer-management-ip-addresses) und [Systemüberwachungsadressen](#health-monitoring-addresses) mit *Internet* als nächstem Hop definieren, um Probleme mit asymmetrischen Routen zu vermeiden.

Beispielsweise müssen für die Region **USA, Westen** die folgenden UDRs definiert werden:

| Name | Adresspräfix | Nächster Hop |
| --- | --- | --- |
| ADX_Management | 13.64.38.225/32 | Internet |
| ADX_Monitoring | 23.99.5.162/32 | Internet |

## <a name="deploy-azure-data-explorer-cluster-into-your-vnet-using-an-azure-resource-manager-template"></a>Bereitstellen eines Azure Data Explorer-Clusters in Ihrem VNET mithilfe einer Azure Resource Manager-Vorlage

Verwenden Sie zum Bereitstellen eines Azure Data Explorer-Clusters in Ihrem virtuellen Netzwerk die [entsprechende Azure Resource Manager-Vorlage](https://azure.microsoft.com/resources/templates/101-kusto-vnet/).

Mit dieser Vorlage werden der Cluster, das virtuelle Netzwerk, das Subnetz, die Netzwerksicherheitsgruppe und die öffentlichen IP-Adressen erstellt.

## <a name="troubleshooting"></a>Problembehandlung

In diesem Abschnitt erfahren Sie, wie Sie Konnektivitäts-, Betriebs- und Clustererstellungsprobleme bei einem Cluster beheben können, der in Ihrem [virtuellen Netzwerk](/azure/virtual-network/virtual-networks-overview) bereitgestellt wird.

### <a name="access-issues"></a>Zugriffsprobleme

Wenn beim Zugriff auf den Cluster über den öffentlichen („cluster.region.kusto.windows.net“) oder privaten („private-cluster.region.kusto.windows.net“) Endpunkt ein Problem auftritt und Sie vermuten, dass er im Zusammenhang mit dem Einrichten des virtuellen Netzwerks steht, führen Sie die folgenden Schritte zur Problembehandlung aus.

#### <a name="check-tcp-connectivity"></a>Überprüfen der TCP-Konnektivität

Der erste Schritt ist die Überprüfung der TCP-Konnektivität mithilfe von Windows oder Linux OS.

# <a name="windows"></a>[Windows](#tab/windows)

   1. Laden Sie [TCping](https://www.elifulkerson.com/projects/tcping.php) auf den Computer herunter, der eine Verbindung mit dem Cluster herstellt.
   2. Pingen Sie das Ziel vom Quellcomputer aus mit folgendem Befehl:

    ```cmd
     C:\> tcping -t yourcluster.kusto.windows.net 443 
    
     ** Pinging continuously.  Press control-c to stop **
    
     Probing 1.2.3.4:443/tcp - Port is open - time=100.00ms
     ```

# <a name="linux"></a>[Linux](#tab/linux)

   1. Installieren Sie *netcat* auf dem Computer, der eine Verbindung mit dem Cluster herstellt.

    ```bash
    $ apt-get install netcat
     ```

   2. Pingen Sie das Ziel vom Quellcomputer aus mit folgendem Befehl:

     ```bash
     $ netcat -z -v yourcluster.kusto.windows.net 443
    
     Connection to yourcluster.kusto.windows.net 443 port [tcp/https] succeeded!
     ```
---

Wenn der Test nicht erfolgreich war, setzen Sie den Vorgang mit den folgenden Schritten fort. Ist der Test aber erfolgreich, wurde das Problem nicht durch ein TCP-Konnektivitätsproblem verursacht. Wechseln Sie zu [Betriebsprobleme](#cluster-creation-and-operations-issues), um mehr zur Problembehandlung zu erfahren.

#### <a name="check-the-network-security-group-nsg"></a>Überprüfen der Netzwerksicherheitsgruppe (NSG)

   Überprüfen Sie, ob es bei der [Netzwerksicherheitsgruppe](/azure/virtual-network/security-overview) (NSG), die an das Subnetz des Clusters angefügt ist, eine eingehende Regel gibt, die den Zugriff auf die IP-Adresse des Clientcomputers für Port 443 zulässt.

#### <a name="check-route-table"></a>Überprüfen der Routingtabelle

   Wenn beim Subnetz des Clusters eine Tunnelerzwingung zur Firewall eingerichtet wurde (Subnetz mit einer [Routentabelle](/azure/virtual-network/virtual-networks-udr-overview), in der die Standardroute '0.0.0.0/0' enthalten ist), vergewissern Sie sich, dass es in der IP-Adresse des Computers eine Route mit [Typ des nächsten Hops](/azure/virtual-network/virtual-networks-udr-overview) zum virtuellen Netzwerk/Internet gibt. Dies ist erforderlich, um Probleme mit asymmetrischen Routen zu vermeiden.

### <a name="ingestion-issues"></a>Erfassungsprobleme

Wenn bei der Erfassung Probleme auftreten und Sie vermuten, dass sie im Zusammenhang mit der Einrichtung des virtuellen Netzwerks stehen, führen Sie die folgenden Schritte aus.

#### <a name="check-ingestion-health"></a>Überprüfen der Erfassungsintegrität

    Check that the [cluster ingestion metrics](/azure/data-explorer/using-metrics#ingestion-health-and-performance-metrics) indicate a healthy state.

#### <a name="check-security-rules-on-data-source-resources"></a>Überprüfen von Sicherheitsregeln für Datenquellenressourcen

Wenn die Metriken angeben, dass keine Ereignisse aus der Datenquelle verarbeitet wurden (Metrik „*Events processed* (Ereignisse verarbeitet) für Ereignis/IoT Hubs), vergewissern Sie sich, dass die Datenquellenressourcen (Event Hub oder Speicher) den Zugriff aus dem Subnetz des Clusters in den Firewallregeln oder Dienstendpunkten zulassen.

#### <a name="check-security-rules-configured-on-clusters-subnet"></a>Überprüfen von Sicherheitsregeln, die im Subnetz des Clusters konfiguriert wurden

Vergewissern Sie sich, dass die NSG-, UDR- und Firewallregeln für das Subnetz des Clusters ordnungsgemäß konfiguriert wurden. Testen Sie außerdem die Netzwerkkonnektivität bei allen abhängigen Endpunkten. 

### <a name="cluster-creation-and-operations-issues"></a>Probleme bei der Clustererstellung und dem Betrieb

Wenn bei der Clustererstellung oder dem Betrieb Probleme auftreten und Sie vermuten, dass sie im Zusammenhang mit der Einrichtung des virtuellen Netzwerks stehen, führen Sie die folgenden Schritte zur Problembehandlung aus.

#### <a name="diagnose-the-virtual-network-with-the-rest-api"></a>Diagnostizieren des virtuellen Netzwerks mit der REST-API

Der [ARMClient](https://chocolatey.org/packages/ARMClient) dient zum Aufrufen der REST-API mithilfe von PowerShell. 

1. Anmelden mit ARMClient

   ```powerShell
   armclient login
   ```

1. Diagnosevorgang aufrufen

    ```powershell
    $subscriptionId = '<subscription id>'
    $clusterName = '<name of cluster>'
    $resourceGroupName = '<resource group name>'
    $apiversion = '2019-11-09'
    
    armclient post "https://management.azure.com/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.Kusto/clusters/$clusterName/diagnoseVirtualNetwork?api-version=$apiversion" -verbose
    ```

1. Die Antwort überprüfen

    ```powershell
    HTTP/1.1 202 Accepted
    ...
    Azure-AsyncOperation: https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Kusto/locations/{location}/operationResults/{operation-id}?api-version=2019-11-09
    ...
    ```

1. Auf Abschluss des Vorgangs warten

    ```powershell
    armclient get https://management.azure.com/subscriptions/$subscriptionId/providers/Microsoft.Kusto/locations/{location}/operationResults/{operation-id}?api-version=2019-11-09
    
    {
      "id": "/subscriptions/{subscription-id}/providers/Microsoft.Kusto/locations/{location}/operationresults/{operation-id}",
      "name": "{operation-name}",
      "status": "[Running/Failed/Completed]",
      "startTime": "{start-time}",
      "endTime": "{end-time}",
      "properties": {...}
    }
    ```
    
   Warten Sie, bis die Eigenschaft *status* die Meldung *Completed* (Abgeschlossen) anzeigt. Dann sollte im Feld *properties* (Eigenschaften) Folgendes angezeigt werden:

    ```powershell
    {
      "id": "/subscriptions/{subscription-id}/providers/Microsoft.Kusto/locations/{location}/operationresults/{operation-id}",
      "name": "{operation-name}",
      "status": "Completed",
      "startTime": "{start-time}",
      "endTime": "{end-time}",
      "properties": {
        "Findings": [...]
      }
    }
    ```

Wenn die Eigenschaft *Findings* (Ergebnisse) ein leeres Ergebnis anzeigt, bedeutet dies, das alle Netzwerktests erfolgreich waren und keine Verbindungen unterbrochen sind. Wenn aber dieser Fehler angezeigt wird: *Outbound dependency '{dependencyName}:{port}' might be not satisfied (Outbound)* (Ausgehende Abhängigkeit '{NameDerAbhängigkeit}:{Port}' wurde möglicherweise nicht berücksichtigt(Ausgehend)), kann der Cluster die abhängigen Dienstendpunkte nicht erreichen. Setzen Sie den Vorgang mit den folgenden Schritten zur Problembehandlung fort.

#### <a name="check-network-security-group-nsg"></a>Überprüfen der Netzwerksicherheitsgruppe (NSG)

Vergewissern Sie sich, dass die [Netzwerksicherheitsgruppe](/azure/virtual-network/security-overview) entsprechend den Anleitungen in [Abhängigkeiten für die VNET-Bereitstellung](/azure/data-explorer/vnet-deployment#dependencies-for-vnet-deployment) ordnungsgemäß konfiguriert wurde.

#### <a name="check-route-table"></a>Überprüfen der Routingtabelle

Wenn beim Subnetz des Clusters eine Tunnelerzwingung zur Firewall eingerichtet wurde (Subnetz mit einer [Routentabelle](/azure/virtual-network/virtual-networks-udr-overview), in der die Standardroute '0.0.0.0/0' enthalten ist), vergewissern Sie sich, dass es in den [Verwaltungs-IP-Adressen](#azure-data-explorer-management-ip-addresses) und [IP-Adressen zur Systemüberwachung](#health-monitoring-addresses) eine Route mit [Typ des nächsten Hops](/azure/virtual-network/virtual-networks-udr-overview##next-hop-types-across-azure-tools), *Internet* und [Quelladresspräfix](/azure/virtual-network/virtual-networks-udr-overview#how-azure-selects-a-route) zu *'management-ip/32'* und *'health-monitoring-ip/32'* gibt. Dies ist erforderlich, um Probleme mit asymmetrischen Routen zu vermeiden.

#### <a name="check-firewall-rules"></a>Überprüfen von Firewallregeln

Wenn Sie den ausgehenden Datenverkehr des Tunnelsubnetzes an eine Firewall erzwingen, vergewissern Sie sich, dass alle Abhängigkeiten mit FQDN (z. B. *.blob.core.windows.net*) in der Firewallkonfiguration zulässig sind, wie in [Sichern von ausgehendem Datenverkehr mit Firewall](/azure/data-explorer/vnet-deployment#securing-outbound-traffic-with-firewall) beschrieben wird.
