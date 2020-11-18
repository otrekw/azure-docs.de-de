---
title: 'Azure App Configuration-REST-API: Konsistenz'
description: Referenzseiten für die Sicherstellung von Echtzeitkonsistenz über die Azure App Configuration-REST-API
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: reference
ms.date: 08/17/2020
ms.openlocfilehash: 4f11e6edcd4bc128f815db7e93b00b72bf990ea8
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2020
ms.locfileid: "93423763"
---
# <a name="real-time-consistency"></a>Echtzeitkonsistenz

Aufgrund der Natur einiger verteilter Systeme lässt sich Echtzeitkonsistenz zwischen Anforderungen nur schwer implizit erzwingen. Eine Lösung für dieses Problem besteht darin, Protokollunterstützung in Form von mehreren **Synchronisierungstoken** zuzulassen. Synchronisierungstoken sind optional.

## <a name="initial-request"></a>Erste Anforderung

Verwenden Sie optionale `Sync-Token`-Anforderungs- bzw. Antwortheader, um Echtzeitkonsistenz zwischen verschiedenen Clientinstanzen und Anforderungen zu gewährleisten.

Syntax:

```http
Sync-Token: <id>=<value>;sn=<sn>
```

|Parameter|BESCHREIBUNG|
|--|--|
| `<id>` | Token-ID (nicht transparent) |
| `<value>` | Der Tokenwert (nicht transparent). Erlaubt Base64-codierte Zeichenfolgen. |
| `<sn>` | Die Tokensequenznummer (Version). Ein höherer Wert bedeutet eine neuere Version desselben Tokens. Ermöglicht bessere Parallelität und Clientzwischenspeicherung. Der Client kann nur die letzte Version des Tokens verwenden, da Tokenversionen inklusiv sind. Ist für Anforderungen nicht erforderlich. |

## <a name="response"></a>Antwort

Der Dienst stellt mit jeder Antwort einen `Sync-Token`-Header bereit.

```http
Sync-Token: jtqGc1I4=MDoyOA==;sn=28
```

## <a name="subsequent-requests"></a>Nachfolgende Anforderungen

Für jede nachfolgende Anforderung ist eine in **Echtzeit** konsistente Antwort in Bezug auf das bereitgestellte `Sync-Token` garantiert.

```http
Sync-Token: <id>=<value>
```

Wenn der `Sync-Token`-Header in der Anforderung weggelassen wird, ist es möglich, dass der Dienst während eines kurzen Zeitraums (maximal einige Sekunden) mit zwischengespeicherten Daten antwortet, bevor die interne Festlegung erfolgt. Dieses Verhalten kann zu inkonsistenten Lesevorgängen führen, wenn direkt vor dem Lesevorgang Änderungen aufgetreten sind.

## <a name="multiple-sync-tokens"></a>Mehrere Synchronisierungstoken

Der Server KANN bei einer einzelnen Anforderung mit mehreren Synchronisierungstoken antworten. Um **Echtzeitkonsistenz** für die nächste Anforderung beizubehalten, MUSS der Client mit allen empfangenen Synchronisierungstoken antworten. Gemäß RFC müssen mehrere Headerwerte durch Kommas getrennt sein.

```http
Sync-Token: <token1-id>=<value>,<token2-id>=<value>
```
