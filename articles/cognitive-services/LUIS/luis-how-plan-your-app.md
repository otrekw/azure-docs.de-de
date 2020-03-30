---
title: Planen Ihrer App – LUIS
titleSuffix: Azure Cognitive Services
description: Beschreiben Sie kurz relevante App-Absichten und -Entitäten, und erstellen Sie dann Ihre Anwendungspläne in Language Understanding Intelligent Service (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 11/20/2019
ms.author: diberry
ms.openlocfilehash: 6a155f4c43da03ccdc40d289742918973aa6da7b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "74326773"
---
# <a name="plan-your-luis-app-schema-with-subject-domain-and-data-extraction"></a>Planen Ihres LUIS-App-Schemas mit Motivdomäne und Datenextraktion

Ein LUIS-App-Schema enthält [Absichten](luis-glossary.md#intent) und [Entitäten](luis-glossary.md#entity), die für Ihre [Motivdomäne](luis-glossary.md#domain) relevant sind. Die Absichten klassifizieren [Benutzeräußerungen](luis-glossary.md#utterance), und die Entitäten extrahieren Daten aus den Benutzeräußerungen.

## <a name="identify-your-domain"></a>Identifizieren Ihrer Domäne

Eine LUIS-App baut auf einer Motivdomäne auf. Beispielsweise könnten Sie eine Reise-App erstellen, die Buchungen von Tickets, Flügen, Hotels und Mietwagen ausführt. Eine andere App könnte Inhalte im Zusammenhang mit Sportaktivitäten, dem Nachverfolgen von Fitnessübungen und dem Festlegen von Zielen bereitstellen. Durch die Identifizierung der Domäne können Sie Wörter oder Ausdrücke finden, die für Ihre Domäne relevant sind.

> [!TIP]
> LUIS bietet [vordefinierte Domänen](luis-how-to-use-prebuilt-domains.md) für viele häufige Szenarien. Überprüfen Sie, ob Sie eine vordefinierte Domäne als Ausgangspunkt für Ihre App verwenden können.

## <a name="identify-your-intents"></a>Identifizieren Ihrer Absichten

Überlegen Sie sich, welche [Absichten](luis-concept-intent.md) wichtig für die Aufgabe Ihrer Anwendung sind.

Beispiel: eine Reise-App mit Funktionen zum Buchen eines Flugs und Überprüfen des Wetters am Ziel des Benutzers. Sie können für diese Aktionen die Absichten `BookFlight` und `GetWeather` definieren.

In einer komplexeren App mit umfassenderen Funktionen gibt es weitere Absichten, und Sie sollten diese sorgfältig definieren, damit sie nicht zu spezifisch sind. Beispielsweise müssen möglicherweise `BookFlight` und `BookHotel` separate Absichten sein, aber `BookInternationalFlight` und `BookDomesticFlight` können zu ähnlich sein.

> [!NOTE]
> Als bewährte Methode sollten Sie nur so viele Absichten verwenden, wie zum Ausführen der Funktionen Ihrer App erforderlich sind. Wenn Sie zu viele Absichten definieren, wird es für LUIS schwieriger, Äußerungen richtig zu klassifizieren. Wenn Sie zu wenige definieren, sind sie möglicherweise so allgemein gehalten, dass sie sich überschneiden.

Wenn Sie nicht die Gesamtabsicht identifizieren müssen, fügen Sie alle Beispielbenutzeräußerungen zur Absicht `None` hinzu. Wenn sich bei Ihrer App die Notwendigkeit weiterer Absichten herausstellt, können Sie diese später erstellen.

## <a name="create-example-utterances-for-each-intent"></a>Erstellen von Beispieläußerungen für jede Absicht

Vermeiden Sie zunächst, zu viele Äußerungen für jede Absicht zu erstellen. Nachdem Sie die Absichten festgelegt haben, können Sie pro Absicht jeweils 15 bis 30 Beispieläußerungen erstellen. Jede Äußerung sollte sich von den zuvor bereitgestellten Äußerungen unterscheiden. Eine gute Vielfalt in den Äußerungen umfasst die Gesamtzahl der Wörter, die Wortwahl, die Verbform und die Interpunktion.

Weitere Informationen finden Sie unter [Grundlegendes zu guten Äußerungen für LUIS-Apps](luis-concept-utterance.md).

## <a name="identify-your-entities"></a>Identifizieren Ihrer Entitäten

Identifizieren Sie in den Beispieläußerungen die Entitäten, die extrahiert werden sollen. Um einen Flug zu buchen, benötigen Sie Informationen wie Ziel, Datum, Fluggesellschaft, Ticketkategorie und Reiseklasse. Erstellen Sie Entitäten für diese Datentypen, und markieren Sie dann die [Entitäten](luis-concept-entity-types.md) in den Beispieläußerungen. Entitäten sind wichtig, um eine Absicht umzusetzen.

Wenn Sie festlegen, welche Entitäten in Ihrer App verwendet werden sollen, bedenken Sie, dass es verschiedene Typen von Entitäten zum Aufzeichnen der Beziehungen zwischen Objekttypen gibt. Unter [Entitäten in LUIS](luis-concept-entity-types.md) finden Sie weitere Details zu den verschiedenen Typen.

> [!TIP]
> LUIS bietet [vordefinierte Entitäten](luis-prebuilt-entities.md) für gängige, interaktive Benutzerszenarios. Verwenden Sie vorgefertigte Entitäten als Ausgangspunkt für die Anwendungsentwicklung.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Einführung in den Entwicklungslebenszyklus von LUIS](luis-concept-app-iteration.md)

