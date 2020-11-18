---
title: 'Erstellen eines Verbindungsmonitors: PowerShell'
titleSuffix: Azure Network Watcher
description: Hier erfahren Sie, wie Sie über PowerShell einen Verbindungsmonitor erstellen.
services: network-watcher
documentationcenter: na
author: vinigam
ms.service: network-watcher
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/30/2020
ms.author: vinigam
ms.openlocfilehash: 532f045233f26a9a2933a19ae7a0a893195ad33f
ms.sourcegitcommit: 8a1ba1ebc76635b643b6634cc64e137f74a1e4da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2020
ms.locfileid: "94384055"
---
# <a name="create-a-connection-monitor-using-powershell"></a>Erstellen eines Verbindungsmonitors mithilfe von PowerShell

Hier erfahren Sie, wie Sie mithilfe von PowerShell einen Verbindungsmonitor erstellen, um die Kommunikation zwischen Ihren Ressourcen zu überwachen.

## <a name="before-you-begin"></a>Voraussetzungen 

In Verbindungsmonitoren, die Sie im Feature „Verbindungsmonitor“ erstellen, können Sie sowohl lokale Computer als auch Azure-VMs als Quellen hinzufügen. Mit diesen Verbindungsmonitoren kann auch die Konnektivität mit Endpunkten überwacht werden. Die Endpunkte können sich in Azure oder einer beliebigen anderen URL oder IP-Adresse befinden.

Der Verbindungsmonitor umfasst die folgenden Entitäten:

* **Verbindungsmonitorressource**: Dies ist eine regionsspezifische Azure-Ressource. Alle folgenden Entitäten sind Eigenschaften einer Verbindungsmonitorressource.
* **Endpunkt**: Eine Quelle oder ein Ziel, die bzw. das an Konnektivitätsprüfungen beteiligt ist. Beispiele für Endpunkte umfassen Azure-VMs, lokale Agents, URLs und IP-Adressen.
* **Testkonfiguration**: Eine protokollspezifische Konfiguration für einen Test. Je nach ausgewähltem Protokoll können Sie den Port, Schwellenwerte, die Testhäufigkeit und weitere Parameter definieren.
* **Testgruppe**: Die Gruppe, die Quellendpunkte, Zielendpunkte und Testkonfigurationen enthält. Ein Verbindungsmonitor kann mehrere Testgruppen enthalten.
* **Test**: Die Kombination aus einem Quellendpunkt, einem Zielendpunkt und einer Testkonfiguration. Ein Test ist die differenzierteste Ebene, auf der Überwachungsdaten verfügbar sind. Die Überwachungsdaten umfassen den Prozentsatz von Überprüfungen mit Fehlern und die Roundtripzeit.

    ![Diagramm eines Verbindungsmonitors mit Definition der Beziehung zwischen Testgruppen und Tests](./media/connection-monitor-2-preview/cm-tg-2.png)

## <a name="steps-to-create-with-powershell"></a>Schritte für die Erstellung mit PowerShell

Verwenden Sie die folgenden Befehle, um über PowerShell einen Verbindungsmonitor zu erstellen.

