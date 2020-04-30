---
title: Verwenden von S2S-VPN als Sicherung für privates Azure ExpressRoute-Peering | Microsoft-Dokumentation
description: Diese Seite enthält architektonische Empfehlungen für das Sichern von privatem Azure ExpressRoute-Peering mit S2S-VPN.
services: networking
author: rambk
ms.service: expressroute
ms.topic: article
ms.date: 02/05/2020
ms.author: rambala
ms.openlocfilehash: a6a22b667bc66d6ee69bfbd7ad1db88f72d8df0e
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/21/2020
ms.locfileid: "81687831"
---
# <a name="using-s2s-vpn-as-a-backup-for-expressroute-private-peering"></a>Verwenden von S2S-VPN als Sicherung für privates ExpressRoute-Peering

Im Artikel [Entwurf für die Notfallwiederherstellung mit privatem ExpressRoute-Peering][DR-PP] wurde die Notwendigkeit einer redundanten Konnektivitätslösung für eine private ExpressRoute-Peeringverbindung erörtert, und es wurde erläutert, wie georedundante ExpressRoute-Verbindungen zu diesem Zweck verwendet werden. In diesem Artikel wird beschrieben, wie Sie Site-to-Site-VPN (S2S-VPN) als Sicherung für privates ExpressRoute-Peering nutzen und verwalten. 

Im Gegensatz zu georedundanten ExpressRoute-Verbindungen können Sie die ExpressRoute-VPN-Kombination zur Notfallwiederherstellung nur im Aktiv/Passiv-Modus verwenden. Bei Verwendung einer Sicherungsnetzwerkkonnektivität im passiven Modus liegt eine große Herausforderung darin, dass die passive Verbindung häufig zusammen mit der primären Verbindung fehlschlägt. Der häufige Grund für die Fehler bei der passiven Verbindung ist die fehlende aktive Verwaltung. Daher konzentrieren wir uns in diesem Artikel darauf, wie Sie die S2S-VPN-Konnektivität, die ein privates ExpressRoute-Peering sichert, überprüfen und verwalten können.

>[!NOTE] 
>Wenn eine bestimmte Route sowohl über ExpressRoute als auch über VPN angekündigt wird, würde Azure das Routing über ExpressRoute vorziehen.  
>

In diesem Artikel erfahren Sie, wie Sie die Konnektivität sowohl aus der Azure-Perspektive als auch aus der Perspektive der kundenseitigen Netzwerkperipherie überprüfen. Die Fähigkeit, die Überprüfung von beiden Seiten aus durchzuführen, ist unabhängig davon hilfreich, ob Sie die kundenseitigen Netzwerkgeräte verwalten, mit denen ein Peering mit den Microsoft-Netzwerkentitäten erfolgt. 

## <a name="example-topology"></a>Topologiebeispiel

In unserem Setup ist ein lokales Netzwerk über eine ExpressRoute-Verbindung und eine S2S-VPN-Verbindung mit einem Azure-Hub-VNet verbunden. Das Azure-Hub-VNet ist wiederum mittels Peering mit einem Spoke-VNet verbunden, wie in der folgenden Abbildung dargestellt:

![1][1]

In diesem Setup wird die ExpressRoute-Verbindung auf einem Paar von Edgeroutern des Kunden (Customer Edge, CE) am lokalen Standort beendet. Das lokale Netzwerk ist mit den CE-Routern über ein Paar von Firewalls verbunden, die im Leader-Follower-Modus ausgeführt werden. Die S2S-VPN-Verbindung wird direkt auf den Firewalls beendet.

In der folgenden Tabelle sind die wichtigsten IP-Präfixe der Topologie aufgelistet:

| **Entität** | **Präfix** |
| --- | --- |
| Lokales LAN | 10.1.11.0/25 |
| Azure-Hub-VNet | 10.17.11.0/25 |
| Azure-Spoke-VNet | 10.17.11.128/26 |
| Lokaler Testserver | 10.1.11.10 |
| Test-VM im Spoke-VNet | 10.17.11.132 |
| Primäre ExpressRoute-Verbindung im P2P-Subnetz | 192.168.11.16/30 |
| Sekundäre ExpressRoute-Verbindung im P2P-Subnetz | 192.168.11.20/30 |
| Primäre BGP-Peer-IP-Adresse des VPN-Gateways | 10.17.11.76 |
| Sekundäre BGP-Peer-IP-Adresse des VPN-Gateways | 10.17.11.77 |
| BGP-Peer-IP-Adresse der lokalen Firewall für VPN | 192.168.11.88 |
| Primäre CE-Router-I/F für Firewall-IP | 192.168.11.0/31 |
| Firewall-I/F für primäre CE-Router-IP | 192.168.11.1/31 |
| Sekundäre CE-Router-I/F für Firewall-IP | 192.168.11.2/31 |
| Firewall-I/F für sekundäre CE-Router-IP | 192.168.11.3/31 |


In der folgenden Tabelle sind die ASNs der Topologie aufgelistet:

| **Autonomes System** | **ASN** |
| --- | --- |
| Lokal | 65020 |
| Microsoft Enterprise Edge | 12076 |
| VNet-GW (ExR) | 65515 |
| VNet-GW (VPN) | 65515 |

## <a name="high-availability-without-asymmetricity"></a>Hochverfügbarkeit ohne Asymmetrie

### <a name="configuring-for-high-availability"></a>Konfigurieren für Hochverfügbarkeit

Unter [Konfigurieren von parallel bestehenden ExpressRoute- und Site-to-Site-Verbindungen][Conf-CoExist] wird beschrieben, wie Sie parallel bestehende ExpressRoute- und S2S-VPN-Verbindungen konfigurieren. Wie unter [Entwurf für Hochverfügbarkeit mit ExpressRoute][HA] erläutert, wird in unserem Setup die Netzwerkredundanz bis zu den Endpunkten aufrechterhalten (Vermeidung einer einzelnen Fehlerquelle), um die Hochverfügbarkeit von ExpressRoute zu verbessern. Außerdem sind die primäre und die sekundäre Verbindung der ExpressRoute-Verbindungen so konfiguriert, dass sie im Aktiv/Aktiv-Modus ausgeführt werden, indem die lokalen Präfixe über beide Verbindungen auf dieselbe Weise angekündigt werden. 

Die lokale Routenankündigung des primären CE-Routers über die primäre Verbindung der ExpressRoute-Verbindung ist unten dargestellt (Junos-Befehle):

    user@SEA-MX03-01> show route advertising-protocol bgp 192.168.11.18 

    Cust11.inet.0: 8 destinations, 8 routes (7 active, 0 holddown, 1 hidden)
      Prefix                  Nexthop              MED     Lclpref    AS path
    * 10.1.11.0/25            Self                                    I

Die lokale Routenankündigung des sekundären CE-Routers über die sekundäre Verbindung der ExpressRoute-Verbindung ist unten dargestellt (Junos-Befehle):

    user@SEA-MX03-02> show route advertising-protocol bgp 192.168.11.22 

    Cust11.inet.0: 8 destinations, 8 routes (7 active, 0 holddown, 1 hidden)
      Prefix                  Nexthop              MED     Lclpref    AS path
    * 10.1.11.0/25            Self                                    I

Um die Hochverfügbarkeit der Sicherungsverbindung zu verbessern, wird das S2S-VPN ebenfalls im Aktiv/Aktiv-Modus konfiguriert. Die Konfiguration des Azure-VPN-Gateways ist unten dargestellt. Beachten Sie, dass die BGP-Peer-IP-Adressen des VPN-Gateways (10.17.11.76 und 10.17.11.77) als Teil der VPN-Konfiguration ebenfalls aufgelistet sind.

![2][2]

