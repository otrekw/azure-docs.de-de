---
title: Verbindungsmonitor (Vorschauversion) | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie mit dem Verbindungsmonitor (Vorschauversion) die Netzwerkkommunikation in einer verteilten Umgebung überwachen.
services: network-watcher
documentationcenter: na
author: vinynigam
manager: agummadi
editor: ''
tags: azure-resource-manager
Customer intent: I need to monitor communication between a VM and another VM. If the communication fails, I need to know why, so that I can resolve the problem.
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/27/2020
ms.author: vinigam
ms.custom: mvc
ms.openlocfilehash: 43c49cce1dd53edd5c2b13b01a31f94752579dff
ms.sourcegitcommit: bdf31d87bddd04382effbc36e0c465235d7a2947
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2020
ms.locfileid: "77169319"
---
# <a name="unified-connectivity-monitoring-with-connection-monitor-preview"></a>Vereinheitlichte Konnektivitätsüberwachung mit dem Verbindungsmonitor (Vorschauversion)

Der Verbindungsmonitor (Vorschauversion) im Azure-Dienst „Network Watcher“ bietet einheitliche End-to-End-Funktionen zur Verbindungsüberwachung in Hybrid- und Azure-Cloudbereitstellungen. Network Watcher stellt Tools für das Überwachen, Diagnostizieren und Anzeigen von Verbindungsmetriken für Ihre Azure-Bereitstellungen zur Verfügung.

Hauptanwendungsfälle:

- Sie verfügen über eine Front-End-Webserver-VM, die in einer Anwendung mit mehreren Ebenen mit einer Datenbankserver-VM kommuniziert, und Sie möchten die Netzwerkkonnektivität zwischen beiden VMs überprüfen.
- Sie möchten, dass VMs in der Region „USA, Osten“ VMs in der Region „USA, Mitte“ pingen und die regionsübergreifenden Netzwerklatenzen vergleichen.
- Sie haben mehrere lokale Unternehmensstandorte in Städten wie Seattle, die mit Office 365-URLs verbunden sind. Sie möchten die Latenzen bei Benutzern vergleichen, die Office 365-URLs in Seattle und Ashburn verwenden.
- Sie verfügen über ein hybrides Anwendungssetup, das eine Verbindung zu einem Azure Storage-Endpunkt benötigt. Sie möchten die Latenzen zwischen einem lokalen Standort und der Azure-Anwendung vergleichen, die beide mit dem gleichen Azure Storage-Endpunkt verbunden sind.
- Sie möchten die Konnektivität von Azure-VMs überprüfen, die Ihre Cloudanwendungen in lokalen Setups hosten.

