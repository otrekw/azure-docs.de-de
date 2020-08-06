---
title: Ausgehende Verbindungen in Azure
titleSuffix: Azure Load Balancer
description: In diesem Artikel wird erläutert, wie Azure virtuellen Computern die Kommunikation mit öffentlichen Internetdiensten ermöglicht.
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
ms.custom: seodec18
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/24/2020
ms.author: allensu
ms.openlocfilehash: a2292dc789938b8bde709728f5bbffe661529cc2
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87072625"
---
# <a name="outbound-connections-in-azure"></a>Ausgehende Verbindungen in Azure

Azure Load Balancer bietet ausgehende Konnektivität über verschiedene Mechanismen. In diesem Artikel werden die Szenarien und ihre Handhabung beschrieben. Wenn Sie Probleme mit ausgehender Konnektivität über einen Azure Load Balancer haben, finden Sie weitere Informationen im [Leitfaden zur Problembehandlung für ausgehende Verbindungen](../load-balancer/troubleshoot-outbound-connection.md).

>[!NOTE]
>Dieser Artikel gilt für Ressource Manager-Bereitstellungen. Microsoft empfiehlt Ressource Manager für Produktionsworkloads.

## <a name="terminology"></a>Begriff

| Begriff | Zutreffendes Protokolle | Details|
| ---------|---------| -------|
| Übersetzung der Quellnetzwerkadresse (Source Network Address Translation, SNAT) | TCP, UDP | Eine Bereitstellung in Azure kann mit Endpunkten außerhalb von Azure im öffentlichen IP-Adressraum kommunizieren. Wenn eine Instanz einen ausgehenden Datenfluss zu einem Ziel im öffentlichen IP-Adressraum initiiert, ordnet Azure die private IP-Adresse dynamisch einer öffentlichen IP-Adresse zu. Nachdem diese Zuordnung erstellt wurde, kann der Antwortdatenverkehr für diesen ursprünglich ausgehenden Datenfluss auch die private IP-Adresse erreichen, von welcher der Datenfluss stammt. Azure verwendet für diese Aufgabe das Verfahren **Übersetzung der Quellnetzwerkadresse** (Source Network Address Translation, SNAT).|
| Portmaskierung mit SNAT (PAT)| TCP, UDP |  Wenn mehrere private IP-Adressen durch eine einzelne öffentliche IP-Adresse maskiert sind, wird in Azure die **Portadressenübersetzung** (Port Address Translation, PAT) genutzt, um private IP-Adressen zu maskieren/auszublenden. Kurzlebige Ports werden für PAT verwendet und sind je nach Poolgröße [vorab zugeordnet](#preallocatedports). Wenn eine öffentliche Load Balancer-Ressource VM-Instanzen zugeordnet wird, die über keine dedizierte öffentliche IP-Adresse verfügen, werden alle ausgehenden Verbindungsquellen umgeschrieben. Die Quelle wird aus dem privaten IP-Adressraum des virtuellen Netzwerks in die öffentliche Front-End-IP-Adresse des Lastenausgleichs umgeschrieben. Im öffentlichen IP-Adressraum müssen die fünf Tupel des Datenflusses (IP-Quelladresse, Quellport, IP-Transportprotokoll, IP-Zieladresse, Zielport) eindeutig sein. Portmaskierung mit SNAT kann mit entweder mit dem TCP- oder UDP IP-Protokoll verwendet werden. Hierfür werden kurzlebige Ports (SNAT-Ports) verwendet, nachdem die private IP-Quelladresse umgeschrieben wurde, da mehrere Datenflüsse von einer einzelnen öffentlichen IP-Adresse stammen. Die Portmaskierung mit SNAT-Algorithmus weist SNAT-Ports für UDP und TCP unterschiedlich zu.|
| SNAT-Ports| TCP | SNAT-Ports sind kurzlebige Ports, die für eine bestimmte öffentliche IP-Quelladresse verfügbar sind. Pro Datenfluss zu einer einzelnen IP-Zieladresse/einem einzelnen Port wird ein SNAT-Port genutzt. Bei mehreren TCP-Datenflüssen zur gleichen IP-Zieladresse bzw. zum Port und Protokoll belegt jeder TCP-Datenfluss einen einzelnen SNAT-Port. Hierdurch wird sichergestellt, dass die Datenflüsse eindeutig sind, wenn sie von der gleichen öffentlichen IP-Adresse stammen und die gleiche IP-Zieladresse, den gleichen Port und das gleiche Protokoll aufweisen. Mehrere Datenflüsse mit jeweils anderen Angaben für IP-Zieladresse, Port und Protokoll verwenden gemeinsam einen einzelnen SNAT-Port. Die IP-Adresse, der Port und das Protokoll für das Ziel machen den Datenfluss eindeutig, ohne dass zusätzliche Quellports verwendet werden müssen, um Datenflüsse im öffentlichen IP-Adressraum zu unterscheiden.|
|SNAT-Ports | UDP | UDP-SNAT-Ports werden von einem anderen Algorithmus als TCP-SNAT-Ports verwaltet.  Load Balancer verwendet für UDP einen Algorithmus, der als „portbeschränktes Cone-NAT“ bezeichnet wird.  Für jeden Datenfluss wird unabhängig von der IP-Zieladresse/dem Zielport ein SNAT-Port genutzt.|
| Auslastung | - | Sobald die SNAT-Portressourcen erschöpft sind, sind ausgehende Datenflüsse erst wieder möglich, wenn SNAT-Ports von vorhandenen Datenflüssen freigegeben werden. Der Load Balancer gibt die SNAT-Ports wieder frei, wenn der Datenflussvorgang abgeschlossen ist. Es wird ein [Leerlauftimeout von 4 Minuten](../load-balancer/troubleshoot-outbound-connection.md#idletimeout) verwendet, um SNAT-Ports für im Leerlauf befindliche Datenflüsse wieder freizugeben. UDP-SNAT-Ports erschöpfen aufgrund des Unterschieds im verwendeten Algorithmus in der Regel viel schneller als TCP-SNAT-Ports. Diesen Unterschied müssen Sie beim Entwerfen und Skalieren berücksichtigen.|
| Verhalten der SNAT-Portfreigabe | TCP | Wenn entweder der Server oder der Client FINACK sendet, wird der SNAT-Port nach 240 Sekunden freigegeben. Tritt ein RST auf, wird der SNAT-Port nach 15 Sekunden freigegeben. Ist das Leerlauftimeout erreicht, wird der Port freigegeben.|
| Verhalten der SNAT-Portfreigabe | UDP |Ist das Leerlauftimeout erreicht, wird der Port freigegeben.|
| Wiederverwendung von SNAT-Ports | TCP, UDP | Nach der Freigabe eines Ports kann er bei Bedarf erneut verwendet werden.  Sie können sich SNAT-Ports als eine Sequenz verfügbarer Ports (vom niedrigsten bis zum höchsten) vorstellen, die für ein bestimmtes Szenario verfügbar sind. Der erste verfügbare SNAT-Port wird für neue Verbindungen verwendet.|

### <a name="port-allocation-algorithm"></a><a name="preallocatedports"></a>Portzuordnungsalgorithmus

In Azure wird ein Algorithmus verwendet, um basierend auf der Größe des Back-End-Pools bei der PAT die Anzahl von verfügbaren vorab zugeordneten SNAT-Ports zu ermitteln. Für alle öffentlichen IP-Adressen, die einem Lastenausgleich zugeordnet sind, stehen 64.000 Ports als SNAT-Ports für jedes IP-Transportprotokoll zur Verfügung. Dieselbe Anzahl von SNAT-Ports wird vorab für UDP bzw. TCP zugeordnet und unabhängig voneinander pro IP-Transportprotokoll genutzt.  Die Verwendung des SNAT-Ports unterscheidet sich jedoch abhängig davon, ob es sich um einen UDP- oder TCP-Datenfluss handelt. Bei Erstellung von ausgehenden Datenflüssen werden diese Ports dynamisch genutzt (bis zum vorab festgelegten Grenzwert) und wieder freigegeben, wenn der Datenfluss geschlossen wird oder ein [Leerlauftimeout](../load-balancer/troubleshoot-outbound-connection.md#idletimeout) eintritt. Ports werden nur genutzt, wenn Datenflüsse eindeutig gemacht werden müssen.

#### <a name="default-snat-ports-allocated"></a><a name="snatporttable"></a> Zugeordnete SNAT-Standardports

In der folgenden Tabelle sind die SNAT-Port-Vorabzuordnungen für die Ebenen der Back-End-Poolgrößen angegeben:

| Poolgröße (VM-Instanzen) | Vorab zugeordnete SNAT-Ports pro IP-Konfiguration |
| --- | --- |
| 1-50 | 1\.024 |
| 51-100 | 512 |
| 101-200 | 256 |
| 201-400 | 128 |
| 401-800 | 64 |
| 801-1.000 | 32 |

Von einer Größenänderung Ihres Back-End-Pools können einige der eingerichteten Datenflüsse betroffen sein:

- Wenn die Größe des Back-End-Pools zunimmt und der Übergang auf die nächste Ebene erfolgt, wird die Hälfte der vorab zugeordneten SNAT-Ports während des Übergangs zur nächstgrößeren Back-End-Poolebene freigegeben. Bei Datenflüssen, die einem wieder freigegebenen SNAT-Port zugeordnet sind, tritt ein Timeout auf, und sie müssen neu eingerichtet werden. Versuche zum Einrichten eines neuen Datenflusses sind sofort erfolgreich, solange vorab zugeordnete Ports verfügbar sind.
- Wenn sich die Größe des Back-End-Pools reduziert und der Übergang auf eine niedrigere Ebene erfolgt, nimmt die Anzahl der verfügbaren SNAT-Ports zu. In diesem Fall sind vorhandene zugeordnete SNAT-Ports und die entsprechenden Datenflüsse nicht betroffen.

## <a name="outbound-connections-scenario-overview"></a><a name="scenarios"></a>Übersicht über das Szenario mit ausgehenden Verbindungen

| Szenario | Methode | IP-Protokolle | BESCHREIBUNG |
|  --- | --- | --- | --- |
|  1. Virtueller Computer mit öffentlicher IP-Adresse (mit oder ohne Azure Load Balancer) | SNAT, keine Portmaskierung | TCP, UDP, ICMP, ESP | In Azure wird die öffentliche IP-Adresse verwendet, die der IP-Konfiguration der NIC einer Instanz für alle ausgehenden Datenflüsse zugewiesen ist. Für die Instanz sind alle kurzlebigen Ports verfügbar. Es ist unerheblich, ob der virtuelle Computer einen Lastenausgleich aufweist oder nicht. Dieses Szenario hat Vorrang vor den anderen Szenarien. Eine öffentliche IP-Adresse, die einem virtuellen Computer zugewiesen ist, ist eine 1:1-Beziehung (keine 1:n-Beziehung) und wird als zustandslose 1:1-NAT implementiert. |
| 2. Öffentlicher Load Balancer, der einem virtuellen Computer zugeordnet ist (keine öffentliche IP-Adresse für die VM/Instanz) | SNAT mit Portmaskierung (PAT) unter Verwendung der Front-Ends des Lastenausgleichs | TCP, UDP | In diesem Szenario muss die Lastenausgleichsressource mit einer Lastenausgleichsregel zum Erstellen einer Verknüpfung der öffentlichen Front-End-IP mit dem Back-End-Pool konfiguriert werden. Wenn Sie diese Regelkonfiguration nicht abschließen, ist das Verhalten wie in Szenario 3 beschrieben. Die Regel muss nicht über einen funktionierenden Listener im Back-End-Pool verfügen, damit der Integritätstest erfolgreich durchgeführt werden kann. Wenn die VM einen ausgehenden Datenfluss erstellt, übersetzt Azure die private IP-Quelladresse des ausgehenden Datenflusses mithilfe von SNAT in die öffentliche IP-Adresse des öffentlichen Lastenausgleichs-Frond-End. Mit kurzlebigen Ports der öffentlichen Front-End-IP-Adresse des Lastenausgleichs werden die einzelnen Datenflüsse unterschieden, die von dem virtuellen Computer stammen. Beim Erstellen ausgehender Datenflüsse werden für SNAT [vorab zugewiesene kurzlebige Ports](#preallocatedports) verwendet. In diesem Zusammenhang werden die kurzlebigen für SNAT verwendeten Ports als SNAT-Ports bezeichnet. SNAT-Ports sind vorab zugeordnet, wie in der Tabelle [Standardmäßig zugeordnete SNAT-Ports](#snatporttable) beschrieben. |
| 3. VM (kein Load Balancer, keine öffentliche IP-Adresse) oder VM, die der internen Load Balancer-Instanz im Tarif „Basic“ zugeordnet ist. | SNAT mit Portmaskierung (PAT) | TCP, UDP | Wenn der virtuelle Computer einen ausgehenden Datenfluss einleitet, übersetzt Azure die private IP-Quelladresse für den ausgehenden Datenfluss in eine öffentliche IP-Quelladresse. Diese öffentliche IP-Adresse ist **nicht konfigurierbar**, kann nicht reserviert werden und wird nicht auf den Grenzwert der öffentlichen IP-Ressourcen des Abonnements angerechnet. Bei der erneuten Bereitstellung der VM, Verfügbarkeitsgruppe oder VM-Skalierungsgruppe wird diese öffentliche IP-Adresse freigegeben und eine neue öffentliche IP-Adresse angefordert. Verwenden Sie dieses Szenario nicht für die Aufnahme von IP-Adressen in die Whitelist. Verwenden Sie stattdessen Szenario 1 oder 2, in dem Sie das ausgehende Verhalten explizit deklarieren. SNAT-Ports sind vorab zugeordnet, wie in der Tabelle [Standardmäßig zugeordnete SNAT-Ports](#snatporttable) beschrieben.

## <a name="outbound-rules"></a><a name="outboundrules"></a>Ausgangsregeln

 Mit Ausgangsregeln können Sie die Netzwerkadressenübersetzung (NAT) einer öffentlichen [Load Balancer Standard](load-balancer-standard-overview.md)-Instanz für ausgehenden Datenverkehr ganz einfach konfigurieren.  Sie verfügen über die vollständige deklarative Steuerung über die ausgehenden Verbindungen, um diese Funktionalität nach Belieben zu skalieren und anzupassen. In diesem Abschnitt wird das oben beschriebene Szenario 2 (B) erweitert.

![Load Balancer-Ausgangsregeln](media/load-balancer-outbound-rules-overview/load-balancer-outbound-rules.png)

Mit Ausgangsregeln können Sie Load Balancer verwenden, um die ausgehende NAT von Grund auf neu zu definieren. Sie können auch das Verhalten einer vorhandenen NAT für ausgehenden Datenverkehr skalieren und optimieren.

Mit Ausgangsregeln können Sie steuern:

- Welchen IP-Adressen von VMs in welche öffentlichen IP-Adressen übersetzt werden
- Wie ausgehende SNAT-Ports zugeordnet werden
- Welche Protokolle zum Übersetzen ausgehenden Datenverkehrs verwendet werden
- Wie lange das Leerlauftimeout für ausgehende Verbindungen dauert (4-120 Minuten)
- Ob eine TCP-Zurücksetzung bei Leerlauftimeout gesendet wird
- TCP- und UDP-Transportprotokolle mit einer einzigen Regel

### <a name="outbound-rule-definition"></a>Definition der Ausgangsregel

Wie alle Load Balancer-Regeln folgen auch die Ausgangsregeln der gleichen vertrauten Syntax wie Lastenausgleichsregeln und NAT-Eingangsregeln: **Front-End** + **Parameter** + **Back-End-Pool**. Eine Ausgangsregel konfiguriert die NAT für ausgehenden Datenverkehr für _alle VMs, die vom Back-End-Pool_ für die _Front-End_-Übersetzung identifiziert wurden.  Die _Parameter_ ermöglichen eine zusätzliche differenzierte Steuerung des NAT-Algorithmus für ausgehenden Datenverkehr.

### <a name="scale-outbound-nat-with-multiple-ip-addresses"></a><a name="scale"></a> Skalieren der NAT für ausgehenden Datenverkehr mit mehreren IP-Adressen

Jede zusätzliche IP-Adresse, die von einem Front-End bereitgestellt wird, stellt zusätzliche 64.000 kurzlebige Ports zur Verfügung, die Load Balancer als SNAT-Ports verwenden kann. Sie können mehrere IP-Adressen verwenden, um umfangreiche Szenarien zu planen. Mithilfe von Ausgangsregeln lassen sich außerdem die für die [SNAT-Überlastung](troubleshoot-outbound-connection.md#snatexhaust) anfälligen Muster reduzieren.  

Sie können auch ein [öffentliches IP-Präfix](https://aka.ms/lbpublicipprefix) direkt mit einer Ausgangsregel verwenden.  Durch die Verwendung von öffentlichen IP-Präfixen ist es einfacher, Datenflüsse aus Ihrer Azure-Bereitstellung zu skalieren und auf die Whitelist zu setzen. Sie können eine Front-End-IP-Konfiguration innerhalb der Load Balancer-Ressource konfigurieren, um direkt auf den Präfix einer öffentlichen IP-Adresse zu verweisen.  Dies ermöglicht Load Balancer die exklusive Steuerung über das Präfix für öffentliche IP-Adressen, und die Ausgangsregel verwendet automatisch alle öffentlichen IP-Adressen, die im Präfix für öffentliche IP-Adressen für ausgehende Verbindungen enthalten sind.  Jede IP-Adresse innerhalb des öffentlichen IP-Präfixes bietet zusätzliche 64.000 kurzlebige Ports pro IP-Adresse, die Load Balancer als SNAT-Ports verwenden kann.

### <a name="outbound-flow-idle-timeout-and-tcp-reset"></a><a name="idletimeout"></a> Leerlauftimeout für ausgehenden Datenfluss und TCP-Zurücksetzung

Ausgangsregeln stellen einen Konfigurationsparameter bereit, um das Leerlauftimeout für den ausgehenden Datenfluss zu steuern und ihn an die Anforderungen Ihrer Anwendung anzupassen. Das Leerlauftimeout für ausgehenden Datenverkehr tritt standardmäßig nach 4 Minuten ein. Sie können lernen, [Leerlauftimeouts](load-balancer-tcp-idle-timeout.md#tcp-idle-timeout) zu konfigurieren. Das Standardverhalten von Load Balancer ist es, den Datenfluss allmählich zu entfernen, wenn das Leerlauftimeout für ausgehenden Datenverkehr erreicht wurde.  Mit dem Parameter `enableTCPReset` können Sie ein besser vorhersagbares Anwendungsverhalten aktivieren und steuern, ob bidirektionale TCP-Zurücksetzung (TCP RST) außerhalb des Leerlauftimeouts für ausgehenden Datenverkehr gesendet wird. Unter [TCP-Zurücksetzung bei Leerlauftimeout](https://aka.ms/lbtcpreset) finden Sie weitere Informationen, unter anderem zur regionalen Verfügbarkeit.

### <a name="preventing-outbound-connectivity"></a><a name="preventoutbound"></a>Verhindern der Konnektivität in ausgehender Richtung

Lastenausgleichsregeln bieten eine automatische Programmierung der NAT für ausgehenden Datenverkehr. In einigen Szenarien müssen Sie jedoch die automatische Programmierung der NAT für ausgehenden Datenverkehr durch die Lastenausgleichsregel deaktivieren, um das Verhalten zu steuern oder zu verfeinern.  
Sie haben zwei Möglichkeiten, diesen Parameter zu verwenden:

1. Optionale Unterdrückung der Verwendung der eingehenden IP-Adresse für die SNAT für ausgehenden Datenverkehr durch Deaktivieren der SNAT für ausgehenden Datenverkehr für eine Lastenausgleichsregel
  
2. Passen Sie die SNAT-Parameter für ausgehenden Datenverkehr einer IP-Adresse an, die zugleich für eingehenden und ausgehenden Datenverkehr verwendet wird.  Die automatische NAT-Programmierung für ausgehenden Datenverkehr muss deaktiviert werden, damit eine Ausgangsregel die Kontrolle übernehmen kann.  Um beispielsweise die SNAT-Portzuordnung einer Adresse zu ändern, die auch für den eingehenden Datenverkehr verwendet wird, muss der Parameter `disableOutboundSnat` TRUE entsprechen.  Wenn Sie versuchen, mit einer Ausgangsregel die Parameter einer IP-Adresse neu zu definieren, die auch für eingehenden Datenverkehr verwendet wird, und die NAT-Programmierung für ausgehenden Datenverkehr für die Lastenausgleichsregel nicht freigegeben wurde, schlägt die Konfiguration einer Ausgangsregel fehl.

>[!IMPORTANT]
> Ihre VM hat keine ausgehenden Verbindungen, wenn Sie diesen Parameter auf TRUE festlegen und keine Ausgangsregel zum Definieren ausgehender Verbindungen haben.  Einige Vorgänge Ihrer VM oder Ihrer Anwendung können davon abhängen, ob ausgehende Verbindungen verfügbar sind. Stellen Sie sicher, dass Sie die Abhängigkeiten Ihres Szenarios verstehen und die Auswirkungen von Änderungen berücksichtigt haben.

Manchmal ist es nicht wünschenswert, einem virtuellen Computer das Erstellen eines ausgehenden Datenflusses zu erlauben. Möglicherweise müssen Sie auch verwalten, welche Ziele mit ausgehenden Datenflüssen erreicht werden bzw. welche Ziele eingehende Datenflüsse beginnen können. In diesem Fall können Sie [Netzwerksicherheitsgruppen](../virtual-network/security-overview.md) zum Verwalten der vom virtuellen Computer erreichbaren Ziele verwenden. Mit Netzwerksicherheitsgruppen können Sie auch verwalten, welches öffentliche Ziel eingehende Datenflüsse initiieren kann.

Wenn Sie eine Netzwerksicherheitsgruppe einem virtuellen Computer mit Lastenausgleich zuordnen, müssen Sie auf die [Diensttags](../virtual-network/security-overview.md#service-tags) und [Standardsicherheitsregeln](../virtual-network/security-overview.md#default-security-rules) achten. Sie müssen sicherstellen, dass die VM Anforderungen von Integritätstests von Azure Load Balancer empfangen kann.

Wenn eine Netzwerksicherheitsgruppe Anforderungen von Integritätstests vom Standardtag AZURE_LOADBALANCER blockiert, misslingt Ihr VM-Integritätstests, weshalb die VM mit „Außer Betrieb“ markiert wird. Der Lastenausgleich beendet das Senden neuer Datenflüsse an diese VM.

## <a name="scenarios-with-outbound-rules"></a>Szenarien mit Ausgangsregeln

| # | Szenario| Details|
|---|---|---|
| I | Bereinigen ausgehender Verbindungen für einen bestimmten Satz öffentlicher IP-Adressen| Sie können eine Ausgangsregel verwenden, um ausgehende Verbindungen so zu konfigurieren, dass sie scheinbar von einem bestimmten Satz öffentlicher IP-Adressen stammen, um Whitelistszenarien zu vereinfachen.  Diese öffentliche Quell-IP-Adresse kann die gleiche sein, die von einer Lastenausgleichsregel verwendet wird, oder ein anderer Satz öffentlicher IP-Adressen, als der von einer Lastenausgleichsregel verwendete.  1. Erstellen Sie ein [Präfix für öffentliche IP-Adressen](https://aka.ms/lbpublicipprefix) (oder öffentliche IP-Adressen aus einem Präfix für öffentliche IP-Adressen). 2. Erstellen Sie eine öffentliche Load Balancer Standard-Instanz. 3. Erstellen Sie Front-Ends, die auf das gewünschte Präfix für öffentliche IP-Adressen (oder öffentliche IP-Adressen) verweisen. 4. Verwenden Sie einen vorhandenen Back-End-Pool erneut, oder erstellen Sie einen neuen Back-End-Pool, und stellen Sie die VMs in einem Back-End-Pool der öffentlichen Load Balancer-Instanz bereit. 5. Konfigurieren Sie eine Ausgangsregel in der öffentlichen Load Balancer-Instanz, um NAT für ausgehenden Datenverkehr für diese VMs über die Front-Ends zu programmieren. Wenn die Lastenausgleichsregel nicht für den ausgehenden Datenverkehr verwendet werden soll, deaktivieren Sie die SNAT für ausgehenden Datenverkehr für die Lastenausgleichsregel.
| II | Ändern der SNAT-Portzuordnung| Sie können Ausgangsregeln verwenden, um die [automatische SNAT-Portzuweisung basierend auf der Back-End-Poolgröße](load-balancer-outbound-connections.md#preallocatedports) anzupassen. Wenn Sie beispielsweise zwei VMs haben, die sich eine einzige öffentliche IP-Adresse für NAT für ausgehenden Datenverkehr teilen, können Sie im Fall einer SNAT-Überlastung die Anzahl von SNAT-Ports, die von den standardmäßigen 1.024 Ports zugeordnet wurden, erhöhen. Jede öffentliche IP-Adresse kann bis zu 64.000 kurzlebige Ports bereitstellen.  Wenn Sie eine Ausgangsregel mit einem einzigen öffentlichen IP-Adressen-Front-End konfigurieren, können Sie insgesamt 64.000 SNAT-Ports auf VMs im Back-End-Pool verteilen.  Bei zwei VMs können maximal 32.000 SNAT-Ports mit einer Ausgangsregel (2 x 32.000 = 64.000) zugeordnet werden. Sie können Ausgangsregeln verwenden, um die standardmäßig zugeordneten SNAT-Ports zu optimieren. Sie ordnen mehr oder weniger zu, als die standardmäßige SNAT-Portzuordnung vorsieht. Jede öffentliche IP-Adresse von allen Front-Ends einer Ausgangsregel steuert bis zu 64.000 kurzlebige Ports zur Verwendung als SNAT-Ports bei.  Load Balancer weist SNAT-Ports als ein Vielfaches von 8 zu. Wenn Sie einen Wert angeben, der nicht durch 8 teilbar ist, wird der Konfigurationsvorgang abgelehnt.  Wenn Sie versuchen, mehr SNAT-Ports zuzuordnen als öffentliche IP-Adressen vorhanden sind, wird der Konfigurationsvorgang abgelehnt.  Wenn Sie beispielsweise 10.000 Ports pro VM zuweisen und 7 VMs in einem Back-End-Pool eine einzige öffentliche IP-Adresse teilen würden, wird die Konfiguration zurückgewiesen (7 x 10.000 SNAT-Ports > 64.000 SNAT-Ports).  Dem Front-End der Ausgangsregel lassen sich weitere öffentliche IP-Adressen hinzufügen, um das Szenario zu ermöglichen. Sie können die [standardmäßige SNAT-Portzuordnung basierend auf der Back-End-Poolgröße](load-balancer-outbound-connections.md#preallocatedports) wiederherstellen, indem Sie als Portanzahl „0“ angeben. In diesem Fall erhalten die ersten 50 VM-Instanzen 1024 Ports. Die VM-Instanzen von 51-100 erhalten 512 usw. entsprechend der [Tabelle](#snatporttable).|
| III| Ausschließliches Aktivieren des ausgehenden Datenverkehrs | Sie können eine öffentliche Load Balancer Standard-Instanz verwenden, um NAT für ausgehenden Datenverkehr für eine Gruppe von VMs bereitzustellen. In diesem Szenario können Sie eine Ausgangsregel alleine ohne zusätzliche Regeln verwenden.|
| IV | NAT für ausgehenden Datenverkehr nur für VMs (kein eingehender Datenverkehr) | Definieren Sie eine öffentliche Load Balancer Standard-Instanz, stellen Sie die VMs im Back-End-Pool bereit, und konfigurieren Sie eine Ausgangsregel, um NAT für ausgehenden Datenverkehr zu programmieren und die ausgehenden Verbindungen so zu konfigurieren, dass sie von einer bestimmten öffentlichen IP-Adresse stammen. Sie können auch ein Präfix für öffentliche IP-Adressen verwenden. Das vereinfacht, die Quelle ausgehender Verbindungen auf die Whitelist zu setzen. 1. Erstellen Sie eine öffentliche Load Balancer Standard-Instanz. 2. Erstellen Sie einen Back-End-Pool, und stellen Sie die VMs in einem Back-End-Pool der öffentlichen Load Balancer-Instanz bereit. 3. Konfigurieren Sie eine Ausgangsregel in der öffentlichen Load Balancer-Instanz, um die NAT für ausgehenden Datenverkehr für diese VMs zu programmieren.
| V| NAT für ausgehenden Datenverkehr für interne Load Balancer Standard-Szenarien| Beim Verwenden einer internen Load Balancer Standardinstanz ist die NAT für ausgehenden Datenverkehr erst verfügbar, nachdem die ausgehende Konnektivität explizit deklariert wurde. Sie können die ausgehende Konnektivität mit einer Ausgangsregel definieren, indem Sie mithilfe dieser Schritte ausgehende Konnektivität für VMs hinter einer internen Load Balancer-Standardinstanz erstellen: 1. Erstellen Sie eine öffentliche Load Balancer Standard-Instanz. 2. Erstellen Sie einen Back-End-Pool, und stellen Sie die VMs über die interne Load Balancer-Instanz hinaus in einem Back-End-Pool der öffentlichen Load Balancer-Instanz bereit. 3. Konfigurieren Sie eine Ausgangsregel in der öffentlichen Load Balancer-Instanz, um die NAT für ausgehenden Datenverkehr für diese VMs zu programmieren.|
| VI | Aktivieren von TCP- und UDP-Protokollen für NAT für ausgehenden Datenverkehr mit einer öffentlichen Load Balancer Standard-Instanz | Beim Verwenden einer öffentlichen Load Balancer Standard-Instanz entspricht die bereitgestellte automatische NAT-Programmierung für ausgehenden Datenverkehr dem Transportprotokoll der Lastenausgleichsregel. 1. Deaktivieren Sie SNAT für ausgehenden Datenverkehr in der Lastenausgleichsregel. 2. Konfigurieren Sie eine Ausgangsregel in derselben Load Balancer-Instanz. 3. Verwenden Sie erneut den Back-End-Pool, den Ihre VMs bereits verwenden. 4. Geben Sie „Protokoll“: „Alle“ als Teil der Ausgangsregel an. Wenn nur NAT-Eingangsregeln verwendet werden, wird keine NAT für ausgehenden Datenverkehr bereitgestellt. 1. Stellen Sie die VMs in einem Back-End-Pool bereit. 2. Definieren Sie mindestens eine Front-End-IP-Konfiguration mit mindestens einer öffentlichen IP-Adresse oder einem Präfix für öffentliche IP-Adressen. 3. Konfigurieren Sie eine Ausgangsregel in derselben Load Balancer-Instanz. 4. Geben Sie „Protokoll“: „Alle“ als Teil der Ausgangsregel an. |


## <a name="limitations"></a>Einschränkungen

- Die maximale Anzahl von verwendbaren kurzlebigen Ports pro Front-End-IP-Adresse beträgt 64.000.
- Das konfigurierbare Leerlauftimeout für ausgehenden Datenverkehr beträgt ist 4 bis 120 Minuten (240 bis 7200 Sekunden).
- Load Balancer unterstützt kein ICMP für die NAT ausgehenden Datenverkehrs.
- Ausgangsregeln können nur auf die primäre IP-Konfiguration einer NIC angewandt werden.  Es werden mehrere NICs unterstützt.
- Auf Web-Workerrollen ohne VNet und andere Plattformdienste von Microsoft kann aufgrund eines Nebeneffekts der Funktionsweise von Diensten vor VNet und anderen Plattformdiensten nur zugegriffen werden, wenn interner Standard-Load Balancer verwendet wird. Verlassen Sie sich nicht auf diesen Nebeneffekt, da der jeweilige Dienst oder die zugrunde liegende Plattform ohne vorherige Ankündigung geändert werden kann. Sie müssen immer davon ausgehen, dass Sie ausgehende Verbindungen, falls gewünscht, explizit erstellen müssen, wenn Sie nur einen internen Load Balancer im Tarif „Standard“ verwenden. Das in diesem Artikel beschriebene Szenario 3 ist nicht verfügbar.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen finden Sie unter [Load Balancer Standard](load-balancer-standard-overview.md).
- Weitere Informationen finden Sie unter den [häufig gestellten Fragen zu Azure Load Balancer](load-balancer-faqs.md).
- Weitere Informationen zu [Ausgangsregeln](load-balancer-outbound-rules-overview.md) für Standard Public Load Balancer.
- Weitere Informationen zu [Load Balancer](load-balancer-overview.md).
- Weitere Informationen zu [Netzwerksicherheitsgruppen](../virtual-network/security-overview.md).
- Erfahren Sie mehr über die anderen zentralen [Netzwerkfunktionen](../networking/networking-overview.md) in Azure.
