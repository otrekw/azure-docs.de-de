---
title: Überwachen von Themen und Ereignisabonnements – Azure Event Grid IoT Edge | Microsoft-Dokumentation
description: Überwachen von Themen und Ereignisabonnements
author: banisadr
ms.author: babanisa
ms.reviewer: spelluru
ms.date: 01/09/2020
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: 79b223de7a0a0cfdaf799b1f80e585a2a55f7e82
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2020
ms.locfileid: "76851321"
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

Die Metriken sind dann unter `5888/metrics` im Modul für HTTP und unter `4438/metrics` für HTTPS verfügbar. Beispiel: `http://<modulename>:4438/metrics?api-version=2019-01-01-preview` für HTTP. An diesem Punkt kann ein Metrikmodul den Endpunkt abrufen, um Metriken zu sammeln, wie in dieser [Beispielarchitektur](https://github.com/veyalla/ehm).

## <a name="available-metrics"></a>Verfügbare Metriken

Sowohl Themen als auch Ereignisabonnements geben Metriken aus, die Ihnen Erkenntnisse zur Leistung der Ereignisübermittlung und von Modulen bieten.

### <a name="topic-metrics"></a>Metriken für Themen

| Metrik | Beschreibung |
| ------ | ----------- |
| EventsReceived | Anzahl der zu diesem Thema veröffentlichten Ereignisse
| UnmatchedEvents | Anzahl der für das Thema veröffentlichten Ereignisse, die keinem Ereignisabonnement entsprechen und verworfen werden
| SuccessRequests | Anzahl der eingehenden Veröffentlichungsanforderungen, die vom Thema empfangen wurden
| SystemErrorRequests | Anzahl der eingehenden Veröffentlichungsanforderungen, bei denen ein interner Systemfehler aufgetreten ist
| UserErrorRequests | Anzahl eingehender Veröffentlichungsanforderungen, bei denen ein Benutzerfehler aufgetreten ist, z. B. falsch formatierter JSON-Code
| SuccessRequestLatencyMs | Latenz der Antworten auf Veröffentlichungsanforderungen in Millisekunden


### <a name="event-subscription-metrics"></a>Metriken für Ereignisabonnements

| Metrik | Beschreibung |
| ------ | ----------- |
| deliverySuccessCounts | Anzahl der erfolgreich an den konfigurierten Endpunkt übermittelten Ereignisse
| deliveryFailureCounts | Anzahl der Fehler bei der Ereignisübermittlung an den konfigurierten Endpunkt
| deliverySuccessLatencyMs | Latenz der erfolgreich übermittelten Ereignisse in Millisekunden
| deliveryFailureLatencyMs | Latenz der fehlerhaften Ereignisübermittlungen in Millisekunden
| systemDelayForFirstAttemptMs | Systemverzögerung vor dem ersten Zustellungsversuch von Ereignissen in Millisekunden
| deliveryAttemptsCount | Anzahl der Versuche zur Ereignisübermittlung – Erfolg und Fehler
| expiredCounts | Anzahl von Ereignissen, die nicht zugestellt werden konnten 