---
title: 'Problembehandlung: Azure Virtual Network NAT-Konnektivität'
titleSuffix: Azure Virtual Network NAT troubleshooting
description: Es wird beschrieben, wie Sie Virtual Network NAT-Probleme beheben.
services: virtual-network
documentationcenter: na
author: asudbring
manager: KumudD
ms.service: virtual-network
Customer intent: As an IT administrator, I want to troubleshoot Virtual Network NAT.
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/05/2020
ms.author: allensu
ms.openlocfilehash: c629b3425cd095a6ac9d305b5cd6de58ed9d572a
ms.sourcegitcommit: bc792d0525d83f00d2329bea054ac45b2495315d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78674334"
---
# <a name="troubleshoot-azure-virtual-network-nat-connectivity-problems"></a>Problembehandlung: Azure Virtual Network NAT-Konnektivität

In diesem Artikel erhalten Administratoren Hilfe beim Diagnostizieren und Beheben von Konnektivitätsproblemen mit Virtual Network NAT.

>[!NOTE] 
>Virtual Network NAT ist derzeit als öffentliche Vorschauversion (Public Preview) verfügbar. Dieser Dienst ist bisher nur in einer begrenzten Zahl von [Regionen](nat-overview.md#region-availability) erhältlich. Diese Vorschau wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Ergänzende Nutzungsbedingungen für Microsoft Azure-Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms).

## <a name="problems"></a>Probleme

* [SNAT-Auslastung](#snat-exhaustion)
* [Fehler bei ICMP-Ping](#icmp-ping-is-failing)
* [Konnektivitätsfehler](#connectivity-failures)
* [IPv6-Koexistenz](#ipv6-coexistence)

Führen Sie die Schritte im folgenden Abschnitt aus, um diese Probleme zu beheben.

## <a name="resolution"></a>Lösung

### <a name="snat-exhaustion"></a>SNAT-Auslastung

Für eine einzelne [NAT-Gatewayressource](nat-gateway-resource.md) werden jeweils 64.000 bis maximal 1 Million gleichzeitige Datenflüsse unterstützt.  Jede IP-Adresse stellt 64.000 SNAT-Ports für den verfügbaren Bestand bereit. Sie können bis zu 16 IP-Adressen pro NAT-Gatewayressource nutzen.  Eine ausführlichere Beschreibung des SNAT-Mechanismus finden Sie [hier](nat-gateway-resource.md#source-network-address-translation).

Die Grundursache für die SNAT-Auslastung ist häufig ein Antimuster bei der Einrichtung und Verwaltung der ausgehenden Konnektivität.  Lesen Sie diesen Abschnitt sorgfältig.

#### <a name="steps"></a>Schritte

1. Ermitteln Sie, wie von Ihrer Anwendung ausgehende Konnektivität erstellt wird (z. B. Code Review oder Paketerfassung). 
2. Ermitteln Sie, ob es sich bei dieser Aktivität um ein erwartetes Verhalten handelt oder die Anwendung ein Fehlverhalten aufweist.  Verwenden Sie [Metriken](nat-metrics.md) in Azure Monitor, um Ihre Erkenntnisse zu untermauern. Verwenden Sie die Kategorie „Fehler“ für die Metrik „SNAT-Verbindungen“.
3. Prüfen Sie, ob die richtigen Muster eingehalten werden.
4. Prüfen Sie, ob die hohe SNAT-Portauslastung verringert werden sollte, indem der NAT-Gatewayressource weitere IP-Adressen zugewiesen werden.

#### <a name="design-patterns"></a>Entwurfsmuster

Nutzen Sie nach Möglichkeit immer die Wiederverwendung von Verbindungen und Verbindungspools.  Bei diesen Mustern werden Probleme aufgrund einer hohen Ressourcenauslastung ganz vermieden, und es wird ein planbares, zuverlässiges und skalierbares Verhalten erreicht. Primitive für diese Muster finden Sie in vielen Entwicklungsbibliotheken und Frameworks.

_**Lösung:**_ Verwenden geeigneter Muster

- Erwägen Sie die Nutzung von [asynchronen Abrufmustern](https://docs.microsoft.com/azure/architecture/patterns/async-request-reply) für zeitintensive Vorgänge, um Verbindungsressourcen für andere Vorgänge freizuhalten.
- Für langlebige Datenflüsse (z. B. wiederverwendete TCP-Verbindungen) sollten TCP-Keepalives oder Anwendungsschicht-Keepalives verwendet werden, um zu verhindern, dass es für zwischengeschaltete Systeme zu Zeitüberschreitungen kommt.
- Es sollten ordnungsgemäße [Wiederholungsmuster](https://docs.microsoft.com/azure/architecture/patterns/retry) verwendet werden, um bei vorübergehenden Fehlern oder der Wiederherstellung nach Fehlern aggressive Wiederholungsversuche oder Bursts zu vermeiden.
Die Erstellung einer neuen TCP-Verbindung für jeden HTTP-Vorgang (auch als „atomische Verbindungen“ bezeichnet) ist ein Antimuster.  Mit atomischen Verbindungen wird verhindert, dass für Ihre Anwendung eine gute Skalierung erzielt wird, und es werden Ressourcen verschwendet.  Fassen Sie immer mehrere Vorgänge per Pipeline in derselben Verbindung zusammen.  Ihre Anwendung profitiert von der Transaktionsgeschwindigkeit und den Ressourcenkosten.  Wenn Ihre Anwendung die Verschlüsselung auf der Transportschicht nutzt (z. B. TLS), ist die Verarbeitung neuer Verbindungen mit erheblichen Kosten verbunden.  Informationen zu weiteren bewährten Mustern finden Sie unter [Cloudentwurfsmuster](https://docs.microsoft.com/azure/architecture/patterns/).

#### <a name="possible-mitigations"></a>Mögliche Gegenmaßnahmen

_**Lösung:**_ Skalieren Sie die ausgehende Konnektivität wie folgt:

| Szenario | Beweise |Minderung |
|---|---|---|
| In Zeiten mit hoher Auslastung kommt es bei SNAT-Ports zu Konflikten und zu einer Überlastung. | In der Kategorie „Fehler“ für die [Metrik](nat-metrics.md) „SNAT-Verbindungen“ in Azure Monitor werden vorübergehende oder dauerhafte Fehler im Zeitverlauf sowie hohes Verbindungsaufkommen angezeigt.  | Ermitteln Sie, ob Sie zusätzliche öffentliche Ressourcen bzw. Präfixressourcen für öffentliche IP-Adressen hinzufügen können. Durch diese Hinzufügung können für Ihr NAT-Gateway maximal 16 IP-Adressen genutzt werden. Aufgrund dieser Maßnahme ist ein größerer Bestand für die verfügbaren SNAT-Ports vorhanden (64.000 pro IP-Adresse), und Sie können Ihr Szenario weiter skalieren.|
| Sie haben bereits 16 IP-Adressen bereitgestellt, und es kommt trotzdem weiterhin zu einer Überlastung bei den SNAT-Ports. | Beim Hinzuzufügen weiterer IP-Adressen tritt ein Fehler auf. Die Gesamtanzahl der IP-Adressen von öffentlichen IP-Adressressourcen oder öffentlichen IP-Präfixressourcen ist insgesamt höher als 16. | Verteilen Sie Ihre Anwendungsumgebung auf mehrere Subnetze, und stellen Sie für jedes Subnetz eine NAT-Gatewayressource bereit.  Optimieren Sie Ihre Entwurfsmuster anhand der obigen [Informationen](#design-patterns). |

>[!NOTE]
>Es ist wichtig, dass Sie verstehen, warum es zur SNAT-Überlastung kommt. Stellen Sie sicher, dass Sie die richtigen Muster verwenden, um skalierbare und zuverlässige Szenarien zu erhalten.  Das Hinzufügen von weiteren SNAT-Ports, ohne dass Ihnen die Ursache des Bedarfs bekannt ist, sollte nur im Notfall erfolgen. Wenn Sie nicht verstehen, warum es bei Ihnen zu einer hohen Auslastung beim SNAT-Portbestand kommt, führt die Erweiterung auf eine größere Zahl von SNAT-Ports durch das Hinzufügen von mehr IP-Adressen nur dazu, dass sich dieser Überlastungsfehler bei der weiteren Skalierung Ihrer Anwendung fortsetzt.  Es kann sein, dass hierdurch andere Ineffizienzen und Antimuster verdeckt werden.

### <a name="icmp-ping-is-failing"></a>Fehler bei ICMP-Ping

[Virtual Network NAT](nat-overview.md) unterstützt IPv4-UDP- und TCP-Protokolle. ICMP wird nicht unterstützt, sodass hierfür das Auftreten von Fehlern zu erwarten ist.  

_**Lösung:**_ Verwenden Sie stattdessen TCP-Verbindungstests (z. B. „TCP-Ping“) und UDP-spezifische Anwendungsschichttests, um die End-to-End-Konnektivität zu überprüfen.

Die folgende Tabelle kann als Ausgangspunkt dafür dienen, welche Tools für den Einstieg in die Tests verwendet werden können.

| Betriebssystem | Allgemeiner TCP-Verbindungstest | TCP-Anwendungsschichttest | UDP |
|---|---|---|---|
| Linux | nc (allgemeiner Verbindungstest) | curl (TCP-Anwendungsschichttest) | Anwendungsspezifisch |
| Windows | [PsPing](https://docs.microsoft.com/sysinternals/downloads/psping) | PowerShell [Invoke-WebRequest](https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/invoke-webrequest) | Anwendungsspezifisch |

### <a name="connectivity-failures"></a>Konnektivitätsfehler

Konnektivitätsprobleme mit [Virtual Network NAT](nat-overview.md) können verschiedene Ursachen haben:

* Vorübergehende oder dauerhafte [SNAT-Auslastung](#snat-exhaustion) des NAT-Gateways
* Vorübergehende Fehler in der Azure-Infrastruktur 
* Vorübergehende Fehler im Pfad zwischen Azure und dem öffentlichen Internetziel 
* Vorübergehende oder dauerhafte Fehler am öffentlichen Internetziel

Verwenden Sie Tools wie die folgenden, um die Konnektivität zu überprüfen. [ICMP-Ping wird nicht unterstützt.](#icmp-ping-is-failing)

| Betriebssystem | Allgemeiner TCP-Verbindungstest | TCP-Anwendungsschichttest | UDP |
|---|---|---|---|
| Linux | nc (allgemeiner Verbindungstest) | curl (TCP-Anwendungsschichttest) | Anwendungsspezifisch |
| Windows | [PsPing](https://docs.microsoft.com/sysinternals/downloads/psping) | PowerShell [Invoke-WebRequest](https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/invoke-webrequest) | Anwendungsspezifisch |

#### <a name="snat-exhaustion"></a>SNAT-Auslastung

Lesen Sie den Abschnitt [SNAT-Auslastung](#snat-exhaustion) in diesem Artikel.

#### <a name="azure-infrastructure"></a>Azure-Infrastruktur

Die Azure-Infrastruktur wird zwar äußerst gewissenhaft überwacht und betrieben, vorübergehende Fehler können jedoch trotzdem auftreten, da es keine Garantie für verlustfreie Übertragungen gibt.  Verwenden Sie Entwurfsmuster, die SYN-Neuübertragungen für TCP-Anwendungen ermöglichen. Verwenden Sie ausreichen große Verbindungstimeouts, um TCP-SYN-Neuübertragungen zuzulassen und die vorübergehenden Auswirkungen von SYN-Paketverlusten zu verringern.

_**Lösung:**_

* Überprüfen Sie, ob eine [SNAT-Auslastung](#snat-exhaustion) vorliegt.
* Der Konfigurationsparameter in einem TCP-Stapel, der das Verhalten von SYN-Neuübertragungen steuert, wird [RTO](https://tools.ietf.org/html/rfc793) (Retransmission Time-Out, Timeout für Neuübertragungen) genannt. Der RTO-Wert ist anpassbar, aber in der Regel standardmäßig auf mindestens eine Sekunde mit exponentiellem Backoff festgelegt.  Sollte das Verbindungstimeout Ihrer Anwendung zu kurz sein (beispielsweise eine Sekunde), treten unter Umständen sporadische Verbindungstimeouts auf.  Erhöhen Sie das Verbindungstimeout der Anwendung.
* Erstellen Sie im Falle von längeren unerwarteten Timeouts mit Standardanwendungsverhalten eine Supportanfrage für die weitere Problembehandlung.

Es wird davon abgeraten, das TCP-Verbindungstimeout künstlich zu verkürzen oder den RTO-Parameter zu optimieren.

#### <a name="public-internet-transit"></a>Öffentliche Internetübertragung

Die Wahrscheinlichkeit vorübergehender Fehler ist direkt proportional zur Länge des Zielpfads und zur Anzahl von Zwischensystemen. Es ist mit einer Zunahme vorübergehender Fehler über die [Azure-Infrastruktur](#azure-infrastructure) zu rechnen. 

Befolgen Sie die gleichen Anweisungen wie im vorherigen [Abschnitt zur Azure-Infrastruktur](#azure-infrastructure).

#### <a name="internet-endpoint"></a>Internetendpunkt

Die obigen Abschnitte gelten zusätzlich zu den Überlegungen im Zusammenhang mit dem Internetendpunkt, der für Ihre Kommunikation verwendet wird. Weitere Faktoren, die sich auf den Erfolg der Konnektivität auswirken können:

* Zielseitige Datenverkehrsverwaltung, einschließlich Folgendem:
- Zielseitige API-Ratenbegrenzung
- Gegenmaßnahmen für volumetrische DDoS-Angriffe oder Traffic-Shaping auf der Transportebene
* Firewall oder andere Komponenten am Ziel 

In der Regel sind Paketerfassungen an der Quelle und am Ziel (sofern verfügbar) erforderlich, um Abläufe zu bestimmen.

_**Lösung:**_

* Überprüfen Sie, ob eine [SNAT-Auslastung](#snat-exhaustion) vorliegt. 
* Überprüfen Sie zum Vergleich die Konnektivität mit einem Endpunkt in der gleichen Region oder an einem anderen Ort.  
* Untersuchen Sie im Falle von Tests für hohes Volumen oder hohe Transaktionsraten, ob weniger Fehler auftreten, wenn Sie die Rate verringern.
* Wenn sich die Ratenänderung auf die Fehlerrate auswirkt, überprüfen Sie, ob API-Ratenbegrenzungen oder andere zielseitige Einschränkungen erreicht wurden.
* Sollte das Ergebnis Ihrer Untersuchung nicht eindeutig sein, erstellen Sie eine Supportanfrage für die weitere Problembehandlung.

#### <a name="tcp-resets-received"></a>TCP-Zurücksetzungen empfangen

Sollten bei dem virtuellen Quellcomputer TCP-Zurücksetzungen (TCP RST-Pakete) eingehen, stammen diese unter Umständen vom NAT-Gateway auf der privaten Seite für Flows, die nicht als aktiv erkannt werden.  Dies kann unter anderen darauf zurückzuführen sein, dass für die TCP-Verbindung ein Leerlauftimeout aufgetreten ist.  Das Leerlauftimeout kann auf einen Wert zwischen vier und 120 Minuten festgelegt werden.

TCP-Zurücksetzungen werden nicht auf der öffentlichen Seite von NAT-Gatewayressourcen generiert. Wenn Sie TCP-Zurücksetzungen aufseiten des Ziels erhalten, stammen diese vom Stapel des virtuellen Quellcomputers und nicht von der NAT-Gatewayressource.

_**Lösung:**_

* Machen Sie sich mit den Empfehlungen für [Entwurfsmuster](#design-patterns) vertraut.  
* Erstellen Sie ggf. eine Supportanfrage für die weitere Problembehandlung.

### <a name="ipv6-coexistence"></a>IPv6-Koexistenz

[Virtual Network NAT](nat-overview.md) unterstützt IPv4-UDP- und TCP-Protokolle. Die Bereitstellung in einem [Subnetz mit IPv6-Präfix wird nicht unterstützt](nat-overview.md#limitations).

_**Lösung:**_ Stellen Sie das NAT-Gateway in einem Subnetz ohne IPv6-Präfix bereit.

Sie können Ihr Interesse an zusätzlichen Funktionen über [UserVoice für Virtual Network NAT](https://aka.ms/natuservoice) kommunizieren.

## <a name="next-steps"></a>Nächste Schritte

* Informieren Sie sich über [Virtual Network NAT](nat-overview.md).
* Informieren Sie sich über die [NAT-Gatewayressource](nat-gateway-resource.md).
* Informieren Sie sich über [Metriken und Warnungen für NAT-Gatewayressourcen](nat-metrics.md).
* [Teilen Sie uns bei UserVoice mit, welche Funktionen wir als Nächstes für Virtual Network NAT entwickeln sollen.](https://aka.ms/natuservoice)

