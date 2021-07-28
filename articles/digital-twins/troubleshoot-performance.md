---
title: Behandeln von Leistungsproblemen
titleSuffix: Azure Digital Twins
description: Tipps zur Behandlung von Leistungsproblemen einer Azure Digital Twins Instanz.
author: baanders
ms.author: baanders
ms.date: 5/11/2021
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 65b1873c1ac4f6ce9b06aad20dbb86e5f4613886
ms.sourcegitcommit: 17345cc21e7b14e3e31cbf920f191875bf3c5914
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2021
ms.locfileid: "110116571"
---
# <a name="troubleshooting-azure-digital-twins-performance"></a>Problembehandlung von Azure Digital Twins: Leistung

Wenn bei der Arbeit mit Azure Digital Twins Verzögerungen oder andere Leistungsprobleme auftreten, können Ihnen die Tipps in diesem Artikel bei der Problembehandlung weiterhelfen.

## <a name="isolate-the-source-of-the-delay"></a>Isolieren der Quelle der Verzögerung

Bestimmen Sie, ob die Verzögerung von Azure Digital Twins oder einem anderen Dienst in Ihrer Lösung stammt. Um dies zu untersuchen, können Sie die Metrik **API-Wartezeit** in [Azure Monitor](../azure-monitor/essentials/quick-monitor-azure-resource.md) über das Azure-Portal verwenden. Anweisungen zum Anzeigen Azure Monitor-Metriken für eine Azure Digital Twins-Instanz finden Sie unter [Problembehandlung: Anzeigen von Metriken mit Azure Monitor](troubleshoot-metrics.md).

## <a name="check-regions"></a>Überprüfen von Regionen

Wenn Ihre Lösung Azure Digital Twins in Kombination mit anderen Azure-Diensten (wie Azure Functions) verwendet, überprüfen Sie die Region für die Bereitstellung der einzelnen Dienste. Wenn Dienste in verschiedenen Regionen bereitgestellt werden, kann dies zu Verzögerungen in Ihrer Lösung führen. Sofern Sie nicht absichtlich eine verteilte Lösung erstellen, sollten Sie erwägen, alle Dienstinstanzen in derselben Region bereitzustellen, um die versehentliche Einführung von Verzögerungen zu vermeiden.

## <a name="leverage-logs"></a>Nutzen von Protokollen

Azure Digital Twins kann Protokolle für Ihre Dienstinstanz zur Überwachung von Leistung, Zugriff und anderen Daten erfassen. Protokolle können an [Log Analytics](../azure-monitor/logs/log-analytics-overview.md) oder Ihren benutzerdefinierten Speichermechanismus gesendet werden. Um die Protokollierung in Ihrer Instanz zu aktivieren, befolgen Sie die Anweisungen unter [Problembehandlung: Einrichten der Diagnose](troubleshoot-diagnostics.md). Sie können die Zeitstempel in den Protokollen analysieren, um Wartezeiten zu messen, um zu bewerten, ob sie konsistent sind, und um ihre Quelle zu verstehen.

## <a name="check-api-frequency"></a>Überprüfen der API-Häufigkeit

Ein weiterer Faktor, der sich auf die Leistung auswirken kann, ist die Zeit, die zum erneuten Autorisieren von API-Aufrufen erforderlich ist. Berücksichtigen Sie die Häufigkeit Ihrer API-Aufrufe. Wenn zwischen den Aufrufen eine Lücke von mehr als 15 Minuten besteht, führt das System möglicherweise bei jedem Aufruf eine erneute Autorisierung durch, was zusätzliche Zeit in Anspruch nimmt. Sie können dies verhindern, indem Sie einen Timer oder ähnliches in Ihrem Code hinzufügen, um sicherzustellen, dass Sie mindestens alle 15 Minuten einmal Azure Digital Twins aufrufen.

## <a name="contact-support"></a>Support kontaktieren

Wenn nach der Problembehandlung mit den oben genannten Schritten weiterhin Leistungsprobleme auftreten, können Sie über „Azure-Hilfe + Support“ eine Supportanfrage erstellen, um zusätzliche Unterstützung bei der Problembehandlung zu erhalten. 

Folgen Sie diesen Schritten:

1. Sammeln Sie [Metriken](troubleshoot-metrics.md) und [Protokolle](troubleshoot-diagnostics.md) für Ihre Instanz.
2. Navigieren Sie zu [Azure-Hilfe + Support](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) im Azure-Portal. Verwenden Sie die Eingabeaufforderungen, um Details zu Ihrem Problem anzugeben, empfohlene Lösungen anzuzeigen, Ihre Metriken/Protokolldateien zu teilen und alle anderen Informationen zu übermitteln, die das Supportteam verwenden kann, um Ihr Problem zu untersuchen. Weitere Informationen zum Erstellen von Supportanfragen finden Sie unter [Erstellen einer Azure-Supportanfrage](../azure-portal/supportability/how-to-create-azure-support-request.md).

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie in den folgenden Artikeln über andere Möglichkeiten zur Problembehandlung Ihrer Azure Digital Twins-Instanz:
* [Problembehandlung: Anzeigen von Metriken mit Azure Monitor](troubleshoot-metrics.md)
* [Problembehandlung: Einrichten der Diagnose](troubleshoot-diagnostics.md).
* [Problembehandlung: Einrichten von Warnungen](troubleshoot-alerts.md).
* [Problembehandlung: Grundlegendes zur Ressourcenintegrität](troubleshoot-resource-health.md)
