---
title: Unterstützung von Azure Resource Manager für Load Balancer
description: In diesem Artikel verwenden Sie Azure PowerShell und Vorlagen mit Azure Load Balancer.
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/19/2019
ms.author: allensu
ms.openlocfilehash: 57a60a65dee995695224aa1b451e848ea8768ab1
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74215387"
---
# <a name="azure-resource-manager-support-with-azure-load-balancer"></a>Unterstützung von Azure Resource Manager für Azure Load Balancer



Azure Resource Manager ist das bevorzugte Verwaltungsframework für Dienste in Azure. Azure Load Balancer kann mithilfe von APIs und Tools auf Basis von Azure Resource Manager verwaltet werden.

## <a name="concepts"></a>Konzepte

Mit Resource Manager enthält Azure Load Balancer die folgenden untergeordneten Ressourcen:

* Front-End-IP-Konfiguration: ein Lastenausgleich kann ein oder mehrere Front-End-IP-Adressen umfassen, die auch als virtuelle IPs (VIPs) bezeichnet werden. Diese IP-Adressen dienen als Eingang für den Datenverkehr.
* Back-End-Adresspool: Bei diesem Pool handelt es sich um eine Sammlung von IP-Adressen, die der Netzwerkschnittstellenkarte des virtuellen Computers zugeordnet ist, an die die Last verteilt wird.
* Lastenausgleichsregeln: Eine Regeleigenschaft ordnet eine bestimmte IP-/Port-Kombination des Front-Ends einer IP-Adressen-/Port-Kombination des Back-Ends zu. Ein einzelnes Lastenausgleichsmodul kann mehrere Lastenausgleichsregeln umfassen. Jede Regel ist eine Kombination aus der Front-End-IP und dem Front-End-Port sowie der Back-End-IP und dem Back-End-Port von virtuellen Computern.
* Überprüfungen – Mit Überprüfungen können Sie die Integrität der VM-Instanzen nachverfolgen. Schlägt eine Integritätsüberprüfung fehl, wird die VM-Instanz automatisch aus der Rotation entfernt.
* NAT-Regeln für eingehenden Datenverkehr: NAT-Regeln definieren den Eingangsdatenverkehr, der durch die Front-End-IP fließt und an die Back-End-IP verteilt wird.

![](./media/load-balancer-arm/load-balancer-arm.png)

## <a name="quickstart-templates"></a>Schnellstartvorlagen

Mit dem Azure-Ressourcen-Manager können Sie Ihre Anwendungen mit einer deklarativen Vorlage bereitstellen. In einer Vorlage können Sie mehrere Dienste zusammen mit ihren Abhängigkeiten bereitstellen. Sie verwenden die gleiche Vorlage, um Ihre Anwendung während jeder Phase des Anwendungslebenszyklus wiederholt bereitzustellen.

Vorlagen können folgende Definitionen enthalten:
* **Virtuelle Computer**
* **Virtuelle Netzwerke**
* **Verfügbarkeitsgruppen**
* **Netzwerkschnittstellen (NICs)**
* **Speicherkonten**
* **Load Balancer**
* **Netzwerksicherheitsgruppen**
* **Öffentliche IP-Adressen.** 

Mit Vorlagen können Sie alle Elemente erstellen, die Sie für eine komplexe Anwendung benötigen. Die Vorlagendatei kann zur Versionskontrolle und Zusammenarbeit in ein Content Management-System eingecheckt werden.

[Weitere Informationen zu Vorlagen](../azure-resource-manager/resource-manager-template-walkthrough.md)

[Weitere Informationen zu Netzwerkressourcen](../networking/networking-overview.md)

Schnellstartvorlagen, die Azure Load Balancer verwenden, finden Sie in diesem [GitHub-Repository](https://github.com/Azure/azure-quickstart-templates), das eine Reihe von in der Community erstellten Vorlagen enthält.

Beispiele für Vorlagen:

* [2 VMs in a Load Balancer and load balancing rules (in englischer Sprache)](https://go.microsoft.com/fwlink/?LinkId=544799)
* [2 VMs in a VNET with an Internal Load Balancer and Load Balancer rules (in englischer Sprache)](https://go.microsoft.com/fwlink/?LinkId=544800)
* [2 VMs in a Load Balancer and configure NAT rules on the LB (in englischer Sprache)](https://go.microsoft.com/fwlink/?LinkId=544801)

## <a name="setting-up-azure-load-balancer-with-a-powershell-or-cli"></a>Einrichten von Azure-Lastenausgleich per PowerShell oder CLI

Erste Schritte mit Cmdlets, Befehlszeilentools und REST-APIs von Azure Resource Manager

* [Azure Networking Cmdlets](https://docs.microsoft.com/powershell/module/az.network#networking) können verwendet werden, um einen Lastenausgleich zu erstellen.
* [Erstellen eines Load Balancers mit dem Azure-Ressourcen-Manager](load-balancer-get-started-ilb-arm-ps.md)
* [Verwenden der Azure-CLI mit der Azure-Ressourcenverwaltung](../xplat-cli-azure-resource-manager.md)
* [Load Balancer REST APIs (in englischer Sprache)](https://msdn.microsoft.com/library/azure/mt163651.aspx)

## <a name="next-steps"></a>Nächste Schritte

Sie können einen [Lastenausgleich mit Internetzugriff erstellen](load-balancer-get-started-internet-arm-ps.md) und die Art des [Verteilungsmodus](load-balancer-distribution-mode.md) für ein bestimmtes Verhalten des Datenverkehrs im Netzwerk konfigurieren.

Erfahren Sie, wie Sie [TCP-Leerlauftimeout-Einstellungen für ein Lastenausgleichsmodul](load-balancer-tcp-idle-timeout.md)verwalten. Diese Einstellungen sind wichtig, wenn Ihre Anwendung Verbindungen für Server hinter einem Lastenausgleichsmodul beibehalten muss.
