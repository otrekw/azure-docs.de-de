---
title: Problembehandlung von Azure Internet Analyzer | Microsoft-Dokumentation
description: Referenz zur Problembehandlung für Azure Internet Analyzer.
services: internet-analyzer
author: diego-perez-botero
ms.service: internet-analyzer
ms.topic: troubleshooting
ms.date: 12/04/2019
ms.author: dibotero
ms.openlocfilehash: fe173ec4d4f28444d43739ea2ed51e43021916b1
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "84744356"
---
# <a name="azure-internet-analyzer-troubleshooting"></a>Problembehandlung für Azure Internet Analyzer

In diesem Artikel werden Schritte zur Problembehandlung häufig auftretender Probleme mit Internet Analyzer beschrieben.

## <a name="things-to-keep-in-mind"></a>Bitte beachten
- Das Clientskript muss in eine **HTTPS**-Website eingebettet werden. Wenn das Skript auf einer unverschlüsselten (**http://** ) oder einer lokalen Website (**file://** ) ausgeführt wird, werden keine Messungen gesammelt.
- Messungsdaten werden nur erfasst, wenn das Clientskript des Internet Analyzer-Profils in eine Anwendung eingebettet wurde, die echten Benutzerdatenverkehr empfängt. Synthetischer Datenverkehr (z.B. Azure-WebApp-Leistungstests) führt in der Regel keinen eingebetteten JavaScript-Code aus, sodass für diese Art von Datenverkehr keine Messungen generiert werden.

## <a name="azure-portal"></a>Azure-Portal
**„Für die ausgewählte Filterkombination wurde keine Scorecard generiert“ im Abschnitt „Scorecards“**
- Scorecards werden täglich generiert (am Ende jedes Tags, UTC-Zeit).
- Scorecards werden nur generiert, wenn mehr als 100 Messungen für die ausgewählte Filterkombination (Test, Zeitraum, Land/Region usw.) erfasst wurden.

**„Gesamtanzahl der Messungen“ ist für einen oder beide Endpunkte in einem Test Null**
- Zeitreihen und die Anzahl von Messungen werden einmal pro Stunde berechnet. Daher müssen Sie mindestens so lange warten, bis neue Messdaten angezeigt werden.
- Internet Analyzer zählt für die Analyse ausschließlich erfolgreiche Messungen (d.h. HTTP 200-Antworten). Wenn einer oder beide Endpunkte in einem Test nicht erreichbar sind oder einen anderen Code als HTTP 200 zurückgeben, wird für die Gesamtzahl der Messungen Null angezeigt.

## <a name="next-steps"></a>Nächste Schritte
Lesen Sie die [häufig gestellten Fragen zu Internet Analyzer](internet-analyzer-faq.md).
