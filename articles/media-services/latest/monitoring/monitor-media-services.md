---
title: Überwachen von Media Services
description: Hier erfahren Sie mehr zur Überwachung von Media Services.
author: IngridAtMicrosoft
ms.author: inhenkel
manager: femilia
ms.topic: how-to
ms.service: media-services
ms.custom: subject-monitoring
ms.date: 03/17/2021
ms.openlocfilehash: 90ca92dc19c588d0b19adf009301cf844e0cdbde
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105609043"
---
# <a name="monitor-media-services"></a>Überwachen von Media Services

Wenn Sie über unternehmenskritische Anwendungen und Geschäftsprozesse verfügen, die auf Azure-Ressourcen beruhen, sollten Sie Verfügbarkeit, Leistung und Betrieb dieser Ressourcen überwachen. In diesem Artikel wird das Überwachen von Daten beschrieben, die von Media Services generiert wurden. Außerdem wird erläutert, wie Sie diese Daten mit den Funktionen von Azure Monitor analysieren und Warnungen dafür erstellen.

## <a name="metrics-are-useful"></a>Praktische Metriken

Hier sind Beispiele dafür, wie Sie durch das Überwachen von Media Services-Metriken Informationen über die Leistung Ihrer Anwendungen erhalten. Media Services-Metriken können beispielsweise für folgende Fragen genutzt werden:

- Wie überwache ich meinen Standardstreamingendpunkt, um zu wissen, wann die Grenzwerte überschritten wurden?
- Wie erkenne ich, ob ich über genügend Premium-Streamingendpunkt-Skalierungseinheiten verfüge?
- Wie kann ich eine Warnung festlegen, um zu wissen, wann die Streamingendpunkte hochskaliert werden müssen?
- Wie lege ich eine Warnung fest, um zu wissen, wann der für das Konto konfigurierte maximale ausgehende Datenverkehr erreicht wurde?
- Wie kann ich die Aufschlüsselung der fehlgeschlagenen Anforderungen und die Fehlerursachen anzeigen?
- Wie kann ich erkennen, wie viele HLS- oder DASH-Anforderungen aus dem Objekt-Manager abgerufen werden?
- Wie lege ich eine Warnung fest, die mich informiert, dass der Schwellenwert für fehlgeschlagene Anforderungen erreicht wurde?

<!--THIS DOESN'T BELONG HERE Concurrency becomes a concern for the number of Streaming Endpoints used in a single account over time. You need to keep in mind the relationship between the number of concurrent streams with complex publishing parameters like dynamic packaging to multiple protocols, multiple DRM encryptions etc. Each additional published live stream adds to the CPU and output bandwidth on the Streaming Endpoint. With that in mind, you should use Azure Monitor to closely watch the Streaming Endpoint's utilization (CPU and Egress capacity) to make certain that you are scaling it appropriately (or split traffic out between multiple Streaming Endpoints if you are getting into very high concurrency).-->

<!-- Optional diagram showing monitoring for your service. If you need help creating one, contact 
robb@microsoft.com -->

## <a name="what-is-azure-monitor"></a>Was ist Azure Monitor?

Media Services generiert Überwachungsdaten mithilfe von [Azure Monitor](../../../azure-monitor/overview.md), einem umfassenden Stapelüberwachungsdienst in Azure, der umfassende Funktionen zum Überwachen Ihrer Azure-Ressourcen sowie von Ressourcen in anderen Clouds und lokalen Umgebungen bereitstellt.

Beginnen Sie mit dem Artikel [Überwachen von Azure-Ressourcen mit Azure Monitor](../../../azure-monitor/essentials/monitor-azure-resource.md), in dem die folgenden Konzepte beschrieben werden:

- Was ist Azure Monitor?
- Kosten für die Überwachung
- In Azure gesammelte Überwachungsdaten
- Konfigurieren der Datensammlung
- Standardtools in Azure zum Analysieren von Überwachungsdaten sowie zum Generieren von Warnungen

## <a name="monitoring-data"></a>Überwachungsdaten

