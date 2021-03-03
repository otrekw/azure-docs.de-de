---
title: Ratenlimits für SMS, E-Mails und Pushbenachrichtigungen
description: Es wird erläutert, wie Azure die Anzahl der möglichen SMS-, E-Mail-, Azure App-Push- oder Webhookbenachrichtigungen aus einer Aktionsgruppe beschränkt.
author: dkamstra
ms.author: dukek
ms.topic: conceptual
ms.date: 3/12/2018
ms.subservice: alerts
ms.openlocfilehash: fc49e923676b63e301b2d63b4274ae11b3043906
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/17/2021
ms.locfileid: "100599674"
---
# <a name="rate-limiting-for-voice-sms-emails-azure-app-push-notifications-and-webhook-posts"></a>Ratenlimits für Sprache, SMS-Nachrichten, E-Mail-Nachrichten, Azure App-Pushbenachrichtigungen und Webhookbeiträge
Das Ratenlimit führt zu einer Unterbrechung der Benachrichtigungen, wenn zu viele an eine bestimmte Telefonnummer, E-Mail-Adresse oder an ein Gerät gesendet werden. Das Ratenlimit stellt sicher, dass Warnungen verwaltbar und verfolgbar sind.

Folgende Ratenlimits gelten:

- **SMS**: Maximal eine SMS alle fünf Minuten
- **Sprache**: Maximal ein Telefonanruf alle fünf Minuten
- **E-Mail**: Maximal 100 E-Mails in einer Stunde.
 
  Für andere Aktionen gelten keine Ratenlimits.

## <a name="rate-limit-rules"></a>Regeln für Ratenlimits
- Für eine bestimmte Telefonnummer oder E-Mail-Adresse wird die Rate begrenzt, wenn sie mehr Nachrichten erhält, als der Schwellenwert zulässt.
- Eine Telefonnummer oder E-Mail-Adresse kann Mitglied von Aktionsgruppen mehrerer Abonnements sein. Das Ratenlimit gilt für alle Abonnements. Es wird wirksam, sobald der Schwellenwert erreicht wird, auch wenn Nachrichten aus mehreren Abonnements gesendet werden.
- Wenn für eine E-Mail-Adresse ein Ratenlimit gilt, wird eine weitere Benachrichtigung gesendet, um über das Ratenlimit zu informieren. Per E-Mail wird mitgeteilt, wann das Ratenlimit abläuft.

## <a name="next-steps"></a>Nächste Schritte ##
* Erfahren Sie mehr über das [SMS-Warnungsverhalten in Aktionsgruppen](alerts-sms-behavior.md).
* Verschaffen Sie sich eine [Übersicht über Aktivitätsprotokollwarnungen](../platform/alerts-overview.md), und erfahren Sie, wie Sie Warnungen empfangen können.  
* Erfahren Sie, wie Sie [Warnungen konfigurieren, wenn eine Dienstintegritätsbenachrichtigung gesendet wird](../../service-health/alerts-activity-log-service-notifications-portal.md).
