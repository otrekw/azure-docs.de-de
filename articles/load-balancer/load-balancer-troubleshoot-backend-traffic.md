---
title: Beheben von Problemen mit Azure Load Balancer
description: Hier erfahren Sie, wie Sie bekannte Probleme mit Azure Load Balancer beheben.
services: load-balancer
documentationcenter: na
author: asudbring
manager: dcscontentpm
ms.custom: seodoc18
ms.service: load-balancer
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/28/2020
ms.author: allensu
ms.openlocfilehash: bfe2b21a86f2ce4b4630ba69cde87796fd367f4b
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "98029143"
---
# <a name="troubleshoot-azure-load-balancer-backend-traffic-responses"></a>Problembehandlung für Azure Load Balancer-Antworten auf Back-End-Datenverkehr

Diese Seite enthält Informationen zur Problembehandlung für Fragen zu Azure Load Balancer.

## <a name="vms-behind-load-balancer-are-not-responding-to-traffic-on-the-configured-data-port"></a>VMs hinter dem Load Balancer antworten nicht auf Datenverkehr am konfigurierten Datenport

Wenn eine Back-End-Pool-VM als fehlerfrei aufgeführt ist und auf die Integritättests antwortet, jedoch noch immer nicht am Lastenausgleich teilnimmt oder nicht auf den Datenverkehr antwortet, kann einer der folgenden Gründe vorliegen:
* Die VM des Load Balancer-Back-End-Pools lauscht nicht am Datenport
* Eine Netzwerksicherheitsgruppe blockiert den Port auf der VM des Load Balancer-Back-End-Pools  
* Zugreifen auf den Load Balancer über die gleiche VM und NIC
* Zugreifen auf das Internet Load Balancer-Front-End über die beteiligte VM des Load Balancer-Back-End-Pools

## <a name="cause-1-load-balancer-backend-pool-vm-is-not-listening-on-the-data-port"></a>Ursache 1: Die VM des Load Balancer-Back-End-Pools lauscht nicht am Datenport

Wenn eine VM nicht auf den Datenverkehr antwortet, kann dies daran liegen, dass der Zielport auf der beteiligten VM nicht geöffnet ist oder dass die VM nicht an diesem Port lauscht. 

**Überprüfung und Lösung**

1. Melden Sie sich an der Back-End-VM an. 
2. Öffnen Sie eine Eingabeaufforderung, und führen Sie den folgenden Befehl aus, um zu überprüfen, ob eine Anwendung am Datenport lauscht:  
            netstat -an 
3. Wenn der Port nicht mit dem Status „EMPFANGSBEREIT“ aufgeführt ist, konfigurieren Sie den richtigen Listenerport. 
4. Wenn der Port als „Empfangsbereit“ markiert ist, überprüfen Sie die Zielanwendung an diesem Port auf mögliche Probleme.

## <a name="cause-2-network-security-group-is-blocking-the-port-on-the-load-balancer-backend-pool-vm"></a>Ursache 2: Eine Netzwerksicherheitsgruppe blockiert den Port auf der VM des Load Balancer-Back-End-Pools  

Wenn eine oder mehrere im Subnetz oder auf der VM konfigurierte Netzwerksicherheitsgruppen die Quell-IP oder den Port blockieren, kann die VM nicht antworten.

Beim öffentlichen Load Balancer wird für die Kommunikation zwischen den Clients und den Back-End-VMs des Load Balancers die IP-Adresse der Internetclients verwendet. Stellen Sie sicher, dass die IP-Adressen der Clients in der Netzwerksicherheitsgruppe der Back-End-VMs zulässig sind.

1. Listen Sie die Netzwerksicherheitsgruppen auf, die auf der Back-End-VM konfiguriert sind. Weitere Informationen finden Sie unter [Verwalten von Netzwerksicherheitsgruppen](../virtual-network/manage-network-security-group.md).
1. Überprüfen Sie Folgendes anhand der Liste von Netzwerksicherheitsgruppen:
    - Beim eingehenden oder ausgehenden Datenverkehr am Datenport tritt eine Störung auf. 
    - Eine Regel **Alle verweigern** für Netzwerksicherheitsgruppen der NIC der VM oder des Subnetzes weist eine höhere Priorität als die Standardregel auf, die Load Balancer-Tests und -Datenverkehr zulässt (Netzwerksicherheitsgruppen müssen die Load Balancer-IP-Adresse 168.63.129.16 (Testport) zulassen).
1. Wenn eine der Regeln den Datenverkehr blockiert, entfernen und konfigurieren Sie diese Regeln, um den Datenverkehr zuzulassen.  
1. Testen Sie, ob die VM jetzt auf Integritätstests antwortet.

## <a name="cause-3-accessing-the-load-balancer-from-the-same-vm-and-network-interface"></a>Ursache 3: Der Zugriff auf den Load Balancer erfolgt über die gleiche VM und Netzwerkschnittstelle 