```powershell

//Connect to your Azure account with the subscription
Connect-AzAccount
Select-AzSubscription -SubscriptionId <your-subscription>
//Select region
$nw = "NetworkWatcher_centraluseuap"
//Declare endpoints like Azure VM below. You can also give VNET,Subnet,Log Analytics workspace
$sourcevmid1 = New-AzNetworkWatcherConnectionMonitorEndpointObject -Name MyAzureVm -ResourceID /subscriptions/<your-subscription>/resourceGroups/<your resourceGroup>/providers/Microsoft.Compute/virtualMachines/<vm-name>
//Declare endpoints like URL, IPs
$bingEndpoint = New-AzNetworkWatcherConnectionMonitorEndpointObject -name Bing -Address www.bing.com # Destination URL
//Create test configuration.Choose Protocol and parametersSample configs below.

$IcmpProtocolConfiguration = New-AzNetworkWatcherConnectionMonitorProtocolConfigurationObject -IcmpProtocol
$TcpProtocolConfiguration = New-AzNetworkWatcherConnectionMonitorProtocolConfigurationObject -TcpProtocol -Port 80
$httpProtocolConfiguration = New-AzNetworkWatcherConnectionMonitorProtocolConfigurationObject -HttpProtocol -Port 443 -Method GET -RequestHeader @{Allow = "GET"} -ValidStatusCodeRange 2xx, 300-308 -PreferHTTPS
$httpTestConfiguration = New-AzNetworkWatcherConnectionMonitorTestConfigurationObject -Name http-tc -TestFrequencySec 60 -ProtocolConfiguration $httpProtocolConfiguration -SuccessThresholdChecksFailedPercent 20 -SuccessThresholdRoundTripTimeMs 30
$icmpTestConfiguration = New-AzNetworkWatcherConnectionMonitorTestConfigurationObject -Name icmp-tc -TestFrequencySec 30 -ProtocolConfiguration $icmpProtocolConfiguration -SuccessThresholdChecksFailedPercent 5 -SuccessThresholdRoundTripTimeMs 500
$tcpTestConfiguration = New-AzNetworkWatcherConnectionMonitorTestConfigurationObject -Name tcp-tc -TestFrequencySec 60 -ProtocolConfiguration $TcpProtocolConfiguration -SuccessThresholdChecksFailedPercent 20 -SuccessThresholdRoundTripTimeMs 30
//Create Test Group
$testGroup1 = New-AzNetworkWatcherConnectionMonitorTestGroupObject -Name testGroup1 -TestConfiguration $httpTestConfiguration, $tcpTestConfiguration, $icmpTestConfiguration -Source $sourcevmid1 -Destination $bingEndpoint,
$testname = "cmtest9"
//Create Connection Monitor
New-AzNetworkWatcherConnectionMonitor -NetworkWatcherName $nw -ResourceGroupName NetworkWatcherRG -Name $testname -TestGroup $testGroup1

```

## <a name="description-of-properties"></a>Beschreibung der Eigenschaften

* connectionMonitorName: Der Name der Verbindungsmonitorressource.

* SUB: Die Abonnement-ID des Abonnements, in dem Sie den Verbindungsmonitor erstellen möchten.

* NW: Die ID der Network Watcher-Ressource, in der der Verbindungsmonitor erstellt wird. 

* location: Die Region, in der der Verbindungsmonitor erstellt wird.

* Endpunkte
    * name: Der eindeutige Name für jeden Endpunkt.
    * resourceId: Bei Azure-Endpunkten bezieht sich die Ressourcen-ID auf die ID der Azure Resource Manager-Ressource für virtuelle Computer. Bei Azure-fremden Endpunkten bezieht sich die Ressourcen-ID auf die ID der Azure Resource Manager-Ressource für den Log Analytics-Arbeitsbereich, der mit Nicht-Azure-Agents verknüpft ist.
    * address: Dies trifft nur zu, wenn entweder keine Ressourcen-ID angegeben oder die Ressourcen-ID der Log Analytics-Arbeitsbereich ist. Bei Verwendung mit Log Analytics-Ressourcen-ID bezieht sich dies auf den FQDN des Agents, der für die Überwachung verwendet werden kann. Bei Verwendung ohne Ressourcen-ID kann dies die URL oder IP-Adresse eines beliebigen öffentlichen Endpunkts sein.
    * filter: Bei Azure-fremden Endpunkten verwenden Sie Filter, um Agents aus dem Log Analytics-Arbeitsbereich auszuwählen, die für die Überwachung in der Verbindungsmonitorressource verwendet werden. Wenn keine Filter festgelegt sind, können alle Agents, die zum Log Analytics-Arbeitsbereich gehören, für die Überwachung verwendet werden.
        * type: Legen Sie den Typ auf „Agent Address“ fest.
        * address: Legen Sie die Adresse als den FQDN Ihres lokalen Agents fest.

