---
title: Von Azure Event Grid unterstützte Metriken
description: In diesem Artikel sind Azure Monitor-Metriken aufgeführt, die vom Azure Event Grid-Dienst unterstützt werden.
ms.topic: conceptual
ms.date: 03/17/2021
ms.openlocfilehash: 321e318f9dab87fde20b33a6a3a906b020ada622
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "104588731"
---
# <a name="metrics-supported-by-azure-event-grid"></a>Von Azure Event Grid unterstützte Metriken
Dieser Artikel enthält Listen von Event Grid-Metriken, die nach Namespaces kategorisiert sind. 

## <a name="system-topics"></a>Systemthemen

|Metrik|Über Diagnoseeinstellungen exportierbar?|Metrikanzeigename|Einheit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|---|
|AdvancedFilterEvaluationCount|Ja|Erweiterte Filterauswertungen|Anzahl|Gesamt|Gesamtanzahl erweiterter Filter, die für dieses Thema in Ereignisabonnements ausgewertet werden.|EventSubscriptionName|
|DeadLetteredCount|Ja|Unzustellbare Ereignisse|Anzahl|Gesamt|Gesamtanzahl der unzustellbaren Ereignisse, die mit diesem Ereignisabonnement übereinstimmen|DeadLetterReason, EventSubscriptionName|
|DeliveryAttemptFailCount|Nein|Ereignisse mit Übermittlungsfehler|Anzahl|Gesamt|Gesamtzahl der Ereignisse, die nicht an dieses Ereignisabonnement übermittelt werden konnten|Error, ErrorType, EventSubscriptionName|
|DeliverySuccessCount|Ja|Übermittelte Ereignisse|Anzahl|Gesamt|Gesamtzahl der Ereignisse, die an dieses Ereignisabonnement übermittelt wurden|EventSubscriptionName|
|DestinationProcessingDurationInMs|Nein|Zielverarbeitungsdauer|Millisekunden|Average|Zielverarbeitungsdauer in Millisekunden|EventSubscriptionName|
|DroppedEventCount|Ja|Gelöschte Ereignisse|Anzahl|Gesamt|Gesamtanzahl der gelöschten Ereignisse, die mit diesem Ereignisabonnement übereinstimmen|DropReason, EventSubscriptionName|
|MatchedEventCount|Ja|Übereinstimmende Ereignisse|Anzahl|Gesamt|Gesamtzahl der Ereignisse, die diesem Ereignisabonnement zugeordnet sind|EventSubscriptionName|
|PublishFailCount|Ja|Ereignisse mit Veröffentlichungsfehler|Anzahl|Gesamt|Gesamtanzahl der zu diesem Thema fehlerhaft veröffentlichten Ereignisse|ErrorType, Error|
|PublishSuccessCount|Ja|Veröffentlichte Ereignisse|Anzahl|Gesamt|Gesamtanzahl der zu diesem Thema veröffentlichten Ereignisse|Keine Dimensionen|
|PublishSuccessLatencyInMs|Ja|Latenz erfolgreicher Veröffentlichungen|Millisekunden|Gesamt|Latenz erfolgreicher Veröffentlichungen in Millisekunden|Keine Dimensionen|
|UnmatchedEventCount|Ja|Ereignisse ohne Übereinstimmung|Anzahl|Gesamt|Gesamtzahl der Ereignisse, die mit keinem der Ereignisabonnements für dieses Thema übereinstimmen|Keine Dimensionen|


## <a name="custom-topics"></a>Benutzerdefinierte Themen

