---
title: Übersicht über SQL Server Always On-Verfügbarkeitsgruppen
description: Dieser Artikel bietet eine Einführung in SQL Server Always On-Verfügbarkeitsgruppen auf Azure Virtual Machines.
services: virtual-machines
documentationCenter: na
author: MashaMSFT
editor: monicar
tags: azure-service-management
ms.assetid: 601eebb1-fc2c-4f5b-9c05-0e6ffd0e5334
ms.service: virtual-machines-sql
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/13/2017
ms.author: mathoma
ms.custom: seo-lt-2019, devx-track-azurecli
ms.openlocfilehash: 705c7dd602d9c908ec9048d131ba66b21c5b2103
ms.sourcegitcommit: 419cf179f9597936378ed5098ef77437dbf16295
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2020
ms.locfileid: "89006515"
---
# <a name="introducing-sql-server-always-on-availability-groups-on-azure-virtual-machines"></a>Einführung in SQL Server Always On-Verfügbarkeitsgruppen auf Azure Virtual Machines

[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

Dieser Artikel enthält eine Einführung in SQL Server-Verfügbarkeitsgruppen in Azure Virtual Machines. 

Always On-Verfügbarkeitsgruppen in Azure Virtual Machines sind vergleichbar mit lokalen Always On-Verfügbarkeitsgruppen. Weitere Informationen finden Sie unter [AlwaysOn-Verfügbarkeitsgruppen (SQL Server)](https://msdn.microsoft.com/library/hh510230.aspx). 

Das folgende Diagramm veranschaulicht die Komponenten einer vollständigen SQL Server-Verfügbarkeitsgruppe auf Azure Virtual Machines.

![Verfügbarkeitsgruppe](./media/availability-group-overview/00-EndstateSampleNoELB.png)

Der wesentliche Unterschied bei einer Verfügbarkeitsgruppe auf Azure Virtual Machines ist, dass diese virtuellen Computer (VMs) einen [Lastenausgleich](../../../load-balancer/load-balancer-overview.md) erfordern. Der Lastenausgleich speichert die IP-Adressen für den Verfügbarkeitsgruppenlistener. Falls Sie über mehrere Verfügbarkeitsgruppen verfügen, benötigt jede Gruppe einen Listener. Ein einzelner Lastenausgleich kann mehrere Listener unterstützen.

Außerdem werden in einem Azure IaaS-VM-Gast-Failovercluster eine einzelne Netzwerkkarte pro Server (Clusterknoten) und ein einzelnes Subnetz empfohlen. Azure-Netzwerktechnologie bietet physische Redundanz, die zusätzliche Netzwerkkarten und Subnetze in einem Azure IaaS-VM-Gastcluster überflüssig macht. Obwohl im Clustervalidierungsbericht eine Warnung ausgegeben wird, dass die Knoten nur in einem einzigen Netzwerk erreichbar sind, kann diese Warnung für Azure IaaS-VM-Gast-Failovercluster einfach ignoriert werden. 

Um für noch mehr Redundanz und Verfügbarkeit zu sorgen, sollten sich die SQL Server-VMS entweder in derselben [Verfügbarkeitsgruppe](availability-group-manually-configure-prerequisites-tutorial.md#create-availability-sets) oder in verschiedenen [Verfügbarkeitszonen](/azure/availability-zones/az-overview) befinden. 

|  | Windows Server-Version | SQL Server-Version | SQL Server-Edition | WSFC-Quorumkonfiguration | Notfallwiederherstellung mit mehreren Regionen | Unterstützung mehrerer Subnetze | Unterstützung für ein vorhandenes AD | Notfallwiederherstellung mit mehreren Zonen in derselben Region | Dist-AG-Unterstützung ohne AD-Domäne | Dist-AG-Unterstützung ohne Cluster |  
| :------ | :-----| :-----| :-----| :-----| :-----| :-----| :-----| :-----| :-----| :-----|
| **[Azure-Portal](availability-group-azure-portal-configure.md)** | 2019 </br> 2016 | 2019 </br>2017 </br>2016   | Ent | Cloudzeuge | Nein | Ja | Ja | Ja | Nein | Nein |
| **[Azure CLI/PowerShell](availability-group-az-cli-configure.md)** | 2019 </br> 2016 | 2019 </br>2017 </br>2016   | Ent | Cloudzeuge | Nein | Ja | Ja | Ja | Nein | Nein |
| **[Schnellstartvorlagen](availability-group-quickstart-template-configure.md)** | 2019 </br> 2016 | 2019 </br>2017 </br>2016  | Ent | Cloudzeuge | Nein | Ja | Ja | Ja | Nein | Nein |
| **[Manuell](availability-group-manually-configure-prerequisites-tutorial.md)** | All | All | All | All | Ja | Ja | Ja | Ja | Ja | Ja |

Die Vorlage **SQL Server Always On Cluster (Vorschau)** wurde aus Azure Marketplace entfernt und ist nicht mehr verfügbar. 

Wenn Sie zur Erstellung einer SQL Server-Verfügbarkeitsgruppe in Azure Virtual Machines bereit sind, sehen Sie sich die folgenden Tutorials an:

## <a name="manually-with-azure-cli"></a>Manuell über die Azure-Befehlszeilenschnittstelle

Es wird empfohlen, die Azure CLI zum Konfigurieren und Bereitstellen einer Verfügbarkeitsgruppe zu verwenden, da sie dadurch am einfachsten und schnellsten bereitzustellen ist. Mit der Azure-Befehlszeilenschnittstelle können das Erstellen des Windows-Failoverclusters, das Einbinden der SQL Server-VMs in den Cluster und das Erstellen des Listeners und des internen Lastenausgleichs in weniger als insgesamt 30 Minuten erreicht werden. Diese Option erfordert immer noch eine manuelle Erstellung der Verfügbarkeitsgruppe, alle anderen erforderlichen Schritte für die Konfiguration sind jedoch automatisiert. 

Weitere Informationen finden Sie unter [Verwenden der Azure SQL-VM-Befehlszeilenschnittstelle zum Konfigurieren von Always On-Verfügbarkeitsgruppe für SQL Server auf einem virtuellen Azure-Computer](availability-group-az-cli-configure.md). 

## <a name="automatically-with-azure-quickstart-templates"></a>Automatisch mit Azure-Schnellstartvorlagen

In Azure-Schnellstartvorlagen wird der SQL-VM-Ressourcenanbieter genutzt, um den Windows-Failovercluster zu erstellen, SQL Server-VMs in diesen einzubinden, den Listener zu erstellen und den internen Lastenausgleich zu konfigurieren. Diese Option erfordert nach wie vor eine manuelle Erstellung der Verfügbarkeitsgruppe und des internen Lastenausgleichs. Alle anderen erforderlichen Konfigurationsschritte (einschließlich Konfiguration des internen Lastenausgleichs) sind jedoch automatisiert und vereinfacht. 

Weitere Informationen finden Sie unter [Verwenden einer Azure-Schnellstartvorlage zum Konfigurieren von Always On-Verfügbarkeitsgruppen für SQL Server auf einem virtuellen Azure-Computer](availability-group-quickstart-template-configure.md).


## <a name="automatically-with-an-azure-portal-template"></a>Automatisch mit einer Azure-Portal-Vorlage

[Automatisches Konfigurieren der AlwaysOn-Verfügbarkeitsgruppe auf virtuellen Azure-Computern – Resource Manager](availability-group-azure-marketplace-template-configure.md)


## <a name="manually-in-the-azure-portal"></a>Manuell im Azure-Portal

Sie können die virtuellen Computer auch manuell ohne Vorlage erstellen. Schließen Sie zunächst die erforderlichen Vorbereitungen ab, und erstellen Sie anschließend die Verfügbarkeitsgruppe. Weitere Informationen finden Sie in den folgenden Artikeln: 

- [Voraussetzungen für die Erstellung von Always On-Verfügbarkeitsgruppen für SQL Server in Azure Virtual Machines](availability-group-manually-configure-prerequisites-tutorial.md)

- [Erstellen einer AlwaysOn-Verfügbarkeitsgruppe zur Verbesserung der Verfügbarkeit und Notfallwiederherstellung](availability-group-manually-configure-tutorial.md)

## <a name="next-steps"></a>Nächste Schritte

[Konfigurieren einer SQL Server Always On-Verfügbarkeitsgruppe auf virtuellen Azure-Computern in verschiedenen Regionen](availability-group-manually-configure-multiple-regions.md)
