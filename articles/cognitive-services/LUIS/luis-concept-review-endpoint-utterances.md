---
title: Überprüfen von Benutzeräußerungen – LUIS
description: Mit aktivem Lernen überprüfen Sie Endpunktäußerungen auf die Richtigkeit von Absicht und Entität. LUIS wählt Endpunktäußerungen aus, bei denen es nicht sicher ist.
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 04/01/2020
ms.openlocfilehash: 82f228d5e6f801539c549e16faea371782ad4b59
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "91316442"
---
# <a name="concepts-for-enabling-active-learning-by-reviewing-endpoint-utterances"></a>Konzepte für das Aktivieren des aktiven Lernens durch Überprüfen von Endpunktäußerungen
Aktives Lernen ist eine von drei Strategien zur Verbesserung der Vorhersagegenauigkeit. Von diesen ist sie am einfachsten zu implementieren. Mit aktivem Lernen überprüfen Sie Endpunktäußerungen auf die Richtigkeit von Absicht und Entität. LUIS wählt Endpunktäußerungen aus, bei denen es nicht sicher ist.

## <a name="what-is-active-learning"></a>Was ist aktives Lernen?
Aktives Lernen ist ein zweistufiger Prozess. LUIS wählt zunächst die am App-Endpunkt empfangenen Äußerungen aus, die validiert werden müssen. Der zweite Schritt wird vom App Besitzer oder Projektmitarbeiter ausgeführt. Dabei werden die ausgewählten Äußerungen [überprüft](luis-how-to-review-endpoint-utterances.md), um auch z.B. die richtige Absicht und eventuelle Entitäten in die Absicht zu validieren. Nach dem Überprüfen der Äußerungen trainieren und veröffentlichen Sie die App erneut.

## <a name="which-utterances-are-on-the-review-list"></a>Äußerungen in der Prüfliste
LUIS fügt der Prüfliste Äußerungen hinzu, wenn die wichtigste ausgelöste Absicht eine niedrigere Bewertung hat oder wenn die Bewertungen der beiden wichtigsten Absichten zu ähnlich sind.

## <a name="single-pool-for-utterances-per-app"></a>Einzelner Pool für Äußerungen pro App
Die Liste **Überprüfen von Endpunktäußerungen** ändert sich nicht je nach Version. Es muss nur ein Pool mit Äußerungen überprüft werden. Dies gilt unabhängig davon, welche Version der Äußerung Sie aktiv bearbeiten oder welche Version der App auf dem Endpunkt veröffentlicht wurde.

In der [REST-API](https://westus.dev.cognitive.microsoft.com/docs/services/luis-programmatic-apis-v3-0-preview/operations/58b6f32139e2bb139ce823c9) ist der Versionsname erforderlich und muss in der Anwendung vorhanden sein, wird aber über diese Prüfung hinaus nicht verwendet. Die Überprüfungsäußerungen gelten für eine gesamte Anwendung. Wenn Sie Äußerungen aus einer _Version_ entfernen, sind alle Versionen betroffen.

## <a name="where-are-the-utterances-from"></a>Ursprung von Äußerungen
Endpunktäußerungen stammen aus Endbenutzerabfragen am HTTP-Endpunkt der Anwendung. Wenn Ihre App noch nicht veröffentlicht wurde oder noch keine Treffer empfangen hat, stehen keine Äußerungen für die Überprüfung bereit. Wenn für eine bestimmte Absicht oder Entität keine Treffer am Endpunkt empfangen werden, stehen auch keine Äußerungen zum Überprüfen bereit.

## <a name="schedule-review-periodically"></a>Planen regelmäßiger Überprüfungen
Das Überprüfen vorgeschlagener Äußerungen muss nicht täglich erfolgen, sollte jedoch Teil Ihrer regelmäßigen Wartung von LUIS sein.

## <a name="delete-review-items-programmatically"></a>Programmgesteuertes Löschen überprüfter Elemente
Verwenden Sie die API zum **[Löschen nicht gekennzeichneter Äußerungen](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/58b6f32139e2bb139ce823c9)** . Sichern Sie diese Äußerungen vor dem Löschen, indem Sie die **[Protokolldateien exportieren](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c36)** .

## <a name="enable-active-learning"></a>Aktivieren des aktiven Lernens

Um das aktive Lernen zu aktivieren, müssen Sie Benutzerabfragen protokollieren. Dies wird durch Aufrufen der [Endpunktabfrage](luis-get-started-create-app.md#query-the-v3-api-prediction-endpoint) mit dem `log=true`-Abfragezeichenfolgenparameter und -wert erreicht.

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie, wie Sie Endpunktäußerungen [überprüfen](luis-how-to-review-endpoint-utterances.md).
