---
title: Verbindungsmonitor (Vorschauversion) | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie mit dem Verbindungsmonitor (Vorschau) die Netzwerkkommunikation in einer verteilten Umgebung überwachen.
services: network-watcher
documentationcenter: na
author: vinynigam
manager: agummadi
editor: ''
tags: azure-resource-manager
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/27/2020
ms.author: vinigam
ms.custom: mvc
ms.openlocfilehash: d926a9f686f0f4c39203b8a217a7c608cfad926e
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/02/2020
ms.locfileid: "80548116"
---
# <a name="network-connectivity-monitoring-with-connection-monitor-preview"></a>Überwachung der Netzwerkkonnektivität mit dem Verbindungsmonitor (Vorschau)

Der Verbindungsmonitor (Vorschau) bietet eine einheitliche End-to-End-Verbindungsüberwachung in Azure Network Watcher. Der Verbindungsmonitor (Vorschau) unterstützt Hybrid- und Azure-Cloudbereitstellungen. Network Watcher stellt Tools für das Überwachen, Diagnostizieren und Anzeigen von Verbindungsmetriken für Ihre Azure-Bereitstellungen zur Verfügung.

Nachfolgend sind einige Anwendungsfälle für den Verbindungsmonitor (Vorschau) aufgeführt:

- Ihre Front-End-Webserver-VM kommuniziert in einer Anwendung mit mehreren Ebenen mit einer Datenbankserver-VM, und Sie möchten die Netzwerkkonnektivität zwischen beiden VMs überprüfen.
- Sie möchten, dass VMs in der Region „USA, Osten“ VMs in der Region „USA, Mitte“ pingen, und Sie möchten die regionsübergreifenden Netzwerklatenzen vergleichen.
- Sie verfügen über mehrere lokale Unternehmensstandorte in Seattle, Washington, und in Ashburn, Virginia. Ihre Unternehmensstandorte sind mit Office 365-URLs verbunden. Sie möchten für die Benutzer der Office 365-URLs die Latenzen in Seattle und Ashburn vergleichen.
- Ihre Hybridanwendung benötigt eine Verbindung mit einem Azure Storage-Endpunkt. Der lokale Standort und die Azure-Anwendung sind mit dem gleichen Azure Storage-Endpunkt verbunden. Sie möchten die Latenzen des lokalen Standorts mit den Latenzen der Azure-Anwendung vergleichen.
- Sie möchten die Konnektivität zwischen den lokalen Setups und den Azure-VMs überprüfen, auf denen die Cloudanwendung gehostet wird.