|Metrik|Über Diagnoseeinstellungen exportierbar?|Metrikanzeigename|Einheit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|---|
|AdvancedFilterEvaluationCount|Ja|Erweiterte Filterauswertungen|Anzahl|Gesamt|Gesamtanzahl erweiterter Filter, die für dieses Thema in Ereignisabonnements ausgewertet werden.|EventSubscriptionName|
|DeadLetteredCount|Ja|Unzustellbare Ereignisse|Anzahl|Gesamt|Gesamtanzahl der unzustellbaren Ereignisse, die mit diesem Ereignisabonnement übereinstimmen|DeadLetterReason, EventSubscriptionName|
|DeliveryAttemptFailCount|Nein|Ereignisse mit Übermittlungsfehler|Anzahl|Gesamt|Gesamtzahl der Ereignisse, die nicht an dieses Ereignisabonnement übermittelt werden konnten|Error, ErrorType, EventSubscriptionName|
|DeliverySuccessCount|Ja|Übermittelte Ereignisse|Anzahl|Gesamt|Gesamtzahl der Ereignisse, die an dieses Ereignisabonnement übermittelt wurden|EventSubscriptionName|
|DestinationProcessingDurationInMs|Nein|Zielverarbeitungsdauer|Millisekunden|Average|Zielverarbeitungsdauer in Millisekunden|EventSubscriptionName|
|DroppedEventCount|Ja|Gelöschte Ereignisse|Anzahl|Gesamt|Gesamtanzahl der gelöschten Ereignisse, die mit diesem Ereignisabonnement übereinstimmen|DropReason, EventSubscriptionName|
|MatchedEventCount|Ja|Übereinstimmende Ereignisse|Anzahl|Gesamt|Gesamtzahl der Ereignisse, die diesem Ereignisabonnement zugeordnet sind|EventSubscriptionName|
|PublishFailCount|Ja|Ereignisse mit Veröffentlichungsfehler|Anzahl|Gesamt|Gesamtanzahl der zu diesem Thema fehlerhaft veröffentlichten Ereignisse|ErrorType, Error|
|PublishSuccessCount|Ja|Veröffentlichte Ereignisse|Anzahl|Gesamt|Gesamtanzahl der zu diesem Thema veröffentlichten Ereignisse|Keine Dimensionen|
|PublishSuccessLatencyInMs|Ja|Latenz erfolgreicher Veröffentlichungen|Millisekunden|Gesamt|Latenz erfolgreicher Veröffentlichungen in Millisekunden|Keine Dimensionen|
|UnmatchedEventCount|Ja|Ereignisse ohne Übereinstimmung|Anzahl|Gesamt|Gesamtzahl der Ereignisse, die mit keinem der Ereignisabonnements für dieses Thema übereinstimmen|Keine Dimensionen|

## <a name="domains"></a>Domänen

|Metrik|Über Diagnoseeinstellungen exportierbar?|Metrikanzeigename|Einheit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|---|
|AdvancedFilterEvaluationCount|Ja|Erweiterte Filterauswertungen|Anzahl|Gesamt|Gesamtanzahl erweiterter Filter, die für dieses Thema in Ereignisabonnements ausgewertet werden.|Topic, EventSubscriptionName, DomainEventSubscriptionName|
|DeadLetteredCount|Ja|Unzustellbare Ereignisse|Anzahl|Gesamt|Gesamtanzahl der unzustellbaren Ereignisse, die mit diesem Ereignisabonnement übereinstimmen|Topic, EventSubscriptionName, DomainEventSubscriptionName, DeadLetterReason|
|DeliveryAttemptFailCount|Nein|Ereignisse mit Übermittlungsfehler|Anzahl|Gesamt|Gesamtzahl der Ereignisse, die nicht an dieses Ereignisabonnement übermittelt werden konnten|Topic, EventSubscriptionName, DomainEventSubscriptionName, Error, ErrorType|
|DeliverySuccessCount|Ja|Übermittelte Ereignisse|Anzahl|Gesamt|Gesamtzahl der Ereignisse, die an dieses Ereignisabonnement übermittelt wurden|Topic, EventSubscriptionName, DomainEventSubscriptionName|
|DestinationProcessingDurationInMs|Nein|Zielverarbeitungsdauer|Millisekunden|Average|Zielverarbeitungsdauer in Millisekunden|Topic, EventSubscriptionName, DomainEventSubscriptionName|
|DroppedEventCount|Ja|Gelöschte Ereignisse|Anzahl|Gesamt|Gesamtanzahl der gelöschten Ereignisse, die mit diesem Ereignisabonnement übereinstimmen|Topic, EventSubscriptionName, DomainEventSubscriptionName, DropReason|
|MatchedEventCount|Ja|Übereinstimmende Ereignisse|Anzahl|Gesamt|Gesamtzahl der Ereignisse, die diesem Ereignisabonnement zugeordnet sind|Topic, EventSubscriptionName, DomainEventSubscriptionName|
|PublishFailCount|Ja|Ereignisse mit Veröffentlichungsfehler|Anzahl|Gesamt|Gesamtanzahl der zu diesem Thema fehlerhaft veröffentlichten Ereignisse|Topic, ErrorType, Error|
|PublishSuccessCount|Ja|Veröffentlichte Ereignisse|Anzahl|Gesamt|Gesamtanzahl der zu diesem Thema veröffentlichten Ereignisse|Thema|
|PublishSuccessLatencyInMs|Ja|Latenz erfolgreicher Veröffentlichungen|Millisekunden|Gesamt|Latenz erfolgreicher Veröffentlichungen in Millisekunden|Keine Dimensionen|