Die lokale Route wird dem primären und dem sekundären BGP-Peer des VPN-Gateways von den Firewalls angekündigt. Die Routenankündigungen sind unten dargestellt (Junos):

    user@SEA-SRX42-01> show route advertising-protocol bgp 10.17.11.76 

    Cust11.inet.0: 14 destinations, 21 routes (14 active, 0 holddown, 0 hidden)
      Prefix                  Nexthop              MED     Lclpref    AS path
    * 10.1.11.0/25            Self                                    I

    {primary:node0}
    user@SEA-SRX42-01> show route advertising-protocol bgp 10.17.11.77    

    Cust11.inet.0: 14 destinations, 21 routes (14 active, 0 holddown, 0 hidden)
      Prefix                  Nexthop              MED     Lclpref    AS path
    * 10.1.11.0/25            Self                                    I

>[!NOTE] 
>Die Konfiguration des S2S-VPNs im Aktiv/Aktiv-Modus bietet nicht nur Hochverfügbarkeit für die Sicherungsnetzwerkkonnektivität zur Notfallwiederherstellung, sondern auch einen höheren Durchsatz für die Sicherungskonnektivität. Anders ausgedrückt: Es wird empfohlen, das S2S-VPN im Aktiv/Aktiv-Modus zu konfigurieren, weil dadurch die Erstellung mehrerer zugrunde liegender Tunnel erzwungen wird.
>

### <a name="configuring-for-symmetric-traffic-flow"></a>Konfigurieren für den symmetrischen Datenverkehrsfluss

Wie bereits erwähnt, würde Azure den ExpressRoute-Pfad vorziehen, wenn eine bestimmte lokale Route sowohl über ExpressRoute als auch über S2S-VPN angekündigt wird. Um zu erzwingen, dass Azure den S2S-VPN-Pfad der parallel bestehenden ExpressRoute-Verbindung vorzieht, müssen Sie spezifischere Routen (längeres Präfix mit größerer Subnetzmaske) über die VPN-Verbindung ankündigen. Unser Ziel ist es, die VPN-Verbindungen nur als Sicherung zu verwenden. Daher steht das Standardverhalten bei der Pfadauswahl von Azure mit unserem Ziel im Einklang. 

Wir müssen sicherstellen, dass der Datenverkehr, der von der lokalen Umgebung an Azure geleitet wird, auch den ExpressRoute-Pfad der S2S-VPN-Verbindung vorzieht. Die lokale Standardeinstellung der CE-Router und Firewalls in unserem lokalen Setup ist 100. Wenn wir also die lokale Einstellung der über die privaten ExpressRoute-Peerings empfangenen Routen auf über 100 (z. B. auf 150) festlegen, können wir dafür sorgen, dass der für Azure bestimmte Datenverkehr die ExpressRoute-Verbindung im stabilen Zustand vorzieht.

Die BGP-Konfiguration des primären CE-Routers, der die primäre Verbindung der ExpressRoute-Verbindung beendet, ist unten dargestellt. Beachten Sie, dass der Wert der lokalen Einstellung der Routen, die über die iBGP-Sitzung angekündigt werden, auf 150 festgelegt ist. Ebenso müssen wir sicherstellen, dass die lokale Einstellung des sekundären CE-Routers, der die sekundäre Verbindung der ExpressRoute-Verbindung beendet, auch auf 150 festgelegt ist.

    user@SEA-MX03-01> show configuration routing-instances Cust11 
    description "Customer 11 VRF";
    instance-type virtual-router;
    interface xe-0/0/0:0.110;
    interface ae0.11;
    protocols {
      bgp {
        group ibgp {
            type internal;
            local-preference 150;
            neighbor 192.168.11.1;
        }
        group ebgp {
            peer-as 12076;
            bfd-liveness-detection {
                minimum-interval 300;
                multiplier 3;
            }
            neighbor 192.168.11.18;
        }
      }
    }

