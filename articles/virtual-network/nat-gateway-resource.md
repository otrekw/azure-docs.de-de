---
title: Entwerfen von virtuellen Netzwerken mit NAT-Gatewayressourcen
titleSuffix: Azure Virtual Network NAT
description: Es wird beschrieben, wie Sie virtuelle Netzwerke mit NAT-Gatewayressourcen entwerfen.
services: virtual-network
documentationcenter: na
author: asudbring
manager: KumudD
ms.service: virtual-network
Customer intent: As an IT administrator, I want to learn more about how to design virtual networks with NAT gateway resources.
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/24/2020
ms.author: allensu
ms.openlocfilehash: 9d9033689472fbc462eefd3d31c2b36a19add81a
ms.sourcegitcommit: 7f929a025ba0b26bf64a367eb6b1ada4042e72ed
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/25/2020
ms.locfileid: "77589469"
---
# <a name="designing-virtual-networks-with-nat-gateway-resources-public-preview"></a>Entwerfen von virtuellen Netzwerken mit NAT-Gatewayressourcen (Public Preview)

NAT-Gatewayressourcen sind Teil von [Virtual Network NAT](nat-overview.md) und stellen die Internetverbindung in ausgehender Richtung für ein oder mehrere Subnetze eines virtuellen Netzwerks bereit. Im Subnetz des virtuellen Netzwerks wird angegeben, welches NAT-Gateway verwendet wird. NAT ermöglicht die „Übersetzung der Quellnetzwerkadresse“ (Source Network Address Translation, SNAT) für ein Subnetz.  Mit NAT-Gatewayressourcen wird angegeben, welche statischen IP-Adressen von virtuellen Computern beim Erstellen ausgehender Datenflüsse verwendet werden. Statische IP-Adressen stammen von Ressourcen für öffentliche IP-Adressen, Präfixressourcen für öffentliche IP-Adressen oder aus beiden Quellen. Für eine NAT-Gatewayressource können für die beiden Quellen jeweils bis zu 16 statische IP-Adressen verwendet werden.


<p align="center">
  <img src="media/nat-overview/flow-direction1.svg" width="256" title="Virtual Network NAT für Internetverbindung in ausgehender Richtung">
</p>

*Abbildung: Virtual Network NAT für Internetverbindung in ausgehender Richtung*


