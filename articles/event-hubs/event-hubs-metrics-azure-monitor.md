---
title: Metriken in Azure Monitor – Azure Event Hubs | Microsoft-Dokumentation
description: Dieser Artikel enthält Informationen zur Verwendung von Azure Monitor zum Überwachen von Azure Event Hubs.
ms.topic: article
ms.date: 02/25/2021
ms.openlocfilehash: 52ab66fe264b85be117eb8e2e21cb89f38d0fcae
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "101715580"
---
# <a name="azure-event-hubs-metrics-in-azure-monitor"></a>Azure Event Hubs-Metriken in Azure Monitor

Event Hubs-Metriken geben Aufschluss über den Zustand von Event Hubs-Ressourcen in Ihrem Azure-Abonnement. Mithilfe eines umfangreichen Satzes von Metrikdaten können Sie die allgemeine Integrität Ihrer Event Hubs nicht nur auf Namespaceebene, sondern auch auf Entitätsebene bewerten. Diese Statistiken können wichtig sein, da sie Sie beim Überwachen des Zustands Ihrer Event Hubs unterstützen. Metriken können auch beim Beheben der Ursachen von Problemen helfen, ohne dass Sie sich an den Azure-Support wenden müssen.

Azure Monitor bietet einheitliche Benutzeroberflächen für die übergreifende Überwachung verschiedener Azure-Dienste. Weitere Informationen finden Sie unter [Überwachung in Microsoft Azure](../azure-monitor/overview.md) und im Beispiel zum [Abrufen von Azure Monitor-Metriken mit .NET](https://github.com/Azure-Samples/monitor-dotnet-metrics-api) auf GitHub.

## <a name="access-metrics"></a>Zugreifen auf Metriken

Azure Monitor bietet mehrere Möglichkeiten für den Metrikzugriff. Sie können über das [Azure-Portal](https://portal.azure.com), über die Azure Monitor-APIs (REST und .NET) sowie über Analyselösungen wie Log Analytics und Event Hubs auf Metriken zugreifen. Weitere Informationen finden Sie unter [Von Azure Monitor gesammelte Überwachungsdaten](../azure-monitor/data-platform.md).

Metriken sind standardmäßig aktiviert, und es stehen Daten für die letzten 30 Tage zur Verfügung. Zur längeren Aufbewahrung können Sie Metrikdaten in einem Azure-Speicherkonto archivieren. Diese Einstellung kann in den [Diagnoseeinstellungen](../azure-monitor/essentials/diagnostic-settings.md) in Azure Monitor konfiguriert werden.


## <a name="access-metrics-in-the-portal"></a>Zugreifen auf Metriken über das Portal

Im [Azure-Portal](https://portal.azure.com) können Sie Metriken im Zeitverlauf überwachen. Das folgende Beispiel zeigt, wie Sie die erfolgreichen und die eingehenden Anforderungen auf Kontoebene anzeigen:

![Anzeigen erfolgreicher Metriken][1]

Sie können auch direkt über den Namespace auf die Metriken zugreifen. Wählen Sie dazu den Namespace und anschließend **Metriken** aus. Wenn Sie Metriken anzeigen möchten, die für den Bereich des Event Hubs gefiltert wurden, wählen Sie den Event Hub und anschließend **Metriken** aus.

Bei Metriken mit Dimensionsunterstützung müssen Sie den gewünschten Dimensionswert als Filter verwenden, wie im folgenden Beispiel zu sehen:

![Filtern mit Dimensionswert][2]

## <a name="billing"></a>Abrechnung

Die Verwendung von Metriken in Azure Monitor ist derzeit kostenlos. Wenn Sie allerdings andere Lösungen zur Metrikdatenerfassung verwenden, fallen unter Umständen Kosten für diese Lösungen an. So werden etwa Gebühren für Azure Storage berechnet, wenn Sie Metrikdaten in einem Azure-Speicherkonto archivieren. Es fallen auch Kosten für Azure an, wenn Sie Metrikdaten für eine ausführlichere Analyse an Azure Monitor-Protokolle streamen.

Die folgenden Metriken bieten Ihnen eine Übersicht über die Integrität eines Diensts. 

> [!NOTE]
> Einige Metriken werden eingestellt, da sie unter einem anderen Namen verwendet werden. Deshalb müssen Sie möglicherweise Ihre Verweise aktualisieren. Metriken, die mit dem Schlüsselwort „veraltet“ markiert sind, werden zukünftig nicht mehr unterstützt.

Alle Metrikwerte werden minütlich an Azure Monitor gesendet. Das Aggregationsintervall definiert das Zeitintervall, für das Metrikwerte dargestellt werden. Das unterstützte Zeitintervall für alle Event Hubs-Metriken beträgt eine Minute.

## <a name="azure-event-hubs-metrics"></a>Azure Event Hubs-Metriken
Eine Liste der Metriken, die vom Dienst unterstützt werden, finden Sie unter [Azure Event Hubs](../azure-monitor/essentials/metrics-supported.md#microsofteventhubnamespaces).

> [!NOTE]
> Wenn ein Benutzerfehler auftritt, aktualisiert Azure Event Hubs die Metrik **Benutzerfehler**, protokolliert jedoch keine weiteren Diagnoseinformationen. Daher müssen Sie Details zu Benutzerfehlern in Ihren Anwendungen erfassen. Sie können auch die Telemetriewerte konvertieren, die generiert werden, wenn Nachrichten an Application Insights gesendet oder dort empfangen werden. Ein Beispiel finden Sie unter [Nachverfolgung mit Application Insights](../service-bus-messaging/service-bus-end-to-end-tracing.md#tracking-with-azure-application-insights).

## <a name="azure-monitor-integration-with-siem-tools"></a>Azure Monitor-Integration mit SIEM-Tools
Durch das Weiterleiten von Überwachungsdaten (Aktivitätsprotokollen, Diagnoseprotokollen usw.) an einen Event Hub mit Azure Monitor erreichen Sie auf einfache Weise eine Integration in SIEM-Tools (Security Information & Event Management). Weitere Informationen finden Sie in den folgenden Artikeln und Blogbeiträgen:

- [Streamen von Azure-Überwachungsdaten an einen Event Hub für die Verwendung durch ein externes Tool](../azure-monitor/essentials/stream-monitoring-data-event-hubs.md)
- [Einführung in Azure Log Integration](/previous-versions/azure/security/fundamentals/azure-log-integration-overview)
- [Verwenden von Azure Monitor zur Integration mit SIEM-Tools](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/)

Wenn Sie in einem Szenario, in dem ein SIEM-Tool Daten von einem Event Hub verwendet, im Metrikendiagramm keine eingehenden Nachrichten oder eingehende Nachrichten, jedoch keine ausgehenden Nachrichten sehen, führen Sie die folgenden Schritte aus:

- Wenn **keine eingehenden Nachrichten** vorhanden sind, bedeutet dies, dass der Azure Monitor-Dienst keine Überwachungs-/Diagnoseprotokolle in den Event Hub verschiebt. Öffnen Sie in einem solchen Szenario ein Supportticket beim Azure Monitor-Team. 
- Sind eingehende Nachrichten, jedoch **keine ausgehenden Nachrichten** vorhanden, heißt dies, dass die Nachrichten von der SIEM-Anwendung nicht gelesen werden. Wenden Sie sich an den SIEM-Anbieter, um festzustellen, ob die Konfiguration des Event Hub für die betreffenden Anwendungen korrekt ist.


## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen finden Sie unter [Übersicht über die Azure-Überwachung](../azure-monitor/overview.md).
* Rufen Sie das [Azure Monitor-Metrikbeispiel mit .NET](https://github.com/Azure-Samples/monitor-dotnet-metrics-api) auf GitHub ab. 

Weitere Informationen zu Event Hubs erhalten Sie unter den folgenden Links:

- Beginnen Sie mit einem Event Hubs-Tutorial
    - [.NET Core](event-hubs-dotnet-standard-getstarted-send.md)
    - [Java](event-hubs-java-get-started-send.md)
    - [Python](event-hubs-python-get-started-send.md)
    - [JavaScript](event-hubs-node-get-started-send.md)
* [Event Hubs – häufig gestellte Fragen](event-hubs-faq.md)
* [Beispielanwendungen mit Event Hubs](https://github.com/Azure/azure-event-hubs/tree/master/samples)

[1]: ./media/event-hubs-metrics-azure-monitor/event-hubs-monitor1.png
[2]: ./media/event-hubs-metrics-azure-monitor/event-hubs-monitor2.png