Die (unten dargestellte) Routingtabelle der lokalen Firewalls bestätigt, dass der bevorzugte Pfad für den lokalen Datenverkehr, der für Azure bestimmt ist, über die ExpressRoute-Verbindung im stabilen Zustand verläuft.

    user@SEA-SRX42-01> show route table Cust11.inet.0 10.17.11.0/24    

    Cust11.inet.0: 14 destinations, 21 routes (14 active, 0 holddown, 0 hidden)
    + = Active Route, - = Last Active, * = Both

    10.17.11.0/25      *[BGP/170] 2d 00:34:04, localpref 150
                          AS path: 12076 I, validation-state: unverified
                        > to 192.168.11.0 via reth1.11
                          to 192.168.11.2 via reth2.11
                        [BGP/170] 2d 00:34:01, localpref 150
                          AS path: 12076 I, validation-state: unverified
                        > to 192.168.11.2 via reth2.11
                        [BGP/170] 2d 21:12:13, localpref 100, from 10.17.11.76
                          AS path: 65515 I, validation-state: unverified
                        > via st0.118
                        [BGP/170] 2d 00:41:51, localpref 100, from 10.17.11.77
                          AS path: 65515 I, validation-state: unverified
                        > via st0.119
    10.17.11.76/32     *[Static/5] 2d 21:12:16
                        > via st0.118
    10.17.11.77/32     *[Static/5] 2d 00:41:56
                        > via st0.119
    10.17.11.128/26    *[BGP/170] 2d 00:34:04, localpref 150
                          AS path: 12076 I, validation-state: unverified
                        > to 192.168.11.0 via reth1.11
                          to 192.168.11.2 via reth2.11
                        [BGP/170] 2d 00:34:01, localpref 150
                          AS path: 12076 I, validation-state: unverified
                        > to 192.168.11.2 via reth2.11
                        [BGP/170] 2d 21:12:13, localpref 100, from 10.17.11.76
                          AS path: 65515 I, validation-state: unverified
                        > via st0.118
                        [BGP/170] 2d 00:41:51, localpref 100, from 10.17.11.77
                          AS path: 65515 I, validation-state: unverified
                        > via st0.119

Aus der obigen Routingtabelle geht hervor, dass für die Hub- und Spoke-VNet-Routen (10.17.11.0/25 und 10.17.11.128/26) die ExpressRoute-Verbindung den VPN-Verbindungen vorgezogen wird. 192.168.11.0 und 192.168.11.2 sind IP-Adressen der Firewallschnittstelle für CE-Router.

## <a name="validation-of-route-exchange-over-s2s-vpn"></a>Überprüfung des Routenaustauschs über S2S-VPN

Weiter oben in diesem Artikel haben wir die lokale Routenankündigung der Firewalls für den primären und den sekundären BGP-Peer des VPN-Gateways überprüft. Außerdem bestätigen wir, dass Azure-Routen von den Firewalls vom primären und sekundären BGP-Peer des VPN-Gateways empfangen werden.

    user@SEA-SRX42-01> show route receive-protocol bgp 10.17.11.76 table Cust11.inet.0 

    Cust11.inet.0: 14 destinations, 21 routes (14 active, 0 holddown, 0 hidden)
      Prefix                  Nexthop              MED     Lclpref    AS path
      10.17.11.0/25           10.17.11.76                             65515 I
      10.17.11.128/26         10.17.11.76                             65515 I

    {primary:node0}
    user@SEA-SRX42-01> show route receive-protocol bgp 10.17.11.77 table Cust11.inet.0    

    Cust11.inet.0: 14 destinations, 21 routes (14 active, 0 holddown, 0 hidden)
      Prefix                  Nexthop              MED     Lclpref    AS path
      10.17.11.0/25           10.17.11.77                             65515 I
      10.17.11.128/26         10.17.11.77                             65515 I

Ebenso überprüfen wir, ob lokale Netzwerkroutenpräfixe vom Azure-VPN-Gateway empfangen werden. 

    PS C:\Users\user> Get-AzVirtualNetworkGatewayLearnedRoute -ResourceGroupName SEA-Cust11 -VirtualNetworkGatewayName SEA-Cust11-VNet01-gw-vpn | where {$_.Network -eq "10.1.11.0/25"} | select Network, NextHop, AsPath, Weight

    Network      NextHop       AsPath      Weight
    -------      -------       ------      ------
    10.1.11.0/25 192.168.11.88 65020        32768
    10.1.11.0/25 10.17.11.76   65020        32768
    10.1.11.0/25 10.17.11.69   12076-65020  32769
    10.1.11.0/25 10.17.11.69   12076-65020  32769
    10.1.11.0/25 192.168.11.88 65020        32768
    10.1.11.0/25 10.17.11.77   65020        32768
    10.1.11.0/25 10.17.11.69   12076-65020  32769
    10.1.11.0/25 10.17.11.69   12076-65020  32769

