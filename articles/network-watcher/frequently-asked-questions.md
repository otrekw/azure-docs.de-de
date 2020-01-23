---
title: Häufig gestellte Fragen zu Azure Network Watcher | Microsoft-Dokumentation
description: In diesem Artikel werden häufig gestellte Fragen zum Dienst Azure Network Watcher beantwortet.
services: network-watcher
documentationcenter: na
author: damendo
manager: ''
editor: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/10/2019
ms.author: damendo
ms.openlocfilehash: 0eea6700b8b248a87666071ee02572d356110cd0
ms.sourcegitcommit: 8b37091efe8c575467e56ece4d3f805ea2707a64
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/09/2020
ms.locfileid: "75830172"
---
# <a name="frequently-asked-questions-faq-about-azure-network-watcher"></a>Häufig gestellte Fragen zu Azure Network Watcher
Dier Dienst [Azure Network Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) stellt eine Sammlung von Tools für die Überwachung, Diagnose, Metrikanzeige sowie zur Aktivierung oder Deaktivierung von Protokollen für Ressourcen in einem virtuellen Azure-Netzwerk bereit. In diesem Artikel werden häufig gestellte Fragen zum Dienst beantwortet.

## <a name="general"></a>Allgemein

### <a name="what-is-network-watcher"></a>Was ist Network Watcher?
Network Watcher ist für die Überwachung und Wiederherstellung der Netzwerkintegrität von IaaS-Komponenten (Infrastructure-as-a-Service) konzipiert. Dazu zählen virtuelle Computer, virtuelle Netzwerke, Application Gateway-Instanzen, Lastenausgleichsmodule und andere Ressourcen in einem virtuellen Azure-Netzwerk. Dabei handelt es sich nicht um eine Lösung für die Überwachung der PaaS-Infrastruktur (Platform-as-a-Service) oder zum Abrufen von Analysen zu Web- oder Mobiltechnologien.

