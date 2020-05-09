---
title: Lastenausgleich für Windows Virtual Desktop-Hostpools – Azure
description: Enthält eine Beschreibung der Lastenausgleichsmethoden für Hostpools in einer Windows Virtual Desktop-Umgebung.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 03/21/2019
ms.author: helohr
manager: lizross
ms.openlocfilehash: 15d50033a1316601dd8c36bd5748c659f4397d66
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2020
ms.locfileid: "82612043"
---
# <a name="host-pool-load-balancing-methods"></a>Lastenausgleichsmethoden für Hostpools

>[!IMPORTANT]
>Dieser Artikel gilt für das Update vom Frühjahr 2020 mit Windows Virtual Desktop-Objekten für Azure Resource Manager. Wenn Sie das Windows Virtual Desktop-Release vom Herbst 2019 ohne Azure Resource Manager-Objekte verwenden, finden Sie weitere Informationen in [diesem Artikel](./virtual-desktop-fall-2019/host-pool-load-balancing-2019.md).
>
> Das Windows Virtual Desktop-Update vom Frühjahr 2020 befindet sich derzeit in der öffentlichen Vorschauphase. Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. 
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Windows Virtual Desktop unterstützt zwei Lastenausgleichsmethoden. Bei beiden Methoden wird ermittelt, welcher Sitzungshost die Sitzung eines Benutzers hostet, wenn dieser eine Verbindung mit einer Ressource in einem Hostpool herstellt.

Unter Windows Virtual Desktop sind die folgenden Lastenausgleichsmethoden verfügbar:

- Beim Lastenausgleich vom Typ „Breiter Ansatz“ können Sie Benutzersitzungen gleichmäßig auf die Sitzungshosts in einem Hostpool verteilen.
- Beim Lastenausgleich vom Typ „Tiefer Ansatz“ können Sie einen Sitzungshost mit Benutzersitzungen in einem Hostpool füllen. Nachdem für die erste Sitzung der Schwellenwert des Sitzungslimits erreicht wurde, leitet der Lastenausgleich alle neuen Benutzerverbindungen an den nächsten Sitzungshost im Hostpool weiter, bis dessen Limit erreicht ist, usw.

Für jeden Hostpool kann nur jeweils eine Art von Lastenausgleich konfiguriert werden. Unabhängig davon, in welchem Hostpool sie sich befinden, weisen beide Lastenausgleichsmethoden aber das gleiche folgende Verhalten auf:

- Wenn ein Benutzer im Hostpool bereits über eine Sitzung verfügt und eine erneute Verbindung mit dieser Sitzung herstellt, leitet der Lastenausgleich ihn erfolgreich an den Sitzungshost mit der vorhandenen Sitzung um. Dieses Verhalten gilt auch, wenn die AllowNewConnections-Eigenschaft des Sitzungshosts auf FALSE festgelegt ist.
- Wenn ein Benutzer im Hostpool nicht bereits über eine Sitzung verfügt, werden Sitzungshosts, für die die AllowNewConnections-Eigenschaft auf „False“ festgelegt ist, vom Lastenausgleichsmodul während des Lastenausgleichs nicht berücksichtigt.

## <a name="breadth-first-load-balancing-method"></a>Lastenausgleichsmethode „Breiter Ansatz“

Bei der Lastenausgleichsmethode „Breiter Ansatz“ können Sie Benutzerverbindungen so verteilen, dass für das Szenario eine optimale Lösung erzielt wird. Diese Methode ist ideal für Organisationen geeignet, die Benutzern, die eine Verbindung mit ihrer virtuellen Desktopumgebung im Pool herstellen, ein optimales Nutzungserlebnis bieten möchten.

Beim „breiten Ansatz“ werden zuerst Sitzungshosts abgefragt, die neue Verbindungen zulassen. Die Methode wählt dann den Sitzungshost mit der geringsten Anzahl von Sitzungen aus. Bei einem Gleichstand wählt die Methode den ersten Sitzungshost der Abfrage aus.

## <a name="depth-first-load-balancing-method"></a>Lastenausgleichsmethode „Tiefer Ansatz“

Bei der Lastenausgleichsmethode „Tiefer Ansatz“ können Sie jeweils einen Sitzungshost füllen, um für das Szenario das optimale Ergebnis zu erzielen. Diese Methode ist ideal für kostenbewusste Organisationen geeignet, die sich eine präzisere Steuerung der Anzahl von virtuellen Computern wünschen, die sie für einen Pool zugeordnet haben.

Beim „tiefen Ansatz“ werden zuerst die Sitzungshosts abgefragt, die neue Verbindungen zulassen und für die das Limit für die maximale Anzahl von Sitzungen noch nicht überschritten wurde. Die Methode wählt dann den Sitzungshost mit der höchsten Anzahl von Sitzungen aus. Bei einem Gleichstand wählt die Methode den ersten Sitzungshost der Abfrage aus.