## <a name="event-subscriptions"></a>Ereignisabonnements

|Metrik|Über Diagnoseeinstellungen exportierbar?|Metrikanzeigename|Einheit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|---|
|DeadLetteredCount|Ja|Unzustellbare Ereignisse|Anzahl|Gesamt|Gesamtanzahl der unzustellbaren Ereignisse, die mit diesem Ereignisabonnement übereinstimmen|DeadLetterReason|
|DeliveryAttemptFailCount|Nein|Ereignisse mit Übermittlungsfehler|Anzahl|Gesamt|Gesamtzahl der Ereignisse, die nicht an dieses Ereignisabonnement übermittelt werden konnten|Error, ErrorType|
|DeliverySuccessCount|Ja|Übermittelte Ereignisse|Anzahl|Gesamt|Gesamtzahl der Ereignisse, die an dieses Ereignisabonnement übermittelt wurden|Keine Dimensionen|
|DestinationProcessingDurationInMs|Nein|Zielverarbeitungsdauer|Millisekunden|Average|Zielverarbeitungsdauer in Millisekunden|Keine Dimensionen|
|DroppedEventCount|Ja|Gelöschte Ereignisse|Anzahl|Gesamt|Gesamtanzahl der gelöschten Ereignisse, die mit diesem Ereignisabonnement übereinstimmen|DropReason|
|MatchedEventCount|Ja|Übereinstimmende Ereignisse|Anzahl|Gesamt|Gesamtzahl der Ereignisse, die diesem Ereignisabonnement zugeordnet sind|Keine Dimensionen|


## <a name="extension-topics"></a>Themen zur Erweiterung

|Metrik|Über Diagnoseeinstellungen exportierbar?|Metrikanzeigename|Einheit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|---|
|PublishFailCount|Ja|Ereignisse mit Veröffentlichungsfehler|Anzahl|Gesamt|Gesamtanzahl der zu diesem Thema fehlerhaft veröffentlichten Ereignisse|ErrorType, Error|
|PublishSuccessCount|Ja|Veröffentlichte Ereignisse|Anzahl|Gesamt|Gesamtanzahl der zu diesem Thema veröffentlichten Ereignisse|Keine Dimensionen|
|PublishSuccessLatencyInMs|Ja|Latenz erfolgreicher Veröffentlichungen|Millisekunden|Gesamt|Latenz erfolgreicher Veröffentlichungen in Millisekunden|Keine Dimensionen|
|UnmatchedEventCount|Ja|Ereignisse ohne Übereinstimmung|Anzahl|Gesamt|Gesamtzahl der Ereignisse, die mit keinem der Ereignisabonnements für dieses Thema übereinstimmen|Keine Dimensionen|

## <a name="partner-namespaces"></a>Partnernamespaces

