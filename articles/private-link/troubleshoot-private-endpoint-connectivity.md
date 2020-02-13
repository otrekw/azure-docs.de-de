---
title: Problembehandlung bei Konnektivitätsproblemen mit privaten Endpunkten in Azure
description: Ausführliche Anleitung zur Diagnose der Konnektivität von privaten Endpunkten
services: private-endpoint
documentationcenter: na
author: rdhillon
manager: narayan
editor: ''
ms.service: private-link
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/31/2020
ms.author: rdhillon
ms.openlocfilehash: df4ec6ddbba029eb29d2440717697968f8c79302
ms.sourcegitcommit: b07964632879a077b10f988aa33fa3907cbaaf0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2020
ms.locfileid: "77191066"
---
# <a name="troubleshoot-private-endpoint-connectivity-problems"></a>Problembehandlung bei Konnektivitätsproblemen mit privaten Endpunkten

In dieser Anleitung finden Sie ausführliche Anweisungen zum Überprüfen und Diagnostizieren der Konnektivität für die Einrichtung Ihres privaten Endpunkts. 

Ein privater Endpunkt in Azure ist eine Netzwerkschnittstelle, über die eine private und sichere Verbindung mit einem Private Link-Dienst hergestellt wird. Diese Lösung unterstützt Sie beim Schützen Ihrer Workloads in Azure, indem Ihnen private Konnektivität mit Ihren Azure-Dienstressourcen über Ihr virtuelles Netzwerk bereitgestellt wird. Dadurch werden diese Dienste effektiv in Ihr virtuelles Netzwerk gebracht. 

Die folgenden Konnektivitätsszenarios sind mit privaten Endpunkten verfügbar: 
- Virtuelle Netzwerke in derselben Region 
- Virtuelle Netzwerke mit regionalem Peering
- Virtuelle Netzwerke mit globalem Peering
- Lokale Kundennetzwerke über VPN oder ExpressRoute-Verbindungen

## <a name="diagnosing-connectivity-problems"></a>Diagnostizieren von Konnektivitätsproblemen 
Führen Sie die unten aufgeführten Schritte aus, um sicherzustellen, dass alle Konfigurationen ordnungsgemäß festgelegt sind und somit Konnektivitätsprobleme mit Ihren privaten Endpunkten gelöst werden können.

1. Überprüfen Sie die Konfiguration des privaten Endpunkts, indem Sie die Ressource durchsuchen: 

    a) Rufen Sie das **Private Link-Center** auf.

      ![Private Link-Center](./media/private-endpoint-tsg/private-link-center.png)

    b) Klicken Sie im linken Navigationsbereich auf „Private Endpunkte“.
    
      ![Private Endpunkte](./media/private-endpoint-tsg/private-endpoints.png)

    c) Filtern Sie nach dem zu diagnostizierenden privaten Endpunkt, und wählen Sie ihn aus.

    d) Überprüfen Sie das virtuelle Netzwerk und die DNS-Informationen:
    
     - Überprüfen Sie, ob der Verbindungsstatus **Genehmigt** vorliegt.
     - Stellen Sie sicher, dass der virtuelle Computer (VM) mit dem VNET (virtuelles Netzwerk) verbunden ist, das die privaten Endpunkte hostet.
     - Überprüfen Sie, ob die FQDN-Informationen (kopieren Sie den FQDN) und die private IP-Adresse zugewiesen sind.
    
       ![VNET- und DNS-Konfiguration](./media/private-endpoint-tsg/vnet-dns-configuration.png)    
    
2. Verwenden Sie [**Azure Monitor**](https://docs.microsoft.com/azure/azure-monitor/overview), um zu überprüfen, ob Daten übermittelt werden.

    a) Klicken Sie bei der privaten Endpunktressource auf **Überwachen**.
     - Wählen Sie „Eingehende Daten“ oder „Ausgehende Daten“ aus, und überprüfen Sie, ob die Daten übermittelt werden, wenn Sie versuchen, eine Verbindung mit dem privaten Endpunkt herzustellen. Rechnen Sie mit einer Verzögerung von etwa 10 Minuten.
    
       ![Telemetriedaten für privaten Endpunkt überprüfen](./media/private-endpoint-tsg/private-endpoint-monitor.png)

