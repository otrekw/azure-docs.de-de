---
title: Behandeln von Problemen mit der Azure Front Door-Konfiguration
description: In diesem Tutorial erfahren Sie, wie Sie selbst einige häufige Probleme mit Front Door behandeln können.
services: frontdoor
documentationcenter: ''
author: duongau
editor: ''
ms.service: frontdoor
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 09/30/2020
ms.author: duau
ms.openlocfilehash: dbce9019e33c07dd4faa91ffd490eba4d313c675
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91630609"
---
# <a name="troubleshooting-common-routing-issues"></a>Behandeln von häufigen Routingproblemen

In diesem Artikel wird beschrieben, wie Sie einige häufige Routingprobleme in Ihrer Azure Front Door-Konfiguration beheben können.

## <a name="503-response-from-front-door-after-a-few-seconds"></a>Antwort des Typs 503 von Front Door nach einigen Sekunden

### <a name="symptom"></a>Symptom

* Gewöhnliche Anforderungen, die an Ihr Back-End gesendet werden, ohne Front Door zu passieren, sind erfolgreich, aber das Passieren von Front Door führt zu Fehlerantworten des Typs 503.
* Die Fehlfunktion von Front Door zeigt sich nach einigen (normalerweise nach ca. 30) Sekunden.

### <a name="cause"></a>Ursache

Für dieses Problem gibt es zwei mögliche Ursachen:
 
* Ihr Back-End benötigt länger als das konfigurierte Timeout (Standardwert: 30 Sekunden), um die Anforderung der Frontdoor zu erhalten.
* Die Zeit, die benötigt wird, um eine Antwort an die Anforderung von Front Door zu senden, ist länger als der Wert des Timeouts. 

### <a name="troubleshooting-steps"></a>Schritte zur Problembehandlung

* Senden Sie die Anforderung direkt an Ihr Back-End (ohne Front Door zu passieren), und prüfen Sie, wie lange Ihr Back-End normalerweise für die Antwort benötigt.
* Senden Sie die Anforderung über Front Door, und prüfen Sie, ob Sie Antworten des Typs 503 erhalten. Falls nicht, ist das Timeout möglicherweise nicht das Problem. Wenden Sie sich an den Support.
* Wenn bei der Verwendung der Frontdoor Antwortcode mit Fehlern des Typs 503 zurückgegeben wird, konfigurieren Sie das `sendReceiveTimeout`-Feld für Ihre Frontdoor. Sie können das Standardtimeout auf bis zu vier Minuten (240 Sekunden) verlängern. Die Einstellung befindet sich unter `backendPoolSettings` und heißt `sendRecvTimeoutSeconds`. 

## <a name="requests-sent-to-the-custom-domain-returns-400-status-code"></a>Für an die benutzerdefinierte Domäne gesendete Anforderungen wird der Statuscode 400 zurückgegeben

### <a name="symptom"></a>Symptom

* Sie haben eine Frontdoor erstellt, aber für eine Anforderung an die Domäne oder den Front-End-Host wird der HTTP-Statuscode 400 zurückgegeben.
* Sie haben eine DNS-Zuordnung für eine benutzerdefinierte Domäne zum konfigurierten Front-End-Host erstellt. Beim Senden einer Anforderung an den Hostnamen der benutzerdefinierten Domäne wird jedoch ein HTTP-Statuscode 400 zurückgegeben. Außerdem scheint das Routing zum von Ihnen konfigurierten Back-End nicht zu erfolgen.

### <a name="cause"></a>Ursache

Das Problem tritt auf, wenn Sie keine Routingregel für die benutzerdefinierte Domäne konfiguriert haben, die als Front-End-Host hinzugefügt wurde. Für diesen Front-End-Host muss explizit eine Routingregel hinzugefügt werden. Selbst wenn bereits eine Regel für den Front-End-Host unter der Front Door-Unterdomäne (*.azurefd.net) konfiguriert wurde, muss dies erfolgen.

### <a name="troubleshooting-steps"></a>Schritte zur Problembehandlung

Fügen Sie eine Routingregel für die benutzerdefinierte Domäne hinzu, um Datenverkehr an den ausgewählten Back-End-Pool weiterzuleiten.

## <a name="front-door-doesnt-redirect-http-to-https"></a>Front Door leitet HTTP oder HTTPS nicht weiter

### <a name="symptom"></a>Symptom

Ihre Front Door-Instanz weist eine Routingregel auf, die besagt, dass HTTP zu HTTPS umgeleitet wird, doch beim Zugriff auf die Domäne wird HTTP als Protokoll beibehalten.

### <a name="cause"></a>Ursache

Dieses Verhalten kann auftreten, wenn Sie die Routingregeln für Ihre Frontdoor nicht ordnungsgemäß konfiguriert haben. Im Grunde genommen ist Ihre aktuelle Konfiguration nicht spezifisch und kann in Konflikt stehende Regeln aufweisen.

### <a name="troubleshooting-steps"></a>Schritte zur Problembehandlung

