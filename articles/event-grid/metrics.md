---
title: Von Azure Event Grid unterstützte Metriken
description: In diesem Artikel sind Azure Monitor-Metriken aufgeführt, die vom Azure Event Grid-Dienst unterstützt werden.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 04/29/2020
ms.author: spelluru
ms.openlocfilehash: 643df2f4cc6347e0fd56f9124b68f1888ab85e26
ms.sourcegitcommit: 1895459d1c8a592f03326fcb037007b86e2fd22f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/01/2020
ms.locfileid: "82629759"
---
# <a name="metrics-supported-by-azure-event-grid"></a>Von Azure Event Grid unterstützte Metriken
Dieser Artikel enthält Listen von Event Grid-Metriken, die nach Namespaces kategorisiert sind. 

## <a name="microsofteventgriddomains"></a>Microsoft.EventGrid/domains

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|PublishSuccessCount|Veröffentlichte Ereignisse|Anzahl|Gesamt|Gesamtanzahl der zu diesem Thema veröffentlichten Ereignisse|Thema|
|PublishFailCount|Ereignisse mit Veröffentlichungsfehler|Anzahl|Gesamt|Gesamtanzahl der zu diesem Thema fehlerhaft veröffentlichten Ereignisse|Topic, ErrorType, Error|
|PublishSuccessLatencyInMs|Latenz erfolgreicher Veröffentlichungen|Millisekunden|Gesamt|Latenz erfolgreicher Veröffentlichungen in Millisekunden|Keine|
|MatchedEventCount|Übereinstimmende Ereignisse|Anzahl|Gesamt|Gesamtzahl der Ereignisse, die diesem Ereignisabonnement zugeordnet sind|Topic, EventSubscriptionName, DomainEventSubscriptionName|
|DeliveryAttemptFailCount|Ereignisse mit Übermittlungsfehler|Anzahl|Gesamt|Gesamtzahl der Ereignisse, die nicht an dieses Ereignisabonnement übermittelt werden konnten|Topic,EventSubscriptionName,DomainEventSubscriptionName,Error,ErrorType|
|DeliverySuccessCount|Übermittelte Ereignisse|Anzahl|Gesamt|Gesamtzahl der Ereignisse, die an dieses Ereignisabonnement übermittelt wurden|Topic, EventSubscriptionName, DomainEventSubscriptionName|
|DestinationProcessingDurationInMs|Zielverarbeitungsdauer|Millisekunden|Average|Zielverarbeitungsdauer in Millisekunden|Topic, EventSubscriptionName, DomainEventSubscriptionName|
|DroppedEventCount|Gelöschte Ereignisse|Anzahl|Gesamt|Gesamtanzahl der gelöschten Ereignisse, die mit diesem Ereignisabonnement übereinstimmen|Topic, EventSubscriptionName, DomainEventSubscriptionName, DropReason|
|DeadLetteredCount|Unzustellbare Ereignisse|Anzahl|Gesamt|Gesamtanzahl der unzustellbaren Ereignisse, die mit diesem Ereignisabonnement übereinstimmen|Topic,EventSubscriptionName,DomainEventSubscriptionName,DeadLetterReason|

## <a name="microsofteventgridtopics"></a>Microsoft.EventGrid/topics

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|PublishSuccessCount|Veröffentlichte Ereignisse|Anzahl|Gesamt|Gesamtanzahl der zu diesem Thema veröffentlichten Ereignisse|Keine|
|PublishFailCount|Ereignisse mit Veröffentlichungsfehler|Anzahl|Gesamt|Gesamtanzahl der zu diesem Thema fehlerhaft veröffentlichten Ereignisse|ErrorType, Error|
|UnmatchedEventCount|Ereignisse ohne Übereinstimmung|Anzahl|Gesamt|Gesamtzahl der Ereignisse, die mit keinem der Ereignisabonnements für dieses Thema übereinstimmen|Keine|
|PublishSuccessLatencyInMs|Latenz erfolgreicher Veröffentlichungen|Millisekunden|Gesamt|Latenz erfolgreicher Veröffentlichungen in Millisekunden|Keine|
|MatchedEventCount|Übereinstimmende Ereignisse|Anzahl|Gesamt|Gesamtzahl der Ereignisse, die diesem Ereignisabonnement zugeordnet sind|EventSubscriptionName|
|DeliveryAttemptFailCount|Ereignisse mit Übermittlungsfehler|Anzahl|Gesamt|Gesamtzahl der Ereignisse, die nicht an dieses Ereignisabonnement übermittelt werden konnten|Error,ErrorType,EventSubscriptionName|
|DeliverySuccessCount|Übermittelte Ereignisse|Anzahl|Gesamt|Gesamtzahl der Ereignisse, die an dieses Ereignisabonnement übermittelt wurden|EventSubscriptionName|
|DestinationProcessingDurationInMs|Zielverarbeitungsdauer|Millisekunden|Average|Zielverarbeitungsdauer in Millisekunden|EventSubscriptionName|
|DroppedEventCount|Gelöschte Ereignisse|Anzahl|Gesamt|Gesamtanzahl der gelöschten Ereignisse, die mit diesem Ereignisabonnement übereinstimmen|DropReason, EventSubscriptionName|
|DeadLetteredCount|Unzustellbare Ereignisse|Anzahl|Gesamt|Gesamtanzahl der unzustellbaren Ereignisse, die mit diesem Ereignisabonnement übereinstimmen|DeadLetterReason, EventSubscriptionName|

