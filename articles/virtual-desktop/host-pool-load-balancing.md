---
title: Lastenausgleich für Windows Virtual Desktop-Hostpools – Azure
description: Erfahren Sie mehr über Lastenausgleichsmethoden für Hostpools für eine Windows Virtual Desktop-Umgebung.
author: Heidilohr
ms.topic: conceptual
ms.date: 10/12/2020
ms.author: helohr
manager: femila
ms.openlocfilehash: ed0cf568ca8d011beb7150f23c0187bbe262d5f0
ms.sourcegitcommit: 56b0c7923d67f96da21653b4bb37d943c36a81d6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2021
ms.locfileid: "106446773"
---
# <a name="host-pool-load-balancing-methods"></a>Lastenausgleichsmethoden für Hostpools

>[!IMPORTANT]
>Dieser Inhalt gilt für Windows Virtual Desktop mit Windows Virtual Desktop-Objekten für Azure Resource Manager. Wenn Sie Windows Virtual Desktop (klassisch) ohne Azure Resource Manager-Objekte verwenden, finden Sie weitere Informationen in [diesem Artikel](./virtual-desktop-fall-2019/host-pool-load-balancing-2019.md).

Windows Virtual Desktop unterstützt zwei Lastenausgleichsmethoden. Bei beiden Methoden wird ermittelt, welcher Sitzungshost die Sitzung eines Benutzers hostet, wenn dieser eine Verbindung mit einer Ressource in einem Hostpool herstellt.

Unter Windows Virtual Desktop sind die folgenden Lastenausgleichsmethoden verfügbar:

- Beim Lastenausgleich vom Typ „Breiter Ansatz“ können Sie Benutzersitzungen gleichmäßig auf die Sitzungshosts in einem Hostpool verteilen.
- Beim Lastenausgleich vom Typ „Tiefer Ansatz“ können Sie einen Sitzungshost mit Benutzersitzungen in einem Hostpool füllen. Nachdem für die erste Sitzung der Schwellenwert des Sitzungslimits erreicht wurde, leitet der Lastenausgleich alle neuen Benutzerverbindungen an den nächsten Sitzungshost im Hostpool weiter, bis dessen Limit erreicht ist, usw.

Für jeden Hostpool kann nur jeweils eine Art von Lastenausgleich konfiguriert werden. Unabhängig davon, in welchem Hostpool sie sich befinden, weisen beide Lastenausgleichsmethoden aber das gleiche folgende Verhalten auf:

- Wenn ein Benutzer im Hostpool bereits über eine Sitzung verfügt und eine erneute Verbindung mit dieser Sitzung herstellt, leitet der Lastenausgleich ihn erfolgreich an den Sitzungshost mit der vorhandenen Sitzung um. Dieses Verhalten gilt auch, wenn die AllowNewConnections-Eigenschaft des Sitzungshosts auf FALSE festgelegt ist.
- Wenn ein Benutzer im Hostpool nicht bereits über eine Sitzung verfügt, werden Sitzungshosts, für die die AllowNewConnections-Eigenschaft auf „False“ festgelegt ist, vom Lastenausgleichsmodul während des Lastenausgleichs nicht berücksichtigt.

## <a name="breadth-first-load-balancing-method"></a>Lastenausgleichsmethode „Breiter Ansatz“

Bei der Lastenausgleichsmethode „Breiter Ansatz“ können Sie Benutzerverbindungen so verteilen, dass für das Szenario eine optimale Lösung erzielt wird. Diese Methode ist ideal für Organisationen geeignet, die Benutzern, die eine Verbindung mit ihrer virtuellen Desktopumgebung im Pool herstellen, ein optimales Nutzungserlebnis bieten möchten.

Beim „breiten Ansatz“ werden zuerst Sitzungshosts abgefragt, die neue Verbindungen zulassen. Entsprechend dieser Methode wird dann ein Sitzungshost nach dem Zufallsprinzip aus der Hälfte der Sitzungshosts ausgewählt, die die geringste Anzahl von Sitzungen haben. Gibt es z. B. neun Computer mit 11, 12, 13, 14, 15, 16, 17, 18 und 19 Sitzungen, wird eine neue Sitzung, die Sie erstellen, nicht automatisch auf dem ersten Computer eingerichtet. Stattdessen kann sie auf einem der ersten fünf Computer mit der niedrigsten Anzahl von Sitzungen (11, 12, 13, 14, 15) eingerichtet werden.

## <a name="depth-first-load-balancing-method"></a>Lastenausgleichsmethode „Tiefer Ansatz“

Bei der Lastenausgleichsmethode „Tiefer Ansatz“ können Sie jeweils einen Sitzungshost füllen, um für das Szenario das optimale Ergebnis zu erzielen. Diese Methode ist ideal für kostenbewusste Organisationen geeignet, die sich eine präzisere Steuerung der Anzahl von virtuellen Computern wünschen, die sie für einen Pool zugeordnet haben.

Beim „tiefen Ansatz“ werden zuerst die Sitzungshosts abgefragt, die neue Verbindungen zulassen und für die das Limit für die maximale Anzahl von Sitzungen noch nicht überschritten wurde. Die Methode wählt dann den Sitzungshost mit der höchsten Anzahl von Sitzungen aus. Bei einem Gleichstand wählt die Methode den ersten Sitzungshost der Abfrage aus.

>[!IMPORTANT]
>Der Algorithmus für den tiefenorientierten Lastenausgleich verteilt Sitzungen auf der Grundlage des maximalen Grenzwerts für Hostsitzungen. Dieser Parameter ist erforderlich, wenn der tiefenorientierte Lastenausgleich verwendet wird. Zum Erzielen einer optimalen Benutzererfahrung sollten Sie darauf achten, den Parameter für den maximalen Grenzwert der Hostsitzungen auf eine Zahl festzulegen, die möglichst gut zu Ihrer Umgebung passt.