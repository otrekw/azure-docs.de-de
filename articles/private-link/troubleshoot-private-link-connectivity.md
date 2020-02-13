---
title: Problembehandlung bei Konnektivitätsproblemen bei Azure Private Link
description: Ausführliche Anleitung zur Diagnose der Private Link-Konnektivität
services: private-link
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
ms.openlocfilehash: 0d26ad6802e551523875dcad13066fdbdbf39ada
ms.sourcegitcommit: b07964632879a077b10f988aa33fa3907cbaaf0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2020
ms.locfileid: "77191051"
---
# <a name="troubleshoot-private-link-service-connectivity-problems"></a>Problembehandlung bei Konnektivitätsproblemen beim Private Link-Dienst

In dieser Anleitung finden Sie ausführliche Anweisungen zum Überprüfen und Diagnostizieren der Konnektivität für die Einrichtung Ihres Private Link-Diensts. 

Mit Azure Private Link können Sie auf Azure PaaS-Dienste (z. B. Azure Storage, Azure Cosmos DB und SQL-Datenbank) und über einen privaten Endpunkt in Ihrem virtuellen Netzwerk auf in Azure gehostete Kunden-/Partnerdienste zugreifen. Der Datenverkehr zwischen Ihrem virtuellen Netzwerk und dem Dienst wird über das Microsoft-Backbone-Netzwerk übertragen und dadurch vom öffentlichen Internet isoliert. Sie können auch Ihren eigenen Private Link-Dienst in Ihrem virtuellen Netzwerk (VNET) erstellen und privat für Ihre Kunden zur Verfügung stellen. 

Sie können den Private Link-Zugriff für Ihren Dienst aktivieren, der mit Azure Load Balancer Standard ausgeführt wird. Consumer Ihres Diensts können einen privaten Endpunkt innerhalb ihres virtuellen Netzwerks erstellen und diesen zum Dienst zuordnen, um privat darauf zuzugreifen.

Die folgenden Konnektivitätsszenarios sind mit dem Private Link-Dienst verfügbar:
- Virtuelle Netzwerke in derselben Region 
- Virtuelle Netzwerke mit regionalem Peering
- Virtuelle Netzwerke mit globalem Peering
- Lokale Kundennetzwerke über VPN oder ExpressRoute-Verbindungen

## <a name="deployment-troubleshooting"></a>Problembehandlung für Bereitstellungen

