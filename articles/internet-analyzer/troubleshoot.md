---
title: Problembehandlung von Azure Internet Analyzer | Microsoft-Dokumentation
description: Referenz zur Problembehandlung für Azure Internet Analyzer.
services: internet-analyzer
author: diego-perez-botero
ms.service: internet-analyzer
ms.topic: guide
ms.date: 12/04/2019
ms.author: dibotero
ms.openlocfilehash: a265278652c16b4682707470d183a02a55b9a0ec
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "77069216"
---
# <a name="azure-internet-analyzer-troubleshooting"></a>Problembehandlung für Azure Internet Analyzer

In diesem Artikel werden Schritte zur Problembehandlung häufig auftretender Probleme mit Internet Analyzer beschrieben.

## <a name="things-to-keep-in-mind"></a>Bitte beachten
- Das Clientskript muss in eine **HTTPS**-Website eingebettet werden. Wenn das Skript auf einer unverschlüsselten (**http://** ) oder einer lokalen Website (**file://** ) ausgeführt wird, werden keine Messungen gesammelt.
- Messungsdaten werden nur erfasst, wenn das Clientskript des Internet Analyzer-Profils in eine Anwendung eingebettet wurde, die echten Benutzerdatenverkehr empfängt. Synthetischer Datenverkehr (z.B. Azure-WebApp-Leistungstests) führt in der Regel keinen eingebetteten JavaScript-Code aus, sodass für diese Art von Datenverkehr keine Messungen generiert werden.

## <a name="azure-portal"></a>Azure-Portal
**„Für die ausgewählte Filterkombination wurde keine Scorecard generiert“ im Abschnitt „Scorecards“**
- Scorecards werden täglich generiert (am Ende jedes Tags, UTC-Zeit).
- Scorecards werden nur generiert, wenn mehr als 100 Messungen für die ausgewählte Filterkombination (Test, Zeitraum, Land usw.) erfasst wurden.

**„Gesamtanzahl der Messungen“ ist für einen oder beide Endpunkte in einem Test Null**
- Zeitreihen und die Anzahl von Messungen werden ein Mal pro Stunde berechnet. Daher müssen Sie mindestens so lange warten, bis neue Messdaten angezeigt werden.
- Internet Analyzer zählt für die Analyse ausschließlich erfolgreiche Messungen (d.h. HTTP 200-Antworten). Wenn einer oder beide Endpunkte in einem Test nicht erreichbar sind oder einen anderen Code als HTTP 200 zurückgeben, wird für die Gesamtzahl der Messungen Null angezeigt.

## <a name="next-steps"></a>Nächste Schritte
Lesen Sie die [häufig gestellten Fragen zu Internet Analyzer](internet-analyzer-faq.md).
