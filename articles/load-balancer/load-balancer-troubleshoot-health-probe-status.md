---
title: Beheben von Problemen mit dem Azure Load Balancer-Integritätsteststatus
description: Hier erfahren Sie, wie Sie bekannte Probleme mit dem Azure Load Balancer-Integritätsteststatus beheben.
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
ms.date: 12/02/2020
ms.author: allensu
ms.openlocfilehash: 28823c997cd974d5061829df88680ed52075caa0
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "98029145"
---
# <a name="troubleshoot-azure-load-balancer-health-probe-status"></a>Beheben von Problemen mit dem Azure Load Balancer-Integritätsteststatus

Diese Seite enthält Informationen zur Problembehandlung für häufige Fragen zum Azure Load Balancer-Integritätstest.

## <a name="symptom-vms-behind-the-load-balancer-are-not-responding-to-health-probes"></a>Symptom: VMs hinter dem Load Balancer antworten nicht auf Integritätstests
Damit Back-End-Server Teil einer Load Balancer-Gruppe sein können, müssen sie die Überprüfung bestehen. Weitere Informationen zu Integritätstests finden Sie unter [Grundlegendes zu Load Balancer-Tests](load-balancer-custom-probe-overview.md). 

Die VMs des Load Balancer-Back-End-Pools antworten möglicherweise aus einem der folgenden Gründe nicht auf die Tests: 
- Die VM des Load Balancer-Back-End-Pools ist fehlerhaft 
- Die VM des Load Balancer-Back-End-Pools lauscht nicht am Testport 
- Die Firewall oder eine Netzwerksicherheitsgruppe blockiert den Port auf den VMs des Load Balancer-Back-End-Pools 
- Andere Fehlkonfigurationen im Load Balancer

### <a name="cause-1-load-balancer-backend-pool-vm-is-unhealthy"></a>Ursache 1: Die VM des Load Balancer-Back-End-Pools ist fehlerhaft

**Überprüfung und Lösung**

Um dieses Problem zu beheben, melden Sie sich an den beteiligten VMs an, und überprüfen Sie, ob der VM-Status fehlerfrei lautet und ob sie auf **PsPing** oder **TCPing** von einer anderen VM im Pool antworten können. Wenn die VM fehlerhaft ist oder nicht auf den Test antworten kann, müssen Sie das Problem beheben und die VM wieder in einen fehlerfreien Status versetzen, bevor sie am Lastenausgleich teilnehmen kann.

### <a name="cause-2-load-balancer-backend-pool-vm-is-not-listening-on-the-probe-port"></a>Ursache 2: Die VM des Load Balancer-Back-End-Pools lauscht nicht am Testport
Wenn die VM fehlerfrei ist, aber nicht auf den Test antwortet, könnte eine mögliche Ursache sein, dass der Testport auf der entsprechenden VM nicht geöffnet ist oder dass die VM nicht an diesem Port lauscht.

**Überprüfung und Lösung**

1. Melden Sie sich an der Back-End-VM an.
2. Öffnen Sie eine Eingabeaufforderung, und führen Sie den folgenden Befehl aus, um zu überprüfen, ob eine Anwendung am Testport lauscht:          netstat -an
3. Wenn der Portstatus nicht als **EMPFANGSBEREIT** aufgeführt ist, konfigurieren Sie den richtigen Port. 
4. Wählen Sie alternativ einen anderen Port aus, der als **EMPFANGSBEREIT** aufgeführt wird, und aktualisieren Sie die Load Balancer-Konfiguration entsprechend.

### <a name="cause-3-firewall-or-a-network-security-group-is-blocking-the-port-on-the-load-balancer-backend-pool-vms"></a>Ursache 3: Die Firewall oder eine Netzwerksicherheitsgruppe blockiert den Port auf den VMs des Load Balancer-Back-End-Pools
Wenn die Firewall auf der VM den Testport blockiert oder eine oder mehrere Netzwerksicherheitsgruppen, die für das Subnetz oder die VM konfiguriert sind, nicht zulassen, dass der Test den Port erreicht, kann die VM nicht auf den Integritätstest antworten.

**Überprüfung und Lösung**

