---
title: 'Häufig gestellte Fragen: Azure Load Balancer'
description: Antworten auf häufig gestellte Fragen zu Azure Load Balancer.
services: load-balancer
author: erichrt
ms.service: load-balancer
ms.topic: article
ms.date: 04/22/2020
ms.author: errobin
ms.openlocfilehash: 94a2398879007e7ecd6d2f1920157eb4627f33cb
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2020
ms.locfileid: "84014926"
---
# <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

## <a name="what-types-of-load-balancer-exist"></a>Welche Typen von Load Balancer sind vorhanden?
Interne Lastenausgleichsmodule, die Datenverkehr in einem VNET ausgleichen, und externe Lastenausgleichsmodule, die Datenverkehr zu und von einem mit dem Internet verbundenen Endpunkt ausgleichen. Weitere Informationen finden Sie unter [Load Balancer-Typen](components.md#frontend-ip-configurations). 

Für beide Typen bietet Azure eine Basic-SKU und eine Standard-SKU mit unterschiedlichen Möglichkeiten für Funktion, Leistung, Sicherheit und Integritätsnachverfolgung. Diese Unterschiede werden im Artikel zum [SKU-Vergleich](skus.md) erläutert.

 ## <a name="how-can-i-upgrade-from-a-basic-to-a-standard-load-balancer"></a>Wie kann ich ein Upgrade von Load Balancer Basic-auf Load Balancer Standard durchführen?
Ein automatisiertes Skript und Anleitungen zum Upgrade einer Load Balancer-SKU finden Sie im Artikel [Upgrade von Basic auf Standard](upgrade-basic-standard.md).

 ## <a name="what-are-the-different-load-balancing-options-in-azure"></a>Was sind die verschiedenen Lastenausgleichsoptionen in Azure?
Informationen zu den verfügbaren Lastenausgleichsdiensten und den jeweils empfohlenen Verwendungsmöglichkeiten finden Sie im [Technologieleitfaden zum Lastenausgleich](https://docs.microsoft.com/azure/architecture/guide/technology-choices/load-balancing-overview).

## <a name="where-can-i-find-load-balancer-arm-templates"></a>Wo finde ich Load Balancer ARM-Vorlagen?
Weitere Informationen zu ARM-Vorlagen für allgemeine Bereitstellungen finden Sie in der [Liste der Azure Load Balancer-Schnellstartvorlagen](https://docs.microsoft.com/azure/templates/microsoft.network/loadbalancers#quickstart-templates).

## <a name="how-are-inbound-nat-rules-different-from-load-balancing-rules"></a>Wie unterscheiden sich NAT-Regeln für eingehenden Datenverkehr von Lastenausgleichsregeln?
NAT-Regeln werden verwendet, um eine Back-End-Ressource anzugeben, an die Datenverkehr weitergeleitet werden soll. Beispielsweise das Konfigurieren eines bestimmten Load Balancer-Ports zum Senden von RDP-Datenverkehr an eine bestimmte VM. Lastenausgleichsregeln werden verwendet, um einen Pool mit Back-End-Ressourcen anzugeben, an die Datenverkehr weitergeleitet werden soll, damit die Last instanzenübergreifend verteilt wird. Beispielsweise kann eine Lastenausgleichsregel TCP-Pakete an Port 80 des Load Balancers über einen Pool von Webservern weiterleiten.

## <a name="what-is-ip-1686312916"></a>Was ist IP 168.63.129.16?
Die virtuelle IP-Adresse für den Host, der als Load Balancer für die Azure-Infrastruktur gekennzeichnet ist und als Ausgangspunkt für die Integritätstests von Azure dient. Beim Konfigurieren von Back-End-Instanzen müssen diese Datenverkehr von dieser IP-Adresse zulassen, um erfolgreich auf Integritätstests reagieren zu können. Diese Regel interagiert nicht mit dem Zugriff auf das Load Balancer-Front-End. Sie können diese Regel außer Kraft setzen, wenn Sie den Azure Load Balancer nicht verwenden. Weitere Informationen zu Diensttags finden Sie [hier](https://docs.microsoft.com/azure/virtual-network/service-tags-overview#available-service-tags).

## <a name="next-steps"></a>Nächste Schritte
Wenn Ihre Frage oben nicht aufgeführt wird, senden Sie uns Feedback zu dieser Seite mit Ihrer Frage. Dadurch wird ein Issue auf GitHub für das Produktteam erstellt, um sicherzustellen, dass alle wichtigen Fragen unserer Kunden beantwortet werden.
