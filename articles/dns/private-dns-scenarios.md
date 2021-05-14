---
title: Szenarien für private Zonen – Azure DNS
description: In diesem Artikel lernen Sie allgemeine Szenarien für das Verwenden von Azure DNS Private Zones kennen.
services: dns
author: rohinkoul
ms.service: dns
ms.topic: article
ms.date: 04/27/2021
ms.author: rohink
ms.openlocfilehash: 5a2572af0fd312efeacb8544b653db1b35809244
ms.sourcegitcommit: 4a54c268400b4158b78bb1d37235b79409cb5816
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108127819"
---
# <a name="azure-dns-private-zones-scenarios"></a>Azure DNS Private Zones: Szenarien

Azure DNS Private Zones bieten Namensauflösung in einem virtuellen Netzwerk und zwischen virtuellen Netzwerken. In diesem Artikel werden einige allgemeine Szenarien untersucht, die von diesem Feature profitieren können.

## <a name="scenario-name-resolution-scoped-to-a-single-virtual-network"></a>Szenario: Namensauflösung für ein einzelnes virtuelles Netzwerk

In diesem Szenario haben Sie ein virtuelles Netzwerk in Azure, das viele Ressourcen enthält, einschließlich virtueller Computer. Sie müssen alle Ressourcen im virtuellen Netzwerk mithilfe eines bestimmten Domänennamens (DNS-Zone) auflösen. Außerdem muss die Namensauflösung privat und nicht über das Internet zugänglich sein. Schließlich muss Azure noch VMs automatisch in der DNS-Zone registrieren.

Dieses Szenario ist nachfolgend dargestellt. Sie haben ein virtuelles Netzwerk namens „A“, das zwei VMs (VNETA-VM1 und VNETA-VM2) enthält. Jeder VM ist eine private IP-Adresse zugeordnet. Nachdem Sie eine private Zone (z. B. `contoso.com`) erstellt und das virtuelle Netzwerk „A“ als virtuelles Registrierungsnetzwerk verknüpft haben, erstellt Azure DNS automatisch zwei A-Einträge in der Zone, die auf die beiden VMs verweisen. DNS-Abfragen von VNETA-VM1 können jetzt `VNETA-VM2.contoso.com` auflösen und erhalten eine DNS-Antwort, die die private IP-Adresse von VNETA-VM2 enthält.
Sie können auch eine Reverse-DNS-Abfrage (PTR) für die private IP-Adresse der VNETA-VM1 (10.0.0.1) von VNETA-VM2 ausführen. Die DNS-Antwort enthält wie erwartet den Namen VNETA-VM1. 

![Auflösung für einzelnes virtuelles Netzwerk](./media/private-dns-scenarios/single-vnet-resolution.png)

## <a name="scenario-name-resolution-across-virtual-networks"></a>Szenario: Namensauflösung bei mehreren virtuellen Netzwerken

In diesem Szenario müssen Sie mehreren virtuellen Netzwerken eine private Zone zuordnen. Sie können diese Lösung in verschiedenen Netzwerkarchitekturen wie dem Hub-and-Spoke-Modell implementieren. Diese Konfiguration liegt vor, wenn ein zentrales virtuelles Hubnetzwerk verwendet wird, um mehrere virtuelle Spoke-Netzwerke miteinander zu verbinden. Das zentrale virtuelle Hubnetzwerk kann als das virtuelle Registrierungsnetzwerk und die virtuellen Spoke-Netzwerke können als virtuelle Auflösungsnetzwerke verknüpft werden. 

