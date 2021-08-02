---
title: Lastenausgleich für Azure Virtual Desktop-Hostpools (klassisch) – Azure
description: Beschreibung der Lastenausgleichsmethoden für Hostpools in einer Azure Virtual Desktop-Umgebung
author: Heidilohr
ms.topic: conceptual
ms.date: 03/30/2020
ms.author: helohr
manager: femila
ms.openlocfilehash: abd52941efc06b79a21e490e4e94f39a6e1d34be
ms.sourcegitcommit: 8bca2d622fdce67b07746a2fb5a40c0c644100c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2021
ms.locfileid: "111745041"
---
# <a name="host-pool-load-balancing-methods-in-azure-virtual-desktop-classic"></a>Hostpool-Lastenausgleichsmethoden in Azure Virtual Desktop (klassisch)

>[!IMPORTANT]
>Dieser Inhalt gilt für Azure Virtual Desktop (klassisch). Der Dienst unterstützt keine Azure Virtual Desktop-Objekte in Azure Resource Manager. Wenn Sie Azure Virtual Desktop-Objekte in Azure Resource Manager verwalten möchten, helfen Ihnen die Informationen in [diesem Artikel](../host-pool-load-balancing.md) weiter.

Azure Virtual Desktop unterstützt zwei Lastenausgleichsmethoden. Bei beiden Methoden wird ermittelt, welcher Sitzungshost die Sitzung eines Benutzers hostet, wenn dieser eine Verbindung mit einer Ressource in einem Hostpool herstellt.

Unter Azure Virtual Desktop sind folgende Lastausgleichsmethoden verfügbar:

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