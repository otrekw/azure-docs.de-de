---
title: Azure PowerShell-Beispiele – Service Fabric
description: Hier erfahren Sie mehr über die Erstellung und Verwaltung von Azure Service Fabric-Clustern, -Apps und -Diensten mithilfe von Powershell.
ms.topic: sample
ms.date: 11/29/2018
ms.custom: mvc
ms.openlocfilehash: f9ba1b4833b3aff13284a68c23f398ea47ef2ae2
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/03/2020
ms.locfileid: "75645649"
---
# <a name="azure-service-fabric-powershell-samples"></a>Azure Service Fabric PowerShell-Beispiele

Die folgende Tabelle enthält Links zu Beispielen von PowerShell-Skripts, die Service Fabric-Cluster, -Anwendungen und -Dienste erstellen und verwalten.

[!INCLUDE [links to azure CLI and service fabric CLI](../../includes/service-fabric-powershell.md)]

| | |
|-|-|
| **Cluster erstellen** ||
| [Erstellen eines Clusters (Azure)](./scripts/service-fabric-powershell-create-secure-cluster-cert.md)| Erstellt einen Azure Service Fabric-Cluster. |
| **Verwalten des Clusters, der Knoten und der Infrastruktur** ||
| [Hinzufügen eines Anwendungszertifikats](./scripts/service-fabric-powershell-add-application-certificate.md)| Erstellt ein X.509-Zertifikat für Key Vault und stellt es für eine VM-Skalierungsgruppe in Ihrem Cluster bereit. |
| [Aktualisieren des RDP-Portbereichs auf Cluster-VMs](./scripts/service-fabric-powershell-change-rdp-port-range.md)|Ändert den RDP-Portbereich auf Clusterknoten-VMs in einem bereitgestellten Cluster|
| [Aktualisieren des Administratorbenutzers und des zugehörigen Kennworts für Clusterknoten-VMs](./scripts/service-fabric-powershell-change-rdp-user-and-pw.md) | Aktualisiert den Administratorbenutzer und das zugehörige Kennwort für Clusterknoten-VMs |
| [Öffnen eines Ports im Lastenausgleichsmodul](./scripts/service-fabric-powershell-open-port-in-load-balancer.md) | Öffnen Sie einen Anwendungsport im Azure-Lastenausgleich, um eingehenden Datenverkehr an einem bestimmten Port zuzulassen. |
| [Erstellen einer Netzwerksicherheitsgruppen-Eingangsregel](./scripts/service-fabric-powershell-add-nsg-rule.md) | Erstellen Sie eine Netzwerksicherheitsgruppen-Eingangsregel zum Zulassen von eingehendem Datenverkehr an den Cluster an einem bestimmten Port. |
| **Anwendungen verwalten** ||
| [Bereitstellen von Anwendungen](./scripts/service-fabric-powershell-deploy-application.md)| Stellt eine Anwendung in einem Cluster bereit.|
| [Upgraden einer Anwendung](./scripts/service-fabric-powershell-upgrade-application.md)| Aktualisieren Sie eine Anwendung.|
| [Entfernen einer Anwendung](./scripts/service-fabric-powershell-remove-application.md)| Entfernt eine Anwendung aus einem Cluster.|
