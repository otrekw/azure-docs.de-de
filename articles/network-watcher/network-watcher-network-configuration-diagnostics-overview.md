---
title: Einführung in die Netzwerkkonfigurationsdiagnose in Azure Network Watcher | Microsoft-Dokumentation
description: Diese Seite bietet eine Übersicht über Network Watcher – Netzwerkkonfigurationsdiagnose
services: network-watcher
documentationcenter: na
author: damendo
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/15/2020
ms.author: damendo
ms.openlocfilehash: 5feef79a08789ad381b0c93cb938abd9effdfcc8
ms.sourcegitcommit: 15d27661c1c03bf84d3974a675c7bd11a0e086e6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2021
ms.locfileid: "102502007"
---
# <a name="introduction-to-network-configuration-diagnostics-in-azure-network-watcher"></a>Einführung in die Netzwerkkonfigurationsdiagnose in Azure Network Watcher

Das Tool zur Netzwerkdatenverkehrsdiagnose hilft Kunden zu verstehen, welcher Datenverkehr in Ihrem Azure Virtual Network zugelassen oder abgelehnt wird, zusammen mit ausführlichen Informationen zum Debuggen. Es kann Ihnen helfen zu verstehen, ob Ihre NSG-Regeln (Netzwerksicherheitsgruppe) ordnungsgemäß konfiguriert sind. 

## <a name="pre-requisites"></a>Voraussetzungen
Zur Verwendung der Netzwerkkonfigurationsdiagnose muss Network Watcher in Ihrem Abonnement aktiviert sein. Informationen zur Aktivierung finden Sie unter [Erstellen einer Azure Network Watcher-Instanz](./network-watcher-create.md).

## <a name="background"></a>Hintergrund

- Ihre Ressourcen in Azure sind über virtuelle Netzwerke (VNETs) und Subnetze verbunden. Die Sicherheit dieser VNETs und Subnetze kann über eine Netzwerksicherheitsgruppe (NSG) verwaltet werden.
- Eine NSG enthält eine Liste mit Sicherheitsregeln, mit denen Netzwerkdatenverkehr für Ressourcen, mit denen sie verbunden ist, zugelassen oder abgelehnt wird. NSGs können Subnetzen, einzelnen VMs oder einzelnen Netzwerkschnittstellen (NICs) zugeordnet werden, die mit virtuellen Computern verbunden sind. 
- Alle Datenverkehrsflüsse in Ihrem Netzwerk werden mithilfe der Regeln in der entsprechenden NSG ausgewertet.
- Regeln werden auf der Grundlage des Prioritätswerts (aufsteigend) bewertet. 

## <a name="how-does-network-configuration-diagnostic-work"></a>Wie funktioniert die Netzwerkkonfigurationsdiagnose? 

Für einen gegebenen Flow führt das Tool zur Netzwerkkonfigurationsdiagnose eine Simulation des Flows durch und gibt zurück, ob der Flow zugelassen (oder abgelehnt) würde, sowie ausführliche Informationen zu Regeln, die den Flow zulassen/ablehnen.  Kunden müssen Details zu einem Flow wie Quelle, Ziel, Protokoll usw. angeben. Das Tool gibt zurück, ob Datenverkehr zugelassen oder abgelehnt wurde, die NSG-Regeln, die für den angegebenen Flow ausgewertet wurden, und die Auswertungsergebnisse für die einzelnen Regeln.

## <a name="next-steps"></a>Nächste Schritte

Verwenden der Netzwerkkonfigurationsdiagnose über andere Schnittstellen
 - [REST-API](/rest/api/network-watcher/networkwatchers/getnetworkconfigurationdiagnostic)
 - [PowerShell](/powershell/module/az.network/invoke-aznetworkwatchernetworkconfigurationdiagnostic)
 - [Azure-Befehlszeilenschnittstelle](/cli/azure/network/watcher#az_network_watcher_run_configuration_diagnostic)