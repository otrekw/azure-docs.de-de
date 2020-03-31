---
title: Was ist die IP-Adresse 168.63.129.16? | Microsoft-Dokumentation
description: Erfahren Sie mehr zur IP-Adresse 168.63.129.16 und deren Funktionsweise mit Ihren Ressourcen.
services: virtual-network
documentationcenter: na
author: genlin
manager: dcscontentpm
editor: v-jesits
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/15/2019
ms.author: genli
ms.openlocfilehash: 287f881fb17dd84357f540ee562e21c66c11ab95
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "77114360"
---
# <a name="what-is-ip-address-1686312916"></a>Was ist die IP-Adresse 168.63.129.16?

Die IP-Adresse 168.63.129.16 ist eine virtuelle öffentliche IP-Adresse, die verwendet wird, um einen Kommunikationskanal zu den Ressourcen der Azure-Plattform zu ermöglichen. Kunden können in Azure einen beliebigen Adressraum für ihr privates virtuelles Netzwerk definieren. Daher müssen die Ressourcen der Azure-Plattform als eindeutige öffentliche IP-Adresse dargestellt werden. Diese virtuelle öffentliche IP-Adresse erleichtert die folgenden Vorgänge:

- Ermöglicht die Kommunikation zwischen VM-Agent und Azure-Plattform, um zu signalisieren, dass er sich im Zustand „Bereit“ befindet.
- Ermöglicht die Kommunikation mit dem virtuellen DNS-Server, um für die Ressource (wie einen VM) eine gefilterte Namensauflösung bereitzustellen, die keinen benutzerdefinierten DNS-Server hat. Dieser Filter stellt sicher, dass Kunden nur die Hostnamen ihrer Ressourcen auflösen können.
- Ermöglicht [Integritätstests des Azure Load Balancers](../load-balancer/load-balancer-custom-probe-overview.md), um den Integritätszustand der VMs zu ermitteln.
- Ermöglichen des Abrufs einer dynamischen IP-Adresse vom DHCP-Dienst in Azure durch die VM.
- Ermöglicht Taktmeldungen für einen Gast-Agent für die PaaS-Rolle.

## <a name="scope-of-ip-address-1686312916"></a>Bereich der IP-Adresse 168.63.129.16

Die öffentliche IP-Adresse 168.63.129.16 wird in allen Regionen und allen nationalen Clouds verwendet. Diese spezielle öffentliche IP-Adresse ist Eigentum von Microsoft und wird nicht geändert. Wir empfehlen Ihnen, diese IP-Adresse in lokalen Firewallrichtlinien (auf der VM, in ausgehender Richtung) zuzulassen. Die Kommunikation zwischen dieser speziellen IP-Adresse und den Ressourcen ist sicher, da nur die interne Azure-Plattform eine Nachricht von dieser IP-Adresse beziehen kann. Wenn diese Adresse gesperrt ist, kann es in verschiedenen Szenarien zu unerwartetem Verhalten kommen. 168.63.129.16 ist eine [virtuelle IP des Hostknotens](../virtual-network/security-overview.md#azure-platform-considerations) und unterliegt daher keinen benutzerdefinierten Routen.

- Für den VM-Agent ist die Kommunikation in ausgehender Richtung über die Ports 80, 443, 32526 mit WireServer (168.63.129.16) erforderlich. Diese Ports müssen in der lokalen Firewall auf dem virtuellen Computer geöffnet sein. Die Kommunikation mit 168.63.129.16 über diese Ports unterliegt nicht den konfigurierten Netzwerksicherheitsgruppen.
- 168.63.129.16 kann DNS-Dienste für die VM bereitstellen. Falls dies nicht erwünscht ist, kann dieser Datenverkehr in der lokalen Firewall auf der VM blockiert werden. Standardmäßig unterliegt die DNS-Kommunikation nicht den konfigurierten Netzwerksicherheitsgruppen, sofern dies nicht speziell eingerichtet und das Diensttag [AzurePlatformDNS](../virtual-network/service-tags-overview.md#available-service-tags) genutzt wird.
- Wenn die VM Teil eines Back-End-Pools für den Lastenausgleich ist, sollte für die [Integritätstest](../load-balancer/load-balancer-custom-probe-overview.md)-Kommunikation zugelassen werden, dass sie von „168.63.129.16“ ausgeht. Die Standardkonfiguration der Netzwerksicherheitsgruppen enthält eine Regel, die diese Kommunikation zulässt. Für diese Regel wird das Diensttag [AzureLoadBalancer](../virtual-network/service-tags-overview.md#available-service-tags) genutzt. Dieser Datenverkehr kann bei Bedarf blockiert werden, indem die Netzwerksicherheitsgruppe konfiguriert wird, aber dies führt zum Auftreten von Fehlern für Tests.

In einem nicht virtuellen Netzwerkszenario (klassisch) wird der Integritätstest von einer privaten IP bezogen und 168.63.129.16 nicht verwendet.

## <a name="next-steps"></a>Nächste Schritte

- [Sicherheitsgruppen](security-overview.md)
- [Erstellen, Ändern oder Löschen einer Netzwerksicherheitsgruppe](manage-network-security-group.md)
