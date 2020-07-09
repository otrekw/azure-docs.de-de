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
ms.date: 08/07/2019
ms.author: allensu
ms.openlocfilehash: 0e46905ad280cd76d66befb1156e428b23f35664
ms.sourcegitcommit: f1132db5c8ad5a0f2193d751e341e1cd31989854
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/31/2020
ms.locfileid: "84235642"
---
# <a name="outbound-connections-in-azure"></a>Ausgehende Verbindungen in Azure

Der Azure Load Balancer erzielt die Konnektivität in ausgehender Richtung für Benutzerbereitstellungen mit mehreren unterschiedlichen Mechanismen. In diesem Artikel wird beschrieben, welche Szenarien es gibt, wann sie zutreffen, wie sie funktionieren und wie sie verwaltet werden. Wenn Sie Probleme mit ausgehender Konnektivität über einen Azure Load Balancer haben, finden Sie weitere Informationen im [Leitfaden zur Problembehandlung für ausgehende Verbindungen] (../load-balancer/troubleshoot-outbound-connection.md).

>[!NOTE] 
>Dieser Artikel gilt nur für Ressourcen-Manager-Bereitstellungen. Lesen Sie für alle klassischen Bereitstellungsszenarien in Azure den Artikel [Ausgehende Verbindungen (klassisch)](load-balancer-outbound-connections-classic.md).

Eine Bereitstellung in Azure kann mit Endpunkten außerhalb von Azure im öffentlichen IP-Adressraum kommunizieren. Wenn eine Instanz einen ausgehenden Datenfluss zu einem Ziel im öffentlichen IP-Adressraum initiiert, ordnet Azure die private IP-Adresse dynamisch einer öffentlichen IP-Adresse zu. Nachdem diese Zuordnung erstellt wurde, kann der Antwortdatenverkehr für diesen ursprünglich ausgehenden Datenfluss auch die private IP-Adresse erreichen, von welcher der Datenfluss stammt.

