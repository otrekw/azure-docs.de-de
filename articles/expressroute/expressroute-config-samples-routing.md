---
title: 'Azure ExpressRoute: Beispiele für die Routerkonfiguration'
description: Verwenden Sie diese Schnittstellen- und Routingkonfigurationsbeispiele für Cisco IOS XE-Router und Juniper-Router der MX-Serie als Beispiele für die Nutzung von Azure ExpressRoute.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: article
ms.date: 03/26/2020
ms.author: duau
ms.openlocfilehash: 3bc850f02884ae0547c2ecf56a46a57a4e66a752
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "89397405"
---
# <a name="router-configuration-samples-to-set-up-and-manage-routing"></a>Beispiele für die Routerkonfiguration zum Einrichten und Verwalten des Routings
Auf dieser Seite finden Sie Schnittstellen- und Routingkonfigurationsbeispiele für Cisco IOS XE-Router und Juniper-Router der MX-Serie, wenn Sie mit Azure ExpressRoute arbeiten.

> [!IMPORTANT]
> Die Beispiele auf dieser Seite dienen lediglich als Orientierungshilfe. Sie müssen mit dem Vertriebs-/Technikteam Ihres Anbieters und Ihrem Netzwerkteam zusammenarbeiten, um geeignete Konfigurationen zu ermitteln, die Ihre Anforderungen erfüllen. Microsoft bietet keine Unterstützung bei Problemen im Zusammenhang mit Konfigurationen, die auf dieser Seite aufgeführt sind. Wenden Sie sich an den Gerätehersteller, wenn Sie Unterstützung bei Problemen benötigen.
> 
> 

## <a name="mtu-and-tcp-mss-settings-on-router-interfaces"></a>Einstellungen für MTU und TCP MSS für Routerschnittstellen
Die maximale Übertragungseinheit (Maximum Transmission Unit, MTU) für die ExpressRoute-Schnittstelle ist 1500. Dies ist der übliche MTU-Standardwert für eine Ethernet-Schnittstelle eines Routers. Nur wenn Ihr Router standardmäßig einen anderen MTU-Wert aufweist, müssen Sie einen Wert für die Routerschnittstelle angeben.

Anders als bei einem Azure-VPN-Gateway muss die maximale TCP-Segmentgröße (Maximum Segment Size, MSS) für eine ExpressRoute-Verbindung nicht angegeben werden.

Die in diesem Artikel enthaltenen Routerkonfigurationsbeispiele gelten für alle Peerings. Unter [ExpressRoute-Peerings](expressroute-circuit-peerings.md) und [Routinganforderungen für ExpressRoute](expressroute-routing.md) finden Sie weitere Details.


## <a name="cisco-ios-xe-based-routers"></a>Cisco IOS-XE-basierte Router
Die Beispiele in diesem Abschnitt beziehen sich auf Router, auf denen die IOS XE-Betriebssystemfamilie ausgeführt wird.

### <a name="configure-interfaces-and-subinterfaces"></a>Konfigurieren von Schnittstellen und Unterschnittstellen
Sie benötigen auf jedem Router, den Sie mit Microsoft verbinden, eine Unterschnittstelle pro Peering. Eine Unterschnittstelle kann anhand einer VLAN-ID oder eines gestapelten Paars von VLAN-IDs und einer IP-Adresse identifiziert werden.

**Dot1Q-Schnittstellendefinition**

Dieses Beispiel stellt die Unterschnittstellendefinition für eine Unterschnittstelle mit einer einzigen VLAN-ID bereit. Die VLAN-ID ist pro Peering eindeutig. Das letzte Oktett Ihrer IPv4-Adresse ist immer eine ungerade Zahl.

```console
interface GigabitEthernet<Interface_Number>.<Number>
 encapsulation dot1Q <VLAN_ID>
 ip address <IPv4_Address><Subnet_Mask>
```

**QinQ-Schnittstellendefinition**

Dieses Beispiel stellt die Unterschnittstellendefinition für eine Unterschnittstelle mit zwei VLAN-IDs bereit. Falls die äußere VLAN-ID (s-Tag) verwendet wird, bleibt sie für alle Peerings gleich. Die innere VLAN-ID (c-Tag) ist pro Peering eindeutig. Das letzte Oktett Ihrer IPv4-Adresse ist immer eine ungerade Zahl.

```console
interface GigabitEthernet<Interface_Number>.<Number>
 encapsulation dot1Q <s-tag> seconddot1Q <c-tag>
 ip address <IPv4_Address><Subnet_Mask>
```

