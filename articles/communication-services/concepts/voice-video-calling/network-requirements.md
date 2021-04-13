---
title: Vorbereiten des Netzwerks Ihrer Organisation für Azure Communication Services
titleSuffix: An Azure Communication Services concept document
description: Enthält Informationen zu den Netzwerkanforderungen für Sprach- und Videoanrufe mit Azure Communication Services.
author: nmurav
manager: jken
services: azure-communication-services
ms.author: nmurav
ms.date: 3/23/2021
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: 22bf8af89771e57621a0d8851ca31bb750b7cda2
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105108348"
---
# <a name="ensure-high-quality-media-in-azure-communication-services"></a>Sicherstellen von hoher Qualität für Medien in Azure Communication Services

Dieses Dokument enthält eine Übersicht über die Faktoren und bewährten Methoden, die Sie beim Entwickeln von qualitativ hochwertigen Umgebungen für die Multimediakommunikation mit Azure Communication Services berücksichtigen sollten.

## <a name="factors-that-affect-media-quality-and-reliability"></a>Faktoren, die die Qualität und Zuverlässigkeit von Medien beeinflussen

Es gibt viele verschiedene Faktoren, die sich auf die Qualität von Echtzeitmedien (Audio, Video und Anwendungsfreigabe) in Azure Communication Services auswirken. Hierzu zählen Netzwerkqualität, Bandbreite, Firewall, Host und Gerätekonfigurationen.


### <a name="network-quality"></a>Netzwerkqualität

Die Qualität von Echtzeitmedien über IP wird durch die Qualität der zugrunde liegenden Netzwerkkonnektivität erheblich beeinträchtigt. Am wichtigsten ist hierbei das Ausmaß der folgenden Faktoren:
* **Wartezeit**: Dies ist die Zeit, die benötigt wird, um ein IP-Paket im Netzwerk von Punkt A zu Punkt B zu übertragen. Diese Verzögerung der Weitergabe im Netzwerk hängt von der physischen Entfernung zwischen den beiden Punkten sowie vom zusätzlichen Aufwand auf den Geräten ab, über die Ihr Datenverkehr fließt. Die Latenz wird für die unidirektionale Strecke bzw. für die Roundtripdauer (Round Trip Time, RTT) gemessen.
* **Paketverlust**: Steht für einen Prozentsatz an Paketen, die innerhalb eines bestimmten Zeitfensters verloren gehen. Der Paketverlust hat eine direkte Auswirkung auf die Audioqualität – von einzelnen kleinen Paketen mit sehr geringer Auswirkung bis zu direkt aufeinanderfolgenden Burstverlusten, die zu vollständigen Audioausfällen führen.
* **Jitter zwischen Paketeingängen oder regulärer Jitter**. Hierbei geht es um die durchschnittliche Änderung der Verzögerung zwischen aufeinanderfolgenden Paketen. In Azure Communication Services kann ein gewisser Grad an Jitter durch eine Pufferung ausgeglichen werden. Nur wenn der Jitter-Grad die Pufferung überschreitet, nimmt ein Teilnehmer die damit verbundenen Auswirkungen wahr.

### <a name="network-bandwidth"></a>Netzwerkbandbreite

Stellen Sie sicher, dass Ihr Netzwerk für die Unterstützung der Bandbreite konfiguriert ist, die für gleichzeitige Azure Communication Services-Mediensitzungen und andere Geschäftsanwendungen benötigt wird. Das Testen des End-to-End-Netzwerkpfads auf Engpässe bei der Bandbreite ist für die erfolgreiche Bereitstellung Ihrer Communication Services-Multimedialösung von entscheidender Bedeutung.

Hier sind die Bandbreitenanforderungen für die JavaScript-SDKs angegeben:

|Bandbreite|Szenarien|
|:--|:--|
|40 KBit/s|Peer-to-Peer-Audioanrufe|
|500 KBit/s|Peer-to-Peer-Audioanrufe und Bildschirmfreigabe|
|500 KBit/s|Videoanrufe in Peer-to-Peer-Qualität mit 360p bei 30 Bildern pro Sekunde|
|1,2 MBit/s|Peer-to-Peer-Videoanrufe in HD-Qualität mit einer Auflösung von HD 720p bei 30 Bildern pro Sekunde|
|500 KBit/s|Video-Gruppenanrufe mit 360p bei 30 Bildern pro Sekunde|
|1,2 MBit/s|Video-Gruppenanrufe in HD-Qualität mit einer Auflösung von HD 720p bei 30 Bildern pro Sekunde| 

Hier sind die Bandbreitenanforderungen für die nativen Android- und iOS-SDKs angegeben:

|Bandbreite|Szenarien|
|:--|:--|
|30 KBit/s|Peer-to-Peer-Audioanrufe |
|130 KBit/s|Peer-to-Peer-Audioanrufe und Bildschirmfreigabe|
|500 KBit/s|Videoanrufe in Peer-to-Peer-Qualität mit 360p bei 30 Bildern pro Sekunde|
|1,2 MBit/s|Peer-to-Peer-Videoanrufe in HD-Qualität mit einer Auflösung von HD 720p bei 30 Bildern pro Sekunde|
|1,5 Mbit/s|Peer-to-Peer-Videoanrufe in HD-Qualität mit einer Auflösung von HD 1080p bei 30 Bildern pro Sekunde |
|500 KBit/s bzw. 1 MBit/s|Video-Gruppenanrufe|
|1 MBit/s bzw. 2 MBit/s|Video-Gruppenanrufe in HD-Qualität (540p-Videos auf 1080p-Bildschirm)|

### <a name="firewalls-configuration"></a>Firewallkonfiguration

Für Azure Communication Services-Verbindungen wird eine Internetkonnektivität mit bestimmten Ports und IP-Adressen benötigt, um eine Multimediaumgebung mit hoher Qualität bereitstellen zu können. Auch wenn kein Zugriff auf diese Ports und IP-Adressen besteht, kann Azure Communication Services funktionieren. Die optimale Leistung wird aber nur erzielt, wenn die empfohlenen Ports und IP-Adressbereiche verfügbar sind.