Azure verwendet für diese Aufgabe das Verfahren „Übersetzung der Quellnetzwerkadresse“ (Source Network Address Translation, SNAT). Wenn mehrere private IP-Adressen durch eine einzelne öffentliche IP-Adresse maskiert sind, wird in Azure die [Portadressenübersetzung](#pat) (Port Address Translation, PAT) genutzt, um private IP-Adressen zu maskieren. Kurzlebige Ports werden für PAT verwendet und sind je nach Poolgröße [vorab zugeordnet](#preallocatedports).

Es gibt mehrere [Szenarien für die ausgehende Richtung](#scenarios). Diese Szenarien können nach Bedarf kombiniert werden. Prüfen Sie sie sorgfältig, um die Funktionen, Einschränkungen und Muster sowie die damit verbundenen Auswirkungen auf Ihr Bereitstellungsmodell und Anwendungsszenario zu verstehen. Sehen Sie sich die Anleitung zur [Verwaltung dieser Szenarien](../load-balancer/troubleshoot-outbound-connection.md#snatexhaust) an.

>[!IMPORTANT] 
>Load Balancer Standard und Standard Public IP führen neue Fähigkeiten und andere Verhaltensweisen für ausgehende Verbindungen ein.  Sie sind nicht identisch mit Basic-SKUs.  Wenn Sie bei der Arbeit mit Standard-SKUs ausgehende Verbindungen wünschen, müssen Sie diese explizit entweder mit Standard Public IP-Adressen oder Standard Public Load Balancer definieren.  Dies umfasst die Erstellung von ausgehenden Verbindungen bei der Verwendung eines internen Load Balancer Standard.  Es wird empfohlen, dass Sie für einen Standard Public Load Balancer immer Ausgangsregeln verwenden.  [Szenario 3](#defaultsnat) ist mit der Standard-SKU nicht verfügbar.  Das bedeutet, dass Sie bei Verwendung eines internen Load Balancer Standard Schritte unternehmen müssen, um die ausgehenden Verbindungen für die VMs im Back-End-Pool herzustellen, wenn ausgehende Verbindungen gewünscht sind.  Im Kontext der ausgehenden Verbindungen verhalten sich eine einzelne eigenständige VM, alle VMs in einer Verfügbarkeitsgruppe sowie alle Instanzen in einem VMSS als Gruppe. Das bedeutet, wenn eine einzelne VM in einer Verfügbarkeitsgruppe einer Standard-SKU zugeordnet ist, verhalten sich jetzt alle VM-Instanzen in dieser Verfügbarkeitsgruppe nach denselben Regeln wie bei der Zuordnung zu einer Standard-SKU, auch wenn eine einzelne Instanz nicht direkt mit ihr verbunden ist. Dieses Verhalten wird auch bei einem eigenständigen virtuellen Computer mit mehreren Netzwerkschnittstellenkarten beobachtet, die an einen Lastenausgleich angeschlossen sind. Wenn eine eigenständige NIC hinzugefügt wird, zeigt sie dasselbe Verhalten. Lesen Sie dieses Dokument sorgfältig und vollständig, um die allgemeinen Konzepte zu verstehen, überprüfen Sie [Load Balancer Standard](load-balancer-standard-overview.md) auf Unterschiede zwischen den SKUs, und überprüfen Sie die [Ausgangsregeln](load-balancer-outbound-rules-overview.md).  Die Verwendung von Ausgangsregeln ermöglicht Ihnen eine differenzierte Steuerung aller Aspekte ausgehender Verbindungen.

## <a name="scenario-overview"></a><a name="scenarios"></a>Übersicht über das Szenario

Azure Load Balancer und die zugehörigen Ressourcen werden bei Verwendung von [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) explizit definiert.  In Azure gibt es derzeit drei verschiedene Methoden, wie die ausgehende Konnektivität für Azure Resource Manager-Ressourcen erreicht werden kann. 

| SKUs | Szenario | Methode | IP-Protokolle | BESCHREIBUNG |
| --- | --- | --- | --- | --- |
| Standard, Basic | [1. Virtuelle Computer mit öffentlicher IP-Adresse auf Instanzebene (mit oder ohne Azure Load Balancer)](#ilpip) | SNAT, keine Portmaskierung | TCP, UDP, ICMP, ESP | In Azure wird die öffentliche IP-Adresse verwendet, die der IP-Konfiguration der NIC einer Instanz zugewiesen ist. Für die Instanz sind alle kurzlebigen Ports verfügbar. Bei Verwendung von Load Balancer Standard werden [Ausgangsregeln](load-balancer-outbound-rules-overview.md) nicht unterstützt, wenn dem virtuellen Computer eine öffentliche IP-Adresse zugewiesen wird. |
| Standard, Basic | [2. Öffentlicher Load Balancer, der einem virtuellen Computer zugeordnet ist (keine öffentliche IP-Adresse für die Instanz)](#lb) | SNAT mit Portmaskierung (PAT) unter Verwendung der Front-Ends des Lastenausgleichs | TCP, UDP |In Azure wird die öffentliche Front-End-IP-Adresse des öffentlichen Lastenausgleichs mit mehreren privaten IP-Adressen gemeinsam genutzt. Für PAT verwendet Azure kurzlebige Ports der Front-Ends. Bei der Verwendung von Load Balancer Standard sollten Sie [Ausgangsregeln](load-balancer-outbound-rules-overview.md) verwenden, um die ausgehenden Verbindungen explizit zu definieren. |
| Keine oder Basic. | [3. Eigenständiger virtueller Computer (kein Load Balancer, keine öffentliche IP-Adresse)](#defaultsnat) | SNAT mit Portmaskierung (PAT) | TCP, UDP | Azure weist SNAT automatisch eine öffentliche IP-Adresse zu, nutzt diese öffentliche IP-Adresse gemeinsam mit mehreren privaten IP-Adressen der Verfügbarkeitsgruppe und verwendet kurzlebige Ports dieser öffentlichen IP-Adresse. Dieses Szenario ist ein Fallback für die vorherigen Szenarien. Es ist nicht zu empfehlen, wenn Sie Sichtbarkeit und Kontrolle benötigen. |

Wenn ein virtueller Computer nicht mit Endpunkten außerhalb von Azure im öffentlichen IP-Adressraum kommunizieren soll, können Sie nach Bedarf zum Blockieren des Zugriffs Netzwerksicherheitsgruppen verwenden. Netzwerksicherheitsgruppen werden im Abschnitt [Verhindern der ausgehenden Konnektivität](#preventoutbound) ausführlicher beschrieben. Das Entwerfen, Implementieren und Verwalten eines virtuellen Netzwerks ohne ausgehenden Zugriff und die entsprechenden Anleitungen hierzu sind nicht Gegenstand dieses Artikels.

### <a name="scenario-1-vm-with-public-ip-address"></a><a name="ilpip"></a>Szenario 1: Virtueller Computer mit öffentlicher IP-Adresse

In diesem Szenario ist dem virtuellen Computer eine öffentliche IP-Adresse zugewiesen. Bei ausgehenden Verbindungen spielt es keine Rolle, ob für den virtuellen Computer ein Lastenausgleich durchgeführt wird. Dieses Szenario hat Vorrang vor den anderen Szenarien. Wenn eine öffentliche IP-Adresse verwendet wird, verwendet der virtuelle Computer die öffentliche IP-Adresse für alle ausgehenden Flows.  

Eine öffentliche IP-Adresse, die einem virtuellen Computer zugewiesen ist, ist eine 1:1-Beziehung (keine 1:n-Beziehung) und wird als zustandslose 1:1-NAT implementiert.  Es wird keine Portmaskierung (PAT) verwendet, und für den virtuellen Computer sind alle kurzlebigen Ports verfügbar.

Wenn Ihre Anwendung viele ausgehende Flows initiiert und es zu einer Überlastung der SNAT-Ports kommt, sollten Sie das [Zuweisen einer öffentlichen IP-Adresse erwägen, um SNAT-Einschränkungen zu mindern](../load-balancer/troubleshoot-outbound-connection.md#assignilpip). Lesen Sie den Abschnitt [Verwalten der SNAT-Auslastung](../load-balancer/troubleshoot-outbound-connection.md#snatexhaust) ganz durch.

### <a name="scenario-2-load-balanced-vm-without-a-public-ip-address"></a><a name="lb"></a>Szenario 2: Virtueller Computer mit Lastenausgleich ohne öffentliche IP-Adresse

Bei diesem Szenario ist die VM Teil eines Back-End-Pools für den öffentlichen Lastenausgleich. Der VM ist keine öffentliche IP-Adresse zugewiesen. Die Lastenausgleichsressource muss mit einer Lastenausgleichsregel zum Erstellen einer Verknüpfung der öffentlichen Front-End-IP mit dem Back-End-Pool konfiguriert werden.

Wenn Sie diese Regelkonfiguration nicht abschließen, ergibt sich das unter [Eigenständiger virtueller Computer ohne öffentliche IP-Adresse](#defaultsnat) beschriebene Verhalten. Die Regel muss nicht über einen funktionierenden Listener im Back-End-Pool verfügen, damit der Integritätstest erfolgreich durchgeführt werden kann.

Wenn die dem Lastenausgleich unterliegende VM einen ausgehenden Datenfluss erstellt, übersetzt Azure die private IP-Quelladresse des ausgehenden Datenflusses in die öffentliche IP-Adresse des öffentlichen Lastenausgleichs-Frond-End. Azure verwendet SNAT, um diese Funktion durchzuführen. Azure verwendet auch [PAT](#pat), um mehrere private IP-Adressen durch eine öffentliche IP-Adresse zu maskieren. 

Mit kurzlebigen Ports der öffentlichen Front-End-IP-Adresse des Lastenausgleich werden die einzelnen Datenflüsse unterschieden, die von dem virtuellen Computer stammen. Beim Erstellen ausgehender Datenflüsse werden für SNAT [vorab zugewiesene kurzlebige Ports](#preallocatedports) verwendet. In diesem Zusammenhang werden die kurzlebigen für SNAT verwendeten Ports als SNAT-Ports bezeichnet.

SNAT-Ports werden vorab zugeordnet, wie im Abschnitt [Grundlagen von SNAT und PAT](#snat) beschrieben. Es handelt sich um eine begrenzte Ressource, die überlastet werden kann. Es ist wichtig zu verstehen, wie sie [genutzt](#pat) wird. Um zu verstehen, wie Sie diese Nutzung beim Entwurf berücksichtigen und je nach Bedarf Abhilfemaßnahmen schaffen können, lesen Sie den Abschnitt [Verwalten der SNAT-Auslastung](../load-balancer/troubleshoot-outbound-connection.md#snatexhaust).

Wenn [Load Balancer Basic mehrere öffentliche IP-Adressen zugeordnet sind](load-balancer-multivip-overview.md), sind diese öffentlichen IP-Adressen Kandidaten für ausgehende Datenflüsse, und eine davon wird nach dem Zufallsprinzip ausgewählt.  

Zum Überwachen der Integrität ausgehender Verbindungen mit Load Balancer Basic können Sie [Azure Monitor-Protokolle für Azure Load Balancer](load-balancer-monitor-log.md) und [Warnungsereignisprotokolle](load-balancer-monitor-log.md#alert-event-log) verwenden.

### <a name="scenario-3-standalone-vm-without-a-public-ip-address"></a><a name="defaultsnat"></a>Szenario 3: Eigenständiger virtueller Computer ohne öffentliche IP-Adresse

In diesem Szenario gehört der virtuelle Computer nicht zu einem öffentlichen Load Balancer-Pool (und sie ist nicht Teil eines internen Load Balancer Standard-Pools), und ihm ist keine öffentliche IP-Adresse zugewiesen. Wenn der virtuelle Computer einen ausgehenden Datenfluss einleitet, übersetzt Azure die private IP-Quelladresse für den ausgehenden Datenfluss in eine öffentliche IP-Quelladresse. Die für diesen ausgehenden Datenfluss verwendete öffentliche IP-Adresse ist nicht konfigurierbar und wird nicht auf die Ressourcengrenze des Abonnements für öffentliche IP-Adressen angerechnet. Diese öffentliche IP-Adresse gehört Ihnen nicht und kann nicht reserviert werden. Bei der erneuten Bereitstellung der VM, Verfügbarkeitsgruppe oder VM-Skalierungsgruppe wird diese öffentliche IP-Adresse freigegeben und eine neue öffentliche IP-Adresse angefordert. Verwenden Sie dieses Szenario nicht für die Aufnahme von IP-Adressen in die Whitelist. Verwenden Sie stattdessen eins der anderen zwei Szenarien, in denen Sie das ausgehende Szenario und die für ausgehende Verbindungen zu verwendende IP-Adresse explizit deklarieren.

>[!IMPORTANT] 
>Dieses Szenario gilt auch, wenn __nur__ ein interner Basic Load Balancer verknüpft ist. Szenario 3 ist __nicht verfügbar__, wenn ein interner Standard Load Balancer mit einer VM verknüpft ist.  Sie müssen [Szenario 1](#ilpip) oder [Szenario 2](#lb) zusätzlich zu einem internen Standard Load Balancer explizit erstellen.

Azure verwendet SNAT mit Portmaskierung ([PAT](#pat)) für diese Aufgabe. Dieses Szenario ähnelt [Szenario 2](#lb). Der Unterschied besteht darin, dass keine Steuerung der IP-Adresse verwendet wird. Dies ist ein Fallbackszenario für den Fall, dass Szenario 1 und Szenario 2 nicht vorhanden sind. Dieses Szenario wird nicht empfohlen, wenn eine Kontrolle über die ausgehende Adresse gewünscht ist. Wenn ausgehende Verbindungen ein wichtiger Teil Ihrer Anwendung sind, sollten Sie ein anderes Szenario auswählen.

SNAT-Ports werden vorab zugeordnet, wie im Abschnitt [Grundlagen von SNAT und PAT](#snat) beschrieben.  Die Anzahl der VMs, die sich eine Verfügbarkeitsgruppe teilen, bestimmt, welche Stufe der Vorabzuordnung gilt.  Eine eigenständige VM ohne Verfügbarkeitsgruppe ist effektiv ein Pool von 1 für die Ermittlung der Vorabzuordnung (1024 SNAT-Ports). SNAT Ports sind begrenzte Ressourcen, die sich erschöpfen können. Es ist wichtig zu verstehen, wie sie [genutzt](#pat) wird. Um zu verstehen, wie Sie diese Nutzung beim Entwurf berücksichtigen und je nach Bedarf Abhilfemaßnahmen schaffen können, lesen Sie den Abschnitt [Verwalten der SNAT-Auslastung](../load-balancer/troubleshoot-outbound-connection.md#snatexhaust).

### <a name="multiple-combined-scenarios"></a><a name="combinations"></a>Mehrere kombinierte Szenarien

Sie können die in den vorherigen Abschnitten beschriebenen Szenarien kombinieren, um ein bestimmtes Ergebnis zu erzielen. Wenn mehrere Szenarien vorhanden sind, gilt eine Prioritätsreihenfolge: [Szenario 1](#ilpip) hat Vorrang vor [Szenario 2](#lb) und [3](#defaultsnat). [Szenario 2](#lb) überschreibt [Szenario 3](#defaultsnat).

Ein Beispiel hierfür ist eine Azure Resource Manager-Bereitstellung, bei der die Anwendung stark von ausgehenden Verbindungen mit einer begrenzten Anzahl von Zielen abhängig ist, aber auch eingehende Datenflüsse über ein Front-End des Lastenausgleichs empfängt. In diesem Fall können Sie als Lösung die Szenarien 1 und 2 kombinieren. Informationen zu zusätzlichen Mustern finden Sie unter [Verwalten der SNAT-Auslastung](../load-balancer/troubleshoot-outbound-connection.md#snatexhaust).

### <a name="multiple-frontends-for-outbound-flows"></a><a name="multife"></a>Mehrere Front-Ends für ausgehende Datenflüsse

#### <a name="standard-load-balancer"></a>Load Balancer Standard

Load Balancer Standard verwendet alle Kandidaten für ausgehende Datenflüsse, wenn gleichzeitig [mehrere (öffentliche) IP-Front-Ends](load-balancer-multivip-overview.md) vorhanden sind. Jedes Front-End multipliziert die Anzahl von verfügbaren, vorab zugeordneten SNAT-Ports, wenn eine Lastenausgleichsregel für ausgehende Verbindungen aktiviert ist.

Sie können die Verwendung einer Front-End-IP-Adresse für ausgehende Verbindungen mit einer neuen Option für Lastausgleichsregeln unterdrücken:

```json    
      "loadBalancingRules": [
        {
          "disableOutboundSnat": false
        }
      ]
```

Standardmäßig ist die Option `disableOutboundSnat` auf _false_ festgelegt. Dies bedeutet, dass diese Regel den ausgehenden SNAT für die zugehörigen VMs im Back-End-Pool der Lastenausgleichsregel programmiert. `disableOutboundSnat` kann auf _true_ festgelegt werden, um zu verhindern, dass für den Lastenausgleich die zugehörige Front-End-IP-Adresse für ausgehende Verbindungen der VMs im Back-End-Pool dieser Lastenausgleichsregel verwendet wird.  Zudem können Sie noch immer für ausgehende Datenflüsse eine bestimmte IP-Adresse festlegen, wie unter [Mehrere kombinierte Szenarien](#combinations) beschrieben.

#### <a name="load-balancer-basic"></a>Load Balancer Basic

Load Balancer Basic wählt ein einzelnes Front-End für ausgehende Datenflüsse aus, wenn [mehrere (öffentliche) Front-End-IP-Adressen](load-balancer-multivip-overview.md) Kandidaten für ausgehende Datenflüsse sind. Diese Auswahl ist nicht konfigurierbar; Sie sollten den Auswahlalgorithmus als zufällig betrachten. Sie können für ausgehende Datenflüsse eine bestimmte IP-Adresse festlegen, wie unter [Mehrere kombinierte Szenarien](#combinations) beschrieben.

### <a name="availability-zones"></a><a name="az"></a>Verfügbarkeitszonen

Bei Verwendung von [Standard Load Balancer mit Verfügbarkeitszonen](load-balancer-standard-availability-zones.md) können zonenredundante ausgehende SNAT-Verbindungen von zonenredundanten Front-Ends bereitgestellt werden. Die SNAT-Programmierung ist nach Zonenausfällen noch immer vorhanden.  Wenn zonenbezogene Front-Ends verwendet werden, geschieht mit ausgehenden SNAT-Verbindungen dasselbe wie mit der Zone, zu der sie gehören.

## <a name="understanding-snat-and-pat"></a><a name="snat"></a>Grundlagen von SNAT und PAT

### <a name="port-masquerading-snat-pat"></a><a name="pat"></a>Portmaskierung mit SNAT (PAT)

Wenn eine öffentliche Load Balancer-Ressource VM-Instanzen zugeordnet wird, die über keine dedizierte öffentliche IP-Adresse verfügen, werden alle ausgehenden Verbindungsquellen umgeschrieben. Die Quelle wird aus dem privaten IP-Adressraum des virtuellen Netzwerks in die öffentliche Front-End-IP-Adresse des Lastenausgleichs umgeschrieben. Im öffentlichen IP-Adressraum müssen die fünf Tupel des Datenflusses (IP-Quelladresse, Quellport, IP-Transportprotokoll, IP-Zieladresse, Zielport) eindeutig sein. Portmaskierung mit SNAT kann mit entweder mit dem TCP- oder UDP IP-Protokoll verwendet werden.

Hierfür werden kurzlebige Ports (SNAT-Ports) verwendet, nachdem die private IP-Quelladresse umgeschrieben wurde, da mehrere Datenflüsse von einer einzelnen öffentlichen IP-Adresse stammen. Die Portmaskierung mit SNAT-Algorithmus weist SNAT-Ports für UDP und TCP unterschiedlich zu.

#### <a name="tcp-snat-ports"></a><a name="tcp"></a>TCP-SNAT-Ports

Pro Datenfluss zu einer einzelnen IP-Zieladresse/einem einzelnen Port wird ein SNAT-Port genutzt. Bei mehreren TCP-Datenflüssen zur gleichen IP-Zieladresse bzw. zum Port und Protokoll belegt jeder TCP-Datenfluss einen einzelnen SNAT-Port. Hierdurch wird sichergestellt, dass die Datenflüsse eindeutig sind, wenn sie von der gleichen öffentlichen IP-Adresse stammen und die gleiche IP-Zieladresse, den gleichen Port und das gleiche Protokoll aufweisen. 

Mehrere Datenflüsse mit jeweils anderen Angaben für IP-Zieladresse, Port und Protokoll verwenden gemeinsam einen einzelnen SNAT-Port. Die IP-Adresse, der Port und das Protokoll für das Ziel machen den Datenfluss eindeutig, ohne dass zusätzliche Quellports verwendet werden müssen, um Datenflüsse im öffentlichen IP-Adressraum zu unterscheiden.

#### <a name="udp-snat-ports"></a><a name="udp"></a> UDP-SNAT-Ports

UDP-SNAT-Ports werden von einem anderen Algorithmus als TCP-SNAT-Ports verwaltet.  Load Balancer verwendet für UDP einen Algorithmus, der als „portbeschränktes Cone-NAT“ bezeichnet wird.  Für jeden Datenfluss wird unabhängig von der IP-Zieladresse/dem Zielport ein SNAT-Port genutzt.

#### <a name="snat-port-reuse"></a>Wiederverwendung von SNAT-Ports

Nach der Freigabe eines Ports kann er bei Bedarf erneut verwendet werden.  Sie können sich SNAT-Ports als eine Sequenz verfügbarer Ports (vom niedrigsten bis zum höchsten) vorstellen, die für ein bestimmtes Szenario verfügbar sind. Der erste verfügbare SNAT-Port wird für neue Verbindungen verwendet. 
 
#### <a name="exhaustion"></a>Auslastung

Sobald die SNAT-Portressourcen erschöpft sind, sind ausgehende Datenflüsse erst wieder möglich, wenn SNAT-Ports von vorhandenen Datenflüssen freigegeben werden. Der Load Balancer gibt die SNAT-Ports wieder frei, wenn der Datenflussvorgang abgeschlossen ist. Es wird ein [Leerlauftimeout von 4 Minuten](../load-balancer/troubleshoot-outbound-connection.md#idletimeout) verwendet, um SNAT-Ports für im Leerlauf befindliche Datenflüsse wieder freizugeben.

UDP-SNAT-Ports erschöpfen aufgrund des Unterschieds im verwendeten Algorithmus in der Regel viel schneller als TCP-SNAT-Ports. Diesen Unterschied müssen Sie beim Entwerfen und Skalieren berücksichtigen.

Im Abschnitt [Verwalten von SNAT](../load-balancer/troubleshoot-outbound-connection.md#snatexhaust) werden Muster für das Entschärfen von Bedingungen beschrieben, die häufig zu einer Überlastung von SNAT-Ports führen.

### <a name="ephemeral-port-preallocation-for-port-masquerading-snat-pat"></a><a name="preallocatedports"></a>Vorabzuordnung von kurzlebigen Ports für die Portmaskierung per SNAT (PAT)

In Azure wird ein Algorithmus verwendet, um basierend auf der Größe des Back-End-Pools bei der Portmaskierung per SNAT ([PAT](#pat)) die Anzahl von verfügbaren vorab zugeordneten SNAT-Ports zu ermitteln. SNAT-Ports sind kurzlebige Ports, die für eine bestimmte öffentliche IP-Quelladresse verfügbar sind. Für alle öffentlichen IP-Adressen, die einem Lastenausgleich zugeordnet sind, stehen 64.000 Ports als SNAT-Ports für jedes IP-Transportprotokoll zur Verfügung.

Dieselbe Anzahl von SNAT-Ports wird vorab für UDP bzw. TCP zugeordnet und unabhängig voneinander pro IP-Transportprotokoll genutzt.  Die Verwendung des SNAT-Ports unterscheidet sich jedoch abhängig davon, ob es sich um einen UDP- oder TCP-Datenfluss handelt.

>[!IMPORTANT]
>Standard-SKU-SNAT Programmierung erfolgt pro IP-Transportprotokoll und wird aus der Lastenausgleichsregel abgeleitet.  Ist nur eine TCP-Lastenausgleichsregel vorhanden, ist SNAT nur für TCP verfügbar. Wenn Sie nur eine TCP-Lastenausgleichsregel haben und ausgehendes SNAT für UDP benötigen, erstellen Sie eine UDP-Lastenausgleichsregel vom selben Front-End- zum selben Back-End-Pool.  Dadurch wird SNAT-Programmierung für UDP ausgelöst.  Eine Arbeitsregel oder ein Integritätstest ist nicht erforderlich.  Basic-SKU-SNAT bewirkt immer, dass SNAT für beide IP-Transportprotokolle programmiert wird, unabhängig von dem Transportprotokoll, das in der Lastenausgleichsregel angegeben ist.

In Azure werden SNAT-Ports vorab der IP-Konfiguration der NIC jeder VM zugeordnet. Wenn dem Pool eine IP-Konfiguration hinzugefügt wird, werden die SNAT-Ports für diese IP-Konfiguration basierend auf der Größe des Back-End-Pools vorab zugeordnet. Bei Erstellung von ausgehenden Datenflüssen werden diese Ports von [PAT](#pat) dynamisch genutzt (bis zum vorab festgelegten Grenzwert) und wieder freigegeben, wenn der Datenfluss geschlossen wird oder ein [Leerlauftimeout](../load-balancer/troubleshoot-outbound-connection.md#idletimeout) eintritt.

In der folgenden Tabelle sind die SNAT-Port-Vorabzuordnungen für die Ebenen der Back-End-Poolgrößen angegeben:

| Poolgröße (VM-Instanzen) | Vorab zugeordnete SNAT-Ports pro IP-Konfiguration|
| --- | --- |
| 1-50 | 1\.024 |
| 51-100 | 512 |
| 101-200 | 256 |
| 201-400 | 128 |
| 401-800 | 64 |
| 801-1.000 | 32 |

>[!NOTE]
> Bei Verwendung von Load Balancer Standard mit [mehreren Front-Ends](load-balancer-multivip-overview.md) multipliziert jede Front-End-IP-Adresse die Anzahl der verfügbaren SNAT-Ports in der vorherigen Tabelle. Beispielsweise verwendet ein Back-End-Pool von 50 VMs mit zwei Lastenausgleichsregeln, jede mit einer eigenen Front-End-IP-Adresse, 2.048 (2 x 1.024) SNAT-Ports pro Regel. Weitere Informationen finden Sie in den Details zu [mehreren Front-Ends](#multife).

Denken Sie daran, dass die Anzahl der verfügbaren SNAT-Ports nicht direkt in die Anzahl der Datenflüsse zu übersetzen ist. Ein einzelner SNAT-Port kann für mehrere eindeutige Ziele wiederverwendet werden. Ports werden nur genutzt, wenn Datenflüsse eindeutig gemacht werden müssen. Eine Anleitung für den Entwurf und Lösungsmöglichkeiten finden Sie in den Abschnitten, in den die [Verwaltung dieser begrenzten Ressource](../load-balancer/troubleshoot-outbound-connection.md#snatexhaust) und [PAT](#pat) beschrieben sind.

Von einer Größenänderung Ihres Back-End-Pools können einige der eingerichteten Datenflüsse betroffen sein. Wenn die Größe des Back-End-Pools zunimmt und der Übergang auf die nächste Ebene erfolgt, wird die Hälfte der vorab zugeordneten SNAT-Ports während des Übergangs zur nächstgrößeren Back-End-Poolebene freigegeben. Bei Datenflüssen, die einem wieder freigegebenen SNAT-Port zugeordnet sind, tritt ein Timeout auf, und sie müssen neu eingerichtet werden. Versuche zum Einrichten eines neuen Datenflusses sind sofort erfolgreich, solange vorab zugeordnete Ports verfügbar sind.

Wenn sich die Größe des Back-End-Pools reduziert und der Übergang auf eine niedrigere Ebene erfolgt, nimmt die Anzahl der verfügbaren SNAT-Ports zu. In diesem Fall sind vorhandene zugeordnete SNAT-Ports und die entsprechenden Datenflüsse nicht betroffen.

SNAT-Portzuordnungen gelten speziell für das jeweilige IP-Transportprotokoll (TCP und UDP werden separat verwaltet) und werden unter den folgenden Bedingungen freigegeben:

### <a name="tcp-snat-port-release"></a>TCP-SNAT-Portfreigabe

- Wenn entweder der Server oder der Client FINACK sendet, wird der SNAT-Port nach 240 Sekunden freigegeben.
- Tritt ein RST auf, wird der SNAT-Port nach 15 Sekunden freigegeben.
- Ist das Leerlauftimeout erreicht, wird der Port freigegeben.

### <a name="udp-snat-port-release"></a>UDP-SNAT-Portfreigabe

- Ist das Leerlauftimeout erreicht, wird der Port freigegeben.

## <a name="discovering-the-public-ip-that-a-vm-uses"></a><a name="discoveroutbound"></a>Ermitteln der von einem virtuellen Computer verwendeten öffentlichen IP-Adresse
Es gibt viele Möglichkeiten, die öffentliche IP-Quelladresse einer ausgehenden Verbindung zu bestimmen. OpenDNS bietet einen Dienst, der die öffentliche IP-Adresse Ihrer VM anzeigen kann. 

Mit dem Befehl „nslookup“ können Sie eine DNS-Abfrage für den Namen „myip.opendns.com“ an den OpenDNS-Resolver senden. Der Dienst gibt die IP-Quelladresse zurück, die zum Senden der Abfrage verwendet wurde. Wenn Sie die folgende Abfrage auf Ihrem virtuellen Computer ausführen, wird die öffentliche IP-Adresse zurückgegeben, die für diesen virtuellen Computer verwendet wird:

    nslookup myip.opendns.com resolver1.opendns.com

## <a name="preventing-outbound-connectivity"></a><a name="preventoutbound"></a>Verhindern der Konnektivität in ausgehender Richtung
Manchmal ist es nicht wünschenswert, einem virtuellen Computer das Erstellen eines ausgehenden Datenflusses zu erlauben. Möglicherweise müssen Sie auch verwalten, welche Ziele mit ausgehenden Datenflüssen erreicht werden bzw. welche Ziele eingehende Datenflüsse beginnen können. In diesem Fall können Sie [Netzwerksicherheitsgruppen](../virtual-network/security-overview.md) zum Verwalten der vom virtuellen Computer erreichbaren Ziele verwenden. Mit Netzwerksicherheitsgruppen können Sie auch verwalten, welches öffentliche Ziel eingehende Datenflüsse initiieren kann.

Wenn Sie eine Netzwerksicherheitsgruppe einem virtuellen Computer mit Lastenausgleich zuordnen, müssen Sie auf die [Diensttags](../virtual-network/security-overview.md#service-tags) und [Standardsicherheitsregeln](../virtual-network/security-overview.md#default-security-rules) achten. Sie müssen sicherstellen, dass die VM Anforderungen von Integritätstests von Azure Load Balancer empfangen kann. 

Wenn eine Netzwerksicherheitsgruppe Anforderungen von Integritätstests vom Standardtag AZURE_LOADBALANCER blockiert, misslingt Ihr VM-Integritätstests, weshalb die VM mit „Außer Betrieb“ markiert wird. Der Lastenausgleich beendet das Senden neuer Datenflüsse an diese VM.

## <a name="connections-to-azure-storage-in-the-same-region"></a>Verbindungen mit Azure Storage in derselben Region

Eine ausgehende Konnektivität mithilfe der oben genannten Szenarios ist nicht erforderlich, um eine Verbindung mit Azure Storage in derselben Region wie die VM herzustellen. Alternativ können Sie NSGs (Netzwerksicherheitsgruppen) verwenden, wenn Sie dies nicht möchten. Für Verbindungen mit Azure Storage in anderen Regionen ist die ausgehende Konnektivität erforderlich. Beachten Sie, dass es sich bei der Quell-IP-Adresse in den Azure Storage-Diagnoseprotokollen um eine interne Anbieteradresse handelt und nicht um die öffentliche IP-Adresse Ihrer VM, wenn Sie eine Verbindung mit Azure Storage über eine VM in derselben Region herstellen. Wenn Sie den Zugriff auf Ihr Azure Storage-Konto für VMs in mindestens einem Subnetz des virtuellen Netzwerks innerhalb derselben Region einschränken möchten, sollten Sie [VNET-Dienstendpunkte](../virtual-network/virtual-network-service-endpoints-overview.md) anstelle Ihrer öffentlichen IP-Adresse verwenden, wenn Sie die Firewall für Ihr Speicherkonto konfigurieren. Sobald die Dienstendpunkte konfiguriert wurden, wird Ihre private VNET-IP-Adresse in Ihren Azure Storage-Diagnoseprotokollen anstelle der internen Anbieteradresse angezeigt.

## <a name="azure-load-balancer-outbound-rules"></a><a name="outboundrules"></a>Azure Load Balancer-Ausgangsregeln

Azure Load Balancer unterstützt zusätzlich zu eingehenden Verbindungen auch ausgehende Verbindungen eines virtuellen Netzwerks.  Mit Ausgangsregeln können Sie die Netzwerkadressenübersetzung (NAT) einer öffentlichen [Load Balancer Standard](load-balancer-standard-overview.md)-Instanz für ausgehenden Datenverkehr ganz einfach konfigurieren.  Sie verfügen über die vollständige deklarative Steuerung über die ausgehenden Verbindungen, um diese Funktionalität nach Belieben zu skalieren und anzupassen.

![Load Balancer-Ausgangsregeln](media/load-balancer-outbound-rules-overview/load-balancer-outbound-rules.png)

Mithilfe von Ausgangsregeln können Sie mit Load Balancer: 
- Die NAT für ausgehenden Datenverkehr von Grund auf neu definieren
- Das Verhalten vorhandener NAT für ausgehenden Datenverkehr skalieren und optimieren 

Mit Ausgangsregeln können Sie steuern:
- Welchen IP-Adressen von VMs in welche öffentlichen IP-Adressen übersetzt werden 
- Wie [ausgehende SNAT-Ports](load-balancer-outbound-connections.md#snat) zugeordnet werden
- Welche Protokolle zum Übersetzen ausgehenden Datenverkehrs verwendet werden
- Wie lange das Leerlauftimeout für ausgehende Verbindungen dauert (4-120 Minuten)
- Ob eine TCP-Zurücksetzung bei Leerlauftimeout gesendet wird

Ausgangsregeln erweitern das in Artikel [Ausgehende Verbindungen](load-balancer-outbound-connections.md) beschriebene [Szenario 2](load-balancer-outbound-connections.md#lb). Die Rangfolge der Szenarien bleibt unverändert.

### <a name="outbound-rule"></a>Ausgehende Regel

Wie alle Load Balancer-Regeln folgen auch die Ausgangsregeln der gleichen vertrauten Syntax wie Lastenausgleichsregeln und NAT-Eingangsregeln:

**Front-End** + **Parameter** + **Back-End-Pool**

Eine Ausgangsregel konfiguriert die NAT für ausgehenden Datenverkehr für _alle VMs, die vom Back-End-Pool_ für die _Front-End_-Übersetzung identifiziert wurden.  _Parameter_ ermöglichen eine zusätzliche differenzierte Steuerung des NAT-Algorithmus für ausgehenden Datenverkehr.

Die API-Version „2018-07-01“ lässt eine Ausgangsregeldefinition mit der folgenden Struktur zu:

```json
      "outboundRules": [
        {
          "frontendIPConfigurations": [ list_of_frontend_ip_configuations ],
          "allocatedOutboundPorts": number_of_SNAT_ports,
          "idleTimeoutInMinutes": 4 through 66,
          "enableTcpReset": true | false,
          "protocol": "Tcp" | "Udp" | "All",
          "backendAddressPool": backend_pool_reference,
        }
      ]
```

>[!NOTE]
>Die effektive NAT-Konfiguration für ausgehenden Datenverkehr ist eine Kombination aller Ausgangs- und Lastenausgleichsregeln. Ausgangsregeln verhalten sich inkrementell zu Lastenausgleichsregeln. In Artikel [Deaktivieren von NAT für ausgehenden Datenverkehr für eine Lastenausgleichsregel](#disablesnat) finden Sie weitere nützliche Informationen, um die effektive NAT für ausgehenden Datenverkehr zu verwalten, wenn mehrere Regeln für eine VM gelten. Sie müssen [SNAT für ausgehenden Datenverkehr deaktivieren](#disablesnat), wenn Sie eine Ausgangsregel definieren, die die gleiche öffentliche IP-Adresse wie eine Lastenausgleichsregel verwendet.

#### <a name="scale-outbound-nat-with-multiple-ip-addresses"></a><a name="scale"></a> Skalieren der NAT für ausgehenden Datenverkehr mit mehreren IP-Adressen

Ausgangsregeln können jeweils mit nur einer einzigen öffentlichen IP-Adresse verwendet werden und erleichtern die Konfiguration beim Skalieren der NAT für ausgehenden Datenverkehr. Sie können mehrere IP-Adressen verwenden, um umfangreiche Szenarien zu planen. Mithilfe von Ausgangsregeln lassen sich außerdem die für die [SNAT-Überlastung](troubleshoot-outbound-connection.md#snatexhaust) anfälligen Muster reduzieren.  

Jede zusätzliche IP-Adresse, die von einem Front-End bereitgestellt wird, stellt 64.000 kurzlebige Ports zur Verfügung, die Load Balancer als SNAT-Ports verwenden kann. Beim Lastenausgleich und bei NAT-Eingangsregeln steht ein einziges Front-End zur Verfügung. Die Ausgangsregel erweitert das Front-End-Konzept und lässt mehrere Front-Ends pro Regel zu.  Bei mehreren Front-Ends pro Regel wird die Anzahl von verfügbaren SNAT-Ports mit jeder öffentlichen IP-Adresse multipliziert. So können auch umfangreiche Szenarien unterstützt werden.

Darüber hinaus können Sie ein [Präfix für öffentliche IP-Adressen](https://aka.ms/lbpublicipprefix) direkt mit einer Ausgangsregel verwenden.  Durch die Verwendung von öffentlichen IP-Präfixen ist es einfacher, Datenflüsse aus Ihrer Azure-Bereitstellung zu skalieren und auf die Whitelist zu setzen. Sie können eine Front-End-IP-Konfiguration innerhalb der Load Balancer-Ressource konfigurieren, um direkt auf den Präfix einer öffentlichen IP-Adresse zu verweisen.  Dies ermöglicht Load Balancer die exklusive Steuerung über das Präfix für öffentliche IP-Adressen, und die Ausgangsregel verwendet automatisch alle öffentlichen IP-Adressen, die im Präfix für öffentliche IP-Adressen für ausgehende Verbindungen enthalten sind.  Jede IP-Adresse im Präfixbereich für öffentliche IP-Adressen bietet 64.000 kurzlebige Ports pro IP-Adresse, die Load Balancer als SNAT-Ports verwenden kann.   

Wenn Sie diese Option nutzen, können Sie keine einzelnen Ressourcen für öffentliche IP-Adressen aus dem Präfix für öffentliche IP-Adressen erstellen, da die Ausgangsregel die vollständige Kontrolle über das Präfix haben muss.  Wenn Sie eine differenziertere Steuerung wünschen, können Sie aus dem Präfix für öffentliche IP-Adressen eine individuelle öffentliche IP-Adressressource erstellen und mehrere öffentliche IP-Adressen einzeln dem Front-End einer Ausgangsregel zuweisen.

#### <a name="tune-snat-port-allocation"></a><a name="snatports"></a> Anpassen der SNAT-Portzuordnung

Sie können Ausgangsregeln verwenden, um die [automatische SNAT-Portzuordnung basierend auf der Back-End-Poolgröße](load-balancer-outbound-connections.md#preallocatedports) anzupassen und mehr oder weniger Ressourcen zuzuweisen, als die automatische SNAT-Portzuordnung bereitstellt.

Verwenden Sie den folgenden Parameter, um 10.000 SNAT-Ports pro VM zuzuordnen (NIC-IP-Konfiguration).
 

          "allocatedOutboundPorts": 10000

Jede öffentliche IP-Adresse aller Front-Ends einer Ausgangsregel stellt bis zu 64.000 kurzlebige Ports als SNAT-Ports bereit.  Load Balancer weist SNAT-Ports als ein Vielfaches von 8 zu. Wenn Sie einen Wert angeben, der nicht durch 8 teilbar ist, wird der Konfigurationsvorgang abgelehnt.  Wenn Sie versuchen, mehr SNAT-Ports zuzuordnen als öffentliche IP-Adressen vorhanden sind, wird der Konfigurationsvorgang abgelehnt.  Wenn Sie beispielsweise 10.000 Ports pro VM zuweisen und 7 VMs in einem Back-End-Pool eine einzige öffentliche IP-Adresse teilen würden, wird die Konfiguration zurückgewiesen (7 x 10.000 SNAT-Ports > 64.000 SNAT-Ports).  Dem Front-End der Ausgangsregel lassen sich weitere öffentliche IP-Adressen hinzufügen, um das Szenario zu ermöglichen.

Sie können die [automatische SNAT-Portzuordnung basierend auf der Back-End-Poolgröße](load-balancer-outbound-connections.md#preallocatedports) wiederherstellen, indem Sie als Portanzahl „0“ angeben. In diesem Fall erhalten die ersten 50 VM-Instanzen 1024 Ports. Die VM-Instanzen von 51-100 erhalten 512 usw. entsprechend der Tabelle.

#### <a name="control-outbound-flow-idle-timeout"></a><a name="idletimeout"></a> Steuern des Leerlauftimeouts für ausgehenden Datenfluss

Ausgangsregeln stellen einen Konfigurationsparameter bereit, um das Leerlauftimeout für den ausgehenden Datenfluss zu steuern und ihn an die Anforderungen Ihrer Anwendung anzupassen.  Das Leerlauftimeout für ausgehenden Datenverkehr tritt standardmäßig nach 4 Minuten ein.  Der Parameter akzeptiert einen Wert von 4 bis 120, um die Minutenanzahl für das Leerlauftimeout für Datenflüsse festzulegen, die dieser speziellen Regel entsprechen.

Verwenden Sie den folgenden Parameter, um dieses Leerlauftimeout auf 1 Stunde festzulegen:

          "idleTimeoutInMinutes": 60

#### <a name="enable-tcp-reset-on-idle-timeout"></a><a name="tcprst"></a> <a name="tcpreset"></a> Aktivieren der TCP-Zurücksetzung bei Leerlauftimeout

Das Standardverhalten von Load Balancer ist es, den Datenfluss allmählich zu entfernen, wenn das Leerlauftimeout für ausgehenden Datenverkehr erreicht wurde.  Mit dem Parameter „enableTCPReset“ können Sie ein besser vorhersagbares Anwendungsverhalten aktivieren und steuern, ob bidirektionale TCP-Zurücksetzung (TCP RST) außerhalb des Leerlauftimeouts für ausgehenden Datenverkehr gesendet wird. 

Verwenden Sie den folgenden Parameter, um die TCP-Zurücksetzung für eine Ausgangsregel zu aktivieren:

           "enableTcpReset": true

Unter [TCP-Zurücksetzung bei Leerlauftimeout](https://aka.ms/lbtcpreset) finden Sie weitere Informationen, unter anderem zur regionalen Verfügbarkeit.

#### <a name="support-both-tcp-and-udp-transport-protocols-with-a-single-rule"></a><a name="proto"></a> Unterstützen von TCP- und UDP-Transportprotokollen mit einer einzigen Regel

Sie werden wahrscheinlich die Option „Alle“ für das Ausgangsregel-Transportprotokoll verwenden wollen, doch Sie können die Ausgangsregel auch auf ein bestimmtes Transportprotokoll anwenden.

Verwenden Sie den folgenden Parameter, um das Protokoll auf TCP und UDP festzulegen:

          "protocol": "All"

#### <a name="disable-outbound-nat-for-a-load-balancing-rule"></a><a name="disablesnat"></a> Deaktivieren der NAT für ausgehenden Datenverkehr für eine Lastenausgleichsregel

Wie bereits erwähnt, bieten Lastenausgleichsregeln eine automatische Programmierung der NAT für ausgehenden Datenverkehr. In einigen Szenarien müssen Sie jedoch die automatische Programmierung der NAT für ausgehenden Datenverkehr durch die Lastenausgleichsregel deaktivieren, um das Verhalten zu steuern oder zu verfeinern.  Ausgangsregeln umfassen Szenarien, in denen es wichtig ist, die automatische Programmierung der NAT für ausgehenden Datenverkehr zu stoppen.

Sie haben zwei Möglichkeiten, diesen Parameter zu verwenden:
- Sie können das Verwenden der eingehenden IP-Adresse für die NAT für ausgehenden Datenverkehr unterdrücken.  Ausgangsregeln verhalten sich inkrementell zu Lastenausgleichsregeln. Wenn dieser Parameter festgelegt ist, übernimmt die Ausgangsregel die Kontrolle.
  
- Passen Sie die NAT-Parameter für ausgehenden Datenverkehr einer IP-Adresse an, die zugleich für eingehenden und ausgehenden Datenverkehr verwendet wird.  Die automatische NAT-Programmierung für ausgehenden Datenverkehr muss deaktiviert werden, damit eine Ausgangsregel die Kontrolle übernehmen kann.  Um beispielsweise die SNAT-Portzuordnung einer Adresse zu ändern, die auch für den eingehenden Datenverkehr verwendet wird, muss dieser Parameter TRUE entsprechen.  Wenn Sie versuchen, mit einer Ausgangsregel die Parameter einer IP-Adresse neu zu definieren, die auch für eingehenden Datenverkehr verwendet wird, und die NAT-Programmierung für ausgehenden Datenverkehr für die Lastenausgleichsregel nicht freigegeben wurde, schlägt die Konfiguration einer Ausgangsregel fehl.

>[!IMPORTANT]
> Ihre VM hat keine ausgehenden Verbindungen, wenn Sie diesen Parameter auf TRUE setzen und keine Ausgangsregel (oder kein [Szenario mit einer öffentlichen IP-Adresse auf Instanzebene](load-balancer-outbound-connections.md#ilpip)) zum Definieren ausgehender Verbindungen haben.  Einige Vorgänge Ihrer VM oder Ihrer Anwendung können davon abhängen, ob ausgehende Verbindungen verfügbar sind. Stellen Sie sicher, dass Sie die Abhängigkeiten Ihres Szenarios verstehen und die Auswirkungen von Änderungen berücksichtigt haben.

Mit diesem Konfigurationsparameter können Sie SNAT für ausgehenden Datenverkehr für die Lastenausgleichsregel deaktivieren:

```json
      "loadBalancingRules": [
        {
          "disableOutboundSnat": true
        }
      ]
```

Der Parameter „disableOutboundSNAT“ **ist** standardmäßig auf FALSE eingestellt. Das bedeutet, dass die Lastenausgleichsregel automatisch NAT für ausgehenden Datenverkehr als Spiegelbild der Konfiguration der Lastenausgleichsregel bereitstellt.  

Wenn Sie „disableOutboundSnat“ in der Lastenausgleichsregel auf TRUE setzen, gibt die Lastenausgleichsregel die Kontrolle über die ansonsten automatische NAT-Programmierung für ausgehenden Datenverkehr frei.  SNAT für ausgehenden Datenverkehr als Ergebnis der Lastenausgleichsregel ist deaktiviert.

#### <a name="reuse-existing-or-define-new-backend-pools"></a>Wiederverwenden vorhandener oder Definieren neuer Back-End-Pools

Ausgangsregeln führen kein neues Konzept zum Definieren der VM-Gruppe ein, für die die Regel gelten soll.  Stattdessen wird das Konzept eines Back-End-Pools verwendet, das auch für Lastenausgleichsregeln gilt. So lässt sich die Konfiguration vereinfachen, indem eine vorhandene Back-End-Pooldefinition wiederverwendet oder eine neue speziell für eine Ausgangsregel erstellt wird.

### <a name="scenarios"></a>Szenarien

#### <a name="groom-outbound-connections-to-a-specific-set-of-public-ip-addresses"></a><a name="groom"></a> Konfigurieren ausgehender Verbindungen für einen bestimmten Satz öffentlicher IP-Adressen

Sie können eine Ausgangsregel verwenden, um ausgehende Verbindungen so zu konfigurieren, dass sie scheinbar von einem bestimmten Satz öffentlicher IP-Adressen stammen, um Whitelistszenarien zu vereinfachen.  Diese öffentliche Quell-IP-Adresse kann die gleiche sein, die von einer Lastenausgleichsregel verwendet wird, oder ein anderer Satz öffentlicher IP-Adressen, als der von einer Lastenausgleichsregel verwendete.  

1. Erstellen Sie ein [Präfix für öffentliche IP-Adressen](https://aka.ms/lbpublicipprefix) (oder öffentliche IP-Adressen aus einem Präfix für öffentliche IP-Adressen).
2. Erstellen einer öffentlichen Load Balancer Standard-Instanz
3. Erstellen Sie Front-Ends, die auf das gewünschte Präfix für öffentliche IP-Adressen (oder öffentliche IP-Adressen) verweisen.
4. Verwenden Sie einen vorhandenen Back-End-Pool erneut, oder erstellen Sie einen neuen Back-End-Pool, und stellen Sie die VMs in einem Back-End-Pool der öffentlichen Load Balancer-Instanz bereit.
5. Konfigurieren Sie eine Ausgangsregel in der öffentlichen Load Balancer-Instanz, um NAT für ausgehenden Datenverkehr für diese VMs über die Front-Ends zu programmieren.
   
Wenn die Lastenausgleichsregel nicht für den ausgehenden Datenverkehr verwendet werden soll, deaktivieren Sie [SNAT für ausgehenden Datenverkehr](#disablesnat) für die Lastenausgleichsregel.

#### <a name="modify-snat-port-allocation"></a><a name="modifysnat"></a> Ändern der SNAT-Portzuordnung

Sie können Ausgangsregeln verwenden, um die [automatische SNAT-Portzuweisung basierend auf der Back-End-Poolgröße](load-balancer-outbound-connections.md#preallocatedports) anzupassen.

Wenn Sie beispielsweise zwei VMs haben, die sich eine einzige öffentliche IP-Adresse für NAT für ausgehenden Datenverkehr teilen, können Sie im Fall einer SNAT-Überlastung die Anzahl von SNAT-Ports, die von den standardmäßigen 1.024 Ports zugeordnet wurden, erhöhen. Jede öffentliche IP-Adresse kann bis zu 64.000 kurzlebige Ports bereitstellen.  Wenn Sie eine Ausgangsregel mit einem einzigen öffentlichen IP-Adressen-Front-End konfigurieren, können Sie insgesamt 64.000 SNAT-Ports auf VMs im Back-End-Pool verteilen.  Bei zwei VMs können maximal 32.000 SNAT-Ports mit einer Ausgangsregel (2 x 32.000 = 64.000) zugeordnet werden.

Erfahren Sie mehr über [ausgehende Verbindungen](load-balancer-outbound-connections.md) und darüber, wie [SNAT](load-balancer-outbound-connections.md#snat)-Ports zugeordnet und verwendet werden.

#### <a name="enable-outbound-only"></a><a name="outboundonly"></a> Ausschließliches Aktivieren des ausgehenden Datenverkehrs

Sie können eine öffentliche Load Balancer Standard-Instanz verwenden, um NAT für ausgehenden Datenverkehr für eine Gruppe von VMs bereitzustellen. In diesem Szenario können Sie eine Ausgangsregel alleine ohne zusätzliche Regeln verwenden.

##### <a name="outbound-nat-for-vms-only-no-inbound"></a>NAT für ausgehenden Datenverkehr nur für VMs (kein eingehender Datenverkehr)

Definieren Sie eine öffentliche Load Balancer Standard-Instanz, stellen Sie die VMs im Back-End-Pool bereit, und konfigurieren Sie eine Ausgangsregel, um NAT für ausgehenden Datenverkehr zu programmieren und die ausgehenden Verbindungen so zu konfigurieren, dass sie von einer bestimmten öffentlichen IP-Adresse stammen. Sie können auch ein Präfix für öffentliche IP-Adressen verwenden. Das vereinfacht, die Quelle ausgehender Verbindungen auf die Whitelist zu setzen.

1. Erstellen Sie eine öffentliche Load Balancer Standard-Instanz.
2. Erstellen Sie einen Back-End-Pool, und stellen Sie die VMs in einem Back-End-Pool der öffentlichen Load Balancer-Instanz bereit.
3. Konfigurieren Sie eine Ausgangsregel in der öffentlichen Load Balancer-Instanz, um die NAT für ausgehenden Datenverkehr für diese VMs zu programmieren.

##### <a name="outbound-nat-for-internal-standard-load-balancer-scenarios"></a>NAT für ausgehenden Datenverkehr für interne Load Balancer Standard-Szenarien

Beim Verwenden einer internen Load Balancer Standardinstanz ist die NAT für ausgehenden Datenverkehr erst verfügbar, nachdem die ausgehende Konnektivität explizit deklariert wurde. Sie können die ausgehende Konnektivität mit einer Ausgangsregel definieren, indem Sie mithilfe dieser Schritte ausgehende Konnektivität für VMs hinter einer internen Load Balancer-Standardinstanz erstellen:

1. Erstellen Sie eine öffentliche Load Balancer Standard-Instanz.
2. Erstellen Sie einen Back-End-Pool, und stellen Sie die VMs über die interne Load Balancer-Instanz hinaus in einem Back-End-Pool der öffentlichen Load Balancer-Instanz bereit.
3. Konfigurieren Sie eine Ausgangsregel in der öffentlichen Load Balancer-Instanz, um die NAT für ausgehenden Datenverkehr für diese VMs zu programmieren.

##### <a name="enable-both-tcp--udp-protocols-for-outbound-nat-with-a-public-standard-load-balancer"></a>Aktivieren von TCP- und UDP-Protokollen für NAT für ausgehenden Datenverkehr mit einer öffentlichen Load Balancer Standard-Instanz

- Beim Verwenden einer öffentlichen Load Balancer Standard-Instanz entspricht die bereitgestellte automatische NAT-Programmierung für ausgehenden Datenverkehr dem Transportprotokoll der Lastenausgleichsregel.  

   1. Deaktivieren Sie SNAT für ausgehenden Datenverkehr in der Lastenausgleichsregel.
   2. Konfigurieren Sie eine Ausgangsregel in derselben Load Balancer-Instanz.
   3. Verwenden Sie erneut den Back-End-Pool, den Ihre VMs bereits verwenden.
   4. Geben Sie „Protokoll“: „Alle“ als Teil der Ausgangsregel an.

- Wenn nur NAT-Eingangsregeln verwendet werden, wird keine NAT für ausgehenden Datenverkehr bereitgestellt.

   1. Stellen Sie die VMs in einem Back-End-Pool bereit.
   2. Definieren Sie mindestens eine Front-End-IP-Konfiguration mit mindestens einer öffentlichen IP-Adresse oder einem Präfix für öffentliche IP-Adressen.
   3. Konfigurieren Sie eine Ausgangsregel in derselben Load Balancer-Instanz.
   4. Geben Sie „Protokoll“: „Alle“ als Teil der Ausgangsregel an.


## <a name="limitations"></a>Einschränkungen
- Die maximale Anzahl von verwendbaren kurzlebigen Ports pro Front-End-IP-Adresse beträgt 64.000.
- Das konfigurierbare Leerlauftimeout für ausgehenden Datenverkehr beträgt ist 4 bis 120 Minuten (240 bis 7200 Sekunden).
- Load Balancer unterstützt kein ICMP für die NAT ausgehenden Datenverkehrs.
- Ausgangsregeln können nur auf die primäre IP-Konfiguration einer NIC angewandt werden.  Es werden mehrere NICs unterstützt.
- Auf Web-Workerrollen ohne VNet und andere Plattformdienste von Microsoft kann aufgrund eines Nebeneffekts der Funktionsweise von Diensten vor VNet und anderen Plattformdiensten nur zugegriffen werden, wenn interner Standard-Load Balancer verwendet wird. Verlassen Sie sich nicht auf diesen Nebeneffekt, da der jeweilige Dienst oder die zugrunde liegende Plattform ohne vorherige Ankündigung geändert werden kann. Sie müssen immer davon ausgehen, dass Sie ausgehende Verbindungen, falls gewünscht, explizit erstellen müssen, wenn Sie nur einen internen Load Balancer im Tarif „Standard“ verwenden. Das in diesem Artikel beschriebene Szenario 3 für [Standard-SNAT](#defaultsnat) ist nicht verfügbar.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen finden Sie unter [Load Balancer Standard](load-balancer-standard-overview.md).
- Weitere Informationen zu [Ausgangsregeln](load-balancer-outbound-rules-overview.md) für Standard Public Load Balancer.
- Weitere Informationen zu [Load Balancer](load-balancer-overview.md).
- Weitere Informationen zu [Netzwerksicherheitsgruppen](../virtual-network/security-overview.md).
- Erfahren Sie mehr über die anderen zentralen [Netzwerkfunktionen](../networking/networking-overview.md) in Azure.
