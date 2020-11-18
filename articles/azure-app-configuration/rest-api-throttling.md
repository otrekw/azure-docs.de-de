---
title: 'Azure App Configuration-REST-API: Drosselung'
description: Referenzseiten für die Drosselung über die Azure App Configuration-REST-API
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: reference
ms.date: 08/17/2020
ms.openlocfilehash: 26464138b7739bb476d9d0dd1078194473ecd23e
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2020
ms.locfileid: "93423747"
---
# <a name="throttling"></a>Einschränkung

Für Konfigurationsspeicher gelten Einschränkungen in Bezug auf die Anforderungen, die sie erfüllen können. Für alle Anforderungen, die ein zugewiesenes Kontingent für einen Konfigurationsspeicher überschreiten, wird die HTTP-Antwort 429 (zu viele Anforderungen) zurückgegeben.

Die Drosselung ist in verschiedene Kontingentrichtlinien unterteilt:

- **Anforderungen gesamt**: Gesamtanzahl von Anforderungen
- **Bandbreite gesamt**: ausgehende Daten in Bytes
- **Speicher**: Gesamtspeichergröße der Benutzerdaten in Bytes

## <a name="handling-throttled-responses"></a>Verarbeitung von Antworten zur Drosselung

Wenn das Ratenlimit für ein bestimmtes Kontingent erreicht ist, antwortet der Server auf weitere Anforderungen dieses Typs mit dem Statuscode _429_. Die Antwort _429_ enthält einen _rety-after-ms_-Header, der eine empfohlene Wartezeit für den Client (in Millisekunden) angibt, damit das Anforderungskontingent wieder aufgefüllt werden kann.

```http
HTTP/1.1 429 (Too Many Requests)
retry-after-ms: 10
Content-Type: application/problem+json; charset=utf-8
```

```json
{
  "type": "https://azconfig.io/errors/too-many-requests",
  "title": "Resource utilization has surpassed the assigned quota",
  "policy": "Total Requests",
  "status": 429
}
```

Im oben stehenden Beispiel hat der Client das zulässige Kontingent überschritten, und dem Client wird empfohlen, vor dem Senden weiterer Anforderungen 10 Millisekunden zu warten. Client sollten auch ein progressives Backoff berücksichtigen.

## <a name="other-retry"></a>Andere Wiederholungsversuche

Möglicherweise identifiziert der Dienst andere Situationen als die Drosselung, in denen Wiederholungsversuche durch den client erforderlich sind (Beispiel: 503 – Dienst nicht verfügbar). In all diesen Fällen wird der Antwortheader `retry-after-ms` bereitgestellt. Um die Stabilität zu erhöhen, wird dem Client empfohlen, das vorgeschlagene Intervall einzuhalten und einen Wiederholungsversuch auszuführen.

```http
HTTP/1.1 503 Service Unavailable
retry-after-ms: 787
```
