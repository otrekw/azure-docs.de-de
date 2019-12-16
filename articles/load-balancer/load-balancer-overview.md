---
title: Was versteht man unter Azure Load Balancer?
titleSuffix: Azure Load Balancer
description: Übersicht über Features, Architektur und Implementierung des Azure Load Balancers. Hier erfahren Sie, wie Load Balancer funktioniert und wie Sie den Dienst in der Cloud verwenden.
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
Customer intent: As an IT administrator, I want to learn more about the Azure Load Balancer service and what I can use it for.
ms.devlang: na
ms.topic: overview
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/05/2019
ms.author: allensu
ms.openlocfilehash: 50cb61394043bb8d0e67cae2aea8be4285f3432c
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/08/2019
ms.locfileid: "74926264"
---
# <a name="what-is-azure-load-balancer"></a>Was versteht man unter Azure Load Balancer?

*Lastenausgleich* bezieht sich auf die effiziente Verteilung von Last oder eingehendem Netzwerkdatenverkehr auf eine Gruppe von Back-End-Ressourcen oder Server. Azure bietet [verschiedene Lastenausgleichsoptionen](https://docs.microsoft.com/azure/architecture/guide/technology-choices/load-balancing-overview) für unterschiedliche Anforderungen. In diesem Dokument wird Azure Load Balancer behandelt.

Azure Load Balancer setzt in der vierten Schicht des OSI-Modells (Open Systems Interconnection) an. Der Dienst ist der zentrale Kontaktpunkt für Clients. Load Balancer verteilt neue eingehende Datenflüsse, die das Front-End von Load Balancer erreichen, gemäß den angegebenen Lastenausgleichsregeln und Integritätstests auf Back-End-Poolinstanzen. Bei den Back-End-Poolinstanzen kann es sich um virtuelle Azure-Computer oder um Instanzen in einer VM-Skalierungsgruppe handeln. 

Mit Azure Load Balancer können Sie Ihre Anwendungen skalieren und hochverfügbare Dienste bereitstellen. Load Balancer unterstützt Szenarien mit eingehenden und ausgehenden Verbindungen, bietet eine geringe Wartezeit und einen hohen Durchsatz und kann Millionen von Datenflüssen für alle TCP- und UDP-Anwendungen zentral hochskalieren.

Eine **[öffentliche Load Balancer-Instanz](#publicloadbalancer)** kann ausgehende Verbindungen für virtuelle Computer in Ihrem virtuellen Netzwerk bereitstellen, indem sie deren private IP-Adressen in öffentliche IP-Adressen übersetzt. Öffentliche Load Balancer-Instanzen werden verwendet, um einen Lastenausgleich für den eingehenden Internetdatenverkehr Ihrer virtuellen Computer vorzunehmen.

Eine **[interne (oder private) Load Balancer-Instanz](#internalloadbalancer)** kann für Szenarien verwendet werden, in denen am Front-End nur private IP-Adressen benötigt werden. Interne Load Balancer-Instanzen werden verwendet, um einen Lastenausgleich für Datenverkehr innerhalb eines virtuellen Netzwerks vorzunehmen. Sie können ein Load Balancer-Front-End auch aus einem lokalen Netzwerk in einem Hybridszenario erreichen.

## <a name="load-balancer-components"></a>Load Balancer-Komponenten
* **Front-End-IP-Konfigurationen**: Die IP-Adresse der Load Balancer-Instanz. Sie ist der Kontaktpunkt für Clients. Die IP-Adressen können öffentlich oder privat sein. Je nach gewählter Option wird entweder eine öffentliche oder eine interne Load Balancer-Instanz erstellt.
* **Back-End-Pool**: Die Gruppe virtueller Computer oder Instanzen in der VM-Skalierungsgruppe, von denen die eingehende Anforderung verarbeitet wird. Für eine kosteneffiziente Skalierung zur Bewältigung großer Mengen an eingehendem Datenverkehr empfiehlt es sich in der Regel, dem Back-End-Pool weitere Instanzen hinzuzufügen. Die Konfiguration von Load Balancer wird automatisch angepasst, wenn Sie Instanzen hoch- oder herunterskalieren. Das Hinzufügen oder Entfernen von virtuellen Computern zum bzw. aus dem Back-End-Pool bewirkt, dass Load Balancer neu konfiguriert wird. Es werden aber keine zusätzlichen Vorgänge für die Load Balancer-Ressource ausgeführt.
* **Integritätstests**: Mithilfe eines Integritätstest wird die Integrität der Instanzen im Back-End-Pool ermittelt. Sie können den gewünschten Fehlerschwellenwert für Ihre Integritätstests definieren. Wenn ein Test nicht reagiert, beendet Load Balancer das Senden neuer Verbindungen an die fehlerhaften Instanzen. Ein Testfehler wirkt sich nicht auf vorhandene Verbindungen aus. Die Verbindung ist weiterhin verfügbar, bis der Flow von der Anwendung beendet wird, eine Leerlaufzeitüberschreitung auftritt oder die VM heruntergefahren wird. Load Balancer verfügt über verschiedene Integritätstesttypen für TCP-, HTTP- und HTTPS-Endpunkte. Weitere Informationen finden Sie unter [Testtypen](load-balancer-custom-probe-overview.md#types).
* **Lastenausgleichsregeln:** Mithilfe von Lastenausgleichsregeln wird gesteuert, welche Aktionen die Load Balancer-Instanz wann ausführen soll. 
* **NAT-Eingangsregeln:** Mit einer NAT-Regel für eingehenden Datenverkehr wird Datenverkehr von einem bestimmten Port einer Front-End-IP-Adresse an einen bestimmten Port einer Back-End-Instanz im virtuellen Netzwerk weitergeleitet. Der Portweiterleitung liegt die gleiche hashbasierte Verteilung zugrunde wie dem Lastenausgleich. Allgemeine Szenarien für diese Funktion sind RDP- (Remotedesktopprotokoll) oder SSH-Sitzungen (Secure Shell) für einzelne VM-Instanzen in einem Azure Virtual Network. Sie können Ports mehrere interne Endpunkte unter derselben Front-End-IP-Adresse zuordnen. Sie können die Front-End-IP-Adressen verwenden, um für Ihre VMs die Remoteverwaltung ohne zusätzliche Jumpbox durchzuführen.
* **Ausgangsregeln**: Eine Ausgangsregel konfiguriert die Netzwerkadressenübersetzung (Network Address Translation, NAT) für ausgehenden Datenverkehr für alle virtuellen Computer oder Instanzen, die vom Back-End-Pool für die Front-End-Übersetzung identifiziert wurden.

## <a name="load-balancer-concepts"></a>Load Balancer-Konzepte

Load Balancer stellt die folgenden grundlegenden Funktionen für TCP und UDP-Anwendungen bereit:

* **Lastenausgleichsalgorithmus**: Mit Azure Load Balancer können Sie eine Lastenausgleichsregel erstellen, nach der Datenverkehr, der beim Front-End eintrifft, auf Back-End-Poolinstanzen verteilt wird. Load Balancer verwendet einen Hashalgorithmus für die Verteilung von eingehenden Datenflüssen und schreibt die Header von Datenflüssen an Back-End-Poolinstanzen um. Wenn der Integritätstest einen fehlerfreien Back-End-Endpunkt angibt, ist ein Server verfügbar, der neue Flows empfangen kann.
Standardmäßig wird für Load Balancer ein 5-Tupel-Hash verwendet. Der Hash umfasst die Elemente IP-Quelladresse, Quellport, IP-Zieladresse, Zielport und IP-Protokollnummer, um Flows den verfügbaren Servern zuzuordnen. Sie können eine Affinität zu einer IP-Quelladresse erstellen, indem Sie für eine bestimmte Regel einen 2- oder 3-Tupel-Hash verwenden. Alle Pakete desselben Paketflows treffen bei derselben Instanz hinter dem Front-End mit Lastenausgleich ein. Wenn der Client einen neuen Flow von derselben IP-Quelladresse initiiert, wird der Quellport geändert. Dies kann dazu führen, dass der Datenverkehr aufgrund des 5-Tupel-Hashs an einen anderen Back-End-Endpunkt gesendet wird.
Weitere Informationen finden Sie unter [Konfigurieren des Verteilungsmodus für Azure Load Balancer](load-balancer-distribution-mode.md). In der folgenden Abbildung wird die hashbasierte Verteilung angezeigt:

  ![Hash-basierte Verteilung](./media/load-balancer-overview/load-balancer-distribution.png)

  *Abbildung: Hashbasierte Verteilung*

* **Anwendungsunabhängigkeit und -transparenz**: Load Balancer interagiert nicht direkt mit TCP oder UDP oder der Anwendungsschicht. Alle TCP- oder UDP-Anwendungsszenarien können unterstützt werden. Von Load Balancer werden keine Flows beendet oder angestoßen, es erfolgt keine Interaktion mit der Nutzlast des Flows, und es wird auch keine Gatewayfunktion auf der Anwendungsschicht bereitgestellt. Protokollhandshakes werden immer direkt zwischen dem Client und der Back-End-Poolinstanz durchgeführt. Bei einer Antwort auf einen eingehenden Flow handelt es sich immer um die Antwort eines virtuellen Computers. Wenn der Flow auf dem virtuellen Computer eingeht, wird auch die IP-Adresse der ursprünglichen Quelle gespeichert.
  * Jeder Endpunkt erhält nur über eine VM eine Antwort. Zum Beispiel wird ein TCP-Handshake immer zwischen dem Client und der ausgewählten Back-End-VM ausgeführt. Eine Antwort auf eine Anforderung, die an ein Front-End gesendet wird, wird von einer Back-End-VM generiert. Wenn Sie eine erfolgreiche Überprüfung der Konnektivität für ein Front-End durchführen, bedeutet dies, dass Sie eine End-to-End-Konnektivität für mindestens eine Back-End-VM überprüfen.
  * Anwendungsnutzlasten sind für Load Balancer transparent. Alle UDP- oder TCP-Anwendungen können unterstützt werden.
  * Da der Load Balancer nicht mit der TCP-Nutzlast interagiert und keine TLS-Abladung bereitstellt, können Sie verschlüsselte End-to-End-Szenarien erstellen. Die Verwendung des Load Balancers ermöglicht ein hohes Maß an horizontaler Skalierung für TLS-Anwendungen, indem die TLS-Verbindung auf dem virtuellen Computer selbst beendet wird. Beispielsweise ist die TLS-Funktion zum erstellen von Sitzungsschlüsseln vom Typ und der Nummer der VMs beschränkt, die Sie zum Back-End-Pool hinzufügen.

* **Ausgehende Verbindungen (SNAT)** : Alle ausgehenden Flows von privaten IP-Adressen in Ihrem virtuellen Netzwerk zu öffentlichen IP-Adressen im Internet können in eine Front-End-IP-Adresse des Load Balancers übersetzt werden. Wenn ein öffentliches Front-End per Lastenausgleichsregel an einen virtuellen Back-End-Computer gebunden ist, übersetzt Azure ausgehende Verbindungen in die IP-Adresse des öffentlichen Front-Ends. Diese Konfiguration hat die folgenden Vorteile:
  * Einfache Upgrades und eine Notfallwiederherstellung von Diensten, da das Front-End dynamisch einer anderen Instanz des Diensts zugeordnet werden kann.
  * Vereinfachte Verwaltung von Zugriffssteuerungslisten. Zugriffssteuerungslisten, die als Front-End-IP-Adressen ausgedrückt werden, ändern sich nicht, wenn Dienste zentral hoch- oder herunterskaliert oder erneut bereitgestellt werden. Die Übersetzung von ausgehenden Verbindungen in eine Anzahl von IP-Adressen, die geringer als die Anzahl von Computern ist, verringert den Aufwand für die Implementierung sicherer Empfängerlisten.
Weitere Informationen finden Sie unter [Ausgehende Verbindungen in Azure](load-balancer-outbound-connections.md).
Load Balancer Standard verfügt über zusätzliche SKU-spezifische Funktionen, die über diese Grundlagen hinausgehen. Dies ist unten beschrieben.

## <a name="skus"></a> Vergleich der Load Balancer-SKUs

Load Balancer unterstützt sowohl Basic- als auch Standard-SKUs. Diese SKUs unterscheiden sich in Bezug auf Skalierung, Features und Preise. Jedes Szenario, das mit dem Load Balancer im Tarif „Basic“ möglich ist, kann auch mit Load Balancer Standard erstellt werden. Die APIs für beide SKUs sind ähnlich, und sie werden über die Spezifikation einer SKU aufgerufen. Die API zur Unterstützung von SKUs für Load Balancer und öffentliche IP-Adressen ist ab der API-Version `2017-08-01` verfügbar. Beide SKUs verfügen über die gleiche allgemeine API und Struktur.

Die Konfiguration des gesamten Szenarios kann sich je nach SKU leicht unterscheiden. In der Load Balancer-Dokumentation wird darauf hingewiesen, wenn ein Artikel nur für eine bestimmte SKU gilt. Weitere Informationen zu den Unterschieden finden Sie in der folgenden Tabelle. Weitere Informationen finden Sie unter [Übersicht: Azure Load Balancer Standard](load-balancer-standard-overview.md).

>[!NOTE]
> Microsoft empfiehlt Load Balancer Standard.
Eigenständige virtuelle Computer, Verfügbarkeitsgruppen und VM-Skalierungsgruppen können nur mit einer SKU, nie mit beiden verbunden werden. Die Load Balancer-SKU muss mit der SKU für öffentliche IP-Adressen übereinstimmen, wenn Sie sie mit öffentlichen IP-Adressen verwenden. Load Balancer-SKUs und SKUs für öffentliche IP-Adressen sind nicht änderbar.

[!INCLUDE [comparison table](../../includes/load-balancer-comparison-table.md)]

Weitere Informationen finden Sie unter [Load Balancer-Grenzwerte](https://aka.ms/lblimits). Lesen Sie für den Standard-Load Balancer auch die ausführlicheren Informationen unter [Übersicht](load-balancer-standard-overview.md), [Preise](https://aka.ms/lbpricing) und [SLA](https://aka.ms/lbsla).

## <a name="load-balancer-types"></a>Load Balancer-Typen

### <a name = "publicloadbalancer"></a>Öffentlicher Load Balancer

Bei einem öffentlichen Load Balancer werden die öffentliche IP-Adresse und der Port des eingehenden Datenverkehrs der privaten IP-Adresse und dem Port der VM zugeordnet. Für den Antwortdatenverkehr von der VM führt der Load Balancer eine Zuordnung in umgekehrter Richtung durch. Sie können bestimmte Typen von Datenverkehr auf verschiedene VMs oder Dienste verteilen, indem Sie Lastenausgleichsregeln anwenden. Sie können zum Beispiel die Netzwerklast von Webanforderungen auf mehrere Webserver verteilen.

>[!NOTE]
>Sie können pro Verfügbarkeitsgruppe nur einen öffentlichen Load Balancer und einen internen Load Balancer implementieren.

Die folgende Abbildung zeigt einen Endpunkt für Webdatenverkehr mit Lastenausgleich, der von drei virtuellen Computern für den öffentlichen TCP-Port 80 genutzt wird. Diese drei virtuellen Computer bilden eine Gruppe mit Lastenausgleich.

![Beispiel für einen öffentlichen Load Balancer](./media/load-balancer-overview/IC727496.png)

*Abbildung: Durchführen des Lastenausgleichs für Webdatenverkehr mit einem öffentlichen Load Balancer*

Internetclients senden Webseitenanforderungen an die öffentliche IP-Adresse einer Web-App über TCP-Port 80. Azure Load Balancer verteilt die Anforderungen auf die drei VMs in der Gruppe mit Lastenausgleich. Weitere Informationen zu Load Balancer-Algorithmen finden Sie unter [Load Balancer-Konzepte](load-balancer-overview.md##load-balancer-concepts).

Standardmäßig verteilt Azure Load Balancer Netzwerkdatenverkehr gleichmäßig auf mehrere Instanzen virtueller Computer. Sie können auch Sitzungsaffinität konfigurieren. Weitere Informationen finden Sie unter [Konfigurieren des Verteilungsmodus für Azure Load Balancer](load-balancer-distribution-mode.md).

### <a name = "internalloadbalancer"></a> Interner Load Balancer

Ein interner Load Balancer leitet Datenverkehr – im Gegensatz zu einem öffentlichen Load Balancer – nur an Ressourcen weiter, die sich innerhalb eines virtuellen Netzwerks befinden oder die für den Zugriff auf die Azure-Infrastruktur ein VPN verwenden. Die Azure-Infrastruktur schränkt den Zugriff auf die Front-End-IP-Adressen eines virtuellen Netzwerks ein, für die ein Lastenausgleich durchgeführt wird. Front-End-IP-Adressen und virtuelle Netzwerke werden nie direkt für einen Internetendpunkt verfügbar gemacht. Interne Branchenanwendungen werden in Azure ausgeführt. Auf sie wird aus Azure oder von lokalen Ressourcen aus zugegriffen.

Ein interner Load Balancer ermöglicht die folgenden Arten von Lastenausgleich:

* **In einem virtuellen Netzwerk:** Lastenausgleich zwischen virtuellen Computern im virtuellen Netzwerk und einer Gruppe von virtuellen Computern, die sich in demselben virtuellen Netzwerk befinden.
* **Für ein standortübergreifendes virtuelles Netzwerk:** Lastenausgleich zwischen lokalen Computern und einer Gruppe von virtuellen Computern, die sich in demselben virtuellen Netzwerk befinden.
* **Für Anwendungen mit mehreren Ebenen:** Lastenausgleich für Anwendungen mit mehreren Ebenen und Internetzugriff, wobei für die Back-End-Ebenen kein Internetzugriff besteht. Die Back-End-Ebenen erfordern einen Lastenausgleich des Datenverkehrs aus einer mit dem Internet verbundenen Ebene. Dies ist in der nächsten Abbildung dargestellt.
* **Für Branchenanwendungen:** Lastenausgleich für Branchenanwendungen, die in Azure ohne zusätzliche Hardware oder Software für den Lastenausgleich gehostet werden. Dieses Szenario enthält lokale Server, die sich in der Gruppe der Computer befinden, für deren Datenverkehr ein Lastenausgleich durchgeführt wird.

![Beispiel für einen internen Load Balancer](./media/load-balancer-overview/IC744147.png)

*Abbildung: Durchführen eines Lastenausgleichs für Anwendungen mit mehreren Ebenen mithilfe eines öffentlichen und eines internen Load Balancers*

## <a name="pricing"></a>Preise

Die Verwendung von Load Balancer Standard ist kostenpflichtig.

* Anzahl von konfigurierten Lastenausgleichsregeln und Regeln für ausgehenden Datenverkehr. NAT-Regeln für eingehenden Datenverkehr werden nicht in die Gesamtzahl von Regeln eingerechnet.
* Menge der verarbeiteten eingehenden und ausgehenden Daten, unabhängig von den Regeln.

Informationen zu den Preisen für Load Balancer Standard finden Sie unter [Load Balancer – Preise](https://azure.microsoft.com/pricing/details/load-balancer/).

Basic Load Balancer wird kostenlos angeboten.

## <a name="sla"></a>SLA

Informationen zur Vereinbarung zum Servicelevel (SLA) für Load Balancer Standard finden Sie unter [SLA für Load Balancer](https://aka.ms/lbsla).

## <a name="limitations"></a>Einschränkungen

* Der Load Balancer ermöglicht den Lastenausgleich und die Portweiterleitung für bestimmte TCP- oder UDP-Protokolle. Für Lastenausgleichsregeln und NAT-Regeln für eingehenden Datenverkehr werden TCP und UDP unterstützt, aber keine anderen IP-Protokolle, z. B. ICMP.

  Der Load Balancer beendet die Nutzlast des UDP- oder TCP-Flows nicht, antwortet nicht darauf und interagiert auch nicht auf andere Weise damit. Es handelt sich nicht um einen Proxy. Die erfolgreiche Überprüfung der Konnektivität mit einem Front-End muss per In-band-Zugriff mit dem gleichen Protokoll erfolgen, das auch in der Lastenausgleichsregel bzw. der NAT-Regel für eingehenden Datenverkehr verwendet wird. Mindestens einer Ihrer virtuellen Computer muss eine Antwort für einen Client generieren, um eine Antwort vom Front-End zu erhalten.

  Wenn Sie keine In-band-Antwort vom Load Balancer-Front-End erhalten, deutet dies darauf hin, dass keine virtuellen Computer antworten konnten. Die Interaktion mit einem Load Balancer-Front-End ist nicht möglich, wenn ein virtueller Computer nicht antworten kann. Dies gilt auch für ausgehende Verbindungen, bei denen die Portmaskierung mit SNAT nur für TCP und UDP unterstützt wird. Für alle anderen IP-Protokolle, z. B. ICMP, tritt ein Fehler auf. Weisen Sie eine öffentliche IP-Adresse auf Instanzebene zu, um dieses Problem zu umgehen. Weitere Informationen finden Sie unter [Grundlagen von SNAT und PAT](load-balancer-outbound-connections.md#snat).

* Interne Load Balancer führen für ausgehende Verbindungen keine Übersetzung für das Front-End eines internen Load Balancers durch, da sich beide im privaten IP-Adressraum befinden. Über öffentliche Load Balancer werden [ausgehende Verbindungen](load-balancer-outbound-connections.md) von privaten IP-Adressen im virtuellen Netzwerk mit öffentlichen IP-Adressen bereitgestellt. Bei internen Load Balancern wird mit diesem Ansatz die potenzielle SNAT-Portüberlastung in einem eindeutigen internen IP-Adressraum vermieden, für den die Übersetzung nicht erforderlich ist.

  Hierbei ergibt sich die folgende Nebenwirkung: Wenn ein von einem virtuellen Computer ausgehender Flow in den Back-End-Pool versucht, einen Flow zum Front-End der internen Load Balancer-Instanz im Pool auszuführen _und_ sich selbst zugewiesen ist, stimmen die beiden Verzweigungen des Flows nicht überein. Aufgrund dieser fehlenden Übereinstimmung tritt für den Flow ein Fehler auf. Der Flow ist erfolgreich, wenn er nicht dem virtuellen Computer im Back-End-Pool zugeordnet ist, die den Flow zum Front-End erstellt hat.

  Wenn der Flow sich selbst zugeordnet ist, geht der ausgehende Flow zum Front-End scheinbar vom virtuellen Computer und der zugehörige eingehende Flow scheinbar vom virtuellen Computer an sich selbst aus. Aus Sicht des Gastbetriebssystems stimmen die eingehenden und ausgehenden Bestandteile desselben Flows innerhalb des virtuellen Computers nicht überein. Der TCP-Stack erkennt nicht, dass es sich bei diesen Hälften um Teile desselben Flows handelt. Die Quelle und das Ziel stimmen nicht überein. Wenn der Flow keinem anderen virtuellen Computer im Back-End-Pool zugeordnet ist, stimmen die Teile des Flows überein, und der virtuelle Computer kann auf den Flow antworten.

  Das Symptom für dieses Szenario sind zeitweilige Verbindungstimeouts, die auftreten, wenn der Flow zu demselben Back-End zurückkehrt, vom dem er ursprünglich stammt. Zu den häufig genutzten Problemumgehungen gehören das Einfügen einer Proxyebene hinter dem internen Load Balancer und das Verwenden von DSR-Stilregeln (Direct Server Return). Weitere Informationen finden Sie unter [Mehrere Front-Ends für Azure Load Balancer](load-balancer-multivip-overview.md).

  Sie können eine interne Load Balancer-Instanz mit dem Proxy eines Drittanbieters kombinieren oder die interne [Application Gateway](../application-gateway/application-gateway-introduction.md)-Instanz für Proxyszenarien mit HTTP/HTTPS verwenden. Es ist zwar möglich, zum Beheben dieses Problems einen öffentlichen Load Balancer zu nutzen, aber das sich ergebende Szenario ist anfällig für [SNAT-Auslastung](load-balancer-outbound-connections.md#snat). Nutzen Sie diesen zweiten Ansatz nur, wenn eine sorgfältige Verwaltung durchgeführt werden kann.

* Im Allgemeinen wird das Weiterleiten von IP-Fragmenten für Lastenausgleichsregeln nicht unterstützt. Die IP-Fragmentierung von UDP- und TCP-Paketen wird für Lastenausgleichsregeln nicht unterstützt. Hochverfügbarkeitsports für Lastenausgleichsregeln können verwendet werden, um vorhandene IP-Fragmente weiterzuleiten. Weitere Informationen finden Sie unter [Übersicht über Hochverfügbarkeitsports](load-balancer-ha-ports-overview.md).

## <a name="next-steps"></a>Nächste Schritte

Informationen zu den ersten Schritten mit einer Load Balancer-Instanz finden Sie unter [Schnellstart: Erstellen eines Load Balancers im Tarif „Standard“ für den Lastenausgleich virtueller Computer über das Azure-Portal](quickstart-load-balancer-standard-public-portal.md). Dort erfahren Sie, wie Sie eine Load Balancer-Instanz und virtuelle Computer mit einer installierten benutzerdefinierten IIS-Erweiterung erstellen und den Lastenausgleich für die Web-App zwischen den virtuellen Computern vornehmen.