* Testgruppen
    * name: Geben Sie der Testgruppe einen Namen.
    * testConfigurations: Die Testkonfigurationen, die darauf basieren, welche Quellendpunkte mit Zielendpunkten verbunden sind.
    * sources: Treffen Sie eine Auswahl aus den oben erstellten Endpunkten. Auf Azure-basierten Quellendpunkten muss die Azure Network Watcher-Erweiterung installiert sein, und bei nicht auf Azure basierenden Quellendpunkten muss der Azure Log Analytics-Agent installiert sein. Informationen zum Installieren eines Agents für Ihre Quelle finden Sie unter [Installieren von Überwachungs-Agents](https://docs.microsoft.com/azure/network-watcher/connection-monitor-preview#install-monitoring-agents).
    * destinations: Treffen Sie eine Auswahl aus den oben erstellten Endpunkten. Sie können die Konnektivität mit Azure-VMs oder anderen Endpunkten (öffentliche IP-Adresse, URL oder FQDN) überwachen, indem Sie diese als Ziele festlegen. In einer einzelnen Testgruppe können Sie Azure-VMs, Office 365-URLs, Dynamics 365-URLs und benutzerdefinierte Endpunkte hinzufügen.
    * disable: Verwenden Sie dieses Feld, um die Überwachung für alle in der Testgruppe angegebenen Quellen und Ziele zu deaktivieren.

* Testkonfigurationen
    * name: Der Name der Testkonfiguration.
    * testFrequencySec: Geben Sie an, wie oft die Quellen die Ziele über das angegebene Protokoll und den angegebenen Port pingen. Sie können Intervalle von 30 Sekunden, 1 Minute, 5 Minuten, 15 Minuten oder 30 Minuten auswählen. Die Quellen testen die Konnektivität mit den Zielen auf Grundlage des von Ihnen ausgewählten Werts. Wenn Sie z. B. 30 Sekunden auswählen, überprüfen die Quellen die Konnektivität mit dem Ziel mindestens einmal alle 30 Sekunden.
    * protocol: Sie können TCP, ICMP, HTTP oder HTTPS auswählen. Je nach Protokoll können Sie einige protokollspezifische Konfigurationen vornehmen.
        * preferHTTPS: Geben Sie an, ob vorzugsweise HTTPS statt HTTP verwendet werden soll.
        * port: Geben Sie den Zielport Ihrer Wahl an.
        * disableTraceRoute: Dies betrifft Testgruppen mit TCP- oder ICMP-Protokoll. Dadurch werden von Quellen weder Topologie noch Hop-by-Hop-Roundtripzeit ermittelt.
    * successThreshold: Sie können Schwellenwerte für die folgenden Netzwerkparameter festlegen:
        * checksFailedPercent: Legen Sie den Prozentsatz der Überprüfungen fest, die Fehler aufweisen können, wenn Quellen die Konnektivität mit Zielen anhand der von Ihnen angegebenen Kriterien überprüfen. Beim TCP- oder ICMP-Protokoll kann der Prozentsatz der Überprüfungen mit Fehlern dem Prozentsatz der Paketverluste gleichgesetzt werden. Beim HTTP-Protokoll stellt dieses Feld den Prozentsatz von HTTP-Anforderungen dar, die keine Antwort erhielen.
        * roundTripTimeMs: Legen Sie die Roundtripzeit in Millisekunden fest, die von Quellen zum Herstellen einer Verbindung mit dem Ziel über die Testkonfiguration benötigt werden darf.

## <a name="scale-limits"></a>Skalierungslimits

Für Verbindungsmonitore gelten die folgenden Skalierungslimits:

* Maximale Anzahl von Verbindungsmonitoren pro Abonnement und Region: 100
* Maximale Anzahl von Testgruppen pro Verbindungsmonitor: 20
* Maximale Anzahl von Quellen und Zielen pro Verbindungsmonitor: 100
* Maximale Anzahl von Testkonfigurationen pro Verbindungsmonitor:

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie, wie Sie [Überwachungsdaten analysieren und Warnungen festlegen](https://docs.microsoft.com/azure/network-watcher/connection-monitor-preview#analyze-monitoring-data-and-set-alerts).
* Erfahren Sie, wie Sie [Probleme in Ihrem Netzwerk diagnostizieren](https://docs.microsoft.com/azure/network-watcher/connection-monitor-preview#diagnose-issues-in-your-network).