Wie oben dargestellt, verfügt das VPN-Gateway über Routen, die sowohl vom primären als auch vom sekundären BGP-Peer des VPN-Gateways empfangen werden. Außerdem sind die über primäre und sekundäre ExpressRoute-Verbindungen empfangenen Routen (bei denen „AsPath“ der Wert „12076“ vorangestellt ist) sichtbar. Um die über VPN-Verbindungen empfangenen Routen zu bestätigen, müssen wir die lokale BGP-Peer-IP-Adresse der Verbindungen kennen. In unserem vorliegenden Setup lautet sie 192.168.11.88, und wir sehen die von ihr empfangenen Routen.

Als Nächstes überprüfen wir die Routen, die vom Azure-VPN-Gateway für den BGP-Peer für die lokale Firewall (192.168.11.88) angekündigt werden.

    PS C:\Users\user> Get-AzVirtualNetworkGatewayAdvertisedRoute -Peer 192.168.11.88 -ResourceGroupName SEA-Cust11 -VirtualNetworkGatewayName SEA-Cust11-VNet01-gw-vpn |  select Network, NextHop, AsPath, Weight

    Network         NextHop     AsPath Weight
    -------         -------     ------ ------
    10.17.11.0/25   10.17.11.76 65515       0
    10.17.11.128/26 10.17.11.76 65515       0
    10.17.11.0/25   10.17.11.77 65515       0
    10.17.11.128/26 10.17.11.77 65515       0


Wenn kein Routenaustausch angezeigt wird, deutet dies auf einen Verbindungsfehler hin. Informationen zum Beheben von VPN-Verbindungsproblemen finden Sie unter [Problembehandlung: Azure-Site-to-Site-VPN-Verbindung kann nicht hergestellt werden und reagiert nicht mehr][VPN Troubleshoot].

## <a name="testing-failover"></a>Testen des Failovers

Nachdem wir den erfolgreichen Routenaustausch über die VPN-Verbindung (Steuerungsebene) bestätigt haben, können wir den Datenverkehr (Datenebene) von der ExpressRoute-Konnektivität auf die VPN-Konnektivität umstellen. 

>[!NOTE] 
>In Produktionsumgebungen müssen Failovertests während der geplanten Netzwerkwartungsarbeiten durchgeführt werden, weil sie zu Dienstunterbrechungen führen können.
>

Vor der Umstellung des Datenverkehrs verfolgen wir die Route des aktuellen Pfads in unserem Setup vom lokalen Testserver zum Test-VM im Spoke-VNet.

    C:\Users\PathLabUser>tracert 10.17.11.132

    Tracing route to 10.17.11.132 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.1.11.1
      2    <1 ms    <1 ms    11 ms  192.168.11.0
      3    <1 ms    <1 ms    <1 ms  192.168.11.18
      4     *        *        *     Request timed out.
      5     6 ms     6 ms     5 ms  10.17.11.132

    Trace complete.

Die primäre und die sekundäre ExpressRoute-Verbindung im P2P-Subnetz unseres Setups lauten 192.168.11.16/30 und 192.168.11.20/30. In der obigen Routenverfolgung ist in Schritt 3 zu sehen, dass wir auf 192.168.11.18 stoßen. Dies ist die Schnittstellen-IP des primären MSEE. Das Vorhandensein der MSEE-Schnittstelle bestätigt, dass unser aktueller Pfad wie erwartet über die ExpressRoute-Verbindung verläuft.