>[!NOTE] 
>Virtual Network NAT ist derzeit als öffentliche Vorschauversion (Public Preview) verfügbar. Dieser Dienst ist bisher nur in einer begrenzten Zahl von [Regionen](nat-overview.md#region-availability) erhältlich. Diese Vorschau wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Ergänzende Nutzungsbedingungen für Microsoft Azure-Vorschauversionen](https://azure.microsoft.comsupport/legal/preview-supplemental-terms).

## <a name="how-to-deploy-nat"></a>Bereitstellen von NAT

Die Konfiguration und Nutzung von NAT-Gateways wurde bewusst einfach gehalten:  

NAT-Gatewayressource:
- Erstellen einer regionalen oder zonenbasierten (nur in einer Zone vorhandenen) NAT-Gatewayressource
- Zuweisen von IP-Adressen
- Ändern des Leerlauftimeouts

Virtuelles Netzwerk:
- Konfigurieren Sie das Subnetz des virtuellen Netzwerks so, dass ein NAT-Gateway verwendet wird.

Benutzerdefinierte Routen sind nicht erforderlich.

## <a name="resource"></a>Resource

Bei der Ressource wurde auf einen einfachen Aufbau geachtet. Dies ist im folgenden Azure Resource Manager-Beispiel im Vorlagenformat dargestellt.  Dieses Vorlagenformat ist hier angegeben, um die Konzepte und die Struktur zu verdeutlichen.  Ändern Sie das Beispiel gemäß Ihren Anforderungen.  Dieses Dokument ist nicht als Tutorial gedacht.

Im folgenden Diagramm sind die schreibbaren Verweise zwischen den verschiedenen Azure Resource Manager-Ressourcen dargestellt.  Der Pfeil gibt die Richtung des Verweises an und geht vom schreibbaren Ort aus. Überprüfung 

<p align="center">
  <img src="media/nat-overview/flow-map.svg" width="256" title="Virtual Network NAT-Objektmodell">
</p>

*Abbildung: Virtual Network NAT-Objektmodell*

NAT wird für die meisten Workloads empfohlen, sofern bei Ihnen nicht eine spezifische Abhängigkeit von [poolbasierter Load Balancer-Konnektivität in ausgehender Richtung](../load-balancer/load-balancer-outbound-connections.md) besteht.  

Sie können die Migration von Standardszenarien für den Lastenausgleich, einschließlich [Ausgangsregeln](../load-balancer/load-balancer-outbound-rules-overview.md), zu einem NAT-Gateway durchführen. Verschieben Sie zur Durchführung der Migration die Ressourcen bzw. Präfixressourcen für öffentliche IP-Adressen von den Front-Ends des Lastenausgleichs auf das NAT-Gateway. Neue IP-Adressen für das NAT-Gateway sind nicht erforderlich. Öffentliche Standard-IP-Adressen und -Präfixe können wiederverwendet werden, sofern die Gesamtzahl von 16 IP-Adressen nicht überschritten wird. Planen Sie die Migration so, dass die Dienstunterbrechung während der Umstellung berücksichtigt wird.  Sie können die Unterbrechungsdauer verringern, indem Sie den Prozess automatisieren. Testen Sie die Migration zuerst in einer Stagingumgebung.  Während der Umstellung werden Datenflüsse, die ursprünglich in eingehender Richtung erfolgt sind, nicht beeinträchtigt.

Im folgenden Beispiel wird eine NAT-Gatewayressource mit dem Namen _myNATGateway_ in der Region _East US 2, AZ 1_ (USA, Osten 2, Verfügbarkeitszone 1) mit einem Leerlauftimeout von _4 Minuten_ erstellt. Die angegebenen ausgehenden IP-Adressen lauten:
- Ressourcen für öffentliche IP-Adressen _myIP1_ und _myIP2_ und 
- Präfixressourcen für öffentliche IP-Adressen _myPrefix1_ und _myPrefix2_. 

Die Gesamtzahl von IP-Adressen, die von allen vier IP-Adressressourcen bereitgestellt wird, darf den Höchstwert von 16 IP-Adressen nicht übersteigen. Eine beliebige Anzahl von IP-Adressen zwischen 1 und 16 ist zulässig.

```json
{
"name": "myNATGateway",
   "type": "Microsoft.Network/natGateways",
   "apiVersion": "2018-11-01",
   "location": "East US 2",
   "sku": { "name": "Standard" },
   "zones": [ "1" ],
   "properties": {
      "idleTimeoutInMinutes": 4, 
      "publicIPPrefixes": [
         {
            "id": "ref to myPrefix1"
         },
         {
            "id": "ref to myPrefix2"
         }
      ],
      "publicIPAddresses": [
         {
            "id": "ref to myIP1"
         },
         {
            "id": "ref to myIP2"
         }
      ]
   }
}
```

Nachdem die NAT-Gatewayressource erstellt wurde, kann sie in einem oder mehreren Subnetzen eines virtuellen Netzwerks verwendet werden. Geben Sie an, von welchen Subnetzen diese NAT-Gatewayressource verwendet wird. Ein NAT-Gateway kann nicht für mehr als ein virtuelles Netzwerk gelten. Es ist nicht erforderlich, allen Subnetzen eines virtuellen Netzwerks dasselbe NAT-Gateway zuzuweisen. Die einzelnen Subnetze können mit unterschiedlichen NAT-Gatewayressourcen konfiguriert werden.

Szenarien, für die keine Verfügbarkeitszonen genutzt werden, sind regional (keine Angabe einer Zone). Bei Verwendung von Verfügbarkeitszonen können Sie eine Zone angeben, um NAT in einer bestimmten Zone zu isolieren. Die Zonenredundanz wird nicht unterstützt. Lesen Sie den Artikel über NAT-[Verfügbarkeitszonen](#availability-zones).


```json
{
   "name": "myVNet",
   "apiVersion": "2018-11-01",
   "type": "Microsoft.Network/virtualNetworks",
   "location": "myRegion", 
   "properties": {
      "addressSpace": {
          "addressPrefixes": [
           "192.168.0.0/16"
          ]
      },
      "subnets": [
         {
            "name": "mySubnet1",
            "properties": {
               "addressPrefix": "192.168.0.0/24",
               "natGateway": {
                  "id": "ref to myNATGateway"
               }
            }
         } 
      ]
   }
}
```
NAT-Gateways werden mit einer Eigenschaft in einem Subnetz eines virtuellen Netzwerks definiert. Für Datenflüsse, die von virtuellen Computern im Subnetz _mySubnet1_ des virtuellen Netzwerks _myVNet_ erstellt werden, wird das NAT-Gateway verwendet. Für alle ausgehenden Verbindungen werden die IP-Adressen verwendet, die _myNatGateway_ als Quell-IP-Adresse zugeordnet sind.


## <a name="design-guidance"></a>Leitfaden zum Entwurf

Lesen Sie diesen Abschnitt, um sich mit den Aspekten in Bezug auf das Entwerfen virtueller Netzwerke mit NAT vertraut zu machen.  

1. [Kostenoptimierung](#cost-optimization)
1. [Koexistenz von eingehenden und ausgehenden Verbindungen](#coexistence-of-inbound-and-outbound)
2. [Verwalten von „Basic“-Ressourcen](#managing-basic-resources)
3. [Verfügbarkeitszonen](#availability-zones)

### <a name="cost-optimization"></a>Kostenoptimierung

[Dienstendpunkte](virtual-network-service-endpoints-overview.md) und [Private Link](../private-link/private-link-overview.md) sind zwei Optionen, die beim Optimieren der Kosten berücksichtigt werden sollten, wenn NAT nicht benötigt wird.  Der gesamte Datenverkehr, der an Dienstendpunkte oder Private Link geleitet wird, wird vom NAT-Dienst des virtuellen Netzwerks nicht verarbeitet.  

Mit Dienstendpunkten werden Azure-Dienstressourcen an Ihr virtuelles Netzwerk gebunden, und sie dienen zum Steuern des Zugriffs auf Ihre Azure-Dienstressourcen. Verwenden Sie beim Zugreifen auf Azure-Speicher beispielsweise einen Dienstendpunkt für die Speicherung, um NAT-Gebühren für die Datenverarbeitung zu vermeiden. Dienstendpunkte sind kostenlos.

Per Private Link wird der Azure PaaS-Dienst (oder andere mit Private Link gehostete Dienste) als privater Endpunkt in einem virtuellen Netzwerk verfügbar gemacht.  Private Link wird basierend auf der Dauer und den verarbeiteten Daten berechnet.

Evaluieren Sie, ob ein oder beide Ansätze für Ihr Szenario geeignet sind, und nutzen Sie sie je nach Bedarf.

### <a name="coexistence-of-inbound-and-outbound"></a>Koexistenz von eingehenden und ausgehenden Verbindungen

Das NAT-Gateway ist kompatibel mit:

 - Load Balancer Standard
 - Öffentliche IP-Standardadresse
 - Öffentliches IP-Standardpräfix

Beginnen Sie bei der Entwicklung einer neuen Bereitstellung mit Standard-SKUs.

<p align="center">
  <img src="media/nat-overview/flow-direction1.svg" width="256" title="Virtual Network NAT für Internetverbindung in ausgehender Richtung">
</p>

*Abbildung: Virtual Network NAT für Internetverbindung in ausgehender Richtung*

Das Szenario mit ausschließlich ausgehender Internetverbindung per NAT-Gateway kann um die Funktionalität für eine eingehende Internetverbindung erweitert werden. Jede Ressource kennt die Richtung, in der ein Datenfluss ausgelöst wird. In einem Subnetz mit einem NAT-Gateway werden alle Szenarien mit ausgehenden Internetverbindungen durch das NAT-Gateway ersetzt. Szenarien mit eingehender Internetverbindung werden von der entsprechenden Ressource bereitgestellt.

#### <a name="nat-and-vm-with-instance-level-public-ip"></a>NAT und VM mit öffentlicher IP-Adresse auf Instanzebene

<p align="center">
  <img src="media/nat-overview/flow-direction2.svg" width="300" title="Virtual Network NAT und VM mit öffentlicher IP-Adresse auf Instanzebene">
</p>

*Abbildung: Virtual Network NAT und VM mit öffentlicher IP-Adresse auf Instanzebene*

| Direction | Resource |
|:---:|:---:|
| Eingehend | VM mit öffentlicher IP-Adresse auf Instanzebene |
| Ausgehend | NAT Gateway |

Von der VM wird das NAT-Gateway für ausgehenden Datenverkehr genutzt.  Ursprünglich eingehender Datenverkehr ist nicht betroffen.

#### <a name="nat-and-vm-with-public-load-balancer"></a>NAT und VM mit öffentlichem Load Balancer

<p align="center">
  <img src="media/nat-overview/flow-direction3.svg" width="350" title="Virtual Network NAT und VM mit öffentlichem Load Balancer">
</p>

*Abbildung: Virtual Network NAT und VM mit öffentlichem Load Balancer*

| Direction | Resource |
|:---:|:---:|
| Eingehend | Öffentlicher Load Balancer |
| Ausgehend | NAT Gateway |

Alle Ausgangskonfigurationen einer Lastenausgleichsregel oder von Ausgangsregeln werden durch das NAT-Gateway ersetzt.  Ursprünglich eingehender Datenverkehr ist nicht betroffen.

#### <a name="nat-and-vm-with-instance-level-public-ip-and-public-load-balancer"></a>NAT und VM mit öffentlicher IP-Adresse auf Instanzebene und öffentlichem Load Balancer

<p align="center">
  <img src="media/nat-overview/flow-direction4.svg" width="425" title="Virtual Network NAT und VM mit öffentlicher IP-Adresse auf Instanzebene und öffentlichem Load Balancer">
</p>

*Abbildung: Virtual Network NAT und VM mit öffentlicher IP-Adresse auf Instanzebene und öffentlichem Load Balancer*

| Direction | Resource |
|:---:|:---:|
| Eingehend | VM mit öffentlicher IP-Adresse auf Instanzebene und öffentlichem Load Balancer |
| Ausgehend | NAT Gateway |

Alle Ausgangskonfigurationen einer Lastenausgleichsregel oder von Ausgangsregeln werden durch das NAT-Gateway ersetzt.  Die VM nutzt auch das NAT-Gateway für ausgehenden Datenverkehr.  Ursprünglich eingehender Datenverkehr ist nicht betroffen.

### <a name="managing-basic-resources"></a>Verwalten von „Basic“-Ressourcen

Load Balancer Standard, die öffentliche IP-Adresse und das Präfix für die öffentliche IP-Adresse sind mit dem NAT-Gateway kompatibel. NAT-Gateways werden im Subnetzbereich eingesetzt. Die Basic-SKU dieser Dienste muss in einem Subnetz ohne NAT-Gateway bereitgestellt werden. Diese Trennung ermöglicht es, dass beide SKU-Varianten in demselben virtuellen Netzwerk vorhanden sein können.

NAT-Gateways haben Vorrang vor Ausgangsszenarien des Subnetzes. Load Balancer im Tarif „Basic“ oder die öffentliche IP-Adresse (und viele integrierte verwaltete Dienste) können mit den richtigen Übersetzungen nicht angepasst werden. Das NAT-Gateway übernimmt für ein Subnetz die Kontrolle über den ausgehenden Datenverkehr ins Internet. Der eingehende Datenverkehr zum Load Balancer im Tarif „Basic“ und an die öffentliche IP-Adresse ist nicht verfügbar. Der eingehende Datenverkehr an einen Load Balancer im Tarif „Basic“ oder eine auf einer VM konfigurierten öffentlichen IP-Adresse ist nicht verfügbar.

### <a name="availability-zones"></a>Verfügbarkeitszonen

Auch ohne Verfügbarkeitszonen ist NAT resilient und kann mehrere Ausfälle von Infrastrukturkomponenten überstehen. Wenn Verfügbarkeitszonen Teil Ihres Szenarios sind, sollten Sie NAT für eine bestimmte Zone konfigurieren.  Die Vorgänge auf der Steuerungs- und Datenebene sind auf die angegebene Zone beschränkt. Ein Ausfall in einer anderen Zone als Ihrer eigenen Zone sollte keine Auswirkungen auf NAT haben. Für ausgehenden Datenverkehr von virtuellen Computern in derselben Zone tritt aufgrund der Zonenisolation ein Fehler auf.

<p align="center">
  <img src="media/nat-overview/az-directions.svg" width="425" title="Virtual Network NAT mit Verfügbarkeitszonen">
</p>

*Abbildung: Virtual Network NAT mit Verfügbarkeitszonen*

Für ein NAT-Gateway mit Zonenisolation müssen die IP-Adressen mit der Zone des NAT-Gateways übereinstimmen. NAT-Gatewayressourcen mit IP-Adressen aus einer anderen Zone oder ohne Zone werden nicht unterstützt.

Virtuelle Netzwerke und Subnetze sind regional und nicht nach Zonen ausgerichtet. Eine VM muss sich in derselben Zone wie das NAT-Gateway befinden, um für ausgehende Verbindungen eine Zonenzusage zu ermöglichen. Die Zonenisolation wird durch die Erstellung von jeweils einem „Zonenstapel“ pro Verfügbarkeitszone erzielt. Es ist keine Zonenzusage vorhanden, wenn Zonen eines NAT-Zonengateways durchlaufen werden oder ein regionales NAT-Gateway mit Zonen-VMs verwendet wird.

Wenn Sie VM-Skalierungsgruppen zur Nutzung mit NAT bereitstellen, stellen Sie eine Zonenskalierungsgruppe im eigenen Subnetz bereit und fügen das passende NAT-Zonengateway an dieses Subnetz an. Bei Verwendung von zonenübergreifenden Skalierungsgruppen (eine Skalierungsgruppe in zwei oder mehr Zonen), wird vom NAT-Dienst keine Zonenzusage bereitgestellt.  NAT unterstützt keine Zonenredundanz.  Nur die Regions- oder Zonenisolation wird unterstützt.

<p align="center">
  <img src="media/nat-overview/az-directions2.svg" width="425" title="Zonenübergreifende Virtual Network NAT">
</p>

*Abbildung: Zonenübergreifende Virtual Network NAT*

Die Zoneneigenschaft ist nicht veränderlich.  Stellen Sie die NAT-Gatewayressource mit der gewünschten Regions- oder Zonenpräferenz erneut bereit.

>[!NOTE] 
>IP-Adressen allein sind nicht zonenredundant, wenn keine Zone angegeben wird.  Das Front-End einer [Load Balancer Standard-Instanz ist zonenredundant](../load-balancer/load-balancer-standard-availability-zones.md#frontend), wenn in einer bestimmten Zone keine IP-Adresse erstellt wird.  Dies gilt nicht für NAT.  Nur die Regions- oder Zonenisolation wird unterstützt.

## <a name="source-network-address-translation"></a>Übersetzung der Quellnetzwerkadresse (Source Network Address Translation, SNAT)

Bei der Übersetzung der Quellnetzwerkadresse (Source Network Address Translation, SNAT) wird die Quelle eines Datenflusses so umgeschrieben, dass dieser von einer anderen IP-Adresse ausgeht.  Für NAT-Gatewayressourcen wird eine Variante von SNAT genutzt, die allgemein als Portadressübersetzung (Port Address Translation, PAT) bezeichnet wird. Bei PAT werden die Quelladresse und der Quellport neu geschrieben. Bei SNAT gibt es keine feste Beziehung zwischen der Anzahl von privaten Adressen und den zugehörigen übersetzten öffentlichen Adressen.  

### <a name="fundamentals"></a>Grundlagen

Wir sehen uns ein Beispiel mit vier Datenflüssen an, um das grundlegende Konzept zu beschreiben.  Für das NAT-Gateway wird die öffentliche IP-Adressressource 65.52.0.2 verwendet.

| Flow | Quelltupel | Zieltupel |
|:---:|:---:|:---:|
| 1 | 192.168.0.16:4283 | 65.52.0.1:80 |
| 2 | 192.168.0.16:4284 | 65.52.0.1:80 |
| 3 | 192.168.0.17.5768 | 65.52.0.1:80 |
| 4 | 192.168.0.16:4285 | 65.52.0.2:80 |

Diese Datenflüsse sehen nach dem PAT-Vorgang ggf. wie folgt aus:

| Flow | Quelltupel | Quelltupel nach SNAT | Zieltupel | 
|:---:|:---:|:---:|:---:|
| 1 | 192.168.0.16:4283 | 65.52.0.2:234 | 65.52.0.1:80 |
| 2 | 192.168.0.16:4284 | 65.52.0.2:235 | 65.52.0.1:80 |
| 3 | 192.168.0.17.5768 | 65.52.0.2:236 | 65.52.0.1:80 |
| 4 | 192.168.0.16:4285 | 65.52.0.2:237 | 65.52.0.2:80 |

Dem Ziel wird die Quelle des Datenflusses als 65.52.0.2 (SNAT-Quelltupel) mit dem zugewiesenen Port angezeigt.  Der PAT-Vorgang wie in der obigen Tabelle wird auch als „SNAT mit Portmaskierung“ bezeichnet.  Mehrere private Quellen werden hinter der IP-Adresse und dem Port maskiert.

Richten Sie keine Abhängigkeit von der spezifischen Zuweisung von Quellports ein.  Die obige Abbildung enthält nur eine Darstellung des grundlegenden Konzepts.

Die Bereitstellung von SNAT über NAT unterscheidet sich in mehreren Punkten von [Load Balancer](../load-balancer/load-balancer-outbound-connections.md).

### <a name="on-demand"></a>Bei Bedarf

Von NAT werden bedarfsgesteuerte SNAT-Ports für neue ausgehende Datenverkehrsflüsse bereitgestellt. Alle verfügbaren SNAT-Ports des Bestands werden von allen virtuellen Computern in Subnetzen genutzt, für die NAT konfiguriert ist. 

<p align="center">
  <img src="media/nat-overview/lb-vnnat-chart.svg" width="550" title="Virtual Network NAT: Bedarfsgesteuerte SNAT in ausgehender Richtung">
</p>

*Abbildung: Virtual Network NAT: Bedarfsgesteuerte SNAT in ausgehender Richtung*

Über die IP-Konfiguration eines virtuellen Computers können je nach Bedarf Datenflüsse in ausgehender Richtung erstellt werden.  Eine Vorabzuteilung (Planung pro Instanz mit Überbereitstellung für Notfälle) ist nicht erforderlich.  

<p align="center">
  <img src="media/nat-overview/exhaustion-threshold.svg" width="550" title="Unterschiede bei Auslastungsszenarien">
</p>

*Abbildung: Unterschiede bei Auslastungsszenarien*

Nach der Freigabe eines SNAT-Ports kann dieser von allen virtuellen Computern in Subnetzen genutzt werden, für die NAT konfiguriert ist.  Bei der bedarfsgesteuerten Zuteilung können SNAT-Ports von dynamischen und divergenten Workloads in Subnetzen je nach Bedarf genutzt werden.  Solange ein Bestand an SNAT-Ports verfügbar ist, sind SNAT-Datenflüsse erfolgreich. SNAT-Port-Hotspots profitieren dagegen von einem größeren Bestand. SNAT-Ports bleiben für virtuelle Computer, die diese nicht aktiv benötigen, nicht ungenutzt.

### <a name="scaling"></a>Skalierung

Für NAT wird ein ausreichender Bestand an SNAT-Ports für das vollständige Ausgangsszenario benötigt. Die Skalierung von NAT ist hauptsächlich eine Funktion, bei der es um die Verwaltung des freigegebenen verfügbaren SNAT-Portbestands geht. Es muss ein ausreichender Bestand vorhanden sein, um den ausgehenden Datenfluss bei Spitzenlast für alle Subnetze abdecken zu können, die mit einer NAT-Gatewayressource verknüpft sind.

Bei SNAT werden mehrere private Adressen einer öffentlichen Adresse zugeordnet und zur Skalierung mehrere öffentliche IP-Adressen genutzt.

Für eine NAT-Gatewayressource werden 64.000 Ports (SNAT-Ports) einer öffentlichen IP-Adresse genutzt.  Diese SNAT-Ports bilden den verfügbaren Bestand für die Zuordnung von Datenflüssen von „Privat“ zu „Öffentlich“. Wenn Sie weitere öffentliche IP-Adressen hinzufügen, wird die Anzahl von verfügbaren SNAT-Ports des Bestands erhöht. NAT-Gatewayressourcen können auf bis zu 16 IP-Adressen und 1 Million SNAT-Ports skaliert werden.  Für TCP und UDP werden separate SNAT-Portbestände verwendet, die nicht miteinander in Beziehung stehen.

Von NAT-Gatewayressourcen werden Quellports auf opportunistische Weise wiederverwendet. Zu Skalierungszwecken sollten Sie davon ausgehen, dass für jeden Datenfluss ein neuer SNAT-Port benötigt wird, und die Gesamtzahl der verfügbaren IP-Adressen für ausgehenden Datenverkehr entsprechend skalieren.

### <a name="protocols"></a>Protokolle

NAT-Gatewayressourcen interagieren mit IP- und IP-Transportheadern von UDP- und TCP-Datenflüssen und sind gegenüber Nutzlasten auf Anwendungsebene agnostisch.  Andere IP-Protokolle werden nicht unterstützt.

### <a name="timers"></a>Timer

Das Leerlauftimeout kann für alle Datenflüsse von vier Minuten (Standard) auf 120 Minuten (2 Stunden) angepasst werden.  Darüber hinaus können Sie den Leerlaufzeitgeber mit Datenverkehr im Datenfluss zurücksetzen.  Ein empfohlenes Muster zur Aktualisierung von Verbindungen mit langer Leerlaufzeit und zur Erkennung des Livezustands von Endpunkten sind TCP-Keep-Alives.  TCP-Keep-Alives werden als doppelte ACK-Vorgänge für die Endpunkte angezeigt und sind mit geringem Aufwand verbunden und für die Anwendungsschicht unsichtbar.

Die folgenden Zeitgeber werden für die Freigabe von SNAT-Ports verwendet:

| Zeitgeber | value |
|---|---|
| TCP FIN (TCP-Beendigung) | 60 Sekunden |
| TCP RST (TCP-Zurücksetzung) | 10 Sekunden |
| TCP half open (TCP halb geöffnet) | 30 Sekunden |

Ein SNAT-Port ist für eine IP-Zieladresse und den entsprechenden Zielport nach fünf Sekunden bereit für die Wiederverwendung.

>[!NOTE] 
>Diese Zeitgebereinstellungen können sich ändern. Die Werte sind hier nur als Hilfe bei der Problembehandlung angegeben, und Sie sollten noch keine Abhängigkeit von bestimmten Zeitgebern einrichten.

## <a name="limitations"></a>Einschränkungen

- NAT ist mit öffentlichen IP-Adressen, Präfixen für öffentliche IP-Adressen und Lastenausgleichsressourcen der Standard-SKU kompatibel.   Basic-Ressourcen (z. B. Load Balancer im Tarif „Basic“) und alle davon abgeleiteten Produkte sind nicht mit NAT kompatibel.  Basic-Ressourcen müssen in einem Subnetz angeordnet sein, für das NAT nicht konfiguriert ist.
- Die Familie der IPv4-Adressen wird unterstützt.  NAT interagiert nicht mit der Familie der IPv6-Adressen.
- Die Protokollierung von NSG-Datenflüssen wird bei Verwendung von NAT nicht unterstützt.
- NAT kann übergreifend für mehrere virtuelle Netzwerke genutzt werden.

## <a name="preview-participation"></a>Teilnahme an der Vorschauversion

Befolgen Sie die [Anleitung zur Aktivierung Ihres Abonnements](nat-overview.md#public-preview-participation).

## <a name="feedback"></a>Feedback

Wir möchten wissen, wie wir den Dienst verbessern können. Senden Sie uns [Feedback zur öffentlichen Vorschauversion](https://aka.ms/natfeedback).  Sie haben auch die Möglichkeit, unter [UserVoice für NAT](https://aka.ms/natuservoice) Vorschläge zu den nächsten Entwicklungsschritten zu machen und darüber abzustimmen.

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich weiter über [Virtual Network NAT](nat-overview.md).
- Tutorial zur Überprüfung des NAT-Gateways
  * [Azure CLI](tutorial-create-validate-nat-gateway-cli.md)
  * [PowerShell](tutorial-create-validate-nat-gateway-cli.md)
  * [Portal](tutorial-create-validate-nat-gateway-cli.md)
- Schnellstart zur Bereitstellung einer NAT-Gatewayressource
  * [Azure CLI](./quickstart-create-nat-gateway-cli.md)
  * [PowerShell](./quickstart-create-nat-gateway-powershell.md)
  * [Portal](./quickstart-create-nat-gateway-portal.md)
- Informieren Sie sich über [Verfügbarkeitszonen](../availability-zones/az-overview.md).
- Informieren Sie sich über [Load Balancer Standard](../load-balancer/load-balancer-standard-overview.md).
- Informieren Sie sich über [Verfügbarkeitszonen und Standard Load Balancer](../load-balancer/load-balancer-standard-availability-zones.md).
- Weitere Informationen zur NAT-Gatewayressourcen-API
  * [REST-API](https://docs.microsoft.com/rest/api/virtualnetwork/natgateways)
  * [Azure CLI](https://docs.microsoft.com/cli/azure/network/nat/gateway?view=azure-cli-latest)
  * [PowerShell](https://docs.microsoft.com/powershell/module/az.network/new-aznatgateway):
- [Schlagen Sie bei UserVoice die nächsten Entwicklungsschritte vor](https://aka/natuservoice).
- [Senden Sie Feedback zur öffentlichen Vorschauversion](https://aka.ms/natfeedback).
