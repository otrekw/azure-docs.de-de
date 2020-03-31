---
title: include file
description: include file
services: batch
documentationcenter: ''
author: LauraBrenner
manager: evansma
editor: ''
ms.assetid: ''
ms.service: batch
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: ''
ms.date: 03/04/2020
ms.author: labrenne
ms.custom: include file
ms.openlocfilehash: e9460108499ca76d1b149b61cebe3d3081bf6544
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79086260"
---
### <a name="general-requirements"></a>Allgemeine Anforderungen

* Das VNET muss sich im gleichen Abonnement und in der gleichen Region befinden wie das für die Poolerstellung verwendete Batch-Konto.

* Der Pool, der das VNET verwendet, kann bis zu 4.096 Knoten umfassen.

* Das für den Pool angegebene Subnetz muss über ausreichend nicht zugewiesene IP-Adressen verfügen, um die Anzahl virtueller Computer aufnehmen zu können, die für den Pool geplant sind, d. h. die Summe der `targetDedicatedNodes`- und `targetLowPriorityNodes`-Eigenschaften des Pools. Wenn das Subnetz nicht über ausreichend nicht zugewiesene IP-Adressen verfügt, belegt der Pool teilweise die Computeknoten und es tritt ein Anpassungsfehler auf.

* Ihr Azure Storage-Endpunkt muss von jedem benutzerdefinierten DNS-Server aufgelöst werden, der Ihr VNET bedient. Insbesondere URLs im Format `<account>.table.core.windows.net`, `<account>.queue.core.windows.net` und `<account>.blob.core.windows.net` müssen auflösbar sein.

Darüber hinaus gelten ggf. noch weitere VNET-Anforderungen. Diese hängen jedoch davon ab, ob sich der Batch-Pool in der Konfiguration des virtuellen Computers oder in der Cloud Services-Konfiguration befindet. Für neue Poolbereitstellungen in einem VNET wird die Konfiguration des virtuellen Computers empfohlen.

### <a name="pools-in-the-virtual-machine-configuration"></a>Pools in der Konfiguration des virtuellen Computers

**Unterstützte VNETs:** Nur Azure Resource Manager-basierte VNETs

**Subnetz-ID:** Verwenden Sie den *Ressourcenbezeichner* des Subnetzes, wenn Sie das Subnetz über die Batch-APIs angeben. Die Subnetz-ID hat folgendes Format:

  ```
  /subscriptions/{subscription}/resourceGroups/{group}/providers/Microsoft.Network/virtualNetworks/{network}/subnets/{subnet}
  ```

**Berechtigungen:** Überprüfen Sie, ob Ihre Sicherheitsrichtlinien oder -sperren für das Abonnement oder die Ressourcengruppe Ihres VNETs die VNET-Verwaltungsberechtigungen eines Benutzers einschränken.

**Zusätzliche Netzwerkressourcen:** Batch ordnet in der Ressourcengruppe, die das VNET enthält, automatisch zusätzliche Netzwerkressourcen zu.

> [!IMPORTANT]
>Dabei ordnet Batch pro 50 dedizierten Knoten (oder pro 20 Knoten mit niedriger Priorität) jeweils eine Netzwerksicherheitsgruppe (NSG), eine öffentliche IP-Adresse und einen Lastenausgleich zu. Diese Ressourcen werden durch die [Ressourcenkontingente](../articles/azure-resource-manager/management/azure-subscription-service-limits.md) des Abonnements beschränkt. Bei umfangreichen Pools muss ggf. eine Kontingenterhöhung für eine oder mehrere der Ressourcen angefordert werden.

#### <a name="network-security-groups-batch-default"></a>Netzwerksicherheitsgruppen: Batch-Standard

Das Subnetz muss eingehende Kommunikation des Batch-Diensts zulassen, um Aufgaben auf den Computeknoten planen zu können, und ausgehende Kommunikation, um gemäß den Anforderungen Ihrer Workload mit Azure Storage und anderen Ressourcen kommunizieren zu können. Für Pools in der Konfiguration des virtuellen Computers fügt Batch NSGs auf der Ebene der Netzwerkschnittstellen (NICs) hinzu, die an Computeknoten angefügt sind. Diese NSGs werden mit den folgenden zusätzlichen Regeln konfiguriert:

* Eingehender TCP-Datenverkehr an den Ports 29876 und 29877 von IP-Adressen des Batch-Diensts, die dem Diensttag `BatchNodeManagement` entsprechen
* Eingehender TCP-Datenverkehr am Port 22 (Linux-Knoten) oder Port 3389 (Windows-Knoten), um Remotezugriff zu ermöglichen Für bestimmte Arten von Multi-Instanz-Aufgaben unter Linux (z.B. MPI) müssen Sie auch den Datenverkehr auf dem SSH-Port 22 für IPs in dem Subnetz zulassen, das die Batch-Computeknoten enthält. Dieser kann durch NSG-Regeln auf Subnetzebene blockiert werden (siehe unten).
* Ausgehender Datenverkehr an allen Ports zum virtuellen Netzwerk Dies kann durch NSG-Regeln auf Subnetzebene angepasst werden (siehe unten).
* Ausgehender Datenverkehr an allen Ports zum Internet. Dies kann durch NSG-Regeln auf Subnetzebene angepasst werden (siehe unten).

> [!IMPORTANT]
> Seien Sie vorsichtig, wenn Sie Eingangs- und Ausgangsregeln in von Batch konfigurierten NSGs ändern. Falls die Kommunikation mit den Computeknoten im angegebenen Subnetz durch eine Netzwerksicherheitsgruppe (NSG) verhindert wird, legt der Batch-Dienst den Zustand der Computeknoten auf **Nicht verwendbar** fest. Darüber hinaus dürfen keine Ressourcensperren auf von Batch erstellte Ressourcen angewendet werden. Dies kann sonst dazu führen, dass die Bereinigung von Ressourcen infolge der vom Benutzer initiierten Aktionen (etwa Löschen eines Pools) verhindert wird.

