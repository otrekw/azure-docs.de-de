---
title: Richtlinien für Proxyserver in Windows Virtual Desktop – Azure
description: Einige Richtlinien und Empfehlungen für die Verwendung von Proxyservern in Windows Virtual Desktop-Bereitstellungen.
author: Heidilohr
ms.topic: conceptual
ms.date: 04/27/2021
ms.author: helohr
ms.reviewer: sefriend
manager: femila
ms.openlocfilehash: cb2bd4ddb553380a037f58cf8c51e80247e9c055
ms.sourcegitcommit: 2e123f00b9bbfebe1a3f6e42196f328b50233fc5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/27/2021
ms.locfileid: "108077667"
---
# <a name="proxy-server-guidelines-for-windows-virtual-desktop"></a>Richtlinien für Proxyserver in Windows Virtual Desktop

In diesem Artikel erfahren Sie, wie Sie einen Proxyserver mit Windows Virtual Desktop verwenden. Die Empfehlungen in diesem Artikel gelten nur für Verbindungen zwischen der Windows Virtual Desktop-Infrastruktur, dem Client und den Sitzungshost-Agents. Die Netzwerkkonnektivität für Office, Windows 10, FSLogix oder andere Microsoft-Anwendungen wird in diesem Artikel nicht behandelt.

## <a name="what-are-proxy-servers"></a>Was sind Proxyserver?