Wie im Artikel [Zurücksetzen von ExpressRoute-Verbindungspeerings][RST] beschrieben, verwenden wir die folgenden PowerShell-Befehle, um das primäre und das sekundäre Peering der ExpressRoute-Verbindung zu deaktivieren.

    $ckt = Get-AzExpressRouteCircuit -Name "expressroute name" -ResourceGroupName "SEA-Cust11"
    $ckt.Peerings[0].State = "Disabled"
    Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt

Die Dauer der Umstellung beim Failover hängt von der BGP-Konvergenzzeit ab. In unserem Setup dauert die Umstellung beim Failover einige Sekunden (weniger als 10 Sekunden). Nach der Umstellung wird beim Wiederholen der Routenverfolgung der folgende Pfad angezeigt:

    C:\Users\PathLabUser>tracert 10.17.11.132

    Tracing route to 10.17.11.132 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.1.11.1
      2     *        *        *     Request timed out.
      3     6 ms     7 ms     9 ms  10.17.11.132

    Trace complete.

Das Ergebnis der Routenverfolgung bestätigt, dass die Sicherungsverbindung über S2S-VPN aktiv ist und Dienstkontinuität gewährleisten kann, wenn sowohl die primäre als auch die sekundäre ExpressRoute-Verbindung ausfällt. Um die Failovertests abzuschließen, aktivieren wir die ExpressRoute-Verbindungen wieder und normalisieren den Datenverkehrsfluss mithilfe der folgenden Befehle.

    $ckt = Get-AzExpressRouteCircuit -Name "expressroute name" -ResourceGroupName "SEA-Cust11"
    $ckt.Peerings[0].State = "Enabled"
    Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt

Um zu bestätigen, dass der Datenverkehr wieder auf ExpressRoute umgestellt wird, wiederholen Sie die Routenverfolgung, und stellen Sie sicher, dass die private ExpressRoute-Peeringverbindung verwendet wird.

## <a name="next-steps"></a>Nächste Schritte

ExpressRoute ist für Hochverfügbarkeit ohne einzelne Fehlerquelle im Microsoft-Netzwerk konzipiert. Trotzdem ist eine ExpressRoute-Verbindung auf eine einzige geografische Region und einen Dienstanbieter beschränkt. S2S-VPN kann eine gute passive Sicherungslösung zur Notfallwiederherstellung für eine ExpressRoute-Verbindung sein. Damit eine passive Sicherungsverbindungslösung zuverlässig funktioniert, sind die regelmäßige Wartung der passiven Konfiguration und die periodische Überprüfung der Verbindung wichtig. Sie müssen unbedingt darauf achten, dass die VPN-Konfiguration nicht veraltet und dass die in diesem Artikel beschriebenen Schritte zur Überprüfung und zum Durchführen von Failovertests während des Wartungsfensters regelmäßig (z. B. vierteljährlich) wiederholt werden.

Informationen zum Aktivieren der Überwachung und von Warnungen basierend auf den Metriken des VPN-Gateways finden Sie unter [Einrichten von Warnungen zu VPN Gateway-Metriken][VPN-alerts].

Um die BGP-Konvergenz nach einem ExpressRoute-Fehler zu beschleunigen, [konfigurieren Sie BFD über ExpressRoute][BFD].

<!--Image References-->
[1]: ./media/use-s2s-vpn-as-backup-for-expressroute-privatepeering/topology.png "Beispieltopologie"
[2]: ./media/use-s2s-vpn-as-backup-for-expressroute-privatepeering/vpn-gw-config.png "VPN-GW – Konfiguration"

<!--Link References-->
[DR-PP]: https://docs.microsoft.com/azure/expressroute/designing-for-disaster-recovery-with-expressroute-privatepeering
[Conf-CoExist]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-coexist-resource-manager
[HA]: https://docs.microsoft.com/azure/expressroute/designing-for-high-availability-with-expressroute
[VPN Troubleshoot]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-troubleshoot-site-to-site-cannot-connect
[VPN-alerts]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric
[BFD]: https://docs.microsoft.com/azure/expressroute/expressroute-bfd
[RST]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-reset-peering



