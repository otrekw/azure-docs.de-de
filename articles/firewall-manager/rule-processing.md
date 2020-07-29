---
title: Logik für die Azure Firewall Manager-Regelverarbeitung
description: Enthält eine Beschreibung der Logik für die Azure Firewall-Regelverarbeitung.
services: firewall-manager
author: vhorne
ms.service: firewall-manager
ms.topic: conceptual
ms.date: 06/30/2020
ms.author: victorh
ms.openlocfilehash: 9184bf7baa85420e067edb4c0aafccb7e6711225
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86512179"
---
# <a name="azure-firewall-rule-processing-logic"></a>Logik für die Azure Firewall-Regelverarbeitung

Azure Firewall verfügt über NAT-Regeln, Netzwerkregeln und Anwendungsregeln. Die Regeln werden gemäß dem Regeltyp verarbeitet.

## <a name="network-rules-and-applications-rules"></a>Netzwerkregeln und Anwendungsregeln

Zuerst werden die Netzwerkregeln angewendet, und dann die Anwendungsregeln. Die Regeln können zur Beendigung von Vorgängen führen. Wenn sich für eine Netzwerkregel eine Übereinstimmung ergibt, werden die Anwendungsregeln also nicht verarbeitet.  Wenn keine Netzwerkregel übereinstimmt und als Paketprotokoll HTTP/HTTPS verwendet wird, wird das Paket von den Anwendungsregeln ausgewertet. Falls sich immer noch keine Übereinstimmung ergibt, wird das Paket von der Regelsammlung der Infrastruktur ausgewertet. Wenn sich auch hierbei keine Übereinstimmung ergibt, wird das Paket standardmäßig abgelehnt.

## <a name="nat-rules"></a>NAT-Regeln

Eingehende Konnektivität kann aktiviert werden, indem DNAT (Destination Network Address Translation) konfiguriert wird. Die Vorgehensweise wird unter [Tutorial: Filtern von eingehendem Datenverkehr per Azure Firewall-DNAT im Azure-Portal](../firewall/tutorial-firewall-dnat.md) beschrieben. DNAT-Regeln werden zuerst angewendet. Wenn sich eine Übereinstimmung ergibt, wird eine implizite entsprechende Netzwerkregel hinzugefügt, um den übersetzten Datenverkehr zuzulassen. Sie können dieses Verhalten außer Kraft setzen, indem Sie explizit eine Netzwerkregelsammlung mit Ablehnungsregeln hinzufügen, die für den übersetzten Datenverkehr geeignet sind. Für diese Verbindungen werden keine Anwendungsregeln angewendet.

## <a name="inherited-rules"></a>Geerbte Regeln

Netzwerkregelsammlungen, die von einer übergeordneten Richtlinie geerbt wurden, haben immer Vorrang vor Netzwerkregelsammlungen, die als Teil Ihrer neuen Richtlinie definiert werden. Gleiches gilt auch für Anwendungsregelsammlungen. Allerdings werden Netzwerkregelsammlungen unabhängig von der Vererbung immer vor Anwendungsregelsammlungen verarbeitet.

Ihre Richtlinie erbt standardmäßig den Threat Intelligence-Modus ihrer übergeordneten Richtlinie. Sie können dies außer Kraft setzen, indem Sie den Threat Intelligence-Modus auf der Seite mit Richtlinieneinstellungen auf einen anderen Wert festlegen. Er kann nur mit einem strengeren Wert überschrieben werden. Beispiel: Wenn die übergeordnete Richtlinie auf *Nur Warnung* festgelegt ist, können Sie diese lokale Richtlinie mit *Warnen und ablehnen* konfigurieren, aber nicht deaktivieren.

## <a name="next-steps"></a>Nächste Schritte

- [Weitere Informationen zu Azure Firewall Manager](overview.md)