Es wird empfohlen, Proxys für Datenverkehr in Windows Virtual Desktop zu umgehen. Proxys sorgen nicht für eine höhere Sicherheit in Windows Virtual Desktop, da der Datenverkehr bereits verschlüsselt ist. Weitere Informationen zur Verbindungssicherheit finden Sie unter [Verbindungssicherheit](network-connectivity.md#connection-security). 

Die meisten Proxyserver sind nicht zur Unterstützung von zeitintensiven WebSocket-Verbindungen konzipiert und können die Verbindungsstabilität beeinträchtigen. Auch die Skalierbarkeit von Proxyservern verursacht Probleme, da in Windows Virtual Desktop mehrere Langzeitverbindungen verwendet werden. Wenn Sie dennoch Proxyserver verwenden, müssen diese die richtige Größe zur Ausführung dieser Verbindungen aufweisen.

Wenn sich ein Proxyserver weit entfernt vom Host befindet, erhöht sich durch die Entfernung die Latenz bei den Verbindungen von Benutzern. Eine höhere Latenz bedeutet eine verlangsamte Verbindungszeit und eingeschränkte Benutzerfreundlichkeit, insbesondere in Szenarien, in denen Grafiken, Audio oder latenzarme Interaktionen mit Eingabegeräten erforderlich sind. Wenn Sie einen Proxyserver verwenden möchten, denken Sie daran, dass Sie den Server in derselben geografischen Region wie den Windows Virtual Desktop-Agent und -Client platzieren müssen.

Wenn Sie den Proxyserver als einzigen Pfad für Windows Virtual Desktop-Datenverkehr konfigurieren, werden die RDP-Daten (Remote Desktop Protocol) nicht über UDP (User Datagram Protocol), sondern über TCP (Transmission Control Protocol) erzwungen. Dadurch verringert sich die visuelle Qualität und die Reaktionsfähigkeit der Remoteverbindung.

Zusammenfassend wird von der Verwendung von Proxyservern in Windows Virtual Desktop abgeraten, da sie durch Latenzverschlechterung und Paketverluste leistungsbezogene Probleme verursachen. 

## <a name="bypassing-a-proxy-server"></a>Umgehen eines Proxyservers

Wenn die Netzwerk- und Sicherheitsrichtlinien Ihrer Organisation Proxyserver für Webdatenverkehr vorschreiben, können Sie die Umgebung so konfigurieren, dass Windows Virtual Desktop-Verbindungen umgangen werden, der Datenverkehr jedoch weiterhin über den Proxyserver geleitet wird. Die Richtlinien jeder Organisation sind jedoch verschieden, sodass sich einige Methoden für Ihre Bereitstellung besser eignen können als andere. Im Folgenden sind einige Konfigurationsmethoden aufgeführt, die Sie testen können, um Beeinträchtigungen bei der Leistung und Zuverlässigkeit in Ihrer Umgebung zu verhindern:

- Azure-Diensttags in Azure Firewall
- Proxyserverumgehung unter Verwendung von PAC-Dateien (Proxy Auto Configuration)
- Umgehungsliste in der lokalen Proxykonfiguration 
- Verwendung von Proxyservern für die benutzerspezifische Konfiguration 
- Verwendung von RDP Shortpath für die RDP-Verbindung unter Beibehaltung des Dienstdatenverkehrs über den Proxy 

## <a name="recommendations-for-using-proxy-servers"></a>Empfehlungen zur Verwendung von Proxyservern

Einige Organisationen setzen voraus, dass der gesamte Benutzerdatenverkehr zur Nachverfolgung oder Paketuntersuchung über einen Proxyserver erfolgt. In diesem Abschnitt wird die empfohlene Konfiguration Ihrer Umgebung in diesen Fällen beschrieben.

### <a name="use-proxy-servers-in-the-same-azure-geography"></a>Verwenden von Proxyservern in derselben geografischen Azure-Region

Bei Verwendung eines Proxyservers wird mit diesem die gesamte Kommunikation in der Windows Virtual Desktop-Infrastruktur verarbeitet und die DNS-Auflösung und das Anycast-Routing in der nächstgelegenen Azure Front Door-Instanz ausgeführt. Wenn die Proxyserver weit entfernt oder über eine geografische Azure-Region verteilt sind, ist die geografische Auflösung weniger genau. Diese ungenaue geografische Auflösung führt dazu, dass Verbindungen an einen weiter entfernten Windows Virtual Desktop-Cluster weitergeleitet werden. Dies lässt sich vermeiden, indem Sie nur Proxyserver verwenden, die sich geografisch in der Nähe des Windows Virtual Desktop-Clusters befinden.

### <a name="use-rdp-shortpath-for-desktop-connectivity"></a>Verwenden von RDP Shortpath für die Desktop-Konnektivität

Wenn Sie RDP Shortpath aktivieren, werden RDP-Daten nach Möglichkeit durch Umgehen des Proxyservers weitergeleitet. Das Umgehen des Proxyservers ermöglicht ein optimales Routing durch Verwendung des UDP-Transportprotokolls. Sonstiger Windows Virtual Desktop-Datenverkehr, z. B. Broker, Orchestrierung und Diagnose, erfolgt weiterhin über den Proxyserver. 

### <a name="dont-use-ssl-termination-on-the-proxy-server"></a>Deaktivieren der SSL-Terminierung auf dem Proxyserver

Bei der SSL-Terminierung (Secure Sockets Layer) werden die Sicherheitszertifikate der Windows Virtual Desktop-Komponenten durch Zertifikate ersetzt, die vom Proxyserver generiert werden. Diese Funktion für Proxyserver ermöglicht die Paketuntersuchung für HTTPS-Datenverkehr auf dem Proxyserver. Durch die Paketuntersuchung erhöht sich jedoch auch die Dienstantwortzeit, sodass die Anmeldung der Benutzer länger dauert. In Szenarien mit umgekehrter Verbindung ist die Paketuntersuchung des RDP-Datenverkehrs nicht erforderlich, da der RDP-Datenverkehr mit umgekehrter Verbindung binär erfolgt und zusätzliche Verschlüsselungsebenen verwendet werden.

Wenn Sie den Proxyserver zur Verwendung der SSL-Überprüfung konfigurieren, sollten Sie beachten, dass der Server nicht auf den ursprünglichen Status zurückgesetzt werden kann, nachdem bei der SSL-Überprüfung Änderungen vorgenommen wurden. Wenn bei aktivierter SSL-Überprüfung in der Windows Virtual Desktop-Umgebung Fehler auftreten, müssen Sie vor dem Öffnen einer Supportanfrage die SSL-Überprüfung deaktivieren und den Vorgang wiederholen. Die SSL-Überprüfung kann auch dazu führen, dass der Windows Virtual Desktop-Agent nicht mehr ausgeführt wird, da vertrauenswürdige Verbindungen zwischen dem Agent und dem Dienst beeinträchtigt werden.

### <a name="dont-use-proxy-servers-that-need-authentication"></a>Verwendung keiner Proxyserver, die eine Authentifizierung erfordern

Windows Virtual Desktop-Komponenten auf dem Sitzungshost werden im Kontext des zugehörigen Betriebssystems ausgeführt, sodass sie keine Proxyserver unterstützen, die eine Authentifizierung erfordern. Wenn eine Authentifizierung für den Proxyserver erforderlich ist, treten bei der Verbindung Fehler auf.

### <a name="plan-for-the-proxy-server-network-capacity"></a>Planen der Netzwerkkapazität von Proxyservern

Für Proxyserver gelten Kapazitätsgrenzen. Im Unterschied zum regulären HTTP-Datenverkehr umfasst der RDP-Datenverkehr zeitintensive, aktive bidirektionale Verbindungen, die eine hohe Bandbreite beanspruchen. Erkundigen Sie sich vor dem Einrichten eines Proxyservers bei dem Proxyserveranbieter nach dem Durchsatz des Servers. Informieren Sie sich außerdem über die Anzahl der Proxysitzungen, die gleichzeitig ausgeführt werden können. Überwachen Sie nach der Bereitstellung des Proxyservers die Ressourcenverwendung sorgfältig auf Engpässe im Windows Virtual Desktop-Datenverkehr.

### <a name="proxy-servers-for-windows-7-session-hosts"></a>Proxyserver für Windows 7-Sitzungshosts

Sitzungshosts, die unter Windows 7 ausgeführt werden, unterstützen keine Proxyserververbindungen für RDP-Daten mit umgekehrter Verbindung. Wenn der Sitzungshost keine direkte Verbindung mit den Windows Virtual Desktop-Gateways herstellen kann, funktioniert die Verbindung nicht.

### <a name="proxy-servers-and--teams-optimization"></a>Proxyserver und Teams-Optimierung

In Windows Virtual Desktop werden Proxyserver nicht für die Teams-Optimierung unterstützt.

## <a name="session-host-configuration-recommendations"></a>Empfehlungen für die Sitzungshostkonfiguration

Zum Konfigurieren eines Proxyservers auf Sitzungshostebene müssen Sie einen systemweiten Proxy aktivieren. Beachten Sie dabei, dass sich die systemweite Konfiguration auf alle Betriebssystemkomponenten und -anwendungen auswirkt, die auf dem Sitzungshost ausgeführt werden. Die folgenden Abschnitte enthalten Empfehlungen zum Konfigurieren systemweiter Proxys.
 
### <a name="use-the-web-proxy-auto-discovery-wpad-protocol"></a>Verwenden des WPAD-Protokolls (Web Proxy Auto-Discovery)

Der Windows Virtual Desktop-Agent versucht automatisch, über das WPAD-Protokoll (Web Proxy Auto-Discovery) einen Proxyserver im Netzwerk zu finden. Bei der Suche durchsucht der Agent den Domänennamenserver (DNS) nach der Datei **wpad.domainsuffix**. Wenn der Agent die Datei auf dem DNS findet, wird eine HTTP-Anforderung für die Datei **wpad.dat** ausgeführt. Die Antwort wird zum Proxykonfigurationsskript, über das der ausgehende Proxyserver ausgewählt wird.

Um das Netzwerk für die Verwendung der DNS-Auflösung für WPAD zu konfigurieren, befolgen Sie die Anweisungen unter [Einstellungen für die automatische Erkennung für Internet Explorer 11](/internet-explorer/ie11-deploy-guide/auto-detect-settings-for-ie11). Stellen Sie sicher, dass die Sperrliste für globale Abfragen des DNS-Servers die WPAD-Auflösung zulässt, indem Sie die Anweisungen unter [Set-DnsServerGlobalQueryBlockList](/powershell/module/dnsserver/set-dnsserverglobalqueryblocklist?view=windowsserver2019-ps&preserve-view=true) befolgen.

### <a name="manually-set-a-device-wide-internet-explorer-proxy"></a>Manuelles Festlegen eines geräteweiten Internet Explorer-Proxys

Mit dem [NetworkProxy-Konfigurationsdienstanbieter](/windows/client-management/mdm/networkproxy-csp) können Sie einen geräteweiten Proxy oder eine PAC-Datei (Proxy Auto Configuration) festlegen, die für alle interaktiven, LocalSystem- und NetworkService-Benutzer gilt. 

Sie können den Proxyserver auch für das lokale Systemkonto konfigurieren, indem Sie den Befehl **bitsadmin** wie im folgenden Beispiel gezeigt ausführen: 

```console
bitsadmin /util /setieproxy LOCALSYSTEM AUTOSCRIPT http://server/proxy.pac 
```

## <a name="client-side-proxy-support"></a>Clientseitige Proxyunterstützung

Der Windows Virtual Desktop-Client unterstützt Proxyserver, die mit Systemeinstellungen oder einem [NetworkProxy-Konfigurationsdienstanbieter](/windows/client-management/mdm/networkproxy-csp) konfiguriert sind.

### <a name="support-for-clients-running-on-windows-7"></a>Unterstützung für Windows 7-Clients

Clients, die unter Windows 7 ausgeführt werden, unterstützen keine Proxyserververbindungen für RDP-Daten mit umgekehrter Verbindung. Wenn der Client keine direkte Verbindung mit den Windows Virtual Desktop-Gateways herstellen kann, funktioniert die Verbindung nicht.

### <a name="windows-virtual-desktop-client-support"></a>Unterstützung von Windows Virtual Desktop-Clients

In der folgenden Tabelle sind die Windows Virtual Desktop-Clients aufgeführt, die Proxyserver unterstützen:

| Clientname | Proxyserverunterstützung |
|---|---|
| Windows Desktop | Ja |
| Webclient | Ja |
| Android | Nein |
| iOS | Nein |
| macOS | Nein |
| Windows Store | Ja |

Weitere Informationen zur Proxyunterstützung auf Linux-basierten Thin Clients finden Sie unter [Thin Client-Unterstützung](linux-overview.md).

## <a name="support-limitations"></a>Einschränkungen bei der Unterstützung

Viele Dienste und Anwendungen von Drittanbietern fungieren als Proxyserver. Zu diesen Drittanbieterdiensten gehören verteilte Firewalls der nächsten Generation, Websicherheitssysteme und einfache Proxyserver. Die Kompatibilität der einzelnen Konfigurationen mit Windows Virtual Desktop kann nicht garantiert werden. Microsoft bietet nur eingeschränkte Unterstützung für Verbindungen, die über einen Proxyserver hergestellt werden. Wenn bei der Verwendung eines Proxyservers Verbindungsprobleme auftreten, empfiehlt der Microsoft-Support, eine Proxyumgehung zu konfigurieren und dann das Problem zu reproduzieren.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Schutz der Windows Virtual Desktop-Bereitstellung finden Sie in unserem [Sicherheitsleitfaden](security-guide.md).
