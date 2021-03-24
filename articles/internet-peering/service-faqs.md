---
title: Peering Service – häufig gestellte Fragen
titleSuffix: Azure
description: Peering Service – häufig gestellte Fragen
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: reference
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: 507e503b881df123ffc6694c53b0e9cc9b6a8872
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "75774025"
---
# <a name="peering-service---faqs"></a>Häufig gestellte Fragen zu Peering Service

Hier finden Sie Antworten auf allgemeine Fragen.

**Kann ein Netzbetreiber das vorhandene Direct Peering mit Microsoft zur Unterstützung von Peering Service verwenden?**

Ja, ein Netzbetreiber kann sein vorhandenes PNI nutzen, um Peering Service zu unterstützen. Ein Peering Service-PNI erfordert Diversität zur Unterstützung von Hochverfügbarkeit. Wenn ein vorhandenes PNI bereits Diversität aufweist, ist keine neue Infrastruktur erforderlich. Wenn ein vorhandenes PNI Diversität benötigt, kann es erweitert werden.

**Kann ein Netzbetreiber neues Direct Peering mit Microsoft zur Unterstützung von Peering Service verwenden?**

Ja, das ist ebenfalls möglich. Microsoft arbeitet mit dem Netzbetreiber zusammen, um neues Direct Peering zur Unterstützung von Peering Service zu erstellen  

**Warum ist Direct Peering eine Voraussetzung für die Unterstützung von Peering Service?**

Einer der wichtigsten Zwecke des Peering Service liegt darin, mithilfe eines gut verbundenen SP Konnektivität mit Microsoft Onlinediensten bereitzustellen. PNI liegen immer im Gbit/s-Bereich und bilden daher einen Grundbaustein für Verbindungen mit hohem Durchsatz zwischen dem Netzbetreiber und Microsoft.

**Wie lauten die Diversitätsanforderungen für Direct Peering zur Unterstützung von Peering Service?**

Ein PNI muss lokale Redundanz und Georedundanz unterstützen. Lokale Redundanz ist als zwei verschiedene Sets von Pfaden an einer bestimmten Peeringsite definiert. Für Georedundanz muss der Netzbetreiber über zusätzliche Konnektivität an einem anderen Microsoft Edge-Standort verfügen, falls der Hauptstandort ausfällt. Für die kurze Dauer von Ausfällen kann der Netzbetreiber den Datenverkehr über den Backupstandort weiterleiten.

**Der Netzbetreiber bietet bereits SLA und Internet auf Unternehmensniveau an, wie unterscheidet sich dieses Angebot?**

Einige Netzbetreiber bieten SLA und Internet auf Unternehmensniveau in ihrem Teil des Netzwerks. Beim Peering Service bietet Microsoft Datenverkehr mit SLA-Angebot im Microsoft-Netzwerk an. Durch die Wahl von Peering Service erhält der Kunde ein End-to-End-SLA. Die SLA vom eigenen Standort zum Microsoft Edge im ISP-Netzwerk kann vom ISP abgedeckt werden. Die SLA im globalen Netzwerk von Microsoft von Microsoft Edge zur Endbenutzeranwendung wird jetzt von Microsoft abgedeckt.

**Wenn ein Dienstanbieter bereits mithilfe von PNI Microsoft-Peer ist, welche Änderungen sind dann erforderlich, um Peering Service zu unterstützen?**

* Softwareänderungen, um einen Peering Service-Benutzer und seinen Datenverkehr zu identifizieren. Dies kann Änderungen an Routingrichtlinien erfordern, um den Datenverkehr eines Benutzers am nächstgelegenen Microsoft Edge über eine Peering Service-Verbindung auszutauschen.
* Stellen Sie sicher, dass die Konnektivität über lokale Redundanz und Georedundanz verfügt.
