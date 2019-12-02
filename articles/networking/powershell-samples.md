---
title: Azure PowerShell-Beispiele – Netzwerke
description: Azure PowerShell-Beispiele
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: mtillman
editor: tysonn
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 05/24/2017
ms.author: gwallace
ms.openlocfilehash: fb4b63d8329f0a175252b819653e192c746489b6
ms.sourcegitcommit: a170b69b592e6e7e5cc816dabc0246f97897cb0c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2019
ms.locfileid: "74092033"
---
# <a name="azure-powershell-samples-for-networking"></a>Azure PowerShell-Beispiele für Netzwerke

Die folgende Tabelle enthält Links zu Skripts, die mithilfe von Azure PowerShell erstellt wurden.

| | |
|-|-|
|**Konnektivität zwischen Azure-Ressourcen**||
| [Erstellen eines virtuellen Netzwerks für Anwendungen mit mehreren Ebenen](./scripts/virtual-network-powershell-sample-multi-tier-application.md?toc=%2fazure%2fnetworking%2ftoc.json) | Erstellt ein virtuelles Netzwerk mit Front-End- und Back-End-Subnetzen. Der Datenverkehr am Front-End-Subnetz ist auf HTTP beschränkt, während der Datenverkehr zum Back-End-Subnetz auf SQL und Port 1433 beschränkt ist. |
| [Herstellen einer Peerverbindung zwischen zwei virtuellen Netzwerken](./scripts/virtual-network-powershell-sample-peer-two-virtual-networks.md?toc=%2fazure%2fnetworking%2ftoc.json) | Erstellt zwei virtuelle Netzwerke in derselben Region und stellt eine Verbindung zwischen diesen her. |
| [Weiterleiten von Datenverkehr über ein virtuelles Netzwerkgerät](./scripts/virtual-network-powershell-sample-route-traffic-through-nva.md?toc=%2fazure%2fnetworking%2ftoc.json) | Erstellt ein virtuelles Netzwerk mit Front-End- und Back-End-Subnetzen und einer VM, die Datenverkehr zwischen den zwei Subnetzen weiterleiten kann. |
| [Filtern von ein- und ausgehenden VM-Netzwerkdatenverkehr](./scripts/virtual-network-powershell-filter-network-traffic.md?toc=%2fazure%2fnetworking%2ftoc.json) | Erstellt ein virtuelles Netzwerk mit Front-End- und Back-End-Subnetzen. Der im Front-End-Subnetz eingehende Netzwerkdatenverkehr ist auf HTTP und HTTPS beschränkt. Aus dem Back-End-Subnetz ausgehender Datenverkehr an das Internet ist nicht zulässig. |
|**Lastenausgleich und Datenverkehrsrichtung**||
| [Lastenausgleich für den Datenverkehr zu virtuellen Computern für Hochverfügbarkeit](./scripts/load-balancer-windows-powershell-sample-nlb.md?toc=%2fazure%2fnetworking%2ftoc.json) | Erstellt mehrere virtuelle Computer in einer Konfiguration mit hoher Verfügbarkeit und Lastenausgleich. |
| [Lastenausgleich für mehrere Websites auf VMs](./scripts/load-balancer-windows-powershell-load-balance-multiple-websites-vm.md?toc=%2fazure%2fnetworking%2ftoc.json) | Erstellt zwei VMs mit verschiedenen IP-Konfigurationen, die zu einer über Azure Load Balancer aufrufbaren Verfügbarkeitsgruppe hinzugefügt werden. |
| [Weiterleiten von Datenverkehr über mehrere Regionen hinweg für hohe Anwendungsverfügbarkeit](./scripts/traffic-manager-powershell-websites-high-availability.md?toc=%2fazure%2fnetworking%2ftoc.json) |  Erstellt zwei App Service-Pläne, zwei Web-Apps, ein Traffic Manager-Profil und zwei Traffic Manager-Endpunkte. |
| | |