1. Wenn die Firewall aktiviert ist, überprüfen Sie, ob sie so konfiguriert ist, dass der Testport zulässig ist. Wenn dies nicht der Fall ist, konfigurieren Sie die Firewall zum Zulassen von Datenverkehr am Testport, und führen Sie den Test erneut aus.
2. Überprüfen Sie anhand der Liste der Netzwerksicherheitsgruppen, ob beim eingehenden oder ausgehenden Datenverkehr am Testport Störungen auftreten.
3. Überprüfen Sie auch, ob eine Regel **Alle verweigern** für Netzwerksicherheitsgruppen der NIC der VM oder des Subnetzes eine höhere Priorität als die Standardregel aufweist, die Load Balancer-Tests und -Datenverkehr zulässt (Netzwerksicherheitsgruppen müssen die Load Balancer-IP-Adresse 168.63.129.16 zulassen).
4. Wenn eine dieser Regeln den Testdatenverkehr blockiert, entfernen und konfigurieren Sie die Regeln, um den Testdatenverkehr zuzulassen.  
5. Testen Sie, ob die VM jetzt auf Integritätstests antwortet.

### <a name="cause-4-other-misconfigurations-in-load-balancer"></a>Ursache 4: Andere Fehlkonfigurationen im Load Balancer
Wenn alle vorhergehenden Ursachen anscheinend ordnungsgemäß überprüft und aufgelöst wurden und die Back-End-VM noch immer nicht auf Integritätstests antwortet, dann testen Sie die Konnektivität manuell, und erfassen Sie einige Ablaufverfolgungen, um die Konnektivität zu analysieren.

**Überprüfung und Lösung**

1. Verwenden Sie **Psping** von einer der anderen VMs im VNET, um die Testportantwort zu testen (Beispiel: .\psping.exe -t 10.0.0.4:3389), und zeichnen Sie die Ergebnisse auf. 
2. Verwenden Sie **TCPing** von einer der anderen VMs im VNET, um die Testportantwort zu testen (Beispiel: .\tcping.exe 10.0.0.4 3389), und zeichnen Sie die Ergebnisse auf. 
3. Wenn in diesen Pingtests keine Antwort empfangen wird, gehen Sie wie folgt vor:
    - Führen Sie eine gleichzeitige Netsh-Ablaufverfolgung für die Ziel-Back-End-Pool-VM und eine andere Test-VM im selben VNET aus. Führen Sie jetzt für einige Zeit einen PsPing-Test aus, erfassen Sie einige Netzwerk-Ablaufverfolgungen, und beenden Sie den Test. 
    - Analysieren Sie die erfassten Netzwerkdaten, und stellen Sie fest, ob es eingehende und ausgehende Pakete im Zusammenhang mit der Pingabfrage gibt. 
        - Wenn keine eingehenden Pakete auf der Back-End-Pool-VM festgestellt werden, blockiert möglicherweise die Fehlkonfiguration von Netzwerksicherheitsgruppen oder UDR den Datenverkehr. 
        - Wenn keine ausgehenden Pakete auf der Back-End-Pool-VM festgestellt werden, muss die VM auf andere Probleme (z. B. das Blockieren des Testports durch eine Anwendung) überprüft werden. 
    - Überprüfen Sie, ob die Testpakete zwangsweise an ein anderes Ziel gesendet werden (möglicherweise über UDR-Einstellungen), bevor sie den Load Balancer erreichen. Dies kann verursachen, dass der Datenverkehr die Back-End-VM nicht erreicht. 
4. Ändern Sie den Testtyp (z.B. von HTTP in TCP), und konfigurieren Sie den entsprechenden Port in Netzwerksicherheitsgruppen-ACLs und der Firewall, um zu prüfen, ob das Problem in der Konfiguration der Testantwort liegt. Weitere Informationen zur Konfiguration von Integritätstests finden Sie unter [Endpoint Load Balancing health probe configuration](/archive/blogs/mast/endpoint-load-balancing-heath-probe-configuration-details) (Konfiguration von Integritätstests für den Endpunktlastenausgleich).

## <a name="next-steps"></a>Nächste Schritte

Sollte sich das Problem mit den oben genannten Schritten nicht beheben lassen, erstellen Sie ein [Supportticket](https://azure.microsoft.com/support/options/).