## <a name="microsofteventgridsystemtopics"></a>Microsoft.EventGrid/systemTopics

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|PublishSuccessCount|Veröffentlichte Ereignisse|Anzahl|Gesamt|Gesamtanzahl der zu diesem Thema veröffentlichten Ereignisse|Keine|
|PublishFailCount|Ereignisse mit Veröffentlichungsfehler|Anzahl|Gesamt|Gesamtanzahl der zu diesem Thema fehlerhaft veröffentlichten Ereignisse|ErrorType, Error|
|UnmatchedEventCount|Ereignisse ohne Übereinstimmung|Anzahl|Gesamt|Gesamtzahl der Ereignisse, die mit keinem der Ereignisabonnements für dieses Thema übereinstimmen|Keine|
|PublishSuccessLatencyInMs|Latenz erfolgreicher Veröffentlichungen|Millisekunden|Gesamt|Latenz erfolgreicher Veröffentlichungen in Millisekunden|Keine|
|MatchedEventCount|Übereinstimmende Ereignisse|Anzahl|Gesamt|Gesamtzahl der Ereignisse, die diesem Ereignisabonnement zugeordnet sind|EventSubscriptionName|
|DeliveryAttemptFailCount|Ereignisse mit Übermittlungsfehler|Anzahl|Gesamt|Gesamtzahl der Ereignisse, die nicht an dieses Ereignisabonnement übermittelt werden konnten|Error,ErrorType,EventSubscriptionName|
|DeliverySuccessCount|Übermittelte Ereignisse|Anzahl|Gesamt|Gesamtzahl der Ereignisse, die an dieses Ereignisabonnement übermittelt wurden|EventSubscriptionName|
|DestinationProcessingDurationInMs|Zielverarbeitungsdauer|Millisekunden|Average|Zielverarbeitungsdauer in Millisekunden|EventSubscriptionName|
|DroppedEventCount|Gelöschte Ereignisse|Anzahl|Gesamt|Gesamtanzahl der gelöschten Ereignisse, die mit diesem Ereignisabonnement übereinstimmen|DropReason, EventSubscriptionName|
|DeadLetteredCount|Unzustellbare Ereignisse|Anzahl|Gesamt|Gesamtanzahl der unzustellbaren Ereignisse, die mit diesem Ereignisabonnement übereinstimmen|DeadLetterReason, EventSubscriptionName|

## <a name="microsofteventgrideventsubscriptions"></a>Microsoft.EventGrid/eventSubscriptions

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|MatchedEventCount|Übereinstimmende Ereignisse|Anzahl|Gesamt|Gesamtzahl der Ereignisse, die diesem Ereignisabonnement zugeordnet sind|Keine|
|DeliveryAttemptFailCount|Ereignisse mit Übermittlungsfehler|Anzahl|Gesamt|Gesamtzahl der Ereignisse, die nicht an dieses Ereignisabonnement übermittelt werden konnten|Error, ErrorType|
|DeliverySuccessCount|Übermittelte Ereignisse|Anzahl|Gesamt|Gesamtzahl der Ereignisse, die an dieses Ereignisabonnement übermittelt wurden|Keine|
|DestinationProcessingDurationInMs|Zielverarbeitungsdauer|Millisekunden|Average|Zielverarbeitungsdauer in Millisekunden|Keine|
|DroppedEventCount|Gelöschte Ereignisse|Anzahl|Gesamt|Gesamtanzahl der gelöschten Ereignisse, die mit diesem Ereignisabonnement übereinstimmen|DropReason|
|DeadLetteredCount|Unzustellbare Ereignisse|Anzahl|Gesamt|Gesamtanzahl der unzustellbaren Ereignisse, die mit diesem Ereignisabonnement übereinstimmen|DeadLetterReason|

## <a name="microsofteventgridextensiontopics"></a>Microsoft.EventGrid/extensionTopics

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|PublishSuccessCount|Veröffentlichte Ereignisse|Anzahl|Gesamt|Gesamtanzahl der zu diesem Thema veröffentlichten Ereignisse|Keine|
|PublishFailCount|Ereignisse mit Veröffentlichungsfehler|Anzahl|Gesamt|Gesamtanzahl der zu diesem Thema fehlerhaft veröffentlichten Ereignisse|ErrorType, Error|
|UnmatchedEventCount|Ereignisse ohne Übereinstimmung|Anzahl|Gesamt|Gesamtzahl der Ereignisse, die mit keinem der Ereignisabonnements für dieses Thema übereinstimmen|Keine|
|PublishSuccessLatencyInMs|Latenz erfolgreicher Veröffentlichungen|Millisekunden|Gesamt|Latenz erfolgreicher Veröffentlichungen in Millisekunden|Keine|

## <a name="next-steps"></a>Nächste Schritte
Lesen Sie den folgenden Artikel: [Diagnoseprotokolle](diagnostic-logs.md)
