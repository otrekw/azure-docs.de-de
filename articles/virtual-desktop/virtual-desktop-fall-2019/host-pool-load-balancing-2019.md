---
title: Lastenausgleich für Windows Virtual Desktop-Hostpools (klassisch) – Azure
description: Enthält eine Beschreibung der Lastenausgleichsmethoden für Hostpools in einer Windows Virtual Desktop-Umgebung.
author: Heidilohr
ms.topic: conceptual
ms.date: 03/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 940863b983b00dbb3c4af590d75868665372f818
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "88002309"
---
# <a name="host-pool-load-balancing-methods-in-windows-virtual-desktop-classic"></a>Hostpool-Lastenausgleichsmethoden in Windows Virtual Desktop (klassisch)

>[!IMPORTANT]
>Dieser Inhalt gilt für den Windows Virtual Desktop-Dienst (klassisch), der keine Windows Virtual Desktop-Objekte in Azure Resource Manager unterstützt. Wenn Sie Windows Virtual Desktop-Objekte in Azure Resource Manager verwalten möchten, helfen Ihnen die Informationen in [diesem Artikel](../host-pool-load-balancing.md) weiter.

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