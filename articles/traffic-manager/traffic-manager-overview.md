---
title: Azure Traffic Manager | Microsoft Docs
description: Dieser Artikel enthält eine Übersicht zu Azure Traffic Manager. Hier erfahren Sie, ob dieser Dienst die richtige Wahl für den Lastenausgleich des Benutzerdatenverkehrs für Ihre Anwendung ist.
services: traffic-manager
author: duongau
ms.service: traffic-manager
customer intent: As an IT admin, I want to learn about Traffic Manager and what I can use it for.
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/19/2021
ms.author: duau
ms.openlocfilehash: 8c8897218b153c8584c89abab98934268ccd555d
ms.sourcegitcommit: 24a12d4692c4a4c97f6e31a5fbda971695c4cd68
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2021
ms.locfileid: "102182161"
---
# <a name="what-is-traffic-manager"></a>Was ist Traffic Manager?
Azure Traffic Manager ist ein auf DNS basierender Lastenausgleichsdienst. Dieser Dienst ermöglicht die Verteilung des Datenverkehrs an Ihre öffentlichen Anwendungen in den globalen Azure-Regionen. Darüber hinaus bietet Traffic Manager Hochverfügbarkeit und kurze Reaktionszeiten für Ihre öffentlichen Endpunkte.

Traffic Manager verwendet DNS, um die Clientanforderungen auf der Grundlage einer Datenverkehrsrouting-Methode an den passenden Dienstendpunkt weiterzuleiten. Des Weiteren bietet Traffic Manager auch eine Integritätsüberwachung für jeden Endpunkt. Der Endpunkt kann ein beliebiger Dienst mit Internetzugriff sein, der innerhalb oder außerhalb von Azure gehostet wird. Traffic Manager bietet eine Reihe von [Datenverkehrsrouting-Methoden](traffic-manager-routing-methods.md) und [Endpunktüberwachungsoptionen](traffic-manager-monitoring.md), die verschiedene Anwendungsanforderungen erfüllen sowie automatisches Failover ermöglichen. Traffic Manager zeichnet sich durch eine geringe Fehleranfälligkeit aus, selbst wenn es zu einem Ausfall einer ganzen Azure-Region kommt.

>[!NOTE]
> Azure stellt eine Sammlung aus vollständig verwalteten Lastenausgleichslösungen für Ihre Szenarios bereit. 
> * Wenn Sie Lastenausgleich zwischen den Servern in einer Region auf Anwendungsebene vornehmen möchten, lesen Sie den Artikel zu [Application Gateway](../application-gateway/overview.md).
> * Wenn Sie das globale Routing Ihres Webdatenverkehrs sowie die Endbenutzerleistung und -zuverlässigkeit auf oberster Ebene mithilfe eines schnellen globalen Failovers optimieren möchten, sehen Sie sich den Artikel zu [Front Door](../frontdoor/front-door-overview.md) an.
> * Um Lastenausgleich auf Netzwerkebene auszuführen, lesen Sie den Artikel zu [Load Balancer](../load-balancer/load-balancer-overview.md). 
> 
> Für Ihre End-to-End-Szenarien kann es vorteilhaft sein, diese Lösungen nach Bedarf zu kombinieren.
> Einen Vergleich der Azure-Lastenausgleichsoptionen finden Sie unter [Übersicht über Lastenausgleichsoptionen in Azure](/azure/architecture/guide/technology-choices/load-balancing-overview).

Traffic Manager bietet folgende Funktionen:

## <a name="increase-application-availability"></a>Steigern der Anwendungsverfügbarkeit

Mit Traffic Manager können Sie die Hochverfügbarkeit wichtiger Anwendungen gewährleisten, indem Sie Ihre Endpunkte überwachen und beim Ausfall eines Endpunkts automatisches Failover bereitstellen.
    
## <a name="improve-application-performance"></a>Verbessern der Anwendungsleistung

Azure ermöglicht die Ausführung von Clouddiensten und Websites in Rechenzentren auf der ganzen Welt. Traffic Manager kann Datenverkehr an den Endpunkt mit der kürzesten Wartezeit weiterleiten und so die Reaktionsfähigkeit Ihrer Website verbessern.

## <a name="service-maintenance-without-downtime"></a>Dienstwartung ohne Downtime

Für Ihre Anwendungen können geplante Wartungsarbeiten ohne Downtime durchgeführt werden. Hierzu kann Traffic Manager den Datenverkehr während der Wartung an andere Endpunkte weiterleiten.

## <a name="combine-hybrid-applications"></a>Kombinieren von Hybridanwendungen

Traffic Manager unterstützt externe, Azure-fremde Endpunkte und kann somit in hybriden, cloudbasierten bzw. lokalen Bereitstellungen einschließlich „[burst-to-cloud](https://azure.microsoft.com/overview/what-is-cloud-bursting/)“ (Ausweichen in die Cloud bei Nachfragespitzen), „migrate-to-cloud“ (Migration in die Cloud) und „failover-to-cloud“ (Failover in die Cloud) verwendet werden.

## <a name="distribute-traffic-for-complex-deployments"></a>Verteilen des Datenverkehrs für komplexe Bereitstellungen

Mehrere Datenverkehrsrouting-Methoden können mit [geschachtelten Traffic Manager-Profilen](traffic-manager-nested-profiles.md) zu komplexen und flexiblen Regeln kombiniert werden, die auch den Anforderungen umfangreicher und komplexer Bereitstellungen gerecht werden.

## <a name="pricing"></a>Preise

Preisinformationen finden Sie unter [Traffic Manager – Preise](https://azure.microsoft.com/pricing/details/traffic-manager/).


## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich über das [Erstellen eines Traffic Manager-Profils](./quickstart-create-traffic-manager-profile.md).
- Erhalten Sie weitere Informationen zur [Funktionsweise von Traffic Manager](traffic-manager-how-it-works.md).
- Lesen Sie die [Häufig gestellten Fragen](traffic-manager-FAQs.md) zu Traffic Manager.