Die Vorschauversion dieser Lösung vereint das Beste aus den zwei Funktionen [Verbindungsmonitor](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview#monitor-communication-between-a-virtual-machine-and-an-endpoint) (Network Watcher) und [Dienstkonnektivitätsmonitor](https://docs.microsoft.com/azure/azure-monitor/insights/network-performance-monitor-service-connectivity) (Netzwerkleistungsmonitor).

Vorteile:

* Einheitliche, intuitive Oberfläche zum Überwachen von Azure- und Hybridbereitstellungen
* Regionsübergreifende und arbeitsbereichübergreifende Verbindungsüberwachung
* Häufigere Prüfungen und bessere Einsicht in die Netzwerkleistung
* Schnellere Warnungen für Ihre Hybridbereitstellungen
* Unterstützung für HTTP-, TCP-und ICMP-basierte Konnektivitätsprüfungen
* Metriken und Unterstützung für Log Analytics, sowohl für Azure- als auch für Nicht-Azure-Testsetups

![Verbindungsmonitor](./media/connection-monitor-2-preview/hero-graphic.png)

Führen Sie die folgenden Schritte aus, um die Überwachung mit dem Verbindungsmonitor (Vorschauversion) zu starten.

## <a name="step-1-install-monitoring-agents"></a>Schritt 1: Installieren von Überwachungs-Agents

Der Verbindungsmonitor verwendet leichtgewichtige, ausführbare Programme für das Ausführen von Konnektivitätsprüfungen.  Konnektivitätsprüfungen werden sowohl in Azure als auch in lokalen Umgebungen unterstützt. Welches ausführbare Programm verwendet werden soll, hängt davon ab, ob Ihre VM in Azure oder lokal gehostet wird.

### <a name="agents-for-azure-virtual-machines"></a>Agents für Azure-VMs

Damit der Verbindungsmonitor Ihre Azure-VMs als Überwachungsquelle erkennt, müssen Sie die VM-Erweiterung „Network Watcher-Agent“ (auch als „Network Watcher“-Erweiterung bekannt) installieren. Die Erweiterung „Network Watcher-Agent“ ist eine Voraussetzung für das Auslösen der End-to-End-Überwachung und anderer erweiterter Funktionalität auf Azure-VMs. Sie können [eine VM erstellen und die Erweiterung „Network Watcher“](https://docs.microsoft.com/azure/network-watcher/connection-monitor#create-the-first-vm)[ darauf installieren](https://docs.microsoft.com/azure/network-watcher/connection-monitor#create-the-first-vm).  Network Watcher lässt sich separat für [Linux](https://docs.microsoft.com/azure/virtual-machines/extensions/network-watcher-linux) und [Windows](https://docs.microsoft.com/azure/virtual-machines/extensions/network-watcher-windows) installieren und konfigurieren. Auch ein betriebssystemspezifisches Troubleshooting ist möglich.

Wenn Netzwerksicherheitsgruppen (network security groups, NSGs) oder Firewallregeln die Kommunikation zwischen Quelle und Ziel blockieren, erkennt der Verbindungsmonitor das Problem und zeigt es in der Topologie als Diagnosemeldung an. Damit eine Verbindungsüberwachung möglich ist, müssen Sie zunächst sicherstellen, dass die NSG- und Firewallregeln eine Paketübertragung über TCP oder ICMP zwischen Quelle und Ziel zulassen.

### <a name="agents-for-on-premise-machines"></a>Agents für lokale Computer

Damit der Verbindungsmonitor Ihre lokalen Computer als Überwachungsquelle erkennt, müssen Sie den Log Analytics-Agent auf den Computern installieren und die Lösung „Netzwerkleistungsmonitor“ aktivieren. Diese Agents sind mit den Log Analytics-Arbeitsbereichen verknüpft. Daher müssen die Arbeitsbereichs-ID und der Primärschlüssel eingerichtet sein, damit sie mit der Überwachung beginnen können.

Wenn Sie den Log Analytics-Agent für Windows-Computer installieren möchten, folgen Sie der Anleitung unter [Azure Monitor-VM-Erweiterung für Windows](https://docs.microsoft.com/azure/virtual-machines/extensions/oms-windows).

Wenn der Pfad Firewalls oder virtuelle Netzwerkgeräte enthält, sollten Sie sicherstellen, dass das Ziel erreichbar ist.

## <a name="step-2-enable-network-watcher-on-your-subscription"></a>Schritt 2: Aktivieren von Network Watcher für Ihr Abonnement

Network Watcher ist für alle Abonnements mit einem virtuellen Netzwerk (VNET) verfügbar. Wenn Sie ein virtuelles Netzwerk in Ihrem Abonnement erstellen, wird Network Watcher automatisch in der Region und für das Abonnement Ihres virtuellen Netzwerks aktiviert. Die automatische Aktivierung von Network Watcher hat keine Auswirkungen auf Ihre Ressourcen oder die damit verbundenen Gebühren. Stellen Sie sicher, dass Network Watcher für Ihr Abonnement nicht explizit deaktiviert ist. Weitere Informationen finden Sie unter [Aktivieren von Network Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-create).

## <a name="step-3-create-connection-monitor"></a>Schritt 3: Erstellen eines Verbindungsmonitors 

Der _Verbindungsmonitor_ überwacht die Kommunikation in regelmäßigen Abständen und informiert Sie, wenn Änderungen bei der Erreichbarkeit, Latenz und Netzwerktopologie zwischen den Quell-Agents und dem Zielendpunkt auftreten. Ziele können Azure-VMs oder lokale Computer sein, auf denen ein Überwachungs-Agent installiert ist. Zielendpunkte können Office 365-URLs, Dynamics 365-URLs, benutzerdefinierte URLs, Azure-VM-Ressourcen-IDs, IPv4/IPv6-Adressen, FQDNs oder beliebige Domänennamen sein.

### <a name="accessing-connection-monitor-preview"></a>Zugreifen auf den Verbindungsmonitor (Vorschauversion)

1. Besuchen Sie auf der Azure-Portal-Startseite Network Watcher.
2. Klicken Sie links in Network Watcher im Bereich „Überwachung“ auf die Registerkarte „Verbindungsmonitor (Vorschau)“.
3. Dort sind alle Verbindungsmonitore aufgeführt, die mit der Vorschauversion des Verbindungsmonitors erstellt wurden. Alle Verbindungsmonitore, die mit dem klassischen Verbindungsmonitor erstellt wurden, werden auf der Registerkarte „Verbindungsmonitor“ angezeigt.

    ![Erstellen eines Verbindungsmonitors](./media/connection-monitor-2-preview/cm-resource-view.png)


### <a name="creating-a-connection-monitor"></a>Erstellen eines Verbindungsmonitors

Bei Verbindungsmonitoren, die mit dem Verbindungsmonitor (Vorschauversion) erstellt wurden, können Sie sowohl lokale als auch Azure-VMs als Quellen hinzufügen und die Konnektivität mit Endpunkten überwachen, die Azure oder jede andere URL/IP-Adresse umfassen.

Ein Verbindungsmonitor besteht aus folgenden Entitäten:

* Verbindungsmonitorressource: Dies ist eine regionsspezifische Azure-Ressource. Alle unten erwähnten Entitäten sind Eigenschaften einer Verbindungsmonitorressource.
* Endpunkte: Alle Quellen und Ziele bei Konnektivitätsprüfungen werden als Endpunkte bezeichnet. Beispiele für Endpunkte: Azure-VMs, lokale Agents, URLs, IP-Adressen.
* Testkonfiguration: Jede Testkonfiguration ist protokollspezifisch. Je nach ausgewähltem Protokoll können Sie den Port, die Schwellenwerte, die Testhäufigkeit und weitere Parameter definieren.
* Testgruppe: Jede Testgruppe enthält Quellendpunkte, Zielendpunkte und Testkonfigurationen. Jeder Verbindungsmonitor kann mehrere Testgruppen enthalten.
* Test: Die Kombination aus einem Quell- und einem Zielendpunkt sowie einer Testkonfiguration stellt einen Test dar. Ein Test ist die niedrigste verfügbare Stufe bei der Datenüberwachung („checks failed %“ (Überprüfungen mit Fehlern %) und „RTT“).

 ![Erstellen eines Verbindungsmonitors](./media/connection-monitor-2-preview/cm-tg-2.png)

#### <a name="from-portal"></a>Über das Portal

Zum Erstellen eines Verbindungsmonitors führen Sie die folgenden Schritte aus:

1. Klicken Sie auf dem Dashboard des Verbindungsmonitors (Vorschauversion) oben links auf „Erstellen“.
2. Geben Sie auf der Registerkarte „Grundlegend“ die Informationen zu Ihrem Verbindungsmonitor ein.
   1. Name des Verbindungsmonitors: Dies ist der Name Ihres Verbindungsmonitors. Hierfür gelten die Standardbenennungsregeln für Azure-Ressourcen.
   2. Abonnement: Wählen Sie ein Abonnement für Ihren Verbindungsmonitor aus.
   3. Region: Wählen Sie eine Region für Ihre Verbindungsmonitorressource aus. Dabei kommen nur die Quell-VMs infrage, die in dieser Region erstellt wurden.
   4. Arbeitsbereichskonfiguration: Sie können Ihre Überwachungsdaten in dem vom Verbindungsmonitor erstellten Standardarbeitsbereich speichern, indem Sie auf das entsprechende Kontrollkästchen klicken. Wenn Sie einen benutzerdefinierten Arbeitsbereich auswählen möchten, deaktivieren Sie dieses Kontrollkästchen. Wählen Sie das Abonnement und die Region für den Arbeitsbereich aus, in dem Ihre Überwachungsdaten gespeichert werden sollen.
   5. Klicken Sie auf „Next: Test Groups“ (Weiter: Testgruppen), um Testgruppen hinzuzufügen.

      ![Erstellen eines Verbindungsmonitors](./media/connection-monitor-2-preview/create-cm-basics.png)

3. Klicken Sie auf der Registerkarte „Testgruppen“ auf „+ Test Group“ (+ Testgruppe), um eine Testgruppe hinzuzufügen. Im Abschnitt _Erstellen von Testgruppen im Verbindungsmonitor_ erfahren Sie, wie Sie Testgruppen hinzufügen. Klicken Sie auf „Überprüfen + erstellen“, um Ihren Verbindungsmonitor zu überprüfen.

   ![Erstellen eines Verbindungsmonitors](./media/connection-monitor-2-preview/create-tg.png)

4. Überprüfen Sie auf der Registerkarte „Überprüfen + erstellen“ die grundlegenden Informationen und Testgruppen, bevor Sie den Verbindungsmonitor erstellen. So bearbeiten Sie den Verbindungsmonitor in der Ansicht „Überprüfen + erstellen“:
   1. Wenn Sie die grundlegenden Details bearbeiten möchten, klicken Sie wie in Feld 1 Abbildung 2 auf das Stiftsymbol.
   2. Zum Bearbeiten einzelner Testgruppen klicken Sie auf die gewünschte Testgruppe, um sie im Bearbeitungsmodus zu öffnen.
   3. „Aktuelle Kosten/Monat“ gibt die Kosten während der Vorschauphase an. Zurzeit werden keine Kosten für die Verwendung des Verbindungsmonitors berechnet. Daher wird in dieser Spalte 0 (null) angezeigt. „Actual cost/month“ (Tatsächliche Kosten/Monat) gibt den Preis an, der ab der allgemeinen Verfügbarkeit abgerechnet wird. Beachten Sie, dass die Erfassung der Protokollanalyse auch während der Vorschauversion wirksam ist.

5. Klicken Sie auf der Registerkarte „Überprüfen + erstellen“ auf „Erstellen“, um den Verbindungsmonitor zu erstellen.

   ![Erstellen eines Verbindungsmonitors](./media/connection-monitor-2-preview/review-create-cm.png)

6.  Der Verbindungsmonitor (Vorschauversion) erstellt die Verbindungsmonitorressource im Hintergrund.

#### <a name="from-armclient"></a>Über ARMClient

```armclient
$connectionMonitorName = "sampleConnectionMonitor"

$ARM = "[https://](https://apac01.safelinks.protection.outlook.com/?url=https%3A%2F%2Fbrazilus.management.azure.com&amp;data=02%7C01%7CManasi.Sant%40microsoft.com%7Cd900da4ed7f24366842108d68022159b%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C636837281231186904&amp;sdata=qHL8zWjkobY9MatRpAVbODwboKSQAqqEFOMnjmfyOnU%3D&amp;reserved=0)management.azure.com"

$SUB = "subscriptions/\&lt;subscription id 1\&gt;"

$NW = "resourceGroups/NetworkWatcherRG/providers/Microsoft.Network/networkWatchers/NetworkWatcher\_centraluseuap"

$body =

"{

location: 'eastus',

properties: {

endpoints: [{

name: 'workspace',

resourceId: '/subscriptions/\&lt;subscription id\&gt;/resourcegroups/\&lt;resource group\&gt;/providers/Microsoft.OperationalInsights/workspaces/sampleWorkspace',

filter: {

 items: [{

type: 'AgentAddress',

address: '\&lt;FQDN of your on-premise agent'

}]

}

          },

 {

name: 'vm1',

resourceId: '/subscriptions/\&lt;subscription id\&gt;/resourceGroups/\&lt;resource group\&gt;/providers/Microsoft.Compute/virtualMachines/\&lt;vm-name\&gt;'

},

 {

name: 'vm2',

resourceId: '/subscriptions/\&lt;subscription id\&gt;/resourceGroups/\&lt;resource group\&gt;/providers/Microsoft.Compute/virtualMachines/\&lt;vm-name\&gt;'

   },

{

name: 'azure portal'

address: '\&lt;URL\&gt;'

   },

 {

    name: 'ip',

     address: '\&lt;IP\&gt;'

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

    testConfigurations: ['http', 'https', 'tcpDisabled', 'icmpDisabled'],

    sources: ['vm1'],

    destinations: ['vm2'],

    disable: true

   }

  ],

  testConfigurations: [{

    name: 'http',

    testFrequencySec: 60,

    protocol: 'HTTP',

    successThreshold: {

     checksFailedPercent: 50,

     roundTripTimeMs: 3.4

    }

   }, {

    name: 'https',

    testFrequencySec: 60,

    protocol: 'HTTP',

    httpConfiguration: {

     preferHTTPS: true

    },

    successThreshold: {

     checksFailedPercent: 50,

     roundTripTimeMs: 3.4

    }

   }, {

    name: 'tcpEnabled',

    testFrequencySec: 30,

    protocol: 'TCP',

    tcpConfiguration: {

     port: 80

    },

    successThreshold: {

     checksFailedPercent: 30,

     roundTripTimeMs: 5.2

    }

   }, {

    name: 'icmpEnabled',

    testFrequencySec: 90,

    protocol: 'ICMP',

    successThreshold: {

     checksFailedPercent: 50,

     roundTripTimeMs: 3.4

    }

   }, {

    name: 'icmpDisabled',

    testFrequencySec: 120,

    protocol: 'ICMP',

    icmpConfiguration: {

     disableTraceRoute: true

    },

    successThreshold: {

     checksFailedPercent: 50,

     roundTripTimeMs: 3.4

    }

   }, {

    name: 'tcpDisabled',

    testFrequencySec: 45,

    protocol: 'TCP',

    tcpConfiguration: {

     port: 80,

     disableTraceRoute: true

    },

    successThreshold: {

     checksFailedPercent: 30,

     roundTripTimeMs: 5.2

    }

   }

  ]

 }

} "
```

Bereitstellungsbefehl:
```
armclient PUT $ARM/$SUB/$NW/connectionMonitors/$connectionMonitorName/?api-version=2019-07-01 $body -verbose
```

### <a name="creating-test-groups-in-connection-monitor"></a>Erstellen von Testgruppen im Verbindungsmonitor

Jede Testgruppe im Verbindungsmonitor enthält Quellen und Ziele, die mithilfe von Testkonfigurationen auf die Netzwerkparameter „Überprüfungen mit Fehlern“und „RTT“ getestet werden.

#### <a name="from-portal"></a>Über das Portal

Geben Sie zum Erstellen einer Testgruppe in einem Verbindungsmonitor in die folgenden Felder einen Wert ein:

1. Disable Test Group (Testgruppe deaktivieren): Durch Aktivieren dieses Felds wird die Überwachung für alle in der Testgruppe angegebenen Quellen und Ziele deaktiviert. Diese Option ist standardmäßig deaktiviert.
2. Name: Dies ist der Name Ihrer Testgruppe.
3. Quellen: Sie können sowohl Azure-VMs als auch lokale Computer als Quellen angeben, wenn Agents darauf installiert sind. Weiter oben in Schritt 1 wurde erläutert, wie ein für Ihre Quelle spezifischer Agent installiert wird.
   1. Klicken Sie auf die Registerkarte „Azure-Agents“, um Azure-Agents auszuwählen. Es werden nur die VMs aufgelistet, die an die Region gebunden sind, die Sie bei der Erstellung des Verbindungsmonitors angegeben haben. VMs werden automatisch in den Abonnements gruppiert, zu denen sie gehören. Diese Gruppen sind zugeklappt. Sie können von der Abonnementebene einen Drilldown auf andere Hierarchieebenen ausführen:

      ```Subscription -\&gt; resource groups -\&gt; VNETs -\&gt; Subnets -\&gt; VMs with agents Y```

      Außerdem lässt sich der Wert des Felds „Gruppieren nach“ ändern, sodass der Baum mit einer beliebigen anderen Ebene beginnt. Beispiel:  Gruppieren nach: Beim Gruppieren nach VNETs werden die VMs mit Agents in folgender Hierarchie angezeigt: VNETs > Subnetze > VMs mit Agents.

      ![Quellen hinzufügen](./media/connection-monitor-2-preview/add-azure-sources.png)

   2. Klicken Sie auf die Registerkarte „Non – Azure Agents“ (Nicht-Azure-Agents), um lokale Agents auszuwählen. Agents werden standardmäßig in Arbeitsbereichen in einer Region gruppiert angezeigt. Nur die Arbeitsbereiche, für die die Lösung „Netzwerkleistungsmonitor“ konfiguriert ist, werden aufgelistet. Fügen Sie die Netzwerkleistungsmonitor-Lösung Ihrem Arbeitsbereich aus [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.NetworkMonitoringOMS?tab=Overview) hinzu. Sie können auch die unter [Überwachungslösungen in Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/insights/solutions) beschriebenen Schritte ausführen. Auf der Registerkarte „Grundlegende Infos“ in der Ansicht „Verbindungsmonitor erstellen“ ist die Region standardmäßig ausgewählt. Sie können die Region ändern und Agents aus Arbeitsbereichen in der neuen Region auswählen. Sie haben auch die Möglichkeit, den Wert im Feld „Gruppieren nach“ zu ändern, sodass nach Subnetzen sortiert wird.

      ![Nicht-Azure-Quellen](./media/connection-monitor-2-preview/add-non-azure-sources.png)


   3. Klicken Sie auf „Überprüfen“, um die ausgewählten Azure- und Nicht-Azure-Agents anzuzeigen.

      ![Quellen überprüfen](./media/connection-monitor-2-preview/review-sources.png)

   4. Klicken Sie auf „Fertig“, sobald Sie alle Quellen ausgewählt haben.

4. Ziele: Sie können die Konnektivität zu Azure-VMs oder anderen beliebigen Endpunkten (öffentliche IP-Adresse, URL, FQDN) überwachen, indem Sie sie als Ziele festlegen. Einer einzelnen Testgruppe können Sie Azure-VMs, O365-URLs, D365-URLs oder benutzerdefinierte Endpunkte hinzufügen.

   1. Klicken Sie auf die Registerkarte „Azure-VMs“, um Azure-VMs als Ziele auszuwählen. Azure-VMs werden standardmäßig in der Abonnementhierarchie in der gleichen Region gruppiert, die auf der Registerkarte „Grundlegende Infos“ in der Ansicht „Verbindungsmonitor erstellen“ ausgewählt wurde. Sie können die Region ändern und die Azure-VMs aus der neuen Region auswählen. Sie können von der Abonnementebene einen Drilldown auf andere Hierarchieebenen wie z. B. die der Azure-Agents ausführen:

      ![Ziele hinzufügen](./media/connection-monitor-2-preview/add-azure-dests1.png)<br>

      ![Ziele hinzufügen 2](./media/connection-monitor-2-preview/add-azure-dests2.png)

   2. Klicken Sie auf die Registerkarte „Endpunkte“, um Endpunkte als Ziele auszuwählen. Die Endpunktliste wird mit den O365- und D365-Test-URLs befüllt, die nach Namen gruppiert sind.  Sie können auch einen Endpunkt auswählen, der in einer anderen Testgruppe im selben Verbindungsmonitor erstellt wurde. Zum Hinzufügen eines neuen Endpunkts klicken Sie in der oberen rechten Ecke des Bildschirms auf „+ Endpoint“ (+ Endpunkt), und geben Sie die Endpunkt-URL/die IP-Adresse/den FQDN und den Namen ein.

      ![Endpunkte hinzufügen](./media/connection-monitor-2-preview/add-endpoints.png)

   3. Klicken Sie auf „Überprüfen“, um die ausgewählten Azure- und Nicht-Azure-Agents anzuzeigen.
   4. Klicken Sie auf „Fertig“, sobald Sie alle Quellen ausgewählt haben.

5. Testkonfiguration: Sie können einer bestimmten Testgruppe eine beliebige Anzahl von Testkonfigurationen zuordnen. Im Portal sind Testgruppen zwar auf eine Testkonfiguration beschränkt, mit ARMClient lassen sich jedoch weitere hinzufügen.
   1. Name: Dies ist der Name der Testkonfiguration.
   2. Protokoll: Sie können zwischen TCP, ICMP und HTTP auswählen. Wenn Sie HTTP in HTTPS ändern möchten, wählen Sie HTTP als Protokoll und 443 als Port aus.
   3. Create Network Test Configuration (Netzwerktestkonfiguration erstellen): Dieses Kontrollkästchen wird nur angezeigt, wenn Sie im Feld „Protokoll“ die Option „HTTP“ ausgewählt haben. Aktivieren Sie dieses Feld, um eine weitere Testkonfiguration mit denselben Quellen und Zielen zu erstellen, die in Schritt 3 und 4 über das TCP/ICMP-Protokoll angegeben wurden. Die neu erstellte Testkonfiguration heißt „<In 5.a angegebener Name>\_networkTestConfig“.
   4. Disable Traceroute (Traceroute deaktivieren): Dieses Feld gilt für Testgruppen mit einem TCP- oder ICMP-Protokoll.  Wenn Sie dieses Feld aktivieren, können Quellen weder die Topologie noch Hop-by-Hop-Roundtripzeiten ermitteln.
   5. Zielport: Sie können in dieses Feld einen Zielport Ihrer Wahl eingeben.
   6. Testhäufigkeit: Dieses Feld entscheidet, wie oft Quellen Ziele auf dem oben angegebenen Protokoll und Port pingen. Sie können zwischen Intervallen von 30 Sekunden, 1 Minute, 5 Minuten, 15 Minuten und 30 Minuten wählen. Quellen testen die Konnektivität zu den Zielen auf Grundlage des von Ihnen gewählten Werts.  Wenn Sie z. B. 30 Sekunden auswählen, überprüfen die Quellen die Verbindungskonnektivität zum Ziel alle 30 Sekunden mindestens einmal.
   7. Health Thresholds (Integritätsschwellenwerte): Sie können Schwellenwerte für die unten erwähnten Netzwerkparameter festlegen.
      1. Checks Failed in % (Überprüfungen mit Fehlern in %): Dies ist der Prozentanteil der fehlgeschlagenen Überprüfungen, wenn Quellen die Konnektivität zum Ziel mithilfe der oben angegebenen Kriterien überprüfen. Bei einem TCP/ICMP-Protokoll können die Überprüfungen mit Fehlern in % mit dem Paketverlust in % gleichgesetzt werden. Beim HTTP-Protokoll stellt dieses Feld die Anzahl der HTTP-Anforderungen dar, die keine Antwort erhalten haben.
      2. RTT in milliseconds (Roundtripzeit in ms): Dies ist die Roundtripzeit in Millisekunden, wenn Quellen über die oben angegebene Testkonfiguration eine Verbindung mit dem Ziel herstellen.

      ![Testgruppe hinzufügen](./media/connection-monitor-2-preview/add-test-config.png)

Alle Quellen und Ziele, die einer Testgruppe mit der angegebenen Testkonfiguration hinzugefügt wurden, werden separat getestet. Beispiel:

* Testgruppe: TG1
* Quellen: 3 (A, B, C)
* Ziele: 2 (D, E)
* Testkonfiguration: 2 (Config 1, Config 2)
* Erstellte Tests: Gesamtzahl = 12

| **Testnummer** | **Quelle** | **Ziel** | **Namen der Testkonfiguration** |
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

* Maximale Anzahl von Verbindungsmonitoren pro Abonnement und Region: 100
* Maximale Anzahl von Testgruppen pro Verbindungsmonitor: 20
* Maximale Anzahl von Quellen und Zielen pro Verbindungsmonitor: 100
* Maximale Anzahl von Testkonfigurationen pro Verbindungsmonitor: 20 über ARMClient, 2 über das Portal

## <a name="step-4--data-analysis-and-alerts"></a>Schritt 4:  Datenanalyse und Warnungen

Wenn ein Verbindungsmonitor erstellt wird, überprüfen die Quellen die Konnektivität zu den Zielen auf Grundlage der angegebenen Testkonfiguration.

### <a name="checks-in-a-test"></a>Überprüfungen in einem Test

Basierend auf dem vom Benutzer in der Testkonfiguration ausgewählten Protokoll führt der Verbindungsmonitor (Vorschauversion) in der festgelegten Testhäufigkeit eine Reihe von Überprüfungen für das Quelle-Ziel-Paar aus.

Wenn HTTP ausgewählt ist, berechnet der Dienst die Anzahl von HTTP-Antworten, die einen Antwortcode zurückgegeben haben, um den Prozentanteil der fehlgeschlagenen Überprüfungen zu ermitteln.  Zum Berechnen der Roundtripzeit wird die Zeit gemessen, die nach einem HTTP-Aufruf bis zum Empfang der Antwort vergeht.

Wenn TCP oder ICMP ausgewählt ist, berechnet der Dienst den Prozentanteil der verlorenen Pakete, um den Prozentanteil der fehlgeschlagenen Überprüfungen zu ermitteln. Zum Berechnen der Roundtripzeit wird die Zeit erfasst, bis die Bestätigung für die gesendeten Pakete eingeht. Wenn Sie Traceroutedaten für Ihre Netzwerktests aktiviert haben, können Sie sich den Hop-by-Hop-Verlust und die Latenz für Ihr lokales Netzwerk anzeigen lassen.

### <a name="states-of-a-test"></a>Teststatus

In Abhängigkeit von den Daten, die von den Überprüfungen in einem Test zurückgegeben werden, kann ein Test die folgenden Status aufweisen:

* Erfolgreich: Dies ist der Fall, wenn die tatsächlichen Werte für „Checks Failed %“ (Überprüfungen mit Fehlern %) und „RTT“ innerhalb der angegebenen Schwellenwerte liegen.
* Fehler: Dies ist der Fall, wenn die tatsächlichen Werte für „Checks Failed %“ (Überprüfungen mit Fehlern %) und „RTT“ die angegebenen Schwellenwerte übersteigen. Wenn kein Schwellenwert festgelegt ist, schlägt ein Test fehl, wenn bei 100 % der Überprüfungen Fehler auftreten.
* Warnung: Dies ist der Fall, wenn keine Kriterien für „checks failed %“ (Überprüfungen mit Fehlern %) angegeben sind. In einem solchen Fall verwendet der Verbindungsmonitor (Vorschauversion) ein automatisch festgelegtes Kriterium als Schwellenwert. Wenn dieser Schwellenwert überschritten wird, wird der Teststatus auf „Warnung“ gesetzt.

### <a name="data-collection-analysis-and-alerts"></a>Datensammlung, Datenanalyse und Datenwarnungen

Alle vom Verbindungsmonitor (Vorschauversion) erfassten Daten werden in dem Log Analytics-Arbeitsbereich gespeichert, der bei der Erstellung des Verbindungsmonitors konfiguriert wurde. Über Azure Monitor-Metriken sind auch Überwachungsdaten verfügbar. Mit Log Analytics können Sie die Überwachungsdaten so lange aufbewahren wie Sie möchten. Azure Monitor speichert Metriken jedoch standardmäßig für 30 Tage **.** Anschließend können Sie [metrikbasierte Warnungen für die Daten festlegen](https://azure.microsoft.com/blog/monitor-at-scale-in-azure-monitor-with-multi-resource-metric-alerts/).

#### <a name="monitoring-dashboards-in-connection-monitor-solution"></a>Überwachen von Dashboards in der Lösung „Verbindungsmonitor“

Ihnen wird eine Liste der Verbindungsmonitore angezeigt, auf die Sie für bestimmte Abonnements, Regionen, Zeitstempel, Quellen und Zieltypen Zugriff haben.

Wenn Sie im Dienst „Network Watcher“ zum Verbindungsmonitor (Vorschauversion) navigieren, können Sie sich mit der Option **Anzeigen nach** Folgendes anzeigen lassen:

* Connection Monitor (default) (Verbindungsmonitor (Standard)): Dies ist eine Liste aller Verbindungsmonitore, die für bestimmte Abonnements, Regionen, Zeitstempel, Quellen und Zieltypen erstellt wurden.
* Testgruppen: Dies ist eine Liste aller Testgruppen, die für bestimmte Abonnements, Regionen, Zeitstempel, Quellen und Zieltypen erstellt wurden. Diese Testgruppen werden nicht im Verbindungsmonitor gefiltert.
* Tests: Dies ist eine Liste aller Tests, die für bestimmte Abonnements, Regionen, Zeitstempel, Quellen und Zieltypen ausgeführt werden. Diese Tests werden nicht nach Verbindungsmonitor oder Testgruppen gefiltert.

Sie können alle Verbindungsmonitore und Testgruppen erweitern, sodass die entsprechenden Testgruppen bzw. die verschiedenen einzelnen Tests angezeigt werden, die im Dashboard ausgeführt werden. Die Option ist im Screenshot unten mit [1] gekennzeichnet.

Für die Liste sind folgende Filter verfügbar:

* Filter der obersten Ebene: Abonnements, Regionen, Zeitstempel, Quellen und Zieltypen. Die Filter sind im Screenshot unten mit [2] gekennzeichnet.
* Zustandsbasierte Filter: Dies sind Filter der zweiten Ebene für den Zustand des Verbindungsmonitors/der Testgruppe/des Tests. Dieser Bereich ist im Screenshot unten mit [3] gekennzeichnet.
* Suchfeld: Für eine generische Suche klicken Sie auf „Alle“. Wenn Sie nach einer bestimmten Entität suchen möchten, grenzen Sie die Suchergebnisse mithilfe der Dropdownliste ein. Diese Filter sind im Screenshot unten mit [4] gekennzeichnet.

![Tests filtern](./media/connection-monitor-2-preview/cm-view.png)

Beispiel:

1. So suchen Sie in allen Verbindungsmonitoren (Vorschauversion) nach allen Tests, bei denen die Quell-IP 10.192.64.56 lautet:
   1. Ändern Sie den Wert der Option „Anzeigen nach“ in „Tests“.
   2. Geben Sie „10.192.64.56“ in das Suchfeld ein.
   3. Wählen Sie in der Dropdownliste neben „Wert“ die Option „Quellen“ aus.
2. So filtern Sie in allen Verbindungsmonitoren (Vorschauversion) nach fehlgeschlagenen Tests, bei denen die Quell-IP 10.192.64.56 lautet:
   1. Ändern Sie den Wert der Option „Anzeigen nach“ in „Tests“.
   2. Wählen Sie aus den zustandsbasierten Filtern „Fehler“ aus.
   3. Geben Sie „10.192.64.56“ in das Suchfeld ein.
   4. Wählen Sie in der Dropdownliste neben „Wert“ die Option „Quellen“ aus.
3. So filtern Sie in allen Verbindungsmonitoren (Vorschauversion) nach fehlgeschlagenen Tests, bei denen das Ziel „outlook.office365.com“ lautet:
   1. Ändern Sie den Wert der Option „Anzeigen nach“ in „Tests“.
   2. Wählen Sie aus den zustandsbasierten Filtern „Fehler“ aus.
   3. Geben Sie „outlook.office365.com“ in das Suchfeld ein.
   4. Wählen Sie in der Dropdownliste neben „Wert“ die Option „Ziele“ aus.

   ![Tests mit Fehlern](./media/connection-monitor-2-preview/tests-view.png)

So zeigen Sie die Trends von „checks failed %“ (Überprüfungen mit Fehlern %) und „RTT“ für Folgendes an:

1. Verbindungsmonitor
   1. Klicken Sie auf den Verbindungsmonitor, den Sie im Detail untersuchen möchten.
   2. Die Überwachungsdaten werden automatisch nach Testgruppen sortiert angezeigt.

      ![Metriken anzeigen nach](./media/connection-monitor-2-preview/cm-drill-landing.png)

   3. Wählen Sie die Testgruppe aus, die Sie im Detail untersuchen möchten.

      ![Metriken nach Testgruppe](./media/connection-monitor-2-preview/cm-drill-select-tg.png)

   4. Für die Testgruppe, die Sie im vorherigen Schritt ausgewählt haben, werden Ihnen die fünf Tests mit den höchsten Werten für „checks failed %“ (Überprüfungen mit Fehlern %) oder „RTT msecs“ (RTT in ms) angezeigt. Für jeden Test werden Trendlinien für „checks failed %“ (Überprüfungen mit Fehlern %) oder „RTT msec“ (RTT in ms) dargestellt.
   5. Wählen Sie aus der Liste oben einen Test aus, oder wählen Sie einen anderen Test aus, den Sie genauer untersuchen möchten.
   6. Für das ausgewählte Zeitintervall und für „checks failed %“ (Überprüfungen mit Fehlern %) werden der Schwellenwert und die tatsächlichen Werte angezeigt. Für „RTT msec“ (RTT in ms) sind die Werte „Schwellenwert“, „Mittelw.“, „Min.“ und „Max.“ verfügbar.

      ![RTT](./media/connection-monitor-2-preview/cm-drill-charts.png)

  7. Ändern Sie das Zeitintervall, damit mehr Daten angezeigt werden.
  8. Sie können die Ansicht in Schritt b ändern und die Ansicht nach Quellen, Zielen oder Testkonfigurationen filtern. Wählen Sie dann nach den Überprüfungen mit Fehlern eine Quelle aus, und untersuchen Sie die fünf Tests mit den meisten Fehlern.  Beispiel: Legen Sie „Anzeigen nach“ auf Quellen und Ziele fest, um alle Tests zu untersuchen, die zwischen dieser Kombination im ausgewählten Verbindungsmonitor ausgeführt werden.

      ![RTT2](./media/connection-monitor-2-preview/cm-drill-select-source.png)

2. Testgruppe
   1. Klicken Sie auf die Testgruppe, die Sie im Detail untersuchen möchten.
   2. Die Überwachungsdaten werden automatisch nach Quelle, Ziel und Testkonfiguration (Test) sortiert angezeigt.

      ![RTT3](./media/connection-monitor-2-preview/tg-drill.png)

   3. Wählen Sie den Test aus, den Sie im Detail untersuchen möchten.
   4. Für das ausgewählte Zeitintervall und für „checks failed %“ (Überprüfungen mit Fehlern %) werden der Schwellenwert und die tatsächlichen Werte angezeigt. Für „RTT msec“ (RTT in ms) sind die Werte „Schwellenwert“, „Mittelw.“, „Min.“ und „Max.“ verfügbar. Ihnen werden außerdem Warnungen angezeigt, die für den ausgewählten Test ausgelöst wurden.
   5. Ändern Sie das Zeitintervall, damit mehr Daten angezeigt werden.
   6. Sie können die Ansicht in Schritt b ändern und die Ansicht nach Quellen, Zielen oder Testkonfigurationen filtern. Wählen Sie dann eine Entität aus, für die Sie die fünf Tests mit den meisten Fehlern untersuchen möchten.  Beispiel: Legen Sie „Anzeigen nach“ auf Quellen und Ziele fest, um alle Tests zu untersuchen, die zwischen dieser Kombination im ausgewählten Verbindungsmonitor ausgeführt werden.

3. Test
   1. Klicken Sie auf die Quelle, das Ziel und die Testkonfiguration, die Sie genauer untersuchen möchten.
   2. Für das ausgewählte Zeitintervall und für „checks failed %“ (Überprüfungen mit Fehlern %) werden der Schwellenwert und die tatsächlichen Werte angezeigt. Für „RTT msec“ (RTT in ms) sind die Werte „Schwellenwert“, „Mittelw.“, „Min.“ und „Max.“ verfügbar. Ihnen werden außerdem Warnungen angezeigt, die für den ausgewählten Test ausgelöst wurden.

      ![Test1](./media/connection-monitor-2-preview/test-drill.png)

   3. Sie können auch auf „Topologie“ klicken, um sich die Netzwerktopologie zu einem beliebigen Zeitpunkt anzeigen zu lassen.

      ![Test2](./media/connection-monitor-2-preview/test-topo.png)

   4. Sie können für das Azure-Netzwerk auf jeden Linkhop klicken, um die vom Verbindungsmonitor ermittelten Probleme aufzurufen. Diese Funktion ist zurzeit nicht für lokale Netzwerke verfügbar.

       ![Test3](./media/connection-monitor-2-preview/test-topo-hop.png)

#### <a name="log-queries-in-azure-monitor-log-analytics"></a>Protokollieren von Abfragen in Azure Monitor Log Analytics

Mit Log Analytics lassen sich benutzerdefinierte Ansichten Ihrer Überwachungsdaten erstellen. Alle Daten, die in der Benutzeroberfläche angezeigt werden, stammen aus Log Analytics. Sie können eine interaktive Analyse von Daten im Repository ausführen und Daten aus verschiedenen Quellen korrelieren, z. B. aus der Agent-Integritätsdiagnose und aus anderen auf Log Analytics basierenden Anwendungen. Außerdem haben Sie die Möglichkeit, die Daten nach Excel oder Power BI zu exportieren oder einen freigabefähigen Link zu erstellen.

#### <a name="metrics-in-azure-monitor"></a>Metriken in Azure Monitor

Bei Verbindungsmonitoren, die vor dem Verbindungsmonitor (Vorschauversion) erstellt wurden, sind alle vier Metriken verfügbar. Bei Verbindungsmonitoren, die mithilfe des Verbindungsmonitors (Vorschauversion) erstellt wurden, sind Daten nur für die mit „(Preview)“ (Vorschau) gekennzeichneten Metriken verfügbar.

Ressourcentyp: Microsoft.Network/networkWatchers/connectionMonitors

| Metrik | Metrikanzeigename | Einheit | Aggregationstyp | BESCHREIBUNG | Dimensionen |
| --- | --- | --- | --- | --- | --- |
| ProbesFailedPercent | Fehlerhafte Tests in Prozent | Percent | Average | Fehlerhafte Tests bei Konnektivitätsüberwachung in Prozent | Keine Dimensionen |
| AverageRoundtripMs | Durchschn. Roundtripzeit (ms) | Millisekunden | Average | Durchschnittliche Netzwerk-Roundtripzeit (ms) für die Konnektivitätsüberwachung von Testdatenverkehr, der zwischen Quelle und Ziel gesendet wurde. |             Keine Dimensionen |
| ChecksFailedPercent (Preview) | % der Überprüfungen mit Fehlern (Vorschau) | Percent | Average | Prozentanteil der Überprüfungen, die bei einem Test fehlgeschlagen sind | * ConnectionMonitorResourceId <br> * SourceAddress <br> * SourceName <br> * SourceResourceId <br> * SourceType <br> * Protocol <br> * DestinationAddress <br> * DestinationName <br> * DestinationResourceId <br> * DestinationType <br> * DestinationPort <br> *  TestGroupName <br> *  TestConfigurationName <br> * Region |
| RoundTripTimeMs (Vorschau) | Roundtripzeit (ms) (Vorschau) | Millisekunden | Average | Roundtripzeit (ms) von Überprüfungen, die zwischen der Quelle und dem Ziel gesendet werden. Es wird kein Durchschnittswert gebildet. | * ConnectionMonitorResourceId <br> * SourceAddress <br> * SourceName <br> * SourceResourceId <br> * SourceType <br> * Protocol <br> * DestinationAddress <br> * DestinationName <br> * DestinationResourceId <br> * DestinationType <br> * DestinationPort <br> *  TestGroupName <br> *  TestConfigurationName <br> * Region |

 ![Monitormetriken](./media/connection-monitor-2-preview/monitor-metrics.png)

#### <a name="metric-alerts-in-azure-monitor"></a>Metrikwarnungen in Azure Monitor

So erstellen Sie eine Warnung

1. Wählen Sie Ihre Verbindungsmonitorressource aus, die Sie mithilfe des Verbindungsmonitors (Vorschauversion) erstellt haben.
2. Stellen Sie sicher, dass „Metrik“ als Signaltyp für die im vorherigen Schritt ausgewählte Ressource angezeigt wird.
3. Wählen Sie unter „Bedingung hinzufügen“ für „Signalname“ „ChecksFailedPercent(Preview)“ oder „RoundTripTimeMs(Preview)“ und für „Signaltyp“ „Metriken“ aus. Beispiel: Wählen Sie „ChecksFailedPercent(Preview)“ aus.
4. Alle für die Metriken verfügbaren Dimensionen werden aufgelistet.  Wählen Sie den Dimensionsnamen und -wert aus. Beispiel: Geben Sie für „Quelladresse“ die IP-Adresse von einer der Quellen an, die in die Verbindungsmonitorressource eingebunden ist, die Sie in Schritt 1 ausgewählt haben.
5. Legen Sie unter „Warnungslogik“ Folgendes fest:
   1. Bedingungstyp: Statisch
   2. Bedingung und Schwellenwert
   3. Aggregationsgranularität und Häufigkeit der Auswertung: Der Verbindungsmonitor (Vorschauversion) wird einmal pro Minute aktualisiert.
6.  Wählen Sie unter „Aktionen“ Ihre Aktionsgruppe aus.
7. Legen Sie die Warnungsdetails fest.
8. Erstellen einer Warnungsregel

   ![Alerts](./media/connection-monitor-2-preview/mdm-alerts.jpg)

## <a name="step-5-diagnose-issues-in-your-network"></a>Schritt 5: Diagnostizieren von Problemen in Ihrem Netzwerk

Mit dem Verbindungsmonitor können Sie leichter Probleme diagnostizieren, die mit der Verbindungsmonitorressource und Ihrem Netzwerk zusammenhängen. Probleme in Ihrem Hybridnetzwerk werden von den Log Analytics-Agents erkannt, die Sie in Schritt 1 installiert haben. Probleme in Azure werden von der Erweiterung „Network Watcher“ ermittelt.  Probleme im Hybridnetzwerk werden auf der Seite „Diagnose“ und Probleme im Azure-Netzwerk in der Netzwerktopologie angezeigt.

Bei Netzwerken mit lokalen VMs als Quellen werden folgende Probleme erkannt:

* Bei der Anforderung ist ein Timeout aufgetreten.
* Der Endpunkt wurde nicht über DNS aufgelöst (vorübergehend oder dauerhaft). Die URL ist ungültig.
* Es wurden keine Hosts gefunden.
* Die Quelle kann keine Verbindung zum Ziel herstellen. Das Ziel ist nicht über ICMP erreichbar.
* Zertifikatbezogene Probleme: Der Agent muss mit dem Clientzertifikat authentifiziert werden, kein Zugriff auf Zertifikatssperrliste, Hostname des Endpunkts stimmt nicht mit dem Antragsteller des Zertifikats oder dem alternativen Antragstellernamen überein, Stammzertifikat fehlt im Speicher der vertrauenswürdigen Zertifizierungsstellen auf dem lokalen Computer der Quelle, SSL-Zertifikat abgelaufen/ungültig/widerrufen, nicht kompatibel.

Bei Netzwerken mit Azure-VMs als Quellen werden folgende Probleme erkannt:

* Probleme mit Agents: Agent angehalten, fehlgeschlagene DNS-Auflösung, keine Anwendung/kein Listener lauscht am Zielport, Socket konnte nicht geöffnet werden
* Probleme mit dem VM-Zustand: wird gestartet/beendet, wurde beendet, Bereitstellung wird aufgehoben, Bereitstellung wurde aufgehoben, wird neu gestartet, nicht bereitgestellt
* Fehlender ARP-Tabelleneintrag
* Datenverkehr aufgrund lokaler Probleme mit den Firewall/NSG-Regeln blockiert
* VNET-Gateway: Fehlende Routen, Tunnel zwischen zwei Gateways getrennt oder fehlend/das zweite Gateway wurde nicht vom Tunnel gefunden, keine Peeringinformationen gefunden
* Fehlende Route in Microsoft Edge
* Datenverkehr aufgrund von Systemrouten oder UDR unterbrochen
* BGP nicht für die Gatewayverbindung aktiviert
* DIP-Test beim Lastenausgleich nicht verfügbar