Wenn die auf der Back-End-VM eines Load Balancers gehostete Anwendung versucht, auf eine andere Anwendung zuzugreifen, die auf der gleichen Back-End-VM gehostet wird und die gleiche Netzwerkschnittstelle verwendet, ist dies ein nicht unterstütztes Szenario, das fehl schlägt. 

**Lösung:** Sie können dieses Problem mit einer der folgenden Methoden beheben:
* Konfigurieren Sie separate Back-End-Pool-VMs für jede Anwendung. 
* Konfigurieren Sie die Anwendung in VMs mit zwei NICs, damit jede Anwendung eine eigene Netzwerkschnittstelle und IP-Adresse verwendet. 

## <a name="cause-4-accessing-the-internal-load-balancer-frontend-from-the-participating-load-balancer-backend-pool-vm"></a>Ursache 4: Der Zugriff auf das Front-End des internen Load Balancers erfolgt über die beteiligte VM des Load Balancer-Back-End-Pools

Wenn ein interner Load Balancer innerhalb eines VNet konfiguriert wird und eine der teilnehmenden Back-End-VMs versucht, auf das Front-End des internen Load Balancers zuzugreifen, treten möglicherweise Fehler auf, wenn der Flow der Ausgangs-VM zugeordnet wird. Dieses Szenario wird nicht unterstützt.

**Auflösung:** Es gibt mehrere Möglichkeiten, die Blockierung für dieses Szenarios aufzuheben, einschließlich der Verwendung eines Proxys. Evaluieren Sie Application Gateway oder andere Proxys von Drittanbietern (z.B. nginx oder haproxy). Weitere Informationen zu Application Gateway finden Sie unter [Übersicht über Application Gateway](../application-gateway/overview.md).

**Details:** Interne Lastenausgleichsmodule führen für ausgehende Verbindungen keine Übersetzung für das Front-End eines internen Lastenausgleichsmoduls durch, da sich beide im privaten IP-Adressraum befinden. Über öffentliche Load Balancer werden [ausgehende Verbindungen](load-balancer-outbound-connections.md) von privaten IP-Adressen im virtuellen Netzwerk mit öffentlichen IP-Adressen bereitgestellt. Bei internen Load Balancern wird mit diesem Ansatz die potenzielle SNAT-Portüberlastung in einem eindeutigen internen IP-Adressraum vermieden, für den die Übersetzung nicht erforderlich ist.

Hierbei ergibt sich die folgende Nebenwirkung: Wenn ein von einem virtuellen Computer ausgehender Flow in den Back-End-Pool versucht, einen Flow zum Front-End der internen Load Balancer-Instanz im Pool auszuführen _und_ sich selbst zugewiesen ist, stimmen die beiden Verzweigungen des Flows nicht überein. Aufgrund dieser fehlenden Übereinstimmung tritt für den Flow ein Fehler auf. Der Flow ist erfolgreich, wenn er nicht dem virtuellen Computer im Back-End-Pool zugeordnet ist, die den Flow zum Front-End erstellt hat.

Wenn der Flow sich selbst zugeordnet ist, geht der ausgehende Flow zum Front-End scheinbar vom virtuellen Computer und der zugehörige eingehende Flow scheinbar vom virtuellen Computer an sich selbst aus. Aus Sicht des Gastbetriebssystems stimmen die eingehenden und ausgehenden Bestandteile desselben Flows innerhalb des virtuellen Computers nicht überein. Der TCP-Stack erkennt nicht, dass es sich bei diesen Hälften um Teile desselben Flows handelt. Die Quelle und das Ziel stimmen nicht überein. Wenn der Flow keinem anderen virtuellen Computer im Back-End-Pool zugeordnet ist, stimmen die Teile des Flows überein, und der virtuelle Computer kann auf den Flow antworten.

Das Symptom für dieses Szenario sind zeitweilige Verbindungstimeouts, die auftreten, wenn der Flow zu demselben Back-End zurückkehrt, vom dem er ursprünglich stammt. Zu den häufig genutzten Problemumgehungen gehören das Einfügen einer Proxyebene hinter dem internen Load Balancer und das Verwenden von DSR-Stilregeln (Direct Server Return). Weitere Informationen finden Sie unter [Mehrere Front-Ends für Azure Load Balancer](load-balancer-multivip-overview.md).

Sie können eine interne Load Balancer-Instanz mit dem Proxy eines Drittanbieters kombinieren oder die interne [Application Gateway](../application-gateway/overview.md)-Instanz für Proxyszenarien mit HTTP/HTTPS verwenden. Es ist zwar möglich, zum Beheben dieses Problems einen öffentlichen Load Balancer zu nutzen, aber das sich ergebende Szenario ist anfällig für [SNAT-Auslastung](load-balancer-outbound-connections.md). Nutzen Sie diesen zweiten Ansatz nur, wenn eine sorgfältige Verwaltung durchgeführt werden kann.

## <a name="next-steps"></a>Nächste Schritte

Sollte sich das Problem mit den oben genannten Schritten nicht beheben lassen, erstellen Sie ein [Supportticket](https://azure.microsoft.com/support/options/).