### <a name="set-up-ebgp-sessions"></a>Einrichten von eBGP-Sitzungen
Sie müssen für jedes Peering eine BGP-Sitzung bei Microsoft einrichten. Richten Sie eine BGP-Sitzung anhand des folgenden Beispiels ein. Wenn Sie für Ihre Unterschnittstelle die IPv4-Adresse „a.b.c.d“ verwendet haben, lautet die IP-Adresse des BGP-Nachbarn (Microsoft) „a.b.c.d+1“. Das letzte Oktett in der IPv4-Adresse des BGP-Nachbarn ist immer eine gerade Zahl.

```console
router bgp <Customer_ASN>
 bgp log-neighbor-changes
 neighbor <IP#2_used_by_Azure> remote-as 12076
 !
 address-family ipv4
 neighbor <IP#2_used_by_Azure> activate
 exit-address-family
!
```

### <a name="set-up-prefixes-to-be-advertised-over-the-bgp-session"></a>Einrichten von Präfixen für die Ankündigung über die BGP-Sitzung
Konfigurieren Sie Ihren Router anhand des folgenden Beispiels so, dass Microsoft bestimmte Präfixe angekündigt werden.

```console
router bgp <Customer_ASN>
 bgp log-neighbor-changes
 neighbor <IP#2_used_by_Azure> remote-as 12076
 !
 address-family ipv4
  network <Prefix_to_be_advertised> mask <Subnet_mask>
  neighbor <IP#2_used_by_Azure> activate
 exit-address-family
!
```

### <a name="route-maps"></a>Routenzuordnungen
Verwenden Sie Routenzuordnungen und Präfixlisten, um Präfixe zu filtern, die in Ihrem Netzwerk weitergegeben werden. Orientieren Sie sich am folgenden Beispiel, und stellen Sie sicher, dass Sie die entsprechenden Präfixlisten eingerichtet haben.

```console
router bgp <Customer_ASN>
 bgp log-neighbor-changes
 neighbor <IP#2_used_by_Azure> remote-as 12076
 !
 address-family ipv4
  network <Prefix_to_be_advertised> mask <Subnet_mask>
  neighbor <IP#2_used_by_Azure> activate
  neighbor <IP#2_used_by_Azure> route-map <MS_Prefixes_Inbound> in
 exit-address-family
!
route-map <MS_Prefixes_Inbound> permit 10
 match ip address prefix-list <MS_Prefixes>
!
```

### <a name="configure-bfd"></a>Konfigurieren von BFD

BFD wird an zwei Stellen konfiguriert: BFD auf der Schnittstellenebene und BFD auf BGP-Ebene. Das hier aufgeführte Beispiel gilt für die QinQ-Schnittstelle. 

```console
interface GigabitEthernet<Interface_Number>.<Number>
 bfd interval 300 min_rx 300 multiplier 3
 encapsulation dot1Q <s-tag> seconddot1Q <c-tag>
 ip address <IPv4_Address><Subnet_Mask>

router bgp <Customer_ASN>
 bgp log-neighbor-changes
 neighbor <IP#2_used_by_Azure> remote-as 12076
 !
 address-family ipv4
  neighbor <IP#2_used_by_Azure> activate
  neighbor <IP#2_used_by_Azure> fall-over bfd
 exit-address-family
!
```


## <a name="juniper-mx-series-routers"></a>Router der Juniper MX-Serie
Die Beispiele in diesem Abschnitt gelten für Juniper-Router der MX-Serie.

### <a name="configure-interfaces-and-subinterfaces"></a>Konfigurieren von Schnittstellen und Unterschnittstellen

**Dot1Q-Schnittstellendefinition**

Dieses Beispiel stellt die Unterschnittstellendefinition für eine Unterschnittstelle mit einer einzigen VLAN-ID bereit. Die VLAN-ID ist pro Peering eindeutig. Das letzte Oktett Ihrer IPv4-Adresse ist immer eine ungerade Zahl.

```console
    interfaces {
        vlan-tagging;
        <Interface_Number> {
            unit <Number> {
                vlan-id <VLAN_ID>;
                family inet {
                    address <IPv4_Address/Subnet_Mask>;
                }
            }
        }
    }
```


**QinQ-Schnittstellendefinition**

Dieses Beispiel stellt die Unterschnittstellendefinition für eine Unterschnittstelle mit zwei VLAN-IDs bereit. Falls die äußere VLAN-ID (s-Tag) verwendet wird, bleibt sie für alle Peerings gleich. Die innere VLAN-ID (c-Tag) ist pro Peering eindeutig. Das letzte Oktett Ihrer IPv4-Adresse ist immer eine ungerade Zahl.

