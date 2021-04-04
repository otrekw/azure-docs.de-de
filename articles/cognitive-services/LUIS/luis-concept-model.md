---
title: Entwerfen mit Modellen – LUIS
description: Language Understanding bietet verschiedene Typen von Modellen. Einige Modelle können auf mehr als eine Weise eingesetzt werden.
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 04/30/2020
ms.openlocfilehash: 576ba945018d13db9cd24888f3c41a2215857694
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "91316510"
---
# <a name="design-with-intent-and-entity-models"></a>Entwerfen mit Absichts- und Entitätsmodellen

Language Understanding bietet zwei Arten von Modellen, mit denen Sie Ihr App-Schema definieren können. Ihr App-Schema bestimmt, welche Informationen die Vorhersage einer neuen Benutzeräußerung liefert.

Das App-Schema basiert auf Modellen, die Sie mithilfe von [Machine Teaching](#authoring-uses-machine-teaching) erstellen:
* [Absichten](#intents-classify-utterances) zum Klassifizieren von Benutzeräußerungen
* [Entitäten](#entities-extract-data) zum Extrahieren von Daten aus einer Äußerung

## <a name="authoring-uses-machine-teaching"></a>Dokumenterstellung verwendet Machine Teaching

Mithilfe der Machine Teaching-Methodik von LUIS können Sie einem Computer problemlos Konzepte vermitteln. Sie müssen nicht mit _Machine Learning_ vertraut sein, um LUIS verwenden zu können. Stattdessen bringen Sie LUIS wie ein Lehrer ein Konzept bei, indem Sie Beispiele für das Konzept geben und anhand von anderen verwandten Konzepten erläutern, wie ein Konzept modelliert werden soll. In Ihrer Rolle als Lehrer können Sie das Modell von LUIS außerdem interaktiv verbessern, indem Sie Vorhersagefehler ermitteln und beheben.

<a name="v3-authoring-model-decomposition"></a>

## <a name="intents-classify-utterances"></a>Klassifizieren von Äußerungen durch Absichten

Beispieläußerungen werden durch eine Absicht klassifiziert, um LUIS Informationen über die Absicht zu vermitteln. Beispieläußerungen innerhalb einer Absicht werden als positive Beispiele der Äußerung verwendet. Diese gleichen Äußerungen werden in allen anderen Absichten als negative Beispiele verwendet.

Stellen Sie sich eine App vor, die die Absicht eines Benutzers bestimmen muss, ein Buch zu bestellen, und eine App, die die Versandadresse für den Kunden benötigt. Diese App weist zwei Absichten auf: `OrderBook` und `ShippingLocation`.

Die folgende Äußerung ist ein **positives Beispiel** für die Absicht `OrderBook` und ein **negatives Beispiel** für die Absichten `ShippingLocation` und `None`:

`Buy the top-rated book on bot architecture.`

## <a name="entities-extract-data"></a>Extrahieren von Daten durch Entitäten

Eine Entität stellt eine Dateneinheit dar, die aus der Äußerung extrahiert werden soll. Eine Machine Learning-Entität ist eine Entität der obersten Ebene, die untergeordnete Entitäten enthält, bei denen es sich ebenfalls um Machine Learning-Entitäten handelt.

Ein Beispiel für eine Machine Learning-Entität ist eine Bestellung eines Flugtickets. Konzeptionell handelt es sich dabei um eine einzelne Transaktion mit vielen kleineren Dateneinheiten, wie etwa Datum, Uhrzeit, Menge der Plätze, Art der Plätze (wie Erste Klasse oder Economy), Startort, Zielort und Auswahl des Bordmenüs.

## <a name="intents-versus-entities"></a>Absichten im Vergleich mit Entitäten

Eine Absicht ist das gewünschte Ergebnis der _gesamten_ Äußerung, während Entitäten Datenstücke darstellen, die aus der Äußerung extrahiert wurden. Absichten sind in der Regel mit Aktionen verbunden, die von der Clientanwendung ausgeführt werden sollen. Entitäten sind die Informationen, die für die Aktion erforderlich sind. Aus Programmiererperspektive würde eine Absicht einen Methodenaufruf auslösen, und die Entitäten würden als Parameter dieses Methodenaufrufs fungieren.

Diese Äußerung _muss_ eine Absicht und _kann_ Entitäten aufweisen:

`Buy an airline ticket from Seattle to Cairo`

Diese Äußerung verfolgt ein einzelnes Ziel:

* den Kauf eines Flugtickets

Diese Äußerung _kann_ verschiedene Entitäten aufweisen:

* Die Orte Seattle (Startort) und Kairo (Zielort)
* Die Menge eines einzelnen Tickets

## <a name="entity-model-decomposition"></a>Aufschlüsselung des Entitätsmodells

LUIS unterstützt die _Modellaufschlüsselung_ mithilfe der Erstellungs-APIs, um ein Konzept in kleinere Bestandteile aufzugliedern. Auf diese Weise können Sie ihre Modelle mit der Zuversicht aufbauen, die sich aus der Kenntnis von Konstruktion und Vorhersagemechanismen der einzelnen Teile speist.

Die Aufschlüsselung von Modellen setzt sich aus folgenden Teilen zusammen:

* [Absichten](#intents-classify-utterances)
    * [features](#features)
* [Machine Learning-Entitäten](reference-entity-machine-learned-entity.md)
    * Untergeordnete Entitäten (ebenfalls Machine Learning-Entitäten)
        * [features](#features)
            * [Ausdrucksliste](luis-concept-feature.md)
            * [Machine Learning-fremde Entitäten](luis-concept-feature.md) (beispielsweise [reguläre Ausdrücke](reference-entity-regular-expression.md), [Listen](reference-entity-list.md) und [vordefinierte Entitäten](luis-reference-prebuilt-entities.md))

<a name="entities-extract-data"></a>
<a name="machine-learned-entities"></a>

## <a name="features"></a>Features

Ein [Feature](luis-concept-feature.md) ist ein eindeutiges Merkmal oder Attribut der Daten, die Ihr System untersucht. Machine Learning-Features geben LUIS wichtige Hinweise darauf, wo nach Informationen gesucht werden soll, die ein Konzept ausmachen. Hierbei handelt es sich um Hinweise, die von LUIS befolgt werden können, aber nicht um feste Regeln. Diese Hinweise werden zusammen mit den Beschriftungen genutzt, um die Daten zu finden.

## <a name="patterns"></a>Muster

[Muster](luis-concept-patterns.md) dienen zur Verbesserung der Genauigkeit, wenn sich mehrere Äußerungen stark ähneln. Ein Muster ermöglicht es Ihnen, größere Genauigkeit für eine Absicht zu erreichen, ohne viele weitere Äußerungen anzugeben.

## <a name="extending-the-app-at-runtime"></a>Erweitern der App zur Laufzeit

Das Schema der App (Modelle und Features) wird trainiert und zum Vorhersageendpunkt veröffentlicht. Sie können neue Informationen zusammen mit der Äußerung des Benutzers an den Vorhersageendpunkt übergeben, um die Vorhersage zu verbessern. Weitere Informationen finden Sie [hier](schema-change-prediction-runtime.md).

## <a name="next-steps"></a>Nächste Schritte

* Grundlagen von [Absichten](luis-concept-intent.md) und [Entitäten](luis-concept-entity-types.md).
* Weitere Informationen zu Features finden Sie [hier](luis-concept-feature.md).