| Category | IP-Adressbereiche oder FQDN | Ports | 
| :-- | :-- | :-- |
| Mediendatenverkehr | [IP-Adressbereiche für öffentliche Azure-Cloud](https://www.microsoft.com/download/confirmation.aspx?id=56519) | UDP 3478 bis 3481, TCP-Port 443 |
| Signalisierung, Telemetrie, Registrierung| *.skype.com, *.microsoft.com, *.azure.net, *.azureedge.net, *.office.com, *.trouter.io | TCP-Ports 443 und 80 |

### <a name="network-optimization"></a>Netzwerkoptimierung

Die folgenden Schritte sind optional und für den Rollout von Azure Communication Services nicht unbedingt erforderlich. Verwenden Sie diese Anleitung zum Optimieren Ihres Netzwerks und der Leistung von Azure Communication Services oder bei bestehenden Netzwerkeinschränkungen.
Eine weitere Optimierung kann in den folgenden Fällen ratsam sein:
* Langsame Ausführung von Azure Communication Services (ggf. aufgrund unzureichender Bandbreite)
* Häufige Trennung von Anrufverbindungen (ggf. aufgrund der Firewall oder von Proxyblockaden)
* Störungen und Ausfälle bei Anrufen, oder Stimmen klingen roboterhaft (ggf. aufgrund von Jitter oder Paketverlust)

| Schritt zur Netzwerkoptimierung | Details |
| :-- | :-- |
| Planen Ihres Netzwerks | In dieser Dokumentation finden Sie Informationen zu den Mindestanforderungen, die Netzwerke in Bezug auf Anrufe erfüllen müssen. Sehen Sie sich hierzu das [Teams-Beispiel zur Planung Ihres Netzwerks](https://docs.microsoft.com/microsoftteams/tutorial-network-planner-example) an. |
| Externe Namensauflösung | Stellen Sie sicher, dass auf allen Computern, auf denen die Azure Communications Services SDKs ausgeführt werden, externe DNS-Abfragen aufgelöst werden können, um die von Azure Communication Servicers bereitgestellten Dienste zu ermitteln. Vergewissern Sie sich auch, dass der Zugriff nicht durch Ihre Firewalls verhindert wird. Stellen Sie sicher, dass die Adressen „*.skype.com“, „*.microsoft.com“, „*.azure.net“, „*.azureedge.net“, „*.office.com“ und „*.trouter.io“ von den SDKs aufgelöst werden können.  |
| Aufrechterhalten der Sitzungspersistenz | Stellen Sie sicher, dass die zugeordneten NAT-Adressen (Netzwerkadressenübersetzung) oder die Ports für UDP von Ihrer Firewall nicht geändert werden.
Überprüfen der NAT-Poolgröße | Überprüfen Sie die NAT-Poolgröße (Netzwerkadressenübersetzung), die für die Benutzerkonnektivität benötigt wird. Falls mehrere Benutzer und Geräte auf Azure Communication Services per [Netzwerkadressenübersetzung (NAT) oder Portadressenübersetzung (PAT)](https://docs.microsoft.com/office365/enterprise/nat-support-with-office-365) zugreifen, sollten Sie sicherstellen, dass für die versteckten Geräte hinter den einzelnen öffentlich routingfähigen IP-Adressen nicht die unterstützte Anzahl überschritten wird. Vergewissern Sie sich, dass die richtigen öffentlichen IP-Adressen den NAT-Pools zugewiesen sind, um eine Überlastung der Ports zu verhindern. Eine Überlastung der Ports trägt dazu bei, dass interne Benutzer und Geräte keine Verbindung mit Azure Communication Services herstellen können. |
| Anleitung zur Angriffserkennung und -verhinderung | Falls Sie in Ihrer Umgebung ein System zur [Erkennung](https://docs.microsoft.com/azure/network-watcher/network-watcher-intrusion-detection-open-source-tools) oder Verhinderung von Angriffen (Intrusion Detection/Prevention System, IDS/IPS) bereitgestellt haben, um die Sicherheit für ausgehende Verbindungen zu erhöhen, sollten Sie alle Azure Communication Services-URLs zulassen. |
| Konfigurieren eines VPN mit getrennten Tunneln | Wir empfehlen Ihnen, einen alternativen Pfad für Teams-Datenverkehr anzugeben, mit dem das virtuelle private Netzwerk (VPN) umgangen wird. Dies wird meist als [VPN mit getrennten Tunneln](https://docs.microsoft.com/windows/security/identity-protection/vpn/vpn-routing) bezeichnet. Die Verwendung von getrennten Tunneln bedeutet, dass Datenverkehr für Azure Communications Services nicht über das VPN fließt, sondern direkt an Azure. Das Umgehen Ihres VPN hat eine positive Auswirkung auf die Medienqualität und verringert die Auslastung für die VPN-Geräte und das Netzwerk Ihrer Organisation. Arbeiten Sie mit Ihrem VPN-Anbieter zusammen, um ein VPN mit getrennten Tunneln zu implementieren. Weitere Gründe für die Empfehlung, das VPN zu umgehen: <ul><li> VPNs sind normalerweise nicht für die Unterstützung von Echtzeitmedien konzipiert bzw. konfiguriert.</li><li> Unter Umständen wird auch UDP (für Azure Communication Services erforderlich) von VPNs nicht unterstützt.</li><li>VPNs bewirken zudem eine zusätzliche Verschlüsselung von Mediendatenverkehr, der bereits verschlüsselt ist.</li><li>Die Konnektivität mit Azure Communication Services ist ggf. nicht effizient, weil der Datenverkehr über ein VPN-Gerät geleitet werden muss.</li></ul>|
| Implementieren von QoS (Quality of Service) | Nutzen Sie [Quality of Service (QoS)](https://docs.microsoft.com/microsoftteams/qos-in-teams), um die Priorisierung für Pakete zu konfigurieren. Hierdurch wird die Anrufqualität verbessert, und Sie können die Überwachung und Problembehandlung durchführen. QoS sollte in allen Segmenten eines verwalteten Netzwerks implementiert werden. Auch wenn für ein Netzwerk ausreichende Bandbreite bereitgestellt wurde, kann QoS zu einer Entschärfung von Risiken bei unerwarteten Netzwerkereignissen beitragen. Bei QoS wird der Sprachdatenverkehr priorisiert, damit diese unerwarteten Ereignisse keine negativen Auswirkungen auf die Qualität haben. | 
| Optimieren des WLAN | Ähnlich wie VPNs sind auch WLANs nicht immer auf die Unterstützung von Echtzeitmedien ausgelegt bzw. dafür konfiguriert. Die Planung bzw. Optimierung eines WLAN für die Unterstützung von Azure Communication Services ist ein wichtiger Aspekt, um eine Bereitstellung mit hoher Qualität zu erzielen. Beachten Sie folgende Faktoren: <ul><li>Implementieren Sie QoS oder WiFi Multimedia (WMM), um sicherzustellen, dass Mediendatenverkehr für Ihre WLAN-Verbindungen richtig priorisiert wird.</li><li>Planen und optimieren Sie die WLAN-Bänder und die Platzierung der Zugriffspunkte. Der 2,4-GHz-Bereich kann je nach Platzierung der Zugriffspunkte eine geeignete Lösung sein. Häufig werden Zugriffspunkte aber durch andere Geräte von Verbrauchern beeinflusst, die in demselben Bereich betrieben werden. Der 5-GHz-Bereich ist aufgrund seiner Dichte besser für Echtzeitmedien geeignet, aber es werden mehr Zugriffspunkte benötigt, um eine ausreichende Abdeckung zu erzielen. Dieser Bereich muss auch von den Endpunkten unterstützt werden, und sie müssen so konfiguriert sein, dass diese Bänder entsprechend genutzt werden.</li><li>Bei Verwendung eines Dualband-WLAN sollten Sie erwägen, das „Band Steering“ zu implementieren. Beim Band Steering handelt es sich um ein Verfahren, das von WLAN-Anbietern implementiert wurde, um Dualband-Kunden zur Nutzung des 5-GHz-Bereichs zu bewegen.</li><li>Wenn die Zugriffspunkte desselben Kanals zu nah beieinander liegen, kann es zu einer Signalüberlappung und unerwünschten Konkurrenzsituationen kommen, die für Benutzer zu Leistungseinbußen führen. Stellen Sie sicher, dass für benachbarte Zugriffspunkte Kanäle genutzt werden, die sich nicht überlappen.</li></ul> Jeder WLAN-Anbieter gibt seine eigenen Empfehlungen zur Bereitstellung der WLAN-Lösung heraus. Eine genaue Anleitung erhalten Sie bei Ihrem WLAN-Anbieter.|



### <a name="operating-system-and-browsers-for-javascript-sdks"></a>Betriebssystem und Browser (für JavaScript-SDKs)

Die Azure Communication Services SDKs für Sprache und Video unterstützen bestimmte Betriebssysteme und Browser.
Informieren Sie sich in der [Dokumentation zu den Konzepten für Anrufe](https://docs.microsoft.com/azure/communication-services/concepts/voice-video-calling/calling-sdk-features) über die Betriebssysteme und Browser, die für die Anruf-SDKs unterstützt werden.

## <a name="next-steps"></a>Nächste Schritte

Die folgenden Dokumente könnten Sie auch interessieren:

- [Anrufbibliotheken](https://docs.microsoft.com/azure/communication-services/concepts/voice-video-calling/calling-sdk-features)
- [Client-Server-Architektur](https://docs.microsoft.com/azure/communication-services/concepts/client-and-server-architecture)
- [Topologien für Anrufabläufe](https://docs.microsoft.com/azure/communication-services/concepts/call-flows)