#### <a name="network-security-groups-specifying-subnet-level-rules"></a>Netzwerksicherheitsgruppen: Angeben von Regeln auf Subnetzebene

Sie müssen keine NSGs auf der Subnetzebene virtueller Netzwerke angeben, da Batch eigene NSGs konfiguriert (siehe oben). Wenn eine Ihrer NSGs dem Subnetz zugeordnet ist, in dem Batch-Computeknoten bereitgestellt sind, oder wenn Sie benutzerdefinierte NSG-Regeln anwenden möchten, um die angewendeten Standardeinstellungen außer Kraft zu setzen, müssen Sie diese NSG mindestens mit den in den folgenden Tabellen gezeigten Eingangs- und Ausgangssicherheitsregeln konfigurieren.

Konfigurieren Sie eingehenden Datenverkehr am Port 3389 (Windows) bzw. am Port 22 (Linux) nur, wenn Sie Remotezugriff auf die Computeknoten von externen Quellen aus zulassen müssen. Möglicherweise müssen Sie Regeln für Port 22 unter Linux aktivieren, wenn Sie Unterstützung für Tasks mit mehreren Instanzen mit bestimmten MPI-Runtimes benötigen. Das Zulassen von Datenverkehr an diesen Ports ist für die Verwendung der Poolcomputeknoten nicht zwingend erforderlich.

**Eingangssicherheitsregeln**

| Quell-IP-Adressen | Quelldiensttag | Quellports | Destination | Zielports | Protocol | Aktion |
| --- | --- | --- | --- | --- | --- | --- |
| – | `BatchNodeManagement` [Diensttag](../articles/virtual-network/security-overview.md#service-tags) (bei Verwendung der regionalen Variante in derselben Region wie Ihr Batch-Konto) | * | Any | 29876–29877 | TCP | Allow |
| Benutzerquellen-IPs für den Remotezugriff auf Computeknoten und/oder Computeknoten-Subnetz für Multi-Instanz-Aufgaben unter Linux, falls erforderlich. | – | * | Any | 3389 (Windows), 22 (Linux) | TCP | Allow |

**Ausgangssicherheitsregeln**

| `Source` | Quellports | Destination | Zieldiensttag | Zielports | Protocol | Aktion |
| --- | --- | --- | --- | --- | --- | --- |
| Any | * | [Diensttag](../articles/virtual-network/security-overview.md#service-tags) | `Storage` Speicher (bei Verwendung der regionalen Variante in der gleichen Region wie Ihr Batch-Konto) | 443 | TCP | Allow |

### <a name="pools-in-the-cloud-services-configuration"></a>Pools in der Cloud Services-Konfiguration

**Unterstützte VNETs:** Nur klassische VNETs

**Subnetz-ID:** Verwenden Sie den *Ressourcenbezeichner* des Subnetzes, wenn Sie das Subnetz über die Batch-APIs angeben. Die Subnetz-ID hat folgendes Format:

  ```
  /subscriptions/{subscription}/resourceGroups/{group}/providers/Microsoft.ClassicNetwork /virtualNetworks/{network}/subnets/{subnet}
  ```

**Berechtigungen:** Der Dienstprinzipal `Microsoft Azure Batch` muss für das angegebene VNET über die RBAC-Rolle `Classic Virtual Machine Contributor` verfügen.

#### <a name="network-security-groups"></a>Netzwerksicherheitsgruppen

Das Subnetz muss eingehende Kommunikation des Batch-Diensts zulassen, um Aufgaben auf den Computeknoten planen zu können, und ausgehende Kommunikation, um mit Azure Storage und anderen Ressourcen kommunizieren zu können.

Sie müssen keine NSG angeben, da Batch nur eingehende Kommunikation von Batch-IP-Adressen an die Poolknoten konfiguriert. Wenn das angegebene Subnetz allerdings über zugewiesene NSGs und/oder eine Firewall verfügt, konfigurieren Sie die Eingangs- und Ausgangssicherheitsregeln gemäß den folgenden Tabellen. Falls die Kommunikation mit den Computeknoten im angegebenen Subnetz durch eine Netzwerksicherheitsgruppe (NSG) verhindert wird, legt der Batch-Dienst den Zustand der Computeknoten auf **Nicht verwendbar** fest.

Konfigurieren Sie eingehenden Datenverkehr am Port 3389 für Windows, wenn Sie RDP-Zugriff auf die Poolknoten zulassen müssen. Dieser Schritt ist nicht erforderlich, um die Poolknoten verwenden zu können.

**Eingangssicherheitsregeln**

| Quell-IP-Adressen | Quellports | Destination | Zielports | Protocol | Aktion |
| --- | --- | --- | --- | --- | --- |
Any <br /><br />Dafür ist zwar im Grunde die Zulassung aller IP-Adressen erforderlich, der Batch-Dienst wendet jedoch auf der Ebene jedes Knotens eine ACL-Regel an, die alle nicht vom Batch-Dienst stammenden IP-Adressen herausfiltert. | * | Any | 10100, 20100, 30100 | TCP | Allow |
| Optional, um RDP-Zugriff auf Computeknoten zu ermöglichen. | * | Any | 3389 | TCP | Allow |

**Ausgangssicherheitsregeln**

| `Source` | Quellports | Destination | Zielports | Protocol | Aktion |
| --- | --- | --- | --- | --- | --- |
| Any | * | Any | 443  | Any | Allow |