Informationen zur Problembehandlung in Fällen, bei denen Sie die Quell-IP-Adresse nicht aus dem Subnetz Ihrer Wahl für Ihren Private Link-Dienst auswählen können, finden Sie unter [Deaktivieren von Netzwerkrichtlinien für die Quell-IP-Adresse eines Private Link-Diensts](https://docs.microsoft.com/azure/private-link/disable-private-link-service-network-policy).

Stellen Sie sicher, dass die Einstellung **privateLinkServiceNetworkPolicies** für das Subnetz deaktiviert ist, aus dem Sie die Quell-IP-Adresse auswählen.

## <a name="diagnosing-connectivity-problems"></a>Diagnostizieren von Konnektivitätsproblemen

Führen Sie die unten aufgeführten Schritte durch, um sicherzustellen, dass die Konfigurationen ordnungsgemäß festgelegt sind, wenn beim Einrichten Ihres Private Link-Diensts Konnektivitätsprobleme auftreten.

1. Überprüfen Sie die Konfiguration des Private Link-Diensts, indem Sie die Ressource durchsuchen: 

    a) Rufen Sie das **Private Link-Center** auf.

      ![Private Link-Center](./media/private-link-tsg/private-link-center.png)

    b) Klicken Sie im linken Navigationsbereich auf **Private Link-Dienste**.

      ![Private Link-Dienste](./media/private-link-tsg/private-link-service.png)

    c) Filtern Sie nach dem zu diagnostizierenden Private Link-Dienst, und wählen Sie ihn aus.

    d) Überprüfen Sie die privaten Endpunktverbindungen.
     - Stellen Sie sicher, dass der private Endpunkt, mit dem Sie eine Verbindung herstellen möchten, mit dem Verbindungsstatus **Genehmigt** aufgeführt wird. 
     - Wählen Sie den Status **Ausstehend** aus, wenn er angezeigt wird, und genehmigen Sie die Verbindung. 

       ![Private Endpunktverbindungen](./media/private-link-tsg/pls-private-endpoint-connections.png)

     - Navigieren Sie zu dem privaten Endpunkt, über den Sie die Verbindung herstellen, indem Sie auf seinen Namen klicken. Stellen Sie sicher, dass der Verbindungsstatus **Genehmigt** angezeigt wird.

       ![Übersicht über die private Endpunktverbindung](./media/private-link-tsg/pls-private-endpoint-overview.png)

     - Testen Sie die Konnektivität noch mal, sobald beide Seiten genehmigt wurden.

    e) Überprüfen Sie den **Alias** auf der Registerkarte „Übersicht“ und die **Ressourcen-ID** auf der Registerkarte „Eigenschaften“. 
     - Stellen Sie sicher, dass **der Alias und die Ressourcen-ID** mit **dem Alias und der Ressourcen-ID** übereinstimmen, die Sie zum Erstellen des privaten Endpunkts für diesen Dienst verwenden. 

       ![Alias überprüfen](./media/private-link-tsg/pls-overview-pane-alias.png)

       ![Ressourcen-ID überprüfen](./media/private-link-tsg/pls-properties-pane-resourceid.png)

    f) Überprüfen Sie die Sichtbarkeit (im Abschnitt „Übersicht“).
     - Stellen Sie sicher, dass Ihr Abonnement dem Bereich **Sichtbarkeit** entspricht.

       ![Sichtbarkeit überprüfen](./media/private-link-tsg/pls-overview-pane-visibility.png)

    g) Überprüfen Sie die Lastenausgleichsinformationen (im Abschnitt „Übersicht“).
     - Sie können zum Lastenausgleich navigieren, indem Sie auf den Lastenausgleichslink klicken.

       ![Lastenausgleich überprüfen](./media/private-link-tsg/pls-overview-pane-ilb.png)

     - Stellen Sie sicher, dass die Einstellungen für den Lastenausgleich wie gewünscht konfiguriert sind.
       - Überprüfen Sie die Front-End-IP-Konfiguration.
       - Überprüfen Sie die Back-End-Pools.
       - Überprüfen Sie die Lastenausgleichsregeln.

       ![Lastenausgleichseigenschaften überprüfen](./media/private-link-tsg/pls-ilb-properties.png)

     - Stellen Sie sicher, dass der Lastenausgleich gemäß der oben gezeigten Einstellungen funktioniert.
       - Wählen Sie einen virtuellen Computer (VM) in einem beliebigen anderen Subnetz als dem Subnetz aus, in dem der Back-End-Pool für den Lastenausgleich verfügbar ist.
       - Versuchen Sie, über die oben gezeigte VM auf das Front-End für den Lastenausgleich zuzugreifen.
       - Wenn gemäß der Lastenausgleichsregeln eine Verbindung mit dem Back-End-Pool hergestellt wird, ist Ihr Lastenausgleich betriebsbereit.
       - Sie können außerdem die Lastenausgleichsmetrik über Azure Monitor überprüfen, um zu überprüfen, ob Daten über den Lastenausgleich geleitet werden.

2. Verwenden Sie [**Azure Monitor**](https://docs.microsoft.com/azure/azure-monitor/overview), um zu überprüfen, ob Daten übermittelt werden.

    a) Klicken Sie bei der Ressource für den Private Link-Dienst auf **Metriken**.
     - Wählen Sie **Eingehende Bytes** oder **Ausgehende Bytes** aus.
     - Überprüfen Sie, ob Daten übermittelt werden, wenn Sie versuchen, eine Verbindung mit dem Private Link-Dienst herzustellen. Rechnen Sie mit einer Verzögerung von etwa 10 Minuten.

       ![Private Link-Dienstmetriken überprüfen](./media/private-link-tsg/pls-metrics.png)

3. Wenden Sie sich an das [Azure-Supportteam](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview), wenn Sie weiterhin Konnektivitätsprobleme haben. 

## <a name="next-steps"></a>Nächste Schritte

 * [Erstellen eines Private Link-Diensts mithilfe der Azure CLI](https://docs.microsoft.com/azure/private-link/create-private-link-service-cli)

 * [Leitfaden zur Problembehandlung bei privaten Endpunkten](troubleshoot-private-endpoint-connectivity.md)
