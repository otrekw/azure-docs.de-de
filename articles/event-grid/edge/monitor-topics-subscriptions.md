---
title: Überwachen von Themen und Ereignisabonnements – Azure Event Grid IoT Edge | Microsoft-Dokumentation
description: Überwachen von Themen und Ereignisabonnements
ms.date: 07/08/2020
ms.topic: article
ms.openlocfilehash: d230be4f74abd61ad7b7f0fdb3facb32ee63da73
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "86171532"
---
# <a name="monitor-topics-and-event-subscriptions"></a>Überwachen von Themen und Ereignisabonnements

Event Grid in Edge macht eine Reihe von Metriken für Themen und Ereignisabonnements im [Prometheus-Offenlegungsformat](https://prometheus.io/docs/instrumenting/exposition_formats/) verfügbar. In diesem Artikel werden die verfügbaren Metriken und deren Aktivierung beschrieben.

## <a name="enable-metrics"></a>Aktivieren von Metriken

Konfigurieren Sie das Modul so, dass es Metriken ausgibt, indem Sie die Umgebungsvariable `metrics__reporterType` in den Optionen für die Containererstellung auf `prometheus` festlegen:

 ```json
        {
          "Env": [
            "metrics__reporterType=prometheus"
          ],
          "HostConfig": {
            "PortBindings": {
              "4438/tcp": [
                {
                    "HostPort": "4438"
                }
              ]
            }
          }
        }
 ```    

Die Metriken sind dann unter `5888/metrics` im Modul für HTTP und unter `4438/metrics` für HTTPS verfügbar. Beispiel: `http://<modulename>:5888/metrics?api-version=2019-01-01-preview` für HTTP. An diesem Punkt kann ein Metrikmodul den Endpunkt abrufen, um Metriken zu sammeln, wie in dieser [Beispielarchitektur](https://github.com/veyalla/ehm).

## <a name="available-metrics"></a>Verfügbare Metriken

Sowohl Themen als auch Ereignisabonnements geben Metriken aus, die Ihnen Erkenntnisse zur Leistung der Ereignisübermittlung und von Modulen bieten.

### <a name="topic-metrics"></a>Metriken für Themen

| Metrik | BESCHREIBUNG |
| ------ | ----------- |
| EventsReceived | Anzahl der zu diesem Thema veröffentlichten Ereignisse
| UnmatchedEvents | Anzahl der für das Thema veröffentlichten Ereignisse, die keinem Ereignisabonnement entsprechen und verworfen werden
| SuccessRequests | Anzahl der eingehenden Veröffentlichungsanforderungen, die vom Thema empfangen wurden
| SystemErrorRequests | Anzahl der eingehenden Veröffentlichungsanforderungen, bei denen ein interner Systemfehler aufgetreten ist
| UserErrorRequests | Anzahl eingehender Veröffentlichungsanforderungen, bei denen ein Benutzerfehler aufgetreten ist, z. B. falsch formatierter JSON-Code
| SuccessRequestLatencyMs | Latenz der Antworten auf Veröffentlichungsanforderungen in Millisekunden


### <a name="event-subscription-metrics"></a>Metriken für Ereignisabonnements

| Metrik | BESCHREIBUNG |
| ------ | ----------- |
| DeliverySuccessCounts | Anzahl der erfolgreich an den konfigurierten Endpunkt übermittelten Ereignisse
| DeliveryFailureCounts | Anzahl von Ereignissen, die nicht an den konfigurierten Endpunkt übermittelt werden konnten
| DeliverySuccessLatencyMs | Latenz der erfolgreich übermittelten Ereignisse in Millisekunden
| DeliveryFailureLatencyMs | Latenz der fehlerhaften Ereignisübermittlungen in Millisekunden
| SystemDelayForFirstAttemptMs | Systemverzögerung vor dem ersten Zustellungsversuch von Ereignissen in Millisekunden
| DeliveryAttemptsCount | Anzahl der Versuche zur Ereignisübermittlung – Erfolg und Fehler
| ExpiredCounts | Anzahl von Ereignissen, die abgelaufen sind und nicht an den konfigurierten Endpunkt übermittelt werden konnten