### <a name="what-tools-does-network-watcher-provide"></a>Welche Tools bietet Network Watcher?
Network Watcher bietet drei Hauptgruppen von Funktionen.
* Überwachung
  * In der [Topologieansicht](https://docs.microsoft.com/azure/network-watcher/view-network-topology) sehen Sie die Ressourcen in Ihrem virtuellen Netzwerk und die Beziehungen zwischen ihnen.
  * Im [Verbindungsmonitor](https://docs.microsoft.com/azure/network-watcher/connection-monitor) können Sie die Konnektivität und Wartezeit zwischen einer VM und einer anderen Netzwerkressource überwachen.
  * Der [Netzwerkleistungsmonitor](https://docs.microsoft.com/azure/azure-monitor/insights/network-performance-monitor) ermöglicht Ihnen, Konnektivität und Wartezeiten in hybriden Netzwerkarchitekturen, bei ExpressRoute-Leitungen und von Dienst- und Anwendungsendpunkten zu überwachen.  
* Diagnose
  * Die Funktion [Überprüfen des IP-Flusses](https://docs.microsoft.com/azure/network-watcher/network-watcher-ip-flow-verify-overview) ermöglicht Ihnen das Erkennen von Problemen beim Filtern von Datenverkehr auf VM-Ebene.
  * [Nächster Hop ](https://docs.microsoft.com/azure/network-watcher/network-watcher-next-hop-overview) unterstützt Sie beim Überprüfen von Datenverkehrsrouten und Erkennen von Routingproblemen.
  * Die Funktion [Problembehandlung für Verbindung](https://docs.microsoft.com/azure/network-watcher/network-watcher-connectivity-portal) bietet eine einmalige Prüfung der Konnektivität und Wartezeit zwischen einer VM und einer anderen Netzwerkressource.
  * Die [Paketerfassung](https://docs.microsoft.com/azure/network-watcher/network-watcher-packet-capture-overview) ermöglicht das Erfassen des gesamten Datenverkehrs auf einer VM in Ihrem virtuellen Netzwerk.
  * Die [VPN-Problembehandlung ](https://docs.microsoft.com/azure/network-watcher/network-watcher-troubleshoot-overview) führt mehrere Diagnoseprüfungen für VPN-Gateways und-Verbindungen aus, um beim Beheben von Problemen zu helfen.
* Protokollierung
  * [NSG-Flussprotokolle](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview) ermöglichen Ihnen das Protokollieren des gesamten Datenverkehrs in Ihren [Netzwerksicherheitsgruppen (NSGs)](https://docs.microsoft.com/azure/virtual-network/security-overview).
  * [Traffic Analytics](https://docs.microsoft.com/azure/network-watcher/traffic-analytics) verarbeitet Ihre NSG-Flowprotokolldaten, sodass Sie Ihren Netzwerkdatenverkehr visualisieren, abfragen, analysieren und verstehen können.


Weitere Informationen finden Sie auf der [Seite mit der Übersicht über Network Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview).


### <a name="how-does-network-watcher-pricing-work"></a>Welche Preise gelten für Network Watcher?
Besuchen Sie die Seite [Preise](https://azure.microsoft.com/pricing/details/network-watcher/), auf der Sie die Network Watcher-Komponenten und ihre Preise finden.

### <a name="which-regions-is-network-watcher-supportedavailable-in"></a>In welchen Regionen ist Network Watcher verfügbar und wird unterstützt?
Sie können die aktuelle regionale Verfügbarkeit auf der Azure-Seite [Verfügbare Produkte nach Region](https://azure.microsoft.com/global-infrastructure/services/?products=network-watcher) anzeigen.

### <a name="what-are-resource-limits-on-network-watcher"></a>Welche Ressourcengrenzwerte gelten für Network Watcher?
Alle Grenzwerte finden Sie unter [Network Watcher-Grenzwerte](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#network-watcher-limits).  

### <a name="why-is-only-one-instance-of-network-watcher-allowed-per-region"></a>Warum ist pro Region nur eine einzelne Network Watcher-Instanz zulässig?
Network Watcher muss lediglich einmal für ein Abonnement aktiviert werden, damit die zugehörigen Features funktionieren. Hierbei handelt es sich nicht um eine Diensteinschränkung.

### <a name="why-do-i-need-to-install-the-network-watcher-extension"></a>Warum muss ich die Network Watcher-Erweiterung installieren? 
Die Network Watcher-Erweiterung ist für alle Features erforderlich, die Datenverkehr von einer VM generieren oder abfangen müssen. 

### <a name="which-features-require-the-network-watcher-extension"></a>Welche Features erfordert die Network Watcher-Erweiterung?
Die Network Watcher-Erweiterung wird nur für die Paketerfassung, die Verbindungsproblembehandlung und den Verbindungsmonitor benötigt.

## <a name="nsg-flow-logs"></a>NSG-Flussprotokolle

### <a name="what-does-nsg-flow-logs-do"></a>Welche Aufgabe haben NSG-Flussprotokolle?
Azure-Netzwerkressourcen können mithilfe von [Netzwerksicherheitsgruppen (NSGs)](https://docs.microsoft.com/azure/virtual-network/security-overview) kombiniert und verwaltet werden. NSG-Flussprotokolle ermöglichen Ihnen, Flussinformationen mit 5 Tupeln zum gesamten Datenverkehr durch Ihre NSGs zu protokollieren. Die unformatierten Flowprotokolle werden in ein Azure Storage-Konto geschrieben, in dem Sie nach Bedarf weiterverarbeitet, analysiert, abgefragt oder exportiert werden können.

### <a name="how-do-i-use-nsg-flow-logs-on-a-storage-account-with-a-firewall-or-through-a-service-endpoints"></a>Wie verwende ich NSG-Flussprotokolle in einem Speicherkonto mit einer Firewall oder über Dienstendpunkte?

Sie müssen vertrauenswürdigen Microsoft-Diensten den Zugriff auf Ihr Speicherkonto erlauben, um ein Speicherkonto mit einer Firewall oder über einen Dienstendpunkt zu verwenden:

* Suchen Sie nach dem Namen des Speicherkontos, indem Sie auf der [Übersichtsseite der NSG-Flussprotokolle](https://ms.portal.azure.com/#blade/Microsoft_Azure_Network/NetworkWatcherMenuBlade/flowLogs) die NSG ermitteln.
* Navigieren Sie zum Speicherkonto, indem Sie im Portal den Namen des Speicherkontos in das Feld für die globale Suche eingeben.
* Wählen Sie unter **EINSTELLUNGEN** die Option **Firewalls und virtuelle Netzwerke**.
* Wählen Sie unter „Zugriff erlauben von“ die Option **Ausgewählte Netzwerke** aus. Aktivieren Sie anschließend unter **Ausnahmen** das Kontrollkästchen neben **Vertrauenswürdigen Microsoft-Diensten den Zugriff auf dieses Speicherkonto erlauben**. 
* Falls die Option bereits ausgewählt ist, ist keine Änderung erforderlich.  

Sie können nach einigen Minuten die Speicherprotokolle überprüfen, um zu ermitteln, ob ein aktualisierter Zeitstempel vorhanden ist oder eine neue JSON-Datei erstellt wurde.

### <a name="what-is-the-difference-between-flow-logs-versions-1--2"></a>Worin besteht der Unterschied bei den Flowprotokollen der Version 1 und 2?
In Flowprotokolle der Version 2 wurde des Konzept des *Flowzustands* eingeführt, gemäß dem Informationen zu Bytes und Paketen gespeichert werden. [Weitere Informationen](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview#log-file).

## <a name="next-steps"></a>Nächste Schritte
 - Besuchen Sie unsere [Seite mit der Übersicht über die Dokumentation](https://docs.microsoft.com/azure/network-watcher/), auf der Sie einige Tutorials finden, die Ihnen den Einstieg in Network Watcher erleichtern.