|Metrik|Über Diagnoseeinstellungen exportierbar?|Metrikanzeigename|Einheit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|---|
|DeadLetteredCount|Ja|Unzustellbare Ereignisse|Anzahl|Gesamt|Gesamtanzahl der unzustellbaren Ereignisse, die mit diesem Ereignisabonnement übereinstimmen|DeadLetterReason, EventSubscriptionName|
|DeliveryAttemptFailCount|Nein|Ereignisse mit Übermittlungsfehler|Anzahl|Gesamt|Gesamtzahl der Ereignisse, die nicht an dieses Ereignisabonnement übermittelt werden konnten|Error, ErrorType, EventSubscriptionName|
|DeliverySuccessCount|Ja|Übermittelte Ereignisse|Anzahl|Gesamt|Gesamtzahl der Ereignisse, die an dieses Ereignisabonnement übermittelt wurden|EventSubscriptionName|
|DestinationProcessingDurationInMs|Nein|Zielverarbeitungsdauer|Millisekunden|Average|Zielverarbeitungsdauer in Millisekunden|EventSubscriptionName|
|DroppedEventCount|Ja|Gelöschte Ereignisse|Anzahl|Gesamt|Gesamtanzahl der gelöschten Ereignisse, die mit diesem Ereignisabonnement übereinstimmen|DropReason, EventSubscriptionName|
|MatchedEventCount|Ja|Übereinstimmende Ereignisse|Anzahl|Gesamt|Gesamtzahl der Ereignisse, die diesem Ereignisabonnement zugeordnet sind|EventSubscriptionName|
|PublishFailCount|Ja|Ereignisse mit Veröffentlichungsfehler|Anzahl|Gesamt|Gesamtanzahl der zu diesem Thema fehlerhaft veröffentlichten Ereignisse|ErrorType, Error|
|PublishSuccessCount|Ja|Veröffentlichte Ereignisse|Anzahl|Gesamt|Gesamtanzahl der zu diesem Thema veröffentlichten Ereignisse|Keine Dimensionen|
|PublishSuccessLatencyInMs|Ja|Latenz erfolgreicher Veröffentlichungen|Millisekunden|Gesamt|Latenz erfolgreicher Veröffentlichungen in Millisekunden|Keine Dimensionen|
|UnmatchedEventCount|Ja|Ereignisse ohne Übereinstimmung|Anzahl|Gesamt|Gesamtzahl der Ereignisse, die mit keinem der Ereignisabonnements für dieses Thema übereinstimmen|Keine Dimensionen|


## <a name="partner-topics"></a>Partnerthemen

|Metrik|Über Diagnoseeinstellungen exportierbar?|Metrikanzeigename|Einheit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|---|
|AdvancedFilterEvaluationCount|Ja|Erweiterte Filterauswertungen|Anzahl|Gesamt|Gesamtanzahl erweiterter Filter, die für dieses Thema in Ereignisabonnements ausgewertet werden.|EventSubscriptionName|
|DeadLetteredCount|Ja|Unzustellbare Ereignisse|Anzahl|Gesamt|Gesamtanzahl der unzustellbaren Ereignisse, die mit diesem Ereignisabonnement übereinstimmen|DeadLetterReason, EventSubscriptionName|
|DeliveryAttemptFailCount|Nein|Ereignisse mit Übermittlungsfehler|Anzahl|Gesamt|Gesamtzahl der Ereignisse, die nicht an dieses Ereignisabonnement übermittelt werden konnten|Error, ErrorType, EventSubscriptionName|
|DeliverySuccessCount|Ja|Übermittelte Ereignisse|Anzahl|Gesamt|Gesamtzahl der Ereignisse, die an dieses Ereignisabonnement übermittelt wurden|EventSubscriptionName|
|DestinationProcessingDurationInMs|Nein|Zielverarbeitungsdauer|Millisekunden|Average|Zielverarbeitungsdauer in Millisekunden|EventSubscriptionName|
|DroppedEventCount|Ja|Gelöschte Ereignisse|Anzahl|Gesamt|Gesamtanzahl der gelöschten Ereignisse, die mit diesem Ereignisabonnement übereinstimmen|DropReason, EventSubscriptionName|
|MatchedEventCount|Ja|Übereinstimmende Ereignisse|Anzahl|Gesamt|Gesamtzahl der Ereignisse, die diesem Ereignisabonnement zugeordnet sind|EventSubscriptionName|
|PublishFailCount|Ja|Ereignisse mit Veröffentlichungsfehler|Anzahl|Gesamt|Gesamtanzahl der zu diesem Thema fehlerhaft veröffentlichten Ereignisse|ErrorType, Error|
|PublishSuccessCount|Ja|Veröffentlichte Ereignisse|Anzahl|Gesamt|Gesamtanzahl der zu diesem Thema veröffentlichten Ereignisse|Keine Dimensionen|
|UnmatchedEventCount|Ja|Ereignisse ohne Übereinstimmung|Anzahl|Gesamt|Gesamtzahl der Ereignisse, die mit keinem der Ereignisabonnements für dieses Thema übereinstimmen|Keine Dimensionen|


## <a name="next-steps"></a>Nächste Schritte
Lesen Sie den folgenden Artikel: [Diagnoseprotokolle](diagnostic-logs.md)