## <a name="request-to-frontend-hostname-returns-404-status-code"></a>Eine Anforderung an den Front-End-Hostnamen gibt den Statuscode 404 zurück

### <a name="symptom"></a>Symptom

 Sie haben eine Frontdoor erstellt, indem Sie einen Front-End-Host, einen Back-End-Pool mit mindestens einem Back-End und eine Routingregel konfiguriert haben, die den Front-End-Host mit dem Back-End-Pool verbindet. Ihre Inhalte sind nicht verfügbar, wenn Sie eine Anforderung an den konfigurierten Front-End-Host senden, da der HTTP-Statuscode 404 zurückgegeben wird.

### <a name="cause"></a>Ursache

Dieses Symptom kann verschiedene Ursachen haben:

* Das Back-End ist nicht öffentlich zugänglich und für die Frontdoor nicht sichtbar.
* Das Back-End ist falsch konfiguriert, was dazu führt, dass die Frontdoor falsche Anforderungen sendet. Anders formuliert akzeptiert Ihr Back-End nur HTTP, und Sie haben nicht deaktiviert, dass HTTPS zulässig ist. Die Frontdoor versucht also, HTTPS-Anforderungen weiterzuleiten.
* Das Back-End lehnt den Hostheader ab, der mit der Anforderung an das Back-End weitergeleitet wurde.
* Die Konfiguration für das Back-End wurde noch nicht vollständig bereitgestellt.

### <a name="troubleshooting-steps"></a>Schritte zur Problembehandlung

1. Bereitstellungszeit
   * Warten Sie etwa zehn Minuten, bis die Konfiguration bereitgestellt wurde.

2. Überprüfen der Back-End-Einstellungen
    * Navigieren Sie zum Back-End-Pool, an den die Anforderung weitergeleitet werden sollte. Dies hängt davon ab, wie Sie die Routingregel konfiguriert haben. Überprüfen Sie, ob der *Back-End-Hosttyp* und der Back-End-Hostname richtig sind. Wenn das Back-End ein benutzerdefinierter Host ist, stellen Sie sicher, dass er richtig geschrieben wurde. 

    * Überprüfen Sie die HTTP- und HTTPS-Ports. In den meisten Fällen sind 80 bzw. 443 korrekt, und es sollten keine Änderungen erforderlich sein. Allerdings besteht die Möglichkeit, dass Ihr Back-End anders konfiguriert ist und an einem anderen Port lauscht.

        * Überprüfen Sie den _Back-End-Hostheader_, der für die Back-Ends konfiguriert wurde, an die der Front-End-Host weiterleiten soll. In den meisten Fällen sollte dieser Header mit dem *Back-End-Hostnamen* identisch sein. Ein falscher Wert kann jedoch zu verschiedenen 4xx-HTTP-Statuscodes führen, wenn das Back-End etwas anderes erwartet. Wenn Sie die IP-Adresse Ihres Back-Ends eingeben, müssen Sie möglicherweise den *Back-End-Hostheader* auf den Back-End-Hostnamen festlegen.

3. Überprüfen der Einstellungen der Routingregel:
    * Navigieren Sie zur Routingregel, die vom fraglichen Front-End-Hostnamen an einen Back-End-Pool weiterleiten soll. Überprüfen Sie, ob die akzeptierten Protokolle ordnungsgemäß konfiguriert sind, wenn die Anforderung weitergeleitet wird. Das Feld *accepted protocols* (Zulässige Protokolle) bestimmt, welche Anforderungen von der Frontdoor akzeptiert werden sollten. Das *Weiterleitungsprotokoll* bestimmt, welches Protokoll Front Door verwenden soll, um die Anforderung an das Back-End weiterzuleiten.
         * Wenn das Back-End beispielsweise nur HTTP-Anforderungen akzeptiert, wären die folgenden Konfigurationen gültig:
            * *Zulässige Protokolle* sind HTTP und HTTPS. *Weiterleitungsprotokoll* ist HTTP. Die Übereinstimmungsanforderung funktioniert nicht, da HTTPS ein zulässiges Protokoll ist, und wenn eine Anforderung als HTTPS gesendet wird, würde Front Door versuchen, sie über HTTPS weiterzuleiten.

            * *Zulässiges Protokoll* ist HTTP. *Weiterleitungsprotokoll* ist entweder Übereinstimmungsanforderung oder HTTP.

    - *URL-Rewrite* ist standardmäßig deaktiviert. Verwenden Sie dieses Feld nur, wenn Sie den Umfang der auf dem Back-End gehosteten Ressourcen, die verfügbar sein sollen, einschränken möchten. Wenn das Feld deaktiviert ist, leitet Front Door den gleichen Anforderungspfad weiter, der eingegangen ist. Es ist möglich, dieses Feld fehlerhaft zu konfigurieren. Wenn Front Door also eine Ressource aus dem Back-End anfordert, die nicht verfügbar ist, wird ein HTTP-Statuscode 404 zurückgegeben.
