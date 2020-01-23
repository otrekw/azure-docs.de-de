---
title: Problembehandlung von Azure Internet Analyzer | Microsoft-Dokumentation
description: Referenz zur Problembehandlung für Azure Internet Analyzer.
services: internet-analyzer
author: diego-perez-botero
ms.service: internet-analyzer
ms.topic: guide
ms.date: 12/04/2019
ms.author: dibotero
ms.openlocfilehash: 74bf0422bbe6c2c1c84365c1e8f9329a01ff9fdd
ms.sourcegitcommit: f9601bbccddfccddb6f577d6febf7b2b12988911
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/12/2020
ms.locfileid: "75909038"
---
# <a name="azure-internet-analyzer-troubleshooting"></a>Problembehandlung für Azure Internet Analyzer

In diesem Artikel werden Schritte zur Problembehandlung häufig auftretender Probleme mit Internet Analyzer beschrieben.

## <a name="azure-portal"></a>Azure-Portal
**„Für diese Scorecard wurden nicht genügend Messungen erfasst“ im Abschnitt „Scorecards“**

Beachten Sie dabei Folgendes:
- Das Clientskript muss in eine **HTTPS**-Website eingebettet werden. Wenn das Skript auf einer unverschlüsselten (**http://** ) oder einer lokalen Website (**file://** ) ausgeführt wird, werden keine Messungen gesammelt.
- Messungsdaten werden nur erfasst, wenn das Clientskript des Internet Analyzer-Profils in eine Anwendung eingebettet wurde, die echten Benutzerdatenverkehr empfängt. Synthetischer Datenverkehr (z.B. Azure-WebApp-Leistungstests) führt in der Regel keinen eingebetteten JavaScript-Code aus, sodass für diese Art von Datenverkehr keine Messungen generiert werden.
- Zeitreihen werden ein Mal pro Stunde generiert, sodass Sie mindestens so lange warten müssen, bis neue Messungsdaten angezeigt werden.
- Scorecards werden täglich generiert (am Ende jedes Tags, UTC-Zeit).
- Scorecards werden nur generiert, wenn mehr als 100 Messungen für die ausgewählte Filterkombination (Test, Zeitraum, Land usw.) erfasst wurden.

## <a name="next-steps"></a>Nächste Schritte
Lesen Sie die [häufig gestellten Fragen zu Internet Analyzer](internet-analyzer-faq.md).
