---
title: Behandeln von Problemen mit der Azure Front Door-Konfiguration
description: In diesem Tutorial erfahren Sie, wie Sie selbst einige häufige Probleme mit Front Door behandeln können.
services: frontdoor
documentationcenter: ''
author: sharad4u
editor: ''
ms.service: frontdoor
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 09/22/2018
ms.author: sharadag
ms.openlocfilehash: 40809fae312401cb62fabb10140b9bb7f60e3715
ms.sourcegitcommit: 3bf69c5a5be48c2c7a979373895b4fae3f746757
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/14/2020
ms.locfileid: "88234770"
---
# <a name="troubleshooting-common-routing-issues"></a>Behandeln von häufigen Routingproblemen

In diesem Artikel wird beschrieben, wie Sie einige häufige Routingprobleme in Ihrer Azure Front Door-Konfiguration beheben können.

## <a name="503-response-from-front-door-after-a-few-seconds"></a>Antwort des Typs 503 von Front Door nach einigen Sekunden

### <a name="symptom"></a>Symptom

- Gewöhnliche Anforderungen, die an Ihr Back-End gesendet werden, ohne Front Door zu passieren, sind erfolgreich, aber das Passieren von Front Door führt zu Fehlerantworten des Typs 503.

- Die Fehlfunktion von Front Door zeigt sich nach einigen (normalerweise nach ca. 30) Sekunden.

### <a name="cause"></a>Ursache

Dieses Symptom tritt auf, wenn entweder Ihr Back-End die Konfiguration des Zeitlimits (Standard: 30 Sekunden) überschreitet, um die Anforderung von Front Door zu empfangen, oder wenn es über dieses Zeitlimit hinaus dauert, eine Antwort auf die Anforderung von Front Door zu senden. 

### <a name="troubleshooting-steps"></a>Schritte zur Problembehandlung

- Senden Sie die Anforderung direkt an Ihr Back-End (ohne Front Door zu passieren), und prüfen Sie, wie lange Ihr Back-End normalerweise für die Antwort benötigt.
- Senden Sie die Anforderung über Front Door, und prüfen Sie, ob Sie Antworten des Typs 503 erhalten. Falls nicht, ist das Zeitlimit möglicherweise nicht das Problem. Wenden Sie sich an den Support.
- Wenn das Passieren von Front Door zu einem Fehlerantwortcode des Typs 503 führt, konfigurieren Sie das Feld sendReceiveTimeout für Ihre Front Door-Instanz so, dass das Standardzeitlimit auf bis zu 4 Minuten (240 Sekunden) verlängert wird. Die Einstellung befindet sich unter `backendPoolSettings` und heißt `sendRecvTimeoutSeconds`. 

## <a name="requests-sent-to-the-custom-domain-returns-400-status-code"></a>Für an die benutzerdefinierte Domäne gesendete Anforderungen wird der Statuscode 400 zurückgegeben

### <a name="symptom"></a>Symptom

- Sie haben eine Front Door-Instanz erstellt, aber für eine Anforderung an die Domäne oder den Front-End-Host wird der HTTP-Statuscode 400 zurückgegeben.

- Sie haben eine DNS-Zuordnung von einer benutzerdefinierten Domäne zum konfigurierten Front-End-Host erstellt. Beim Senden einer Anforderung an den Hostnamen der benutzerdefinierten Domäne wird jedoch der HTTP-Statuscode 400 zurückgegeben, und das Routing an die konfigurierten Back-Ends scheint nicht zu funktionieren.

### <a name="cause"></a>Ursache

Dieses Symptom ist möglich, wenn Sie keine Routingregel für die benutzerdefinierte Domäne konfiguriert haben, die Sie als Front-End-Host hinzugefügt haben. Eine Routingregel muss explizit für den Front-End-Host hinzugefügt werden, auch wenn bereits eine für den Front-End-Host in der Front Door-Unterdomäne (*.azurefd.net) konfiguriert wurde, für die eine DNS-Zuordnung für Ihre benutzerdefinierte Domäne besteht.

### <a name="troubleshooting-steps"></a>Schritte zur Problembehandlung

Fügen Sie eine Routingregel von der benutzerdefinierten Domäne zum gewünschten Back-End-Pool hinzu.

## <a name="front-door-is-not-redirecting-http-to-https"></a>Front Door leitet HTTP nicht an HTTPS um

### <a name="symptom"></a>Symptom

Ihre Front Door-Instanz weist eine Routingregel auf, die besagt, dass HTTP zu HTTPS umgeleitet wird, doch beim Zugriff auf die Domäne wird HTTP als Protokoll beibehalten.

### <a name="cause"></a>Ursache

Dieses Verhalten kann eintreten, wenn Sie die Routingregeln für Ihre Front Door-Instanz nicht ordnungsgemäß konfiguriert haben. Im Grunde genommen ist Ihre aktuelle Konfiguration nicht spezifisch und kann in Konflikt stehende Regeln aufweisen.

### <a name="troubleshooting-steps"></a>Schritte zur Problembehandlung