In der Vorschauphase bietet der Verbindungsmonitor eine Kombination aus dem Besten von zwei Features: dem Feature [Verbindungsmonitor](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview#monitor-communication-between-a-virtual-machine-and-an-endpoint) von Network Watcher und dem Feature [Dienstkonnektivitätsmonitor](https://docs.microsoft.com/azure/azure-monitor/insights/network-performance-monitor-service-connectivity) des Netzwerkleistungsmonitors (NPM).

Nachfolgend sind einige Vorteile des Verbindungsmonitors (Vorschau) aufgeführt:

* Einheitliche, intuitive Oberfläche zum Überwachen von Azure- und Hybridbereitstellungen
* Regionsübergreifende und arbeitsbereichsübergreifende Konnektivitätsüberwachung
* Häufigere Prüfungen und bessere Einsicht in die Netzwerkleistung
* Schnellere Warnungen für Ihre Hybridbereitstellungen
* Unterstützung für Konnektivitätsprüfungen basierend auf HTTP, TCP und ICMP 
* Metriken und Unterstützung für Log Analytics, sowohl für Azure- als auch für Nicht-Azure-Testsetups

![Diagramm der Interaktion des Verbindungsmonitors mit Azure-VMs, Nicht-Azure-Hosts, Endpunkten und Datenspeicherorten](./media/connection-monitor-2-preview/hero-graphic.png)

Führen Sie die folgenden Schritte aus, um den Verbindungsmonitor (Vorschau) für die Überwachung zu verwenden: 

1. Installieren Sie Überwachungs-Agents.
1. Aktivieren Sie Network Watcher für Ihr Abonnement.
1. Erstellen Sie einen Verbindungsmonitor.
1. Richten Sie die Datenanalyse und Warnungen ein.
1. Diagnostizieren Sie Problemen in Ihrem Netzwerk.

Die folgenden Abschnitte enthalten weitere Informationen zu diesen Schritten.

## <a name="install-monitoring-agents"></a>Installieren von Überwachungs-Agents

Der Verbindungsmonitor verwendet einfache ausführbare Dateien zum Ausführen von Konnektivitätsprüfungen.  Er unterstützt Konnektivitätsprüfungen sowohl in Azure-Umgebungen als auch lokalen Umgebungen. Die verwendete ausführbare Datei hängt davon ab, ob Ihre VM in Azure oder lokal gehostet wird.

### <a name="agents-for-azure-virtual-machines"></a>Agents für Azure-VMs

Damit der Verbindungsmonitor Ihre Azure-VMs als Überwachungsquellen erkennt, installieren Sie die VM-Erweiterung für den Network Watcher-Agent auf den VMs. Diese Erweiterung wird auch als *Network Watcher-Erweiterung* bezeichnet. Für Azure-VMs muss die Erweiterung eine End-to-End-Überwachung und andere erweiterte Funktionen auslösen. 

Sie können die Network Watcher-Erweiterung beim [Erstellen einer VM](https://docs.microsoft.com/azure/network-watcher/connection-monitor#create-the-first-vm) installieren. Die Network Watcher-Erweiterung für [Linux](https://docs.microsoft.com/azure/virtual-machines/extensions/network-watcher-linux) und [Windows](https://docs.microsoft.com/azure/virtual-machines/extensions/network-watcher-windows) kann auch separat installiert und konfiguriert sowie eine separate Problembehandlung durchgeführt werden.

Durch Regeln für eine Netzwerksicherheitsgruppe (NSG) oder Firewall kann die Kommunikation zwischen Quelle und Ziel blockiert werden. Der Verbindungsmonitor erkennt dieses Problem und zeigt es als Diagnosemeldung in der Topologie an. Damit eine Verbindungsüberwachung möglich ist, müssen Sie zunächst sicherstellen, dass die NSG- und Firewallregeln eine Paketübertragung über TCP oder ICMP zwischen der Quelle und dem Ziel zulassen.

### <a name="agents-for-on-premises-machines"></a>Agents für lokale Computer

Damit der Verbindungsmonitor die lokalen Computer als Quellen für die Überwachung erkennt, installieren Sie den Log Analytics-Agent auf den Computern. Dann aktivieren Sie die Netzwerkleistungsmonitor-Lösung. Diese Agents sind mit den Log Analytics-Arbeitsbereichen verknüpft. Daher müssen Sie die Arbeitsbereichs-ID und den Primärschlüssel einrichten, bevor die Agents mit der Überwachung beginnen können.

Informationen zum Installieren des Log Analytics-Agents für Windows-Computer finden Sie unter [Azure Monitor-VM-Erweiterung für Windows](https://docs.microsoft.com/azure/virtual-machines/extensions/oms-windows).

Wenn der Pfad Firewalls oder virtuelle Netzwerkgeräte (NVAs) umfasst, stellen Sie sicher, dass das Ziel erreichbar ist.

## <a name="enable-network-watcher-on-your-subscription"></a>Aktivieren von Network Watcher für Ihr Abonnement

Für alle Abonnements, die über ein virtuelles Netzwerk verfügen, wird Network Watcher aktiviert. Wenn Sie ein virtuelles Netzwerk in Ihrem Abonnement erstellen, wird Network Watcher automatisch in der Region und für das Abonnement des virtuellen Netzwerks aktiviert. Diese automatische Aktivierung wirkt sich nicht auf Ihre Ressourcen aus, und es fallen auch keine Gebühren an. Stellen Sie sicher, dass Network Watcher für Ihr Abonnement nicht explizit deaktiviert ist. 

Weitere Informationen finden Sie unter [Aktivieren von Network Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-create).

## <a name="create-a-connection-monitor"></a>Erstellen eines Verbindungsmonitors 

Der Verbindungsmonitor überwacht die Kommunikation in regelmäßigen Abständen. Er informiert Sie über Änderungen der Erreichbarkeit und Latenz. Sie können auch die aktuelle und die frühere Netzwerktopologie zwischen Quell-Agents und Zielendpunkten überprüfen.

Ziele können Azure-VMs oder lokale Computer sein, auf denen ein Überwachungs-Agent installiert ist. Zielendpunkte können Office 365-URLs, Dynamics 365-URLs, benutzerdefinierte URLs, Azure-VM-Ressourcen-IDs, IPv4/IPv6-Adressen, FQDNs oder beliebige Domänennamen sein.

### <a name="access-connection-monitor-preview"></a>Zugreifen auf den Verbindungsmonitor (Vorschau)

1. Navigieren Sie auf der Startseite des Azure-Portals zu **Network Watcher**.
1. Wählen Sie links im Abschnitt **Überwachung** die Option **Verbindungsmonitor (Vorschau)** aus.
1. Es werden alle Verbindungsmonitore angezeigt, die im Verbindungsmonitor (Vorschau) erstellt wurden. Zum Anzeigen der Verbindungsmonitore, die auf der klassischen Benutzeroberfläche des Verbindungsmonitors erstellt wurden, wechseln Sie zur Registerkarte **Verbindungsmonitor**.

    ![Screenshot: Verbindungsmonitore, die im Verbindungsmonitor (Vorschau) erstellt wurden](./media/connection-monitor-2-preview/cm-resource-view.png)


### <a name="create-a-connection-monitor"></a>Erstellen eines Verbindungsmonitors

In Verbindungsmonitoren, die Sie im Verbindungsmonitor (Vorschau) erstellen, können Sie sowohl lokale Computer als auch Azure-VMs als Quellen hinzufügen. Mit diesen Verbindungsmonitoren kann auch die Konnektivität mit Endpunkten überwacht werden. Die Endpunkte können sich in Azure oder einer beliebigen anderen URL oder IP-Adresse befinden.

Der Verbindungsmonitor (Vorschau) umfasst die folgenden Entitäten:

* **Verbindungsmonitorressource**: Dies ist eine regionsspezifische Azure-Ressource. Alle folgenden Entitäten sind Eigenschaften einer Verbindungsmonitorressource.
* **Endpunkt**: Eine Quelle oder ein Ziel, die bzw. das an Konnektivitätsprüfungen beteiligt ist. Beispiele für Endpunkte umfassen Azure-VMs, lokale Agents, URLs und IP-Adressen.
* **Testkonfiguration**: Eine protokollspezifische Konfiguration für einen Test. Je nach ausgewähltem Protokoll können Sie den Port, Schwellenwerte, die Testhäufigkeit und weitere Parameter definieren.
* **Testgruppe**: Die Gruppe, die Quellendpunkte, Zielendpunkte und Testkonfigurationen enthält. Ein Verbindungsmonitor kann mehrere Testgruppen enthalten.
* **Test**: Die Kombination aus einem Quellendpunkt, einem Zielendpunkt und einer Testkonfiguration. Ein Test ist die differenzierteste Ebene, auf der Überwachungsdaten verfügbar sind. Die Überwachungsdaten umfassen den Prozentsatz von Überprüfungen mit Fehlern und die Roundtripzeit.

 ![Diagramm eines Verbindungsmonitors mit Definition der Beziehung zwischen Testgruppen und Tests](./media/connection-monitor-2-preview/cm-tg-2.png)

#### <a name="create-a-connection-monitor-from-the-azure-portal"></a>Erstellen eines Verbindungsmonitors über das Azure-Portal

Führen Sie die folgenden Schritte aus, um einen Verbindungsmonitor über das Azure-Portal zu erstellen:

1. Wählen Sie im Dashboard **Verbindungsmonitor (Vorschau)** oben links die Option **Erstellen** aus.
1. Geben Sie auf der Registerkarte **Grundlagen** die Informationen zu Ihrem Verbindungsmonitor ein:
   * **Name des Verbindungsmonitors**: Fügen Sie den Namen Ihres Verbindungsmonitors hinzu. Verwenden Sie die Standardbenennungsregeln für Azure-Ressourcen.
   * **Abonnement**: Wählen Sie ein Abonnement für Ihren Verbindungsmonitor aus.
   * **Region**: Wählen Sie eine Region für Ihren Verbindungsmonitor aus. Dabei kommen nur die Quell-VMs infrage, die in dieser Region erstellt wurden.
   * **Arbeitsbereichskonfiguration**: Der Arbeitsbereich enthält Ihre Überwachungsdaten. Sie können einen benutzerdefinierten Arbeitsbereich oder den Standardarbeitsbereich verwenden. 
       * Aktivieren Sie das Kontrollkästchen, um den Standardarbeitsbereich zu verwenden. 
       * Wenn Sie einen benutzerdefinierten Arbeitsbereich auswählen möchten, deaktivieren Sie das Kontrollkästchen. Wählen Sie dann das Abonnement und die Region für Ihren benutzerdefinierten Arbeitsbereich aus. 
1. Wählen Sie unten auf der Registerkarte die Option **Weiter: Testgruppen** aus.

   ![Screenshot: Registerkarte „Grundlagen“ im Verbindungsmonitor](./media/connection-monitor-2-preview/create-cm-basics.png)

1. Wählen Sie auf der Registerkarte **Testgruppen** die Option **+ Testgruppe** aus. Informationen zum Einrichten der Testgruppen finden Sie unter [Erstellen von Testgruppen im Verbindungsmonitor](#create-test-groups-in-a-connection-monitor). 
1. Wählen Sie unten auf der Registerkarte die Option **Weiter: Überprüfen + erstellen** aus, um Ihren Verbindungsmonitor zu überprüfen.

   ![Screenshot: Registerkarte „Testgruppen“ und Bereich zum Hinzufügen von Testgruppendetails](./media/connection-monitor-2-preview/create-tg.png)

1. Überprüfen Sie auf der Registerkarte **Überprüfen + erstellen** die grundlegenden Informationen und Testgruppen, bevor Sie den Verbindungsmonitor erstellen. Wenn Sie den Verbindungsmonitor bearbeiten müssen, gehen Sie folgendermaßen vor:
   * Zum Bearbeiten grundlegender Informationen wählen Sie das Stiftsymbol aus.
   * Zum Bearbeiten einer Testgruppe wählen Sie diese aus.

   > [!NOTE] 
   > Auf der Registerkarte **Überprüfen + erstellen** werden die Kosten pro Monat während der Vorschauphase des Verbindungsmonitors angezeigt. Derzeit werden in der Spalte **AKTUELLE KOSTEN** keine Kosten angezeigt. Sobald der Verbindungsmonitor allgemein verfügbar ist, wird in dieser Spalte eine monatliche Gebühr angegeben. 
   > 
   > Auch in der Vorschauphase des Verbindungsmonitors fallen Gebühren für die Log Analytics-Erfassung an.

1. Wenn Sie den Verbindungsmonitor erstellen möchten, wählen Sie unten auf der Registerkarte **Überprüfen + erstellen** die Option **Erstellen** aus.

   ![Screenshot: Verbindungsmonitor mit der Registerkarte „Überprüfen + erstellen“](./media/connection-monitor-2-preview/review-create-cm.png)

Der Verbindungsmonitor (Vorschau) erstellt die Verbindungsmonitorressource im Hintergrund.

#### <a name="create-a-connection-monitor-by-using-armclient"></a>Erstellen eines Verbindungsmonitors mithilfe von ARMClient

Verwenden Sie den folgenden Code, um einen Verbindungsmonitor mithilfe von ARMClient zu erstellen.

```armclient
$connectionMonitorName = "sampleConnectionMonitor"

$ARM = "https://management.azure.com"

$SUB = "subscriptions/<subscription id 1>;"

$NW = "resourceGroups/NetworkWatcherRG/providers/Microsoft.Network/networkWatchers/NetworkWatcher\_<region>"

$body =

"{

location: '<region>',

properties: {

endpoints: [{

name: 'workspace',

resourceId: '/subscriptions/<subscription id>/resourcegroups/<resource group>/providers/Microsoft.OperationalInsights/workspaces/sampleWorkspace',

filter: {

 items: [{

type: 'AgentAddress',

address: '<FQDN of your on-premises agent>'

}]

}

          },

 {

name: 'vm1',

resourceId: '/subscriptions/<subscription id>/resourceGroups/<resource group>/providers/Microsoft.Compute/virtualMachines/<vm-name>'

},

 {

name: 'vm2',

resourceId: '/subscriptions/<subscription id>/resourceGroups/<resource group>/providers/Microsoft.Compute/virtualMachines/<vm-name>'

   },

{

name: 'azure portal'

address: '<URL>'

   },

 {

    name: 'ip',

     address: '<IP>'

 }

  ],

  testGroups: [{

    name: 'Connectivity to Azure Portal and Public IP',

    testConfigurations: ['http', 'https', 'tcpEnabled', 'icmpEnabled'],

    sources: ['vm1', 'workspace'],

    destinations: ['azure portal', 'ip']

   },

{

    name: 'Connectivty from Azure VM 1 to Azure VM 2',

   // Choose your protocol
   
    testConfigurations: ['http', 'https', 'tcpDisabled', 'icmpDisabled'],

    sources: ['vm1'],

    destinations: ['vm2'],

    disable: true

   }

  ],

  testConfigurations: [{

    name: 'http',

    testFrequencySec: <frequency>,

    protocol: 'HTTP',

    successThreshold: {

     checksFailedPercent: <threshold for checks failed %>,

     roundTripTimeMs: <threshold for RTT>

    }

   }, {

    name: 'https',

    testFrequencySec: <frequency>,

    protocol: 'HTTP',

    httpConfiguration: {

     preferHTTPS: true

    },

    successThreshold: {

     checksFailedPercent: <choose your checks failed threshold>,

     roundTripTimeMs: <choose your RTT threshold>

    }

   }, {

    name: 'tcpEnabled',

    testFrequencySec: <frequency>,

    protocol: 'TCP',

    tcpConfiguration: {

     port: 80

    },

    successThreshold: {

     checksFailedPercent: <choose your checks failed threshold>,

     roundTripTimeMs: <choose your RTT threshold>

    }

   }, {

    name: 'icmpEnabled',

    testFrequencySec: <frequency>,

    protocol: 'ICMP',

    successThreshold: {

     checksFailedPercent: <choose your checks failed threshold>,

     roundTripTimeMs: <choose your RTT threshold>

    }

   }, {

    name: 'icmpDisabled',

    testFrequencySec: <frequency>,

    protocol: 'ICMP',

    icmpConfiguration: {

     disableTraceRoute: true

    },

    successThreshold: {

     checksFailedPercent: <choose your checks failed threshold>,

     roundTripTimeMs: <choose your RTT threshold>

    }

   }, {

    name: 'tcpDisabled',

    testFrequencySec: <frequency>,

    protocol: 'TCP',

    tcpConfiguration: {

     port: 80,

     disableTraceRoute: true

    },

    successThreshold: {

     checksFailedPercent: <choose your checks failed threshold>,

     roundTripTimeMs: <choose your RTT threshold>

    }

   }

  ]

 }

} "
```

Dies ist der Bereitstellungsbefehl:
```
armclient PUT $ARM/$SUB/$NW/connectionMonitors/$connectionMonitorName/?api-version=2019-07-01 $body -verbose
```

### <a name="create-test-groups-in-a-connection-monitor"></a>Erstellen von Testgruppen in einem Verbindungsmonitor

Jede Testgruppe in einem Verbindungsmonitor enthält Quellen und Ziele, die auf Netzwerkparameter getestet werden. Sie werden mithilfe von Testkonfigurationen auf den Prozentsatz von Überprüfungen mit Fehlern und die Roundtripzeit getestet.

Wenn Sie eine Testgruppe in einem Verbindungsmonitor über das Azure-Portal erstellen möchten, geben Sie Werte für die folgenden Felder an:

* **Testgruppe deaktivieren**: Sie können dieses Feld auswählen, um die Überwachung für alle in der Testgruppe angegebenen Quellen und Ziele zu deaktivieren. Diese Auswahl ist standardmäßig deaktiviert.
* **Name**: Geben Sie der Testgruppe einen Namen.
* **Quellen**: Sie können sowohl Azure-VMs als auch lokale Computer als Quellen angeben, wenn Agents darauf installiert sind. Informationen zum Installieren eines Agents für Ihre Quelle finden Sie unter [Installieren von Überwachungs-Agents](#install-monitoring-agents).
   * Wechseln Sie zum Auswählen von Azure-Agents zur Registerkarte **Azure-Agents**. Hier werden nur VMs angezeigt, die an die Region gebunden sind, die Sie beim Erstellen des Verbindungsmonitors angegeben haben. Standardmäßig sind VMs in den Abonnements gruppiert, denen Sie angehören. Diese Gruppen sind reduziert. 
   
       Sie können von der Abonnementebene einen Drilldown auf andere Hierarchieebenen ausführen:

      **Abonnement** > **Ressourcengruppen** > **VNETs** > **Subnetze** > **VMs mit Agents**

      Außerdem lässt sich der Wert des Felds **Gruppieren nach** ändern, sodass der Baum mit einer beliebigen anderen Ebene beginnt. Wenn Sie z. B. nach virtuellem Netzwerk gruppieren, werden die VMs mit Agents in der Hierarchie **VNETs** > **Subnetze** > **VMs mit Agents** angezeigt.

      ![Screenshot: Verbindungsmonitor mit dem Bereich „Quellen hinzufügen“ und der Registerkarte „Azure-Agents“](./media/connection-monitor-2-preview/add-azure-sources.png)

   * Zum Auswählen lokaler Agents wählen Sie die Registerkarte **Nicht-Azure-Agents** aus. Standardmäßig sind Agents in Arbeitsbereichen nach Region gruppiert. Für alle diese Arbeitsbereiche ist die Netzwerkleistungsmonitor-Lösung konfiguriert. 
   
       Wenn Sie dem Arbeitsbereich den Netzwerkleistungsmonitor hinzufügen müssen, rufen Sie ihn aus dem [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.NetworkMonitoringOMS?tab=Overview) ab. Informationen zum Hinzufügen des Netzwerkleistungsmonitors finden Sie unter [Überwachungslösungen in Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/insights/solutions). 
   
       In der Ansicht **Verbindungsmonitor erstellen** ist auf der Registerkarte **Grundlagen** die Standardregion ausgewählt. Wenn Sie die Region ändern, können Sie Agents aus Arbeitsbereichen in der neuen Region auswählen. Sie haben auch die Möglichkeit, den Wert im Feld **Gruppieren nach** zu ändern, sodass nach Subnetzen sortiert wird.

      ![Screenshot: Verbindungsmonitor mit dem Bereich „Quellen hinzufügen“ und der Registerkarte „Nicht-Azure-Agents“](./media/connection-monitor-2-preview/add-non-azure-sources.png)


   * Zum Überprüfen der ausgewählten Azure- und Nicht-Azure-Agents wechseln Sie zur Registerkarte **Überprüfen**.

      ![Screenshot: Verbindungsmonitor mit dem Bereich „Quellen hinzufügen“ und der Registerkarte „Überprüfen“](./media/connection-monitor-2-preview/review-sources.png)

   * Wenn Sie die Einrichtung von Quellen abgeschlossen haben, wählen Sie unten im Bereich **Quellen hinzufügen** die Option **Fertig** aus.

* **Ziele**: Sie können die Konnektivität mit Azure-VMs oder anderen Endpunkten (öffentliche IP-Adresse, URL oder FQDN) überwachen, indem Sie diese als Ziele festlegen. In einer einzelnen Testgruppe können Sie Azure-VMs, Office 365-URLs, Dynamics 365-URLs und benutzerdefinierte Endpunkte hinzufügen.

    * Wenn Sie Azure-VMs als Ziele auswählen möchten, wechseln Sie zur Registerkarte **Azure-VMs**. Die Azure-VMs sind standardmäßig in einer Abonnementhierarchie gruppiert, die sich in der gleichen Region befindet, die Sie in der Ansicht **Verbindungsmonitor erstellen** auf der Registerkarte **Grundlagen** ausgewählt haben. Sie können die Region ändern und die Azure-VMs aus der neuen Region auswählen. Anschließend können Sie von der Abonnementebene einen Drilldown auf andere Hierarchieebenen wie z. B. die Ebene der Azure-Agents ausführen.

       ![Screenshot: Bereich „Ziele hinzufügen“ mit der Registerkarte „Azure-VMs“](./media/connection-monitor-2-preview/add-azure-dests1.png)

       ![Screenshot: Bereich „Ziele hinzufügen“ mit der Abonnementebene](./media/connection-monitor-2-preview/add-azure-dests2.png)

    * Wenn Sie Endpunkte als Ziele auswählen möchten, wechseln Sie zur Registerkarte **Endpunkte**. Die Liste der Endpunkte umfasst die Office 365- und Dynamics 365-Test-URLs, die nach Namen gruppiert sind. Zusätzlich zu diesen Endpunkten können Sie auch einen Endpunkt auswählen, der in einer anderen Testgruppe im selben Verbindungsmonitor erstellt wurde. 
    
        Zum Hinzufügen eines neuen Endpunkts wählen Sie oben rechts die Option **+ Endpunkte** aus. Geben Sie dann einen Endpunktnamen und eine URL, eine IP-Adresse oder einen FQDN an.

       ![Screenshot: Hinzufügen von Endpunkten als Ziele im Verbindungsmonitor](./media/connection-monitor-2-preview/add-endpoints.png)

    * Zum Überprüfen der von Ihnen ausgewählten Azure-VMs und-Endpunkte wählen Sie die Registerkarte **Überprüfen** aus.
    * Wenn Sie die Auswahl von Zielen abgeschlossen haben, wählen Sie **Fertig** aus.

* **Testkonfigurationen**: Sie können Testkonfigurationen in einer Testgruppe zuordnen. Im Azure-Portal ist nur eine Testkonfiguration pro Testgruppe zulässig. Sie können aber mit ARMClient weitere Testkonfigurationen hinzufügen.

    * **Name**: Geben Sie der Testkonfiguration einen Namen.
    * **Protokoll**: Wählen Sie TCP, ICMP oder HTTP aus. Wenn Sie HTTP in HTTPS ändern möchten, wählen Sie **HTTP** als Protokoll und **443** als Port aus.
        * **Netzwerktestkonfiguration erstellen**: Dieses Kontrollkästchen wird nur angezeigt, wenn Sie im Feld **Protokoll** die Option **HTTP** ausgewählt haben. Aktivieren Sie dieses Kontrollkästchen, um eine weitere Testkonfiguration zu erstellen, die dieselben Quellen und Ziele verwendet, die Sie an anderer Stelle in der Konfiguration angegeben haben. Die neu erstellte Testkonfiguration erhält den Namen `<the name of your test configuration>_networkTestConfig`.
        * **Traceroute deaktivieren** Dieses Feld gilt für Testgruppen mit TCP- oder ICMP-Protokoll. Aktivieren Sie dieses Kontrollkästchen, damit Quellen weder die Topologie noch Hop-by-Hop-Roundtripzeit ermitteln.
    * **Zielport**: Sie können dieses Feld mit einem Zielport Ihrer Wahl anpassen.
    * **Testhäufigkeit**: Mit diesem Feld können Sie auswählen, wie oft die Quellen die Ziele über das angegebene Protokoll und den angegebenen Port pingen. Sie können Intervalle von 30 Sekunden, 1 Minute, 5 Minuten, 15 Minuten oder 30 Minuten auswählen. Die Quellen testen die Konnektivität mit den Zielen auf Grundlage des von Ihnen ausgewählten Werts.  Wenn Sie z. B. 30 Sekunden auswählen, überprüfen die Quellen die Konnektivität mit dem Ziel mindestens einmal alle 30 Sekunden.
    * **Schwellenwert für erfolgreiche Tests**: Sie können Schwellenwerte für die folgenden Netzwerkparameter festlegen:
       * **Überprüfungen mit Fehlern**: Legen Sie den Prozentsatz der Überprüfungen fest, die Fehler aufweisen können, wenn Quellen die Konnektivität mit Zielen anhand der von Ihnen angegebenen Kriterien überprüfen. Beim TCP- oder ICMP-Protokoll kann der Prozentsatz der Überprüfungen mit Fehlern dem Prozentsatz der Paketverluste gleichgesetzt werden. Beim HTTP-Protokoll stellt dieses Feld den Prozentsatz von HTTP-Anforderungen dar, die keine Antwort erhielen.
       * **Roundtripzeit**: Legen Sie die Roundtripzeit in Millisekunden fest, die von Quellen zum Herstellen einer Verbindung mit dem Ziel über die Testkonfiguration benötigt werden darf.
    
       ![Screenshot: Einrichten einer Testkonfiguration im Verbindungsmonitor](./media/connection-monitor-2-preview/add-test-config.png)

Alle Quellen, Ziele und Testkonfigurationen, die Sie einer Testgruppe hinzufügen, werden auf einzelne Tests aufgeteilt. Hier folgt ein Beispiel dafür, wie Quellen und Ziele aufgeteilt werden:

* Testgruppe: TG1
* Quellen: 3 (A, B, C)
* Ziele: 2 (D, E)
* Testkonfigurationen: 2 (Config 1, Config 2)
* Gesamtzahl erstellter Tests: 12

| Testnummer | `Source` | Destination | Testkonfiguration |
| --- | --- | --- | --- |
| 1 | Ein | D | Config 1 |
| 2 | Ein | D | Config 2 |
| 3 | Ein | E | Config 1 |
| 4 | Ein | E | Config 2 |
| 5 | B | D | Config 1 |
| 6 | B | D | Config 2 |
| 7 | B | E | Config 1 |
| 8 | B | E | Config 2 |
| 9 | C | D | Config 1 |
| 10 | C | D | Config 2 |
| 11 | C | E | Config 1 |
| 12 | C | E | Config 2 |

### <a name="scale-limits"></a>Skalierungslimits

Für Verbindungsmonitore gelten die folgenden Skalierungslimits:

* Maximale Anzahl von Verbindungsmonitoren pro Abonnement und Region: 100
* Maximale Anzahl von Testgruppen pro Verbindungsmonitor: 20
* Maximale Anzahl von Quellen und Zielen pro Verbindungsmonitor: 100
* Maximale Anzahl von Testkonfigurationen pro Verbindungsmonitor: 
    * 20 über ARMClient
    * 2 über das Azure-Portal

## <a name="analyze-monitoring-data-and-set-alerts"></a>Analysieren von Überwachungsdaten und Festlegen von Warnungen

Nachdem Sie einen Verbindungsmonitor erstellt haben, überprüfen die Quellen die Konnektivität mit den Zielen auf Grundlage der Testkonfiguration.

### <a name="checks-in-a-test"></a>Überprüfungen in einem Test

Basierend auf dem von Ihnen in der Testkonfiguration ausgewählten Protokoll führt der Verbindungsmonitor (Vorschau) eine Reihe von Überprüfungen für das Quelle-Ziel-Paar aus. Die Überprüfungen werden gemäß der ausgewählten Testhäufigkeit durchgeführt.

Wenn Sie HTTP verwenden, berechnet der Dienst die Anzahl von HTTP-Antworten, die einen Antwortcode zurückgegeben haben. Das Ergebnis bestimmt den Prozentsatz der Überprüfungen mit Fehlern. Zum Berechnen der Roundtripzeit misst der Dienst die Zeit zwischen einem HTTP-Aufruf und der Antwort.

Wenn Sie TCP oder ICMP verwenden, berechnet der Dienst den Prozentsatz an Paketverlusten, um den Prozentsatz der Überprüfungen mit Fehlern zu ermitteln. Zum Berechnen der Roundtripzeit misst der Dienst die Zeit, die zum Empfangen der Bestätigung (ACK) für die gesendeten Pakete benötigt wird. Wenn Sie Traceroutedaten für Ihre Netzwerktests aktiviert haben, können Sie sich Hop-by-Hop-Verlust und -Latenz für Ihr lokales Netzwerk anzeigen lassen.

### <a name="states-of-a-test"></a>Teststatus

Basierend auf den Daten, die bei den Überprüfungen zurückgegeben werden, können Tests einen der folgenden Status aufweisen:

* **Erfolgreich**: Die tatsächlichen Werte für den Prozentsatz der Überprüfungen mit Fehlern und die Roundtripzeit liegen innerhalb der angegebenen Schwellenwerte.
* **Fehler**: Die tatsächlichen Werte für den Prozentsatz der Überprüfungen mit Fehlern oder die Roundtripzeit lagen oberhalb der angegebenen Schwellenwerte. Wenn kein Schwellenwert angegeben ist, weist ein Test den Fehlerstatus auf, sobald der Prozentsatz der Überprüfungen mit Fehlern bei 100 liegt.
* **Warnung**: Für den Prozentsatz der Überprüfungen mit Fehlern wurden keine Kriterien angegeben. Wenn keine Kriterien angegeben sind, weist der Verbindungsmonitor (Vorschau) automatisch einen Schwellenwert zu. Wird dieser Schwellenwert überschritten, ändert sich der Teststatus in „Warnung“.

### <a name="data-collection-analysis-and-alerts"></a>Datensammlung, Datenanalyse und Datenwarnungen

Die vom Verbindungsmonitor (Vorschau) gesammelten Daten werden im Log Analytics-Arbeitsbereich gespeichert. Sie haben diesen Arbeitsbereich beim Erstellen des Verbindungsmonitors eingerichtet. 

Über Azure Monitor-Metriken sind auch Überwachungsdaten verfügbar. Mit Log Analytics können Sie die Überwachungsdaten über den gewünschten Zeitraum aufbewahren. Azure Monitor speichert Metriken standardmäßig nur für 30 Tage. 

Sie können [metrikbasierte Warnungen für die Daten festlegen](https://azure.microsoft.com/blog/monitor-at-scale-in-azure-monitor-with-multi-resource-metric-alerts/).

#### <a name="monitoring-dashboards"></a>Überwachungsdashboards

Auf den Überwachungsdashboards wird eine Liste der Verbindungsmonitore angezeigt, auf die Sie für Ihre Abonnements, Regionen, Zeitstempel, Quellen und Zieltypen zugreifen können.

Wenn Sie von Network Watcher zum Verbindungsmonitor (Vorschau) wechseln, können Sie Daten auf folgende Arten anzeigen:

* **Verbindungsmonitor**: Dies ist eine Liste aller Verbindungsmonitore, die für Ihre Abonnements, Regionen, Zeitstempel, Quellen und Zieltypen erstellt wurden. Dies ist die Standardansicht.
* **Testgruppen**: Dies ist eine Liste aller Testgruppen, die für Ihre Abonnements, Regionen, Zeitstempel, Quellen und Zieltypen erstellt wurden. Diese Testgruppen sind nicht nach Verbindungsmonitoren gefiltert.
* **Test**: Dies ist eine Liste aller Tests, die für Ihre Abonnements, Regionen, Zeitstempel, Quellen und Zieltypen ausgeführt werden. Diese Tests sind nicht nach Verbindungsmonitoren oder Testgruppen gefiltert.

In der folgenden Abbildung sind die drei Datenansichten mit Pfeil 1 gekennzeichnet.

Sie können auf dem Dashboard die einzelnen Verbindungsmonitore erweitern, um deren Testgruppen anzuzeigen. Anschließend können Sie die einzelnen Testgruppen erweitern, um die darin ausgeführten Tests anzuzeigen. 

Für eine Liste sind folgende Filter verfügbar:

* **Filter der obersten Ebene**: Wählen Sie Abonnements, Regionen, Zeitstempel, Quellen und Zieltypen aus. Siehe Kasten 2 in der folgenden Abbildung.
* **Zustandsbasierte Filter**: Filtern Sie nach dem Zustand des Verbindungsmonitors, der Testgruppe oder des Tests. Siehe Pfeil 3 in der folgenden Abbildung.
* **Benutzerdefinierte Filter**: Wählen Sie **Alle auswählen** aus, um eine allgemeine Suche durchzuführen. Treffen Sie eine Auswahl aus der Dropdownliste, um nach einer bestimmten Entität zu suchen. Siehe Pfeil 4 in der folgenden Abbildung.

![Screenshot: Filtern von Ansichten der Verbindungsmonitore, Testgruppen und Tests im Verbindungsmonitor (Vorschau)](./media/connection-monitor-2-preview/cm-view.png)

Wenn Sie beispielsweise alle Tests im Verbindungsmonitor (Vorschau) anzeigen möchten, bei denen die Quell-IP 10.192.64.56 lautet, gehen Sie folgendermaßen vor:
1. Ändern Sie die Ansicht in **Test**.
1. Geben Sie *10.192.64.56* in das Suchfeld ein.
1. Wählen Sie in der Dropdownliste die Option **Quellen** aus.

Wenn Sie nur Tests mit Fehlern im Verbindungsmonitor (Vorschau) anzeigen möchten, bei denen die Quell-IP 10.192.64.56 lautet, gehen Sie folgendermaßen vor:
1. Ändern Sie die Ansicht in **Test**.
1. Wählen Sie aus den zustandsbasierten Filtern die Option **Fehler** aus.
1. Geben Sie *10.192.64.56* in das Suchfeld ein.
1. Wählen Sie in der Dropdownliste die Option **Quellen** aus.

Wenn Sie nur Tests mit Fehlern im Verbindungsmonitor (Vorschau) anzeigen möchten, bei denen das Ziel „outlook.office365.com“ lautet, gehen Sie folgendermaßen vor:
1. Ändern Sie die Ansicht in **Test**.
1. Wählen Sie aus den zustandsbasierten Filtern die Option **Fehler** aus.
1. Geben Sie *outlook.office365.com* in das Suchfeld ein.
1. Wählen in der Dropdownliste die Option **Ziele** aus.

   ![Screenshot: Gefilterte Ansicht zur ausschließlichen Anzeige von Tests mit Fehlern für das Ziel „outlook.office365.com“](./media/connection-monitor-2-preview/tests-view.png)

Wenn Sie die Trends bei der Roundtripzeit und dem Prozentsatz der Überprüfungen mit Fehlern für einen Verbindungsmonitor anzeigen möchten, gehen Sie folgendermaßen vor:
1. Wählen Sie den Verbindungsmonitor aus, den Sie untersuchen möchten. Standardmäßig sind die Überwachungsdaten nach Testgruppe organisiert.

   ![Screenshot: Metriken für einen Verbindungsmonitor, nach Testgruppe angezeigt](./media/connection-monitor-2-preview/cm-drill-landing.png)

1. Wählen Sie die Testgruppe aus, die Sie untersuchen möchten.

   ![Screenshot: Auswahl einer Testgruppe](./media/connection-monitor-2-preview/cm-drill-select-tg.png)

    Es werden die Top 5 der Tests mit Fehlern in der Testgruppe basierend auf der Roundtripzeit oder dem Prozentsatz der Überprüfungen mit Fehlern angezeigt. Für jeden Test werden die Roundtripzeit und Trendlinien für den Prozentsatz der Überprüfungen mit Fehlern angezeigt.
1. Wählen Sie einen Test aus der Liste aus, oder wählen Sie einen anderen Test aus, den Sie untersuchen möchten. Für das Zeitintervall und den Prozentsatz der Überprüfungen mit Fehlern werden die Schwellenwerte und die tatsächlichen Werte angezeigt. Für die Roundtripzeit werden Schwellenwert, Mittelwert, Mindest- und Höchstwert angezeigt.

   ![Screenshot: Testergebnisse für Roundtripzeit und Prozentsatz der Überprüfungen mit Fehlern](./media/connection-monitor-2-preview/cm-drill-charts.png)

1. Ändern Sie das Zeitintervall, damit mehr Daten angezeigt werden.
1. Ändern Sie die Ansicht, um Quellen, Ziele oder Testkonfigurationen anzuzeigen. 
1. Wählen Sie eine Quelle basierend auf Tests mit Fehlern aus, und untersuchen Sie die Top 5 der Tests mit Fehlern. Wählen Sie z. B. **Anzeigen nach** > **Quellen** und **Anzeigen nach** > **Ziele** aus, um die relevanten Tests im Verbindungsmonitor zu untersuchen.

   ![Screenshot: Leistungsmetriken für die Top 5 der Tests mit Fehlern](./media/connection-monitor-2-preview/cm-drill-select-source.png)

Wenn Sie die Trends bei der Roundtripzeit und dem Prozentsatz der Überprüfungen mit Fehlern für eine Testgruppe anzeigen möchten, gehen Sie folgendermaßen vor:

1. Wählen Sie die Testgruppe aus, die Sie untersuchen möchten. 

    Die Überwachungsdaten sind standardmäßig nach Quellen, Zielen und Testkonfigurationen (Tests) angeordnet. Sie können die Ansicht später von Testgruppen in Quellen, Ziele oder Testkonfigurationen ändern. Wählen Sie dann eine Entität aus, für die Sie die Top 5 der Tests mit Fehlern untersuchen möchten. Ändern Sie die Ansicht beispielsweise in Quellen und Ziele, um die relevanten Tests im ausgewählten Verbindungsmonitor zu untersuchen.
1. Wählen Sie den Test aus, den Sie untersuchen möchten.

   ![Screenshot: Auswahl eines Tests](./media/connection-monitor-2-preview/tg-drill.png)

    Für das Zeitintervall und den Prozentsatz der Überprüfungen mit Fehlern werden die Schwellenwerte und die tatsächlichen Werte angezeigt. Für die Roundtripzeit werden Schwellenwert, Mittelwert, Mindest- und Höchstwert angezeigt. Außerdem werden ausgelöste Warnungen für den von Ihnen ausgewählten Test angezeigt.
1. Ändern Sie das Zeitintervall, damit mehr Daten angezeigt werden.

Wenn Sie die Trends bei der Roundtripzeit und dem Prozentsatz der Überprüfungen mit Fehlern für einen Test anzeigen möchten, gehen Sie folgendermaßen vor:
1. Wählen Sie die Quelle, das Ziel und die Testkonfiguration aus, die Sie untersuchen möchten.

    Für das Zeitintervall und den Prozentsatz der Überprüfungen mit Fehlern werden die Schwellenwerte und die tatsächlichen Werte angezeigt. Für die Roundtripzeit werden Schwellenwert, Mittelwert, Mindest- und Höchstwert angezeigt. Außerdem werden ausgelöste Warnungen für den von Ihnen ausgewählten Test angezeigt.

   ![Screenshot: Metriken für einen Test](./media/connection-monitor-2-preview/test-drill.png)

1. Wählen Sie **Topologie** aus, um die Netzwerktopologie anzuzeigen.

   ![Screenshot: Registerkarte „Topologie“](./media/connection-monitor-2-preview/test-topo.png)

1. Wählen Sie in der Topologie einen beliebigen Hop im Pfad aus, um die erkannten Probleme anzuzeigen. (Diese Hops sind Azure-Ressourcen.) Für lokale Netzwerke steht diese Funktion derzeit nicht zur Verfügung.

   ![Screenshot: Ausgewählter Hop-Link auf der Registerkarte „Topologie“](./media/connection-monitor-2-preview/test-topo-hop.png)

#### <a name="log-queries-in-log-analytics"></a>Protokollabfragen in Log Analytics

Mit Log Analytics lassen sich benutzerdefinierte Ansichten Ihrer Überwachungsdaten erstellen. Alle auf der Benutzeroberfläche angezeigten Daten stammen aus Log Analytics. Sie können Daten im Repository interaktiv analysieren. Korrelieren Sie die Daten aus der Agent-Integritätsdiagnose oder anderen Lösungen, die auf Log Analytics basieren. Exportieren Sie die Daten nach Excel oder Power BI, oder erstellen Sie einen freigabefähigen Link.

#### <a name="metrics-in-azure-monitor"></a>Metriken in Azure Monitor

In Verbindungsmonitoren, die vor dem Verbindungsmonitor (Vorschau) erstellt wurden, sind alle vier Metriken verfügbar: ProbesFailedPercent, AverageRoundtripMs, ChecksFailedPercent (Preview) und RoundTripTimeMs (Preview). In Verbindungsmonitoren, die im Verbindungsmonitor (Vorschau) erstellt wurden, sind Daten nur für die mit *(Preview)* (Vorschau) gekennzeichneten Metriken verfügbar.

![Screenshot: Metriken im Verbindungsmonitor (Vorschau)](./media/connection-monitor-2-preview/monitor-metrics.png)

Legen Sie bei Verwendung von Metriken den Ressourcentyp auf „Microsoft.Network/networkWatchers/connectionMonitors“ fest.

| Metrik | `Display name` | Einheit | Aggregationstyp | BESCHREIBUNG | Dimensionen |
| --- | --- | --- | --- | --- | --- |
| ProbesFailedPercent | Fehlerhafte Tests in Prozent | Prozentwert | Average | Prozentsatz fehlerhafter Tests bei der Konnektivitätsüberwachung. | Keine Dimensionen |
| AverageRoundtripMs | Durchschn. Roundtripzeit (ms) | Millisekunden | Average | Durchschnittliche Netzwerk-Roundtripzeit für die Konnektivitätsüberwachung von Testdatenverkehr, der zwischen Quelle und Ziel gesendet wurde. |             Keine Dimensionen |
| ChecksFailedPercent (Preview) | % der Überprüfungen mit Fehlern (Vorschau) | Prozentwert | Average | Prozentsatz der Überprüfungen mit Fehlern für einen Test. | ConnectionMonitorResourceId <br>SourceAddress <br>SourceName <br>SourceResourceId <br>SourceType <br>Protocol <br>DestinationAddress <br>DestinationName <br>DestinationResourceId <br>DestinationType <br>DestinationPort <br>TestGroupName <br>TestConfigurationName <br>Region |
| RoundTripTimeMs (Vorschau) | Roundtripzeit (ms) (Vorschau) | Millisekunden | Average | Roundtripzeit für Überprüfungen, die zwischen Quelle und Ziel gesendet wurden. Es wird kein Durchschnittswert gebildet. | ConnectionMonitorResourceId <br>SourceAddress <br>SourceName <br>SourceResourceId <br>SourceType <br>Protocol <br>DestinationAddress <br>DestinationName <br>DestinationResourceId <br>DestinationType <br>DestinationPort <br>TestGroupName <br>TestConfigurationName <br>Region |

#### <a name="metric-alerts-in-azure-monitor"></a>Metrikwarnungen in Azure Monitor

Zum Erstellen einer Warnung in Azure Monitor gehen Sie folgendermaßen vor:

1. Wählen Sie die Verbindungsmonitorressource aus, die Sie im Verbindungsmonitor (Vorschau) erstellt haben.
1. Stellen Sie sicher, dass **Metrik** als Signaltyp für den Verbindungsmonitor angezeigt wird.
1. Wählen Sie unter **Bedingung hinzufügen** für **Signalname** die Option **ChecksFailedPercent(Preview)** oder **RoundTripTimeMs(Preview)** aus.
1. Wählen Sie für **Signaltyp** die Option **Metriken** aus. Wählen Sie z. B. **ChecksFailedPercent(Preview)** aus.
1. Alle Dimensionen für die Metrik werden aufgelistet. Wählen Sie den Dimensionsnamen und -wert aus. Wählen Sie z. B. **Quelladresse** aus, und geben Sie dann die IP-Adresse einer beliebigen Quelle in Ihrem Verbindungsmonitor ein.
1. Geben Sie unter **Warnungslogik** die folgenden Details ein:
   * **Bedingungstyp**: **Statisch**
   * **Bedingung** und **Schwellenwert**
   * **Aggregationsgranularität und Häufigkeit der Auswertung**: Der Verbindungsmonitor (Vorschau) aktualisiert die Daten jede Minute.
1. Wählen Sie unter **Aktionen** Ihre Aktionsgruppe aus.
1. Legen Sie die Warnungsdetails fest.
1. Erstellen Sie die Warnungsregel.

   ![Screenshot: Bereich „Regel erstellen“ in Azure Monitor mit hervorgehobenen Feldern für „Quelladresse“ und „Quellendpunktname“](./media/connection-monitor-2-preview/mdm-alerts.jpg)

## <a name="diagnose-issues-in-your-network"></a>Diagnostizieren von Problemen in Ihrem Netzwerk

Mit dem Verbindungsmonitor (Vorschau) können Sie Probleme in Ihrem Verbindungsmonitor und Ihrem Netzwerk diagnostizieren. Probleme in Ihrem Hybridnetzwerk werden von den Log Analytics-Agents erkannt, die Sie zuvor installiert haben. Probleme in Azure werden von der Network Watcher-Erweiterung erkannt. 

Sie können Probleme im Azure-Netzwerk in der Netzwerktopologie anzeigen.

Bei Netzwerken, deren Quellen sich auf lokalen VMs befinden, können die folgenden Probleme erkannt werden:

* Timeout bei der Anforderung.
* Der Endpunkt wurde nicht über DNS aufgelöst (vorübergehend oder dauerhaft). Die URL ist ungültig.
* Es wurden keine Hosts gefunden.
* Die Quelle kann keine Verbindung zum Ziel herstellen. Das Ziel ist nicht über ICMP erreichbar.
* Zertifikatbezogene Probleme: 
    * Der Agent muss mit dem Clientzertifikat authentifiziert werden. 
    * Es besteht kein Zugriff auf die Zertifikatssperrliste. 
    * Der Hostname des Endpunkts stimmt nicht mit dem Antragsteller des Zertifikats oder dem alternativen Antragstellernamen überein. 
    * Das Stammzertifikat fehlt im Speicher der vertrauenswürdigen Zertifizierungsstellen auf dem lokalen Computer der Quelle. 
    * Das SSL-Zertifikat ist abgelaufen, ungültig, widerrufen oder nicht kompatibel.

Bei Netzwerken, deren Quellen sich auf Azure-VMs befinden, können die folgenden Probleme erkannt werden:

* Probleme mit Agents:
    * Der Agent wurde angehalten.
    * Fehler bei der DNS-Auflösung.
    * Es lauscht keine Anwendung oder kein Listener am Zielport.
    * Der Socket konnte nicht geöffnet werden.
* Probleme mit dem VM-Status: 
    * Wird gestartet
    * Wird beendet
    * Beendet
    * Zuordnung wird aufgehoben
    * Zuordnung aufgehoben
    * Wird neu gestartet
    * Nicht zugewiesen
* Der ARP-Tabelleneintrag fehlt.
* Der Datenverkehr wurde aufgrund von Problemen mit der lokalen Firewall oder NSG-Regeln blockiert.
* Probleme mit dem Gateway des virtuellen Netzwerks: 
    * Es fehlen Routen.
    * Der Tunnel zwischen zwei Gateways ist getrennt oder fehlt.
    * Das zweite Gateway wurde nicht vom Tunnel gefunden.
    * Es wurden keine Peeringinformationen gefunden.
* Die Route fehlte in Microsoft Edge.
* Der Datenverkehr wurde aufgrund von Systemrouten oder UDR unterbrochen.
* BGP ist für die Gatewayverbindung nicht aktiviert.
* Der DIP-Test ist beim Lastenausgleich nicht verfügbar.
