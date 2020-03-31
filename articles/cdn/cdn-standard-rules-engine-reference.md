---
title: Referenz zur Standardregel-Engine für Azure CDN | Microsoft-Dokumentation
description: Referenzdokumentation für Übereinstimmungsbedingungen und Aktionen in der Standardregel-Engine für Azure Content Delivery Network (Azure CDN).
services: cdn
author: mdgattuso
ms.service: azure-cdn
ms.topic: article
ms.date: 11/01/2019
ms.author: magattus
ms.openlocfilehash: aa401150ee7a0f02e809ad702b8247e18081c8a3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "74171557"
---
# <a name="standard-rules-engine-reference-for-azure-cdn"></a>Referenz zur Standardregel-Engine für Azure CDN

In der [Standardregel-Engine](cdn-standard-rules-engine.md) für Azure Content Delivery Network (Azure CDN) besteht eine Regel aus mindestens einer Übereinstimmungsbedingung und einer Aktion. In diesem Artikel finden Sie ausführliche Beschreibungen der Übereinstimmungsbedingungen und Features, die in der Standardregel-Engine für Azure CDN verfügbar sind.

Die Regel-Engine fungiert als endgültige Autorität dafür, wie bestimmte Typen von Anforderungen von Azure CDN Standard verarbeitet werden.

**Allgemeine Anwendungsfälle für die Regeln:**

- Außerkraftsetzen oder Definieren einer benutzerdefinierten Cacherichtlinie
- Umleiten von Anforderungen
- Anpassen von HTTP-Anforderungen und Antwortheadern

## <a name="terminology"></a>Begriff

Legen Sie [Übereinstimmungsbedingungen](cdn-standard-rules-engine-match-conditions.md) und [Aktionen](cdn-standard-rules-engine-actions.md) fest, um eine Regel in der Regel-Engine zu definieren:

 ![Azure CDN-Regelstruktur](./media/cdn-standard-rules-engine-reference/cdn-rules-structure.png)

Jede Regel kann bis zu vier Übereinstimmungsbedingungen und drei Aktionen aufweisen. Jeder Azure CDN-Endpunkt kann bis zu fünf Regeln enthalten. 

In der aktuellen Einschränkung auf fünf Regeln für einen Azure CDN-Endpunkt ist eine *globale Standardregel* enthalten. Diese globale Regel verfügt über keine Übereinstimmungsbedingungen. Die in einer globalen Regel definierten Aktionen werden immer ausgelöst.

## <a name="syntax"></a>Syntax

Wie Sonderzeichen in einer Regel behandelt werden, hängt davon ab, wie verschiedene Übereinstimmungsbedingungen und Aktionen Textwerte verarbeiten. Eine Übereinstimmungsbedingung oder Aktion kann Text auf folgende Weisen interpretieren:

- [Literalwerte](#literal-values)
- [Platzhalterwerte](#wildcard-values)


### <a name="literal-values"></a>Literalwerte

Text, der als Literalwert interpretiert wird, behandelt alle Sonderzeichen (*mit Ausnahme des Zeichens „%“* ) als Teil des Werts, für den eine Übereinstimmung in der Regel gesucht wird. Eine literale Übereinstimmungsbedingung, die auf `'*'` festgelegt ist, wird nur erfüllt, wenn der exakte Wert `'*'` ermittelt wird.

Ein Prozentzeichen wird zum Angeben der URL-Codierung verwendet (z. B. `%20`).

### <a name="wildcard-values"></a>Platzhalterwerte

Text, der als Platzhalterwert interpretiert wird, weist Sonderzeichen eine zusätzliche Bedeutung zu. In der folgenden Tabelle wird beschrieben, wie spezifische Sonderzeichen in der Standardregel-Engine interpretiert werden:

Zeichen | BESCHREIBUNG
----------|------------
\ | Ein umgekehrter Schrägstrich wird als Escapezeichen für die in dieser Tabelle angegebenen Zeichen verwendet. Direkt vor dem Sonderzeichen, das mit Escapezeichen versehen werden soll, muss ein umgekehrter Schrägstrich eingegeben werden. Es folgt als Beispiel die Syntax für das Escapezeichen eines Sternchens: `\*`
% | Ein Prozentzeichen wird zum Angeben der URL-Codierung verwendet (z. B. `%20`).
\* | Ein Sternchen ist ein Platzhalter, der ein oder mehrere Zeichen darstellt.
Leerzeichen | Ein Leerzeichen gibt an, dass eine Übereinstimmungsbedingung entweder mit den angegebenen Werten oder Mustern erfüllt werden kann.
Einfache Anführungszeichen | Ein einfaches Anführungszeichen hat keine besondere Bedeutung. Eine gruppe einfacher Anführungszeichen gibt jedoch an, dass ein Wert als Literalwert behandelt werden soll. Einfache Anführungszeichen können auf folgende Weisen verwendet werden:<ul><li>Zum Erfüllen einer Übereinstimmungsbedingung, wenn der angegebene Wert mit einem beliebigen Teil des Vergleichswerts übereinstimmt.  Beispielsweise stimmt `'ma'` mit beliebigen der folgenden Zeichenfolgen überein: <ul><li>/business/**ma**rathon/asset.htm</li><li>**ma**p.gif</li><li>/business/template.**ma**p</li></ul><li>Zum Zulassen des Festlegens eines Sonderzeichens als Literalzeichen. Sie können beispielsweise ein literales Leerzeichen festlegen, indem Sie ein Leerzeichen in einfachen Anführungszeichen einschließen (`' '` oder `'<sample value>'`).</li><li>Zum Ermöglichen der Angabe eines leeren Werts. Sie können einen leeren Wert angeben, indem Sie einfache Anführungszeichen angeben ( **''** ).</li></ul>**Wichtig**:<br /><ul><li>Wenn der angegebene Wert keinen Platzhalter enthält, wird der Wert automatisch als Literalwert behandelt. Für einen Literalwert müssen Sie keine einfachen Anführungszeichen angeben.</li><li>Wenn ein umgekehrter Schrägstrich nicht als Escapezeichen für ein anderes Zeichen in dieser Tabelle verwendet wird, wird der umgekehrte Schrägstrich ignoriert, wenn er in einfachen Anführungszeichen steht.</li><li>Eine andere Möglichkeit zum Angeben eines Sonderzeichens als Literalzeichen besteht darin, es mithilfe eines umgekehrten Schrägstrichs (`\`) mit einem Escapezeichen zu versehen.</li></ul>

## <a name="next-steps"></a>Nächste Schritte

- [Übereinstimmungsbedingungen in der Standardregel-Engine](cdn-standard-rules-engine-match-conditions.md)
- [Aktionen in der Standardregel-Engine](cdn-standard-rules-engine-actions.md)
- [Erzwingen von HTTPS mithilfe der Standardregel-Engine](cdn-standard-rules-engine.md)
- [Übersicht über das Azure Content Delivery Network](cdn-overview.md)