## <a name="request-to-frontend-hostname-returns-404-status-code"></a>Eine Anforderung an den Front-End-Hostnamen gibt den Statuscode 404 zurück

### <a name="symptom"></a>Symptom

- Sie haben eine Front Door-Instanz erstellt und einen Front-End-Host, einen Back-End-Pool mit mindestens einem Back-End und eine Routingregel, die den Front-End-Host mit dem Back-End-Pool verbindet, konfiguriert. Ihre Inhalte sind offenbar nicht verfügbar, wenn Sie eine Anforderung an den konfigurierten Front-End-Host senden, da der HTTP-Statuscode 404 zurückgegeben wird.

### <a name="cause"></a>Ursache

Dieses Symptom kann verschiedene Ursachen haben:

- Das Back-End ist nicht öffentlich zugänglich und für die Front Door-Instanz nicht sichtbar.
- Das Back-End ist falsch konfiguriert. Daher sendet die Front Door-Instanz die falsche Anforderung (das heißt, Ihr Back-End akzeptiert nur HTTP, aber Sie haben das Zulassen von HTTPS nicht deaktiviert, sodass Front Door versucht, HTTPS-Anforderungen weiterzuleiten).
- Das Back-End lehnt den Hostheader ab, der mit der Anforderung an das Back-End weitergeleitet wurde.
- Die Konfiguration für das Back-End wurde noch nicht vollständig bereitgestellt.

### <a name="troubleshooting-steps"></a>Schritte zur Problembehandlung

1. Bereitstellungszeit
   - Stellen Sie sicher, dass Sie etwa 10 Minuten warten, bis die Konfiguration bereitgestellt wurde.

2. Überprüfen der Back-End-Einstellungen
    - Navigieren Sie zum Back-End-Pool, an den die Anforderung weitergeleitet werden sollte (hängt davon ab, wie Sie die Routingregel konfiguriert haben), und überprüfen Sie, ob der _Back-End-Hosttyp_ und der Back-End-Hostname richtig sind. Wenn das Back-End ein benutzerdefinierter Host ist, stellen Sie sicher, dass er richtig geschrieben wurde. 

    - Überprüfen Sie die HTTP- und HTTPS-Ports. In den meisten Fällen sind 80 bzw. 443 korrekt, und es sollten keine Änderungen erforderlich sein. Allerdings besteht die Möglichkeit, dass Ihr Back-End anders konfiguriert ist und an einem anderen Port lauscht.

        - Überprüfen Sie den _Back-End-Hostheader_, der für die Back-Ends konfiguriert wurde, an die der Front-End-Host weiterleiten soll. In den meisten Fällen sollte dieser Header mit dem _Back-End-Hostnamen_ identisch sein. Ein falscher Wert kann jedoch zu verschiedenen 4xx-HTTP-Statuscodes führen, wenn das Back-End etwas anderes erwartet. Wenn Sie die IP-Adresse Ihres Back-Ends eingeben, müssen Sie möglicherweise den _Back-End-Hostheader_ auf den Back-End-Hostnamen festlegen.


3. Überprüfen der Einstellungen der Routingregel
    - Navigieren Sie zur Routingregel, die vom fraglichen Front-End-Hostnamen an einen Back-End-Pool weiterleiten soll. Stellen Sie sicher, dass die zulässigen Protokolle ordnungsgemäß konfiguriert sind. Wenn dies nicht der Fall ist, stellen Sie sicher, dass das von Front Door beim Weiterleiten der Anforderung verwendete Protokoll ordnungsgemäß konfiguriert ist. Das Feld _Zulässige Protokolle_ bestimmt, welche Anforderungen Front Door akzeptieren soll. und Das _Weiterleitungsprotokoll_ bestimmt, welches Protokoll Front Door für die Weiterleitung der Anforderung an das Back-End verwenden soll.
         - Wenn das Back-End beispielsweise nur HTTP-Anforderungen akzeptiert, wären die folgenden Konfigurationen gültig:
            - _Zulässige Protokolle_ sind HTTP und HTTPS. _Weiterleitungsprotokoll_ ist HTTP. Die Übereinstimmungsanforderung funktioniert nicht, da HTTPS ein zulässiges Protokoll ist, und wenn eine Anforderung als HTTPS gesendet wird, würde Front Door versuchen, sie über HTTPS weiterzuleiten.

            - _Zulässiges Protokoll_ ist HTTP. _Weiterleitungsprotokoll_ ist entweder Übereinstimmungsanforderung oder HTTP.

    - _URL-Rewrite_ ist standardmäßig deaktiviert. Verwenden Sie dieses Feld nur, wenn Sie den Umfang der auf dem Back-End gehosteten Ressourcen, die verfügbar sein sollen, einschränken möchten. Wenn das Feld deaktiviert ist, leitet Front Door den gleichen Anforderungspfad weiter, der eingegangen ist. Es ist möglich, dass dieses Feld falsch konfiguriert ist und Front Door eine Ressource vom Back-End anfordert, die nicht verfügbar ist. Daher wird der HTTP-Statuscode 404 zurückgegeben.