3. Verwenden Sie die **Network Watcher**-Problembehandlung für VM-Verbindungen.

    a) Wählen Sie die Client-VM aus.

    b) Klicken Sie unter **Problembehandlung für Verbindung** auf die Registerkarte **Ausgehende Verbindung**.
    
      ![Network Watcher: Ausgehende Verbindungen testen](./media/private-endpoint-tsg/network-watcher-outbound-connection.png)
    
    c) Klicken Sie auf **Network Watcher für detaillierte Verbindungsablaufverfolgung verwenden**.
    
      ![Network Watcher: Problembehandlung für die Verbindung](./media/private-endpoint-tsg/network-watcher-connection-troubleshoot.png)

    d) Klicken Sie auf **Test by FQDN** (Nach FQDN testen).
     - Fügen Sie den FQDN der privaten Endpunktressource ein.
     - Geben Sie einen Port an (*in der Regel Port 443 für Azure Storage oder COSMOS, 1336 für SQL usw.* ).

    e) Klicken Sie auf **Testen**, und überprüfen Sie die Testergebnisse.
    
      ![Network Watcher: Testergebnisse](./media/private-endpoint-tsg/network-watcher-test-results.png)
    
        
4. Die DNS-Auflösung der Testergebnisse muss dieselbe private IP-Adresse aufweisen, die dem privaten Endpunkt zugewiesen ist.

    a) Wenn die DNS-Einstellungen fehlerhaft sind, gehen Sie wie folgt vor:
     - Wenn Sie eine private Zone verwenden: 
       - Stellen Sie sicher, dass das VNET der Client-VM der privaten Zone zugeordnet ist.
       - Überprüfen Sie, ob ein Eintrag für die private DNS-Zone vorhanden ist, und erstellen Sie einen, falls dies nicht der Fall ist.
    
     - Wenn Sie benutzerdefiniertes DNS verwenden:
       - Überprüfen Sie Ihre benutzerdefinierten DNS-Einstellungen und überprüfen Sie, ob die DNS-Konfiguration richtig ist.
       Anleitungen hierzu finden Sie unter [Übersicht über private Endpunkte: DNS-Konfiguration](https://docs.microsoft.com/azure/private-link/private-endpoint-overview#dns-configuration).

    b) Wenn die Verbindung aufgrund von NSGs/UDRs (Netzwerksicherheitsgruppen/benutzerdefinierte Routen) fehlschlägt:
     - Überprüfen Sie die NSG-Ausgangsregeln, und erstellen Sie entsprechende Ausgangsregeln, um den Datenverkehr zuzulassen.
    
       ![NSG-Ausgangsregeln](./media/private-endpoint-tsg/nsg-outbound-rules.png)

5. Wenn die Ergebnisse der Verbindung überprüft wurden, kann das Konnektivitätsproblem mit anderen Aspekten wie Geheimnissen, Token oder Kennwörtern auf der Anwendungsebene zusammenhängen.
   - Überprüfen Sie in diesem Fall die Konfiguration der Private Link-Ressource, die dem privaten Endpunkt zugeordnet ist. Informationen dazu finden Sie im [Leitfaden zur Problembehandlung für Private Link](troubleshoot-private-link-connectivity.md). 

6. Wenden Sie sich an das [Azure-Supportteam](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview), wenn Sie weiterhin Konnektivitätsprobleme haben. 

## <a name="next-steps"></a>Nächste Schritte

 * [Erstellen eines privaten Endpunkts im aktualisierten Subnetz über das Azure-Portal](https://docs.microsoft.com/azure/private-link/create-private-endpoint-portal)

 * [Leitfaden zur Problembehandlung für Private Link](troubleshoot-private-link-connectivity.md).