```console
    interfaces {
        <Interface_Number> {
            flexible-vlan-tagging;
            unit <Number> {
                vlan-tags outer <S-tag> inner <C-tag>;
                family inet {
                    address <IPv4_Address/Subnet_Mask>;
                }                           
            }                               
        }                                   
    }                           
```

### <a name="set-up-ebgp-sessions"></a>Einrichten von eBGP-Sitzungen
Sie müssen für jedes Peering eine BGP-Sitzung bei Microsoft einrichten. Richten Sie eine BGP-Sitzung anhand des folgenden Beispiels ein. Wenn Sie für Ihre Unterschnittstelle die IPv4-Adresse „a.b.c.d“ verwendet haben, lautet die IP-Adresse des BGP-Nachbarn (Microsoft) „a.b.c.d+1“. Das letzte Oktett in der IPv4-Adresse des BGP-Nachbarn ist immer eine gerade Zahl.

```console
    routing-options {
        autonomous-system <Customer_ASN>;
    }
    }
    protocols {
        bgp { 
            group <Group_Name> { 
                peer-as 12076;              
                neighbor <IP#2_used_by_Azure>;
            }                               
        }                                   
    }
```

### <a name="set-up-prefixes-to-be-advertised-over-the-bgp-session"></a>Einrichten von Präfixen für die Ankündigung über die BGP-Sitzung
Konfigurieren Sie Ihren Router anhand des folgenden Beispiels so, dass Microsoft bestimmte Präfixe angekündigt werden.

```console
    policy-options {
        policy-statement <Policy_Name> {
            term 1 {
                from protocol OSPF;
                route-filter; 
                <Prefix_to_be_advertised/Subnet_Mask> exact;
                then {
                    accept;
                }
            }
        }
    }
    protocols {
        bgp { 
            group <Group_Name> { 
                export <Policy_Name>;
                peer-as 12076;              
                neighbor <IP#2_used_by_Azure>;
            }                               
        }                                   
    }
```

### <a name="route-policies"></a>Routenrichtlinien
Sie können Routenzuordnungen und Präfixlisten verwenden, um Präfixe zu filtern, die in Ihrem Netzwerk weitergegeben werden. Orientieren Sie sich am folgenden Beispiel, und stellen Sie sicher, dass Sie die entsprechenden Präfixlisten eingerichtet haben.

```console
    policy-options {
        prefix-list MS_Prefixes {
            <IP_Prefix_1/Subnet_Mask>;
            <IP_Prefix_2/Subnet_Mask>;
        }
        policy-statement <MS_Prefixes_Inbound> {
            term 1 {
                from {
                    prefix-list MS_Prefixes;
                }
                then {
                    accept;
                }
            }
        }
    }
    protocols {
        bgp { 
            group <Group_Name> { 
                export <Policy_Name>;
                import <MS_Prefixes_Inbound>;
                peer-as 12076;              
                neighbor <IP#2_used_by_Azure>;
            }                               
        }                                   
    }
```

### <a name="configure-bfd"></a>Konfigurieren von BFD
Konfigurieren Sie BFD nur unter dem Abschnitt „protocols“ > „bgp“.

```console
    protocols {
        bgp { 
            group <Group_Name> { 
                peer-as 12076;              
                neighbor <IP#2_used_by_Azure>;
                bfd-liveness-detection {
                       minimum-interval 3000;
                       multiplier 3;
                }
            }                               
        }                                   
    }
```

### <a name="configure-macsec"></a>Konfigurieren von MACsec
Bei der MACSec-Konfiguration müssen der Konnektivitätszuordnungsschlüssel (Key Association Key, CAK) und der Konnektivitätszuordnungsschlüssel-Name (Connectivity Association Key Name, CKN) mit über PowerShell-Befehle konfigurierten Werten übereinstimmen.

```console
    security {
        macsec {
            connectivity-association <Connectivity_Association_Name> {
                cipher-suite gcm-aes-xpn-128;
                security-mode static-cak;
                pre-shared-key {
                    ckn <Connectivity_Association_Key_Name>;
                    cak <Connectivity_Association_Key>; ## SECRET-DATA
                }
            }
            interfaces {
                <Interface_Number> {
                    connectivity-association <Connectivity_Association_Name>;
                }
            }
        }
    }
```

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie unter [ExpressRoute – FAQ](expressroute-faqs.md) .



