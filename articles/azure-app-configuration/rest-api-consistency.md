---
title: 'Azure App Configuration-REST-API: Konsistenz'
description: Referenzseiten für die Sicherstellung von Echtzeitkonsistenz über die Azure App Configuration-REST-API
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: reference
ms.date: 08/17/2020
ms.openlocfilehash: db9553c2c9c79a6beb9c66d0cb1a1a60435b2abd
ms.sourcegitcommit: 30906a33111621bc7b9b245a9a2ab2e33310f33f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/22/2020
ms.locfileid: "95253336"
---
# <a name="real-time-consistency"></a>Echtzeitkonsistenz

Aufgrund der Natur einiger verteilter Systeme lässt sich Echtzeitkonsistenz zwischen Anforderungen nur schwer implizit erzwingen. Eine Lösung für dieses Problem besteht darin, Protokollunterstützung in Form von mehreren Synchronisierungstoken zuzulassen. Synchronisierungstoken sind optional.

## <a name="initial-request"></a>Erste Anforderung

Um Echtzeitkonsistenz zwischen verschiedenen Clientinstanzen und Anforderungen zu gewährleisten, verwenden Sie optionale `Sync-Token`-Anforderungs- und Antwortheader.

Syntax:

```http
Sync-Token: <id>=<value>;sn=<sn>
```

|Parameter|BESCHREIBUNG|
|--|--|
| `<id>` | Token-ID (nicht transparent) |
| `<value>` | Der Tokenwert (nicht transparent). Erlaubt Base64-codierte Zeichenfolgen. |
| `<sn>` | Die Tokensequenznummer (Version). Ein höherer Wert bedeutet eine neuere Version desselben Tokens. Ermöglicht bessere Parallelität und Clientzwischenspeicherung. Der Client kann nur die letzte Version des Tokens verwenden, da Tokenversionen inklusiv sind. Dieser Parameter ist für Anforderungen nicht erforderlich. |

## <a name="response"></a>Antwort

Der Dienst stellt mit jeder Antwort einen `Sync-Token`-Header bereit.

```http
Sync-Token: jtqGc1I4=MDoyOA==;sn=28
```

## <a name="subsequent-requests"></a>Nachfolgende Anforderungen

Für jede nachfolgende Anforderung ist eine in Echtzeit konsistente Antwort in Bezug auf das bereitgestellte `Sync-Token` garantiert.

```http
Sync-Token: <id>=<value>
```

Wenn Sie den `Sync-Token`-Header in der Anforderung auslassen, ist es möglich, dass der Dienst während eines kurzen Zeitraums (maximal einige Sekunden) mit zwischengespeicherten Daten antwortet, bevor die interne Festlegung erfolgt. Dieses Verhalten kann zu inkonsistenten Lesevorgängen führen, wenn direkt vor dem Lesevorgang Änderungen aufgetreten sind.

## <a name="multiple-sync-tokens"></a>Mehrere Synchronisierungstoken

Der Server könnte bei einer einzelnen Anforderung mit mehreren Synchronisierungstoken antworten. Um Echtzeitkonsistenz für die nächste Anforderung beizubehalten, muss der Client mit allen empfangenen Synchronisierungstoken antworten. Mehrere Headerwerte müssen durch Kommas getrennt sein.

```http
Sync-Token: <token1-id>=<value>,<token2-id>=<value>
```