Media Services sammelt dieselben Arten von Überwachungsdaten wie andere Azure-Ressourcen, die in [Überwachungsdaten von Azure-Ressourcen](../../../azure-monitor/essentials/monitor-azure-resource.md#monitoring-data) beschrieben werden.

Alle von Azure Monitor gesammelten Daten gehören einem von zwei Grundtypen an: Metriken und Protokollen. Diese beiden Typen ermöglichen Folgendes:

- Visualisieren und Analysieren der Metrikdaten im Metrik-Explorer
- Überwachen von Media Services-Diagnoseprotokollen und Erstellen zugehöriger Warnungen und Benachrichtigungen
- Protokolle lassen folgende Vorgänge zu:
  - An Azure Storage senden
  - An Azure Event Hubs streamen
  - In Log Analytics exportieren
  - Dienste anderer Anbieter nutzen

Ausführliche Informationen zu den von Media Services erstellten Metriken und Protokollen finden Sie im Artikel [Referenz zu Media Services-Überwachungsdaten](monitor-media-services-data-reference.md).

## <a name="collection-and-routing"></a>Sammlung und Routing

*Plattformmetriken* und das *Aktivitätsprotokoll* werden erfasst und gespeichert, können aber mithilfe einer Diagnoseeinstellung an andere Speicherorte geleitet werden.  

*Ressourcenprotokolle* werden **erst** dann erfasst und gespeichert, nachdem Sie eine Diagnoseeinstellung erstellt und die Protokolle an mindestens einen Speicherort geleitet haben.

Ausführliche Informationen zum Erstellen einer Diagnoseeinstellung über das Azure-Portal, die Befehlszeilenschnittstelle oder PowerShell finden Sie im Artikel [Erstellen einer Diagnoseeinstellung zum Sammeln von Plattformprotokollen und Metriken in Azure](../../../azure-monitor/essentials/diagnostic-settings.md).

Wenn Sie eine Diagnoseeinstellung erstellen, legen Sie fest, welche Kategorien von Protokollen gesammelt werden sollen. Die Kategorien für Media Services sind in der [Referenz zu Media Services-Überwachungsdaten](monitor-media-services-data-reference.md) aufgeführt.

## <a name="analyzing-metrics"></a>Analysieren von Metriken

Sie können im Metrik-Explorer Metriken für Media Services mit Metriken aus anderen Azure-Diensten analysieren, indem Sie im Menü **Azure Monitor** die Option **Metriken** öffnen. Ausführliche Informationen zur Verwendung dieses Tools finden Sie unter [Erste Schritte mit dem Azure-Metrik-Explorer](../../../azure-monitor/essentials/metrics-getting-started.md).

Eine Liste der für Media Services erfassten Metriken finden Sie in der [Referenz zu Media Services-Überwachungsdaten](monitor-media-services-data-reference.md).

## <a name="analyzing-logs"></a>Analysieren von Protokollen

Daten in Azure Monitor-Protokollen werden in Tabellen gespeichert, wobei jede Tabelle ihren eigenen Satz an eindeutigen Eigenschaften hat.  

Alle Ressourcenprotokolle in Azure Monitor enthalten dieselben Felder, gefolgt von dienstspezifischen Feldern. Das allgemeine Schema wird in [Azure Monitor-Ressourcenprotokollschema](../../../azure-monitor/essentials/resource-logs-schema.md#top-level-common-schema) beschrieben.

Das Schema für Media Services-Ressourcenprotokolle finden Sie in der [Referenz zu Media Services-Überwachungsdaten](monitor-media-services-data-reference.md).

Das [Aktivitätsprotokoll](../../../azure-monitor/essentials/activity-log.md) ist ein Plattformprotokoll in Azure, das einen Einblick in Ereignisse auf Abonnementebene ermöglicht. Sie können es unabhängig anzeigen oder an Azure Monitor-Protokolle weiterleiten, in denen Sie mithilfe von Log Analytics viel komplexere Abfragen durchführen können.

Eine Liste der für Media Services erfassten Typen von Ressourcenprotokollen finden Sie in der [Referenz zu Media Services-Überwachungsdaten](monitor-media-services-data-reference.md).

### <a name="why-would-i-want-to-use-diagnostic-logs"></a>Warum sollte ich Diagnoseprotokolle verwenden?

Mit Diagnoseprotokollen können Sie beispielsweise Folgendes untersuchen:

- Die Anzahl bereitgestellter Lizenzen nach DRM-Typ
- Die Anzahl bereitgestellter Lizenzen nach Richtlinie
- Fehler nach DRM- oder Richtlinientyp
- Anzahl nicht autorisierter Lizenzanforderungen von Clients

## <a name="alerts"></a>Alerts

Azure Monitor-Warnungen informieren Sie proaktiv, wenn wichtige Bedingungen in Ihren Überwachungsdaten gefunden werden. Sie ermöglichen Ihnen, Probleme in Ihrem System zu identifizieren und zu beheben, bevor Ihre Kunden sie bemerken. Sie können Warnungen für [Metriken](../../../azure-monitor/alerts/alerts-metric-overview.md), [Protokolle](../../../azure-monitor/alerts/alerts-unified-log.md) und das [Aktivitätsprotokoll](../../../azure-monitor/alerts/activity-log-alerts.md) festlegen.

Media Services-Metriken werden in regelmäßigen Abständen gesammelt, unabhängig davon, ob sich der Wert ändert oder nicht. Sie eignen sich gut für Warnmeldungen, da sie häufig abgefragt werden können. Eine Warnung kann mit relativ einfacher Logik schnell ausgelöst werden.

<!--
The following table lists common and recommended alert rules for Media Services.

<!-- Fill in the table with metric and log alerts that would be valuable for your service. Change the format as necessary to make it more readable
**PLACEHOLDER** table

| Alert type | Condition | Description  |
|:---|:---|:---|
| | | |
| | | |
-->

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [monitoring-next-steps](../includes/monitoring-next-steps.md)]