Im folgenden Diagramm ist eine vereinfachte Version dieses Szenarios mit nur zwei virtuellen Netzwerken (A und B) dargestellt. A ist als ein virtuelles Registrierungsnetzwerk und B als ein virtuelles Auflösungsnetzwerk definiert. Vorgesehen ist, dass für beide virtuellen Netzwerke eine gemeinsame Zone `contoso.com` verwendet wird. Wenn die Zone erstellt wird, registrieren virtuelle Netzwerke, die als Registrierungsnetzwerke definiert sind, automatisch DNS-Einträge für VMs im virtuellen Netzwerk (VNETA-VM1 und VNETA-VM2). Sie können auch manuell DNS-Einträge in der Zone für VMs im virtuellen Auflösungsnetzwerk B hinzufügen. Bei dieser Konfiguration ergibt sich das folgende Verhalten für Weiterleitungs- und Reverse-DNS-Abfragen:
* Eine DNS-Abfrage von VNETB-VM1 im virtuellen Auflösungsnetzwerk B für „VNETA-VM1.contoso.com“ erhält eine DNS-Antwort, die die private IP-Adresse von VNETA-VM1 enthält.
* Eine Reverse-DNS-Abfrage (PTR-Abfrage) von VNETB-VM2 im virtuellen Auflösungsnetzwerk B für 10.1.0.1 erhält eine DNS-Antwort, die „FQDN VNETB-VM1.contoso.com“ enthält.  
* Eine Reverse-DNS-Abfrage (PTR-Abfrage) von VNETB-VM3 im virtuellen Auflösungsnetzwerk B für 10.0.0.1 erhält NXDOMAIN. Der Grund ist, dass Reverse-DNS-Abfragen auf lediglich dasselbe virtuelle Netzwerk beschränkt sind. 

![Mehrere Auflösungen für ein virtuelles Netzwerk](./media/private-dns-scenarios/multi-vnet-resolution.png)

## <a name="scenario-split-horizon-functionality"></a>Szenario: Split-Horizon-Funktionalität

In diesem Szenario benötigen Sie für die gleiche DNS-Zone eine unterschiedliche Namensauflösung, je nachdem, wo sich der Client befindet. Möglicherweise verfügen Sie über eine private und eine öffentliche Version Ihrer Anwendung, die jeweils unterschiedliche Funktionen oder Verhaltensweisen aufweisen. Sie müssen aber für beide Versionen denselben Domänennamen verwenden. Dieses Szenario kann durch Erstellen einer öffentlichen und privaten Zone in Azure DNS mit demselben Namen erreicht werden. 

Im folgenden Diagramm ist dieses Szenario dargestellt. Sie haben ein virtuelles Netzwerk A mit zwei VMs (VNETA-VM1 und VNETA-VM2). Für beide ist eine private IP-Adresse und eine öffentliche IP-Adresse konfiguriert. Es wurde eine öffentliche DNS-Zone namens `contoso.com` erstellt, und die öffentlichen IP-Adressen für diese VMs werden als DNS-Einträge in der Zone registriert. Es wird auch eine private DNS-Zone mit dem Namen `contoso.com` erstellt. Sie haben das virtuelle Netzwerk A als ein virtuelles Registrierungsnetzwerk definiert. Azure registriert dann die VMs automatisch als A-Einträge in der privaten Zone, die auf ihre privaten IP-Adressen verweisen.

Wenn nun ein Internetclient eine DNS-Abfrage für `VNETA-VM1.contoso.com` durchführt, gibt Azure den öffentlichen IP-Eintrag aus der öffentlichen Zone zurück. Wird die gleiche DNS-Abfrage von einem anderen virtuellen Computer (z. B. VNETA-VM2) im selben virtuellen Netzwerk gesendet, gibt Azure den privaten IP-Eintrag aus der privaten Zone zurück. 

![Split-Horizon-Auflösung](./media/private-dns-scenarios/split-brain-resolution.png)

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu privaten DNS-Zonen finden Sie unter [Using Azure DNS for private domains](private-dns-overview.md) (Verwenden von Azure DNS für private Domänen).

Erfahren Sie, wie Sie in Azure DNS [eine private DNS-Zone erstellen](./private-dns-getstarted-powershell.md).

Erfahren Sie mehr zu DNS-Zonen und -Einträgen im folgenden Artikel: [DNS-Zonen und -Einträge: Übersicht](dns-zones-records.md).

Erfahren Sie mehr über die anderen zentralen [Netzwerkfunktionen](../networking/fundamentals/networking-overview.md) von Azure.
